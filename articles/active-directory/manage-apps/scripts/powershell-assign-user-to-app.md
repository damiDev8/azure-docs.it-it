---
title: Esempio di PowerShell - Assegnare un utente a un'app di Application Proxy
description: Esempio di PowerShell che assegna un utente a un'applicazione di Azure Active Directory (Azure AD) Application Proxy.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: c725d0a19dd549d1f7a90efa9edec4cae7b9ea9d
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99258508"
---
# <a name="assign-a-user-to-a-specific-azure-ad-application-proxy-application"></a>Assegnare un utente a un'applicazione specifica di Azure AD Application Proxy

Questo script di esempio di PowerShell consente di assegnare un utente a un'applicazione specifica di Azure AD Application Proxy.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Per questo esempio è richiesto il [modulo AzureAD V2 PowerShell per Graph](/powershell/azure/active-directory/install-adv2) (AzureAD) o la [versione di anteprima del modulo AzureAD V2 PowerShell per Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview).

## <a name="sample-script"></a>Script di esempio

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/assign-user-to-app.ps1 "Assign a user to an application")]

## <a name="script-explanation"></a>Spiegazione dello script

| Comando | Note |
|---|---|
| [New-AzureADUserAppRoleAssignment](/powershell/module/AzureAD/new-azureaduserapproleassignment) | Assegna un utente a un ruolo applicazione. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sul modulo Azure AD PowerShell, vedere la [panoramica del modulo di Azure AD PowerShell](/powershell/azure/active-directory/overview).

Per altri esempi di PowerShell relativi ad Application Proxy, vedere [Esempi di Azure AD PowerShell per Azure AD Application Proxy](../application-proxy-powershell-samples.md).
