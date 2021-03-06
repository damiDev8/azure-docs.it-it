---
title: Supporto per la lingua-Translator
titleSuffix: Azure Cognitive Services
description: Il convertitore di servizi cognitivi supporta le seguenti lingue per la traduzione da testo a testo tramite la traduzione di Neural Machine (NMT).
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 06/10/2020
ms.author: lajanuar
ms.openlocfilehash: 935a9e92de88c2519dc1a1042315d204e8f60099
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98919919"
---
# <a name="language-and-region-support-for-text-and-speech-translation"></a>Supporto della lingua e dell'area per la traduzione testuale e vocale

Usare Translator per tradurre in e da uno dei più di 70 linguaggi di traduzione del testo. La conversione di macchine neurali (NMT) è il nuovo standard per le traduzioni di macchine basate su intelligenza artificiale di alta qualità ed è disponibile come impostazione predefinita con V3 di Translator quando è disponibile un sistema neurale.

È anche possibile usare Translator insieme a Translator personalizzato per creare sistemi di traduzione neurale che comprendono la terminologia usata nella propria azienda e nel settore e con Microsoft Speech Service per aggiungere la traduzione vocale all'app.

[Altre informazioni sull'uso della traduzione automatica](https://www.microsoft.com/translator/mt.aspx)

## <a name="text-translation"></a>Traduzione testuale
La traduzione del testo è disponibile tramite l'operazione di conversione da o verso qualsiasi lingua disponibile in Translator. L'API offre anche il rilevamento del linguaggio usando l'operazione di rilevamento, la traslitterazione usando l'operazione transliterate e i dizionari bilingui usando le operazioni Dictionary Lookup e Dictionary examples. Di seguito sono elencate le lingue disponibili per ciascuna di queste operazioni. 

### <a name="translate"></a>Traduci

Translator supporta le seguenti lingue per la traduzione da testo a testo. 

[Visualizza la documentazione di riferimento per l'operazione di traduzione](reference/v3-0-translate.md)

| Linguaggio | Codice lingua |
|:-|:-:|
| Afrikaans | `af` |
| Arabo | `ar` |
| Assamese | `as` |
| Bengalese | `bn` |
| Bosniaco (latino) | `bs` |
| Bulgaro | `bg` |
| Cantonese (tradizionale) | `yue` |
| Catalano | `ca` |
| Cinese semplificato | `zh-Hans` |
| Cinese tradizionale | `zh-Hant` |
| Croato | `hr` |
| Ceco | `cs` |
| Dari | `prs` |
| Danese | `da` |
| Olandese | `nl` |
| Inglese | `en` |
| Estone | `et` |
| Figiano | `fj` |
| Filippino | `fil` |
| Finlandese | `fi` |
| Francese | `fr` |
| Francese (Canada) | `fr-ca` |
| Tedesco | `de` |
| Greco | `el` |
| Gujarati | `gu` |
| Creolo haitiano | `ht` |
| Ebraico | `he` |
| Hindi | `hi` |
| Hmong Daw | `mww` |
| Ungherese | `hu` |
| Islandese | `is` |
| Indonesiano | `id` |
| Inuktitut | `iu` |
| Irlandese | `ga` |
| Italiano | `it` |
| Giapponese | `ja` |
| Kannada | `kn` |
| Kazako | `kk` |
| Klingon | `tlh-Latn` |
| Klingon (plqaD) | `tlh-Piqd` |
| Coreano | `ko` |
| Curdo centrale | `ku` |
| Curdo settentrionale | `kmr` |
| Lettone | `lv` |
| Lituano | `lt` |
| Malgascio | `mg` |
| Malese | `ms` |
| Malayalam | `ml` |
| Maltese | `mt` |
| Maori | `mi` |
| Marathi | `mr` |
| Norvegese | `nb` |
| Odia | `or` |
| Pashto | `ps` |
| Persiano | `fa` |
| Polacco | `pl` |
| Portoghese (Brasile) | `pt` |
| Portoghese (Portogallo) | `pt-pt` |
| Punjabi | `pa` |
| Querétaro Otomi | `otq` |
| Rumeno | `ro` |
| Russo | `ru` |
| Samoano | `sm` |
| Serbo (alfabeto cirillico) | `sr-Cyrl` |
| Serbo (alfabeto latino) | `sr-Latn` |
| Slovacco | `sk` |
| Sloveno | `sl` |
| Spagnolo | `es` |
| Swahili | `sw` |
| Svedese | `sv` |
| Tahitiano | `ty` |
| Tamil | `ta` |
| Telugu | `te` |
| Thai | `th` |
| Tongano | `to` |
| Turco | `tr` |
| Ucraino | `uk` |
| Urdu | `ur` |
| Vietnamita | `vi` |
| Gallese | `cy` |
| Yucatec Maya | `yua` |

> [!NOTE]
> Il codice lingua `pt` predefinito è `pt-br` , portoghese (Brasile).

### <a name="detect"></a>Detect

Translator rileva le seguenti lingue per la conversione e la traslitterazione.

[Visualizza la documentazione di riferimento per l'operazione di rilevamento](reference/v3-0-detect.md)

| Linguaggio | Codice lingua |
|:-|:-:|
| Afrikaans | `af` |
| Arabo | `ar` |
| Bulgaro | `bg` |
| Catalano | `ca` |
| Cinese semplificato | `zh-Hans` |
| Cinese tradizionale | `zh-Hant` |
| Croato | `hr` |
| Ceco | `cs` |
| Danese | `da` |
| Olandese | `nl` |
| Inglese | `en` |
| Estone | `et` |
| Finlandese | `fi` |
| Francese | `fr` |
| Tedesco | `de` |
| Greco | `el` |
| Gujarati | `gu` |
| Creolo haitiano | `ht` |
| Ebraico | `he` |
| Hindi | `hi` |
| Ungherese | `hu` |
| Islandese | `is` |
| Indonesiano | `id` |
| Irlandese | `ga` |
| Italiano | `it` |
| Giapponese | `ja` |
| Klingon | `tlh-Latn` |
| Coreano | `ko` |
| Curdo centrale | `ku-Arab` |
| Lettone | `lv` |
| Lituano | `lt` |
| Malese | `ms` |
| Maltese | `mt` |
| Norvegese | `nb` |
| Pashto | `ps` |
| Persiano | `fa` |
| Polacco | `pl` |
| Portoghese | `pt` |
| Romeno | `ro` |
| Russo | `ru` |
| Serbo (alfabeto cirillico) | `sr-Cyrl` |
| Serbo (alfabeto latino) | `sr-Latn` |
| Slovacco | `sk` |
| Sloveno | `sl` |
| Spagnolo | `es` |
| Swahili | `sw` |
| Svedese | `sv` |
| Tahitiano | `ty` |
| Thai | `th` |
| Turco | `tr` |
| Ucraino | `uk` |
| Urdu | `ur` |
| Vietnamita | `vi` |
| Gallese | `cy` |
| Yucatec Maya | `yua` |

### <a name="transliterate"></a>Transliterate

Il metodo Transliterate supporta le lingue seguenti. Nella colonna "Verso/Da" il simbolo "<-->" indica che la lingua può essere traslitterata da o verso entrambi gli alfabeti elencati. Il simbolo "-->" indica che la lingua può essere traslitterata solo da un alfabeto all'altro.

[Visualizza la documentazione di riferimento per l'operazione transliterate](reference/v3-0-translate.md)


| Linguaggio    | Codice lingua | Script | A/da | Script|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Arabo | `ar` | Arabo `Arab` | <--> | Latino `Latn` |
| Assamese | `as` | Bengalese `Beng` | <--> | Latino `Latn` |
| Bengalese  | `bn` | Bengalese `Beng` | <--> | Latino `Latn` |
|Bielorusso| `be` | Cirillico `Cyrl`  | <--> | Latino `Latn` |
|Bulgaro| `bg` | Cirillico `Cyrl`  | <--> | Latino `Latn` |
| Cinese (semplificato) | `zh-Hans` | Cinese semplificato `Hans`| <--> | Latino `Latn` |
| Cinese (semplificato) | `zh-Hans` | Cinese semplificato `Hans`| <--> | Cinese tradizionale `Hant`|
| Cinese (tradizionale) | `zh-Hant` | Cinese tradizionale `Hant`| <--> | Latino `Latn` |
| Cinese (tradizionale) | `zh-Hant` | Cinese tradizionale `Hant`| <--> | Cinese semplificato `Hans` |
|Greco| `el` | Greco `Grek`  | <--> | Latino `Latn` |
| Gujarati | `gu`  | Gujarati `Gujr` | <--> | Latino `Latn` |
| Ebraico | `he` | Ebraico `Hebr` | <--> | Latino `Latn` |
| Hindi | `hi` | Devanagari `Deva` | <--> | Latino `Latn` |
| Giapponese | `ja` | Giapponese `Jpan` | <--> | Latino `Latn` |
| Kannada | `kn` | Kannada `Knda` | <--> | Latino `Latn` |
|Kazako| `kk` | Cirillico `Cyrl`  | <--> | Latino `Latn` |
|Coreano| `ko` | Coreano `Kore`  | <--> | Latino `Latn` |
|kirghiso| `ky` | Cirillico `Cyrl`  | <--> | Latino `Latn` |
|Macedone| `mk` | Cirillico `Cyrl`  | <--> | Latino `Latn` |
| Malayalam | `ml` | Malayalam `Mlym` | <--> | Latino `Latn` |
| Marathi | `mr` | Devanagari `Deva` | <--> | Latino `Latn` |
|Mongolo| `mn` | Cirillico `Cyrl`  | <--> | Latino `Latn` |
| Odia | `or` | Oriya `Orya` | <--> | Latino `Latn` |
|Persiano| `fa` | Arabo `Arab`  | <--> | Latino `Latn` |
| Punjabi | `pa` | Gurmukhi `Guru`  | <--> | Latino `Latn`  |
|Russo| `ru` | Cirillico `Cyrl`  | <--> | Latino `Latn` |
| Serbo (alfabeto cirillico) | `sr-Cyrl` | Cirillico `Cyrl`  | --> | Latino `Latn` |
| Serbo (alfabeto latino) | `sr-Latn` | Latino `Latn` | --> | Cirillico `Cyrl`|
|Sindhi| `sd` | Arabo `Arab`  | <--> | Latino `Latn` |
|Singalese| `si` | Singalese `Sinh`  | <--> | Latino `Latn` |
|Tagico| `tg` | Cirillico `Cyrl`  | <--> | Latino `Latn` |
| Tamil | `ta` | Tamil `Taml` | <--> | Latino `Latn` |
|Tatar| `tt` | Cirillico `Cyrl`  | <--> | Latino `Latn` |
| Telugu | `te` | Telugu `Telu` | <--> | Latino `Latn` |
| Thai | `th` | Thai `Thai` | --> | Latino `Latn` |
|Ucraino| `uk` | Cirillico `Cyrl`  | <--> | Latino `Latn` |
|Urdu| `ur` | Arabo `Arab`  | <--> | Latino `Latn` |

### <a name="dictionary"></a>Dizionario

Il dizionario supporta le lingue seguenti verso o dalla lingua inglese tramite i metodi Lookup ed Examples.

Vedere la documentazione di riferimento per le operazioni di [ricerca dizionario](reference/v3-0-dictionary-lookup.md) e [esempi di dizionario](reference/v3-0-dictionary-examples.md) .

| Linguaggio    | Codice lingua |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Arabo       | `ar`          |
| Bengalese      | `bn`          |
| Bosniaco (latino)      | `bs`          |
| Bulgaro      | `bg`          |
| Catalano      | `ca`          |
| Cinese semplificato      | `zh-Hans`          |
| Croato      | `hr`          |
| Ceco      | `cs`          |
| Danese      | `da`          |
| Olandese      | `nl`          |
| Estone      | `et`          |
| Finlandese      | `fi`          |
| Francese      | `fr`          |
| Tedesco      | `de`          |
| Greco      | `el`          |
| Creolo haitiano      | `ht`          |
| Ebraico      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Ungherese      | `hu`          |
| Islandese    | `is`  |
| Indonesiano      | `id`          |
| Italiano      | `it`          |
| Giapponese      | `ja`          |
| Klingon      | `tlh`          |
| Coreano      | `ko`          |
| Lettone      | `lv`          |
| Lituano      | `lt`          |
| Malese      | `ms`          |
| Maltese      | `mt`          |
| Norvegese      | `nb`          |
| Persiano      | `fa`          |
| Polacco      | `pl`          |
| Portoghese (Brasile)     | `pt`          |
| Romeno      | `ro`          |
| Russo      | `ru`          |
| Serbo (alfabeto latino)      | `sr-Latn`          |
| Slovacco     | `sk`          |
| Sloveno      | `sl`          |
| Spagnolo      | `es`          |
| Swahili      | `sw`          |
| Svedese      | `sv`          |
| Tamil      | `ta`          |
| Thai      | `th`          |
| Turco      | `tr`          |
| Ucraino      | `uk`          |
| Urdu      | `ur`          |
| Vietnamita      | `vi`          |
| Gallese      | `cy`          |

### <a name="access-the-translator-language-list-programmatically"></a>Accesso all'elenco di lingue di conversione a livello di codice

È possibile recuperare un elenco di lingue supportate per Translator usando il metodo languages. È possibile visualizzare l'elenco in base alla funzionalità, al codice della lingua, nonché al nome della lingua in inglese o in qualsiasi altra lingua supportata. L'elenco viene aggiornato automaticamente dal servizio Microsoft Translator quando diventano disponibili nuove lingue.

[Visualizzare la documentazione di riferimento dell'operazione Languages](reference/v3-0-languages.md)

## <a name="customization"></a>Personalizzazione

Le lingue seguenti sono disponibili per la personalizzazione da o verso l'inglese utilizzando il [convertitore personalizzato](https://aka.ms/CustomTranslator).

| Linguaggio    | Codice lingua |
|:----------- |:-------------:|
|Afrikaans| `af`|
| Arabo       | `ar`          |
| Bengalese      | `bn`          |
| Bosniaco (latino)      | `bs`          |
| Bulgaro      | `bg`          |
|Catalano|   `ca`    |
| Cinese semplificato      | `zh-Hans`          |
|Cinese tradizionale|   `zh-Hant`   |
| Croato      | `hr`          |
| Ceco      | `cs`          |
| Danese      | `da`          |
| Olandese      | `nl`          |
| Inglese    | `en`     |
| Estone      | `et`          |
|Figiano|    `fj`    |
|Filippino|  `fil`   |
| Finlandese      | `fi`          |
| Francese      | `fr`          |
| Tedesco      | `de`          |
| Greco      | `el`          |
| Gujarati| `gu`    |
| Ebraico      | `he`          |
| Hindi      | `hi`          |
| Ungherese      | `hu`          |
| Islandese | `is` |
| Indonesiano|   `id`    |
| Irlandese | `ga`  |
| Italiano      | `it`          |
| Giapponese      | `ja`          |
|Kannada|`kn`|
| Coreano      | `ko`          |
| Lettone      | `lv`          |
| Lituano      | `lt`          |
| Malgascio| `mg`    |
| Malese|    `ms` |
|Maltese|   `mt`    |
| Maori| `mi`  |
| Marathi| `mr`  |
| Norvegese      | `nb`          |
| Persiano      | `fa`          |
| Polacco      | `pl`          |
| Portoghese (Brasile) | `pt` |
| Punjabi|`pa`|
| Romeno      | `ro`          |
| Russo      | `ru`          |
| Samoano|   `sm`    |
| Serbo (alfabeto latino)      | `sr-Latn`          |
| Slovacco     | `sk`          |
| Sloveno      | `sl`          |
| Spagnolo      | `es`          |
| Swahili|  `sw`    |
| Svedese      | `sv`          |
|Tahitiano|  `ty`    |
| Thai      | `th`          |
|Tongano|    `to`    |
| Turco      | `tr`          |
| Ucraino      | `uk`          |
| Urdu| `ur`    |
| Vietnamita      | `vi`          |
| Gallese | `cy` |

## <a name="speech-translation"></a>Traduzione vocale
La traduzione vocale è disponibile tramite Translator con il servizio di riconoscimento vocale di servizi cognitivi. Per ulteriori informazioni sull'utilizzo della traduzione vocale e per visualizzare tutte le [Opzioni di lingua disponibili](../speech-service/language-support.md), vedere la [documentazione relativa al servizio di sintesi vocale](../speech-service/index.yml) .

### <a name="speech-to-text"></a>Riconoscimento vocale
Converte la voce in testo in modo da tradurla nella lingua del testo desiderata. Il riconoscimento vocale viene usato per la traduzione di testo vocale o per la traduzione vocale quando viene usato in combinazione con la sintesi vocale.

| Linguaggio    |
|:----------- |
|Arabo|
|Cantonese (tradizionale)|
|Catalano|
|Cinese semplificato|
|Cinese tradizionale|
|Danese|
|Olandese|
|Inglese|
|Finlandese|
|Francese|
|Francese (Canada)|
|Tedesco|
|Gujarati|
|Hindi|
|Italiano|
|Giapponese|
|Coreano|
|Marathi|
|Norvegese|
|Polacco|
|Portoghese (Brasile)|
|Portoghese (Portogallo)|
|Russo|
|Spagnolo|
|Svedese|
|Tamil|
|Telugu|
|Thai|
|Turco|

### <a name="text-to-speech"></a>Sintesi vocale
Converte il testo in sintesi vocale. Il riconoscimento vocale viene usato per aggiungere l'output acustico dei risultati della traduzione o per la traduzione vocale quando viene usato con la sintesi vocale. 

| Linguaggio |
|:-|
| Arabo |
| Bulgaro |
| Cantonese (tradizionale) |
| Catalano |
| Cinese semplificato |
| Cinese tradizionale |
| Croato |
| Ceco |
| Danese |
| Olandese |
| Inglese |
| Finlandese |
| Francese |
| Francese (Canada) |
| Tedesco |
| Greco |
| Ebraico |
| Hindi |
| Ungherese |
| Indonesiano |
| Italiano |
| Giapponese |
| Coreano |
| Malese |
| Norvegese |
| Polacco |
| Portoghese (Brasile) |
| Portoghese (Portogallo) |
| Romeno |
| Russo |
| Slovacco |
| Sloveno |
| Spagnolo |
| Svedese |
| Tamil |
| Telugu |
| Thai |
| Turco |
| Vietnamita |

## <a name="view-the-language-list-on-the-microsoft-translator-website"></a>Visualizzare l'elenco delle lingue nel sito Web Microsoft Translator

Per una rapida panoramica delle lingue, il sito Web Microsoft Translator Mostra tutte le lingue supportate da Translator per la traduzione testuale e il servizio vocale per la traduzione vocale. L'elenco non include informazioni specifiche per sviluppatori, ad esempio i codici delle lingue.

[Visualizzare l'elenco delle lingue](https://www.microsoft.com/translator/languages.aspx)
