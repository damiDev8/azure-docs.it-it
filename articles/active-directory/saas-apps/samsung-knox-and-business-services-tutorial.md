---
title: 'Esercitazione: integrazione di Azure Active Directory Single Sign-On (SSO) con Samsung Knox e servizi aziendali | Microsoft Docs'
description: Informazioni su come configurare Single Sign-On tra Azure Active Directory e Samsung Knox e i servizi aziendali.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/27/2021
ms.author: jeedes
ms.openlocfilehash: e1cf12d676de84bc18a123fbdf05b1170725eda8
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99072885"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-samsung-knox-and-business-services"></a>Esercitazione: integrazione di Azure Active Directory Single Sign-On (SSO) con Samsung Knox e servizi aziendali

In questa esercitazione si apprenderà come integrare Samsung Knox e i servizi aziendali con Azure Active Directory (Azure AD). Quando si integrano Samsung Knox e i servizi aziendali con Azure AD, è possibile:

* Controllare in Azure AD chi ha accesso a Samsung Knox e ai servizi aziendali.
* Consentire agli utenti di accedere automaticamente a Samsung Knox e ai servizi aziendali con gli account Azure AD.
* Gestire gli account in un'unica posizione centrale: il portale di Azure.

## <a name="prerequisites"></a>Prerequisiti

Per iniziare, sono necessari gli elementi seguenti:

