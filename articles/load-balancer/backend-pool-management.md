---
title: Gestione del pool back-end
titleSuffix: Azure Load Balancer
description: Introduzione alla configurazione e alla gestione del pool back-end di un'istanza di Azure Load Balancer
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: how-to
ms.date: 07/07/2020
ms.author: allensu
ms.openlocfilehash: e5efbf695b85f474e5d7c84c86809acb2f5a1035
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99429603"
---
# <a name="backend-pool-management"></a>Gestione del pool back-end
Il pool back-end è un componente cruciale del servizio di bilanciamento del carico. Definisce il gruppo di risorse che gestirà il traffico per una specifica regola di bilanciamento del carico.

È possibile configurare un pool back-end in due modi:
* Scheda di interfaccia di rete
* Combinazione di indirizzo IP e ID risorsa di rete virtuale

Configurare il pool back-end tramite una scheda di interfaccia di rete quando si usano macchine virtuali e set di scalabilità di macchine virtuali esistenti. Questo metodo crea il collegamento più diretto tra la risorsa e il pool back-end. 

Quando si prealloca il pool back-end con un intervallo di indirizzi IP che si prevede di usare per creare macchine virtuali e set di scalabilità di macchine virtuali in un secondo momento, configurare il pool back-end in base alla combinazione di indirizzo IP e ID rete virtuale.

È possibile configurare i pool back-end basati su IP e NIC per lo stesso servizio di bilanciamento del carico, tuttavia non è possibile creare un singolo pool back-end che combina gli indirizzi supportati da NIC e indirizzi IP all'interno dello stesso pool.

Le sezioni di questo articolo sulla configurazione useranno:

* Azure PowerShell
* Interfaccia della riga di comando di Azure
* API REST
* Modelli di Gestione risorse di Azure 

Queste sezioni forniscono informazioni dettagliate sulla struttura dei pool back-end per ogni opzione di configurazione.

## <a name="configuring-backend-pool-by-nic"></a>Configurazione del pool back-end tramite scheda di interfaccia di rete
Il pool back-end viene creato come parte dell'operazione di bilanciamento del carico. La proprietà di configurazione IP della scheda di interfaccia di rete viene usata per aggiungere membri del pool back-end.

Gli esempi seguenti illustrano come creare e popolare le operazioni per il pool back-end per evidenziare questo flusso di lavoro e la relazione.

  >[!NOTE] 
  >È importante notare che i pool back-end configurati tramite l'interfaccia di rete non possono essere aggiornati come parte di un'operazione nel pool back-end. Qualsiasi aggiunta o eliminazione di risorse back-end deve essere eseguita nell'interfaccia di rete della risorsa.

### <a name="powershell"></a>PowerShell
Creare nuovo pool back-end:
 
```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"

$backendPool = 
New-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -BackendAddressPoolName $backendPoolName  
```

Creare una nuova interfaccia di rete e aggiungerla al pool back-end:

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"
$nicname = "myNic"
$location = "eastus"
$vnetname = <your-vnet-name>

$vnet = 
Get-AzVirtualNetwork -Name $vnetname -ResourceGroupName $resourceGroup

$nic = 
New-AzNetworkInterface -ResourceGroupName $resourceGroup -Location $location -Name $nicname -LoadBalancerBackendAddressPool $backendPoolName -Subnet $vnet.Subnets[0]
```

Recuperare le informazioni sul pool back-end per il servizio di bilanciamento del carico per confermare che questa interfaccia di rete viene aggiunta al pool back-end:

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"

$lb =
Get-AzLoadBalancer -ResourceGroupName $res
Get-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -BackendAddressPoolName $backendPoolName 
```

Creare una nuova macchina virtuale e collegare l'interfaccia di rete per inserirla nel pool back-end:

