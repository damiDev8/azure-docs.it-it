---
title: Configurare i parametri del server-Azure PowerShell database di Azure per MariaDB
description: Questo articolo descrive come configurare i parametri del servizio in database di Azure per MariaDB usando PowerShell.
author: savjani
ms.author: pariks
ms.service: jroth
ms.devlang: azurepowershell
ms.topic: how-to
ms.date: 10/1/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 891cb5c6e570269d0414e7b7cece760af143927f
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/21/2021
ms.locfileid: "98662458"
---
# <a name="configure-server-parameters-in-azure-database-for-mariadb-using-powershell"></a>Configurare i parametri del server nel database di Azure per MariaDB con PowerShell

È possibile elencare, visualizzare e aggiornare i parametri di configurazione per un database di Azure per il server MariaDB usando PowerShell. Un subset delle configurazioni del motore viene esposto a livello di server e può essere modificato.

>[!Note]
> I parametri del server possono essere aggiornati globalmente a livello di server tramite l'[interfaccia della riga di comando di Azure](./howto-configure-server-parameters-cli.md), [PowerShell](./howto-configure-server-parameters-using-powershell.md) o il [portale di Azure](./howto-server-parameters.md).

## <a name="prerequisites"></a>Prerequisiti

Per completare questa guida, è necessario:

- Il [modulo AZ PowerShell](/powershell/azure/install-az-ps) installato localmente o [Azure cloud Shell](https://shell.azure.com/) nel browser
- Un [database di Azure per il server MariaDB](quickstart-create-mariadb-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Durante la fase di anteprima del modulo Az.MariaDb PowerShell, è necessario installarlo separatamente dal modulo Az PowerShell usando il comando seguente: `Install-Module -Name Az.MariaDb -AllowPrerelease`.
> Quando il modulo Az.MariaDb di PowerShell sarà disponibile a livello generale, diventerà parte delle future versioni del modulo Az PowerShell e disponibile in modo nativo dall'interno di Azure Cloud Shell.

Se si sceglie di usare PowerShell in locale, connettersi all'account di Azure usando il cmdlet [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) .

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="list-server-configuration-parameters-for-azure-database-for-mariadb-server"></a>Elencare i parametri di configurazione del server per Database di Azure per il server MariaDB

Per elencare tutti i parametri modificabili in un server e i relativi valori, eseguire il `Get-AzMariaDbConfiguration` cmdlet.

Nell'esempio seguente vengono elencati i parametri di configurazione del server per il server **mydemoserver** nel gruppo di risorse **myresourcegroup**.

```azurepowershell-interactive
Get-AzMariaDbConfiguration -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

Per la definizione di ognuno dei parametri elencati, vedere la sezione di riferimento di MariaDB nell'articolo sulle [variabili di sistema del server](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html).

## <a name="show-server-configuration-parameter-details"></a>Visualizzare i dettagli dei parametri di configurazione server

Per visualizzare i dettagli di un determinato parametro di configurazione per un server, eseguire il `Get-AzMariaDbConfiguration` cmdlet e specificare il parametro **Name** .

Questo esempio mostra i dettagli del parametro di configurazione del server **Slow \_ query \_ log** per il server **mydemoserver** nel gruppo di risorse **myresourcegroup**.

```azurepowershell-interactive
Get-AzMariaDbConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>Modificare un valore di un parametro di configurazione server

È anche possibile modificare il valore di un determinato parametro di configurazione del server, che aggiorna il valore di configurazione sottostante del motore del server MariaDB. Per aggiornare la configurazione, usare il `Update-AzMariaDbConfiguration` cmdlet.

Per aggiornare il parametro di configurazione del server **Slow \_ query \_ log** del server **mydemoserver** nel gruppo di risorse **myresourcegroup**.

```azurepowershell-interactive
Update-AzMariaDbConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver -Value On
```

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Aumento automatico delle dimensioni di archiviazione nel database di Azure per il server MariaDB con PowerShell](howto-auto-grow-storage-powershell.md).