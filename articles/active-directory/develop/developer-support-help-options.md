---
title: Opzioni di supporto e guida per gli sviluppatori di app Azure AD
description: Scoprire come ottenere assistenza e supporto per domande e problemi correlati allo sviluppo durante la creazione di applicazioni che si integrano con le identità di Microsoft (Azure Active Directory e account Microsoft)
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/23/2019
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: bce9479d063d091eb4fa68d2452d8a4218d45db9
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2021
ms.locfileid: "99219944"
---
# <a name="support-and-help-options-for-developers"></a>Opzioni di supporto tecnico e assistenza per gli sviluppatori

Se si stia iniziando l'integrazione con Azure Active Directory (Azure AD), con le identità di Microsoft o con l'API Microsoft Graph o se si sta implementando una nuova funzionalità nell'applicazione, in alcuni momenti è necessario l'aiuto della community o la comprensione approfondita delle opzioni di supporto disponibili per gli sviluppatori. Questo articolo aiuta a comprendere queste opzioni, tra cui:

> [!div class="checklist"]
> * Come eseguire una ricerca per verificare se la community non ha ancora risposto alla domanda che si vuole porre o se esiste già la documentazione della funzionalità che si sta provando a implementare
> * In alcuni casi si vogliono solo usare gli strumenti di supporto che consentono di eseguire il debug per un problema specifico
> * Se non si riesce a trovare la risposta necessaria, è possibile porre una domanda su *Microsoft Q&a*
> * In caso di problemi con una delle librerie di autenticazione Microsoft, segnalare il problema in *GitHub*
> * Se, infine, è necessario contattare direttamente il personale di assistenza, è consigliabile aprire una richiesta di supporto

## <a name="search"></a>Cerca

In caso di domande relative allo sviluppo, potrebbe essere possibile trovare la risposta nella documentazione, [esempi di GitHub](https://github.com/azure-samples)o risposte a [Microsoft Q&](https://docs.microsoft.com/answers/products/) domande.

### <a name="scoped-search"></a>Ricerca per ambito

Per ottenere risultati più rapidi, definire l'ambito della ricerca a [Microsoft Q&](https://docs.microsoft.com/answers/products/)la documentazione e gli esempi di codice usando la query seguente nel motore di ricerca preferito:

```
{Your Search Terms} (site:http://www.docs.microsoft.com/answers/products/ OR site:docs.microsoft.com OR site:github.com/azure-samples OR site:cloudidentity.com OR site:developer.microsoft.com/graph)
```

Dove *{Your Search Terms}* corrisponde alle parole chiave di ricerca.

## <a name="use-the-development-support-tools"></a>Usare gli strumenti di supporto Microsoft per lo sviluppo

| Strumento  | Descrizione  |
|---------|---------|
| [jwt.ms](https://jwt.ms) | Incollare un ID o un token di accesso per decodificare i nomi e i valori di un'attestazione. |
| [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)| Strumento che consente di effettuare richieste e visualizzare le risposte con l'API Microsoft Graph. |

## <a name="post-a-question-to-microsoft-qa"></a>Inviare una domanda a Microsoft Q&A

[Microsoft Q&A](https://docs.microsoft.com/answers/products/) è il canale preferito per le domande relative allo sviluppo. Qui, membri della community degli sviluppatori e membri dei team Microsoft sono direttamente coinvolti nell'assistenza agli utenti per la risoluzione dei problemi.

Se non si riesce a trovare una risposta alla domanda tramite la ricerca, inviare una nuova domanda a [Microsoft Q&a](https://docs.microsoft.com/answers/products/) . Per porre domande alla community in modo che possa identificarle e rispondere più rapidamente, usare uno dei tag seguenti:

|Componente/area  | Tag |
|---------|---------|
| Libreria ADAL | [adal](https://docs.microsoft.com/answers/topics/azure-ad-adal-deprecation.html) |
| Libreria MSAL     | [MSAL](https://docs.microsoft.com/answers/topics/azure-ad-msal.html) |
| Middleware OWIN  | [[Azure-Active-Directory]](https://docs.microsoft.com/answers/topics/azure-active-directory.html) |
| [Azure B2B](../external-identities/what-is-b2b.md)  | [[azure-ad-b2b]](https://docs.microsoft.com/answers/topics/azure-ad-b2b.html) |
| [Azure B2C](https://azure.microsoft.com/services/active-directory-b2c/)  | [[azure-ad-b2c]](https://docs.microsoft.com/answers/topics/azure-ad-b2c.html) |
| [API Microsoft Graph](https://developer.microsoft.com/graph/) | [[Azure-AD-Graph]](https://docs.microsoft.com/answers/topics/azure-ad-graph.html) |
| Qualsiasi altra area correlata ad argomenti relativi all'autenticazione o all'autorizzazione | [[Azure-Active-Directory]](https://docs.microsoft.com/answers/topics/azure-active-directory.html) |

I post seguenti di [Microsoft Q&A](https://docs.microsoft.com/answers/products/) contengono suggerimenti su come porre domande e su come aggiungere il codice sorgente. Seguire queste linee guida per aumentare la probabilità che i membri della community valutino le domande e rispondano rapidamente:

* [Ricerca per categorie porre una domanda corretta](https://docs.microsoft.com/answers/articles/24951/how-to-write-a-quality-question.html)
* [Come creare un esempio minimo, completo e verificabile](https://docs.microsoft.com/answers/articles/24907/how-to-write-a-quality-answer.html)

## <a name="create-a-github-issue"></a>Segnalare un problema in GitHub

In caso di bug o di problema relativo alle librerie Microsoft, segnalare il problema nei repository di GitHub. Le librerie sono open source, quindi gli utenti possono anche inviare anche una richiesta pull.

Per un elenco di librerie e dei rispettivi repository GitHub, vedere gli argomenti seguenti:

* Librerie di [Azure Active Directory Authentication Library (adal)](../azuread-dev/active-directory-authentication-libraries.md) e repository GitHub
* Librerie di [Microsoft Authentication Library (MSAL)](reference-v2-libraries.md) e repository GitHub

## <a name="open-a-support-request"></a>Aprire una richiesta di supporto

Se è necessario rivolgersi a qualcuno, è consigliabile aprire una richiesta di supporto. Per i clienti Azure sono disponibili diverse opzioni di supporto. Per confrontare i piani, vedere [questa pagina](https://azure.microsoft.com/support/plans/). Il supporto per gli sviluppatori è disponibile anche per i clienti Azure. Per informazioni su come acquistare piani di supporto per sviluppatori, vedere [questa pagina](https://azure.microsoft.com/support/plans/developer/).

* Se si ha già un piano di supporto per Azure, [aprire una richiesta di supporto qui](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)

* Se l'utente non è un cliente Azure, è anche possibile aprire una richiesta di supporto con Microsoft tramite [il supporto commerciale](https://support.serviceshub.microsoft.com/supportforbusiness).

Per ottenere supporto o porre domande, è anche possibile provare un [agente virtuale](https://support.microsoft.com/contactus/?ws=support).