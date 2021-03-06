---
title: "Guida introduttiva: Ottenere informazioni dettagliate sulle immagini con l'API REST e Java - Ricerca visiva Bing"
titleSuffix: Azure Cognitive Services
description: Informazioni su come caricare un'immagine nell'API Ricerca visiva Bing e ottenere informazioni dettagliate su di essa.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.custom: devx-track-java
ms.author: scottwhi
ms.openlocfilehash: 8974dbef47938460e5a1b32d4f6004a9fd35f9d2
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499085"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-java"></a>Guida introduttiva: Ottenere informazioni dettagliate sulle immagini usando l'API REST Ricerca visiva Bing e Java

> [!WARNING]
> Le API Ricerca Bing sono state trasferite da Servizi cognitivi ai servizi di Ricerca Bing. A partire dal **30 ottobre 2020**, è necessario effettuare il provisioning di tutte le nuove istanze di Ricerca Bing seguendo la procedura documentata [qui](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> Le API Ricerca Bing di cui viene effettuato il provisioning con Servizi cognitivi saranno supportate per i prossimi tre anni oppure fino alla data di fine del contratto Enterprise, se precedente.
> Per le istruzioni sulla migrazione, vedere [Servizi di Ricerca Bing](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Usare questa guida di avvio rapido per effettuare la prima chiamata all'API Ricerca visiva Bing. Quest'applicazione Java carica un'immagine nell'API e visualizza le informazioni restituite. Anche se l'applicazione è scritta in Java, l'API è un servizio Web RESTful compatibile con la maggior parte dei linguaggi di programmazione.

## <a name="prerequisites"></a>Prerequisiti

* [Java Development Kit (JDK) 7 o 8](/azure/developer/java/fundamentals/java-jdk-long-term-support)
* [Libreria Java Gson](https://github.com/google/gson)
* [Apache HttpComponents](https://hc.apache.org/downloads.cgi)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Creare e inizializzare un progetto

1. Creare un nuovo progetto Java nell'ambiente di sviluppo integrato o nell'editor preferito e importare le librerie seguenti:

    ```java
    import java.util.*;
    import java.io.*;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    
    // HttpClient libraries
    
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.ContentType;
    import org.apache.http.entity.mime.MultipartEntityBuilder;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClientBuilder;
    ```

2. Creare variabili per l'endpoint dell'API, la chiave di sottoscrizione e il percorso dell'immagine. Per il valore di `endpoint`, è possibile usare l'endpoint globale nel codice seguente o l'endpoint del [sottodominio personalizzato](../../../cognitive-services/cognitive-services-custom-subdomains.md) visualizzato nel portale di Azure per la risorsa.

    ```java
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "your-key-here";
    static String imagePath = "path-to-your-image";
    ```

    
3. Quando si carica un'immagine locale, i dati del modulo devono includere l'intestazione `Content-Disposition`. Impostare il parametro `name` su "image" e il parametro `filename` sul nome file dell'immagine. Il contenuto del modulo include i dati binari dell'immagine. La dimensione massima delle immagini che è possibile caricare è 1 MB.
    
    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

## <a name="create-the-json-parser"></a>Creare il parser JSON

Creare un metodo per rendere più leggibile la risposta JSON dell'API usando `JsonParser`.

```java
public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
```

## <a name="construct-the-search-request-and-query"></a>Costruire la richiesta di ricerca e la query

1. Nel metodo main dell'applicazione creare un client HTTP usando `HttpClientBuilder.create().build();`.

    ```java
    CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    ```

2. Creare un oggetto `HttpEntity` per caricare l'immagine nell'API.

    ```java
    HttpEntity entity = MultipartEntityBuilder
        .create()
        .addBinaryBody("image", new File(imagePath))
        .build();
    ```

3. Creare un oggetto `httpPost` con l'endpoint e impostare l'intestazione in modo da usare la chiave di sottoscrizione.

    ```java
    HttpPost httpPost = new HttpPost(endpoint);
    httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
    httpPost.setEntity(entity);
    ```

## <a name="receive-and-process-the-json-response"></a>Ricevere ed elaborare la risposta JSON

1. Usare il metodo `HttpClient.execute()` per inviare una richiesta all'API e archiviare la risposta in un oggetto `InputStream`.
    
    ```java
    HttpResponse response = httpClient.execute(httpPost);
    InputStream stream = response.getEntity().getContent();
    ```

2. Archiviare la stringa JSON e visualizzare la risposta.

    ```java
    String json = new Scanner(stream).useDelimiter("\\A").next();
    System.out.println("\nJSON Response:\n");
    System.out.println(prettify(json));
    ```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Creare un'app Web a pagina singola con Ricerca visiva](../tutorial-bing-visual-search-single-page-app.md)