---
title: Configurare località denominate in Azure Active Directory | Microsoft Docs
description: Informazioni su come configurare località denominate.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.subservice: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94f4d17596936dd9d0ebbdae3c351cac9ed2a570
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/05/2020
ms.locfileid: "89299865"
---
# <a name="quickstart-configure-named-locations-in-azure-active-directory"></a>Guida introduttiva: Configurare località denominate in Azure Active Directory

Con le località denominate è possibile etichettare gli intervalli di indirizzi IP attendibili all'interno dell'organizzazione. Azure AD usa le località denominate per:
- Rilevare i falsi positivi nei [rilevamenti dei rischi](../identity-protection/overview-identity-protection.md). L'accesso da una posizione attendibile riduce il rischio di accesso dell'utente.   
- Configurare l'[Accesso condizionale basato sulla località](../conditional-access/location-condition.md).

In questa guida introduttiva si apprenderà come configurare le località denominate nell'ambiente in uso.

## <a name="prerequisites"></a>Prerequisiti

Per completare l'esercitazione introduttiva, sono necessari gli elementi seguenti:

* Un tenant di Azure AD. Iscriversi per ottenere una [versione di valutazione gratuita](https://azure.microsoft.com/trial/get-started-active-directory/). 
* Un utente con il ruolo di amministratore globale per il tenant.
* Un intervallo IP stabilito e attendibile nell'organizzazione. L'intervallo IP deve essere in formato **CIDR (Classless Inter-Domain Routing)** .

## <a name="configure-named-locations"></a>Configurare località denominate

1. Accedere al [portale di Azure](https://portal.azure.com).

2. Nel riquadro sinistro selezionare **Azure Active Directory**, quindi selezionare **Accesso condizionale** nella sezione **Sicurezza**.

    ![Scheda Accesso condizionale](./media/quickstart-configure-named-locations/entrypoint.png)

3. Nella pagina **Accesso condizionale** selezionare **Località denominate** e quindi **Nuova località**.

    ![Località denominate](./media/quickstart-configure-named-locations/namedlocation.png)

6. Compilare il modulo nella pagina nuova. 

   * Nella casella **Nome** digitare un nome per la località denominata.
   * Nella casella **Intervalli IP** digitare l'intervallo IP in formato CIDR.  
   * Fare clic su **Crea**.
    
     ![Pannello Nuovo](./media/quickstart-configure-named-locations/61.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere:

- [Località come condizione nell’accesso condizionale](../conditional-access/concept-conditional-access-conditions.md#locations).
- [Report sugli accessi a rischio](../identity-protection/overview-identity-protection.md).