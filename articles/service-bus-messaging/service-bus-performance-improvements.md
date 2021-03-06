---
title: Procedimientos recomendados para mejorar el rendimiento mediante Azure Service Bus | Microsoft Docs
description: "Describe cómo usar Service Bus para optimizar el rendimiento al intercambiar mensajes asincrónicos."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e756c15d-31fc-45c0-8df4-0bca0da10bb2
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/31/2018
ms.author: sethm
ms.openlocfilehash: aba53fcadb9cefa70afc175dd02e4723eb6e5f5d
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2018
---
# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Procedimientos recomendados para mejorar el rendimiento mediante la mensajería de Service Bus

En este artículo se describe cómo usar [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) para optimizar el rendimiento al intercambiar mensajes asincrónicos. En la primera parte de este artículo se describen los distintos mecanismos que se ofrecen para ayudar a mejorar el rendimiento. La segunda parte proporciona orientación sobre cómo usar Service Bus de manera que pueda ofrecer el mejor rendimiento en un escenario determinado.

En todo este tema, el término "cliente" hace referencia a cualquier entidad que acceda a Service Bus. Un cliente puede asumir el rol de un remitente o receptor. El término "remitente" se usa para referirse a cualquier cliente de una cola o un tema de Service Bus que envía mensajes a una suscripción de una cola o un tema de Service Bus. El término "receptor" hace referencia a cualquier cliente de una cola o suscripción de Service Bus que recibe mensajes de una cola o suscripción de Service Bus.

En estas secciones se presentan algunos conceptos que Service Bus usa para ayudar a mejorar el rendimiento.

## <a name="protocols"></a>Protocolos

Service Bus permite a los clientes enviar y recibir mensajes a través de uno de estos tres protocolos:

1. Advanced Message Queuing Protocol (AMQP)
2. Protocolo de mensajería de Service Bus (SBMP)
3. HTTP

AMQP y SBMP son más eficaces, ya que mantienen la conexión a Service Bus, siempre y cuando exista la fábrica de mensajería. También implementa el procesamiento por lotes y la captura previa. A menos que se mencione explícitamente, en todo el contenido de este tema se asume que se usa AMQP o SBMP.

## <a name="reusing-factories-and-clients"></a>Reutilización de factorías y clientes

Los objetos de cliente de Service Bus, como [QueueClient][QueueClient] o [MessageSender][MessageSender], se crean mediante un objeto [MessagingFactory][MessagingFactory], que también proporciona administración interna de las conexiones. Ni los generadores de mensajería ni los clientes de cola, tema y suscripción deben cerrarse después de enviar un mensaje y después volver a crearlos al enviar el mensaje siguiente. Al cerrar una factoría de mensajería, se elimina la conexión con el servicio Service Bus y, al volver a crearla, se establece una nueva conexión. Establecer una conexión es una operación costosa que puede evitar si vuelve a usar la misma factoría y los mismos objetos de cliente para varias operaciones. Puede utilizar el objeto [QueueClient][QueueClient] para enviar mensajes desde varios subprocesos y operaciones asincrónicas simultáneas. 

## <a name="concurrent-operations"></a>Operaciones simultáneas

Se tarda tiempo en realizar una operación (enviar, recibir, eliminar, etc.). Este tiempo incluye el procesamiento de la operación por el servicio Service Bus, además de la latencia de la solicitud y la respuesta. Para aumentar el número de operaciones por tiempo, las operaciones deberán ejecutarse simultáneamente. Puede hacerlo de varias maneras diferentes:

* **Operaciones asincrónicas**: el cliente programa las operaciones mediante la realización de operaciones asincrónicas. La siguiente solicitud se inicia antes de que se complete la anterior. A continuación, se proporciona un ejemplo de operación asincrónica de envío:
  
 ```csharp
  BrokeredMessage m1 = new BrokeredMessage(body);
  BrokeredMessage m2 = new BrokeredMessage(body);
  
  Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #1");
    });
  Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #2");
    });
  Task.WaitAll(send1, send2);
  Console.WriteLine("All messages sent");
  ```
  
  Esto es un ejemplo de operación asincrónica de recepción:
  
  ```csharp
  Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
  
  Task.WaitAll(receive1, receive2);
  Console.WriteLine("All messages received");
  
  async void ProcessReceivedMessage(Task<BrokeredMessage> t)
  {
    BrokeredMessage m = t.Result;
    Console.WriteLine("{0} received", m.Label);
    await m.CompleteAsync();
    Console.WriteLine("{0} complete", m.Label);
  }
  ```

