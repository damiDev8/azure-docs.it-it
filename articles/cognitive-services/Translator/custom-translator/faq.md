---
title: Domande frequenti-convertitore personalizzato
titleSuffix: Azure Cognitive Services
description: Questo articolo contiene le risposte alle domande più frequenti sul traduttore personalizzato di servizi cognitivi di Azure.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: lajanuar
ms.topic: reference
ms.openlocfilehash: 001314817b0c18a8023258d01bcfb02eaaffe79b
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98895833"
---
# <a name="custom-translator-frequently-asked-questions"></a>Domande frequenti sul traduttore personalizzato

Questo articolo contiene le risposte alle domande frequenti su [Custom Translator](https://portal.customtranslator.azure.ai).

## <a name="what-are-the-current-restrictions-in-custom-translator"></a>Quali sono le attuali restrizioni di Custom Translator?

Sono previsti limiti e restrizioni relativamente alle dimensioni dei file, al training dei modelli e alla distribuzione dei modelli. Tenere presenti queste restrizioni quando si configura il training per la creazione di un modello in Custom Translator.

- I file inviati devono avere dimensioni inferiori a 100 MB.
- Non sono supportati dati monolingue.

## <a name="when-should-i-request-deployment-for-a-translation-system-that-has-been-trained"></a>Quando è consigliabile richiedere la distribuzione di un sistema di traduzione sottoposto a training?

È possibile che siano necessarie più sessioni di training per creare un sistema di traduzione ottimale per un progetto. Se il punteggio BLEU e/o i risultati dei test non sono soddisfacenti, è possibile provare a usare più dati di training o dati filtrati più attentamente. È importante progettare il set di ottimizzazione e il set di test con estrema precisione e attenzione, in modo che siano completamente rappresentativi della terminologia e dello stile del materiale che si vuole tradurre. È possibile invece essere meno rigidi nella composizione dei dati di training ed effettuare esperimenti con varie opzioni. Richiedere una distribuzione del sistema se si è soddisfatti delle traduzioni presenti nei risultati dei test di sistema, se non si hanno altri dati da aggiungere al training per migliorare il sistema sottoposto a training e se si vuole accedere al modello sottoposto a training tramite API.

## <a name="how-many-trained-systems-can-be-deployed-in-a-project"></a>Quanti sistemi sottoposti a training possono essere distribuiti in un progetto?

È possibile distribuire un solo sistema sottoposto a training per ogni progetto. È possibile che siano necessarie più sessioni di training per creare un sistema di traduzione appropriato per il progetto e si consiglia di richiedere la distribuzione di un modello di training in grado di offrire risultati ottimali. È possibile determinare la qualità del training in base al punteggio BLEU (più alto è il punteggio, migliore è il risultato) ed è opportuno confrontarsi con i revisori prima di decidere se la qualità della traduzione è adeguata per la distribuzione.

## <a name="when-can-i-expect-my-trainings-to-be-deployed"></a>Quando vengono distribuiti i miei dati di training?

La distribuzione richiede in genere meno di un'ora.

## <a name="how-do-you-access-a-deployed-system"></a>Come si accede a un sistema distribuito?

È possibile accedere ai modelli distribuiti tramite l'API Traduzione testuale V3 di Microsoft specificando il CategoryID. Altre informazioni sull'API Traduzione testuale sono disponibili nella pagina Web [Informazioni di riferimento sulle API](../reference/v3-0-reference.md).

## <a name="how-do-i-skip-alignment-and-sentence-breaking-if-my-data-is-already-sentence-aligned"></a>Com'è possibile evitare le fasi di allineamento e segmentazione delle frasi se i miei file sono già allineati?

Custom Translator non esegue l'allineamento e la segmentazione delle frasi con file TMX e file di testo con estensione `.align`. I file `.align` consentono agli utenti di ignorare i processi di allineamento e segmentazione delle frasi di Custom Translator per i file che sono già perfettamente allineati e non richiedono ulteriori elaborazioni. È consigliabile usare l'estensione `.align` solo per i file perfettamente allineati.

Se il numero di frasi estratte non corrisponde ai due file con lo stesso nome di base, Custom Translator eseguirà comunque il processo di allineamento sui file `.align`.

## <a name="i-tried-uploading-my-tmx-but-it-says-document-processing-failed"></a>Ho provato a caricare il TMX, ma è stato visualizzato il messaggio "elaborazione del documento non riuscita"


Verificare che il file TMX sia conforme alle specifiche TMX 1.4b illustrate all'indirizzo <https://www.gala-global.org/tmx-14b>.