---
title: Funzionalità di rendering
description: Le funzionalità standard di Azure Batch vengono usate per eseguire app e carichi di lavoro di rendering. Batch include funzionalità specifiche per supportare i carichi di lavoro di rendering.
author: mscurrell
ms.author: markscu
ms.date: 02/01/2021
ms.topic: how-to
ms.openlocfilehash: c748c51311a70cd3b4fc6d435e97a33d709467b9
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99429803"
---
# <a name="azure-batch-rendering-capabilities"></a>Funzionalità di rendering di Azure Batch

Le funzionalità standard di Azure Batch vengono usate per eseguire applicazioni e carichi di lavoro di rendering. Batch include anche funzionalità specifiche per supportare i carichi di lavoro di rendering.

Per una panoramica dei concetti di Batch, inclusi pool, processi e attività, vedere [questo articolo](./batch-service-workflow-features.md).

## <a name="batch-pools-using-custom-vm-images-and-standard-application-licensing"></a>Pool di batch con immagini di VM personalizzate e licenze di applicazioni standard

Come per altri carichi di lavoro e tipi di applicazione, è possibile creare un'immagine di macchina virtuale personalizzata con le applicazioni e i plug-in necessari per il rendering. L'immagine di macchina virtuale personalizzata viene inserita nella [raccolta di immagini condivise](../virtual-machines/shared-image-galleries.md) e [può essere usata per creare pool di batch](batch-sig-images.md).

Le stringhe della riga di comando dell'attività dovranno fare riferimento alle applicazioni e ai percorsi usati durante la creazione dell'immagine di macchina virtuale personalizzata.

Per la maggior parte delle applicazioni di rendering sono necessarie licenze ottenute da un server licenze. Se è presente un server licenze locale esistente, il pool e il server licenze devono trovarsi nella stessa [rete virtuale](../virtual-network/virtual-networks-overview.md). È anche possibile eseguire un server licenze in una macchina virtuale di Azure, con il pool di batch e la VM del server licenze che si trova nella stessa rete virtuale.

## <a name="batch-pools-using-rendering-vm-images"></a>Pool di batch che usano il rendering delle immagini di VM

### <a name="rendering-application-installation"></a>Installazione delle applicazioni di rendering

Se è necessario usare solo le applicazioni pre-installate, è possibile specificare un'immagine di VM di rendering di Azure Marketplace nella configurazione pool.

Sono disponibili un'immagine di Windows e un'immagine CentOS.  In [Azure Marketplace](https://azuremarketplace.microsoft.com) cercare "rendering di batch" per trovare le immagini di VM.

Per una configurazione pool di esempio, vedere l'[esercitazione sul rendering nell'interfaccia della riga di comando di Azure](./tutorial-rendering-cli.md).  Il portale di Azure e Batch Explorer forniscono strumenti dell'interfaccia utente grafica per selezionare un'immagine di VM di rendering quando si crea un pool.  Se si usa un'API Batch, specificare i valori della proprietà seguenti per [ImageReference](/rest/api/batchservice/pool/add#imagereference) quando si crea un pool:

| Editore | Offerta | Sku | Versione |
|---------|---------|---------|--------|
| o batch | rendering-centos73 | rendering | più recenti |
| o batch | rendering-windows2016 | rendering | più recenti |

Sono disponibili altre opzioni se sono necessarie applicazioni aggiuntive nelle VM del pool:

* Un'immagine personalizzata dalla Raccolta immagini condivise:
  * Con questa opzione, è possibile configurare la macchina virtuale esattamente con le applicazioni e le versioni specifiche necessarie. Per altre informazioni, vedere [Creare un pool con Raccolta immagini condivise](batch-sig-images.md). Autodesk e Chaos Group hanno modificato, rispettivamente, Arnold e V-Ray per eseguire la convalida in base al servizio di licenze di Azure Batch. Verificare di avere le versioni di queste applicazioni con questo supporto. In caso contrario, le licenze con pagamento in base al consumo non funzioneranno. Le versioni correnti di Maya o 3ds Max non richiedono un server licenze durante l'esecuzione headless (in modalità batch/riga di comando). In caso di dubbi su come procedere con questa opzione, contattare il supporto di Azure.
