---
title: Uso dell'interfaccia della riga di comando di Hadoop Distributed File System con Azure Data Lake Storage Gen2
description: Usare l'interfaccia della riga di comando Hadoop Distributed File System (HDFS) per Azure Data Lake Storage Gen2. Creare un contenitore, ottenere un elenco di file o directory e altro ancora.
services: storage
author: normesta
ms.service: storage
ms.topic: how-to
ms.date: 12/06/2018
ms.author: normesta
ms.subservice: data-lake-storage-gen2
ms.reviewer: artek
ms.openlocfilehash: d2b36dd600efa864913e0087c49bffd556e8330d
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "95912400"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>Uso dell'interfaccia della riga di comando di Hadoop Distributed File System con Anteprima di Azure Data Lake Storage Gen2

È possibile accedere e gestire i dati nell'account di archiviazione usando un'interfaccia della riga di comando Analogamente a una [Hadoop Distributed file System (HDFS)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Questo articolo fornisce alcuni esempi utili per iniziare.

HDInsight consente di accedere al contenitore distribuito localmente collegato ai nodi di calcolo. È possibile accedere a questo contenitore usando la shell che interagisce direttamente con HDFS e con gli altri file system supportati da Hadoop.

Per altre informazioni sull'interfaccia della riga di comando di HDFS, vedere la [documentazione ufficiale](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) e la [Guida alle autorizzazioni di HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

>[!NOTE]
>Se si usa Azure Databricks anziché HDInsight e si vuole interagire con i dati usando un'interfaccia della riga di comando, è possibile usare l'interfaccia della riga di comando di databricks per interagire con i file system di databricks. Vedere interfaccia della riga di comando di [databricks](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html).

## <a name="use-the-hdfs-cli-with-an-hdinsight-hadoop-cluster-on-linux"></a>Usare l’infrastruttura CLI di HDFS con un cluster Hadoop di HDInsight in Linux

Per prima cosa stabilire l’[accesso remoto ai servizi](../../hdinsight/hdinsight-hadoop-linux-information.md#remote-access-to-services). Selezionando [SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md), il codice di PowerShell di esempio si presenta come segue:

```powershell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```
La stringa di connessione è reperibile nella sezione "SSH + Cluster login" del pannello del cluster HDInsight nel portale di Azure. Al momento della creazione del cluster sono state specificate le credenziali SSH.

>[!IMPORTANT]
>La fatturazione del cluster HDInsight inizia dopo la creazione del cluster e si interrompe solo quando questo viene eliminato. La fatturazione avviene con tariffa oraria, perciò si deve sempre eliminare il cluster in uso quando non lo si usa più. Per informazioni su come eliminare un cluster, vedere l'[articolo relativo all'argomento](../../hdinsight/hdinsight-delete-cluster.md). I dati archiviati in un account di archiviazione nel quale è abilitato Data Lake Storage Gen2 persistono anche dopo l'eliminazione di un cluster HDInsight.

## <a name="create-a-container"></a>Creare un contenitore

`hdfs dfs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<container-name>@<storage-account-name>.dfs.core.windows.net/`

* Sostituire il segnaposto `<container-name>` con il nome che si vuole assegnare al contenitore.

* Sostituire il segnaposto `<storage-account-name>` con il nome del proprio account di archiviazione.

## <a name="get-a-list-of-files-or-directories"></a>Ottenere un elenco di file o directory

`hdfs dfs -ls <path>`

Sostituire il `<path>` segnaposto con l'URI del contenitore o della cartella del contenitore.

ad esempio `hdfs dfs -ls abfs://my-file-system@mystorageaccount.dfs.core.windows.net/my-directory-name`

## <a name="create-a-directory"></a>Creare una directory

`hdfs dfs -mkdir [-p] <path>`

Sostituire il `<path>` segnaposto con il nome del contenitore radice o una cartella all'interno del contenitore.

ad esempio `hdfs dfs -mkdir abfs://my-file-system@mystorageaccount.dfs.core.windows.net/`

## <a name="delete-a-file-or-directory"></a>Eliminare un file o una directory

`hdfs dfs -rm <path>`

Sostituire il segnaposto `<path>` con l'URI del file o della cartella che si vuole eliminare.

ad esempio `hdfs dfs -rmdir abfs://my-file-system@mystorageaccount.dfs.core.windows.net/my-directory-name/my-file-name`

## <a name="display-the-access-control-lists-acls-of-files-and-directories"></a>Visualizzare gli elenchi di controllo di accesso (ACL) di file e directory

`hdfs dfs -getfacl [-R] <path>`

Esempio:

`hdfs dfs -getfacl -R /dir`

Vedere [getfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#getfacl)

## <a name="set-acls-of-files-and-directories"></a>Impostare gli ACL su file e directory

`hdfs dfs -setfacl [-R] [-b|-k -m|-x <acl_spec> <path>]|[--set <acl_spec> <path>]`

Esempio:

`hdfs dfs -setfacl -m user:hadoop:rw- /file`

Vedere [setfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#setfacl)

## <a name="change-the-owner-of-files"></a>Modificare il proprietario dei file

`hdfs dfs -chown [-R] <new_owner>:<users_group> <URI>`

Vedere [chown](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chown)

## <a name="change-group-association-of-files"></a>Modificare l'associazione del gruppo di file

`hdfs dfs -chgrp [-R] <group> <URI>`

Vedere [chgrp](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chgrp)

## <a name="change-the-permissions-of-files"></a>Modificare le autorizzazioni dei file

`hdfs dfs -chmod [-R] <mode> <URI>`

Vedere [chmod](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chmod)

È possibile visualizzare l'elenco completo dei comandi nel sito Web della [Guida alla Shell del filesystem Apache Hadoop 2.4.1](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html).

## <a name="next-steps"></a>Passaggi successivi

* [Usare un account con supporto di Azure Data Lake Storage Gen2 in Azure Databricks](./data-lake-storage-quickstart-create-databricks-account.md)

* [Informazioni sugli elenchi di controllo di accesso in file e directory](./data-lake-storage-access-control.md)