---
title: Archivio delle novità del Centro sicurezza di Azure
description: Descrizione delle novità e delle modifiche apportate al Centro sicurezza di Azure a partire da sei mesi fa e in versioni precedenti.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2020
ms.author: memildin
ms.openlocfilehash: b7cb6edf825519bb3048de7a8c5326842f2db097
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99524294"
---
# <a name="archive-for-whats-new-in-azure-security-center"></a>Archivio delle novità del Centro sicurezza di Azure

La pagina principale delle note sulla versione [Novità del Centro sicurezza di Azure](release-notes.md) contiene aggiornamenti relativi agli ultimi sei mesi, mentre in questa pagina sono contenuti gli elementi meno recenti.

Questa pagina illustra quanto segue:

- Nuove funzionalità
- Correzioni di bug
- Funzionalità deprecate


## <a name="august-2020"></a>Agosto 2020

Gli aggiornamenti del mese di agosto includono quanto segue:

- [Inventario delle risorse: nuova visualizzazione potente del comportamento di sicurezza delle risorse](#asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets)
- [Aggiunta di supporto per le impostazioni predefinite per la sicurezza di Azure Active Directory per l'autenticazione a più fattori](#added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication)
- [Aggiunta di una raccomandazione per le entità servizio](#service-principals-recommendation-added)
- [Valutazione delle vulnerabilità nelle VM: consolidamento di raccomandazioni e criteri](#vulnerability-assessment-on-vms---recommendations-and-policies-consolidated)
- [Aggiunta di nuovi criteri di sicurezza del servizio Azure Kubernetes all'iniziativa ASC_default per l'uso esclusivo da parte dei clienti dell'anteprima privata](#new-aks-security-policies-added-to-asc_default-initiative--for-use-by-private-preview-customers-only)


### <a name="asset-inventory---powerful-new-view-of-the-security-posture-of-your-assets"></a>Inventario delle risorse: nuova visualizzazione potente del comportamento di sicurezza delle risorse

L'inventario delle risorse del Centro sicurezza, attualmente disponibile in anteprima, offre una modalità per visualizzare il comportamento di sicurezza delle risorse connesse al Centro sicurezza.

Il Centro sicurezza analizza periodicamente lo stato di sicurezza delle risorse di Azure per identificare potenziali vulnerabilità di sicurezza. Fornisce quindi raccomandazioni su come correggere tali vulnerabilità. Le risorse a cui sono associate raccomandazioni in attesa verranno visualizzate nell'inventario.

È possibile usare la visualizzazione e i rispettivi filtri per esplorare i dati del comportamento di sicurezza ed eseguire altre azioni in base ai risultati.

Altre informazioni sull'[inventario delle risorse](asset-inventory.md).


### <a name="added-support-for-azure-active-directory-security-defaults-for-multi-factor-authentication"></a>Aggiunta di supporto per le impostazioni predefinite per la sicurezza di Azure Active Directory per l'autenticazione a più fattori

Il Centro sicurezza ha aggiunto il supporto completo per le [impostazioni predefinite per la sicurezza](../active-directory/fundamentals/concept-fundamentals-security-defaults.md), ovvero le protezione gratuite di Microsoft per la sicurezza delle identità.

Le impostazioni predefinite per la sicurezza offrono impostazioni preconfigurate per la sicurezza delle identità per proteggere l'organizzazione da attacchi comuni correlati alle identità. Le impostazioni predefinite per la sicurezza proteggono già più di 5 milioni di tenant complessivamente. 50.000 tenant sono protetti anche dal Centro sicurezza.

Il Centro sicurezza fornisce ora una raccomandazione sulla sicurezza ogni volta che identifica una sottoscrizione di Azure in cui non sono abilitate le impostazioni predefinite per la sicurezza. Fino a oggi il Centro sicurezza ha consigliato l'abilitazione dell'autenticazione a più fattori con l'accesso condizionale, incluso nella licenza di Azure Active Directory (AD) Premium. Per i clienti che usano Azure AD Gratuito è ora consigliabile abilitare le impostazioni predefinite per la sicurezza. 

L'obiettivo consiste nell'invitare più clienti a proteggere gli ambienti cloud tramite autenticazione a più fattori e attenuare uno dei rischi più elevati e con impatto più significativo sul [punteggio di sicurezza](secure-score-security-controls.md).

Altre informazioni sulle [impostazioni predefinite per la sicurezza](../active-directory/fundamentals/concept-fundamentals-security-defaults.md).


### <a name="service-principals-recommendation-added"></a>Aggiunta di una raccomandazione per le entità servizio

È stata aggiunta una nuova raccomandazione per consigliare ai clienti del Centro sicurezza che usano i certificati di gestione per gestire le sottoscrizioni di passare alle entità servizio.

La raccomandazione **Per proteggere le sottoscrizioni è consigliabile usare le entità servizio invece dei certificati di gestione** consiglia di usare le entità servizio o Azure Resource Manager per migliorare la sicurezza della gestione delle sottoscrizioni. 

Altre informazioni su [Oggetti applicazione ed entità servizio in Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object).


### <a name="vulnerability-assessment-on-vms---recommendations-and-policies-consolidated"></a>Valutazione delle vulnerabilità nelle VM: consolidamento di raccomandazioni e criteri

Il Centro sicurezza esamina le VM per rilevare se eseguono una soluzione di valutazione delle vulnerabilità. Se non viene trovata alcuna soluzione di valutazione delle vulnerabilità, il Centro sicurezza fornisce una raccomandazione per semplificare la distribuzione.

Quando vengono trovate vulnerabilità, il Centro sicurezza fornisce una raccomandazione che riepiloga i risultati per consentire l'indagine e la correzione, se necessario.

Per assicurare un'esperienza coerente per tutti gli utenti, indipendentemente dal tipo di rilevatore usato, quattro raccomandazioni sono state combinate nelle due raccomandazioni seguenti:

|Raccomandazione unificata|Descrizione delle modifiche|
|----|:----|
|**È consigliabile abilitare una soluzione di valutazione della vulnerabilità nelle macchine virtuali**|Sostituisce le due raccomandazioni seguenti:<br> **•** Abilitare la soluzione di valutazione delle vulnerabilità predefinita nelle macchine virtuali (con tecnologia Qualys) - Ora deprecata e inclusa nel livello Standard<br> **•** È consigliabile installare una soluzione di valutazione della vulnerabilità nelle macchine virtuali - Ora deprecata (livelli Standard e Gratuito)|
|**È consigliabile correggere le vulnerabilità nelle macchine virtuali**|Sostituisce le due raccomandazioni seguenti:<br>**•** Correggere le vulnerabilità rilevate nelle macchine virtuali (con tecnologia Qualys) - Ora deprecata<br>**•** È consigliabile correggere le vulnerabilità tramite una soluzione di valutazione della vulnerabilità - Ora deprecata|
|||

Sarà ora possibile usare la stessa raccomandazione per distribuire l'estensione di valutazione delle vulnerabilità del Centro sicurezza o una soluzione con licenza privata ("BYOL") da un partner quale Qualys o Rapid7.

Quando le vulnerabilità vengono trovate e segnalate al Centro sicurezza, una singola raccomandazione informerà gli utenti in merito alla disponibilità dei risultati, indipendentemente dalla soluzione di valutazione delle vulnerabilità che le ha individuate.

#### <a name="updating-dependencies"></a>Aggiornamento delle dipendenze

Se sono presenti script, query o automazioni che fanno riferimento a raccomandazioni o chiavi/nomi di criteri precedenti, usare la tabella seguente per aggiornare i riferimenti:

##### <a name="before-august-2020"></a>Prima del mese di agosto 2020

|Recommendation|Ambito|
|----|:----|
|**Abilitare la soluzione di valutazione delle vulnerabilità predefinita nelle macchine virtuali (con tecnologia Qualys)**<br>Chiave: 550e890b-e652-4d22-8274-60b3bdb24c63|Predefinito|
|**Correggere le vulnerabilità rilevate nelle macchine virtuali (con tecnologia Qualys)**<br>Chiave: 1195afff-c881-495e-9bc5-1486211ae03f|Predefinito|
|**È consigliabile installare una soluzione di valutazione della vulnerabilità nelle macchine virtuali**<br>Chiave: 01b1ed4c-b733-4fee-b145-f23236e70cf3|BYOL|
|**Le vulnerabilità devono essere risolte tramite una soluzione di valutazione della vulnerabilità**<br>Chiave: 71992a2a-d168-42e0-b10e-6b45fa2ecddb|BYOL|
||||


|Policy|Ambito|
|----|:----|
|**La soluzione Valutazione della vulnerabilità deve essere abilitata nelle macchine virtuali**<br>ID criterio: 501541f7-f7e7-4cd6-868c-4190fdad3ac9|Predefinito|
|**È consigliabile correggere le vulnerabilità tramite una soluzione di valutazione della vulnerabilità**<br>ID criterio: 760a85ff-6162-42b3-8d70-698e268f648c|BYOL|
||||


##### <a name="from-august-2020"></a>Dal mese di agosto 2020

|Recommendation|Ambito|
|----|:----|
|**È consigliabile abilitare una soluzione di valutazione della vulnerabilità nelle macchine virtuali**<br>Chiave: ffff0522-1e88-47fc-8382-2a80ba848f5d|Predefinito + BYOL|
|**È consigliabile correggere le vulnerabilità nelle macchine virtuali**<br>Chiave: 1195afff-c881-495e-9bc5-1486211ae03f|Predefinito + BYOL|
||||

|Policy|Ambito|
|----|:----|
|[**La soluzione Valutazione della vulnerabilità deve essere abilitata nelle macchine virtuali**](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f501541f7-f7e7-4cd6-868c-4190fdad3ac9)<br>ID criterio: 501541f7-f7e7-4cd6-868c-4190fdad3ac9 |Predefinito + BYOL|
||||


### <a name="new-aks-security-policies-added-to-asc_default-initiative--for-use-by-private-preview-customers-only"></a>Aggiunta di nuovi criteri di sicurezza del servizio Azure Kubernetes all'iniziativa ASC_default per l'uso esclusivo da parte dei clienti dell'anteprima privata

Per assicurare che i carichi di lavoro di Kubernetes siano protetti per impostazione predefinita, il Centro sicurezza aggiunge criteri e raccomandazioni per la protezione avanzata a livello di Kubernetes, incluse opzioni di imposizione con il controllo ammissione di Kubernetes.

La fase iniziale di questo progetto include un'anteprima privata e l'aggiunta di nuovi criteri, disabilitati per impostazione predefinita, all'iniziativa ASC_default.

È possibile ignorare questi criteri, senza impatto sull'ambiente. Se si vogliono abilitare i criteri, iscriversi all'anteprima in https://aka.ms/SecurityPrP e selezionare una delle opzioni seguenti:

1. **Anteprima singola**: per partecipare solo a questa anteprima privata. Indicare esplicitamente "Analisi continua del Centro sicurezza di Azure" come anteprima a cui si vuole partecipare.
1. **Programma in corso**: per partecipare a questa anteprima privata e alle anteprime private future. È necessario completare un profilo e un accordo sulla privacy.


## <a name="july-2020"></a>Luglio 2020

Gli aggiornamenti del mese di luglio includono quanto segue:
- [Disponibilità della valutazione delle vulnerabilità per macchine virtuali per immagini non del marketplace](#vulnerability-assessment-for-virtual-machines-is-now-available-for-non-marketplace-images)
- [Espansione della protezione dalle minacce per Archiviazione di Azure in modo da includere File di Azure e Azure Data Lake Storage Gen2 (anteprima)](#threat-protection-for-azure-storage-expanded-to-include-azure-files-and-azure-data-lake-storage-gen2-preview)
- [Otto nuove raccomandazioni per abilitare le funzionalità di protezione dalle minacce](#eight-new-recommendations-to-enable-threat-protection-features)
- [Miglioramenti alla sicurezza dei contenitori: analisi più rapida dei registri e documentazione aggiornata](#container-security-improvements---faster-registry-scanning-and-refreshed-documentation)
- [Aggiornamento dei controlli applicazioni adattivi con una nuova raccomandazione e supporto per caratteri jolly nelle regole di percorso](#adaptive-application-controls-updated-with-a-new-recommendation-and-support-for-wildcards-in-path-rules)
- [Sei criteri per la funzionalità deprecata Sicurezza dei dati avanzata SQL](#six-policies-for-sql-advanced-data-security-deprecated)




### <a name="vulnerability-assessment-for-virtual-machines-is-now-available-for-non-marketplace-images"></a>Disponibilità della valutazione delle vulnerabilità per macchine virtuali per immagini non del marketplace

Durante la distribuzione di una soluzione di valutazione delle vulnerabilità, il Centro sicurezza eseguiva in precedenza un controllo di convalida prima della distribuzione. Il controllo consentiva di confermare la disponibilità di uno SKU del marketplace della macchina virtuale di destinazione. 

A partire da questo aggiornamento, il controllo è stato rimosso ed è ora possibile distribuire strumenti di valutazione delle vulnerabilità in computer Windows e Linux "personalizzati". Le immagini personalizzate sono immagini modificate a partire da impostazioni predefinite del marketplace.

Anche se è ora possibile distribuire l'estensione di valutazione delle vulnerabilità integrata (con tecnologia Qualys) in molti altri computer, il supporto è disponibile solo se si usa un sistema operativo elencato in [Distribuire il rilevatore di vulnerabilità integrato nelle macchine virtuali di livello Standard](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines)

Altre informazioni sul [rilevatore di vulnerabilità integrato per macchine virtuali (richiede Azure Defender)](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner).

Per altre informazioni sull'uso di una soluzione personalizzata di valutazione delle vulnerabilità con licenza privata di Qualys o Rapid7, vedere [Distribuzione di una soluzione di analisi delle vulnerabilità di partner](deploy-vulnerability-assessment-vm.md).


### <a name="threat-protection-for-azure-storage-expanded-to-include-azure-files-and-azure-data-lake-storage-gen2-preview"></a>Espansione della protezione dalle minacce per Archiviazione di Azure in modo da includere File di Azure e Azure Data Lake Storage Gen2 (anteprima)

Threat Protection per Archiviazione di Azure rileva le attività potenzialmente dannose negli account di archiviazione di Azure. Il Centro sicurezza mostra avvisi quando rileva tentativi di accesso o di exploit degli account di archiviazione. 

I dati possono essere protetti indipendentemente dal fatto che siano archiviati come contenitori BLOB, condivisioni file o data lake.




### <a name="eight-new-recommendations-to-enable-threat-protection-features"></a>Otto nuove raccomandazioni per abilitare le funzionalità di protezione dalle minacce

Sono state aggiunte otto nuove raccomandazioni per offrire un modo semplice per abilitare le funzionalità di protezione dalle minacce del Centro sicurezza di Azure per i tipi di risorse seguenti: macchine virtuali, piani di servizio app, server di database SQL di Azure, Server SQL nei computer, account di archiviazione di Azure, cluster del servizio Azure Kubernetes, registri del Registro Azure Container e insiemi di credenziali di Azure Key Vault.

Di seguito sono elencate le nuove raccomandazioni:

- **La soluzione Sicurezza dei dati avanzata deve essere abilitata nei server del database SQL di Azure**
- **La soluzione Sicurezza dei dati avanzata deve essere abilitata in SQL Server in macchine virtuali**
- **La soluzione Advanced Threat Protection deve essere abilitata nei piani di servizio app di Azure**
- **La soluzione Advanced Threat Protection deve essere abilitata nei registri in Registro Azure Container**
- **La soluzione Advanced Threat Protection deve essere abilitata negli insiemi di credenziali in Azure Key Vault**
- **La soluzione Advanced Threat Protection deve essere abilitata nei cluster del servizio Azure Kubernetes**
- **La soluzione Advanced Threat Protection deve essere abilitata negli account di archiviazione di Azure**
- **È consigliabile abilitare Advanced Threat Protection nelle macchine virtuali**

Queste nuove raccomandazioni appartengono al controllo di sicurezza **Abilita Azure Defender**.

Le raccomandazioni includono anche la capacità di correzione rapida. 

> [!IMPORTANT]
> La correzione in base a una di queste raccomandazioni comporterà addebiti per la protezione delle risorse rilevanti. L'applicazione degli addebiti inizierà immediatamente se sono presenti risorse correlate nella sottoscrizione corrente oppure inizierà in futuro se si aggiungono risorse in un momento successivo.
> 
> Se ad esempio non sono presenti cluster del servizio Azure Kubernetes nella sottoscrizione e si abilita la protezione dalle minacce, non verranno applicati addebiti. Se in futuro si aggiunge un cluster nella stessa sottoscrizione, il cluster verrà protetto automaticamente e l'applicazione degli addebiti avrà inizio in tale momento.

Per altre informazioni su ogni raccomandazione, vedere la [pagina di riferimento delle raccomandazioni sulla sicurezza](recommendations-reference.md).

Altre informazioni sulla [protezione dalle minacce nel Centro sicurezza di Azure](azure-defender.md).




### <a name="container-security-improvements---faster-registry-scanning-and-refreshed-documentation"></a>Miglioramenti alla sicurezza dei contenitori: analisi più rapida dei registri e documentazione aggiornata

Come parte degli investimenti continui nel dominio della sicurezza dei contenitori, è ora disponibile nel Centro sicurezza un miglioramento significativo delle prestazioni per le analisi dinamiche di immagini dei contenitori archiviate nel Registro Azure Container. Le analisi vengono ora in genere completate in due minuti circa. In alcuni casi l'analisi potrebbe richiedere fino a 15 minuti.

Per migliorare la chiarezza e le indicazioni correlate alle funzionalità di sicurezza dei contenitori del Centro sicurezza di Azure, sono stati anche apportati aggiornamenti alle pagine della documentazione sulla sicurezza. 

Per altre informazioni sulla sicurezza dei contenitori del Centro sicurezza, vedere gli articoli seguenti:

- [Panoramica delle funzionalità di sicurezza dei contenitori del Centro sicurezza](container-security.md)
- [Dettagli dell'integrazione con il Registro Azure Container](defender-for-container-registries-introduction.md)
- [Dettagli dell'integrazione con il servizio Azure Kubernetes](defender-for-kubernetes-introduction.md)
- [Come analizzare i registri e applicare la protezione avanzata agli host Docker](container-security.md)
- [Avvisi di sicurezza dalle funzionalità di protezione dalle minacce per cluster del servizio Azure Kubernetes](alerts-reference.md#alerts-akscluster)
- [Avvisi di sicurezza dalle funzionalità di protezione dalle minacce per host del servizio Azure Kubernetes](alerts-reference.md#alerts-containerhost)
- [Raccomandazioni sulla sicurezza per i contenitori](recommendations-reference.md#recs-compute)



### <a name="adaptive-application-controls-updated-with-a-new-recommendation-and-support-for-wildcards-in-path-rules"></a>Aggiornamento dei controlli applicazioni adattivi con una nuova raccomandazione e supporto per caratteri jolly nelle regole di percorso

Sono stati apportati due aggiornamenti significativi alla funzionalità Controlli applicazioni adattivi:

* Una nuova raccomandazione identifica un comportamento potenzialmente legittimo che non era consentito in precedenza. La nuova raccomandazione, **Le regole dell'elenco Consentiti dei criteri dei controlli applicazioni adattivi devono essere aggiornate**, richiede l'aggiunta di nuove regole al criterio esistente per ridurre il numero di falsi positivi negli avvisi di violazione dei controlli applicazioni adattivi.

* Le regole di percorso supportano ora i caratteri jolly. A partire da questo aggiornamento, è possibile configurare le regole di percorso consentite usando i caratteri jolly. Sono supportati due scenari:

    * Uso di un carattere jolly alla fine del percorso per consentire tutti i file eseguibili entro tale cartella e nelle sottocartelle

    * Uso di un carattere jolly al centro del percorso per consentire un nome di file eseguibile noto con un nome di cartella modificabile (ad esempio cartelle personali dell'utente con un file eseguibile noto, nomi di cartella generati automaticamente e così via)


[Altre informazioni sull'applicazione di controlli applicazioni adattivi](security-center-adaptive-application.md).



### <a name="six-policies-for-sql-advanced-data-security-deprecated"></a>Sei criteri per la funzionalità deprecata Sicurezza dei dati avanzata SQL

Verranno deprecati sei criteri correlati alla sicurezza dei dati avanzata per i computer SQL:

- I tipi di protezione di Advanced Threat Protection devono essere impostati su "Tutti" nelle impostazioni di Sicurezza dei dati avanzata dell'istanza gestita di SQL
- È necessario impostare i tipi di protezione di Advanced Threat Protection su "Tutti" nelle impostazioni di Sicurezza dei dati avanzata di SQL Server
- Le impostazioni avanzate di sicurezza dei dati per l'istanza gestita di SQL devono contenere un indirizzo di posta elettronica a cui ricevere gli avvisi di sicurezza
- Le impostazioni avanzate di sicurezza dei dati per SQL Server devono contenere un indirizzo di posta elettronica a cui ricevere gli avvisi di sicurezza
- Le notifiche tramite posta elettronica agli amministratori e ai proprietari della sottoscrizione devono essere abilitate nelle impostazioni di Sicurezza dei dati avanzata dell'istanza gestita di SQL
- Le notifiche tramite posta elettronica agli amministratori e ai proprietari della sottoscrizione devono essere abilitate nelle impostazioni di Sicurezza dei dati avanzata del server SQL

Altre informazioni sui [criteri predefiniti](./policy-reference.md).



## <a name="june-2020"></a>Giugno 2020

Gli aggiornamenti del mese di giugno includono quanto segue:
- [API Secure Score (anteprima)](#secure-score-api-preview)
- [Sicurezza dei dati avanzata per computer SQL (Azure, altri cloud e in locale) (anteprima)](#advanced-data-security-for-sql-machines-azure-other-clouds-and-on-premises-preview)
- [Due nuove raccomandazioni per distribuire l'agente di Log Analytics in computer Azure Arc (anteprima)](#two-new-recommendations-to-deploy-the-log-analytics-agent-to-azure-arc-machines-preview)
- [Nuovi criteri per creare configurazioni di esportazione continua e di automazione dei flussi di lavoro su larga scala](#new-policies-to-create-continuous-export-and-workflow-automation-configurations-at-scale)
- [Nuova raccomandazione per l'uso dei gruppi di sicurezza di rete per proteggere macchine virtuali non connesse a Internet](#new-recommendation-for-using-nsgs-to-protect-non-internet-facing-virtual-machines)
- [Nuovi criteri per l'abilitazione della protezione dalle minacce e della sicurezza dei dati avanzata](#new-policies-for-enabling-threat-protection-and-advanced-data-security)



### <a name="secure-score-api-preview"></a>API Secure Score (anteprima)

È ora possibile accedere al punteggio tramite l'[API Secure Score](/rest/api/securitycenter/securescores/), attualmente disponibile in anteprima. I metodi dell'API offrono la flessibilità necessaria per eseguire query nei dati e creare un meccanismo personalizzato per la creazione di report sui punteggi di sicurezza nel tempo. È ad esempio possibile usare l'API **Secure Score** per ottenere il punteggio per una sottoscrizione specifica. È anche possibile usare l'API **Secure Score Controls** per elencare i controlli di sicurezza e il punteggio corrente delle sottoscrizioni.

Per esempi di strumenti esterni consentiti dall'API Secure Score, vedere l'[area relativa a Secure Score della community di GitHub](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score).

Altre informazioni sul [punteggio di sicurezza e i controlli di sicurezza nel Centro sicurezza di Azure](secure-score-security-controls.md).



### <a name="advanced-data-security-for-sql-machines-azure-other-clouds-and-on-premises-preview"></a>Sicurezza dei dati avanzata per computer SQL (Azure, altri cloud e in locale) (anteprima)

La funzionalità Sicurezza dei dati avanzata per computer SQL del Centro sicurezza di Azure protegge ora i server SQL ospitati in Azure, in altri ambienti cloud e anche in computer locali. Le protezioni per i server SQL nativi di Azure vengono quindi estese in modo da supportare completamente gli ambienti ibridi.

Sicurezza dei dati avanzata offre la valutazione delle vulnerabilità e la protezione avanzata dalle minacce per i computer SQL, ovunque si trovino.

La configurazione comporta due passaggi:

1. Distribuzione dell'agente di Log Analytics nel computer host di SQL Server per fornire la connessione all'account Azure.

1. Abilitazione dell'aggregazione facoltativa della pagina dei prezzi e delle impostazioni del Centro sicurezza.

Altre informazioni sulla [sicurezza dei dati avanzata per computer SQL](defender-for-sql-usage.md).



### <a name="two-new-recommendations-to-deploy-the-log-analytics-agent-to-azure-arc-machines-preview"></a>Due nuove raccomandazioni per distribuire l'agente di Log Analytics in computer Azure Arc (anteprima)

Sono state aggiunte due nuove raccomandazioni per semplificare la distribuzione dell'[Agente di Log Analytics](../azure-monitor/platform/log-analytics-agent.md) nei computer Azure Arc e per assicurare che siano protetti dal Centro sicurezza di Azure:

- **L'agente di Log Analytics deve essere installato nelle macchine virtuali Azure Arc basate su Windows (anteprima)**
- **L'agente di Log Analytics deve essere installato nelle macchine virtuali Azure Arc basate su Linux (anteprima)**

Queste nuove raccomandazioni verranno visualizzate negli stessi quattro controlli di sicurezza che includono la raccomandazione esistente correlata, **È consigliabile installare l'agente di monitoraggio nei computer**: Correggi le configurazioni di sicurezza, Applica il controllo applicazioni adattivo, Applica gli aggiornamenti del sistema e Abilita Endpoint Protection.

Le raccomandazioni includono anche la funzionalità di correzione rapida per accelerare il processo di distribuzione. 

Per altre informazioni su queste due nuove raccomandazioni, vedere la tabella [Raccomandazioni sulle risorse di calcolo e sulle app](recommendations-reference.md#recs-compute).

Per altre informazioni sul modo in cui il Centro sicurezza di Azure usa l'agente, vedere [Che cos'è l'agente di Log Analytics?](faq-data-collection-agents.md#what-is-the-log-analytics-agent).

Altre informazioni sulle [estensioni per i computer Azure Arc](../azure-arc/servers/manage-vm-extensions.md).


### <a name="new-policies-to-create-continuous-export-and-workflow-automation-configurations-at-scale"></a>Nuovi criteri per creare configurazioni di esportazione continua e di automazione dei flussi di lavoro su larga scala

L'automazione dei processi di monitoraggio e risposta agli eventi imprevisti dell'organizzazione può migliorare significativamente il tempo necessario per indagare e attenuare gli eventi imprevisti relativi alla sicurezza.

Per distribuire le configurazioni di automazione nell'organizzazione, usare questi criteri "DeployIfdNotExist" predefiniti di Azure per creare e configurare le procedure di [esportazione continua](continuous-export.md) e [automazione dei flussi di lavoro](workflow-automation.md):

I criteri sono disponibili in Criteri di Azure:


|Obiettivo  |Policy  |ID condizione  |
|---------|---------|---------|
|Esportazione continua in Hub eventi|[Distribuisci esportazione in hub eventi per gli avvisi e le raccomandazioni del Centro sicurezza di Azure](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fcdfcce10-4578-4ecd-9703-530938e4abcb)|cdfcce10-4578-4ecd-9703-530938e4abcb|
|Esportazione continua nell'area di lavoro Log Analytics|[Distribuisci esportazione nell'area di lavoro Log Analytics per gli avvisi e le raccomandazioni del Centro sicurezza di Azure](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fffb6f416-7bd2-4488-8828-56585fef2be9)|ffb6f416-7bd2-4488-8828-56585fef2be9|
|Automazione dei flussi di lavoro per gli avvisi di sicurezza|[Distribuisci automazione del flusso di lavoro per gli avvisi del Centro sicurezza di Azure](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2ff1525828-9a90-4fcf-be48-268cdd02361e)|f1525828-9a90-4fcf-be48-268cdd02361e|
|Automazione dei flussi di lavoro per le raccomandazioni sulla sicurezza|[Distribuisci automazione del flusso di lavoro per le raccomandazioni del Centro sicurezza di Azure](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f73d6ab6c-2475-4850-afd6-43795f3492ef)|73d6ab6c-2475-4850-afd6-43795f3492ef|
||||

Introduzione ai [modelli di automazione dei flussi di lavoro](https://github.com/Azure/Azure-Security-Center/tree/master/Workflow%20automation).

Per altre informazioni sull'uso dei due criteri di esportazione, vedere [Configurare l'automazione del flusso di lavoro su larga scala usando i criteri forniti](workflow-automation.md#configure-workflow-automation-at-scale-using-the-supplied-policies) e [Configurare un'esportazione continua](continuous-export.md#set-up-a-continuous-export).


### <a name="new-recommendation-for-using-nsgs-to-protect-non-internet-facing-virtual-machines"></a>Nuova raccomandazione per l'uso dei gruppi di sicurezza di rete per proteggere macchine virtuali non connesse a Internet

Il controllo di sicurezza "Implementa le procedure consigliate per la sicurezza" include ora la nuova raccomandazione seguente:

- **Le macchine virtuali senza connessione Internet devono essere protette con i gruppi di sicurezza di rete**

Una raccomandazione esistente, **Le macchine virtuali con connessione Internet devono essere protette con i gruppi di sicurezza di rete**, non distingueva tra le macchine virtuali connesse a Internet e non connesse a Internet. Veniva generata per entrambi i tipi di VM una raccomandazione con gravità alta se una VM non era assegnata a un gruppo di sicurezza di rete. Questa nuova raccomandazione separa le macchine virtuali non connesse a Internet per ridurre i falsi positivi ed evitare avvisi con gravità elevata non necessari.

Per altre informazioni, vedere la tabella [Raccomandazioni sulla rete](recommendations-reference.md#recs-networking).




### <a name="new-policies-for-enabling-threat-protection-and-advanced-data-security"></a>Nuovi criteri per l'abilitazione della protezione dalle minacce e della sicurezza dei dati avanzata

I nuovi criteri seguenti sono stati aggiunti all'iniziativa predefinita del Centro sicurezza di Azure e sono stati progettati per agevolare l'abilitazione della protezione dalle minacce e della sicurezza dei dati avanzata per i tipi di risorse rilevanti.

I criteri sono disponibili in Criteri di Azure:


| Policy                                                                                                                                                                                                                                                                | ID condizione                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| [La soluzione Sicurezza dei dati avanzata deve essere abilitata nei server del database SQL di Azure](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f7fe3b40f-802b-4cdd-8bd4-fd799c948cc2)     | 7fe3b40f-802b-4cdd-8bd4-fd799c948cc2 |
| [La soluzione Sicurezza dei dati avanzata deve essere abilitata in SQL Server in macchine virtuali](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f6581d072-105e-4418-827f-bd446d56421b) | 6581d072-105e-4418-827f-bd446d56421b |
| [La soluzione Advanced Threat Protection deve essere abilitata negli account di archiviazione di Azure](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f308fbb08-4ab8-4e67-9b29-592e93fb94fa)           | 308fbb08-4ab8-4e67-9b29-592e93fb94fa |
| [La soluzione Advanced Threat Protection deve essere abilitata negli insiemi di credenziali in Azure Key Vault](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f0e6763cc-5078-4e64-889d-ff4d9a839047)           | 0e6763cc-5078-4e64-889d-ff4d9a839047 |
| [La soluzione Advanced Threat Protection deve essere abilitata nei piani di servizio app di Azure](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f2913021d-f2fd-4f3d-b958-22354e2bdbcb)                | 2913021d-f2fd-4f3d-b958-22354e2bdbcb |
| [La soluzione Advanced Threat Protection deve essere abilitata nei registri in Registro Azure Container](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fc25d9a16-bc35-4e15-a7e5-9db606bf9ed4)   | c25d9a16-bc35-4e15-a7e5-9db606bf9ed4 |
| [La soluzione Advanced Threat Protection deve essere abilitata nei cluster del servizio Azure Kubernetes](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f523b5cd1-3e23-492f-a539-13118b6d1e3a)   | 523b5cd1-3e23-492f-a539-13118b6d1e3a |
| [La soluzione Advanced Threat Protection deve essere abilitata nelle macchine virtuali](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2f4da35fc9-c9e7-4960-aec9-797fe7d9051d)           | 4da35fc9-c9e7-4960-aec9-797fe7d9051d |
|                                                                                                                                                                                                                                                                       |                                      |

Altre informazioni sulla [protezione dalle minacce nel Centro sicurezza di Azure](azure-defender.md).


## <a name="may-2020"></a>Maggio 2020

Gli aggiornamenti del mese di maggio includono quanto segue:
- [Regole di eliminazione degli avvisi (anteprima)](#alert-suppression-rules-preview)
- [La valutazione delle vulnerabilità delle macchine virtuali è ora disponibile a livello generale](#virtual-machine-vulnerability-assessment-is-now-generally-available)
- [Modifiche all'accesso JIT (just-in-time) alle macchine virtuali](#changes-to-just-in-time-jit-virtual-machine-vm-access)
- [Le raccomandazioni personalizzate sono state spostate in un controllo di sicurezza distinto](#custom-recommendations-have-been-moved-to-a-separate-security-control)
- [Interruttore aggiunto per visualizzare le raccomandazioni nei controlli o come elenco semplice](#toggle-added-to-view-recommendations-in-controls-or-as-a-flat-list)
- [Controllo di sicurezza espanso "Implement security best practices" (Implementa procedure consigliate per la sicurezza)](#expanded-security-control-implement-security-best-practices)
- [I criteri personalizzati con metadati personalizzati sono ora disponibili a livello generale](#custom-policies-with-custom-metadata-are-now-generally-available)
- [Migrazione delle funzionalità di analisi dei dump di arresto anomalo al rilevamento di attacchi senza file](#crash-dump-analysis-capabilities-migrating-to-fileless-attack-detection)


### <a name="alert-suppression-rules-preview"></a>Regole di eliminazione degli avvisi (anteprima)

Questa nuova funzionalità, attualmente in anteprima, semplifica le attività correlate agli avvisi. Usare regole per nascondere automaticamente gli avvisi noti come innocui o correlati alle normali attività dell'organizzazione. In questo modo è possibile concentrarsi sulle minacce più pertinenti. 

Gli avvisi che corrispondono alle regole di eliminazione abilitate vengono comunque generati, ma il relativo stato verrà impostato su Ignorato. È possibile visualizzare lo stato nel portale di Azure o accedere agli avvisi di sicurezza del Centro sicurezza.

Le regole di eliminazione definiscono i criteri per cui gli avvisi devono essere automaticamente ignorati. In genere, si usa una regola di eliminazione per:

- eliminare gli avvisi identificati come falsi positivi

- eliminare gli avvisi che vengono attivati troppo spesso per essere utili

Sono disponibili altre informazioni sull'[eliminazione degli avvisi](alerts-suppression-rules.md) dalla protezione dalle minacce del Centro sicurezza di Azure.


### <a name="virtual-machine-vulnerability-assessment-is-now-generally-available"></a>La valutazione delle vulnerabilità delle macchine virtuali è ora disponibile a livello generale

Il livello standard del Centro sicurezza include ora una valutazione integrata delle vulnerabilità per le macchine virtuali, senza costi aggiuntivi. Questa estensione è supportata da Qualys, ma segnala i risultati direttamente al Centro sicurezza. Non è necessaria una licenza Qualys, né un account Qualys: tutto viene gestito senza interruzioni all'interno del Centro sicurezza.

La nuova soluzione può analizzare continuamente le macchine virtuali per individuare le vulnerabilità e presentare i risultati nel Centro sicurezza. 

Per distribuire la soluzione, usare la nuova raccomandazione per la sicurezza:

"Abilitare la soluzione di valutazione delle vulnerabilità predefinita nelle macchine virtuali (con tecnologia Qualys)"

Sono disponibili altre informazioni sulla [valutazione delle vulnerabilità integrata del Centro sicurezza per le macchine virtuali](deploy-vulnerability-assessment-vm.md#overview-of-the-integrated-vulnerability-scanner).



### <a name="changes-to-just-in-time-jit-virtual-machine-vm-access"></a>Modifiche all'accesso JIT (just-in-time) alle macchine virtuali

Il Centro sicurezza include una funzionalità opzionale per proteggere le porte di gestione delle macchine virtuali. Questa funzionalità garantisce una difesa contro la forma più comune di attacchi di forza bruta.

Questo aggiornamento apporta le seguenti modifiche a questa funzionalità:

- La raccomandazione che consiglia di abilitare JIT in una macchina virtuale è stata rinominata. "Il controllo di accesso alla rete JIT deve essere applicato alle macchine virtuali" è ora diventato: "Le porte di gestione delle macchine virtuali devono essere protette tramite un controllo di accesso alla rete JIT".

- La raccomandazione viene attivata solo se sono presenti porte di gestione aperte.

Sono disponibili altre informazioni sulla [funzionalità di accesso JIT](security-center-just-in-time.md).


### <a name="custom-recommendations-have-been-moved-to-a-separate-security-control"></a>Le raccomandazioni personalizzate sono state spostate in un controllo di sicurezza distinto

Un controllo di sicurezza introdotto con il punteggio di sicurezza migliorato è "Implementa le procedure consigliate per la sicurezza". Eventuali raccomandazioni personalizzate create per le sottoscrizioni sono state inserite automaticamente nel controllo. 

Per semplificare l'individuazione delle raccomandazioni personalizzate, sono state spostate in un controllo di sicurezza dedicato, "Custom Recommendations" (Raccomandazioni personalizzate). Questo controllo non ha alcun effetto sul punteggio di sicurezza.

Altre informazioni sui controlli di sicurezza sono disponibili in [Enhanced secure score (preview) in Azure Security Center](secure-score-security-controls.md) (Punteggio di sicurezza migliorato (anteprima) nel Centro sicurezza di Azure).


### <a name="toggle-added-to-view-recommendations-in-controls-or-as-a-flat-list"></a>Interruttore aggiunto per visualizzare le raccomandazioni nei controlli o come elenco semplice

I controlli di sicurezza sono gruppi logici di raccomandazioni correlate alla sicurezza. Riflettono le superfici di attacco vulnerabili. Un controllo è un set di raccomandazioni sulla sicurezza, con istruzioni che consentono di implementare tali raccomandazioni.

Per verificare immediatamente in che modo l'organizzazione protegge ogni singola superficie di attacco, esaminare i punteggi per ogni controllo di sicurezza.

Per impostazione predefinita, le raccomandazioni vengono visualizzate nei controlli di sicurezza. Da questo aggiornamento è inoltre possibile visualizzarle come elenco. Per visualizzarle come semplici elenchi ordinati in base allo stato di integrità delle risorse interessate, usare il nuovo interruttore di raggruppamento per controlli. L'interruttore è in cima all'elenco nel portale.

I controlli di sicurezza, e questo interruttore, fanno parte della nuova esperienza di assegnazione dei punteggi di sicurezza. Ricordarsi di inviare feedback dall'interno del portale.

Altre informazioni sui controlli di sicurezza sono disponibili in [Enhanced secure score (preview) in Azure Security Center](secure-score-security-controls.md) (Punteggio di sicurezza migliorato (anteprima) nel Centro sicurezza di Azure).

:::image type="content" source="./media/secure-score-security-controls/recommendations-group-by-toggle.gif" alt-text="Interruttore Raggruppa in base ai controlli per le raccomandazioni":::

### <a name="expanded-security-control-implement-security-best-practices"></a>Controllo di sicurezza espanso "Implement security best practices" (Implementa procedure consigliate per la sicurezza) 

Un controllo di sicurezza introdotto con il punteggio di sicurezza migliorato è "Implementa le procedure consigliate per la sicurezza". Quando una raccomandazione si trova in questo controllo, non ha alcun effetto sul punteggio di sicurezza. 

Con questo aggiornamento, tre raccomandazioni sono state spostate dai controlli in cui erano originariamente posizionate e inserite in questo controllo di procedure consigliate. Ciò è dovuto al fatto che il rischio di queste tre raccomandazioni è risultato inferiore a quello inizialmente previsto.

Sono state introdotte due nuove raccomandazioni aggiunte a questo controllo.

Le tre raccomandazioni spostate sono:

- **La funzionalità MFA deve essere abilitata per gli account con autorizzazioni di lettura per la sottoscrizione** (in origine, il controllo "Abilita MFA")
- **Gli account esterni con autorizzazioni di lettura devono essere rimossi dalla sottoscrizione** (in origine, il controllo "Manage access and permissions") (Gestisci accesso e autorizzazioni)
- **Deve essere designato un massimo di 3 proprietari per la sottoscrizione** (in origine, il controllo "Manage access and permissions") (Gestisci accesso e autorizzazioni)

Le due nuove raccomandazioni aggiunte al controllo sono:

- **È consigliabile installare l'estensione Configurazione guest nelle macchine virtuali Windows (anteprima)** : l'uso di [Configurazione guest di Criteri di Azure](../governance/policy/concepts/guest-configuration.md) fornisce la visibilità all'interno delle macchine virtuali per le impostazioni di server e applicazioni (solo Windows).

- **È consigliabile abilitare Windows Defender Exploit Guard nei computer (anteprima)** - Windows Defender Exploit Guard sfrutta i vantaggi di Configurazione guest di Criteri di Azure. Exploit Guard include quattro componenti progettati per bloccare i dispositivi da un'ampia gamma di vettori di attacco e comportamenti di blocco usati comunemente negli attacchi malware, consentendo al contempo alle aziende di bilanciare i requisiti di rischi per la sicurezza e produttività (solo Windows).

Per altre informazioni su Windows Defender Exploit Guard, vedere [Creare e distribuire criteri di Exploit Guard](/mem/configmgr/protect/deploy-use/create-deploy-exploit-guard-policy).

Per altre informazioni sui controlli di sicurezza, vedere [Punteggio di sicurezza migliorato (anteprima)](secure-score-security-controls.md).



### <a name="custom-policies-with-custom-metadata-are-now-generally-available"></a>I criteri personalizzati con metadati personalizzati sono ora disponibili a livello generale

I criteri personalizzati fanno ora parte delle raccomandazioni del Centro sicurezza, del punteggio di sicurezza e del dashboard degli standard di conformità alle normative. Questa funzionalità è ora disponibile a livello generale e consente di estendere la copertura della valutazione della sicurezza dell'organizzazione nel Centro sicurezza. 

Creare un'iniziativa personalizzata in Criteri di Azure, aggiungervi criteri e integrarla nel Centro sicurezza di Azure, quindi visualizzarla come raccomandazione.

È stata anche aggiunta la possibilità di modificare i metadati delle raccomandazioni personalizzate. Le opzioni dei metadati includono gravità, procedure di correzione, informazioni sulle minacce e altro ancora.  

Sono disponibili altre informazioni sull'[ottimizzazione delle raccomandazioni personalizzate con informazioni dettagliate](custom-security-policies.md#enhance-your-custom-recommendations-with-detailed-information).



### <a name="crash-dump-analysis-capabilities-migrating-to-fileless-attack-detection"></a>Migrazione delle funzionalità di analisi dei dump di arresto anomalo al rilevamento di attacchi senza file 

Le funzionalità di rilevamento delle analisi dei dump di arresto anomalo di Windows sono state integrate nel [rilevamento degli attacchi senza file](defender-for-servers-introduction.md#what-are-the-benefits-of-azure-defender-for-servers). L'analisi di rilevamento degli attacchi senza file offre versioni migliorate dei seguenti avvisi di sicurezza per i computer Windows: individuazione di code injection, rilevamento di moduli Windows mascherati, individuazione di shellcode e rilevamento di un segmento di codice sospetto.

Alcuni vantaggi di questa transizione:

- **Rilevamento proattivo e tempestivo di malware**: l'approccio di analisi dei dump di arresto anomalo prevedeva l'attesa che si verificasse un arresto anomalo e quindi l'esecuzione dell'analisi per individuare gli artefatti dannosi. L'uso del rilevamento di attacchi senza file consente di identificare in modo proattivo le minacce in memoria durante l'esecuzione. 

- **Avvisi avanzati**: gli avvisi di sicurezza dal rilevamento di attacchi senza file includono miglioramenti non disponibili con l'analisi dei dump di arresto anomalo, come le informazioni sulle connessioni di rete attive. 

- **Aggregazione di avvisi**: quando l'analisi dei dump di arresto anomalo rilevava più schemi di attacco in un singolo evento di arresto, attivava più avvisi di sicurezza. Il rilevamento di attacchi senza file combina tutti gli schemi di attacco identificati dallo stesso processo in un singolo avviso, eliminando la necessità di correlare più avvisi.

- **Riduzione dei requisiti nell'area di lavoro Log Analytics**: i dump di arresto anomalo contenenti dati potenzialmente sensibili non verranno più caricati nell'area di lavoro Log Analytics.






## <a name="april-2020"></a>Aprile 2020

Gli aggiornamenti del mese di aprile includono quanto segue:
- [I pacchetti di conformità dinamici sono ora disponibili a livello generale](#dynamic-compliance-packages-are-now-generally-available)
- [Raccomandazioni per l'identità ora incluse nel livello gratuito del Centro sicurezza di Azure](#identity-recommendations-now-included-in-azure-security-center-free-tier)


### <a name="dynamic-compliance-packages-are-now-generally-available"></a>I pacchetti di conformità dinamici sono ora disponibili a livello generale

Il dashboard di conformità alle normative del Centro sicurezza di Azure include ora **pacchetti di conformità dinamici** (ora disponibili a livello generale) per tenere traccia di altri standard di settore e normativi.

È possibile aggiungere pacchetti di conformità dinamici alla sottoscrizione o al gruppo di gestione dalla pagina dei criteri di sicurezza del Centro sicurezza. Quando viene caricato uno standard o un benchmark, viene visualizzato nel dashboard di conformità alle normative insieme ai dati di conformità associati mappati come valutazioni. È anche possibile scaricare un report di riepilogo per gli standard caricati.

Ora è possibile aggiungere standard come:

- **NIST SP 800-53 R4**
- **SWIFT CSP CSCF-v2020**
- **UK Official e UK NHS**
- **Canada Federal PBMM**
- **Azure CIS 1.1.0 (novità)** (una rappresentazione più completa di Azure CIS 1.1.0)

Inoltre, è stato aggiunto di recente **Azure Security Benchmark**, le linee guida specifiche di Azure, create da Microsoft per le procedure consigliate per la sicurezza e la conformità basate su framework di conformità comuni. Nel dashboard verranno supportati altri standard non appena saranno disponibili.  
 
Sono disponibili altre informazioni sulla [personalizzazione del set di standard nel dashboard di conformità alle normative](update-regulatory-compliance-packages.md).


### <a name="identity-recommendations-now-included-in-azure-security-center-free-tier"></a>Raccomandazioni per l'identità ora incluse nel livello gratuito del Centro sicurezza di Azure

Sono ora disponibili a livello generale raccomandazioni di sicurezza relative a identità e accesso nel livello gratuito del Centro sicurezza di Azure. Questa operazione rientra nell'impegno volto a rendere gratuite le funzionalità CSPM (Cloud Security Posture Management). Fino ad ora, queste raccomandazioni erano disponibili solo nel piano tariffario standard.

Esempi di raccomandazioni relative a identità e accesso includono:

- "La funzionalità MFA deve essere abilitata per gli account con autorizzazioni di proprietario per la sottoscrizione".
- "Deve essere designato un massimo di 3 proprietari per la sottoscrizione".
- "Gli account deprecati devono essere rimossi dalla sottoscrizione".

Se si hanno sottoscrizioni nel piano tariffario gratuito, il punteggio di sicurezza corrispondente sarà influenzato da questa modifica, in quanto non sono state precedentemente valutate in termini di sicurezza dell'accesso e delle identità.

Sono disponibili altre informazioni sulle [raccomandazioni relative a identità e accesso](recommendations-reference.md#recs-identityandaccess).

Sono disponibili altre informazioni sul [monitoraggio di identità e accesso](security-center-identity-access.md).



## <a name="march-2020"></a>Marzo 2020

Gli aggiornamenti di marzo includono:

- [L'automazione del flusso di lavoro è ora disponibile a livello generale](#workflow-automation-is-now-generally-available)
- [Integrazione del Centro sicurezza di Azure con Windows Admin Center](#integration-of-azure-security-center-with-windows-admin-center)
- [Protezione per il servizio Azure Kubernetes](#protection-for-azure-kubernetes-service)
- [Esperienza JIT migliorata](#improved-just-in-time-experience)
- [Due raccomandazioni sulla sicurezza per le applicazioni Web sono ora deprecate](#two-security-recommendations-for-web-applications-deprecated)


### <a name="workflow-automation-is-now-generally-available"></a>L'automazione del flusso di lavoro è ora disponibile a livello generale

La funzionalità di automazione del flusso di lavoro del Centro sicurezza di Azure è ora disponibile a livello generale. È possibile usarla per attivare automaticamente le app per la logica negli avvisi di sicurezza e nelle raccomandazioni. Sono inoltre disponibili trigger manuali per gli avvisi e tutte le raccomandazioni per le quali è disponibile l'opzione di correzione rapida.

Ogni programma di sicurezza include più flussi di lavoro per la risposta agli eventi imprevisti. Questi processi possono includere la notifica a stakeholder di rilievo, l'avvio di un processo di gestione delle modifiche e l'applicazione di procedure di correzione specifiche. Gli esperti di sicurezza raccomandano di automatizzare quante più procedure possibili. L'automazione riduce il sovraccarico e può migliorare la sicurezza garantendo che i passaggi del processo vengano eseguiti rapidamente, in modo coerente e in base ai requisiti predefiniti.

Per altre informazioni sulle funzionalità automatiche e manuali del Centro sicurezza per l'esecuzione dei flussi di lavoro, vedere [Automazione del flusso di lavoro](workflow-automation.md).

Sono disponibili altre informazioni sulla [creazione di app per la logica](../logic-apps/logic-apps-overview.md).


### <a name="integration-of-azure-security-center-with-windows-admin-center"></a>Integrazione del Centro sicurezza di Azure con Windows Admin Center

È ora possibile spostare i server Windows locali da Windows Admin Center direttamente nel Centro sicurezza di Azure. Il Centro sicurezza diventa quindi il punto centrale in cui visualizzare le informazioni di sicurezza per tutte le risorse Windows Admin Center, inclusi i server locali, le macchine virtuali e carichi di lavoro PaaS aggiuntivi.

Dopo aver spostato un server da Windows Admin Center al Centro sicurezza di Azure, sarà possibile:

- Visualizzare gli avvisi di sicurezza e le raccomandazioni nell'estensione del Centro sicurezza di Windows Admin Center.
- Visualizzare il comportamento di sicurezza e recuperare informazioni dettagliate aggiuntive dei server gestiti di Windows Admin Center nel Centro sicurezza all'interno del portale di Azure (o tramite un'API).

Sono disponibili altre informazioni su [come integrare il Centro sicurezza di Azure con Windows Admin Center](windows-admin-center-integration.md).


### <a name="protection-for-azure-kubernetes-service"></a>Protezione per il servizio Azure Kubernetes

Il Centro sicurezza di Azure espande le funzionalità di sicurezza dei contenitori per proteggere il servizio Azure Kubernetes.

La piattaforma Kubernetes open source è talmente diffusa da essere diventata standard di settore per l'orchestrazione dei contenitori. Ciononostante, non sono ancora disponibili informazioni chiare su come proteggere un ambiente Kubernetes. Per difendere le aree di attacco di un'applicazione aggiunta a un contenitore, è necessario avere una certa esperienza per garantire che l'infrastruttura sia configurata in modo sicuro e costantemente monitorata per potenziali minacce.

La difesa del Centro sicurezza include:

- **Individuazione e visibilità**: individuazione continua delle istanze gestite del servizio Azure Kubernetes all'interno delle sottoscrizioni registrate nel Centro sicurezza.
- **Raccomandazioni sulla sicurezza**: raccomandazioni utili per la conformità alle procedure consigliate sulla sicurezza per il servizio Azure Kubernetes. Queste raccomandazioni sono incluse nel punteggio di sicurezza per garantire che vengano visualizzate nell'ambito del comportamento di sicurezza dell'organizzazione. Un esempio di raccomandazione relativa al servizio Azure Kubernetes potrebbe essere "Il controllo degli accessi in base al ruolo deve essere usato per limitare l'accesso a un cluster del servizio Kubernetes".
- **Protezione dalle minacce**: grazie all'analisi continua della distribuzione del servizio Azure Kubernetes, il Centro sicurezza avvisa l'utente in merito a minacce e attività dannose rilevate a livello di host e del cluster del servizio.

Sono disponibili altre informazioni sull'[integrazione del servizio Azure Kubernetes con il Centro sicurezza](defender-for-kubernetes-introduction.md).

Sono disponibili altre informazioni sulle [funzionalità di sicurezza dei contenitori nel Centro sicurezza](container-security.md).


### <a name="improved-just-in-time-experience"></a>Esperienza JIT migliorata

Le funzionalità, il funzionamento e l'interfaccia utente per gli strumenti just-in-time (JIT) del Centro sicurezza di Azure che proteggono le porte di gestione sono stati migliorati come segue: 

- **Campo di giustificazione**: quando si richiede l'accesso a una macchina virtuale tramite la pagina JIT del portale di Azure, è disponibile un nuovo campo facoltativo per immettere una giustificazione per la richiesta. Le informazioni immesse in questo campo possono essere rilevate nel log attività. 
- **Pulizia automatica di regole JIT ridondanti**: ogni volta che si aggiorna un criterio JIT, viene eseguito automaticamente uno strumento di pulizia per verificare la validità dell'intero set di regole. Lo strumento cerca eventuali mancate corrispondenze tra le regole nei criteri e le regole nel gruppo di sicurezza di rete. Se lo strumento di pulizia rileva una mancata corrispondenza, determina la cause e, se l'operazione è sicura, rimuove le regole incorporate che non sono più necessarie. Lo strumento di pulizia non elimina mai le regole create dall'utente. 

Sono disponibili altre informazioni sulla [funzionalità di accesso JIT](security-center-just-in-time.md).


### <a name="two-security-recommendations-for-web-applications-deprecated"></a>Due raccomandazioni sulla sicurezza per le applicazioni Web sono ora deprecate

Due raccomandazioni sulla sicurezza per le applicazioni Web sono ora considerate deprecate: 

- È consigliabile applicare la protezione avanzata alle regole per le applicazioni Web nei gruppi di sicurezza di rete IaaS
    (criterio correlato: È necessario rafforzare le regole dei gruppi di sicurezza di rete per le applicazioni Web nella soluzione IaaS).

- L'accesso ai Servizi app deve essere limitato
    (criterio correlato: [Anteprima]: L'accesso ai Servizi app deve essere limitato).

Queste raccomandazioni non verranno più visualizzate nel relativo elenco del Centro sicurezza. I criteri correlati non verranno più inclusi nell'iniziativa denominata "Criteri predefiniti del Centro sicurezza".

Sono disponibili altre informazioni sulle [raccomandazioni relative alla sicurezza](recommendations-reference.md).




## <a name="february-2020"></a>Febbraio 2020

### <a name="fileless-attack-detection-for-linux-preview"></a>Rilevamento di attacchi senza file per Linux (anteprima)

Dal momento che gli utenti malintenzionati impiegano metodi sempre più sofisticati per evitare il rilevamento, il Centro sicurezza di Azure estende il rilevamento degli attacchi senza file a Linux, oltre che a Windows. Gli attacchi senza file sfruttano le vulnerabilità del software, introducono payload dannosi in processi di sistema benigni e si nascondono nella memoria. Queste tecniche:

- riducono al minimo o eliminano del tutto le tracce di malware sul disco
- riducono notevolmente le probabilità di rilevamento da parte di soluzioni di analisi dei malware basate su disco

Per contrastare questa minaccia, il Centro sicurezza di Azure ha rilasciato il rilevamento degli attacchi senza file per Windows nell'ottobre 2018 e ora ha esteso questa funzionalità anche a Linux. 



## <a name="january-2020"></a>Gennaio 2020

### <a name="enhanced-secure-score-preview"></a>Punteggio di sicurezza migliorato (anteprima)

Una versione migliorata della funzionalità di punteggio di sicurezza del Centro sicurezza di Azure è ora disponibile in anteprima. In questa versione, più raccomandazioni vengono raggruppate nei controlli di sicurezza che riflettono meglio le superfici di attacco vulnerabili, limitando ad esempio l'accesso alle porte di gestione.

Acquisire familiarità con le modifiche apportate al punteggio di sicurezza durante la fase di anteprima e determinare altre correzioni che consentiranno di proteggere ulteriormente l'ambiente.

Altre informazioni su [Punteggio sicuro avanzato (anteprima)](secure-score-security-controls.md).



## <a name="november-2019"></a>Novembre 2019

Gli aggiornamenti del mese di novembre includono quanto segue:
 - [Protezione dalle minacce per Azure Key Vault in aree America del Nord (anteprima)](#threat-protection-for-azure-key-vault-in-north-america-regions-preview)
 - [La protezione dalle minacce per Archiviazione di Azure include lo screening della reputazione del malware](#threat-protection-for-azure-storage-includes-malware-reputation-screening)
 - [Automazione del flusso di lavoro con App per la logica (anteprima)](#workflow-automation-with-logic-apps-preview)
 - [Correzione rapida per le risorse bulk disponibili a livello generale](#quick-fix-for-bulk-resources-generally-available)
 - [Analizzare le immagini del contenitore per le vulnerabilità (anteprima)](#scan-container-images-for-vulnerabilities-preview)
 - [Standard aggiuntivi di conformità con le normative (anteprima)](#additional-regulatory-compliance-standards-preview)
 - [Protezione dalle minacce per il servizio Azure Kubernetes (anteprima)](#threat-protection-for-azure-kubernetes-service-preview)
 - [Valutazione della vulnerabilità della macchina virtuale (anteprima)](#virtual-machine-vulnerability-assessment-preview)
 - [Sicurezza dei dati avanzata per i server SQL in macchine virtuali di Azure (anteprima)](#advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview)
 - [Supporto per criteri personalizzati (anteprima)](#support-for-custom-policies-preview)
 - [Estensione della copertura del Centro sicurezza di Azure con piattaforma per community e partner](#extending-azure-security-center-coverage-with-platform-for-community-and-partners)
 - [Integrazioni avanzate con l'esportazione di raccomandazioni e avvisi (anteprima)](#advanced-integrations-with-export-of-recommendations-and-alerts-preview)
 - [Eseguire l'onboarding dei server locali nel Centro sicurezza da Windows Admin Center (anteprima)](#onboard-on-prem-servers-to-security-center-from-windows-admin-center-preview)

### <a name="threat-protection-for-azure-key-vault-in-north-america-regions-preview"></a>Protezione dalle minacce per Azure Key Vault in aree America del Nord (anteprima)

Azure Key Vault è un servizio essenziale per la protezione dei dati e il miglioramento delle prestazioni delle applicazioni cloud che offre la possibilità di gestire in modo centralizzato chiavi, segreti, chiavi crittografiche e criteri nel cloud. Poiché Azure Key Vault archivia dati sensibili e aziendali critici, richiede la massima sicurezza per gli insiemi di credenziali delle chiavi e i dati archiviati.

Il supporto del Centro sicurezza di Azure per la protezione dalle minacce per Azure Key Vault offre un livello aggiuntivo di intelligence di sicurezza in grado di rilevare tentativi insoliti e potenzialmente dannosi di accesso o sfruttamento di insiemi di credenziali delle chiavi. Questo nuovo livello di protezione consente ai clienti di affrontare le minacce agli insiemi di credenziali delle chiavi senza dover essere esperti di sicurezza o dover gestire sistemi di monitoraggio della sicurezza. La funzionalità è in anteprima pubblica nelle aree dell'America del Nord.


### <a name="threat-protection-for-azure-storage-includes-malware-reputation-screening"></a>La protezione dalle minacce per Archiviazione di Azure include lo screening della reputazione del malware

La protezione dalle minacce per Archiviazione di Azure offre nuovi rilevamenti basati su Microsoft Threat Intelligence per il rilevamento di caricamenti di malware in Archiviazione di Azure tramite l'analisi della reputazione hash e l'accesso sospetto da un nodo di uscita Tor attivo (anonimizzazione proxy). È ora possibile visualizzare i malware rilevati negli account di archiviazione usando il Centro sicurezza di Azure.


### <a name="workflow-automation-with-logic-apps-preview"></a>Automazione del flusso di lavoro con App per la logica (anteprima)

Le organizzazioni con sicurezza gestita a livello centralizzato e IT/operazioni implementano processi di flusso di lavoro interni per condurre le azioni necessarie all'interno dell'organizzazione quando le discrepanze vengono individuate nei propri ambienti. In molti casi, questi flussi di lavoro sono processi ripetibili e l'automazione può semplificare significativamente i processi all'interno dell'organizzazione.

Attualmente viene introdotta una nuova funzionalità del Centro sicurezza che consente ai clienti di creare configurazioni di automazione sfruttando App per la logica di Azure e di creare criteri che li attiveranno automaticamente in base a risultati del certificato del servizio app specifici, ad esempio raccomandazioni o avvisi. App per la logica di Azure può essere configurato in modo da eseguire qualsiasi azione personalizzata supportata dalla vasta community di connettori di App per la logica oppure da usare uno dei modelli forniti dal Centro sicurezza, ad esempio l'invio di un messaggio di posta elettronica o l'apertura di un ticket ServiceNow™.

Per altre informazioni sulle funzionalità automatiche e manuali del Centro sicurezza per l'esecuzione dei flussi di lavoro, vedere [automazione del flusso di lavoro](workflow-automation.md).

Per informazioni sulla creazione di app per la logica, vedere [App per la logica di Azure](../logic-apps/logic-apps-overview.md).


### <a name="quick-fix-for-bulk-resources-generally-available"></a>Correzione rapida per le risorse bulk disponibili a livello generale

Con le numerose attività fornite a un utente come parte del punteggio di sicurezza, può essere difficile correggere efficacemente i problemi in una flotta di grandi dimensioni.

Per semplificare la correzione di configurazioni di sicurezza errate e per poter correggere rapidamente le raccomandazioni in un blocco di risorse e migliorare il punteggio di sicurezza, usare la correzione rapida.

Questa operazione consente di selezionare le risorse a cui si vuole applicare la correzione e di avviare un'azione di correzione che configurerà l'impostazione per conto dell'utente.

La correzione rapida è attualmente disponibile a livello generale come parte della pagina Consigli del Centro sicurezza.

Nella [guida di riferimento alle raccomandazioni sulla sicurezza](recommendations-reference.md), vedere quali raccomandazioni sono abilitate per la correzione rapida.


### <a name="scan-container-images-for-vulnerabilities-preview"></a>Analizzare le immagini del contenitore per le vulnerabilità (anteprima)

Il Centro sicurezza di Azure può ora analizzare le immagini del contenitore in Registro Azure Container per individuare eventuali vulnerabilità.

L'analisi delle immagini funziona analizzando il file dell'immagine del contenitore e quindi controllando se sono presenti vulnerabilità note (con tecnologia Qualys).

L'analisi viene attivata automaticamente quando si effettua il push di nuove immagini del contenitore in Registro Azure Container. Le vulnerabilità rilevate verranno riportate come raccomandazioni del Centro sicurezza e incluse nel punteggio di sicurezza di Azure insieme alle informazioni su come applicare una patch per ridurre la superficie di attacco consentita.


### <a name="additional-regulatory-compliance-standards-preview"></a>Standard aggiuntivi di conformità con le normative (anteprima)

Il dashboard per la conformità con le normative fornisce informazioni approfondite sul comportamento di conformità in base alle valutazioni del Centro sicurezza. Il dashboard mostra il modo in cui l'ambiente è conforme ai controlli e ai requisiti designati da standard normativi specifici e da benchmark di settore e fornisce consigli per la gestione di questi requisiti.

Il dashboard di conformità con le normative ha finora supportato quattro standard predefiniti:  Azure CIS 1.1.0, PCI-DSS, ISO 27001 e SOC-TSP. Microsoft sta per annunciare la versione di anteprima pubblica degli standard aggiuntivi supportati: NIST SP 800-53 R4, SWIFT CSP CSCF v2020, Canada Federal PBMM e UK Official insieme a UK NHS. Sta anche rilasciando una versione aggiornata di Azure CIS 1.1.0, che include più controlli dello standard e un miglioramento dell'estendibilità.

[Sono disponibili altre informazioni sulla personalizzazione del set di standard nel dashboard di conformità con le normative](update-regulatory-compliance-packages.md).


### <a name="threat-protection-for-azure-kubernetes-service-preview"></a>Protezione dalle minacce per il servizio Azure Kubernetes (anteprima)

Kubernetes sta diventando rapidamente il nuovo standard per la distribuzione e la gestione del software nel cloud. Pochi utenti hanno un'esperienza completa con Kubernetes e molti si concentrano solo sulla progettazione e l'amministrazione generali e danno uno sguardo rapido all'aspetto della sicurezza. Per garantire la sicurezza, l'ambiente Kubernetes deve essere configurato con cura, assicurandosi che non siano aperte porte della superficie di attacco incentrate sul contenitore per gli utenti malintenzionati. Il Centro sicurezza sta espandendo il supporto nello spazio del contenitore a uno dei servizi dalla crescita più rapida in Azure: il servizio Azure Kubernetes.

Le nuove funzionalità di questa versione di anteprima pubblica includono:

- **Individuazione e visibilità**: individuazione continua delle istanze del servizio Azure Kubernetes gestite nelle sottoscrizioni registrate del Centro sicurezza.
- **Raccomandazioni per il Punteggio sicuro** : elementi utilizzabili per aiutare i clienti a rispettare le procedure consigliate per la sicurezza per AKS e ad aumentare il Punteggio sicuro. Le raccomandazioni includono elementi come il controllo degli accessi in base al ruolo da usare per limitare l'accesso a un cluster del servizio Kubernetes ".
- **Rilevamento delle minacce**: analisi basata su cluster e host, ad esempio "rilevato contenitore con privilegi".


### <a name="virtual-machine-vulnerability-assessment-preview"></a>Valutazione della vulnerabilità della macchina virtuale (anteprima)

Le applicazioni installate in macchine virtuali possono spesso avere vulnerabilità che potrebbero causare una violazione della macchina virtuale. Si sta annunciando che il livello standard del Centro sicurezza include la valutazione della vulnerabilità incorporata per le macchine virtuali senza costi aggiuntivi. La valutazione della vulnerabilità, basata su Qualys nell'anteprima pubblica, consentirà di analizzare continuamente tutte le applicazioni installate in una macchina virtuale per individuare le applicazioni vulnerabili e presentare i risultati nell'esperienza del portale del Centro sicurezza. Il Centro sicurezza si occupa di tutte le operazioni di distribuzione in modo che non sia necessario alcun lavoro aggiuntivo da parte dell'utente. In futuro si prevede di fornire opzioni per la valutazione della vulnerabilità per supportare le esigenze aziendali specifiche dei clienti.

[Altre informazioni sulle valutazioni della vulnerabilità per le macchine virtuali di Azure](deploy-vulnerability-assessment-vm.md).


### <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview"></a>Sicurezza dei dati avanzata per i server SQL in macchine virtuali di Azure (anteprima)

Il supporto del Centro sicurezza di Azure per la protezione dalle minacce e la valutazione della vulnerabilità per i database SQL in esecuzione sulle macchine virtuali IaaS è ora disponibile in anteprima.

[Valutazione della vulnerabilità](../azure-sql/database/sql-vulnerability-assessment.md) è un servizio semplice da configurare che consente di individuare, tenere traccia e risolvere potenziali vulnerabilità del database. Consente di visualizzare il comportamento di sicurezza come parte del punteggio di sicurezza di Azure e prevede passaggi per risolvere i problemi di sicurezza e migliorare la protezione del database.

[Advanced Threat Protection](../azure-sql/database/threat-detection-overview.md) rileva attività anomale che indicano tentativi insoliti e potenzialmente dannosi di accedere o sfruttare il server SQL. Monitora in modo continuo il database in caso di attività sospette e fornisce avvisi di sicurezza orientati all'azione su modelli di accesso ai database anomali. Questi avvisi forniscono dettagli sulle attività sospette e azioni consigliate per l'analisi e la mitigazione della minaccia.


### <a name="support-for-custom-policies-preview"></a>Supporto per criteri personalizzati (anteprima)

Il Centro sicurezza di Azure supporta ora criteri personalizzati (in anteprima).

I clienti Microsoft attendono da tempo la possibilità di estendere la copertura attuale della valutazione della sicurezza nel Centro sicurezza con le proprie valutazioni della sicurezza in base ai criteri creati in Criteri di Azure. Con il supporto per i criteri personalizzati, l'estensione della copertura è ora possibile.

Questi nuovi criteri faranno parte delle raccomandazioni del Centro sicurezza, del punteggio di sicurezza e del dashboard degli standard di conformità con le normative. Con il supporto per i criteri personalizzati, è ora possibile creare un'iniziativa personalizzata in Criteri di Azure e quindi aggiungerla come criterio nel Centro sicurezza e visualizzarla come raccomandazione.


### <a name="extending-azure-security-center-coverage-with-platform-for-community-and-partners"></a>Estensione della copertura del Centro sicurezza di Azure con piattaforma per community e partner

Usare il Centro sicurezza per ricevere raccomandazioni non solo da Microsoft, ma anche da soluzioni esistenti di partner quali Check Point, Tenable e CyberArk con molte altre integrazioni in arrivo.  Il semplice flusso di caricamento del Centro sicurezza è in grado di connettere le soluzioni esistenti al Centro sicurezza, consentendo di visualizzare le raccomandazioni sul comportamento di sicurezza in un'unica posizione, eseguire report unificati e sfruttare tutte le funzionalità del Centro sicurezza in base alle raccomandazioni predefinite e ai partner. È anche possibile esportare le raccomandazioni del Centro sicurezza nei prodotti partner.

[Altre informazioni su Microsoft Intelligent Security Association](https://www.microsoft.com/security/partnerships/intelligent-security-association).



### <a name="advanced-integrations-with-export-of-recommendations-and-alerts-preview"></a>Integrazioni avanzate con l'esportazione di raccomandazioni e avvisi (anteprima)

Per abilitare gli scenari a livello aziendale sul Centro sicurezza, è ora possibile utilizzare gli avvisi e le raccomandazioni del Centro sicurezza in altre posizioni, ad eccezione del portale di Azure o dell'API. È possibile esportarli direttamente in un hub eventi e nelle aree di lavoro Log Analytics. Ecco alcuni flussi di lavoro che è possibile creare con queste nuove funzionalità:

- Con l'esportazione nell'area di lavoro Log Analytics è possibile creare dashboard personalizzati con Power BI.
- Con l'esportazione nell'hub eventi, sarà possibile esportare gli avvisi e le raccomandazioni del Centro sicurezza in SIEM di terze parti, in una soluzione di terze parti in tempo reale o in Esplora dati di Azure.


### <a name="onboard-on-prem-servers-to-security-center-from-windows-admin-center-preview"></a>Eseguire l'onboarding dei server locali nel Centro sicurezza da Windows Admin Center (anteprima)

Windows Admin Center è un portale di gestione per i server Windows non distribuiti in Azure, che offre diverse funzionalità di gestione di Azure, ad esempio backup e aggiornamenti di sistema. Microsoft ha recentemente aggiunto la possibilità di eseguire l'onboarding di questi server non Azure, in modo che siano protetti dal certificato del servizio app direttamente dall'esperienza di Windows Admin Center.

Grazie a questa nuova esperienza, gli utenti potranno eseguire l'onboarding di un server Windows Admin Center nel Centro sicurezza di Azure e consentire la visualizzazione degli avvisi di sicurezza e delle raccomandazioni direttamente nell'esperienza di Windows Admin Center.


## <a name="september-2019"></a>Settembre 2019

Gli aggiornamenti del mese di settembre includono quanto segue:

 - [Gestione delle regole con i miglioramenti apportati ai controlli applicazioni adattivi](#managing-rules-with-adaptive-application-controls-improvements)
 - [Controllare la raccomandazione sulla sicurezza del contenitore usando Criteri di Azure](#control-container-security-recommendation-using-azure-policy)

### <a name="managing-rules-with-adaptive-application-controls-improvements"></a>Gestione delle regole con i miglioramenti apportati ai controlli applicazioni adattivi

L'esperienza di gestione delle regole per le macchine virtuali che usano i controlli applicazioni adattivi è stata migliorata. I controlli applicazioni adattivi del Centro sicurezza di Azure consentono di controllare quali applicazioni possono essere eseguite nelle macchine virtuali. Oltre a un miglioramento generale per la gestione delle regole, un nuovo vantaggio consiste nella possibilità di controllare quali tipi di file verranno protetti quando si aggiunge una nuova regola.

[Altre informazioni sull'applicazione di controlli applicazioni adattivi](security-center-adaptive-application.md).


### <a name="control-container-security-recommendation-using-azure-policy"></a>Controllare la raccomandazione sulla sicurezza del contenitore usando Criteri di Azure

La raccomandazione del Centro sicurezza di Azure per correggere le vulnerabilità nella sicurezza del contenitore ora può essere abilitata o disabilitata tramite Criteri di Azure.

Per visualizzare i criteri di sicurezza abilitati, dal Centro sicurezza aprire la pagina Criteri di sicurezza.


## <a name="august-2019"></a>Agosto 2019

Gli aggiornamenti del mese di agosto includono quanto segue:

 - [Accesso just-in-time (JIT) alla macchina virtuale per Firewall di Azure](#just-in-time-jit-vm-access-for-azure-firewall)
 - [Correzione con un solo clic per migliorare il comportamento di sicurezza (anteprima)](#single-click-remediation-to-boost-your-security-posture-preview)
 - [Gestione tra tenant](#cross-tenant-management)

### <a name="just-in-time-jit-vm-access-for-azure-firewall"></a>Accesso just-in-time (JIT) alla macchina virtuale per Firewall di Azure 

L'accesso just-in-time alla macchina virtuale per Firewall di Azure è ora disponibile a livello generale. Usarlo per proteggere gli ambienti protetti da Firewall di Azure in aggiunta agli ambienti protetti con gruppo di sicurezza di rete.

L'accesso JIT alla macchina virtuale riduce l'esposizione agli attacchi volumetrici di rete, fornendo l'accesso controllato alle macchine virtuali solo quando necessario, usando le regole di Firewall di Azure e il gruppo di sicurezza di rete.

Quando si abilita JIT per le macchine virtuali, si crea un criterio che determina le porte da proteggere, per quanto tempo devono rimanere aperte e gli indirizzi IP approvati da cui è possibile accedere a queste porte. Questo criterio consente di mantenere il controllo sulle operazioni che gli utenti possono eseguire quando richiedono l'accesso.

Le richieste vengono registrate nel log attività di Azure, in modo che sia possibile monitorare e controllare facilmente l'accesso. La pagina JIT consente anche di identificare rapidamente le macchine virtuali esistenti in cui è abilitato JIT e le macchine virtuali in cui è consigliabile usare JIT.

[Altre informazioni su Firewall di Azure](../firewall/overview.md).


### <a name="single-click-remediation-to-boost-your-security-posture-preview"></a>Correzione con un solo clic per migliorare il comportamento di sicurezza (anteprima)

Il punteggio di sicurezza è uno strumento che consente di valutare le condizioni di sicurezza del carico di lavoro. Esamina i consigli sulla sicurezza e assegna a ciascuno un livello di priorità che indica quali consigli implementare per primi. Questo è utile per trovare le vulnerabilità della sicurezza più gravi e stabilire le priorità di indagine.

Per semplificare la correzione delle configurazioni di sicurezza errate e contribuire a migliorare rapidamente il punteggio di sicurezza, è stata aggiunta una nuova funzionalità che consente di correggere una raccomandazione su un blocco di risorse con un solo clic.

Questa operazione consente di selezionare le risorse a cui si vuole applicare la correzione e di avviare un'azione di correzione che configurerà l'impostazione per conto dell'utente.

Nella [guida di riferimento alle raccomandazioni sulla sicurezza](recommendations-reference.md), vedere quali raccomandazioni sono abilitate per la correzione rapida.


### <a name="cross-tenant-management"></a>Gestione tra tenant

Il Centro sicurezza supporta ora scenari di gestione tra tenant come parte di Azure Lighthouse. Questo consente di ottenere visibilità e gestire il comportamento di sicurezza di più tenant nel Centro sicurezza. 

[Informazioni sulle esperienze di gestione tra tenant](security-center-cross-tenant-management.md).


## <a name="july-2019"></a>Luglio 2019

### <a name="updates-to-network-recommendations"></a>Aggiornamenti alle raccomandazioni sulle risorse di rete

Il Centro sicurezza di Azure ha lanciato nuove raccomandazioni sulle risorse di rete e ne sono state migliorate alcune esistenti. Ora, l'uso del Centro sicurezza garantisce una maggiore protezione della rete per le risorse. 

[Altre informazioni sulle raccomandazioni sulle risorse di rete](recommendations-reference.md#recs-networking).


## <a name="june-2019"></a>Giugno 2019

### <a name="adaptive-network-hardening---generally-available"></a>Protezione avanzata adattiva per la rete disponibile a livello generale

Una delle maggiori superfici di attacco per i carichi di lavoro in esecuzione nel cloud pubblico sono le connessioni da e verso la rete Internet pubblica. I clienti Microsoft hanno difficoltà a capire quali regole del gruppo di sicurezza di rete (NSG) devono essere applicate per assicurarsi che i carichi di lavoro di Azure siano disponibili solo per gli intervalli di origine richiesti. Con questa funzionalità, il Centro sicurezza apprende i modelli del traffico di rete e di connettività dei carichi di lavoro di Azure e fornisce indicazioni sulle regole del gruppo di sicurezza di rete per le macchine virtuali con connessione Internet. Questo consente ai clienti di configurare meglio i criteri di accesso alla rete e di limitare l'esposizione agli attacchi. 

[Altre informazioni sulla protezione avanzata adattiva per la rete](security-center-adaptive-network-hardening.md).