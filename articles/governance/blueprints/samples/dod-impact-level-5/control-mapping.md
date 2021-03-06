---
title: Controlli dell'esempio di progetto DoD Impact Level 5
description: Mapping dei controlli dell'esempio di progetto DoD Impact Level 5. Ogni controllo viene mappato a una o più definizioni di Criteri di Azure che assistono nella valutazione.
ms.date: 01/08/2021
ms.topic: sample
ms.openlocfilehash: 01f786684e5f8d73f57eb9f4741593c01fe1c8d4
ms.sourcegitcommit: c4c554db636f829d7abe70e2c433d27281b35183
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98034782"
---
# <a name="control-mapping-of-the-dod-impact-level-5-blueprint-sample"></a>Mapping dei controlli dell'esempio di progetto DoD Impact Level 5

L'articolo seguente illustra come viene eseguito il mapping tra l'esempio di progetto di Azure Blueprints Department of Defense Impact Level 5 (DoD IL5) e i controlli DoD Impact Level 5. Per altre informazioni sui controlli, vedere [DoD Cloud Computing Security Requirements Guide (SRG)](https://dl.dod.cyber.mil/wp-content/uploads/cloud/pdf/Cloud_Computing_SRG_v1r3.pdf).
La Defense Information Systems Agency (DISA) è un'agenzia del Department of Defense (DoD) degli Stati Uniti responsabile dello sviluppo e della gestione della DoD Cloud Computing Security Requirements Guide (SRG). La guida SRG definisce i requisiti di sicurezza di base per i provider di servizi cloud che ospitano informazioni, sistemi e applicazioni del DoD e per l'uso di servizi cloud da parte del DoD.  

I mapping seguenti fanno riferimento ai controlli **DoD Impact Level 5**. Usare la barra di spostamento a destra per passare direttamente a uno specifico mapping. Molti controlli mappati vengono implementati con un'iniziativa di [Criteri di Azure](../../../policy/overview.md). Per esaminare l'iniziativa completa, aprire **Criteri** nel portale di Azure e selezionare la pagina **Definizioni**. Trovare e selezionare l'iniziativa dei criteri predefinita **\[Anteprima\]: DoD Impact Level 5**.

> [!IMPORTANT]
> Ogni controllo tra quelli riportati di seguito è associato a una o più definizioni di [Criteri di Azure](../../../policy/overview.md). Questi criteri possono aiutare a [valutare la conformità](../../../policy/how-to/get-compliance-data.md) con il controllo. In molti casi tuttavia non si tratta di una corrispondenza uno-a-uno o completa tra un controllo e uno o più criteri. Di per sé, **Conforme** in Criteri di Azure si riferisce solo ai criteri stessi e non garantisce che l'utente sia completamente conforme a tutti i requisiti di un controllo. Inoltre, in questo momento lo standard di conformità include controlli che non vengono gestiti da alcuna definizione di Criteri di Azure. La conformità in Criteri di Azure è quindi solo una visualizzazione parziale dello stato di conformità generale. Le associazioni tra i controlli e le definizioni di Criteri di Azure per questo esempio di progetto di conformità possono cambiare nel tempo. Per visualizzare la cronologia delle modifiche, vedere la [cronologia dei commit di GitHub](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/dod-impact-level-5/control-mapping.md).

## <a name="ac-2-account-management"></a>AC-2 Gestione degli account

Questo progetto consente di esaminare gli account che potrebbero non conformi ai requisiti di gestione degli account dell'organizzazione. Il progetto assegna definizioni di [Criteri di Azure](../../../policy/overview.md) che controllano gli account esterni con autorizzazioni di lettura, scrittura e proprietario per una sottoscrizione e gli account deprecati. Verificando gli account controllati da questi criteri, è possibile intervenire per assicurarsi che i requisiti di gestione degli account siano soddisfatti.

- Gli account deprecati devono essere rimossi dalla sottoscrizione
- Gli account deprecati con autorizzazioni di proprietario devono essere rimossi dalla sottoscrizione
- Gli account esterni con autorizzazioni di proprietario devono essere rimossi dalla sottoscrizione
- Gli account esterni con autorizzazioni di lettura devono essere rimossi dalla sottoscrizione
- Gli account esterni con autorizzazioni di scrittura devono essere rimossi dalla sottoscrizione

## <a name="ac-2-7-account-management--role-based-schemes"></a>AC-2 (7) Gestione degli account | Schemi basati sui ruoli

Azure implementa il [Controllo degli accessi in base al ruolo di Azure](../../../../role-based-access-control/overview.md) che semplifica la gestione dell'accesso degli utenti alle risorse in Azure. Usando il portale di Azure, è possibile verificare chi ha accesso alle risorse di Azure e le relative autorizzazioni. Questo progetto assegna inoltre definizioni di [Criteri di Azure](../../../policy/overview.md) per controllare l'uso dell'autenticazione di Azure Active Directory per istanze di SQL Server e Service Fabric. L'uso dell'autenticazione di Azure Active Directory consente una gestione semplificata delle autorizzazioni e una gestione centralizzata delle identità degli utenti di database e di altri servizi Microsoft. Questo progetto assegna inoltre una definizione di Criteri di Azure per controllare l'uso di regole personalizzate del Controllo degli accessi in base al ruolo di Azure. Identificando dove vengono implementate regole personalizzate del Controllo degli accessi in base al ruolo di Azure, è possibile verificare l'esigenza e la corretta implementazione, perché tali regole sono soggette a errore.

- È consigliabile effettuare il provisioning di un amministratore di Azure Active Directory per SQL Server
- Controlla l'uso di ruoli di controllo degli accessi in base al ruolo personalizzati
- I cluster di Service Fabric deve usare solo Azure Active Directory per l'autenticazione client

## <a name="ac-2-12-account-management--account-monitoring--atypical-usage"></a>AC-2 (12) Gestione degli account | Monitoraggio/Utilizzo atipico degli account

L'accesso JIT alle macchine virtuali blocca il traffico in ingresso alle macchine virtuali di Azure, riducendo l'esposizione agli attacchi e al tempo stesso offrendo un facile accesso per connettersi alle macchine virtuali quando necessario. Tutte le richieste JIT di accesso alle macchine virtuali vengono registrate nel Log attività allo scopo di monitorare l'eventuale utilizzo atipico. Questo progetto assegna una definizione di [Criteri di Azure](../../../policy/overview.md) che consente di monitorare le macchine virtuali in grado di supportare l'accesso JIT ma che non sono state ancora configurate.

- Le porte di gestione delle macchine virtuali devono essere protette tramite un controllo di accesso alla rete JIT

## <a name="ac-4-information-flow-enforcement"></a>AC-4 Imposizione del controllo dei flussi di informazioni

Con la funzionalità Condivisione di risorse tra le origini (CORS) le risorse di Servizi app possono essere richieste da un dominio esterno. Microsoft consiglia di consentire solo ai domini richiesti di interagire con l'API, la funzione e le applicazioni Web. Questo progetto assegna una definizione di [Criteri di Azure](../../../policy/overview.md) per consentire il monitoraggio delle limitazioni di accesso alle risorse CORS in Centro sicurezza di Azure. La comprensione delle implementazioni CORS può essere utile per verificare l'implementazione dei controlli del flusso di informazioni.

- CORS non deve consentire a tutte le risorse di accedere alle applicazioni Web

## <a name="ac-5-separation-of-duties"></a>AC-5 Separazione dei compiti

La presenza di un solo proprietario di sottoscrizioni di Azure non consente la ridondanza amministrativa. Al contrario, la presenza di troppi proprietari di sottoscrizioni di Azure può aumentare la probabilità che si verifichi una violazione tramite un account di proprietario compromesso. Questo progetto consente di mantenere un numero appropriato di proprietari di sottoscrizioni di Azure assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che controllano questo numero. Questo progetto assegna anche definizioni di Criteri di Azure che consentono di controllare l'appartenenza del gruppo Administrators nelle macchine virtuali Windows. La gestione delle autorizzazioni dei proprietari di sottoscrizioni e degli amministratori di macchine virtuali consente di implementare la separazione appropriata dei compiti.

- Per la sottoscrizione devono essere designati al massimo 3 proprietari
- Mostra i risultati del controllo dalle macchine virtuali Windows in cui il gruppo Administrators contiene uno qualsiasi dei membri specificati
- Mostra i risultati del controllo dalle macchine virtuali Windows in cui il gruppo Administrators non contiene tutti i membri specificati
- Distribuisci i prerequisiti per controllare le macchine virtuali Windows in cui il gruppo Administrators contiene uno qualsiasi dei membri specificati
- Distribuisci i prerequisiti per controllare le macchine virtuali Windows in cui il gruppo Administrators non contiene tutti i membri specificati
- Alla sottoscrizione deve essere assegnato più di un proprietario

## <a name="ac-6-7-least-privilege--review-of-user-privileges"></a>AC-6 (7) Privilegi minimi | Revisione dei privilegi degli utenti

Azure implementa il [Controllo degli accessi in base al ruolo di Azure](../../../../role-based-access-control/overview.md) che semplifica la gestione dell'accesso degli utenti alle risorse in Azure. Usando il portale di Azure, è possibile verificare chi ha accesso alle risorse di Azure e le relative autorizzazioni. Questo progetto assegna definizioni di [Criteri di Azure](../../../policy/overview.md) per controllare gli account la cui verifica dovrebbe essere prioritaria. L'esame di questi indicatori di account è utile per verificare l'implementazione dei controlli dei privilegi minimi.

- Per la sottoscrizione devono essere designati al massimo 3 proprietari
- Mostra i risultati del controllo dalle macchine virtuali Windows in cui il gruppo Administrators contiene uno qualsiasi dei membri specificati
- Mostra i risultati del controllo dalle macchine virtuali Windows in cui il gruppo Administrators non contiene tutti i membri specificati
- Distribuisci i prerequisiti per controllare le macchine virtuali Windows in cui il gruppo Administrators contiene uno qualsiasi dei membri specificati
- Distribuisci i prerequisiti per controllare le macchine virtuali Windows in cui il gruppo Administrators non contiene tutti i membri specificati
- Alla sottoscrizione deve essere assegnato più di un proprietario

## <a name="ac-17-1-remote-access--automated-monitoring--control"></a>AC-17 (1) Accesso remoto | Monitoraggio/Controllo automatico

Questo progetto consente di monitorare e controllare l'accesso remoto assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) per verificare che il debug remoto per l'applicazione del Servizio app di Azure sia disattivato e altre definizioni di criteri per controllare le macchine virtuali Linux che consentono connessioni remote da account senza password. Il progetto assegna anche una definizione di Criteri di Azure per il monitoraggio dell'accesso illimitato agli account di archiviazione. Il monitoraggio di questi indicatori è utile per verificare la conformità dei metodi di accesso remoto ai criteri di sicurezza.

- Mostra i risultati del controllo dalle macchine virtuali Linux che consentono connessioni remote da account senza password
- Distribuisci prerequisiti per controllare le macchine virtuali Linux che consentono connessioni remote da account senza password
- Gli account di archiviazione devono limitare l'accesso alla rete
- Il debug remoto deve essere disattivato per le app per le API
- Il debug remoto deve essere disattivato per le app per le funzioni
- Il debug remoto deve essere disattivato per le applicazioni Web

## <a name="ac-23-data-mining"></a>AC-23 Data mining

Questo progetto garantisce che il controllo e la funzionalità Sicurezza dei dati avanzata siano configurati nei server SQL.

- Sicurezza dei dati avanzata deve essere abilitata nei server SQL
- La soluzione Sicurezza dei dati avanzata deve essere abilitata nell'istanza gestita di SQL
- È consigliabile abilitare il controllo in SQL Server

## <a name="au-3-2-content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>AU-3 (2) Contenuto dei record di controllo | Gestione centralizzata del contenuto dei record di controllo pianificati

I dati del log applicazioni raccolti da Monitoraggio di Azure vengono archiviati in un'area di lavoro Log Analytics per la centralizzazione della configurazione e della gestione. Questo progetto consente di assicurarsi che gli eventi vengano registrati assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che controllano e impongono la distribuzione dell'agente di Log Analytics nelle macchine virtuali di Azure.

- \[Anteprima\]: Controlla la distribuzione dell'agente di Log Analytics - Immagine macchina virtuale (sistema operativo) non in elenco
- Controlla la distribuzione dell'agente di Log Analytics nei set di scalabilità di macchine virtuali - Immagine macchina virtuale (sistema operativo) non in elenco
- Controlla area di lavoro Log Analytics per la macchina virtuale - Segnala mancata corrispondenza
- L'agente di Log Analytics deve essere installato nei set di scalabilità di macchine virtuali
- L'agente di Log Analytics deve essere installato nelle macchine virtuali

## <a name="au-5-response-to-audit-processing-failures"></a>AU-5 Risposta a errori di elaborazione di controllo

Questo progetto assegna definizioni di [Criteri di Azure](../../../policy/overview.md) che monitorano le configurazioni di controllo e registrazione degli eventi. Il monitoraggio di queste configurazioni è indicativo di errori o errate configurazioni del sistema di controllo ed è utile per adottare misure correttive.

- Audit diagnostic setting (Controllare le impostazioni di diagnostica)
- È consigliabile abilitare il controllo in SQL Server
- La soluzione Sicurezza dei dati avanzata deve essere abilitata nell'istanza gestita di SQL
- Sicurezza dei dati avanzata deve essere abilitata nei server SQL

## <a name="au-6-4-audit-review-analysis-and-reporting--central-review-and-analysis"></a>AU-6 (4) Verifica, analisi e report di controllo | Verifica e analisi centralizzate

I dati del log raccolti da Monitoraggio di Azure vengono archiviati in un'area di lavoro Log Analytics per la centralizzazione della creazione di report e dell'analisi. Questo progetto consente di assicurarsi che gli eventi vengano registrati assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che controllano e impongono la distribuzione dell'agente di Log Analytics nelle macchine virtuali di Azure.

- \[Anteprima\]: Controlla la distribuzione dell'agente di Log Analytics - Immagine macchina virtuale (sistema operativo) non in elenco
- Controlla la distribuzione dell'agente di Log Analytics nei set di scalabilità di macchine virtuali - Immagine macchina virtuale (sistema operativo) non in elenco
- Controlla area di lavoro Log Analytics per la macchina virtuale - Segnala mancata corrispondenza

## <a name="au-6-5-audit-review-analysis-and-reporting--integration--scanning-and-monitoring-capabilities"></a>AU-6 (5) Verifica, analisi e report di controllo | Funzionalità di integrazione/analisi e monitoraggio

Questo progetto fornisce le definizioni dei criteri per il controllo dei record con l'analisi della valutazione della vulnerabilità in macchine virtuali, set di scalabilità di macchine virtuali, server di database SQL e server dell'istanza gestita di SQL. Controllano anche la configurazione dei log di diagnostica per fornire informazioni dettagliate sulle operazioni eseguite nelle risorse di Azure. Queste informazioni dettagliate forniscono dati in tempo reale sullo stato di sicurezza delle risorse distribuite e consentono di assegnare priorità alle azioni correttive. Per informazioni dettagliate sull'analisi e sul monitoraggio della vulnerabilità, è consigliabile sfruttare anche Azure Sentinel e il Centro sicurezza di Azure.

- Audit diagnostic setting (Controllare le impostazioni di diagnostica)
- La valutazione della vulnerabilità deve essere abilitata nell'istanza gestita di SQL
- La valutazione delle vulnerabilità deve essere abilitata nei server SQL
- Le vulnerabilità nella configurazione di sicurezza delle macchine devono essere risolte
- Le vulnerabilità dei database SQL devono essere risolte
- Le vulnerabilità devono essere risolte tramite una soluzione di valutazione della vulnerabilità
- Le vulnerabilità nella configurazione di sicurezza dei set di scalabilità di macchine virtuali devono essere risolte
- \[Anteprima\]: Controlla la distribuzione dell'agente di Log Analytics - Immagine macchina virtuale (sistema operativo) non in elenco
- Controlla la distribuzione dell'agente di Log Analytics nei set di scalabilità di macchine virtuali - Immagine macchina virtuale (sistema operativo) non in elenco

## <a name="au-12-audit-generation"></a>AU-12 Generazione di controlli

Questo progetto fornisce le definizioni dei criteri che controllano e impongono la distribuzione dell'agente di Log Analytics nelle macchine virtuali di Azure e la configurazione delle impostazioni di controllo per altri tipi di risorse di Azure.
Controllano anche la configurazione dei log di diagnostica per fornire informazioni dettagliate sulle operazioni eseguite nelle risorse di Azure. Il controllo e la funzionalità Sicurezza dei dati avanzata vengono inoltre configurati nei server SQL.

- \[Anteprima\]: Controlla la distribuzione dell'agente di Log Analytics - Immagine macchina virtuale (sistema operativo) non in elenco
- Controlla la distribuzione dell'agente di Log Analytics nei set di scalabilità di macchine virtuali - Immagine macchina virtuale (sistema operativo) non in elenco
- Controlla area di lavoro Log Analytics per la macchina virtuale - Segnala mancata corrispondenza
- Audit diagnostic setting (Controllare le impostazioni di diagnostica)
- È consigliabile abilitare il controllo in SQL Server
- La soluzione Sicurezza dei dati avanzata deve essere abilitata nell'istanza gestita di SQL
- Sicurezza dei dati avanzata deve essere abilitata nei server SQL

## <a name="au-12-01-audit-generation--system-wide--time-correlated-audit-trail"></a>AU-12 (01) Verifica generazione | Audit trail a livello di sistema/correlato al tempo

Questo progetto consente di assicurarsi che gli eventi di sistema vengano registrati assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che controllano le impostazioni dei log nelle risorse di Azure.
Questo criterio predefinito richiede di specificare una matrice di tipi di risorse per controllare se le impostazioni di diagnostica sono abilitate o meno.

- Audit diagnostic setting (Controllare le impostazioni di diagnostica)

## <a name="cm-7-2-least-functionality--prevent-program-execution"></a>CM-7 (2) Funzionalità minima | Impedire l'esecuzione di programmi

Il controllo applicazione adattivo in Centro sicurezza di Azure è una soluzione per l'inserimento delle applicazioni nell'elenco elementi consentiti end-to-end, intelligente e automatizzata in grado di bloccare o impedire l'esecuzione di software specifico nelle macchine virtuali. Il controllo delle applicazioni può essere eseguito in una modalità di imposizione che impedisce l'esecuzione di applicazioni non approvate. Questo progetto assegna una definizione di Criteri di Azure che consente di monitorare le macchine virtuali quando è consigliato un elenco elementi consentiti che però non è stato ancora configurato.

- I controlli applicazioni adattivi per la definizione delle applicazioni sicure devono essere abilitati nei computer

## <a name="cm-7-5-least-functionality--authorized-software--whitelisting"></a>CM-7 (5) Funzionalità minima | Software autorizzato/elenco elementi consentiti

Il controllo applicazione adattivo in Centro sicurezza di Azure è una soluzione per l'inserimento delle applicazioni nell'elenco elementi consentiti end-to-end, intelligente e automatizzata in grado di bloccare o impedire l'esecuzione di software specifico nelle macchine virtuali. Il controllo delle applicazioni consente di creare elenchi di applicazioni approvate per le macchine virtuali. Questo progetto assegna una definizione di [Criteri di Azure](../../../policy/overview.md) che consente di monitorare le macchine virtuali quando è consigliato un elenco elementi consentiti che però non è stato ancora configurato.

- I controlli applicazioni adattivi per la definizione delle applicazioni sicure devono essere abilitati nei computer

## <a name="cm-11-user-installed-software"></a>CM-11 Software installato dall'utente

Il controllo applicazione adattivo in Centro sicurezza di Azure è una soluzione per l'inserimento delle applicazioni nell'elenco elementi consentiti end-to-end, intelligente e automatizzata in grado di bloccare o impedire l'esecuzione di software specifico nelle macchine virtuali. Il controllo delle applicazioni consente di imporre e monitorare la conformità ai criteri di restrizione software. Questo progetto assegna una definizione di [Criteri di Azure](../../../policy/overview.md) che consente di monitorare le macchine virtuali quando è consigliato un elenco elementi consentiti che però non è stato ancora configurato.

- I controlli applicazioni adattivi per la definizione delle applicazioni sicure devono essere abilitati nei computer

## <a name="cp-7-alternate-processing-site"></a>CP-7 Sito di elaborazione alternativo

Azure Site Recovery consente di replicare carichi di lavoro in esecuzione in macchine da una località primaria a una località secondaria. Se si verifica un'interruzione nel sito primario, viene effettuato il failover del carico di lavoro nella località secondaria. Questo progetto assegna una definizione di [Criteri di Azure](../../../policy/overview.md) per controllare le macchine virtuali senza aver configurato il ripristino di emergenza. Il monitoraggio di questo indicatore è utile per verificare che siano già predisposti i controlli di emergenza necessari.

- Controlla macchine virtuali in cui non è configurato il ripristino di emergenza

## <a name="cp-9-05--information-system-backup--transfer-to-alternate-storage-site"></a>CP-9 (05) Backup del sistema informatico | Trasferimento a un sito di archiviazione alternativo

Questo progetto assegna le definizioni di Criteri di Azure che controllano in modalità elettronica le informazioni di backup del sistema dell'organizzazione nel sito di archiviazione alternativo. Per la spedizione fisica dei metadati di archiviazione, valutare l'utilizzo di Azure Data Box.

- L'archiviazione con ridondanza geografica deve essere abilitata per gli account di archiviazione
- Il backup con ridondanza geografica deve essere abilitato per i database di Azure per PostgreSQL
- Il backup con ridondanza geografica deve essere abilitato per i database di Azure per MySQL
- Il backup con ridondanza geografica a lungo termine deve essere abilitato per i database SQL di Azure

## <a name="ia-2-1-identification-and-authentication-organizational-users--network-access-to-privileged-accounts"></a>IA-2 (1) Identificazione e autenticazione (utenti dell'organizzazione) | Accesso alla rete per gli account con privilegi

Questo progetto consente di limitare e controllare l'accesso con privilegi assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che controllano gli account con autorizzazioni di proprietario e/o di scrittura per cui non è abilitata l'autenticazione a più fattori. L'autenticazione a più fattori consente di mantenere sicuri gli account anche se una parte delle informazioni di autenticazione viene compromessa. Monitorando gli account senza autenticazione a più fattori abilitata, è possibile identificare quelli che potrebbero venire compromessi con più probabilità.

- L'autenticazione MFA deve essere abilitata negli account con autorizzazioni di proprietario per la sottoscrizione
- L'autenticazione MFA deve essere abilitata per gli account con autorizzazioni di scrittura per la sottoscrizione

## <a name="ia-2-2-identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts"></a>IA-2 (2) Identificazione e autenticazione (utenti dell'organizzazione) | Accesso alla rete per gli account senza privilegi

Questo progetto consente di limitare e controllare l'accesso assegnando una definizione di [Criteri di Azure](../../../policy/overview.md) per controllare gli account con autorizzazioni di lettura per cui non è abilitata l'autenticazione a più fattori. L'autenticazione a più fattori consente di mantenere sicuri gli account anche se una parte delle informazioni di autenticazione viene compromessa. Monitorando gli account senza autenticazione a più fattori abilitata, è possibile identificare quelli che potrebbero venire compromessi con più probabilità.

- L'autenticazione MFA deve essere abilitata negli account con autorizzazioni di lettura per la sottoscrizione

## <a name="ia-5-authenticator-management"></a>IA-5 Gestione degli autenticatori

Questo progetto assegna definizioni di [Criteri di Azure](../../../policy/overview.md) che controllano le macchine virtuali Linux che consentono connessioni remote da account senza password e/o per cui sono impostate autorizzazioni errate nel file passwd. Questo progetto assegna anche definizioni di criteri che controllano la configurazione del tipo di crittografia delle password per le macchine virtuali Windows. Il monitoraggio di questi indicatori è utile per verificare che gli autenticatori di sistema siano conformi ai criteri di identificazione e autenticazione dell'organizzazione.

- Mostra i risultati del controllo dalle macchine virtuali Linux in cui le autorizzazioni per il file passwd non sono impostate su 0644
- Mostra i risultati del controllo dalle macchine virtuali Linux in cui sono presenti account senza password
- Mostra i risultati del controllo dalle macchine virtuali Windows che non archiviano le password usando la crittografia reversibile
- Distribuisci i prerequisiti per controllare le macchine virtuali Linux in cui le autorizzazioni per il file passwd non sono impostate su 0644
- Distribuisci i prerequisiti per controllare le macchine virtuali Linux in cui sono presenti account senza password
- Distribuisci i prerequisiti per controllare le macchine virtuali Windows che non archiviano le password usando la crittografia reversibile

## <a name="ia-5-1-authenticator-management--password-based-authentication"></a>IA-5 (1) Gestione autenticatori | Autenticazione basata su password

Questo progetto consente di imporre password complesse assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che controllano le macchine virtuali Windows che non impongono il requisito minimo di complessità o altri requisiti delle password. Identificando le macchine virtuali che violano i criteri di complessità delle password, è possibile adottare azioni correttive per assicurarsi che le password di tutti gli account utente delle macchine virtuali siano conformi ai criteri delle password dell'organizzazione.

- Mostra i risultati del controllo dalle macchine virtuali Windows che consentono il riutilizzo delle 24 password precedenti
- Mostra i risultati del controllo dalle macchine virtuali Windows in cui la validità minima della password non è impostata su 70 giorni
- Mostra i risultati del controllo dalle macchine virtuali Windows in cui la validità minima della password non è impostata su 1 giorno
- Mostra i risultati del controllo dalle macchine virtuali Windows in cui non è abilitata l'impostazione relativa alla complessità della password
- Mostra i risultati del controllo dalle macchine virtuali Windows in cui la lunghezza minima della password non è limitata a 14 caratteri
- Mostra i risultati del controllo dalle macchine virtuali Windows che non archiviano le password usando la crittografia reversibile
- Distribuisci i prerequisiti per controllare le macchine virtuali Windows che consentono il riutilizzo delle 24 password precedenti
- Distribuisci i prerequisiti per controllare le macchine virtuali Windows in cui la validità massima della password non è impostata su 70 giorni
- Distribuisci i prerequisiti per controllare le macchine virtuali Windows in cui la validità minima della password non è impostata su 1 giorno
- Distribuisci i prerequisiti per controllare le macchine virtuali Windows in cui non è abilitata l'impostazione relativa alla complessità della password
- Distribuisci i prerequisiti per controllare le macchine virtuali Windows che non limitano la lunghezza minima della password a 14 caratteri
- Distribuisci i prerequisiti per controllare le macchine virtuali Windows che non archiviano le password usando la crittografia reversibile

## <a name="ir-6-2-incident-reporting--vulnerabilities-related-to-incidents"></a>IR-6 (2) Segnalazione di eventi imprevisti | Vulnerabilità correlate agli eventi imprevisti

Questo progetto fornisce le definizioni dei criteri per il controllo dei record con l'analisi della valutazione della vulnerabilità in macchine virtuali, set di scalabilità di macchine virtuali e server SQL. Queste informazioni dettagliate forniscono dati in tempo reale sullo stato di sicurezza delle risorse distribuite e consentono di assegnare priorità alle azioni correttive.

- Le vulnerabilità nella configurazione di sicurezza dei set di scalabilità di macchine virtuali devono essere risolte
- Le vulnerabilità devono essere risolte tramite una soluzione di valutazione della vulnerabilità
- Le vulnerabilità nella configurazione di sicurezza delle macchine devono essere risolte
- È consigliabile correggere le vulnerabilità nelle configurazioni della sicurezza dei contenitori
- Le vulnerabilità dei database SQL devono essere risolte

## <a name="ra-5-vulnerability-scanning"></a>RA-5 Analisi delle vulnerabilità

Questo progetto consente di gestire le vulnerabilità dei sistemi informativi assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che monitorano le vulnerabilità del sistema operativo, nonché di SQL e delle macchine virtuali nel Centro sicurezza di Azure.
Centro sicurezza di Azure fornisce funzionalità di report che consentono di ricevere informazioni dettagliate in tempo reale sullo stato di sicurezza delle risorse di Azure distribuite. Questo progetto assegna anche definizioni di criteri che controllano e impongono l'uso di Sicurezza dei dati avanzata nei server SQL. Sicurezza dei dati avanzata include le funzionalità Valutazione della vulnerabilità e Advanced Threat Protection che consentono di comprendere le vulnerabilità nelle risorse distribuite.

- La soluzione Sicurezza dei dati avanzata deve essere abilitata nell'istanza gestita di SQL
- Sicurezza dei dati avanzata deve essere abilitata nei server SQL
- Le vulnerabilità nella configurazione di sicurezza dei set di scalabilità di macchine virtuali devono essere risolte
- Le vulnerabilità nella configurazione di sicurezza delle macchine devono essere risolte
- Le vulnerabilità dei database SQL devono essere risolte
- Le vulnerabilità devono essere risolte tramite una soluzione di valutazione della vulnerabilità

## <a name="sc-5-denial-of-service-protection"></a>SC-5 Protezione da attacchi Denial of Service

Il livello Standard per gli attacchi Distributed Denial of Service (DDoS) di Azure offre funzionalità aggiuntive e di mitigazione dei rischi rispetto al livello di servizio Basic. Queste funzionalità aggiuntive includono l'integrazione di Monitoraggio di Azure e la possibilità di esaminar ei report di mitigazione dei rischi dopo gli attacchi. Questo progetto assegna una definizione di [Criteri di Azure](../../../policy/overview.md) per controllare l'abilitazione del livello Standard per gli attacchi Distributed Denial of Service. La comprensione delle differenze nelle funzionalità dei livelli di servizio consente di selezionare la soluzione più adatta per rispondere ad attacchi Denial of Service per l'ambiente Azure.

- La protezione DDoS di Azure Standard deve essere abilitata

## <a name="sc-7-boundary-protection"></a>SC-7 Protezione dei limiti

Questo progetto consente di gestire e controllare i limiti di sistema assegnando una definizione di [Criteri di Azure](../../../policy/overview.md) che monitora l'applicazione delle raccomandazioni sulla protezione avanzata per i gruppi di sicurezza di rete nel Centro sicurezza di Azure. Centro sicurezza di Azure analizza i criteri relativi al traffico di macchine virtuali connesse a Internet e fornisce raccomandazioni sulle regole dei gruppi di sicurezza di rete per ridurre la potenziale superficie di attacco. Questo progetto assegna inoltre definizioni di criteri che monitorano endpoint, applicazioni e account di archiviazione non protetti. Gli endpoint e le applicazioni non protetti da un firewall e gli account di archiviazione con accesso illimitato possono consentire l'accesso non autorizzato alle informazioni contenute nel sistema informativo.

- L'accesso tramite endpoint con connessione Internet deve essere limitato
- Gli account di archiviazione devono limitare l'accesso alla rete

## <a name="sc-7-3-boundary-protection--access-points"></a>SC-7 (3) Protezione dei limiti | Punti di accesso

L'accesso JIT alle macchine virtuali blocca il traffico in ingresso alle macchine virtuali di Azure, riducendo l'esposizione agli attacchi e al tempo stesso offrendo un facile accesso per connettersi alle macchine virtuali quando necessario. L'accesso JIT alle macchine virtuali consente di limitare il numero di connessioni esterne alle risorse in Azure. Questo progetto assegna una definizione di [Criteri di Azure](../../../policy/overview.md) che consente di monitorare le macchine virtuali in grado di supportare l'accesso JIT ma che non sono state ancora configurate.

- Le porte di gestione delle macchine virtuali devono essere protette tramite un controllo di accesso alla rete JIT

## <a name="sc-7-4-boundary-protection--external-telecommunications-services"></a>SC-7 (4) Protezione dei limiti | Servizi di telecomunicazione esterni

L'accesso JIT alle macchine virtuali blocca il traffico in ingresso alle macchine virtuali di Azure, riducendo l'esposizione agli attacchi e al tempo stesso offrendo un facile accesso per connettersi alle macchine virtuali quando necessario. L'accesso JIT alle macchine virtuali consente di gestire le eccezioni ai criteri di flusso del traffico agevolando i processi di richiesta e approvazione degli accessi. Questo progetto assegna una definizione di [Criteri di Azure](../../../policy/overview.md) che consente di monitorare le macchine virtuali in grado di supportare l'accesso JIT ma che non sono state ancora configurate.

- Alle macchine virtuali deve essere applicato il controllo di accesso alla rete JIT

## <a name="sc-8-1-transmission-confidentiality-and-integrity--cryptographic-or-alternate-physical-protection"></a>Riservatezza e integrità delle trasmissioni SC-8 (1) | Protezione crittografica o fisica in alternativa

Questo progetto consente di proteggere la riservatezza e l'integrità delle informazioni trasmesse assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che consentono di monitorare il meccanismo di crittografia implementato per i protocolli di comunicazione. Assicurandosi che le comunicazioni vengano correttamente crittografate, è possibile soddisfare i requisiti dell'organizzazione per la protezione delle informazioni da divulgazione e modifiche non autorizzate.

- L'app per le API deve essere accessibile solo tramite HTTPS
- Mostra i risultati del controllo dai server Web Windows che non usano protocolli di comunicazione sicuri
- Distribuisci i prerequisiti per controllare i server Web Windows che non usano protocolli di comunicazione sicuri
- L'app per le funzioni deve essere accessibile solo tramite HTTPS
- Devono essere abilitate solo le connessioni sicure alla cache di Azure per Redis
- Il trasferimento sicuro negli account di archiviazione deve essere abilitato
- L'applicazione Web deve essere accessibile solo tramite HTTPS

## <a name="sc-28-1-protection-of-information-at-rest--cryptographic-protection"></a>SC-28 (1) Protezione delle informazioni inattive | Protezione crittografica

Questo progetto consente di imporre i criteri sull'uso dei controlli crittografici per la protezione delle informazioni inattive assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che impongono controlli crittografici specifici e controllano l'uso di impostazioni di crittografia meno sicure. Identificando le risorse di Azure le cui configurazioni di crittografia potrebbero non essere ottimali, è possibile adottare azioni correttive per assicurarsi che le risorse siano configurate in conformità ai criteri di sicurezza delle informazioni. In particolare, le definizioni di criteri assegnate da questo progetto richiedono la crittografia per gli account Data Lake Storage, richiedono l'applicazione di Transparent Data Encryption per i database SQL e controllano se manca la crittografia in database SQL, dischi di macchine virtuali e variabili degli account di automazione.

- La soluzione Sicurezza dei dati avanzata deve essere abilitata nell'istanza gestita di SQL
- Sicurezza dei dati avanzata deve essere abilitata nei server SQL
- La crittografia del disco deve essere applicata nelle macchine virtuali
- Transparent Data Encryption deve essere abilitata nei database SQL

## <a name="si-2-flaw-remediation"></a>SI-2 Correzione degli errori

Questo progetto consente di gestire le vulnerabilità dei sistemi informativi assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che monitorano gli aggiornamenti di sistema mancanti, oltre alle vulnerabilità del sistema operativo, di SQL e delle macchine virtuali nel Centro sicurezza di Azure. Centro sicurezza di Azure fornisce funzionalità di report che consentono di ricevere informazioni dettagliate in tempo reale sullo stato di sicurezza delle risorse di Azure distribuite. Questo progetto assegna anche una definizione di criteri che assicura la distribuzione automatica di patch del sistema operativo per i set di scalabilità di macchine virtuali.

- Gli aggiornamenti di sistema nei set di scalabilità di macchine virtuali devono essere installati
- Gli aggiornamenti di sistema devono essere installati nelle macchine
- Le vulnerabilità nella configurazione di sicurezza dei set di scalabilità di macchine virtuali devono essere risolte
- Le vulnerabilità nella configurazione di sicurezza delle macchine devono essere risolte
- Le vulnerabilità dei database SQL devono essere risolte
- Le vulnerabilità devono essere risolte tramite una soluzione di valutazione della vulnerabilità

## <a name="si-02-06-flaw-remediation--removal-of-previous-versions-of-software--firmware"></a>SI-02 (06) Correzione degli errori | Rimozione delle versioni precedenti del software/firmware

Questo progetto assegna le definizioni dei criteri per assicurare che le applicazioni usino la versione più recente di HTTP, Java, PHP, Python e TLS. Questo progetto assegna anche una definizione dei criteri che garantisce che i servizi Kubernetes vengano aggiornati alla relativa versione non vulnerabile.

- Assicurarsi che la 'versione di HTTP' sia la più recente, se usata per eseguire l'app per le API
- Assicurarsi che la 'versione di HTTP' sia la più recente, se usata per eseguire l'app per le funzioni
- Assicurarsi che la 'versione di HTTP' sia la più recente, se usata per eseguire l'app Web
- Assicurarsi che la 'versione di Java' sia la più recente, se usata come parte dell'app per le API
- Assicurarsi che la 'versione di Java' sia la più recente, se usata come parte dell'app per le funzioni
- Assicurarsi che la 'versione di Java' sia la più recente, se usata come parte dell'app Web
- Assicurarsi che la 'versione di PHP' sia la più recente, se usata come parte dell'app per le API
- Assicurarsi che la 'versione di PHP' sia la più recente, se usata come parte dell'app Web
- Assicurarsi che la 'versione di Python' sia la più recente, se usata come parte dell'app per le API
- Assicurarsi che la 'versione di Python' sia la più recente, se usata come parte dell'app per le funzioni
- Assicurarsi che la 'versione di Python' sia la più recente, se usata come parte dell'app Web
- Nell'app per le API è necessario usare la versione più recente di TLS
- Nell'app per le funzioni è necessario usare la versione più recente di TLS
- Nell'app Web è necessario usare la versione più recente di TLS
- I servizi Kubernetes devono essere aggiornati a una versione di Kubernetes non vulnerabile

## <a name="si-3-malicious-code-protection"></a>SI-3 Protezione dal malware

Questo progetto consente di gestire Endpoint Protection, inclusa la protezione dal malware, assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che monitorano la presenza di Endpoint Protection nelle macchine virtuali nel Centro sicurezza di Azure e impongono la soluzione antimalware Microsoft nelle macchine virtuali Windows.

- La soluzione Endpoint Protection deve essere installata nei set di scalabilità di macchine virtuali
- Monitorare il server senza Endpoint Protection nel Centro sicurezza di Azure
- È consigliabile distribuire l'estensione Microsoft IaaSAntimalware nei server Windows

## <a name="si-3-1-malicious-code-protection--central-management"></a>SI-3 (1) Protezione dal malware | Gestione centrale

Questo progetto consente di gestire Endpoint Protection, inclusa la protezione dal malware, assegnando definizioni di [Criteri di Azure](../../../policy/overview.md) che monitorano la presenza di Endpoint Protection nelle macchine virtuali nel Centro sicurezza di Azure. Centro sicurezza di Azure fornisce funzionalità centralizzate di gestione e creazione di report che consentono di ricevere informazioni dettagliate in tempo reale sullo stato di sicurezza delle risorse di Azure distribuite.

- La soluzione Endpoint Protection deve essere installata nei set di scalabilità di macchine virtuali
- Monitorare il server senza Endpoint Protection nel Centro sicurezza di Azure

## <a name="si-4-information-system-monitoring"></a>SI-4 Monitoraggio dei sistemi informativi

Questo progetto consente di monitorare il sistema controllando e imponendo la registrazione e la sicurezza dei dati nelle risorse di Azure. In particolare, i criteri assegnati controllano e impongono la distribuzione dell'agente di Log Analytics, nonché impostazioni di sicurezza avanzata per database SQL, account di archiviazione e risorse di rete. Queste funzionalità consentono di rilevare comportamenti anomali e indicatori di attacchi in modo che sia possibile adottare le misure appropriate.

- \[Anteprima\]: Controlla la distribuzione dell'agente di Log Analytics - Immagine macchina virtuale (sistema operativo) non in elenco
- Controlla la distribuzione dell'agente di Log Analytics nei set di scalabilità di macchine virtuali - Immagine macchina virtuale (sistema operativo) non in elenco
- Controlla area di lavoro Log Analytics per la macchina virtuale - Segnala mancata corrispondenza
- La soluzione Sicurezza dei dati avanzata deve essere abilitata nell'istanza gestita di SQL
- Sicurezza dei dati avanzata deve essere abilitata nei server SQL
- È consigliabile abilitare Network Watcher

## <a name="si-4-12-information-system-monitoring--automated-alerts"></a>SI-4 (12) Monitoraggio dei sistemi informativi | Avvisi automatizzati

Questo progetto assegna le definizioni dei criteri che consentono di assicurarsi che le notifiche di sicurezza siano abilitate correttamente. Inoltre, questo progetto garantisce che per il Centro sicurezza di Azure sia abilitato il piano tariffario Standard. Si noti che il piano tariffario Standard consente il rilevamento delle minacce per reti e macchine virtuali e fornisce informazioni sulle minacce, rilevamento delle anomalie e analisi del comportamento nel Centro sicurezza di Azure.

- È consigliabile abilitare le notifiche di posta elettronica al proprietario della sottoscrizione per gli avvisi con gravità alta
- È consigliabile fornire un indirizzo di posta elettronica dei contatti di sicurezza per la sottoscrizione
- È consigliabile fornire il numero di telefono dei contatti di sicurezza per la sottoscrizione

> [!NOTE]
> La disponibilità di specifiche definizioni di Criteri di Azure può variare in Azure per enti pubblici e in altri cloud nazionali. 

## <a name="next-steps"></a>Passaggi successivi

Dopo aver esaminato il mapping dei controlli del progetto DoD Impact Level 5, vedere gli articoli seguenti per informazioni sul progetto e su come distribuire questo esempio:

> [!div class="nextstepaction"]
> [Progetto DoD Impact Level 5 - Panoramica](./index.md)
> [Progetto DoD Impact Level 5 - Procedura di distribuzione](./deploy.md)

Altri articoli sui progetti e su come usarli:

- Informazioni sul [ciclo di vita del progetto](../../concepts/lifecycle.md).
- Informazioni su come usare [parametri statici e dinamici](../../concepts/parameters.md).
- Informazioni su come personalizzare l'[ordine di sequenziazione del progetto](../../concepts/sequencing-order.md).
- Informazioni su come usare in modo ottimale il [blocco delle risorse del progetto](../../concepts/resource-locking.md).
- Informazioni su come [aggiornare assegnazioni esistenti](../../how-to/update-existing-assignments.md).