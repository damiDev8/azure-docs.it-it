---
title: "Guida introduttiva: eseguire il routing della cache di Azure per gli eventi Redis all'endpoint Web con PowerShell"
description: Usare griglia di eventi di Azure per sottoscrivere la cache di Azure per gli eventi Redis, inviare gli eventi a un webhook e gestire gli eventi in un'applicazione Web.
ms.date: 1/5/2021
author: curib
ms.author: cauribeg
ms.topic: quickstart
ms.service: cache
ms.openlocfilehash: 615f3b023ded6583dfedca99f561d09689b86b51
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99055533"
---
# <a name="quickstart-route-azure-cache-for-redis-events-to-web-endpoint-with-powershell"></a>Guida introduttiva: eseguire il routing della cache di Azure per gli eventi Redis all'endpoint Web con PowerShell

La griglia di eventi di Azure è un servizio di gestione degli eventi per il cloud. In questa Guida introduttiva si userà Azure PowerShell per sottoscrivere la cache di Azure per gli eventi Redis, attivare un evento e visualizzare i risultati. 

In genere, si inviano eventi a un endpoint che elabora i dati dell'evento e intraprende azioni. Tuttavia, per semplificare questa Guida introduttiva, sarà possibile inviare eventi a un'app Web che raccoglierà e visualizzerà i messaggi. Quando si completano i passaggi descritti in questa Guida introduttiva, si noterà che i dati dell'evento sono stati inviati all'app Web.

## <a name="setup"></a>Configurazione

Questa Guida introduttiva richiede che sia in esecuzione la versione più recente di Azure PowerShell. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [installare e configurare Azure PowerShell](/powershell/azure/install-Az-ps).

## <a name="sign-in-to-azure"></a>Accedere ad Azure

Accedere alla sottoscrizione di Azure con il comando `Connect-AzAccount` e seguire le istruzioni visualizzate per eseguire l'autenticazione.

```powershell
Connect-AzAccount
```

Questo esempio usa **westus2** e archivia la selezione in una variabile che verrà usata in tutta la procedura.

```powershell
$location = "westus2"
```

## <a name="create-a-resource-group"></a>Creare un gruppo di risorse

Gli argomenti di griglia di eventi vengono distribuiti come singole risorse di Azure e devono essere sottoposti a provisioning in un gruppo di risorse di Azure. Un gruppo di risorse è una raccolta logica in cui le risorse di Azure vengono distribuite e gestite.

Creare un gruppo di risorse con il comando [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup).

L'esempio seguente crea un gruppo di risorse denominato **gridResourceGroup** nella località **westus2**.  

```powershell
$resourceGroup = "gridResourceGroup"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

## <a name="create-an-azure-cache-for-redis-instance"></a>Creare un'istanza di Azure Cache per Redis 

```powershell
New-AzRedisCache
   -ResourceGroupName <String>
   -Name <String>
   -Location <String>
   [-Size <String>]
   [-Sku <String>]
   [-RedisConfiguration <Hashtable>]
   [-EnableNonSslPort <Boolean>]
   [-TenantSettings <Hashtable>]
   [-ShardCount <Int32>]
   [-MinimumTlsVersion <String>]
   [-SubnetId <String>]
   [-StaticIP <String>]
   [-Tag <Hashtable>]
   [-Zone <String[]>]
   [-DefaultProfile <IAzureContextContainer>]
   [-WhatIf]
   [-Confirm]
   [<CommonParameters>]
```
Per ulteriori informazioni sulla creazione di un'istanza della cache in PowerShell, vedere il [riferimento Azure PowerShell](https://docs.microsoft.com/powershell/module/az.rediscache/new-azrediscache?view=azps-5.2.0). 

## <a name="create-a-message-endpoint"></a>Creare un endpoint del messaggio

Prima di sottoscrivere l'argomento, creare l'endpoint per il messaggio dell'evento. L'endpoint richiede in genere azioni basate sui dati degli eventi. Per semplificare questa guida introduttiva, si distribuisce un'[app Web preesistente](https://github.com/Azure-Samples/azure-event-grid-viewer) che visualizza i messaggi di evento. La soluzione distribuita include un piano di servizio app, un'app Web del servizio app e codice sorgente da GitHub.

Sostituire `<your-site-name>` con un nome univoco per l'app Web. Il nome dell'app Web deve essere univoco perché fa parte della voce DNS.

```powershell
$sitename="<your-site-name>"

