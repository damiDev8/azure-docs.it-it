---
title: includere file
description: includere file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/24/2020
ms.author: cynthn
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 9556b20ba0ceac2d4c1ad92897e6f9d46293387f
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027945"
---
## <a name="create-an-image-gallery"></a>Creare un raccolta di immagini 

Una raccolta di immagini è la risorsa principale usata per l'abilitazione della condivisione di immagini. 

I caratteri consentiti per i nomi delle raccolte sono lettere maiuscole o minuscole, numeri e punti. Il nome della raccolta non può contenere trattini.   I nomi di raccolta devono essere univoci all'interno della sottoscrizione. 

Creare una raccolta di immagini usando [sig az create](/cli/azure/sig#az-sig-create). L'esempio seguente crea un gruppo di risorse denominato *myGalleryRG* nell'area *Stati Uniti orientali* e una raccolta denominata *myGallery*.

```azurecli-interactive
az group create --name myGalleryRG --location eastus
az sig create --resource-group myGalleryRG --gallery-name myGallery
```

## <a name="share-the-gallery"></a>Condividere la raccolta

È possibile condividere immagini tra sottoscrizioni usando il controllo degli accessi in base al ruolo. È possibile condividere immagini a livello della raccolta, della definizione dell'immagine o della versione dell'immagine. Qualsiasi utente che abbia autorizzazioni di lettura per una versione di immagine, anche tra sottoscrizioni diverse, potrà distribuire una VM usando la versione dell'immagine.

È consigliabile condividere con altri utenti a livello di raccolta. Per ottenere l'ID oggetto della raccolta, usare [az sig show](/cli/azure/sig#az-sig-show).

```azurecli-interactive
az sig show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --query id
```

Usare l'ID oggetto come ambito, insieme a un indirizzo di posta elettronica e a [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create), per concedere a un utente l'accesso alla raccolta di immagini condivise. Sostituire `<email-address>` e `<gallery iD>` con le proprie informazioni.

```azurecli-interactive
az role assignment create \
   --role "Reader" \
   --assignee <email address> \
   --scope <gallery ID>
```

Per altre informazioni su come condividere risorse usando RBCA, vedere [Gestione dell'accesso usando RBCA e L'interfaccia della riga di comando di Azure](../articles/role-based-access-control/role-assignments-cli.md).