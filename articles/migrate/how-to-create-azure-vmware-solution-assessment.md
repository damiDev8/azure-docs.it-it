---
title: Creare una valutazione AVS con Azure Migrate server Assessment | Microsoft Docs
description: Viene descritto come creare una valutazione AVS con lo strumento Azure Migrate server Assessment
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 06/26/2020
ms.openlocfilehash: fb1ec55bc68ccc323f8dee90982a9169e3085219
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2021
ms.locfileid: "98567649"
---
# <a name="create-an-azure-vmware-solution-avs-assessment"></a>Creare una valutazione della soluzione VMware di Azure (AVS)

Questo articolo descrive come creare una valutazione della soluzione VMware di Azure (AVS) per le macchine virtuali VMware locali con Azure Migrate: server assessment.

[Azure migrate](migrate-services-overview.md) consente di eseguire la migrazione ad Azure. Azure Migrate offre un hub centralizzato per tenere traccia dell'individuazione, della valutazione e della migrazione ad Azure dell'infrastruttura, delle applicazioni e dei dati locali. L'hub fornisce gli strumenti di Azure per la valutazione e la migrazione, oltre a offerte di fornitori di software indipendenti (ISV) di terze parti.

## <a name="before-you-start"></a>Prima di iniziare

- Assicurarsi di aver [creato](./create-manage-projects.md) un progetto Azure migrate.
- Se è già stato creato un progetto, verificare di aver [aggiunto](how-to-assess.md) lo strumento Azure migrate: server assessment.
- Per creare una valutazione, è necessario configurare un'appliance Azure Migrate per [VMware](how-to-set-up-appliance-vmware.md), che individua i computer locali e invia i metadati e i dati sulle prestazioni a Azure migrate: valutazione del server. [Altre informazioni](migrate-appliance.md)
- È anche possibile [importare i metadati del server](./tutorial-discover-import.md) nel formato con valori delimitati da virgole (CSV).


## <a name="azure-vmware-solution-avs-assessment-overview"></a>Panoramica sulla valutazione della soluzione VMware di Azure (AVS)

È possibile creare due tipi di valutazioni con Azure Migrate: Valutazione server.

**Tipo di valutazione** | **Dettagli**
--- | --- 
**Macchina virtuale di Azure** | Valutazioni per la migrazione dei server locali in macchine virtuali di Azure. <br/><br/> È possibile valutare le [macchine virtuali VMware](how-to-set-up-appliance-vmware.md)locali, le [VM Hyper-V](how-to-set-up-appliance-hyper-v.md)e i [server fisici](how-to-set-up-appliance-physical.md) per la migrazione ad Azure usando questo tipo di valutazione. [Altre informazioni](concepts-assessment-calculation.md)
**Soluzione Azure VMware** | Valutazioni per la migrazione dei server locali nella [soluzione Azure VMware](../azure-vmware/introduction.md). <br/><br/> È possibile valutare le [macchine virtuali VMware](how-to-set-up-appliance-vmware.md) locali per la migrazione alla soluzione Azure VMware usando questo tipo di valutazione. [Altre informazioni](concepts-azure-vmware-solution-assessment-calculation.md)

> [!NOTE]
> La valutazione della soluzione VMware di Azure (AVS) è attualmente in anteprima e può essere creata solo per le macchine virtuali VMware.


Esistono due tipi di criteri di ridimensionamento che è possibile usare per creare valutazioni della soluzione VMware di Azure (AVS):

**Valutazione** | **Dettagli** | **Dati**
--- | --- | ---
**Basata sulle prestazioni** | Valutazioni basate sui dati sulle prestazioni raccolti delle macchine virtuali locali. | **Dimensioni consigliate del nodo**: basate sui dati di utilizzo della CPU e della memoria con il tipo di nodo, il tipo di archiviazione e l'impostazione di ITF selezionati per la valutazione.
**Come in locale** | Valutazioni basate sul dimensionamento locale. | **Dimensioni consigliate del nodo**: in base alle dimensioni della macchina virtuale locale con il tipo di nodo, il tipo di archiviazione e l'impostazione di ITF selezionati per la valutazione.


## <a name="run-an-azure-vmware-solution-avs-assessment"></a>Eseguire una valutazione della soluzione VMware di Azure (AVS)

Eseguire una valutazione della soluzione VMware di Azure (AVS) come indicato di seguito:

1. Rivedere le [procedure consigliate](best-practices-assessment.md) per la creazione di valutazioni.

