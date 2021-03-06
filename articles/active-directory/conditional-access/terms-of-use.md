---
title: Condizioni per l'utilizzo Azure Active Directory | Microsoft Docs
description: Iniziare a usare Azure Active Directory condizioni per l'utilizzo per presentare informazioni ai dipendenti o ai guest prima di ottenere l'accesso.
services: active-directory
ms.service: active-directory
ms.subservice: compliance
ms.topic: how-to
ms.date: 01/27/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95fe70c774b933113c94125d227976e32a9e353f
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98919630"
---
# <a name="azure-active-directory-terms-of-use"></a>Azure Active Directory le condizioni per l'utilizzo

Azure AD criteri per le condizioni per l'utilizzo forniscono un metodo semplice che le organizzazioni possono utilizzare per presentare informazioni agli utenti finali. In questo modo si garantisce che gli utenti vedano le dichiarazioni rilevanti di non responsabilità che si riferiscono ai requisiti legali o di conformità. Questo articolo descrive come iniziare a usare i criteri di condizioni per l'utilizzo (ToU).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview-videos"></a>Panoramica video

Il video seguente offre una rapida panoramica dei criteri di ToU.

>[!VIDEO https://www.youtube.com/embed/tj-LK0abNao]

Per altri video, vedere:
- [Come distribuire un criterio di condizioni per l'utilizzo in Azure Active Directory](https://www.youtube.com/embed/N4vgqHO2tgY)
- [Come implementare un criterio di condizioni per l'utilizzo in Azure Active Directory](https://www.youtube.com/embed/t_hA4y9luCY)

## <a name="what-can-i-do-with-terms-of-use"></a>Cosa è possibile fare con le condizioni per l'utilizzo?

Azure AD criteri per le condizioni per l'utilizzo hanno le funzionalità seguenti:

- Chiedere ai dipendenti o ai guest di accettare i criteri di utilizzo prima di ottenere l'accesso.
- Chiedere ai dipendenti o ai guest di accettare le condizioni per l'utilizzo in ogni dispositivo prima di ottenere l'accesso.
- Richiedere ai dipendenti o ai guest di accettare le condizioni per l'utilizzo in base a una pianificazione ricorrente.
- Richiedere ai dipendenti o ai guest di accettare i criteri di utilizzo prima di registrare le informazioni di sicurezza in Azure AD Multi-Factor Authentication (multi-factor authentication).
- Richiedere ai dipendenti di accettare i criteri di utilizzo prima di registrare le informazioni di sicurezza in Azure AD la reimpostazione della password self-service (SSPR).
- Presentare un criterio per le condizioni per l'utilizzo generale per tutti gli utenti dell'organizzazione.
- Presentare condizioni specifiche per l'utilizzo dei criteri in base a attributi utente (ad esempio medici o infermieri oppure dipendenti nazionali o internazionali, tramite [gruppi dinamici](../enterprise-users/groups-dynamic-membership.md)).
- Presentare condizioni di utilizzo specifiche per l'accesso ad applicazioni a elevato utilizzo aziendale, ad esempio Salesforce.
- Presentare le condizioni per l'utilizzo dei criteri in lingue diverse.
- Elenco che ha o non ha accettato le condizioni per l'utilizzo dei criteri.
- Contribuire a rispettare le normative sulla privacy.
- Consente di visualizzare un log delle condizioni per l'utilizzo dei criteri per la conformità e il controllo.
- Creare e gestire i criteri di utilizzo usando le [api Microsoft Graph](/graph/api/resources/agreement?view=graph-rest-beta) (attualmente in anteprima).

## <a name="prerequisites"></a>Prerequisiti

Per usare e configurare Azure AD le condizioni per l'utilizzo dei criteri, è necessario disporre di:

- Una sottoscrizione di Azure AD Premium P1, P2, EMS E3 o EMS E5.
   - Se non si dispone di una di queste sottoscrizioni, è possibile [ottenere Azure AD Premium](../fundamentals/active-directory-get-started-premium.md) oppure [abilitare la versione di valutazione di Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).
- Uno dei seguenti account di amministratore per la directory da configurare:
   - Amministratore globale
   - Amministratore della protezione
   - Amministratore accesso condizionale

## <a name="terms-of-use-document"></a>Documento sulle condizioni per l'utilizzo

Azure AD le condizioni per l'utilizzo dei criteri usano il formato PDF per presentare il contenuto. Il contenuto del file PDF può essere di qualsiasi tipo, ad esempio documenti di contratti esistenti. Questo consente di acquisire il consenso degli utenti finali durante l'accesso. Per supportare gli utenti sui dispositivi mobili, le dimensioni del carattere consigliato nel file PDF sono 24 punti.

## <a name="add-terms-of-use"></a>Aggiungere le condizioni per l'utilizzo

Dopo aver completato il documento dei criteri di utilizzo, attenersi alla procedura seguente per aggiungerlo.

1. Accedere ad Azure come amministratore globale, amministratore della sicurezza o amministratore di accesso condizionale.
1. Passare a **Condizioni per l'utilizzo** all'indirizzo [https://aka.ms/catou](https://aka.ms/catou).

    ![Accesso condizionale-pannello Condizioni per l'utilizzo](./media/terms-of-use/tou-blade.png)

1. Fare clic su **Nuove condizioni**.

    ![Nuovo termine del riquadro utilizzo per specificare le impostazioni delle condizioni per l'utilizzo](./media/terms-of-use/new-tou.png)

1. Nella casella **nome** immettere un nome per i criteri per le condizioni di utilizzo che verranno usati nella portale di Azure.
1. Nella casella **Nome visualizzato** immettere un titolo visualizzato dagli utenti quando eseguono l'accesso.
1. Per **condizioni per l'utilizzo documento**, passare al formato PDF per le condizioni per l'utilizzo finalizzato e selezionarlo.
1. Selezionare la lingua per il documento dei criteri di utilizzo. L'opzione lingua consente di caricare più condizioni per l'utilizzo, ognuna con una lingua diversa. La versione dei criteri di utilizzo che verrà visualizzato da un utente finale sarà basata sulle preferenze del browser.
1. Per richiedere agli utenti finali di visualizzare le condizioni per l'utilizzo prima di accettarle, impostare **Richiedi agli utenti di espandere le condizioni per l'utilizzo** **su on**.
1. Per richiedere agli utenti finali di accettare i criteri di utilizzo per ogni dispositivo da cui accedono, impostare **Richiedi agli utenti di fornire il consenso su ogni dispositivo** **.** Se questa opzione è abilitata, agli utenti potrebbe essere richiesto di installare applicazioni aggiuntive. Per altre informazioni, vedere [condizioni per l'utilizzo per ogni dispositivo](#per-device-terms-of-use).
1. Se si desidera impostare come scaduti i criteri per le condizioni per l'utilizzo in base a una pianificazione, impostare la **scadenza consentita** su **on**. Se si imposta su On, vengono visualizzate due impostazioni aggiuntive di pianificazione.

    ![Impostazioni di scadenza consentite per impostare la data di inizio, la frequenza e la durata](./media/terms-of-use/expire-consents.png)

1. Utilizzare le impostazioni **scadenza inizio** e **frequenza** per specificare la pianificazione per le scadenze dei criteri di utilizzo. La tabella seguente illustra il risultato di un paio di impostazioni di esempio:

   | Scadenza a partire da | Frequenza | Risultato |
   | --- | --- | --- |
   | Data odierna  | Ogni mese | A partire da oggi, gli utenti devono accettare le condizioni per l'utilizzo e quindi riaccettare ogni mese. |
   | Data nel futuro  | Ogni mese | A partire da oggi, gli utenti devono accettare i criteri per le condizioni d'uso. Quando si verifica la data futura, scadono i consensi e quindi gli utenti devono riaccettare ogni mese.  |

   Ad esempio, se si imposta la scadenza a partire dalla data **1 gen** e la frequenza su **Mensile**, ecco come le scadenze potrebbero verificarsi per i due utenti:

   | User | Prima data di accettazione | Prima data di scadenza | Seconda data di scadenza | Terza data di scadenza |
   | --- | --- | --- | --- | --- |
   | Alice | 1 gen | 1 feb | 1 mar | 1 apr |
   | Bob | 15 gen | 1 feb | 1 mar | 1 apr |

1. Per specificare il numero di giorni prima che l'utente debba riaccettare i criteri di condizioni per l'utilizzo, utilizzare l'impostazione **durata prima della riaccettazione richiede (giorni)** . Ciò consente agli utenti di seguire una pianificazione personalizzata. Ad esempio, se si imposta la durata su **30** giorni, ecco come le scadenze potrebbero verificarsi per due utenti:

   | User | Prima data di accettazione | Prima data di scadenza | Seconda data di scadenza | Terza data di scadenza |
   | --- | --- | --- | --- | --- |
   | Alice | 1 gen | 31 gen | 2 mar | 1 apr |
   | Bob | 15 gen | 14 feb | 16 mar | 15 apr |

   È possibile utilizzare i **consensi di scadenza** e la **durata prima che riaccettino le impostazioni (giorni)** , ma in genere si utilizza una o l'altra.

1. In **accesso condizionale** usare l'elenco **applica con i modelli di criteri di accesso condizionale** per selezionare il modello per applicare i criteri di condizioni per l'utilizzo.

    ![Elenco a discesa accesso condizionale per selezionare un modello di criteri](./media/terms-of-use/conditional-access-templates.png)

   | Modello | Descrizione |
   | --- | --- |
   | **Accesso alle app cloud per tutti i guest** | Verranno creati criteri di accesso condizionale per tutti i guest e tutte le app cloud. Questi criteri influiscono sul portale di Azure. Al termine della creazione, potrebbe essere necessario disconnettersi ed eseguire l'accesso. |
   | **Accesso alle app cloud per tutti gli utenti** | Verranno creati criteri di accesso condizionale per tutti gli utenti e tutte le app cloud. Questi criteri influiscono sul portale di Azure. Una volta creato, sarà necessario disconnettersi ed eseguire l'accesso. |
   | **Criteri personalizzati** | Selezionare gli utenti, i gruppi e le app a cui verranno applicati i criteri per le condizioni d'uso. |
   | **Crea criteri di accesso condizionale in un secondo momento** | Questo criterio di condizioni per l'utilizzo verrà visualizzato nell'elenco di controllo di concessione quando si crea un criterio di accesso condizionale. |

   >[!IMPORTANT]
   >I controlli dei criteri di accesso condizionale (incluse le condizioni per l'utilizzo dei criteri) non supportano l'imposizione degli account del servizio. È consigliabile escludere tutti gli account di servizio dai criteri di accesso condizionale.

    I criteri di accesso condizionale personalizzati abilitano i criteri granulari per l'utilizzo, fino a una specifica applicazione cloud o a un gruppo di utenti. Per altre informazioni, vedere [Guida introduttiva: richiedere che le condizioni per l'utilizzo vengano accettate prima di accedere alle app Cloud](require-tou.md).

1. Fare clic su **Crea**.

    Se è stato selezionato un modello di accesso condizionale personalizzato, viene visualizzata una nuova schermata che consente di creare i criteri di accesso condizionale personalizzato.

   ![Nuovo riquadro accesso condizionale se è stato scelto il modello di criteri di accesso condizionale personalizzato](./media/terms-of-use/custom-policy.png)

   Verranno ora visualizzate le nuove condizioni per l'utilizzo dei criteri.

   ![Nuove condizioni per l'utilizzo elencate nel pannello condizioni per l'utilizzo](./media/terms-of-use/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Visualizzare il report degli utenti che hanno accettato e rifiutato

Nel pannello delle condizioni per l'utilizzo è visualizzato il numero di utenti che hanno accettato e rifiutato. Questi conteggi e i destinatari accettati/rifiutati vengono archiviati per la durata dei criteri di utilizzo.

1. Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo [https://aka.ms/catou](https://aka.ms/catou).

    ![Pannello Condizioni per l'utilizzo che elenca il numero di show utente accettati e rifiutati](./media/terms-of-use/view-tou.png)

1. Per i criteri di utilizzo, fare clic sui numeri in **accettato** o **rifiutato** per visualizzare lo stato corrente degli utenti.

    ![Condizioni per l'utilizzo riquadro consentiti in cui sono elencati gli utenti che hanno accettato](./media/terms-of-use/accepted-tou.png)

1. Per visualizzare la cronologia per un singolo utente, fare clic sui puntini di sospensione (**...**) e quindi **Visualizza cronologia**.

    ![Menu di scelta rapida Visualizza cronologia per un utente](./media/terms-of-use/view-history-menu.png)

   Nel riquadro di visualizzazione della cronologia, si visualizza una cronologia di tutte le accettazioni, i rifiuti e le scadenze.

   ![Visualizza riquadro cronologia elenca la cronologia accetta, declina e scade per un utente](./media/terms-of-use/view-history-pane.png)

## <a name="view-azure-ad-audit-logs"></a>Visualizzare i log di controllo di Azure AD

Se si desidera visualizzare ulteriori attività, Azure AD i criteri di condizioni per l'utilizzo includono i log di controllo. Ogni consenso dell'utente genera un evento nei log di controllo che viene conservato per **30 giorni**. È possibile visualizzare questi log nel portale o scaricarli come file CSV.

Per iniziare a usare i log di controllo di Azure AD, seguire questa procedura:

1. Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo [https://aka.ms/catou](https://aka.ms/catou).
1. Selezionare un criterio di condizioni per l'utilizzo.
1. Fare clic su **Visualizza log di controllo**.

    ![Pannello Condizioni per l'utilizzo con l'opzione Visualizza log di controllo evidenziata](./media/terms-of-use/audit-tou.png)

1. Nella schermata dei log di controllo di Azure AD è possibile filtrare le informazioni usando i menu a discesa per visualizzare informazioni specifiche dei log di controllo.

    È inoltre possibile fare clic su **Download** per scaricare le informazioni in un file con estensione CSV per l'uso in locale.

   ![Schermata dei log di controllo Azure AD elenco di date, criteri di destinazione, avviati da e attività](./media/terms-of-use/audit-logs-tou.png)

   Se si fa clic su un log, viene visualizzato un riquadro con i dettagli delle attività aggiuntive.

   ![Dettagli attività per un log che mostra l'attività, lo stato dell'attività, avviato da, i criteri di destinazione](./media/terms-of-use/audit-log-activity-details.png)

## <a name="what-terms-of-use-looks-like-for-users"></a>Quali sono le condizioni per l'utilizzo per gli utenti

Dopo la creazione e l'applicazione di criteri ToU, gli utenti, che rientrano nell'ambito, visualizzeranno la schermata seguente durante l'accesso.

![Condizioni d'uso di esempio visualizzate quando un utente accede](./media/terms-of-use/user-tou.png)

Gli utenti possono visualizzare le condizioni per l'utilizzo dei criteri e, se necessario, utilizzare i pulsanti per eseguire lo zoom avanti e indietro.

![Visualizzazione delle condizioni per l'utilizzo con i pulsanti zoom](./media/terms-of-use/zoom-buttons.png)

La schermata seguente illustra il modo in cui un criterio ToU Cerca i dispositivi mobili.

![Condizioni d'uso di esempio visualizzate quando un utente accede a un dispositivo mobile](./media/terms-of-use/mobile-tou.png)

Gli utenti devono accettare le condizioni per l'utilizzo una sola volta e non vedranno più le condizioni per l'utilizzo dei criteri per gli accessi successivi.

### <a name="how-users-can-review-their-terms-of-use"></a>Come gli utenti possono esaminare le condizioni per l'utilizzo

Gli utenti possono esaminare e vedere le condizioni per l'utilizzo dei criteri accettati usando la procedura seguente.

1. Accedere a [https://myapps.microsoft.com](https://myapps.microsoft.com).
1. Nell'angolo superiore destro fare clic sul proprio nome e selezionare **Profilo**.

    ![Sito app con il riquadro dell'utente aperto](./media/terms-of-use/tou14.png)

1. Nella pagina Profilo fare clic su **Verificare le condizioni d'uso**.

    ![Pagina del profilo per un utente che Visualizza il collegamento controlla le condizioni per l'utilizzo](./media/terms-of-use/tou13a.png)

1. Da qui è possibile esaminare le condizioni per l'utilizzo dei criteri accettati.

## <a name="edit-terms-of-use-details"></a>Modificare i dettagli delle condizioni per l'utilizzo

È possibile modificare alcuni dettagli delle condizioni per l'utilizzo dei criteri, ma non è possibile modificare un documento esistente. La procedura seguente descrive come modificare i dettagli.

1. Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo [https://aka.ms/catou](https://aka.ms/catou).
1. Selezionare le condizioni per l'utilizzo che si desidera modificare.
1. Fare clic su **Modifica le condizioni**.
1. Nel riquadro Modifica condizioni per l'utilizzo è possibile modificare gli elementi seguenti:
    - **Nome** : nome interno delle condizioni non condivise con gli utenti finali
    - **Nome visualizzato** : questo è il nome che gli utenti finali possono visualizzare quando si visualizzano le condizioni
    - **Richiedi agli utenti di espandere le condizioni** per l'utilizzo. Se si imposta questa opzione **su on** , l'utilizzo finale verrà forzato per espandere il documento dei criteri di utilizzo prima di accettarlo.
    - Anteprima È possibile **aggiornare un documento sulle condizioni per l'utilizzo esistente**
    - È possibile aggiungere una lingua a una ToU esistente

   Se sono presenti altre impostazioni che si desidera modificare, ad esempio il documento PDF, richiedere agli utenti di fornire il consenso su ogni dispositivo, scadenza dei consensi, durata prima della riaccettazione o criteri di accesso condizionale, è necessario creare un nuovo criterio ToU.

    ![Modificare la visualizzazione di opzioni di lingua diverse ](./media/terms-of-use/edit-terms-use.png)

1. Al termine, fare clic su **Salva** per salvare le modifiche.

## <a name="update-the-version-or-pdf-of-an-existing-terms-of-use"></a>Aggiornare la versione o il PDF delle condizioni per l'utilizzo esistenti

1.  Accedere ad Azure e passare a [condizioni per l'utilizzo](https://aka.ms/catou)
2.  Selezionare le condizioni per l'utilizzo che si desidera modificare.
3.  Fare clic su **Modifica le condizioni**.
4.  Per la lingua che si desidera aggiornare una nuova versione, fare clic su **Aggiorna** nella colonna azione

    ![Modificare il riquadro Condizioni per l'utilizzo mostrando le opzioni nome ed Espandi](./media/terms-of-use/edit-terms-use.png)

5.  Nel riquadro a destra caricare il file PDF per la nuova versione
6.  Qui è disponibile anche un'opzione di attivazione/disconnessione **, se si desidera richiedere agli** utenti di accettare la nuova versione al successivo accesso. Se è necessario che gli utenti riaccettino, la volta successiva che tentano di accedere alla risorsa definita nei criteri di accesso condizionale verrà richiesto di accettare la nuova versione. Se non è necessario che gli utenti riaccettino, il consenso precedente resterà aggiornato e solo i nuovi utenti che non hanno acconsentito prima o il cui consenso scade vedranno la nuova versione.

    ![Modifica l'opzione di riaccettazione delle condizioni per l'utilizzo evidenziata](./media/terms-of-use/re-accept.png)

7.  Dopo aver caricato il nuovo PDF e aver deciso di riaccettarlo, fare clic su Aggiungi nella parte inferiore del riquadro.
8.  Verrà ora visualizzata la versione più recente sotto la colonna documento.

## <a name="view-previous-versions-of-a-tou"></a>Visualizzare le versioni precedenti di una ToU

1.  Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo https://aka.ms/catou.
2.  Selezionare i criteri per le condizioni per l'utilizzo per i quali si desidera visualizzare la cronologia delle versioni.
3.  Fare clic su **lingue e cronologia versioni**
4.  Fare clic su **vedere versioni precedenti.**

    ![dettagli del documento, incluse le versioni della lingua](./media/terms-of-use/document-details.png)

5.  È possibile fare clic sul nome del documento per scaricare la versione

## <a name="see-who-has-accepted-each-version"></a>Vedere chi ha accettato ogni versione

1.  Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo https://aka.ms/catou.
2.  Per visualizzare gli utenti che hanno attualmente accettato le condizioni, fare clic sul numero nella colonna **accettato** per le condizioni desiderate.
3.  Per impostazione predefinita, nella pagina successiva verrà visualizzato lo stato corrente di ogni utente che accetta le condizioni
4.  Se si desidera visualizzare gli eventi di consenso precedenti, è possibile selezionare **tutto** dall'elenco a discesa **stato corrente** . Ora è possibile visualizzare tutti gli eventi degli utenti in dettagli su ogni versione e cosa è successo.
5.  In alternativa, è possibile selezionare una versione specifica dall'elenco a discesa **versione**  per vedere chi ha accettato la versione specifica.


## <a name="add-a-tou-language"></a>Aggiungi un linguaggio ToU

Nella procedura riportata di seguito viene descritto come aggiungere un linguaggio ToU.

1. Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo [https://aka.ms/catou](https://aka.ms/catou).
1. Selezionare le condizioni per l'utilizzo che si desidera modificare.
1. Fare clic su **modifica termini**
1. Fare clic su **Aggiungi lingua** nella parte inferiore della pagina.
1. Nel riquadro Aggiungi la lingua delle condizioni per l'utilizzo caricare il PDF localizzato e selezionare la lingua.

    ![Condizioni per l'utilizzo selezionata e visualizzata la scheda lingue nel riquadro dei dettagli](./media/terms-of-use/select-language.png)

1. Fare clic su **Aggiungi lingua**.
1. Fare clic su **Save** (Salva).

1. Fare clic su **Aggiungi** per aggiungere la lingua.

## <a name="per-device-terms-of-use"></a>Condizioni per l'utilizzo per dispositivo

L'impostazione **Richiedi agli utenti di acconsentire su ogni dispositivo** consente di richiedere agli utenti finali di accettare i criteri di utilizzo per ogni dispositivo da cui accedono. All'utente finale verrà richiesto di registrare il dispositivo in Azure AD. Quando il dispositivo è registrato, l'ID dispositivo viene usato per applicare i criteri di utilizzo in ogni dispositivo.

Ecco un elenco dei software e delle piattaforme supportate.

> [!div class="mx-tableFixed"]
> |  | iOS | Telefoni | Windows 10 | Altro |
> | --- | --- | --- | --- | --- |
> | **App nativa** | Sì | Sì | Sì |  |
> | **Microsoft Edge** | Sì | Sì | Sì |  |
> | **Internet Explorer** | Sì | Sì | Sì |  |
> | **Chrome (con estensione)** | Sì | Sì | Sì |  |

Le condizioni per l'utilizzo per dispositivo hanno i vincoli seguenti:

- Un dispositivo può essere aggiunto a un solo tenant.
- Un utente deve disporre delle autorizzazioni per aggiungere il dispositivo.
- L'app di registrazione di Intune non è supportata. Assicurarsi che sia escluso dai criteri di accesso condizionale che richiedono criteri di utilizzo.
- Azure AD utenti B2B non sono supportati.

Se il dispositivo dell'utente non è stato aggiunto, riceveranno un messaggio che indica che devono aggiungere il dispositivo. L'esperienza sarà dipendente dalla piattaforma e dal software.

### <a name="join-a-windows-10-device"></a>Aggiungere un dispositivo Windows 10

Se un utente sta usando Windows 10 e Microsoft Edge, riceverà un messaggio simile al seguente per [aggiungere il dispositivo](../user-help/user-help-join-device-on-network.md#to-join-an-already-configured-windows-10-device).

![Windows 10 e Microsoft Edge-messaggio che indica che il dispositivo deve essere registrato](./media/terms-of-use/per-device-win10-edge.png)

Se si usa Chrome, verrà richiesto di installare l'[estensione account di Windows 10](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

### <a name="register-an-ios-device"></a>Registrare un dispositivo iOS

Se un utente usa un dispositivo iOS, verrà richiesto di installare l' [app Microsoft Authenticator](https://apps.apple.com/us/app/microsoft-authenticator/id983156458).

### <a name="register-an-android-device"></a>Registrare un dispositivo Android

Se un utente usa un dispositivo Android, verrà richiesto di installare l' [app Microsoft Authenticator](https://play.google.com/store/apps/details?id=com.azure.authenticator).

### <a name="browsers"></a>Browser

Se un utente sta usando un browser che non è supportato, si dovrà usare un browser diverso.

![Messaggio che indica che il dispositivo deve essere registrato, ma il browser non è supportato](./media/terms-of-use/per-device-browser-unsupported.png)

## <a name="delete-terms-of-use"></a>Eliminare le condizioni per l'utilizzo

È possibile eliminare i criteri di utilizzo obsoleti utilizzando la procedura riportata di seguito.

1. Accedere ad Azure e passare a **Condizioni per l'utilizzo** all'indirizzo [https://aka.ms/catou](https://aka.ms/catou).
1. Selezionare le condizioni per l'utilizzo che si desidera rimuovere.
1. Fare clic su **Elimina le condizioni**.
1. Nel messaggio visualizzato in cui viene chiesto se si vuole continuare fare clic su **Sì**.

    ![Messaggio in cui viene chiesto di confermare l'eliminazione delle condizioni per l'utilizzo](./media/terms-of-use/delete-tou.png)

   Non è più possibile visualizzare i criteri delle condizioni per l'utilizzo.

## <a name="user-acceptance-record-deletion"></a>Eliminazione del record di accettazione utente

I record di accettazione utente vengono eliminati:

- Quando l'amministratore Elimina in modo esplicito le condizioni. In tal caso, vengono eliminati anche tutti i record di accettazione associati a determinate condizioni.
- Quando il tenant perde la licenza Azure Active Directory Premium.
- Quando il tenant viene eliminato.

## <a name="policy-changes"></a>Modifiche dei criteri

I criteri di accesso condizionale hanno effetto immediato. In questo caso l'amministratore inizierà a vedere nuvolette "tristi" o messaggi relativi a problemi di token di Azure AD. Dovrà quindi disconnettersi e accedere nuovamente per soddisfare il nuovo criterio.

> [!IMPORTANT]
> Gli utenti inclusi nell'ambito devono disconnettersi e accedere per soddisfare un nuovo criterio se:
>
> - un criterio di accesso condizionale è abilitato in base a criteri di utilizzo
> - o viene creato un secondo criterio di condizioni per l'utilizzo

## <a name="b2b-guests"></a>Guest B2B

La maggior parte delle organizzazioni dispone di un processo per consentire ai propri dipendenti di fornire il consenso alle condizioni per l'utilizzo e alle informative sulla privacy dell'organizzazione. Ma come è possibile applicare gli stessi consensi per i guest di Azure AD business-to-business (B2B) quando vengono aggiunti tramite SharePoint o i team? Usando l'accesso condizionale e i criteri di condizioni per l'utilizzo, è possibile applicare un criterio direttamente agli utenti Guest B2B. Durante il flusso di riscatto dell'invito, l'utente viene visualizzato con i criteri per le condizioni per l'utilizzo. Questo supporto è attualmente disponibile in versione di anteprima.

I criteri di Condizioni per l'utilizzo verranno visualizzati solo quando l'utente dispone di un account Guest nel Azure AD. SharePoint Online dispone attualmente di un' [esperienza di destinatario di condivisione esterna ad hoc](/sharepoint/what-s-new-in-sharing-in-targeted-release) per condividere un documento o una cartella che non richiede che l'utente disponga di un account Guest. In questo caso, non viene visualizzato alcun criterio di condizioni per l'utilizzo.

![Riquadro utenti e gruppi-scheda Includi con l'opzione tutti gli utenti Guest selezionata](./media/terms-of-use/b2b-guests.png)

## <a name="support-for-cloud-apps"></a>Supporto per le app Cloud

È possibile usare i criteri di Condizioni per l'utilizzo per diverse app Cloud, ad esempio Azure Information Protection e Microsoft Intune. Questo supporto è attualmente disponibile in versione di anteprima.

### <a name="azure-information-protection"></a>Azure Information Protection

È possibile configurare un criterio di accesso condizionale per l'app Azure Information Protection e richiedere un criterio di condizioni d'uso quando un utente accede a un documento protetto. In questo modo verrà attivato un criterio di condizioni per l'utilizzo prima che un utente acceda a un documento protetto per la prima volta.

![Riquadro app cloud con Microsoft Azure Information Protection app selezionata](./media/terms-of-use/cloud-app-info-protection.png)

### <a name="microsoft-intune-enrollment"></a>Registrazione di Microsoft Intune

È possibile configurare un criterio di accesso condizionale per l'app di registrazione Microsoft Intune e richiedere un criterio di condizioni per l'utilizzo prima della registrazione di un dispositivo in Intune. Per altre informazioni, vedere il Leggi il [post di blog sulla scelta della soluzione di Condizioni di utilizzo più adatta per l'organizzazione](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

![Riquadro app cloud con Microsoft Intune app selezionata](./media/terms-of-use/cloud-app-intune.png)

> [!NOTE]
> L'app di registrazione di Intune non è supportata per le condizioni per l' [utilizzo per dispositivo](#per-device-terms-of-use).

## <a name="frequently-asked-questions"></a>Domande frequenti

**D: non è possibile accedere con PowerShell quando le condizioni per l'utilizzo sono abilitate.**<br />
R: Condizioni per l'utilizzo può essere accettato solo quando si esegue l'autenticazione in modo interattivo.

**D: Ricerca per categorie vedere quando/se un utente ha accettato le condizioni per l'utilizzo?**<br />
R: Nel pannello Condizioni per l'utilizzo fare clic sul numero sotto **Accettato**. È anche possibile visualizzare o cercare l'attività accettata nei log di controllo di Azure AD. Per altre informazioni, vedere Visualizzare il report degli utenti che hanno accettato e rifiutato e [Visualizzare i log di controllo di Azure AD](#view-azure-ad-audit-logs).

**D: per quanto tempo sono archiviate le informazioni?**<br />
R: l'utente conta le condizioni per l'utilizzo e le persone accettate/rifiutate vengono archiviate per la durata delle condizioni per l'utilizzo. I log di controllo di Azure AD vengono archiviati per 30 giorni.

**D: perché viene visualizzato un numero diverso di consensi nel rapporto sulle condizioni per l'utilizzo rispetto ai log di controllo Azure AD?**<br />
R: il report sulle condizioni per l'utilizzo viene archiviato per la durata di tali criteri di utilizzo, mentre i log di controllo Azure AD vengono archiviati per 30 giorni. Inoltre, il report condizioni per l'utilizzo Visualizza solo lo stato di consenso dell'utente corrente. Se, ad esempio, un utente rifiuta e quindi accetta, il report sulle condizioni per l'utilizzo indicherà solo l'accettazione dell'utente. Se è necessario visualizzare la cronologia, è possibile usare i log di controllo di Azure AD.

**D: se si modificano i dettagli per i criteri di utilizzo, è necessario che gli utenti accettino di nuovo?**<br />
R: No. se un amministratore modifica i dettagli relativi a un criterio di condizioni d'uso (nome, nome visualizzato, richiedere agli utenti di espandere o aggiungere una lingua), non richiede che gli utenti riaccettino le nuove condizioni.

**D: è possibile aggiornare un documento per i criteri di condizioni per l'utilizzo esistente?**<br />
R: attualmente non è possibile aggiornare un documento di criteri per le condizioni per l'utilizzo esistente. Per modificare un documento relativo ai criteri delle condizioni per l'utilizzo, è necessario creare una nuova istanza dei criteri di utilizzo.

**D: se i collegamenti ipertestuali sono presenti nel documento PDF Condizioni per l'utilizzo, gli utenti finali potranno fare clic su di essi?**<br />
R: Sì, gli utenti finali possono selezionare collegamenti ipertestuali a pagine aggiuntive, ma i collegamenti alle sezioni all'interno del documento non sono supportati. Inoltre, i collegamenti ipertestuali in termini di utilizzo i file PDF non funzionano quando si accede dal portale Azure AD app/account.

**D: i criteri per le condizioni per l'utilizzo supportano più lingue?**<br />
R: Sì. Attualmente sono disponibili 108 lingue diverse che possono essere configurate da un amministratore per un singolo criterio di condizioni per l'utilizzo. Un amministratore può caricare più documenti PDF e contrassegnare i documenti con una lingua corrispondente (fino a 108). Quando gli utenti finali accedono, vengono esaminate le preferenze della lingua del browser e viene visualizzato il documento corrispondente. Se non viene trovata alcuna corrispondenza, verrà visualizzato il documento predefinito, ovvero il primo documento che viene caricato.

**D: quando vengono attivati i criteri delle condizioni per l'utilizzo?**<br />
R: i criteri per le condizioni per l'utilizzo vengono attivati durante l'esperienza di accesso.

**D: a quali applicazioni è possibile indirizzare I criteri di utilizzo?**<br />
R: è possibile creare un criterio di accesso condizionale per le applicazioni aziendali usando l'autenticazione moderna. Per altre informazioni, vedere le [applicazioni aziendali](./../manage-apps/view-applications-portal.md).

**D: è possibile aggiungere più condizioni per l'utilizzo dei criteri a un determinato utente o app?**<br />
R: Sì, creando più criteri di accesso condizionale destinati a tali gruppi o applicazioni. Se un utente rientra nell'ambito di più criteri di utilizzo, accetta un criterio di utilizzo per volta.

**D: cosa accade se un utente rifiuta le condizioni per l'utilizzo dei criteri?**<br />
R: ne viene bloccato l'accesso all'applicazione. L'utente deve accedere nuovamente e accettare le condizioni.

**D: è possibile che non si accettino i criteri di utilizzo accettati in precedenza?**<br />
R: è possibile [esaminare le condizioni per l'utilizzo accettate in precedenza](#how-users-can-review-their-terms-of-use), ma attualmente non esiste un modo per non accettarle.

**D: Cosa succede se si usano anche termini e condizioni di Intune?**<br />
R: se sono state configurate sia Azure AD condizioni per l'utilizzo che i [termini e le condizioni di Intune](/intune/terms-and-conditions-create), l'utente dovrà accettare entrambi i requisiti. Per altre informazioni, vedere il [post di blog sulla scelta della soluzione di Condizioni di utilizzo più adatta per l'organizzazione](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

**D: quali endpoint utilizzano le condizioni per l'utilizzo del servizio per l'autenticazione?**<br />
R: Condizioni per l'utilizzo utilizza gli endpoint seguenti per l'autenticazione: https://tokenprovider.termsofuse.identitygovernance.azure.com e https://account.activedirectory.windowsazure.com . Se l'organizzazione ha un elenco di URL consentiti per la registrazione, è necessario aggiungere questi endpoint all'elenco Consenti insieme agli endpoint Azure AD per l'accesso.

## <a name="next-steps"></a>Passaggi successivi

- [Avvio rapido: Richiedere l'accettazione di condizioni per l'utilizzo prima dell'accesso alle app cloud](require-tou.md)
