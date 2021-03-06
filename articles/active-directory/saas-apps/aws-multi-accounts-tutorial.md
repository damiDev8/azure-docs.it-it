---
title: 'Esercitazione: Integrazione di Azure Active Directory con Amazon Web Service (AWS) per connettere più account | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure AD e AWS (Amazon Web Services) (esercitazione legacy).
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/24/2020
ms.author: jeedes
ms.openlocfilehash: 440fe52689a345eec36c3ac613d6bc2cc2dccc13
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2021
ms.locfileid: "98735461"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws-legacy-tutorial"></a>Esercitazione: Integrazione di Azure Active Directory con AWS (Amazon Web Service) (esercitazione legacy)

Questa esercitazione descrive come integrare Azure Active Directory (Azure AD) con più account di Amazon Web Services (AWS) (esercitazione legacy).

L'integrazione di Amazon Web Service (AWS) con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere ad Amazon Web Services (AWS).
- È possibile abilitare gli utenti per l'accesso automatico ad Amazon Web Services (AWS) (Single Sign-On) con i propri account Azure AD.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

![Amazon Web Services (AWS) nell'elenco dei risultati](./media/aws-multi-accounts-tutorial/amazonwebservice.png)

> [!NOTE]
> Si noti che collegare un'app AWS a tutti gli account AWS non è l'approccio consigliato. Si consiglia di usare [questo](./amazon-web-service-tutorial.md) approccio per configurare più istanze di account AWS con più istanze di app AWS in Azure AD. È consigliabile usare questo approccio solo se sono disponibili pochi account AWS e pochi ruoli al loro interno, perché questo modello non è scalabile con la crescita degli account AWS e dei relativi ruoli. Questo approccio non usa la funzionalità di importazione dei ruoli AWS con il provisioning utenti di Azure AD, quindi è necessario aggiungere, aggiornare e/o eliminare manualmente i ruoli. Per altre limitazioni su questo approccio, vedere i dettagli riportati di seguito.

**Non è consigliabile usare questo approccio per i motivi seguenti:**

* È necessario usare l'approccio Microsoft Graph Explorer per applicare la patch a tutti i ruoli dell'app. Non è consigliabile usare l'approccio del file manifesto.

* Molti clienti hanno segnalato che dopo aver aggiunto ~1200 ruoli dell'app per una singola app AWS, qualsiasi operazione sull'app generava errori relativi alle dimensioni. Esiste un limite rigido di dimensioni nell'oggetto applicazione.

* È necessario aggiungere manualmente il ruolo quando viene aggiunto in uno degli account. Si tratta di un approccio Replace e non Append. Inoltre, se gli account aumentano, diventa una relazione n x n con account e ruoli.

* Tutti gli account AWS useranno lo stesso file XML dei metadati della federazione e al momento del rollover dei certificati sarà necessario aggiornare il certificato in tutti gli account AWS contemporaneamente

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Amazon Web Service (AWS), sono necessari gli elementi seguenti:

* Una sottoscrizione di Azure AD. Se non si dispone di un ambiente Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/)
* Sottoscrizione di Amazon Web Services (AWS) abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario

In questa esercitazione vengono eseguiti la configurazione e il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.

* Amazon Web Services (AWS) supporta l'accesso SSO avviato da **SP e IDP**

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Aggiunta di Amazon Web Service (AWS) dalla raccolta

Per configurare l'integrazione di Amazon Web Service (AWS) in Azure AD, è necessario aggiungere Amazon Web Service (AWS) dalla raccolta all'elenco di app SaaS gestite.

1. Accedere al portale di Azure con un account aziendale o dell'istituto di istruzione oppure con un account Microsoft personale.
1. Nel riquadro di spostamento a sinistra selezionare il servizio **Azure Active Directory**.
1. Passare ad **Applicazioni aziendali** e quindi selezionare **Tutte le applicazioni**.
1. Per aggiungere una nuova applicazione, selezionare **Nuova applicazione**.
1. Nella sezione **Aggiungi dalla raccolta** digitare **Amazon Web Services (AWS)** nella casella di ricerca.
1. Selezionare **Amazon Web Service (AWS)** nel pannello dei risultati e quindi aggiungere l'app. Attendere alcuni secondi che l'app venga aggiunta al tenant.

