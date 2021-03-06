---
title: 'Esercitazione: Integrazione di Azure Active Directory con xMatters OnDemand | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e xMatters OnDemand.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/19/2020
ms.author: jeedes
ms.openlocfilehash: 762bd1c536df0ca307149ba7c201f08f5bdfded5
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2021
ms.locfileid: "98735612"
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Esercitazione: Integrazione di Azure Active Directory con xMatters OnDemand

Questa esercitazione descrive come integrare xMatters OnDemand con Azure Active Directory (Azure AD).
L'integrazione di xMatters OnDemand con Azure AD offre i vantaggi seguenti:

* È possibile controllare in Azure AD chi può accedere a xMatters OnDemand.
* È possibile abilitare gli utenti per l'accesso automatico (Single Sign-On) a xMatters OnDemand con gli account Azure AD personali.
* È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con xMatters OnDemand sono necessari gli elementi seguenti:

* Una sottoscrizione di Azure AD. Se non si dispone di un ambiente di Azure AD, è possibile ottenere un [account gratuito](https://azure.microsoft.com/free/).
* Sottoscrizione di xMatters OnDemand abilitata per l'accesso Single Sign-On

## <a name="scenario-description"></a>Descrizione dello scenario

In questa esercitazione vengono eseguiti la configurazione e il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.

* xMatters OnDemand supporta l'accesso SSO avviato da **IDP**

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>Aggiunta di xMatters OnDemand dalla raccolta

Per configurare l'integrazione di xMatters OnDemand in Azure AD è necessario aggiungere xMatters OnDemand dalla raccolta al proprio elenco di app SaaS gestite.

1. Accedere al portale di Azure con un account aziendale o dell'istituto di istruzione oppure con un account Microsoft personale.
1. Nel riquadro di spostamento a sinistra selezionare il servizio **Azure Active Directory**.
1. Passare ad **Applicazioni aziendali** e quindi selezionare **Tutte le applicazioni**.
1. Per aggiungere una nuova applicazione, selezionare **Nuova applicazione**.
1. Nella sezione **Aggiungi dalla raccolta** digitare **xMatters OnDemand** nella casella di ricerca.
1. Selezionare **xMatters OnDemand** nel pannello dei risultati e quindi aggiungere l'app. Attendere alcuni secondi che l'app venga aggiunta al tenant.


## <a name="configure-and-test-azure-ad-sso-for-xmatters-ondemand"></a>Configurare e testare l'accesso Single Sign-On di Azure AD per xMatters OnDemand

Configurare e testare l'accesso SSO di Azure AD con xMatters OnDemand usando un utente di test di nome **B.Simon**. Per il corretto funzionamento dell'accesso Single Sign-On, è necessario stabilire una relazione di collegamento tra un utente di Azure AD e l'utente correlato in xMatters OnDemand.

Per configurare e testare l'accesso SSO di Azure AD con xMatters OnDemand, seguire questa procedura:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-sso)** : per consentire agli utenti di usare questa funzionalità.
    1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
    2. **[Assegnare l'utente di test di Azure AD](#assign-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
2. **[Configurare l'accesso Single Sign-On di xMatters OnDemand](#configure-xmatters-ondemand-sso)** : per configurare le impostazioni di Single Sign-On sul lato applicazione.
    1. **[Creare un utente test di xMatters OnDemand](#create-xmatters-ondemand-test-user)**: per avere una controparte di Britta Simon in xMatters OnDemand collegata alla rappresentazione dell'utente in Azure AD.
3. **[Testare l'accesso Single Sign-On](#test-sso)** : per verificare se la configurazione funziona.

### <a name="configure-azure-ad-sso"></a>Configurare l'accesso SSO di Azure AD

Per abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure, seguire questa procedura.

1. Nella pagina di integrazione dell'applicazione **xMatters OnDemand** del portale di Azure individuare la sezione **Gestione** e selezionare **Single Sign-On**.
1. Nella pagina **Selezionare un metodo di accesso Single Sign-On** selezionare **SAML**.
1. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sull'icona Modifica (la penna) relativa a **Configurazione SAML di base** per modificare le impostazioni.

   ![Modificare la configurazione SAML di base](common/edit-urls.png)

1. Nella sezione **Configurazione SAML di base** immettere i valori per i campi seguenti:

    a. Nella casella di testo **Identificatore** digitare un URL nel formato seguente:

    | Identificatore |
    | ---------- |
    | `https://<companyname>.au1.xmatters.com.au/` |
    | `https://<companyname>.cs1.xmatters.com/` |
    | `https://<companyname>.xmatters.com/` |
    | `https://www.xmatters.com` |
    | `https://<companyname>.xmatters.com.au/` |

    b. Nella casella di testo **URL di risposta** digitare un URL in uno dei formati seguenti:

    | URL di risposta |
    | ---------- |
    |  `https://<companyname>.au1.xmatters.com.au` |
    | `https://<companyname>.xmatters.com/sp/<instancename>` |
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>` |
    | `https://<companyname>.au1.xmatters.com.au/<instancename>` |

    > [!NOTE]
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'identificatore e l'URL di risposta effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di xMatters OnDemand](https://www.xmatters.com/company/contact-us/). È anche possibile fare riferimento ai modelli mostrati nella sezione **Configurazione SAML di base** del portale di Azure.

5. Nella pagina **Configura l'accesso Single Sign-On con SAML**, nella sezione **Certificato di firma SAML**, fare clic su **Scarica** per scaricare il **Certificato (Base64)** definito dalle opzioni specificate in base ai propri requisiti e salvarlo in questo computer.

    ![Collegamento di download del certificato](common/certificatebase64.png)

    > [!IMPORTANT]
    > Inoltrare il certificato al [team di supporto di xMatters OnDemand](https://www.xmatters.com/company/contact-us/). Per poter completare la configurazione di Single Sign-On, è necessario che il certificato venga caricato dal team di supporto xMatters.

6. Nella sezione **Configura xMatters OnDemand** copiare gli URL appropriati in base alle esigenze.

    ![Copiare gli URL di configurazione](common/copy-configuration-urls.png)

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

In questa sezione si abiliterà B.Simon all'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a xMatters OnDemand.

1. Nel portale di Azure selezionare **Applicazioni aziendali** e quindi **Tutte le applicazioni**.
1. Nell'elenco delle applicazioni selezionare **xMatters OnDemand**.
1. Nella pagina di panoramica dell'app trovare la sezione **Gestione** e selezionare **Utenti e gruppi**.
1. Selezionare **Aggiungi utente** e quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.
1. Nella finestra di dialogo **Utenti e gruppi** selezionare **B.Simon** dall'elenco degli utenti e quindi fare clic sul pulsante **Seleziona** nella parte inferiore della schermata.
1. Se si prevede che agli utenti venga assegnato un ruolo, è possibile selezionarlo nell'elenco a discesa **Selezionare un ruolo**. Se per questa app non è stato configurato alcun ruolo, il ruolo selezionato è "Accesso predefinito".
1. Nella finestra di dialogo **Aggiungi assegnazione** fare clic sul pulsante **Assegna**.


## <a name="configure-xmatters-ondemand-sso"></a>Configurare l'accesso Single Sign-On di xMatters OnDemand

1. In un'altra finestra del Web browser accedere al sito aziendale di XMatters OnDemand come amministratore.

2. Fare clic su **Admin** (Amministrazione) e quindi su **Company Details** (Dettagli sulla società).

    ![Pagina Admin](./media/xmatters-ondemand-tutorial/admin.png "Admin")

3. Nella pagina **Configurazione SAML** seguire la procedura seguente:

    ![Sezione relativa alla configurazione SAML](./media/xmatters-ondemand-tutorial/saml-configuration.png "Configurazione SAML")

    a. Selezionare **Enable SAML**.

    b. Nella casella di testo **Identity Provider ID** (ID provider di identità) incollare il valore di **Identificatore Azure AD** copiato dal portale di Azure.

    c. Nella casella di testo **Single Sign-On URL** (URL di accesso SSO) incollare il valore di **URL di accesso** copiato dal portale di Azure.

    d. Nella casella di testo **Logout URL Redirect** (Reindirizzamento URL di disconnessione) incollare il valore di **URL di disconnessione** copiato dal portale di Azure.

    e. Fare clic su **Choose file** (Scegli file) per caricare il **certificato (Base64)** scaricato dal portale di Azure. 

    f. Nella pagina Company Details, in alto, fare clic su **Save Changes**.

    ![Dettagli della società](./media/xmatters-ondemand-tutorial/save-button.png "Dettagli della società")

### <a name="create-xmatters-ondemand-test-user"></a>Creare un utente test di xMatters OnDemand

1. Accedere al tenant di **XMatters OnDemand**.

2. Passare a **Users Icon** > **Users** (Icona Utenti > Utenti) e quindi fare clic su **Add Users** (Aggiungi utenti).

    ![Utenti](./media/xmatters-ondemand-tutorial/add-user.png "Utenti")

3. Nella sezione **Add Users** (Aggiungi utenti) compilare i campi obbligatori e fare clic sul pulsante **Add User** (Aggiungi utente).

    ![Add a User](./media/xmatters-ondemand-tutorial/add-user-2.png "Aggiungi un utente")



### <a name="test-sso"></a>Testare l'accesso SSO

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD con le opzioni seguenti.

* Dopo aver fatto clic su Test this application (Testa questa applicazione) nel portale di Azure, si dovrebbe accedere automaticamente all'istanza di xMatters OnDemand per cui si è configurato l'accesso SSO

* È possibile usare App personali Microsoft. Quando si fa clic sul riquadro di xMatters OnDemand in App personali, si dovrebbe accedere automaticamente all'applicazione xMatters OnDemand per cui si è configurato l'accesso SSO. Per altre informazioni su App personali, vedere l'[introduzione ad App personali](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Passaggi successivi

Dopo aver configurato xMatters OnDemand, è possibile applicare il controllo sessione che consente di proteggere in tempo reale l'esfiltrazione e l'infiltrazione dei dati sensibili dell'organizzazione. Il controllo sessione costituisce un'estensione dell'accesso condizionale. [Informazioni su come applicare il controllo sessione con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).