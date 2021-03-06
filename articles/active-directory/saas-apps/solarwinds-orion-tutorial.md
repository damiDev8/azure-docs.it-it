---
title: "Esercitazione: Integrazione dell'accesso Single Sign-On (SSO) di Azure Active Directory con SolarWinds Orion | Microsoft Docs"
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e SolarWinds Orion.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/24/2020
ms.author: jeedes
ms.openlocfilehash: dbccf38bcb89a6e0715604567be021cc890f209b
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/24/2020
ms.locfileid: "92514810"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-solarwinds-orion"></a>Esercitazione: Integrazione dell'accesso Single Sign-On (SSO) di Azure Active Directory con SolarWinds Orion

Questa esercitazione descrive come integrare SolarWinds Orion con Azure Active Directory (Azure AD). Integrando SolarWinds Orion con Azure AD, è possibile:

* Controllare in Azure AD chi può accedere a SolarWinds Orion.
* Abilitare gli utenti per l'accesso automatico a SolarWinds Orion con gli account Azure AD personali.
* Gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Accesso Single Sign-On alle applicazioni in Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per iniziare, sono necessari gli elementi seguenti:

* Una sottoscrizione di Azure AD. Se non si ha una sottoscrizione, è possibile ottenere un [account gratuito](https://azure.microsoft.com/free/).
* Sottoscrizione di SolarWinds Orion abilitata per l'accesso Single Sign-On (SSO).

## <a name="scenario-description"></a>Descrizione dello scenario

In questa esercitazione vengono eseguiti la configurazione e il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.

* SolarWinds Orion supporta l'accesso SSO avviato da **SP e IDP**
* Dopo aver configurato SolarWinds Orion, è possibile applicare il controllo sessione che consente di proteggere in tempo reale l'esfiltrazione e l'infiltrazione dei dati sensibili dell'organizzazione. Il controllo sessione costituisce un'estensione dell'accesso condizionale. [Informazioni su come applicare il controllo sessione con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-solarwinds-orion-from-the-gallery"></a>Aggiunta di SolarWinds Orion dalla raccolta

Per configurare l'integrazione di SolarWinds Orion in Azure AD, è necessario aggiungere SolarWinds Orion dalla raccolta all'elenco di app SaaS gestite.

1. Accedere al [portale di Azure](https://portal.azure.com) con un account aziendale o dell'istituto di istruzione oppure con un account Microsoft personale.
1. Nel riquadro di spostamento a sinistra selezionare il servizio **Azure Active Directory** .
1. Passare ad **Applicazioni aziendali** e quindi selezionare **Tutte le applicazioni** .
1. Per aggiungere una nuova applicazione, selezionare **Nuova applicazione** .
1. Nella sezione **Aggiungi dalla raccolta** digitare **SolarWinds Orion** nella casella di ricerca.
1. Selezionare **SolarWinds Orion** nel pannello dei risultati e quindi aggiungere l'app. Attendere alcuni secondi che l'app venga aggiunta al tenant.


## <a name="configure-and-test-azure-ad-sso-for-solarwinds-orion"></a>Configurare e testare l'accesso SSO di Azure AD per SolarWinds Orion

Configurare e testare l'accesso SSO di Azure AD con SolarWinds Orion usando un utente di test di nome **B.Simon** . Per consentire il funzionamento dell'accesso Single Sign-On, è necessario stabilire una relazione di collegamento tra un utente di Azure AD e l'utente correlato in SolarWinds Orion.

Per configurare e testare l'accesso SSO di Azure AD con SolarWinds Orion, completare le procedure di base seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-sso)** : per consentire agli utenti di usare questa funzionalità.
    1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente B. Simon.
    1. **[Assegnare l'utente di test di Azure AD](#assign-the-azure-ad-test-user)** : per abilitare B. Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Configurare l'accesso Single Sign-On di SolarWinds Orion](#configure-solarwinds-orion-sso)** : per configurare le impostazioni di Single Sign-On sul lato applicazione.
    1. **[Creare l'utente di test di SolarWinds Orion](#create-solarwinds-orion-test-user)** : per avere una controparte di B.Simon in SolarWinds Orion collegata alla rappresentazione dell'utente in Azure AD.
1. **[Testare l'accesso Single Sign-On](#test-sso)** : per verificare se la configurazione funziona.

## <a name="configure-azure-ad-sso"></a>Configurare l'accesso SSO di Azure AD

Per abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure, seguire questa procedura.

1. Nella pagina di integrazione dell'applicazione **SolarWinds Orion** del [portale di Azure](https://portal.azure.com/) individuare la sezione **Gestione** e selezionare **Single Sign-On** .
1. Nella pagina **Selezionare un metodo di accesso Single Sign-On** selezionare **SAML** .
1. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sull'icona Modifica (la penna) relativa a **Configurazione SAML di base** per modificare le impostazioni.

   ![Modificare la configurazione SAML di base](common/edit-urls.png)

1. Nella sezione **Configurazione SAML di base** immettere i valori per i campi seguenti se si vuole configurare l'applicazione in modalità avviata da **IDP** :

    a. Nella casella di testo **Identificatore** digitare un URL nel formato seguente: `https://<ORION-HOSTNAME-OR-EXTERNAL-URL>`

    b. Nella casella di testo **URL di risposta** digitare un URL nel formato seguente: `https://<ORION-HOSTNAME-OR-EXTERNAL-URL>/Orion/SAMLLogin.aspx`

1. Fare clic su **Impostare URL aggiuntivi** e seguire questa procedura se si vuole configurare l'applicazione in modalità avviata da **SP** :

    Nella casella di testo **URL accesso** digitare un URL nel formato seguente: `https://<ORION-HOSTNAME-OR-EXTERNAL-URL>/Orion/Login.aspx`

    > [!NOTE]
    > Poiché questi non sono i valori reali, aggiornarli con l'identificatore, l'URL di risposta e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di SolarWinds Orion](mailto:technicalsupport@solarwinds.com). È anche possibile fare riferimento ai modelli mostrati nella sezione **Configurazione SAML di base** del portale di Azure.

1. L'applicazione SolarWinds Orion prevede un formato specifico per le asserzioni SAML. È quindi necessario aggiungere mapping di attributi personalizzati alla configurazione degli attributi del token SAML. Lo screenshot seguente mostra l'elenco degli attributi predefiniti.

    ![image](common/default-attributes.png)

1. Oltre quelli elencati in precedenza, l'applicazione SolarWinds Orion prevede il passaggio di altri attributi nella risposta SAML. Tali attributi sono indicati di seguito. Anche questi attributi vengono prepopolati, ma è possibile esaminarli in base ai requisiti.
    
    | Nome |  Attributo di origine|
    | ----------- | --------- |
    | FirstName | user.givenname |
    | LastName | user.surname |
    | Email |user.mail |

1. Nella sezione **Certificato di firma SAML** della pagina **Configura l'accesso Single Sign-On con SAML** individuare **Certificato (Base64)** e selezionare **Scarica** per scaricare il certificato e salvarlo nel computer.

    ![Collegamento di download del certificato](common/certificatebase64.png)

1. Nella sezione **Configura SolarWinds Orion** copiare gli URL appropriati in base alle esigenze.

    ![Copiare gli URL di configurazione](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creare un utente di test di Azure AD

In questa sezione verrà creato un utente di test di nome B.Simon nel portale di Azure.

1. Nel riquadro sinistro del portale di Azure selezionare **Azure Active Directory** , **Utenti** e quindi **Tutti gli utenti** .
1. Selezionare **Nuovo utente** in alto nella schermata.
1. In **Proprietà utente** seguire questa procedura:
   1. Nel campo **Nome** immettere `B.Simon`.  
   1. Nel campo **Nome utente** immettere username@companydomain.extension. Ad esempio: `B.Simon@contoso.com`.
   1. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password** .
   1. Fare clic su **Crea** .

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente di test di Azure AD

In questa sezione si abiliterà B.Simon all'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a SolarWinds Orion.

1. Nel portale di Azure selezionare **Applicazioni aziendali** e quindi **Tutte le applicazioni** .
1. Nell'elenco di applicazioni selezionare **SolarWinds Orion** .
1. Nella pagina di panoramica dell'app trovare la sezione **Gestione** e selezionare **Utenti e gruppi** .

   ![Collegamento "Utenti e gruppi"](common/users-groups-blade.png)

1. Selezionare **Aggiungi utente** e quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione** .

    ![Collegamento Aggiungi utente](common/add-assign-user.png)

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **B.Simon** dall'elenco degli utenti e quindi fare clic sul pulsante **Seleziona** nella parte inferiore della schermata.
1. Se si prevede un valore di ruolo nell'asserzione SAML, nella finestra di dialogo **Selezionare un ruolo** selezionare il ruolo appropriato per l'utente dall'elenco e quindi fare clic sul pulsante **Seleziona** nella parte inferiore della schermata.
1. Nella finestra di dialogo **Aggiungi assegnazione** fare clic sul pulsante **Assegna** .

## <a name="configure-solarwinds-orion-sso"></a>Configurare l'accesso Single Sign-On (SSO) di SolarWinds

1. Accedere a SolarWinds Orion e passare a **Settings** -> **All Settings** (Impostazioni -> Tutte le Impostazioni).

    ![Screenshot che mostra l'opzione All Settings selezionata in Settings.](./media/solarwinds-orion-tutorial/settings.png)

1. Nella sezione **USER ACCOUNTS** (Account utente) selezionare **SAML Configuration** (Configurazione di SAML).

    ![Screenshot che mostra l'opzione SAML Configuration selezionata in User Accounts.](./media/solarwinds-orion-tutorial/configure-user-accounts.png)

1. Fare clic su **ADD IDENTITY PROVIDER** (Aggiungi provider di identità).

    ![Screenshot che mostra la finestra SAML Configuration in cui è possibile selezionare ADD IDENTITY PROVIDER.](./media/solarwinds-orion-tutorial/configure-add-identity-provider.png)

1. Seguire questa procedura nella pagina **Add Identity Provider** (Aggiungi provider di identità):

    ![Screenshot che mostra la pagina Add Identity Provider in cui è possibile immettere i valori descritti.](./media/solarwinds-orion-tutorial/configure-solarwinds.png)

    a. Passare alla scheda **Configure** (Configura).

    b. Nella casella di testo **Identity Provider Name** (Nome del provider di identità) specificare un nome valido, ad esempio `My SSO service`.

    c. Nella casella di testo **SSO Target URL** (URL di destinazione SSO) incollare il valore di **URL di accesso** copiato dal portale di Azure.

    d.  Nella casella di testo **Issuer URL** (URL autorità di certificazione) incollare il valore di **Identificatore Azure AD** copiato dal portale di Azure.

    e. Aprire nel Blocco note il file del **certificato (Base64)** scaricato dal portale di Azure e incollarne il contenuto nella casella di testo **X.509 Signing Certificate** (Certificato di firma X.509).

    f. Fare clic su **Save** .

### <a name="create-solarwinds-orion-test-user"></a>Creare un utente di test di SolarWinds Orion

1. Accedere al sito Web di SolarWinds Orion e passare a **Settings** -> **All Settings** (Impostazioni -> Tutte le Impostazioni).

    ![Screenshot che mostra l'opzione All Settings selezionata in Settings.](./media/solarwinds-orion-tutorial/settings.png)

1. Nella sezione **USER ACCOUNTS** (Account utente) selezionare **Manage Accounts** (Gestisci account).

    ![Screenshot che mostra l'opzione SAML Configuration selezionata.](./media/solarwinds-orion-tutorial/user-accounts.png)

1. Nella scheda **INDIVIDUAL ACCOUNTS** (Account individuali) fare clic su **ADD NEW ACCOUNT** (Aggiungi nuovo account).

    ![Screenshot che mostra l'opzione ADD NEW ACCOUNT selezionata in Manage Accounts.](./media/solarwinds-orion-tutorial/create-user.png)

1. Selezionare il tipo di account. Questo passaggio è necessario per creare singoli utenti o gruppi di SAML.

    ![Screenshot che mostra la finestra Add New Account in cui è possibile selezionare il tipo di account.](./media/solarwinds-orion-tutorial/create-user-new-account.png)

1.  Nella casella di testo **NAME ID** (ID nome) immettere il nome, che deve corrispondere esattamente al nome utente o al nome del gruppo presente in Azure AD.

1.  Fare clic su **Next** (Avanti) e quindi inviare la pagina.

    ![Screenshot che mostra la pagina Add New Account in cui è possibile immettere l'ID del nome da Azure AD.](./media/solarwinds-orion-tutorial/create-user-name-id.png)

## <a name="test-sso"></a>Testare l'accesso SSO 

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro di SolarWinds Orion nel pannello di accesso, si dovrebbe accedere automaticamente all'istanza di SolarWinds Orion per cui si è configurato l'accesso SSO. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Risorse aggiuntive

- [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](./tutorial-list.md)

- [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

- [Che cos'è l'accesso condizionale in Azure Active Directory?](../conditional-access/overview.md)

- [Provare SolarWinds Orion con Azure AD](https://aad.portal.azure.com/)

- [Informazioni sul controllo sessioni in Microsoft Cloud App Security](/cloud-app-security/proxy-intro-aad)

- [Come proteggere SolarWinds Orion con visibilità e controlli avanzati](/cloud-app-security/proxy-intro-aad)