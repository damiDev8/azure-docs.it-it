---
title: Connettere i dati di protezione DDoS di Azure ad Azure Sentinel
description: Informazioni su come inserire i dati di protezione DDoS di Azure in Sentinel di Azure, in modo che sia possibile visualizzarli, analizzarli ed esaminarli.
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: how-to
ms.date: 01/20/2021
ms.author: yelevin
ms.openlocfilehash: 8089b1e74e88db81c1c15ad2cbf2072abcfff241
ms.sourcegitcommit: a0c1d0d0906585f5fdb2aaabe6f202acf2e22cfc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2021
ms.locfileid: "98621346"
---
# <a name="connect-data-from-azure-ddos-protection"></a>Connettere i dati da protezione DDoS di Azure

Gli attacchi Distributed Denial of Service (DDoS) tentano di esaurire le risorse di un'applicazione, rendendo l'applicazione non disponibile per gli utenti legittimi. Gli attacchi DDoS possono avere come obiettivo qualsiasi endpoint che è raggiungibile pubblicamente tramite Internet. [Protezione DDoS di Azure](../ddos-protection/ddos-protection-overview.md), in combinazione con le procedure consigliate per la progettazione di applicazioni, offre una difesa efficace contro gli attacchi DDoS. È possibile connettere i log di protezione DDoS di Azure ad Azure Sentinel, consentendo di visualizzare i dati di log nelle cartelle di lavoro, usarli per creare avvisi personalizzati e incorporarli per migliorare le indagini. 

## <a name="prerequisites"></a>Prerequisiti

- È necessario disporre delle autorizzazioni di lettura e scrittura per l'area di lavoro di Azure Sentinel.

- È necessario avere un [piano di protezione standard DDoS di Azure](../ddos-protection/manage-ddos-protection.md#create-a-ddos-protection-plan)configurato.

- È necessario disporre di una [rete virtuale configurata con lo standard DDoS di Azure abilitato](../ddos-protection/manage-ddos-protection.md#enable-ddos-protection-for-a-new-virtual-network).

## <a name="connect-to-azure-ddos-protection"></a>Connettersi ad Azure DDoS Protection
    
1. Dal menu di navigazione di Azure Sentinel selezionare **connettori dati**.

1. Selezionare **protezione DDoS di Azure** dalla raccolta Data Connector e quindi selezionare **Apri pagina connettore** nel riquadro di anteprima.

1. Abilitare i **log di diagnostica** in tutti gli indirizzi IP pubblici i cui log si desidera connettere:

    1. Selezionare il collegamento **Apri impostazioni di diagnostica >** e scegliere una risorsa **indirizzo IP pubblico** dall'elenco.

    1. Fare clic su **+ Aggiungi impostazione di diagnostica**.

    1. Nella schermata **impostazioni di diagnostica** :
       - Immettere un nome nel campo  **Nome impostazione di diagnostica** .

       - Contrassegnare la casella **di controllo Invia a log Analytics** . Sotto di essa verranno visualizzati due nuovi campi. Scegliere la **sottoscrizione** pertinente e l' **area di lavoro log Analytics** (in cui risiede Azure Sentinel).

       - Contrassegnare le caselle di controllo dei tipi di regole di cui si desidera inserire i log. È consigliabile usare **DDoSProtectionNotifications**, **DDoSMitigationFlowLogs** e **DDoSMitigationReports**.

    1. Fare clic su **Salva** nella parte superiore della schermata. Ripetere questa procedura per eventuali firewall aggiuntivi (indirizzi IP pubblici) per i quali è stata abilitata la protezione DDoS.

1. Per usare lo schema pertinente in Log Analytics per gli avvisi di protezione DDoS di Azure, cercare **AzureDiagnostics**.

> [!NOTE]
>
> Con questo particolare connettore dati, gli indicatori di stato della connettività (uno stripe di colori nella raccolta di connettori dati e nelle icone di connessione accanto ai nomi dei tipi di dati) vengono visualizzati come connessi (verde) solo se i dati sono stati *inseriti* in un determinato momento nelle ultime due settimane. Una volta passate due settimane senza inserimento dati, il connettore verrà visualizzato come disconnesso. Il momento in cui arrivano più dati, lo stato *connesso* restituirà.

## <a name="next-steps"></a>Passaggi successivi

In questo documento si è appreso come connettere i log di protezione DDoS di Azure ad Azure Sentinel. Per altre informazioni su Azure Sentinel, vedere gli articoli seguenti:
- Informazioni su come [ottenere visibilità sui dati e sulle potenziali minacce](quickstart-get-visibility.md).
- Iniziare a [rilevare minacce con Azure Sentinel](tutorial-detect-threats-built-in.md).
