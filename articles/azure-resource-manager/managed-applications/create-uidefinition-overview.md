---
title: CreateUiDefinition.jssul file per il riquadro del portale
description: Viene descritto come creare definizioni dell'interfaccia utente per la portale di Azure. Usato durante la definizione di applicazioni gestite di Azure.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: tomfitz
ms.openlocfilehash: 327fa1d7eb73d8e65bb4f81c1dff0fe2bec2913b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "89319569"
---
# <a name="createuidefinitionjson-for-azure-managed-applications-create-experience"></a>CreateUiDefinition.json per l'esperienza di creazione di un'applicazione gestita di Azure

In questo documento vengono introdotti i concetti di base del **createUiDefinition.jssu** file. Il portale di Azure usa questo file per definire l'interfaccia utente durante la creazione di un'applicazione gestita.

Il modello è il seguente:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Azure.CreateUIDef",
    "version": "0.1.2-preview",
    "parameters": {
        "config": {
            "isWizard": false,
            "basics": { }
        },
        "basics": [ ],
        "steps": [ ],
        "outputs": { },
        "resourceTypes": [ ]
    }
}
```

Un oggetto `CreateUiDefinition` contiene sempre tre proprietà:

* gestore
* version
* parametri

Il gestore deve essere sempre `Microsoft.Azure.CreateUIDef` e la versione supportata più recente è `0.1.2-preview` .

Lo schema della proprietà parameters dipende dalla combinazione delle proprietà handler e version specificate. Per le applicazioni gestite, le proprietà supportate sono `config` ,, `basics` `steps` e `outputs` . Usare `config` solo quando è necessario eseguire l'override del comportamento predefinito del `basics` passaggio. Le proprietà basics e steps contengono [elementi](create-uidefinition-elements.md), ad esempio caselle di testo ed elenchi a discesa, da visualizzare nel portale di Azure. La proprietà Outputs viene utilizzata per eseguire il mapping dei valori di output degli elementi specificati ai parametri del modello di Azure Resource Manager.

L'inclusione di `$schema` è consigliata ma facoltativa. Se specificato, il valore per la proprietà `version` deve corrispondere alla versione nell'URI di `$schema`.

È possibile usare un editor JSON per creare il createUiDefinition, quindi testarlo nella [sandbox createUiDefinition](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade) per visualizzarne l'anteprima. Per altre informazioni sulla sandbox, vedere [testare l'interfaccia del portale per le applicazioni gestite di Azure](test-createuidefinition.md).

## <a name="config"></a>File di configurazione

La proprietà `config` è facoltativa. Utilizzarlo per eseguire l'override del comportamento predefinito del passaggio di base o per impostare l'interfaccia come procedura dettagliata. Se `config` si usa, si tratta della prima proprietà nella sezione **createUiDefinition.js** nella sezione del file `parameters` . Nell'esempio seguente vengono illustrate le proprietà disponibili.

```json
"config": {
    "isWizard": false,
    "basics": {
        "description": "Customized description with **markdown**, see [more](https://www.microsoft.com).",
        "subscription": {
            "constraints": {
                "validations": [
                    {
                        "isValid": "[expression for checking]",
                        "message": "Please select a valid subscription."
                    },
                    {
                        "permission": "<Resource Provider>/<Action>",
                        "message": "Must have correct permission to complete this step."
                    }
                ]
            },
            "resourceProviders": [
                "<Resource Provider>"
            ]
        },
        "resourceGroup": {
            "constraints": {
                "validations": [
                    {
                        "isValid": "[expression for checking]",
                        "message": "Please select a valid resource group."
                    }
                ]
            },
            "allowExisting": true
        },
        "location": {
            "label": "Custom label for location",
            "toolTip": "provide a useful tooltip",
            "resourceTypes": [
                "Microsoft.Compute/virtualMachines"
            ],
            "allowedValues": [
                "eastus",
                "westus2"
            ],
            "visible": true
        }
    }
},
```

### <a name="wizard"></a>Procedura guidata

La `isWizard` proprietà consente di richiedere la convalida corretta di ogni passaggio prima di procedere al passaggio successivo. Quando la `isWizard` proprietà non è specificata, il valore predefinito è **false**e la convalida dettagliata non è obbligatoria.

Quando `isWizard` è abilitato, impostare su **true**, la scheda **nozioni di base** è disponibile e tutte le altre schede sono disabilitate. Quando si seleziona il pulsante **Avanti** , l'icona della scheda indica se la convalida di una scheda è stata superata o non riuscita. Dopo che i campi obbligatori della scheda sono stati completati e convalidati, il pulsante **Avanti** consente la navigazione alla scheda successiva. Quando tutte le schede passano la convalida, è possibile passare alla pagina **Verifica e crea** e selezionare il pulsante **Crea** per avviare la distribuzione.

:::image type="content" source="./media/create-uidefinition-overview/tab-wizard.png" alt-text="Creazione guidata scheda":::

### <a name="override-basics"></a>Nozioni fondamentali sull'override

La configurazione di base consente di personalizzare il passaggio di base.

Per `description` , specificare una stringa abilitata per Markdown che descrive la risorsa. Sono supportati il formato a più righe e i collegamenti.

Gli `subscription` `resourceGroup` elementi e consentono di specificare convalide aggiuntive. La sintassi per specificare le convalide è identica alla [casella di testo](microsoft-common-textbox.md)convalida personalizzata. È anche possibile specificare `permission` convalide per la sottoscrizione o il gruppo di risorse.  

Il controllo della sottoscrizione accetta un elenco di spazi dei nomi del provider di risorse. Ad esempio, è possibile specificare **Microsoft. Compute**. Viene visualizzato un messaggio di errore quando l'utente seleziona una sottoscrizione che non supporta il provider di risorse. L'errore si verifica quando il provider di risorse non è registrato nella sottoscrizione e l'utente non è autorizzato a registrare il provider di risorse.  

Il controllo del gruppo di risorse dispone di un'opzione per `allowExisting` . Se `true` , gli utenti possono selezionare i gruppi di risorse che dispongono già di risorse. Questo flag è più applicabile ai modelli di soluzione, in cui il comportamento predefinito impone agli utenti di selezionare un gruppo di risorse nuovo o vuoto. Nella maggior parte degli altri scenari, non è necessario specificare questa proprietà.  

Per `location` , specificare le proprietà per il controllo del percorso di cui si desidera eseguire l'override. Le proprietà di cui non è stato eseguito l'override sono impostate sui valori predefiniti. `resourceTypes` accetta una matrice di stringhe contenenti nomi di tipi di risorse completi. Le opzioni relative al percorso sono limitate solo alle aree che supportano i tipi di risorse.  `allowedValues`   accetta una matrice di stringhe di area. Nell'elenco a discesa vengono visualizzate solo le aree.È possibile impostare sia `allowedValues`   che  `resourceTypes` . Il risultato è l'intersezione di entrambi gli elenchi. Infine, la `visible` proprietà può essere usata per disabilitare in modo condizionale o completamente l'elenco a discesa percorso.  

## <a name="basics"></a>Nozioni di base

Il passaggio di **base** è il primo passaggio generato quando il portale di Azure analizza il file. Per impostazione predefinita, il passaggio di base consente agli utenti di scegliere la sottoscrizione, il gruppo di risorse e la località per la distribuzione.

:::image type="content" source="./media/create-uidefinition-overview/basics.png" alt-text="Creazione guidata scheda":::

È possibile aggiungere altri elementi in questa sezione. Quando possibile, aggiungere elementi che eseguono query sui parametri a livello di distribuzione, ad esempio il nome di un cluster o le credenziali di amministratore.

Nell'esempio seguente viene illustrata una casella di testo aggiunta agli elementi predefiniti.

```json
"basics": [
    {
        "name": "textBox1",
        "type": "Microsoft.Common.TextBox",
        "label": "Textbox on basics",
        "defaultValue": "my text value",
        "toolTip": "",
        "visible": true
    }
]
```

## <a name="steps"></a>Passaggi

La proprietà Steps contiene zero o più passaggi aggiuntivi da visualizzare dopo le nozioni di base. Ogni passaggio contiene uno o più elementi. Prendere in considerazione l'aggiunta di passaggi per ogni ruolo o livello dell'applicazione in fase di distribuzione. Ad esempio, aggiungere un passaggio per gli input del nodo master e un passaggio per i nodi del ruolo di lavoro in un cluster.

```json
"steps": [
    {
        "name": "demoConfig",
        "label": "Configuration settings",
        "elements": [
          ui-elements-needed-to-create-the-instance
        ]
    }
]
```

## <a name="outputs"></a>Output

Il portale di Azure usa la proprietà `outputs` per il mapping di elementi da `basics` e `steps` ai parametri del modello di distribuzione Azure Resource Manager. Le chiavi di questo dizionario sono i nomi dei parametri del modello e i valori sono le proprietà degli oggetti di output dagli elementi a cui si fa riferimento.

Per impostare il nome della risorsa applicazione gestita, è necessario includere un valore denominato `applicationResourceName` nella proprietà di output. Se non si imposta questo valore, l'applicazione assegna un GUID per il nome. È possibile includere una casella di testo nell'interfaccia utente che richiede un nome all'utente.

```json
"outputs": {
    "vmName": "[steps('appSettings').vmName]",
    "trialOrProduction": "[steps('appSettings').trialOrProd]",
    "userName": "[steps('vmCredentials').adminUsername]",
    "pwd": "[steps('vmCredentials').vmPwd.password]",
    "applicationResourceName": "[steps('appSettings').vmName]"
}
```

## <a name="resource-types"></a>Tipi di risorsa

Per filtrare i percorsi disponibili solo per i percorsi che supportano i tipi di risorse da distribuire, fornire una matrice di tipi di risorse. Se si specifica più di un tipo di risorsa, vengono restituiti solo i percorsi che supportano tutti i tipi di risorsa. Questa proprietà è facoltativa.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Azure.CreateUIDef",
    "version": "0.1.2-preview",
    "parameters": {
        "resourceTypes": ["Microsoft.Compute/disks"],
        "basics": [
          ...
```  

## <a name="functions"></a>Funzioni

CreateUiDefinition fornisce [funzioni](create-uidefinition-functions.md) per l'uso di input e output degli elementi e funzionalità come le istruzioni condizionali. Queste funzioni sono simili nella sintassi e nella funzionalità per Azure Resource Manager funzioni di modello.

## <a name="next-steps"></a>Passaggi successivi

Il file createUiDefinition.json è contraddistinto da uno schema semplice ma supporta numerosi elementi e funzioni, illustrati in modo dettagliato nei documenti seguenti:

- [Elementi](create-uidefinition-elements.md)
- [Funzioni](create-uidefinition-functions.md)

Uno schema JSON corrente per createUiDefinition è disponibile qui: `https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json`.

Per un esempio di file di interfaccia utente, vedere [createUiDefinition.json](https://github.com/Azure/azure-managedapp-samples/blob/master/Managed%20Application%20Sample%20Packages/201-managed-app-using-existing-vnet/createUiDefinition.json).
