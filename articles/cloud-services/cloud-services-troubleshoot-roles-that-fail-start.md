---
title: Risolvere i problemi dei ruoli che non vengono avviati | Documentazione Microsoft
description: Informazioni su alcuni motivi comuni del mancato avvio di un ruolo del servizio cloud. Include anche soluzioni per questi problemi.
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 2453fa2d9b4e78b60d4922e09347799266a84cff
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2021
ms.locfileid: "98743220"
---
# <a name="troubleshoot-azure-cloud-service-classic-roles-that-fail-to-start"></a>Risolvere i problemi dei ruoli del servizio cloud di Azure (versione classica) che non vengono avviati

> [!IMPORTANT]
> [Servizi cloud di Azure (supporto esteso)](../cloud-services-extended-support/overview.md) è un nuovo modello di distribuzione basato su Azure Resource Manager per il prodotto servizi cloud di Azure.Con questa modifica, i servizi cloud di Azure in esecuzione nel modello di distribuzione basato su Service Manager di Azure sono stati rinominati come servizi cloud (versione classica) e tutte le nuove distribuzioni devono usare i [servizi cloud (supporto esteso)](../cloud-services-extended-support/overview.md).

Ecco alcuni problemi e soluzioni comini correlati ai ruoli di Servizi cloud di Azure che non vengono avviati.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>File DLL o dipendenze mancanti
I problemi dovuti a ruoli che non rispondono e ruoli che passano in ciclo tra gli stati **Inizializzazione**, **Occupato** e **Arresto** possono essere provocati da file DLL o assembly mancanti.

I sintomi di file DLL o assembly mancati includono i seguenti:

* L'istanza del ruolo passa in ciclo tra gli stati **Inizializzazione**, **Occupato** e **Arresto**.
* L'istanza del ruolo è passata allo stato **Pronto** , ma, se si passa all'applicazione Web, la pagina non viene visualizzata.

Sono disponibili diversi metodi consigliati per esaminare questi problemi.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Diagnosticare i problemi di DLL mancanti in un ruolo Web
Quando si passa a un sito Web distribuito in un ruolo Web e il browser visualizza un errore server simile al seguente, è possibile che indichi una DLL mancante.

