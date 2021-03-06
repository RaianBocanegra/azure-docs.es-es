---
title: "Controles de aplicación adaptables en Azure Security Center | Microsoft Docs"
description: "Este documento le ayuda a usar el control de aplicación adaptable en Azure Security Center para incluir en la lista de permitidos las aplicaciones que se ejecutan en máquinas virtuales de Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/31/2018
ms.author: yurid
ms.openlocfilehash: ee15b602dc90b0e777b7ccd29572b9d560ee719b
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2018
---
# <a name="adaptive-application-controls-in-azure-security-center-preview"></a>Controles de aplicación adaptables en Azure Security Center (versión preliminar)
Obtenga información acerca de cómo configurar el control de aplicación en Azure Security Center con este tutorial.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>¿Qué son los controles de aplicación adaptables en Security Center?
Los controles de aplicación adaptables ayudan a controlar qué aplicaciones se pueden ejecutar en las máquinas virtuales que se encuentran en Azure y, entre otras ventajas, le ayuda a proteger las máquinas virtuales frente a malware. Security Center usa el aprendizaje automático para analizar los procesos que se ejecutan en la máquina virtual y le ayuda a aplicar reglas de inclusión en listas de permitidos con esta inteligencia. Esta funcionalidad simplifica enormemente el proceso de configuración y mantenimiento de listas de aplicaciones permitidas, lo que le permite:

- Bloquear o alertar sobre intentos de ejecución de aplicaciones malintencionadas, incluidas aquellas que podrían ser omitidas por las soluciones antimalware
- Cumplir con la directiva de seguridad de la organización que dicta el uso de software con licencia únicamente.
- Evitar el uso de software no deseado en el entorno.
- Evitar la ejecución de aplicaciones anteriores y no compatibles.
- Impedir herramientas de software específicas no permitidas en la organización.
- Permiten que TI controle el acceso a información confidencial a través del uso de aplicaciones.

## <a name="how-to-enable-adaptive-application-controls"></a>¿Cómo habilitar los controles de aplicación adaptables?
Los controles de aplicación adaptables ayudan a definir un conjunto de aplicaciones que se pueden ejecutar en los grupos de recursos configurados. Esta característica solo está disponible para máquinas de Windows (todas las versiones, clásica o Azure Resource Manager). Los pasos siguientes se pueden usar para configurar la inclusión de aplicaciones en las listas de permitidos de Security Center:

1. Abra el panel **Security Center**.
2. En el panel de la izquierda, seleccione **Controles de aplicaciones adaptables** que se encuentra en **Protección en la nube avanzada**.

    ![Defensa](./media/security-center-adaptive-application/security-center-adaptive-application-fig1-new.png)

Se muestra la página **Controles de aplicaciones adaptables**.

