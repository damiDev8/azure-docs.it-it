---
title: Registrare le app daemon che chiamano API Web-piattaforma di identità Microsoft | Azure
description: Informazioni su come creare un'app daemon che chiama API Web-registrazione di app
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/15/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 938a19276839d5e0d2bd3e0244510fc068cb029c
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2021
ms.locfileid: "99582908"
---
# <a name="daemon-app-that-calls-web-apis---app-registration"></a>App daemon che chiama le API Web-registrazione app

Per un'applicazione daemon, ecco cosa è necessario sapere quando si registra l'app.

## <a name="supported-account-types"></a>Tipi di account supportati

Le applicazioni daemon hanno senso solo nei tenant Azure AD. Quindi, quando si crea l'applicazione, scegliere una delle opzioni seguenti:

- **Solo gli account in questa directory aziendale**. Questa scelta è la più comune perché le applicazioni daemon vengono in genere scritte da sviluppatori line-of-business (LOB).
- **Account in qualsiasi directory dell'organizzazione**. Questa scelta è consigliabile se si è un ISV che fornisce agli utenti uno strumento di utilità. Per approvare l'approvazione, saranno necessari gli amministratori del tenant dei clienti.

## <a name="authentication---no-reply-uri-needed"></a>Autenticazione-nessun URI di risposta necessario

Nel caso in cui l'applicazione client riservata usi *solo* il flusso di credenziali client, non è necessario registrare l'URI di risposta. Non è necessario per la configurazione o la costruzione dell'applicazione. Il flusso di credenziali client non lo usa.

## <a name="api-permissions---app-permissions-and-admin-consent"></a>Autorizzazioni per le API: autorizzazioni per le app e consenso dell'amministratore

Un'applicazione daemon può richiedere solo le autorizzazioni dell'applicazione per le API (non le autorizzazioni delegate). Nella pagina **autorizzazioni API** per la registrazione dell'applicazione, dopo aver selezionato **Aggiungi un'autorizzazione** e scelto la famiglia di API, scegliere **Autorizzazioni applicazione**, quindi selezionare le autorizzazioni.

![Autorizzazioni dell'app e consenso dell'amministratore](media/scenario-daemon-app/app-permissions-and-admin-consent.png)

> [!NOTE]
> L'API Web che si vuole chiamare deve definire le *autorizzazioni dell'applicazione (ruoli app)*, non le autorizzazioni delegate. Per informazioni dettagliate su come esporre un'API di questo tipo, vedere [API Web protetta: registrazione dell'app-quando l'API Web viene chiamata da un'app daemon](scenario-protected-web-api-app-registration.md#if-your-web-api-is-called-by-a-daemon-app).

Per le applicazioni daemon è necessario che un amministratore tenant preacconsente all'applicazione che chiama l'API Web. Gli amministratori del tenant forniscono questo consenso nella stessa pagina di **autorizzazione dell'API** selezionando **Concedi al consenso dell'amministratore per l' *organizzazione***

Se si è un ISV che compila un'applicazione multi-tenant, è necessario leggere la sezione [distribuzione-caso di app daemon multi-tenant](scenario-daemon-production.md#deployment---multitenant-daemon-apps).

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="next-steps"></a>Passaggi successivi

Passare all'articolo successivo in questo scenario, configurazione del [codice dell'app](./scenario-daemon-app-configuration.md).
