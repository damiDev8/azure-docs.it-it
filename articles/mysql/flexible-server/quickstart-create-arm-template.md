---
title: 'Avvio rapido: Creare un server flessibile di Database di Azure per MySQL - Modello di Resource Manager'
description: Questo argomento di avvio rapido illustra come creare un server flessibile di Database di Azure per MySQL usando un modello di Resource Manager.
author: mksuni
ms.service: mysql
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: sumuth
ms.date: 10/23/2020
ms.openlocfilehash: a7dc6a6b11d3bfacf0aac5472a872ffaa7acc92b
ms.sourcegitcommit: 003ac3b45abcdb05dc4406661aca067ece84389f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/07/2020
ms.locfileid: "96748706"
---
# <a name="quickstart-use-an-arm-template-to-create-an-azure-database-for-mysql---flexible-server-preview"></a>Avvio rapido: Usare un modello di Resource Manager per creare un server flessibile di Database di Azure per MySQL (anteprima)

> [!IMPORTANT]
> Il server flessibile di Database di Azure per MySQL è attualmente disponibile in anteprima pubblica.

Il server flessibile di Database di Azure per MySQL (anteprima) è un servizio gestito che consente di eseguire, gestire e dimensionare database MySQL a disponibilità elevata nel cloud. È possibile usare un modello di Azure Resource Manager per effettuare il provisioning di un server flessibile per distribuire più server o più database in un server.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Prerequisiti

Un account Azure con una sottoscrizione attiva. [È possibile crearne uno gratuitamente](https://azure.microsoft.com/free/).

## <a name="review-the-template"></a>Rivedere il modello

Un server flessibile di Database di Azure per MySQL è la risorsa padre di uno o più database all'interno di un'area. Fornisce l'ambito per i criteri di gestione applicati ai relativi database: account di accesso, firewall, utenti, ruoli, configurazioni.

Creare un file _mysql-flexible-server-template.json_ e copiarvi questo script JSON.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "administratorLogin": {
      "type": "String"
    },
    "administratorLoginPassword": {
      "type": "SecureString"
    },
    "location": {
      "type": "String"
    },
    "serverName": {
      "type": "String"
    },
    "serverEdition": {
      "type": "String"
    },
    "storageSizeMB": {
      "type": "Int"
    },
    "haEnabled": {
      "type": "string",
      "defaultValue": "Disabled"
    },
    "availabilityZone": {
      "type": "String"
    },
    "version": {
      "type": "String"
    },
    "tags": {
      "defaultValue": {},
      "type": "Object"
    },
    "firewallRules": {
      "defaultValue": {},
      "type": "Object"
    },
    "vnetData": {
      "defaultValue": {},
      "type": "Object"
    },
    "backupRetentionDays": {
      "type": "Int"
    }
  },
  "variables": {
    "api": "2020-07-01-preview",
    "firewallRules": "[parameters('firewallRules').rules]",
    "publicNetworkAccess": "[if(empty(parameters('vnetData')), 'Enabled', 'Disabled')]",
    "vnetDataSet": "[if(empty(parameters('vnetData')), json('{ \"subnetArmResourceId\": \"\" }'), parameters('vnetData'))]",
    "finalVnetData": "[json(concat('{ \"subnetArmResourceId\": \"', variables('vnetDataSet').subnetArmResourceId, '\"}'))]"
  },
  "resources": [
    {
      "type": "Microsoft.DBforMySQL/flexibleServers",
      "apiVersion": "[variables('api')]",
      "name": "[parameters('serverName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard_D4ds_v4",
        "tier": "[parameters('serverEdition')]"        
      },
      "tags": "[parameters('tags')]",
      "properties": {
        "version": "[parameters('version')]",
        "administratorLogin": "[parameters('administratorLogin')]",
        "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
        "publicNetworkAccess": "[variables('publicNetworkAccess')]",
        "DelegatedSubnetArguments": "[if(empty(parameters('vnetData')), json('null'), variables('finalVnetData'))]",
        "haEnabled": "[parameters('haEnabled')]",
        "storageProfile": {
          "storageMB": "[parameters('storageSizeMB')]",
          "backupRetentionDays": "[parameters('backupRetentionDays')]"
        },
        "availabilityZone": "[parameters('availabilityZone')]"
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-08-01",
      "name": "[concat('firewallRules-', copyIndex())]",
      "dependsOn": [
        "[concat('Microsoft.DBforMySQL/flexibleServers/', parameters('serverName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": [
            {
              "type": "Microsoft.DBforMySQL/flexibleServers/firewallRules",
              "name": "[concat(parameters('serverName'),'/',variables('firewallRules')[copyIndex()].name)]",
              "apiVersion": "[variables('api')]",
              "properties": {
                "StartIpAddress": "[variables('firewallRules')[copyIndex()].startIPAddress]",
                "EndIpAddress": "[variables('firewallRules')[copyIndex()].endIPAddress]"
              }
            }
          ]
        }
      },
      "copy": {
        "name": "firewallRulesIterator",
        "count": "[if(greater(length(variables('firewallRules')), 0), length(variables('firewallRules')), 1)]",
        "mode": "Serial"
      },
      "condition": "[greater(length(variables('firewallRules')), 0)]"
    }
  ]
}
```

Nel modello sono definite queste risorse:

- Microsoft.DBforMySQL/flexibleServers

## <a name="deploy-the-template"></a>Distribuire il modello

Selezionare **Provalo** nel blocco di codice PowerShell seguente per aprire [Azure Cloud Shell](../../cloud-shell/overview.md).

```azurepowershell-interactive
$serverName = Read-Host -Prompt "Enter a name for the new Azure Database for MySQL server"
$resourceGroupName = Read-Host -Prompt "Enter a name for the new resource group where the server will exist"
$location = Read-Host -Prompt "Enter an Azure region (for example, centralus) for the resource group"
$adminUser = Read-Host -Prompt "Enter the Azure Database for MySQL server's administrator account name"
$adminPassword = Read-Host -Prompt "Enter the administrator password" -AsSecureString