![controls](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

La sección **Grupos de recursos** contiene tres pestañas:

* **Configurados**: lista de grupos de recursos con máquinas virtuales configuradas con control de aplicación.
* **Recomendados**: lista de grupos de recursos para los que se recomienda el control de aplicación. Security Center utiliza el aprendizaje automático para identificar las máquinas virtuales que son buenas candidatas para el control de aplicación en función de si las máquinas virtuales ejecutan de forma coherente las mismas aplicaciones.
* **Ninguna recomendación**: lista de grupos de recursos que contienen máquinas virtuales sin ninguna recomendación sobre el control de aplicación. Por ejemplo, máquinas virtuales en las que las aplicaciones cambian constantemente y no han alcanzado un estado estable.

### <a name="configure-a-new-application-control-policy"></a>Configuración de una nueva directiva de control de aplicación
1. Haga clic en la pestaña **Recomendados** para obtener una lista de grupos de recursos con las recomendaciones de control de aplicación:

  ![Recomendado](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

  La lista incluye:

  - **NOMBRE**: el nombre de la suscripción y el grupo de recursos
  - **MÁQUINAS VIRTUALES**: el número de máquinas virtuales en el grupo de recursos
  - **ESTADO**: el estado de las recomendaciones, que en la mayoría de los casos será abierto
  - **GRAVEDAD**: el nivel de gravedad de las recomendaciones

2. Seleccione un grupo de recursos para abrir la opción **Crear reglas de control de aplicaciones**.

  ![Reglas de control de aplicación](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

3. En **Seleccionar máquinas virtuales**, revise la lista de máquinas virtuales recomendadas y desactive cualquiera a la que no desee aplicar el control de aplicación. En **Seleccionar procesos para reglas de inclusión en lista blanca**, revise la lista de aplicaciones recomendadas y desactive aquellas a las que no desea que se apliquen. La lista incluye:

  - **NOMBRE**: la ruta de acceso completa de la aplicación
  - **PROCESOS**: el número de aplicaciones que residen dentro de cada ruta de acceso
  - **COMÚN**: "Yes" indica que estos procesos se han ejecutado en la mayoría de las máquinas virtuales de este grupo de recursos.
  - **INFRINGIBLE**: un icono de advertencia indica si un atacante podría usar las aplicaciones para evitar las listas de aplicaciones permitidas. Se recomienda revisar estas aplicaciones antes de su aprobación.
  - **USUARIOS**: los usuarios pueden ejecutar la aplicación

4. Cuando haya terminado de realizar las selecciones, elija **Crear**.

De forma predeterminada, Security Center siempre habilita el control de aplicación en modo *Auditoría*. Después de comprobar que la lista de permitidos no tenga efectos negativos en la carga de trabajo, puede cambiar al modo *Forzar*.

Security Center necesita al menos dos semanas de datos para crear una base de referencia y rellenar las recomendaciones únicas para cada grupo de máquinas virtuales. Los clientes nuevos del nivel estándar de Security Center experimentarán un comportamiento en el que, al principio, sus grupos de máquinas virtuales aparecen en la pestaña *Ninguna recomendación*.

> [!NOTE]
> Como procedimiento recomendado de seguridad, Security Center siempre intenta crear una regla de publicador para las aplicaciones que deben estar en la lista de permitidos y solo si una aplicación no tiene información del publicador (también conocido como no firmada), se creará una regla de ruta de acceso para la ruta de acceso completa del archivo EXE específico.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Edición y supervisión de un grupo configurado con control de aplicación

1. Para modificar y supervisar un grupo configurado con control de la aplicación, vuelva a la página **Controles de aplicación adaptables** y seleccione **CONFIGURADO** en **Grupos de recursos**:

  ![Grupos de recursos](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

  La lista incluye:

  - **NOMBRE**: el nombre de la suscripción y el grupo de recursos
  - **MÁQUINAS VIRTUALES**: el número de máquinas virtuales en el grupo de recursos
  - **MODO**: el modo Auditoría registrará los intentos de ejecución de aplicaciones que no están en la lista de permitidos; el modo Bloquear no permitirá la ejecución de aplicaciones que no están en la lista de permitidos.
  - **PROBLEMAS**: cualquier infracción actual

2. Seleccione un grupo de recursos para realizar cambios en la página **Editar directiva de control de aplicación**.

  ![Protección](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

3. En **Modo de protección**, puede seleccionar entre las siguientes opciones:

  - **Auditoría**: en este modo, la solución de control de aplicación no exige el cumplimiento de las reglas y solo audita la actividad en las máquinas virtuales protegidas. Esto se recomienda para escenarios donde desea primero observar el comportamiento general antes de bloquear la ejecución de una aplicación en la máquina virtual de destino.
  - **Forzar**: en este modo, la solución de control de aplicación exige el cumplimiento de las reglas y se asegura de bloquear las aplicaciones cuya ejecución no está permitida.

  Como se mencionó anteriormente, una nueva directiva de control de aplicación siempre se configura en modo *Auditoría* de forma predeterminada. En **Extensión de directiva** puede agregar sus propias rutas de acceso de aplicación que desea incluir en la lista de permitidos. Una vez que agrega estas rutas de acceso, Security Center crea las reglas adecuadas para estas aplicaciones, además de las que ya están configuradas.

  En la sección **Problemas recientes** se enumeran las infracciones actuales.

  ![Issues](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

  Esta lista incluye:
  - **PROBLEMAS**: cualquier infracción que se haya registrado, incluidas las siguientes:

      - **ViolationsBlocked**: cuando la solución está activada en modo Exigir y se intenta ejecutar una aplicación que no está en la lista de permitidos.
      - **ViolationsAudited**: cuando la solución está activada en modo Auditoría y se ejecuta una aplicación que no está en la lista de permitidos.
      - **RulesViolatedManually**: cuando un usuario ha intentado configurar manualmente reglas en las máquinas virtuales y no mediante el portal de administración de ASC.

 - **NÚMERO DE MÁQUINAS VIRTUALES**: número de máquinas virtuales con este tipo de problema.

  Si hace clic en cada línea, se le redirige a la página [Registro de actividad de Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), donde puede ver información acerca de todas las máquinas virtuales con este tipo de infracción. Si hace clic en los tres puntos al final de cada línea, puede eliminar esa entrada en concreto. En la sección **Máquinas virtuales configuradas** se enumeran las máquinas virtuales a la que se aplican las reglas.

  ![Máquinas virtuales configuradas](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

  En **Reglas de inclusión en lista aprobada de publicadores**, la lista contiene:

  - **REGLA**: aplicaciones para las que se ha creado una regla de publicador en función de la información del certificado que se ha encontrado para cada aplicación
  - **USUARIOS**: número de usuarios con permiso para ejecutar cada aplicación

  Consulte [Descripción de las reglas de publicador en Applocker](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker) para más información.

  ![Reglas de inclusión en listas de permitidos](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

  Si hace clic en los tres puntos al final de cada línea, puede eliminar esa regla específica o modificar los usuarios permitidos.

  La sección **Reglas de inclusión en lista aprobada de rutas** muestra la ruta de acceso completa (incluido el archivo ejecutable) de las aplicaciones que no están firmadas con un certificado digital, pero todavía están en las reglas de las listas de rutas de acceso permitidas actuales.

  > [!NOTE]
  > De forma predeterminada, como procedimiento recomendado de seguridad, Security Center siempre intenta crear una regla de publicador para los archivos EXE que deben estar en la lista de permitidos, y solo si un archivo EXE no tiene información del publicador (también conocido como no firmado), se creará una regla de ruta de acceso para la ruta de acceso completa del archivo EXE específico.

  ![Reglas de inclusión en listas de rutas de acceso permitidas](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

  La lista contiene:
  - **NOMBRE**: la ruta de acceso completa del archivo ejecutable
  - **USUARIOS**: número de usuarios con permiso para ejecutar cada aplicación

  Si hace clic en los tres puntos al final de cada línea, puede eliminar esa regla específica o modificar los usuarios permitidos.

4. Después de realizar cambios en la página **Controles de aplicación adaptables**, haga clic en el botón **Guardar**. Si decide no aplicar los cambios, haga clic en **Descartar**.

### <a name="not-recommended-list"></a>Lista Ninguna recomendación

Security Center solo recomienda las listas de aplicaciones permitidas para las máquinas virtuales que ejecutan un conjunto estable de aplicaciones. No se crean recomendaciones si las aplicaciones en las máquinas virtuales asociadas cambian continuamente.

![Recomendación](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

La lista contiene:
- **NOMBRE**: el nombre de la suscripción y el grupo de recursos
- **MÁQUINAS VIRTUALES**: el número de máquinas virtuales en el grupo de recursos

## <a name="next-steps"></a>Pasos siguientes
En este documento ha aprendido a usar el control de aplicación adaptable en Azure Security Center para incluir en la lista de permitidos las aplicaciones que se ejecutan en máquinas virtuales de Azure. Para obtener más información sobre el Centro de seguridad de Azure, consulte los siguientes recursos:

* [Administración y respuesta a las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Aprenda a administrar las alertas y responder a incidentes de seguridad en Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md). Aprenda a supervisar el estado de los recursos de Azure.
* [Comprensión de las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Obtenga información acerca de los distintos tipos de alertas de seguridad.
* [Guía de solución de problemas de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Obtenga información acerca de cómo solucionar problemas comunes en Security Center.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md). Preguntas más frecuentes acerca del uso del servicio.
* [Blog de seguridad de Azure](http://blogs.msdn.com/b/azuresecurity/). Encuentre artículos de blog sobre el cumplimiento y la seguridad de Azure.
