---
title: Visualizzare i log e le metriche di ricerca cognitiva di Azure con Power BI
description: Visualizzare i log e le metriche di ricerca cognitiva di Azure con Power BI
manager: eladz
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/25/2020
ms.openlocfilehash: 4056e892855c06ce6c412ec4a592ebcd97fc11a6
ms.sourcegitcommit: 4295037553d1e407edeb719a3699f0567ebf4293
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/30/2020
ms.locfileid: "96325384"
---
# <a name="visualize-azure-cognitive-search-logs-and-metrics-with-power-bi"></a>Visualizzare i log e le metriche di ricerca cognitiva di Azure con Power BI
[Azure ricerca cognitiva](./search-what-is-azure-search.md) consente di archiviare i log delle operazioni e le metriche dei servizi relativi al servizio di ricerca in un account di archiviazione di Azure. In questa pagina vengono fornite istruzioni su come è possibile visualizzare tali informazioni tramite un'app modello di Power BI. L'app fornisce informazioni dettagliate sul servizio di ricerca, incluse informazioni su ricerca, indicizzazione, operazioni e metriche del servizio.

È possibile trovare l'app modello di Power BI **Azure ricerca cognitiva: analizzare i log e le metriche** nel [Marketplace delle app di Power bi](https://appsource.microsoft.com/marketplace/apps).

## <a name="how-to-get-started-with-the-app"></a>Come iniziare a usare l'app

1. Abilitare le metriche e la registrazione delle risorse per il servizio di ricerca:

    1. Creare o identificare un [account di archiviazione di Azure](../storage/common/storage-account-create.md) esistente in cui è possibile archiviare i log
    1. Passare al servizio ricerca cognitiva di Azure nella portale di Azure
    1. Nella sezione monitoraggio della colonna a sinistra selezionare impostazioni di **diagnostica** .

        :::image type="content" source="media/search-monitor-logs-powerbi/diagnostic-settings.png" alt-text="Screenshot che illustra come selezionare le impostazioni di diagnostica nella sezione monitoraggio del servizio ricerca cognitiva di Azure." border="false":::

    1. Selezionare **+ Aggiungi impostazione di diagnostica**
    1. Selezionare **archivia in un account di archiviazione**, specificare le informazioni sull'account di archiviazione e controllare **OperationLogs** e **AllMetrics**

        :::image type="content" source="media/search-monitor-logs-powerbi/add-diagnostic-setting.png" alt-text="Screenshot che illustra come effettuare le selezioni per le metriche e la registrazione delle risorse nella pagina impostazioni di diagnostica.":::
    1. Selezionare **Salva**

1. Dopo aver abilitato la registrazione, usare il servizio di ricerca per iniziare a generare log e metriche. Richiede fino a un'ora prima che i contenitori vengano visualizzati nell'archivio BLOB con questi log. Viene visualizzato un contenitore **Insights-logs-operationlogs** per i log del traffico di ricerca e un contenitore **Insights-Metrics-PT1M** per le metriche.

1. Trovare l'app Azure ricerca cognitiva Power BI nel [Marketplace delle app Power bi](https://appsource.microsoft.com/marketplace/apps) e installarla in una nuova area di lavoro o in un'area di lavoro esistente. L'app è denominata **Azure ricerca cognitiva: analizzare i log e le metriche**.

1. Dopo aver installato l'app, selezionarla dall'elenco di app in Power BI.

    :::image type="content" source="media/search-monitor-logs-powerbi/azure-search-app-tile.png" alt-text="Screenshot che illustra l'app Azure ricerca cognitiva per effettuare una selezione dall'elenco di app.":::

1. Selezionare **Connetti** per connettere i dati

    :::image type="content" source="media/search-monitor-logs-powerbi/get-started-with-your-new-app.png" alt-text="Screenshot che illustra come connettersi ai dati nell'app ricerca cognitiva di Azure.":::

1. Immettere il nome dell'account di archiviazione che contiene i log e le metriche. Per impostazione predefinita, l'app esaminerà gli ultimi 10 giorni di dati, ma questo valore può essere modificato con il parametro **Days** .

    :::image type="content" source="media/search-monitor-logs-powerbi/connect-to-storage-account.png" alt-text="Screenshot che illustra come inserire il nome dell'account di archiviazione e il numero di giorni per eseguire una query nella pagina connessione ad Azure ricerca cognitiva.":::

