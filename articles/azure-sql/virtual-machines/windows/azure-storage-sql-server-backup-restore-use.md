---
title: Come usare archiviazione di Azure per il backup e il ripristino SQL Server | Microsoft Docs
description: Informazioni su come eseguire il backup di SQL Server in Archiviazione di Azure. Vengono illustrati i vantaggi del backup dei database SQL in Archiviazione di Azure.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
tags: azure-service-management
ms.assetid: 0db7667d-ef63-4e2b-bd4d-574802090f8b
ms.service: virtual-machines-sql
ms.subservice: backup
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: mathoma
ms.openlocfilehash: 35fff49a53f5a0a9532fd0dff841356c5deaf3ea
ms.sourcegitcommit: a4533b9d3d4cd6bb6faf92dd91c2c3e1f98ab86a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/22/2020
ms.locfileid: "97724783"
---
# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Usare archiviazione di Azure per il backup e il ripristino SQL Server
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

A partire da SQL Server 2012 SP1 CU2, è ora possibile scrivere backup SQL Server database direttamente nell'archivio BLOB di Azure. Usare questa funzionalità per eseguire il backup e il ripristino dall'archiviazione BLOB di Azure. Il backup nel cloud offre i vantaggi della disponibilità, l'archiviazione fuori sede con replica geografica illimitata e la facilità di migrazione dei dati da e verso il cloud. È possibile rilasciare `BACKUP` `RESTORE` istruzioni o utilizzando Transact-SQL o Smo.

## <a name="overview"></a>Panoramica
In SQL Server 2016 sono disponibili nuove funzionalità. È possibile usare il [backup di snapshot di file](/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure) per eseguire backup quasi istantanei e ripristini estremamente rapidi.

Questo argomento illustra il motivo per cui è possibile scegliere di usare archiviazione di Azure per SQL Server backup e quindi descrive i componenti interessati. È possibile usare le risorse fornite alla fine dell'articolo per accedere a procedure dettagliate e informazioni aggiuntive per iniziare a usare questo servizio con i backup del SQL Server.

## <a name="benefits-of-using-azure-blob-storage-for-sql-server-backups"></a>Vantaggi dell'uso dell'archiviazione BLOB di Azure per i backup di SQL Server
Quando si eseguono backup di SQL Server, è necessario affrontare diverse problematiche, inclusi la gestione dell'archiviazione, i rischi correlati agli errori di archiviazione, l'accesso all'archiviazione fuori sede e la configurazione dell'hardware. Molti di questi problemi vengono risolti tramite l'archiviazione BLOB di Azure per i backup SQL Server. Prendere in considerazione i vantaggi seguenti:

* **Semplicità d'uso**: l'archiviazione dei backup nel servizio BLOB di Azure è un'opzione di archiviazione fuori sede pratica, flessibile e di facile accesso. La creazione di un sistema di archiviazione fuori sede per i backup di SQL Server è estremamente semplice ed è realizzabile modificando gli script e i processi esistenti in modo da usare la sintassi **BACKUP TO URL** . Generalmente, l'archiviazione in una posizione esterna deve essere sufficientemente lontana dalla posizione del database di produzione per impedire che una singola situazione di emergenza possa influire sia sulla posizione esterna sia su quella del database di produzione. Scegliendo di eseguire la [replica geografica dei BLOB di Azure](../../../storage/common/storage-redundancy.md), si otterrà un livello aggiuntivo di protezione nel caso in cui si verifichi un'emergenza che potrebbe interessare l'intera area.
* **Archivio di backup**: archiviazione BLOB di Azure offre un'alternativa migliore rispetto all'opzione del nastro spesso usata per archiviare i backup. L'archiviazione su nastro può richiedere il trasporto fisico alla struttura fuori sede e l'adozione di determinate misure per la protezione dei supporti. L'archiviazione dei backup nell'archiviazione BLOB di Azure offre un'opzione di archiviazione immediata, a disponibilità elevata e durevole.
* **Hardware gestito**: con i servizi di Azure non viene addebitato alcun sovraccarico per la gestione dell'hardware. I servizi di Azure consentono di gestire l'hardware con l'aggiunta della replica geografica per la ridondanza e la protezione dagli errori hardware.
* **Archiviazione illimitata**: abilitando un backup diretto sui BLOB di Azure, si ottiene l'accesso a uno spazio di archiviazione teoricamente illimitato. L'esecuzione di backup su una macchina virtuale di Azure presenta invece dei limiti correlati alle dimensioni della macchina stessa. Il numero di dischi che è possibile collegare a una macchina virtuale di Azure per i backup è limitato. vale a dire 16 per un'istanza di dimensioni elevate e un po' di meno per istanze di dimensioni inferiori.
* **Disponibilità dei backup**: i backup archiviati in BLOB di Azure sono disponibili ovunque e in qualsiasi momento. Sono facilmente accessibili per il ripristino in un'istanza di SQL Server, senza che sia necessario collegare e scollegare il database o scaricare e collegare il disco rigido virtuale.
* **Costo**: si paga solo per il servizio usato. Può rivelarsi una soluzione economica per il backup e l'archiviazione fuori sede. Per altre informazioni, vedere il [Calcolatore prezzi di Azure](https://go.microsoft.com/fwlink/?LinkId=277060 "Calcolatore prezzi") e la [pagina relativa ai prezzi di Azure](https://go.microsoft.com/fwlink/?LinkId=277059 "Articolo sui prezzi").
* **Snapshot di archiviazione**: quando i file di database sono archiviati in un BLOB di Azure e si usa SQL Server 2016, è possibile usare il [backup di snapshot di file](/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure) per eseguire backup quasi istantanei e ripristini estremamente rapidi.