1. Dopo avere aggiunto l'applicazione, andare alla pagina **Proprietà** e copiare l'**ID oggetto**.

    ![ID dell'oggetto.](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-properties.png)

## <a name="configure-and-test-azure-ad-sso"></a>Configurare e testare l'accesso SSO di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Amazon Web Services (AWS) usando un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve sapere qual è l'utente di Amazon Web Service (AWS) che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l’utente correlato in Amazon Web Service (AWS).

In Amazon Web Services (AWS) assegnare il valore del **nome utente** di Azure AD come valore dell'attributo **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con AWS (Amazon Web Services), seguire questa procedura:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-sso)** : per consentire agli utenti di usare questa funzionalità.
2. **[Configurare l'accesso Single Sign-On di Amazon Web Services (AWS)](#configure-amazon-web-services-aws-sso)** : per configurare le impostazioni di Single Sign-On sul lato applicazione.
3. **[Testare l'accesso Single Sign-On](#test-sso)** : per verificare se la configurazione funziona.

### <a name="configure-azure-ad-sso"></a>Configurare l'accesso SSO di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Amazon Web Services (AWS).

**Per configurare l'accesso Single Sign-On di Azure AD con Amazon Web Service (AWS), seguire la procedura seguente:**

1. Nella pagina di integrazione dell'applicazione **Amazon Web Services (AWS)** del portale di Azure selezionare **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On](common/select-sso.png)

2. Nella finestra di dialogo **Selezionare un metodo di accesso Single Sign-On** selezionare la modalità **SAML/WS-Fed** per abilitare il Single Sign-On.

    ![Selezione della modalità Single Sign-On](common/select-saml-option.png)

3. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sull'icona della **matita** per aprire la finestra di dialogo **Configurazione SAML di base**.

    ![Modificare la configurazione SAML di base](common/edit-urls.png)

4. Nella sezione **Configurazione SAML di base** l'utente non deve eseguire alcuna operazione perché l'app è già preintegrata in Azure. Fare clic su **Salva**.

5. L'applicazione Amazon Web Services (AWS) si aspetta che le asserzioni SAML abbiano un formato specifico. Configurare le attestazioni seguenti per questa applicazione. È possibile gestire i valori di questi attributi dalla sezione **Attributi utente e attestazioni** nella pagina di integrazione dell'applicazione. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sul pulsante **Modifica** per aprire la finestra di dialogo **Attributi utente e attestazioni**.

    ![Screenshot che mostra la finestra Attributi utente con il controllo di modifica evidenziato.](common/edit-attribute.png)

6. Nella sezione **Attestazioni utente** della finestra di dialogo **Attributi utente** configurare l'attributo del token SAML come mostrato nell'immagine precedente e seguire questa procedura:

    | Nome  | Attributo di origine  | Spazio dei nomi |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | `https://aws.amazon.com/SAML/Attributes` |
    | Ruolo | user.assignedroles | `https://aws.amazon.com/SAML/Attributes`|
    | SessionDuration | "inserire un valore compreso tra 900 secondi (15 minuti) e 43200 secondi (12 ore)" |  `https://aws.amazon.com/SAML/Attributes` |

    1. Fare clic su **Aggiungi nuova attestazione** per aprire la finestra di dialogo **Gestisci attestazioni utente**.

        ![Screenshot che mostra la finestra Attestazioni utente con i pulsanti Aggiungi nuova attestazione e Salva evidenziati.](common/new-save-attribute.png)

        ![Screenshot che mostra la finestra di dialogo Gestisci attestazioni utente in cui è possibile immettere i valori descritti in questo passaggio.](common/new-attribute-details.png)

    b. Nella casella di testo **Nome** digitare il nome dell'attributo indicato per la riga.

    c. Nella casella di testo **Spazio dei nomi** digitare il valore dello spazio dei nomi indicato per la riga.

    d. Per Origine selezionare **Attributo**.

    e. Nell'elenco **Attributo di origine** selezionare il valore dell'attributo indicato per la riga.

    f. Fare clic su **OK**.

    g. Fare clic su **Salva**.

    >[!NOTE]
    >Per altre informazioni sui ruoli in Azure AD, vedere [qui](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui--preview).

7. Nella pagina **Configura l'accesso Single Sign-On con SAML**, nella sezione **Certificato di firma SAML** fare clic su **Scarica** per scaricare il file **XML dei metadati federazione** e salvarlo nel computer in uso.

    ![Collegamento di download del certificato](common/metadataxml.png)

### <a name="configure-amazon-web-services-aws-sso"></a>Configurare l'accesso Single Sign-On di Amazon Web Services (AWS)

1. In un'altra finestra del Web browser accedere al sito aziendale di Amazon Web Service (AWS) come amministratore.

1. Fare clic su **AWS** nella home page della console.

    ![Configurare la home page dell'accesso Single Sign-On][11]

1. Fare clic su **Identity and Access Management**.

    ![Configurare l'identità dell'accesso Single Sign-On][12]

1. Fare clic su **Provider di identità** e quindi fare clic su **Create Provider** (Crea provider).

    ![Configurare il provider dell'accesso Single Sign-On][13]

1. Nella pagina **Configure Provider** seguire questa procedura:

    ![Configurare la finestra di dialogo dell'accesso Single Sign-On][14]

    a. In **Tipo provider** selezionare **SAML**.

    b. Nella casella di testo **Nome provider** digitare un nome di provider, ad esempio *WAAD*.

    c. Per caricare il **file di metadati** scaricato dal portale di Azure, fare clic su **Scegli file**.

    d. Fare clic su **Next Step**.

1. Nella pagina della finestra di dialogo **Verify Provider Information** (Verifica informazioni provider) fare clic su **Crea**.

    ![Configurare la verifica dell'accesso Single Sign-On][15]

1. Fare clic su **Roles** (Ruoli) e quindi su **Create role** (Crea ruolo).

    ![Configurare i ruoli dell'accesso Single Sign-On][16]

    > [!NOTE]
    > La lunghezza combinata del ruolo ARN e del provider SAML ARN per un ruolo da importare non deve superare 240 caratteri.

1. Nella pagina **Create role** (Crea ruolo) seguire questa procedura:  

    ![Configurare una relazione di trust dell'accesso Single Sign-On][19]

    a. Selezionare **SAML 2.0 federation** (Federazione SAML 2.0) in **Select type of trusted entity** (Selezionare un tipo di entità attendibile).

    b. Nella sezione **Choose a SAML 2.0 Provider** (Scegliere un provider SAML 2.0) selezionare il **provider SAML** creato in precedenza, ad esempio *WAAD*.

    c. Selezionare **Allow programmatic and AWS Management Console access** (Consenti accesso a livello di codice e tramite AWS Management Console).

    d. Fare clic su **Next: Permissions** (Avanti: Autorizzazioni).

1. Immettere **Administrator Access** nella barra di ricerca e selezionare la casella di controllo **AdministratorAccess** (Accesso amministratore), quindi fare clic su **Avanti: Tag**.

    ![Screenshot della casella AdministratorAccess selezionata come nome di criterio.](./media/aws-multi-accounts-tutorial/administrator-access.png)

1. Nella sezione **Add tags (optional)** (Aggiungi tag, facoltativo) seguire questa procedura:

    ![Aggiungi tag](./media/aws-multi-accounts-tutorial/config2.png)

    a. Nella casella di testo **Key** (Chiave) immettere il nome della chiave, ad esempio Azureadtest.

    b. Nella casella di testo **Value (optional)** (Valore,facoltativo) immettere il valore della chiave nel formato `accountname-aws-admin` seguente. Il nome dell'account deve essere specificato tutto in minuscolo.

    c. Fare clic su **Avanti: Review** (Avanti: Revisione).

1. Nella finestra di dialogo **Review** seguire questa procedura:

    ![Riepilogo della configurazione dell'accesso Single Sign-On][34]

    a. Nella casella di testo **Role name** (Nome ruolo) immettere il valore seguendo il modello `accountname-aws-admin`.

    b. Nella casella di testo **Role description** (Descrizione ruolo) immettere lo stesso valore usato per il nome del ruolo.

    c. Fare clic su **Crea ruolo**.

    d. Creare tutti i ruoli necessari in base alle esigenze ed eseguirne il mapping per il Provider di identità.

    > [!NOTE]
    > Allo stesso modo creare gli altri ruoli rimanenti, come accountname-finance-admin, accountname-read-only-user, accountname-devops-user, accountname-tpm-user, con criteri diversi da collegare. In un secondo momento, anche questi criteri del ruolo possono essere cambiati in base ai requisiti per ogni account AWS, ma è sempre preferibile mantenere gli stessi criteri per ogni ruolo.

1. Prendere nota dell'ID account per l'account AWS nelle proprietà EC2 o nel dashboard IAM, come evidenziato di seguito:

    ![Screenshot che mostra dove viene visualizzato l'ID account nella finestra AWS.](./media/aws-multi-accounts-tutorial/aws-accountid.png)

1. Accedere ora al portale di Azure e passare a **Gruppi**.

1. Creare nuovi gruppi con lo stesso nome dei ruoli di IAM creati in precedenza e prendere nota degli **ID oggetto** di questi nuovi gruppi.

    ![Selezionare Administrator Access 1](./media/aws-multi-accounts-tutorial/copy-objectids.png)

1. Disconnettersi dall'account AWS corrente e accedere con l'altro account in cui si vuole configurare Single Sign-On con Azure AD.

1. Dopo che tutti i ruoli sono stati creati negli account, vengono visualizzati nell'elenco **Roles** (Ruoli) per tali account.

    ![Screenshot che mostra l'elenco dei ruoli con il nome del ruolo, la descrizione e le entità attendibili.](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-listofroles.png)

1. È necessario acquisire tutti gli ARN di ruolo e le entità attendibili per tutti i ruoli in tutti gli account, che servono per eseguire il mapping manuale con l'applicazione Azure AD.

1. Fare clic sui ruoli per copiare i valori di **Role ARN** (ARN del ruolo) e **Trusted Entities** (Entità attendibili). Questi valori sono necessari per tutti i ruoli da creare in Azure AD.

    ![Configurazione dei ruoli 2](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-role-summary.png)

1. Eseguire il passaggio precedente per tutti i ruoli in tutti gli account e archiviarli tutti nel formato **Ruolo ARN,Entità attendibili** nel Blocco note.

1. Aprire [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) in un'altra finestra.

    1. Accedere al sito di Microsoft Graph Explorer con credenziali di amministratore globale/coamministratore per il tenant.

    1. È necessario avere autorizzazioni sufficienti per creare i ruoli. Fare clic su **modify permissions** (autorizzazioni di modifica) per ottenere le autorizzazioni necessarie.

        ![Finestra di dialogo Microsoft Graph Explorer 1](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    1. Selezionare le autorizzazioni seguenti dell'elenco (se non sono già disponibili) e fare clic su "Autorizzazioni di modifica" 

        ![Finestra di dialogo Microsoft Graph Explorer 2](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    1. Verrà chiesto di eseguire di nuovo l'accesso e di accettare il consenso. Dopo aver accettato il consenso, si viene nuovamente connessi a Microsoft Graph Explorer.

    1. Modificare l'elenco a discesa della versione con **beta**. Per recuperare tutte le entità servizio dal tenant, usare la query seguente: `https://graph.microsoft.com/beta/servicePrincipals`. Se si usano più directory, è possibile usare il modello seguente, che include il dominio primario: `https://graph.microsoft.com/beta/contoso.com/servicePrincipals`.

        ![Finestra di dialogo Microsoft Graph Explorer 3](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)

    1. Dall'elenco delle entità servizio recuperate ottenere quella da modificare. È anche possibile usare CTRL+F per cercare l'applicazione in tutte le entità servizio elencate. È possibile usare la query seguente usando l'**ID oggetto entità servizio** copiato dalla pagina delle proprietà di Azure AD per passare alla rispettiva entità servizio.

        `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

        ![Finestra di dialogo Microsoft Graph Explorer 4](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    1. Estrarre la proprietà appRoles dall'oggetto entità servizio.

        ![Finestra di dialogo Microsoft Graph Explorer 5](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

    1. È ora necessario generare nuovi ruoli per l'applicazione. 

    1. Di seguito è riportato un codice JSON di esempio di oggetto appRoles. Creare un oggetto simile per aggiungere i ruoli desiderati per l'applicazione.

    ```
    {
    "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "4aacf5a4-f38b-4861-b909-bae023e88dde",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Admin,arn:aws:iam::12345:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Auditors,WAAD",
            "displayName": "Auditors,WAAD",
            "id": "bcad6926-67ec-445a-80f8-578032504c09",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Auditors,arn:aws:iam::12345:saml-provider/WAAD"
        }    ]
    }
    ```

    > [!Note]
    > È possibile aggiungere i nuovi ruoli solo dopo **msiam_access** per l'operazione patch. È anche possibile aggiungere tutti i ruoli desiderati in base alle esigenze dell'organizzazione. Azure AD invierà il **valore** di questi ruoli come valore attestazione nella risposta SAML.

    1. Tornare in Microsoft Graph Explorer e cambiare il metodo da **GET** a **PATCH**. Applicare la patch all'oggetto entità servizio per avere i ruoli desiderati aggiornando la proprietà appRoles in modo simile a quella visualizzata sopra nell'esempio. Fare clic su **Esegui query** per eseguire l'operazione patch. Un messaggio di operazione completata conferma la creazione del ruolo per l'applicazione Amazon Web Services.

        ![Finestra di dialogo Microsoft Graph Explorer 6](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

1. Dopo che all'entità servizio è stata applicata la patch con più ruoli, è possibile assegnare gli utenti o i gruppi ai rispettivi ruoli. Questa operazione può essere eseguita accedendo al portale e passando all'applicazione Amazon Web Services. Fare clic sulla scheda **Utenti e gruppi** nella parte superiore.

1. È consigliabile creare nuovi gruppi per ogni ruolo AWS in modo che sia possibile assegnare un determinato ruolo in un determinato gruppo. Si noti che si tratta di un mapping uno-a-uno per un gruppo a un ruolo. È quindi possibile aggiungere i membri che appartengono a tale gruppo.

1. Dopo che sono stati creati i gruppi, selezionare il gruppo e assegnarlo all'applicazione.

    ![Configurare l'aggiunta di Single Sign-On 1](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

    > [!Note]
    > I gruppi annidati non sono supportati quando si assegnano i gruppi.

1. Per assegnare il ruolo al gruppo, selezionare il ruolo e fare clic sul pulsante **Assegna** nella parte inferiore della pagina.

    ![Configurare l'aggiunta di Single Sign-On 2](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

    > [!Note]
    > Si noti che è necessario aggiornare la sessione nel portale di Azure per visualizzare i nuovi ruoli.

### <a name="test-sso"></a>Testare l'accesso SSO

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando App personali.

Quando si fa clic sul riquadro Amazon Web Service (AWS) in App personali, si dovrebbe accedere alla pagina dell'applicazione Amazon Web Service (AWS) con l'opzione per selezionare il ruolo.

![Testare Single Sign-On 1](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-screen.png)

È anche possibile verificare la risposta SAML per visualizzare i ruoli passati come attestazioni.

![Testare Single Sign-On 2](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-saml.png)

Per altre informazioni su App personali, vedere l'[introduzione ad App personali](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Passaggi successivi

Dopo aver configurato Amazon Web Services (AWS), è possibile applicare il controllo sessione che protegge in tempo reale l'esfiltrazione e l'infiltrazione dei dati sensibili dell'organizzazione. Il controllo sessione costituisce un'estensione dell'accesso condizionale. [Informazioni su come applicare il controllo sessione con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad)

<!--Image references-->

[11]: ./media/aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning.png
[34]: ./media/aws-multi-accounts-tutorial/config3.png
[36]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-securitycredentials.png
[37]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-securitycredentials-continue.png
[38]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-createnewaccesskey.png
[39]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning-automatic.png
[40]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning-testconnection.png
[41]: ./media/aws-multi-accounts-tutorial/
