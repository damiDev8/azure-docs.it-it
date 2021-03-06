---
title: includere file
description: includere file
services: storage
author: alkohli
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: alkohli
ms.custom: include file
ms.openlocfilehash: 1be6a654962b513cfcf755d45e562b86067e7b25
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "95995013"
---
#### <a name="to-create-a-new-service"></a>Per creare un nuovo servizio

1.  Usando le credenziali dell'account Microsoft, accedere al portale di Azure all'URL seguente: <https://portal.azure.com/>. Se si esegue la distribuzione del dispositivo nel portale per enti pubblici, accedere a <https://portal.azure.us/>

2.  Nel portale di Azure fare clic su **Crea una risorsa** &gt; **Archiviazione** &gt; **Serie di dispositivi virtuali StorSimple**.

    ![Creare un nuovo servizio](./media/storsimple-virtual-array-create-new-service/createnewservice2.png) 

3.  Nel pannello **Gestione dispositivi StorSimple** visualizzato eseguire queste operazioni:

    1.  Specificare un **Nome della risorsa** univoco per il servizio. Il nome della risorsa è un nome descrittivo che può essere usato per identificare il servizio. Il nome può contenere da 2 a 50 caratteri che possono essere lettere, numeri e trattini. Il nome deve iniziare e terminare con una lettera o un numero.

    2.  Selezionare una sottoscrizione nell'elenco a discesa **Sottoscrizione**. La sottoscrizione viene collegata all'account di fatturazione. Questo campo non è presente se si dispone di una sola sottoscrizione.

    3.  Per **Gruppo di risorse** selezionare un gruppo esistente o creare un nuovo gruppo. Per altre informazioni, vedere [Azure resource groups](../articles/azure-resource-manager/management/manage-resource-groups-portal.md) (Gruppi di risorse di Azure).

    4.  In **Località** specificare una località per il servizio. Per altre informazioni sui servizi disponibili in aree specifiche, vedere [Aree di Azure](https://azure.microsoft.com/regions/#services) . In generale, scegliere la **posizione** più vicina all'area geografica in cui si vuole distribuire il dispositivo. È inoltre possibile tenere in considerazione quanto segue:

        -   Se sono presenti altri carichi di lavoro in Azure che si intende distribuire con il dispositivo StorSimple, è consigliabile usare il data center specifico.

        -   Gestione dispositivi StorSimple e Archiviazione di Azure possono trovarsi in due posizioni distinte. In tal caso, è necessario creare l'account di Gestione dispositivi StorSimple e di Archiviazione di Azure separatamente. Per creare un account di archiviazione di Azure, passare ad Archiviazione di Azure nel portale di Azure e seguire i passaggi descritti in [Creare un account di archiviazione](../articles/storage/common/storage-account-create.md). Dopo aver creato l'account, aggiungerlo al servizio Gestione dispositivi StorSimple seguendo la procedura descritta in [Configurare un nuovo account di archiviazione per il servizio](../articles/storsimple/storsimple-virtual-array-manage-storage-accounts.md#add-a-storage-account-credential).

        -   Se si esegue la distribuzione del dispositivo virtuale nel portale per enti pubblici, il servizio Gestione dispositivi StorSimple è disponibile nelle località statunitensi dell'Iowa e della Virginia.

    5.  Selezionare **Crea un nuovo account di archiviazione di Azure** per creare automaticamente un account di archiviazione con il servizio. Specificare un **Nome dell'account di archiviazione**. Se è necessario che i dati siano in un percorso diverso, deselezionare questa casella.

    6.  Selezionare **Aggiungi al dashboard** se si vuole inserire un collegamento rapido al servizio nel dashboard.

    7.  Fare clic su **Crea** per creare il servizio Gestione dispositivi StorSimple.

        ![Crea nuovo servizio 2](./media/storsimple-virtual-array-create-new-service/createnewservice4.png)  

Si viene indirizzati alla pagina di destinazione **Servizio**. La creazione del servizio richiede alcuni minuti. Al termine della creazione del servizio, viene inviata una notifica all'utente e lo stato del servizio viene modificato in **Attivo**.