* [Pacchetti dell'applicazione](./batch-application-packages.md):
  * Includere in un pacchetto i file dell'applicazione usando uno o più file ZIP, caricarlo tramite il portale di Azure e specificare il pacchetto nella configurazione pool. Quando vengono create le VM del pool, i file ZIP vengono scaricati e i file vengono estratti.
* File di risorse:
  * I file dell'applicazione vengono caricati nell'archivio BLOB di Azure, quindi si specificano i riferimenti ai file nell'[attività di avvio del pool](/rest/api/batchservice/pool/add#starttask). Quando vengono create le VM del pool, i file di risorse vengono scaricati in ogni VM.

### <a name="pay-for-use-licensing-for-pre-installed-applications"></a>Licenze con pagamento in base al consumo per le applicazioni pre-installate

Le applicazioni che verranno usate e hanno una tariffa per le licenze devono essere specificate nella configurazione del pool.

* Specificare la proprietà `applicationLicenses` quando si [crea un pool](/rest/api/batchservice/pool/add#request-body).  I valori seguenti possono essere specificati nella matrice di stringhe: "v-ray", "arnold", "3dsmax", "maya".
* Quando si specifica una o più applicazioni, il costo di tali applicazioni viene aggiunto al costo delle VM.  I prezzi delle applicazioni sono elencati nella [pagina dei prezzi di Azure Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

> [!NOTE]
> Se invece ci si connette a un server licenze per usare le applicazioni di rendering, non si specifica la proprietà `applicationLicenses`.

È possibile usare il portale di Azure o Batch Explorer per selezionare le applicazioni e visualizzare i prezzi delle applicazioni.

Se viene fatto un tentativo di usare un'applicazione, ma l'applicazione non è stata specificata nella proprietà `applicationLicenses` della configurazione pool o non raggiunge un server licenze, l'esecuzione dell'applicazione non riesce con un errore di licenza e un codice di uscita diverso da zero.

### <a name="environment-variables-for-pre-installed-applications"></a>Variabili di ambiente per le applicazioni pre-installate

Per poter creare la riga di comando per le attività di rendering, è necessario specificare il percorso di installazione dei file eseguibili delle applicazioni di rendering.  Nelle immagini di VM di Azure Marketplace sono state create variabili di ambiente di sistema, che possono essere usate invece di dover specificare i percorsi effettivi.  Queste variabili di ambiente si aggiungono alle [variabili di ambiente di Batch standard](./batch-compute-node-environment-variables.md) create per ogni attività.

|Applicazione|File eseguibile dell'applicazione|Variabile di ambiente|
|---------|---------|---------|
|Autodesk 3ds Max 2021|3dsmaxcmdio.exe|3DSMAX_2021_EXEC|
|Autodesk Maya 2020|render.exe|MAYA_2020_EXEC|
|Chaos Group V-Ray Standalone|vray.exe|VRAY_4 VRAY_4.10.03_EXEC|
|Riga di comando di Arnold 2020|kick.exe|ARNOLD_2020_EXEC|
|Blender|blender.exe|BLENDER_2018_EXEC|

## <a name="azure-vm-families"></a>Famiglie di VM di Azure

Come con altri carichi di lavoro, i requisiti di sistema delle applicazioni di rendering sono diversi e i requisiti delle prestazioni variano a seconda dei progetti e dei processi.  In Azure è disponibile un'ampia gamma di famiglie di VM a seconda dei requisiti: costo più basso, miglior rapporto prezzo/prestazioni, migliori prestazioni e così via.
Alcune applicazioni di rendering, ad esempio Arnold, sono basate sulla CPU, altre, ad esempio V-Ray e Blender Cycles, possono usare CPU e/o GPU.
Per una descrizione delle famiglie di VM e delle dimensioni di VM disponibili, [vedere i tipi e le dimensioni delle VM](../virtual-machines/sizes.md).

## <a name="low-priority-vms"></a>Macchine virtuali con priorità bassa

Come con altri carichi di lavoro, le VM con priorità bassa possono essere utilizzate nei pool di Batch per il rendering.  Le VM con priorità bassa funzionano come le normali VM dedicate, ma utilizzano la capacità in eccesso di Azure e sono disponibili a un prezzo fortemente scontato.  Il compromesso per l'uso di macchine virtuali con priorità bassa è che queste macchine virtuali possono non essere disponibili per l'allocazione o essere interrotte in qualsiasi momento, a seconda della capacità disponibile. Per questo motivo, le VM con priorità bassa non saranno idonee per tutti i processi di rendering. Se ad esempio il rendering delle immagini richiede diverse ore, è probabile che l'interruzione e il riavvio del rendering di tali immagini a causa della terminazione delle VM non sarà accettabile.

Per altre informazioni sulle caratteristiche delle VM con priorità bassa e sui diversi modi di configurarle usando Batch, vedere [Usare le macchine virtuali con priorità bassa in Batch](./batch-low-pri-vms.md).

## <a name="jobs-and-tasks"></a>Processi e attività

Nessun supporto specifico per il rendering è necessario per i processi e le attività.  L'elemento di configurazione principale è la riga di comando delle attività, che deve fare riferimento all'applicazione necessaria.
Quando vengono usate le immagini di VM di Azure Marketplace, la procedura consigliata è usare le variabili di ambiente per specificare il percorso e il file eseguibile dell'applicazione.

## <a name="next-steps"></a>Passaggi successivi

Per esempi del rendering di Batch provare le due esercitazioni:

* [Eseguire il rendering con l'interfaccia della riga di comando di Azure](./tutorial-rendering-cli.md)
* [Eseguire il rendering con Batch Explorer](./tutorial-rendering-batchexplorer-blender.md)