New-AzResourceGroup -Name $resourceGroupName -Location $location # Use this command when you need to create a new resource group for your deployment
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
    -TemplateFile "D:\Azure\Templates\EngineeringSite.json
    -serverName $serverName `
    -administratorLogin $adminUser `
    -administratorLoginPassword $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

## <a name="review-deployed-resources"></a>Esaminare le risorse distribuite

Seguire questa procedura per verificare se il server è stato creato in Azure.

### <a name="azure-portal"></a>Portale di Azure

1. Nel [portale di Azure](https://portal.azure.com) cercare e selezionare **Database di Azure per i server MySQL**.
1. Nell'elenco dei database selezionare il nuovo server. Verrà visualizzata la pagina **Panoramica** per il nuovo database di Azure per il server MySQL.

### <a name="powershell"></a>PowerShell

Per visualizzare i dettagli del server flessibile di Database di Azure per MySQL sarà necessario immettere il nome del nuovo server.

```azurepowershell-interactive
$serverName = Read-Host -Prompt "Enter the name of your Azure Database for MySQL server"
Get-AzResource -ResourceType "Microsoft.DBforMySQL/flexibleServers" -Name $serverName | ft
Write-Host "Press [ENTER] to continue..."
```

### <a name="cli"></a>CLI

Per visualizzare i dettagli del server flessibile di Database di Azure per MySQL sarà necessario immettere il nome e il gruppo di risorse del nuovo server.

```azurecli-interactive
echo "Enter your Azure Database for MySQL server name:" &&
read serverName &&
echo "Enter the resource group where the Azure Database for MySQL server exists:" &&
read resourcegroupName &&
az resource show --resource-group $resourcegroupName --name $serverName --resource-type "Microsoft.DbForMySQL/flexibleServers"
```

## <a name="clean-up-resources"></a>Pulire le risorse

Se si vuole procedere con i [Passaggi successivi](#next-steps), conservare il gruppo di risorse, il server e il database singolo. I passaggi successivi illustrano come connettersi al database ed eseguire query con diversi metodi.

Per eliminare il gruppo di risorse:

### <a name="azure-portal"></a>Portale di Azure

1. Nel [portale di Azure](https://portal.azure.com) cercare e selezionare **Gruppi di risorse**.
1. Nell'elenco dei gruppi di risorse scegliere il nome del gruppo di risorse.
1. Nella pagina **Panoramica** del gruppo di risorse selezionare **Elimina gruppo di risorse**.
1. Nella finestra di dialogo di conferma, digitare il nome del gruppo di risorse e quindi selezionare **Elimina**.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

### <a name="cli"></a>CLI

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```
---

## <a name="next-steps"></a>Passaggi successivi

Per un'esercitazione dettagliata che illustra il processo di creazione di un modello di Resource Manager, vedere:

> [!div class="nextstepaction"]
> [Creare e distribuire il primo modello di Resource Manager](../../azure-resource-manager/templates/template-tutorial-create-first-template.md)

Per un'esercitazione dettagliata sulla creazione di un'app con il servizio app usando MySQL, vedere:

> [!div class="nextstepaction"]
>[Creare un'app Web PHP (Laravel) con MySQL](tutorial-php-database-app.md)
