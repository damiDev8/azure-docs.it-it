---
title: includere file
description: includere file
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.date: 12/29/2020
ms.subservice: language-understanding
ms.topic: include
ms.custom: include file
ms.author: aahi
ms.reviewer: roy-har
ms.openlocfilehash: 7aa2fba6ef551a745ccaf5b00f36021b9d8680ce
ms.sourcegitcommit: 1140ff2b0424633e6e10797f6654359947038b8d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/30/2020
ms.locfileid: "97820498"
---
1. Selezionare [pizza-app-for-luis-v6.json](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-app-for-luis-v6.json) per aprire la pagina di GitHub per il file `pizza-app-for-luis.json`.
1. Toccare e tenere premuto a lungo o fare clic con il pulsante destro del mouse sul pulsante **Raw** e selezionare **Salva collegamento come** per salvare `pizza-app-for-luis.json` nel computer.
1. Accedere al [portale LUIS](https://www.luis.ai).
1. Selezionare [App personali](https://www.luis.ai/applications).
1. Nella pagina **App personali**, selezionare **+ Nuova app di conversazione**.
1. Selezionare **Importa come JSON**.
1. Nella finestra di dialogo **Importa nuova app** selezionare il pulsante **Scegli file**.
1. Selezionare il file `pizza-app-for-luis.json` scaricato e quindi selezionare **Apri**.
1. Nel campo **Nome** della finestra di dialogo **Importa nuova app** immettere un nome per l'app Pizza, quindi selezionare il pulsante **Fatto**.

L'app verrà importata.

Se viene visualizzata una finestra di dialogo **Come creare un'app LUIS efficace**, chiudere la finestra di dialogo.

## <a name="train-and-publish-the-pizza-app"></a>Eseguire il training dell’app Pizza e pubblicarla

Verrà visualizzata la pagina **Finalità** con un elenco delle finalità dell'app pizza.

[!INCLUDE [How to train](howto-train.md)]

[!INCLUDE [How to publish](howto-publish.md)]

L'app Pizza è ora pronta per essere usata.

## <a name="record-the-app-id-prediction-key-and-prediction-endpoint-of-your-pizza-app"></a>Registrare l'ID app, la chiave di previsione e l'endpoint di previsione dell'app Pizza

Per usare la nuova app Pizza, sono necessari l'ID app, la chiave di previsione e l'endpoint previsione dell'app Pizza.

Per trovare questi valori:

1. Nella pagina **Finalità**, selezionare **GESTISCi**.
1. Nella pagina **Impostazioni applicazione**, registrare l’**ID app**.
1. Selezionare **Risorse di Azure**.
1. Dalla pagina **Risorse di Azure**, registrare la **chiave primaria**. Questo valore è la chiave di previsione.
1. Registrare l’**URL dell'endpoint**. Questo valore è l’endpoint di previsione.
