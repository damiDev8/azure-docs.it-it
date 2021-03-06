---
title: Resource Manager e distribuzione classica
description: Vengono descritte le differenze tra il modello di distribuzione di Gestione risorse e il modello di distribuzione classico (o gestione dei servizi).
ms.topic: conceptual
ms.date: 02/06/2020
ms.openlocfilehash: da260db84de0cc8bd092eb95491b7cc38df93a12
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "96185845"
---
# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Confronto tra distribuzione di Azure Resource Manager e classica: comprensione dei modelli di implementazione e dello stato delle risorse

> [!NOTE]
> Le informazioni fornite in questo articolo vengono usate solo quando si esegue la migrazione dalla distribuzione classica alla distribuzione di Azure Resource Manager.

In questo articolo si apprenderanno informazioni sui modelli di distribuzione classica e di Azure Resource Manager. I modelli di distribuzione classica e Resource Manager rappresentano due diversi modi di distribuire e gestire le soluzioni Azure. Vengono usati con due diversi set di API e le risorse distribuite possono contenere differenze significative. I due modelli non sono compatibili tra loro. Questo articolo ne descrive le differenze.

Per semplificare la distribuzione e la gestione delle risorse, è consigliabile usare Resource Manager per tutte le nuove risorse e, se possibile, ridistribuire le risorse esistenti con Resource Manager.

Se non si ha familiarità con Gestione risorse, è consigliabile esaminare prima di tutto la terminologia definita nella [Panoramica di Azure Resource Manager](overview.md).

## <a name="history-of-the-deployment-models"></a>Cronologia dei modelli di distribuzione

In origine, Azure metteva a disposizione solo il modello di distribuzione classica. In questo modello ogni risorsa era indipendente, non era possibile raggruppare le risorse correlate. Era invece necessario verificare manualmente quali risorse componessero la soluzione o l'applicazione e ricordare di gestirle in un approccio coordinato. Per distribuire una soluzione è necessario creare ogni risorsa singolarmente con il portale oppure creare uno script che distribuisca tutte le risorse nell'ordine corretto. Per eliminare una soluzione era necessario eliminare ogni risorsa singolarmente. Non è possibile applicare e aggiornare facilmente i criteri di controllo di accesso per le risorse correlate. Infine, non è stato possibile applicare tag alle risorse per etichettarli con termini che consentono di monitorare le risorse e gestire la fatturazione.

Nel 2014, Azure ha introdotto Resource Manager e il nuovo concetto di gruppo di risorse. Un gruppo di risorse è un contenitore di risorse che condividono un ciclo di vita comune. Il modello di distribuzione di Resource Manager offre diversi vantaggi:

* È possibile distribuire, gestire e monitorare tutti i servizi per la soluzione come un gruppo, anziché gestire singolarmente questi servizi.
* È possibile distribuire ripetutamente la soluzione nel corso del ciclo di vita garantendo al contempo che le risorse vengano distribuite in uno stato coerente.
* È possibile applicare il controllo di accesso a tutte le risorse nel gruppo di risorse e tali criteri vengono applicati automaticamente quando si aggiungono nuove risorse al gruppo di risorse.
* È possibile applicare tag alle risorse per organizzare in modo logico tutte le risorse nella sottoscrizione.
* È possibile usare JavaScript Object Notation (JSON) per definire l'infrastruttura per la soluzione. Il file JSON è noto come modello di Resource Manager.
* È possibile definire le dipendenze tra risorse in modo che vengano distribuite nell'ordine corretto.

Quando è stato aggiunto Gestione risorse, tutte le risorse retroattive sono state aggiunti ai gruppi di risorse predefiniti. Se si crea una risorsa tramite la distribuzione classica ora, la risorsa viene creata automaticamente all'interno di un gruppo di risorse predefinito per il servizio, anche se non è stato specificato il gruppo di risorse in fase di distribuzione. Tuttavia, appena esistente all'interno di un gruppo di risorse non significa che la risorsa è stata convertita nel modello di Gestione risorse.

## <a name="understand-support-for-the-models"></a>Informazioni sul supporto dei modelli

Tenere presenti tre scenari:

1. Servizi cloud non supporta il modello di distribuzione Gestione risorse.
2. Macchine virtuali, gli account di archiviazione e le reti virtuali supportano i modelli di distribuzione classica e di Resource Manager.
3. Tutti gli altri servizi di Azure supportano il modello di Resource Manager.

