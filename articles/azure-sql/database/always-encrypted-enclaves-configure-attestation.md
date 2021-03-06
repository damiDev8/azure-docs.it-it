---
title: Configurare l'attestazione di Azure per il server logico SQL di Azure
description: Configurare l'attestazione di Azure per Always Encrypted con enclave sicure nel database SQL di Azure.
keywords: Crittografa dati, crittografia SQL, crittografia del database, dati sensibili, Always Encrypted, enclavi sicure, SGX, attestazione
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: how-to
author: jaszymas
ms.author: jaszymas
ms.reviwer: vanto
ms.date: 01/15/2021
ms.openlocfilehash: 51431bf0da9145e1b61da708942b675e4c3eea78
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2021
ms.locfileid: "98733820"
---
# <a name="configure-azure-attestation-for-your-azure-sql-logical-server"></a>Configurare l'attestazione di Azure per il server logico SQL di Azure

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Always Encrypted con enclave sicure per il database SQL di Azure è attualmente disponibile in **anteprima pubblica**.

[Microsoft Azure attestazione](../../attestation/overview.md) è una soluzione per l'attestazione di ambienti di esecuzione attendibile (TEEs), incluse le enclave Intel Software Guard Extensions (Intel SGX). 

Per usare l'attestazione di Azure per attestare le enclave Intel SGX usate per [Always Encrypted con enclavi sicure](/sql/relational-databases/security/encryption/always-encrypted-enclaves) nel database SQL di Azure, è necessario:

1. Creare un [provider di attestazione](../../attestation/basic-concepts.md#attestation-provider) e configurarlo con i criteri di attestazione consigliati.

2. Concedere al server logico SQL di Azure l'accesso al provider di attestazione.

> [!NOTE]
> La configurazione dell'attestazione è responsabilità dell'amministratore di attestazione. [Quando si configurano le enclave e l'attestazione SGX, vedere ruoli e responsabilità](always-encrypted-enclaves-plan.md#roles-and-responsibilities-when-configuring-sgx-enclaves-and-attestation).

## <a name="requirements"></a>Requisiti

Il server logico SQL di Azure e il provider di attestazione devono appartenere allo stesso tenant di Azure Active Directory. Le interazioni tra tenant non sono supportate. 

Al server logico di Azure SQL deve essere assegnata un'identità Azure AD. L'amministratore di attestazione deve ottenere l'identità Azure AD del server dall'amministratore del database SQL di Azure per tale server. Si utilizzerà l'identità per concedere al server l'accesso al provider di attestazione. 

Per istruzioni su come creare un server con un'identità o assegnare un'identità a un server esistente usando PowerShell e l'interfaccia della riga di comando di Azure, vedere [assegnare un'identità del Azure ad al server](transparent-data-encryption-byok-configure.md#assign-an-azure-active-directory-azure-ad-identity-to-your-server).

## <a name="create-and-configure-an-attestation-provider"></a>Creare e configurare un provider di attestazione

Un [provider di attestazione](../../attestation/basic-concepts.md#attestation-provider) è una risorsa nell'attestazione di Azure che valuta [le richieste di attestazione](../../attestation/basic-concepts.md#attestation-request) in base ai [criteri di attestazione](../../attestation/basic-concepts.md#attestation-request) e rilascia i [token di attestazione](../../attestation/basic-concepts.md#attestation-token). 

I criteri di attestazione vengono specificati utilizzando la [grammatica delle regole attestazioni](../../attestation/claim-rule-grammar.md).

Microsoft consiglia i criteri seguenti per l'attestazione delle enclave Intel SGX usate per Always Encrypted nel database SQL di Azure:

```output
version= 1.0;
authorizationrules 
{
       [ type=="x-ms-sgx-is-debuggable", value==false ]
        && [ type=="x-ms-sgx-product-id", value==4639 ]
        && [ type=="x-ms-sgx-svn", value>= 0 ]
        && [ type=="x-ms-sgx-mrsigner", value=="e31c9e505f37a58de09335075fc8591254313eb20bb1a27e5443cc450b6e33e5"] 
    => permit();
};
```

Il criterio precedente verifica:

- L'enclave nel database SQL di Azure non supporta il debug (che ridurrebbe il livello di protezione fornito dall'enclave).
- L'ID prodotto della libreria all'interno dell'Enclave è l'ID prodotto assegnato a Always Encrypted con le enclave sicure (4639).
- L'ID versione (SVN) della libreria è maggiore di 0.
- La libreria nell'enclave è stata firmata con la chiave di firma Microsoft (il valore dell'attestazione x-ms-SGX-mrsigner è l'hash della chiave di firma).

> [!IMPORTANT]
> Viene creato un provider di attestazione con i criteri predefiniti per le enclave Intel SGX, che non convalidano il codice in esecuzione nell'enclave. Microsoft consiglia di impostare i criteri consigliati in precedenza e non usare i criteri predefiniti per Always Encrypted con le enclave sicure.

Per istruzioni su come creare un provider di attestazione e configurare con i criteri di attestazione utilizzando:

- [Guida introduttiva: configurare l'attestazione di Azure con portale di Azure](../../attestation/quickstart-portal.md)
    > [!IMPORTANT]
    > Quando si configurano i criteri di attestazione con portale di Azure, impostare il tipo di attestazione su `SGX-IntelSDK` .
- [Avvio rapido: Configurare Attestazione di Azure con Azure PowerShell](../../attestation/quickstart-powershell.md)
    > [!IMPORTANT]
    > Quando si configurano i criteri di attestazione con Azure PowerShell, impostare il `Tee` parametro su `SgxEnclave` .
- [Avvio rapido: Configurare Attestazione di Azure con l'interfaccia della riga di comando di Azure](../../attestation/quickstart-azure-cli.md)
    > [!IMPORTANT]
    > Quando si configura il criterio di attestazione con l'interfaccia della riga di comando di Azure, impostare il `attestation-type` parametro su `SGX-IntelSDK` .

## <a name="determine-the-attestation-url-for-your-attestation-policy"></a>Determinare l'URL di attestazione per i criteri di attestazione

Dopo aver configurato un criterio di attestazione, è necessario condividere l'URL di attestazione, facendo riferimento al criterio, agli amministratori di applicazioni che usano Always Encrypted con enclavi sicure nel database SQL di Azure. Gli amministratori di applicazioni o/e gli utenti delle applicazioni dovranno configurare le proprie app con l'URL di attestazione, in modo che possano eseguire le istruzioni che usano le enclave sicure.

### <a name="use-powershell-to-determine-the-attestation-url"></a>Usare PowerShell per determinare l'URL di attestazione

Per determinare l'URL di attestazione, usare lo script seguente:

```powershell
$attestationProvider = Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroupName 
$attestationUrl = $attestationProvider.AttestUri + “/attest/SgxEnclave”
Write-Host "Your attestation URL is: " $attestationUrl 
```

### <a name="use-azure-portal-to-determine-the-attestation-url"></a>Usare portale di Azure per determinare l'URL di attestazione

1. Nel riquadro Panoramica per il provider di attestazione, copiare il valore della proprietà URI attestazione negli Appunti. Un URI di attestazione dovrebbe essere simile al seguente: `https://MyAttestationProvider.us.attest.azure.net` .

2. Aggiungere quanto segue all'URI di attestazione: `/attest/SgxEnclave` . 

L'URL di attestazione risultante avrà un aspetto simile al seguente: `https://MyAttestationProvider.us.attest.azure.net/attest/SgxEnclave`

## <a name="grant-your-azure-sql-logical-server-access-to-your-attestation-provider"></a>Concedere al server logico SQL di Azure l'accesso al provider di attestazione

Durante il flusso di lavoro di attestazione, il server logico SQL di Azure contenente il database chiama il provider di attestazione per inviare una richiesta di attestazione. Affinché il server logico di Azure SQL sia in grado di inviare richieste di attestazione, il server deve disporre di un'autorizzazione per l' `Microsoft.Attestation/attestationProviders/attestation/read` azione nel provider di attestazione. Il modo consigliato per concedere l'autorizzazione è che l'amministratore del provider di attestazione assegni l'identità del Azure AD del server al ruolo di lettore di attestazione per il provider di attestazione o il gruppo di risorse che lo contiene.

### <a name="use-azure-portal-to-assign-permission"></a>Usare portale di Azure per assegnare l'autorizzazione

Per assegnare l'identità di un server SQL di Azure al ruolo di lettore di attestazione per un provider di attestazione, seguire le istruzioni generali in [aggiungere o rimuovere assegnazioni di ruolo di Azure usando il portale di Azure](../../role-based-access-control/role-assignments-portal.md). Quando ci si trova nel riquadro **Aggiungi assegnazione ruolo** :

1. Nell'elenco a discesa **ruolo** selezionare il ruolo **lettore di attestazione** .
1. Nel campo **Seleziona** immettere il nome del server di Azure SQL per cercarlo.

Per un esempio, vedere la schermata seguente.

![assegnazione di ruolo lettore di attestazione](./media/always-encrypted-enclaves/attestation-provider-role-assigment.png)

> [!NOTE]
> Affinché un server venga visualizzato nel riquadro **Aggiungi assegnazione ruolo** , è necessario che il server disponga di un'identità Azure ad assegnata. vedere i [requisiti](#requirements).

### <a name="use-powershell-to-assign-permission"></a>Usare PowerShell per assegnare l'autorizzazione

1. Trovare il server logico SQL di Azure.

```powershell
$serverResourceGroupName = "<server resource group name>"
$serverName = "<server name>" 
$server = Get-AzSqlServer -ServerName $serverName -ResourceGroupName
```
 
2. Assegnare il server al ruolo di lettore di attestazione per il gruppo di risorse contenente il provider di attestazione.

```powershell
$attestationResourceGroupName = "<attestation provider resource group name>"
New-AzRoleAssignment -ObjectId $server.Identity.PrincipalId -RoleDefinitionName "Attestation Reader" -ResourceGroupName $attestationResourceGroupName
```

Per altre informazioni, vedere [aggiungere o rimuovere assegnazioni di ruolo di Azure con Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md#add-role-assignment-examples).

## <a name="next-steps"></a>Passaggi successivi

- [Gestire le chiavi per Always Encrypted con enclave sicuri](/sql/relational-databases/security/encryption/always-encrypted-enclaves-manage-keys)

## <a name="see-also"></a>Vedere anche

- [Esercitazione: Introduzione ad Always Encrypted con enclave sicure nel database SQL di Azure](always-encrypted-enclaves-getting-started.md)
