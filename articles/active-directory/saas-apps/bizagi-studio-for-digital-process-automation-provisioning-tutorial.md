---
title: "Esercitazione: configurare BizAgi Studio per l'automazione dei processi digitali per il provisioning utenti automatico con Azure Active Directory | Microsoft Docs"
description: Informazioni su come eseguire automaticamente il provisioning e il deprovisioning degli account utente da Azure AD a BizAgi Studio per l'automazione dei processi digitali.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 2fbff65a-5345-4c08-a6c7-60b80d867a3e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2020
ms.author: Zhchia
ms.openlocfilehash: 72e021f47bb8db4dedf0e434d0d94bb2118a4c00
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2021
ms.locfileid: "98728161"
---
# <a name="tutorial-configure-bizagi-studio-for-digital-process-automation-for-automatic-user-provisioning"></a>Esercitazione: configurare BizAgi Studio per l'automazione dei processi digitali per il provisioning utenti automatico

Questa esercitazione descrive i passaggi da eseguire in BizAgi Studio per l'automazione dei processi digitali e Azure Active Directory (Azure AD) per configurare il provisioning utenti automatico. Quando questa operazione viene configurata, Azure AD esegue automaticamente il provisioning e il deprovisioning di utenti e gruppi in [BizAgi Studio per l'automazione dei processi digitali](https://www.bizagi.com/) usando il servizio di provisioning Azure ad. Per informazioni dettagliate sul funzionamento di questo servizio e domande frequenti, vedere [Automatizzare il provisioning e il deprovisioning utenti in applicazioni SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funzionalità supportate
> [!div class="checklist"]
> * Creare utenti in BizAgi Studio per l'automazione dei processi digitali
> * Rimuovere gli utenti in BizAgi Studio per l'automazione dei processi digitali quando non sono più necessari per l'accesso
> * Mantieni gli attributi utente sincronizzati tra Azure AD e BizAgi Studio per l'automazione dei processi digitali
> * [Single Sign-on](./bizagi-studio-for-digital-process-automation-tutorial.md) per BizAgi Studio per l'automazione dei processi digitali (scelta consigliata)

## <a name="prerequisites"></a>Prerequisiti

Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

* [Un tenant di Azure AD](../develop/quickstart-create-new-tenant.md). 
* Un account utente in Azure AD con l'[autorizzazione](../roles/permissions-reference.md) per configurare il provisioning, Gli esempi includono amministratore dell'applicazione, amministratore di applicazioni cloud, proprietario dell'applicazione o amministratore globale. 
* BizAgi Studio per l'automazione dei processi digitali versione 11.2.4.2 X o successiva.

## <a name="plan-your-provisioning-deployment"></a>Pianificare la distribuzione del provisioning
Per la pianificazione, attenersi alla procedura seguente:

1. Acquisire informazioni su [come funziona il servizio di provisioning](../app-provisioning/user-provisioning.md).
2. Determinare chi sarà [nell'ambito per il provisioning](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Determinare quali dati eseguire il [mapping tra Azure ad e BizAgi Studio per l'automazione dei processi digitali](../app-provisioning/customize-application-attributes.md). 

## <a name="configure-to-support-provisioning-with-azure-ad"></a>Configurare per supportare il provisioning con Azure AD
Per configurare BizAgi Studio per l'automazione dei processi digitali per supportare il provisioning con Azure AD, seguire questa procedura:

1. Accedere al portale aziendale come utente con **autorizzazioni di amministratore**.

2. Passare a **admin**  >  **Security**  >  **OAuth 2 Applications**.

   ![Screenshot di BizAgi, con le applicazioni OAuth 2 evidenziate.](media/bizagi-studio-for-digital-process-automation-provisioning-tutorial/admin.png)

3. Selezionare **Aggiungi**.
4. Per **tipo di concessione** selezionare **token di porta**. Per **ambito consentito** selezionare **API** e **sincronizzazione utente**. Selezionare quindi **Salva**.

   ![Screenshot dell'applicazione Register, con tipo di concessione e ambito consentito evidenziato.](media/bizagi-studio-for-digital-process-automation-provisioning-tutorial/token.png)

5. Copiare e salvare il **segreto client**. Nel portale di Azure, per l'applicazione BizAgi Studio per l'automazione dei processi digitali, nella scheda **provisioning** il valore del segreto client viene immesso nel campo **token segreto** .

   ![Screenshot di OAuth con il segreto client evidenziati.](media/bizagi-studio-for-digital-process-automation-provisioning-tutorial/secret.png)

## <a name="add-the-application-from-the-azure-ad-gallery"></a>Aggiungere l'applicazione dalla raccolta di Azure AD

Per iniziare a gestire il provisioning in BizAgi Studio per l'automazione dei processi digitali, aggiungere l'app dalla raccolta di applicazioni Azure AD. Se in precedenza è stato configurato BizAgi Studio per l'automazione dei processi digitali per Single Sign-On, è possibile usare la stessa applicazione. Quando si esegue il test iniziale dell'integrazione, tuttavia, è necessario creare un'app separata. Per altre informazioni, vedere [Guida introduttiva: aggiungere un'applicazione al tenant di Azure Active Directory (Azure ad)](../manage-apps/add-application-portal.md). 

## <a name="define-who-is-in-scope-for-provisioning"></a>Definire gli utenti che rientrano nell'ambito del provisioning 

Con il servizio di provisioning di Azure AD, è possibile definire l'ambito di cui viene eseguito il provisioning in base all'assegnazione all'applicazione, in base agli attributi dell'utente e del gruppo o a entrambi. Se l'ambito è basato sull'assegnazione, vedere la procedura in [assegnare o annullare l'assegnazione di utenti e gruppi per un'app che usa la API Graph](../manage-apps/assign-user-or-group-access-portal.md) per assegnare utenti e gruppi all'applicazione. Se l'ambito è basato esclusivamente sugli attributi dell'utente o del gruppo, è possibile utilizzare un filtro di ambito. Per altre informazioni, vedere [provisioning di applicazioni basate su attributi con filtri di ambito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

Si notino i seguenti punti sull'ambito:

* Quando si assegnano utenti e gruppi a BizAgi Studio per l'automazione dei processi digitali, è necessario selezionare un ruolo diverso da quello **predefinito**. Gli utenti con il ruolo di accesso predefinito vengono esclusi dal provisioning e sono contrassegnati nei log di provisioning come verranno contrassegnati come non autorizzati. Se l'unico ruolo disponibile nell'applicazione è il ruolo di accesso predefinito, è possibile [aggiornare il manifesto dell'applicazione](../develop/howto-add-app-roles-in-azure-ad-apps.md) per aggiungere altri ruoli. 