* **Varias factorías**: todos los clientes (remitentes, además de receptores) creados por la misma factoría comparten una conexión TCP. El rendimiento máximo de mensajes está limitado por el número de operaciones que se pueden procesar mediante esta conexión TCP. El rendimiento que puede obtenerse con una sola factoría varía en gran medida en función del tamaño del mensaje y los tiempos de ida y vuelta de TCP. Para obtener mayores tasas de rendimiento, debería usar varias factorías de mensajes.

## <a name="receive-mode"></a>Modo de recepción

Al crear un cliente de cola o suscripción, puede especificar un modo de recepción: *Peek-lock* (Inspección y bloqueo) o *Receive And Delete* (Recepción y eliminación). El modo de recepción predeterminado es [PeekLock][PeekLock]. Cuando se trabaja en este modo, el cliente envía una solicitud para recibir un mensaje de Service Bus. Después de que el cliente haya recibido el mensaje, envía una solicitud para completarlo.

Cuando se establece el modo de recepción en [ReceiveAndDelete][ReceiveAndDelete], ambos pasos se combinan en una sola solicitud. Esto reduce el número total de operaciones y puede mejorar el rendimiento general de los mensajes. Esta mejora del rendimiento conlleva el riesgo de la pérdida de mensajes.

Service Bus no admite transacciones para las operaciones de recepción y eliminación. Además, se necesita la semántica de PeekLock para aquellos escenarios en los que el cliente desee aplazar un mensaje o [procesarlo como correo devuelto](service-bus-dead-letter-queues.md).

## <a name="client-side-batching"></a>Procesamiento por lotes del lado cliente

El procesamiento por lotes del lado cliente permite que un cliente de cola o tema retrase el envío de un mensaje durante un período determinado. Si el cliente envía más mensajes durante este período, los transmite en un único lote. El procesamiento por lotes del lado cliente también hace que un cliente de cola o suscripción procese varias solicitudes **Complete** por lotes en una única solicitud. El procesamiento por lotes solo está disponible para las operaciones **Send** y **Complete** asincrónicas. Las operaciones sincrónicas se envían de inmediato al servicio Service Bus. El procesamiento por lotes no tiene lugar para las operaciones de inspección o recepción, así como tampoco entre clientes.

De forma predeterminada, un cliente usa un intervalo entre lotes de 20 ms. Puede cambiar el intervalo entre lotes si configura la propiedad [BatchFlushInterval][BatchFlushInterval] antes de crear la factoría de mensajería. Esta configuración afecta a todos los clientes que se creen en esta factoría.

Para deshabilitar el procesamiento por lotes, establezca la propiedad [BatchFlushInterval][BatchFlushInterval] en **TimeSpan.Zero**. Por ejemplo: 

```csharp
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

El procesamiento por lotes no afecta al número de operaciones de mensajería facturables y solo está disponible para el protocolo de cliente de Service Bus. El protocolo HTTP no admite el procesamiento por lotes.

## <a name="batching-store-access"></a>Acceso al almacén de procesamiento por lotes

Para aumentar el rendimiento de una cola, un tema o una suscripción, Service Bus procesa varios mensajes por lotes al escribir en su almacén interno. Si se habilita en una cola o en un tema, la escritura de mensajes en el almacén se procesará por lotes. Si se habilita en una cola o en una suscripción, la eliminación de mensajes del almacén se procesará por lotes. Si el acceso al almacén de procesamiento por lotes está habilitado para una entidad, Service Bus retrasa una operación de escritura en almacenamiento relacionada con dicha entidad unos 20 ms. 

> [!NOTE]
> No hay ningún riesgo de pérdida de mensajes con procesamiento por lotes, aunque se produzca un error de Service Bus al final de un intervalo de procesamiento por lotes de 20 ms. 

Las demás operaciones de almacenamiento que se producen durante este intervalo se agregan al lote. El acceso al almacén de procesamiento por lotes solo afecta a las operaciones **Send** y **Complete**, pero no a las de recepción. El acceso al almacén de procesamiento por lotes es una propiedad de una entidad. El procesamiento por lotes se produce en todas las entidades que tengan habilitado el acceso al almacén de procesamiento por lotes.

Cuando se crea una cola, un tema o una suscripción nuevos, el acceso al almacén de procesamiento por lotes está habilitado de manera predeterminada. Para deshabilitar el acceso al almacén de procesamiento por lotes, establezca la propiedad [EnableBatchedOperations][EnableBatchedOperations] en **false** antes de crear la entidad. Por ejemplo: 

```csharp
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

