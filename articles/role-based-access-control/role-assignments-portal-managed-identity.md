---
title: Aggiungere un'assegnazione di ruolo per un'identità gestita (anteprima)-RBAC di Azure
description: Informazioni su come aggiungere un'assegnazione di ruolo iniziando dall'identità gestita e quindi selezionare l'ambito e il ruolo usando il portale di Azure e il controllo degli accessi in base al ruolo di Azure (RBAC di Azure).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 01/11/2021
ms.author: rolyon
ms.openlocfilehash: a01246c0cf35653f4d13262183cf9df28b056c69
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2021
ms.locfileid: "98122240"
---
# <a name="add-a-role-assignment-for-a-managed-identity-preview"></a>Aggiungere un'assegnazione di ruolo per un'identità gestita (anteprima)

È possibile aggiungere assegnazioni di ruolo per un'identità gestita usando la pagina **controllo di accesso (IAM)** come descritto in [aggiungere o rimuovere assegnazioni di ruolo di Azure usando il portale di Azure](role-assignments-portal.md). Quando si usa la pagina controllo di accesso (IAM), si inizia con l'ambito e quindi si seleziona l'identità e il ruolo gestiti. Questo articolo descrive un modo alternativo per aggiungere assegnazioni di ruolo per un'identità gestita. Utilizzando questa procedura, si inizia con l'identità gestita e quindi si selezionano l'ambito e il ruolo.

> [!IMPORTANT]
> L'aggiunta di un'assegnazione di ruolo per un'identità gestita usando questi passaggi alternativi è attualmente in anteprima.
> Questa versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate.
> Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [Azure role assignment prerequisites](../../includes/role-based-access-control/prerequisites-role-assignments.md)]

## <a name="system-assigned-managed-identity"></a>Identità gestita assegnata dal sistema

Seguire questa procedura per assegnare un ruolo a un'identità gestita assegnata dal sistema iniziando con l'identità gestita.

1. Nella portale di Azure aprire un'identità gestita assegnata dal sistema.

1. Nel menu a sinistra fare clic su **Identity**.

    ![Identità gestita assegnata dal sistema](./media/shared/identity-system-assigned.png)

1. In **autorizzazioni** fare clic su **assegnazioni di ruolo di Azure**.

    Se i ruoli sono già stati assegnati all'identità gestita assegnata dal sistema selezionata, viene visualizzato l'elenco di assegnazioni di ruolo. Questo elenco include tutte le assegnazioni di ruolo per le quali si dispone dell'autorizzazione di lettura.

    ![Assegnazioni di ruolo per un'identità gestita assegnata dal sistema](./media/shared/role-assignments-system-assigned.png)

1. Per modificare la sottoscrizione, fare clic sull'elenco **sottoscrizione** .

1. Fare clic su **Aggiungi assegnazione ruolo (anteprima)**.

1. Usare gli elenchi a discesa per selezionare il set di risorse a cui si applica l'assegnazione di ruolo, ad esempio **sottoscrizione**, **gruppo di risorse** o risorsa.

    Se non si dispone delle autorizzazioni di scrittura per l'assegnazione di ruolo per l'ambito selezionato, verrà visualizzato un messaggio inline. 

1. Nell'elenco a discesa **Ruolo** selezionare un ruolo, ad esempio **Collaboratore Macchina virtuale**.

   ![Riquadro Aggiungi assegnazione ruolo per identità gestita assegnata dal sistema](./media/role-assignments-portal-managed-identity/add-role-assignment-with-scope.png)

1. Fare clic su **Salva** per assegnare un ruolo.

   Dopo alcuni istanti, l'identità gestita viene assegnata al ruolo nell'ambito selezionato.

## <a name="user-assigned-managed-identity"></a>Identità gestita assegnata dall'utente

Seguire questa procedura per assegnare un ruolo a un'identità gestita assegnata dall'utente iniziando con l'identità gestita.

1. Nella portale di Azure aprire un'identità gestita assegnata dall'utente.

1. Nel menu a sinistra fare clic su **assegnazioni di ruolo di Azure**.

    Se i ruoli sono già stati assegnati all'identità gestita assegnata dall'utente selezionata, viene visualizzato l'elenco di assegnazioni di ruolo. Questo elenco include tutte le assegnazioni di ruolo per le quali si dispone dell'autorizzazione di lettura.

    ![Assegnazioni di ruolo per un'identità gestita assegnata dall'utente](./media/shared/role-assignments-user-assigned.png)

1. Per modificare la sottoscrizione, fare clic sull'elenco **sottoscrizione** .

1. Fare clic su **Aggiungi assegnazione ruolo (anteprima)**.

1. Usare gli elenchi a discesa per selezionare il set di risorse a cui si applica l'assegnazione di ruolo, ad esempio **sottoscrizione**, **gruppo di risorse** o risorsa.

    Se non si dispone delle autorizzazioni di scrittura per l'assegnazione di ruolo per l'ambito selezionato, verrà visualizzato un messaggio inline. 

1. Nell'elenco a discesa **Ruolo** selezionare un ruolo, ad esempio **Collaboratore Macchina virtuale**.

   ![Riquadro Aggiungi assegnazione ruolo per un'identità gestita assegnata dall'utente](./media/role-assignments-portal-managed-identity/add-role-assignment-with-scope.png)

1. Fare clic su **Salva** per assegnare un ruolo.

   Dopo alcuni istanti, l'identità gestita viene assegnata al ruolo nell'ambito selezionato.

## <a name="next-steps"></a>Passaggi successivi

- [Informazioni sulle identità gestite per le risorse di Azure](../active-directory/managed-identities-azure-resources/overview.md)
- [Aggiungere o rimuovere assegnazioni di ruolo di Azure usando il portale di Azure](role-assignments-portal.md)
- [Elencare le assegnazioni di ruolo di Azure usando il portale di Azure](role-assignments-list-portal.md)