2. Nella scheda **Server**, nel riquadro **Azure Migrate: Valutazione server**, fare clic su **Valuta**.

    ![Screenshot mostra i server Azure Migrate con valutazione selezionata in strumenti di valutazione.](./media/how-to-create-assessment/assess.png)

3. In **valuta Server** selezionare il tipo di valutazione come "soluzione VMware di Azure (AVS)" e selezionare l'origine di individuazione.

    :::image type="content" source="./media/how-to-create-avs-assessment/assess-servers-avs.png" alt-text="Aggiungi nozioni di base sulla valutazione":::

4. Fare clic su **modifica** per esaminare le proprietà della valutazione.

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/assess-servers.png" alt-text="Posizione del pulsante modifica per esaminare le proprietà di valutazione":::

1. In **Seleziona computer per valutare**  >  il **nome della valutazione** > specificare un nome per la valutazione. 
 
1. In **selezionare o creare un gruppo** > selezionare **Crea nuovo** e specificare un nome di gruppo. Un gruppo raccoglie una o più VM per la valutazione.
    
    :::image type="content" source="./media/tutorial-assess-vmware-azure-vmware-solution/assess-group.png" alt-text="Aggiungere macchine virtuali a un gruppo":::

1. In **Aggiungere le macchine virtuali al gruppo** selezionare le VM da aggiungere al gruppo.

1. Fare clic su **Avanti** per passare a **Rivedi e crea valutazione** e rivedere i dettagli della valutazione.

1. Fare clic su **Crea valutazione** per creare il gruppo ed eseguire la valutazione.

1. Dopo aver creato la valutazione, visualizzarla in **Server** > **Azure Migrate: Valutazione server** > **Valutazioni**.

1. Fare clic su **Esporta valutazione** per scaricarla come file di Excel.


## <a name="review-an-azure-vmware-solution-avs-assessment"></a>Esaminare una valutazione della soluzione VMware di Azure (AVS)

Una valutazione della soluzione VMware di Azure (AVS) descrive:

- Idoneità per la **soluzione VMware di Azure**: se le VM locali sono adatte per la migrazione alla soluzione VMware di Azure (AVS).
- **Numero di nodi AVS**: numero stimato di nodi AVS necessari per eseguire le VM.
- **Utilizzo tra nodi AVS**: utilizzo di CPU, memoria e spazio di archiviazione proiettato in tutti i nodi.
    - L'utilizzo include il primo fattore per il sovraccarico di gestione cluster, ad esempio server vCenter, NSX Manager (Large), NSX Edge, se HCX è distribuito anche HCX Manager e IX Appliance consumo ~ 44vCPU (11 CPU), 75 GB di RAM e 722GB di archiviazione prima della compressione e della deduplicazione.
    - La memoria, la deduplicazione e la compressione sono attualmente impostate per l'utilizzo del 100% per la memoria e la deduplicazione e la compressione di 1,5, che saranno un input definito dall'utente nelle versioni future consentendo all'utente di ottimizzare le dimensioni richieste.
- **Stima dei costi mensili**: i costi mensili stimati per tutti i nodi della soluzione VMware di Azure (AVS) che eseguono le macchine virtuali locali.


### <a name="view-an-assessment"></a>Visualizzare una valutazione

1. In **Obiettivi della migrazione** >  **Server** fare clic su **Valutazioni** in **Azure Migrate: Valutazione server**.

2. In **Valutazioni** fare clic su una valutazione per aprirla.

    :::image type="content" source="./media/how-to-create-avs-assessment/avs-assessment-summary.png" alt-text="Riepilogo di AVS Assessment":::

### <a name="review-azure-vmware-solution-avs-readiness"></a>Esaminare la conformità della soluzione VMware di Azure (AVS)

1. In **Azure conformità** verificare se le macchine virtuali sono pronte per la migrazione a AVS.

2. Verificare lo stato delle VM:
    - **Pronto per AVS**: è possibile eseguire la migrazione del computer in Azure (AVS) senza alcuna modifica. Verrà avviato in AVS con supporto AVS completo.
    - **Pronto con le condizioni**: la macchina virtuale potrebbe avere problemi di compatibilità con la versione corrente di vSphere, nonché richiedere gli strumenti VMware e altre impostazioni prima che sia possibile ottenere le funzionalità complete dalla macchina virtuale in AVS.
    - **Non pronto per AVS**: la macchina virtuale non viene avviata in AVS. Ad esempio, se la macchina virtuale VMware locale dispone di un dispositivo esterno collegato, ad esempio un CD-ROM, l'operazione VMotion avrà esito negativo (se si usa VMware VMotion).
    - **Conformità sconosciuta**: Azure migrate non è stato in grado di determinare la conformità del computer a causa di metadati insufficienti raccolti dall'ambiente locale.

