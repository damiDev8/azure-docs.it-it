---
title: Esempi di Azure PowerShell per l'API Cassandra di Azure Cosmos DB
description: Ottenere gli esempi di Azure PowerShell per eseguire attività comuni nell'API Cassandra di Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 01/20/2021
ms.author: mjbrown
ms.openlocfilehash: 7594e97353b38e258ba89954bc200c2c9cfdad88
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98684485"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-cassandra-api"></a>Esempi di Azure PowerShell per l'API Cassandra di Azure Cosmos DB
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

La tabella seguente contiene collegamenti a script di Azure PowerShell comunemente usati per Azure Cosmos DB. Usare i collegamenti a destra per passare agli esempi specifici delle API. Gli esempi comuni sono uguali per tutte le API. Per tutti i cmdlet di PowerShell per Azure Cosmos DB sono disponibili pagine di riferimento nelle [informazioni di riferimento su Azure PowerShell](/powershell/module/az.cosmosdb). Il `Az.CosmosDB` modulo fa ora parte del `Az` modulo. [Scaricare e installare](/powershell/azure/install-az-ps?preserve-view=true&view=azps-5.4.0) la versione più recente di AZ Module per ottenere i cmdlet di Azure Cosmos DB. È anche possibile ottenere la versione più recente dal [PowerShell Gallery](https://www.powershellgallery.com/packages/Az/5.4.0). È anche possibile creare una copia tramite fork di questi esempi di PowerShell per Cosmos DB dal repository GitHub, nella pagina degli [esempi di PowerShell per Cosmos DB in GitHub](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb).

## <a name="common-samples"></a>Esempi comuni

|Attività | Descrizione |
|---|---|
|[Aggiornare un account](scripts/powershell/common/account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Aggiornare il livello di coerenza predefinito di un account Azure Cosmos DB. |
|[Aggiornare le aree di un account](scripts/powershell/common/update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Aggiornare le aree di un account Cosmos DB. |
|[Cambiare la priorità di failover o attivare un failover](scripts/powershell/common/failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cambiare la priorità di failover a livello di area di un account Azure Cosmos o attivare un failover manuale. |
|[Chiavi dell'account o stringhe di connessione](scripts/powershell/common/keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ottenere le chiavi primarie e secondarie e le stringhe di connessione oppure rigenerare la chiave di un account Azure Cosmos DB. |
|[Creare un account Cosmos con il firewall IP](scripts/powershell/common/firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Creare un account Azure Cosmos DB con il firewall IP abilitato. |
|||

## <a name="cassandra-api-samples"></a>Esempi per l'API Cassandra

|Attività | Descrizione |
|---|---|
|[Creare un account, un keyspace e una tabella](scripts/powershell/cassandra/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea un account, un keyspace e una tabella Azure Cosmos DB. |
|[Creare un account, un keyspace e una tabella con scalabilità automatica](scripts/powershell/cassandra/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Creare un account, un keyspace e una tabella con scalabilità automatica Azure Cosmos. |
|[Elencare o ottenere keyspace o tabelle](scripts/powershell/cassandra/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Elencare o ottenere keyspace o tabelle. |
|[Operazioni di velocità effettiva](scripts/powershell/cassandra/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Operazioni di velocità effettiva per un keyspace o una tabella, incluse le operazioni get, update e migrate, tra la velocità effettiva con scalabilità automatica e la velocità effettiva standard. |
|[Bloccare le risorse per impedirne l'eliminazione](scripts/powershell/cassandra/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Impedire l'eliminazione delle risorse tramite blocchi. |
|||
