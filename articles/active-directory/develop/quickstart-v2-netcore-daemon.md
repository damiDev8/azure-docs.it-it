---
title: "Avvio rapido: Ottenere un token e chiamare Microsoft Graph in un'app console | Azure"
titleSuffix: Microsoft identity platform
description: Questa guida di avvio rapido illustra come un'app di esempio .NET Core può usare il flusso di credenziali client per ottenere un token e chiamare Microsoft Graph.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/05/2020
ms.author: jmprieur
ms.reviewer: marsma
ms.custom: devx-track-csharp, aaddev, identityplatformtop40, scenarios:getting-started, languages:aspnet-core
ms.openlocfilehash: 305b76f0dea0f39d9a981298903dfaa83f09b9a7
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2021
ms.locfileid: "99583296"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-using-console-apps-identity"></a>Avvio rapido: Acquisire un token e chiamare l'API Microsoft Graph usando l'identità dell'app console

In questa guida di avvio rapido si scarica e si esegue un esempio di codice che illustra come un'applicazione console .NET Core può ottenere un token di accesso per chiamare l'API Microsoft Graph e visualizzare un [elenco di utenti](/graph/api/user-list) nella directory. L'esempio di codice dimostra anche come è possibile eseguire un processo o un servizio di Windows con un'identità dell'applicazione invece che con un'identità dell'utente. 