```azurepowershell-interactive
# Create a username and password for the virtual machine
$cred = Get-Credential

# Create a virtual machine configuration
$vmname = "myVM1"
$vmsize = "Standard_DS1_v2"
$pubname = "MicrosoftWindowsServer"
$nicname = "myNic"
$off = "WindowsServer"
$sku = "2019-Datacenter"
$resourceGroup = "myResourceGroup"
$location = "eastus"

$nic =
Get-AzNetworkInterface -Name $nicname -ResourceGroupName $resourceGroup

$vmConfig = 
New-AzVMConfig -VMName $vmname -VMSize $vmsize | Set-AzVMOperatingSystem -Windows -ComputerName $vmname -Credential $cred | Set-AzVMSourceImage -PublisherName $pubname -Offer $off -Skus $sku -Version latest | Add-AzVMNetworkInterface -Id $nic.Id
 
# Create a virtual machine using the configuration
$vm1 = New-AzVM -ResourceGroupName $resourceGroup -Zone 1 -Location $location -VM $vmConfig
```

### <a name="cli"></a>CLI
Creare il pool back-end:

```azurecli-interactive
az network lb address-pool create \
--resource-group myResourceGroup \
--lb-name myLB \
--name myBackendPool 
```

Creare una nuova interfaccia di rete e aggiungerla al pool back-end:

```azurecli-interactive
az network nic create \
--resource-group myResourceGroup \
--name myNic \
--vnet-name myVnet \
--subnet mySubnet \
--network-security-group myNetworkSecurityGroup \
--lb-name myLB \
--lb-address-pools myBackEndPool
```

Recuperare il pool back-end per verificare che l'indirizzo IP sia stato aggiunto correttamente:

```azurecli-interactive
az network lb address-pool show \
--resource-group myResourceGroup \
--lb-name myLb \
--name myBackendPool
```

Creare una nuova macchina virtuale e collegare l'interfaccia di rete per inserirla nel pool back-end:

```azurecli-interactive
az vm create \
--resource-group myResourceGroup \
--name myVM \
--nics myNic \
--image UbuntuLTS \
--admin-username azureuser \
--generate-ssh-keys
```

### <a name="rest-api"></a>API REST
Creare il pool back-end:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/loadBalancers/{load-balancer-name}/backendAddressPools/{backend-pool-name}?api-version=2020-05-01
```

Creare un'interfaccia di rete e aggiungerla al pool back-end creato tramite la proprietà di configurazione IP dell'interfaccia di rete:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/networkInterfaces/{nic-name}?api-version=2020-05-01
```

Corpo della richiesta JSON:
```json
{
  "properties": {
    "enableAcceleratedNetworking": true,
    "ipConfigurations": [
      {
        "name": "ipconfig1",
        "properties": {
          "subnet": {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}/subnets/{subnet-name}"
          },
          "loadBalancerBackendAddressPools": {
                                    "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/loadBalancers/{load-balancer-name}/backendAddressPools/{backend-pool-name}"
          }
        }
      }
    ]
  },
  "location": "eastus"
}
```