El acceso al almacén de procesamiento por lotes no afecta al número de operaciones de mensajería facturables y es una propiedad de una cola, un tema o una suscripción. Es independiente del modo de recepción y del protocolo que se usa entre un cliente y el servicio Service Bus.

## <a name="prefetching"></a>Captura previa

La [captura previa](service-bus-prefetch.md) permite que el cliente de cola o suscripción cargue más mensajes desde el servicio cuando realice una operación de recepción. El cliente almacena estos mensajes en una memoria caché local. El tamaño de la memoria caché lo determinan las propiedades [QueueClient.PrefetchCount][QueueClient.PrefetchCount] o [SubscriptionClient.PrefetchCount][SubscriptionClient.PrefetchCount]. Cada cliente con la captura previa habilitada mantiene su propia memoria caché. La memoria caché no se comparte entre los clientes. Si el cliente inicia una operación de recepción y su memoria caché está vacía, el servicio transmite un lote de mensajes. El tamaño del lote equivale al tamaño de la memoria caché o a 256 KB, con preferencia por el menor valor. Si el cliente inicia una operación de recepción y la memoria caché contiene un mensaje, el mensaje se recupera de la memoria caché.

Cuando se lleva a cabo la captura previa de un mensaje, el servicio bloquea dicho mensaje. De esta manera, se impide que otro receptor reciba el mensaje con captura previa. Si el receptor no puede completar el mensaje antes de que expire el bloqueo, el mensaje pasa a estar disponible para otros receptores. La copia de captura previa del mensaje permanece en la memoria caché. El receptor que consume la copia en caché expirada recibirá una excepción cuando intente completar dicho mensaje. De forma predeterminada, el bloqueo del mensaje expira tras 60 segundos. Este valor puede ampliarse a 5 minutos. Para evitar el consumo de mensajes expirados, el tamaño de la memoria caché debe ser siempre menor que el número de mensajes que un cliente puede consumir en el intervalo de tiempo de espera de bloqueo.

Cuando se usa la expiración de bloqueo predeterminada de 60 segundos, un buen valor para [SubscriptionClient.PrefetchCount][SubscriptionClient.PrefetchCount] es el que se obtiene de multiplicar por 20 las tasas máximas de procesamiento de todos los receptores del generador. Por ejemplo, una factoría crea 3 receptores y cada receptor puede procesar hasta 10 mensajes por segundo. El número de capturas previas no debe superar 20 X 3 X 10 = 600. De forma predeterminada, [QueueClient.PrefetchCount][QueueClient.PrefetchCount] se establece en 0, lo que significa que no se realiza la captura previa de ningún mensaje adicional desde el servicio.

La captura previa de los mensajes aumenta el rendimiento general de una cola o una suscripción porque reduce el número total de operaciones de mensajes o recorridos de ida y vuelta. Sin embargo, capturar el primer mensaje tardará más tiempo (debido al aumento del tamaño del mensaje). Recibir mensajes con captura previa será más rápido porque el cliente ya ha descargado estos mensajes.

El servidor comprueba la propiedad de período de vida (TTL) de un mensaje en el momento en que envía el mensaje al cliente. El cliente no comprueba la propiedad TTL del mensaje cuando lo recibe. En su lugar, se puede recibir el mensaje incluso si se ha superado el TTL del mensaje mientras el cliente lo almacenaba en caché.

La captura previa no afecta al número de operaciones de mensajería facturables y solo está disponible para el protocolo de cliente de Service Bus. El protocolo HTTP no admite la captura previa. La captura previa está disponible para las operaciones de recepción sincrónicas y asincrónicas.

## <a name="express-queues-and-topics"></a>Colas y temas exprés

