---
title: Ripristino temporizzato (ripristino temporizzato)
titleSuffix: Azure SQL Managed Instance
description: Ripristinare un database in Azure SQL Istanza gestita a un momento precedente.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, mathoma
ms.date: 08/25/2019
ms.openlocfilehash: d3414cb31192211c1663a84e1541f56b63674660
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99525382"
---
# <a name="restore-a-database-in-azure-sql-managed-instance-to-a-previous-point-in-time"></a>Ripristinare un database in Azure SQL Istanza gestita a un momento precedente
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Usare il ripristino temporizzato (ripristino temporizzato) per creare un database come copia di un altro database da un po' di tempo nel passato. Questo articolo descrive come eseguire un ripristino temporizzato di un database in Azure SQL Istanza gestita.

Il ripristino temporizzato è utile in scenari di ripristino, ad esempio eventi imprevisti causati da errori, dati caricati in modo errato o eliminazione di dati cruciali. È anche possibile usarlo semplicemente per i test o il controllo. I file di backup vengono conservati da 7 a 35 giorni, a seconda delle impostazioni del database.

Il ripristino temporizzato consente di ripristinare un database:

- da un database esistente.
- da un database eliminato.
- per lo stesso Istanza gestita SQL o per un altro Istanza gestita SQL. 

## <a name="limitations"></a>Limitazioni

Il ripristino temporizzato a SQL Istanza gestita presenta le limitazioni seguenti:

- Quando si esegue il ripristino da un'istanza di SQL Istanza gestita a un'altra, entrambe le istanze devono trovarsi nella stessa sottoscrizione e nella stessa area. Il ripristino tra più aree e tra sottoscrizioni non è attualmente supportato.
- Il ripristino temporizzato di un intero Istanza gestita SQL non è possibile. Questo articolo illustra solo le operazioni possibili: ripristino temporizzato di un database ospitato in SQL Istanza gestita.

> [!WARNING]
> Tenere presente le dimensioni di archiviazione del Istanza gestita SQL. A seconda delle dimensioni dei dati da ripristinare, è possibile che l'archiviazione dell'istanza venga esaurita. Se non è disponibile spazio sufficiente per i dati ripristinati, utilizzare un approccio diverso.

La tabella seguente illustra gli scenari di ripristino temporizzato per SQL Istanza gestita:

|           |Ripristinare il database esistente nella stessa istanza di SQL Istanza gestita| Ripristinare il database esistente in un altro Istanza gestita SQL|Ripristinare il database eliminato nello stesso Istanza gestita SQL|Ripristinare il database eliminato in un altro Istanza gestita SQL|
|:----------|:----------|:----------|:----------|:----------|
|**Azure portal**| Sì|No |Sì|No|
|**Interfaccia della riga di comando di Azure**|Sì |Sì |No|No|
|**PowerShell**| Sì|Sì |Sì|Sì|

## <a name="restore-an-existing-database"></a>Ripristinare un database esistente

Ripristinare un database esistente nello stesso Istanza gestita SQL usando il portale di Azure, PowerShell o l'interfaccia della riga di comando di Azure. Per ripristinare un database in un altro Istanza gestita SQL, usare PowerShell o l'interfaccia della riga di comando di Azure in modo da poter specificare le proprietà per il gruppo di risorse e il Istanza gestita SQL di destinazione. Se non si specificano questi parametri, il database verrà ripristinato nel Istanza gestita SQL esistente per impostazione predefinita. Il portale di Azure attualmente non supporta il ripristino in un'altra Istanza gestita SQL.

# <a name="portal"></a>[Portale](#tab/azure-portal)

