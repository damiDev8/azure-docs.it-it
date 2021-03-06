---
title: Asset in servizi multimediali di Azure
description: Informazioni sulle risorse e sul modo in cui vengono usate da servizi multimediali di Azure.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18
ms.openlocfilehash: 5159432107e60f6c21bcf70e0bbc9a9e2123a728
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98897697"
---
# <a name="assets-in-azure-media-services-v3"></a>Asset in servizi multimediali di Azure V3

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

In servizi multimediali di Azure, un [Asset](/rest/api/media/assets) è un concetto di base. È il punto in cui vengono inseriti i supporti (ad esempio, tramite il caricamento o l'inserimento Live), i supporti di output (dall'output di un processo) e la pubblicazione di supporti da (per il flusso). 

Un asset viene mappato a un contenitore BLOB nell' [account di archiviazione di Azure](storage-account-concept.md) e i file nell'asset vengono archiviati come BLOB in blocchi in tale contenitore. Gli asset contengono informazioni sui file digitali archiviati in archiviazione di Azure (inclusi video, audio, immagini, raccolte di anteprime, tracce di testo e file di didascalia chiusi).

Servizi multimediali supporta i livelli BLOB quando l'account usa l'archiviazione Utilizzo generico v2 (GPv2). Con GPv2, è possibile spostare i file in uno [spazio di archiviazione ad accesso sporadico o archivio](../../storage/blobs/storage-blob-storage-tiers.md). Storage **Archive** è adatto per l'archiviazione di file di origine quando non è più necessario, ad esempio dopo la codifica.

Il livello di archiviazione **archivio** è consigliato solo per file di origine di dimensioni molto estese già codificati e con l'output del processo di codifica inserito in un contenitore BLOB di output. I BLOB nel contenitore di output che si vuole associare a un asset e usare per eseguire lo streaming o l'analisi del contenuto devono esistere **in un livello di archiviazione** **ad** accesso frequente o sporadico.

## <a name="naming"></a>Denominazione 

### <a name="assets"></a>Asset

I nomi degli asset devono essere univoci. I nomi di risorsa di servizi multimediali V3, ad esempio asset, processi, trasformazioni, sono soggetti a Azure Resource Manager vincoli di denominazione. Per altre informazioni, vedere [convenzioni di denominazione](media-services-apis-overview.md#naming-conventions).

### <a name="blobs"></a>BLOB

I nomi di file/BLOB all'interno di un asset devono rispettare i [requisiti del nome del BLOB](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) e i [requisiti del nome NTFS](/windows/win32/fileio/naming-a-file). Questi requisiti sono necessari perché i file possano essere copiati dall'archiviazione BLOB in un disco NTFS locale per l'elaborazione.

## <a name="next-steps"></a>Passaggi successivi

[Panoramica di servizi multimediali](media-services-overview.md)

## <a name="see-also"></a>Vedi anche

[Differenze tra Servizi multimediali v2 e v3](migrate-v-2-v-3-migration-introduction.md)
