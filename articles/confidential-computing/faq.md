---
title: Domande frequenti su Confidential computing di Azure
description: Risposte ad alcune domande frequenti su Confidential computing di Azure.
author: JBCook
ms.topic: troubleshooting
ms.workload: infrastructure
ms.service: virtual-machines
ms.subservice: workloads
ms.date: 4/17/2020
ms.author: jencook
ms.openlocfilehash: 38bf12b46002e767bba50cf833637e2c8ace078f
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98872333"
---
# <a name="frequently-asked-questions-for-azure-confidential-computing"></a>Domande frequenti su Confidential computing di Azure

Questo articolo fornisce le risposte ad alcune delle domande più comuni sull'esecuzione di [carichi di lavoro di elaborazione riservati in macchine virtuali di Azure](overview.md).

Se il problema riguardante Azure non viene risolto in questo articolo, visitare i forum di Azure su [MSDN e Stack Overflow](https://azure.microsoft.com/support/forums/). È possibile pubblicare il problema in questi forum o in [@AzureSupport su Twitter](https://twitter.com/AzureSupport). È anche possibile inviare una richiesta di supporto tecnico di Azure. Per inviare una richiesta di supporto tecnico, selezionare Supporto nella pagina [Supporto tecnico di Azure](https://azure.microsoft.com/support/options/).

## <a name="confidential-computing-virtual-machines"></a>Macchine virtuali di confidential computing<a id="vm-faq"></a>

**Come è possibile distribuire macchine virtuali serie DCsv2 in Azure?**

Ecco alcuni modi in cui è possibile distribuire una macchina virtuale DCsv2:
   - Uso di un [modello di Azure Resource Manager](../virtual-machines/windows/template-description.md)
   - Dal [portale di Azure](https://portal.azure.com/#create/hub)
   - Nel modello di soluzione [Confidential Computing di Azure (macchina virtuale)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview) del marketplace. Il modello di soluzione del marketplace consente di vincolare un cliente agli scenari supportati (aree, immagini, disponibilità, crittografia del disco). 

**Le immagini del sistema operativo funzionano tutte con Confidential computing di Azure?**

No. Le macchine virtuali possono essere distribuite solo in computer operativi di generazione 2 con Ubuntu Server 18.04, Ubuntu Server 16.04, Windows Server 2019 Datacenter e Windows Server 2016 Datacenter. Altre informazioni sulle macchine virtuali di generazione 2 in [Linux](../virtual-machines/generation-2.md) e [Windows](../virtual-machines/generation-2.md)

**Le macchine virtuali DCsv2 sono disabilitate nel portale e non è possibile selezionarne una**

Nella finestra informativa accanto alla macchina virtuale sono disponibili diverse azioni da eseguire:
   -    **UnsupportedGeneration**: Modificare la generazione dell'immagine della macchina virtuale in "Gen2".
   -    **NotAvailableForSubscription**: L'area non è ancora disponibile per la sottoscrizione. Selezionare un'area disponibile.
   -    **InsufficientQuota**: [Creare una richiesta di supporto per aumentare la quota](../azure-portal/supportability/per-vm-quota-requests.md). Le sottoscrizioni della versione di prova gratuita non prevedono alcuna quota per le macchine virtuali di confidential computing. 

**Le macchine virtuali DCsv2 non vengono visualizzate quando si tenta di cercarle nel selettore delle dimensioni del portale**

Assicurarsi di aver selezionato un'[area disponibile](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines). Assicurarsi di selezionare anche "Cancella tutti i filtri" nel selettore delle dimensioni. 

**È possibile abilitare la rete accelerata con Azure Confidential computing?**

 No. La rete accelerata non è supportata nelle macchine virtuali DC-Series o DCsv2-Series. Non è possibile abilitare la rete accelerata per la distribuzione di macchine virtuali di elaborazione riservata o per la distribuzione di cluster di servizi Kubernetes di Azure in esecuzione in un computer riservato.

**È possibile usare l'host dedicato di Azure con questi computer?**

Sì. Host dedicato di Azure supporta macchine virtuali serie DCsv2. Host dedicato di Azure fornisce un server fisico a tenant singolo per l'esecuzione delle macchine virtuali. Gli utenti in genere usano host dedicato di Azure per soddisfare i requisiti di conformità per la sicurezza fisica, l'integrità dei dati e il monitoraggio. 

**Si verifica un errore di distribuzione del modello di Azure Resource Manager: "Non è stato possibile completare l'operazione perché comporta il superamento della quota standard approvata di core della famiglia DcsV2"**

[Creare una richiesta di supporto per aumentare la quota](../azure-portal/supportability/per-vm-quota-requests.md). Le sottoscrizioni della versione di prova gratuita non prevedono alcuna quota per le macchine virtuali di confidential computing. 

**Qual è la differenza tra le macchine virtuali serie DCsv2 e serie DC?**

Le macchine virtuali serie DC vengono eseguite in processori Intel a 6 core precedenti con Intel SGX e presentano una memoria totale inferiore, una memoria EPC (Enclave Page Cache) ridotta e sono disponibili solo in due aree (Stati Uniti orientali ed Europa occidentale con dimensioni Standard_DC2s e Standard_DC4s). Non ci sono piani per rendere queste macchine virtuali disponibili a livello generale e non sono consigliate per l'uso in produzione. Per distribuire queste macchine virtuali, usare l'istanza del marketplace [Macchina virtuale di calcolo serie DC di Confidential Computing [Anteprima]](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.confidentialcompute?tab=Overview).

**Le macchine virtuali DCsv2 sono disponibili a livello globale?**

No. Attualmente, queste macchine virtuali sono disponibili solo in aree selezionate. Consultare la pagina [Products by Regions](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) (Prodotti per area) per le aree più recenti disponibili. 

**Hyper-Threading è disattivato su questi computer?**

Hyper-Threading è disabilitato per tutti i cluster di Azure Confidential computing.

**Ricerca per categorie: installare Open Enclave SDK su macchine virtuali DCsv2**
   
Per indicazioni su come installare Open Enclave SDK in un computer Azure o locale, seguire le istruzioni riportate nel [repository GitHub di SDK Open Enclave](https://github.com/openenclave/openenclave).
     
Per istruzioni di installazione specifiche per il sistema operativo, vedere il repository GitHub di SDK Open Enclave:
   - [Installare Open Enclave SDK in Windows](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Windows.md)
   - [Installare Open Enclave SDK in Ubuntu 18.04](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_18.04.md)
   - [Installare Open Enclave SDK in Ubuntu 16.04](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_16.04.md)