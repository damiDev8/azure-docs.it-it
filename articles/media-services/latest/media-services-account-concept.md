---
title: Gestire gli account di servizi multimediali di Azure V3
description: Per avviare le operazioni di gestione, crittografia, codifica, analisi e streaming dei contenuti multimediali in Azure, è necessario creare un account di Servizi multimediali. Questo articolo illustra come gestire gli account di servizi multimediali di Azure V3.
services: media-services
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 11/05/2020
ms.author: inhenkel
ms.openlocfilehash: 49cdee15923123ced03c2c6bc750e1b98dd42887
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98896377"
---
# <a name="manage-azure-media-services-v3-accounts"></a>Gestire gli account di servizi multimediali di Azure V3

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Per avviare le operazioni di gestione, crittografia, codifica, analisi e streaming dei contenuti multimediali in Azure, è necessario creare un account di Servizi multimediali. Quando si crea un account di Servizi multimediali, è necessario specificare il nome di una risorsa dell'account di Archiviazione di Azure. L'account di archiviazione specificato è collegato all'account personale di Servizi multimediali. L'account di Servizi multimediali e tutti gli account di archiviazione associati devono far parte della stessa sottoscrizione di Azure. Per altre informazioni, vedere [account di archiviazione](storage-account-concept.md).

[!INCLUDE [account creation note](./includes/note-2020-05-01-account-creation.md)]

## <a name="moving-a-media-services-account-between-subscriptions"></a>Trasferimento di un account di servizi multimediali tra sottoscrizioni 

Se è necessario spostare un account di servizi multimediali in una nuova sottoscrizione, è necessario innanzitutto spostare l'intero gruppo di risorse contenente l'account di servizi multimediali nella nuova sottoscrizione. È necessario spostare tutte le risorse collegate: account di archiviazione di Azure, profili della rete CDN di Azure e così via. Per altre informazioni, vedere [spostare le risorse in un gruppo di risorse o una sottoscrizione nuovi](../../azure-resource-manager/management/move-resource-group-and-subscription.md). Come per tutte le risorse in Azure, il completamento dello spostamento del gruppo di risorse può richiedere del tempo.

> [!NOTE]
> Media Services V3 supporta il modello multi-tenant.

### <a name="considerations"></a>Considerazioni

* Creare backup di tutti i dati nell'account prima di eseguire la migrazione a una sottoscrizione diversa.
* È necessario arrestare tutti gli endpoint di streaming e le risorse di streaming live. Gli utenti non saranno in grado di accedere al contenuto per la durata dello spostamento del gruppo di risorse. 

> [!IMPORTANT]
> Non avviare l'endpoint di streaming finché il passaggio non viene completato correttamente.

### <a name="troubleshoot"></a>Risolvere problemi

Se un account di servizi multimediali o un account di archiviazione di Azure associato diventa "disconnesso" dopo lo spostamento del gruppo di risorse, provare a ruotare le chiavi dell'account di archiviazione. Se la rotazione delle chiavi dell'account di archiviazione non risolve lo stato "disconnesso" dell'account di servizi multimediali, inserire una nuova richiesta di supporto dal menu "supporto e risoluzione dei problemi" nell'account di servizi multimediali.  

## <a name="next-steps"></a>Passaggi successivi

[Creare un account](./create-account-howto.md)
