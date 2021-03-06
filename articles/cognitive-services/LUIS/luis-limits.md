---
title: Limiti-LUIS
description: Questo articolo illustra i limiti di LUIS (Language Understanding) dei Servizi cognitivi di Azure. LUIS presenta diverse aree di limiti. Il limite di modelli controlla gli Intent, le entità e le funzionalità di LUIS. I limiti di quota si basano sul tipo di chiave. La combinazione di tasti controlla il sito Web di LUIS.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 06/04/2020
ms.openlocfilehash: 41423ce34a62dfdbd5b9a60f683a2366a94d1bfd
ms.sourcegitcommit: 8f0803d3336d8c47654e119f1edd747180fe67aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2021
ms.locfileid: "97976793"
---
# <a name="limits-for-your-luis-model-and-keys"></a>Limiti per il modello e le chiavi LUIS
LUIS presenta diverse aree di limite. Il primo è il [limite del modello](#model-limits), che controlla gli Intent, le entità e le funzionalità in Luis. La seconda area è [limiti di quota](#key-limits) basata sul tipo di chiave. Una terza area dei limiti è la [combinazione di tasti](#keyboard-controls) per il controllo del sito Web Luis. Una quarta area è data dal [mapping dell'area globale](luis-reference-regions.md) tra il sito Web di creazione LUIS e le API dell'[endpoint LUIS](luis-glossary.md#endpoint).

<a name="model-boundaries"></a>

## <a name="model-limits"></a>Limiti del modello

Se l'app supera i limiti del modello LUIS, provare a usare un'app [Luis dispatch](luis-concept-enterprise.md#dispatch-tool-and-model) o a usare un [contenitore Luis](luis-container-howto.md).

|Area|Limite|
|--|:--|
| [Nome app][luis-get-started-create-app] | *Numero max predefinito di caratteri |
| Applicazioni| 500 applicazioni per ogni risorsa di creazione di Azure |
| [Test in batch][batch-testing]| 10 set di dati, 1000 espressioni per ogni set di dati|
| Elenco esplicito | 50 per applicazione|
| Entità esterne | Nessun limite |
| [Intent][intents]|500 per applicazione: 499 Intent personalizzati e la finalità obbligatoria _None_ .<br>L'applicazione [basata su dispatch](https://aka.ms/dispatch-tool) ha origini di invio 500 corrispondenti.|
| [Elencare entità](./luis-concept-entity-types.md) | Padre: 50, figlio: 20.000 elementi. Il nome canonico è il *numero max predefinito di caratteri. I sinonimi non hanno restrizioni di lunghezza. |
| [entità e ruoli di Machine Learning](./luis-concept-entity-types.md):<br> composito<br>semplice<br>ruolo entità|Limite di 100 entità padre o di 330 entità, a seconda del limite che viene raggiunto per primo dall'utente. Un ruolo viene conteggiato come entità ai fini di questo limite. Un esempio è costituito da un composito con un'entità semplice, che ha 2 ruoli: 1 composito + 1 Simple + 2 Roles = 4 delle entità 330.<br>Le sottoentità possono essere annidate fino a 5 livelli, con un massimo di 10 elementi figlio per livello.|
|Modello come funzionalità| Numero massimo di modelli che possono essere utilizzati come funzionalità per un modello specifico come 10 modelli. Il numero massimo di elenchi di frasi usati come funzionalità per un modello specifico come un elenco di 10 frasi.|
| [Anteprima-entità elenco dinamico](./luis-migration-api-v3.md)|2 elenchi di ~ 1K per richiesta dell'endpoint di stima di query|
| [Modelli](luis-concept-patterns.md)|500 criteri per ogni applicazione.<br>Il criterio può contenere al massimo 400 caratteri.<br>3 entità pattern.any per criterio<br>Il criterio può contenere al massimo 2 testi facoltativi annidati|
| [Pattern.any](./luis-concept-entity-types.md)|100 per applicazione, 3 entità pattern.any per criterio |
| [Elenco frasi][phrase-list]|500 elenchi di frasi. 10 elenchi di frasi globali a causa del modello come limite di funzionalità. L'elenco di frasi non intercambiabili è costituito da un massimo di 5.000 frasi. L'elenco di frasi intercambiabili è costituito da un massimo di 50.000 frasi. Numero massimo di frasi totali per ogni applicazione di 500.000 frasi.|
| [Entità predefinite](./howto-add-prebuilt-models.md) | nessun limite|
| [Entità di espressione regolare](./luis-concept-entity-types.md)|20 entità<br>È consentito un numero massimo di 500 caratteri. per ogni criterio di entità di espressione regolare|
| [Ruoli](./luis-concept-entity-types.md)|300 ruoli per ogni applicazione. 10 per entità|
| [Espressione][utterances] | 500 caratteri<br><br>Se il testo è più lungo di questo limite di caratteri, è necessario segmentare l'espressione prima di eseguire l'input di LUIS e si riceveranno singole risposte Intent per segmento. Ci sono ovvie interruzioni che è possibile usare, ad esempio segni di punteggiatura e pause lunghe nel discorso.|
| [Esempi di espressioni][utterances] | 15.000 per applicazione: non esiste alcun limite al numero di enunciati per finalità<br><br>Se è necessario eseguire il training dell'applicazione con altri esempi, usare un approccio del modello di [invio](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Dispatch) . È possibile eseguire il training di singole app LUIS (note come app figlio per l'app di distribuzione padre) con uno o più Intent e quindi eseguire il training di un'app di distribuzione che campiona da ogni espressione dell'app LUIS figlio per indirizzare la richiesta di stima all'app figlio corretta. |
| [Versions](./luis-concept-app-iteration.md) (Versioni)| 100 versioni per applicazione |
| [Nome della versione][luis-how-to-manage-versions] | 128 caratteri |

*Il numero max predefinito di caratteri è 50.

<a name="intent-and-entity-naming"></a>

## <a name="name-uniqueness"></a>Univocità del nome

I nomi degli oggetti devono essere univoci rispetto ad altri oggetti dello stesso livello.

|Oggetti|Restrizioni|
|--|--|
|Finalità, entità|Tutte le finalità e i nomi di entità devono essere univoci in una versione di un'app.|
|Componenti dell'entità ML|Tutti i componenti dell'entità Machine Learning (entità figlio) devono essere univoci all'interno di tale entità per i componenti allo stesso livello.|
|Funzionalità | Tutte le funzionalità denominate, ad esempio gli elenchi di frasi, devono essere univoche all'interno di una versione di un'app.|
|Ruoli entità|Tutti i ruoli in un componente entità o entità devono essere univoci quando sono allo stesso livello di entità (padre, figlio, nipote e così via).|

## <a name="object-naming"></a>Denominazione degli oggetti

Non usare i caratteri seguenti nei nomi seguenti.

|Oggetto|Escludi caratteri|
|--|--|
|Finalità, entità e nomi di ruoli|`:`<br>`$` <br> `&`|
|Nome della versione|`\`<br> `/`<br> `:`<br> `?`<br> `&`<br> `=`<br> `*`<br> `+`<br> `(`<br> `)`<br> `%`<br> `@`<br> `$`<br> `~`<br> `!`<br> `#`|

## <a name="resource-usage-and-limits"></a>Limiti e utilizzo delle risorse

La lingua comprende risorse separate, un tipo per la creazione e un tipo per l'esecuzione di query sull'endpoint di stima. Per altre informazioni sulle differenze tra i tipi di chiave, vedere [Chiavi di creazione e di endpoint per query di stima in LUIS](luis-how-to-azure-subscription.md).

<a name="key-limits"></a>

### <a name="authoring-resource-limits"></a>Creazione dei limiti delle risorse

Utilizzare il _tipo_, `LUIS.Authoring` , quando si filtrano le risorse nell'portale di Azure. LUIS limita le applicazioni 500 per ogni risorsa di creazione di Azure.

|Creazione di una risorsa|Creazione di TPS|
|--|--|
|Starter|1 milione/mese, 5/secondo|
|Livello gratuito F0 |1 milione/mese, 5/secondo|

* TPS = transazioni al secondo

[Altre informazioni sui prezzi.][pricing]

### <a name="query-prediction-resource-limits"></a>Limiti delle risorse di stima delle query

Utilizzare il _tipo_, `LUIS` , quando si filtrano le risorse nell'portale di Azure. La risorsa LUIS query PREDICTION endpoint, usata nel runtime, è valida solo per le query di endpoint.

|Risorsa di stima query|Eseguire query su TPS|
|--|--|
|Livello gratuito F0 |10.000/mese, 5/secondo|
|S0-livello standard|50/secondo|

### <a name="sentiment-analysis"></a>Analisi del sentiment

L' [integrazione dell'analisi dei sentimenti](luis-how-to-publish-app.md#enable-sentiment-analysis), che fornisce informazioni sui sentimenti, viene fornita senza richiedere un'altra risorsa di Azure.

### <a name="speech-integration"></a>Integrazione riconoscimento vocale

L' [integrazione vocale](../speech-service/how-to-recognize-intents-from-speech-csharp.md) fornisce le richieste di endpoint 1000 per ogni costo unitario.

[Altre informazioni sui prezzi.][pricing]

## <a name="keyboard-controls"></a>Controlli tastiera

|Input da tastiera | Descrizione |
|--|--|
|Controllo + E|passa dai token alle entità nell'elenco delle espressioni|

## <a name="website-sign-in-time-period"></a>Periodo di accesso del sito Web

Il periodo di tempo di accesso è di **60 minuti**. Trascorso questo tempo, si verificherà il seguente errore. Sarà necessario eseguire nuovamente l'accesso.

[luis-get-started-create-app]: ./luis-get-started-create-app.md
[batch-testing]: ./luis-concept-test.md#batch-testing
[intents]: ./luis-concept-intent.md
[phrase-list]: ./luis-concept-feature.md
[utterances]: ./luis-concept-utterance.md
[luis-how-to-manage-versions]: ./luis-how-to-manage-versions.md
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
