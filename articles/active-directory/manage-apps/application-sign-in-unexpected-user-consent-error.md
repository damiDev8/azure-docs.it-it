---
title: Errore imprevisto durante la richiesta di consenso per un'applicazione | Microsoft Docs
description: Vengono illustrati gli errori che possono verificarsi quando si tenta di ottenere il consenso per un'applicazione, nonché suggerite le operazioni da eseguire per risolverli
services: active-directory
documentationcenter: ''
author: kenwith
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f829672f88ea848e4611000b54d9cc200bc166d
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99259978"
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a>Errore imprevisto durante la richiesta di consenso per un'applicazione

Questo articolo descrive gli errori che possono verificarsi quando si tenta di ottenere il consenso per un'applicazione. Per risolvere i problemi relativi a richieste di consenso impreviste che non contengono messaggi di errore, vedere [Scenari di autenticazione per Azure AD](../develop/authentication-vs-authorization.md).

Molte applicazioni integrate in Azure Active Directory richiedono autorizzazioni per accedere ad altre risorse e garantire il funzionamento. Quando anche queste risorse sono integrate in Azure Active Directory, le autorizzazioni per il relativo accesso vengono spesso richieste tramite il framework di consenso comune. Questo comporta la visualizzazione di una richiesta di consenso, che in genere viene inviata al primo utilizzo di un'applicazione ma a volte anche successivamente.

Perché un utente possa concedere le autorizzazioni richieste da un'applicazione, è necessario che siano soddisfatte determinate condizioni. In caso contrario, possono verificarsi gli errori seguenti.

## <a name="requesting-not-authorized-permissions-error"></a>Errore di richiesta di autorizzazioni non consentite
* **AADSTS90093:** &lt;NomeVisualizzatoAppClient&gt; richiede una o più autorizzazioni che l'utente non è autorizzato a concedere. Contattare un amministratore che possa concedere il consenso per l'applicazione per conto dell'utente.
* **AADSTS90094:** &lt;NomeVisualizzatoAppClient&gt; richiede l'autorizzazione per accedere alle risorse dell'organizzazione che solo un amministratore può concedere. Prima di usarla, è necessario chiedere a un amministratore di concedere l'autorizzazione a quest'app.

Questo errore si verifica quando un utente che non è un amministratore globale tenta di utilizzare un'applicazione che richiede autorizzazioni che solo un amministratore può concedere. Il problema può essere risolto chiedendo a un amministratore di concedere l'accesso all'applicazione per conto dell'organizzazione.

Questo errore può verificarsi anche quando si impedisce a un utente di acconsentire a un'applicazione a causa del rilevamento da parte di Microsoft che la richiesta di autorizzazioni è rischiosa. In questo caso, verrà registrato anche un evento di controllo con una categoria "ApplicationManagement", un tipo di attività di "consenso per l'applicazione" e il motivo dello stato "applicazione rischiosa rilevata".

Un altro scenario in cui questo errore potrebbe verificarsi è quando è richiesta l'assegnazione dell'utente per l'applicazione, ma non è stato fornito alcun consenso da parte dell'amministratore. In questo caso, l'amministratore deve innanzitutto fornire il consenso dell'amministratore.   

## <a name="policy-prevents-granting-permissions-error"></a>Errore dovuto a criteri che impediscono di concedere le autorizzazioni
* **AADSTS90093:** un amministratore di &lt;NomeVisualizzatoTenant&gt; ha configurato criteri che impediscono di concedere a &lt;nome dell'app&gt; le autorizzazioni che richiede. Contattare un amministratore di &lt;NomeVisualizzatoTenant&gt; che possa concedere le autorizzazioni richieste dall'app per conto dell'utente.

Questo errore si verifica quando un amministratore globale disattiva la possibilità per gli utenti di fornire il consenso alle applicazioni, quindi un utente non amministratore tenta di utilizzare un'applicazione che richiede il consenso. Il problema può essere risolto chiedendo a un amministratore di concedere l'accesso all'applicazione per conto dell'organizzazione.

## <a name="intermittent-problem-error"></a>Errore dovuto a un problema intermittente
* **AADSTS90090:** sembra che la procedura di accesso rilevi un problema intermittente durante la registrazione delle autorizzazioni che si è tentato di concedere a &lt;NomeVisualizzazioneAppClient&gt;. Riprovare in un secondo momento.

Questo errore indica che si è verificato un problema intermittente relativo al servizio. È possibile risolverlo provando a concedere di nuovo il consenso per l'applicazione.