3. Esaminare lo strumento consigliato:
    - **VMware HCX o Enterprise**: per i computer VMware, la soluzione VMware Hybrid Cloud Extension (HCx) è lo strumento di migrazione suggerito per eseguire la migrazione del carico di lavoro locale al cloud privato della soluzione VMware di Azure (AVS). [Altre informazioni](../azure-vmware/tutorial-deploy-vmware-hcx.md).
    - **Sconosciuto**: Per i computer importati tramite un file con estensione csv, lo strumento di migrazione predefinito è sconosciuto. Tuttavia, per i computer VMware è consigliabile usare la soluzione VMware Hybrid Cloud Extension (HCX). 

4. Fare clic su uno stato di idoneità **AVS** . È possibile visualizzare i dettagli sull'idoneità delle VM ed eseguire il drill-down per visualizzare i dettagli delle VM, incluse le impostazioni di calcolo, archiviazione e rete.



### <a name="review-cost-details"></a>Esaminare i dettagli dei costi

Questa visualizzazione Mostra il costo stimato per l'esecuzione di macchine virtuali in Azure VMware Solution (AVS).

1. Esaminare i costi totali mensili. I costi sono aggregati per tutte le VM nel gruppo valutato. 

    - Le stime dei costi si basano sul numero di nodi AVS necessari considerando i requisiti delle risorse di tutte le macchine virtuali in totale.
    - Dato che i prezzi per la soluzione VMware di Azure (AVS) sono per nodo, il costo totale non prevede costi di calcolo e distribuzione dei costi di archiviazione.
    - La stima dei costi è relativa all'esecuzione delle macchine virtuali locali nella soluzione Azure VMware. Valutazione server di Azure Migrate non considera i costi per PaaS o SaaS.
    
2. È possibile esaminare le stime dei costi di archiviazione mensile. Questa visualizzazione mostra i costi di archiviazione aggregati per il gruppo valutato, suddivisi in base ai diversi tipi di dischi di archiviazione.

3. È possibile eseguire il drill-down per visualizzare i dettagli relativi a VM specifiche.


### <a name="review-confidence-rating"></a>Esaminare la classificazione di attendibilità

Quando si eseguono valutazioni basate sulle prestazioni, alla valutazione viene assegnata una classificazione di attendibilità.

![Classificazione di attendibilità](./media/how-to-create-assessment/confidence-rating.png)

- La classificazione è compresa tra 1 stella (minima) e 5 stelle (massima).
- La classificazione di attendibilità aiuta a stimare l'affidabilità delle indicazioni relative alle dimensioni fornite dalla valutazione.
- La classificazione di attendibilità è basata sulla disponibilità dei punti dati necessari per calcolare la valutazione.
- Per il dimensionamento in base alle prestazioni, le valutazioni AVS in server Assessment richiedono i dati di utilizzo per la CPU e la memoria della macchina virtuale. I dati sulle prestazioni seguenti vengono raccolti ma non usati nei consigli per il dimensionamento per le valutazioni AVS:
  - I dati di IOPS e velocità effettiva del disco per ogni disco collegato alla VM.
  - I/O di rete per gestire il dimensionamento in base alle prestazioni per ogni scheda di rete collegata a una macchina virtuale.

Di seguito sono elencate le classificazioni di attendibilità per una valutazione.

**Disponibilità dei punti dati** | **Classificazione di attendibilità**
--- | ---
0%-20% | 1 stella
21%-40% | 2 stelle
41%-60% | 3 stelle
61%-80% | 4 stelle
81%-100% | 5 stelle

[Altre](concepts-azure-vmware-solution-assessment-calculation.md) informazioni sui dati sulle prestazioni 


## <a name="next-steps"></a>Passaggi successivi

- Informazioni su come usare il [mapping delle dipendenze](how-to-create-group-machine-dependencies.md) per creare gruppi con attendibilità elevata.
- [Altre](concepts-azure-vmware-solution-assessment-calculation.md) informazioni su come vengono calcolate le valutazioni AVS.