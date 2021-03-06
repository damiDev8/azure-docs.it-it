---
title: Autenticazione con passcode monouso per utenti guest B2B - Azure AD
description: Come usare il passcode monouso tramite indirizzo di posta elettronica per autenticare gli utenti guest B2B senza la necessità di un account Microsoft.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 12/18/2020
ms.author: mimart
author: msmimart
manager: CelesteDG
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9a0668b3ea651d129dc076e5f2247e38f5ab7d0
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2021
ms.locfileid: "98725496"
---
# <a name="email-one-time-passcode-authentication"></a>Autenticazione del codice di accesso monouso tramite posta elettronica

Questo articolo descrive come abilitare l'autenticazione del codice di posta elettronica monouso per gli utenti Guest B2B. La funzionalità di accesso monouso per il codice di posta elettronica consente di autenticare gli utenti Guest B2B quando non è possibile eseguire l'autenticazione con altri strumenti, ad esempio Azure AD, un account Microsoft (MSA) o una Federazione Google. Grazie all'autenticazione con passcode monouso, non è necessario creare un account Microsoft. Quando l'utente guest riscatta un invito o accede a una risorsa condivisa, può richiedere un codice temporaneo, che viene inviato all'indirizzo di posta elettronica. Quindi immette tale codice per continuare ad accedere.

![Diagramma di panoramica del codice di accesso monouso di posta elettronica](media/one-time-passcode/email-otp.png)

> [!IMPORTANT]
> **A partire dal 2021 marzo**, la funzionalità di accesso monouso per il codice di posta elettronica verrà attivata per tutti i tenant esistenti e abilitata per impostazione predefinita per i nuovi tenant. Se non si vuole consentire l'attivazione automatica di questa funzionalità, è possibile disabilitarla. Vedere la pagina relativa alla [disabilitazione della password una sola volta](#disable-email-one-time-passcode) .

> [!NOTE]
> Gli utenti con passcode monouso devono accedere tramite un collegamento che include il contesto tenant (ad esempio `https://myapps.microsoft.com/?tenantid=<tenant id>` o `https://portal.azure.com/<tenant id>` oppure, nel caso di un dominio verificato, `https://myapps.microsoft.com/<verified domain>.onmicrosoft.com`). È possibile usare anche collegamenti diretti alle applicazioni e alle risorse, purché includano tale contesto. Gli utenti guest attualmente non possono accedere tramite endpoint privi di un contesto tenant. Se ad esempio `https://myapps.microsoft.com` si usa, `https://portal.azure.com` verrà generato un errore.

## <a name="user-experience-for-one-time-passcode-guest-users"></a>Esperienza utente per gli utenti guest con passcode monouso