Recuperare le informazioni sul pool back-end per il servizio di bilanciamento del carico per confermare che questa interfaccia di rete viene aggiunta al pool back-end:

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name/providers/Microsoft.Network/loadBalancers/{load-balancer-name/backendAddressPools/{backend-pool-name}?api-version=2020-05-01
```

Creare una macchina virtuale e collegare la scheda di interfaccia di rete che fa riferimento al pool back-end:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Compute/virtualMachines/{vm-name}?api-version=2019-12-01
```

Corpo della richiesta JSON:
```JSON
{
  "location": "easttus",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_D1_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "2016-Datacenter",
        "publisher": "MicrosoftWindowsServer",
        "version": "latest",
        "offer": "WindowsServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Standard_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/{nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "myVM",
      "adminPassword": "{your-password}"
    }
  }
}
```

### <a name="resource-manager-template"></a>Modello di Resource Manager
Seguire questo [argomento di avvio rapido sul modello di Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-load-balancer-standard-create/) per distribuire un servizio di bilanciamento del carico e le macchine virtuali, quindi aggiungere le macchine virtuali al pool back-end tramite l'interfaccia di rete.

## <a name="configure-backend-pool-by-ip-address-and-virtual-network"></a>Configurare il pool back-end tramite indirizzo IP e rete virtuale
Negli scenari con pool back-end già popolati, usare l'indirizzo IP e la rete virtuale.

Tutte le operazioni di gestione dei pool back-end vengono eseguite direttamente nell'oggetto pool back-end, come evidenziato negli esempi seguenti.

  >[!IMPORTANT] 
  >Questa funzionalità è attualmente in anteprima. Per informazioni sui limiti correnti di questa funzionalità, vedere la [sezione Limitazioni](#limitations).

### <a name="powershell"></a>PowerShell
Creare un nuovo pool back-end:

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"
$vnetName = "myVnet"
$location = "eastus"
$nicName = "myNic"

$backendPool = New-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -Name $backendPoolName  
```

Aggiornare il pool back-end con un nuovo indirizzo IP della rete virtuale esistente:
 
```azurepowershell-interactive
$virtualNetwork = 
Get-AzVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroup 
 
$ip1 = New-AzLoadBalancerBackendAddressConfig -IpAddress "10.0.0.5" -Name "TestVNetRef" -VirtualNetwork $virtualNetwork  
 
$backendPool.LoadBalancerBackendAddresses.Add($ip1) 

Set-AzLoadBalancerBackendAddressPool -InputObject $backendPool
```

Recuperare le informazioni sul pool back-end per il servizio di bilanciamento del carico per confermare che gli indirizzi back-end sono stati aggiunti al pool back-end:

```azurepowershell-interactive
Get-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -Name $backendPoolName 
```
Creare una nuova interfaccia di rete e aggiungerla al pool back-end. Impostare l'indirizzo IP su uno degli indirizzi back-end:

```azurepowershell-interactive
$nic = 
New-AzNetworkInterface -ResourceGroupName $resourceGroup -Location $location -Name $nicName -PrivateIpAddress 10.0.0.4 -Subnet $virtualNetwork.Subnets[0]
```

Creare una macchina virtuale e collegare la scheda di interfaccia di rete con un indirizzo IP nel pool back-end:
```azurepowershell-interactive
# Create a username and password for the virtual machine
$cred = Get-Credential

# Create a virtual machine configuration
$vmname = "myVM1"
$vmsize = "Standard_DS1_v2"
$pubname = "MicrosoftWindowsServer"
$nicname = "myNic"
$off = "WindowsServer"
$sku = "2019-Datacenter"
$resourceGroup = "myResourceGroup"
$location = "eastus"

$nic =
Get-AzNetworkInterface -Name $nicname -ResourceGroupName $resourceGroup

$vmConfig = 
New-AzVMConfig -VMName $vmname -VMSize $vmsize | Set-AzVMOperatingSystem -Windows -ComputerName $vmname -Credential $cred | Set-AzVMSourceImage -PublisherName $pubname -Offer $off -Skus $sku -Version latest | Add-AzVMNetworkInterface -Id $nic.Id

# Create a virtual machine using the configuration
$vm1 = New-AzVM -ResourceGroupName $resourceGroup -Zone 1 -Location $location -VM $vmConfig
```

### <a name="cli"></a>CLI
Usando l'interfaccia della riga di comando è possibile popolare il pool back-end tramite parametri della riga di comando o tramite un file di configurazione JSON. 

Creare e popolare il pool back-end tramite i parametri della riga di comando:

```azurecli-interactive
az network lb address-pool create \
--resource-group myResourceGroup \
--lb-name myLB \
--name myBackendPool \
--vnet {VNET resource ID} \
--backend-address name=addr1 ip-address=10.0.0.4 \
--backend-address name=addr2 ip-address=10.0.0.5
```

Creare e popolare il pool back-end tramite il file di configurazione JSON:

```azurecli-interactive
az network lb address-pool create \
--resource-group myResourceGroup \
--lb-name myLB \
--name myBackendPool \
--vnet {VNET resource ID} \
--backend-address-config-file @config_file.json
```

File di configurazione JSON:
```JSON
        [
          {
            "name": "address1",
            "virtualNetwork": "/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}",
            "ipAddress": "10.0.0.4"
          },
          {
            "name": "address2",
            "virtualNetwork": "/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}",
            "ipAddress": "10.0.0.5"
          }
        ]
```

Recuperare le informazioni sul pool back-end per il servizio di bilanciamento del carico per confermare che gli indirizzi back-end sono stati aggiunti al pool back-end:

```azurecli-interactive
az network lb address-pool show \
--resource-group myResourceGroup \
--lb-name MyLb \
--name MyBackendPool
```

Creare una nuova interfaccia di rete e aggiungerla al pool back-end. Impostare l'indirizzo IP su uno degli indirizzi back-end:

```azurecli-interactive
az network nic create \
  --resource-group myResourceGroup \
  --name myNic \
  --vnet-name myVnet \
  --subnet mySubnet \
  --network-security-group myNetworkSecurityGroup \
  --lb-name myLB \
  --private-ip-address 10.0.0.4
```

Creare una macchina virtuale e collegare la scheda di interfaccia di rete con un indirizzo IP nel pool back-end:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --nics myNic \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

### <a name="rest-api"></a>API REST

Creare il pool back-end e definire gli indirizzi back-end tramite una richiesta PUT al pool back-end. Configurare gli indirizzi back-end nel corpo JSON della richiesta PUT tramite:

* Nome indirizzo
* Indirizzo IP
* ID rete virtuale 

```
PUT https://management.azure.com/subscriptions/subid/resourceGroups/testrg/providers/Microsoft.Network/loadBalancers/lb/backendAddressPools/backend?api-version=2020-05-01
```

Corpo della richiesta JSON:
```JSON
{
  "properties": {
    "loadBalancerBackendAddresses": [
      {
        "name": "address1",
        "properties": {
          "virtualNetwork": {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}"
          },
          "ipAddress": "10.0.0.4"
        }
      },
      {
        "name": "address2",
        "properties": {
          "virtualNetwork": {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}"
          },
          "ipAddress": "10.0.0.5"
        }
      }
    ]
  }
}
```

Recuperare le informazioni sul pool back-end per il servizio di bilanciamento del carico per confermare che gli indirizzi back-end sono stati aggiunti al pool back-end:
```
GET https://management.azure.com/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/loadBalancers/{load-balancer-name}/backendAddressPools/{backend-pool-name}?api-version=2020-05-01
```

Creare una nuova interfaccia di rete e aggiungerla al pool back-end. Impostare l'indirizzo IP su uno degli indirizzi back-end:
```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/networkInterfaces/{nic-name}?api-version=2020-05-01
```

Corpo della richiesta JSON:
```JSON
{
  "properties": {
    "enableAcceleratedNetworking": true,
    "ipConfigurations": [
      {
        "name": "ipconfig1",
        "properties": {
          "privateIPAddress": "10.0.0.4",
          "subnet": {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}/subnets/{subnet-name}"
          }
        }
      }
    ]
  },
  "location": "eastus"
}
```

Creare una macchina virtuale e collegare la scheda di interfaccia di rete con un indirizzo IP nel pool back-end:

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Compute/virtualMachines/{vm-name}?api-version=2019-12-01
```

