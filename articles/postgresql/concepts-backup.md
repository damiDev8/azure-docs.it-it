---
title: Backup e ripristino-database di Azure per PostgreSQL-server singolo
description: Informazioni sui backup automatici e il ripristino del database di Azure per il server PostgreSQL-server singolo.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/29/2021
ms.openlocfilehash: e74c96e0c03d75f34a16d95d0bed642c1900f558
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2021
ms.locfileid: "99219724"
---
# <a name="backup-and-restore-in-azure-database-for-postgresql---single-server"></a>Backup e ripristino nel database di Azure per PostgreSQL-server singolo

Database di Azure per PostgreSQL crea automaticamente backup del server e li archivia in un archivio con ridondanza locale o geografica configurato dall'utente. I backup possono essere usati per ripristinare il server a un momento specifico. Il backup e il ripristino sono una parte essenziale di qualsiasi strategia di continuità aziendale, perché proteggono i dati dal danneggiamento o dall'eliminazione accidentale.

## <a name="backups"></a>Backup

Database di Azure per PostgreSQL esegue il backup dei file di dati e del log delle transazioni. A seconda delle dimensioni massime di archiviazione supportate, è possibile eseguire backup completi e differenziali (server di archiviazione max da 4 TB) o backup di snapshot (fino a 16 TB di server di archiviazione max). Questi backup consentono di ripristinare un server a qualsiasi momento specifico all'interno del periodo di conservazione dei backup configurato. Il periodo di conservazione dei backup predefinito è di sette giorni. Facoltativamente, è possibile configurare fino a 35 giorni. Tutti i backup vengono crittografati con crittografia AES a 256 bit.

Non è possibile esportare i file di backup. I backup possono essere usati solo per le operazioni di ripristino nel database di Azure per PostgreSQL. È possibile utilizzare [pg_dump](howto-migrate-using-dump-and-restore.md) per copiare un database.

### <a name="backup-frequency"></a>Frequenza di backup

#### <a name="servers-with-up-to-4-tb-storage"></a>Server con un massimo di 4 TB di archiviazione

Per i server che supportano fino a 4 TB di spazio di archiviazione massimo, i backup completi vengono eseguiti una volta alla settimana. I backup differenziali si verificano due volte al giorno. I backup del log delle transazioni vengono eseguiti ogni cinque minuti.


#### <a name="servers-with-up-to-16-tb-storage"></a>Server con un massimo di 16 TB di archiviazione

