---
title: Avvio rapido per la libreria client di Rilevamento anomalie per JavaScript
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/22/2020
ms.author: mbullwin
ms.custom: devx-track-js
ms.openlocfilehash: 36b8a6952a8dc0b34df7bf32a708c71547bf5b33
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/28/2021
ms.locfileid: "98947422"
---
Questo argomento costituisce un'introduzione alla libreria client di Rilevamento anomalie per JavaScript. Seguire questi passaggi per installare il pacchetto e iniziare a usare gli algoritmi forniti dal servizio. Il servizio Rilevamento anomalie consente di trovare le anomalie nei dati delle serie temporali applicando automaticamente i modelli di mapping più appropriati, indipendentemente dal settore, dallo scenario o dal volume di dati.

Usare la libreria client di Rilevamento anomalie per JavaScript per:

* Rilevare anomalie nell'intero set di dati della serie temporale come richiesta batch
* Rilevare lo stato di anomalia del punto dati più recente nella serie temporale
* Rilevare i punti di modifica di una tendenza nel set di dati.

[Documentazione di riferimento della libreria](https://go.microsoft.com/fwlink/?linkid=2090788) | [Codice sorgente della libreria](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/AnomalyDetector) | [Pacchetto (npm)](https://www.npmjs.com/package/%40azure/ai-anomaly-detector) | [Trovare il codice in GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/javascript/AnomalyDetector)

## <a name="prerequisites"></a>Prerequisiti

* Sottoscrizione di Azure: [creare un account gratuito](https://azure.microsoft.com/free/cognitive-services)
* Versione corrente di [Node.js](https://nodejs.org/)
* Dopo aver creato la sottoscrizione di Azure, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title="creare una risorsa Rilevamento anomalie"  target="_blank">creare una risorsa Rilevamento anomalie <span class="docon docon-navigate-external x-hidden-focus"></span></a> nel portale di Azure per ottenere la chiave e l'endpoint. Attendere che venga distribuita e fare clic sul pulsante **Vai alla risorsa**.
    * La chiave e l'endpoint della risorsa creata sono necessari per connettere l'applicazione all'API Rilevamento anomalie. La chiave e l'endpoint verranno incollati nel codice riportato di seguito nell'argomento di avvio rapido.
    È possibile usare il piano tariffario gratuito (`F0`) per provare il servizio ed eseguire in un secondo momento l'aggiornamento a un livello a pagamento per la produzione.

## <a name="setting-up"></a>Configurazione

[!INCLUDE [anomaly-detector-environment-variables](../environment-variables.md)]

### <a name="create-a-new-nodejs-application"></a>Creare una nuova applicazione Node.js

In una finestra della console, ad esempio cmd, PowerShell o Bash, creare e passare a una nuova directory per l'app. 

```console
mkdir myapp && cd myapp
```

Eseguire il comando `npm init` per creare un'applicazione Node con un file `package.json`. 

```console
npm init
```

Creare un file denominato `index.js` e importare le librerie seguenti:

[!code-javascript[Import statements](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=imports)]

Creare le variabili per l'endpoint e per la chiave di Azure della risorsa. Se la variabile di ambiente è stata creata dopo l'avvio dell'applicazione, per accedervi sarà necessario chiudere e riaprire l'editor, l'IDE o la shell in cui è in esecuzione. Creare un'altra variabile per il file di dati di esempio che verrà scaricato in un passaggio successivo e un elenco vuoto per i punti dati. Creare quindi un oggetto `ApiKeyCredentials` per contenere la chiave.

[!code-javascript[Initial endpoint and key variables](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=vars)]

### <a name="install-the-client-library"></a>Installare la libreria client

Installare i pacchetti NPM `ms-rest-azure` e `azure-cognitiveservices-anomalydetector`. In questo argomento di avvio rapido viene usata anche la libreria csv-parse:

```console
npm install @azure/ai-anomaly-detector @azure/ms-rest-js csv-parse
```

Il file `package.json` dell'app viene aggiornato con le dipendenze.

## <a name="object-model"></a>Modello a oggetti

Il client di Rilevamento anomalie è un oggetto [AnomalyDetectorClient](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient) che esegue l'autenticazione in Azure tramite la chiave dell'utente. Il client può eseguire il rilevamento anomalie in un intero set di dati usando [entireDetect()](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient#entiredetect-request--servicecallback-entiredetectresponse--) oppure nel punto dati più recente usando [LastDetect()](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient#lastdetect-request--msrest-requestoptionsbase-). Il metodo [ChangePointDetectAsync](https://go.microsoft.com/fwlink/?linkid=2090788) rileva i punti che contrassegnano le modifiche in una tendenza. 

I dati delle serie temporali vengono inviati come serie di oggetti [Point](/javascript/api/@azure/cognitiveservices-anomalydetector/point) in un oggetto [Request](/javascript/api/@azure/cognitiveservices-anomalydetector/request). L'oggetto `Request` contiene le proprietà per la descrizione dei dati (ad esempio, [Granularity](/javascript/api/@azure/cognitiveservices-anomalydetector/request#granularity)) e i parametri per il rilevamento delle anomalie. 

La risposta del servizio Rilevamento anomalie è un oggetto [LastDetectResponse](/javascript/api/@azure/cognitiveservices-anomalydetector/lastdetectresponse), [EntireDetectResponse](/javascript/api/@azure/cognitiveservices-anomalydetector/entiredetectresponse) o [ChangePointDetectResponse](https://go.microsoft.com/fwlink/?linkid=2090788), a seconda del metodo usato. 

## <a name="code-examples"></a>Esempi di codice 

Questi frammenti di codice mostrano come eseguire le operazioni seguenti con la libreria client di Rilevamento anomalie per Node.js:

* [Autenticare il client](#authenticate-the-client)
* [Caricare il set di dati di una serie temporale da un file](#load-time-series-data-from-a-file)
* [Rilevare le anomalie nell'intero set di dati](#detect-anomalies-in-the-entire-data-set) 
* [Rilevare lo stato di anomalia del punto dati più recente](#detect-the-anomaly-status-of-the-latest-data-point)
* [Rilevare i punti di modifica nel set di dati](#detect-change-points-in-the-data-set)

## <a name="authenticate-the-client"></a>Autenticare il client

Creare un'istanza di un oggetto [AnomalyDetectorClient](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient) con l'endpoint e le credenziali.

[!code-javascript[Authentication](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=authentication)]

## <a name="load-time-series-data-from-a-file"></a>Caricare i dati di una serie temporale da un file

Scaricare i dati di esempio per questo avvio rapido da [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/AnomalyDetector/request-data.csv):
1. Nel browser fare clic con il pulsante destro del mouse su **Raw** (Non elaborato).
2. Fare clic su **Salva link con nome**.
3. Salvare il file nella directory dell'applicazione come file con estensione csv.

I dati della serie temporale vengono formattati come file con estensione csv e vengono inviati all'API Rilevamento anomalie.

Leggere il file di dati con il metodo `readFileSync()` della libreria csv-parse e analizzare il file con `parse()`. Per ogni riga, effettuare il push di un oggetto [Point](/javascript/api/@azure/cognitiveservices-anomalydetector/point) contenente il timestamp e il valore numerico.

[!code-javascript[Read the data file](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=readFile)]

## <a name="detect-anomalies-in-the-entire-data-set"></a>Rilevare le anomalie nell'intero set di dati 

Chiamare l'API per rilevare le anomalie nell'intera serie temporale come batch con il metodo [entireDetect()](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient#entiredetect-request--msrest-requestoptionsbase-) del client. Archiviare l'oggetto [EntireDetectResponse](/javascript/api/@azure/cognitiveservices-anomalydetector/entiredetectresponse) restituito. Scorrere l'elenco `isAnomaly` della risposta e visualizzare l'indice dei valori `true`. Questi valori corrispondono all'indice dei punti dati anomali, se presenti.

[!code-javascript[Batch detection function](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=batchCall)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Rilevare lo stato delle anomalie del punto dati più recente

Chiamare l'API Rilevamento anomalie per determinare se il punto dati più recente è un'anomalia usando il metodo [lastDetect()](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient#lastdetect-request--msrest-requestoptionsbase-) del client, quindi archiviare l'oggetto [LastDetectResponse](/javascript/api/@azure/cognitiveservices-anomalydetector/lastdetectresponse) restituito. Il valore `isAnomaly` della risposta è un valore booleano che specifica lo stato di anomalia del punto.  

[!code-javascript[Last point detection function](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=lastDetection)]

## <a name="detect-change-points-in-the-data-set"></a>Rilevare i punti di modifica nel set di dati

Chiamare l'API per rilevare i punti di modifica nella serie temporale con il metodo [detectChangePoint()](https://go.microsoft.com/fwlink/?linkid=2090788) del client. Archiviare l'oggetto [ChangePointDetectResponse](https://go.microsoft.com/fwlink/?linkid=2090788) restituito. Scorrere l'elenco `isChangePoint` della risposta e visualizzare l'indice dei valori `true`. Questi valori corrispondono agli indici dei punti di modifica della tendenza, se individuati.

[!code-javascript[detect change points](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=changePointDetection)]

## <a name="run-the-application"></a>Eseguire l'applicazione

Eseguire l'applicazione con il comando `node` nel file quickstart.

```console
node index.js
```

[!INCLUDE [anomaly-detector-next-steps](../quickstart-cleanup-next-steps.md)]
