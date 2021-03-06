---
title: Esperienza utente di accesso SMS per numero di telefono-Azure AD
description: Informazioni sull'esperienza utente di accesso tramite SMS per numeri di telefono nuovi o esistenti
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 01/21/2021
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 1a50f2032a978a552205d1bba602249f34f0478a
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2021
ms.locfileid: "98661594"
---
# <a name="use-your-phone-number-as-a-user-name"></a>Usa il tuo numero di telefono come nome utente

La registrazione di un dispositivo consente al telefono di accedere ai servizi dell'organizzazione e non consente all'organizzazione di accedere al telefono. Gli amministratori possono trovare altre informazioni in [Configurare e abilitare gli utenti per l'autenticazione basata su SMS](../authentication/howto-authentication-sms-signin.md).

Se l'organizzazione non ha reso disponibile l'accesso tramite SMS, l'opzione non sarà visualizzata durante la registrazione di un telefono con l'account.  

## <a name="when-you-have-a-new-phone-number"></a>Nuovo numero di telefono

Se si riceve un nuovo telefono o un nuovo numero e lo si registra con un'organizzazione che ha reso disponibile l'accesso tramite SMS, il telefono viene registrato tramite la normale procedura:

1. Selezionare **Aggiungi metodo**.
1. Selezionare **Telefono**.
1. Immettere il numero di telefono e selezionare **Invia un SMS**.
1. Dopo aver immesso il codice, selezionare **Avanti**.
1. Verrà visualizzato il messaggio "Verificato tramite SMS. Il telefono è stato registrato".

> [!Important]
> A causa di un problema noto, per un breve periodo di tempo l'aggiunta del numero di telefono non registrerà il numero per l'accesso a SMS. Sarà necessario accedere con il numero aggiunto e seguire le istruzioni per registrare il numero per l'accesso tramite SMS.

### <a name="when-the-phone-number-is-in-use"></a>Numero di telefono in uso

Se si prova a usare un numero di telefono usato da un altro utente nell'organizzazione, verrà visualizzato il messaggio seguente:

![Messaggio di errore quando il numero di telefono è già in uso](media/sms-sign-in-explainer/sms-sign-in-error.png)

Rivolgersi all'amministratore per correggere il problema.

## <a name="when-you-have-an-existing-number"></a>Numero esistente

Se si usa già un numero di telefono con un'organizzazione ed è disponibile l'opzione per usare il numero di telefono come nome utente, la procedura seguente può facilitare l'accesso.

1. Quando l'accesso tramite SMS è disponibile, viene visualizzato un banner in cui si chiede se si vuole abilitare il numero di telefono per eseguire l'accesso tramite SMS:

    :::image type="content" source="media/sms-sign-in-explainer/sms-sign-in-banner.png" alt-text="Screenshot che mostra il banner per abilitare l'accesso SMS per un numero di telefono con l'azione ' Abilità selezionata." lightbox="media/sms-sign-in-explainer/sms-sign-in-banner.png":::

1. Viene visualizzato anche un pulsante **Abilita** se si seleziona l'icona a forma di accento circonflesso nel riquadro del metodo per il telefono:

    [![Banner per abilitare l'accesso SMS per un numero di telefono.](media/sms-sign-in-explainer/sms-sign-in-phone-method.png)](media/sms-sign-in-explainer/sms-sign-in-phone-method.png#lightbox)

1. Per abilitare il metodo, selezionare **Abilita**. Verrà chiesto di confermare l'azione:

    ![Finestra di dialogo di conferma per abilitare l'accesso tramite SMS per un numero di telefono](media/sms-sign-in-explainer/sms-sign-in-confirmation.png)

1. Selezionare **Abilita**.

## <a name="when-you-remove-your-phone-number"></a>Rimozione del numero di telefono

1. Per eliminare il numero di telefono, selezionare il pulsante Elimina nel riquadro del metodo di accesso tramite SMS per il telefono.

    [![Banner per eliminare l'accesso SMS per un numero di telefono.](media/sms-sign-in-explainer/sms-sign-in-delete-method.png)](media/sms-sign-in-explainer/sms-sign-in-delete-method.png#lightbox)

2. Quando viene chiesto di confermare l'operazione, selezionare **OK**.

Non è possibile rimuovere un numero di telefono che viene usato come metodo di accesso predefinito. Per rimuovere il numero, è necessario modificare il metodo di accesso predefinito, quindi rimuovere di nuovo il numero di telefono.
