---
title: Origini eventi di inserimento di flussi-Azure Time Series Insights Gen2 | Microsoft Docs
description: Informazioni sul flusso di dati in Azure Time Series Insights Gen2.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 01/19/2021
ms.openlocfilehash: ae07f51a91745acdaf2601d3a50bf282129dac71
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98881806"
---
# <a name="azure-time-series-insights-gen2-event-sources"></a>Origini eventi Azure Time Series Insights Gen2

 L'ambiente di Azure Time Series Insights Gen2 può avere fino a due origini di eventi di streaming. Come input sono supportati due tipi di risorse di Azure:

- [Hub IoT Azure](../iot-hub/about-iot-hub.md)
- [Hub eventi di Azure](../event-hubs/event-hubs-about.md)

Gli eventi devono essere inviati come JSON con codifica UTF-8.

## <a name="create-or-edit-event-sources"></a>Crea o modifica origini eventi

Le risorse di origine evento possono risiedere nella stessa sottoscrizione di Azure dell'ambiente Azure Time Series Insights Gen2 o in una sottoscrizione diversa. È possibile usare la [portale di Azure](./tutorials-set-up-tsi-environment.md#create-an-azure-time-series-insights-gen2-environment), l' [interfaccia](https://github.com/Azure/azure-cli-extensions/tree/master/src/timeseriesinsights)della riga di comando di Azure, i [modelli ARM](time-series-insights-manage-resources-using-azure-resource-manager-template.md)e l' [API REST](/rest/api/time-series-insights/management(gen1/gen2)/eventsources) per creare, modificare o rimuovere le origini eventi dell'ambiente.

Quando si connette un'origine evento, l'ambiente Azure Time Series Insights Gen2 leggerà tutti gli eventi attualmente archiviati nell'hub eventi o in Internet, a partire dall'evento meno recente.

> [!IMPORTANT]
>
> - È possibile che si verifichi una latenza iniziale elevata quando si connette un'origine evento all'ambiente di Azure Time Series Insights Gen2.
> - La latenza dell'origine evento dipende dal numero di eventi attualmente presenti nell'hub IoT o nell'hub eventi.
> - La latenza elevata si ridurrà dopo l'inserimento iniziale dei dati di origine dell'evento. Inviare un ticket di supporto con il portale di Azure se si verifica una latenza elevata.

## <a name="streaming-ingestion-best-practices"></a>Procedure consigliate per l'inserimento di flussi

- Creare sempre un gruppo di consumer univoco per l'ambiente Azure Time Series Insights Gen2 per utilizzare i dati dell'origine evento. Il riutilizzo dei gruppi di consumer può causare disconnessioni casuali e potrebbe causare la perdita di dati.

- Configurare l'ambiente di Azure Time Series Insights Gen2 e l'hub Internet e/o gli hub eventi nella stessa area di Azure. Sebbene sia possibile configurare un'origine evento in un'area separata, questo scenario non è supportato e non è possibile garantire la disponibilità elevata.

- Non superare il [limite di velocità effettiva](./concepts-streaming-ingress-throughput-limits.md) dell'ambiente o il limite per partizione.

- Configurare un [avviso](./time-series-insights-environment-mitigate-latency.md#monitor-latency-and-throttling-with-alerts) di ritardo per ricevere una notifica nel caso in cui si verifichino problemi di elaborazione dei dati nell'ambiente. Vedere [carichi di lavoro di produzione](./concepts-streaming-ingestion-event-sources.md#production-workloads) di seguito per le condizioni di avviso suggerite.

- Usare l'inserimento streaming solo per i dati near real-time e recenti. I dati cronologici di streaming non sono supportati.

- Comprendere in che modo le proprietà verranno sottoposte a escape e [i dati JSON appiattiti e archiviati.](./concepts-json-flattening-escaping-rules.md)

- Seguire il principio dei privilegi minimi quando si forniscono stringhe di connessione all'origine evento. Per hub eventi, configurare un criterio di accesso condiviso con l'attestazione di *invio* e per l'hub Internet usare solo l'autorizzazione per la *connessione al servizio* .

## <a name="production-workloads"></a>Carichi di lavoro di produzione

Oltre alle procedure consigliate descritte in precedenza, è consigliabile implementare quanto segue per i carichi di lavoro aziendali critici.

- Aumentare il periodo di conservazione dei dati dell'hub eventi o dell'hub eventi fino a un massimo di 7 giorni.

- Creare avvisi di ambiente nell'portale di Azure. Gli avvisi basati sulle [metriche](./how-to-monitor-tsi-reference.md#metrics) della piattaforma consentono di convalidare il comportamento della pipeline end-to-end. Le istruzioni per la creazione e la gestione degli avvisi sono disponibili [qui](./time-series-insights-environment-mitigate-latency.md#monitor-latency-and-throttling-with-alerts). Condizioni di avviso consigliate:

  - IngressReceivedMessagesTimeLag è maggiore di 5 minuti
  - IngressReceivedBytes è 0
- Mantieni il bilanciamento del carico di inserimento tra le partizioni dell'hub eventi o dell'hub eventi.

### <a name="historical-data-ingestion"></a>Inserimento dei dati cronologici

L'uso della pipeline di streaming per importare i dati cronologici non è attualmente supportato in Azure Time Series Insights Gen2. Se è necessario importare i dati passati nell'ambiente in uso, seguire le linee guida indicate sotto:

- Non trasmettere dati in tempo reale e cronologici in parallelo. L'inserimento di dati non in ordine comporterà un calo delle prestazioni delle query.
- Inserire i dati cronologici rispettando la sequenza temporale per ottenere prestazioni ottimali.
- Rispettare i limiti di velocità effettiva di inserimento indicati sotto.
- Disabilitare l'archivio ad accesso frequente se i dati sono precedenti al periodo di conservazione dell'archivio ad accesso frequente.

## <a name="event-source-timestamp"></a>Timestamp origine evento

Quando si configura un'origine evento, verrà richiesto di specificare una proprietà ID timestamp. La proprietà timestamp viene utilizzata per tenere traccia degli eventi nel corso del tempo, ovvero il tempo che verrà utilizzato come $event. $ts nelle [API di query](/rest/api/time-series-insights/dataaccessgen2/query/execute) e per il tracciato delle serie in Azure Time Series Insights Explorer. Se non viene fornita alcuna proprietà al momento della creazione o se la proprietà timestamp non è presente in un evento, il tempo di accodamento dell'hub eventi o degli hub eventi verrà usato come valore predefinito. I valori delle proprietà timestamp vengono archiviati in formato UTC.

In generale, gli utenti scelgono di personalizzare la proprietà timestamp e di usare l'ora in cui il sensore o il tag ha generato la lettura, anziché usare l'ora di accodamento dell'Hub predefinito. Questa operazione è particolarmente necessaria quando i dispositivi hanno una perdita di connettività intermittente e un batch di messaggi ritardati viene inviato a Azure Time Series Insights Gen2.

Se il timestamp personalizzato si trova all'interno di un oggetto JSON annidato o di una matrice, è necessario fornire il nome di proprietà corretto seguendo le [convenzioni di denominazione flat e di escape](concepts-json-flattening-escaping-rules.md). Ad esempio, il timestamp dell'origine evento per il payload JSON riportato [qui](concepts-json-flattening-escaping-rules.md#example-a) dovrebbe essere specificato come `"values.time"` .

### <a name="time-zone-offsets"></a>Offset di fusi orari

I timestamp devono essere inviati in formato ISO 8601 e verranno archiviati in formato UTC. Se viene specificata una differenza di fuso orario, l'offset verrà applicato e quindi l'ora archiviata e restituita in formato UTC. Se l'offset non è formattato correttamente, verrà ignorato. Nelle situazioni in cui la soluzione potrebbe non avere il contesto dell'offset originale, è possibile inviare i dati di offset in una proprietà di evento distinta aggiuntiva per assicurarsi che venga mantenuta e che l'applicazione possa fare riferimento a una risposta di query.

La differenza di fuso orario deve essere formattata in uno dei seguenti elementi:

HHMMZ ±</br>
± HH: MM</br>
± HH: MMZ</br>

## <a name="next-steps"></a>Passaggi successivi

- Leggere le [regole di escape e Flat JSON](./concepts-json-flattening-escaping-rules.md) per comprendere come verranno archiviati gli eventi.

- Informazioni sulle limitazioni della [velocità effettiva](./concepts-streaming-ingress-throughput-limits.md) dell'ambiente
