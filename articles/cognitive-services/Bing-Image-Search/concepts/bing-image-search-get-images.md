---
title: Ottenere immagini dal Web - API Ricerca immagini Bing
titleSuffix: Azure Cognitive Services
description: Usare l'API Ricerca immagini Bing per cercare e trovare immagini pertinenti nel Web.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: AB1B9898-C94A-4B59-91A1-8623C94BA3D4
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 988a1332d03bf2c9563ab0576f7a20ee6b0615aa
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/30/2020
ms.locfileid: "96342057"
---
# <a name="get-images-from-the-web-with-the-bing-image-search-api"></a>Ottenere immagini dal Web con l'API Ricerca immagini Bing

> [!WARNING]
> Le API Ricerca Bing sono state trasferite da Servizi cognitivi ai servizi di Ricerca Bing. A partire dal **30 ottobre 2020**, è necessario effettuare il provisioning di tutte le nuove istanze di Ricerca Bing seguendo la procedura documentata [qui](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> Le API Ricerca Bing di cui viene effettuato il provisioning con Servizi cognitivi saranno supportate per i prossimi tre anni oppure fino alla data di fine del contratto Enterprise, se precedente.
> Per le istruzioni sulla migrazione, vedere [Servizi di Ricerca Bing](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Quando si usa l'API REST Ricerca immagini Bing, è possibile ottenere dal Web immagini correlate al termine di ricerca inviando la richiesta GET seguente:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Usare il parametro di query [q](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#query) per il termine di ricerca con codifica URL. Se ad esempio si immette *sailing dinghies*, impostare `q` su `sailing+dinghies` o `sailing%20dinghies`.

> [!IMPORTANT]
> * Tutte le richieste devono essere eseguite da un server, non da un client.
> * Se è la prima volta che si chiama un'API di ricerca Bing, non includere l'intestazione dell'ID client. Includere l'ID client solo se in precedenza è già stata chiamata un'API Bing che ha restituito un ID client per la combinazione utente e dispositivo.

## <a name="get-images-from-a-specific-web-domain"></a>Ottenere immagini da un dominio Web specifico

Per ottenere immagini da un dominio specifico, usare l'operatore query [site:](/previous-versions/bing/search/ff795613(v=msdn.10)).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

> [!NOTE]
> Le risposte alle query tramite l'operatore `site:` potrebbero includere contenuti per adulti indipendentemente dall'impostazione [safeSearch](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#safesearch). Usare `site:` solo se si è a conoscenza del contenuto nel dominio.

## <a name="filter-images"></a>Filtrare le immagini

 Per impostazione predefinita, l'API Ricerca immagini restituisce tutte le immagini pertinenti alla query. Per filtrare le immagini restituite da Bing, ad esempio se sono necessarie solo immagini con uno sfondo trasparente o di una dimensione specifica, usare i parametri di query seguenti:

* [aspetto](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#aspect): filtra le immagini in base alle proporzioni, ad esempio immagini standard o a schermo intero.
* [color](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#color): consente di filtrare le immagini in base al colore dominante o nero e bianco.
* [aggiornamento](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#freshness): filtra le immagini in base all'età, ad esempio le immagini individuate da Bing nell'ultima settimana.
* [altezza](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#height), [larghezza](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#width): filtra le immagini per larghezza e altezza.
* [imageContent](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagecontent): filtra le immagini in base al contenuto, ad esempio immagini che mostrano solo la faccia di una persona.
* [ImageType](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagetype): filtra le immagini per tipo, ad esempio clip art, gif animate o sfondi trasparenti.
* [licenza](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#license): consente di filtrare le immagini in base al tipo di licenza associato al sito.
* [dimensioni](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#size): filtra le immagini in base alle dimensioni, ad esempio immagini piccole fino a 200x200 pixel.

Per ottenere immagini da un dominio specifico, usare l'operatore query [site:](/previous-versions/bing/search/ff795613(v=msdn.10)).

L'esempio seguente illustra come ottenere immagini di piccole dimensioni da ContosoSailing.com individuate da Bing la scorsa settimana.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="bing-image-search-response-format"></a>Formato della risposta di Ricerca immagini Bing

Il messaggio restituito da Bing contiene una risposta [Images](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) contenente un elenco di immagini che Servizi cognitivi hanno determinato come pertinenti alla query. Ogni oggetto [Immagine](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#image) contenuto nell'elenco include le informazioni seguenti sull'immagine: l'URL, le dimensioni, le proporzioni, il formato di codifica, un URL di un'anteprima dell'immagine e le dimensioni dell'anteprima.

> [!NOTE]
> * È necessario visualizzare le immagini nell'ordine indicato nella risposta.
> * Poiché i formati e i parametri degli URL sono soggetti a modifica senza preavviso, usare gli URL così come sono. Non acquisire dipendenze da formati o parametri degli URL se non diversamente specificato.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "en.contoso.org\/wiki\/File:Rich_Passage...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "imageInsightsToken": "ccid_GNarK7ma*mid_CCF85447ADA6...",
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    },
    "imageId": "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
    "accentColor": "376094"
},
```

Quando si chiama l'API Ricerca immagini Bing, Bing restituisce un elenco di risultati. L'elenco è un subset del numero totale di risultati pertinenti alla query. Il campo `totalEstimatedMatches` della risposta contiene una stima del numero di immagini disponibili da visualizzare. Per informazioni dettagliate su come sfogliare le immagini rimanenti, vedere [Sfogliare le immagini](../../bing-web-search/paging-search-results.md).

## <a name="next-steps"></a>Passaggi successivi

Se non si è ancora provato l'API Ricerca immagini Bing, consultare una [Guida introduttiva](../quickstarts/csharp.md). Se si cercano contenuti più complessi, provare l'esercitazione che consente di creare un'[app Web a pagina singola](../tutorial-bing-image-search-single-page-app.md).