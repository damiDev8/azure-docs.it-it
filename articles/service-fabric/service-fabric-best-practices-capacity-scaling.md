---
title: Pianificazione della capacità e scalabilità per Azure Service Fabric
description: Procedure consigliate per la pianificazione e il ridimensionamento di cluster e applicazioni Service Fabric.
author: peterpogorski
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: pepogors
ms.custom: devx-track-csharp
ms.openlocfilehash: d7d9ed8fa695c636e7aaf36fd034babb4de012d9
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98784681"
---
# <a name="capacity-planning-and-scaling-for-azure-service-fabric"></a>Pianificazione della capacità e scalabilità per Azure Service Fabric

Prima di creare un cluster di Azure Service Fabric o ridimensionare le risorse di calcolo che ospitano il cluster, è importante pianificare la capacità. Per altre informazioni sulla pianificazione della capacità, vedere [Pianificazione della capacità del cluster di Service Fabric](./service-fabric-cluster-capacity.md). Per altre indicazioni sulle procedure consigliate per la scalabilità dei cluster, vedere [Service Fabric considerazioni sulla scalabilità](/azure/architecture/reference-architectures/microservices/service-fabric#scalability-considerations).

Oltre a considerare il tipo di nodo e le caratteristiche del cluster, è necessario prevedere che le operazioni di ridimensionamento imprendano più di un'ora per il completamento di un ambiente di produzione. Questa considerazione è valida indipendentemente dal numero di macchine virtuali che si stanno aggiungendo.

## <a name="autoscaling"></a>Scalabilità automatica
È consigliabile eseguire operazioni di ridimensionamento tramite Azure Resource Manager modelli, perché è la procedura consigliata considerare le [configurazioni delle risorse come codice](./service-fabric-best-practices-infrastructure-as-code.md). 

Con il ridimensionamento automatico tramite i set di scalabilità di macchine virtuali, il modello di Gestione risorse con versione viene definito in modo non accurato per i set di scalabilità di macchine virtuali. Una definizione non accurata aumenta il rischio che le distribuzioni future provochino operazioni di ridimensionamento indesiderate. In generale, è consigliabile usare la scalabilità automatica se:

* La distribuzione di modelli di Resource Manager con un'opportuna capacità dichiarata non è adatta al proprio caso.
     
   Oltre al ridimensionamento manuale, è possibile configurare una [pipeline di integrazione e recapito continua in Azure DevOps Services usando i progetti di distribuzione del gruppo di risorse di Azure](../azure-resource-manager/templates/add-template-to-azure-pipelines.md). Questa pipeline viene in genere attivata da un'app per la logica che usa le metriche delle prestazioni delle macchine virtuali sottoposte a query dall' [API REST di monitoraggio di Azure](../azure-monitor/platform/rest-api-walkthrough.md). La pipeline viene ridimensionata in modo efficiente in base alle metriche desiderate, ottimizzando al contempo i modelli Gestione risorse.
* È necessario ridimensionare orizzontalmente un solo nodo del set di scalabilità di macchine virtuali alla volta.
   
   Per applicare la scalabilità orizzontale di tre o più nodi alla volta, è consigliabile [scalare in orizzontale un cluster Service Fabric aggiungendo un set di scalabilità di macchine virtuali](virtual-machine-scale-set-scale-node-type-scale-out.md). È più sicuro ridimensionare orizzontalmente e scalare orizzontalmente i set di scalabilità di macchine virtuali, un nodo alla volta.
* Per il cluster Service Fabric è disponibile un'affidabilità Silver o superiore e la durabilità Silver o superiore su qualsiasi scala in cui si configurano le regole di scalabilità automatica.
  
   La capacità minima per le regole di scalabilità automatica deve essere maggiore o uguale a cinque istanze di macchine virtuali. Deve anche essere uguale o maggiore del valore minimo del livello di affidabilità per il tipo di nodo primario.

> [!NOTE]
> Il Service Fabric Service fabric con stato:/System/InfastructureService/<NODE_TYPE_NAME> viene eseguito in ogni tipo di nodo con durabilità Silver o superiore. Si tratta dell'unico servizio di sistema supportato per l'esecuzione in Azure in qualsiasi tipo di nodo del cluster.

> [!IMPORTANT]
> Il ridimensionamento automatico Service Fabric supporta `Default` le `NewestVM` [configurazioni di scalabilità di](../virtual-machine-scale-sets/virtual-machine-scale-sets-scale-in-policy.md)macchine virtuali e set di scalabilità.

## <a name="vertical-scaling-considerations"></a>Considerazioni sul ridimensionamento verticale

Il [ridimensionamento verticale](./virtual-machine-scale-set-scale-node-type-scale-out.md) di un tipo di nodo in Azure Service Fabric richiede una serie di passaggi e considerazioni. Ad esempio:

* Prima del ridimensionamento, il cluster deve essere integro, In caso contrario, sarà necessario destabilizzare ulteriormente il cluster.
* Il livello di durabilità Silver o un valore superiore è necessario per tutti i tipi di nodo del cluster Service Fabric che ospitano i servizi con stato.

> [!NOTE]
> Il tipo di nodo primario che ospita i servizi di sistema Service Fabric con stato deve essere di livello di durabilità Silver o superiore. Una volta abilitata la durabilità Silver, le operazioni del cluster quali gli aggiornamenti, l'aggiunta o la rimozione di nodi e così via saranno più lente perché il sistema ottimizza la protezione dei dati sulla velocità delle operazioni.

Il ridimensionamento verticale di un set di scalabilità di macchine virtuali semplicemente modificando lo SKU di risorse è un'operazione distruttiva, perché ricrea le immagini degli host, rimuovendo così tutto lo stato locale salvato. È invece opportuno ridimensionare orizzontalmente il cluster aggiungendo un nuovo set di scalabilità con lo SKU desiderato e quindi migrare i servizi al nuovo set di scalabilità per completare un'operazione di ridimensionamento verticale sicura.

Il cluster USA Service Fabric [proprietà del nodo e vincoli di posizionamento](./service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints) per decidere dove ospitare i servizi dell'applicazione. Quando si esegue il ridimensionamento verticale di un tipo di nodo primario, si distribuirà un secondo tipo di nodo primario e quindi si imposterà ( `"isPrimary": false` ) sul tipo di nodo primario originale e si procederà con la disabilitazione dei nodi e la rimozione del set di scalabilità e delle risorse correlate. Per informazioni dettagliate, vedere [ridimensionare un Service Fabric tipo di nodo primario del cluster](service-fabric-scale-up-primary-node-type.md).

> [!NOTE]
> Convalidare sempre le operazioni negli ambienti di test prima di provare le modifiche nell'ambiente di produzione. Per impostazione predefinita, Service Fabric servizio di sistema cluster dispone di un vincolo di posizionamento solo per il tipo di nodo primario di destinazione.

Con le proprietà dei nodi e i vincoli di posizionamento dichiarati, eseguire i passaggi seguenti in un'istanza di macchina virtuale alla volta. In questo modo, i servizi di sistema e i servizi con stato vengono arrestati correttamente nell'istanza di macchina virtuale che si sta rimuovendo quando vengono create nuove repliche altrove.

1. Da PowerShell eseguire `Disable-ServiceFabricNode` con lo scopo `RemoveNode` di disabilitare il nodo che si intende rimuovere. Rimuovere il tipo di nodo con il numero più alto. Se, ad esempio, si dispone di un cluster a sei nodi, rimuovere l'istanza di macchina virtuale "MyNodeType_5".
2. Eseguire `Get-ServiceFabricNode` per assicurarsi che il nodo sia disabilitato. In caso contrario, attendere la disabilitazione del nodo. Questa operazione potrebbe richiedere un paio di ore per ogni nodo. Non continuare finché il nodo non risulta disabilitato.
3. Ridurre il numero di macchine virtuali di uno in quel tipo di nodo. L'istanza di macchina virtuale con il numero più alto verrà rimossa.
4. Ripetere i passaggi da 1 a 3 in base alle esigenze, ma mai ridimensionare il numero di istanze nei tipi di nodo primari minori rispetto a quanto garantito dal livello di affidabilità. Per un elenco di istanze consigliate, vedere [Pianificazione della capacità del cluster di Service Fabric](./service-fabric-cluster-capacity.md).
5. Quando tutte le macchine virtuali non sono più disponibili (rappresentate come "inattive"), l'infrastruttura:/System/InfrastructureService/[nome nodo] mostrerà uno stato di errore. Quindi, è possibile aggiornare la risorsa cluster per rimuovere il tipo di nodo. È possibile usare la distribuzione del modello ARM o modificare la risorsa cluster tramite [Azure Resource Manager](https://resources.azure.com). Verrà avviato un aggiornamento del cluster che rimuoverà il servizio Fabric:/System/InfrastructureService/[tipo di nodo] in stato di errore.
 6. Al termine di questa operazione, è possibile eliminare il VMScaleSet. i nodi vengono comunque visualizzati come "inattivo" da Service Fabric Explorer vista. L'ultimo passaggio consiste nel pulirli con il `Remove-ServiceFabricNodeState` comando.

## <a name="horizontal-scaling"></a>Scalabilità orizzontale

La scalabilità orizzontale può essere eseguita [manualmente](./service-fabric-cluster-scale-in-out.md) o [a livello di codice](./service-fabric-cluster-programmatic-scaling.md).

> [!NOTE]
> Se si sta eseguendo il ridimensionamento di un tipo di nodo con durabilità Silver o Gold, la scalabilità sarà lenta.

### <a name="scaling-out"></a>Aumento del numero di istanze

Ridimensionare un cluster di Service Fabric aumentando il numero di istanze per un determinato set di scalabilità di macchine virtuali. È possibile scalare in orizzontale a livello di codice usando `AzureClient` e l'ID del set di scalabilità desiderato per aumentare la capacità.

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Per eseguire la scalabilità orizzontale manualmente, aggiornare la capacità nella proprietà SKU della risorsa del [set di scalabilità di macchine virtuali](/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) desiderata.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

### <a name="scaling-in"></a>Riduzione

La scalabilità in richiede una maggiore considerazione rispetto alla scalabilità orizzontale. Per esempio:

* Service Fabric i servizi di sistema vengono eseguiti nel tipo di nodo primario del cluster. Non arrestare mai o ridimensionare il numero di istanze per quel tipo di nodo in modo da avere un numero di istanze inferiore rispetto a quello garantito dal livello di affidabilità. 
* Per un servizio con stato, è necessario un certo numero di nodi sempre disponibili per mantenere la disponibilità e mantenere lo stato del servizio. Come minimo, è necessario un numero di nodi uguale al numero di set di repliche di destinazione della partizione o del servizio.

Per ridurre il numero di istanze, seguire questi passaggi:

1. Da PowerShell eseguire `Disable-ServiceFabricNode` con lo scopo `RemoveNode` di disabilitare il nodo che si intende rimuovere. Rimuovere il tipo di nodo con il numero più alto. Se, ad esempio, si dispone di un cluster a sei nodi, rimuovere l'istanza di macchina virtuale "MyNodeType_5".
2. Eseguire `Get-ServiceFabricNode` per assicurarsi che il nodo sia disabilitato. In caso contrario, attendere la disabilitazione del nodo. Questa operazione potrebbe richiedere un paio di ore per ogni nodo. Non continuare finché il nodo non risulta disabilitato.
3. Ridurre il numero di macchine virtuali di uno in quel tipo di nodo. L'istanza di macchina virtuale con il numero più alto verrà rimossa.
4. Ripetere i passaggi da 1 a 3 in base alle esigenze finché non si esegue il provisioning della capacità desiderata. Non ridimensionare il numero di istanze nei tipi di nodo primario a un livello inferiore rispetto a quello garantito dal livello di affidabilità. Per un elenco di istanze consigliate, vedere [Pianificazione della capacità del cluster di Service Fabric](./service-fabric-cluster-capacity.md).

Per applicare la scalabilità manuale, aggiornare la capacità nella proprietà SKU della risorsa del [set di scalabilità di macchine virtuali](/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) desiderata.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

È necessario preparare il nodo per la scalabilità a livello di codice. Trovare il nodo da rimuovere (il nodo di istanza più elevato). Ad esempio:

```csharp
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

Disattivare e rimuovere il nodo utilizzando la stessa `FabricClient` istanza ( `client` in questo caso) e l'istanza del nodo ( `instanceIdString` in questo caso) utilizzata nel codice precedente:

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shut down
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement virtual machine scale set capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply();
```

> [!NOTE]
> Quando si esegue la scalabilità in un cluster, l'istanza di nodo/VM rimossa verrà visualizzata in uno stato non integro in Service Fabric Explorer. Per una spiegazione di questo comportamento, vedere [comportamenti che è possibile osservare in Service Fabric Explorer](./service-fabric-cluster-scale-in-out.md#behaviors-you-may-observe-in-service-fabric-explorer). È possibile:
> * Chiamare il [comando Remove-ServiceFabricNodeState](/powershell/module/servicefabric/remove-servicefabricnodestate) con il nome del nodo appropriato.
> * Distribuire il [Service Fabric applicazione di supporto per la scalabilità](https://github.com/Azure/service-fabric-autoscale-helper/) automatica nel cluster. Questa applicazione garantisce che i nodi con scalabilità orizzontale vengano cancellati dal Service Fabric Explorer.

## <a name="reliability-levels"></a>Livelli di affidabilità

Il [livello di affidabilità](./service-fabric-cluster-capacity.md) è una proprietà della risorsa cluster Service Fabric. Non può essere configurato in modo diverso per i singoli tipi di nodo. Questo livello controlla il fattore di replica dei servizi di sistema per il cluster ed è definito tramite un'impostazione a livello di risorsa cluster. 

Il livello di affidabilità determina il numero minimo di nodi che deve avere il tipo di nodo primario. Il livello di affidabilità può avere i valori seguenti:

* Platinum: esegue i servizi di sistema con un numero di set di repliche di destinazione pari a sette e nove nodi di inizializzazione.
* Gold: esegue i servizi di sistema con un numero di set di repliche di destinazione pari a sette e sette nodi di inizializzazione.
* Silver: esegue i servizi di sistema con un numero di set di repliche di destinazione pari a cinque e cinque nodi di inizializzazione.
* Bronze: esegue i servizi di sistema con un numero di set di repliche di destinazione pari a tre e tre nodi di inizializzazione.

Il livello minimo di affidabilità consigliato è Silver.

Il livello di affidabilità è impostato nella sezione delle proprietà della [risorsa Microsoft.ServiceFabric/clusters](/azure/templates/microsoft.servicefabric/2018-02-01/clusters), come in questo esempio:

```json
"properties":{
    "reliabilityLevel": "Silver"
}
```

## <a name="durability-levels"></a>Livelli di durabilità

> [!WARNING]
> I tipi di nodo in esecuzione con durabilità Bronze _non ottengono privilegi_. I processi di infrastruttura che interessano i carichi di lavoro senza stato non verranno interrotti o posticipati, che potrebbero influire sui carichi di lavoro. 
>
> Usare il livello di durabilità Bronze solo per i tipi di nodo che eseguono carichi di lavoro senza stato. Per i carichi di lavoro di produzione, eseguire Silver o versione successiva per garantire la coerenza dello stato. Scegliere il livello di affidabilità appropriato seguendo le indicazioni riportate nella [documentazione sulla pianificazione della capacità](./service-fabric-cluster-capacity.md).

Il livello di durabilità deve essere impostato in due risorse: Uno è il profilo di estensione della [risorsa del set di scalabilità di macchine virtuali](/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile):

```json
"extensionProfile": {
    "extensions":          {
        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
        "properties": {
            "settings": {
                "durabilityLevel": "Bronze"
            }
        }
    }
}
```

L'altra risorsa è sotto `nodeTypes` nella [risorsa Microsoft. ServiceFabric/Clusters](/azure/templates/microsoft.servicefabric/2018-02-01/clusters): 

```json
"nodeTypes": [
    {
        "name": "[variables('vmNodeType0Name')]",
        "durabilityLevel": "Bronze"
    }
]
```

## <a name="next-steps"></a>Passaggi successivi

* Creare un cluster in macchine virtuali o computer che eseguono Windows Server: [Service Fabric la creazione di cluster per Windows Server](service-fabric-cluster-creation-for-windows-server.md).
* Creare un cluster nelle VM o nei computer che eseguono Linux: [creare un cluster Linux](service-fabric-cluster-creation-via-portal.md).
* Informazioni sulle [opzioni di supporto di Service Fabric](service-fabric-support.md).

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