Per le macchine virtuali, gli account di archiviazione e le reti virtuali, se la risorsa è stata creata con una distribuzione classica è necessario continuare eseguire operazioni classiche sulla risorsa. Se per creare la macchina virtuale, l'account di archiviazione o la rete virtuale è stata usata la distribuzione di Resource Manager è necessario continuare a eseguire operazioni di Resource Manager. Questa distinzione può creare confusione quando la sottoscrizione contiene una combinazione di risorse create con Resource Manager e con la distribuzione classica. Questa combinazione di risorse può creare risultati imprevisti perché le risorse non supportano le stesse operazioni.

In alcuni casi, un comando di Resource Manager può recuperare informazioni su una risorsa creata con la distribuzione classica o può eseguire attività amministrative, come lo spostamento di una risorsa classica in un altro gruppo di risorse, Tuttavia, questi casi non dovrebbero dare l'impressione che il tipo supporti Gestione risorse operazioni. Si supponga ad esempio di avere un gruppo di risorse che contiene una macchina virtuale creata con la distribuzione classica. Se si esegue questo comando PowerShell di Resource Manager:

```powershell
Get-AzResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines
```

Verrà restituita la macchina virtuale:

```powershell
Name              : ExampleClassicVM
ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
ResourceName      : ExampleClassicVM
ResourceType      : Microsoft.ClassicCompute/virtualMachines
ResourceGroupName : ExampleGroup
Location          : westus
SubscriptionId    : {guid}
```

Il cmdlet **Get-AzVM** di Resource Manager restituisce tuttavia solo le macchine virtuali distribuite con Resource Manager. Il comando seguente non restituisce la macchina virtuale creata tramite la distribuzione classica.

```powershell
Get-AzVM -ResourceGroupName ExampleGroup
```

Solo le risorse create tramite il tag di supporto di Gestione risorse. Non è possibile applicare i tag alle risorse classiche.

## <a name="changes-for-compute-network-and-storage"></a>Cambiamenti a livello di calcolo, rete e archiviazione

Il diagramma seguente illustra le risorse di calcolo, rete e archiviazione distribuite con Resource Manager.

![Architettura di Resource Manager](./media/deployment-models/arm_arch3.png)

SRP: provider di risorse di archiviazione, CRP: provider di risorse di calcolo, NRP: provider di risorse di rete

Tenere presenti le relazioni tra le risorse elencate di seguito:

* Tutte le risorse si trovano in un gruppo di risorse.
* La macchina virtuale dipende da un account di archiviazione specifico definito nel provider di risorse di archiviazione per archiviare i dischi nell'archivio BLOB (obbligatorio).
* La macchina virtuale fa riferimento a una scheda di interfaccia di rete specifica definita nel provider di risorse di rete (obbligatorio) e a un set di disponibilità definito nel provider di risorse di calcolo (facoltativo).
* La scheda di interfaccia di rete fa riferimento all'indirizzo IP assegnato della macchina virtuale (obbligatorio), alla subnet della rete virtuale per la macchina virtuale (obbligatorio) e a un gruppo di sicurezza di rete (facoltativo).
* La subnet all'interno di una rete virtuale fa riferimento a un gruppo di sicurezza di rete (facoltativo).
* L'istanza del servizio di bilanciamento del carico fa riferimento al pool back-end di indirizzi IP che includono la scheda di interfaccia di rete di una macchina virtuale (facoltativo) e fa riferimento a un indirizzo IP pubblico o privato del servizio di bilanciamento del carico (facoltativo).

Di seguito sono illustrati i componenti e le relative relazioni per la distribuzione classica:

![architettura classica](./media/deployment-models/arm_arch1.png)

La soluzione classica per l'hosting di una macchina virtuale include:

* Un servizio cloud obbligatorio che funge da contenitore per l'hosting di macchine virtuali (calcolo). Le macchine virtuali vengono fornite automaticamente con una scheda di interfaccia di rete e un indirizzo IP assegnato da Azure. Il servizio cloud contiene inoltre un'istanza del servizio di bilanciamento del carico esterno, un indirizzo IP pubblico ed endpoint predefiniti per consentire il traffico di desktop remoto e di PowerShell remoto per le macchine virtuali basate su Windows e il traffico Secure Shell (SSH) per le macchine virtuali basate su Linux.
* Un account di archiviazione necessario che archivia i dischi rigidi virtuali per una macchina virtuale, inclusi il sistema operativo, i dischi temporanei e i dischi dati aggiuntivi (archiviazione).
* Una rete virtuale facoltativa che funge da contenitore aggiuntivo, in cui è possibile creare una struttura subnettata e scegliere la subnet in cui si trova la macchina virtuale (rete).

