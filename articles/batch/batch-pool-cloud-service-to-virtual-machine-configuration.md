---
title: Eseguire la migrazione della configurazione del pool di batch dai servizi cloud alle macchine virtuali
description: Informazioni su come aggiornare la configurazione del pool alla configurazione più recente e consigliata
ms.topic: how-to
ms.date: 1/6/2021
ms.openlocfilehash: 417738be2c69101129079b8ff3a3d80634f9f99c
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2021
ms.locfileid: "98731500"
---
# <a name="migrate-batch-pool-configuration-from-cloud-services-to-virtual-machines"></a>Eseguire la migrazione della configurazione del pool di batch dai servizi cloud alle macchine virtuali

I pool di batch possono essere creati usando [cloudServiceConfiguration](/rest/api/batchservice/pool/add#cloudserviceconfiguration) o [virtualMachineConfiguration](/rest/api/batchservice/pool/add#virtualmachineconfiguration). ' virtualMachineConfiguration ' è la configurazione consigliata perché supporta tutte le funzionalità batch. i pool ' cloudServiceConfiguration ' non supportano tutte le funzionalità e non sono state pianificate nuove funzionalità.

Se si usano i pool ' cloudServiceConfiguration ', si consiglia di passare all'uso dei pool ' virtualMachineConfiguration '. Ciò consentirà di trarre vantaggio da tutte le funzionalità batch, ad esempio una selezione ampliata [di serie di VM](batch-pool-vm-sizes.md), VM Linux, [contenitori](batch-docker-container-workloads.md), [Azure Resource Manager reti virtuali](batch-virtual-network.md)e [crittografia del disco del nodo](disk-encryption.md).

Questo articolo descrive come eseguire la migrazione a' virtualMachineConfiguration '.

## <a name="new-pools-are-required"></a>Sono necessari nuovi pool

Non è possibile aggiornare i pool attivi esistenti da' cloudServiceConfiguration ' a' virtualMachineConfiguration '. è necessario creare nuovi pool. La creazione di pool con ' virtualMachineConfiguration ' è supportata da tutte le API batch, dagli strumenti da riga di comando portale di Azure e dall'interfaccia utente Batch Explorer.

**Le esercitazioni su [.NET](tutorial-parallel-dotnet.md) e [Python](tutorial-parallel-python.md) forniscono esempi di creazione di pool con "virtualMachineConfiguration".**

## <a name="pool-configuration-differences"></a>Differenze di configurazione del pool

Quando si aggiorna la configurazione del pool, è necessario considerare quanto segue:

- i nodi del pool ' cloudServiceConfiguration ' sono sempre sistemi operativi Windows, i pool ' virtualMachineConfiguration ' possono essere di tipo Linux o Windows.
- Rispetto ai pool ' cloudServiceConfiguration ', i pool ' virtualMachineConfiguration ' hanno un set più completo di funzionalità, ad esempio il supporto dei contenitori, i dischi dati e la crittografia del disco.
- i nodi del pool ' virtualMachineConfiguration ' usano dischi del sistema operativo gestiti. Il [tipo di disco gestito](../virtual-machines/disks-types.md) usato per ogni nodo dipende dalle dimensioni della macchina virtuale scelte per il pool. Se per il pool è specificata la dimensione della VM, ad esempio "Standard_D2s_v3", viene usata un'unità SSD Premium. Se viene specificata una dimensione della macchina virtuale "non s", ad esempio "Standard_D2_v3", viene usato un disco rigido standard.

   > [!IMPORTANT]
   > Come per le macchine virtuali e i set di scalabilità di macchine virtuali, il disco gestito del sistema operativo usato per ogni nodo comporta un costo, che è aggiuntivo per il costo delle macchine virtuali. Non è previsto alcun costo del disco del sistema operativo per i nodi ' cloudServiceConfiguration ' durante la creazione del disco del sistema operativo nell'unità SSD locale dei nodi.

- I tempi di avvio e di eliminazione del pool e del nodo possono variare leggermente tra i pool ' cloudServiceConfiguration ' e i pool ' virtualMachineConfiguration '.

## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni sulle [configurazioni del pool](nodes-and-pools.md#configurations).
- Altre informazioni sulle [procedure](best-practices.md#pools)consigliate per il pool.
- Informazioni di riferimento sull'API REST per l' [aggiunta del pool](/rest/api/batchservice/pool/add) e [virtualMachineConfiguration](/rest/api/batchservice/pool/add#virtualmachineconfiguration).