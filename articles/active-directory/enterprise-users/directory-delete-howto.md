---
title: Eliminare un'organizzazione di Azure AD (tenant) - Azure Active Directory | Microsoft Docs
description: Illustra come preparare un'organizzazione di Azure AD (tenant) per l'eliminazione, incluse le organizzazioni self-service
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.topic: how-to
ms.date: 12/02/2020
ms.author: curtand
ms.reviewer: addimitu
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2edc6fb98359c5360836bc369e5ae1928464df92
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/08/2020
ms.locfileid: "96861031"
---
# <a name="delete-a-tenant-in-azure-active-directory"></a>Eliminare un tenant in Azure Active Directory

Quando viene eliminata un'organizzazione di Azure AD (tenant), vengono eliminate anche tutte le risorse contenute al suo interno. Preparare l'organizzazione riducendo al minimo le risorse associate prima di eliminarla. Solo un amministratore globale di Azure Active Directory (Azure AD) può eliminare un'organizzazione di Azure AD dal portale.

## <a name="prepare-the-organization"></a>Preparare l'organizzazione

Non è possibile eliminare un'organizzazione di Azure AD fino a quando non supera numerosi controlli. Questi controlli riducono il rischio che l'eliminazione di un Azure AD organizzazione influisca negativamente sull'accesso degli utenti, ad esempio la possibilità di accedere a Microsoft 365 o accedere alle risorse in Azure. Se l'organizzazione associata a una sottoscrizione viene eliminata accidentalmente, ad esempio, gli utenti non potranno accedere alle risorse di Azure per tale sottoscrizione. Viene verificato che siano soddisfatte le condizioni seguenti:

* Nell'organizzazione (tenant) di Azure AD non deve essere presente alcun utente, ad eccezione dell'amministratore globale che deve eliminare l'organizzazione. Per poter eliminare l'organizzazione, è prima necessario eliminare tutti gli altri utenti. Se gli utenti vengono sincronizzati dall'ambiente locale, è prima necessario disattivare la sincronizzazione ed eliminare gli utenti nell'organizzazione cloud tramite il portale di Azure o i cmdlet di Azure PowerShell.
* L'organizzazione non può contenere applicazioni. Per poter eliminare l'organizzazione, è prima necessario rimuovere tutte le applicazioni.
* All'organizzazione non possono essere collegati provider di autenticazione a più fattori.
* Non possono essere presenti sottoscrizioni per i Microsoft Online Services, ad esempio Microsoft Azure, Microsoft 365 o Azure AD Premium associate all'organizzazione. Se, ad esempio, in Azure è stata creata automaticamente un'organizzazione di Azure AD predefinita, non è possibile eliminare questa organizzazione se la sottoscrizione di Azure si basa ancora su di essa per l'autenticazione. Analogamente, non è possibile eliminare un'organizzazione se la sottoscrizione di un altro utente è associata a tale organizzazione.

## <a name="delete-the-organization"></a>Eliminare l'organizzazione

1. Accedere all'[interfaccia di amministrazione di Azure AD](https://aad.portal.azure.com) con un account di amministratore globale per l'organizzazione.

2. Selezionare **Azure Active Directory**.

3. Passare all'organizzazione da eliminare.
  
   ![Confermare l'organizzazione prima dell'eliminazione](./media/directory-delete-howto/delete-directory-command.png)

