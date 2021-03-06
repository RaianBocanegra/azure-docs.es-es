---
title: Reconfiguración de Integration Runtime de SSIS de Azure | Microsoft Docs
description: Aprenda a reconfigurar una instancia de Integration Runtime de SSIS de Azure en Azure Data Factory después de haberlo aprovisionado.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: ''
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: douglasl
ms.openlocfilehash: 9932ee862a9cdc7591c62c016e888d9e5d593cf7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2018
---
# <a name="manage-an-azure-ssis-integration-runtime"></a>Administración de una instancia de Integration Runtime de SSIS de Azure
En el artículo [Creación de una instancia de Integration Runtime de SSIS de Azure](create-azure-ssis-integration-runtime.md) se muestra cómo crear una instancia de Integration Runtime (IR) de SSIS de Azure mediante Azure Data Factory. En este artículo se proporciona información acerca de cómo volver a configurar una instancia de Integration Runtime de SSIS de Azure.  

> [!NOTE]
> Este artículo se aplica a la versión 2 de Data Factory, que actualmente se encuentra en versión preliminar. Si usa la versión 1 del servicio Data Factory, que está disponible con carácter general, vea la [documentación de Data Factory versión 1](v1/data-factory-introduction.md).


## <a name="data-factory-ui"></a>Interfaz de usuario de Data Factory 
Puede usar la interfaz de usuario de Data Factory para detener, editar y volver a configurar o eliminar un entorno de ejecución de integración de SSIS de Azure. 

1. En la **interfaz de usuario de Data Factory**, cambie a la pestaña **Edit** (Editar). Para iniciar la interfaz de usuario de Data Factory, haga clic en **Author & Monitor** (Creación y supervisión) en la página principal de su factoría de datos.
2. En el panel izquierdo, haga clic en **Connections** (Conexiones).
3. En el panel derecho, cambie a **Integration Runtimes** (Entornos de ejecución de integración). 
4. Puede usar los botones de la columna Actions (Acciones) para **detener**, **editar**, o **eliminar** el entorno de ejecución de integración. El botón **Code** (Código) de la columna **Actions** (Acciones) le permite ver la definición de JSON asociada con el entorno de ejecución de integración.  
    
    ![Acciones para entornos de ejecución de integración de SSIS de Azure](./media/manage-azure-ssis-integration-runtime/actions-for-azure-ssis-ir.png)

### <a name="to-reconfigure-an-azure-ssis-ir"></a>Para reconfigurar un entorno de ejecución de integración de SSIS de Azure
1. Haga clic en **Stop** (Detener) en la columna **Actions** (Acciones) para detener el entorno de ejecución de integración. Para actualizar la vista de lista, haga clic en **Refresh** (Actualizar) en la barra de herramientas. Una vez detenida la instancia de IR, verá que la primera acción le permite iniciar el entorno de ejecución de integración. 

    ![Acciones para entornos de ejecución de integración de SSIS de Azure después de la detención](./media/manage-azure-ssis-integration-runtime/actions-after-ssis-ir-stopped.png)
2. Edite o reconfigure el entorno de ejecución de integración, para lo que debe hacer clic en el botón **Edit** (Editar) de la columna **Actions** (Acciones). En la ventana **Integration Runtime Setup** (Configuración del entorno de ejecución), cambie la configuración (por ejemplo, el tamaño del nodo, el número de nodos o el máximo de ejecuciones en paralelo por nodo). 
3. Para reiniciar el entorno de ejecución de integración, haga clic en el botón **Start** (Start) de la columna **Actions** (Acciones).     

## <a name="azure-powershell"></a>Azure PowerShell
Después de aprovisionar e iniciar una instancia de Integration Runtime de SSIS de Azure, puede volver a configurarla mediante la ejecución de una secuencia de los cmdlets `Stop` - `Set` - `Start` de PowerShell de forma consecutiva. Por ejemplo, el siguiente script de PowerShell cambia el número de nodos asignados a la instancia de Integration Runtime de SSIS de Azure a cinco.

### <a name="reconfigure-an-azure-ssis-ir"></a>Reconfigurar una instancia de IR de SSIS de Azure

1. En primer lugar, detenga la instancia de Integration Runtime de SSIS de Azure con el cmdlet [Stop-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/stop-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1). Este comando libera todos sus nodos y detiene la facturación.

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
    ```
2. A continuación, reconfigure la instancia de IR de SSIS de Azure con el cmdlet [Set-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1). El siguiente comando de ejemplo permite escalar una instancia de Integration Runtime de SSIS de Azure a cinco nodos.

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
    ```  
3. A continuación, inicie la instancia de Integration Runtime de SSIS de Azure con el cmdlet [Start-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/start-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1). Este comando asigna todos sus nodos para ejecutar paquetes SSIS.   

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
    ```

### <a name="delete-an-azure-ssis-ir"></a>Eliminar una instancia de IR de SSIS de Azure
1. En primer lugar, cree una lista de todas las instancias de IR de SSIS de Azure en la factoría de datos.

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName -Status
    ```
2. A continuación, detenga todas las instancias de IR de SSIS de Azure existentes en la factoría de datos.

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
3. A continuación, quite todas las instancias de IR de SSIS de Azure existentes en la factoría de datos de una en una.

    ```powershell
    Remove-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
4. Por último, quite la factoría de datos.

    ```powershell
    Remove-AzureRmDataFactoryV2 -Name $DataFactoryName -ResourceGroupName $ResourceGroupName -Force
    ```
5. Si ha creado un nuevo grupo de recursos, quite el grupo de recursos.

    ```powershell
    Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force 
    ```

## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes temas para más información sobre Integration Runtime de SSIS de Azure: 

- [Integration Runtime de SSIS de Azure](concepts-integration-runtime.md#azure-ssis-integration-runtime). En este artículo se proporciona información conceptual acerca de Integration Runtime en general, lo que incluye Integration Runtime de SSIS de Azure. 
- [Tutorial: Implementación de paquetes SSIS en Azure](tutorial-create-azure-ssis-runtime-portal.md). En este artículo se proporcionan instrucciones paso a paso para crear una instancia de IR de SSIS de Azure y se usa una base de datos de SQL Azure para hospedar el catálogo de SSIS. 
- [How to: Create an Azure-SSIS integration runtime](create-azure-ssis-integration-runtime.md) (Creación de una instancia de Integration Runtime de SSIS de Azure). Este artículo amplía el tutorial y proporciona instrucciones sobre el uso de la instancia administrada de Azure SQL (versión preliminar) y la unión de Integration Runtime a una red virtual. 
- [Unión de Integration Runtime de SSIS de Azure a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md). En este artículo se proporciona información conceptual sobre cómo unir una instancia de Integration Runtime de SSIS de Azure a una red virtual de azure (VNet). También se proporcionan los pasos para configurar la red virtual mediante Azure Portal para que se una la instancia de Integration Runtime de SSIS de Azure. 
- [Monitor an Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime) (Supervisión de una instancia de Integration Runtime de SSIS de Azure). En este artículo se muestra cómo recuperar información sobre una instancia de IR de SSIS de Azure, junto con descripciones de los estados en la información devuelta. 
 
