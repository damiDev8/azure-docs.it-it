---
title: Gestire i log per un cluster HDInsight - Azure HDInsight
description: Determinare i tipi, le dimensioni e i criteri di conservazione per i file di log attività di HDInsight.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 02/05/2020
ms.openlocfilehash: 0a6e837284917129bb56c6230e68927b79e95dac
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/28/2021
ms.locfileid: "98945267"
---
# <a name="manage-logs-for-an-hdinsight-cluster"></a>Gestire i log per un cluster HDInsight

Un cluster HDInsight genera svariati file di log. Apache Hadoop e i servizi correlati, come Apache Spark, generano ad esempio log di esecuzione dei processi dettagliati. La gestione dei file di log è una delle attività che consentono di mantenere integro un cluster HDInsight. Possono anche esistere requisiti normativi per l'archiviazione dei log.  A causa del numero e delle dimensioni dei file di log, ottimizzando la memorizzazione e l'archiviazione dei log è possibile semplificare la gestione dei costi dei servizi.

La gestione dei log cluster di HDInsight include la conservazione delle informazioni su tutti gli aspetti dell'ambiente cluster, che comprendono tutti i log dei servizi di Azure associati, la configurazione cluster, le informazioni sull'esecuzione dei processi, eventuali stati di errore e altri dati necessari.

I passaggi tipici della gestione dei log di HDInsight sono:

* Passaggio 1: Determinare i criteri di conservazione dei log
* Passaggio 2: Gestire i log di configurazione delle versioni dei servizi cluster
* Passaggio 3: Gestire i file di log di esecuzione dei processi cluster
* Passaggio 4: Prevedere le dimensioni e i costi di archiviazione dei volumi di log
* Passaggio 5: Determinare i criteri e i processi di archiviazione dei log

## <a name="step-1-determine-log-retention-policies"></a>Passaggio 1: Determinare i criteri di conservazione dei log

Il primo passaggio per creare una strategia di gestione dei registri cluster di HDInsight prevede la raccolta di informazioni sugli scenari aziendali e sui requisiti di archiviazione della cronologia di esecuzione dei processi.

### <a name="cluster-details"></a>Dettagli dei cluster

I dettagli dei cluster seguenti sono utili per raccogliere informazioni nell'ambito della strategia di gestione dei log. Raccogliere queste informazioni da tutti i cluster HDInsight creati in un determinato account Azure.

* Nome cluster
* Area del cluster e zona di disponibilità di Azure
* Stato del cluster, inclusi i dettagli dell'ultima modifica dello stato
* Tipo e numero di istanze di HDInsight specificate per i nodi master, principali e attività

È possibile ottenere la maggior parte di queste informazioni generali usando il portale di Azure.  In alternativa, è possibile usare l'interfaccia della riga di comando di [Azure](/cli/azure/) per ottenere informazioni sui cluster HDInsight:

```azurecli
az hdinsight list --resource-group <ResourceGroup>
az hdinsight show --resource-group <ResourceGroup> --name <ClusterName>
```

Per visualizzare queste informazioni, è anche possibile usare PowerShell.  Per altre informazioni, vedere [Gestire cluster Apache Hadoop in HDInsight usando Azure PowerShell](hdinsight-administer-use-powershell.md).

### <a name="understand-the-workloads-running-on-your-clusters"></a>Informazioni sui carichi di lavoro in esecuzione nei cluster

È importante conoscere i tipi di carichi di lavoro in esecuzione nei cluster HDInsight per progettare strategie di registrazione appropriate per ogni tipo.

* I carichi di lavoro sono sperimentali (ad esempio, sviluppo o test) o di qualità idonea ad ambienti di produzione?
* Con quale frequenza vengono in genere eseguiti i carichi di lavoro di qualità idonea ad ambienti di produzione?
* I carichi di lavoro sono a elevato utilizzo di risorse e/o a esecuzione prolungata?
* I carichi di lavoro usano un set complesso di servizi Hadoop per cui vengono generati più tipi di log?
* Ai carichi di lavoro sono associati requisiti normativi di derivazione dell'esecuzione?

### <a name="example-log-retention-patterns-and-practices"></a>Modelli e procedure di conservazione dei log di esempio