Per un'illustrazione, vedere [Funzionamento dell'esempio](#how-the-sample-works).

## <a name="prerequisites"></a>Prerequisiti

Per questa guida di avvio rapido è necessario [.NET Core 3.1](https://www.microsoft.com/net/download/dotnet-core).

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Registrare e scaricare l'app della guida introduttiva

> [!div renderon="docs" class="sxs-lookup"]
>
> Per avviare l'applicazione della guida introduttiva sono disponibili due opzioni: Rapida (Opzione 1 di seguito) e Manuale (Opzione 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Opzione 1: Registrare e configurare automaticamente l'app e quindi scaricare l'esempio di codice
>
> 1. Passare all'esperienza di avvio rapido <a href="https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/DotNetCoreDaemonQuickstartPage/sourceType/docs" target="_blank">Portale di Azure - Registrazioni app<span class="docon docon-navigate-external x-hidden-focus"></span></a>.
> 1. Immettere un nome per l'applicazione e fare clic su **Registra**.
> 1. Seguire le istruzioni per scaricare e configurare automaticamente la nuova applicazione con un clic.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Opzione 2: Registrare e configurare manualmente l'applicazione e il codice di esempio

> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>Passaggio 1: Registrare l'applicazione
> Per registrare l'applicazione e aggiungere manualmente le informazioni di registrazione dell'app alla soluzione, seguire questa procedura:
>
> 1. Accedere al <a href="https://portal.azure.com/" target="_blank">portale di Azure<span class="docon docon-navigate-external x-hidden-focus"></span></a>.
> 1. Se si accede a più tenant, usare il filtro **Directory e sottoscrizione** :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: nel menu in alto e selezionare il tenant in cui si vuole registrare un'applicazione.
> 1. Cercare e selezionare **Azure Active Directory**.
> 1. In **Gestisci** selezionare **Registrazioni app** > **Nuova registrazione**.
> 1. Immettere un **Nome** per l'applicazione, ad esempio `Daemon-console`. Tale nome, che potrebbe essere visualizzato dagli utenti dell'app, può essere modificato in un secondo momento.
> 1. Selezionare **Registra** per creare l'applicazione.
> 1. In **Gestisci**, selezionare **Certificati e segreti**.
> 1. In **Segreti client** selezionare **Nuovo segreto client**, immettere un nome e quindi selezionare **Aggiungi**. Registrare il valore del segreto in una posizione sicura per usarlo in un passaggio successivo.
> 1. In **Gestisci** selezionare **Autorizzazioni API** > **Aggiungi un'autorizzazione**. Selezionare **Microsoft Graph**.
> 1. Selezionare **Autorizzazioni applicazione**.
> 1. Nel nodo **Utente** selezionare **User.Read.All**, quindi selezionare **Aggiungi autorizzazioni**.

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="download-and-configure-your-quickstart-app"></a>Scaricare e configurare l'app della guida introduttiva
>
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>Passaggio 1: Configurare l'applicazione nel portale di Azure
> Per il funzionamento dell'esempio di codice in questa Guida introduttiva, creare un segreto client e aggiungere l'autorizzazione **utente. Read. All** dell'applicazione API Graph.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Apporta queste modifiche per me]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Già configurata](media/quickstart-v2-netcore-daemon/green-check.png) L'applicazione è configurata con questi attributi.

#### <a name="step-2-download-your-visual-studio-project"></a>Passaggio 2: Scaricare il progetto di Visual Studio

> [!div renderon="docs"]
> [Scaricare il progetto di Visual Studio](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/archive/master.zip)
>
> È possibile eseguire il progetto fornito in Visual Studio o in Visual Studio per Mac.


> [!div class="sxs-lookup" renderon="portal"]
> Eseguire il progetto con Visual Studio 2019.
> [!div class="sxs-lookup" renderon="portal" id="autoupdate" class="nextstepaction"]
> [Scaricare il codice di esempio](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-visual-studio-project"></a>Passaggio 3: Configurare il progetto di Visual Studio
>
> 1. Estrarre il file ZIP in una cartella locale vicina alla radice del disco, ad esempio **C:\Azure-Samples**.
> 1. Aprire la soluzione **1-Call-MSGraph\daemon-console.sln** in Visual Studio (facoltativo).
> 1. Modificare **appsettings.json** e sostituire i valori dei campi `ClientId`, `Tenant` e `ClientSecret` con il codice seguente:
>
>    ```json
>    "Tenant": "Enter_the_Tenant_Id_Here",
>    "ClientId": "Enter_the_Application_Id_Here",
>    "ClientSecret": "Enter_the_Client_Secret_Here"
>    ```
>   Dove:
>   - `Enter_the_Application_Id_Here` è l'**ID applicazione (client)** per l'applicazione registrata.
>   - `Enter_the_Tenant_Id_Here`: sostituire questo valore con l'**ID tenant** o il **nome del tenant** (ad esempio, contoso.microsoft.com)
>   - `Enter_the_Client_Secret_Here`: sostituire questo valore con il segreto client creato nel passaggio 1.

> [!div renderon="docs"]
> > [!TIP]
> > Per trovare i valori di **ID applicazione (client)** e **ID della directory (tenant)** , passare alla pagina **Panoramica** dell'app nel portale di Azure. Per generare una nuova chiave, passare alla pagina **Certificati e segreti**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-admin-consent"></a>Passaggio 3: Consenso dell'amministratore

> [!div renderon="docs"]
> #### <a name="step-4-admin-consent"></a>Passaggio 4: Consenso dell'amministratore

Se si prova a eseguire l'applicazione a questo punto, si riceverà l'errore *HTTP 403 - Accesso negato*: `Insufficient privileges to complete the operation`. Questo errore si verifica perché le *autorizzazioni solo per app* richiedono il consenso amministratore. È quindi necessario che un amministratore globale della directory conceda il consenso all'applicazione. Selezionare una delle opzioni seguenti in base al ruolo:

##### <a name="global-tenant-administrator"></a>Amministratore del tenant globale

> [!div renderon="docs"]
> Se si è un amministratore tenant globale, nel portale di Azure passare a **Applicazioni aziendali**, selezionare la registrazione dell'app, quindi scegliere **"Autorizzazioni"** nella sezione Sicurezza del riquadro di spostamento sinistro. Selezionare il grande pulsante **Concedi consenso amministratore per {nome tenant}** , dove {nome tenant} è il nome della directory.

> [!div renderon="portal" class="sxs-lookup"]
> Gli amministratori globali devono passare alla pagina **Autorizzazioni API** e selezionare **Concedi consenso amministratore per Immettere_il_nome_tenant_qui**
> > [!div id="apipermissionspage"]
> > [Passare alla pagina Autorizzazioni API]()

##### <a name="standard-user"></a>Utente standard

Se si è un utente standard del tenant, rivolgersi a un amministratore globale per concedere il consenso dell'amministratore per l'applicazione. A tale scopo, assegnare l'URL seguente all'amministratore:

```url
https://login.microsoftonline.com/Enter_the_Tenant_Id_Here/adminconsent?client_id=Enter_the_Application_Id_Here
```

> [!div renderon="docs"]
>> Dove:
>> * `Enter_the_Tenant_Id_Here`: sostituire questo valore con l'**ID tenant** o il **nome del tenant** (ad esempio, contoso.microsoft.com)
>> * `Enter_the_Application_Id_Here` è l'**ID applicazione (client)** per l'applicazione registrata.

> [!NOTE]
> Dopo aver concesso il consenso all'app usando l'URL precedente, potrebbe essere visualizzato l'errore *'AADSTS50011: No reply address is registered for the application'* (AADSTS50011: Nessun indirizzo di risposta registrato per l'applicazione). Ciò accade perché questa applicazione e l'URL non hanno un URI di reindirizzamento. Ignorare l'errore.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-4-run-the-application"></a>Passaggio 4: Eseguire l'applicazione

> [!div renderon="docs"]
> #### <a name="step-5-run-the-application"></a>Passaggio 5: Eseguire l'applicazione

Se si usa Visual Studio o Visual Studio per Mac, premere **F5** per eseguire l'applicazione; in caso contrario, eseguire l'applicazione tramite prompt dei comandi, console o terminale:

```dotnetcli
cd {ProjectFolder}\1-Call-MSGraph\daemon-console
dotnet run
```

> Dove:
> * *{ProjectFolder}* è la cartella in cui è stato estratto il file ZIP, ad esempio **C:\Azure-Samples\active-directory-dotnetcore-daemon-v2**

Come risultato si dovrebbe vedere un elenco di utenti nel tenant di Azure AD.

> [!IMPORTANT]
> Questa applicazione della guida introduttiva usa un segreto client per identificarsi come client riservato. Poiché il segreto client viene aggiunto come testo normale ai file di progetto, per motivi di sicurezza è consigliabile usare un certificato anziché un segreto client prima di considerare l'applicazione come applicazione di produzione. Per altre informazioni su come usare un certificato, vedere [queste istruzioni](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/#variation-daemon-application-using-client-credentials-with-certificates) nel repository GitHub per questo esempio.

## <a name="more-information"></a>Ulteriori informazioni

### <a name="how-the-sample-works"></a>Funzionamento dell'esempio
![Mostra come funziona l'app di esempio generata da questo avvio rapido](media/quickstart-v2-netcore-daemon/netcore-daemon-intro.svg)

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)) è la libreria usata per concedere l'accesso agli utenti e richiedere i token usati per accedere a un'API protetta da Microsoft Identity Platform. Come descritto, questo avvio rapido richiede i token usando l'identità propria dell'applicazione invece delle autorizzazioni delegate. Il flusso di autenticazione usato in questo caso è noto come *[flusso delle credenziali client OAuth](v2-oauth2-client-creds-grant-flow.md)* . Per altre informazioni su come usare MSAL.NET con il flusso delle credenziali client, vedere [questo articolo](https://aka.ms/msal-net-client-credentials).

 È possibile installare MSAL.NET eseguendo questo comando in **Console di Gestione pacchetti** in Visual Studio:

```dotnetcli
dotnet add package Microsoft.Identity.Client
```

### <a name="msal-initialization"></a>Inizializzazione della libreria MSAL

È possibile aggiungere il riferimento per la libreria MSAL aggiungendo il codice seguente:

```csharp
using Microsoft.Identity.Client;
```

Inizializzare quindi la libreria MSAL usando il codice seguente:

```csharp
IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
                                          .WithClientSecret(config.ClientSecret)
                                          .WithAuthority(new Uri(config.Authority))
                                          .Build();
```

> | Dove: | Descrizione |
> |---------|---------|
> | `config.ClientSecret` | È il segreto client creato per l'applicazione nel portale di Azure. |
> | `config.ClientId` | **ID applicazione (client)** dell'applicazione registrata nel portale di Azure. Questo valore è riportato nella pagina **Panoramica** dell'app nel portale di Azure. |
> | `config.Authority`    | (Facoltativo) Endpoint del servizio token di sicurezza per l'utente da autenticare, in genere `https://login.microsoftonline.com/{tenant}` per il cloud pubblico, dove {tenant} è il nome del tenant o l'ID tenant.|

Per altre informazioni, vedere la [documentazione di riferimento`ConfidentialClientApplication`](/dotnet/api/microsoft.identity.client.iconfidentialclientapplication)

### <a name="requesting-tokens"></a>Richiesta di token

Per richiedere un token con l'identità dell'app, usare il metodo `AcquireTokenForClient`:

```csharp
result = await app.AcquireTokenForClient(scopes)
                  .ExecuteAsync();
```

> |Dove:| Descrizione |
> |---------|---------|
> | `scopes` | Contiene gli ambiti richiesti. Per i client riservati, dovrebbe essere usato un formato simile a `{Application ID URI}/.default` per indicare che gli ambiti che vengono richiesti sono quelli definiti in modo statico nell'oggetto app impostato nel portale di Azure. Per Microsoft Graph, `{Application ID URI}` punta a `https://graph.microsoft.com`. Per le API Web personalizzate, `{Application ID URI}` è definito nella sezione **Esporre un'API** della registrazione dell'applicazione (anteprima) del portale di Azure. |

Per altre informazioni, vedere la [documentazione di riferimento`AcquireTokenForClient`](/dotnet/api/microsoft.identity.client.confidentialclientapplication.acquiretokenforclient)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle applicazioni daemon, vedere la panoramica dello scenario:

> [!div class="nextstepaction"]
> [Applicazione daemon che chiama le API Web](scenario-daemon-overview.md)