Las entidades con la opción Rápido habilitan escenarios de latencia reducida y alto rendimiento, y se admiten solo en el plan de mensajería Estándar. Las entidades creadas en [espacios de nombres Premium](service-bus-premium-messaging.md) no admiten la opción Rápido. Con las entidades exprés, si se envía un mensaje a una cola o un tema, este no se guarda de inmediato en el almacén de mensajería. En su lugar, se almacena en la memoria caché. Si un mensaje permanece en la cola durante más de unos segundos, se escribe automáticamente en almacenamiento estable, lo que lo protege contra la pérdida debida a una interrupción. La escritura del mensaje en una memoria caché aumenta el rendimiento y reduce la latencia porque no se accede al almacenamiento estable en el momento en que se envía el mensaje. Los mensajes que se consumen en unos segundos no se escriben en el almacén de mensajería. En el ejemplo siguiente se crea un tema exprés.

```csharp
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Si se envía un mensaje que contiene información importante y que no debe perderse a una entidad exprés, el remitente puede forzar que Service Bus lo conserve inmediatamente en almacenamiento estable si se establece la propiedad [ForcePersistence][ForcePersistence] en **true**.

> [!NOTE]
> Las entidades con la opción Rápido no admiten transacciones.

## <a name="use-of-partitioned-queues-or-topics"></a>Uso de colas o temas con particiones

De forma interna, Service Bus usa el mismo nodo y almacén de mensajería para procesar y almacenar todos los mensajes de una entidad de mensajería (cola o tema). Por otra parte, una [cola o un tema con particiones](service-bus-partitioning.md) se distribuyen entre varios nodos y almacenes de mensajería. Las colas y los temas con particiones no solo producen un rendimiento mayor que las colas y los temas normales, sino que también ofrecen una mayor disponibilidad. Para crear una entidad particionada, establezca la propiedad [EnablePartitioning][EnablePartitioning] en **true** como se muestra en el ejemplo siguiente. Para obtener más información sobre las entidades con particiones, consulte [Entidades de mensajería con particiones][Partitioned messaging entities].

```csharp
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Uso de varias colas

Si no se pueden usar una cola o un tema con particiones, o si una única cola o tema con particiones no pueden procesar la carga esperada, debe usar varias entidades de mensajería. Cuando use varias entidades, cree un cliente dedicado para cada una, en lugar de usar el mismo cliente para todas.

## <a name="development-and-testing-features"></a>Características de desarrollo y pruebas

Service Bus tiene una característica que se utiliza específicamente para desarrollo que **nunca debe utilizarse en configuraciones de producción**: [TopicDescription.EnableFilteringMessagesBeforePublishing][].

Cuando se agregan nuevas reglas o filtros al tema, se puede utilizar [TopicDescription.EnableFilteringMessagesBeforePublishing][] para comprobar que la nueva expresión de filtro funciona según lo esperado.

## <a name="scenarios"></a>Escenarios

En las secciones siguientes, se describen escenarios habituales de mensajería y se explica la configuración preferida de Service Bus. Las tasas de rendimiento se clasifican como pequeñas (menos de 1 mensaje/segundo), moderadas (1 mensaje/segundo o más, pero menos de 100 mensajes/segundo) y altas (100 mensajes/segundo o más). El número de clientes se clasifica como pequeño (5 o menos), moderado (más de 5 pero un máximo de 20) y grande (más de 20).

### <a name="high-throughput-queue"></a>Cola de alto rendimiento

Objetivo: Maximizar el rendimiento de una sola cola. El número de remitentes y receptores es pequeño.

* Use una cola particionada para mejorar la disponibilidad y el rendimiento.
* Para aumentar la tasa general de envío a la cola, use varias factorías de mensajes para crear remitentes. Para cada remitente, use operaciones asincrónicas o varios subprocesos.
* Para aumentar la tasa general de recepción de la cola, use varias factorías de mensajes para crear receptores.
* Use operaciones asincrónicas para aprovechar el procesamiento por lotes del lado cliente.
* Establezca el intervalo de procesamiento por lotes en 50 ms para reducir el número de transmisiones del protocolo de cliente de Service Bus. Si se usan varios remitentes, aumente el intervalo de procesamiento por lotes a 100 ms.
* Deje habilitado el acceso al almacén de procesamiento por lotes. Esto aumenta la tasa general con que se pueden escribir mensajes en la cola.
* Establezca el número de capturas previas en el valor resultante de multiplicar por 20 las tasas máximas de procesamiento de todos los receptores de una factoría. Esto reduce el número de transmisiones del protocolo de cliente de Service Bus.

### <a name="multiple-high-throughput-queues"></a>Varias colas de alto rendimiento

