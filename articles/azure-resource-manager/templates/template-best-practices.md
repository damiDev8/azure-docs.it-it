---
title: Procedure consigliate per i modelli
description: Vengono descritti gli approcci consigliati per la creazione di modelli di Azure Resource Manager (modelli ARM). Offre suggerimenti per evitare problemi comuni quando si usano i modelli.
ms.topic: conceptual
ms.date: 12/01/2020
ms.openlocfilehash: 583a113df9cdb1951daf1002dd69531f050cfb54
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99257998"
---
# <a name="arm-template-best-practices"></a>Procedure consigliate per il modello ARM

Questo articolo illustra come usare le procedure consigliate per la creazione di un modello di Azure Resource Manager (modello ARM). Questi consigli consentono di evitare problemi comuni quando si usa un modello ARM per distribuire una soluzione.

## <a name="template-limits"></a>Limiti del modello

Limitare le dimensioni del modello a 4 MB e ogni file di parametri a 64 KB. Il limite di 4 MB si applica allo stato finale del modello dopo che è stato espanso con le definizioni di risorse iterative e i valori per variabili e parametri.

Esistono anche i limiti seguenti:

* 256 parametri
* 256 variabili
* 800 risorse (incluso il conteggio copie)
* 64 valori di output
* 24.576 caratteri in un'espressione di modello

È possibile superare alcuni limiti del modello usando un modello annidato. Per altre informazioni, vedere [uso di modelli collegati e annidati durante la distribuzione di risorse di Azure](linked-templates.md). Per ridurre il numero di parametri, variabili o output, è possibile combinare più valori in un oggetto. Per altre informazioni, vedere [Oggetti come parametri](/azure/architecture/guide/azure-resource-manager/advanced-templates/objects-as-parameters).

## <a name="resource-group"></a>Resource group

Quando si distribuiscono le risorse in un gruppo di risorse, il gruppo di risorse archivia i metadati relativi alle risorse. I metadati vengono archiviati nella posizione del gruppo di risorse.

Se l'area del gruppo di risorse è temporaneamente non disponibile, non è possibile aggiornare le risorse nel gruppo di risorse perché i metadati non sono disponibili. Le risorse in altre aree continueranno a funzionare come previsto, ma non è possibile aggiornarle. Per ridurre il rischio, collocare il gruppo di risorse e le risorse nella stessa area.

## <a name="parameters"></a>Parametri

Le informazioni di questa sezione possono essere utili quando si usano i [parametri](template-parameters.md).

### <a name="general-recommendations-for-parameters"></a>Raccomandazioni generali per i parametri

* Ridurre al minimo l'uso di parametri. Usare invece i valori letterali o le variabili per le proprietà che non è necessario specificare durante la distribuzione.

* Usare la notazione Camel per i nomi dei parametri.

* Usare i parametri per impostazioni che variano in base all'ambiente, ad esempio SKU, dimensioni o capacità.

* Usare i parametri per i nomi di risorse da specificare per facilitare l'identificazione.

* Fornire una descrizione di ogni parametro nei metadati.

    ```json
    "parameters": {
      "storageAccountType": {
        "type": "string",
        "metadata": {
          "description": "The type of the new storage account created to store the VM disks."
        }
      }
    }
    ```

* Definire i valori predefiniti per i parametri non riservati. Specificando un valore predefinito, è più semplice distribuire il modello e gli utenti del modello vedono un esempio di un valore appropriato. Qualsiasi valore predefinito per un parametro deve essere valido per tutti gli utenti nella configurazione della distribuzione predefinita.

    ```json
    "parameters": {
      "storageAccountType": {
        "type": "string",
        "defaultValue": "Standard_GRS",
        "metadata": {
          "description": "The type of the new storage account created to store the VM disks."
        }
      }
    }
    ```

* Per specificare un parametro facoltativo, non usare una stringa vuota come valore predefinito. Usare invece un valore letterale o un'espressione del linguaggio per costruire un valore.

   ```json
   "storageAccountName": {
     "type": "string",
     "defaultValue": "[concat('storage', uniqueString(resourceGroup().id))]",
     "metadata": {
       "description": "Name of the storage account"
     }
   }
   ```

* Usare `allowedValues` solo in casi limitati. Usarlo solo quando è necessario assicurarsi che alcuni valori non vengano inclusi nelle opzioni consentite. Se si usa `allowedValues` troppo ampio, è possibile bloccare le distribuzioni valide non mantenendo aggiornato l'elenco.

* Quando il nome di un parametro nel modello corrisponde a un parametro nel comando di distribuzione di PowerShell, Resource Manager risolve questo conflitto di denominazione aggiungendo il suffisso **FromTemplate** al parametro del modello. Se, ad esempio, si include un parametro denominato **ResourceGroupName** nel modello, si crea un conflitto con il parametro **ResourceGroupName** nel cmdlet [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). Durante la distribuzione verrà quindi richiesto di specificare un valore per **ResourceGroupNameFromTemplate**.

### <a name="security-recommendations-for-parameters"></a>Raccomandazioni sulla sicurezza per i parametri

* Usare sempre i parametri per i nomi utente e le password (o i segreti).

* Usare `securestring` per tutte le password e i segreti. Se si passano dati sensibili in un oggetto JSON, usare il tipo `secureObject`. Non è possibile leggere i parametri di modello di tipo stringa sicura o oggetto sicuro dopo la distribuzione delle risorse.

    ```json
    "parameters": {
      "secretValue": {
        "type": "securestring",
        "metadata": {
          "description": "The value of the secret to store in the vault."
        }
      }
    }
    ```

* Non fornire valori predefiniti per nomi utente, password o valori che richiedono un tipo `secureString`.

* Non fornire valori predefiniti per le proprietà che aumentano la superficie di attacco dell'applicazione.

### <a name="location-recommendations-for-parameters"></a>Raccomandazioni sulla posizione per i parametri

* Usare un parametro per specificare la posizione delle risorse e impostare il valore predefinito su `resourceGroup().location`. Fornire un parametro location consente agli utenti del modello di specificare un percorso in cui sono autorizzati a distribuire le risorse.

   ```json
   "parameters": {
     "location": {
       "type": "string",
       "defaultValue": "[resourceGroup().location]",
       "metadata": {
         "description": "The location in which the resources should be deployed."
       }
     }
   }
   ```

* Non specificare `allowedValues` per il parametro location. Le posizioni specificate potrebbero non essere disponibili in tutti i cloud.

* Usare il valore del parametro location per le risorse che potrebbero essere nella stessa posizione. Questo approccio permette di ridurre al minimo il numero di volte in cui gli utenti devono dare informazioni sulla posizione.

* Per le risorse che non sono disponibili in tutte le posizioni, usare un parametro distinto oppure specificare un valore letterale per location.

## <a name="variables"></a>Variabili

Le informazioni seguenti possono essere utili quando si usano le [variabili](template-variables.md):

* Usare il caso Camel per i nomi delle variabili.

* Usare le variabili per i valori da usare più volte in un modello. Se un valore viene usato una sola volta, un valore hardcoded facilita la lettura del modello.

* Usare le variabili per i valori costruiti da una disposizione complessa delle funzioni del modello. Il modello è più facile da leggere quando l'espressione complessa viene visualizzata solo nelle variabili.

* Non è possibile usare la funzione [Reference](template-functions-resource.md#reference) nella `variables` sezione del modello. La `reference` funzione deriva il proprio valore dallo stato di runtime della risorsa. ma le variabili vengono risolte durante l'analisi iniziale del modello. Costruire valori che richiedono la `reference` funzione direttamente nella `resources` sezione o `outputs` del modello.

* Includere le variabili per i nomi di risorse che devono essere univoci.

* Usare un [ciclo di copia nelle variabili](copy-variables.md) per creare un modello ripetitivo di oggetti JSON.

* Rimuovere le variabili non usate.

## <a name="api-version"></a>Versione dell'API

Impostare la `apiVersion` proprietà su una versione API hardcoded per il tipo di risorsa. Quando si crea un nuovo modello, è consigliabile usare la versione più recente dell'API per un tipo di risorsa. Per determinare i valori disponibili, vedere [riferimento ai modelli](/azure/templates/).

Quando il modello funziona come previsto, è consigliabile continuare a usare la stessa versione API. Con la stessa versione dell'API, non è necessario preoccuparsi delle modifiche di rilievo che potrebbero essere introdotte nelle versioni successive.

Non usare un parametro per la versione dell'API. Le proprietà e i valori delle risorse possono variare in base alla versione dell'API. Quando la versione dell'API è impostata su un parametro, IntelliSense negli editor di codice non può determinare lo schema corretto. Se si passa una versione dell'API che non corrisponde alle proprietà del modello, la distribuzione avrà esito negativo.

Non usare le variabili per la versione dell'API. In particolare, non usare la [funzione Providers](template-functions-resource.md#providers) per ottenere dinamicamente le versioni dell'API durante la distribuzione. La versione dell'API recuperata in modo dinamico potrebbe non corrispondere alle proprietà del modello.

## <a name="resource-dependencies"></a>Dipendenze delle risorse

Quando si definiscono le [dipendenze](define-resource-dependency.md) da impostare, attenersi alle linee guida seguenti:

* Usare la `reference` funzione e passare il nome della risorsa per impostare una dipendenza implicita tra le risorse che devono condividere una proprietà. Non aggiungere un elemento `dependsOn` esplicito quando è già stata definita una dipendenza implicita. Questo approccio riduce il rischio di creare dipendenze non necessarie. Per un esempio di impostazione di una dipendenza implicita, vedere [funzioni di riferimento ed elenco](define-resource-dependency.md#reference-and-list-functions).

* Impostare una risorsa figlio come dipendente dalla risorsa padre.

* Le risorse con l'[elemento condition](conditional-resource-deployment.md) impostato su false vengono automaticamente rimosse dall'ordine di dipendenza. Impostare le dipendenze come se la risorsa venisse sempre distribuita.

* Consentire la propagazione a catena delle dipendenze senza impostarle esplicitamente. Ad esempio, una macchina virtuale dipende da un'interfaccia di rete virtuale e tale interfaccia dipende da una rete virtuale e indirizzi IP pubblici. La macchina virtuale viene quindi distribuita dopo tutte e tre le risorse, ma non deve essere impostata esplicitamente come dipendente da tutte e tre. Questo approccio offre chiarezza nell'ordine delle dipendenze e semplifica la successiva modifica del modello.

* Se un valore può essere determinato prima della distribuzione, provare a distribuire le risorse senza una dipendenza. Se un valore di configurazione richiede il nome di un'altra risorsa, ad esempio, potrebbe non essere necessaria una dipendenza. Questa indicazione non è sempre valida perché alcune risorse verificano l'esistenza dell'altra risorsa. Se viene visualizzato un errore, aggiungere una dipendenza.

## <a name="resources"></a>Risorse

Le informazioni seguenti possono essere utili quando si usano le [risorse](template-syntax.md#resources):

* Per consentire ad altri collaboratori di comprendere lo scopo della risorsa, specificare `comments` per ogni risorsa nel modello.

    ```json
    "resources": [
      {
        "name": "[variables('storageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2019-06-01",
        "location": "[resourceGroup().location]",
        "comments": "This storage account is used to store the VM disks.",
          ...
      }
    ]
    ```

* Se si usa un *endpoint pubblico* nel modello, come ad esempio un endpoint pubblico di archiviazione BLOB di Azure, *non impostare come hardcoded* lo spazio dei nomi. Utilizzare la `reference` funzione per recuperare in modo dinamico lo spazio dei nomi. Questo approccio consente di distribuire il modello in ambienti diversi dello spazio dei nomi pubblico senza dover modificare manualmente l'endpoint nel modello. Impostare la versione dell'API sulla stessa versione usata per l'account di archiviazione nel modello.

    ```json
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": "true",
        "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), '2019-06-01').primaryEndpoints.blob]"
      }
    }
    ```

   Se l'account di archiviazione viene distribuito nello stesso modello che si sta creando e il nome dell'account di archiviazione non è condiviso con un'altra risorsa nel modello, non è necessario specificare lo spazio dei nomi del provider o `apiVersion` quando si fa riferimento alla risorsa. Nell'esempio seguente viene illustrata la sintassi semplificata.

    ```json
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": "true",
        "storageUri": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

   È anche possibile fare riferimento a un account di archiviazione esistente che si trova in un gruppo di risorse diverso.

    ```json
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": "true",
        "storageUri": "[reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('existingStorageAccountName')), '2019-06-01').primaryEndpoints.blob]"
      }
    }
    ```

* Assegnare indirizzi IP pubblici a una macchina virtuale solo se richiesto per un'applicazione. Per connettersi a una macchina virtuale (VM) per il debug o per la gestione o a scopi amministrativi, usare le regole NAT in ingresso, un gateway di rete virtuale o un jumpbox.

     Per altre informazioni sulla connessione alle macchine virtuali, vedere:

   * [Eseguire macchine virtuali per un'architettura a più livelli in Azure](/azure/architecture/reference-architectures/n-tier/n-tier-sql-server)
   * [Configurare l'accesso WinRM per le macchine virtuali in Azure Resource Manager](../../virtual-machines/windows/winrm.md)
   * [Consentire l'accesso esterno alla macchina virtuale tramite il portale di Azure](../../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Consentire l'accesso esterno alla macchina virtuale tramite PowerShell](../../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Consentire l'accesso esterno alla macchina virtuale Linux tramite l'interfaccia della riga di comando di Azure](../../virtual-machines/linux/nsg-quickstart.md)

* La `domainNameLabel` proprietà per gli indirizzi IP pubblici deve essere univoca. Il `domainNameLabel` valore deve avere una lunghezza compresa tra 3 e 63 caratteri e seguire le regole specificate dall'espressione regolare: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$` . Poiché la `uniqueString` funzione genera una stringa di 13 caratteri, il `dnsPrefixString` parametro è limitato a 50 caratteri.

    ```json
    "parameters": {
      "dnsPrefixString": {
        "type": "string",
        "maxLength": 50,
        "metadata": {
          "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
        }
      }
    },
    "variables": {
      "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
    }
    ```

* Quando si aggiunge una password a un'estensione di script personalizzata, utilizzare la `commandToExecute` proprietà nella `protectedSettings` Proprietà.

    ```json
    "properties": {
      "publisher": "Microsoft.Azure.Extensions",
      "type": "CustomScript",
      "typeHandlerVersion": "2.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
        ]
      },
      "protectedSettings": {
        "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
      }
    }
    ```

   > [!NOTE]
   > Per assicurarsi che i segreti vengano crittografati quando vengono passati come parametri alle macchine virtuali e alle estensioni, usare la `protectedSettings` proprietà delle estensioni pertinenti.

* Specificare i valori espliciti per le proprietà con valori predefiniti che potrebbero cambiare nel tempo. Se ad esempio si distribuisce un cluster AKS, è possibile specificare o omettere la `kubernetesVersion` Proprietà. Se non viene specificato, [il cluster viene impostato per impostazione predefinita sulla versione secondaria N-1 e sulla patch più recente](../../aks/supported-kubernetes-versions.md#azure-portal-and-cli-versions). Quando si distribuisce il cluster usando un modello ARM, questo comportamento predefinito potrebbe non corrispondere a quello previsto. La ridistribuzione del modello può comportare l'aggiornamento imprevisto del cluster a una nuova versione di Kubernetes. È invece consigliabile specificare un numero di versione esplicito e quindi modificarlo manualmente quando si è pronti ad aggiornare il cluster.

## <a name="use-test-toolkit"></a>USA test Toolkit

ARM template test Toolkit è uno script che controlla se il modello usa le procedure consigliate. Quando il modello non è conforme alle procedure consigliate, restituisce un elenco di avvisi con le modifiche suggerite. Il Toolkit di test può essere utile per apprendere come implementare le procedure consigliate nel modello.

Una volta completato il modello, eseguire il Toolkit di test per verificare se è possibile migliorare l'implementazione. Per altre informazioni, vedere [use ARM template test Toolkit](test-toolkit.md).

## <a name="next-steps"></a>Passaggi successivi

* Per informazioni sulla struttura del file di modello, vedere [comprendere la struttura e la sintassi dei modelli ARM](template-syntax.md).
* Per consigli su come compilare modelli che funzionano in tutti gli ambienti cloud di Azure, vedere [sviluppare modelli ARM per la coerenza del cloud](templates-cloud-consistency.md).
