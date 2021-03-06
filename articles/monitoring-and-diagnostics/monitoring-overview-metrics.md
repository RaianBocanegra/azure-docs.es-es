---
title: Información general sobre las métricas en Microsoft Azure | Microsoft Docs
description: Información general sobre las métricas y su uso en Microsoft Azure
author: anirudhcavale
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 405ec51c-0946-4ec9-b535-60f65c4a5bd1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2018
ms.author: ancav
ms.openlocfilehash: 537213fdf106da1c07d549d65b1d8cf71887db9f
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Información general sobre las métricas en Microsoft Azure
En este artículo se explica qué son las métricas en Microsoft Azure, sus beneficios y cómo comenzar a utilizarlas.  

## <a name="what-are-metrics"></a>¿Qué son las métricas?
Azure Monitor permite utilizar telemetría para obtener información sobre el rendimiento y el estado de las cargas de trabajo en Azure. El tipo de telemetría de datos de Azure más importante son las métricas (también denominadas contadores de rendimiento) emitidas por la mayoría de los recursos de Azure. Azure Monitor proporciona varias maneras de configurar y usar estas métricas para supervisar y solucionar problemas.

## <a name="what-can-you-do-with-metrics"></a>¿Para qué sirven las métricas?
Las métricas son una valiosa fuente de telemetría y le permiten realizar las siguientes tareas:

* **Realizar un seguimiento del rendimiento** del recurso (por ejemplo, una máquina virtual, un sitio web o una aplicación lógica) mediante el trazado de las métricas en un gráfico de portal y anclando ese gráfico a un panel.
* **Recibir una notificación de un problema** que afecta al rendimiento del recurso cuando una métrica cruza un umbral determinado.
* **Configurar acciones automatizadas**, como escalar automáticamente un recurso o activar un runbook cuando una métrica cruza un umbral determinado.
* **Realizar análisis avanzados** o elaborar informes de tendencias de rendimiento o de uso de los recursos.
* **Archivar** el historial de rendimiento o estado de los recursos **para fines de cumplimiento y auditoría**.

## <a name="what-are-the-characteristics-of-metrics"></a>¿Cuáles son las características de las métricas?
Las métricas presentan las características siguientes:

* Todas las métricas tienen una **frecuencia de un minuto** (a menos que se especifique lo contrario en la definición de una métrica). Recibe un valor de métrica del recurso cada minuto, lo que proporciona visibilidad en tiempo real del estado y la integridad del recurso.
* Las métricas están **disponibles de forma inmediata**. No tiene que habilitarlas ni configurar diagnósticos adicionales.
* Puede obtener acceso a **93 días del historial** de cada métrica. Puede buscar rápidamente las tendencias recientes y mensuales en el rendimiento o el estado del recurso.
* Algunas métricas pueden tener atributos de par nombre-valor denominado **dimensiones**. Estas le permiten segmentar aún más y explorar una métrica de una manera más significativa.

También puede:

* Configurar una **regla de alerta de métrica que envía una notificación o realiza una acción automatizada** cuando la métrica cruza el umbral establecido. El escalado automático es una acción automatizada especial que permite escalar horizontalmente los recursos para satisfacer las solicitudes entrantes o las cargas en el sitio web o los recursos de procesos. Puede definir una regla de configuración de escalado automático para escalar o reducir horizontalmente si una métrica supera un umbral.

* **Enrute** todas las métricas a Application Insights o Log Analytics para habilitar análisis instantáneos, búsquedas y alertas personalizadas de los datos de métricas de los recursos. También puede transmitir métricas a un centro de eventos, lo que permite redirigirlas a Azure Stream Analytics o a aplicaciones personalizadas para realizar análisis casi en tiempo real. Puede configurar la transmisión del centro de eventos con la configuración de diagnóstico.

* **Archivar métricas en almacenamiento** para una retención más prolongada o para crear informes sin conexión. Puede redirigir las métricas a Azure Blob Storage al configurar opciones de diagnóstico para el recurso.

* Detectar fácilmente todas las métricas, acceder a ellas y **verlas** en Azure Portal al seleccionar un recurso y trazarlo en un gráfico.

* **Consumir** las métricas a través de las nuevas API de REST de Azure Monitor.

* **Consultar** métricas con los cmdlets de PowerShell o la API de REST multiplataforma.

  ![Enrutamiento de métricas en Azure Monitor](./media/monitoring-overview-metrics/Metrics_Overview_v4.png)

## <a name="access-metrics-via-the-portal"></a>Acceso a métricas a través del portal
A continuación, mostramos un breve tutorial sobre cómo crear un gráfico de métricas mediante Azure Portal.

### <a name="to-view-metrics-after-creating-a-resource"></a>Visualización de métricas después de crear un recurso
1. Abra Azure Portal.
2. Cree un sitio web de Azure App Service.
3. Después de crear un sitio web, vaya a la hoja **Información general** del sitio web.
4. Puede ver las nuevas métricas como un icono de **Supervisión**. Además, podrá editar el icono y seleccionar más métricas.

   ![Métricas de un recurso en Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview1.png)

### <a name="to-access-all-metrics-in-a-single-place"></a>Acceso a todas las métricas en un solo lugar
1. Abra Azure Portal.
2. Vaya a la nueva pestaña **Supervisión** y seleccione en ella la opción **Métricas**.
3. Seleccione la suscripción, el grupo de recursos y el nombre del recurso en la lista desplegable.
4. Vea la lista de métricas disponibles. Luego, seleccione la métrica que le interesa y trácela.
5. Puede anclarla al panel haciendo clic en la opción para anclar que se encuentra en la esquina superior derecha.

   ![Acceso a todas las métricas en un solo lugar en Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview2.png)

