---
title: Supporto per la valutazione VMware in Azure Migrate
description: 'Informazioni sul supporto per la valutazione delle macchine virtuali VMware con lo strumento Azure Migrate: valutazione server.'
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: conceptual
ms.date: 11/10/2020
ms.openlocfilehash: ce8a1d77ae74a3946174ef58abf9add2e81eb90b
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98762990"
---
# <a name="support-matrix-for-vmware-assessment"></a>Matrice di supporto per la valutazione di VMware 

Questo articolo riepiloga i prerequisiti e i requisiti di supporto quando si individuano e si valutano le macchine virtuali VMware per la migrazione ad Azure, usando lo strumento [Azure migrate: server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool) . Per valutare le macchine virtuali VMware, è necessario creare un progetto Azure Migrate e aggiungere lo strumento Server Assessment al progetto. Dopo avere aggiunto lo strumento, distribuire l'appliance di Azure Migrate. L'appliance individua i computer locali su base continuativa e invia metadati e dati sulle prestazioni dei computer ad Azure. Dopo l'individuazione, è possibile raccogliere i computer individuati in gruppi ed eseguire valutazioni per un gruppo. 

Per eseguire la migrazione di macchine virtuali VMware in Azure, vedere la [matrice di supporto della migrazione](migrate-support-matrix-vmware-migration.md).



## <a name="limitations"></a>Limitazioni

