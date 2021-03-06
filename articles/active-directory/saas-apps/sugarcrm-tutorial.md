---
title: "Esercitazione: Integrazione dell'accesso Single Sign-On (SSO) di Azure Active Directory con Sugar CRM | Microsoft Docs"
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Sugar CRM.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/22/2021
ms.author: jeedes
ms.openlocfilehash: 9cd91bf40354a40129d20a6ed0381801fece4ba4
ms.sourcegitcommit: 1a98b3f91663484920a747d75500f6d70a6cb2ba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99063140"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sugar-crm"></a>Esercitazione: Integrazione dell'accesso Single Sign-On (SSO) di Azure Active Directory con Sugar CRM

Questa esercitazione descrive come integrare Sugar CRM con Azure Active Directory (Azure AD). Integrando Sugar CRM con Azure AD è possibile:

* Controllare in Azure AD chi può accedere a Sugar CRM.
* Abilitare gli utenti per l'accesso automatico a Sugar CRM con gli account Azure AD personali.
* Gestire gli account in un'unica posizione centrale: il portale di Azure.

## <a name="prerequisites"></a>Prerequisiti

Per iniziare, sono necessari gli elementi seguenti:

* Una sottoscrizione di Azure AD. Se non si ha una sottoscrizione, è possibile ottenere un [account gratuito](https://azure.microsoft.com/free/).
* Sottoscrizione di Sugar CRM abilitata per l'accesso Single Sign-On (SSO).

## <a name="scenario-description"></a>Descrizione dello scenario

In questa esercitazione vengono eseguiti la configurazione e il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.

* Sugar CRM supporta l'accesso SSO avviato da **SP**

> [!NOTE]
> Dal momento che l'identificatore di questa applicazione è un valore stringa fisso, è possibile configurare una sola istanza in un solo tenant.

## <a name="add-sugar-crm-from-the-gallery"></a>Aggiungere Sugar CRM dalla raccolta

Per configurare l'integrazione di Sugar CRM in Azure AD, è necessario aggiungere Sugar CRM dalla raccolta all'elenco di app SaaS gestite.

1. Accedere al portale di Azure con un account aziendale o dell'istituto di istruzione oppure con un account Microsoft personale.
1. Nel riquadro di spostamento a sinistra selezionare il servizio **Azure Active Directory**.
1. Passare ad **Applicazioni aziendali** e quindi selezionare **Tutte le applicazioni**.
1. Per aggiungere una nuova applicazione, selezionare **Nuova applicazione**.
1. Nella sezione **Aggiungi dalla raccolta** digitare **Sugar CRM** nella casella di ricerca.
1. Selezionare **Sugar CRM** nel pannello dei risultati e quindi aggiungere l'app. Attendere alcuni secondi che l'app venga aggiunta al tenant.

## <a name="configure-and-test-azure-ad-sso-for-sugar-crm"></a>Configurare e testare Azure AD SSO per Sugar CRM

Configurare e testare l'accesso SSO di Azure AD con Sugar CRM usando un utente di test di nome **B.Simon**. Per consentire il funzionamento dell'accesso Single Sign-On, è necessario stabilire una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Sugar CRM.

Per configurare e testare Azure AD SSO con Sugar CRM, seguire questa procedura:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-sso)** : per consentire agli utenti di usare questa funzionalità.
    1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente B.Simon.
    1. **[Assegnare l'utente di test di Azure AD](#assign-the-azure-ad-test-user)** : per abilitare B. Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Configurare l'accesso Single Sign-On di Sugar CRM](#configure-sugar-crm-sso)** : per configurare le impostazioni di Single Sign-On sul lato applicazione.
    1. **[Creare l'utente di test di Sugar CRM](#create-sugar-crm-test-user)** : per avere una controparte di B.Simon in Sugar CRM collegata alla rappresentazione dell'utente in Azure AD.
1. **[Testare l'accesso Single Sign-On](#test-sso)** : per verificare se la configurazione funziona.

## <a name="configure-azure-ad-sso"></a>Configurare l'accesso SSO di Azure AD

Per abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure, seguire questa procedura.

1. Nella pagina di integrazione dell'applicazione **Sugar CRM** del portale di Azure trovare la sezione **Gestisci** e selezionare **Single Sign-on**.
1. Nella pagina **Selezionare un metodo di accesso Single Sign-On** selezionare **SAML**.
1. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sull'icona della matita per modificare le impostazioni di **Configurazione SAML di base**.

   ![Modificare la configurazione SAML di base](common/edit-urls.png)

1. Nella sezione **Configurazione SAML di base** immettere i valori per i campi seguenti:

    a. Nella casella di testo **URL accesso** digitare un URL nel formato seguente:

    - `https://<companyname>.sugarondemand.com`
    - `https://<companyname>.trial.sugarcrm`

    b. Nella casella di testo **URL di risposta** digitare un URL nel formato seguente:

    - `https://<companyname>.sugarondemand.com/<companyname>`
    - `https://<companyname>.trial.sugarcrm.com/<companyname>`
    - `https://<companyname>.trial.sugarcrm.eu/<companyname>`

    > [!NOTE]
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'URL di accesso e l'URL di risposta effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di Sugar CRM](https://support.sugarcrm.com/). È anche possibile fare riferimento ai modelli mostrati nella sezione **Configurazione SAML di base** del portale di Azure.

1. Nella sezione **Certificato di firma SAML** della pagina **Configura l'accesso Single Sign-On con SAML** individuare **Certificato (Base64)** e selezionare **Scarica** per scaricare il certificato e salvarlo nel computer.

    ![Collegamento di download del certificato](common/certificatebase64.png)

1. Nella sezione **Configura Sugar CRM** copiare gli URL appropriati in base alle esigenze.

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

In questa sezione si abiliterà B.Simon all'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Sugar CRM.

1. Nel portale di Azure selezionare **Applicazioni aziendali** e quindi **Tutte le applicazioni**.
1. Nell'elenco delle applicazioni selezionare **Sugar CRM**.
1. Nella pagina di panoramica dell'app trovare la sezione **Gestione** e selezionare **Utenti e gruppi**.
1. Selezionare **Aggiungi utente** e quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.
1. Nella finestra di dialogo **Utenti e gruppi** selezionare **B.Simon** dall'elenco degli utenti e quindi fare clic sul pulsante **Seleziona** nella parte inferiore della schermata.
1. Se si prevede che agli utenti venga assegnato un ruolo, è possibile selezionarlo nell'elenco a discesa **Selezionare un ruolo**. Se per questa app non è stato configurato alcun ruolo, il ruolo selezionato è "Accesso predefinito".
1. Nella finestra di dialogo **Aggiungi assegnazione** fare clic sul pulsante **Assegna**.

## <a name="configure-sugar-crm-sso"></a>Configurare l'accesso Single Sign-On di Sugar CRM

1. In un'altra finestra del Web browser accedere al sito aziendale di Sugar CRM come amministratore.

1. Passare alla pagina **Admin**.

    ![Admin](./media/sugarcrm-tutorial/ic795888.png "Amministrativi")

1. Nella sezione **Amministrazione**, fare clic su **Gestione password**.

    ![Screenshot che mostra la sezione Administration in cui è possibile selezionare Password Management.](./media/sugarcrm-tutorial/ic795889.png "Amministrazione")

1. Selezionare **Abilita autenticazione SAML**.

    ![Screenshot che mostra l'opzione per selezionare l'autenticazione SAML.](./media/sugarcrm-tutorial/ic795890.png "Amministrazione")

1. Nella sezione **SAML Authentication** seguire questa procedura:

    ![Autenticazione SAML](./media/sugarcrm-tutorial/save.png "Autenticazione SAML")  

    a. Nella casella di testo **URL di accesso** incollare il valore di **URL di accesso** copiato dal portale di Azure.
  
    b. Nella casella di testo **URL SLO** incollare il valore di **URL disconnessione** copiato dal portale di Azure.
  
    c. Aprire il certificato con codifica Base 64 nel Blocco note, copiarne il contenuto negli Appunti e incollare l’intero certificato nella casella di testo **Certificate X.509** .
  
    d. Fare clic su **Salva**.

### <a name="create-sugar-crm-test-user"></a>Creare un utente di test di Sugar CRM

Per consentire agli utenti di Azure AD di accedere a Sugar CRM, è necessario effettuarne il provisioning in Sugar CRM. Nel caso di Sugar CRM, il provisioning è un'attività manuale.

**Per eseguire il provisioning di un account utente, seguire questa procedura:**

1. Accedere al sito aziendale di **Sugar CRM** come amministratore.

1. Passare alla pagina **Admin**.

    ![Admin](./media/sugarcrm-tutorial/ic795888.png "Amministrativi")

1. Nella sezione **Administration** (Amministrazione) fare clic su **User Management** (Gestione utenti).

    ![Screenshot che mostra la sezione Administration in cui è possibile selezionare User Management.](./media/sugarcrm-tutorial/ic795893.png "Amministrazione")

1. Passare a **Users \> Create New User** (Utenti > Crea nuovo utente).

    ![Creare un nuovo utente](./media/sugarcrm-tutorial/ic795894.png "Crea nuovo utente")

1. Nella scheda **Profilo utente** eseguire la procedura seguente:

    ![Screenshot che mostra la scheda User Profile in cui è possibile immettere i valori indicati.](./media/sugarcrm-tutorial/ic795895.png "Nuovo utente")

    * Digitare il **nome utente**, il **cognome** e l'**indirizzo e-mail** di un utente di Azure Active Directory valido nelle caselle di testo correlate.
  
1. Per **Status** (Stato) selezionare **Active** (Attivo).

1. Nella scheda Password, eseguire la procedura seguente:

    ![Screenshot che mostra la scheda Password in cui è possibile immettere i valori indicati.](./media/sugarcrm-tutorial/ic795896.png "Nuovo utente")

    a. Digitare la password nella casella di controllo correlata.

    b. Fare clic su **Salva**.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornito da Sugar CRM per effettuare il provisioning degli account utente Azure AD.

## <a name="test-sso"></a>Testare l'accesso SSO 

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD con le opzioni seguenti. 

* Fare clic su **Test this application** (Testa questa applicazione) nel portale di Azure. Verrà eseguito il reindirizzamento all'URL di accesso di Sugar CRM in cui è possibile avviare il flusso di accesso. 

* Passare direttamente all'URL di accesso di Sugar CRM e avviare il flusso di accesso da qui.

* È possibile usare App personali Microsoft. Quando si fa clic sul riquadro Sugar CRM in app personali, questo verrà reindirizzato all'URL di accesso a Sugar CRM. Per altre informazioni su App personali, vedere l'[introduzione ad App personali](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Passaggi successivi

Dopo aver configurato Sugar CRM, è possibile applicare il controllo della sessione, che protegge exfiltration e infiltrando i dati sensibili dell'organizzazione in tempo reale. Il controllo sessione costituisce un'estensione dell'accesso condizionale. [Informazioni su come applicare il controllo sessione con Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).