1. Selezionare **chiave** come metodo di autenticazione e specificare la chiave dell'account di archiviazione. Selezionare **privato** come livello di privacy. Fare clic su Accedi per iniziare il processo di caricamento.

    :::image type="content" source="media/search-monitor-logs-powerbi/connect-to-storage-account-step-two.png" alt-text="Screenshot che illustra come inserire il metodo di autenticazione, la chiave dell'account e il livello di privacy nella pagina connessione ad Azure ricerca cognitiva.":::

1. Attendere l'aggiornamento dei dati. Questa operazione può richiedere del tempo a seconda della quantità di dati. È possibile verificare se i dati sono ancora aggiornati in base all'indicatore seguente.

    :::image type="content" source="media/search-monitor-logs-powerbi/workspace-view-refreshing.png" alt-text="Screenshot che illustra come leggere le informazioni nella pagina di aggiornamento dati.":::

1. Al termine dell'aggiornamento dei dati, selezionare **Azure ricerca cognitiva report** per visualizzare il report.

    :::image type="content" source="media/search-monitor-logs-powerbi/workspace-view-select-report.png" alt-text="Screenshot che illustra come selezionare il report ricerca cognitiva di Azure nella pagina aggiornamento dati.":::

1. Assicurarsi di aggiornare la pagina dopo avere aperto il report in modo che venga aperto con i dati.

    :::image type="content" source="media/search-monitor-logs-powerbi/powerbi-search.png" alt-text="Screenshot del report Power BI di Azure ricerca cognitiva.":::

## <a name="how-to-change-the-app-parameters"></a>Come modificare i parametri dell'app
Se si desidera visualizzare i dati da un account di archiviazione diverso o modificare il numero di giorni di dati su cui eseguire la query, attenersi ai passaggi seguenti per modificare i parametri **Days** e **StorageAccount** .

1. Passare alle app Power BI, trovare l'app Azure ricerca cognitiva e selezionare il pulsante **modifica app** per visualizzare l'area di lavoro.

    :::image type="content" source="media/search-monitor-logs-powerbi/azure-search-app-tile-edit.png" alt-text="Screenshot che illustra come selezionare il pulsante modifica app per l'app Azure ricerca cognitiva.":::

1. Selezionare **Impostazioni** dalle opzioni del set di dati.

    :::image type="content" source="media/search-monitor-logs-powerbi/workspace-view-select-settings.png" alt-text="Screenshot che illustra come selezionare le impostazioni dalle opzioni del set di dati di Azure ricerca cognitiva.":::

1. Nella scheda DataSets (set di impostazioni) modificare i valori dei parametri e selezionare **Apply (applica**). Se si verifica un problema con la connessione, aggiornare le credenziali dell'origine dati nella stessa pagina.

1. Tornare all'area di lavoro e selezionare **Aggiorna ora** dalle opzioni del set di dati.

    :::image type="content" source="media/search-monitor-logs-powerbi/workspace-view-select-refresh-now.png" alt-text="Screenshot che illustra come selezionare Aggiorna ora dalle opzioni del set di dati di Azure ricerca cognitiva.":::

1. Aprire il report per visualizzare i dati aggiornati. Potrebbe inoltre essere necessario aggiornare il report per visualizzare i dati più recenti.

## <a name="troubleshooting"></a>Risoluzione dei problemi
Se non è possibile visualizzare i dati, seguire questa procedura di risoluzione dei problemi:

1. Aprire il report e aggiornare la pagina per assicurarsi che siano visualizzati i dati più recenti. Nel report è disponibile un'opzione per aggiornare i dati. Selezionare questa operazione per ottenere i dati più recenti.

1. Verificare che il nome dell'account di archiviazione e la chiave di accesso specificati siano corretti. Il nome dell'account di archiviazione deve corrispondere all'account configurato con i log del servizio di ricerca.

1. Verificare che l'account di archiviazione contenga i contenitori **Insights-logs-operationlogs** e **Insights-Metrics-PT1M** e che ogni contenitore disponga di dati. I log e le metriche saranno in un paio di livelli di cartelle.

1. Verificare se il set di dati è ancora in corso di aggiornamento. L'indicatore di stato di aggiornamento è illustrato nel passaggio 8 precedente. Se è ancora in corso l'aggiornamento, attendere il completamento dell'aggiornamento per aprire e aggiornare il report.

## <a name="next-steps"></a>Passaggi successivi
[Scopri di più su Azure ricerca cognitiva](./index.yml)

[Che cos'è Power BI?](/power-bi/fundamentals/power-bi-overview)

[Concetti di base del servizio Power BI](/power-bi/service-basic-concepts)