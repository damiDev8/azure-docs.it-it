---
title: Aggiornare l'API Ricerca notizie Bing da v5 a v7
titleSuffix: Azure Cognitive Services
description: Identifica le parti dell'applicazione Ricerca notizie Bing che è necessario aggiornare per usare la versione 7.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: scottwhi
ms.openlocfilehash: a114cb24d79189f9e370fae1962f60ca97241d90
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/30/2020
ms.locfileid: "96351368"
---
# <a name="news-search-api-upgrade-guide"></a>Guida all'aggiornamento dell'API Ricerca notizie

> [!WARNING]
> Le API Ricerca Bing sono state trasferite da Servizi cognitivi ai servizi di Ricerca Bing. A partire dal **30 ottobre 2020**, è necessario effettuare il provisioning di tutte le nuove istanze di Ricerca Bing seguendo la procedura documentata [qui](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> Le API Ricerca Bing di cui viene effettuato il provisioning con Servizi cognitivi saranno supportate per i prossimi tre anni oppure fino alla data di fine del contratto Enterprise, se precedente.
> Per le istruzioni sulla migrazione, vedere [Servizi di Ricerca Bing](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Questa guida all'aggiornamento indica le differenze tra la versione 5 e la versione 7 dell'API Ricerca notizie Bing. Usare questa guida per identificare le parti dell'applicazione da aggiornare per usare la versione 7.

## <a name="breaking-changes"></a>Modifiche di rilievo

### <a name="endpoints"></a>Endpoint

- Il numero di versione dell'endpoint è stato modificato da v5 a v7. Ad esempio: `https://api.cognitive.microsoft.com/bing/v7.0/news/search`.

### <a name="error-response-objects-and-error-codes"></a>Oggetti di risposta di errore e codici di errore

- Tutte le richieste non riuscite devono ora includere un oggetto `ErrorResponse` nel corpo della risposta.

- All'oggetto `Error` sono stati aggiunti i campi seguenti.  
  - `subCode`&mdash;Partiziona il codice di errore in bucket discreti, se possibile
  - `moreDetails`&mdash;Include informazioni aggiuntive sull'errore descritto nel campo `message`

- I codici di errore della versione 5 sono stati sostituiti con i possibili valori `code` e `subCode` seguenti.

|Codice|Sottocodice|Descrizione
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|Bing restituisce ServerError ogni volta che si verifica una delle condizioni del sottocodice. La risposta include questi errori se il codice di stato HTTP è 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Bloccato|Bing restituisce InvalidRequest ogni volta che una parte della richiesta non è valida, ad esempio quando non è specificato un parametro obbligatorio o un valore di parametro non è valido.<br/><br/>Se l'errore è ParameterMissing o ParameterInvalidValue, il codice di stato HTTP è 400.<br/><br/>Se l'errore è HttpNotAllowed, il codice di stato HTTP è 410.
|RateLimitExceeded||Bing restituisce RateLimitExceeded ogni volta che si supera la quota di query al secondo (QPS) o di query al mese (QPM).<br/><br/>Bing restituisce il codice di stato HTTP 429 se è stata superata la quota di query al secondo e 403 se è stata superata la quota di query al mese.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing restituisce InvalidAuthorization quando non riesce ad autenticare il chiamante, ad esempio quando l'intestazione `Ocp-Apim-Subscription-Key` non è specificata o la chiave di sottoscrizione non è valida.<br/><br/>La ridondanza si verifica se si specificano più metodi di autenticazione.<br/><br/>Se l'errore è InvalidAuthorization, il codice di stato HTTP è 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing restituisce InsufficientAuthorization quando il chiamante non ha le autorizzazioni per accedere alla risorsa. Ciò può verificarsi se la chiave di sottoscrizione è stata disabilitata o è scaduta. <br/><br/>Se l'errore è InsufficientAuthorization, il codice di stato HTTP è 403.

- L'elenco seguente associa i codici di errore precedenti ai nuovi codici. Se è stata definita una dipendenza dai codici di errore della versione 5, aggiornare il codice in modo appropriato.

|Codice della versione 5|Codice e sottocodice della versione 7
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Disabled|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
NotImplemented|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Bloccato|InvalidRequest.Blocked

### <a name="object-changes"></a>Modifiche agli oggetti

- Il campo `contractualRules` è stato aggiunto all'oggetto [NewsArticle](/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle). Il campo `contractualRules` contiene un elenco di regole che è necessario seguire (ad esempio sull'attribuzione di articoli). È necessario applicare l'attribuzione specificata in `contractualRules` invece di usare `provider`. L'articolo include `contractualRules` solo quando la risposta dell'[API Ricerca Web](../bing-web-search/overview.md) contiene un riscontro relativo a notizie.

## <a name="non-breaking-changes"></a>Modifiche che non causano un'interruzione

### <a name="query-parameters"></a>Parametri di query

- Il valore Products è stato aggiunto come possibile valore su cui può essere impostato il parametro di query [category](/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#category). Vedere [Categories By Markets](/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference) (Categorie per mercato).

- È stato aggiunto il parametro di query [SortBy](/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#sortby), che restituisce gli argomenti di tendenza ordinati per data con il più recente all'inizio.

- È stato aggiunto il parametro di query [Since](/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#since), che restituisce gli argomenti di tendenza individuati da Bing in corrispondenza o dopo il timestamp del periodo Unix specificato.

### <a name="object-changes"></a>Modifiche agli oggetti

- Il campo `mentions` è stato aggiunto all'oggetto [NewsArticle](/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle). Il campo `mentions` contiene un elenco di entità (persone o luoghi) trovate nell'articolo.

- Il campo `video` è stato aggiunto all'oggetto [NewsArticle](/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle). Il campo `video` contiene un video correlato all'articolo di notizie. Il video è un oggetto \<iframe\> che è possibile incorporare o un'anteprima di movimento.

- Il campo `sort` è stato aggiunto all'oggetto [News](/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#news). Il campo `sort` mostra l'ordinamento degli articoli. Ad esempio, gli articoli sono ordinati per pertinenza (impostazione predefinita) o per data.

- È stato aggiunto l'oggetto [SortValue](/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#sortvalue), che definisce un ordinamento. Il campo `isSelected` indica se la risposta ha usato l'ordinamento. Se **true**, la risposta ha usato l'ordinamento. Se `isSelected` è **false**, è possibile usare l'URL nel campo `url` per richiedere un ordinamento diverso.