Per informazioni dettagliate, vedere [SQL Server backup e ripristino con l'archiviazione BLOB di Azure](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service).

Le due sezioni seguenti introducono l'archivio BLOB di Azure, inclusi i componenti SQL Server necessari. È importante comprendere i componenti e la relativa interazione per usare correttamente il backup e il ripristino dall'archiviazione BLOB di Azure.

## <a name="azure-blob-storage-components"></a>Componenti di archiviazione BLOB di Azure
Quando si esegue il backup nell'archiviazione BLOB di Azure, vengono usati i componenti di Azure seguenti.

| Componente | Descrizione |
| --- | --- |
| **Account di archiviazione** |l'account di archiviazione è il punto di partenza per tutti i servizi di archiviazione. Per accedere all'archiviazione BLOB di Azure, creare prima un account di archiviazione di Azure. Per altre informazioni sull'archiviazione BLOB di Azure, vedere [come usare l'archivio BLOB di Azure](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/). |
| **Contenitore** |Un contenitore fornisce il raggruppamento di un set di BLOB ed è in grado di archiviare un numero di BLOB illimitato. Per scrivere un backup SQL Server nell'archiviazione BLOB di Azure, è necessario che sia stato creato almeno il contenitore radice. |
| **BLOB** |file di qualsiasi tipo e dimensioni. I BLOB sono indirizzabili usando il formato di URL seguente: `https://<storageaccount>.blob.core.windows.net/<container>/<blob>` . Per altre informazioni sui BLOB di pagine, vedere [Informazioni sui BLOB in blocchi, sui BLOB di aggiunta e sui BLOB di pagine](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) |

## <a name="sql-server-components"></a>Componenti SQL Server
Quando si esegue il backup nell'archiviazione BLOB di Azure, vengono usati i componenti SQL Server seguenti.

| Componente | Descrizione |
| --- | --- |
| **URL** |un URL specifica un URI (Uniform Resource Identifier) in un file di backup univoco. L'URL fornisce il percorso e il nome del file di backup SQL Server. L'URL deve puntare a un BLOB effettivo, non solo a un contenitore. Se il BLOB non esiste, viene creato da Azure. Se viene specificato un BLOB esistente, il comando backup ha esito negativo, a meno che non `WITH FORMAT` venga specificata l'opzione. Di seguito è riportato un esempio dell'URL da specificare nel comando BACKUP: `https://<storageaccount>.blob.core.windows.net/<container>/<FILENAME.bak>` .<br><br> HTTPS non è obbligatorio ma è consigliato. |
| **Credenziali** |Le informazioni necessarie per connettersi ed eseguire l'autenticazione nell'archivio BLOB di Azure vengono archiviate come credenziale. Per fare in modo che SQL Server sia in grado di scrivere backup in un BLOB di Azure o di eseguire un ripristino da quest'ultimo, è necessario creare una credenziale di SQL Server. Per altre informazioni, vedere [Credenziali di SQL Server](/sql/t-sql/statements/create-credential-transact-sql). |

> [!NOTE]
> SQL Server 2016 è stato aggiornato per supportare i BLOB in blocchi. Per informazioni dettagliate, vedere l' [esercitazione sull'uso di Microsoft Azure archiviazione BLOB con SQL Server database 2016](/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016) .
> 

## <a name="next-steps"></a>Passaggi successivi

1. Se non se ne possiede già uno, creare un account di Azure. Se si sta valutando Azure, è consigliabile usare una [versione di prova gratuita](https://azure.microsoft.com/free/).
2. Eseguire quindi una delle esercitazioni seguenti, in cui vengono fornite informazioni dettagliate sulla creazione di un account di archiviazione e sull'esecuzione di un ripristino.
   
   * **SQL Server 2014**: [esercitazione: eseguire il backup e il ripristino di SQL Server 2014 nell'archivio BLOB Microsoft Azure](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url).
   * **SQL Server 2016**: [esercitazione: uso dell'archiviazione BLOB Microsoft Azure con i database SQL Server 2016](/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016)
3. Esaminare la documentazione aggiuntiva a partire da [SQL Server backup e ripristino con Microsoft Azure archiviazione BLOB](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service).

Se si verificano problemi, consultare l'argomento [Procedure consigliate e risoluzione dei problemi per il backup di SQL Server nell'URL](/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).

Per altre SQL Server opzioni di backup e ripristino, vedere [backup e ripristino per SQL Server in macchine virtuali di Azure](backup-restore.md).