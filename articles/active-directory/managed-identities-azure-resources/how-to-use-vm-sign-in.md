---
title: Usare identità gestite in una macchina virtuale di Azure per l'accesso-Azure ADV
description: Istruzioni dettagliate ed esempi per l'uso di identità gestite da macchine virtuali di Azure per l'entità servizio per le risorse di Azure per l'accesso client e le risorse di script.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/29/2021
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.custom: devx-track-azurecli
ms.openlocfilehash: 61e83bd27c9434c4222e0161e3b643b183d1aa84
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99090961"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-for-sign-in"></a>Come usare le identità gestite per le risorse di Azure in una macchina virtuale di Azure per l'accesso 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Questo articolo riporta esempi di script di PowerShell e dell'interfaccia della riga di comando per l'accesso tramite l'entità servizio delle identità gestite per le risorse di Azure, oltre a indicazioni su argomenti importanti come la gestione degli errori.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Se si prevede di usare gli esempi di Azure PowerShell e dell'interfaccia della riga di comando di Azure presenti in questo articolo, assicurarsi di installare la versione più recente di [Azure PowerShell](/powershell/azure/install-az-ps) o dell'[interfaccia della riga di comando di Azure](/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Tutti gli esempi di script in questo articolo presuppongono che il client da riga di comando sia in esecuzione su una macchina virtuale con le identità gestite per le risorse di Azure abilitate. Usare la funzionalità di connessione alla macchina virtuale nel portale di Azure per connettersi in remoto alla macchina virtuale. Per informazioni dettagliate sull'abilitazione delle identità gestite per risorse di Azure in una macchina virtuale, vedere [Configurare le identità gestite per le risorse di Azure in una macchina virtuale tramite il portale di Azure](qs-configure-portal-windows-vm.md) o una delle varianti dell'articolo riguardanti PowerShell, l'interfaccia della riga di comando, un modello o un Azure SDK. 
> - Per evitare errori durante l'accesso alle risorse, è necessario che all'identità gestita della macchina virtuale venga assegnato almeno l'accesso in "Lettura" nell'ambito appropriato (almeno la macchina virtuale) per consentire ad Azure Resource Manager di eseguire operazioni sulla macchina virtuale. Per i dettagli, vedere [Assign managed identities for Azure resources access to a resource using the Azure portal](howto-assign-access-portal.md) (Assegnare l'accesso identità gestite per le risorse di Azure a una risorsa tramite il portale di Azure).

## <a name="overview"></a>Panoramica

Le identità gestite per le risorse di Azure forniscono un [oggetto entità servizio](../develop/developer-glossary.md#service-principal-object) , che viene [creato quando si abilitano le identità gestite per le risorse di Azure](overview.md) nella macchina virtuale. All'entità servizio è possibile concedere l'accesso a risorse di Azure, usandola come identità tramite client di script/da riga di comando per l'accesso utente e alle risorse. In genere, per poter accedere a risorse protette con la propria identità, un client di script deve:  

   - essere registrato e autorizzato da Azure AD come applicazione client Web/riservata
   - accedere con la propria entità servizio usando le credenziali dell'app (probabilmente integrate nello script)

Con le identità gestite per le risorse di Azure, il client di script non deve più fare nulla in quanto può eseguire l'accesso con l'entità servizio delle identità gestite per le risorse di Azure. 

## <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

Lo script seguente illustra come:

1. Accedere ad Azure AD con l'identità gestita della macchina virtuale per l'entità servizio per le risorse di Azure  
2. Chiamare Azure Resource Manager e ottenere l'ID dell'entità servizio della macchina virtuale. L'interfaccia della riga di comando si occupa di gestire automaticamente l'acquisizione/uso dei token. Assicurarsi di sostituire il nome della macchina virtuale per `<VM-NAME>`.  

   ```azurecli
   az login --identity
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The managed identity for Azure resources service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

Lo script seguente illustra come:

1. Accedere ad Azure AD con l'identità gestita della macchina virtuale per l'entità servizio per le risorse di Azure  
2. Chiamare un cmdlet di Azure Resource Manager per ottenere informazioni sulla macchina virtuale. PowerShell si occupa di gestire automaticamente l'uso dei token.  

   ```azurepowershell
   Add-AzAccount -identity

   # Call Azure Resource Manager to get the service principal ID for the VM's managed identity for Azure resources. 
   $vmInfoPs = Get-AzVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The managed identity for Azure resources service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>ID di risorsa per i servizi di Azure

Per un elenco di risorse che supportano Azure AD e che sono state testate con le identità gestite per le risorse di Azure e i relativi ID risorsa, vedere [Servizi di Azure che supportano l'autenticazione di Azure AD](services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

## <a name="error-handling-guidance"></a>Istruzioni per la gestione degli errori 

Risposte come quelle riportate di seguito possono indicare che l'identità gestita per le risorse di Azure della macchina virtuale non è stata configurata correttamente:

- PowerShell: *Invoke-WebRequest: Impossibile connettersi al server remoto*
- CLI: *MSI: non è stato possibile recuperare un token da `http://localhost:50342/oauth2/token` con un errore di ' HTTPConnectionPool (host =' localhost ', Port = 50342)* 

Se viene visualizzato uno di questi errori, tornare alla macchina virtuale di Azure nella [portale di Azure](https://portal.azure.com) e passare alla pagina **identità** e verificare che il **sistema assegnato** sia impostato su "Sì".

## <a name="next-steps"></a>Passaggi successivi

- Per abilitare le identità gestite per le risorse di Azure in una macchina virtuale di Azure, vedere [Configure managed identities for Azure resources on an Azure VM using PowerShell](qs-configure-powershell-windows-vm.md) (Configurare le identità gestite per le risorse di Azure in una macchina virtuale di Azure mediante PowerShell) o [Configure managed identities for Azure resources on an Azure VM using Azure CLI](qs-configure-cli-windows-vm.md) (Configurare le identità gestite per le risorse di Azure in una macchina virtuale di Azure mediante l'interfaccia della riga di comando di Azure)