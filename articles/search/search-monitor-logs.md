---
title: Raccogli dati di log
titleSuffix: Azure Cognitive Search
description: Raccogliere e analizzare i dati di log abilitando un'impostazione di diagnostica e quindi usare il linguaggio di query kusto per esplorare i risultati.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: e6fcf5980cf64b5fc088dfa295ef6221ffda6de9
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499935"
---
# <a name="collect-and-analyze-log-data-for-azure-cognitive-search"></a>Raccogliere e analizzare i dati di log per Azure ricerca cognitiva

I log di diagnostica o operativi forniscono informazioni approfondite sulle operazioni dettagliate di Azure ricerca cognitiva e sono utili per il monitoraggio dei processi del servizio e del carico di lavoro. Internamente, alcune informazioni sul sistema esistono nel back-end per un breve periodo di tempo, sufficiente per l'analisi e l'analisi se si segnala un ticket di supporto. Tuttavia, se si desidera la direzione automatica sui dati operativi, è necessario configurare un'impostazione di diagnostica per specificare la posizione in cui vengono raccolte le informazioni di registrazione.

La registrazione diagnostica viene abilitata tramite l'integrazione con [monitoraggio di Azure](../azure-monitor/index.yml). 

Quando si configura la registrazione diagnostica, verrà richiesto di specificare un meccanismo di archiviazione. Nella tabella seguente sono elencate le opzioni per la raccolta e la conservazione dei dati.