La tabella seguente descrive i cambiamenti nelle modalità di interazione dei provider di risorse di calcolo, rete e archiviazione:

| Elemento | Classic | Gestione risorse |
| --- | --- | --- |
| Servizio cloud per macchine virtuali |Servizio cloud era un contenitore per mantenere le macchine virtuali che richiedevano disponibilità dalla piattaforma e bilanciamento del carico. |Servizio cloud non è più un oggetto richiesto per la creazione di una macchina virtuale che utilizza il nuovo modello. |
| Reti virtuali |La rete virtuale è facoltativa per la macchina virtuale. Se inclusa, la rete virtuale non può essere distribuita con Gestione risorse. |La macchina virtuale richiede una rete virtuale distribuita con Resource Manager. |
| Account di archiviazione |Per la macchina virtuale è necessario un account di archiviazione in cui sono archiviati i dischi rigidi virtuali per il sistema operativo, i dischi temporanei e i dischi dati aggiuntivi. |La macchina virtuale richiede un account di archiviazione per l'archiviazione dei dischi nell'archivio BLOB. |
| SET DI DISPONIBILITÀ |La disponibilità per la piattaforma era indicata mediante la configurazione  dello stesso oggetto "AvailabilitySetName" nelle macchine virtuali. Il numero massimo di domini di errore era 2. |Il set di disponibilità è una risorsa esposta dal provider Microsoft.Compute. Le macchine virtuali che richiedono un'elevata disponibilità devono essere incluse nel set di disponibilità. Il numero massimo di domini di errore è ora 3. |
| Gruppi di affinità |I gruppi di affinità erano necessari per la creazione di reti virtuali. Tuttavia, con l'introduzione delle reti virtuali regionali, non era più necessario. |Per semplificare, non esiste il concetto di gruppi di affinità nelle API esposte tramite Gestione risorse di Microsoft Azure. |
| Bilanciamento del carico |La creazione di un servizio cloud fornisce un servizio di bilanciamento del carico implicito per le macchine virtuali distribuite. |Il bilanciamento del carico è una risorsa esposta dal provider Microsoft.Network. L'interfaccia di rete primaria delle macchine virtuali che deve essere configurata con carico bilanciato deve fare riferimento al servizio di bilanciamento del carico. I servizi di bilanciamento del carico possono essere interni o esterni. Un'istanza del servizio di bilanciamento del carico fa riferimento al pool back-end di indirizzi IP che includono la NIC di una macchina virtuale (facoltativo) e fa riferimento a un indirizzo IP pubblico o privato del servizio di bilanciamento del carico (facoltativo). |
| Indirizzo IP virtuale |Servizi cloud ottiene un indirizzo IP virtuale (VIP) predefinito quando una VM viene aggiunta a un servizio cloud. L'indirizzo IP virtuale è l'indirizzo associato al servizio di bilanciamento del carico implicito. |Il bilanciamento del carico è una risorsa esposta dal provider Microsoft.Network. L'indirizzo IP pubblico può essere statico (riservato) o dinamico. Gli indirizzi IP pubblici possono essere assegnati a un Load Balancer. Gli indirizzi IP pubblici possono essere protetti tramite gruppi di protezione. |
| Indirizzo IP riservato |In Microsoft Azure è possibile riservare un indirizzo IP e associarlo a un servizio cloud per assicurarsi che l'indirizzo IP sia permanente. |L'indirizzo IP pubblico può essere creato in modalità statica e offre le stesse funzionalità di un indirizzo IP riservato. |
| Indirizzo IP pubblico per macchina virtuale |Gli indirizzi IP pubblici possono anche essere associati direttamente a una VM. |Il bilanciamento del carico è una risorsa esposta dal provider Microsoft.Network. L'indirizzo IP pubblico può essere statico (riservato) o dinamico. |
| Endpoint |In una macchina virtuale era necessario configurare gli endpoint di input per  consentire connettività a determinate porte. Una delle modalità comuni di connessione alle macchine virtuali eseguita mediante l'impostazione di endpoint di input. |È possibile configurare le regole NAT in ingresso in servizi di bilanciamento del carico per ottenere le stesse funzionalità di abilitazione degli endpoint su porte specifiche per la connessione alle macchine virtuali. |
| Nome DNS |Un servizio cloud otterrebbe un nome DNS univoco globale implicito. Ad esempio: `mycoffeeshop.cloudapp.net`. |I nomi DNS sono parametri facoltativi che possono essere specificati in una risorsa di indirizzo IP pubblico. Il nome di dominio completo ha il formato seguente: `<domainlabel>.<region>.cloudapp.azure.com`. |
| Interfacce di rete |Interfacce di rete primarie e secondarie e le relative proprietà sono state definite come configurazione di rete di una macchina virtuale. |L’interfaccia di rete è una risorsa esposta dal provider Microsoft.Network. Il ciclo di vita dell'interfaccia di rete non è collegato a una macchina virtuale. Fa riferimento all'indirizzo IP assegnato della macchina virtuale (obbligatorio), alla subnet della rete virtuale per la macchina virtuale (obbligatorio) e a un gruppo di sicurezza di rete (facoltativo). |