Corpo della richiesta JSON:
```JSON
{
  "location": "eastus",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_D1_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "2016-Datacenter",
        "publisher": "MicrosoftWindowsServer",
        "version": "latest",
        "offer": "WindowsServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Standard_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/{nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "myVM",
      "adminPassword": "{your-password}"
    }
  }
}
```

## <a name="limitations"></a>Limitazioni
Un pool back-end configurato in base all'indirizzo IP presenta le limitazioni seguenti:
  * Si applica solo a Load Balancer Standard
  * Limite di 100 indirizzi IP nel pool back-end
  * Le risorse back-end devono trovarsi nella stessa rete virtuale del servizio di bilanciamento del carico
  * Un'istanza di Load Balancer con pool back-end non può funzionare come servizio di collegamento privato
  * Questa funzionalità non è attualmente supportata nel portale di Azure
  * I contenitori dell'istanza di Istanze di contenitore di Azure non sono attualmente supportati da questa funzionalità
  * I servizi di bilanciamento del carico o i servizi gestiti da servizi di bilanciamento del carico non possono essere inseriti nel pool back-end del servizio di bilanciamento del carico
  * Le regole NAT in ingresso non possono essere specificate in base all'indirizzo IP
  
## <a name="next-steps"></a>Passaggi successivi
In questo articolo sono state fornite informazioni sulla gestione del pool back-end di Azure Load Balancer e su come configurare il pool back-end tramite indirizzo IP e rete virtuale.

Altre informazioni su [Azure Load Balancer](load-balancer-overview.md).