| Risorsa | Utilizzo |
|----------|----------|
| [Inviare all'area di lavoro Log Analytics](../azure-monitor/learn/tutorial-resource-logs.md) | Gli eventi e le metriche vengono inviati a un'area di lavoro Log Analytics, su cui è possibile eseguire query nel portale per restituire informazioni dettagliate. Per informazioni introduttive, vedere Introduzione [ai log di monitoraggio di Azure](../azure-monitor/log-query/log-analytics-tutorial.md) |
| [Archivia con archiviazione BLOB](../storage/blobs/storage-blobs-overview.md) | Gli eventi e le metriche vengono archiviati in un contenitore BLOB e archiviati in file JSON. I log possono essere abbastanza granulari (in base all'ora/minuto), utili per la ricerca di un evento imprevisto specifico, ma non per l'analisi aperta. Usare un editor JSON per visualizzare un file di log non elaborato o Power BI per aggregare e visualizzare i dati di log.|
| [Flusso a hub eventi](../event-hubs/index.yml) | Gli eventi e le metriche vengono trasmessi a un servizio Hub eventi di Azure. Scegliere questa opzione come servizio di raccolta dati alternativo per i log di dimensioni molto grandi. |

## <a name="prerequisites"></a>Prerequisiti

Creare le risorse in anticipo in modo che sia possibile selezionarne una o più durante la configurazione della registrazione diagnostica.

+ [Creare un'area di lavoro di log Analytics](../azure-monitor/learn/quick-create-workspace.md)

+ [Creare un account di archiviazione](../storage/common/storage-account-create.md)

+ [Creare un hub eventi](../event-hubs/event-hubs-create.md)

## <a name="enable-data-collection"></a>Abilitare la raccolta dati

Le impostazioni di diagnostica specificano come vengono raccolti gli eventi e le metriche registrate.

1. Selezionare **Impostazioni di diagnostica** in **Monitoraggio**.

   ![Impostazioni di diagnostica](./media/search-monitor-usage/diagnostic-settings.png "Impostazioni di diagnostica")

1. Selezionare **+ Aggiungi impostazione di diagnostica**

1. Controllare **log Analytics**, selezionare l'area di lavoro e selezionare **OperationLogs** e **AllMetrics**.

   ![Configurare la raccolta dei dati](./media/search-monitor-usage/configure-storage.png "Configurare la raccolta dei dati")

1. Salvare l'impostazione.

1. Dopo aver abilitato la registrazione, usare il servizio di ricerca per iniziare a generare log e metriche. Prima che le metriche e gli eventi registrati diventino disponibili, l'operazione verrà eseguita.

Per Log Analytics, saranno necessari alcuni minuti prima che i dati siano disponibili, dopo i quali è possibile eseguire query kusto per restituire i dati. Per altre informazioni, vedere [monitorare le richieste di query](search-monitor-logs.md).

Per l'archiviazione BLOB, è necessaria un'ora prima che i contenitori vengano visualizzati nell'archivio BLOB. È presente un BLOB ogni ora per ciascun contenitore. I contenitori vengono creati solo quando è presente un'attività da registrare o misurare. I dati che vengono copiati in un account di archiviazione vengono formattati come JSON e inseriti in due contenitori:

+ insights-logs-operationlogs: per i log relativi al traffico ricerca
+ insights-metrics-pt1m: per le metriche

## <a name="query-log-information"></a>Informazioni sul log di query

Due tabelle contengono log e metriche per ricerca cognitiva di Azure: **AzureDiagnostics** e **AzureMetrics**.

1. In **monitoraggio** selezionare **log**.

1. Immettere **AzureMetrics** nella finestra query. Eseguire questa semplice query per acquisire familiarità con i dati raccolti in questa tabella. Scorrere la tabella per visualizzare le metriche e i valori. Si noti il numero di record nella parte superiore e se il servizio raccoglie le metriche per un certo periodo di tempo, potrebbe essere necessario modificare l'intervallo di tempo per ottenere un set di dati gestibile.

   ![Tabella AzureMetrics](./media/search-monitor-usage/azuremetrics-table.png "Tabella AzureMetrics")

1. Immettere la query seguente per restituire un set di risultati tabulare.

   ```
   AzureMetrics
    | project MetricName, Total, Count, Maximum, Minimum, Average
   ```

1. Ripetere i passaggi precedenti, a partire da **AzureDiagnostics** per restituire tutte le colonne a scopo informativo, seguito da una query più selettiva che estrae informazioni più interessanti.

   ```
   AzureDiagnostics
   | project OperationName, resultSignature_d, DurationMs, Query_s, Documents_d, IndexName_s
   | where OperationName == "Query.Search" 
   ```

   ![Tabella AzureDiagnostics](./media/search-monitor-usage/azurediagnostics-table.png "Tabella AzureDiagnostics")

## <a name="kusto-query-examples"></a>Esempi di query kusto

Se è stata abilitata la registrazione diagnostica, è possibile eseguire una query su **AzureDiagnostics** per un elenco di operazioni eseguite nel servizio e quando. È anche possibile correlare l'attività per esaminare le modifiche delle prestazioni.

#### <a name="example-list-operations"></a>Esempio: operazioni di elenco 

Restituisce un elenco di operazioni e un conteggio di ciascuna di esse.

```
AzureDiagnostics
| summarize count() by OperationName
```

#### <a name="example-correlate-operations"></a>Esempio: correlare le operazioni

Correlare le richieste di query con le operazioni di indicizzazione ed eseguire il rendering dei punti dati in un grafico temporale per vedere le operazioni in coincidenza.

```
AzureDiagnostics
| summarize OperationName, Count=count()
| where OperationName in ('Query.Search', 'Indexing.Index')
| summarize Count=count(), AvgLatency=avg(DurationMs) by bin(TimeGenerated, 1h), OperationName
| render timechart
```

## <a name="logged-operations"></a>Operazioni registrate

Gli eventi registrati acquisiti da monitoraggio di Azure includono quelli correlati all'indicizzazione e alle query. La tabella **AzureDiagnostics** in log Analytics raccoglie dati operativi correlati a query e indicizzazione.

| OperationName | Descrizione |
|---------------|-------------|
| ServiceStats | Questa operazione è una chiamata di routine per [ottenere le statistiche del servizio](/rest/api/searchservice/get-service-statistics), chiamate direttamente o in modo implicito per popolare una pagina di panoramica del portale quando viene caricata o aggiornata. |
| Query. search |  Richieste di query su un indice vedere [monitorare](search-monitor-queries.md) le query per ottenere informazioni sulle query registrate.|
| Indexing. index  | Questa operazione è una chiamata per [aggiungere, aggiornare o eliminare documenti](/rest/api/searchservice/addupdate-or-delete-documents). |
| indici. Prototipo | Si tratta di un indice creato dalla procedura guidata Importa dati. |
| Indicizzatori. crea | Creare un indicizzatore in modo esplicito o implicito tramite la procedura guidata Importa dati. |
| Indicizzatori. Get | Restituisce il nome di un indicizzatore ogni volta che viene eseguito l'indicizzatore. |
| Indicizzatori. stato | Restituisce lo stato di un indicizzatore ogni volta che viene eseguito l'indicizzatore. |
| Origini dati. Get | Restituisce il nome dell'origine dati ogni volta che viene eseguito un indicizzatore.|
| Indexes. Get | Restituisce il nome di un indice ogni volta che viene eseguito un indicizzatore. |

## <a name="log-schema"></a>Schema del log

Se si compilano report personalizzati, le strutture di dati che contengono i dati di log di Azure ricerca cognitiva sono conformi allo schema riportato di seguito. Per l'archiviazione BLOB, ogni BLOB ha un oggetto radice denominato **record** che contiene una matrice di oggetti log. Ogni BLOB contiene record su tutte le operazioni eseguite nell'arco della stessa ora.

La tabella seguente è un elenco parziale dei campi comuni alla registrazione delle risorse.

| Nome | Type | Esempio | Note |
| --- | --- | --- | --- |
| timeGenerated |Datetime |"2018-12-07T00:00:43.6872559Z" |Timestamp dell'operazione |
| resourceId |string |"/SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111/<br/>RESOURCEGROUPS/DEFAULT/PROVIDERS/<br/>  MICROSOFT.SEARCH/SEARCHSERVICES/SEARCHSERVICE" |resourceId in uso |
| operationName |string |"Query.Search" |Nome dell'operazione |
| operationVersion |string |"2020-06-30" |api-version usata |
| category |string |"OperationLogs" |constant |
| resultType |string |"Esito positivo" |Valori possibili: esito positivo o negativo |
| resultSignature |INT |200 |Codice risultato HTTP |
| durationMS |INT |50 |Durata dell'operazione in millisecondi |
| properties |object |Vedere la tabella seguente |Oggetto contenente dati specifici dell'operazione |

### <a name="properties-schema"></a>Schema delle proprietà

Le proprietà seguenti sono specifiche per ricerca cognitiva di Azure.

| Nome | Type | Esempio | Note |
| --- | --- | --- | --- |
| Description_s |string |"GET /indexes('content')/docs" |Endpoint dell'operazione |
| Documents_d |INT |42 |Numero di documenti elaborati |
| IndexName_s |string |"test-index" |Nome dell'indice associato all'operazione |
| Query_s |string |"? Search = AzureSearch&$count = true&API-Version = 2020-06-30" |Parametri della query |

## <a name="metrics-schema"></a>Schema delle metriche

Le metriche vengono acquisite per le richieste di query e misurate in intervalli di un minuto. Ogni metrica espone i valori minimi, massimi e medi al minuto. Per altre informazioni, vedere [monitorare le richieste di query](search-monitor-queries.md).

| Nome | Type | Esempio | Note |
| --- | --- | --- | --- |
| resourceId |string |"/SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111/<br/>RESOURCEGROUPS/DEFAULT/PROVIDERS/<br/> MICROSOFT.SEARCH/SEARCHSERVICES/SEARCHSERVICE" |ID risorsa |
| metricName |string |"Latenza" |Nome della metrica |
| time |Datetime |"2018-12-07T00:00:43.6872559Z" |Timestamp dell'operazione |
| average |INT |64 |Valore medio degli esempi non elaborati nell'intervallo di tempo della metrica, unità in secondi o percentuale, a seconda della metrica. |
| minimum |INT |37 |Valore minimo degli esempi non elaborati nell'intervallo di tempo della metrica, unità in secondi. |
| maximum |INT |78 |Valore massimo degli esempi non elaborati nell'intervallo di tempo della metrica, unità in secondi.  |
| total |INT |258 |Il valore totale degli esempi non elaborati nell'intervallo di tempo della metrica, unità in secondi.  |
| count |INT |4 |Numero di metriche emesse da un nodo al log entro l'intervallo di un minuto.  |
| timegrain |string |"PT1M" |Granularità temporale della metrica in ISO 8601. |

In genere, le query vengono eseguite in millisecondi, quindi solo le query che vengono misurate come secondi verranno visualizzate in metriche come query al secondo.

Per la metrica **query di ricerca al secondo** , il valore minimo è il valore minimo per le query di ricerca al secondo registrate durante tale minuto. Lo stesso vale anche per il valore massimo. Il valore medio è l'aggregato nel corso dell'intero minuto. Ad esempio, all'interno di un minuto è possibile che si disponga di un modello simile al seguente: un secondo di carico elevato, che rappresenta il valore massimo per SearchQueriesPerSecond, seguito da 58 secondi di carico medio e infine da un secondo con una sola query, che corrisponde al valore minimo.

Per le **query di ricerca limitate, percentuale**, minimo, massimo, medio e totale, hanno lo stesso valore: la percentuale di query di ricerca che sono state limitate, dal numero totale di query di ricerca in un minuto.

## <a name="view-raw-log-files"></a>Visualizza file di log non elaborati

L'archiviazione BLOB viene utilizzata per l'archiviazione dei file di log. È possibile usare qualsiasi editor JSON per visualizzare il file di log. Se non ne è già disponibile uno, è consigliabile usare [Visual Studio Code](https://code.visualstudio.com/download).

1. Nel portale di Azure aprire l'account di archiviazione. 

2. Nel riquadro di spostamento a sinistra fare clic su **BLOB**. Dovrebbero essere disponibili i contenitori **insights-logs-operationlogs** e **insights-metrics-pt1m**. Questi contenitori vengono creati da Azure ricerca cognitiva quando i dati di log vengono esportati nell'archivio BLOB.

3. Scorrere la gerarchia di cartelle verso il basso fino a raggiungere il file con estensione JSON.  Usare il menu di scelta rapida per scaricare il file.

Dopo aver scaricato il file, aprirlo in un editor JSON per visualizzare il contenuto.

## <a name="next-steps"></a>Passaggi successivi

Se non è già stato fatto, esaminare le nozioni di base del monitoraggio del servizio di ricerca per informazioni sull'intera gamma di funzionalità di supervisione.

> [!div class="nextstepaction"]
> [Monitorare operazioni e attività in Azure ricerca cognitiva](search-monitor-usage.md)