Objetivo: Maximizar el rendimiento general de varias colas. El rendimiento de una cola individual es moderado o alto.

Para obtener el máximo rendimiento en varias colas, use la configuración descrita para maximizar el rendimiento de una sola cola. Además, use distintas factorías para crear clientes que envíen o reciban desde diferentes colas.

### <a name="low-latency-queue"></a>Cola de baja latencia

Objetivo: minimizar la latencia de un extremo a otro de una cola o un tema. El número de remitentes y receptores es pequeño. El rendimiento de la cola es pequeño o moderado.

* Use una cola particionada para mejorar la disponibilidad.
* Deshabilite el procesamiento por lotes del lado cliente. El cliente envía inmediatamente un mensaje.
* Deshabilite el acceso al almacén de procesamiento por lotes. El servicio escribe inmediatamente el mensaje en el almacén.
* Si usa un solo cliente, establezca el número de capturas previas en el valor resultante de multiplicar por 20 la tasa de procesamiento del receptor. Si llegan varios mensajes a la cola al mismo tiempo, el protocolo de cliente de Service Bus los transmite todos a la vez. Cuando el cliente recibe el siguiente mensaje, ese mensaje ya se encuentra en la memoria caché local. La memoria caché debe ser pequeña.
* Si se usan varios clientes, establezca el número de capturas previas en 0. De esta manera, el segundo cliente puede recibir el segundo mensaje mientras el primer cliente todavía está procesando el primer mensaje.

### <a name="queue-with-a-large-number-of-senders"></a>Cola con un gran número de remitentes

Objetivo: maximizar el rendimiento de una cola o un tema con un gran número de remitentes. Cada remitente envía mensajes a una tasa moderada. El número de receptores es pequeño.

Service Bus permite hasta 1000 conexiones simultáneas a una entidad de mensajería (o 5000 mediante AMQP). Este límite se aplica en el nivel de espacio de nombres, y las colas, los temas y las suscripciones quedan restringidos por el límite de conexiones simultáneas por espacio de nombres. Para las colas, este número se comparte entre remitentes y receptores. Si se requieren las mil conexiones para los remitentes, debería reemplazar la cola por un tema y una sola suscripción. Un tema acepta un máximo de mil conexiones simultáneas de los remitentes, mientras que la suscripción acepta otras mil conexiones simultáneas de receptores. Si se requieren más de mil remitentes simultáneos, los remitentes deberían enviar mensajes al protocolo de Service Bus a través de HTTP.

Para maximizar el rendimiento, haga lo siguiente:

* Use una cola particionada para mejorar la disponibilidad y el rendimiento.
* Si cada remitente reside en un proceso diferente, use solo una única factoría para cada proceso.
* Use operaciones asincrónicas para aprovechar el procesamiento por lotes del lado cliente.
* Use el intervalo predeterminado de procesamiento por lotes de 20 ms para reducir el número de transmisiones del protocolo de cliente de Service Bus.
* Deje habilitado el acceso al almacén de procesamiento por lotes. Esto aumenta la velocidad global con que se pueden escribir mensajes en la cola o el tema.
* Establezca el número de capturas previas en el valor resultante de multiplicar por 20 las tasas máximas de procesamiento de todos los receptores de una factoría. Esto reduce el número de transmisiones del protocolo de cliente de Service Bus.

### <a name="queue-with-a-large-number-of-receivers"></a>Cola con un gran número de receptores

Objetivo: maximizar la velocidad de recepción de una cola o suscripción con un gran número de receptores. Cada receptor recibe mensajes a una tasa moderada. El número de remitentes es pequeño.

Service Bus permite hasta 1000 conexiones simultáneas a una entidad. Si una cola requiere más de mil receptores, debería reemplazar la cola por un tema y varias suscripciones. Cada suscripción admite hasta mil conexiones simultáneas. Como alternativa, los receptores pueden acceder a la cola mediante el protocolo HTTP.

Para maximizar el rendimiento, haga lo siguiente:

