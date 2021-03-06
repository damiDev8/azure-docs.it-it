---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 02/20/2020
ms.author: trbye
ms.openlocfilehash: 7ff53a700ab1b158d94af84907940f56afa74eb5
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99213554"
---
In questa Guida introduttiva, si apprenderà come usare Speech Devices SDK per Windows per compilare un prodotto abilitato al riconoscimento vocale o usarlo come dispositivo di [trascrizione conversazione](../conversation-transcription.md). Per la trascrizione conversazione è supportato solo [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/). Per altri tipi di riconoscimento vocale sono supportati gruppi di microfoni lineari che forniscono una geometria per gruppi di microfoni.

L'applicazione è compilata con il pacchetto Speech SDK ed Eclipse Java IDE (v4) in Windows a 64 bit. Viene eseguito su un ambiente Java 8 runtime a 64 bit (JRE).

Per riprodurre le procedure di questa guida, è necessario un account di [Servizi cognitivi di Azure](../overview.md#try-the-speech-service-for-free) con una risorsa del servizio Voce.

Il codice sorgente dell'[applicazione di esempio](https://aka.ms/sdsdk-download-JRE) è incluso in Speech Devices SDK ed è anche [disponibile in GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Prerequisiti

Questa guida introduttiva richiede:

* Sistema operativo: Windows a 64 bit
* Un gruppo di microfoni come [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)
* [Ambiente IDE Java Eclipse](https://www.eclipse.org/downloads/)
* Solo [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) o [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Microsoft Visual C++ Redistributable Package](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)
* Una chiave di sottoscrizione di Azure per il servizio Voce. [È possibile ottenerne una gratuitamente](../overview.md#try-the-speech-service-for-free).
* Scaricare la versione più recente di [Speech Devices SDK](https://aka.ms/sdsdk-download-JRE) per Java ed estrarre il file zip nella directory di lavoro.
   > [!NOTE]
   > Questo argomento di avvio rapido presuppone che l'app venga estratta in C:\SDSDK\JRE-Sample-Release

La trascrizione conversazione è attualmente disponibile solo per "en-US" e "zh-CN", nelle aree geografiche "centralus" e "eastasia". È necessario avere una chiave di riconoscimento vocale in una di queste aree per usare la trascrizione conversazione.

Se si prevede di usare la finalità è necessaria una sottoscrizione al [servizio LUIS (Language Understanding)](../../luis/luis-how-to-azure-subscription.md). Per altre informazioni su LUIS e sul riconoscimento delle finalità, vedere [Riconoscere le finalità voce con LUIS, C#](../how-to-recognize-intents-from-speech-csharp.md). Un [modello LUIS di esempio](https://aka.ms/sdsdk-luis) è disponibile per questa app.

## <a name="create-and-configure-the-project"></a>Creare e configurare il progetto

1. Avviare Eclipse.

1. Nell’**utilità di avvio di Eclipse**, nel campo **Area di lavoro**, inserire il nome di una nuova directory dell’area di lavoro. Selezionare quindi **Avvio**.

   ![Screenshot che mostra l'utilità di avvio di Eclipse in cui immettere il nome della directory dell'area di lavoro.](../media/speech-devices-sdk/eclipse-launcher.png)

1. Dopo qualche istante verrà visualizzata la finestra principale dell'IDE di Eclipse. Se è presente, chiudere la schermata iniziale.

1. Dalla barra dei menu di Eclipse, creare un nuovo progetto selezionando **File** > **New** > **Java Project**. Se non è disponibile scegliere **Progetto** e quindi **Progetto Java**.

1. Viene avviata la procedura guidata **Nuovo progetto Java**. **Sfogliare** per cercare il percorso del progetto di esempio. Selezionare **Fine**.

   ![Screenshot della procedura guidata Nuovo progetto Java.](../media/speech-devices-sdk/eclipse-new-java-project.png)

1. In **Esplora pacchetti**, fare clic con il pulsante destro del mouse sul progetto. Selezionare **Configura** > **Converti in progetto Maven** dal menu di scelta rapida. Selezionare **Fine**.

   ![Screenshot di Esplora pacchetti](../media/speech-devices-sdk/eclipse-convert-to-maven.png)

1. Aprire il file pom.xml e modificarlo.

    Alla fine del file, prima del tag di chiusura `</project>`, creare elementi `repositories` e `dependencies`, come illustrato di seguito, e verificare che `version` corrisponda alla versione corrente:
    ```xml
    <repositories>
         <repository>
             <id>maven-cognitiveservices-speech</id>
             <name>Microsoft Cognitive Services Speech Maven Repository</name>
             <url>https://csspeechstorage.blob.core.windows.net/maven/</url>
         </repository>
    </repositories>
 
    <dependencies>
        <dependency>
             <groupId>com.microsoft.cognitiveservices.speech</groupId>
             <artifactId>client-sdk</artifactId>
             <version>1.15.0</version>
        </dependency>
    </dependencies>
   ```

1. Copiare il contenuto di **Windows-x64** nel percorso del progetto Java, ad esempio **C:\SDSDK\JRE-Sample-Release**

1. Copiare `kws.table`, `participants.properties` e `Microsoft.CognitiveServices.Speech.extension.pma.dll` nella cartella del progetto **target/classes**

## <a name="configure-the-sample-application"></a>Configurare l'applicazione di esempio

1. Aggiungere la chiave di sottoscrizione del Servizio di riconoscimento vocale al codice sorgente. Se si vuole provare la funzionalità Riconoscimento finalità, aggiungere anche la chiave di sottoscrizione e l'ID applicazione del [servizio Language Understanding](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

   Per il riconoscimento vocale e LUIS, le informazioni vanno in `FunctionsList.java`:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

   Se si usa la trascrizione conversazione, le informazioni sulla chiave di riconoscimento e sull'area sono necessarie anche in `Cts.java`:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
   ```

1. La parola chiave predefinita è "Computer". È anche possibile provare una delle altre parole chiave disponibili, come "Machine" o "Assistant". I file di risorse per queste parole chiave alternative sono disponibili nella cartella keyword di Speech Devices SDK. `C:\SDSDK\JRE-Sample-Release\keyword\Computer`, ad esempio, contiene i file usati per la parola chiave "Computer".

    > [!TIP]
    > È anche possibile [creare una parola chiave personalizzata](../custom-keyword-basics.md).

    Per usare una nuova parola chiave, aggiornare la riga seguente in `FunctionsList.java` e copiare la parola chiave nell'app. Ad esempio, per usare la parola chiave 'Machine' del pacchetto di parole chiave `machine.zip`:

   * Copiare il file `kws.table` dal pacchetto ZIP nella cartella del progetto **target/classes**.
   * Aggiornare `FunctionsList.java` con il nome della parola chiave:

     ```java
     private static final String Keyword = "Machine";
     ```

## <a name="run-the-sample-application-from-eclipse"></a>Eseguire l'applicazione di esempio da Eclipse

1. Dalla barra dei menu di Eclipse scegliere **Run** > **Run As** > **Java Application**. Quindi selezionare **FunctionsList** e **OK**.

   ![Screenshot di Select Java Application](../media/speech-devices-sdk/eclipse-run-sample.png)

1. L'applicazione di esempio di Speech Devices SDK viene avviata e visualizza le opzioni seguenti:

   ![Screenshot di un'applicazione di esempio di Speech Devices SDK e opzioni.](../media/speech-devices-sdk/java-sample-app-windows.png)

1. Provare la nuova **demo di Trascrizione conversazione**. Avviare la trascrizione con **Avvia** > **sessione**. Per impostazione predefinita, ogni utente è un guest. Se però si hanno le firme vocali dei partecipanti, è possibile inserirle in un file `participants.properties` nella cartella del progetto **target/classes**. Per generare la firma vocale, vedere [Trascrivere le conversazioni (SDK)](../how-to-use-conversation-transcription.md).

   ![Screenshot di un'applicazione demo di trascrizione conversazione.](../media/speech-devices-sdk/cts-sample-app-windows.png)

## <a name="create-and-run-a-standalone-application"></a>Creare ed eseguire un'applicazione autonoma

1. In **Esplora pacchetti**, fare clic con il pulsante destro del mouse sul progetto. Scegliere **Esporta**.

1. Verrà visualizzata la finestra **Esporta**. Espandere **Java** e selezionare **Runnable JAR file** (File JAR eseguibile) e quindi selezionare **Avanti**.

   ![Screenshot che mostra la finestra Esporta in cui selezionare il file JAR eseguibile.](../media/speech-devices-sdk/eclipse-export-windows.png)

1. Verrà visualizzata la finestra **Runnable JAR File Export**. Scegliere una **destinazione di esportazione** per l'applicazione e quindi selezionare **Fine**.

   ![Screenshot che mostra la finestra di esportazione file del file JAR eseguibile in cui scegliere la destinazione di esportazione.](../media/speech-devices-sdk/eclipse-export-jar-windows.png)

1. Inserire `kws.table`, `participants.properties`, `unimic_runtime.dll`, `pma.dll` e `Microsoft.CognitiveServices.Speech.extension.pma.dll` nella cartella di destinazione selezionata in precedenza perché questi file sono necessari per l'applicazione.

1. Per eseguire l'applicazione autonoma

   ```powershell
   java -jar SpeechDemo.jar
   ```