> [!NOTE]
> Puede acceder a las métricas en el nivel de host de máquinas virtuales (basadas en Azure Resource Manager) y conjuntos de escalas de máquina virtual sin ninguna configuración adicional de diagnóstico. Estas nuevas métricas a nivel de host están disponibles para las instancias de Windows y Linux. Estas métricas no deben confundirse con las de nivel de SO invitado a las que se accede al activar Diagnósticos de Azure en máquinas virtuales o conjuntos de escalas de máquina virtual. Para obtener más información sobre la configuración de Diagnostics, vea [¿Qué es Microsoft Azure Diagnostics?](../azure-diagnostics.md)
>
>

Azure Monitor también tiene una nueva experiencia de gráficos de métricas disponibles en versión preliminar. Esta experiencia permite a los usuarios superponer métricas de varios recursos en un gráfico. Los usuarios también pueden trazar, segmentar y filtrar métricas multidimensionales con esta nueva experiencia de gráficos de métricas. Para obtener más información, [haga clic aquí](https://aka.ms/azuremonitor/new-metrics-charts).

## <a name="access-metrics-via-the-rest-api"></a>Acceso a métricas a través de la API de REST
Se puede acceder a las métricas de Azure a través de las API de Azure Monitor. Hay dos API que facilitan la detección de métricas y el acceso a ellas:

* Utilice la [API de REST de definiciones de métricas de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) para acceder a la lista de métricas, y las dimensiones, disponibles para un servicio.
* Use la [API de REST de métricas de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/metrics) para segmentar y filtrar los datos de métricas reales y acceder a estos.

> [!NOTE]
> En este artículo se tratan las métricas a través de la [API nueva para las métricas](https://docs.microsoft.com/rest/api/monitor/) para recursos de Azure. La versión de API para las nuevas definiciones de métricas y las API de métricas es 2018-01-01. Se puede acceder a las métricas y definiciones de métricas heredadas con la versión de API 2014-04-01.
>
>

Para ver un tutorial más detallado mediante las API de REST de Azure Monitor, vea el [Tutorial sobre la API de REST de Azure Monitor](monitoring-rest-api-walkthrough.md).

## <a name="export-metrics"></a>Exportación de métricas
Puede ir a la hoja **Configuración de diagnóstico** de la pestaña **Supervisión** y ver las opciones de exportación de métricas. Puede seleccionar las métricas (y los registros de diagnóstico) para redirigirlas a Blob Storage, Event Hubs o Log Analytics para aquellos casos de uso que se han mencionado anteriormente en este artículo.

 ![Opciones de exportación de métricas en Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview3.png)

Puede configurar esto a través de plantillas de Resource Manager, [PowerShell](insights-powershell-samples.md), la [CLI de Azure](insights-cli-samples.md) o las [API de REST](https://msdn.microsoft.com/library/dn931943.aspx).

> [!NOTE]
> Actualmente no se admite el envío de métricas de varias dimensiones a través de la configuración de diagnóstico. Las métricas con dimensiones se exportan como métricas unidimensionales planas agregadas a través de los valores de dimensión.
>
> *Por ejemplo*: la métrica "Mensajes entrantes" de una instancia de Event Hub se puede explorar y representar gráficamente por colas. Sin embargo, cuando se exporta a través de la configuración de diagnóstico, la métrica se representarán como todos los mensajes entrantes de todas las colas de Event Hub.
>
>

## <a name="take-action-on-metrics"></a>Realización de acciones en las métricas
Para recibir notificaciones o realizar acciones automatizadas en datos métricos, puede configurar reglas de alerta o los ajustes de escalado automático.

### <a name="configure-alert-rules"></a>Configuración de reglas de alerta
Puede configurar reglas de alerta basadas en las métricas. Estas reglas de alerta pueden comprobar si una métrica ha superado un umbral determinado. Azure Monitor ofrece dos funciones de alertas de métricas.

Alertas de métricas: estas pueden enviarle una notificación por correo electrónico o activar un webhook que podrá utilizar para ejecutar un script personalizado. También puede utilizar el webhook para configurar integraciones de productos de terceros.

 ![Métricas y reglas de alerta en Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview4.png)

Las alertas de métricas más recientes tienen la capacidad de supervisar varias métricas, además de umbrales, para un recurso y luego le envían una notificación a través de un [grupo de acciones](/monitoring-action-groups.md). Obtenga más información sobre las [alertas más recientes aquí](https://aka.ms/azuremonitor/near-real-time-alerts).


### <a name="autoscale-your-azure-resources"></a>Escalado automático de recursos de Azure
Algunos recursos de Azure pueden escalar o reducir horizontalmente varias instancias para controlar las cargas de trabajo. El escalado automático se aplica a App Service (Web Apps), conjuntos de escalas de máquina virtual y servicios Cloud Services de Azure clásicos. Puede configurar reglas de escalado automático para escalar o reducir horizontalmente cuando una métrica determinada que afecta a su carga de trabajo supere un umbral especificado. Para obtener más información, vea la [información general sobre la funcionalidad de escalado automático](monitoring-overview-autoscale.md).

 ![Métricas y escalado automático en Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="learn-about-supported-services-and-metrics"></a>Más información sobre los servicios y las métricas compatibles
Puede ver una lista detallada de todos los servicios admitidos y sus métricas en [Métricas de Azure Monitor: métricas admitidas por tipo de recurso](monitoring-supported-metrics.md).

## <a name="next-steps"></a>Pasos siguientes
Consulte los vínculos de este artículo. Además, puede obtener información sobre los siguientes temas:  

* [Métricas comunes de escalado automático](insights-autoscale-common-metrics.md)
* [Cómo crear reglas de alertas](insights-alerts-portal.md)
* [Análisis de registros desde Azure Storage con Log Analytics](../log-analytics/log-analytics-azure-storage.md)