Quando è abilitata la funzionalità di accesso monouso per il messaggio di posta elettronica, gli utenti appena invitati [che soddisfano determinate condizioni](#when-does-a-guest-user-get-a-one-time-passcode) utilizzeranno l'autenticazione con un solo tempo. Gli utenti guest che hanno riscattato un invito prima dell'abilitazione del codice di posta elettronica monouso continueranno a usare lo stesso metodo di autenticazione.

Grazie all'autenticazione con passcode monouso, l'utente guest può riscattare l'invito facendo clic su un collegamento diretto o tramite l'indirizzo di posta elettronica di invito. In entrambi i casi, un messaggio nel browser indica che verrà inviato un codice all'indirizzo di posta elettronica dell'utente guest. L'utente guest seleziona **Send code** (Invia codice):

   ![Screenshot che mostra il pulsante Invia codice](media/one-time-passcode/otp-send-code.png)

Un passcode viene inviato all'indirizzo di posta elettronica dell'utente. L'utente recupera il passcode dal messaggio di posta elettronica e lo immette nella finestra del browser:

   ![Screenshot che mostra la pagina Immetti il codice](media/one-time-passcode/otp-enter-code.png)

L'utente guest viene autenticato e può quindi visualizzare la risorsa condivisa o continuare ad accedere.

> [!NOTE]
> I passcode monouso sono validi per 30 minuti. Dopo 30 minuti, tale passcode monouso specifico non è più valido e l'utente deve richiederne uno nuovo. Le sessioni utente scadono dopo 24 ore. Dopo tale periodo, l'utente guest riceve un nuovo passcode quando accede alla risorsa. La scadenza della sessione offre una sicurezza maggiore, in particolare quando un utente guest lascia la società o non ha più bisogno dell'accesso.

## <a name="when-does-a-guest-user-get-a-one-time-passcode"></a>In quali casi un utente guest ottiene un passcode monouso?

Quando un utente guest riscatta un invito o usa un collegamento a una risorsa che è stato condivisa con tale utente, questo riceve un passcode monouso se:

- Non ha un account Azure AD
- Non ha un account Microsoft
- Il tenant che emette l'invito non ha impostato federazione Google per gli utenti @gmail.com e @googlemail.com

Al momento dell'invito non è presente alcuna indicazione del fatto che l'utente che si sta invitando userà l'autenticazione con passcode monouso. Tuttavia quando l'utente guest accede, l'autenticazione con passcode monouso sarà il metodo di fallback se non è possibile utilizzare altri metodi di autenticazione.

È possibile verificare se un utente guest esegue l'autenticazione usando i codici di accesso monouso visualizzando la proprietà **source** nei dettagli dell'utente. Nella portale di Azure passare a **Azure Active Directory**  >  **utenti**, quindi selezionare l'utente per aprire la pagina dei dettagli.

![Screenshot che mostra un utente con passcode monouso e il valore di origine Passcode monouso](media/one-time-passcode/guest-user-properties.png)

> [!NOTE]
> Quando un utente riscatta un passcode monouso e in un secondo momento ottiene un account Microsoft, un account di Azure AD o un altro account federato, continuerà a essere autenticato con un passcode monouso. Se si vuole aggiornare il metodo di autenticazione di tale utente, è possibile eliminare l'account utente guest e invitarlo nuovamente.

### <a name="example"></a>Esempio

Utente guest teri@gmail.com viene invitato a Fabrikam, che non dispone di federazione Google configurata. Teri non dispone di un account Microsoft. Riceverà un passcode monouso per l'autenticazione.

## <a name="disable-email-one-time-passcode"></a>Disabilitare il codice di posta elettronica monouso

A partire dal 2021 marzo, la funzionalità di accesso monouso per il codice di posta elettronica verrà attivata per tutti i tenant esistenti e abilitata per impostazione predefinita per i nuovi tenant. In quel momento, Microsoft non supporterà più il riscatto degli inviti mediante la creazione di account Azure AD e tenant non gestiti ("virale" o "just-in-Time") per gli scenari di collaborazione B2B. È in corso l'abilitazione della funzionalità di accesso monouso per la posta elettronica perché fornisce un metodo di autenticazione di fallback semplice per gli utenti guest. Tuttavia, è possibile disabilitare questa funzionalità se si sceglie di non usarla.

> [!NOTE]
>
> Se è stata abilitata la funzionalità di accesso monouso per il messaggio di posta elettronica nel tenant e la si disattiva, gli utenti guest che hanno riscattato un codice di accesso monouso non saranno in grado di eseguire l'accesso. È possibile eliminare l'utente Guest e inserirlo di nuovo in modo che possa eseguire di nuovo l'accesso con un altro metodo di autenticazione.

### <a name="to-disable-the-email-one-time-passcode-feature"></a>Per disabilitare la funzionalità di accesso monouso per la posta elettronica

1. Accedere al [portale di Azure](https://portal.azure.com/) come amministratore globale di Azure AD.

2. Nel riquadro di spostamento selezionare **Azure Active Directory**.

3. Selezionare **Identità esterne** > **Impostazioni di collaborazione esterna**.

4. In **password monouso per i guest** selezionare Disabilita il **codice di accesso monouso per i guest**.

    ![Impostazioni di password monouso per la posta elettronica](media/one-time-passcode/otp-admin-settings.png)

   > [!NOTE]
   > Se viene visualizzato il seguente interruttore anziché le opzioni sopra riportate, questo significa che in precedenza è stata abilitata, disabilitata o è stata scelta l'anteprima della funzionalità. Selezionare **No** per disabilitare la funzionalità.
   >
   >![Abilita la password monouso per la posta elettronica](media/delegate-invitations/enable-email-otp-opted-in.png)

5. Selezionare **Salva**.

## <a name="note-for-public-preview-customers"></a>Nota per i clienti dell'anteprima pubblica

Se si è scelto in precedenza l'anteprima pubblica di un codice di accesso monouso per la posta elettronica, la data di marzo 2021 per l'abilitazione automatica della funzionalità non è applicabile, quindi i processi aziendali correlati non saranno interessati. Inoltre, nel portale di Azure, sotto le proprietà del **codice di posta elettronica** monouso per i guest, non verrà visualizzata l'opzione per abilitare automaticamente il codice di accesso monouso della **posta elettronica per i guest nel 2021 marzo**. Al contrario, verrà visualizzato **quanto segue:** 

![Abilita la password monouso per la posta elettronica](media/delegate-invitations/enable-email-otp-opted-in.png)

Tuttavia, se si preferisce rifiutare esplicitamente la funzionalità e consentirne l'abilitazione automatica nel 2021 marzo, è possibile ripristinare le impostazioni predefinite usando il [tipo di risorsa di configurazione del metodo di autenticazione della posta elettronica](/graph/api/resources/emailauthenticationmethodconfiguration)Microsoft Graph API. Dopo aver ripristinato le impostazioni predefinite, le opzioni seguenti saranno disponibili con il **codice di posta elettronica** monouso per i guest:

- **Abilitare automaticamente il codice di accesso monouso tramite posta elettronica per i guest nel 2021 marzo**. Predefinita Se la funzionalità di accesso monouso per il messaggio di posta elettronica non è già abilitata per il tenant, verrà attivata automaticamente il 2021 marzo. Non è necessaria alcuna azione aggiuntiva se si desidera che la funzionalità sia abilitata in quel momento. Se la funzionalità è già stata abilitata o disabilitata, questa opzione non sarà disponibile.

- **Consente di abilitare il codice di posta elettronica monouso per i guest**. Attiva la funzionalità di accesso monouso della posta elettronica per il tenant.

- **Disabilitare il codice di posta elettronica monouso per i guest**. Disattiva la funzionalità di accesso monouso di un messaggio di posta elettronica per il tenant e impedisce l'attivazione della funzionalità nel 2021 marzo.