Per informazioni sulla connessione di reti virtuali da diversi modelli di distribuzione, vedere [Connettere reti virtuali da modelli di distribuzione diversi nel portale](../../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Eseguire la migrazione da distribuzioni classiche a distribuzioni Resource Manager

Se si è pronti per eseguire la migrazione delle risorse dalla distribuzione classica alla distribuzione Gestione risorse, vedere:

1. [Approfondimento tecnico sulla migrazione supportata dalla piattaforma dal modello di distribuzione classica ad Azure Resource Manager](../../virtual-machines/migration-classic-resource-manager-deep-dive.md)
2. [Migrazione supportata dalla piattaforma di risorse IaaS dal modello classico al modello di Azure Resource Manager](../../virtual-machines/migration-classic-resource-manager-overview.md)
3. [Eseguire la migrazione di risorse IaaS dal modello classico al modello di Azure Resource Manager tramite Azure PowerShell](../../virtual-machines/migration-classic-resource-manager-ps.md)
4. [Eseguire la migrazione di risorse IaaS dal modello classico al modello di Azure Resource Manager tramite l'interfaccia della riga di comando di Azure](../../virtual-machines/migration-classic-resource-manager-cli.md)

## <a name="frequently-asked-questions"></a>Domande frequenti

**È possibile creare una macchina virtuale con Resource Manager per la distribuzione in una rete virtuale creata con la distribuzione classica?**

Questa configurazione non è supportata. Non è possibile usare Gestione risorse per distribuire una macchina virtuale in una rete virtuale creata con la distribuzione classica.

**È possibile creare una macchina virtuale con Resource Manager da un'immagine utente creata con il modello di distribuzione classica?**

Questa configurazione non è supportata. Tuttavia, è possibile copiare i file del disco rigido virtuale da un account di archiviazione creato con il modello di distribuzione classica e aggiungerli a un nuovo account creato tramite Gestione risorse.

**Quali sono le conseguenze sulla quota della mia sottoscrizione?**

Le quote per le macchine virtuali, le reti virtuali e gli account di archiviazione creati con Azure Resource Manager sono separate dalle altre quote. Ogni sottoscrizione ottiene quote per creare le risorse con le nuove API. Per ulteriori informazioni sulle quote aggiuntive, fare clic [qui](../../azure-resource-manager/management/azure-subscription-service-limits.md).

**È possibile continuare a usare gli script automatizzati per il provisioning di macchine virtuali, reti virtuali e account di archiviazione tramite le API di Resource Manager?**

Tutta l'automazione e tutti gli script creati dall'utente continueranno a funzionare per le macchine virtuali e le reti virtuali esistenti create con la modalità Azure Service Management. È tuttavia necessario aggiornare gli script per usare il nuovo schema per la creazione delle stesse risorse con la modalità Resource Manager.

**Dove è possibile trovare esempi di modelli di gestione risorse di Azure?**

È possibile trovare un set completo di modelli di base in [Modelli di avvio rapido di Azure Resource Manager](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Passaggi successivi

* Per vedere i comandi per la distribuzione di un modello, vedere [Distribuire un'applicazione con il modello di Gestione risorse di Azure](../templates/deploy-powershell.md).