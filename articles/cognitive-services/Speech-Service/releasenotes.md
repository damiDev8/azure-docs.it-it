---
title: Note sulla versione-servizio riconoscimento vocale
titleSuffix: Azure Cognitive Services
description: Un log in esecuzione di versioni della funzionalità del servizio vocale, miglioramenti, correzioni di bug e problemi noti.
services: cognitive-services
author: oliversc
manager: jhakulin
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/27/2021
ms.author: oliversc
ms.custom: seodec18
ms.openlocfilehash: 4393607d6714bc4c1b10ac89d5ac69c173f8fef4
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99257322"
---
# <a name="speech-service-release-notes"></a>Note sulla versione del Servizio di riconoscimento vocale

## <a name="speech-sdk-1150-2021-january-release"></a>Speech SDK 1.15.0:2021-versione di gennaio

**Nota**: l'SDK di riconoscimento vocale in Windows dipende dal Microsoft Visual C++ ridistribuibile condiviso per Visual Studio 2015, 2017 e 2019. Scaricarla [qui](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads).

**Riepilogo evidenziazioni**
- Memoria e footprint del disco inferiori rendono più efficiente l'SDK.
- Formati di output con fedeltà più elevata disponibili per l'anteprima privata personalizzata per la voce neurale.
- Il riconoscitore preventivo può ora ottenere un risultato superiore alla finalità superiore, offrendo la possibilità di eseguire una valutazione separata sulle finalità del cliente.
- L'Assistente vocale o il bot è ora più semplice da configurare ed è possibile fare in modo che smetta di restare in ascolto immediatamente e esercitare un maggiore controllo sulle modalità di risposta agli errori.
- Miglioramento delle prestazioni del dispositivo grazie alla compressione facoltativa.
- Usare l'SDK di riconoscimento vocale in Windows ARM/ARM64.
- Miglioramento del debug di basso livello.
- La funzionalità di valutazione della pronuncia è ora più ampia.
- Diverse correzioni di bug per risolvere i problemi, i nostri clienti di valore, hanno contrassegnato su GitHub. Grazie! Continua a ricevere i tuoi commenti!

**Miglioramenti**
- Speech SDK è ora più efficiente e leggero. È stata avviata una versione a più versioni per ridurre l'utilizzo della memoria e l'impronta del disco dell'SDK di riconoscimento vocale. Come primo passaggio abbiamo apportato riduzioni significative delle dimensioni dei file nelle librerie condivise sulla maggior parte delle piattaforme. Rispetto alla versione 1,14:
  - le librerie Windows compatibili con UWP a 64 bit sono inferiori al 30%.
  - le librerie Windows a 32 bit non stanno ancora osservando un miglioramento delle dimensioni.
  - Le librerie Linux sono inferiori al 20-25%.
  - Le librerie Android sono inferiori al 3-5%.