* Iniziare con pochi elementi. Eseguire il test con un piccolo set di utenti e gruppi prima di eseguire la distribuzione a tutti. Quando l'ambito per il provisioning è impostato su utenti e gruppi assegnati, è possibile controllarlo assegnando uno o due utenti o gruppi all'app. Quando l'ambito è impostato su tutti gli utenti e i gruppi, è possibile specificare un [filtro di ambito basato su attributi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="configure-automatic-user-provisioning"></a>Configurare il provisioning automatico degli utenti 

Questa sezione illustra i passaggi per configurare il servizio di provisioning Azure AD per creare, aggiornare e disabilitare utenti e gruppi. Questa operazione viene eseguita nell'app di test, in base alle assegnazioni di utenti e gruppi in Azure AD.

### <a name="configure-automatic-user-provisioning-for-bizagi-studio-for-digital-process-automation-in-azure-ad"></a>Configurare il provisioning utenti automatico per BizAgi Studio per l'automazione dei processi digitali in Azure AD

1. Accedere al [portale di Azure](https://portal.azure.com). Selezionare **Applicazioni aziendali** > **Tutte le applicazioni**.

    ![Screenshot del portale di Azure, con applicazioni aziendali e tutte le applicazioni evidenziate.](common/enterprise-applications.png)

2. Nell'elenco delle applicazioni selezionare **BizAgi Studio per l'automazione dei processi digitali**.

3. Selezionare la scheda **Provisioning**.

    ![Screenshot delle opzioni di gestione con provisioning evidenziato.](common/provisioning.png)

4. Impostare **Modalità di provisioning** su **Automatico**.

    ![Controllo della modalità di provisioning di Screenshotof, con evidenziato automaticamente.](common/provisioning-automatic.png)

5. Nella sezione **credenziali amministratore** immettere l'URL del tenant e il token segreto per BizAgi Studio per l'automazione dei processi digitali. 

      * **URL tenant:** Immettere l'endpoint SCIM di BizAgi, con la struttura seguente:  `<Your_Bizagi_Project>/scim/v2/` .
         Ad esempio: `https://my-company.bizagi.com/scim/v2/`.

      * **Token segreto:** Questo valore viene recuperato dal passaggio descritto in precedenza in questo articolo.

      Per assicurarsi che Azure AD possa connettersi a BizAgi Studio per l'automazione dei processi digitali, selezionare **Test connessione**. Se la connessione non riesce, verificare che l'account di automazione del processo di BizAgi Studio per il processo digitale disponga di autorizzazioni di amministratore e riprovare.

   ![Screenshot delle credenziali di amministratore, con la connessione di test evidenziata.](common/provisioning-testconnection-tenanturltoken.png)

6. Per **messaggio di posta elettronica di notifica** immettere l'indirizzo di posta elettronica di una persona o un gruppo che riceverà le notifiche degli errori di provisioning. Selezionare l'opzione per **inviare una notifica tramite posta elettronica quando si verifica un errore**.

    ![Screenshot delle opzioni di posta elettronica di notifica.](common/provisioning-notification-email.png)

7. Selezionare **Salva**.

8. Nella sezione **mapping** selezionare **Sincronizza Azure Active Directory utenti a BizAgi Studio per l'automazione dei processi digitali**.

9. Nella sezione **mapping** degli attributi esaminare gli attributi utente che vengono sincronizzati da Azure ad a BizAgi Studio per l'automazione dei processi digitali. Gli attributi selezionati come proprietà **corrispondenti** vengono usati per trovare le corrispondenze con gli account utente in BizAgi Studio per l'automazione dei processi digitali per le operazioni di aggiornamento. Se si modifica l' [attributo di destinazione corrispondente](../app-provisioning/customize-application-attributes.md), è necessario assicurarsi che l'API BizAgi Studio per l'automazione dei processi digitali supporti il filtraggio degli utenti in base a tale attributo. Selezionare **Salva** per eseguire il commit delle modifiche.

   |Attributo|Type|Supportato per il filtro|
   |---|---|---|
   |userName|string|&check;|
   |active|Boolean|
   |emails[type eq "work"].value|string|
   |name.givenName|string|
   |name.familyName|string|
   |name.formatted|string|
   |phoneNumbers[type eq "mobile"].value|string|

   Gli attributi di estensione personalizzati possono essere aggiunti passando a **Mostra opzioni avanzate > modifica elenco attributi per BizAgi**. Gli attributi dell'estensione personalizzata devono essere preceduti da **urn: IETF: params: SCIM: schemas: Extension: BizAgi: 2.0: UserProperties:**. Se, ad esempio, l'attributo di estensione personalizzato è **IdentificationNumber**, è necessario aggiungere l'attributo come **urn: IETF: params: SCIM: schemas: Extension: BizAgi: 2.0: UserProperties: IdentificationNumber**. Selezionare **Salva** per eseguire il commit delle modifiche.
   
    ![Modifica elenco attributi.](media/bizagi-studio-for-digital-process-automation-provisioning-tutorial/edit.png)  

   Altre informazioni su come aggiungere attributi personalizzati sono disponibili in [personalizzare gli attributi dell'applicazione](../app-provisioning/customize-application-attributes.md).

> [!NOTE]
> Sono supportate solo le proprietà di base del tipo, ad esempio String, Integer, Boolean, DateTime e così via. Le proprietà collegate a tabelle parametriche o a più tipi non sono ancora supportate.

10. Per configurare i filtri di ambito, vedere l'esercitazione relativa al [filtro di ambito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Per abilitare il servizio di provisioning Azure AD per BizAgi Studio per l'automazione dei processi digitali, nella sezione **Impostazioni** impostare **stato del provisioning** **su** attivato.

    ![Screenshot dello stato del provisioning dello stato di provisioning.](common/provisioning-toggle-on.png)

12. Definire gli utenti e i gruppi di cui si vuole eseguire il provisioning in BizAgi Studio per l'automazione dei processi digitali. Nella sezione **Impostazioni** scegliere i valori desiderati nell' **ambito**.

    ![Screenshot delle opzioni di ambito.](common/provisioning-scope.png)

13. Quando si è pronti per eseguire il provisioning, selezionare **Salva**.

    ![Screenshot del controllo Save.](common/provisioning-configuration-save.png)

L'operazione avvia il ciclo di sincronizzazione iniziale di tutti gli utenti e i gruppi definiti in **Ambito** nella sezione **Impostazioni**. Il ciclo di sincronizzazione iniziale richiede più tempo dei cicli successivi, che verranno eseguiti ogni 40 minuti circa quando il servizio di provisioning di Azure AD è in esecuzione. 

## <a name="monitor-your-deployment"></a>Monitorare la distribuzione
Dopo aver configurato il provisioning, usare le risorse seguenti per monitorare la distribuzione:

- Usare i [log di provisioning](../reports-monitoring/concept-provisioning-logs.md) per determinare gli utenti di cui è stato eseguito il provisioning con esito positivo o negativo.
- Controllare l' [indicatore di stato per visualizzare](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) lo stato del ciclo di provisioning e la relativa chiusura.
- Se la configurazione del provisioning è in uno stato non integro, l'applicazione entra in quarantena. Per ulteriori informazioni, vedere [provisioning di applicazioni in stato di quarantena](../app-provisioning/application-provisioning-quarantine-status.md).  

## <a name="additional-resources"></a>Risorse aggiuntive

* [Gestione del provisioning degli account utente per app aziendali](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Passaggi successivi

* [Informazioni su come esaminare i log e ottenere report sulle attività di provisioning](../app-provisioning/check-status-user-account-provisioning.md)