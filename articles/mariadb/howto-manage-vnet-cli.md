---
title: Gestire gli endpoint VNet-interfaccia della riga di comando di Azure-database di Azure per MariaDB
description: Questo articolo descrive come creare e gestire gli endpoint e le regole di servizio di rete virtuale per Database di Azure per MariaDB tramite l'interfaccia della riga di comando di Azure.
author: savjani
ms.author: pariks
ms.service: jroth
ms.devlang: azurecli
ms.topic: how-to
ms.date: 3/18/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: bee4af12b30b64409812d6758521a9ec29b2b61f
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2021
ms.locfileid: "98665090"
---
# <a name="create-and-manage-azure-database-for-mariadb-vnet-service-endpoints-using-azure-cli"></a>Creare e gestire gli endpoint di servizio di rete virtuale per Database di Azure per MariaDB tramite l'interfaccia della riga di comando di Azure

Gli endpoint e le regole dei servizi di rete virtuale estendono lo spazio di indirizzi privato di una rete virtuale a un server di Database di Azure per MariaDB. Usando pratici comandi dell'interfaccia della riga di comando di Azure è possibile creare, aggiornare, eliminare, elencare e visualizzare gli endpoint e le regole di servizio rete virtuale per gestire il server. Per una panoramica degli endpoint di servizio di rete virtuale per Database di Azure per MariaDB, incluse le limitazioni, vedere [Endpoint di servizio di rete virtuale per server di Database di Azure per MariaDB](concepts-data-access-security-vnet.md). Gli endpoint di servizio di rete virtuale sono disponibili in tutte le aree supportate per Database di Azure per MariaDB.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- Sono necessari un [database di Azure per il server e il database di MariaDB](quickstart-create-mariadb-server-database-using-azure-cli.md).

- Questo articolo richiede la versione 2.0 dell'interfaccia della riga di comando di Azure. Se si usa Azure Cloud Shell, la versione più recente è già installata.

> [!NOTE]
> Gli endpoint di servizio di rete virtuale sono supportati solo per i server per utilizzo generico e ottimizzati per la memoria.

## <a name="configure-vnet-service-endpoints"></a>Configurare endpoint di servizio delle reti virtuali

Per configurare le reti virtuali vengono usati i comandi [az network vnet](/cli/azure/network/vnet).

