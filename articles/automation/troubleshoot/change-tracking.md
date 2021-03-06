---
title: Risolvere i problemi di Rilevamento modifiche e inventario di Automazione di Azure
description: Questo articolo mostra come risolvere i problemi relativi alla funzionalità di Rilevamento modifiche e inventario di Automazione di Azure.
services: automation
ms.subservice: change-inventory-management
ms.date: 01/31/2019
ms.topic: troubleshooting
ms.openlocfilehash: 516f1a4e5e7c677b17a2941ee3c300db44d49a3b
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98896546"
---
# <a name="troubleshoot-change-tracking-and-inventory-issues"></a>Risolvere i problemi relativi a Rilevamento modifiche e Inventario

Questo articolo descrive come risolvere i problemi relativi a Rilevamento modifiche e inventario di Automazione di Azure. Per informazioni generali su Rilevamento modifiche e inventario, vedere [Panoramica di rilevamento modifiche e inventario](../change-tracking/overview.md).

## <a name="windows"></a>Windows

### <a name="scenario-change-tracking-and-inventory-records-arent-showing-for-windows-machines"></a><a name="records-not-showing-windows"></a>Scenario: I record di Rilevamento modifiche e inventario non vengono visualizzati per i computer Windows

#### <a name="issue"></a>Problema

Non viene visualizzato alcun risultato di Rilevamento modifiche e inventario per i computer Windows abilitati per la funzionalità.

#### <a name="cause"></a>Causa

Questo errore può avere le seguenti cause:

* L'agente di Log Analytics di Azure per Windows non è in esecuzione.
* Le comunicazioni verso l'account di Automazione sono bloccate.
* I Management Pack per Rilevamento modifiche e inventario non vengono scaricati.
* L'immagine della macchina virtuale da abilitare potrebbe provenire da un computer clonato senza preparazione sistema (sysprep) con l'agente di Log Analytics per Windows installato.

#### <a name="resolution"></a>Risoluzione

Nel computer dell'agente di Log Analytics passare a **C:\Programmi\Microsoft Monitoring Agent\Agent\Tools** ed eseguire i comandi seguenti:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

Se è ancora necessaria assistenza, è possibile raccogliere informazioni di diagnostica e contattare il supporto tecnico.

> [!NOTE]
> Per impostazione predefinita, l'agente di Log Analytics abilita l'analisi degli errori. Per abilitare i messaggi di errore dettagliati come nell'esempio precedente, usare il parametro `VER`. Per l'analisi delle informazioni, usare `INF` quando si richiama `StartTracing.cmd`.

##### <a name="log-analytics-agent-for-windows-not-running"></a>Agente di Log Analytics per Windows non in esecuzione

Verificare che nel sistema sia in esecuzione l'agente di Log Analytics per Windows (**HealthService.exe**).

##### <a name="communication-to-automation-account-blocked"></a>Le comunicazioni con l'account di Automazione sono bloccate

Controllare Visualizzatore eventi nel computer e cercare gli eventi che contengono la parola `changetracking`.

Per informazioni sugli indirizzi e sulle porte da abilitare per il funzionamento di Rilevamento modifiche e inventario, vedere [Pianificazione della rete](../automation-hybrid-runbook-worker.md#network-planning).

##### <a name="management-packs-not-downloaded"></a>Management Pack non scaricati

Verificare che i seguenti Management Pack di Rilevamento modifiche e inventario siano installati in locale:

* `Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*`
* `Microsoft.IntelligencePacks.InventoryChangeTracking.*`
* `Microsoft.IntelligencePacks.SingletonInventoryCollection.*`

##### <a name="vm-from-cloned-machine-that-has-not-been-sysprepped"></a>Macchina virtuale da computer clonato che non è stato preparata con Sysprep

Se si usa un'immagine clonata, preparare prima l'immagine con Sysprep e installare l'agente di Log Analytics per Windows al termine dell'operazione.

## <a name="linux"></a>Linux

### <a name="scenario-no-change-tracking-and-inventory-results-on-linux-machines"></a>Scenario: Nessun risultato di Rilevamento modifiche e inventario in computer Linux

#### <a name="issue"></a>Problema

Non viene visualizzato alcun risultato di Rilevamento modifiche e inventario per i computer Linux abilitati per la funzionalità. 

#### <a name="cause"></a>Causa
Ecco alcune possibili cause specifiche di questo problema:
* L'agente di Log Analytics per Linux non è in esecuzione.
* L'agente di Log Analytics per Linux non è configurato correttamente.
* Sono presenti conflitti di monitoraggio dell'integrità dei file.

#### <a name="resolution"></a>Risoluzione 

##### <a name="log-analytics-agent-for-linux-not-running"></a>Agente di Log Analytics per Linux non in esecuzione

Verificare che il daemon per l'agente di Log Analytics per Linux (**omsagent**) sia in esecuzione nel computer. Eseguire la query seguente nell'area di lavoro Log Analytics collegata all'account di Automazione.

```loganalytics Copy
Heartbeat
| summarize by Computer, Solutions
```

Se il computer non viene visualizzato nei risultati della query, non è stato sincronizzato di recente. Probabilmente si è verificato un problema di configurazione locale ed è necessario reinstallare l'agente. Per informazioni sull'installazione e la configurazione, vedere [Raccogliere dati di log con l'agente di Log Analytics](../../azure-monitor/platform/log-analytics-agent.md).

Se il computer viene visualizzato nei risultati della query, verificare la configurazione dell'ambito. Vedere [Targeting delle soluzioni di monitoraggio in Monitoraggio di Azure](../../azure-monitor/insights/solution-targeting.md).

Per altre informazioni sulla risoluzione di questo problema, vedere [Problema: I dati di Linux non vengono visualizzati](../../azure-monitor/platform/agent-linux-troubleshoot.md#issue-you-are-not-seeing-any-linux-data).

##### <a name="log-analytics-agent-for-linux-not-configured-correctly"></a>Agente di Log Analytics per Linux non configurato correttamente

L'agente di Log Analytics per Linux potrebbe non essere configurato correttamente per la raccolta di output della riga di comando e di log usando lo strumento agente di raccolta log di OMS. Vedere [Panoramica di Rilevamento modifiche e inventario](../change-tracking/overview.md).

##### <a name="fim-conflicts"></a>Conflitti FIM

La funzionalità FIM del Centro sicurezza di Azure potrebbe convalidare erroneamente l'integrità dei file Linux. Verificare che la funzionalità FIM sia operativa e configurata correttamente per il monitoraggio dei file di Linux. Vedere [Panoramica di Rilevamento modifiche e inventario](../change-tracking/overview.md).

## <a name="next-steps"></a>Passaggi successivi

Se il problema riscontrato non è presente in questo elenco o se non si riesce a risolverlo, visitare uno dei canali seguenti per ottenere maggiore assistenza:

* Ottenere risposte dagli esperti di Azure tramite i [forum di Azure](https://azure.microsoft.com/support/forums/).
* Connettersi con [@AzureSupport](https://twitter.com/azuresupport), l'account ufficiale Microsoft Azure per migliorare l'esperienza del cliente. Il supporto di Azure mette in contatto la community di Azure con le risorse giuste: risposte, supporto ed esperti.
* Archiviare un incidente del supporto tecnico di Azure. Accedere al [sito del supporto tecnico di Azure](https://azure.microsoft.com/support/options/) e selezionare **Supporto tecnico**.
