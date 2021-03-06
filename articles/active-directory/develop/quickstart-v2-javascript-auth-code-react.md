---
title: 'Guida introduttiva: gli utenti di accesso in JavaScript reagiscono alle app a singola pagina (SPA) con il codice di autenticazione e chiamano Microsoft Graph | Azure'
titleSuffix: Microsoft identity platform
description: In questa Guida introduttiva si apprenderà come un'applicazione a singola pagina REACT (SPA) di JavaScript possa accedere agli utenti di account personali, account di lavoro e account dell'Istituto di istruzione utilizzando il flusso del codice di autorizzazione e chiamare Microsoft Graph.
services: active-directory
author: j-mantu
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 01/14/2021
ms.author: jamesmantu
ms.custom: aaddev, scenarios:getting-started, languages:JavaScript, devx-track-js
ms.openlocfilehash: 9c30b1ff7b6e708fdd64d082b14b4e9d3c7a0f1a
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2021
ms.locfileid: "99583316"
---
# <a name="quickstart-sign-in-and-get-an-access-token-in-a-react-spa-using-the-auth-code-flow"></a>Guida introduttiva: accedere e ottenere un token di accesso in una SPA React usando il flusso del codice di autenticazione

In questa Guida introduttiva viene scaricato ed eseguito un esempio di codice che illustra il modo in cui un'applicazione a singola pagina REACT (SPA) di JavaScript può accedere agli utenti e chiamare Microsoft Graph usando il flusso del codice di autorizzazione. L'esempio di codice dimostra come ottenere un token di accesso per chiamare l'API Microsoft Graph o qualsiasi API Web. 

Per un'illustrazione, vedere [Funzionamento dell'esempio](#how-the-sample-works).

Questa Guida introduttiva usa MSAL React con il flusso del codice di autorizzazione. Per una guida introduttiva simile che usa MSAL.js con il flusso implicito, vedere [Guida introduttiva: accedere agli utenti nelle app a singola pagina di JavaScript](./quickstart-v2-javascript.md).

## <a name="prerequisites"></a>Prerequisiti

* Sottoscrizione di Azure - [Creare una sottoscrizione di Azure gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) o un altro editor di codice

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Registrare e scaricare l'app dell'avvio rapido
> Per avviare l'applicazione di avvio rapido, usare una delle opzioni seguenti.
>
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Opzione 1 (Modalità rapida): Registrare e configurare automaticamente l'app e quindi scaricare l'esempio di codice
>
> 1. Passare all'esperienza di avvio rapido <a href="https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs" target="_blank">Portale di Azure - Registrazioni app<span class="docon docon-navigate-external x-hidden-focus"></span></a>.
> 1. Immettere un nome per l'applicazione.
> 1. In **Tipi di account supportati** selezionare **Account in qualsiasi directory organizzativa e account Microsoft personali**.
> 1. Selezionare **Registra**.
> 1. Passare al riquadro dell'avvio rapido e seguire le istruzioni per scaricare e configurare automaticamente la nuova applicazione.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Opzione 2 (Manuale): Registrare e configurare manualmente l'applicazione e il codice di esempio
>
> #### <a name="step-1-register-your-application"></a>Passaggio 1: Registrare l'applicazione
>
> 1. Accedere al <a href="https://portal.azure.com/" target="_blank">portale di Azure<span class="docon docon-navigate-external x-hidden-focus"></span></a>.
> 1. Se si accede a più tenant, usare il filtro **Directory e sottoscrizione** :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: nel menu in alto e selezionare il tenant in cui si vuole registrare un'applicazione.
> 1. Cercare e selezionare **Azure Active Directory**.
> 1. In **Gestisci** selezionare **Registrazioni app** > **Nuova registrazione**.
> 1. Nella pagina **Registra un'applicazione** visualizzata immettere il nome dell'applicazione.
> 1. In **Tipi di account supportati** selezionare **Account in qualsiasi directory organizzativa e account Microsoft personali**.
> 1. Selezionare **Registra**. Nella pagina **Panoramica** dell'app prendere nota del valore del campo **ID applicazione (client)** per uso successivo.
> 1. In **Gestisci** selezionare **Autenticazione**.
> 1. In **Configurazioni della piattaforma** selezionare **Aggiungi una piattaforma**. Nel riquadro visualizzato selezionare **Applicazione a pagina singola**.
> 1. Impostare il valore **URI di reindirizzamento** su `http://localhost:3000/` . Questa è la porta predefinita NodeJS che resterà in ascolto sul computer locale. Verrà restituita la risposta di autenticazione a questo URI dopo che l'utente è stato autenticato correttamente. 
> 1. Selezionare **Configura** per applicare le modifiche.
> 1. In **configurazioni piattaforma** espandere **applicazione a pagina singola**.
> 1. Verificare che in **Concedi tipi** ![ già configurati ](media/quickstart-v2-javascript/green-check.png) l'URI di reindirizzamento sia idoneo per il flusso del codice di autorizzazione con PKCE.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Passaggio 1: Configurare l'applicazione nel portale di Azure
> Per il funzionamento dell'esempio di codice in questa Guida introduttiva, aggiungere un **URI di reindirizzamento** `http://localhost:3000/` .
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Apporta queste modifiche per me]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Già configurata](media/quickstart-v2-javascript/green-check.png) L'applicazione è configurata con questi attributi.

