---
title: includere file
description: includere file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8fba8aa577bcb3b5ef44d57c388a1f1de7494782
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "95995617"
---
Se si desidera che agli utenti venga richiesto un secondo fattore di autenticazione prima di concedere l'accesso, è possibile configurare Azure AD Multi-Factor Authentication (AMF). È possibile configurare l'autenticazione a più fattori per ogni utente oppure è possibile sfruttare l'autenticazione a più fattori tramite [l'accesso condizionale](../articles/active-directory/conditional-access/overview.md).

* L'autenticazione a più fattori per utente può essere abilitata senza costi aggiuntivi. Quando si Abilita l'autenticazione a più fattori per utente, all'utente verrà richiesto di eseguire l'autenticazione a due fattori rispetto a tutte le applicazioni associate al tenant Azure AD. Per la procedura, vedere l' [opzione 1](#peruser) .
* L'accesso condizionale consente un controllo più granulare sulla modalità di promozione di un secondo fattore. Può consentire l'assegnazione dell'autenticazione a più fattori solo alla VPN ed escludere altre applicazioni legate al tenant Azure AD. Per la procedura, vedere l' [opzione 2](#conditional) .