Se si possiedono più sottoscrizioni, scegliere quella appropriata in cui verrà fatturata la risorsa. Selezionare l'ID sottoscrizione specifico sotto l'account tramite il comando [az account set](/cli/azure/account#az-account-set). Sostituire la proprietà **id** dell'output **az login** per la sottoscrizione nel segnaposto dell'ID sottoscrizione.

- L'account deve avere le autorizzazioni necessarie per la creazione di una rete virtuale e di un endpoint di servizio.

Gli endpoint di servizio possono essere configurati sulle reti virtuali, in modo indipendente, da un utente con accesso in scrittura alla rete virtuale.

Per associare le risorse dei servizi di Azure a una rete virtuale, l'utente deve avere l'autorizzazione "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" per le subnet da aggiungere. Per impostazione predefinita, questa autorizzazione è inclusa nei ruoli di amministratore del servizio predefiniti e può essere modificata creando ruoli personalizzati.

Altre informazioni sui [ruoli predefiniti](../role-based-access-control/built-in-roles.md) e sull'assegnazione di autorizzazioni specifiche ai [ruoli personalizzati](../role-based-access-control/custom-roles.md).

Le reti virtuali e le risorse dei servizi di Azure possono essere nella stessa sottoscrizione o in sottoscrizioni diverse. Se la rete virtuale e le risorse dei servizi di Azure si trovano in sottoscrizioni diverse, le risorse devono trovarsi nello stesso tenant di Active Directory (AD). Verificare che in entrambe le sottoscrizioni sia registrato il provider di risorse **Microsoft.Sql**. Per altre informazioni, fare riferimento a [resource-manager-registration][resource-manager-portal].

> [!IMPORTANT]
> Prima configurare gli endpoint di servizio è consigliabile leggere questo articolo in cui sono riportate considerazioni e istruzioni di configurazione per gli endpoint di servizio. **Endpoint servizio di rete virtuale:** un [endpoint servizio di rete virtuale](../virtual-network/virtual-network-service-endpoints-overview.md) è una subnet in cui i valori delle proprietà includono uno o più nomi formali di tipi di servizi di Azure. Gli endpoint dei servizi di rete virtuale usano il nome del tipo di servizio **Microsoft.Sql**, che fa riferimento al servizio di Azure denominato Database SQL. Questo tag di servizio si applica al database SQL di Azure e ai servizi di Database di Azure per MariaDB, PostgreSQL e MySQL. È importante tenere presente che, quando si applica il tag di servizio **Microsoft.Sql** a un endpoint di servizio di rete virtuale, viene configurato il traffico dell'endpoint per tutti i servizi di Database di Azure, tra cui i server Database SQL di Azure, Database di Azure per PostgreSQL, Database di Azure per MariaDB e Database di Azure per MySQL nella subnet.

### <a name="sample-script"></a>Script di esempio

Questo script di esempio viene usato per creare un server di Database di Azure per MariaDB, una rete virtuale e un endpoint di servizio di rete virtuale e per proteggere il server nella subnet con una regola di rete virtuale. In questo script di esempio modificare nome utente e password amministratore. Sostituire l'ID sottoscrizione usato nel comando `az account set --subscription` con il proprio ID sottoscrizione.

```azurecli-interactive
# To find the name of an Azure region in the CLI run this command: az account list-locations
# Substitute  <subscription id> with your identifier
az account set --subscription <subscription id>

# Create a resource group
az group create \
--name myresourcegroup \
--location westus

# Create a MariaDB server in the resource group
# Name of a server maps to DNS name and is thus required to be globally unique in Azure.
# Substitute the <server_admin_password> with your own value.
az mariadb server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location westus \
--admin-user mylogin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2

# Get available service endpoints for Azure region output is JSON
# Use the command below to get the list of services supported for endpoints, for an Azure region, say "westus".
az network vnet list-endpoint-services \
-l westus

# Add Azure SQL service endpoint to a subnet *mySubnet* while creating the virtual network *myVNet* output is JSON
az network vnet create \
-g myresourcegroup \
-n myVNet \
--address-prefixes 10.0.0.0/16 \
-l westus

# Creates the service endpoint
az network vnet subnet create \
-g myresourcegroup \
-n mySubnet \
--vnet-name myVNet \
--address-prefix 10.0.1.0/24 \
--service-endpoints Microsoft.SQL

# View service endpoints configured on a subnet
az network vnet subnet show \
-g myresourcegroup \
-n mySubnet \
--vnet-name myVNet

# Create a VNet rule on the sever to secure it to the subnet Note: resource group (-g) parameter is where the database exists. VNet resource group if different should be specified using subnet id (URI) instead of subnet, VNet pair.
az mariadb server vnet-rule create \
-n myRule \
-g myresourcegroup \
-s mydemoserver \
--vnet-name myVNet \
--subnet mySubnet
```

<!-- 
In this sample script, change the highlighted lines to customize the admin username and password. Replace the SubscriptionID used in the `az account set --subscription` command with your own subscription identifier.
[!code-azurecli-interactive[main](../../cli_scripts/mariadb/create-mysql-server-vnet/create-mysql-server.sh?highlight=5,20 "Create an Azure Database for MariaDB, VNet, VNet service endpoint, and VNet rule.")]
-->

## <a name="clean-up-deployment"></a>Pulire la distribuzione
Dopo l'esecuzione dello script di esempio, è possibile usare il comando seguente per rimuovere il gruppo di risorse e tutte le risorse ad esso associate.

```azurecli-interactive
az group delete --name myresourcegroup
```


<!--
[!code-azurecli-interactive[main](../../cli_scripts/mysql/create-mysql-server-vnet/delete-mysql.sh "Delete the resource group.")]
-->

<!-- Link references, to text, Within this same GitHub repo. --> 
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md