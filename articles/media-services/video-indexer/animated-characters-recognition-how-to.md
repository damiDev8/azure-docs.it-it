---
title: Rilevamento dei caratteri animati con Video Indexer procedura
titleSuffix: Azure Media Services
description: In questa procedura viene illustrato come utilizzare il rilevamento di caratteri animati con Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.custom: references_regions
ms.topic: how-to
ms.date: 12/07/2020
ms.author: juliako
ms.openlocfilehash: 29dbdb896d1a6063fec277afa33327c84cb2f0cd
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98880928"
---
# <a name="use-the-animated-character-detection-preview-with-portal-and-api"></a>Usare il rilevamento dei caratteri animati (anteprima) con il portale e l'API 

Servizi multimediali di Azure Video Indexer supporta il rilevamento, il raggruppamento e il riconoscimento di caratteri nel contenuto animato. questa funzionalità è disponibile tramite l'API portale di Azure e tramite. Vedere [questo argomento introduttivo](animated-characters-recognition.md) .

Questo articolo illustra come usare il rilevamento dei caratteri animati con il portale di Azure e l'API Video Indexer.

## <a name="use-the-animated-character-detection-with-portal"></a>Usare il rilevamento dei caratteri animati con il portale 

Negli account di valutazione l'integrazione Visione personalizzata viene gestita da Video Indexer, è possibile iniziare a creare e utilizzare il modello di caratteri animati. Se si usa l'account di prova, è possibile ignorare la sezione seguente ("Connetti l'account Visione personalizzata").

### <a name="connect-your-custom-vision-account-paid-accounts-only"></a>Connetti il tuo account di Visione personalizzata (solo account a pagamento)

Se si è proprietari di un account Video Indexer a pagamento, è necessario prima connettere un account di Visione personalizzata. Se non si dispone già di un account Visione personalizzata, crearne uno. Per ulteriori informazioni, vedere [visione personalizzata](../../cognitive-services/custom-vision-service/overview.md).

> [!NOTE]
> Entrambi gli account devono trovarsi nella stessa area. L'integrazione di Visione personalizzata non è attualmente supportata nell'area Giappone.

Gli account a pagamento che hanno accesso al proprio account di Visione personalizzata possono visualizzare i modelli e le immagini con tag. Altre informazioni su come [migliorare il classificatore in visione personalizzata](../../cognitive-services/custom-vision-service/getting-started-improving-your-classifier.md). 

Si noti che il training del modello deve essere eseguito solo tramite Video Indexer e non tramite il sito Web di Visione personalizzata. 

#### <a name="connect-a-custom-vision-account-with-api"></a>Connettere un account di Visione personalizzata con l'API 

Seguire questa procedura per connettere l'account Visione personalizzata al Video Indexer o per modificare l'account Visione personalizzata attualmente connesso a Video Indexer:

1. Passare a [www.customvision.ai](https://www.customvision.ai) e accedere.
1. Copiare le chiavi per le risorse di training e di stima:

    > [!NOTE]
    > Per fornire tutte le chiavi è necessario disporre di due risorse separate in Visione personalizzata, una per il training e una per la stima.
1. Fornire altre informazioni:

    * Endpoint 
    * ID della risorsa di stima
1. Esplorare e accedere al [video Indexer](https://vi.microsoft.com/).
1. Fare clic sul punto interrogativo nell'angolo superiore destro della pagina e scegliere **riferimento API**.
1. Assicurarsi di aver effettuato la sottoscrizione a gestione API facendo clic sulla scheda **prodotti** . Se è stata connessa un'API, è possibile continuare con il passaggio successivo, in caso contrario, sottoscrivere. 
1. Nel portale per sviluppatori fare clic sul **riferimento completo all'API** e selezionare **operazioni**.  
1. Selezionare **connetti visione personalizzata account (anteprima)** e fare clic su **prova**.
1. Compilare i campi obbligatori e il token di accesso e fare clic su **Send (Invia**). 

    Per ulteriori informazioni su come ottenere il token di accesso Video Indexer passare al [portale per sviluppatori](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Account-Access-Token?)e vedere la [documentazione pertinente](video-indexer-use-apis.md#obtain-access-token-using-the-authorization-api).  
1. Quando la chiamata restituisce 200 risposta OK, l'account è connesso.
1. Per verificare la connessione, passare al portale [video Indexer](https://vi.microsoft.com/)):
1. Fare clic sul pulsante **personalizzazione modello di contenuto** nell'angolo superiore destro.
1. Passare alla scheda **caratteri animati** .
1. Quando si fa clic su Gestisci modelli in Visione personalizzata, si verrà trasferiti all'account Visione personalizzata appena connesso.

> [!NOTE]
> Attualmente sono supportati solo i modelli creati tramite Video Indexer. I modelli creati tramite Visione personalizzata non saranno disponibili. Inoltre, la procedura consigliata consiste nel modificare i modelli creati tramite Video Indexer solo tramite la piattaforma Video Indexer, dal momento che le modifiche apportate tramite Visione personalizzata possono provocare risultati imprevisti.

### <a name="create-an-animated-characters-model"></a>Creare un modello di caratteri animati

1. Passare al sito Web di [Video Indexer](https://vi.microsoft.com/) ed eseguire l'accesso.
1. Per personalizzare un modello nell'account, selezionare il pulsante **personalizzazione modello di contenuto** a sinistra della pagina.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/content-model-customization/content-model-customization.png" alt-text="Personalizzare il modello di contenuto in Video Indexer":::
1. Passare alla scheda **caratteri animati** nella sezione personalizzazione del modello.
1. Fare clic su **Aggiungi modello**.
1. Assegnare un nome al modello e fare clic su invio per salvare il nome.

> [!NOTE]
> La procedura consigliata consiste nel disporre di un modello di visione artificiale personalizzato per ogni serie animata. 

### <a name="index-a-video-with-an-animated-model"></a>Indicizzare un video con un modello animato

Per la formazione iniziale, caricare almeno due video. Ogni deve essere preferibilmente più lungo di 15 minuti, prima di prevedere un modello di riconoscimento valido. Se si hanno episodi più brevi, è consigliabile caricare almeno 30 minuti di contenuto video prima del training. In questo modo sarà possibile unire i gruppi che appartengono allo stesso carattere da scene e sfondi diversi, aumentando quindi la probabilità che rilevi il carattere negli episodi seguenti indicizzati. Per eseguire il training di un modello su più video (episodi), è necessario indicizzarli tutti con lo stesso modello di animazione. 

1. Fare clic sul pulsante **upload (carica** ).
1. Scegliere un video da caricare (da un file o un URL).
1. Fare clic su **Opzioni avanzate**.
1. In **persone/caratteri animati** scegliere **modelli di animazione**.
1. Se si dispone di un modello, questo verrà scelto automaticamente e se si dispone di più modelli è possibile scegliere quello pertinente dal menu a discesa.
1. Fare clic su carica.
1. Una volta indicizzato il video, i caratteri rilevati vengono visualizzati nella sezione **caratteri animati** del riquadro **Insights** .

Prima di contrassegnare e eseguire il training del modello, tutti i caratteri animati verranno denominati "Unknown #X". Dopo aver eseguito il training del modello, verranno riconosciuti anche.

### <a name="customize-the-animated-characters-models"></a>Personalizzare i modelli di caratteri animati

1. Denominare i caratteri in Video Indexer.

    1. Dopo il gruppo di caratteri creato dal modello, è consigliabile esaminare questi gruppi in Visione personalizzata. 
    1. Per contrassegnare un carattere animato nel video, passare alla scheda **Insights**   e fare clic sul pulsante **Edit (modifica**) nell'   angolo superiore destro della finestra. 
    1. Nel riquadro **Insights**   fare clic su uno dei caratteri animati rilevati e modificare i relativi nomi da "Unknown #X" a un nome temporaneo o al nome assegnato in precedenza al carattere. 
    1. Dopo aver digitato il nuovo nome, fare clic sull'icona di controllo accanto al nuovo nome. In questo modo il nuovo nome viene salvato nel modello in Video Indexer. 
1. Solo account a pagamento: esaminare i gruppi in Visione personalizzata 

    > [!NOTE]
    > Gli account a pagamento che hanno accesso al proprio account di Visione personalizzata possono visualizzare i modelli e le immagini con tag. Altre informazioni su come [migliorare il classificatore in visione personalizzata](../../cognitive-services/custom-vision-service/getting-started-improving-your-classifier.md). È importante sottolineare che il training del modello deve essere eseguito solo tramite Video Indexer, come descritto in questo topi, e non tramite il sito Web di Visione personalizzata. 

    1. Passare alla pagina **modelli personalizzati** in video Indexer e scegliere la scheda **caratteri animati** . 
    1. Fare clic sul pulsante modifica per il modello su cui si sta lavorando per gestirlo in Visione personalizzata. 
    1. Esaminare ogni gruppo di caratteri: 

        * Se il gruppo contiene immagini non correlate, è consigliabile eliminarle nel sito Web Visione personalizzata. 
        * Se sono presenti immagini che appartengono a un carattere diverso, modificare il tag per queste immagini specifiche facendo clic sull'immagine, aggiungendo il tag destro ed eliminando il tag errato. 
        * Se il gruppo non è corretto, ovvero contiene principalmente immagini non di tipo carattere o immagini di più caratteri, è possibile eliminare in Visione personalizzata sito Web o in Video Indexer Insights. 
        * L'algoritmo di raggruppamento a volte suddivide i caratteri in gruppi diversi. È quindi consigliabile assegnare a tutti i gruppi che appartengono allo stesso carattere lo stesso nome (in Video Indexer Insights), che determinerà immediatamente la visualizzazione di tutti questi gruppi come nel sito Web Visione personalizzata. 
    1. Una volta che il gruppo è stato perfezionato, verificare che il nome iniziale a cui è stato aggiunto il nome rispecchi il carattere del gruppo. 
1. Eseguire il training del modello 

    1. Al termine della modifica di tutti i nomi desiderati, è necessario eseguire il training del modello. 
    1. Una volta eseguito il training di un carattere nel modello, esso verrà riconosciuto come il video successivo indicizzato con il modello. 
    1. Aprire la pagina Personalizzazione e fare clic sulla scheda **caratteri animati**   , quindi fare clic sul pulsante di **Train** per eseguire il training del modello. Per garantire la connessione tra video 
    
Indicizzatore e modello, non eseguire il training del modello nel sito Web Visione personalizzata (gli account a pagamento possono accedere al sito Web Visione personalizzata), solo in Video Indexer. Una volta eseguito il training, qualsiasi video che verrà indicizzato o reindicizzato con tale modello riconoscerà i caratteri sottoposti a training. 

## <a name="delete-an-animated-character-and-the-model"></a>Eliminare un carattere animato e il modello

1. Eliminare un carattere animato.

    1. Per eliminare un carattere animato in Insights video, passare alla scheda **Insights** e fare clic sul pulsante Edit ( **modifica** ) nell'angolo superiore destro della finestra.
    1. Scegliere il carattere animato, quindi fare clic sul pulsante **Elimina** sotto il nome.

    > [!NOTE]
    > Questa operazione eliminerà le informazioni da questo video, ma non influirà sul modello.
1. Eliminare un modello.

    1. Fare clic sul pulsante **personalizzazione modello di contenuto** nel menu in alto e passare alla scheda **caratteri animati** .
    1. Fare clic sull'icona con i puntini di sospensione a destra del modello che si desidera eliminare e quindi sul pulsante Elimina.
    
    * Account a pagamento: il modello verrà disconnesso da Video Indexer e non sarà possibile riconnetterlo.
    * Account di valutazione: il modello verrà eliminato anche dalla visione doganale. 
    
        > [!NOTE]
        > In un account di valutazione è presente un solo modello che è possibile usare. Dopo averla eliminata, non è possibile eseguire il training di altri modelli.

## <a name="use-the-animated-character-detection-with-api"></a>Usare il rilevamento dei caratteri animati con l'API 

1. Connettere un account Visione personalizzata.

    Se si è proprietari di un account Video Indexer a pagamento, è necessario prima connettere un account di Visione personalizzata. <br/>
    Se non si dispone già di un account Visione personalizzata, crearne uno. Per ulteriori informazioni, vedere [visione personalizzata](../../cognitive-services/custom-vision-service/overview.md).

    [Connettere l'account visione personalizzata usando l'API](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Connect-Custom-Vision-Account?tags=&pattern=&groupBy=tag).
1. Creare un modello di caratteri animati.

    Usare l'API [Crea modello di animazione](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Animation-Model?&groupBy=tag) .
1. Indicizzare o reindicizzare un video.

    Usare l'API di [reindicizzazione](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-Index-Video?) . 
1. Personalizzare i modelli di caratteri animati.

    Usare l'API del [modello di animazione del training](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Train-Animation-Model?&groupBy=tag) .

### <a name="view-the-output"></a>Visualizzare l'output

Vedere i caratteri animati nel file JSON generato.

```json
"animatedCharacters": [
    {
    "videoId": "e867214582",
    "confidence": 0,
    "thumbnailId": "00000000-0000-0000-0000-000000000000",
    "seenDuration": 201.5,
    "seenDurationRatio": 0.3175,
    "isKnownCharacter": true,
    "id": 4,
    "name": "Bunny",
    "appearances": [
        {
            "startTime": "0:00:52.333",
            "endTime": "0:02:02.6",
            "startSeconds": 52.3,
            "endSeconds": 122.6
        },
        {
            "startTime": "0:02:40.633",
            "endTime": "0:03:16.6",
            "startSeconds": 160.6,
            "endSeconds": 196.6
        },
    ]
    },
]
```

## <a name="limitations"></a>Limitazioni

* Attualmente, la funzionalità di identificazione dell'animazione non è supportata in East-Asia area.
* I caratteri che sembrano essere piccoli o lontani nel video potrebbero non essere identificati correttamente se la qualità del video è insufficiente.
* Si consiglia di usare un modello per set di caratteri animati, ad esempio per una serie animata.

## <a name="next-steps"></a>Passaggi successivi

[Panoramica di Video Indexer](video-indexer-overview.md)