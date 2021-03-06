---
title: 'Esercitazione: Configurare Salesforce Sandbox per il provisioning utenti automatico con Azure Active Directory | Microsoft Docs'
description: Informazioni sulle procedure da eseguire in Salesforce Sandbox e Azure AD per effettuare automaticamente il provisioning e il deprovisioning degli account utente da Azure AD a Salesforce Sandbox.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 7e3f8e5e975468b468712ae8907cdca0e80a5f9f
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2020
ms.locfileid: "94352609"
---
# <a name="tutorial-configure-salesforce-sandbox-for-automatic-user-provisioning"></a>Esercitazione: Configurare Salesforce Sandbox per il provisioning utenti automatico

Questa esercitazione descrive le procedure da eseguire in Salesforce Sandbox e Azure AD per effettuare automaticamente il provisioning e il deprovisioning degli account utente da Azure AD a Salesforce Sandbox.

## <a name="prerequisites"></a>Prerequisiti

Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

*   Tenant di Azure Active Directory.
*   Un tenant valido per Salesforce Sandbox for Work o Salesforce Sandbox for Education. È possibile usare un account della versione di valutazione gratuita per entrambi i servizi.
*   Account utente in Salesforce Sandbox con autorizzazioni di amministratore di team.

## <a name="assigning-users-to-salesforce-sandbox"></a>Assegnazione di utenti a Salesforce Sandbox

Per determinare gli utenti che dovranno ricevere l'accesso alle app selezionate, Azure Active Directory usa il concetto delle "assegnazioni". Nel contesto del provisioning automatico degli account utente, vengono sincronizzati solo gli utenti e i gruppi che sono stati "assegnati" a un'applicazione in Azure AD.