**Nuove funzionalità**
- **All**: nuovi formati di output a 48 kHz disponibili per l'anteprima privata della voce neurale personalizzata tramite l'API di sintesi vocale TTS: Audio48Khz192KBitRateMonoMp3, audio-kHz-192kbitrate-mono-MP3, Audio48Khz96KBitRateMonoMp3, audio-48-96kbitrate-mono-MP3, Raw48Khz16BitMonoPcm, RAW-kHz-16 bit-mono-PCM, Riff48Khz16BitMonoPcm, riff-kHz-16 bit-mono-PCM.
- **All**: la voce personalizzata è anche più facile da usare. Aggiunta del supporto per l'impostazione di Voice personalizzata tramite `EndpointId` ([C++](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#setendpointid), [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.endpointid?view=azure-dotnet#Microsoft_CognitiveServices_Speech_SpeechConfig_EndpointId), [Java](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.setendpointid?view=azure-java-stable#com_microsoft_cognitiveservices_speech_SpeechConfig_setEndpointId_String_), [JavaScript](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#endpointId), [Objective-C](https://docs.microsoft.com/objectivec/cognitive-services/speech/spxspeechconfiguration#endpointid), [Python](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#endpoint-id)). Prima di questa modifica, gli utenti vocali personalizzati dovevano impostare l'URL dell'endpoint tramite il `FromEndpoint` metodo. Ora i clienti possono usare il `FromSubscription` metodo come le voci pubbliche, quindi specificare l'ID distribuzione impostando `EndpointId` . Questa operazione semplifica la configurazione di voci personalizzate. 
- **C++/c #/Java/Objective-C/Python**: ottenere più di quanto previsto dall'inizio `IntentRecognizer` . Supporta ora la configurazione del risultato JSON contenente tutti gli Intent e non solo l'intento di assegnazione dei punteggi più alto tramite il metodo tramite il `LanguageUnderstandingModel FromEndpoint` `verbose=true` parametro URI. Questo risolve il [problema di GitHub #880](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/880). Vedere la documentazione aggiornata [qui](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/intent-recognition/#add-a-languageunderstandingmodel-and-intents).
- **C++/c #/Java**: fare in modo che l'Assistente vocale o il bot arresti l'ascolto in modo immediato. `DialogServiceConnector` ([C++](https://docs.microsoft.com/cpp/cognitive-services/speech/dialog-dialogserviceconnector), [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector?view=azure-dotnet), [Java](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.dialog.dialogserviceconnector?view=azure-java-stable)) dispone ora `StopListeningAsync()` di un metodo da associare `ListenOnceAsync()` . In questo modo si arresterà immediatamente l'acquisizione audio e si attenderà normalmente un risultato, rendendolo perfetto per l'uso con il pulsante "arresta ora"-premere gli scenari.
- **C++/c #/Java/JavaScript**: fare in modo che l'Assistente vocale o il bot reagisca meglio agli errori di sistema sottostanti. `DialogServiceConnector` ([C++](https://docs.microsoft.com/cpp/cognitive-services/speech/dialog-dialogserviceconnector), [C#](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector?view=azure-dotnet), [Java](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.dialog.dialogserviceconnector?view=azure-java-stable), [JavaScript](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/dialogserviceconnector?view=azure-node-latest)) dispone ora di un nuovo `TurnStatusReceived` gestore eventi. Questi eventi facoltativi corrispondono a ogni [`ITurnContext`](https://docs.microsoft.com/dotnet/api/microsoft.bot.builder.iturncontext?view=botbuilder-dotnet-stable) risoluzione sul bot e segnaleranno i problemi di esecuzione quando si verificano, ad esempio in seguito a un'eccezione non gestita, un timeout o una riduzione della rete tra la voce Direct Line e il bot. `TurnStatusReceived` semplifica la risposta alle condizioni di errore. Se, ad esempio, un bot impiega troppo tempo in una query di database back-end, ad esempio se si cerca un prodotto, `TurnStatusReceived` consente al client di sapere di ricevere una richiesta di conferma con l'errore.
- **C++/c #**: usare l'SDK di riconoscimento vocale su più piattaforme. Il [pacchetto NuGet dell'SDK vocale](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech) supporta ora i file binari nativi di Windows ARM/arm64 desktop (UWP è già supportato) per rendere l'SDK di riconoscimento vocale più utile per più tipi di computer.
- **Java**: [`DialogServiceConnector`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.dialog.dialogserviceconnector?view=azure-java-stable) dispone ora di un `setSpeechActivityTemplate()` metodo che è stato escluso intenzionalmente dal linguaggio precedente. Questa operazione equivale a impostare la `Conversation_Speech_Activity_Template` proprietà e richiede che tutte le attività future del Framework di bot originate dal servizio di sintesi vocale diretta riuniscono il contenuto fornito nei payload JSON.
- **Java**: è stato migliorato il debug di basso livello. La [`Connection`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.connection?view=azure-java-stable) classe dispone ora `MessageReceived` di un evento, simile ad altri linguaggi di programmazione (C++, C#). Questo evento fornisce accesso di basso livello ai dati in ingresso dal servizio e può essere utile per la diagnostica e il debug.
- **JavaScript**: installazione più semplice per gli assistenti vocali e i bot tramite [`BotFrameworkConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/botframeworkconfig) , che ora dispone di `fromHost()` `fromEndpoint()` metodi e Factory che semplificano l'uso di posizioni di servizio personalizzate rispetto all'impostazione manuale delle proprietà. Sono state inoltre standardizzate le specifiche facoltative di `botId` per usare un bot non predefinito nelle factory di configurazione.
- **JavaScript**: miglioramento delle prestazioni del dispositivo tramite la proprietà del controllo stringa aggiunta per la compressione WebSocket. Per motivi di prestazioni, per impostazione predefinita la compressione WebSocket è stata disabilitata. Questa operazione può essere riabilitata per scenari a larghezza di banda ridotta. Ulteriori dettagli [qui](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/propertyid). Questo risolve il [problema di GitHub #242](https://github.com/microsoft/cognitive-services-speech-sdk-js/issues/242).
- **JavaScript**: è stato aggiunto il supporto per la valutazione della pronuncia per consentire la valutazione della pronuncia vocale. Vedere la Guida introduttiva [qui](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-pronunciation-assessment?pivots=programming-language-javascript).

**Correzioni di bug**
- **All** (eccetto JavaScript): correzione di una regressione nella versione 1,14, in cui è stata allocata una quantità eccessiva di memoria da parte del riconoscimento.
- **C++**: correzione di un problema di Garbage Collection con `DialogServiceConnector` , che risolve [#794 problema di GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/794).
- **C#**: è stato risolto un problema con l'arresto del thread che causava il blocco degli oggetti per circa un secondo quando eliminato.
- **C++/c #/Java**: è stata risolta un'eccezione che impedisce a un'applicazione di impostare un token di autorizzazione vocale o un modello di attività più di una volta in un oggetto `DialogServiceConnector` .
- **C++/c #/Java**: correzione di un arresto anomalo del riconoscimento a causa di un race condition in teardown.
- **JavaScript**: [`DialogServiceConnector`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/dialogserviceconnector) in precedenza non è stato rispettato il `botId` parametro facoltativo specificato nelle `BotFrameworkConfig` Factory. Questo ha reso necessario impostare manualmente il `botId` parametro della stringa di query per usare un bot non predefinito. Il bug è stato corretto e i `botId` valori forniti alle `BotFrameworkConfig` fabbriche verranno rispettati e usati, incluse le nuove `fromHost()` e `fromEndpoint()` aggiunte. Questo vale anche per il `applicationId` parametro per `CustomCommandsConfig` .
- **JavaScript**: correzione del [problema di GitHub #881](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/881), che consente di riutilizzare l'oggetto riconoscitore.
- **JavaScript**: è stato risolto un problema a causa del quale il SKD stava inviando `speech.config` più volte in una sessione TTS, sprecando la larghezza di banda.
- **JavaScript**: gestione degli errori semplificata sull'autorizzazione del microfono, che consente un messaggio più descrittivo per la propagazione quando l'utente non ha consentito l'input del microfono nel browser.
- **JavaScript**: risolto il [problema di GitHub #249](https://github.com/microsoft/cognitive-services-speech-sdk-js/issues/249) in cui gli errori di tipo in `ConversationTranslator` e `ConversationTranscriber` causavano un errore di compilazione per gli utenti di typescript.
- **Objective-C**: è stato risolto un problema per cui la compilazione GSTreamer non è riuscita per iOS in Xcode 11,4, risolvendo il [problema di GitHub #911](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/911).
- **Python**: correzione del [problema di GitHub #870](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/870), rimozione di "DeprecationWarning: il modulo IMP è deprecato a favore di importlib".

**Esempi**
- [Esempio from-file per il browser JavaScript](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/quickstart/javascript/browser/from-file/index.html) USA ora i file per il riconoscimento vocale. Questo risolve il [problema di GitHub #884](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/884).

## <a name="speech-cli-also-known-as-spx-2021-january-release"></a>INTERFACCIA della riga di comando vocale (anche nota come SPX): 2021-versione di gennaio

**Nuove funzionalità**
- L'interfaccia della riga di comando vocale è ora disponibile come [pacchetto NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech.CLI/) e può essere installata tramite l'interfaccia della riga di comando .NET come strumento globale .NET che è possibile chiamare dalla riga di comando o dalla Shell.
- Il [repository del modello di riconoscimento vocale personalizzato DevOps](https://github.com/Azure-Samples/Speech-Service-DevOps-Template) è stato aggiornato per usare l'interfaccia della riga di comando vocale per i flussi di lavoro riconoscimento vocale personalizzato.

**COVID-19 test abbreviati**: poiché la pandemia continuativa continua a richiedere ai nostri tecnici di lavorare da casa, gli script di verifica manuali pre-pandemia sono stati notevolmente ridotti. Viene testato un minor numero di dispositivi con meno configurazioni e la probabilità che si verifichino bug specifici dell'ambiente potrebbe essere aumentata. Viene comunque convalidata rigorosamente con un ampio set di automazione. Nel caso improbabile in cui si è verificato un problema, inviare informazioni su [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br>
Rimanere integro.

## <a name="text-to-speech-2020-december-release"></a>Sintesi vocale 2020-versione di dicembre

**Nuove voci neurali in GA e anteprima**

Sono state rilasciate 51 nuove voci per un totale di 129 voci neurali in 54 lingue/impostazioni locali:

- **46 nuove voci nelle impostazioni locali di GA**: in arabo ( `ar-EG` Egitto), Hamed in `ar-SA` arabo (Arabia Saudita), Borislav in `bg-BG` bulgaro (Bulgaria), Joana in `ca-ES` Catalano (Spagna), Antonin in `cs-CZ` ceco (Repubblica Ceca), Jeppe in `da-DK` danese (Danimarca), Jonas in `de-AT` tedesco (Austria), Jan in `de-CH` tedesco (Svizzera), Nestori in `el-GR` greco (Grecia), Liam in `en-CA` inglese (Canada), Connor in `en-IE` inglese (Irlanda), Martiner in Hindi (India) `en-IN` , Mohan in `en-IN` Telugu (India), in `en-IN` inglese (India), Valluvar in `en-IN` Tamil (India), Enric in `es-ES` Catalano (Spagna), Kert in `et-EE` estone (Estonia), Harrier in `fi-FI` finlandese (Finlandia), Selma in `fi-FI` finlandese (Finlandia), Fabrice in `fr-CH` francese (Svizzera), Colm in `ga-IE` irlandese (Irlanda), avri in `he-IL` Ebraico (Israele), Srecko in `hr-HR` croato (Croazia), Tamas in `hu-HU` ungherese (Ungheria), Garcia in `id-ID` Indonesiano (Indonesia), leoni in `lt-LT` lituano (Lituania), Nils in `lv-LV` lettone (Lettonia), Osman nel `ms-MY` malese (Malaysia), Joseph in `mt-MT` maltese (Malta) , Finn in `nb-NO` norvegese, Bokmål (Norvegia), Pernille in `nb-NO` norvegese, Bokmål (Norvegia), frattinia in `nl-NL` olandese (Paesi Bassi), Maarten in `nl-NL` olandese (Paesi Bassi), Agnieszka in `pl-PL` polacco (Polonia), Marek in `pl-PL` polacco (Polonia), Duarte in `pt-BR` portoghese (Brasile), Raquel in `pt-PT` portoghese (Potugal), Emil in `ro-RO` rumeno (Romania), Dmitry in `ru-RU` russo (Russia), Svetlana in `ru-RU` russo (Russia), Lukas in `sk-SK` slovacco (Slovacchia), Rok in `sl-SI` sloveno (Slovenia), Mattias in `sv-SE` svedese (Svezia), Sofie in `sv-SE` svedese (Svezia), Fabrizio in `th-TH` Thai (Tailandia), Ahmet in `tr-TR` turco (Turkey), NamMinh in `vi-VN` vietnamita (Vietnam), HsiaoChen in `zh-TW` Taiwan mandarino (Taiwan), YunJhe nel `zh-TW` taiwanese taiwanese (Taiwan), HiuMaan in `zh-HK` cinese cantonese (Hong Kong), WanLung in `zh-HK` cinese cantonese (Hong Kong).

- **5 nuove voci nelle impostazioni locali di anteprima**: Kert in `et-EE` estone (Estonia), Colm in `ga-IE` irlandese (Irlanda), Nils in `lv-LV` lettone (Lettonia), leoni in `lt-LT` lituano (Lituania), Joseph in `mt-MT` maltese (Malta).

Con questa versione, è ora supportato un totale di 129 voci neurali in 54 lingue/impostazioni locali. Inoltre, oltre 70 voci standard sono disponibili in 49 lingue/impostazioni locali. Per l'elenco completo, vedere Supporto per le [lingue](language-support.md#text-to-speech) .

**Aggiornamenti per la creazione di contenuti audio**
- Interfaccia utente di selezione voce migliorata con categorie vocali e descrizioni vocali dettagliate. 
- Abilitare l'ottimizzazione dell'intonazione per tutte le voci neurali in linguaggi diversi.
- Automatizzare l'interfaccia utente localizaiton in base al linguaggio del browser.
- Abilita `StyleDegree` i controlli per tutte le `zh-CN` voci neurali.
Visitare lo [strumento di creazione di contenuti audio](https://speech.microsoft.com/audiocontentcreation) per estrarre le nuove funzionalità. 

**Aggiornamenti per le voci zh-CN**
- Aggiornamento di tutte le `zh-CN` voci neurali per supportare la lingua inglese.
- Abilitazione `zh-CN` di tutte le voci neurali per supportare la regolazione dell'intonazione. È possibile usare SSML o lo strumento di creazione di contenuti audio per modificare l'intonazione migliore.
- Sono state aggiornate tutte le `zh-CN` voci neurali in più stile per supportare il `StyleDegree` controllo. L'intensità dell'emozione (soft o Strong) può essere regolata.
- Aggiornamento `zh-CN-YunyeNeural` per supportare più stili che possono eseguire emozioni diverse.

## <a name="text-to-speech-2020-november-release"></a>Sintesi vocale 2020-versione di novembre

**Nuove impostazioni locali e voci in anteprima**
- Per il portfolio TTS neurale sono state introdotte **cinque nuove voci e lingue** . Sono: Grace in maltese (Malta), Ona in lituano (Lituania), ANU in estone (Estonia), Orla in irlandese (Irlanda) e everita in lettone (Lettonia).
- **Cinque nuove `zh-CN` voci con più stili e ruoli supportano**: Xiaohan, xiaomo, Xiaorui, Xiaoxuan e Yunxi.

> Queste voci sono disponibili in anteprima pubblica in tre aree di Azure: Eastus, SouthEastAsia e WestEurope.

**GA contenitore TTS neurale**
- Con il contenitore TTS neurale, gli sviluppatori possono eseguire sintesi vocale con le voci digitali più naturali nel proprio ambiente per requisiti specifici di sicurezza e governance dei dati. Controllare [come installare i contenitori di sintesi vocale](speech-container-howto.md). 

**Nuove funzionalità**
- **Voce personalizzata**: gli utenti possono copiare un modello vocale da un'area all'altra; sospensione e ripresa dell'endpoint supportati. Passare al [portale](https://speech.microsoft.com/customvoice) qui.
- Supporto di [SSML Silence Tag](speech-synthesis-markup.md#add-silence) . 
- Miglioramenti della qualità della voce TTS generale: accuratezza della pronuncia a livello di parola migliorata in nb-NO. Errore di pronuncia del 53% ridotto.

> Per altre informazioni, vedere [questo Blog tecnico](https://techcommunity.microsoft.com/t5/azure-ai/neural-text-to-speech-previews-five-new-languages-with/ba-p/1907604).

## <a name="text-to-speech-2020-october-release"></a>Sintesi vocale 2020-versione di ottobre

**Nuove funzionalità**
- Jenny supporta un nuovo `newscast` stile. Vedere [How to use the Speaking Styles in SSML](speech-synthesis-markup.md#adjust-speaking-styles).
- **Voci neurali aggiornate a HiFiNet vocoder con maggiore fedeltà audio e velocità di sintesi più rapida**. Si tratta di vantaggi per i clienti il cui scenario si basa sull'audio Hi-Fi o sulle interazioni lunghe, tra cui il doppiaggio video, i libri audio o materiali di formazione online. [Scopri di più sulla storia e ascolta gli esempi di Voice nel Blog della community tecnica](https://techcommunity.microsoft.com/t5/azure-ai/azure-neural-tts-upgraded-with-hifinet-achieving-higher-audio/ba-p/1847860)
- ** &  [Studio per la creazione di contenuto audio](https://speech.microsoft.com/audiocontentcreation) vocale personalizzato localizzato a 17 impostazioni locali [](https://speech.microsoft.com/customvoice)**. Gli utenti possono passare facilmente l'interfaccia utente a una lingua locale per un'esperienza più intuitiva.   
- **Creazione di contenuto audio**: aggiunto controllo del grado di stile per XiaoxiaoNeural; Perfezionata la funzionalità di interruzione personalizzata per includere interruzioni incrementali di 50 ms. 

**Miglioramenti alla qualità della voce TTS generale**
- Accuratezza della pronuncia a livello di parola migliorata `pl-PL` (riduzione della frequenza degli errori: 51%) e `fi-FI` (riduzione della frequenza degli errori: 58%)
- `ja-JP`Lettura di una singola parola migliorata per lo scenario del dizionario. Errore di pronuncia ridotto del 80%.
- `zh-CN-XiaoxiaoNeural`: Miglioramento della qualità della voce di valutazione/CustomerService, telegiornale/stile allegro/arrabbiato.
- `zh-CN`: Migliore pronuncia Erhua e tono chiaro e spazio raffinato prosodia, che migliora notevolmente l'intelligibilità. 

## <a name="speech-sdk-1140-2020-october-release"></a>Speech SDK 1.14.0:2020-versione di ottobre

**Nota**: l'SDK di riconoscimento vocale in Windows dipende dal Microsoft Visual C++ ridistribuibile condiviso per Visual Studio 2015, 2017 e 2019. Scaricarla [qui](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads).

**Nuove funzionalità**
- **Linux**: è stato aggiunto il supporto per Debian 10 e Ubuntu 20,04 LTS.
- **Python/Objective-C**: è stato aggiunto il supporto per l' `KeywordRecognizer` API. La documentazione sarà disponibile [qui](./custom-keyword-basics.md).
- **C++/Java/C #**: è stato aggiunto il supporto per impostare qualsiasi `HttpHeader` chiave/valore tramite `ServicePropertyChannel::HttpHeader` .
- **JavaScript**: è stato aggiunto il supporto per l' `ConversationTranscriber` API. Leggi la documentazione [qui](./how-to-use-conversation-transcription.md?pivots=programming-language-javascript). 
- **C++/c #**: aggiunto nuovo `AudioDataStream FromWavFileInput` Metodo (per la lettura. File WAV) [qui (C++)](/cpp/cognitive-services/speech/audiodatastream) e [qui (C#)](/dotnet/api/microsoft.cognitiveservices.speech.audiodatastream).
-  **C++/c #/Java/Python/Objective-C/Swift**: è stato aggiunto un `stopSpeakingAsync()` metodo per arrestare la sintesi da testo a discorso. Leggere qui la documentazione di riferimento [(C++)](/cpp/cognitive-services/speech/microsoft-cognitiveservices-speech-namespace), qui ( [C#)](/dotnet/api/microsoft.cognitiveservices.speech), qui ( [Java)](/java/api/com.microsoft.cognitiveservices.speech), [qui (Python)](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech)e [qui (Objective-C/Swift)](/objectivec/cognitive-services/speech/).
- **C#, C++, Java**: aggiunta `FromDialogServiceConnector()` di una funzione alla `Connection` classe che può essere usata per monitorare gli eventi di connessione e disconnessione per `DialogServiceConnector` . Leggere qui la documentazione di riferimento [(C#)](/dotnet/api/microsoft.cognitiveservices.speech.connection), [qui (C++)](/cpp/cognitive-services/speech/connection)e [qui (Java)](/java/api/com.microsoft.cognitiveservices.speech.connection).
- **C++/c #/Java/Python/Objective-C/Swift**: è stato aggiunto il supporto per la valutazione della pronuncia, che valuta la pronuncia vocale e fornisce commenti e suggerimenti sull'accuratezza e la fluidità dell'audio parlato. Leggi la documentazione [qui](how-to-pronunciation-assessment.md).

**Modifica di rilievo**
- **JavaScript**: PullAudioOutputStream. Read () ha una modifica del tipo restituito da una promessa interna a una promessa JavaScript nativa.

**Correzioni di bug**
- **All**: correzione della regressione 1,13 nel `SetServiceProperty` punto in cui i valori con determinati caratteri speciali sono stati ignorati.
- **C#**: gli esempi di console di Windows corretti in Visual Studio 2019 non sono riusciti a trovare DLL native.
- **C#**: correzione di un arresto anomalo con la gestione della memoria se il flusso viene usato come `KeywordRecognizer` input.
- **ObjectiveC/Swift**: arresto anomalo del sistema con la gestione della memoria se il flusso viene usato come input del riconoscimento.
- **Windows**: è stato risolto un problema di co-esistenza con BT HFP/A2DP in UWP.
- **JavaScript**: correzione del mapping degli ID di sessione per migliorare la registrazione e facilitare le correlazioni di debug/servizio interne.
- **JavaScript**: `DialogServiceConnector` è stata aggiunta la correzione per disabilitare `ListenOnce` le chiamate dopo la prima chiamata.
- **JavaScript**: è stato risolto un problema per cui l'output dei risultati sarebbe sempre "semplice".
- **JavaScript**: è stato risolto un problema di riconoscimento continuo in Safari in MacOS.
- **JavaScript**: mitigazione del carico CPU per lo scenario di velocità effettiva elevata della richiesta.
- **JavaScript**: consentire l'accesso ai dettagli del risultato della registrazione del profilo vocale.
- **JavaScript**: aggiunta correzione per il riconoscimento continuo in `IntentRecognizer` .
- **C++/c #/Java/Python/Swift/ObjectiveC**: correzione dell'URL errato per australiaeast e BrazilSouth in `IntentRecognizer` .
- **C++/c #**: aggiunto `VoiceProfileType` come argomento durante la creazione di un `VoiceProfile` oggetto.
- **C++/c #/Java/Python/Swift/ObjectiveC**: è stato corretto `SPX_INVALID_ARG` il potenziale durante il tentativo di lettura `AudioDataStream` da una posizione specificata.
- **IOS**: correzione di un arresto anomalo del riconoscimento vocale in Unity

**Esempi**
- **ObjectiveC**: aggiunto esempio per [il riconoscimento delle](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/objective-c/ios/speech-samples)parole chiave.
- **C#/JavaScript**: aggiunta di Guida introduttiva per la trascrizione delle conversazioni [qui (c#)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/dotnet/conversation-transcription) e [qui (JavaScript)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/conversation-transcription).
- **C++/c #/Java/Python/Swift/ObjectiveC**: è stato aggiunto un esempio per la valutazione della pronuncia [qui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples)
- **Novell**: aggiornamento rapido aggiornato al modello più recente di Visual Studio [qui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/xamarin).

**Problema noto**
- Il certificato DigiCert Global Root G2 non è supportato per impostazione predefinita in HoloLens 2 e Android 4,4 (KitKat) e deve essere aggiunto al sistema per rendere funzionante l'SDK di riconoscimento vocale. Il certificato verrà aggiunto alle immagini del sistema operativo HoloLens 2 nel prossimo futuro. I clienti Android 4,4 devono aggiungere il certificato aggiornato al sistema.

**COVID-19 test abbreviato:** A causa del funzionamento remoto nelle ultime settimane, non è stato possibile eseguire il test di verifica manuale come in genere. Non sono state apportate modifiche che potrebbero essere state interrotte e i test automatizzati sono stati superati. Nel caso improbabile in cui si è verificato un problema, inviare informazioni su [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br>
Rimanere integro.

## <a name="speech-cli-also-known-as-spx-2020-october-release"></a>INTERFACCIA della riga di comando vocale (anche nota come SPX): 2020-versione di ottobre
SPX è l'interfaccia della riga di comando per usare il servizio riconoscimento vocale di Azure senza scrivere codice. Scaricare la versione più recente [qui](./spx-basics.md). <br>

**Nuove funzionalità**
- `spx csr dataset upload --kind audio|language|acoustic` : creare set di dati da dati locali, non solo da URL.
- `spx csr evaluation create|status|list|update|delete` : Confronta i nuovi modelli con i modelli di base verità/altri modelli.
- `spx * list` : supporta l'esperienza non di paging (non richiede--Top X--Skip X).
- `spx * --http header A=B` : supporta le intestazioni personalizzate (aggiunte per Office per l'autenticazione personalizzata). 
- `spx help` : testo migliorato e colore del testo con segno di inverso (blu).

## <a name="text-to-speech-2020-september-release"></a>Sintesi vocale 2020-versione di settembre

### <a name="new-features"></a>Nuove funzionalità

* **TTS neurale** 
    * **Esteso per supportare 18 nuove lingue/impostazioni locali.** Sono bulgaro, ceco, tedesco (Austria), tedesco (Svizzera), greco, inglese (Irlanda), francese (Svizzera), ebraico, croato, ungherese, Indonesiano, Malese, rumeno, slovacco, sloveno, Tamil, telugu e vietnamita. 
    * **Sono state rilasciate 14 nuove voci per arricchire le varie lingue esistenti.** Vedere il [linguaggio completo e l'elenco delle voci](language-support.md#neural-voices).
    * **Nuovi stili di pronuncia per `en-US` e `zh-CN` Voices.** Jenny, la nuova voce in inglese (Stati Uniti), supporta gli stili chatbot, Customer Service e Assistant. con il nostro zh-CN Voice, XiaoXiao, sono disponibili 10 nuovi stili di lingua. Inoltre, la XiaoXiao Neural Voice supporta l' `StyleDegree` ottimizzazione. Vedere [How to use the Speaking Styles in SSML](speech-synthesis-markup.md#adjust-speaking-styles).

* **Contenitori: contenitore TTS neurale rilasciato in anteprima pubblica con 16 voci disponibili in 14 lingue.** Altre informazioni su [come distribuire i contenitori di sintesi vocale per neurale TTS](speech-container-howto.md)  

Leggi l' [annuncio completo degli aggiornamenti TTS per ignite 2020](https://techcommunity.microsoft.com/t5/azure-ai/ignite-2020-neural-tts-updates-new-language-support-more-voices/ba-p/1698544) 

## <a name="text-to-speech-2020-august-release"></a>Sintesi vocale 2020-versione di agosto

### <a name="new-features"></a>Nuove funzionalità

* **TTS neurale: nuovo stile di pronuncia per `en-US` Voce aria**. AriaNeural può sembrare un cast di notizie durante la lettura delle notizie. Lo stile "telegiornale-formale" suona più grave, mentre lo stile "telegiornale-informale" è più rilassato e informale. Vedere [How to use the Speaking Styles in SSML](speech-synthesis-markup.md).

* **Voce personalizzata: viene rilasciata una nuova funzionalità per verificare automaticamente la qualità dei dati di training**. Quando si caricano i dati, il sistema esamina i vari aspetti dei dati audio e trascrizioni e corregge o filtra automaticamente i problemi per migliorare la qualità del modello vocale. In questo modo si copre il volume dell'audio, il livello di disturbo, l'accuratezza della pronuncia del riconoscimento vocale, l'allineamento del parlato con il testo normalizzato, il silenzio nell'audio, oltre al formato audio e script. 

* **Creazione di contenuti audio: un set di nuove funzionalità che consentono di ottimizzare le funzionalità di ottimizzazione vocale e di gestione audio**.

    * Pronuncia: la funzionalità di ottimizzazione della pronuncia viene aggiornata al set di fonemi più recente. È possibile scegliere l'elemento fonema corretto dalla libreria e perfezionare la pronuncia delle parole selezionate. 

    * Download: la funzionalità "download"/"esportazione" audio è stata migliorata per supportare la generazione di audio per paragrafo. È possibile modificare il contenuto nello stesso file/SSML durante la generazione di più output audio. Anche la struttura di file "download" è stata perfezionata. A questo punto, è possibile ottenere facilmente tutti i file audio in una cartella. 

    * Stato attività: l'esperienza di esportazione a più file è stata migliorata. Quando si esportano più file in passato, in caso di errore di uno dei file, l'intera attività avrà esito negativo. Ma ora tutti gli altri file verranno esportati correttamente. Il report attività è arricchito con informazioni più dettagliate e strutturate. È possibile controllare i log per tutti i file e le frasi con errori ora con il report. 

    * Documentazione di SSML: collegata al documento SSML per controllare le regole relative all'utilizzo di tutte le funzionalità di ottimizzazione.

* **L'API elenco vocale viene aggiornata per includere un nome visualizzato intuitivo e gli stili di pronuncia supportati per le voci neurali**.

### <a name="general-tts-voice-quality-improvements"></a>Miglioramenti alla qualità della voce TTS generale

* Riduzione dell'errore di pronuncia a livello di parola per `ru-RU` (errori ridotti del 56%) e `sv-SE` (errori ridotti del 49%)

* Miglioramento della lettura delle parole di polifonia sulle `en-US` voci neurali del 40%. Esempi di parole polifonia includono "Read", "Live", "Content", "record", "Object" e così via. 

* Miglioramento della natura del tono della domanda in `fr-FR` . Miglioramento del valore MOS (media Opinion Score): + 0,28

* Aggiornamento del vocoder per le voci seguenti, con miglioramenti della fedeltà e velocità complessiva delle prestazioni del 40%.

    | Locale | Chiamata vocale |
    |---|---|    
    | `en-GB` | Mia |
    | `es-MX` | Dalia |
    | `fr-CA` | Sylvie |
    | `fr-FR` | Denise |
    | `ja-JP` | Nanami |
    | `ko-KR` | Sun-Hi |

### <a name="bug-fixes"></a>Correzioni di bug

* Correzione di un numero di bug con lo strumento di creazione del contenuto audio 
    * Correzione del problema relativo all'aggiornamento automatico. 
    * Correzione dei problemi relativi agli stili vocali in zh-CN nell'area Asia orientale meridionale.
    * Correzione di un problema di stabilità, incluso un errore di esportazione con il tag "break" ed errori di punteggiatura.

## <a name="new-speech-to-text-locales-2020-august-release"></a>Nuove impostazioni locali per sintesi vocale: 2020-versione di agosto
Riconoscimento vocale rilasciate 26 nuove impostazioni locali nell'agosto: 2 lingue europee `cs-CZ` e `hu-HU` 5 impostazioni locali inglesi e 19 impostazioni locali in spagnolo che coprono la maggior parte dei paesi dell'America del sud. Di seguito è riportato un elenco delle nuove impostazioni locali. Vedere l'elenco completo della lingua [qui](./language-support.md).

| Impostazioni locali  | Linguaggio                          |
|---------|-----------------------------------|
| `cs-CZ` | Ceco (Repubblica Ceca)            | 
| `en-HK` | Inglese (Hong Kong)               | 
| `en-IE` | Inglese (Irlanda)                 | 
| `en-PH` | Inglese (Filippine)             | 
| `en-SG` | Inglese (Singapore)               | 
| `en-ZA` | Inglese (Sudafrica)            | 
| `es-AR` | Spagnolo (Argentina)               | 
| `es-BO` | Spagnolo (Bolivia)                 | 
| `es-CL` | Spagnolo (Cile)                   | 
| `es-CO` | Spagnolo (Colombia)                | 
| `es-CR` | Spagnolo (Costa Rica)              | 
| `es-CU` | Spagnolo (Cuba)                    | 
| `es-DO` | Spagnolo (Repubblica dominicana)      | 
| `es-EC` | Spagnolo (Ecuador)                 | 
| `es-GT` | Spagnolo (Guatemala)               | 
| `es-HN` | Spagnolo (Honduras)                | 
| `es-NI` | Spagnolo (Nicaragua)               | 
| `es-PA` | Spagnolo (Panama)                  | 
| `es-PE` | Spagnolo (Perù)                    | 
| `es-PR` | Spagnolo (Puerto Rico)             | 
| `es-PY` | Spagnolo (Paraguay)                | 
| `es-SV` | Spagnolo (El Salvador)             | 
| `es-US` | Spagnolo (Stati Uniti)                     | 
| `es-UY` | Spagnolo (Uruguay)                 | 
| `es-VE` | Spagnolo (Venezuela)               | 
| `hu-HU` | Ungherese (Ungheria)               | 


## <a name="speech-sdk-1130-2020-july-release"></a>Speech SDK 1.13.0:2020-versione luglio

**Nota**: l'SDK di riconoscimento vocale in Windows dipende dal Microsoft Visual C++ ridistribuibile condiviso per Visual Studio 2015, 2017 e 2019. Scaricare e installare l'app da [qui](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads).

**Nuove funzionalità**
- **C#**: è stato aggiunto il supporto per la trascrizione di conversazioni asincrona. Vedere la documentazione [qui](./how-to-async-conversation-transcription.md).  
- **JavaScript**: aggiunta del supporto riconoscimento del parlante per [browser](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser/speaker-recognition) e [node.js](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/speaker-recognition).
- **JavaScript**: è stato aggiunto il supporto per l'ID lingua e il rilevamento automatico della lingua. Vedere la documentazione [qui](./how-to-automatic-language-detection.md?pivots=programming-language-javascript).
- **Objective-C**: è stato aggiunto il supporto per la [trascrizione](./conversation-transcription.md)di conversazioni e conversazioni su più [dispositivi](./multi-device-conversation.md) . 
- **Python**: è stato aggiunto il supporto audio compresso per Python in Windows e Linux. Vedere la documentazione [qui](./how-to-use-codec-compressed-audio-input-streams.md). 

**Correzioni di bug**
- **All**: è stato risolto un problema che causava la mancata spostamento dei flussi da parte di KeywordRecognizer dopo un riconoscimento.
- **All**: è stato risolto un problema che ha causato il flusso ottenuto da un KeywordRecognitionResult in modo che non contenga la parola chiave.
- **All**: è stato risolto un problema a causa del quale il SendMessageAsync non invia effettivamente il messaggio in rete al termine dell'attesa da parte degli utenti.
- **All**: correzione di un arresto anomalo nelle API riconoscimento del parlante quando gli utenti chiamano più volte il metodo VoiceProfileClient:: SpeakerRecEnrollProfileAsync e non attendono il completamento delle chiamate.
- **All**: è stata corretta l'abilitazione della registrazione file nelle classi VoiceProfileClient e SpeakerRecognizer.
- **JavaScript**: è stato risolto un [problema relativo](https://github.com/microsoft/cognitive-services-speech-sdk-js/issues/74) alla limitazione quando il browser è ridotto a icona.
- **JavaScript**: è stato risolto un [problema](https://github.com/microsoft/cognitive-services-speech-sdk-js/issues/78) con una perdita di memoria nei flussi.
- **JavaScript**: aggiunta della memorizzazione nella cache per le risposte OCSP da NodeJS.
- **Java**: è stato risolto un problema che causava che i campi BigInteger restituivano sempre 0.
- **iOS**: è stato risolto un [problema relativo](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/702) alla pubblicazione di app basate su SDK vocale nell'App Store iOS.

**Esempi**
- **C++**: aggiunta del codice di esempio per riconoscimento del parlante [qui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/cpp/windows/console/samples/speaker_recognition_samples.cpp).

**COVID-19 test abbreviato:** A causa del funzionamento remoto nelle ultime settimane, non è stato possibile eseguire il test di verifica manuale come in genere. Non sono state apportate modifiche che potrebbero essere state interrotte e i test automatizzati sono stati superati. Nel caso improbabile in cui si è verificato un problema, inviare informazioni su [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br>
Rimanere integro.

## <a name="text-to-speech-2020-july-release"></a>Sintesi vocale 2020-versione luglio

### <a name="new-features"></a>Nuove funzionalità

* **TTS neurale, 15 nuove voci neurali**: le nuove voci aggiunte al portfolio TTS neurale sono salma in `ar-EG` arabo (Egitto), Zariyah in `ar-SA` arabo (Arabia Saudita), alba in `ca-ES` Catalano (Spagna), Christel in `da-DK` danese (Danimarca), Neerja in `es-IN` inglese (India), Noora in `fi-FI` finlandese (Finlandia), Swara in `hi-IN` Hindi (India), Colette in `nl-NL` olandese (Paesi Bassi), Zofia in `pl-PL` polacco (Polonia), Fernanda in `pt-PT` portoghese (Portogallo), Dariya in `ru-RU` russo (Russia), Hillevi in `sv-SE` svedese (Svezia), Achara in `th-TH` Thai (Tailandia), HiuGaai in `zh-HK` cinese (cantonese, tradizionale) e HsiaoYu in `zh-TW` cinese (Taiwan mandarino). Controllare tutte le [lingue supportate](./language-support.md#neural-voices).  

* **Voce personalizzata, testing vocale semplificato con il flusso di training per semplificare l'esperienza utente**: con la nuova funzionalità di test, ogni voce verrà automaticamente testata con un set di test predefinito ottimizzato per ogni lingua per coprire scenari generali e di Assistente vocale. Questi set di test vengono accuratamente selezionati e testati per includere casi d'uso e fonemi tipici nel linguaggio. Inoltre, gli utenti possono comunque selezionare di caricare i propri script di test durante il training di un modello.

* **Creazione di contenuti audio: viene rilasciata una serie di nuove funzionalità per abilitare funzionalità di ottimizzazione vocale e di gestione audio più potenti**

    * `Pitch`, `rate` e `volume` sono stati migliorati per supportare l'ottimizzazione con un valore predefinito, ad esempio Slow, medium e fast. È ora semplice per gli utenti selezionare un valore "costante" per la modifica audio.

    ![Ottimizzazione audio](media/release-notes/audio-tuning.png)

    * Gli utenti possono ora rivedere il `Audio history` per il file di lavoro. Con questa funzionalità, gli utenti possono tenere traccia facilmente di tutti gli audio generati correlati a un file di lavoro. Possono controllare la versione della cronologia e confrontare la qualità durante l'ottimizzazione nello stesso momento. 

    ![Cronologia audio](media/release-notes/audio-history.png)

    * La `Clear` funzionalità è ora più flessibile. Gli utenti possono cancellare un parametro di ottimizzazione specifico mantenendo gli altri parametri disponibili per il contenuto selezionato.  

    * Un video di esercitazione è stato aggiunto nella [pagina di destinazione](https://speech.microsoft.com/audiocontentcreation) per aiutare gli utenti a iniziare rapidamente a usare l'ottimizzazione vocale e la gestione audio. 

### <a name="general-tts-voice-quality-improvements"></a>Miglioramenti alla qualità della voce TTS generale

* Miglioramento del vocoder TTS in per una maggiore fedeltà e una latenza più bassa.

    * Aggiornamento di Elsa in `it-IT` a un nuovo vocoder che ha ottenuto più di 0,464 CMOS (Punteggio di opinione media comparativa) con qualità voce, 40% più veloce in sintesi e riduzione del 30% sulla latenza del primo byte. 
    * Aggiornamento di Xiaoxiao in `zh-CN` al nuovo vocoder con + 0148 di guadagno CMOS per il dominio generale, + 0,348 per lo stile del telegiornale e + 0,195 per lo stile lirico. 

* `de-DE` `ja-JP` Modelli vocali aggiornati e per rendere più naturale l'output TTS.
    
    * Aggiornamento di Katja in `de-DE` con il metodo di modellazione prosodia più recente, il guadagno MOS (media Opinion Score) è + 0,13. 
    * Aggiornamento di nanami in `ja-JP` con un nuovo modello di accento prosodia di pitch, il guadagno MOS (media Opinion Score) è + 0,19;  

* Accuratezza della pronuncia a livello di parola migliorata in cinque lingue.

    | Linguaggio | Riduzione degli errori di pronuncia |
    |---|---|
    | `en-GB` | 51% |
    | `ko-KR` | 17% |
    | `pt-BR` | 39% |
    | `pt-PT` | 77% |
    | `id-ID` | 46% |

### <a name="bug-fixes"></a>Correzioni di bug

* Lettura valuta
    * Correzione del problema relativo alla lettura della valuta per `es-ES` e `es-MX`
     
    | Linguaggio | Input | Lettura dopo miglioramento |
    |---|---|---|
    | `es-MX` | $1,58 | un peso cincuenta y Ocho centavos |
    | `es-ES` | $1,58 | Dólar cincuenta y Ocho centavos |

    * Supporto per la valuta negativa (ad esempio "-€325") nelle impostazioni locali seguenti: `en-US` , `en-GB` , `fr-FR` , `it-IT` , `en-AU` , `en-CA` .

* Miglioramento della lettura degli indirizzi in `pt-PT` .
* Correzione dei problemi di pronuncia di Natasha ( `en-AU` ) e Libby ( `en-UK` ) per la parola "for" e "Four".  
* Correzione dei bug sullo strumento di creazione del contenuto audio
    * Sospensione aggiuntiva e imprevista dopo la correzione del secondo paragrafo.  
    * La funzionalità' No break ' viene riaggiunta da un bug di regressione. 
    * Il problema di aggiornamento casuale di speech studio è stato risolto.  

### <a name="samplessdk"></a>Esempi/SDK

* JavaScript: corregge il problema di riproduzione in Firefox e Safari in macOS e iOS. 

## <a name="speech-sdk-1121-2020-june-release"></a>Speech SDK 1.12.1:2020-versione di giugno
**INTERFACCIA della riga di comando vocale (anche nota come SPX)**
-   Aggiunto funzionalità di ricerca della Guida dell'interfaccia della riga di comando:
    -   `spx help find --text TEXT`
    -   `spx help find --topic NAME`
-   Aggiornamento per lavorare con le API batch e Riconoscimento vocale personalizzato della versione 3.0 appena distribuite:
    -   `spx help batch examples`
    -   `spx help csr examples`

**Nuove funzionalità**
-   **C \# , C++**: riconoscimento del parlante anteprima: questa funzionalità consente l'identificazione dell'altoparlante (che parla?) e la verifica dell'altoparlante (è il relatore che affermano di essere?). Per una [Panoramica](./speaker-recognition-overview.md), vedere l' [articolo Nozioni di base su riconoscimento del parlante](./get-started-speaker-recognition.md)o la documentazione di riferimento per le [API](/rest/api/speakerrecognition/).

**Correzioni di bug**
-   **C \# , C++**: la registrazione del microfono fisso non funzionava in 1,12 nel riconoscimento del parlante.
-   **JavaScript**: correzioni per sintesi vocale in Firefox e Safari in MacOS e iOS.
-   Correzione dell'arresto anomalo della violazione di accesso di Windows Application Verifier durante la trascrizione delle conversazioni quando si usa il flusso a otto canali
-   Correzione dell'arresto anomalo della violazione di accesso di Windows Application Verifier nella conversione di conversazioni per più dispositivi.

**Esempi**
-   **C#**: [esempio di codice](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/dotnet/speaker-recognition) per il riconoscimento del parlante.
-   **C++**: [esempio di codice](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp/windows/speaker-recognition) per il riconoscimento del parlante.
-   **Java**: [esempio di codice](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/intent-recognition) per il riconoscimento preventivo in Android. 

**COVID-19 test abbreviato:** A causa del funzionamento remoto nelle ultime settimane, non è stato possibile eseguire il test di verifica manuale come in genere. Non sono state apportate modifiche che potrebbero essere state interrotte e i test automatizzati sono stati superati. Nel caso improbabile in cui si è verificato un problema, inviare informazioni su [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br>
Rimanere integro.


## <a name="speech-sdk-1120-2020-may-release"></a>Speech SDK 1.12.0:2020-versione di maggio
**INTERFACCIA della riga di comando vocale (anche noto come SPX)**
- **SPX** è un nuovo strumento da riga di comando che consente di eseguire operazioni di riconoscimento, sintesi, traduzione, trascrizione batch e gestione vocale personalizzata dalla riga di comando. Usarlo per testare il servizio di riconoscimento vocale o per generare script per le attività del servizio vocale che è necessario eseguire. Scaricare lo strumento e leggere la documentazione [qui](./spx-overview.md).

**Nuove funzionalità**
- **Go**: nuovo supporto per il linguaggio Go per il [riconoscimento vocale](./get-started-speech-to-text.md?pivots=programming-language-go) e l' [Assistente vocale personalizzato](./quickstarts/voice-assistants.md?pivots=programming-language-go). Configurare l'ambiente di sviluppo [qui](./quickstarts/setup-platform.md?pivots=programming-language-go). Per il codice di esempio, vedere la sezione esempi riportata di seguito. 
- **JavaScript**: aggiunto il supporto del browser per la sintesi vocale. Vedere la documentazione [qui](./get-started-text-to-speech.md?pivots=programming-language-JavaScript).
- **C++, C#, Java**: nuovi `KeywordRecognizer` oggetti e API supportati in piattaforme Windows, Android, Linux & iOS. Leggi la documentazione [qui](./custom-keyword-overview.md). Per il codice di esempio, vedere la sezione esempi riportata di seguito. 
- **Java**: è stata aggiunta la conversazione per più dispositivi con supporto per la traduzione. Vedere la documentazione di riferimento [qui](/java/api/com.microsoft.cognitiveservices.speech.transcription).

**Miglioramenti & ottimizzazioni**
- **JavaScript**: implementazione ottimizzata del microfono del browser miglioramento dell'accuratezza del riconoscimento vocale.
- **Java**: binding sottoposti a refactoring usando l'implementazione JNI diretta senza sorso. Questa modifica riduce di 10 volte le dimensioni dei binding per tutti i pacchetti Java usati per Windows, Android, Linux e Mac e semplifica ulteriormente lo sviluppo dell'implementazione Java per l'SDK di riconoscimento vocale.
- **Linux**: è stata aggiornata la [documentazione](./speech-sdk.md?tabs=linux) del supporto con le note specifiche più recenti di RHEL 7.
- Logica di connessione migliorata per tentare di connettersi più volte quando si verificano errori di servizio e di rete.
- È stata aggiornata la pagina introduttiva di [Portal.Azure.com](https://portal.azure.com) Speech per aiutare gli sviluppatori a eseguire il passaggio successivo nel percorso vocale di Azure.

**Correzioni di bug**
- **C#, Java**: è stato risolto un [problema](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/587) relativo al caricamento delle librerie SDK su ARM Linux (a 32 bit e a 64 bit).
- **C#**: correzione dell'eliminazione esplicita degli handle nativi per gli oggetti TranslationRecognizer, IntentRecognizer e Connection.
- **C#**: correzione della durata dell'input audio per l'oggetto ConversationTranscriber.
- Correzione di un problema `IntentRecognizer` per cui il motivo del risultato non è stato impostato correttamente quando si riconoscono gli Intent da frasi semplici.
- Correzione di un problema per cui l' `SpeechRecognitionEventArgs` offset dei risultati non è stato impostato correttamente.
- Correzione di un race condition in cui SDK stava tentando di inviare un messaggio di rete prima di aprire la connessione WebSocket. È stato riproducibile per mentre venivano `TranslationRecognizer` aggiunti i partecipanti.
- Correzione di perdite di memoria nel motore di riconoscimento delle parole chiave.

**Esempi**
- **Go**: aggiunte guide introduttive per [riconoscimento vocale](./get-started-speech-to-text.md?pivots=programming-language-go) e [Assistente vocale personalizzato](./quickstarts/voice-assistants.md?pivots=programming-language-go). Trovare il codice di esempio [qui](https://github.com/microsoft/cognitive-services-speech-sdk-go/tree/master/samples). 
- **JavaScript**: aggiunte guide introduttive per [sintesi vocale](./get-started-text-to-speech.md?pivots=programming-language-javascript), [traduzione](./get-started-speech-translation.md?pivots=programming-language-csharp&tabs=script)e [riconoscimento finalità](./quickstarts/intent-recognition.md?pivots=programming-language-javascript).
- Esempi di riconoscimento delle parole chiave per [C \# ](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/uwp/keyword-recognizer) e [Java](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/keyword-recognizer) (Android).  

**COVID-19 test abbreviato:** A causa del funzionamento remoto nelle ultime settimane, non è stato possibile eseguire il test di verifica manuale come in genere. Non sono state apportate modifiche che potrebbero essere state interrotte e i test automatizzati sono stati superati. In caso di mancata risposta, inviaci informazioni su [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br>
Rimanere integro.

## <a name="speech-sdk-1110-2020-march-release"></a>Speech SDK 1.11.0:2020-versione di marzo
**Nuove funzionalità**
- Linux: è stato aggiunto il supporto per Red Hat Enterprise Linux (RHEL)/CentOS 7 x64 con [le istruzioni](./how-to-configure-rhel-centos-7.md) su come configurare il sistema per l'SDK di riconoscimento vocale.
- Linux: è stato aggiunto il supporto per .NET Core C# in Linux ARM32 e ARM64. Altre informazioni sono disponibili [qui](./speech-sdk.md?tabs=linux). 
- C#, C++: aggiunto `UtteranceId` in `ConversationTranscriptionResult` , un ID coerente in tutti i risultati intermedi e riconoscimento vocale finale. Dettagli per [C#](/dotnet/api/microsoft.cognitiveservices.speech.transcription.conversationtranscriptionresult), [C++](/cpp/cognitive-services/speech/transcription-conversationtranscriptionresult).
- Python: è stato aggiunto il supporto per `Language ID` . Vedere speech_sample. py nel [repository GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/python/console).
- Windows: aggiunta del supporto per il formato di input audio compresso nella piattaforma Windows per tutte le applicazioni console Win32. Dettagli [qui](./how-to-use-codec-compressed-audio-input-streams.md). 
- JavaScript: supporto sintesi vocale (sintesi vocale) in NodeJS. Fare clic [qui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/text-to-speech) per altre informazioni. 
- JavaScript: aggiungere nuove API per consentire l'ispezione di tutti i messaggi di trasmissione e ricezione. Fare clic [qui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript) per altre informazioni. 
        
**Correzioni di bug**
- C#, C++: è stato risolto un problema `SendMessageAsync` in modo che ora invii un messaggio binario come tipo binario. Dettagli per [C#](/dotnet/api/microsoft.cognitiveservices.speech.connection.sendmessageasync#Microsoft_CognitiveServices_Speech_Connection_SendMessageAsync_System_String_System_Byte___System_UInt32_), [C++](/cpp/cognitive-services/speech/connection).
- C#, C++: è stato risolto un problema per cui l'uso dell' `Connection MessageReceived` evento può causare un arresto anomalo se `Recognizer` viene eliminato prima dell' `Connection` oggetto. Dettagli per [C#](/dotnet/api/microsoft.cognitiveservices.speech.connection.messagereceived), [C++](/cpp/cognitive-services/speech/connection#messagereceived).
- Android: le dimensioni del buffer audio dal microfono sono diminuite da 800 MS a 100 ms per migliorare la latenza.
- Android: è stato risolto un [problema](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/563) con l'emulatore Android x86 in Android Studio.
- JavaScript: è stato aggiunto il supporto per le aree in Cina con l' `fromSubscription` API. Dettagli [qui](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig#fromsubscription-string--string-). 
- JavaScript: aggiungere altre informazioni sull'errore per gli errori di connessione da NodeJS.
        
**Esempi**
- Unity: l'esempio pubblico relativo al riconoscimento preventivo è fisso, dove l'importazione di LUIS JSON non è riuscita. Dettagli [qui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/369).
- Python: esempio aggiunto per `Language ID` . Dettagli [qui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/python/console/speech_sample.py).
    
**Test abbreviato in Covid19:** A causa del funzionamento remoto nelle ultime settimane, non è stato possibile eseguire il test di verifica manuale dei dispositivi come in genere. Ad esempio, non è stato possibile testare l'input e l'output del microfono in Linux, iOS e macOS. Non sono state apportate modifiche che potrebbero essere state interrotte da queste piattaforme e i test automatizzati sono stati superati. Nel caso improbabile in cui si è verificato un problema, inviare informazioni su [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen).<br> Grazie per il supporto continuo. Come sempre, inviare domande o commenti e suggerimenti su [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen) o [stack overflow](https://stackoverflow.microsoft.com/questions/tagged/731).<br>
Rimanere integro.

## <a name="speech-sdk-1100-2020-february-release"></a>Speech SDK 1.10.0:2020-versione di febbraio

**Nuove funzionalità**

 - Sono stati aggiunti pacchetti Python per supportare la nuova versione 3,8 di Python.
 - Supporto Red Hat Enterprise Linux (RHEL)/CentOS 8 x64 (C++, C#, Java, Python).
   > [!NOTE] 
   > I clienti devono configurare OpenSSL seguendo [queste istruzioni](./how-to-configure-openssl-linux.md).
 - Supporto di Linux ARM32 per Debian e Ubuntu.
 - DialogServiceConnector supporta ora un parametro "ID bot" facoltativo in BotFrameworkConfig. Questo parametro consente l'uso di più bot per la comunicazione diretta con una sola risorsa di sintesi vocale di Azure. Senza il parametro specificato, verrà usato il bot predefinito, come determinato dalla pagina di configurazione del canale di riconoscimento vocale diretto.
 - DialogServiceConnector dispone ora di una proprietà SpeechActivityTemplate. Il contenuto di questa stringa JSON verrà usato da Direct Line Speech per pre-popolare un'ampia gamma di campi supportati in tutte le attività che raggiungono un bot Direct Line Speech, incluse le attività generate automaticamente in risposta a eventi come il riconoscimento vocale.
 - TTS ora usa la chiave di sottoscrizione per l'autenticazione, riducendo la latenza del primo byte del primo risultato di sintesi dopo la creazione di un sintetizzatore.
 - Modelli di riconoscimento vocale aggiornati per 19 impostazioni locali per una riduzione della frequenza degli errori di Word media pari al 18,6% (es-ES, es-MX, fr-CA, fr-FR, it-IT, ja-JP, ko-KR, PT-BR, zh-CN, ZH-HK, nb-NO, Fi-FL, ur-ur, pl-PL, ca-ES, ZH-TW, th-TH, PT-PT, TR-TR). I nuovi modelli comportano miglioramenti significativi in più domini, tra cui la dettatura, Call-Center la trascrizione e gli scenari di indicizzazione video.

**Correzioni di bug**

 - Correzione del bug per cui il trascrittore delle conversazioni non era in attesa correttamente nelle API JAVA 
 - Correzione dell'emulatore Android x86 per il [problema di GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/363) Novell
 - Aggiungi mancante (Get | Set) metodi della proprietà su AudioConfig
 - Correzione di un bug TTS in cui non è stato possibile arrestare il audioDataStream quando la connessione non riesce
 - L'uso di un endpoint senza un'area provocherebbe errori di USP per il convertitore di conversazioni
 - La generazione di ID nelle applicazioni Windows universali USA ora un algoritmo GUID univoco in modo appropriato. per impostazione predefinita in precedenza e involontariamente in un'implementazione con stub che produceva spesso collisioni su grandi set di interazioni.
 
 **Esempi**
 
 - Esempio di Unity per l'uso di Speech SDK con [Unity microphone and push mode streaming](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/unity/from-unitymicrophone)

**Altre modifiche**

 - [Documentazione di configurazione di OpenSSL aggiornata per Linux](./how-to-configure-openssl-linux.md)

## <a name="speech-sdk-190-2020-january-release"></a>Speech SDK 1.9.0:2020-versione di gennaio

**Nuove funzionalità**

- Conversazione a più dispositivi: connettere più dispositivi alla stessa conversazione vocale o basata su testo e, facoltativamente, convertire i messaggi inviati tra di essi. Altre informazioni sono disponibili in [questo articolo](multi-device-conversation.md). 
- Aggiunta del supporto per il riconoscimento delle parole chiave per il pacchetto Android. Aar e aggiunta del supporto per le versioni x86 e x64. 
- Objective-C: `SendMessage` e `SetMessageProperty` metodi aggiunti all' `Connection` oggetto. Vedere la documentazione [qui](/objectivec/cognitive-services/speech/spxconnection).
- L'API TTS C++ supporta ora `std::wstring` come input di testo di sintesi, eliminando la necessità di convertire una wstring in stringa prima di passarla all'SDK. Vedere i dettagli [qui](/cpp/cognitive-services/speech/speechsynthesizer#speaktextasync). 
- C#: l' [ID lingua](./how-to-automatic-language-detection.md?pivots=programming-language-csharp) e la [configurazione della lingua di origine](./how-to-specify-source-language.md?pivots=programming-language-csharp) sono ora disponibili.
- JavaScript: aggiunta di una funzionalità all' `Connection` oggetto per passare i messaggi personalizzati dal servizio di riconoscimento vocale come callback `receivedServiceMessage` .
- JavaScript: è stato aggiunto il supporto per `FromHost API` per semplificare l'uso con i contenitori locali e i cloud sovrani. Vedere la documentazione [qui](speech-container-howto.md).
- JavaScript: ora è `NODE_TLS_REJECT_UNAUTHORIZED` possibile rispettare un contributo di [orgads](https://github.com/orgads). Vedere i dettagli [qui](https://github.com/microsoft/cognitive-services-speech-sdk-js/pull/75).

**Modifiche di rilievo**

- `OpenSSL` è stato aggiornato alla versione 1.1.1 b ed è collegato in modo statico alla libreria principale dell'SDK vocale per Linux. Questa operazione può causare un'interrotta se la posta `OpenSSL` in arrivo non è stata installata `/usr/lib/ssl` nella directory del sistema. Per risolvere il problema, vedere la [documentazione relativa](how-to-configure-openssl-linux.md) ai documenti dell'SDK per la sintesi vocale.
- È stato modificato il tipo di dati restituito per C# `WordLevelTimingResult.Offset` da `int` a per `long` consentire l'accesso a `WordLevelTimingResults` quando i dati vocali sono più lunghi di 2 minuti.
- `PushAudioInputStream` e `PullAudioInputStream` ora invia le informazioni di intestazione WAV al servizio di riconoscimento vocale in base a `AudioStreamFormat` , facoltativamente specificata al momento della creazione. I clienti devono ora usare il [formato di input audio supportato](how-to-use-audio-input-streams.md). Eventuali altri formati otterranno risultati di riconoscimento non ottimali o potrebbero causare altri problemi. 

**Correzioni di bug**

- Vedere l' `OpenSSL` aggiornamento in modifiche di rilievo precedenti. Sono stati corretti un arresto intermittente e un problema di prestazioni (conflitti di blocco sotto carico elevato) in Linux e Java. 
- Java: miglioramenti apportati alla chiusura degli oggetti in scenari di concorrenza elevata.
- Il pacchetto NuGet è stato ristrutturato. Sono state rimosse le tre copie di `Microsoft.CognitiveServices.Speech.core.dll` e `Microsoft.CognitiveServices.Speech.extension.kws.dll` in cartelle lib, rendendo il pacchetto NuGet più piccolo e veloce da scaricare e abbiamo aggiunto le intestazioni necessarie per compilare alcune app native C++.
- Correzione degli esempi di avvio rapido [qui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp). Si è verificata l'uscita senza visualizzare l'eccezione "microfono non trovato" in Linux, macOS e Windows.
- Correzione di un arresto anomalo dell'SDK con i risultati del riconoscimento vocale lungo su determinati percorsi di codice come [questo esempio](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/csharp/uwp/speechtotext-uwp).
- Correzione dell'errore di distribuzione dell'SDK nell'ambiente app Web di Azure per risolvere [questo problema del cliente](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/396).
- Correzione di un errore TTS durante `<voice>` l'utilizzo di tag o tag diversi `<audio>` per risolvere [il problema del cliente](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/433). 
- Correzione di un errore TTS 401 quando l'SDK viene recuperato dalla sospensione.
- JavaScript: è stata corretta un'importazione circolare di dati audio grazie a un contributo di [euirim](https://github.com/euirim). 
- JavaScript: è stato aggiunto il supporto per l'impostazione delle proprietà del servizio, come aggiunto in 1,7.
- JavaScript: è stato risolto un problema per cui un errore di connessione poteva causare tentativi di riconnessione WebSocket continui e non riusciti.

**Esempi**

- Aggiunta dell'esempio di riconoscimento delle parole chiave [per Android.](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/java/android/sdkdemo)
- Aggiunto esempio TTS per lo scenario server [qui](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/speech_synthesis_server_scenario_sample.cs).
- Aggiunta di guide introduttive per la conversazione per più dispositivi per C# e C++ [qui](quickstarts/multi-device-conversation.md).

**Altre modifiche**

- Dimensioni della libreria principale ottimizzate SDK in Android.
- SDK in 1.9.0 e versioni successive supporta `int` `string` i tipi e nel campo della versione della firma vocale per il trascrittore delle conversazioni.

## <a name="speech-sdk-180-2019-november-release"></a>Speech SDK 1.8.0:2019-versione di novembre

**Nuove funzionalità**

- Aggiunta di un' `FromHost()` API per semplificare l'uso con i contenitori locali e i cloud sovrani.
- Aggiunta Rilevamento lingua di origine automatica per il riconoscimento vocale (in Java e C++)
- È stato aggiunto un `SourceLanguageConfig` oggetto per il riconoscimento vocale, usato per specificare le lingue di origine previste (in Java e C++)
- Aggiunta `KeywordRecognizer` del supporto in Windows (UWP), Android e iOS tramite i pacchetti NuGet e Unity
- Aggiunta dell'API Java per la conversazione remota per eseguire la trascrizione in batch asincroni.

**Modifiche di rilievo**

- Funzionalità del trascrittore di conversazione spostate nello spazio dei nomi `Microsoft.CognitiveServices.Speech.Transcription` .
- Parti dei metodi del trascrittore di conversazione vengono spostati nella nuova `Conversation` classe.
- Supporto eliminato per iOS a 32 bit (ARMv7 e x86)

**Correzioni di bug**

- Correzione dell'arresto anomalo se `KeywordRecognizer` viene utilizzata la chiave locale senza una chiave di sottoscrizione del servizio vocale valida

**Esempi**

- Esempio di Novell per `KeywordRecognizer`
- Esempio di Unity per `KeywordRecognizer`
- Esempi di C++ e Java per l'origine automatica Rilevamento lingua.

## <a name="speech-sdk-170-2019-september-release"></a>Speech SDK 1.7.0:2019-versione di settembre

**Nuove funzionalità**

- Aggiunta del supporto beta per Novell in piattaforma UWP (Universal Windows Platform) (UWP), Android e iOS
- Aggiunta del supporto iOS per Unity
- Aggiunta `Compressed` del supporto di input per alegge, mulaw, FLAC in Android, iOS e Linux
- Aggiunta `SendMessageAsync` nella `Connection` classe per l'invio di un messaggio al servizio
- Aggiunta `SetMessageProperty` nella `Connection` classe per l'impostazione della proprietà di un messaggio
- Binding aggiunto TTS per Java (JRE e Android), Python, Swift e Objective-C
- TTS aggiunto supporto per la riproduzione per macOS, iOS e Android.
- Sono state aggiunte informazioni "confine di parola" per TTS.

**Correzioni di bug**

- Correzione del problema di compilazione di IL2CPP in Unity 2019 per Android
- Correzione del problema relativo all'elaborazione non corretta delle intestazioni in formato non corretto nell'input del file WAV
- Correzione di un problema con UUID non univoco in alcune proprietà di connessione
- Correzione di alcuni avvisi sugli identificatori di supporto di valori null nelle associazioni Swift (potrebbe richiedere modifiche minime al codice)
- Correzione di un bug che causava la chiusura non corretta delle connessioni WebSocket in un carico di rete
- È stato risolto un problema in Android che a volte produce ID di impressione duplicati usati da `DialogServiceConnector`
- Miglioramenti alla stabilità delle connessioni tra interazioni a più turni e segnalazione di errori (tramite `Canceled` eventi) quando si verificano con `DialogServiceConnector`
- `DialogServiceConnector` l'avvio della sessione ora fornirà correttamente gli eventi, ad esempio quando si chiama `ListenOnceAsync()` durante una chiamata attiva `StartKeywordRecognitionAsync()`
- È stato risolto un arresto anomalo associato alle `DialogServiceConnector` attività ricevute

**Esempi**

- Guida introduttiva per Novell
- Guida introduttiva di CPP aggiornata con Linux ARM64 informazioni
- Aggiornamento rapido di Unity con informazioni iOS

## <a name="speech-sdk-160-2019-june-release"></a>Speech SDK 1.6.0:2019-versione di giugno

**Esempi**

- Esempi di avvio rapido per sintesi vocale in UWP e Unity
- Esempio di Guida introduttiva per SWIFT in iOS
- Esempi di Unity per la & vocale Riconoscimento finalità e traduzione
- Esempi di avvio rapido aggiornati per `DialogServiceConnector`

**Miglioramenti/modifiche**

- Spazio dei nomi finestra di dialogo:
  - `SpeechBotConnector` è stata rinominata `DialogServiceConnector`
  - `BotConfig` è stata rinominata `DialogServiceConfig`
  - `BotConfig::FromChannelSecret()` è stato rimappato a `DialogServiceConfig::FromBotSecret()`
  - Tutti i client di sintesi vocale diretta esistenti continuano a essere supportati dopo la ridenominazione
- Aggiornare l'adapter REST TTS per supportare il proxy, la connessione permanente
- Miglioramento del messaggio di errore quando viene passata un'area non valida
- Swift/Objective-C:
  - Segnalazione errori migliorata: i metodi che possono generare un errore sono ora presenti in due versioni: una che espone un `NSError` oggetto per la gestione degli errori e una che genera un'eccezione. Il primo è esposto a Swift. Questa modifica richiede adattamenti al codice Swift esistente.
  - Gestione degli eventi migliorata

**Correzioni di bug**

- Correzione per TTS: dove `SpeakTextAsync` future ha restituito senza attendere il completamento del rendering dell'audio
- Correzione per il marshalling delle stringhe in C# per abilitare il supporto completo del linguaggio
- Correzione del problema dell'app .NET Core per caricare la libreria principale con il Framework di destinazione net461 negli esempi
- Correzione di problemi occasionali per la distribuzione di librerie native nella cartella di output in esempi
- Correzione per la chiusura di Web socket in modo affidabile
- Correzione di un possibile arresto anomalo durante l'apertura di una connessione con carico elevato in Linux
- Correzione per i metadati mancanti nel bundle del Framework per macOS
- Correzione dei problemi con `pip install --user` in Windows

## <a name="speech-sdk-151"></a>Speech SDK 1.5.1

Si tratta di una versione di correzione di bug che interessa solo l'SDK nativo/gestito. Non influisce sulla versione JavaScript dell'SDK.

**Correzioni di bug**

- Correggere FromSubscription consente quando viene usato con la trascrizione delle conversazioni.
- Correzione di un bug nell'individuazione di parole chiave per gli assistenti vocali.

## <a name="speech-sdk-150-2019-may-release"></a>Speech SDK 1.5.0:2019-versione di maggio

**Nuove funzionalità**

- L'individuazione delle parole chiave (KWS) è ora disponibile per Windows e Linux. La funzionalità KWS potrebbe funzionare con qualsiasi tipo di microfono, il supporto ufficiale di KWS, tuttavia, è attualmente limitato agli array di microfoni presenti nell'hardware Kinect DK di Azure o nell'SDK per i dispositivi vocali.
- La funzionalità hint per la frase è disponibile tramite l'SDK. Per altre informazioni, vedere [qui](./get-started-speech-to-text.md).
- La funzionalità di trascrizione delle conversazioni è disponibile tramite l'SDK. Vedere [qui](./conversation-transcription.md).
- Aggiunta del supporto per gli assistenti vocali tramite il canale di riconoscimento vocale diretto.

**Esempi**

- Sono stati aggiunti esempi per le nuove funzionalità o i nuovi servizi supportati dall'SDK.

**Miglioramenti/modifiche**

- Sono state aggiunte diverse proprietà di riconoscimento per modificare il comportamento del servizio o i risultati del servizio (ad esempio la maschera di volgarità e altri).
- È ora possibile configurare il riconoscimento tramite le proprietà di configurazione standard, anche se è stato creato il riconoscimento `FromEndpoint` .
- Objective-C: la `OutputFormat` proprietà è stata aggiunta a `SPXSpeechConfiguration` .
- SDK supporta ora Debian 9 come distribuzione Linux.

**Correzioni di bug**

- Correzione di un problema a causa del quale la risorsa Speaker è stata distrutta troppo presto in sintesi vocale.

## <a name="speech-sdk-142"></a>Speech SDK 1.4.2

Si tratta di una versione di correzione di bug che interessa solo l'SDK nativo/gestito. Non influisce sulla versione JavaScript dell'SDK.

## <a name="speech-sdk-141"></a>Speech SDK 1.4.1

Questa è una versione solo per JavaScript. Non sono state aggiunte funzionalità. Sono state apportate le correzioni seguenti:

- Impedisci al Web Pack di caricare HTTPS-proxy-Agent.

## <a name="speech-sdk-140-2019-april-release"></a>Speech SDK 1.4.0:2019-versione di aprile

**Nuove funzionalità**

- L'SDK supporta ora il servizio di sintesi vocale come versione beta. È supportato nel desktop Windows e Linux da C++ e C#. Per altre informazioni, vedere [Cenni preliminari](text-to-speech.md#get-started)sulla sintesi vocale.
- L'SDK supporta ora i file audio MP3 e Opus/OGG come file di input di flusso. Questa funzionalità è disponibile solo in Linux da C++ e C# ed è attualmente in versione beta (altri dettagli [qui](how-to-use-codec-compressed-audio-input-streams.md)).
- L'SDK di riconoscimento vocale per Java, .NET Core, C++ e Objective-C ha acquisito supporto macOS. Il supporto Objective-C per macOS è attualmente in versione beta.
- iOS: l'SDK di riconoscimento vocale per iOS (Objective-C) è ora pubblicato anche come CocoaPod.
- JavaScript: supporto per microfoni non predefiniti come dispositivo di input.
- JavaScript: supporto del proxy per Node.js.

**Esempi**

- Sono stati aggiunti esempi per l'uso dell'SDK di riconoscimento vocale con C++ e Objective-C in macOS.
- Sono stati aggiunti esempi che dimostrano l'uso del servizio di sintesi vocale.

**Miglioramenti/modifiche**

- Python: le proprietà aggiuntive dei risultati del riconoscimento sono ora esposte tramite la `properties` Proprietà.
- Per un ulteriore supporto per lo sviluppo e il debug, è possibile reindirizzare le informazioni di diagnostica e registrazione SDK in un file di log. per ulteriori informazioni, vedere [qui](how-to-use-logging.md).
- JavaScript: migliorare le prestazioni di elaborazione audio.

**Correzioni di bug**

- Mac/iOS: un bug che ha provocato un lungo periodo di attesa quando non è stato possibile stabilire una connessione al servizio di riconoscimento vocale è stato risolto.
- Python: migliorare la gestione degli errori per gli argomenti nei callback Python.
- JavaScript: correzione della creazione di report sullo stato errato per la voce terminata in RequestSession.

## <a name="speech-sdk-131-2019-february-refresh"></a>Speech SDK 1.3.1:2019-aggiornamento di febbraio

Si tratta di una versione di correzione di bug che interessa solo l'SDK nativo/gestito. Non influisce sulla versione JavaScript dell'SDK.

**Correzione di bug**

- Correzione di una perdita di memoria quando si utilizza l'input del microfono. L'input basato sul flusso o sul file non è interessato.

## <a name="speech-sdk-130-2019-february-release"></a>Speech SDK 1.3.0:2019-versione di febbraio

**Nuove funzionalità**

- Speech SDK supporta la selezione del microfono di input tramite la `AudioConfig` classe. In questo modo è possibile trasmettere i dati audio al servizio riconoscimento vocale da un microfono non predefinito. Per ulteriori informazioni, vedere la documentazione che descrive la [selezione del dispositivo di input audio](how-to-select-audio-input-devices.md). Questa funzionalità non è ancora disponibile in JavaScript.
- Speech SDK supporta ora Unity in versione beta. Inviare commenti e suggerimenti tramite la sezione problema nel [repository di esempio GitHub](https://aka.ms/csspeech/samples). Questa versione supporta Unity in Windows x86 e x64 (applicazioni desktop o per la piattaforma UWP) e Android (ARM32/64, x86). Altre informazioni sono disponibili nell'[Avvio rapido di Unity](./get-started-speech-to-text.md?pivots=programming-language-csharp&tabs=unity).
- Il file `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (fornito nelle versioni precedenti) non è più necessario. La funzionalità è ora integrata nell'SDK di base.

**Esempi**

I nuovi contenuti seguenti sono disponibili nel [repository di esempio](https://aka.ms/csspeech/samples):

- Esempi aggiuntivi per `AudioConfig.FromMicrophoneInput` .
- Altri esempi di Python per il riconoscimento finalità e la traduzione.
- Esempi aggiuntivi per l'uso dell' `Connection` oggetto in iOS.
- Esempi aggiuntivi per Java per la traduzione con output audio.
- Nuovo esempio per l'uso dell'[API REST di trascrizione batch](batch-transcription.md).

**Miglioramenti/modifiche**

- Python
  - Miglioramento della verifica dei parametri e dei messaggi di errore in `SpeechConfig` .
  - Aggiunta del supporto per l' `Connection` oggetto.
  - Supporto per Python a 32 bit (x86) in Windows.
  - Speech SDK per Python non è incluso nella beta.
- iOS
  - L'SDK è ora basato sull'SDK per iOS versione 12.1.
  - L'SDK supporta ora iOS 9.2 e versioni successive.
  - Miglioramento della documentazione di riferimento e correzione di diversi nomi di proprietà.
- JavaScript
  - Aggiunta del supporto per l' `Connection` oggetto.
  - Aggiunta di file di definizione del tipo per JavaScript in bundle.
  - Supporto iniziale e implementazione per gli hint della frase.
  - Restituzione della raccolta di proprietà con il file JSON del servizio per il riconoscimento.
- Le DLL di Windows contengono ora una risorsa di versione.
- Se si crea un riconoscimento `FromEndpoint` , è possibile aggiungere parametri direttamente all'URL dell'endpoint. Con `FromEndpoint` non è possibile configurare il riconoscimento tramite le proprietà di configurazione standard.

**Correzioni di bug**

- Il nome utente proxy e la password proxy vuoti non erano gestiti correttamente. Con questa versione, se si imposta il nome utente proxy e la password proxy su una stringa vuota, non verranno inviati quando ci si connette al proxy.
- Gli ID sessione creati dall'SDK non erano sempre realmente casuali per alcune lingue&nbsp;/ ambienti. Aggiunta dell'inizializzazione del generatore casuale per risolvere il problema.
- Gestione migliorata del token di autorizzazione. Se si desidera utilizzare un token di autorizzazione, specificare in `SpeechConfig` e lasciare vuota la chiave della sottoscrizione. Creare quindi il sistema di riconoscimento come di consueto.
- In alcuni casi l' `Connection` oggetto non è stato rilasciato correttamente. Il problema è stato risolto.
- L'esempio di JavaScript è stato corretto per supportare l'output audio per la sintesi della traduzione anche in Safari.

## <a name="speech-sdk-121"></a>Speech SDK 1.2.1

Questa è una versione solo per JavaScript. Non sono state aggiunte funzionalità. Sono state apportate le correzioni seguenti:

- Attivazione della fine del flusso in corrispondenza di turn.end e non di speech.end.
- Correzione di un bug nel data pump audio che non pianificava l'invio successivo in caso di errore dell'invio corrente.
- Correzione del riconoscimento continuo con il token di autenticazione.
- Correzione di bug per sistemi di riconoscimento/endpoint diversi.
- Miglioramenti alla documentazione.

## <a name="speech-sdk-120-2018-december-release"></a>Speech SDK 1.2.0:2018-versione di dicembre

**Nuove funzionalità**

- Python
  - La versione beta del supporto di Python (versione 3.5 e successive) è disponibile con questa versione. Per ulteriori informazioni, vedere qui] (avvio rapido-python.md).
- JavaScript
  - Speech SDK per JavaScript è ora open source. Il codice sorgente è disponibile in [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js).
  - È ora supportato Node.js. Altre informazioni sono reperibili [qui](./get-started-speech-to-text.md).
  - È stata rimossa la restrizione di lunghezza per le sessioni audio. La riconnessione avverrà automaticamente.
- Oggetto `Connection`
  - Dalla `Recognizer` è possibile accedere a un `Connection` oggetto. Questo oggetto consente di avviare in modo esplicito la connessione al servizio e di aggiungere una sottoscrizione per connettere e disconnettere gli eventi.
    Questa funzionalità non è ancora disponibile da JavaScript e Python.
- Supporto per Ubuntu 18.04.
- Android
  - Supporto di ProGuard abilitato durante la generazione di APK.

**Miglioramenti**

- Miglioramenti nell'uso dei thread interni, con riduzione del numero di thread, blocchi e mutex.
- Miglioramento della segnalazione errori e delle informazioni sugli errori. In molti casi, i messaggi di errore non sono stati propagati fino a questo punto.
- Aggiornamento delle dipendenze di sviluppo in JavaScript per usare i moduli aggiornati.

**Correzioni di bug**

- Perdite di memoria fisse dovute a un tipo non corrispondente in `RecognizeAsync` .
- In alcuni casi perdita delle eccezioni.
- Correzione della perdita di memoria negli argomenti degli eventi di conversione.
- Correzione di un problema di blocco per la riconnessione in sessioni a esecuzione prolungata.
- Correzione di un problema che potrebbe causare la mancata riuscita delle traduzioni non riuscite.
- C#: se un' `async` operazione non è stata attesa nel thread principale, è possibile che il riconoscimento possa essere eliminato prima del completamento dell'attività asincrona.
- Java: è stato risolto un problema che causava un arresto anomalo della macchina virtuale Java.
- Objective-C: mapping di enumerazione fisso; RecognizedIntent è stato restituito invece di `RecognizingIntent` .
- JavaScript: impostare il formato di output predefinito su' Simple ' in `SpeechConfig` .
- JavaScript: rimozione dell'incoerenza tra le proprietà nell'oggetto config in JavaScript e in altri linguaggi.

**Esempi**

- Sono stati aggiornati e corretti diversi esempi, ad esempio le voci di output per la traduzione e così via.
- Aggiunta di esempi di Node.js nel [repository degli esempi](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-110"></a>Speech SDK 1.1.0

**Nuove funzionalità**

- Supporto per Android x86/x64.
- Supporto del proxy: nell' `SpeechConfig` oggetto è ora possibile chiamare una funzione per impostare le informazioni sul proxy (nome host, porta, nome utente e password). Questa funzionalità non è ancora disponibile in iOS.
- Messaggi e codice di errore migliorati. Se un riconoscimento ha restituito un errore, l'elemento `Reason` (nell'evento annullato) o `CancellationDetails` (nel risultato del riconoscimento) è già stato impostato su `Error`. L'evento annullato contiene ora due membri aggiuntivi: `ErrorCode` e `ErrorDetails`. Se il server ha restituito l'errore corredato da informazioni aggiuntive, le informazioni saranno ora disponibili nei nuovi membri.

**Miglioramenti**

- È stata aggiunta una verifica supplementare nella configurazione del sistema di riconoscimento ed è stato aggiunto un nuovo messaggio di errore.
- È stata migliorata la gestione di un silenzio prolungato nel mezzo di un file audio.
- Pacchetto NuGet: per i progetti .NET Framework, impedisce la compilazione con una configurazione AnyCPU.

**Correzioni di bug**

- Sono state corrette varie eccezioni rilevate nei sistemi di riconoscimento. Inoltre, le eccezioni vengono rilevate e convertite in un `Canceled` evento.
- È stata corretta una perdita di memoria nella gestione delle proprietà.
- È stato corretto un bug per il quale un file di input audio può determinare l'arresto anomalo del sistema di riconoscimento.
- È stato risolto un bug per il quale si continuano a ricevere eventi anche dopo un evento di arresto della sessione.
- Sono state corrette alcune race condition nel threading.
- È stato corretto un problema di compatibilità iOS che può determinare un arresto anomalo.
- Sono stati apportati miglioramenti di stabilità per il supporto di microfoni Android.
- È stato risolto un bug per il quale un sistema di riconoscimento in JavaScript ignorava la lingua di riconoscimento.
- Correzione di un bug che impedisce l'impostazione di `EndpointId` (in alcuni casi) in JavaScript.
- Modifica dell'ordine del parametro in AddIntent in JavaScript e aggiunta della `AddIntent` firma JavaScript mancante.

**Esempi**

- Sono stati aggiunti esempi di C++ e C# per l'utilizzo dei flussi pull e push nel [repository di esempio](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-101"></a>Speech SDK 1.0.1

Miglioramenti apportati alla stabilità e correzioni di bug:

- Risoluzione di un potenziale errore irreversibile dovuto a una race condition nell'eliminazione dello strumento di riconoscimento.
- Correzione di un potenziale errore irreversibile quando si verificano proprietà non impostate.
- Aggiunta di un ulteriore controllo di errori e parametri.
- Objective-C: risoluzione di un potenziale errore irreversibile causato dall'override del nome in NSString.
- Objective-C: modifica della visibilità dell'API
- JavaScript: correzioni relative a eventi e payload correlati.
- Miglioramenti alla documentazione.

Al [repository di esempi](https://aka.ms/csspeech/samples) è stato aggiunto un nuovo esempio per JavaScript.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>Speech SDK 1.0.0 di Servizi cognitivi: versione di settembre 2018

**Nuove funzionalità**

- Supporto per Objective-C in iOS. Vedere la [Guida introduttiva su Objective-C per iOS](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/objectivec/ios/from-microphone).
- Supporto per JavaScript nel browser. Vedere la [Guida introduttiva su JavaScript](./get-started-speech-to-text.md).

**Modifiche di rilievo**

- In questa versione sono state introdotte alcune modifiche di rilievo.
  Per informazioni dettagliate, vedere [Questa pagina](https://aka.ms/csspeech/breakingchanges_1_0_0) .

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Speech SDK 0.6.0 di Servizi cognitivi: versione di agosto 2018

**Nuove funzionalità**

- Le app UWP compilate con Speech SDK ora possono superare il Kit di certificazione app Windows (WACK).
  Consultare la [Guida introduttiva della piattaforma UWP](./get-started-speech-to-text.md?pivots=programming-language-chsarp&tabs=uwp).
- Supporto per .NET Standard 2.0 in Linux (Ubuntu 16.04 x64).
- Sperimentale: supporto di Java 8 in Windows (64 bit) e Linux (Ubuntu 16.04 x64).
  Consultare la [Guida introduttiva di Java Runtime Environment](./get-started-speech-to-text.md?pivots=programming-language-java&tabs=jre).

**Modifica funzionale**

- Informazioni aggiuntive dettagliate sull'errore in caso di errori di connessione.

**Modifiche di rilievo**

- In Java (Android), la funzione `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` non richiede più un parametro di percorso. Il percorso viene ora rilevato automaticamente in tutte le piattaforme supportate.
- La funzione di accesso get della proprietà `EndpointUrl` in Java e C# è stata rimossa.

**Correzioni di bug**

- In Java, il risultato di sintesi audio sul sistema di riconoscimento di traduzione è ora implementato.
- È stato risolto un bug che potrebbe causare un maggior numero di socket aperti e inutilizzati e thread inattivi.
- È stato risolto un problema in cui un riconoscimento con esecuzione prolungata terminava la trasmissione a metà.
- Correzione di una race condition nel sistema di riconoscimento di arresto.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Speech SDK 0.5.0 di Servizi cognitivi: versione di luglio 2018

**Nuove funzionalità**

- Supporto della piattaforma Android (API 23: Android Marshmallow 6.0 o versione successiva). Consultare la [Guida introduttiva di Android](./get-started-speech-to-text.md?pivots=programming-language-java&tabs=android).
- Supporto di .NET Standard 2.0 in Windows. Consultare la [Guida introduttiva di .NET Core](./get-started-speech-to-text.md?pivots=programming-language-csharp&tabs=dotnetcore).
- Sperimentale: supporto di UWP in Windows (versione 1709 o successiva).
  - Consultare la [Guida introduttiva della piattaforma UWP](./get-started-speech-to-text.md?pivots=programming-language-csharp&tabs=uwp).
  - Nota: le app UWP compilate con Speech SDK non superano ancora il Kit di certificazione app Windows (WACK).
- Supporto del riconoscimento a esecuzione prolungata con riconnessione automatica.

**Modifiche funzionali**

- `StartContinuousRecognitionAsync()` supporta il riconoscimento a esecuzione prolungata.
- Il risultato del riconoscimento contiene più campi. Scostamento da inizio audio e durata (entrambi in tick) del testo riconosciuto, valori aggiuntivi che rappresentano lo stato di riconoscimento, ad esempio `InitialSilenceTimeout` e `InitialBabbleTimeout`.
- Supporto del token di autorizzazione per la creazione di istanze di factory.

**Modifiche di rilievo**

- Eventi di riconoscimento: `NoMatch` il tipo di evento è stato unito nell' `Error` evento.
- SpeechOutputFormat in C# è stato rinominato in `OutputFormat` per restare allineato con C++.
- Il tipo restituito di alcuni metodi dell'interfaccia `AudioInputStream` è stato leggermente modificato:
  - In Java, il metodo `read` restituisce ora `long` invece di `int`.
  - In C#, il metodo `Read` restituisce ora `uint` invece di `int`.
  - In C++, i metodi `Read` e `GetFormat` restituiscono ora `size_t` invece di `int`.
- C++: le istanze di flussi di input audio possono ora essere passate solo come `shared_ptr`.

**Correzioni di bug**

- Sono stati corretti i valori restituiti errati nel risultato alla scadenza di `RecognizeAsync()`.
- È stata rimossa la dipendenza dalle librerie di Media Foundation in Windows. L'SDK usa ora le API Audio Core.
- Correzione della documentazione: è stata aggiunta una pagina relativa alle [aree](regions.md) per descrivere le aree supportate.

**Problema noto**

- Speech SDK per Android non segnala i risultati della sintesi vocale per la traduzione. Questo problema verrà risolto nella prossima versione.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Speech SDK di Servizi cognitivi 0.4.0: versione di giugno 2018

**Modifiche funzionali**

- AudioInputStream

  Uno strumento di riconoscimento può ora usare un flusso come origine audio. Per informazioni dettagliate, vedere la [guida pratica](how-to-use-audio-input-streams.md).

- Formato dettagliato dell'output

  Durante la creazione di uno `SpeechRecognizer`, è possibile richiedere un formato di output `Detailed` o `Simple`. Il `DetailedSpeechRecognitionResult` contiene punteggio di attendibilità, testo riconosciuto, forma lessicale non elaborata, forma normalizzata e forma normalizzata con messaggi dal contenuto volgare mascherati.

**Modifica di rilievo**

- Modifica da `SpeechRecognitionResult.RecognizedText` a `SpeechRecognitionResult.Text` in linguaggio C#.

**Correzioni di bug**

- È stato corretto un possibile problema di callback nel livello USP durante l'arresto.
- Se un riconoscimento usa un file di input audio, significa che esso contiene l'handle del file più a lungo rispetto al necessario.
- Sono stati rimossi diversi deadlock tra message pump e strumento di riconoscimento.
- Attiva un risultato `NoMatch` quando la risposta dal servizio è scaduta.
- Le librerie di Media Foundation in Windows sono a caricamento ritardato. Questa libreria è richiesta solo per l'input del microfono.
- La velocità di caricamento dei dati audio è limitata a circa due volte la velocità dell'audio originale.
- In Windows, gli assembly C# .NET hanno ora un nome sicuro.
- Correzione della documentazione: `Region` è un'informazione obbligatoria per la creazione di un riconoscimento.

Sono stati aggiunti altri esempi che sono costantemente in corso l'aggiornamento. Per il set di esempi più recente, vedere il [repository GitHub degli esempi di Speech SDK](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Speech SDK di Servizi cognitivi 0.2.12733: versione di maggio 2018

Questa è la prima versione di anteprima pubblica di Speech SDK di Servizi cognitivi.
