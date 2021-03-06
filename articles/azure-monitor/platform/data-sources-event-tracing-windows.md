---
title: Raccolta di eventi di Event Tracing for Windows (ETW) per Analysis log di monitoraggio di Azure
description: Informazioni su come raccogliere Event Tracing for Windows (ETW) per l'analisi nei log di monitoraggio di Azure.
services: azure-monitor
ms.subservice: logs
ms.topic: conceptual
ms.author: jamesfit
author: jimmyfit
ms.date: 01/29/2021
ms.openlocfilehash: 6239cf48794c74c5dd810fda42476df399300578
ms.sourcegitcommit: 8c8c71a38b6ab2e8622698d4df60cb8a77aa9685
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99223783"
---
# <a name="collecting-event-tracing-for-windows-etw-events-for-analysis-azure-monitor-logs"></a>Raccolta di eventi di Event Tracing for Windows (ETW) per Analysis log di monitoraggio di Azure

*Event Tracing for Windows (ETW)* fornisce un meccanismo per la strumentazione delle applicazioni in modalità utente e dei driver in modalità kernel. L'agente di Log Analytics viene utilizzato per [raccogliere gli eventi di Windows](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-windows-events) scritti nei [canali ETW](https://docs.microsoft.com/windows/win32/wes/eventmanifestschema-channeltype-complextype)amministrativi e operativi. Tuttavia, è talvolta necessario acquisire e analizzare altri eventi, ad esempio quelli scritti nel canale analitico.  

## <a name="event-flow"></a>Flusso eventi

Per raccogliere correttamente [gli eventi ETW basati su manifesto](https://docs.microsoft.com/windows/win32/etw/about-event-tracing#types-of-providers) per l'analisi nei log di monitoraggio di Azure, è necessario usare l' [estensione diagnostica di Azure](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview) per Windows (WAD). In questo scenario, l'estensione di diagnostica funge da consumer ETW, scrivendo eventi in archiviazione di Azure (tabelle) come archivio intermedio. Qui verrà archiviato in una tabella denominata **WADETWEventTable**. Log Analytics raccoglie quindi i dati della tabella dall'archiviazione di Azure e li presenta come una tabella denominata **ETWEvent**.

![Flusso eventi](./media/data-sources-event-tracing-windows/event-flow.png)

## <a name="configuring-etw-log-collection"></a>Configurazione della raccolta di log ETW

### <a name="step-1-locate-the-correct-etw-provider"></a>Passaggio 1: individuare il provider ETW corretto

Usare uno dei comandi seguenti per enumerare i provider ETW in un sistema Windows di origine.

Riga di comando:

```
logman query providers
```

PowerShell:
```
Get-NetEventProvider -ShowInstalled | Select-Object Name, Guid
```
Facoltativamente, è possibile scegliere di inviare tramite pipe l'output di PowerShell a Out-Gridview per facilitare la navigazione.

Registrare il nome e il GUID del provider ETW allineati al log analitico o di debug presentato nel Visualizzatore eventi o al modulo per il quale si intende raccogliere i dati dell'evento.

### <a name="step-2-diagnostics-extension"></a>Passaggio 2: estensione di diagnostica

Verificare che l' *estensione diagnostica Windows* sia [installata](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-windows-install#install-with-azure-portal) in tutti i sistemi di origine.

### <a name="step-3-configure-etw-log-collection"></a>Passaggio 3: configurare la raccolta di log ETW

1. Passare al pannello **impostazioni di diagnostica** della macchina virtuale

2. Selezionare la scheda **log**

3. Scorrere verso il basso e abilitare l'opzione **eventi traccia eventi per Windows (ETW)** ![ screenshot delle impostazioni di diagnostica](./media/data-sources-event-tracing-windows/enable-event-tracing-windows-collection.png)

4. Impostare il GUID del provider o la classe del provider in base al provider per cui si sta configurando la raccolta

5. Impostare il [**livello di registrazione**](https://docs.microsoft.com/windows/win32/etw/configuring-and-starting-an-event-tracing-session) in base alle esigenze

6. Fare clic sui puntini di sospensione adiacenti al provider specificato e fare clic su **Configura** .

7. Verificare che la **tabella di destinazione predefinita** sia impostata su **etweventtable**

8. Imposta un [**filtro parole chiave**](https://docs.microsoft.com/windows/win32/wes/defining-keywords-used-to-classify-types-of-events) , se necessario

9. Salvare le impostazioni del provider e del log

Una volta generati gli eventi di corrispondenza, è consigliabile iniziare a visualizzare gli eventi ETW visualizzati nella tabella **WADetweventtable** in archiviazione di Azure. Per confermare questa operazione, è possibile usare Azure Storage Explorer.

### <a name="step-4-configure-log-analytics-storage-account-collection"></a>Passaggio 4: configurare la raccolta di account di archiviazione Log Analytics

Seguire [queste istruzioni](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-logs#collect-logs-from-azure-storage) per raccogliere i log da archiviazione di Azure. Una volta configurati, i dati degli eventi ETW dovrebbero essere visualizzati in Log Analytics nella tabella **ETWEvent** .

## <a name="next-steps"></a>Passaggi successivi
- Usare [campi personalizzati](https://docs.microsoft.com/azure/azure-monitor/platform/custom-fields) per creare la struttura negli eventi ETW
- Altre informazioni sulle [query di log](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) per analizzare i dati raccolti dalle origini dati e dalle soluzioni.
