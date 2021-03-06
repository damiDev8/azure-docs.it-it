---
title: Prerequisiti per la distribuzione di servizi cloud di Azure (supporto esteso)
description: Prerequisiti per la distribuzione di servizi cloud di Azure (supporto esteso)
ms.topic: article
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 42416b1fc06ff59a68a6f5044b8bcca5dc7f035f
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98880187"
---
# <a name="prerequisites-for-deploying-azure-cloud-services-extended-support"></a>Prerequisiti per la distribuzione di servizi cloud di Azure (supporto esteso)

> [!IMPORTANT]
> Servizi cloud (supporto esteso) è attualmente disponibile in anteprima pubblica.
> Questa versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate. Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Per garantire una distribuzione corretta dei servizi cloud (supporto esteso), rivedere i passaggi seguenti e completare ogni elemento prima di provare le distribuzioni. 

## <a name="register-the-cloudservices-feature"></a>Registrare la funzionalità CloudServices
Registrare la funzionalità per la sottoscrizione. Il completamento della registrazione potrebbe richiedere diversi minuti. 

```powershell
Register-AzProviderFeature -FeatureName CloudServices -ProviderNamespace Microsoft.Compute
```

Verificare lo stato della registrazione usando quanto segue:  
```powershell
Get-AzProviderFeature 

#Sample output
FeatureName               ProviderName      RegistrationState
CloudServices           Microsoft.Compute    Registered
```

## <a name="required-service-configuration-cscfg-file-updates"></a>Aggiornamenti file necessari per la configurazione del servizio (. cscfg)

### <a name="1-virtual-network"></a>1) rete virtuale
Le distribuzioni del servizio cloud (supporto esteso) devono trovarsi in una rete virtuale. La rete virtuale può essere creata tramite [portale di Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-portal), [PowerShell](https://docs.microsoft.com/azure/virtual-network/quick-create-powershell), l'interfaccia della riga di comando di [Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-cli) o il [modello ARM](https://docs.microsoft.com/azure/virtual-network/quick-create-template). È necessario fare riferimento anche alla rete virtuale e alle subnet nella configurazione del servizio (con estensione cscfg) nella sezione [NetworkConfiguration](schema-cscfg-networkconfiguration.md) . 

Per le reti virtuali appartenenti allo stesso gruppo di risorse del servizio cloud, è sufficiente fare riferimento solo al nome della rete virtuale nel file di configurazione del servizio (con estensione cscfg). Se la rete virtuale e il servizio cloud si trovano in due gruppi di risorse diversi, è necessario specificare l'ID Azure Resource Manager completo della rete virtuale nel file di configurazione del servizio (con estensione cscfg).
 
#### <a name="virtual-network-located-in-same-resource-group"></a>Rete virtuale che si trova nello stesso gruppo di risorse
```xml
<VirtualNetworkSite name="<vnet-name>"/> 
  <AddressAssignments> 
    <InstanceAddress roleName="<role-name>"> 
     <Subnets> 
       <Subnet name="<subnet-name>"/> 
     </Subnets> 
    </InstanceAddress> 
```

#### <a name="virtual-network-located-in-different-resource-group"></a>Rete virtuale che si trova in un gruppo di risorse diverso
```xml
<VirtualNetworkSite name="/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>"/> 
   <AddressAssignments> 
     <InstanceAddress roleName="<role-name>"> 
       <Subnets> 
        <Subnet name="<subnet-name>"/> 
       </Subnets> 
     </InstanceAddress> 
```
### <a name="2-remove-the-old-plugins"></a>2) rimuovere i vecchi plug-in

Rimuovere le impostazioni di desktop remoto obsolete dal file di configurazione del servizio (. cscfg).  

```xml
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="gachandw" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="XXXX" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="2021-12-17T23:59:59.0000000+05:30" /> 
<Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" /> 
```

## <a name="required-service-definition-file-csdef-updates"></a>Aggiornamenti necessari per il file di definizione del servizio (con estensione csdef)

### <a name="1-virtual-machine-sizes"></a>1) dimensioni delle macchine virtuali
Le dimensioni seguenti sono deprecate in Azure Resource Manager. Tuttavia, se si vuole continuare a usarli, aggiornare il `vmsize` nome con la convenzione di denominazione Azure Resource Manager associata.  

| Nome dimensioni precedenti | Nome dimensioni aggiornate | 
|---|---|
| Molto piccola | Standard_A0 | 
| Piccola | Standard_A1 |
| Medio | Standard_A2 | 
| Grande | Standard_A3 | 
| Molto grande | Standard_A4 | 
| A5 | Standard_A5 | 
| A6 | Standard_A6 | 
| A7 | Standard_A7 |  
| A8 | Standard_A8 | 
| A9 | Standard_A9 |
| A10 | Standard_A10 | 
| A11 | Standard_A11 | 
| MSODSG5 | Standard_MSODSG5 | 

 Ad esempio, `<WorkerRole name="WorkerRole1" vmsize="Medium"` diventerebbe `<WorkerRole name="WorkerRole1" vmsize="Standard_A2"`.
 
> [!NOTE]
> Per recuperare un elenco di dimensioni disponibili, vedere [SKU di risorse-elencare](https://docs.microsoft.com/rest/api/compute/resourceskus/list) e applicare i filtri seguenti: <br>
`ResourceType = virtualMachines ` <br>
`VMDeploymentTypes = PaaS `


### <a name="2-remove-old-remote-desktop-plugins"></a>2) rimuovere i plug-in Desktop remoto obsoleti
Per le distribuzioni che utilizzano i vecchi plug-in di desktop remoto è necessario rimuovere i moduli dal file di definizione del servizio (con estensione csdef) ed eventuali certificati associati. 

```xml
<Imports> 
<Import moduleName="RemoteAccess" /> 
<Import moduleName="RemoteForwarder" /> 
</Imports> 
```

## <a name="key-vault-creation"></a>Creazione Key Vault 

Key Vault viene utilizzato per archiviare i certificati associati ai servizi cloud (supporto esteso). Aggiungere i certificati a Key Vault, quindi fare riferimento alle identificazioni personali del certificato nel file di configurazione del servizio. È anche necessario abilitare Key Vault per le autorizzazioni appropriate in modo che la risorsa servizi cloud (supporto esteso) possa recuperare il certificato archiviato come segreto da Key Vault. È possibile creare Key Vault tramite [portale di Azure](https://docs.microsoft.com/azure/key-vault/general/quick-create-portal)e  [PowerShell](https://docs.microsoft.com/azure/key-vault/general/quick-create-powershell). Il Key Vault deve essere creato nella stessa area e nella stessa sottoscrizione del servizio cloud. Per altre informazioni, vedere [usare i certificati con servizi cloud di Azure (supporto esteso)](certificates-and-key-vault.md).

## <a name="next-steps"></a>Passaggi successivi 
- Esaminare i [prerequisiti di distribuzione](deploy-prerequisite.md) per i servizi cloud (supporto esteso).
- Distribuire un servizio cloud (supporto esteso) usando il [portale di Azure](deploy-portal.md), [PowerShell](deploy-powershell.md), il [modello](deploy-template.md) o [Visual Studio](deploy-visual-studio.md).
- Esaminare le [domande frequenti](faq.md) per i servizi cloud (supporto esteso).
- Visitare il [repository di esempi di servizi cloud (supporto esteso)](https://github.com/Azure-Samples/cloud-services-extended-support)