#### <a name="step-2-download-the-project"></a>Passaggio 2: Scaricare il progetto

> [!div renderon="docs"]
> Per eseguire il progetto con un server Web, ad esempio Node.js, [scaricare i file di progetto di base](https://github.com/Azure-Samples/ms-identity-javascript-react-spa/archive/main.zip).

> [!div renderon="portal" class="sxs-lookup"]
> Eseguire il progetto con un server Web usando Node.js

> [!div renderon="portal" class="sxs-lookup" id="autoupdate" class="nextstepaction"]
> [Scaricare il codice di esempio](https://github.com/Azure-Samples/ms-identity-javascript-react-spa/archive/main.zip)

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-javascript-app"></a>Passaggio 3: Configurare l'app JavaScript
>
> Nella cartella *src* aprire il file di *authConfig.js* e aggiornare i `clientID` valori, `authority` e `redirectUri` nell' `msalConfig` oggetto.
>
> ```javascript
> /**
> * Configuration object to be passed to MSAL instance on creation. 
> * For a full list of MSAL.js configuration parameters, visit:
> * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-browser/docs/configuration.md 
> */
> export const msalConfig = {
>    auth: {
>        clientId: "Enter_the_Application_Id_Here",
>        authority: "Enter_the_Cloud_Instance_Id_HereEnter_the_Tenant_Info_Here",
>        redirectUri: "Enter_the_Redirect_Uri_Here"
>    },
>    cache: {
>        cacheLocation: "sessionStorage", // This configures where your cache will be stored
>        storeAuthStateInCookie: false, // Set this to "true" if you are having issues on IE11 or Edge
>    },
> ```

> [!div renderon="portal" class="sxs-lookup"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
>
> Modificare i valori nella sezione `msalConfig` come descritto di seguito:
>
> - `Enter_the_Application_Id_Here` è l'**ID applicazione (client)** per l'applicazione registrata.
> - `Enter_the_Cloud_Instance_Id_Here` è l'istanza del cloud di Azure. Per il cloud di Azure principale o globale immettere `https://login.microsoftonline.com/`. Per i cloud **nazionali** (ad esempio, Cina), vedere [Cloud nazionali](authentication-national-cloud.md).
> - `Enter_the_Tenant_info_here` è impostato su uno degli elementi seguenti:
>   - Se l'applicazione supporta *account in questa directory organizzativa*, sostituire questo valore con l'**ID tenant** o il **nome del tenant**. Ad esempio: `contoso.microsoft.com`.
>   - Se l'applicazione supporta *account in qualsiasi directory organizzativa*, sostituire questo valore con `organizations`.
>   - Se l'applicazione supporta *account in qualsiasi directory organizzativa e account Microsoft personali*, sostituire questo valore con `common`. **Per questa guida introduttiva**, usare `common`.
>   - Per limitare il supporto ai *soli account Microsoft personali*, sostituire questo valore con `consumers`.
> - `Enter_the_Redirect_Uri_Here` è `http://localhost:3000/`.
>
> Se si usa il cloud principale (globale) di Azure, il valore `authority` nel file *authConfig.js* deve essere simile al seguente:
>
> ```javascript
> authority: "https://login.microsoftonline.com/common",
> ```
>
> > [!TIP]
> > Per trovare i valori di **ID applicazione (client)** , **ID della directory (tenant)** e **Tipi di account supportati**, passare alla pagina di **panoramica** di registrazione dell'app nel portale di Azure.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Passaggio 3: L'app è configurata e pronta per l'esecuzione
> Il progetto è stato configurato con i valori delle proprietà dell'app.

> [!div renderon="docs"]
>
> Scorrere verso il basso nello stesso file e aggiornare `graphMeEndpoint` . 
> - Sostituire la stringa `Enter_the_Graph_Endpoint_Herev1.0/me` con `https://graph.microsoft.com/v1.0/me`
> - `Enter_the_Graph_Endpoint_Herev1.0/me` è l'endpoint a cui verranno eseguite le chiamate API. Per il servizio API Microsoft Graph principale (globale), immettere `https://graph.microsoft.com/` (includere la barra finale). Per altre informazioni, vedere la [documentazione](https://docs.microsoft.com/graph/deployments).
>
>
>
> ```javascript
>   // Add here the endpoints for MS Graph API services you would like to use.
>    export const graphConfig = {
>        graphMeEndpoint: "Enter_the_Graph_Endpoint_Herev1.0/me"
>    };
> ```
>
>
#### <a name="step-4-run-the-project"></a>Passaggio 4: Eseguire il progetto

Eseguire il progetto con un server Web usandoNode.js:

1. Per avviare il server, eseguire i comandi seguenti nella directory del progetto:
    ```console
    npm install
    npm start
    ```
1. Passare a `http://localhost:3000/`.

1. Fare clic su **Accedi** per avviare il processo di accesso e quindi chiamare l'API Microsoft Graph.

    Al primo accesso viene chiesto di concedere il proprio consenso per consentire all'applicazione di accedere al profilo e completare la procedura di accesso. Dopo aver eseguito l'accesso, fare clic su **informazioni sul profilo della richiesta** per visualizzare le informazioni sul profilo nella pagina.

## <a name="more-information"></a>Ulteriori informazioni

### <a name="how-the-sample-works"></a>Funzionamento dell'esempio

![Diagramma che mostra il flusso del codice di autorizzazione per un'applicazione a pagina singola.](media/quickstart-v2-javascript-auth-code/diagram-01-auth-code-flow.png)

### <a name="msaljs"></a>msal.js

La libreria MSAL.js firma gli utenti e richiede i token usati per accedere a un'API protetta dalla piattaforma di identità Microsoft.

Se Node.js è già installato, è possibile scaricare l'ultima versione usando Node.js Package Manager (npm):

```console
npm install @azure/msal-browser @azure/msal-react
```

## <a name="next-steps"></a>Passaggi successivi

Per una guida dettagliata sulla creazione dell'applicazione flusso del codice di autenticazione con la vaniglia JavaScript, vedere l'esercitazione seguente:

> [!div class="nextstepaction"]
> [Esercitazione per l'accesso e la chiamata a MS Graph](./tutorial-v2-javascript-auth-code.md)
