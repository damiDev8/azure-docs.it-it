---
title: Autorizzare l'accesso a Configurazione app di Azure tramite Azure Active Directory
description: Abilitare RBAC di Azure per autorizzare l'accesso all'istanza di configurazione di app Azure
author: AlexandraKemperMS
ms.author: alkemper
ms.date: 05/26/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: 4768dbe292b7c71770ded1e8ad27025bc9944608
ms.sourcegitcommit: 1756a8a1485c290c46cc40bc869702b8c8454016
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2020
ms.locfileid: "96930263"
---
# <a name="authorize-access-to-azure-app-configuration-using-azure-active-directory"></a>Autorizzare l'accesso a Configurazione app di Azure tramite Azure Active Directory
Oltre all'uso di Message Authentication Code basato su hash (HMAC), app Azure configurazione supporta l'uso di Azure Active Directory (Azure AD) per autorizzare le richieste alle istanze di configurazione dell'app.  Azure AD consente di usare il controllo degli accessi in base al ruolo di Azure (RBAC di Azure) per concedere le autorizzazioni a un'entità di sicurezza.  Un'entità di sicurezza può essere un utente, un' [identità gestita](../active-directory/managed-identities-azure-resources/overview.md) o un' [entità servizio dell'applicazione](../active-directory/develop/app-objects-and-service-principals.md).  Per altre informazioni sui ruoli e sulle assegnazioni di ruolo, vedere [Informazioni sui diversi ruoli](../role-based-access-control/overview.md).

## <a name="overview"></a>Panoramica
È necessario autorizzare le richieste effettuate da un'entità di sicurezza per accedere a una risorsa di configurazione dell'app. Con Azure AD, l'accesso a una risorsa è un processo in due passaggi:
1. L'identità dell'entità di sicurezza viene autenticata e viene restituito un token OAuth 2.0.  Il nome della risorsa per richiedere un token è `https://login.microsoftonline.com/{tenantID}` dove `{tenantID}` corrisponde all'ID tenant di Azure Active Directory a cui appartiene l'entità servizio.
2. Il token viene passato come parte di una richiesta al servizio Configurazione app per autorizzare l'accesso alla risorsa specificata.

Il passaggio di autenticazione richiede che una richiesta dell'applicazione contenga un token di accesso OAuth 2.0 in fase di esecuzione.  Se un'applicazione è in esecuzione in un'entità di Azure, ad esempio un'app Funzioni di Azure, un'app Web di Azure o una macchina virtuale di Azure, può usare un'identità gestita per accedere alle risorse.  Per informazioni su come autenticare le richieste effettuate da un'identità gestita a Configurazione app di Azure, vedere [Autenticare l'accesso alle risorse di Configurazione app di Azure con Azure Active Directory e le identità gestite per le risorse di Azure](howto-integrate-azure-managed-service-identity.md).

Il passaggio di autorizzazione richiede che uno o più ruoli di Azure siano assegnati all'entità di sicurezza. App Azure configurazione fornisce i ruoli di Azure che includono i set di autorizzazioni per le risorse di configurazione dell'app. I ruoli assegnati a un'entità di sicurezza determinano le autorizzazioni fornite all'entità. Per altre informazioni sui ruoli di Azure, vedere [ruoli predefiniti di Azure per la configurazione di app Azure](#azure-built-in-roles-for-azure-app-configuration). 

## <a name="assign-azure-roles-for-access-rights"></a>Assegnare i ruoli di Azure per i diritti di accesso
Azure Active Directory (Azure AD) autorizza i diritti di accesso alle risorse protette tramite il [controllo degli accessi in base al ruolo di Azure (RBAC di Azure)](../role-based-access-control/overview.md).

Quando un ruolo di Azure viene assegnato a un'entità di sicurezza Azure AD, Azure concede l'accesso a tali risorse per l'entità di sicurezza. L'accesso è limitato alla risorsa di Configurazione app. Un'entità di sicurezza di Azure AD può essere un utente o un'entità servizio dell'applicazione oppure un'[identità gestita per le risorse di Azure](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="azure-built-in-roles-for-azure-app-configuration"></a>Ruoli predefiniti di Azure per la configurazione di app Azure
Azure fornisce i seguenti ruoli predefiniti di Azure per autorizzare l'accesso ai dati di configurazione delle app usando Azure AD e OAuth:

- **Proprietario dei dati di Configurazione dell'app**: usare questo ruolo per concedere l'accesso in lettura/scrittura/eliminazione ai dati di Configurazione app. Questo ruolo non concede l'accesso alla risorsa di Configurazione app.
- **Ruolo con autorizzazioni di lettura per i dati di Configurazione dell'app**: usare questo ruolo per concedere l'accesso in lettura ai dati di Configurazione app. Questo ruolo non concede l'accesso alla risorsa di Configurazione app.
- **Collaboratore**: usare questo ruolo per gestire la risorsa di Configurazione app. Mentre è possibile accedere ai dati di configurazione dell'app usando le chiavi di accesso, questo ruolo non concede l'accesso diretto ai dati usando Azure AD.
- **Lettore**: usare questo ruolo per concedere l'accesso in lettura alla risorsa di Configurazione app. Questo ruolo non concede l'accesso alle chiavi di accesso della risorsa né ai dati archiviati in Configurazione app.

> [!NOTE]
> Attualmente, il portale di Azure supporta solo l'autenticazione HMAC per accedere ai dati di configurazione dell'app. L'autenticazione Azure AD non è supportata. Per questo motivo, gli utenti del portale di Azure richiedono il ruolo *collaboratore* per recuperare le chiavi di accesso della risorsa di configurazione dell'app. La concessione di ruoli del *proprietario dei dati* *di configurazione dell'app o* dei dati di configurazione delle app non ha alcun effetto sull'accesso tramite il portale.

## <a name="next-steps"></a>Passaggi successivi
Altre informazioni sull'uso delle [identità gestite](howto-integrate-azure-managed-service-identity.md) per amministrare il servizio Configurazione app.
