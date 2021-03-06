---
title: Connettere i dati del firewall di Sophos XG ad Azure Sentinel | Microsoft Docs
description: Informazioni su come connettere i dati del firewall di Sophos XG ad Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: b48773272e050b47af73b197d6f1c8156318fd71
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "87099345"
---
# <a name="connect-your-sophos-xg-firewall-to-azure-sentinel"></a>Connettere il firewall di Sophos XG ad Azure Sentinel

> [!IMPORTANT]
> Il connettore dati di Sophos XG firewall in Azure Sentinel è attualmente disponibile in anteprima pubblica.
> Questa funzionalità viene fornita senza un contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate. Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Questo articolo illustra come connettere il dispositivo di [Sophos XG firewall](https://www.sophos.com/products/next-gen-firewall.aspx) ad Azure Sentinel. Sophos XG Firewall Data Connector consente di connettere facilmente i log del firewall di Sophos XG ad Azure Sentinel, visualizzare i dashboard, creare avvisi personalizzati e migliorare l'analisi. L'integrazione tra il firewall di Sophos XG e Azure Sentinel usa syslog.

> [!NOTE]
> I dati verranno archiviati nella posizione geografica dell'area di lavoro in cui viene eseguito Azure Sentinel.

## <a name="forward-sophos-xg-firewall-logs-to-the-syslog-agent"></a>Inviare i log del firewall di Sophos XG all'agente syslog  

Configurare il firewall Sophos XG per l'invio dei messaggi syslog all'area di lavoro di Azure tramite l'agente syslog.

1. Nel portale di Azure Sentinel, fare clic su **connettori dati** e selezionare **Sophos XG firewall** Connector.

1. Selezionare **Apri connettore pagina**.

1. Seguire le istruzioni nella pagina del **firewall di Sophos XG** .

## <a name="find-your-data"></a>Trovare i dati

Una volta stabilita la connessione, i dati vengono visualizzati in Log Analytics in syslog.

## <a name="validate-connectivity"></a>Convalidare la connettività

Potrebbero essere necessari fino a 20 minuti prima che i log si avviino in Log Analytics.

## <a name="next-steps"></a>Passaggi successivi

In questo documento si è appreso come connettere il firewall Sophos XG ad Azure Sentinel. Per altre informazioni su Azure Sentinel, vedere gli articoli seguenti:

- Informazioni su come [ottenere visibilità sui dati e sulle potenziali minacce](quickstart-get-visibility.md).
- Iniziare a [rilevare minacce con Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Usare le cartelle di lavoro](tutorial-monitor-your-data.md) per monitorare i dati.