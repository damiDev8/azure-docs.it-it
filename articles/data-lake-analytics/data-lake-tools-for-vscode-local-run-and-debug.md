---
title: Eseguire il debug di processi U-SQL-codice Strumenti Azure Data Lake per Visual Studio
description: Informazioni su come usare Strumenti Azure Data Lake per Visual Studio Code per l'esecuzione e il debug dei processi U-SQL in locale.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 07/14/2017
ms.openlocfilehash: 194fe3494915be9f6784e1596e647885634817ad
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2021
ms.locfileid: "97969011"
---
# <a name="run-u-sql-and-debug-locally-in-visual-studio-code"></a>Eseguire U-SQL e il debug in locale in Visual Studio Code
Questo articolo descrive come eseguire i processi di U-SQL in un computer di sviluppo locale per velocizzare le fasi iniziali di codifica o eseguire il debug del codice in locale in Visual Studio Code. Per istruzioni su Strumenti Azure Data Lake per Visual Studio Code, vedere [Usare Strumenti Azure Data Lake per Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

Solo le installazioni di Windows degli strumenti Azure Data Lake per Visual Studio supportano l'azione per l'esecuzione e il debug di U-SQL in locale. Le installazioni in sistemi operativi macOS e Linux non supportano questa funzionalità.

## <a name="set-up-the-u-sql-local-run-environment"></a>Configurare un ambiente di esecuzione locale di U-SQL

1. Premere CTRL+MAIUSC+P per aprire il riquadro comandi e quindi digitare **ADL: Download Local Run Package** (ADL: Scarica pacchetto esecuzione locale) per scaricare i pacchetti.  

   ![Scaricare i pacchetti di dipendenza ADL LocalRun](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/downloadtheadllocalrunpackage.png)

2. Trovare i pacchetti di dipendenza nel percorso visualizzato nel pannello **Output**, quindi installare BuildTools e Win10SDK 10240. Di seguito è fornito un esempio di percorso:  
`C:\Users\xxx\AppData\Roaming\LocalRunDependency` 

   ![Individuare i pacchetti di dipendenze](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   2.1 Per installare **BuildTools**, fare clic su visualcppbuildtools_full.exe nella cartella LocalRunDependency e quindi seguire le istruzioni della procedura guidata.   

    ![Installare BuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   2.2 Per installare **Win10SDK 10240**, fare clic su sdksetup.exe nella cartella LocalRunDependency/Win10SDK_10.0.10240_2 e quindi seguire le istruzioni della procedura guidata.  

    ![Installare Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Configurare la variabile di ambiente. Impostare la variabile di ambiente **SCOPE_CPP_SDK** per:  
`C:\Users\XXX\AppData\Roaming\LocalRunDependency\CppSDK_3rdparty`  


## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>Avviare il servizio di esecuzione locale e inviare il processo U-SQL a un account locale 
Per gli utenti non esperti usare il comando **ADL: Download Local Run Package** (ADL: Scarica pacchetto esecuzione locale) per scaricare i pacchetti di esecuzione locali se in precedenza non è stato [configurato l'ambiente di esecuzione locale di U-SQL](#set-up-the-u-sql-local-run-environment).

1. Premere CTRL+MAIUSC+P per aprire il riquadro comandi e immettere **ADL: Start Local Run Service** (ADL: avvia servizio di esecuzione locale).   
2. Selezionare **Accetto** per accettare le condizioni di licenza software Microsoft per la prima volta. 

   ![Accettare le condizioni di licenza software Microsoft](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. Viene aperta la console dei comandi. Dato che si accede per la prima volta immettere **3** e quindi individuare il percorso di cartella locale per l'input e l'output di dati. Se non si riesce a definire il percorso con barre rovesciate, provare a usare le barre. Per le altre opzioni è possibile usare il valore predefinito.

   ![Console dei comandi per esecuzione locale di Strumenti Data Lake per Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Premere CTRL+MAIUSC+P per aprire il riquadro comandi, immettere **ADL: Submit Job** (ADL: Invia processo), quindi selezionare **Local** (Locale) per inviare il processo all'account locale.

   ![Selezionare Local (Locale) in Strumenti Data Lake per Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. Dopo aver inviato il processo è possibile visualizzare i dettagli di invio. Per visualizzare i dettagli di invio, selezionare **jobUrl** nella finestra **output** . È possibile visualizzare lo stato di invio del processo anche dalla console dei comandi. Immettere **7** nella console dei comandi se si desidera conoscere ulteriori dettagli del processo.

   ![Output dell'esecuzione locale di Strumenti Data Lake per Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Stato comandi dell'esecuzione locale di Strumenti Data Lake per Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>Avviare il debug locale per il processo U-SQL  
Per gli utenti non esperti:

1. Usare il comando **ADL: Download Local Run Package** (ADL: Scarica pacchetto esecuzione locale) per scaricare i pacchetti di esecuzione locali se in precedenza non è stato [configurato l'ambiente di esecuzione locale di U-SQL](#set-up-the-u-sql-local-run-environment).
2. Installare .NET Core SDK 2.0 come suggerito nella finestra di messaggio, se non è già installato.
 
  ![Promemoria di installazione di Dotnet](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/remind-install-dotnet.png)
3. Installare C# per Visual Studio Code come suggerito nella finestra di messaggio, se non è già installato. Fare clic su **Installa** per continuare e quindi riavviare VSCode.

![Promemoria di installazione di C#](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/install-csharp.png)

Attenersi alla procedura seguente per eseguire il debug locale:
  
1. Premere CTRL+MAIUSC+P per aprire il riquadro comandi e immettere **ADL: Start Local Run Service** (ADL: avvia servizio di esecuzione locale). Viene aperta la console dei comandi. Assicurarsi che **DataRoot** sia impostata.
2. Impostare un punto di interruzione nel code-behind C#.
3. Nell'editor di script fare clic con il pulsante destro del mouse e selezionare **ADL: Local Debug** (ADL: Debug locale).
    
   ![Risultati debug locale di Strumenti Data Lake per Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Passaggi successivi
* [Usare gli strumenti di Azure Data Lake per Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
* [Eseguire lo sviluppo U-SQL con Python, R e CSharp per Azure Data Lake Analytics in VSCode](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
* [Introduzione ad Analisi Data Lake di Azure mediante Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Introduzione ad Analisi Data Lake mediante il portale di Azure](data-lake-analytics-get-started-portal.md)
* [Utilizzare gli strumenti Data Lake per Visual Studio per lo sviluppo di applicazioni U-SQL](data-lake-analytics-data-lake-tools-get-started.md)
* [Usare il catalogo Data Lake Analytics(U-SQL)](./data-lake-analytics-u-sql-get-started.md)
