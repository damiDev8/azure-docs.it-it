---
title: Endpoint di Ricerca personalizzata Bing
titleSuffix: Azure Cognitive Services
description: Creazione di esperienze di ricerca personalizzate per gli argomenti a cui si è interessati. Gli utenti visualizzano i risultati della ricerca personalizzati per il contenuto di cui si occupano.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 35dd147574eb7820debba55b105deed001be80fb
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/30/2020
ms.locfileid: "96338487"
---
# <a name="custom-search"></a>Ricerca personalizzata

> [!WARNING]
> Le API Ricerca Bing sono state trasferite da Servizi cognitivi ai servizi di Ricerca Bing. A partire dal **30 ottobre 2020**, è necessario effettuare il provisioning di tutte le nuove istanze di Ricerca Bing seguendo la procedura documentata [qui](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> Le API Ricerca Bing di cui viene effettuato il provisioning con Servizi cognitivi saranno supportate per i prossimi tre anni oppure fino alla data di fine del contratto Enterprise, se precedente.
> Per le istruzioni sulla migrazione, vedere [Servizi di Ricerca Bing](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
Ricerca personalizzata Bing consente di creare esperienze di ricerca personalizzata per gli argomenti a cui si è interessati. Gli utenti visualizzano risultati della ricerca specifici per il contenuto a cui sono interessati anziché scorrere i risultati della ricerca che possono includere contenuto irrilevante.

## <a name="custom-search-endpoint"></a>Endpoint di Ricerca personalizzata
Per ottenere risultati tramite l'API Ricerca personalizzata Bing, inviare una richiesta `GET` all'endpoint seguente. Usare le intestazioni e i parametri URL per definire altre specifiche.

Endpoint: restituisce suggerimenti per la ricerca come risultati JSON pertinenti per l'input dell'utente definito da `?q=""`.
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Per gli esempi che descrivono come configurare le origini di Ricerca personalizzata, vedere l'[esercitazione](./tutorials/custom-search-web-page.md). Per maggiori dettagli su intestazioni, parametri, codici di mercato, oggetti di risposta, errori e altro ancora, vedere le informazioni di riferimento per l'[API Ricerca personalizzata Bing v7](/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference).

## <a name="custom-search-response-json"></a>Codice JSON della risposta di Ricerca personalizzata
Una richiesta di ricerca personalizzata restituisce i risultati come oggetti JSON. Vedere [Oggetti di risposta](/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#response-objects). 

## <a name="custom-autosuggest"></a>Suggerimenti automatici personalizzati
L'API Suggerimenti automatici personalizzati consente di inviare un termine di query di ricerca parziale a Bing e ottenere un elenco di query suggerite che è possibile configurare. Con Suggerimenti automatici personalizzati è possibile aggiungere i suggerimenti restituiti dall'API e facoltativamente specificare se includere i suggerimenti generati da Bing.

## <a name="custom-autosuggest-endpoint"></a>Endpoint di Suggerimenti automatici personalizzati
Per richiedere i suggerimenti di query personalizzati, inviare una richiesta GET a:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions
```  

Per informazioni sulla definizione di suggerimenti personalizzati, vedere [Definire i suggerimenti di ricerca personalizzati](define-custom-suggestions.md).

## <a name="custom-image-search"></a>Ricerca immagini personalizzata
L'API Ricerca immagine personalizzata consente di inviare una query di ricerca a Bing e ottenere un elenco di immagini pertinenti dall'istanza di Ricerca personalizzata.

## <a name="custom-image-search-endpoint"></a>Endpoint di Ricerca immagini personalizzata
Per richiedere immagini all'istanza di Ricerca personalizzata, inviare una richiesta GET all'URL seguente:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/images/search
```

Per informazioni sulla configurazione di un'istanza di Ricerca personalizzato, vedere [Configurare un'esperienza di ricerca personalizzata](./define-your-custom-view.md).

## <a name="next-steps"></a>Passaggi successivi
Le API **Bing** supportano azioni di ricerca che restituiscono risultati in base al tipo. Tutti gli endpoint di ricerca restituiscono risultati come oggetti della risposta JSON.  Tutti gli endpoint supportano query che restituiscono una posizione e/o una lingua specifica in base a longitudine, latitudine e raggio di ricerca.

Per informazioni complete sui parametri supportati da ogni endpoint, vedere le pagine di riferimento per ogni tipo.
Per esempi di richieste di base eseguite con l'API Ricerca personalizzata, vedere le [guide introduttive per Ricerca personalizzata](/azure/cognitive-services/bing-custom-search/)