---
title: Modificare dinamicamente il livello di servizio di un volume per Azure NetApp Files | Microsoft Docs
description: Viene descritto come modificare dinamicamente il livello di servizio di un volume.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 01/14/2021
ms.author: b-juche
ms.openlocfilehash: 78cc68d2be600cec78c433ae3eae1de09d31ac94
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98251812"
---
# <a name="dynamically-change-the-service-level-of-a-volume"></a>Modificare dinamicamente il livello di servizio di un volume

> [!IMPORTANT] 
> La modifica dinamica del livello di servizio di un volume di destinazione della replica non è attualmente supportata.

È possibile modificare il livello di servizio di un volume esistente spostando il volume in un altro pool di capacità che utilizza il [livello di servizio](azure-netapp-files-service-levels.md) desiderato per il volume. Questa modifica sul posto del livello di servizio per il volume non richiede la migrazione dei dati. Non influisca inoltre sull'accesso al volume.  

Questa funzionalità consente di soddisfare le esigenze del carico di lavoro su richiesta.  È possibile modificare un volume esistente per usare un livello di servizio superiore per ottenere prestazioni migliori oppure per usare un livello di servizio inferiore per l'ottimizzazione dei costi. Ad esempio, se il volume si trova attualmente in un pool di capacità che usa il livello di servizio *standard* e si vuole che il volume usi il livello di servizio *Premium* , è possibile spostare il volume in modo dinamico in un pool di capacità che usa il livello di servizio *Premium* .  

Il pool di capacità in cui si desidera spostare il volume deve essere già esistente. Il pool di capacità può contenere altri volumi.  Se si vuole spostare il volume in un nuovo pool di capacità, è necessario [creare il pool di capacità](azure-netapp-files-set-up-capacity-pool.md) prima di spostare il volume.  

## <a name="considerations"></a>Considerazioni

* Dopo che il volume è stato spostato in un altro pool di capacità, non sarà più possibile accedere ai log attività e alle metriche del volume precedenti. Il volume inizierà con i nuovi log attività e le metriche nel nuovo pool di capacità.

* Se si sposta un volume in un pool di capacità di un livello di servizio superiore (ad esempio, passando da *standard* a *Premium* o livello di servizio *ultra* ), è necessario attendere almeno sette giorni prima di poter spostare *nuovamente* il volume in un pool di capacità di un livello di servizio inferiore (ad esempio, passando da *ultra* a *Premium* o *standard*).  

## <a name="register-the-feature"></a>Registrare la funzionalità

La funzionalità per spostare un volume in un altro pool di capacità è attualmente in fase di anteprima. Se si usa questa funzionalità per la prima volta, è necessario prima registrarla.

1. Registrare la funzionalità: 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```

2. Verificare lo stato della registrazione della funzionalità: 

    > [!NOTE]
    > Il **RegistrationState** potrebbe trovarsi nello `Registering` stato per un massimo di 60 minuti prima di modificare in `Registered` . Prima di continuare, attendere che lo stato sia **registrato** .

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```
È anche possibile usare i [comandi dell'interfaccia](/cli/azure/feature?preserve-view=true&view=azure-cli-latest) `az feature register` della riga di comando di Azure e `az feature show` registrare la funzionalità e visualizzare lo stato della registrazione. 
 
## <a name="move-a-volume-to-another-capacity-pool"></a>Spostare un volume in un altro pool di capacità

1.  Nella pagina volumi, fare clic con il pulsante destro del mouse sul volume di cui si desidera modificare il livello di servizio. Selezionare **Cambia pool**.

    ![Pulsante destro del mouse su volume](../media/azure-netapp-files/right-click-volume.png)

2. Nella finestra Cambia pool selezionare il pool di capacità in cui si desidera spostare il volume. 

    ![Cambia pool](../media/azure-netapp-files/change-pool.png)

3.  Fare clic su **OK**.


## <a name="next-steps"></a>Passaggi successivi  

* [Livelli di servizio per Azure NetApp Files](azure-netapp-files-service-levels.md)
* [Configurare un pool di capacità](azure-netapp-files-set-up-capacity-pool.md)
* [Risolvere i problemi relativi alla modifica del pool di capacità di un volume](troubleshoot-capacity-pools.md#issues-when-changing-the-capacity-pool-of-a-volume)