Prima di configurare e abilitare il servizio di provisioning, è necessario stabilire quali utenti o gruppi in Azure AD devono accedere all'app Salesforce Sandbox. Dopo aver deciso, è possibile assegnare questi utenti all'app Salesforce Sandbox seguendo le istruzioni riportate in [Assegnare un utente o un gruppo a un'app aziendale](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-salesforce-sandbox"></a>Suggerimenti importanti per l'assegnazione di utenti a Salesforce Sandbox

* È consigliabile assegnare un singolo utente di Azure AD a Salesforce Sandbox per testare la configurazione del provisioning. È possibile assegnare utenti e/o gruppi aggiuntivi in un secondo momento.

* Quando si assegna un utente a Salesforce Sandbox, è necessario selezionare un ruolo utente valido. Il ruolo "Default Access" (Accesso predefinito) non è applicabile per il provisioning.

> [!NOTE]
> Questa app importa ruoli personalizzati da Salesforce Sandbox come parte del processo di provisioning che il cliente può decidere di selezionare durante l'assegnazione di utenti.

## <a name="enable-automated-user-provisioning"></a>Abilitare il provisioning utenti automatico

Questa sezione illustra la connessione di Azure AD all'API per il provisioning degli account utente di Salesforce Sandbox e la configurazione del servizio di provisioning per la creazione, l'aggiornamento e la disabilitazione degli account utente assegnati in Salesforce Sandbox in base all'assegnazione di utenti e gruppi in Azure AD.

>[!Tip]
>Si può anche scegliere di abilitare l'accesso Single Sign-On basato su SAML per Salesforce Sandbox, seguendo le istruzioni disponibili nel [portale di Azure](https://portal.azure.com). L'accesso Single Sign-On può essere configurato indipendentemente dal provisioning automatico, nonostante queste due funzionalità siano complementari.

### <a name="configure-automatic-user-account-provisioning"></a>Configurare il provisioning automatico degli account utente

In questa sezione viene descritto come abilitare il provisioning utente degli account utente di Active Directory in Salesforce Sandbox.

1. Nel [portale di Azure](https://portal.azure.com) passare alla sezione **Azure Active Directory > App aziendali > Tutte le applicazioni**.

1. Se è già stato configurato Salesforce Sandbox per l'accesso Single Sign-On, cercare l'istanza di Salesforce Sandbox usando il campo di ricerca. In caso contrario, selezionare **Aggiungi** e cercare **Salesforce Sandbox** nella raccolta di applicazioni. Selezionare Salesforce Sandbox nei risultati della ricerca e aggiungerlo all'elenco delle applicazioni.

1. Selezionare l'istanza di Salesforce Sandbox e quindi la scheda **Provisioning**.

1. Impostare **Modalità di provisioning** su **Automatico**.

    ![Screenshot che mostra la pagina di provisioning di Salesforce Sandbox, con l'opzione Modalità di provisioning impostata su Automatica e altri valori che è possibile impostare.](./media/salesforce-sandbox-provisioning-tutorial/provisioning.png)

1. Nella sezione **Credenziali di amministratore** specificare le impostazioni di configurazione seguenti:
   
    a. Nella casella di testo **Nome utente amministratore** digitare un nome account di Salesforce Sandbox con il profilo **Amministratore di sistema** assegnato in Salesforce.com.
   
    b. Nella casella di testo **Password amministratore** digitare la password per questo account.

1. Per ottenere il token di sicurezza di Salesforce Sandbox, aprire una nuova scheda e accedere allo stesso account amministratore di Salesforce Sandbox. Nell'angolo superiore destro della pagina fare clic sul proprio nome e quindi su **Impostazioni**.

     ![Screenshot che mostra il collegamento Impostazioni selezionato.](./media/salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "Abilita provisioning utenti automatico")

1. Nel pannello di navigazione sinistro fare clic su **My Personal Information** (Informazioni personali) per espandere la sezione corrispondente e quindi fare clic su **Reset My Security Token** (Reimposta token di sicurezza personale).
  
    ![Screenshot che mostra Reset My Security Token (Reimposta token di sicurezza personale) selezionato in Personal Information (Informazioni personali).](./media/salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "Abilita provisioning utenti automatico")

1. Nella pagina **Reset Security Token** (Reimposta token di sicurezza) fare clic sul pulsante **Reset Security Token** (Reimposta token di sicurezza).

    ![Screenshot che mostra la pagina Reset Security Token (Reimposta token di sicurezza), con testo esplicativo e l'opzione Reset Security Token (Reimposta token di sicurezza)](./media/salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "Abilita provisioning utenti automatico")

1. Controllare la casella di posta elettronica associata a questo account di amministratore. Cercare un messaggio di posta elettronica da Salesforce Sandbox.com contenente il nuovo token di sicurezza.

1. Copiare il token, passare alla finestra di Azure AD e incollarlo nel campo **Token segreto**.

1. Nel portale di Azure fare clic su **Test connessione** per verificare che Azure AD possa connettersi all'app Salesforce Sandbox.

1. Nel campo **Messaggio di posta elettronica di notifica** immettere l'indirizzo di posta elettronica di una persona o un gruppo che riceverà le notifiche di errore relative al provisioning e selezionare la casella di controllo.

1. Fare clic su **Salva**.  
    
1.  Nella sezione Mapping selezionare **Synchronize Azure Active Directory Users to Salesforce Sandbox** (Sincronizza utenti di Azure Active Directory in Salesforce Sandbox).

1. Nella sezione **Mapping degli attributi** esaminare gli attributi utente che vengono sincronizzati da Azure AD a Salesforce Sandbox. Gli attributi selezionati come proprietà **corrispondenti** vengono usati per trovare le corrispondenze con gli account utente in Salesforce Sandbox per le operazioni di aggiornamento. Selezionare il pulsante Salva per eseguire il commit delle modifiche.

1. Per abilitare il servizio di provisioning di Azure AD per Salesforce Sandbox, impostare **Stato del provisioning** su **Sì** nella sezione Impostazioni

1. Fare clic su **Salva**.

Viene avviata la sincronizzazione iniziale di tutti gli utenti e/o i gruppi assegnati a Salesforce Sandbox nella sezione Utenti e gruppi. La sincronizzazione iniziale richiede più tempo delle sincronizzazioni successive, che saranno eseguite circa ogni 40 minuti per tutto il tempo che il servizio è in esecuzione. È possibile usare la sezione **Dettagli sincronizzazione** per monitorare lo stato di avanzamento e selezionare i collegamenti ai log delle attività di provisioning, che descrivono tutte le azioni eseguite dal servizio di provisioning sull'app Salesforce Sandbox.

Per altre informazioni sulla lettura dei log di provisioning di Azure AD, vedere l'esercitazione relativa alla [creazione di report sul provisioning automatico degli account utente](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Gestione del provisioning degli account utente per app aziendali](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)
* [Configurare l'accesso Single Sign-On](./salesforce-sandbox-tutorial.md)