* Use una cola particionada para mejorar la disponibilidad y el rendimiento.
* Si cada receptor reside en un proceso diferente, use solo una única factoría por proceso.
* Los receptores pueden usar operaciones sincrónicas o asincrónicas. Dada la tasa de recepción moderada de un receptor individual, el procesamiento por lotes del lado cliente de una solicitud Complete no afecta al rendimiento del receptor.
* Deje habilitado el acceso al almacén de procesamiento por lotes. Esto reduce la carga general de la entidad. También reduce la velocidad global con que se pueden escribir mensajes en la cola o el tema.
* Establezca el número de capturas previas en un valor pequeño (por ejemplo, PrefetchCount = 10). Esto evita que haya receptores inactivos mientras otros tienen un gran número de mensajes almacenados en caché.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Tema con un número pequeño de suscripciones

Objetivo: maximizar el rendimiento de un tema con un número pequeño de suscripciones. Muchas suscripciones reciben un mensaje, lo que significa que la velocidad de recepción combinada de todas las suscripciones es mayor que la velocidad de envío. El número de remitentes es pequeño. El número de receptores por suscripción es pequeño.

Para maximizar el rendimiento, haga lo siguiente:

* Use un tema con particiones para mejorar la disponibilidad y el rendimiento.
* Para aumentar la velocidad de envío general al tema, use varios generadores de mensajes para crear remitentes. Para cada remitente, use operaciones asincrónicas o varios subprocesos.
* Para aumentar la velocidad de recepción general desde una suscripción, use varios generadores de mensajes para crear receptores. Para cada receptor, use operaciones asincrónicas o varios subprocesos.
* Use operaciones asincrónicas para aprovechar el procesamiento por lotes del lado cliente.
* Use el intervalo predeterminado de procesamiento por lotes de 20 ms para reducir el número de transmisiones del protocolo de cliente de Service Bus.
* Deje habilitado el acceso al almacén de procesamiento por lotes. Esto aumenta la velocidad general con que se pueden escribir mensajes en el tema.
* Establezca el número de capturas previas en el valor resultante de multiplicar por 20 las tasas máximas de procesamiento de todos los receptores de una factoría. Esto reduce el número de transmisiones del protocolo de cliente de Service Bus.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Tema con un gran número de suscripciones

Objetivo: maximizar el rendimiento de un tema con un gran número de suscripciones. Muchas suscripciones reciben un mensaje, lo que significa que la velocidad de recepción combinada de todas las suscripciones es mucho mayor que la velocidad de envío. El número de remitentes es pequeño. El número de receptores por suscripción es pequeño.

Los temas con un gran número de suscripciones suelen ofrecer un rendimiento general bajo si todos los mensajes se enrutan a todas las suscripciones. Esto se debe al hecho de que cada mensaje se recibe muchas veces y todos los mensajes que se encuentran en un tema y en todas sus suscripciones se guardan en el mismo almacén. Se asume que el número de remitentes y el número de receptores por suscripción son pequeños. Service Bus admite hasta 2000 suscripciones por tema.

Para maximizar el rendimiento, haga lo siguiente:

* Use un tema con particiones para mejorar la disponibilidad y el rendimiento.
* Use operaciones asincrónicas para aprovechar el procesamiento por lotes del lado cliente.
* Use el intervalo predeterminado de procesamiento por lotes de 20 ms para reducir el número de transmisiones del protocolo de cliente de Service Bus.
* Deje habilitado el acceso al almacén de procesamiento por lotes. Esto aumenta la velocidad general con que se pueden escribir mensajes en el tema.
* Establezca el número de capturas previas en el valor resultante de multiplicar por 20 la tasa de recepción esperada en segundos. Esto reduce el número de transmisiones del protocolo de cliente de Service Bus.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo optimizar el rendimiento de Service Bus, consulte [Entidades de mensajería con particiones][Partitioned messaging entities].

[QueueClient]: /dotnet/api/microsoft.azure.servicebus.queueclient
[MessageSender]: /dotnet/api/microsoft.azure.servicebus.core.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[PeekLock]: /dotnet/api/microsoft.azure.servicebus.receivemode
[ReceiveAndDelete]: /dotnet/api/microsoft.azure.servicebus.receivemode
[BatchFlushInterval]: /dotnet/api/microsoft.servicebus.messaging.messagesender.batchflushinterval
[EnableBatchedOperations]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations
[QueueClient.PrefetchCount]: /dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount
[SubscriptionClient.PrefetchCount]: /dotnet/api/microsoft.azure.servicebus.subscriptionclient.prefetchcount
[ForcePersistence]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.forcepersistence
[EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[Partitioned messaging entities]: service-bus-partitioning.md
[TopicDescription.EnableFilteringMessagesBeforePublishing]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing
