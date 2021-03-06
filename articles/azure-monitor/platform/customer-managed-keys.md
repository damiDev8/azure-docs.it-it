---
title: Chiave gestita dal cliente di Monitoraggio di Azure
description: Informazioni e procedure per configurare la chiave gestita dal cliente per crittografare i dati nelle aree di lavoro Log Analytics usando una chiave di Azure Key Vault.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 01/10/2021
ms.openlocfilehash: 9d8d37e1b161dfc8344d7ff03bc0093d23f86101
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98917833"
---
# <a name="azure-monitor-customer-managed-key"></a>Chiave gestita dal cliente di Monitoraggio di Azure 

I dati in monitoraggio di Azure vengono crittografati con le chiavi gestite da Microsoft. È possibile usare la propria chiave di crittografia per proteggere i dati e le query salvate nelle aree di lavoro. Quando si specifica una chiave gestita dal cliente, questa chiave viene usata per proteggere e controllare l'accesso ai dati e, una volta configurata, i dati inviati alle aree di lavoro vengono crittografati con la chiave Azure Key Vault. Le chiavi gestite dal cliente offrono maggiore flessibilità per gestire i controlli di accesso.

È consigliabile esaminare [limiti e vincoli](#limitationsandconstraints) di seguito prima di procedere alla configurazione.

## <a name="customer-managed-key-overview"></a>Panoramica della chiave gestita dal cliente

La [crittografia di](../../security/fundamentals/encryption-atrest.md) dati inattivi è un requisito comune per la privacy e la sicurezza nelle organizzazioni. È possibile lasciare che Azure gestisca completamente la crittografia inattiva, mentre sono disponibili diverse opzioni per la gestione delle chiavi di crittografia e crittografia.

Monitoraggio di Azure garantisce che tutti i dati e le query salvate siano crittografati a riposo usando chiavi gestite da Microsoft (MMK). Monitoraggio di Azure offre anche un'opzione per la crittografia usando la propria chiave archiviata nel [Azure Key Vault](../../key-vault/general/overview.md), che offre il controllo per revocare l'accesso ai dati in qualsiasi momento. L'uso della crittografia da parte di monitoraggio di Azure è identico a quello di [Azure Storage Encryption](../../storage/common/storage-service-encryption.md#about-azure-storage-encryption) .

La chiave gestita dal cliente viene distribuita su [cluster dedicati](../log-query/logs-dedicated-clusters.md) garantendo un livello di protezione e un controllo più elevati. I dati inseriti in cluster dedicati vengono crittografati due volte, una volta a livello di servizio usando chiavi gestite da Microsoft o chiavi gestite dal cliente e una volta a livello di infrastruttura usando due algoritmi di crittografia diversi e due chiavi diverse. La [crittografia doppia](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) protegge da uno scenario in cui uno degli algoritmi o delle chiavi di crittografia può essere compromesso. In questo caso, il livello di crittografia aggiuntivo continua a proteggere i dati. Il cluster dedicato consente inoltre di proteggere i dati con il controllo dell' [archivio protetto](#customer-lockbox-preview) .

I dati inseriti negli ultimi 14 giorni vengono anche mantenuti nella cache ad accesso frequente (con supporto SSD) per un efficace funzionamento del motore di query. Questi dati rimangono crittografati con le chiavi di Microsoft indipendentemente dalla configurazione della chiave gestita dal cliente, ma il controllo sui dati SSD rispetta la [revoca](#key-revocation)delle chiavi. Si sta lavorando per crittografare i dati SSD con la chiave gestita dal cliente nella prima metà del 2021.

Log Analytics cluster dedicati usano un [modello di determinazione dei prezzi](../log-query/logs-dedicated-clusters.md#cluster-pricing-model) per la prenotazione della capacità a partire da 1000 GB/giorno.

## <a name="how-customer-managed-key-works-in-azure-monitor"></a>Funzionamento della chiave gestita dal cliente in monitoraggio di Azure

Monitoraggio di Azure usa l'identità gestita per concedere l'accesso al Azure Key Vault. L'identità del cluster Log Analytics è supportata a livello di cluster. Per consentire la protezione delle chiavi gestite dal cliente su più aree di lavoro, una nuova risorsa *Cluster* log Analytics viene eseguita come connessione di identità intermedia tra la Key Vault e le aree di lavoro log Analytics. Lo spazio di archiviazione del cluster usa l'identità gestita \' associata alla risorsa *cluster* per l'autenticazione al Azure Key Vault tramite Azure Active Directory. 

Dopo la configurazione della chiave gestita dal cliente, i nuovi dati inseriti nelle aree di lavoro collegate al cluster dedicato vengono crittografati con la chiave. È possibile scollegare le aree di lavoro dal cluster in qualsiasi momento. I nuovi dati vengono quindi inseriti nell'archiviazione Log Analytics e crittografati con la chiave Microsoft, mentre è possibile eseguire facilmente query sui dati nuovi e obsoleti.

> [!IMPORTANT]
> La funzionalità chiave gestita dal cliente è a livello di area. Le aree di lavoro Azure Key Vault, cluster e Log Analytics collegate devono trovarsi nella stessa area, ma possono trovarsi in sottoscrizioni diverse.

![Panoramica della chiave gestita dal cliente](media/customer-managed-keys/cmk-overview.png)

1. Key Vault
2. La risorsa *Cluster* di Log Analytics con identità gestita con le autorizzazioni per Key Vault: l'identità viene propagata all'archivio cluster sottostante dedicato Log Analytics
3. Cluster Log Analytics dedicato
4. Aree di lavoro collegate alla risorsa *cluster* 

### <a name="encryption-keys-operation"></a>Funzionamento delle chiavi di crittografia

La crittografia dei dati di archiviazione include tre tipi di chiavi:

- Chiave di crittografia della chiave **KEK** (chiave gestita dal cliente)
- **AEK** - chiave di crittografia dell'account
- **DEK** - chiave di crittografia dei dati

Sono applicabili le regole seguenti:

- Gli account di archiviazione del cluster Log Analytics generano una chiave di crittografia univoca per ogni account di archiviazione, noto come AEK.
- L'AEK viene usato per derivare chiavi DEK, ovvero le chiavi usate per crittografare ogni blocco di dati scritti su disco.
- Quando si configura la chiave in Key Vault e vi si fa riferimento nel cluster, archiviazione di Azure invia le richieste al Azure Key Vault per eseguire il wrapping e annullare il wrapping dell'AEK per eseguire operazioni di crittografia e decrittografia dei dati.
- La KEK non lascia mai il Key Vault e, nel caso di una chiave HSM, non lascia mai l'hardware.
- Archiviazione di Azure usa l'identità gestita associata alla risorsa *cluster* per l'autenticazione e l'accesso ai Azure Key Vault tramite Azure Active Directory.

### <a name="customer-managed-key-provisioning-steps"></a>Procedura di provisioning delle chiavi Customer-Managed

1. Creazione di Azure Key Vault e archiviazione della chiave
1. Creazione del cluster
1. Concessione delle autorizzazioni a Key Vault
1. Aggiornamento del cluster con i dettagli dell'identificatore di chiave
1. Collegamento di aree di lavoro Log Analytics

La configurazione della chiave gestita dal cliente non è attualmente supportata in portale di Azure e il provisioning può essere eseguito tramite [PowerShell](/powershell/module/az.operationalinsights/), l' [interfaccia](/cli/azure/monitor/log-analytics) della riga di comando o le richieste [Rest](/rest/api/loganalytics/) .

### <a name="asynchronous-operations-and-status-check"></a>Operazioni asincrone e controllo dello stato

Alcuni passaggi di configurazione vengono eseguiti in modo asincrono perché non possono essere completati rapidamente. La `status` risposta in può essere una delle seguenti:' InProgress ',' Updating ',' deleting ',' Success o ' failed ' con codice di errore.

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

N/D

# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

N/D

# <a name="powershell"></a>[PowerShell](#tab/powershell)

N/D

# <a name="rest"></a>[REST](#tab/rest)

Quando si usa REST, la risposta restituisce inizialmente un codice di stato HTTP 202 (accettato) e un'intestazione con la proprietà *Azure-AsyncOperation* :
```json
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2020-08-01"
```

È possibile controllare lo stato dell'operazione asincrona inviando una richiesta GET all'endpoint nell'intestazione *Azure-AsyncOperation* :
```rst
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2020-08-01
Authorization: Bearer <token>
```

---

## <a name="storing-encryption-key-kek"></a>Archiviazione della chiave di crittografia (KEK)

Creare o usare un'istanza di Azure Key Vault già disponibile per generare o importare una chiave da usare per la crittografia dei dati. L'istanza di Azure Key Vault deve essere configurata come recuperabile per proteggere la chiave e l'accesso ai dati in Monitoraggio di Azure. È possibile verificare questa configurazione nelle proprietà dell'istanza di Key Vault. Devono essere abilitate sia *Eliminazione temporanea* che *Protezione dall'eliminazione*.

![Impostazioni Eliminazione temporanea e Protezione dall'eliminazione](media/customer-managed-keys/soft-purge-protection.png)

Queste impostazioni possono essere aggiornate in Key Vault tramite l'interfaccia della riga di comando e PowerShell:

- [eliminazione temporanea](../../key-vault/general/soft-delete-overview.md)
- [Protezione dall'eliminazione](../../key-vault/general/soft-delete-overview.md#purge-protection) protegge dall'eliminazione forzata del segreto/insieme di credenziali anche dopo l'eliminazione temporanea

## <a name="create-cluster"></a>Creare cluster

I cluster supportano due [tipi di identità gestiti](../../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types): assegnati dal sistema e assegnati dall'utente, mentre una singola identità può essere definita in un cluster a seconda dello scenario. 
- L'identità gestita assegnata dal sistema è più semplice e viene generata automaticamente con la creazione del cluster quando Identity `type` è impostato su "*SystemAssigned*". Questa identità può essere usata in un secondo momento per concedere l'accesso di archiviazione al Key Vault per le operazioni di wrapping e unwrap. 
  
  Impostazioni di identità nel cluster per l'identità gestita assegnata dal sistema
  ```json
  {
    "identity": {
      "type": "SystemAssigned"
      }
  }
  ```

- Se si vuole configurare la chiave gestita dal cliente durante la creazione del cluster, è necessario disporre di una chiave e di un'identità assegnata dall'utente nella Key Vault in anticipo, quindi creare il cluster con queste impostazioni: Identity `type` As "*UserAssigned*" `UserAssignedIdentities` con l' *ID risorsa* dell'identità.

  Impostazioni Identity nel cluster per l'identità gestita assegnata dall'utente
  ```json
  {
  "identity": {
  "type": "UserAssigned",
    "userAssignedIdentities": {
      "subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft. ManagedIdentity/UserAssignedIdentities/<cluster-assigned-managed-identity>"
      }
  }
  ```

> [!IMPORTANT]
> Non è possibile usare l'identità gestita assegnata dall'utente se il Key Vault è in Private-Link (vNet). In questo scenario è possibile utilizzare l'identità gestita assegnata dal sistema.

Seguire la procedura illustrata nell' [articolo sui cluster dedicati](../log-query/logs-dedicated-clusters.md#creating-a-cluster). 

## <a name="grant-key-vault-permissions"></a>Concedere le autorizzazioni di Key Vault

Creare criteri di accesso in Key Vault per concedere le autorizzazioni al cluster. Queste autorizzazioni vengono usate dall'archiviazione di monitoraggio di Azure sottostante. Aprire il Key Vault in portale di Azure e fare clic su *"criteri di accesso"* e quindi su *"+ Aggiungi criteri di accesso"* per creare un criterio con le impostazioni seguenti:

- Autorizzazioni per le chiavi: selezionare *' Get '*, *' wrap Key '* e *' Unwrap Key '*.
- Selezionare l'entità di protezione: a seconda del tipo di identità usato nel cluster (sistema o identità gestita assegnata dall'utente) immettere il nome del cluster o l'ID dell'entità cluster per l'identità gestita assegnata dal sistema o il nome dell'identità gestita assegnata dall'utente.

![concedere le autorizzazioni di Key Vault](media/customer-managed-keys/grant-key-vault-permissions-8bit.png)

L'autorizzazione *Recupera* è necessaria per verificare che l'istanza di Key Vault sia configurata come recuperabile per proteggere la chiave e l'accesso ai dati di Monitoraggio di Azure.

## <a name="update-cluster-with-key-identifier-details"></a>Aggiornare il cluster con i dettagli dell'identificatore di chiave

Per tutte le operazioni nel cluster è necessaria l' `Microsoft.OperationalInsights/clusters/write` autorizzazione azione. Questa autorizzazione può essere concessa tramite il proprietario o il collaboratore che contiene l' `*/write` azione o tramite il ruolo di collaboratore log Analytics che contiene l' `Microsoft.OperationalInsights/*` azione.

Questo passaggio Aggiorna l'archiviazione di monitoraggio di Azure con la chiave e la versione da usare per la crittografia dei dati. Quando viene aggiornata, la nuova chiave viene usata per eseguire il wrapping e annullare il wrapping della chiave di archiviazione (AEK).

Selezionare la versione corrente della chiave in Azure Key Vault per ottenere i dettagli dell'identificatore di chiave.

![Concedere le autorizzazioni di Key Vault](media/customer-managed-keys/key-identifier-8bit.png)

Aggiornare KeyVaultProperties nel cluster con i dettagli dell'identificatore di chiave.

L'operazione è asincrona e può richiedere del tempo per il completamento.

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

N/D

# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

```azurecli
az monitor log-analytics cluster update --name "cluster-name" --resource-group "resource-group-name" --key-name "key-name" --key-vault-uri "key-uri" --key-version "key-version"
```
# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -KeyVaultUri "key-uri" -KeyName "key-name" -KeyVersion "key-version"
```

# <a name="rest"></a>[REST](#tab/rest)

```rst
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/cluster-name?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "keyVaultProperties": {
      "keyVaultUri": "https://key-vault-name.vault.azure.net",
      "kyName": "key-name",
      "keyVersion": "current-version"
  },
  "sku": {
    "name": "CapacityReservation",
    "capacity": 1000
  }
}
```

**Risposta**

Il completamento della propagazione della chiave richiede alcuni minuti. È possibile controllare lo stato di aggiornamento in due modi:
1. Copiare il valore dell'URL di Azure-AsyncOperation dalla risposta e seguire la [verifica dello stato delle operazioni asincrone](#asynchronous-operations-and-status-check).
2. Inviare una richiesta GET nel cluster ed esaminare le proprietà *KeyVaultProperties* . La chiave aggiornata di recente dovrebbe restituire nella risposta.

Una risposta alla richiesta GET dovrebbe avere un aspetto simile al seguente quando l'aggiornamento della chiave è completo: 202 (accettato) e l'intestazione
```json
{
  "identity": {
    "type": "SystemAssigned",
    "tenantId": "tenant-id",
    "principalId": "principle-id"
    },
  "sku": {
    "name": "capacityReservation",
    "capacity": 1000,
    "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
    },
  "properties": {
    "keyVaultProperties": {
      "keyVaultUri": "https://key-vault-name.vault.azure.net",
      "kyName": "key-name",
      "keyVersion": "current-version"
      },
    "provisioningState": "Succeeded",
    "billingType": "cluster",
    "clusterId": "cluster-id"
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```

---

## <a name="link-workspace-to-cluster"></a>Collega area di lavoro a cluster

> [!IMPORTANT]
> Questo passaggio deve essere eseguito solo dopo il completamento del provisioning del cluster Log Analytics. Se si collegano le aree di lavoro e si inseriscono dati prima del provisioning, i dati inseriti verranno eliminati e non saranno recuperabili.

Per eseguire questa operazione, è necessario disporre delle autorizzazioni ' Write ' sia per l'area di lavoro che per il cluster, tra cui `Microsoft.OperationalInsights/workspaces/write` e `Microsoft.OperationalInsights/clusters/write` .

Seguire la procedura illustrata nell' [articolo sui cluster dedicati](../log-query/logs-dedicated-clusters.md#link-a-workspace-to-cluster).

## <a name="key-revocation"></a>Revoca della chiave

> [!IMPORTANT]
> - Il modo consigliato per revocare l'accesso ai dati è disabilitare la chiave o eliminare i criteri di accesso nel Key Vault.
> - L'impostazione del cluster `identity` `type` su "None" consente anche di revocare l'accesso ai dati, ma questo approccio non è consigliato perché non è possibile ripristinare la revoca quando si ridichiara il `identity` nel cluster senza aprire la richiesta di supporto.

L'archiviazione del cluster rispetta sempre le modifiche apportate alle autorizzazioni chiave entro un'ora o prima e l'archiviazione non sarà più disponibile. Tutti i nuovi dati inseriti nelle aree di lavoro collegate al cluster vengono eliminati e non saranno recuperabili, i dati diventeranno inaccessibili e le query su tali aree di lavoro avranno esito negativo. I dati inseriti in precedenza rimangono nello spazio di archiviazione purché il cluster e le aree di lavoro non vengano eliminati. I dati inaccessibili sono regolati dai criteri di conservazione dei dati e verranno eliminati al raggiungimento della scadenza della conservazione. I dati inseriti negli ultimi 14 giorni vengono anche mantenuti nella cache ad accesso frequente (con supporto SSD) per un efficace funzionamento del motore di query. Questa operazione viene eliminata durante l'operazione di revoca della chiave e diventa inaccessibile.

L'archiviazione del cluster controlla periodicamente il Key Vault per tentare di annullare il wrapping della chiave di crittografia e, una volta eseguito l'accesso, l'inserimento e la query dei dati vengono ripresi entro 30 minuti.

## <a name="key-rotation"></a>Rotazione delle chiavi

La rotazione della chiave gestita dal cliente richiede un aggiornamento esplicito al cluster con la nuova versione della chiave in Azure Key Vault. [Aggiornare il cluster con i dettagli dell'identificatore di chiave](#update-cluster-with-key-identifier-details). Se non si aggiorna la nuova versione della chiave nel cluster, l'archiviazione del cluster Log Analytics continuerà a usare la chiave precedente per la crittografia. Se si disabilita o si elimina la chiave precedente prima di aggiornare la nuova chiave nel cluster, si otterrà lo stato di [revoca della chiave](#key-revocation) .

Tutti i dati rimarranno accessibili dopo l'operazione di rotazione della chiave perché i dati vengono sempre crittografati con la chiave di crittografia dell'account (AEK), mentre la chiave AEK viene ora crittografata con la nuova versione della chiave di crittografia della chiave (KEK) in Key Vault.

## <a name="customer-managed-key-for-saved-queries"></a>Chiave gestita dal cliente per le query salvate

Il linguaggio di query utilizzato nel Log Analytics è espressivo e può contenere informazioni riservate nei commenti aggiunti alle query o nella sintassi della query. Alcune organizzazioni richiedono che tali informazioni vengano mantenute protette con i criteri chiave gestiti dal cliente ed è necessario salvare le query crittografate con la chiave. Monitoraggio di Azure consente di archiviare le query salvate e per le *ricerche* con *avvisi di log* crittografate con la chiave nel proprio account di archiviazione quando si è connessi all'area di lavoro. 

> [!NOTE]
> Log Analytics le query possono essere salvate in diversi archivi a seconda dello scenario utilizzato. Le query rimangono crittografate con la chiave Microsoft (MMK) negli scenari seguenti indipendentemente dalla configurazione della chiave gestita dal cliente, ovvero cartelle di lavoro in monitoraggio di Azure, dashboard di Azure, app per la logica di Azure, Azure Notebooks e manuali operativi di automazione.

Quando si porta la propria risorsa di archiviazione (BYOS) e la si collega all'area di lavoro, il servizio carica le query *salvate* e di *log-alerts* nell'account di archiviazione. Ciò significa che è possibile controllare l'account di archiviazione e i [criteri di crittografia](../../storage/common/customer-managed-keys-overview.md) dei dati inattivi usando la stessa chiave usata per crittografare i dati in log Analytics cluster o una chiave diversa. Si sarà tuttavia responsabili dei costi associati a tale account di archiviazione. 

**Considerazioni prima di impostare la chiave gestita dal cliente per le query**
* È necessario disporre delle autorizzazioni di scrittura per l'area di lavoro e l'account di archiviazione
* Assicurarsi di creare l'account di archiviazione nella stessa area in cui si trova l'area di lavoro Log Analytics
* Il *salvataggio delle ricerche* nell'archiviazione viene considerato come artefatti del servizio e il relativo formato potrebbe cambiare
* Le *ricerche di salvataggio* esistenti vengono rimosse dall'area di lavoro. Copiare ed eventuali *ricerche di salvataggio* necessarie prima della configurazione. È possibile visualizzare le *ricerche salvate* usando  [PowerShell](/powershell/module/az.operationalinsights/get-azoperationalinsightssavedsearch)
* La cronologia delle query non è supportata e non sarà possibile visualizzare le query eseguite
* È possibile collegare un singolo account di archiviazione all'area di lavoro allo scopo di salvare le query, ma è possibile usarlo per le query di *ricerca salvate* e di *log-alerts*
* Il PIN al dashboard non è supportato

**Configurare BYOS per le query di ricerca salvate**

Collegare un account di archiviazione per la *query* all'area di lavoro: le query *salvate* vengono salvate nell'account di archiviazione. 

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

N/D

# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

```azurecli
$storageAccountId = '/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage name>'
az monitor log-analytics workspace linked-storage create --type Query --resource-group "resource-group-name" --workspace-name "workspace-name" --storage-accounts $storageAccountId
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
$storageAccount.Id = Get-AzStorageAccount -ResourceGroupName "resource-group-name" -Name "storage-account-name"
New-AzOperationalInsightsLinkedStorageAccount -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -DataSourceType Query -StorageAccountIds $storageAccount.Id
```

# <a name="rest"></a>[REST](#tab/rest)

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>/linkedStorageAccounts/Query?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "dataSourceType": "Query", 
    "storageAccountIds": 
    [
      "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-account-name>"
    ]
  }
}
```

---

Dopo la configurazione, qualsiasi nuova query di *ricerca salvata* verrà salvata nella risorsa di archiviazione.

**Configurare BYOS per le query log-alerts**

Collegare un account di archiviazione per gli *avvisi* all'area di lavoro: le query *log-alerts* vengono salvate nell'account di archiviazione. 

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

N/D

# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

```azurecli
$storageAccountId = '/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage name>'
az monitor log-analytics workspace linked-storage create --type ALerts --resource-group "resource-group-name" --workspace-name "workspace-name" --storage-accounts $storageAccountId
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
$storageAccount.Id = Get-AzStorageAccount -ResourceGroupName "resource-group-name" -Name "storage-account-name"
New-AzOperationalInsightsLinkedStorageAccount -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -DataSourceType Alerts -StorageAccountIds $storageAccount.Id
```

# <a name="rest"></a>[REST](#tab/rest)

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>/linkedStorageAccounts/Alerts?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "dataSourceType": "Alerts", 
    "storageAccountIds": 
    [
      "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-account-name>"
    ]
  }
}
```

---

Dopo la configurazione, qualsiasi nuova query di avviso verrà salvata nella risorsa di archiviazione.

## <a name="customer-lockbox-preview"></a>Customer Lockbox (anteprima)

Archivio dati consente di approvare o rifiutare la richiesta del tecnico Microsoft per accedere ai dati durante una richiesta di supporto.

In monitoraggio di Azure questo controllo sui dati nelle aree di lavoro collegate al cluster Log Analytics dedicato. Il controllo dell'archivio protetto si applica ai dati archiviati in un Log Analytics cluster dedicato, dove viene mantenuto isolato negli account di archiviazione del cluster nella sottoscrizione protetta da file protetto.  

Altre informazioni su [Customer Lockbox per Microsoft Azure](../../security/fundamentals/customer-lockbox-overview.md)

## <a name="customer-managed-key-operations"></a>Operazioni di Customer-Managed chiave

Customer-Managed chiave viene fornita nel cluster dedicato e queste operazioni sono indicate in un [articolo cluster dedicato](../log-query/logs-dedicated-clusters.md#change-cluster-properties)

- Ottenere tutti i cluster nel gruppo di risorse  
- Ottenere tutti i cluster nella sottoscrizione
- Aggiornare la *prenotazione di capacità* nel cluster
- Aggiornare *billingType* nel cluster
- Scollegare un'area di lavoro dal cluster
- Eliminare il cluster

## <a name="limitations-and-constraints"></a>Limiti e vincoli

- Il numero massimo di cluster per area e sottoscrizione è 2

- Il numero massimo di aree di lavoro che possono essere collegate a un cluster è 1000

- È possibile collegare un'area di lavoro al cluster e quindi scollegarla. Il numero di operazioni di collegamento dell'area di lavoro su un'area di lavoro specifica è limitato a 2 in un periodo di 30 giorni.

- La crittografia della chiave gestita dal cliente si applica ai dati appena inseriti dopo l'ora di configurazione. I dati inseriti prima della configurazione rimangono crittografati con la chiave Microsoft. È possibile eseguire facilmente query sui dati inseriti prima e dopo la configurazione della chiave gestita dal cliente.

- Il Azure Key Vault deve essere configurato come reversibile. Queste proprietà non sono abilitate per impostazione predefinita e devono essere configurate tramite CLI o PowerShell:<br>
  - [eliminazione temporanea](../../key-vault/general/soft-delete-overview.md)
  - La [protezione ripulisce](../../key-vault/general/soft-delete-overview.md#purge-protection) deve essere attivata per prevenire l'eliminazione forzata del segreto/insieme di credenziali anche dopo l'eliminazione temporanea.

- Il passaggio del cluster a un altro gruppo di risorse o a una sottoscrizione non è attualmente supportato.

- Il Azure Key Vault, il cluster e le aree di lavoro devono trovarsi nella stessa area e nello stesso tenant di Azure Active Directory (Azure AD), ma possono trovarsi in sottoscrizioni diverse.

- L'archivio protetto non è attualmente disponibile in Cina. 

- La [crittografia doppia](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) viene configurata automaticamente per i cluster creati a partire dal 2020 ottobre nelle aree supportate. È possibile verificare se il cluster è configurato per la crittografia doppia inviando una richiesta GET nel cluster e osservando che il `isDoubleEncryptionEnabled` valore è `true` per i cluster con crittografia doppia abilitata. 
  - Se si crea un cluster e si riceve un errore "<Region-Name> non supporta la crittografia doppia per i cluster.", è comunque possibile creare il cluster senza crittografia doppia aggiungendo `"properties": {"isDoubleEncryptionEnabled": false}` nel corpo della richiesta REST.
  - Non è possibile modificare l'impostazione di crittografia doppia dopo la creazione del cluster.

  - Se il cluster è impostato con l'identità gestita assegnata dall'utente, `UserAssignedIdentities` l'impostazione di con `None` sospende il cluster e impedisce l'accesso ai dati, ma non è possibile ripristinare la revoca e attivare il cluster senza aprire la richiesta di supporto. Questa limitazione non viene applicata all'identità gestita assegnata dal sistema.

  - Non è possibile usare la chiave gestita dal cliente con identità gestita assegnata dall'utente se il Key Vault è in Private-Link (vNet). In questo scenario è possibile utilizzare l'identità gestita assegnata dal sistema.

## <a name="troubleshooting"></a>Risoluzione dei problemi

- Comportamento con la disponibilità di Key Vault
  - Nel funzionamento normale: l'archiviazione memorizza la chiave AEK nella cache per brevi periodi di tempo e torna a Key Vault per annullare il wrapping periodicamente.
    
  - Errori di connessione temporanei: l'archiviazione gestisce gli errori temporanei (timeout, errori di connessione, problemi relativi a DNS) consentendo alle chiavi di rimanere nella cache per un po' più di tempo, superando così piccoli problemi di disponibilità. Le funzionalità di query e inserimento proseguono senza interruzioni.
    
  - Sito Live: una indisponibilità di circa 30 minuti comporterà la mancata disponibilità dell'account di archiviazione. La funzionalità di query non sarà disponibile e i dati inseriti verranno memorizzati nella cache per diverse ore usando la chiave Microsoft per evitare la perdita dei dati. Quando viene ripristinato l'accesso Key Vault, la query diventa disponibile e i dati temporanei memorizzati nella cache vengono inseriti nell'archivio dati e crittografati con la chiave gestita dal cliente.

  - Frequenza di accesso a Key Vault: la frequenza con cui l'archivio di Monitoraggio di Azure accede a Key Vault per le operazioni di wrapping e annullamento del wrapping è compresa tra 6 e 60 secondi.

- Se si aggiorna il cluster mentre il cluster si trova nello stato di provisioning o aggiornamento, l'aggiornamento avrà esito negativo.

- Se si verifica un errore di conflitto durante la creazione di un cluster, è possibile che sia stato eliminato il cluster negli ultimi 14 giorni ed è in fase di eliminazione temporanea. Il nome del cluster rimane riservato durante il periodo di eliminazione temporanea e non è possibile creare un nuovo cluster con tale nome. Il nome viene rilasciato dopo il periodo di eliminazione temporanea quando il cluster viene eliminato definitivamente.

- Il collegamento dell'area di lavoro al cluster non riuscirà se è collegato a un altro cluster.

- Se si crea un cluster e si specifica immediatamente il KeyVaultProperties, l'operazione potrebbe non riuscire perché i criteri di accesso non possono essere definiti fino a quando non viene assegnata l'identità del sistema al cluster.

- Se si aggiorna il cluster esistente con KeyVaultProperties e i criteri di accesso alla chiave ' Get ' mancano nell'Key Vault, l'operazione avrà esito negativo.

- Se non si riesce a distribuire il cluster, verificare che le aree di lavoro Azure Key Vault, cluster e Log Analytics collegate si trovino nella stessa area. Le sottoscrizioni possono essere diverse.

- Se si aggiorna la versione della chiave in Key Vault e non si aggiornano i nuovi dettagli dell'identificatore di chiave nel cluster, il cluster Log Analytics continuerà a usare la chiave precedente e i dati diventeranno inaccessibili. Aggiornare i nuovi dettagli dell'identificatore di chiave nel cluster per riprendere l'inserimento dei dati e la possibilità di eseguire query sui dati.

- Alcune operazioni sono lunghe e possono richiedere del tempo per essere completate, ovvero creazione di cluster, aggiornamento della chiave del cluster e eliminazione del cluster. È possibile controllare lo stato dell'operazione in due modi:
  1. Quando si usa REST, copiare il valore di Azure-AsyncOperation URL dalla risposta e seguire la [Verifica dello stato delle operazioni asincrone](#asynchronous-operations-and-status-check).
  2. Inviare una richiesta GET a un cluster o a un'area di lavoro e osservare la risposta. Ad esempio, l'area di lavoro scollegata non avrà *clusterResourceId* in *funzionalità*.

- messaggi di errore
  
  **Creazione cluster**
  -  400--il nome del cluster non è valido. Il nome del cluster può contenere i caratteri a-z, A-Z, 0-9 e la lunghezza di 3-63.
  -  400: il corpo della richiesta è null o in formato non valido.
  -  400--il nome dello SKU non è valido. Impostare il nome dello SKU su capacityReservation.
  -  400: è stata specificata la capacità, ma lo SKU non è capacityReservation. Impostare il nome dello SKU su capacityReservation.
  -  400: capacità mancante nello SKU. Impostare valore capacità su 1000 o superiore nei passaggi di 100 (GB).
  -  400--la capacità nello SKU non è compresa nell'intervallo. Deve essere minimo 1000 e fino alla capacità massima consentita disponibile in ' utilizzo e costo stimato ' nell'area di lavoro.
  -  400: la capacità è bloccata per 30 giorni. La riduzione della capacità è consentita 30 giorni dopo l'aggiornamento.
  -  400--non è stato impostato alcuno SKU. Impostare il nome dello SKU su capacityReservation e il valore della capacità su 1000 o una versione successiva nei passaggi di 100 (GB).
  -  400--Identity è null o vuoto. Impostare Identity con systemAssigned Type.
  -  400--KeyVaultProperties impostati al momento della creazione. Aggiornare KeyVaultProperties dopo la creazione del cluster.
  -  400: non è possibile eseguire l'operazione ora. L'operazione asincrona è in uno stato diverso da succeeded. Prima di eseguire qualsiasi operazione di aggiornamento, il cluster deve completare l'operazione.

  **Aggiornamento cluster**
  -  400--il cluster è in stato di eliminazione. L'operazione asincrona è in corso. Prima di eseguire qualsiasi operazione di aggiornamento, il cluster deve completare l'operazione.
  -  400--KeyVaultProperties non è vuoto ma presenta un formato non valido. Vedere [aggiornamento dell'identificatore di chiave](../platform/customer-managed-keys.md#update-cluster-with-key-identifier-details).
  -  400--non è stato possibile convalidare la chiave in Key Vault. La causa potrebbe essere la mancanza di autorizzazioni o la chiave non esiste. Verificare di aver [impostato i criteri di accesso e chiave](../platform/customer-managed-keys.md#grant-key-vault-permissions) in Key Vault.
  -  400--la chiave non è reversibile. Key Vault deve essere impostato su soft-delete ed Purge-Protection. Vedere la [documentazione di Key Vault](../../key-vault/general/soft-delete-overview.md)
  -  400: non è possibile eseguire l'operazione ora. Attendere il completamento dell'operazione asincrona e riprovare.
  -  400--il cluster è in stato di eliminazione. Attendere il completamento dell'operazione asincrona e riprovare.

  **Ottieni cluster**
    -  404: Impossibile trovare il cluster. è possibile che sia stato eliminato. Se si tenta di creare un cluster con tale nome e si verifica un conflitto, il cluster viene eliminato temporaneamente per 14 giorni. È possibile contattare il supporto tecnico per ripristinarlo o usare un altro nome per creare un nuovo cluster. 

  **Eliminazione del cluster**
    -  409: non è possibile eliminare un cluster in stato di provisioning. Attendere il completamento dell'operazione asincrona e riprovare.

  **Collegamento area di lavoro**
  -  404--area di lavoro non trovata. L'area di lavoro specificata non esiste o è stata eliminata.
  -  409--collegamento dell'area di lavoro o operazione di scollegamento del collegamento nel processo.
  -  400--cluster non trovato, il cluster specificato non esiste o è stato eliminato. Se si tenta di creare un cluster con tale nome e si verifica un conflitto, il cluster viene eliminato temporaneamente per 14 giorni. È possibile contattare il supporto tecnico per recuperarlo.

  **Scollegamento dell'area di lavoro**
  -  404--area di lavoro non trovata. L'area di lavoro specificata non esiste o è stata eliminata.
  -  409--collegamento dell'area di lavoro o operazione di scollegamento del collegamento nel processo.
## <a name="next-steps"></a>Passaggi successivi

- Informazioni su [log Analytics la fatturazione del cluster dedicato](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters)
- Informazioni sulla [progettazione corretta delle aree di lavoro log Analytics](../platform/design-logs-deployment.md)