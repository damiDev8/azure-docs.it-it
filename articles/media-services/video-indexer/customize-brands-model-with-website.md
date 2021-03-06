---
title: Personalizzare un modello di Marchi con il sito Web di Video Indexer
titleSuffix: Azure Media Services
description: Informazioni su come personalizzare un modello di marchi con il sito Web Video Indexer.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 12/15/2019
ms.author: kumud
ms.openlocfilehash: a2de9dbb479f43d6b646cd9f6cf604d6a08c8b6a
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97586109"
---
# <a name="customize-a-brands-model-with-the-video-indexer-website"></a>Personalizzare un modello di Marchi con il sito Web di Video Indexer

Video Indexer supporta il rilevamento del marchio dal parlato e dal testo visivo durante l'indicizzazione e la reindicizzazione di contenuto audio e video. La funzionalità di rilevamento del marchio identifica citazioni di prodotti, servizi e aziende suggerite dal database dei marchi di Bing. Se, ad esempio, Microsoft è citata in contenuti video o audio o se viene visualizzata in un testo visivo in un video, Video Indexer lo rileva come marchio nel contenuto.

Un modello di marchi personalizzato consente di:

- Selezionare se si desidera che Video Indexer rilevi i marchi dal database dei marchi Bing.
- Selezionare questa opzione se si desidera che Video Indexer escludere determinati marchi dal rilevamento (creando essenzialmente un elenco di marchi negato).
- Selezionare se si desidera che Video Indexer includa i marchi che devono far parte del modello che potrebbero non trovarsi nel database dei marchi di Bing (essenzialmente creando un elenco di marchi Accept).

Per una panoramica dettagliata, vedere questa [Panoramica](customize-brands-model-overview.md).

È possibile usare il sito Web di Video Indexer per creare, usare e modificare modelli di Marchi personalizzati rilevati in un video, come descritto in questo argomento. È anche possibile usare l'API, come descritto in [Personalizzare il modello di Marchi usando le API](customize-brands-model-with-api.md).

> [!NOTE]
> Se il video è stato indicizzato prima dell'aggiunta di un marchio, è necessario reindicizzarlo. Nel menu a discesa associato al video sarà presente l'elemento **REINDEX** . Selezionare **Opzioni avanzate**  ->  **categorie del marchio** e selezionare **tutti i marchi**.

## <a name="edit-brands-model-settings"></a>Modificare le impostazioni del modello di marchi

È possibile scegliere se rilevare o meno i marchi dal database dei marchi di Bing. Per impostare questa opzione, è necessario modificare le impostazioni del modello di marchi. A tale scopo, seguire questa procedura:

1. Accedere al sito Web di [video Indexer](https://www.videoindexer.ai/) ed eseguire l'accesso.
1. Per personalizzare un modello nell'account, selezionare il pulsante **personalizzazione modello di contenuto** a sinistra della pagina.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/content-model-customization/content-model-customization.png" alt-text="Personalizzare il modello di contenuto in Video Indexer":::
1. Per modificare i marchi, selezionare la scheda **Marchi**.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/customize-brand-model/customize-brand-model.png" alt-text="Screenshot mostra la scheda marchi della finestra di dialogo personalizzazione modello di contenuto":::
1. Selezionare l'opzione **Mostra marchi suggeriti da Bing** se si vuole che video Indexer rilevi i marchi suggeriti da Bing. se non è possibile, lasciare l'opzione deselezionata.

## <a name="include-brands-in-the-model"></a>Includere i marchi nel modello

La sezione **Includi marchi** rappresenta le marche personalizzate che si desidera rilevare video indexer, anche se non sono suggerite da Bing.  

### <a name="add-a-brand-to-include-list"></a>Aggiungere un marchio da includere nell'elenco

1. Selezionare **+ Crea nuovo marchio**.

    Specificare un nome (obbligatorio), una categoria (facoltativa), una descrizione (facoltativa) e un URL di riferimento (facoltativo).
    Il campo categoria è pensato per facilitare l'aggiunta di tag ai marchi. Questo campo viene visualizzato come *tag* del marchio quando si usano le API di Video Indexer. Ad esempio, il marchio "Azure" può essere contrassegnato o classificato come "Cloud".

    Il campo URL di riferimento può essere qualsiasi sito Web di riferimento per il marchio (ad esempio un collegamento alla pagina di Wikipedia).

2. Selezionare **Save (Salva** ). si noterà che il marchio è stato aggiunto all'elenco **Includi marchi** .

### <a name="edit-a-brand-on-the-include-list"></a>Modificare un marchio nell'elenco di inclusione

1. Selezionare l'icona della matita accanto al marchio che si desidera modificare.

    È possibile aggiornare la categoria, la descrizione o l'URL di riferimento di un marchio. Non è possibile modificare il nome di un marchio perché i nomi di marchio sono univoci. Se è necessario modificare il nome del marchio, eliminare l'intero marchio (vedere la sezione successiva) e creare un nuovo marchio con il nuovo nome.

2. Selezionare il pulsante **Aggiorna** per aggiornare il marchio con le nuove informazioni.

### <a name="delete-a-brand-on-the-include-list"></a>Eliminare un marchio nell'elenco di inclusione

1. Selezionare l'icona del cestino accanto al marchio che si vuole eliminare.
2. Selezionare **Elimina** e il marchio non verrà più visualizzato nell'elenco *Includi marchi* .

## <a name="exclude-brands-from-the-model"></a>Escludere un marchio dal modello

La sezione **Escludi marchi** rappresenta le marche che non si desidera rilevare video Indexer.

### <a name="add-a-brand-to-exclude-list"></a>Aggiungere un elenco di marchi da escludere

1. Selezionare **+ Crea nuovo marchio.**

    Specificare un nome (obbligatorio) e una categoria (facoltativa).

2. Selezionare **Save (Salva** ). si noterà che il marchio è stato aggiunto all'elenco *Escludi marchi* .

### <a name="edit-a-brand-on-the-exclude-list"></a>Modificare un marchio nell'elenco di esclusione

1. Selezionare l'icona della matita accanto al marchio che si desidera modificare.

    È possibile aggiornare solo la categoria di un marchio. Non è possibile modificare il nome di un marchio perché i nomi di marchio sono univoci. Se è necessario modificare il nome del marchio, eliminare l'intero marchio (vedere la sezione successiva) e creare un nuovo marchio con il nuovo nome.

2. Selezionare il pulsante **Aggiorna** per aggiornare il marchio con le nuove informazioni.

### <a name="delete-a-brand-on-the-exclude-list"></a>Eliminare un marchio nell'elenco di esclusione

1. Selezionare l'icona del cestino accanto al marchio che si vuole eliminare.
2. Selezionare **Elimina** e il marchio non verrà più visualizzato nell'elenco *Escludi marchi* .

## <a name="next-steps"></a>Passaggi successivi

[Personalizzare il modello di Marchi tramite API](customize-brands-model-with-api.md)