1. Accedere al [portale di Azure](https://portal.azure.com). 
2. Passare al Istanza gestita SQL e selezionare il database che si desidera ripristinare.
3. Selezionare **Ripristina** nella pagina database:

    ![Ripristinare un database utilizzando il portale di Azure](./media/point-in-time-restore/restore-database-to-mi.png)

4. Nella pagina **Ripristina** selezionare il punto per la data e l'ora in cui si desidera ripristinare il database.
5. Selezionare **conferma** per ripristinare il database. Questa azione avvia il processo di ripristino, che crea un nuovo database e lo popola con i dati del database originale nel punto nel tempo specificato. Per ulteriori informazioni sul processo di ripristino, vedere [tempo di ripristino](../database/recovery-using-backups.md#recovery-time).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Se Azure PowerShell non è ancora installato, vedere [Install the Azure PowerShell Module](/powershell/azure/install-az-ps).

Per ripristinare il database tramite PowerShell, specificare i valori per i parametri nel comando seguente. Eseguire quindi il comando:

```powershell-interactive
$subscriptionId = "<Subscription ID>"
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<SQL Managed Instance name>"
$databaseName = "<Source-database>"
$pointInTime = "2018-06-27T08:51:39.3882806Z"
$targetDatabase = "<Name of new database to be created>"

Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
```

Per ripristinare il database in un altro Istanza gestita SQL, specificare anche i nomi del gruppo di risorse di destinazione e di destinazione SQL Istanza gestita:  

```powershell-interactive
$targetResourceGroupName = "<Resource group of target SQL Managed Instance>"
$targetInstanceName = "<Target SQL Managed Instance name>"

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
                              -TargetResourceGroupName $targetResourceGroupName `
                              -TargetInstanceName $targetInstanceName 
```

Per informazioni dettagliate, vedere [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase).

# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

Se l'interfaccia della riga di comando di Azure non è ancora installata, vedere [Install the Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

Per ripristinare il database usando l'interfaccia della riga di comando di Azure, specificare i valori per i parametri nel comando seguente. Eseguire quindi il comando:

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename |
-n mymanageddbname --dest-name targetmidbname --time "2018-05-20T05:34:22"
```

Per ripristinare il database in un altro Istanza gestita SQL, specificare anche i nomi del gruppo di risorse di destinazione e di SQL Istanza gestita:  

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename -n mymanageddbname |
       --dest-name targetmidbname --time "2018-05-20T05:34:22" |
       --dest-resource-group mytargetinstancegroupname |
       --dest-mi mytargetinstancename
```

Per una spiegazione dettagliata dei parametri disponibili, vedere la documentazione dell'interfaccia della riga di comando [per il ripristino di un database in un istanza gestita SQL](/cli/azure/sql/midb?view=azure-cli-latest#az-sql-midb-restore).

---

## <a name="restore-a-deleted-database"></a>Ripristino di un database eliminato

Il ripristino di un database eliminato può essere eseguito tramite PowerShell o portale di Azure. Per ripristinare un database eliminato nella stessa istanza, usare il portale di Azure o PowerShell. Per ripristinare un database eliminato a un'altra istanza, usare PowerShell. 

### <a name="portal"></a>Portale 


Per ripristinare un database gestito utilizzando il portale di Azure, aprire la pagina Panoramica di SQL Istanza gestita e selezionare **database eliminati**. Scegliere un database eliminato che si desidera ripristinare e digitare il nome del nuovo database che verrà creato con i dati ripristinati dal backup.

  ![Schermata del ripristino del database dell'istanza SQL di Azure](./media/point-in-time-restore/restore-deleted-sql-managed-instance-annotated.png)

.. /.. /sql-database

### <a name="powershell"></a>PowerShell

Per ripristinare un database nella stessa istanza, aggiornare i valori dei parametri e quindi eseguire il comando PowerShell seguente: 

```powershell-interactive
$subscriptionId = "<Subscription ID>"
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<SQL Managed Instance name>"
$deletedDatabaseName = "<Source database name>"
$targetDatabaseName = "<target database name>"

$deletedDatabase = Get-AzSqlDeletedInstanceDatabaseBackup -ResourceGroupName $resourceGroupName `
-InstanceName $managedInstanceName -DatabaseName $deletedDatabaseName

Restore-AzSqlinstanceDatabase -FromPointInTimeBackup -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName
```

Per ripristinare il database in un altro Istanza gestita SQL, specificare anche i nomi del gruppo di risorse di destinazione e di destinazione SQL Istanza gestita:

```powershell-interactive
$targetResourceGroupName = "<Resource group of target SQL Managed Instance>"
$targetInstanceName = "<Target SQL Managed Instance name>"

Restore-AzSqlinstanceDatabase -FromPointInTimeBackup -Name $deletedDatabase.Name `
   -InstanceName $deletedDatabase.ManagedInstanceName `
   -ResourceGroupName $deletedDatabase.ResourceGroupName `
   -DeletionDate $deletedDatabase.DeletionDate `
   -PointInTime UTCDateTime `
   -TargetInstanceDatabaseName $targetDatabaseName `
   -TargetResourceGroupName $targetResourceGroupName `
   -TargetInstanceName $targetInstanceName 
```

## <a name="overwrite-an-existing-database"></a>Sovrascrivi un database esistente

Per sovrascrivere un database esistente, è necessario:

1. Eliminare il database esistente che si desidera sovrascrivere.
2. Rinominare il database con ripristino temporizzato con il nome del database eliminato.

### <a name="drop-the-original-database"></a>Elimina il database originale

È possibile eliminare il database usando il portale di Azure, PowerShell o l'interfaccia della riga di comando di Azure.

È anche possibile eliminare il database connettendosi direttamente a SQL Istanza gestita, iniziando SQL Server Management Studio (SSMS) ed eseguendo quindi il comando Transact-SQL (T-SQL) seguente:

```sql
DROP DATABASE WorldWideImporters;
```

Usare uno dei metodi seguenti per connettersi al database nel Istanza gestita SQL:

- [SSMS/Azure Data Studio tramite una macchina virtuale di Azure](./connect-vm-instance-configure.md)
- [Da punto a sito](./point-to-site-p2s-configure.md)
- [Endpoint pubblico](./public-endpoint-configure.md)

# <a name="portal"></a>[Portale](#tab/azure-portal)

Nella portale di Azure selezionare il database da SQL Istanza gestita, quindi selezionare **Elimina**.

   ![Eliminare un database usando il portale di Azure](./media/point-in-time-restore/delete-database-from-mi.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Usare il comando di PowerShell seguente per eliminare un database esistente da un Istanza gestita SQL:

```powershell
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<SQL Managed Instance name>"
$databaseName = "<Source database>"

Remove-AzSqlInstanceDatabase -Name $databaseName -InstanceName $managedInstanceName -ResourceGroupName $resourceGroupName
```

# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azure-cli)

Usare il comando dell'interfaccia della riga di comando di Azure seguente per eliminare un database esistente da un Istanza gestita SQL:

```azurecli-interactive
az sql midb delete -g mygroupname --mi myinstancename -n mymanageddbname
```

---

### <a name="alter-the-new-database-name-to-match-the-original-database-name"></a>Modificare il nome del nuovo database in modo che corrisponda al nome del database originale

Connettersi direttamente al Istanza gestita SQL e avviare SQL Server Management Studio. Eseguire quindi la query Transact-SQL (T-SQL) seguente. La query modificherà il nome del database ripristinato a quello del database eliminato che si desidera sovrascrivere.

```sql
ALTER DATABASE WorldWideImportersPITR MODIFY NAME = WorldWideImporters;
```

Usare uno dei metodi seguenti per connettersi al database in SQL Istanza gestita:

- [Macchina virtuale di Azure](./connect-vm-instance-configure.md)
- [Da punto a sito](./point-to-site-p2s-configure.md)
- [Endpoint pubblico](./public-endpoint-configure.md)

## <a name="next-steps"></a>Passaggi successivi

Informazioni sui [backup automatici](../database/automated-backups-overview.md).