## <a name="resource-not-available-error"></a>Errore di risorsa non disponibile
* **AADSTS65005:** l'app &lt;NomeVisualizzatoAppClient&gt; ha richiesto autorizzazioni per accedere alla risorsa &lt;NomeVisualizzatoAppRisorsa&gt; che non è disponibile. 

Contattare lo sviluppatore dell'applicazione.

##  <a name="resource-not-available-in-tenant-error"></a>Errore di risorsa non disponibile nel tenant
* **AADSTS65005:** &lt;NomeVisualizzatoAppClient&gt; richiede l'accesso alla risorsa &lt;NomeVisualizzatoAppRisorsa&gt; che non è disponibile all'interno dell'organizzazione &lt;NomeVisualizzatoTenant&gt;. 

Assicurarsi che la risorsa sia disponibile o contattare un amministratore di &lt;NomeVisualizzatoTenant&gt;.

## <a name="permissions-mismatch-error"></a>Errore di mancata corrispondenza delle autorizzazioni
* **AADSTS65005:** l'app ha richiesto il consenso per accedere alla risorsa &lt;NomeVisualizzatoAppRisorsa&gt;. Questa richiesta ha avuto esito negativo perché non corrisponde al modo in cui l'app è stata preconfigurata durante la registrazione. Contattare il fornitore dell'app.**

Tutti questi errori si verificano quando l'applicazione per la quale un utente sta provando a concedere il consenso richiede autorizzazioni per accedere a un'applicazione della risorsa non trovata nella directory dell'organizzazione (tenant). Questa situazione può verificarsi per diversi motivi:

-   Lo sviluppatore dell'applicazione client ha configurato l'applicazione in modo errato per cui questa richiede l'accesso a una risorsa non valida. In questo caso, lo sviluppatore dell'applicazione deve aggiornare la configurazione dell'applicazione client per risolvere il problema.

-   Un'entità servizio che rappresenta l'applicazione della risorsa di destinazione non esiste nell'organizzazione o esisteva in precedenza ma è stata rimossa. Per risolvere questo problema, è necessario eseguire il provisioning di un'entità servizio per l'applicazione della risorsa all'interno dell'organizzazione, in modo che l'applicazione client possa richiedere le autorizzazioni per tale entità. È possibile eseguire il provisioning dell'entità servizio in diversi modi, in funzione del tipo di applicazione, incluso:

    -   Acquisendo una sottoscrizione per l'applicazione della risorsa (applicazioni pubblicate da Microsoft)

    -   Concedendo il consenso per l'applicazione della risorsa

    -   Concedendo le autorizzazioni all'applicazione tramite il portale di Azure

    -   Aggiungendo l'applicazione dalla raccolta di applicazioni di Azure AD

## <a name="risky-app-error-and-warning"></a>Errore e avviso dell'app rischiosa
* **AADSTS900941:** Il consenso dell'amministratore è obbligatorio. L'app è considerata rischiosa. (AdminConsentRequiredDueToRiskyApp)
* Questa app potrebbe essere rischiosa. Se si considera attendibile questa app, rivolgersi all'amministratore per concedere l'accesso.
* **AADSTS900981:** Per un'app rischiosa è stata ricevuta una richiesta di consenso dell'amministratore. (AdminConsentRequestRiskyAppWarning)
* Questa app potrebbe essere rischiosa. Continuare solo se si considera attendibile l'app.

Entrambi questi messaggi verranno visualizzati quando Microsoft ha stabilito che la richiesta di consenso potrebbe essere rischiosa. Tra molti altri fattori, questo problema può verificarsi se un [editore verificato](../develop/publisher-verification-overview.md) non è stato aggiunto alla registrazione dell'app. Il primo codice di errore e il messaggio verranno visualizzati agli utenti finali quando il [flusso di lavoro di consenso dell'amministratore](configure-admin-consent-workflow.md) è disabilitato. Il secondo codice e messaggio verrà visualizzato agli utenti finali quando il flusso di lavoro di consenso dell'amministratore è abilitato e agli amministratori. 

Gli utenti finali non saranno in grado di concedere il consenso alle app che sono state rilevate come rischiose. Gli amministratori sono in grado di valutare l'app in modo molto accurato e procedere con cautela. Se l'app sembra sospetta dopo un'ulteriore verifica, può essere segnalata a Microsoft dalla schermata del consenso. 

## <a name="next-steps"></a>Passaggi successivi 

[App, autorizzazioni e consenso in Azure Active Directory](../develop/quickstart-register-app.md)<br>

[Ambiti, autorizzazioni e consenso in Azure Active Directory (endpoint v2.0)](../develop/v2-permissions-and-consent.md)