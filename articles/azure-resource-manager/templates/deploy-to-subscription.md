---
title: Distribuire risorse in una sottoscrizione
description: Questo articolo descrive come creare un gruppo di risorse in un modello di Azure Resource Manager. Illustra anche come distribuire le risorse nell'ambito della sottoscrizione di Azure.
ms.topic: conceptual
ms.date: 01/13/2021
ms.openlocfilehash: b5c99e5dc21c2b93f1c9da3977302a2dd311277f
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99491504"
---
# <a name="subscription-deployments-with-arm-templates"></a>Distribuzioni di sottoscrizioni con modelli ARM

Per semplificare la gestione delle risorse, è possibile usare un modello di Azure Resource Manager (modello ARM) per distribuire le risorse al livello della sottoscrizione di Azure. Ad esempio, è possibile distribuire i [criteri](../../governance/policy/overview.md) e il [controllo degli accessi in base al ruolo di Azure (RBAC di Azure)](../../role-based-access-control/overview.md) alla sottoscrizione, che li applica all'intera sottoscrizione. È anche possibile creare gruppi di risorse all'interno della sottoscrizione e distribuire le risorse ai gruppi di risorse nella sottoscrizione.

> [!NOTE]
> È possibile eseguire una distribuzione a livello di sottoscrizione in un massimo di 800 gruppi di risorse diversi.

Per distribuire i modelli a livello di sottoscrizione, usare l'interfaccia della riga di comando di Azure, PowerShell, l'API REST o il portale.

## <a name="supported-resources"></a>Risorse supportate

Non tutti i tipi di risorse possono essere distribuiti a livello di sottoscrizione. Questa sezione elenca i tipi di risorse supportati.

Per i progetti di Azure, usare:

* [artifacts](/azure/templates/microsoft.blueprint/blueprints/artifacts)
* [blueprints](/azure/templates/microsoft.blueprint/blueprints)
* [blueprintAssignments](/azure/templates/microsoft.blueprint/blueprintassignments)
* [versioni (progetti)](/azure/templates/microsoft.blueprint/blueprints/versions)

Per i criteri di Azure, usare:

* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [remediations](/azure/templates/microsoft.policyinsights/remediations)

Per il controllo degli accessi in base al ruolo di Azure (RBAC di Azure), usare:

* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

Per i modelli annidati che vengono distribuiti ai gruppi di risorse, usare:

* [distribuzioni](/azure/templates/microsoft.resources/deployments)

Per la creazione di nuovi gruppi di risorse, usare:

* [resourceGroups](/azure/templates/microsoft.resources/resourcegroups)

Per la gestione della sottoscrizione, usare:

* [Configurazioni di Advisor](/azure/templates/microsoft.advisor/configurations)
* [budgets](/azure/templates/microsoft.consumption/budgets)
* [Modifica profilo di analisi](/azure/templates/microsoft.changeanalysis/profile)
* [supportPlanTypes](/azure/templates/microsoft.addons/supportproviders/supportplantypes)
* [tag](/azure/templates/microsoft.resources/tags)

Altri tipi supportati includono:

* [scopeAssignments](/azure/templates/microsoft.managednetwork/scopeassignments)
* [eventSubscriptions](/azure/templates/microsoft.eventgrid/eventsubscriptions)
* [peerAsns](/azure/templates/microsoft.peering/2019-09-01-preview/peerasns)

## <a name="schema"></a>SCHEMA

Lo schema usato per le distribuzioni a livello di sottoscrizione è diverso rispetto allo schema per le distribuzioni di gruppi di risorse.

Per i modelli, usare:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    ...
}
```

Lo schema per un file di parametri è lo stesso per tutti gli ambiti di distribuzione. Per i file di parametri, usare:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="deployment-commands"></a>Comandi di distribuzione

Per eseguire la distribuzione in una sottoscrizione, usare i comandi di distribuzione a livello di sottoscrizione.

# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

Per l'interfaccia della riga di comando di Azure usare [az deployment sub create](/cli/azure/deployment/sub#az-deployment-sub-create). L'esempio seguente distribuisce un modello per creare un gruppo di risorse:

```azurecli-interactive
az deployment sub create \
  --name demoSubDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json" \
  --parameters rgName=demoResourceGroup rgLocation=centralus
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Per il comando di distribuzione di PowerShell, usare [New-AzDeployment](/powershell/module/az.resources/new-azdeployment) o il relativo alias `New-AzSubscriptionDeployment` . L'esempio seguente distribuisce un modello per creare un gruppo di risorse:

```azurepowershell-interactive
New-AzSubscriptionDeployment `
  -Name demoSubDeployment `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json" `
  -rgName demoResourceGroup `
  -rgLocation centralus
```

---

Per informazioni più dettagliate sui comandi e sulle opzioni di distribuzione per la distribuzione di modelli ARM, vedere:

* [Distribuire le risorse con i modelli ARM e portale di Azure](deploy-portal.md)
* [Distribuire le risorse con i modelli di Azure Resource Manager e l'interfaccia della riga di comando di Azure](deploy-cli.md)
* [Distribuire le risorse con i modelli di Azure Resource Manager e Azure PowerShell](deploy-powershell.md)
* [Distribuire le risorse con i modelli ARM e Azure Resource Manager API REST](deploy-rest.md)
* [Usare un pulsante di distribuzione per distribuire i modelli dal repository GitHub](deploy-to-azure-button.md)
* [Distribuire modelli ARM da Cloud Shell](deploy-cloud-shell.md)

## <a name="deployment-location-and-name"></a>Percorso e nome della distribuzione

Per le distribuzioni a livello di sottoscrizione, è necessario specificare un percorso di distribuzione. Il percorso di distribuzione è separato dal percorso delle risorse distribuite e specifica dove archiviare i dati di distribuzione. Il [gruppo di gestione](deploy-to-management-group.md) e le distribuzioni [tenant](deploy-to-tenant.md) richiedono anche un percorso. Per le distribuzioni di [gruppi di risorse](deploy-to-resource-group.md) , il percorso del gruppo di risorse viene usato per archiviare i dati di distribuzione.

È possibile specificare un nome per la distribuzione oppure usare il nome predefinito. Il nome predefinito è il nome del file modello. Ad esempio, la distribuzione di un modello denominato _azuredeploy.json_ crea un nome di distribuzione predefinito di **azuredeploy**.

Per ogni nome di distribuzione il percorso non è modificabile. Non è possibile creare una distribuzione in un percorso se esiste una distribuzione con lo stesso nome in un percorso diverso. Se, ad esempio, si crea una distribuzione di sottoscrizione con il nome **Deployment1** in **centralus**, in un secondo momento non sarà possibile creare un'altra distribuzione con il nome **Deployment1** ma un percorso di **westus**. Se viene visualizzato il codice di errore `InvalidDeploymentLocation`, utilizzare un nome diverso o lo stesso percorso come la distribuzione precedente per tale nome.

## <a name="deployment-scopes"></a>Ambiti di distribuzione

Quando si esegue la distribuzione in una sottoscrizione, è possibile distribuire le risorse in:

* sottoscrizione di destinazione dall'operazione
* qualsiasi sottoscrizione nel tenant
* gruppi di risorse all'interno della sottoscrizione o di altre sottoscrizioni
* tenant per la sottoscrizione

Una [risorsa di estensione](scope-extension-resources.md) può avere come ambito una destinazione diversa dalla destinazione di distribuzione.

L'utente che distribuisce il modello deve avere accesso all'ambito specificato.

In questa sezione viene illustrato come specificare ambiti diversi. È possibile combinare questi ambiti diversi in un singolo modello.

### <a name="scope-to-target-subscription"></a>Ambito della sottoscrizione di destinazione

Per distribuire le risorse nella sottoscrizione di destinazione, aggiungere tali risorse alla sezione Resources del modello.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/default-sub.json" highlight="5":::

Per esempi di distribuzione nella sottoscrizione, vedere [creare gruppi di risorse](#create-resource-groups) e [assegnare la definizione dei criteri](#assign-policy-definition).

### <a name="scope-to-other-subscription"></a>Ambito ad altra sottoscrizione

Per distribuire le risorse in una sottoscrizione diversa dalla sottoscrizione dell'operazione, aggiungere una distribuzione nidificata. Impostare la `subscriptionId` proprietà sull'ID della sottoscrizione in cui si desidera eseguire la distribuzione. Impostare la `location` proprietà per la distribuzione nidificata.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/sub-to-sub.json" highlight="9,10,14":::

### <a name="scope-to-resource-group"></a>Ambito al gruppo di risorse

Per distribuire le risorse in un gruppo di risorse all'interno della sottoscrizione, aggiungere una distribuzione annidata e includere la `resourceGroup` Proprietà. Nell'esempio seguente la distribuzione annidata è destinata a un gruppo di risorse denominato `demoResourceGroup` .

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/sub-to-resource-group.json" highlight="9,13":::

Per un esempio di distribuzione in un gruppo di risorse, vedere [creare risorse e gruppi di risorse](#create-resource-group-and-resources).

### <a name="scope-to-tenant"></a>Ambito al tenant

Per creare risorse nel tenant, impostare `scope` su `/` . L'utente che distribuisce il modello deve avere l' [accesso necessario per la distribuzione nel tenant](deploy-to-tenant.md#required-access).

Per utilizzare una distribuzione annidata, impostare `scope` e `location` .

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/subscription-to-tenant.json" highlight="9,10,14":::

In alternativa, è possibile impostare l'ambito su `/` per alcuni tipi di risorse, ad esempio i gruppi di gestione.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/subscription-create-mg.json" highlight="12,15":::

Per ulteriori informazioni, vedere [gruppo di gestione](deploy-to-management-group.md#management-group).

## <a name="resource-groups"></a>Gruppi di risorse

### <a name="create-resource-groups"></a>Creare gruppi di risorse

Per creare un gruppo di risorse in un modello ARM, definire una risorsa [Microsoft. resources/resourceGroups](/azure/templates/microsoft.resources/allversions) con un nome e un percorso per il gruppo di risorse.

Il modello seguente crea un gruppo di risorse vuoto.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgName": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2020-06-01",
      "name": "[parameters('rgName')]",
      "location": "[parameters('rgLocation')]",
      "properties": {}
    }
  ],
  "outputs": {}
}
```

Per creare più gruppi di risorse usare l'[elemento copy](copy-resources.md).

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgNamePrefix": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    },
    "instanceCount": {
      "type": "int"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2020-06-01",
      "location": "[parameters('rgLocation')]",
      "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
      "copy": {
        "name": "rgCopy",
        "count": "[parameters('instanceCount')]"
      },
      "properties": {}
    }
  ],
  "outputs": {}
}
```

Per informazioni sull'iterazione delle risorse, vedere [iterazione delle risorse nei modelli ARM](./copy-resources.md)ed [esercitazione: creare più istanze di risorse con i modelli ARM](./template-tutorial-create-multiple-instances.md).

### <a name="create-resource-group-and-resources"></a>Creare il gruppo di risorse e le risorse

Per creare un gruppo di risorse e distribuire risorse a tale gruppo, usare un modello annidato. Questo tipo di modello definisce le risorse da distribuire al gruppo. Impostare il modello annidato come dipendente dal gruppo di risorse per assicurarsi che il gruppo sia presente prima della distribuzione delle risorse. È possibile eseguire la distribuzione in un massimo di 800 gruppi di risorse.

L'esempio seguente crea un gruppo di risorse e distribuisce un account di archiviazione al gruppo.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgName": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    },
    "storagePrefix": {
      "type": "string",
      "maxLength": 11
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
  },
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2020-06-01",
      "name": "[parameters('rgName')]",
      "location": "[parameters('rgLocation')]",
      "properties": {}
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      "name": "storageDeployment",
      "resourceGroup": "[parameters('rgName')]",
      "dependsOn": [
        "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2019-06-01",
              "name": "[variables('storageName')]",
              "location": "[parameters('rgLocation')]",
              "sku": {
                "name": "Standard_LRS"
              },
              "kind": "StorageV2"
            }
          ],
          "outputs": {}
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="azure-policy"></a>Criteri di Azure

### <a name="assign-policy-definition"></a>Assegnare una definizione di criteri

L'esempio seguente assegna una definizione di criteri esistente alla sottoscrizione. Se la definizione di criteri accetta parametri, specificarli come oggetto. Se non accetta parametri, usare l'oggetto vuoto predefinito.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "policyDefinitionID": {
      "type": "string"
    },
    "policyName": {
      "type": "string"
    },
    "policyParameters": {
      "type": "object",
      "defaultValue": {}
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyAssignments",
      "apiVersion": "2018-03-01",
      "name": "[parameters('policyName')]",
      "properties": {
        "scope": "[subscription().id]",
        "policyDefinitionId": "[parameters('policyDefinitionID')]",
        "parameters": "[parameters('policyParameters')]"
      }
    }
  ]
}
```

Per distribuire questo modello con l'interfaccia della riga di comando di Azure, usare:

```azurecli-interactive
# Built-in policy definition that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json" \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Per distribuire questo modello con PowerShell, usare:

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzSubscriptionDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json" `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