* Prendere in considerazione la possibilità di gestire la verifica della derivazione dei dati aggiungendo un identificatore a ogni voce di log o con altre tecniche. Ciò consente di risalire all'origine dei dati e dell'operazione e di seguire i dati attraverso ogni fase per conoscerne la coerenza e la validità.

* Considerare come sia possibile raccogliere i log dal cluster o da più di un cluster e collazionarli, ad esempio a scopo di controllo, monitoraggio, pianificazione e creazione di avvisi. È possibile usare una soluzione personalizzata per accedere ai file di log e scaricarli a intervalli regolari e per combinarli e analizzarli per poter fornire una visualizzazione dashboard. È anche possibile aggiungere altre funzionalità per creare avvisi per la sicurezza o il rilevamento di errori. È possibile compilare queste utilità con PowerShell, gli SDK di HDInsight o il codice che accede al modello di distribuzione classica di Azure.

* Stabilire se una soluzione o un servizio di monitoraggio possa essere vantaggioso. In Microsoft System Center è disponibile un [Management Pack per HDInsight](https://systemcenter.wiki/?Get_ManagementPackBundle=Microsoft.HDInsight.mpb&FileMD5=10C7D975C6096FFAA22C84626D211259). Per raccogliere e centralizzare i log è anche possibile usare strumenti di terze parti, ad esempio Apache Chukwa e Ganglia. Molte aziende offrono servizi per il monitoraggio di soluzioni Big Data basate su Hadoop, ad esempio: Centerity, Compuware APM, Sematext SPM e agente di orchestrazione Zettaset.

## <a name="step-2-manage-cluster-service-versions-and-view-logs"></a>Passaggio 2: gestire le versioni dei servizi cluster e visualizzare i log

Un tipico cluster HDInsight usa diversi servizi e pacchetti software open source (ad esempio, Apache HBase, Apache Spark e così via). Per alcuni carichi di lavoro, ad esempio quelli di bioinformatica, potrebbe essere necessario conservare la cronologia dei log di configurazione dei servizi oltre ai log di esecuzione dei processi.

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Visualizzare le impostazioni di configurazione cluster con l'interfaccia utente di Ambari

Apache Ambari semplifica la gestione, la configurazione e il monitoraggio di un cluster HDInsight grazie a un'interfaccia utente Web e a un'API REST. Ambari è incluso nei cluster HDInsight basati su Linux. Selezionare il riquadro **Dashboard cluster** nella pagina portale di Azure HDInsight per aprire la pagina di collegamento **Dashboard cluster** .  Selezionare quindi il riquadro **Dashboard cluster HDInsight** per aprire l'interfaccia utente di Ambari.  Verranno richieste le credenziali di accesso del cluster.

Per aprire un elenco di visualizzazioni di servizi, selezionare il riquadro **Visualizzazioni di Ambari** nella pagina del portale di Azure per HDInsight.  Questo elenco varia a seconda delle librerie installate.  È ad esempio possibile visualizzare YARN Queue Manager, Hive View e Tez View.  Selezionare un collegamento al servizio per visualizzare le informazioni sulla configurazione e sul servizio.  La pagina **Stack and Version** (Stack e versione) dell'interfaccia utente di Ambari contiene informazioni sulla configurazione dei servizi cluster e la cronologia delle versioni dei servizi. Per passare a questa sezione dell'interfaccia utente di Ambari, scegliere **Stacks and Versions** (Stack e versioni) dal menu **Admin** (Amministratore).  Selezionare la scheda **Versions** (Versioni) per visualizzare le informazioni sulle versioni dei servizi.

![Stack e versioni di amministrazione di Apache Ambari](./media/hdinsight-log-management/ambari-stack-versions.png)

Usando l'interfaccia utente di Ambari, è possibile scaricare la configurazione per qualsiasi servizio (o per tutti) in esecuzione in un determinato host (o nodo) del cluster.  Scegliere il collegamento dell'host a cui si è interessati dal menu **Hosts** (Host). Nella pagina di tale host selezionare il pulsante **Host Actions** (Azioni host) e quindi **Download Client Configs** (Scarica configurazioni client).

![Apache Ambari scaricare le configurazioni del client host](./media/hdinsight-log-management/download-client-configs.png)

### <a name="view-the-script-action-logs"></a>Visualizzare i log delle azioni script

Le [azioni script](hdinsight-hadoop-customize-cluster-linux.md) di HDInsight eseguono script in un cluster, manualmente o se specificato. Le azioni script, ad esempio, possono essere usate per installare software aggiuntivo nel cluster o modificare i valori predefiniti delle impostazioni di configurazione. I log delle azioni script forniscono informazioni sugli errori verificatisi durante la configurazione del cluster e anche sulle modifiche delle impostazioni di configurazione che possono avere effetto sulle prestazioni e sulla disponibilità del cluster.  Per visualizzare lo stato di un'azione script, selezionare il pulsante **ops** (operazioni) nell'interfaccia utente di Ambari o accedere ai log di stato nell'account di archiviazione predefinito. I log di archiviazione sono disponibili in `/STORAGE_ACCOUNT_NAME/DEFAULT_CONTAINER_NAME/custom-scriptaction-logs/CLUSTER_NAME/DATE`.

### <a name="view-ambari-alerts-status-logs"></a>Visualizzare i log di stato degli avvisi di Ambari

Apache Ambari scrive le modifiche dello stato dell'avviso in `ambari-alerts.log` . Il percorso completo è `/var/log/ambari-server/ambari-alerts.log` . Per abilitare il debug per il log, modificare una proprietà in `/etc/ambari-server/conf/log4j.properties.` modifica quindi voce in `# Log alert state changes` da:

```
log4j.logger.alerts=INFO,alerts

to

log4j.logger.alerts=DEBUG,alerts
```

## <a name="step-3-manage-the-cluster-job-execution-log-files"></a>Passaggio 3: Gestire i file di log di esecuzione dei processi cluster

Il passaggio successivo prevede la revisione dei file di log di esecuzione dei processi per i diversi servizi.  I servizi possono includere Apache HBase, Apache Spark e molti altri. Un cluster Hadoop produce un numero elevato di log dettagliati, quindi determinare quali log sono utili (e quali non sono) può richiedere molto tempo.  Conoscere il sistema di registrazione è importante per la gestione mirata dei file di log.  L'immagine seguente è un file di log di esempio.

![Esempio di output di esempio del file di log di HDInsight](./media/hdinsight-log-management/hdi-log-file-example.png)

### <a name="access-the-hadoop-log-files"></a>Accedere ai file di log di Hadoop

HDInsight archivia i file di log sia nel cluster file system che in archiviazione di Azure. È possibile esaminare i file di log nel cluster aprendo una connessione [SSH](hdinsight-hadoop-linux-use-ssh-unix.md) al cluster ed esplorando il file System oppure usando il portale di stato di Hadoop Yarn sul server del nodo head remoto. È possibile esaminare i file di log in archiviazione di Azure usando uno degli strumenti che possono accedere ai dati e scaricarli da archiviazione di Azure. Esempi sono [AzCopy](../storage/common/storage-use-azcopy-v10.md), [CloudXplorer](https://clumsyleaf.com/products/cloudxplorer)e Visual Studio Esplora server. È anche possibile usare PowerShell e le librerie client di Archiviazione di Azure o gli SDK di Azure .NET per accedere ai dati nell'archivio BLOB di Azure.

Hadoop esegue i processi come *tentativi di attività* in diversi nodi del cluster. HDInsight può avviare tentativi di attività speculativi, terminando eventuali altri tentativi di attività che non vengono completati per primi. Viene così generata una significativa attività che viene immediatamente registrata nei file di log del controller, di stderr e di syslog. Vengono inoltre eseguiti simultaneamente più tentativi di attività, ma un file di log può visualizzare i risultati solo in modo lineare.

#### <a name="hdinsight-logs-written-to-azure-blob-storage"></a>Log di HDInsight scritti nell'archivio BLOB di Azure

I cluster HDInsight sono configurati per scrivere i log delle attività in un account di archiviazione BLOB di Azure per qualsiasi processo inviato usando i cmdlet di Azure PowerShell o le API per l'invio di processi tramite .NET.  Se si inviano i processi tramite SSH al cluster, le informazioni di registrazione di esecuzione vengono archiviate nelle tabelle di Azure, come illustrato nella sezione precedente.

Oltre ai file di log principali generati da HDInsight, anche i servizi installati, ad esempio YARN, generano file di log di esecuzione dei processi.  Il numero e il tipo di file di log dipendono dai servizi installati.  Servizi comuni sono Apache HBase, Apache Spark e così via.  Esaminare i file di log di esecuzione dei processi di ogni servizio per comprendere i file di registrazione completa nel cluster.  Ogni servizio ha metodi di registrazione univoci e percorsi univoci per l'archiviazione dei file di log.  Come esempio, nella sezione seguente vengono illustrati i dettagli per accedere ai file di log dei servizi più comuni (da YARN).

### <a name="hdinsight-logs-generated-by-yarn"></a>Log di HDInsight generati da YARN

YARN aggrega i log di tutti i contenitori in un nodo di lavoro e archivia tali log come file di log aggregati per ogni nodo di lavoro. Quando un'applicazione termina, tale log viene archiviato nel file system predefinito. L'applicazione può usare centinaia o migliaia di contenitori, ma i log di tutti i contenitori eseguiti su un singolo nodo di lavoro vengono sempre aggregati in un unico file. È disponibile un solo log per ogni nodo di lavoro usato dall'applicazione. La funzione di aggregazione dei log è abilitata per impostazione predefinita nei cluster HDInsight versione 3.0 o successiva. I log aggregati sono disponibili nella risorsa di archiviazione predefinita per il cluster.

```
/app-logs/<user>/logs/<applicationId>
```

I log aggregati non sono leggibili direttamente, perché sono scritti in un formato binario oggetto tfile indicizzato dal contenitore. Usare i log di YARN ResourceManager o gli strumenti dell'interfaccia della riga di comando per visualizzare i log come testo normale per le applicazioni o i contenitori di interesse.

#### <a name="yarn-cli-tools"></a>Strumenti dell’interfaccia di riga di comando YARN

Per utilizzare gli strumenti dell'interfaccia della riga di comando YARN, è innanzitutto necessario connettersi al cluster HDInsight tramite SSH. Specificare le informazioni `<applicationId>`, `<user-who-started-the-application>`, `<containerId>` e `<worker-node-address>` quando si eseguono questi comandi. È possibile visualizzare i log come testo normale con uno dei comandi seguenti:

```bash
yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>
```

#### <a name="yarn-resourcemanager-ui"></a>Interfaccia utente di ResourceManager YARN

L'interfaccia utente di YARN ResourceManager viene eseguita sul nodo head del cluster ed è accessibile tramite l'interfaccia utente Web Ambari. Per visualizzare i log di YARN, procedere come segue:

1. In un Web browser passare a `https://CLUSTERNAME.azurehdinsight.net`. Sostituire CLUSTERNAME con il nome del cluster HDInsight.
2. Nell'elenco dei servizi a sinistra selezionare YARN.
3. Dall'elenco a discesa Quick Links (Collegamenti rapidi) selezionare uno dei nodi head del cluster e quindi **ResourceManager logs** (Log di ResourceManager). Viene visualizzato un elenco di collegamenti ai log YARN.

## <a name="step-4-forecast-log-volume-storage-sizes-and-costs"></a>Passaggio 4: Prevedere le dimensioni e i costi di archiviazione dei volumi di log

Dopo avere completato i passaggi precedenti, si è a conoscenza dei tipi e dei volumi di file di log generati dai cluster HDInsight.

Analizzare ora il volume dei dati dei log nelle posizioni di archiviazione chiave per un periodo di tempo. È ad esempio possibile analizzare il volume e la crescita per periodi di 30, 60 e 90 giorni.  Registrare queste informazioni in un foglio di calcolo o usare altri strumenti, ad esempio Visual Studio, Azure Storage Explorer o Power Query per Excel. ```

Ora si hanno informazioni sufficienti per creare una strategia di gestione per i log chiave.  Usare il foglio di calcolo (o lo strumento scelto) per valutare sia la crescita delle dimensioni dei log che i costi dei servizi di Azure per l'archiviazione dei log in futuro.  Prendere in considerazione anche eventuali requisiti di conservazione dei log per il set di log che si sta esaminando.  È ora possibile riprevedere i costi di archiviazione dei log futuri, dopo aver determinato i file di registro che possono essere eliminati (se presenti) e quali log devono essere conservati e archiviati in archiviazione di Azure meno costosa.

## <a name="step-5-determine-log-archive-policies-and-processes"></a>Passaggio 5: Determinare i criteri e i processi di archiviazione dei log

Dopo avere determinato quali file di log possono essere eliminati, è possibile modificare i parametri di registrazione in molti servizi Hadoop per eliminare automaticamente i file di log dopo un periodo di tempo specificato.

Per determinati file di log, è possibile usare un approccio di archiviazione di prezzo inferiore. Per Azure Resource Manager i log attività, è possibile esplorare questo approccio usando il portale di Azure.  Configurare l'archiviazione dei log Gestione risorse selezionando il collegamento **log attività** nel portale di Azure per l'istanza di HDInsight.  Nella parte superiore della pagina di ricerca Log attività scegliere la voce di menu **Esporta** per aprire il riquadro **Esporta log attività**.  Specificare la sottoscrizione, l'area, se eseguire l'esportazione in un account di archiviazione e per quanti giorni conservare i log. In questo stesso riquadro è anche possibile indicare se eseguire l'esportazione in un hub eventi.

![Anteprima del log attività esportazione portale di Azure](./media/hdinsight-log-management/hdi-export-log-files.png)

In alternativa, è possibile generare uno script per archiviare i log con PowerShell.  Per uno script di PowerShell di esempio, vedere [Archive Azure Automation logs to Azure Blob Storage](https://gallery.technet.microsoft.com/scriptcenter/Archive-Azure-Automation-898a1aa8) (Archiviare i log di Automazione di Azure in Archiviazione BLOB di Azure).

### <a name="accessing-azure-storage-metrics"></a>Accesso alle metriche di archiviazione di Azure

Archiviazione di Azure può essere configurato per registrare le operazioni di archiviazione e l'accesso. È possibile usare questi log estremamente dettagliati per monitorare e pianificare la capacità e per controllare le richieste alla risorsa di archiviazione. Le informazioni registrate includono i dettagli sulla latenza, che consentono di monitorare e ottimizzare le prestazioni delle soluzioni.
È possibile usare .NET SDK per Hadoop per esaminare i file di log generati per l'archiviazione di Azure che include i dati per un cluster HDInsight.

### <a name="control-the-size-and-number-of-backup-indexes-for-old-log-files"></a>Controllare le dimensioni e il numero di indici di backup per i file di log obsoleti

Per controllare le dimensioni e il numero di file di log conservati, impostare le proprietà seguenti di `RollingFileAppender`:

* `maxFileSize` indica le dimensioni critiche del file, oltre le quali il file viene aggiornato. Il valore predefinito è 10 MB.
* `maxBackupIndex` specifica il numero di file di backup da creare. Il valore predefinito è 1.

### <a name="other-log-management-techniques"></a>Altre tecniche di gestione dei log

Per evitare di esaurire lo spazio su disco, è possibile usare alcuni strumenti del sistema operativo, ad esempio [logrotate](https://linux.die.net/man/8/logrotate) , per gestire la gestione dei file di log. È possibile configurare `logrotate` per l'esecuzione giornaliera, in cui vengono compressi i file di log e vengono rimossi quelli obsoleti. L'approccio dipende dai requisiti, ad esempio per quanto tempo conservare i file di log nei nodi locali.  

È anche possibile controllare se per uno o più servizi è abilitata la registrazione di DEBUG, che aumenta considerevolmente le dimensioni del log di output.  

Per raccogliere i log da tutti i nodi in una posizione centrale, è possibile creare un flusso di dati, ad esempio inserendo tutte le voci di log in Solr.

## <a name="next-steps"></a>Passaggi successivi

* [Monitoring and Logging Practice for HDInsight (Procedura di monitoraggio e registrazione per HDInsight)](/previous-versions/msp-n-p/dn749790(v=pandp.10))
* [Accedere ai log applicazioni di Apache Hadoop YARN in HDInsight basato su Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [How to control size of log files for various Apache Hadoop components](https://community.hortonworks.com/articles/8882/how-to-control-size-of-log-files-for-various-hdp-c.html) (Come controllare le dimensioni dei file di log per diversi componenti Apache Hadoop)
