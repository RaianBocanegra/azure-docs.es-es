---
title: Conexión de equipos Windows a Azure Log Analytics | Microsoft Docs
description: En este artículo se describe la manera de conectar equipos Windows hospedados en otras nubes o de forma local en Log Analytics mediante Microsoft Monitoring Agent (MMA).
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: magoedte
ms.openlocfilehash: 778810001952daf9ac63a7f1f880b05234549965
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2018
---
# <a name="connect-windows-computers-to-the-log-analytics-service-in-azure"></a>Conexión de equipos Windows al servicio Log Analytics de Azure

Con el fin de supervisar y administrar máquinas virtuales o equipos físicos en el centro de datos local o en otro entorno en la nube mediante Log Analytics, debe implementar Microsoft Monitoring Agent (MMA) y configurarlo para que informe a una o varias áreas de trabajo de Log Analytics.  Asimismo, el agente admite el rol de Hybrid Runbook Worker de Azure Automation.  

En un equipo Windows supervisado, el agente aparece como el servicio Microsoft Monitoring Agent. El servicio Microsoft Monitoring Agent se encarga de recopilar eventos de archivos de registro y del registro de eventos de Windows, datos de rendimiento y otros datos de telemetría. Incluso cuando el agente no puede comunicarse con el servicio Log Analytics al que debe informar, este continúa ejecutándose y pone en cola los datos recopilados en el disco del equipo supervisado. Cuando se restaura la conexión, el servicio Microsoft Monitoring Agent envía los datos recopilados al servicio.

El agente puede instalarse mediante uno de los métodos siguientes. La mayoría de las instalaciones usan una combinación de estos métodos para instalar los distintos conjuntos de equipos, según corresponda.  Más adelante en el artículo se proporcionan detalles sobre el uso de cada método.

* Instalación manual. El programa de instalación se ejecuta de forma manual en el equipo mediante el Asistente para la instalación, desde la línea de comandos, o bien se implementa mediante una herramienta de distribución de software existente.
* Desired State Configuration (DSC) de Azure Automation. Use DSC de Azure Automation con un script para equipos Windows que ya esté implementado en su entorno.  
* Script de PowerShell.
* Plantilla de Resource Manager para máquinas virtuales que ejecutan Windows de forma local en Azure Stack.  

