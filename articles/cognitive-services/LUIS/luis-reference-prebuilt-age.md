---
title: Entità precompilata Age-LUIS
titleSuffix: Azure Cognitive Services
description: Questo articolo contiene informazioni sull'entità predefinita età in Language Understanding, ovvero LUIS.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/04/2019
ms.openlocfilehash: 4e1a6b1b6fb99a2786de06e89960e8480a5e1338
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91532925"
---
# <a name="age-prebuilt-entity-for-a-luis-app"></a>Entità predefinita age per un'app LUIS
L'entità predefinita età acquisisce il valore dell'età sia numericamente che in termini di giorni, settimane, mesi e anni. Poiché è già stato eseguito il training per questa entità, non è necessario aggiungere espressioni di esempio contenenti l'età per le finalità dell'applicazione. L'entità età è supportata in [molte impostazioni cultura](luis-reference-prebuilt-entities.md).

## <a name="types-of-age"></a>Tipi di età
Age è gestito dal repository GitHub del [testo dei riconoscitori](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3)

## <a name="resolution-for-prebuilt-age-entity"></a>Risoluzione per l'entità età predefinita



#### <a name="v3-response"></a>[Risposta V3](#tab/V3)

Il codice JSON seguente è con il `verbose` parametro impostato su `false` :

```json
"entities": {
    "age": [
        {
            "number": 90,
            "unit": "Day"
        }
    ]
}
```
#### <a name="v3-verbose-response"></a>[Risposta dettagliata V3](#tab/V3-verbose)
Il codice JSON seguente è con il `verbose` parametro impostato su `true` :

```json
"entities": {
    "age": [
        {
            "number": 90,
            "unit": "Day"
        }
    ],
    "$instance": {
        "age": [
            {
                "type": "builtin.age",
                "text": "90 day old",
                "startIndex": 2,
                "length": 10,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor"
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[Risposta V2](#tab/V2)

L'esempio seguente illustra la risoluzione dell'entità **builtin.age**.

```json
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
```
* * *

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sull'[endpoint di previsione V3](luis-migration-api-v3.md).

Informazioni sulle entità [valuta](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md) e [dimensioni](luis-reference-prebuilt-dimension.md).