4. Selezionare **Elimina tenant**.
  
   ![selezionare il comando per eliminare l'organizzazione](./media/directory-delete-howto/delete-directory-list.png)

5. Se l'organizzazione non supera uno o più controlli, viene fornito un collegamento per ottenere altre informazioni su come superarli. Dopo aver superato tutti i controlli, selezionare **Elimina** per completare il processo.

## <a name="if-you-cant-delete-the-organization"></a>Se non è possibile eliminare l'organizzazione

Quando è stata configurata la Azure AD organizzazione, è possibile che siano state attivate anche le sottoscrizioni basate su licenza per l'organizzazione, ad esempio Azure AD Premium P2, Microsoft 365 Business standard o Enterprise Mobility + Security E5. Per evitare la perdita accidentale di dati, non è possibile eliminare un'organizzazione fino a quando le sottoscrizioni non sono state completamente eliminate. Per consentire l'eliminazione dell'organizzazione, lo stato delle sottoscrizioni deve essere **Deprovisioning eseguito**. Una sottoscrizione **Scaduta** o **Annullata** passa allo stato **Disabilitato** e la fase finale è lo stato **Deprovisioning eseguito**.

Per sapere cosa accade quando una sottoscrizione di Microsoft 365 di valutazione scade (escluso partner a pagamento/CSP, Enterprise Agreement o contratti multilicenza), vedere la tabella seguente. Per ulteriori informazioni sul ciclo di vita delle sottoscrizioni e sulla conservazione dei dati Microsoft 365, vedere [cosa accade ai dati e all'accesso quando termina la sottoscrizione di Microsoft 365 for business?](https://support.office.com/article/what-happens-to-my-data-and-access-when-my-office-365-for-business-subscription-ends-4436582f-211a-45ec-b72e-33647f97d8a3). 

Stato sottoscrizione | Data | Accesso ai dati
----- | ----- | -----
Attivo (30 giorni per la versione di prova gratuita) | Dati accessibili a tutti | Gli utenti hanno accesso normale a file Microsoft 365 o app<br>Gli amministratori hanno accesso normale all'interfaccia di amministrazione di Microsoft 365 e alle risorse 
Scaduto (30 giorni) | Dati accessibili a tutti| Gli utenti hanno accesso normale a file Microsoft 365 o app<br>Gli amministratori hanno accesso normale all'interfaccia di amministrazione di Microsoft 365 e alle risorse
Disattivato (30 giorni) | Dati accessibili solo all'amministratore | Gli utenti non possono accedere ai file di Microsoft 365 o alle app<br>Gli amministratori possono accedere all'interfaccia di amministrazione di Microsoft 365, ma non possono assegnare licenze o aggiornare gli utenti
Deprovisioning eseguito (30 giorni dopo la disattivazione) | Dati eliminati (eliminati automaticamente se nessun altro servizio è in funzione) | Gli utenti non possono accedere ai file di Microsoft 365 o alle app<br>Gli amministratori possono accedere all'interfaccia di amministrazione di Microsoft 365 per acquistare e gestire altre sottoscrizioni

## <a name="delete-a-subscription"></a>Eliminare una sottoscrizione

È possibile impostare una sottoscrizione sullo stato **Deprovisoning eseguito** per essere eliminata entro 3 giorni tramite l'interfaccia di amministrazione di Microsoft 365.

1. Accedere all'[interfaccia di amministrazione di Microsoft 365](https://admin.microsoft.com) con un account di amministratore globale nell'organizzazione. Se si sta tentando di eliminare l'organizzazione "Contoso" con il dominio predefinito iniziale contoso.onmicrosoft.com, accedere con un nome dell'entità utente, ad esempio admin@contoso.onmicrosoft.com.

2. Visualizzare l'anteprima della nuova interfaccia di amministrazione di Microsoft 365 assicurandosi che l'interruttore **Prova la nuova interfaccia di amministrazione** sia abilitato.

   ![Anteprima della nuova esperienza dell'interfaccia di amministrazione di Microsoft 365](./media/directory-delete-howto/preview-toggle.png)

3. Dopo aver abilitato la nuova interfaccia di amministrazione, è necessario annullare una sottoscrizione prima di poterla eliminare. Selezionare **Fatturazione**, quindi **Prodotti e servizi** e infine fare clic su **Annulla sottoscrizione** per la sottoscrizione che si vuole annullare. Verrà visualizzata una pagina di feedback.

   ![Scegliere una sottoscrizione da annullare](./media/directory-delete-howto/cancel-choose-subscription.png)

4. Completare il modulo di feedback e selezionare **Annulla sottoscrizione** per annullare la sottoscrizione.

   ![Comando Annulla nell'anteprima della sottoscrizione](./media/directory-delete-howto/cancel-command.png)

5. A questo punto è possibile eliminare la sottoscrizione. Selezionare **Elimina** per la sottoscrizione che si vuole eliminare. Se non è possibile trovare la sottoscrizione nella pagina **Prodotti e servizi**, assicurarsi che **Stato sottoscrizione** sia impostato su **Tutti**.

   ![Eliminare il collegamento per l'eliminazione della sottoscrizione](./media/directory-delete-howto/delete-command.png)

6. Selezionare **Elimina sottoscrizione** per eliminare la sottoscrizione e accettare le condizioni. Tutti i dati verranno definitivamente eliminati entro tre giorni. Se si cambia idea, è possibile [riattivare la sottoscrizione](/office365/admin/subscriptions-and-billing/reactivate-your-subscription) entro un periodo di tre giorni.
  
   ![leggere attentamente le condizioni](./media/directory-delete-howto/delete-terms.png)

7. A questo punto lo stato della sottoscrizione è cambiato e la sottoscrizione è contrassegnata per l'eliminazione. La sottoscrizione passa allo stato **Deprovisioning eseguito** 72 ore dopo.

8. Trascorse 72 ore dall'eliminazione della sottoscrizione nell'organizzazione, è possibile effettuare nuovamente l'accesso all'interfaccia di amministrazione di Azure AD: non dovrebbe essere richiesto alcun intervento da parte dell'utente e non dovrebbero essere presenti sottoscrizioni che bloccano l'eliminazione dell'organizzazione. Sarà quindi possibile procedere alla corretta eliminazione dell'organizzazione di Azure AD.
  
   ![superare il controllo della sottoscrizione nella schermata di eliminazione](./media/directory-delete-howto/delete-checks-passed.png)

## <a name="i-have-a-trial-subscription-that-blocks-deletion"></a>Ho una sottoscrizione di valutazione che blocca l'eliminazione

Sono disponibili [prodotti di iscrizione self-service](/office365/admin/misc/self-service-sign-up) come Microsoft Power BI, Rights Management Services, Microsoft Power Apps o Dynamics 365, i singoli utenti possono iscriversi tramite Microsoft 365, che crea anche un utente Guest per l'autenticazione nell'organizzazione Azure ad. Questi prodotti self-service bloccano le eliminazioni di directory fino a quando i prodotti non vengono eliminati completamente dall'organizzazione, per evitare la perdita di dati. Possono essere eliminati solo dall'amministratore di Azure AD, indipendentemente dal fatto che l'utente abbia effettuato l'iscrizione singolarmente o che sia stato assegnato al prodotto.

Esistono due tipi di prodotti con iscrizione self-service con modalità di assegnazione diverse: 

* Assegnazione a livello di organizzazione: Un amministratore di Azure AD assegna il prodotto all'intera organizzazione e un utente può usare attivamente il servizio con questa assegnazione a livello di organizzazione anche se non gli è stata concessa una licenza individuale.
* Assegnazione a livello di utente: Un utente singolo durante l'iscrizione self-service assegna il prodotto a se stesso senza un amministratore. Quando l'organizzazione diventa gestita da un amministratore (vedere [Acquisizione di un'organizzazione non gestita da parte di un amministratore](domains-admin-takeover.md)), l'amministratore può assegnare direttamente il prodotto agli utenti senza l'iscrizione self-service.  

Quando si avvia l'eliminazione del prodotto con iscrizione self-service, l'azione elimina definitivamente i dati e rimuove tutti gli accessi utente al servizio. A tutti gli utenti a cui è stata assegnata l'offerta singolarmente o a livello di organizzazione viene quindi impedito l'accesso al servizio e a tutti i dati esistenti. Per impedire la perdita di dati per il prodotto con iscrizione self-service, ad esempio i [dashboard di Microsoft Power BI](/power-bi/service-export-to-pbix) o la [configurazione dei criteri di Rights Management Services](/azure/information-protection/configure-policy#how-to-configure-the-azure-information-protection-policy), assicurarsi che i dati vengano sottoposti a backup e salvati altrove.

Per ulteriori informazioni sui prodotti e i servizi con iscrizione self-service attualmente disponibili, vedere [Programmi self-service disponibili](/office365/admin/misc/self-service-sign-up#available-self-service-programs).

Per sapere cosa accade quando una sottoscrizione di Microsoft 365 di valutazione scade (escluso partner a pagamento/CSP, Enterprise Agreement o contratti multilicenza), vedere la tabella seguente. Per ulteriori informazioni sul ciclo di vita delle sottoscrizioni e sulla conservazione dei dati Microsoft 365, vedere [cosa accade ai dati e all'accesso quando termina la sottoscrizione di Microsoft 365 for business?](/office365/admin/subscriptions-and-billing/what-if-my-subscription-expires).

Stato del prodotto | Data | Accesso ai dati
------------- | ---- | --------------
Attivo (30 giorni per la versione di prova gratuita) | Dati accessibili a tutti | Gli utenti hanno accesso normale al prodotto con iscrizione self-service, ai file o alle app<br>Gli amministratori hanno accesso normale all'interfaccia di amministrazione di Microsoft 365 e alle risorse
Deleted | Dati eliminati | Gli utenti non possono accedere al prodotto con iscrizione self-service, ai file o alle app<br>Gli amministratori possono accedere all'interfaccia di amministrazione di Microsoft 365 per acquistare e gestire altre sottoscrizioni

## <a name="how-can-i-delete-a-self-service-sign-up-product-in-the-azure-portal"></a>Come è possibile eliminare un prodotto con iscrizione self-service nel portale di Azure?

È possibile impostare un prodotto con iscrizione self-service, ad esempio Microsoft Power BI o Azure Rights Management Services su uno stato di **eliminazione** per essere eliminato immediatamente nel portale di Azure AD.

1. Accedere all'[interfaccia di amministrazione di Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) con un account di amministratore globale nell'organizzazione. Se si sta tentando di eliminare l'organizzazione "Contoso" con il dominio predefinito iniziale contoso.onmicrosoft.com, accedere con un nome dell'entità utente, ad esempio admin@contoso.onmicrosoft.com.

2. Selezionare **Licenze**, quindi selezionare **Self-service sign-up products** (Prodotti con iscrizione self-service). È possibile visualizzare tutti i prodotti con iscrizione self-service separatamente dalle sottoscrizioni basate su postazione. Scegliere il prodotto da eliminare definitivamente. Di seguito è riportato un esempio in Microsoft Power BI:

    ![Screenshot che mostra la pagina "licenze-self-service per l'iscrizione ai prodotti".](./media/directory-delete-howto/licenses-page.png)

3. Selezionare **Elimina** per eliminare il prodotto e accettare le condizioni che spiegano che i dati verranno eliminati immediatamente e irrevocabilmente. Questa azione di eliminazione rimuove tutti gli utenti e rimuove l'accesso dell'organizzazione al prodotto. Fare clic su Sì per procedere con l'eliminazione.  

    ![Screenshot che mostra la pagina "licenze-self-service per l'iscrizione dei prodotti" con la finestra "Elimina il prodotto di iscrizione self-service" aperta.](./media/directory-delete-howto/delete-product.png)

4. Quando si seleziona **Sì**, viene avviata l'eliminazione del prodotto self-service. Verrà visualizzata una notifica che indica che l'eliminazione è in corso.  

    ![Screenshot che mostra la pagina "licenze-self-service per l'iscrizione dei prodotti" con la notifica "eliminazione in corso" visualizzata.](./media/directory-delete-howto/progress-message.png)

5. A questo punto, lo stato del prodotto con iscrizione self-service è passato a **Eliminato**. Quando si aggiorna la pagina, il prodotto non sarà più presente nella pagina **Self-service sign-up products** (Prodotti con iscrizione self-service).  

    ![Screenshot che mostra la pagina "licenze-self-service per l'iscrizione dei prodotti" con il riquadro "prodotto di iscrizione self-service eliminato" sul lato destro.](./media/directory-delete-howto/product-deleted.png)

6. Dopo aver eliminato tutti i prodotti, è possibile effettuare nuovamente l'accesso all'interfaccia di amministrazione di Azure AD: non dovrebbe essere richiesto alcun intervento da parte dell'utente e non dovrebbero essere presenti prodotti che bloccano l'eliminazione dell'organizzazione. Sarà quindi possibile procedere alla corretta eliminazione dell'organizzazione di Azure AD.

    ![il nome utente non è corretto o non è stato trovato](./media/directory-delete-howto/delete-organization.png)

## <a name="next-steps"></a>Passaggi successivi

[Documentazione di Azure Active Directory](../index.yml)
