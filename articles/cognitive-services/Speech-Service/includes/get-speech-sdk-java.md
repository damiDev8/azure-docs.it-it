---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 2ab636679e59536a2ddfaa8603dc2da45811cd2f
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99213987"
---
:::row:::
    :::column span="3":::
        Java SDK per Android è incluso in un pacchetto <a href="https://developer.android.com/studio/projects/android-library" target="_blank">AAR (libreria Android) <span class="docon docon-navigate-external x-hidden-focus"></span> </a>, che include le librerie necessarie e le autorizzazioni necessarie per Android. È ospitato in un repository Maven in `https://csspeechstorage.blob.core.windows.net/maven/` come pacchetto `com.microsoft.cognitiveservices.speech:client-sdk:1.15.0`.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Java" src="https://docs.microsoft.com/media/logos/logo_java.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

Per usare il pacchetto dal progetto Android Studio apportare le modifiche seguenti:

1. Nel file *Build. Gradle* a livello di progetto aggiungere quanto segue alla `repositories` sezione:
  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

2. Nel file *Build. Gradle* a livello di modulo aggiungere quanto segue alla `dependencies` sezione:
  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.15.0'
  ```

Java SDK fa inoltre parte di [Speech Devices SDK](../speech-devices-sdk.md).

#### <a name="additional-resources"></a>Risorse aggiuntive

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android" target="_blank">Codice sorgente di avvio rapido Java specifico per Android <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/jre" target="_blank">Codice sorgente avvio rapido Java Runtime (JRE) <span class="docon docon-navigate-external x-hidden-focus"></span></a>
