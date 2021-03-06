---
title: Creare sottoscrizioni di Azure a livello di codice
description: Questo articolo illustra le opzioni disponibili per creare sottoscrizioni di Azure a livello di codice.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 01/13/2021
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 546ed24b5f9e7892f40c9d425b668f60ad705f8f
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99493889"
---
# <a name="create-azure-subscriptions-programmatically"></a>Creare sottoscrizioni di Azure a livello di codice

Questo articolo illustra le opzioni disponibili per creare sottoscrizioni di Azure a livello di codice.

Usando varie API REST è possibile creare una sottoscrizione per i tipi di contratto di Azure seguenti:

- Contratto Enterprise Agreement (EA)
- Contratto del cliente Microsoft
- Contratto Microsoft Partner

Non è possibile creare sottoscrizioni aggiuntive a livello di codice per altri tipi di contratto con le API REST.

I requisiti e i dettagli per la creazione di sottoscrizioni variano per contratti e versioni delle API differenti. Vedere gli articoli seguenti applicabili alla propria situazione:

API più recenti:

- [Creare sottoscrizioni EA](programmatically-create-subscription-enterprise-agreement.md)
- [Creare sottoscrizioni MCA](programmatically-create-subscription-microsoft-customer-agreement.md)
- [Creare sottoscrizioni MPA](programmatically-create-subscription-microsoft-partner-agreement.md)

Se si usano ancora le [API di anteprima](programmatically-create-subscription-preview.md), è possibile continuare a usarle per creare sottoscrizioni. 

Inoltre, è possibile [creare sottoscrizioni con un modello di Resource Manager](create-subscription-template.md). Un modello di Resource Manager consente di automatizzare il processo di creazione di sottoscrizioni con le API REST. 

## <a name="next-steps"></a>Passaggi successivi

* Dopo aver creato una sottoscrizione, è possibile concedere la medesima possibilità ad altri utenti ed entità servizio. Per altre informazioni, vedere [Grant access to create Azure Enterprise subscriptions (preview)](grant-access-to-create-subscription.md) (Concedere l'accesso a creare sottoscrizioni di Azure Enterprise (anteprima)).
* Per altre informazioni sulla gestione di un numero elevato di sottoscrizioni mediante i gruppi di gestione, vedere [Organizzare le risorse con i gruppi di gestione di Azure](../../governance/management-groups/overview.md).