**Requisito** | **Dettagli**
--- | ---
**Limiti di progetto** | In una sottoscrizione di Azure è possibile creare più progetti.<br/><br/> È possibile individuare e valutare fino a 35.000 macchine virtuali VMware in un singolo [progetto](migrate-support-matrix.md#azure-migrate-projects). Un progetto può includere anche server fisici e macchine virtuali Hyper-V, fino ai limiti di valutazione previsti per ciascuno di essi.
**Individuazione** | L'appliance Azure Migrate è in grado di rilevare fino a 10.000 macchine virtuali VMware in un server vCenter.
**Valutazione** | È possibile aggiungere fino a 35.000 computer in un singolo gruppo.<br/><br/> È anche possibile valutare fino a 35.000 macchine virtuali in una singola valutazione.

[Altre informazioni](concepts-assessment-calculation.md) sulle valutazioni.


## <a name="vmware-requirements"></a>Requisiti di VMware

**VMware** | **Dettagli**
--- | ---
**Server vCenter** | I computer che si desidera individuare e valutare devono essere gestiti da server vCenter versione 5,5, 6,0, 6,5, 6,7 o 7,0.<br/><br/> L'individuazione delle macchine virtuali VMware fornendo i dettagli dell'host ESXi nell'appliance non è attualmente supportata.
**Autorizzazioni** | Per la valutazione del server è necessaria una server vCenter account di sola lettura per l'individuazione e la valutazione.<br/><br/> Se si vuole eseguire l'individuazione delle applicazioni o la visualizzazione delle dipendenze, l'account deve avere i privilegi abilitati per  >  **le operazioni Guest** delle macchine virtuali.

## <a name="vm-requirements"></a>Requisiti della macchina virtuale
**VMware** | **Dettagli**
--- | ---
**VM VMware** | Tutti i sistemi operativi possono essere valutati per la migrazione. 
**Archiviazione** | Sono supportati i dischi collegati ai controller SCSI, IDE e SATA.


## <a name="azure-migrate-appliance-requirements"></a>Requisiti dell'appliance di Azure Migrate

Per l'individuazione e la valutazione, Azure Migrate usa l'[appliance di Azure Migrate](migrate-appliance.md). È possibile distribuire l'appliance come macchina virtuale VMware usando un modello OVA, importato in server vCenter o usando uno [script di PowerShell](deploy-appliance-script.md).

- Informazioni sui [requisiti dell'appliance](migrate-appliance.md#appliance---vmware) per VMware.
- In Azure per enti pubblici è necessario distribuire l'appliance [tramite lo script](deploy-appliance-script-government.md).
- Esaminare gli URL necessari all'appliance per accedere ai cloud [pubblici](migrate-appliance.md#public-cloud-urls) e [governativi](migrate-appliance.md#government-cloud-urls) .


## <a name="port-access-requirements"></a>Requisiti di accesso alle porte

**Dispositivo** | **Connection**
--- | ---
**Appliance** | Connessioni in ingresso sulla porta TCP 3389 per consentire la connessione dal desktop remoto al dispositivo.<br/><br/> Connessioni in ingresso sulla porta 44368 per accedere in remoto all'app di gestione dell'appliance tramite l'URL: ```https://<appliance-ip-or-name>:44368``` <br/><br/>Connessioni in uscita sulla porta 443 (HTTPS) per inviare metadati di individuazione e prestazioni ad Azure Migrate.
**Server vCenter** | Connessioni in ingresso sulla porta TCP 443 per consentire all'appliance di raccogliere metadati di configurazione e prestazioni per le valutazioni. <br/><br/> Per impostazione predefinita, l'appliance si connette a vCenter sulla porta 443. Se il server vCenter è in ascolto su una porta diversa, è possibile modificare la porta quando si configura l'individuazione.
**Host ESXi** | Se si vuole eseguire l' [individuazione delle app](how-to-discover-applications.md)o l' [analisi delle dipendenze senza agenti](concepts-dependency-visualization.md#agentless-analysis), l'appliance si connette agli host ESXi sulla porta TCP 443, per individuare le applicazioni, per eseguire la visualizzazione delle dipendenze senza agenti nelle macchine virtuali.

## <a name="application-discovery-requirements"></a>Requisiti di individuazione delle applicazioni

Oltre a individuare i computer, server assessment può individuare app, ruoli e funzionalità in esecuzione nei computer. L'individuazione dell'inventario delle app consente di identificare e pianificare un percorso di migrazione personalizzato per i carichi di lavoro locali. 

**Supporto** | **Dettagli**
--- | ---
**Computer supportati** | Attualmente supportato solo per le macchine virtuali VMware. È possibile individuare le app installate in un massimo di 10000 VM VMware, da ogni appliance Azure Migrate.
**Sistemi operativi** | Supporto per le macchine virtuali che eseguono tutte le versioni di Windows e Linux.
**Requisiti della macchina virtuale** | Gli strumenti VMware devono essere installati e in esecuzione nelle macchine virtuali in cui si desidera individuare le app. <br/><br/> La versione degli strumenti VMware deve essere successiva alla 10.2.0.<br/><br/> Sulle macchine virtuali deve essere installato PowerShell 2.0 o versioni successive.
**Individuazione** | Le informazioni sulle app installate in una macchina virtuale vengono raccolte dalla server vCenter, usando gli strumenti VMware installati nella macchina virtuale. L'appliance raccoglie le informazioni sull'app dal server vCenter, usando le API di vSphere. L'individuazione di app è senza agente. Nessun elemento viene installato nelle macchine virtuali e l'appliance non si connette direttamente alle macchine virtuali. WMI/SSH deve essere abilitato e disponibile nelle macchine virtuali.
**vCenter** | Il server vCenter account di sola lettura usato per la valutazione, richiede i privilegi abilitati per  >  **le operazioni Guest** delle macchine virtuali, in modo da interagire con la macchina virtuale per l'individuazione delle applicazioni.
**Accesso alla macchina virtuale** | App Discovery necessita di un account utente locale nella macchina virtuale per l'individuazione delle applicazioni.<br/><br/> Azure Migrate attualmente supporta l'utilizzo di una credenziale per tutti i server Windows e una credenziale per tutti i server Linux.<br/><br/> Si crea un account utente Guest per macchine virtuali Windows e un account utente regolare/normale (accesso non sudo) per tutte le macchine virtuali Linux.
**Accesso alla porta** | Il dispositivo di Azure Migrate deve essere in grado di connettersi alla porta TCP 443 negli host ESXi che eseguono macchine virtuali in cui si desidera individuare le app. Il server vCenter restituisce una connessione all'host ESXI, per scaricare il file contenente le informazioni sull'app.



## <a name="dependency-analysis-requirements-agentless"></a>Requisiti di analisi delle dipendenze (senza agenti)

L'[analisi delle dipendenze](concepts-dependency-visualization.md) consente di identificare le dipendenze tra computer locali che si intende valutare e migrare in Azure. Nella tabella sono riepilogati i requisiti per la configurazione dell'analisi delle dipendenze senza agente. 

**Supporto** | **Dettagli**
--- | --- 
**Computer supportati** | Attualmente supportato solo per le macchine virtuali VMware.
**Macchine virtuali di Windows** | Windows Server 2016<br/> Windows Server 2012 R2<br/> Windows Server 2012<br/> Windows Server 2008 R2 (64 bit).<br/>Microsoft Windows Server 2008 (32 bit). 
**Macchine virtuali di Linux** | Red Hat Enterprise Linux 7, 6, 5<br/> Ubuntu Linux 14.04, 16.04<br/> Debian 7, 8<br/> Oracle Linux 6, 7<br/> CentOS 5, 6, 7.<br/> SUSE Linux Enterprise Server 11 e versioni successive.
**Requisiti della macchina virtuale** | È necessario che gli strumenti VMware (successivi a 10.2.0) siano installati e in esecuzione nelle macchine virtuali che si desidera analizzare.<br/><br/> Sulle macchine virtuali deve essere installato PowerShell 2.0 o versioni successive.
**Metodo di individuazione** |  Le informazioni sulle dipendenze tra le macchine virtuali vengono raccolte dalla server vCenter, usando gli strumenti VMware installati nella macchina virtuale. L'appliance raccoglie le informazioni dalla server vCenter, usando le API di vSphere. L'individuazione è senza agente. Nella macchina virtuale non è installato alcun elemento e l'appliance non si connette direttamente alle macchine virtuali. WMI/SSH deve essere abilitato e disponibile nelle macchine virtuali.
**account vCenter** | L'account di sola lettura utilizzato da Azure Migrate per la valutazione necessita dei privilegi abilitati per **le macchine virtuali > le operazioni Guest**.
**Autorizzazioni VM Windows** |  Un account (amministratore locale o dominio) con autorizzazioni di amministratore locale per le macchine virtuali.
**Account Linux** | Account utente radice o un account con queste autorizzazioni per i file/bin/netstat e/bin/ls: CAP_DAC_READ_SEARCH e CAP_SYS_PTRACE.<br/><br/> Configurare queste funzionalità usando i comandi seguenti: <br/><br/> sudo setcap CAP_DAC_READ_SEARCH, CAP_SYS_PTRACE = EP/bin/ls<br/><br/> sudo setcap CAP_DAC_READ_SEARCH, CAP_SYS_PTRACE = EP/bin/netstat
**Accesso alla porta** | Il dispositivo di Azure Migrate deve essere in grado di connettersi alla porta TCP 443 negli host ESXI che eseguono le macchine virtuali di cui si desidera individuare le dipendenze. Il server vCenter restituisce una connessione all'host ESXI, per scaricare il file contenente le informazioni sulle dipendenze.


## <a name="dependency-analysis-requirements-agent-based"></a>Requisiti dell'analisi delle dipendenze (basata su agenti)

L'[analisi delle dipendenze](concepts-dependency-visualization.md) consente di identificare le dipendenze tra computer locali che si intende valutare e migrare in Azure. Nella tabella sono riepilogati i requisiti per la configurazione dell'analisi delle dipendenze basata su agente. 

**Requisito** | **Dettagli** 
--- | --- 
**Prima della distribuzione** | È necessario disporre di un progetto Azure Migrate con lo strumento Azure Migrate: Strumento Valutazione server aggiunto al progetto.<br/><br/>  La visualizzazione delle dipendenze viene distribuita dopo aver configurato un'appliance Azure Migrate per individuare i computer locali<br/><br/> [Informazioni](create-manage-projects.md) su come creare un progetto per la prima volta.<br/> [Informazioni](how-to-assess.md) su come aggiungere uno strumento di valutazione a un progetto esistente.<br/> Informazioni su come configurare l'appliance Azure Migrate per la valutazione di server [Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md) o fisici.
**Computer supportati** | Supportato per tutti i computer.
**Azure Government** | La visualizzazione delle dipendenze non è disponibile in Azure per enti pubblici.
**Log Analytics** | Azure Migrate usa la soluzione [Mapping dei servizi](../azure-monitor/insights/service-map.md) in [Log di Monitoraggio di Azure](../azure-monitor/log-query/log-query-overview.md) per la visualizzazione delle dipendenze.<br/><br/> Un'area di lavoro Log Analytics nuova o esistente viene associata al progetto Azure Migrate. Non è possibile modificare l'area di lavoro per un progetto Azure Migrate dopo che è stata aggiunta. <br/><br/> L'area di lavoro deve trovarsi nella stessa sottoscrizione del progetto Azure Migrate.<br/><br/> L'area di lavoro deve trovarsi nelle aree Stati Uniti orientali, Asia sud-orientale o Europa occidentale. Non è possibile associare a un progetto aree di lavoro di altre regioni.<br/><br/> L'area di lavoro deve trovarsi in una regione in cui la soluzione [Mapping dei servizi è supportata](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions).<br/><br/> In Log Analytics, l'area di lavoro associata ad Azure Migrate è contrassegnata con la chiave di migrazione progetto e con il nome del progetto.
**Agenti obbligatori** | In ogni computer che si desidera analizzare, installare gli agenti seguenti:<br/><br/> [Microsoft Monitoring Agent (MMA)](../azure-monitor/platform/agent-windows.md).<br/> [Dependency Agent](../azure-monitor/platform/agents-overview.md#dependency-agent).<br/><br/> Se vi sono computer locali non connessi a Internet, è necessario scaricare e installare il gateway di Log Analytics.<br/><br/> Altre informazioni sull'installazione di [Dependency Agent](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) e [MMA](how-to-create-group-machine-dependencies.md#install-the-mma).
**area di lavoro Log Analytics** | L'area di lavoro deve trovarsi nella stessa sottoscrizione del progetto Azure Migrate.<br/><br/> Azure Migrate supporta aree di lavoro nelle regioni Stati Uniti orientali, Asia sud-orientale ed Europa occidentale.<br/><br/>  L'area di lavoro deve trovarsi in una regione in cui la soluzione [Mapping dei servizi](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions) è supportata.<br/><br/> Non è possibile modificare l'area di lavoro per un progetto Azure Migrate dopo che è stata aggiunta.
**Costi** | La soluzione Mapping dei servizi non genera addebiti per i primi 180 giorni a partire dal giorno dell'associazione dell'area di lavoro Log Analytics al progetto di Azure Migrate/<br/><br/> Dopo 180 giorni verranno applicati gli addebiti standard di Log Analytics.<br/><br/> Se si usa una soluzione diversa da Mapping dei servizi nell'area di lavoro Log Analytics associata verranno applicati gli [addebiti standard](https://azure.microsoft.com/pricing/details/log-analytics/) di Log Analytics.<br/><br/> All'eliminazione del progetto di Azure Migrate, l'area di lavoro non viene eliminata con il progetto. Dopo l'eliminazione del progetto, l'uso di Mapping dei servizi non sarà gratuito e ogni nodo verrà addebitato in base al livello a pagamento dell'area di lavoro Log Analytics/<br/><br/>Se sono presenti progetti creati prima che Azure Migrate fosse generalmente disponibile (disponibilità generale: 28 febbraio 2018), è possibile che vengano addebitati costi aggiuntivi per Mapping dei servizi. Per garantire che l'addebito avvenga solo dopo 180 giorni, è consigliabile creare un nuovo progetto, perché le aree di lavoro esistenti prima della disponibilità generale sono ancora addebitabili.
**Gestione** | Quando si registrano agenti nell'area di lavoro, si usano l'ID e la chiave forniti dal progetto Azure Migrate.<br/><br/> È possibile usare l'area di lavoro Log Analytics all'esterno di Azure Migrate.<br/><br/> Se si elimina il progetto Azure Migrate associato, l'area di lavoro non viene eliminata automaticamente. [Eliminarla manualmente](../azure-monitor/platform/manage-access.md).<br/><br/> Non eliminare l'area di lavoro creata da Azure Migrate, a meno che non si rimuova il progetto Azure Migrate. In caso contrario, la funzionalità di visualizzazione delle dipendenze non funzionerà come previsto.
**Connettività Internet** | Se vi sono computer non connessi a Internet, è necessario installare il gateway di Log Analytics.
**Azure Government** | L'analisi delle dipendenze basata su agente non è supportata.


## <a name="next-steps"></a>Passaggi successivi

- [Rivedere](best-practices-assessment.md) le procedure consigliate per la creazione di valutazioni.
- [Preparare la valutazione VMware](./tutorial-discover-vmware.md).