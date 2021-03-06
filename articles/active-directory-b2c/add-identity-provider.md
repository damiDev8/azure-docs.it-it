---
title: Aggiungere un provider di identità-Azure Active Directory B2C | Microsoft Docs
description: Informazioni su come aggiungere un provider di identità al tenant di Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 01/14/2021
ms.custom: mvc
ms.topic: how-to
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 1665f8f595e2bb9ba2a5f2c8528f85854630ab4f
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2021
ms.locfileid: "98216581"
---
# <a name="add-an-identity-provider-to-your-azure-active-directory-b2c-tenant"></a>Aggiungere un provider di identità al tenant di Azure Active Directory B2C

È possibile configurare Azure AD B2C per consentire agli utenti di accedere all'applicazione con credenziali ottenute da provider di identità di social network o aziendali. Azure AD B2C supporta provider di identità esterni come Facebook, account Microsoft, Google, Twitter e qualsiasi provider di identità che supporta i protocolli OAuth 1.0, OAuth 2.0, OpenID Connect e SAML.

Con la federazione dei provider di identità esterni è possibile offrire agli utenti la possibilità di accedere con i propri account social o aziendali senza dover creare un nuovo account solo per l'applicazione.

Nella pagina di iscrizione o accesso Azure AD B2C presenta un elenco di provider di identità esterni tra cui l'utente può scegliere per eseguire l'accesso. Dopo aver selezionato uno dei provider di identità esterni, l'utente viene reindirizzato al sito Web del provider selezionato per completare il processo di accesso. Dopo l'accesso, l'utente viene reindirizzato ad Azure AD B2C per l'autenticazione dell'account nell'applicazione.

![Esempio di accesso tramite dispositivo mobile con un account di social network (Facebook)](media/add-identity-provider/external-idp.png)

È possibile aggiungere provider di identità supportati da Azure Active Directory B2C (Azure AD B2C) ai propri [flussi utente](user-flow-overview.md) usando il portale di Azure. È anche possibile aggiungere provider di identità ai [criteri personalizzati](custom-policy-get-started.md).

## <a name="select-an-identity-provider"></a>Selezione di un provider di identità

In genere si usa un solo provider di identità nelle applicazioni, ma è possibile aggiungerne altri. Gli articoli di procedure seguenti illustrano come creare l'applicazione del provider di identità, aggiungere il provider di identità al tenant e aggiungere il provider di identità al flusso utente o ai criteri personalizzati.

* [AD FS](identity-provider-adfs.md)
* [Amazon](identity-provider-amazon.md)
* [Azure AD (tenant singolo)](identity-provider-azure-ad-single-tenant.md)
* [Azure AD (multi-tenant)](identity-provider-azure-ad-multi-tenant.md)
* [Azure AD B2C](identity-provider-azure-ad-b2c.md)
* [Facebook](identity-provider-facebook.md)
* [Provider di identità generico](identity-provider-generic-openid-connect.md)
* [GitHub](identity-provider-github.md)
* [ID.me](identity-provider-id-me.md)
* [Google](identity-provider-google.md)
* [LinkedIn](identity-provider-linkedin.md)
* [Account Microsoft](identity-provider-microsoft-account.md)
* [QQ](identity-provider-qq.md)
* [Salesforce](identity-provider-salesforce.md)
* [Salesforce (protocollo SAML)](identity-provider-salesforce-saml.md)
* [Twitter](identity-provider-twitter.md)
* [WeChat](identity-provider-wechat.md)
* [Weibo](identity-provider-weibo.md)
