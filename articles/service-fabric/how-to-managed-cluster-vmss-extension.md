---
title: Aggiungere un'estensione del set di scalabilità di macchine virtuali a un tipo di nodo del cluster gestito Service Fabric (anteprima)
description: Di seguito viene illustrato come aggiungere un'estensione del set di scalabilità di macchine virtuali a un tipo di nodo del cluster gestito Service Fabric
ms.topic: article
ms.date: 09/28/2020
ms.openlocfilehash: 64df4b82795f382e176d66dc61470296447b9e29
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98788079"
---
# <a name="add-a-virtual-machine-scale-set-extension-to-a-service-fabric-managed-cluster-node-type-preview"></a>Aggiungere un'estensione del set di scalabilità di macchine virtuali a un tipo di nodo del cluster gestito Service Fabric (anteprima)

Ogni tipo di nodo in un cluster gestito da Service Fabric è supportato da un set di scalabilità di macchine virtuali. In questo modo è possibile aggiungere [estensioni del set di scalabilità di macchine virtuali](../virtual-machines/extensions/overview.md) ai tipi di nodo del cluster gestito Service Fabric.

È possibile aggiungere un'estensione del set di scalabilità di macchine virtuali a un tipo di nodo usando il comando di PowerShell [Add-AzServiceFabricManagedNodeTypeVMExtension](/powershell/module/az.servicefabric/add-azservicefabricmanagednodetypevmextension) .

In alternativa, è possibile un'estensione del set di scalabilità di macchine virtuali in un Service Fabric tipo di nodo del cluster gestito nel modello di Azure Resource Manager, ad esempio:

```json
{
    "type": "Microsoft.ServiceFabric/managedclusters/nodetypes",
    "apiVersion": "[variables('sfApiVersion')]",
    "name": "[concat(parameters('clusterName'), '/', parameters('nodeTypeName'))]",
    "dependsOn": [
        "[concat('Microsoft.ServiceFabric/managedclusters/', parameters('clusterName'))]"
    ],
    "location": "[resourceGroup().location]",
    "properties": {
        "isPrimary": true,
        "vmInstanceCount": 3,
        "dataDiskSizeGB": 100,
        "vmSize": "Standard_D2",
        "vmImagePublisher": "MicrosoftWindowsServer",
        "vmImageOffer": "WindowsServer",
        "vmImageSku": "2019-Datacenter",
        "vmImageVersion": "latest",
        "vmExtensions": [{
            "name": "ExtensionA",
            "properties": {
                "publisher": "ExtensionA.Publisher",
                "type": "KeyVaultForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                }
            }
        }]
    }
}
```

Per ulteriori informazioni sulla configurazione di Service Fabric tipi di nodo del cluster gestito, vedere [tipo di nodo del cluster gestito](/azure/templates/microsoft.servicefabric/2020-01-01-preview/managedclusters/nodetypes).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui cluster gestiti di Service Fabric Mesh, vedere:

> [!div class="nextstepaction"]
> [Domande frequenti sui cluster gestiti di Service Fabric](./faq-managed-cluster.md)