In un subset di [aree di Azure](./concepts-pricing-tiers.md#storage), tutti i server di cui è stato effettuato il provisioning possono supportare fino a 16 TB di archiviazione. I backup su questi server di archiviazione di grandi dimensioni sono basati su snapshot. Il primo backup completo dello snapshot viene pianificato subito dopo la creazione di un server. Il primo backup completo dello snapshot viene mantenuto come backup di base del server. I backup dello snapshot successivi sono solo backup differenziali. I backup differenziali degli snapshot non vengono eseguiti in base a una pianificazione fissa. In un giorno vengono eseguiti tre backup differenziali degli snapshot. I backup del log delle transazioni vengono eseguiti ogni cinque minuti. 

### <a name="backup-retention"></a>Conservazione dei backup

I backup vengono conservati in base all'impostazione del periodo di conservazione dei backup nel server. È possibile selezionare un periodo di conservazione compreso tra 7 e 35 giorni. Il periodo di memorizzazione predefinito è 7 giorni. È possibile impostare il periodo di conservazione durante la creazione del server o in un secondo momento aggiornando la configurazione di backup usando [portale di Azure](./howto-restore-server-portal.md#set-backup-configuration) o l' [interfaccia](./howto-restore-server-cli.md#set-backup-configuration)della riga di comando 

Il periodo di conservazione dei backup determina quanto è possibile tornare indietro nel tempo con un ripristino temporizzato, essendo il ripristino basato sui backup disponibili. Il periodo di conservazione dei backup può essere trattato anche come una finestra di ripristino da una prospettiva di ripristino. Tutti i backup necessari per eseguire un ripristino temporizzato entro il periodo di conservazione dei backup vengono conservati nell'archivio di backup. Ad esempio, se il periodo di conservazione dei backup è impostato su 7 giorni, la finestra di ripristino viene considerata gli ultimi 7 giorni. In questo scenario vengono conservati tutti i backup necessari per ripristinare il server negli ultimi 7 giorni. Con un intervallo di conservazione dei backup di sette giorni:
- I server con archiviazione fino a 4 TB manterranno fino a 2 backup completi del database, tutti i backup differenziali e i backup del log delle transazioni eseguiti dopo il primo backup completo del database.
-   I server con archiviazione fino a 16 TB manterranno lo snapshot completo del database, tutti gli snapshot differenziali e i backup del log delle transazioni negli ultimi 8 giorni.

### <a name="backup-redundancy-options"></a>Opzioni di ridondanza per il backup

Nei livelli Utilizzo generico e Con ottimizzazione per la memoria, Database di Azure per PostgreSQL offre la possibilità di scegliere tra archiviazione dei backup con ridondanza locale o con ridondanza geografica. Quando vengono archiviati in un archivio di backup con ridondanza geografica, i backup non solo vengono archiviati nell'area in cui è ospitato il server, ma vengono anche replicati in un [data center abbinato](../best-practices-availability-paired-regions.md). Questo garantisce una migliore protezione e la possibilità di ripristinare il server in un'area diversa in caso di emergenza. Il livello Basic offre solo l'archiviazione dei backup con ridondanza locale.

> [!IMPORTANT]
> La configurazione dell'archiviazione con ridondanza locale o geografica per il backup è consentita solo durante la creazione del server. Dopo il provisioning del server, non è possibile modificare l'opzione di ridondanza per l'archivio di backup.

### <a name="backup-storage-cost"></a>Costo dell'archiviazione dei backup

Database di Azure per PostgreSQL offre fino al 100% delle risorse di archiviazione del server di cui è stato effettuato il provisioning come archivio di backup senza costi aggiuntivi. Ogni ulteriore spazio di archiviazione di backup utilizzato viene addebitato in GB al mese. Se, ad esempio, è stato effettuato il provisioning di un server con 250 GB di spazio di archiviazione, sono disponibili 250 GB di spazio di archiviazione aggiuntivo per i backup del server senza costi aggiuntivi. Lo spazio di archiviazione utilizzato per i backup con più di 250 GB viene addebitato in base al [modello di determinazione prezzi](https://azure.microsoft.com/pricing/details/postgresql/).

È possibile usare la metrica di [archiviazione di backup utilizzata](concepts-monitoring.md) in monitoraggio di Azure disponibile nel portale di Azure per monitorare l'archiviazione di backup utilizzata da un server. La metrica di archiviazione di backup utilizzata rappresenta la somma dello spazio di archiviazione utilizzato da tutti i backup completi del database, backup differenziali e backup del log mantenuti in base al periodo di conservazione dei backup impostato per il server. La frequenza dei backup è gestita dal servizio e illustrata in precedenza. Un'intensa attività transazionale sul server può causare un aumento dell'uso dell'archivio di backup indipendentemente dalle dimensioni totali del database. Per l'archiviazione con ridondanza geografica, l'utilizzo dell'archiviazione di backup è due volte quello dell'archiviazione con ridondanza locale. 

Il modo principale per controllare i costi di archiviazione dei backup consiste nell'impostare il periodo di conservazione dei backup appropriato e scegliere le opzioni di ridondanza di backup corrette per soddisfare gli obiettivi di ripristino desiderati. È possibile selezionare un periodo di conservazione compreso tra 7 e 35 giorni. I server per utilizzo generico e con ottimizzazione per la memoria possono scegliere di disporre di archiviazione con ridondanza geografica per i backup.

## <a name="restore"></a>Restore

In Database di Azure per PostgreSQL, l'esecuzione di un ripristino crea un nuovo server dai backup del server originale. 

Sono disponibili due tipi di ripristino:

- Il **ripristino temporizzato** è disponibile con entrambe le opzioni di ridondanza dei backup e crea un nuovo server nella stessa area del server originale.
- Il **ripristino geografico** è disponibile solo se il server è stato configurato per l'archiviazione con ridondanza geografica e consente di ripristinare il server in un'area diversa.

Il tempo stimato per il ripristino dipende da diversi fattori, tra cui le dimensioni dei database, le dimensioni dei log delle transazioni, la larghezza di banda di rete e il numero totale di database ripristinati contemporaneamente nella stessa area. Il tempo di recupero di solito è inferiore a 12 ore.

> [!NOTE] 
> Se il server PostgreSQL di origine è crittografato con le chiavi gestite dal cliente, consultare la [documentazione](concepts-data-encryption-postgresql.md) per ulteriori considerazioni. 

> [!NOTE]
> Se vuoi ripristinare un server PostgreSQL eliminato, segui la procedura descritta [qui](howto-restore-dropped-server.md).

### <a name="point-in-time-restore"></a>Ripristino temporizzato

Indipendentemente dall'opzione di ridondanza per il backup scelta, è possibile eseguire il ripristino a qualsiasi momento specifico all'interno del periodo di conservazione dei backup. Verrà creato un nuovo server nella stessa area di Azure del server originale, nonché con la stessa configurazione in termini di piano tariffario, generazione di calcolo, numero di vCore, dimensioni di archiviazione, periodo di conservazione dei backup e opzione di ridondanza per il backup.

Il ripristino temporizzato è utile in più scenari, ad esempio quando un utente per errore elimina dati o rimuove un database o una tabella importante oppure se un'applicazione sovrascrive accidentalmente dati corretti con dati non validi a causa di un difetto dell'applicazione.

Per poter eseguire il ripristino a un momento specifico negli ultimi cinque minuti, potrebbe essere prima necessario attendere l'esecuzione del backup del log delle transazioni successivo.

Se si desidera ripristinare una tabella eliminata, 
1. Ripristinare il server di origine utilizzando un metodo temporizzato.
2. Esegui il dump della tabella utilizzando `pg_dump` dal server ripristinato.
3. Rinominare la tabella di origine nel server originale.
4. Importa la tabella usando la riga di comando di PSQL nel server originale.
5. Facoltativamente, è possibile eliminare il server ripristinato.

>[!Note]
> Si consiglia di non creare più ripristini per lo stesso server nello stesso momento. 

### <a name="geo-restore"></a>Ripristino geografico

Se il server è stato configurato per backup con ridondanza geografica, è possibile ripristinare un server in un'altra area di Azure in cui il servizio è disponibile. I server che supportano fino a 4 TB di spazio di archiviazione possono essere ripristinati nell'area geografica abbinata o in qualsiasi area che supporta fino a 16 TB di spazio di archiviazione. Per i server che supportano fino a 16 TB di spazio di archiviazione, è possibile ripristinare i backup geografici in qualsiasi area che supporti i server a 16 TB. Esaminare i [piani tariffari di database di Azure per PostgreSQL](concepts-pricing-tiers.md) per l'elenco delle aree supportate.

Il ripristino geografico è l'opzione di ripristino predefinita quando il server non è disponibile a causa di un evento imprevisto nell'area in cui è ospitato. Se un evento imprevisto su larga scala determina la mancata disponibilità dell'applicazione di database, è possibile ripristinare un server dai backup con ridondanza geografica in un server in un'altra area. Esiste un ritardo tra il momento in cui un backup viene creato e quando ne viene eseguita la replica in un'area diversa. Questo ritardo può essere al massimo di un'ora, quindi, in caso di emergenza, può verificarsi una perdita massima di un'ora di dati.

Durante il ripristino geografico è possibile modificare le seguenti opzioni relative alle configurazioni del server: generazione delle risorse di calcolo, vCore, periodo di conservazione dei backup e ridondanza per il backup. La modifica del piano tariffario (Basic, Utilizzo generico oppure Ottimizzato per la memoria) o delle dimensioni della risorsa di archiviazione non è supportata.

> [!NOTE]
> Se nel server di origine viene utilizzata la crittografia a doppia infrastruttura, per il ripristino del server sono presenti limitazioni che includono le aree disponibili. Per altri dettagli, vedere la [crittografia a doppia infrastruttura](concepts-infrastructure-double-encryption.md) .

### <a name="perform-post-restore-tasks"></a>Eseguire le attività post-ripristino

Dopo il ripristino con uno dei due meccanismi, per rendere nuovamente operativi gli utenti e le applicazioni è consigliabile eseguire queste attività:

- Se il nuovo server ha lo scopo di sostituire il server originale, reindirizzare i client e le applicazioni client al nuovo server. Modificare anche il nome utente in `username@new-restored-server-name` .
- Assicurarsi che le regole del firewall a livello di server e del VNet siano appropriate per consentire agli utenti di connettersi. Queste regole non vengono copiate dal server originale.
- Verificare che siano presenti gli account di accesso e le autorizzazioni a livello di database appropriati
- Configurare gli avvisi in base alle proprie esigenze.

## <a name="next-steps"></a>Passaggi successivi

- Informazioni su come eseguire il ripristino usando [il portale di Azure](howto-restore-server-portal.md).
- Informazioni su come eseguire il ripristino usando [l'interfaccia della](howto-restore-server-cli.md)riga di comando di Azure.
- Per altre informazioni sulla continuità aziendale, vedere la relativa  [panoramica](concepts-business-continuity.md).