* Una sottoscrizione di Azure AD. Se non si ha una sottoscrizione, è possibile ottenere un [account gratuito](https://azure.microsoft.com/free/).
* Sottoscrizione di Samsung Knox e Business Services Single Sign-On (SSO) abilitata.

## <a name="scenario-description"></a>Descrizione dello scenario

In questa esercitazione vengono eseguiti la configurazione e il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.

* Samsung Knox e i servizi aziendali supportano SSO avviato da **SP**

> [!NOTE]
> Dal momento che l'identificatore di questa applicazione è un valore stringa fisso, è possibile configurare una sola istanza in un solo tenant.

## <a name="adding-samsung-knox-and-business-services-from-the-gallery"></a>Aggiunta di Samsung Knox e Business Services dalla raccolta

Per configurare l'integrazione di Samsung Knox e dei servizi aziendali in Azure AD, è necessario aggiungere i servizi Samsung Knox e business dalla raccolta al proprio elenco di app SaaS gestite.

1. Accedere al portale di Azure con un account aziendale o dell'istituto di istruzione oppure con un account Microsoft personale.
1. Nel riquadro di spostamento a sinistra selezionare il servizio **Azure Active Directory**.
1. Passare ad **Applicazioni aziendali** e quindi selezionare **Tutte le applicazioni**.
1. Per aggiungere una nuova applicazione, selezionare **Nuova applicazione**.
1. Nella sezione **Aggiungi dalla raccolta** digitare **Samsung Knox e Business Services** nella casella di ricerca.
1. Selezionare **Samsung Knox e Business Services** nel pannello dei risultati e quindi aggiungere l'app. Attendere alcuni secondi che l'app venga aggiunta al tenant.

## <a name="configure-and-test-azure-ad-sso-for-samsung-knox-and-business-services"></a>Configurare e testare Azure AD SSO per Samsung Knox e i servizi aziendali

Configurare e testare Azure AD SSO con Samsung Knox e i servizi aziendali usando un utente di test di nome **B. Simon**. Per il funzionamento dell'accesso SSO, è necessario stabilire una relazione di collegamento tra un utente Azure AD e l'utente correlato in Samsung Knox e nei servizi aziendali.

Per configurare e testare Azure AD SSO con Samsung Knox e i servizi aziendali, seguire questa procedura:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-sso)** : per consentire agli utenti di usare questa funzionalità.
    1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente B.Simon.
    1. **[Assegnare l'utente di test di Azure AD](#assign-the-azure-ad-test-user)** : per abilitare B.Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Configurare Samsung Knox e Business Services SSO](#configure-samsung-knox-and-business-services-sso)** per configurare le impostazioni di Single Sign-on sul lato applicazione.
    1. **[Creare un utente di test di Samsung Knox e Business Services](#create-samsung-knox-and-business-services-test-user)** : per avere una controparte di B. Simon in Samsung Knox e Business Services collegata alla rappresentazione Azure ad dell'utente.
1. **[Testare l'accesso Single Sign-On](#test-sso)** : per verificare se la configurazione funziona.

## <a name="configure-azure-ad-sso"></a>Configurare l'accesso SSO di Azure AD

Per abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure, seguire questa procedura.

1. Nella pagina di integrazione dell'applicazione **Samsung Knox e Business Services** del portale di Azure individuare la sezione **Gestisci** e selezionare **Single Sign-on**.
1. Nella pagina **Selezionare un metodo di accesso Single Sign-On** selezionare **SAML**.
1. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sull'icona della matita per modificare le impostazioni di **Configurazione SAML di base**.

   ![Modificare la configurazione SAML di base](common/edit-urls.png)

1. Nella sezione **Configurazione SAML di base** immettere i valori per i campi seguenti:

    Nella casella di testo **URL di accesso** digitare l'URL: `https://www.samsungknox.com`

1. Nella sezione **Certificato di firma SAML** della pagina **Configura l'accesso Single Sign-On con SAML** fare clic sul pulsante Copia per copiare l'**URL dei metadati di federazione dell'app** e salvarlo nel computer.

    ![Collegamento di download del certificato](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Creare un utente di test di Azure AD

In questa sezione verrà creato un utente di test di nome B.Simon nel portale di Azure.

1. Nel riquadro sinistro del portale di Azure selezionare **Azure Active Directory**, **Utenti** e quindi **Tutti gli utenti**.
1. Selezionare **Nuovo utente** in alto nella schermata.
1. In **Proprietà utente** seguire questa procedura:
   1. Nel campo **Nome** immettere `B.Simon`.  
   1. Nel campo **Nome utente** immettere username@companydomain.extension. Ad esempio: `B.Simon@contoso.com`.
   1. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.
   1. Fare clic su **Crea**.

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente di test di Azure AD

In questa sezione si consentirà a B. Simon di usare Azure Single Sign-On concedendo l'accesso a Samsung Knox e ai servizi aziendali.

1. Nel portale di Azure selezionare **Applicazioni aziendali** e quindi **Tutte le applicazioni**.
1. Nell'elenco delle applicazioni selezionare **Samsung Knox e Business Services**.
1. Nella pagina di panoramica dell'app trovare la sezione **Gestione** e selezionare **Utenti e gruppi**.
1. Selezionare **Aggiungi utente** e quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.
1. Nella finestra di dialogo **Utenti e gruppi** selezionare **B.Simon** dall'elenco degli utenti e quindi fare clic sul pulsante **Seleziona** nella parte inferiore della schermata.
1. Se si prevede che agli utenti venga assegnato un ruolo, è possibile selezionarlo nell'elenco a discesa **Selezionare un ruolo**. Se per questa app non è stato configurato alcun ruolo, il ruolo selezionato è "Accesso predefinito".
1. Nella finestra di dialogo **Aggiungi assegnazione** fare clic sul pulsante **Assegna**.

## <a name="configure-samsung-knox-and-business-services-sso"></a>Configurare Samsung Knox e Business Services SSO

1. In un'altra finestra del Web browser accedere al sito aziendale di Samsung Knox e Business Services come amministratore.

1. Fare clic sull' **avatar** nell'angolo superiore destro.

    ![Avatar Samsung Knox](./media/samsung-knox-and-business-services-tutorial/avatar.png)

1. Nella barra laterale sinistra fare clic su **impostazioni di Active Directory** e seguire questa procedura.

    ![IMPOSTAZIONI DI ACTIVE DIRECTORY](./media/samsung-knox-and-business-services-tutorial/sso-settings.png)

    a. Nella casella di testo **identificatore (ID entità)** incollare il valore dell' **identificatore** immesso nel portale di Azure.

    b. Nella casella di testo **URL di metadati di Federazione** , incollare il valore dell' **URL dei metadati di Federazione dell'app** copiato dal portale di Azure.

    c. fare clic su **Connetti ad SSO ad**.

### <a name="create-samsung-knox-and-business-services-test-user"></a>Creare un utente di test di Samsung Knox e Business Services

In questa sezione viene creato un utente di nome Britta Simon in Samsung Knox e Business Services. Collaborare con il [team di supporto di Samsung Knox e dei servizi aziendali](mailto:noreplyk.sec@samsung.com) per aggiungere gli utenti nella piattaforma Samsung Knox e Business Services. Gli utenti devono essere creati e attivati prima di usare l'accesso Single Sign-On.

## <a name="test-sso"></a>Testare l'accesso SSO 

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD con le opzioni seguenti. 

* Fare clic su **Test this application** (Testa questa applicazione) nel portale di Azure. Verrà eseguito il reindirizzamento a Samsung Knox e all'URL di accesso ai servizi aziendali in cui è possibile avviare il flusso di accesso. 

* Passare direttamente all'URL di accesso a Samsung Knox e ai servizi aziendali e avviare il flusso di accesso da qui.

* È possibile usare App personali Microsoft. Quando si fa clic sul riquadro Samsung Knox e Business Services in app personali, questo verrà reindirizzato all'URL di accesso a Samsung Knox e ai servizi aziendali. Per altre informazioni su App personali, vedere l'[introduzione ad App personali](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).


## <a name="next-steps"></a>Passaggi successivi

Una volta configurati Samsung Knox e i servizi aziendali, è possibile applicare il controllo della sessione, che protegge exfiltration e infiltrando i dati sensibili dell'organizzazione in tempo reale. Il controllo sessione costituisce un'estensione dell'accesso condizionale. [Informazioni su come applicare il controllo sessione con Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