Para comprender los requisitos de red y del sistema para implementar el agente de Windows, revise los [requisitos previos para equipos Windows](log-analytics-concept-hybrid.md#prerequisites).

## <a name="obtain-workspace-id-and-key"></a>Obtención de la clave y el identificador de área de trabajo
Antes de instalar Microsoft Monitoring Agent para Windows, necesita la clave y el identificador de área de trabajo para el área de trabajo de Log Analytics.  Esta información es necesaria durante la configuración de cada método de instalación para configurar de forma correcta el agente y garantizar que puede comunicarse correctamente con Log Analytics en la nube Gobierno de EE. UU. y comercial de Azure.  

1. En Azure Portal, haga clic en **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.
2. En la lista de áreas de trabajo de Log Analytics, seleccione el área de trabajo que quiera configurar para informar al agente.
3. Seleccione **Configuración avanzada**.<br><br> ![Configuración avanzada de Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Seleccione **Orígenes conectados** y **Servidores Windows**.   
5. En su editor favorito, copie y pegue **Id. del área de trabajo** y **Clave principal**.    
   
## <a name="install-the-agent-using-setup-wizard"></a>Instalación del agente con el asistente para instalación
Los siguientes pasos permiten la instalación y configuración de Log Analytics en Azure y en la nube de Azure Government mediante el asistente para configuración para Microsoft Monitoring Agent en su equipo.  

1. En el área de trabajo de Log Analyics, en la página **Servidores Windows** a la que navegó anteriormente, seleccione la versión **Descargar el agente de Windows** que desee descargar según la arquitectura del procesador del sistema operativo Windows.   
2. Ejecute el programa de instalación para instalar al agente en el equipo.
2. En la página **principal**, haga clic en **Siguiente**.
3. En la página **Términos de licencia**, lea la licencia y haga clic en **Acepto**.
4. En la página e **Carpeta de destino**, cambie o mantenga la carpeta de instalación predeterminada y haga clic en **Siguiente**.
5. En la página **Opciones de instalación del agente**, elija la opción de conectar el agente a Azure Log Analytics (OMS) y, luego, haga clic en **Siguiente**.   
6. En la página **Azure Log Analytics**, realice lo siguiente:
   1. Pegue el **Id. del área de trabajo** y la **clave del área de trabajo (clave principal)** que copió anteriormente.  Si el equipo tiene que notificar a un área de trabajo de Log Analytics en Azure Government Cloud, seleccione **Azure Gobierno de EE.UU.** desde la lista desplegable **Azure Cloud**.  
   2. Si el equipo necesita comunicarse a través de un servidor proxy con el servicio de Log Analytics, haga clic en **Avanzado** y proporcione la dirección URL y el número de puerto del servidor proxy.  Si el servidor proxy requiere autenticación, escriba el nombre de usuario y la contraseña para autenticar con el servidor proxy y, luego, haga clic en **Siguiente**.  
7. Haga clic en **Siguiente** cuando haya terminado de proporcionar las opciones de configuración necesarias.<br><br> ![pegar identificador del área de trabajo y clave principal](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-setup-laworkspace.png)<br><br>
8. En la página **Preparado para instalar**, revise las opciones seleccionadas y haga clic en **Instalar**.
9. En la página **La configuración finalizó correctamente**, haga clic en **Finalizar**.

Una vez completado el proceso, el **Agente de administración de Microsoft** aparece en el **Panel de control**. Para confirmar que está informando a Log Analytics, revise [Comprobar la conectividad del agente con Log Analytics](#verify-agent-connectivity-to-log-analytics). 

## <a name="install-the-agent-using-the-command-line"></a>Instalación del agente a través de la línea de comandos
El archivo descargado del agente es un paquete de instalación independiente.  El programa de instalación del agente y los archivos auxiliares se encuentran en el paquete y deben extraerse para realizar la instalación correctamente mediante la línea de comandos que se muestra en los ejemplos siguientes.    

>[!NOTE]
>Si desea actualizar a un agente, debe utilizar la API de scripting de Log Analytics. Si quiere obtener más información, consulte [Administrar y mantener el agente de Log Analytics para Windows y Linux](log-analytics-agent-manage.md).

En la tabla siguiente se resaltan los parámetros de Log Analytics específicos que admite el programa de instalación del agente, aunque la implementación se haya realizado mediante DSC de Automatización.

|Opciones específicas de MMA                   |Notas         |
|---------------------------------------|--------------|
| NOAPM=1                               | Parámetro opcional. Instala al agente sin la supervisión de rendimiento de aplicaciones .NET.|   
|ADD_OPINSIGHTS_WORKSPACE               | 1 = Configurar el agente para informar a un área de trabajo                |
|OPINSIGHTS_WORKSPACE_ID                | Id. de área de trabajo (guid) para el área de trabajo que se agregará                    |
|OPINSIGHTS_WORKSPACE_KEY               | Clave del área de trabajo que se usa para autenticar inicialmente con el área de trabajo |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Especificar el entorno en la nube donde se encuentra el área de trabajo <br> 0 = nube comercial de Azure (valor predeterminado) <br> 1= Azure Government |
|OPINSIGHTS_PROXY_URL               | Identificador URI del proxy que se va a usar |
|OPINSIGHTS_PROXY_USERNAME               | Nombre de usuario para acceder a un proxy autenticado |
|OPINSIGHTS_PROXY_PASSWORD               | Contraseña para acceder a un proxy autenticado |

1. Para extraer los archivos de instalación del agente, desde un símbolo del sistema con privilegios elevados ejecute `MMASetup-<platform>.exe /c` y se le solicitará la ruta en la que extraer los archivos.  Como alternativa, puede especificar la ruta de acceso pasando los argumentos `MMASetup-<platform>.exe /c /t:<Path>`.  
2. Para instalar el agente de forma silenciosa y configurarlo para que informe a un área de trabajo en la nube comercial de Azure, vaya a la carpeta en la que extrajo los archivos de instalación y escriba: 
   
     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=0 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
    ```

   o bien, para configurar el agente para que informe a la nube de la administración pública de EE. UU., escriba: 

     ```dos
    setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
    ```

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Instalación del agente utilizando DSC en Azure Automation

Puede usar el siguiente script de ejemplo para instalar el agente mediante DSC de Azure Automation.   Si no tiene una cuenta de Automation, consulte [Introducción a Azure Automation](../automation/automation-offering-get-started.md) para comprobar los requisitos y los pasos necesarios para crear una cuenta de Automation antes de poder usar DSC de Automatización.  Si no está familiarizado con DSC de Automatización, consulte [Introducción a DSC de Automatización](../automation/automation-dsc-getting-started.md).

En el ejemplo siguiente se instala el agente de 64 bits, identificado mediante el valor `URI`. También puede utilizar la versión de 32 bits si se reemplaza el valor del identificador URI. Estos son los identificadores URI para ambas versiones:

- Agente de Windows de 64 bits: https://go.microsoft.com/fwlink/?LinkId=828603
- Agente de Windows de 32 bits: https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
>Este procedimiento y el script de ejemplo no admiten la actualización del agente que ya está implementado en un equipo Windows.

Las versiones de 32 y 64 bits del paquete de agente tienen códigos de producto diferentes y las nuevas versiones publicadas también tienen un valor único.  El código de producto es un GUID, que es la identificación principal de una aplicación o un producto, y se representa mediante la propiedad **ProductCode** de Windows Installer.  El `ProductId value` del script **MMAgent.ps1** tiene que coincidir con el código de producto del paquete de instalador del agente de 32 o 64 bits.

Para recuperar el código de producto del paquete para instalar el agente directamente, puede usar Orca.exe desde la página [Windows SDK Components for Windows Installer Developers](https://msdn.microsoft.com/library/windows/desktop/aa370834%28v=vs.85%29.aspx) (Componentes del SDK de Windows para desarrolladores de Windows Installer), donde encontrará un componente del Kit de desarrollo de software de Windows, o mediante PowerShell y un [script de ejemplo](http://www.scconfigmgr.com/2014/08/22/how-to-get-msi-file-information-with-powershell/) que haya escrito un MVP (profesional más valioso) de Microsoft.  Para cualquier enfoque, primero debe extraer el archivo **MOMagent.msi** del paquete de instalación MMASetup.  Esto se explicó anteriormente en el primero paso de la sección [Instalación del agente a través de la línea de comandos](#install-the-agent-using-the-command-line).  

1. Importe el módulo DSC xPSDesiredStateConfiguration de [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) en Azure Automation.  
2.  En Azure Automation, cree los recursos de variable *OPSINSIGHTS_WS_ID* y *OPSINSIGHTS_WS_KEY*. Establezca *OPSINSIGHTS_WS_ID* en el identificador del área de trabajo de Log Analytics y *OPSINSIGHTS_WS_KEY* en la clave principal del área de trabajo.
3.  Copie el script y guárdelo como MMAgent.ps1.

    ```PowerShell
    Configuration MMAgent
    {
        $OIPackageLocalPath = "C:\Deploy\MMASetup-AMD64.exe"
        $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
        $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"

        Import-DscResource -ModuleName xPSDesiredStateConfiguration
        Import-DscResource –ModuleName PSDesiredStateConfiguration

        Node OMSnode {
            Service OIService
            {
                Name = "HealthService"
                State = "Running"
                DependsOn = "[Package]OI"
            }

            xRemoteFile OIPackage {
                Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
                DestinationPath = $OIPackageLocalPath
            }

            Package OI {
                Ensure = "Present"
                Path  = $OIPackageLocalPath
                Name = "Microsoft Monitoring Agent"
                ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
                Arguments = '/C:"setup.exe /qn NOAPM=1 ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
                DependsOn = "[xRemoteFile]OIPackage"
            }
        }
    }

    ```

4. [Importe el script de configuración MMAgent.ps1](../automation/automation-dsc-getting-started.md#importing-a-configuration-into-azure-automation) en su cuenta de Automation. 
5. [Asigne un equipo Windows o un nodo](../automation/automation-dsc-getting-started.md#onboarding-an-azure-vm-for-management-with-azure-automation-dsc) a la configuración. Durante los 15 minutos siguientes, el nodo comprueba la configuración y el agente se inserta en el nodo.

## <a name="verify-agent-connectivity-to-log-analytics"></a>Comprobación de la conectividad del agente a Log Analytics

Una vez completada la instalación del agente, compruebe que se ha conectado correctamente y que la creación de informes puede realizarse desde ambos lados.  

En el **Panel de control** del equipo, busque el elemento **Microsoft Monitoring Agent**.  Cuando lo seleccione, en la pestaña **Azure Log Analytics (OMS)**, el agente mostrará un mensaje similar al siguiente: **Microsoft Monitoring Agent se conectó correctamente al servicio Microsoft Operations Management Suite.**<br><br> ![Estado de la conexión de MMA en Log Analytics](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-laworkspace-status.png)

También puede realizar una búsqueda de registros simple en Azure Portal.  

1. En Azure Portal, haga clic en **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.  
2. En la página del área de trabajo de Log Analytics, seleccione el área de trabajo de destino y, a continuación, seleccione el icono **Búsqueda de registros**. 
2. Escriba lo siguiente en el campo de consulta del panel Búsqueda de registros:  

    ```
    search * 
    | where Type == "Heartbeat" 
    | where Category == "Direct Agent" 
    | where TimeGenerated > ago(30m)  
    ```

En los resultados de la búsqueda devueltos, verá los registros de latido del equipo que indican que está conectado y que está creando informes para el servicio.   

## <a name="next-steps"></a>Pasos siguientes

Lea [Administrar y mantener el agente de Log Analytics para Windows y Linux](log-analytics-agent-manage.md) para obtener información acerca de cómo administrar el agente durante su ciclo de vida de implementación en las máquinas.  