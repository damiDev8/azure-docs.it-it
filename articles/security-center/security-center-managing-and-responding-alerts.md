---
title: Gestire gli avvisi di sicurezza nel Centro sicurezza di Azure | Documentazione Microsoft
description: Questo documento illustra come usare le funzionalità del Centro sicurezza di Azure per gestire e rispondere agli avvisi di sicurezza.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: how-to
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2020
ms.author: memildin
ms.openlocfilehash: 3f319c35631f8c85cab4613df0f1c14f98356caa
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97563348"
---
# <a name="manage-and-respond-to-security-alerts-in-azure-security-center"></a>Gestire e rispondere agli avvisi di sicurezza nel centro sicurezza di Azure

> [!TIP]
> Le informazioni contenute in questa pagina fanno riferimento alla nuova esperienza di avvisi (anteprima) disponibile nel banner nella parte superiore della pagina degli avvisi di sicurezza. 
>
> :::image type="content" source="media/security-center-managing-and-responding-alerts/preview-alerts-experience-banner.png" alt-text="Banner con collegamento alla nuova esperienza avvisi di anteprima":::

In questo argomento viene illustrato come visualizzare ed elaborare gli avvisi del Centro sicurezza e proteggere le risorse.

I rilevamenti avanzati che attivano gli avvisi di sicurezza sono disponibili solo con Azure Defender. È disponibile una versione di valutazione gratuita. Per eseguire l'aggiornamento, vedere [abilitare Azure Defender](security-center-pricing.md#enable-azure-defender).

## <a name="what-are-security-alerts"></a>Informazioni sugli avvisi di sicurezza
Il Centro sicurezza raccoglie, analizza e integra automaticamente i dati di log delle risorse di Azure, della rete e delle soluzioni dei partner connesse, come soluzioni di protezione endpoint e firewall, per rilevare le minacce reali e ridurre i falsi positivi. Il Centro sicurezza visualizza un elenco degli avvisi di sicurezza in ordine di priorità, nonché le informazioni necessarie per analizzare rapidamente il problema e indicazioni per risolvere un attacco.

Per informazioni sui diversi tipi di avvisi, vedere [avvisi di sicurezza-Guida di riferimento](alerts-reference.md).

Per una panoramica del modo in cui il Centro sicurezza genera avvisi, vedere [come il Centro sicurezza di Azure rileva e risponde alle minacce](security-center-alerts-overview.md).


## <a name="manage-your-security-alerts"></a>Gestire gli avvisi di sicurezza

1. Dalla pagina Panoramica del Centro sicurezza, selezionare il riquadro **avvisi di sicurezza** nella parte superiore della pagina o il collegamento dalla barra laterale.

    :::image type="content" source="media/security-center-managing-and-responding-alerts/overview-page-alerts-links.png" alt-text="Visualizzazione della pagina degli avvisi di sicurezza dalla pagina Panoramica del Centro sicurezza di Azure":::

    Verrà visualizzata la pagina avvisi di sicurezza.

    :::image type="content" source="media/security-center-managing-and-responding-alerts/alerts-page.png" alt-text="Elenco degli avvisi di sicurezza del Centro sicurezza di Azure":::

1. Per filtrare l'elenco degli avvisi, selezionare uno dei filtri pertinenti. Facoltativamente, è possibile aggiungere altri filtri con l'opzione **Aggiungi filtro** .

    :::image type="content" source="./media/security-center-managing-and-responding-alerts/alerts-adding-filters-small.png" alt-text="Aggiunta di filtri alla visualizzazione avvisi" lightbox="./media/security-center-managing-and-responding-alerts/alerts-adding-filters-large.png":::

    L'elenco viene aggiornato in base alle opzioni di filtro selezionate. Il filtro può essere molto utile. Ad esempio, potrebbe essere necessario gestire gli avvisi di sicurezza che si sono verificati nelle ultime 24 ore, perché si sta esaminando una potenziale violazione del sistema.


## <a name="respond-to-security-alerts"></a>Rispondere agli avvisi di sicurezza

1. Selezionare un avviso dall'elenco **avvisi di sicurezza** . Viene visualizzato un riquadro laterale con una descrizione dell'avviso e di tutte le risorse interessate. 

    :::image type="content" source="./media/security-center-managing-and-responding-alerts/alerts-details-pane.png" alt-text="Visualizzazione dettagli mini di un avviso di sicurezza":::

    > [!TIP]
    > Con questo riquadro laterale aperto, è possibile esaminare rapidamente l'elenco degli avvisi con le frecce su e giù sulla tastiera.

1. Per ulteriori informazioni, selezionare **Visualizza dettagli completi**.

    Il riquadro sinistro della pagina avviso di sicurezza Mostra informazioni di alto livello relative all'avviso di sicurezza: titolo, gravità, stato, tempo attività, descrizione dell'attività sospetta e la risorsa interessata. Insieme alla risorsa interessata sono i tag di Azure rilevanti per la risorsa. Usare questi per dedurre il contesto aziendale della risorsa quando si esamina l'avviso.

    Il riquadro destro include la scheda **Dettagli avviso** che contiene ulteriori dettagli dell'avviso che consentono di esaminare il problema: indirizzi IP, file, processi e altro ancora.
     
    ![Suggerimenti sulle operazioni da eseguire per gli avvisi di sicurezza](./media/security-center-managing-and-responding-alerts/security-center-alert-remediate.png)

    Inoltre, nel riquadro di destra è presente la scheda **azione** . Utilizzare questa scheda per eseguire ulteriori azioni relative all'avviso di sicurezza. Azioni come:
    - *Attenuazione della minaccia* : fornisce la procedura di correzione manuale per questo avviso di sicurezza
    - *Impedisci attacchi futuri* : fornisce consigli sulla sicurezza che consentono di ridurre la superficie di attacco, aumentare il comportamento di sicurezza e prevenire quindi attacchi futuri
    - *Attiva risposta automatica* : consente di attivare un'app per la logica come risposta a questo avviso di sicurezza
    - Non *visualizzare avvisi simili* : consente di disattivare gli avvisi futuri con caratteristiche simili se l'avviso non è pertinente per l'organizzazione

    ![Scheda azione](./media/security-center-managing-and-responding-alerts/alert-take-action.png)




## <a name="see-also"></a>Vedere anche

In questo documento si è appreso come visualizzare gli avvisi di sicurezza. Vedere le pagine seguenti per il materiale correlato:

- [Configurare le regole di eliminazione degli avvisi](alerts-suppression-rules.md)
- [Automatizzare le risposte ai trigger del Centro sicurezza](workflow-automation.md)
- [Guida di riferimento per gli avvisi di sicurezza](alerts-reference.md)