![Errore del server nell'applicazione '/'.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Diagnosticare i problemi disabilitando gli errori personalizzati
Per visualizzare informazioni sugli errori più completi, configurare il file web.config per il ruolo Web per poter disattivare la modalità di errore personalizzata e ridistribuire il servizio.

Per visualizzare errori più completi senza usare Desktop remoto:

1. Aprire la soluzione in Microsoft Visual Studio.
2. In **Esplora soluzioni** individuare il file web.config e aprirlo.
3. Nel file web.config individuare la sezione system.web e aggiungere la riga seguente:

    ```xml
    <customErrors mode="Off" />
    ```
4. Salvare il file.
5. Ricreare il pacchetto e ridistribuire il servizio.

Quando il servizio viene ridistribuito, verrà visualizzato un messaggio di errore con il nome dell'assembly o della DLL mancante.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Diagnosticare i problemi visualizzando l'errore in remoto
È possibile usare Desktop remoto per accedere al ruolo e visualizzare informazioni sugli errori più complete in remoto. Seguire questa procedura per visualizzare gli errori usando Desktop remoto:

1. Verificare che sia installato Azure SDK 1.3 o versione successiva.
2. Durante la distribuzione della soluzione con Visual Studio, abilitare Desktop remoto. Per altre informazioni, vedere [Enable Remote Desktop Connection for a Role in Azure Cloud Services using Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md) (Abilitare una connessione Desktop remoto per un ruolo in Servizi cloud di Azure con Visual Studio).
3. Quando lo stato dell'istanza è **Pronto**, nel portale di Microsoft Azure accedere in remoto all'istanza. Per altre informazioni sull'utilizzo di Desktop remoto con Servizi cloud di Microsoft Azure, vedere [Accedere in remoto alle istanze del ruolo](cloud-services-role-enable-remote-desktop-new-portal.md#remote-into-role-instances).
5. Accedere alla macchina virtuale usando le credenziali specificate durante la configurazione di Desktop remoto.
6. Aprire una finestra di comando.
7. Digitare `IPconfig`.
8. Annotare il valore dell'indirizzo IPV4.
9. Aprire Internet Explorer.
10. Digitare l'indirizzo e il nome dell'applicazione Web, Ad esempio: `http://<IPV4 Address>/default.aspx`.

Se si passa al sito Web, ora verranno restituiti messaggi di errore più espliciti:

* Errore del server nell'applicazione '/'.
* Descrizione: Eccezione non gestita durante l'esecuzione della richiesta Web corrente. Per altre informazioni sull'errore e sul suo punto di origine nel codice, vedere l'analisi dello stack.
* Dettagli dell'eccezione: System.IO.FIleNotFoundException: Impossibile caricare il file o l'assembly 'Microsoft.WindowsAzure.StorageClient, Version=1.1.0.0, Culture=neutral, PublicKeyToken=31bf856ad364e35' o una delle relative dipendenze. Non è possibile trovare il file specificato.

Ad esempio:

![Errore esplicito del server nell'applicazione '/'](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Diagnosticare i problemi usando l'emulatore di calcolo
È possibile utilizzare l'emulatore di calcolo Microsoft Azure per diagnosticare e risolvere i problemi relativi a dipendenze mancanti ed errori di web.config.

Per ottenere risultati ottimali con questo metodo di diagnosi, è consigliabile usare un computer o una macchina virtuale in cui è presente un'installazione pulita di Windows. Per simulare in modo ottimale l'ambiente Azure, usare Windows Server 2008 R2 x64.

1. Installare la versione autonoma di [Azure SDK](https://azure.microsoft.com/downloads/).
2. Nel computer di sviluppo compilare il progetto di servizio cloud.
3. In Esplora risorse passare alla cartella bin\debug del progetto di servizio cloud.
4. Copiare la cartella con estensione csx e il file con estensione cscfg nel computer usato per eseguire il debug dei problemi.
5. Nel computer pulito aprire una finestra del prompt dei comandi di Azure SDK e digitare `csrun.exe /devstore:start`.
6. Al prompt dei comandi digitare `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.
7. All'avvio del ruolo, verranno visualizzate informazioni dettagliate sull'errore in Internet Explorer. È anche possibile usare gli strumenti di risoluzione dei problemi standard di Windows per diagnosticare ulteriormente il problema.

## <a name="diagnose-issues-by-using-intellitrace"></a>Diagnosticare i problemi usando IntelliTrace
Per i ruoli di lavoro e i ruoli Web che fanno uso di .NET Framework 4, è possibile usare [IntelliTrace](/visualstudio/debugger/intellitrace), disponibile in Microsoft Visual Studio Enterprise.

Seguire questa procedura per distribuire il servizio con IntelliTrace abilitato:

1. Verificare che sia installato Azure SDK 1.3 o versione successiva.
2. Distribuire la soluzione usando Visual Studio. Durante la distribuzione selezionare la casella di controllo **Abilita IntelliTrace per ruoli .NET 4** .
3. Una volta avviata l'istanza, aprire **Esplora server**.
4. Espandere il nodo **Azure\\Servizi cloud** e trovare la distribuzione.
5. Espandere la distribuzione finché non vengono visualizzate le istanze del ruolo. Fare clic con il pulsante destro del mouse su una delle istanze.
6. Selezionare **Visualizza log IntelliTrace**. Verrà aperto **Riepilogo IntelliTrace**.
7. Individuare la sezione delle eccezioni del riepilogo. Se sono presenti eccezioni, la sezione sarà denominata **Dati eccezione**.
8. Espandere **Dati eccezione** e cercare errori **System.IO.FileNotFoundException** simili al seguente:

![Dati eccezione, assembly o file mancante](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Risolvere DLL e assembly mancanti
Per risolvere errori di assembly e DLL mancanti, seguire questa procedura:

1. Aprire la soluzione in Visual Studio.
2. In **Esplora soluzioni** aprire la cartella **Riferimenti**.
3. Fare clic sull'assembly specificato nell'errore.
4. Nel riquadro **Proprietà** trovare la proprietà **Copia localmente** e impostare il valore su **True**.
5. Ridistribuire il servizio cloud.

Dopo avere verificato che tutti gli errori sono stati corretti, è possibile distribuire il servizio senza selezionare la casella di controllo **Abilita IntelliTrace per ruoli .NET 4** .

## <a name="next-steps"></a>Passaggi successivi
Altri [articoli sulla risoluzione dei problemi](../index.yml?product=cloud-services&tag=top-support-issue) per i servizi cloud.

Per informazioni su come risolvere i problemi dei ruoli del servizio cloud utilizzando i dati di diagnostica del calcolo Azure PaaS, vedere la [serie di blog di Kevin Williamson](/archive/blogs/kwill/windows-azure-paas-compute-diagnostics-data).
