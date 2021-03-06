---
title: Risolvere gli errori di creazione di risorse in Azure HDInsight
description: In questo articolo sono disponibili gli errori e le tecniche di mitigazione comuni relativi alla capacità.
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: seoapr2020
ms.date: 04/22/2020
ms.openlocfilehash: bad934e02184c46c19dcda91e18e7c7ce2f1c884
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/28/2021
ms.locfileid: "98944653"
---
# <a name="troubleshoot-resource-creation-failures-in-azure-hdinsight"></a>Risolvere gli errori di creazione di risorse in Azure HDInsight

Questo articolo descrive le procedure di risoluzione dei problemi e le possibili soluzioni per i problemi relativi all'interazione con Azure HDInsight.

## <a name="error-the-deployment-would-exceed-the-quota-of-800"></a>Errore: la distribuzione supererà la quota di ' 800'

Azure prevede un limite di quota di 800 distribuzioni per gruppo di risorse. Si applicano quote diverse per gruppo di risorse, sottoscrizione, account o per altri ambiti. Ad esempio, la sottoscrizione potrebbe essere configurata in modo da limitare il numero di core per un'area. Il tentativo di distribuire una macchina virtuale con un numero di core superiore a quello consentito restituisce un messaggio di errore che informa che la quota è stata superata.

Per risolvere questo problema, eliminare le distribuzioni che non sono più necessarie usando il portale di Azure, l'interfaccia della riga di comando o PowerShell.

Per altre informazioni, vedere [Risolvere gli errori delle quote di risorse](../azure-resource-manager/templates/error-resource-quota.md).

## <a name="error-the-maximum-node-exceeded-the-available-cores-in-this-region"></a>Errore: il nodo massimo supera i core disponibili in questa area

La sottoscrizione potrebbe essere configurata in modo da limitare il numero di core per un'area. Il tentativo di distribuire una risorsa con un numero di core superiore a quello consentito restituisce un messaggio di errore che informa che la quota è stata superata.

Per richiedere un aumento delle quote, seguire questi passaggi:

1. Passare alla [portale di Azure](https://portal.azure.com)e selezionare Guida e **supporto**.

1. Selezionare **Nuova richiesta di supporto**.

1. Nella scheda informazioni di **base** della pagina **nuova richiesta di supporto** specificare le informazioni seguenti:

   * **Tipo di problema:** Selezionare i **limiti del servizio e della sottoscrizione (quote)**.
   * **Sottoscrizione:** Selezionare la sottoscrizione che si desidera modificare.
   * **Tipo di quota:** Selezionare **HDInsight**.

Per altre informazioni, vedere [Creare un ticket di supporto per aumentare i core](hdinsight-capacity-planning.md#quotas).

## <a name="next-steps"></a>Passaggi successivi

[!INCLUDE [troubleshooting next steps](../../includes/hdinsight-troubleshooting-next-steps.md)]