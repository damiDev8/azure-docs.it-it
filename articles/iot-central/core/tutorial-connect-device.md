---
title: Esercitazione - Connettere un'app client generica ad Azure IoT Central | Microsoft Docs
description: Questa esercitazione illustra agli sviluppatori di dispositivi come connettere un dispositivo che esegue un'app client C, C#, Java, JavaScript o Python all'applicazione Azure IoT Central. Si modificherà il modello di dispositivo generato automaticamente aggiungendo visualizzazioni che consentono a un operatore di interagire con un dispositivo connesso.
author: dominicbetts
ms.author: dobett
ms.date: 11/24/2020
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom:
- mqtt
- device-developer
zone_pivot_groups: programming-languages-set-twenty-six
ms.openlocfilehash: 8f1b5eabe235d107b48dc7b2db5b6d4b1188a3fa
ms.sourcegitcommit: d1b0cf715a34dd9d89d3b72bb71815d5202d5b3a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99833967"
---
# <a name="tutorial-create-and-connect-a-client-application-to-your-azure-iot-central-application"></a>Esercitazione: Creare e connettere un'applicazione client all'applicazione Azure IoT Central

*Le informazioni di questo articolo sono destinate a generatori di soluzioni e sviluppatori di dispositivi.*

Questa esercitazione illustra agli sviluppatori di dispositivi come connettere un'applicazione client all'applicazione Azure IoT Central. L'applicazione simula il comportamento di un dispositivo termostato. Quando l'applicazione si connette a IoT Central, invia l'ID modello del modello del dispositivo termostato. IoT Central usa l'ID modello per recuperare il modello di dispositivo e creare un modello di dispositivo per l'utente. È possibile aggiungere personalizzazioni e visualizzazioni al modello di dispositivo per consentire a un operatore di interagire con un dispositivo.

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Creare ed eseguire il codice del dispositivo per connetterlo all'applicazione IoT Central.
> * Visualizzare i dati di telemetria simulati inviati dal dispositivo.
> * Aggiungere visualizzazioni personalizzate a un modello di dispositivo.
> * Pubblicare il modello di dispositivo.
> * Usare una visualizzazione per gestire le proprietà del dispositivo.
> * Chiamare un comando per controllare il dispositivo.

:::zone pivot="programming-language-ansi-c"

[!INCLUDE [iot-central-connect-device-c](../../../includes/iot-central-connect-device-c.md)]

:::zone-end

:::zone pivot="programming-language-csharp"

[!INCLUDE [iot-central-connect-device-csharp](../../../includes/iot-central-connect-device-csharp.md)]

:::zone-end

:::zone pivot="programming-language-java"

[!INCLUDE [iot-central-connect-device-java](../../../includes/iot-central-connect-device-java.md)]

:::zone-end

:::zone pivot="programming-language-javascript"

[!INCLUDE [iot-central-connect-device-nodejs](../../../includes/iot-central-connect-device-nodejs.md)]

:::zone-end

:::zone pivot="programming-language-python"

[!INCLUDE [iot-central-connect-device-python](../../../includes/iot-central-connect-device-python.md)]

:::zone-end

## <a name="view-raw-data"></a>Visualizzare dati non elaborati

Gli sviluppatori di dispositivi possono usare la visualizzazione **Dati non elaborati** per esaminare i dati non elaborati inviati dal dispositivo a IoT Central:

:::image type="content" source="media/tutorial-connect-device/raw-data.png" alt-text="Visualizzazione dati non elaborati":::

In questa visualizzazione è possibile selezionare le colonne da visualizzare e impostare un intervallo di tempo da visualizzare. La colonna **Dati senza modello** contiene i dati del dispositivo che non corrispondono ad alcuna definizione di proprietà o di telemetria nel modello di dispositivo.

## <a name="clean-up-resources"></a>Pulire le risorse

[!INCLUDE [iot-central-clean-up-resources](../../../includes/iot-central-clean-up-resources.md)]

## <a name="next-steps"></a>Passaggi successivi

Se si preferisce continuare il set di esercitazioni di IoT Central e saperne di più sulla creazione di una soluzione di IoT Central, vedere:

> [!div class="nextstepaction"]
> [Creare un modello di dispositivo gateway](./tutorial-define-gateway-device-type.md)

A questo punto, dopo aver appreso le nozioni di base relative alla creazione di un dispositivo con Java, i passaggi successivi consigliati per gli sviluppatori di dispositivi sono i seguenti:

* Vedere [Che cosa sono i modelli di dispositivo?](./concepts-device-templates.md) per altre informazioni sul ruolo dei modelli di dispositivo durante l'implementazione del codice del dispositivo.
* Articolo [Connettersi ad Azure IoT Central](./concepts-get-connected.md) per altre informazioni su come registrare dispositivi con IoT Central e sulla protezione delle connessioni ai dispositivi in IoT Central.
* Per altre informazioni sui dati che il dispositivo scambia con IoT Central, vedere [Payload di telemetria, proprietà e comandi](concepts-telemetry-properties-commands.md).
* Leggere la [guida per sviluppatori di dispositivi Plug and Play IoT](../../iot-pnp/concepts-developer-guide-device.md).