### <a name="create-and-assign-policy-definitions"></a>Creare e assegnare definizioni di criteri

È possibile [definire](../../governance/policy/concepts/definition-structure.md) e assegnare una definizione di criteri nello stesso modello.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyDefinitions",
      "apiVersion": "2018-05-01",
      "name": "locationpolicy",
      "properties": {
        "policyType": "Custom",
        "parameters": {},
        "policyRule": {
          "if": {
            "field": "location",
            "equals": "northeurope"
          },
          "then": {
            "effect": "deny"
          }
        }
      }
    },
    {
      "type": "Microsoft.Authorization/policyAssignments",
      "apiVersion": "2018-05-01",
      "name": "location-lock",
      "dependsOn": [
        "locationpolicy"
      ],
      "properties": {
        "scope": "[subscription().id]",
        "policyDefinitionId": "[subscriptionResourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
      }
    }
  ]
}
```

Per creare la definizione di criteri nella sottoscrizione e assegnarla alla sottoscrizione, usare il comando seguente dell'interfaccia della riga di comando:

```azurecli
az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json"
```

Per distribuire questo modello con PowerShell, usare:

```azurepowershell
New-AzSubscriptionDeployment `
  -Name definePolicy `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json"
```

## <a name="azure-blueprints"></a>Azure Blueprint

### <a name="create-blueprint-definition"></a>Creare una definizione di progetto

È possibile [creare](../../governance/blueprints/tutorials/create-from-sample.md) una definizione di progetto da un modello.

:::code language="json" source="~/quickstart-templates/subscription-deployments/blueprints-new-blueprint/azuredeploy.json":::

Per creare la definizione del progetto nella sottoscrizione, usare il comando dell'interfaccia della riga di comando seguente:

```azurecli
az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/blueprints-new-blueprint/azuredeploy.json"
```

Per distribuire questo modello con PowerShell, usare:

```azurepowershell
New-AzSubscriptionDeployment `
  -Name demoDeployment `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/blueprints-new-blueprint/azuredeploy.json"
```

## <a name="access-control"></a>Controllo di accesso

Per informazioni sull'assegnazione dei ruoli, vedere [aggiungere assegnazioni di ruolo di Azure usando modelli di Azure Resource Manager](../../role-based-access-control/role-assignments-template.md).

Nell'esempio seguente viene creato un gruppo di risorse, viene applicato un blocco e viene assegnato un ruolo a un'entità.

:::code language="json" source="~/quickstart-templates/subscription-deployments/create-rg-lock-role-assignment/azuredeploy.json":::

## <a name="next-steps"></a>Passaggi successivi

* Per un esempio di distribuzione delle impostazioni dell'area di lavoro per il Centro sicurezza di Azure, vedere [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Modelli di esempio sono disponibili in [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-deployments).
* È anche possibile distribuire modelli a [livello di gruppo di gestione](deploy-to-management-group.md) e [a livello di tenant](deploy-to-tenant.md).
