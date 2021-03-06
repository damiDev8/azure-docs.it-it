---
title: Informazioni sulla migrazione per gli avvisi di monitoraggio di Azure
description: Informazioni sul funzionamento della migrazione degli avvisi e sulla risoluzione dei problemi.
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: yalavi
author: yalavi
ms.subservice: alerts
ms.openlocfilehash: e57b3dd31455db245103469874c517fe54479110
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99526908"
---
# <a name="understand-migration-options-to-newer-alerts"></a>Informazioni sulle opzioni di migrazione per gli avvisi più recenti

Gli avvisi classici sono [ritirati](./monitoring-classic-retirement.md) per gli utenti del cloud pubblico, anche se sono in uso limitato per le risorse che non supportano ancora i nuovi avvisi. Una nuova data verrà annunciata a breve per la migrazione degli avvisi rimanenti, il [cloud di Azure per enti pubblici](../../azure-government/documentation-government-welcome.md)e [Azure Cina 21ViaNet](https://docs.azure.cn/).

Questo articolo illustra come funziona lo strumento migrazione manuale e migrazione volontaria, che verrà usato per eseguire la migrazione delle regole di avviso rimanenti. Vengono inoltre descritte le soluzioni per alcuni problemi comuni.

> [!IMPORTANT]
> Gli avvisi del log attività (inclusi gli avvisi di integrità del servizio) e gli avvisi del log non sono interessati dalla migrazione. La migrazione si applica solo alle regole di avviso classiche descritte di [seguito](monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform).

> [!NOTE]
> Se le regole di avviso classiche non sono valide, vale a dire che si trovano su metriche o risorse [deprecate](#classic-alert-rules-on-deprecated-metrics) che sono state eliminate, non verranno migrate e non saranno disponibili dopo il ritiro del servizio.

## <a name="manually-migrating-classic-alerts-to-newer-alerts"></a>Migrazione manuale di avvisi classici a avvisi più recenti

I clienti interessati alla migrazione manuale dei rimanenti avvisi possono già farlo usando le sezioni seguenti. Queste sezioni definiscono anche le metriche che vengono ritirate dal provider di risorse e attualmente non possono essere migrate direttamente.

### <a name="guest-metrics-on-virtual-machines"></a>Metriche Guest nelle macchine virtuali

Prima di poter creare nuovi avvisi sulle metriche Guest, è necessario inviare le metriche Guest all'archivio delle metriche personalizzate di monitoraggio di Azure. Seguire queste istruzioni per abilitare il sink di monitoraggio di Azure nelle impostazioni di diagnostica:

- [Abilitazione delle metriche Guest per macchine virtuali Windows](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Abilitazione delle metriche Guest per macchine virtuali Linux](collect-custom-metrics-linux-telegraf.md)

Al termine di questa procedura, è possibile creare nuovi avvisi delle metriche per le metriche Guest. Dopo aver creato nuovi avvisi per le metriche, è possibile eliminare gli avvisi classici.

### <a name="storage-account-metrics"></a>Metriche relative all'account di archiviazione

È possibile eseguire la migrazione di tutti gli avvisi classici sugli account di archiviazione, eccetto gli avvisi relativi a queste metriche:

- PercentAuthorizationError
- PercentClientOtherError
- PercentNetworkError
- PercentServerOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- AnonymousThrottlingError
- SASThrottlingError
- ThrottlingError

È necessario eseguire la migrazione delle regole di avviso classiche sulla percentuale di metriche in base [al mapping tra la metrica di archiviazione precedente e quella nuova](../../storage/common/storage-metrics-migration.md#metrics-mapping-between-old-metrics-and-new-metrics). Le soglie dovranno essere modificate in modo appropriato perché la nuova metrica disponibile è assoluta.

Le regole di avviso classiche in AnonymousThrottlingError, SASThrottlingError e ThrottlingError devono essere suddivise in due nuovi avvisi perché non esiste alcuna metrica combinata che fornisce la stessa funzionalità. Le soglie dovranno essere adattate in modo appropriato.

### <a name="cosmos-db-metrics"></a>Metriche di Cosmos DB

È possibile eseguire la migrazione di tutti gli avvisi classici sulle metriche di Cosmos DB, eccetto gli avvisi relativi a queste metriche:

- Media richieste al secondo
- Livello di coerenza
- Http 2xx
- Http 3xx
- Http 400
- Http 401
- Internal Server Error
- Numero massimo di RUPM utilizzati al minuto
- Numero massimo di ur al secondo
- Numero di richieste non riuscite di Mongo
- Richieste di eliminazione Mongo non riuscite
- Richieste di inserimento non riuscite di Mongo
- Mongo altre richieste non riuscite
- Addebito per altre richieste di Mongo
- Frequenza di richieste altre
- Richieste di query Mongo non riuscite
- Richieste di aggiornamento Mongo non riuscite
- Latenza di lettura osservata
- Latenza di scrittura osservata
- Disponibilità del servizio
- Capacità di archiviazione
- Richieste limitate
- Totale richieste

Media di richieste al secondo, livello di coerenza, numero massimo di RUPM consumate al minuto, numero massimo di ur al secondo, latenza di lettura osservata, latenza di scrittura osservata, capacità di archiviazione attualmente non disponibile nel [nuovo sistema](metrics-supported.md#microsoftdocumentdbdatabaseaccounts).

Gli avvisi sulle metriche delle richieste come HTTP 2xx, HTTP 3xx, http 400, HTTP 401, errore interno del server, disponibilità del servizio, richieste limitate e richieste totali non vengono migrati perché la modalità di conteggio delle richieste è diversa tra le metriche classiche e le nuove metriche. Gli avvisi su questi dovranno essere ricreati manualmente con le soglie regolate.

Gli avvisi per le metriche delle richieste non riuscite di Mongo devono essere suddivisi in più avvisi perché non esiste una metrica combinata che fornisce la stessa funzionalità. Le soglie dovranno essere adattate in modo appropriato.

### <a name="classic-compute-metrics"></a>Metriche di calcolo classiche

Tutti gli avvisi sulle metriche di calcolo classiche non verranno migrati usando lo strumento di migrazione perché le risorse di calcolo classiche non sono ancora supportate con i nuovi avvisi. Il supporto per i nuovi avvisi per questi tipi di risorse è attualmente disponibile in anteprima pubblica e i clienti possono ricreare nuove regole di avviso equivalenti in base alle regole di avviso classiche.

### <a name="classic-alert-rules-on-deprecated-metrics"></a>Regole di avviso classiche su metriche deprecate

Si tratta di regole di avviso classiche sulle metriche che in precedenza erano supportate ma che erano deprecate. Una piccola percentuale di clienti potrebbe avere regole di avviso classiche non valide per tali metriche. Poiché queste regole di avviso non sono valide, non verranno migrate.

| Tipo di risorsa| Metriche deprecate |
|-------------|----------------- |
| Microsoft.DBforMySQL/servers | compute_consumption_percent, compute_limit |
| Microsoft.DBforPostgreSQL/servers | compute_consumption_percent, compute_limit |
| Microsoft.Network/publicIPAddresses | defaultddostriggerrate |
| Microsoft.SQL/servers/databases | service_level_objective, storage_limit, storage_used, limitazione, dtu_consumption_percent, storage_used |
| Microsoft. Web/hostingEnvironments/multirolepools | averagememoryworkingset |
| Microsoft. Web/hostingEnvironments/dei pool | BytesReceived, httpqueuelength |

## <a name="how-equivalent-new-alert-rules-and-action-groups-are-created"></a>Modalità di creazione di nuovi gruppi di azione e regole di avviso equivalenti

Lo strumento di migrazione converte le regole di avviso classiche in nuovi gruppi di azione e regole di avviso equivalenti. Per la maggior parte delle regole di avviso classiche, le nuove regole di avviso equivalenti sono sulla stessa metrica con le stesse proprietà, ad esempio `windowSize` e `aggregationType` . Tuttavia, alcune regole di avviso classiche sono relative alle metriche con una metrica equivalente diversa nel nuovo sistema. I principi seguenti si applicano alla migrazione di avvisi classici, a meno che non sia specificato nella sezione seguente:

- **Frequency**: definisce la frequenza con cui una regola di avviso classica o nuova controlla la condizione. Le `frequency` regole di avviso classiche non possono essere configurate dall'utente ed è sempre 5 minuti per tutti i tipi di risorse tranne Application Insights componenti per i quali è stato di 1 min. La frequenza delle regole equivalenti viene anche impostata rispettivamente su 5 min e 1 min.
- **Tipo di aggregazione**: definisce la modalità di aggregazione della metrica sulla finestra di interesse. `aggregationType`È anche lo stesso tra gli avvisi classici e i nuovi avvisi per la maggior parte delle metriche. In alcuni casi, poiché la metrica è diversa tra gli avvisi classici e i nuovi avvisi, viene usato il valore equivalente `aggregationType` o quello `primary Aggregation Type` definito per la metrica.
- **Unità**: proprietà della metrica in cui viene creato l'avviso. Alcune metriche equivalenti hanno unità diverse. La soglia viene modificata in modo appropriato in base alle esigenze. Se, ad esempio, la metrica originale presenta secondi come unità, ma la nuova metrica equivalente ha milliSecondi come unità, la soglia originale viene moltiplicata per 1000 per garantire lo stesso comportamento.
- **Dimensioni finestra**: definisce la finestra su cui vengono aggregati i dati delle metriche per il confronto con la soglia. Per `windowSize` i valori standard come 5 minuti, 15mins, 30mins, 1 ora, 3 ore, 6 ore, 12 ore, 1 giorno, non sono state apportate modifiche per la nuova regola di avviso equivalente. Per gli altri valori, viene scelto il più vicino `windowSize` da usare. Per la maggior parte dei clienti, questa modifica non ha alcun effetto. Per una piccola percentuale di clienti, potrebbe essere necessario modificare la soglia per ottenere lo stesso comportamento.

Nelle sezioni seguenti vengono illustrate in dettaglio le metriche con una metrica equivalente diversa nel nuovo sistema. Tutte le metriche rimanenti per le regole di avviso classiche e nuove non sono elencate. È possibile trovare un elenco delle metriche supportate nel nuovo sistema [qui](metrics-supported.md).

### <a name="microsoftstorageaccountsservices"></a>Microsoft. StorageAccounts/Services

Per i servizi dell'account di archiviazione come BLOB, tabelle, file e code, le metriche seguenti sono mappate alle metriche equivalenti, come illustrato di seguito:

| Metrica negli avvisi classici | Metrica equivalente nei nuovi avvisi | Commenti|
|--------------------------|---------------------------------|---------|
| AnonymousAuthorizationError| Metrica transazioni con dimensioni "ResponseType" = "AuthorizationError" e "autenticazione" = "anonima"| |
| AnonymousClientOtherError | Metrica transazioni con dimensioni "ResponseType" = "ClientOtherError" e "autenticazione" = "anonima" | |
| AnonymousClientTimeOutError| Metrica transazioni con dimensioni "ResponseType" = "ClientTimeOutError" e "autenticazione" = "anonima" | |
| AnonymousNetworkError | Metrica transazioni con dimensioni "ResponseType" = "NetworkError" e "autenticazione" = "anonima" | |
| AnonymousServerOtherError | Metrica transazioni con dimensioni "ResponseType" = "ServerOtherError" e "autenticazione" = "anonima" | |
| AnonymousServerTimeOutError | Metrica transazioni con dimensioni "ResponseType" = "ServerTimeOutError" e "autenticazione" = "anonima" | |
| AnonymousSuccess | Metrica transazioni con dimensioni "ResponseType" = "operazione riuscita" e "autenticazione" = "anonima" | |
| AuthorizationError | Metrica transazioni con dimensioni "ResponseType" = "AuthorizationError" | |
| AverageE2ELatency | SuccessE2ELatency | |
| AverageServerLatency | SuccessServerLatency | |
| Capacità | BlobCapacity | Usare `aggregationType` ' Average ' invece di ' Last '. La metrica si applica solo ai servizi BLOB |
| ClientOtherError | Metrica transazioni con dimensioni "ResponseType" = "ClientOtherError"  | |
| ClientTimeoutError | Metrica transazioni con dimensioni "ResponseType" = "ClientTimeOutError" | |
| ContainerCount | ContainerCount | Usare `aggregationType` ' Average ' invece di ' Last '. La metrica si applica solo ai servizi BLOB |
| NetworkError | Metrica transazioni con dimensioni "ResponseType" = "NetworkError" | |
| ObjectCount | BlobCount| Usare `aggregationType` ' Average ' invece di ' Last '. La metrica si applica solo ai servizi BLOB |
| SASAuthorizationError | Metrica delle transazioni con dimensioni "ResponseType" = "AuthorizationError" e "Authentication" = "SAS" | |
| SASClientOtherError | Metrica delle transazioni con dimensioni "ResponseType" = "ClientOtherError" e "Authentication" = "SAS" | |
| SASClientTimeOutError | Metrica delle transazioni con dimensioni "ResponseType" = "ClientTimeOutError" e "Authentication" = "SAS" | |
| SASNetworkError | Metrica delle transazioni con dimensioni "ResponseType" = "NetworkError" e "Authentication" = "SAS" | |
| SASServerOtherError | Metrica delle transazioni con dimensioni "ResponseType" = "ServerOtherError" e "Authentication" = "SAS" | |
| SASServerTimeOutError | Metrica delle transazioni con dimensioni "ResponseType" = "ServerTimeOutError" e "Authentication" = "SAS" | |
| SASSuccess | Metrica Transactions con Dimensions "ResponseType" = "success" e "Authentication" = "SAS" | |
| ServerOtherError | Metrica transazioni con dimensioni "ResponseType" = "ServerOtherError" | |
| ServerTimeOutError | Metrica transazioni con dimensioni "ResponseType" = "ServerTimeOutError"  | |
| Operazione riuscita | Metrica transazioni con dimensioni "ResponseType" = "operazione riuscita" | |
| TotalBillableRequests| Transazioni | |
| TotalEgress | Egress | |
| TotalIngress | Dati in ingresso | |
| TotalRequests | Transazioni | |

### <a name="microsoftinsightscomponents"></a>Microsoft. Insights/Components

Per Application Insights, le metriche equivalenti sono illustrate di seguito:

| Metrica negli avvisi classici | Metrica equivalente nei nuovi avvisi | Commenti|
|--------------------------|---------------------------------|---------|
| Availability. availabilityMetric. Value | availabilityResults/availabilityPercentage|   |
| Availability. durationMetric. Value | availabilityResults/duration| Moltiplica la soglia originale di 1000 come unità per la metrica classica sono in secondi e per una nuova sono in milliSecondi.  |
| basicExceptionBrowser. Count | exceptions/browser|  Usare `aggregationType` "count" invece di "sum". |
| basicExceptionServer. Count | exceptions/server| Usare `aggregationType` "count" invece di "sum".  |
| clientPerformance. clientProcess. Value | browserTimings/processingDuration| Moltiplica la soglia originale di 1000 come unità per la metrica classica sono in secondi e per una nuova sono in milliSecondi.  |
| clientPerformance. networkConnection. Value | browserTimings/networkDuration|  Moltiplica la soglia originale di 1000 come unità per la metrica classica sono in secondi e per una nuova sono in milliSecondi. |
| clientPerformance. receiveRequest. Value | browserTimings/receiveDuration| Moltiplica la soglia originale di 1000 come unità per la metrica classica sono in secondi e per una nuova sono in milliSecondi.  |
| clientPerformance. sendRequest. Value | browserTimings/sendDuration| Moltiplica la soglia originale di 1000 come unità per la metrica classica sono in secondi e per una nuova sono in milliSecondi.  |
| clientPerformance. Total. Value | browserTimings/totalDuration| Moltiplica la soglia originale di 1000 come unità per la metrica classica sono in secondi e per una nuova sono in milliSecondi.  |
| performanceCounter.available_bytes. Value | performanceCounters/memoryAvailableBytes|   |
| performanceCounter.io_data_bytes_per_sec. Value | performanceCounters/processIOBytesPerSecond|   |
| performanceCounter.number_of_exceps_thrown_per_sec. Value | performanceCounters/exceptionsPerSecond|   |
| performanceCounter.percentage_processor_time_normalized. Value | performanceCounters/processCpuPercentage|   |
| performanceCounter.percentage_processor_time. Value | performanceCounters/processCpuPercentage| Il valore soglia deve essere modificato in modo appropriato perché la metrica originale è stata applicata a tutti i core e la nuova metrica viene normalizzata in un core. Lo strumento di migrazione non modifica le soglie.  |
| performanceCounter.percentage_processor_total. Value | performanceCounters/processorCpuPercentage|   |
| performanceCounter.process_private_bytes. Value | performanceCounters/processPrivateBytes|   |
| performanceCounter.request_execution_time. Value | performanceCounters/requestExecutionTime|   |
| performanceCounter.requests_in_application_queue. Value | performanceCounters/requestsInQueue|   |
| performanceCounter.requests_per_sec. Value | performanceCounters/requestsPerSecond|   |
| Request. Duration | requests/duration| Moltiplica la soglia originale di 1000 come unità per la metrica classica sono in secondi e per una nuova sono in milliSecondi.  |
| Request. rate | requests/rate|   |
| requestFailed. Count | requests/failed| Usare `aggregationType` "count" invece di "sum".   |
| Visualizza. Count | pageViews/count| Usare `aggregationType` "count" invece di "sum".   |

### <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

Per Cosmos DB, le metriche equivalenti sono illustrate di seguito:

| Metrica negli avvisi classici | Metrica equivalente nei nuovi avvisi | Commenti|
|--------------------------|---------------------------------|---------|
| AvailableStorage     |AvailableStorage|   |
| Dimensioni dei dati | DataUsage| |
| Conteggio documenti | DocumentCount||
| Dimensioni dell'indice | IndexUsage||
| Addebito richiesta numero Mongo| MongoRequestCharge con Dimension "CommandName" = "count"||
| Frequenza richieste conteggio Mongo | MongoRequestsCount con Dimension "CommandName" = "count"||
| Costo richiesta di eliminazione Mongo | MongoRequestCharge con Dimension "CommandName" = "Delete"||
| Mongo Delete Request Rate (Frequenza richieste di eliminazione Mongo) | MongoRequestsCount con Dimension "CommandName" = "Delete"||
| Costo richiesta di inserimento Mongo | MongoRequestCharge con Dimension "CommandName" = "Insert"||
| Mongo Insert Request Rate (Frequenza richieste di inserimento Mongo) | MongoRequestsCount con Dimension "CommandName" = "Insert"||
| Addebito richieste di query Mongo | MongoRequestCharge con Dimension "CommandName" = "Find"||
| Mongo Query Request Rate (Frequenza richieste di query Mongo) | MongoRequestsCount con Dimension "CommandName" = "Find"||
| Costo richiesta aggiornamento Mongo | MongoRequestCharge con Dimension "CommandName" = "Update"||
| Servizio non disponibile| ServiceAvailability||
| TotalRequestUnits | TotalRequestUnits||

### <a name="how-equivalent-action-groups-are-created"></a>Modalità di creazione di gruppi di azioni equivalenti

Le regole di avviso classiche contengono posta elettronica, webhook, app per la logica e azioni Runbook associate alla regola di avviso. Le nuove regole di avviso usano i gruppi di azioni che possono essere riutilizzati in più regole di avviso. Lo strumento di migrazione crea il gruppo di azioni singolo per le stesse azioni indipendentemente dal numero di regole di avviso che usano l'azione. I gruppi di azioni creati dallo strumento di migrazione usano il formato di denominazione ' Migrated_AG *'.

> [!NOTE]
> Gli avvisi classici hanno inviato messaggi di posta elettronica localizzati in base alle impostazioni locali dell'amministratore classico quando vengono usati per notificare i ruoli di amministratore classico. I nuovi messaggi di avviso vengono inviati tramite gruppi di azioni e sono solo in lingua inglese.

## <a name="rollout-phases"></a>Fasi di implementazione

Lo strumento di migrazione sta implementando fasi per i clienti che usano le regole di avviso classiche. I proprietari delle sottoscrizioni riceveranno un messaggio di posta elettronica quando la sottoscrizione è pronta per la migrazione tramite lo strumento.

> [!NOTE]
> Poiché lo strumento viene implementato in fasi, è possibile notare che alcune sottoscrizioni non sono ancora pronte per essere migrate durante le fasi iniziali.

La maggior parte delle sottoscrizioni è attualmente contrassegnata come pronta per la migrazione. Solo le sottoscrizioni con avvisi classici sui tipi di risorse seguenti non sono ancora pronte per la migrazione.

- Microsoft. classicCompute/domainNames/Slots/Roles
- Microsoft. Insights/Components

## <a name="who-can-trigger-the-migration"></a>Chi può attivare la migrazione?

Tutti gli utenti che dispongono del ruolo predefinito di collaboratore del monitoraggio a livello di sottoscrizione possono attivare la migrazione. Gli utenti che dispongono di un ruolo personalizzato con le autorizzazioni seguenti possono anche attivare la migrazione:

- */lettura
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*
- Microsoft.AlertsManagement/smartDetectorAlertRules/*

> [!NOTE]
> Oltre a disporre delle autorizzazioni precedenti, la sottoscrizione deve essere registrata anche con il provider di risorse Microsoft. AlertsManagement. Questa operazione è necessaria per eseguire correttamente la migrazione degli avvisi di anomalia degli errori in Application Insights. 

## <a name="common-problems-and-remedies"></a>Problemi comuni e soluzioni

Dopo l' [attivazione della migrazione](alerts-using-migration-tool.md), si riceverà un messaggio di posta elettronica presso gli indirizzi forniti per notificare che la migrazione è stata completata o se è necessaria un'azione da parte dell'utente. In questa sezione vengono descritti alcuni problemi comuni e le relative modalità di gestione.

### <a name="validation-failed"></a>Convalida non riuscita

A causa di alcune recenti modifiche apportate alle regole di avviso classiche nella sottoscrizione, non è possibile eseguire la migrazione della sottoscrizione. Questo problema è temporaneo. È possibile riavviare la migrazione dopo che lo stato della migrazione è stato spostato di nuovo **pronto per la migrazione** entro pochi giorni.

### <a name="scope-lock-preventing-us-from-migrating-your-rules"></a>Blocco dell'ambito che impedisce la migrazione delle regole

Come parte della migrazione, verranno creati nuovi avvisi per le metriche e nuovi gruppi di azioni, quindi verranno eliminate le regole di avviso classiche. Tuttavia, un blocco di ambito può impedire la creazione o l'eliminazione di risorse. A seconda del blocco dell'ambito, non è stato possibile eseguire la migrazione di alcune o di tutte le regole. È possibile risolvere il problema rimuovendo il blocco dell'ambito per la sottoscrizione, il gruppo di risorse o la risorsa, elencato nello [strumento di migrazione](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel)e riattivando la migrazione. Il blocco dell'ambito non può essere disabilitato e deve essere rimosso per la durata del processo di migrazione. [Altre informazioni sulla gestione dei blocchi di ambito](../../azure-resource-manager/management/lock-resources.md#portal).

### <a name="policy-with-deny-effect-preventing-us-from-migrating-your-rules"></a>I criteri con effetto ' nega ' impediscono la migrazione delle regole

Come parte della migrazione, verranno creati nuovi avvisi per le metriche e nuovi gruppi di azioni, quindi verranno eliminate le regole di avviso classiche. Tuttavia, un'assegnazione di [criteri di Azure](../../governance/policy/index.yml) può impedire la creazione di risorse. A seconda dell'assegnazione dei criteri, non è stato possibile eseguire la migrazione di alcune o di tutte le regole. Le assegnazioni di criteri che bloccano il processo sono elencate nello [strumento di migrazione](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel). Per risolvere il problema, eseguire una delle operazioni seguenti:

- Esclusione di sottoscrizioni, gruppi di risorse o singole risorse per la durata del processo di migrazione dall'assegnazione dei criteri. [Altre informazioni sulla gestione degli ambiti di esclusione dei criteri](../../governance/policy/tutorials/create-and-manage.md#remove-a-non-compliant-or-denied-resource-from-the-scope-with-an-exclusion).
- Impostare la modalità di imposizione su **disabilitata** nell'assegnazione dei criteri. [Altre informazioni sulla proprietà enforcementMode dell'assegnazione dei criteri](../../governance/policy/concepts/assignment-structure.md#enforcement-mode).
- Impostare un'esenzione di criteri di Azure (anteprima) per le sottoscrizioni, i gruppi di risorse o le singole risorse nell'assegnazione dei criteri. [Altre informazioni sulla struttura di esenzione dei criteri di Azure](../../governance/policy/concepts/exemption-structure.md).
- La rimozione o la modifica dell'effetto in ' disabled ',' audit ',' append ' o ' Modify ' (che, ad esempio, può risolvere i problemi relativi ai tag mancanti). [Altre informazioni sulla gestione degli effetti dei criteri](../../governance/policy/concepts/definition-structure.md#policy-rule).

## <a name="next-steps"></a>Passaggi successivi

- [Come usare lo strumento di migrazione](alerts-using-migration-tool.md)
- [Preparare la migrazione](alerts-prepare-migration.md)