New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure-Samples/azure-event-grid-viewer/master/azuredeploy.json" `
  -siteName $sitename `
  -hostingPlanName viewerhost
```

Per il completamento della distribuzione possono essere necessari alcuni minuti. Dopo il completamento della distribuzione, visualizzare l'app Web per assicurarsi che sia in esecuzione. In un Web browser passare a: `https://<your-site-name>.azurewebsites.net`

Il sito dovrebbe essere visibile senza messaggi attualmente visualizzati.

:::image type="content" source="media/cache-event-grid-portal/blank-event-grid-viewer.png" alt-text="Sito del Visualizzatore di griglia di eventi vuoto.":::

## <a name="subscribe-to-your-azure-cache-for-redis-event"></a>Sottoscrivere l'evento cache di Azure per Redis

In questo passaggio si sottoscrive un argomento per indicare a griglia di eventi gli eventi di cui si vuole tenere traccia. L'esempio seguente sottoscrive l'istanza della cache creata e passa l'URL dall'app Web come endpoint per la notifica degli eventi. L'endpoint per l'app Web deve includere il suffisso `/api/updates/`.

```powershell
$cacheId = (Get-AzRedisCache -ResourceGroupName $resourceGroup -Name $cacheName).Id
$endpoint="https://$sitename.azurewebsites.net/api/updates"

New-AzEventGridSubscription `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint $endpoint `
  -ResourceId $cacheId
```

Visualizzare nuovamente l'app Web e notare che all'app è stato inviato un evento di convalida della sottoscrizione. Selezionare l'icona a forma di occhio per espandere i dati dell'evento. Griglia di eventi invia l'evento di convalida in modo che l'endpoint possa verificare che voglia ricevere i dati dell'evento. L'app Web include il codice per convalidare la sottoscrizione.

  :::image type="content" source="media/cache-event-grid-portal/subscription-event.png" alt-text="Visualizzatore griglia di eventi di Azure.":::

## <a name="trigger-an-event-from-azure-cache-for-redis"></a>Attivare un evento dalla cache di Azure per Redis

A questo punto, attivare un evento per vedere come la griglia di eventi distribuisce il messaggio nell'endpoint.

```powershell
Import-AzRedisCache
      [-ResourceGroupName <String>]
      -Name <String>
      -Files <String[]>
      [-Format <String>]
      [-Force]
      [-PassThru]
      [-DefaultProfile <IAzureContextContainer>]
      [-WhatIf]
      [-Confirm]
      [<CommonParameters>]
```
Per ulteriori informazioni sull'importazione in PowerShell, vedere il [riferimento Azure PowerShell](https://docs.microsoft.com/powershell/module/az.rediscache/import-azrediscache?view=azps-5.2.0). 

È stato attivato l'evento e Griglia di eventi ha inviato il messaggio all'endpoint configurato al momento della sottoscrizione. Visualizzare l'app Web per vedere l'evento appena inviato.

```json
[{
"id": "e1ceb52d-575c-4ce4-8056-115dec723cff",
  "eventType": "Microsoft.Cache.ImportRDBCompleted",
  "topic": "/subscriptions/{subscription_id}/resourceGroups/{resource_group_name}/providers/Microsoft.Cache/Redis/{cache_name}",
  "data": {
    "name": "ImportRDBCompleted",
    "timestamp": "2020-12-10T18:07:54.4937063+00:00",
    "status": "Succeeded"
  },
  "subject": "ImportRDBCompleted",
  "dataversion": "1.0",
  "metadataVersion": "1",
  "eventTime": "2020-12-10T18:07:54.4937063+00:00"
}]

```

## <a name="clean-up-resources"></a>Pulire le risorse
Se si prevede di continuare a usare questa cache di Azure per l'istanza di redis e la sottoscrizione di eventi, non eliminare le risorse create in questa Guida introduttiva. Se non si prevede di continuare, usare il comando seguente per eliminare le risorse create in questa Guida introduttiva.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Passaggi successivi

Ora che si è appreso come creare argomenti e sottoscrizioni di eventi, altre informazioni sugli eventi di cache di Azure per Redis e sulla griglia di eventi possono essere utili:

- [Reazione alla cache di Azure per gli eventi Redis](cache-event-grid.md)
- [Informazioni sulla griglia di eventi](../event-grid/overview.md)