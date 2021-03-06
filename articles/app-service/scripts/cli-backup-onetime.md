---
title: "CLI: eseguire il backup di un'app"
description: Informazioni su come usare l'interfaccia della riga di comando di Azure per automatizzare la distribuzione e la gestione dell'app Servizio app. Questo esempio illustra come eseguire il backup di un'app.
author: msangapu-msft
tags: azure-service-management
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/07/2017
ms.author: msangapu
ms.reviewer: cephalin
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: 4f87e610c1f3315b21a9c157f127e713ff5444f2
ms.sourcegitcommit: 3c8964a946e3b2343eaf8aba54dee41b89acc123
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98747543"
---
# <a name="back-up-an-app-using-cli"></a>Eseguire il backup di un'app usando l'interfaccia della riga di comando

Questo script di esempio crea un'app nel servizio app con le risorse correlate e quindi crea un backup singolo per tale app. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se si sceglie di installare e usare l'interfaccia della riga di comando in locale, è necessaria la versione 2.0 o successiva dell'interfaccia della riga di comando di Azure. Per trovare la versione, eseguire `az --version`. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script di esempio

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-onetime/backup-onetime.sh?highlight=3-7 "Back up an app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Spiegazione dello script

Questo script usa i comandi seguenti. Ogni comando della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [`az group create`](/cli/azure/group#az-group-create) | Consente di creare un gruppo di risorse in cui sono archiviate tutte le risorse. |
| [`az storage account create`](/cli/azure/storage/account#az-storage-account-create) | Crea un account di archiviazione. |
| [`az storage container create`](/cli/azure/storage/container#az-storage-container-create) | Crea un contenitore di Archiviazione di Azure. |
| [`az storage container generate-sas`](/cli/azure/storage/container#az-storage-container-generate-sas) | Genera un token di firma di accesso condiviso per un contenitore di archiviazione di Azure.  |
| [`az appservice plan create`](/cli/azure/appservice/plan#az-appservice-plan-create) | Consente di creare un piano di servizio app. |
| [`az webapp create`](/cli/azure/webapp#az-webapp-create) | Consente di creare un'app del servizio app. |
| [`az webapp config backup create`](/cli/azure/webapp/config/backup#az-webapp-config-backup-create) | Consente di creare un backup per un'app del servizio app. |
| [`az webapp config backup list`](/cli/azure/webapp/config/backup#az-webapp-config-backup-list) | Consente di ottenere un elenco di backup per un'app del servizio app. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sull'interfaccia della riga di comando di Azure, vedere la [documentazione sull'interfaccia della riga di comando di Azure](/cli/azure).

Altri esempi di script dell'interfaccia della riga di comando del servizio app sono disponibili nella [documentazione del servizio app di Azure](../samples-cli.md).
