---
title: Caricare contenuto in un'interfaccia della riga di comando di asset
description: Lo script dell'interfaccia della riga di comando di Azure in questo argomento mostra come creare un asset di Servizi multimediali di Azure in cui caricare contenuto.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: how-to
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 6f3c5fa41150f5df2b0e89c4253cb0bb55b1d625
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/28/2021
ms.locfileid: "98955818"
---
# <a name="create-an-asset"></a>Creare un asset

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Questo articolo illustra come creare un asset di Servizi multimediali.  L'asset verrà usato per includere contenuti multimediali per la codifica e lo streaming.  Per altre informazioni sugli asset di Servizi multimediali, leggere [Asset in Servizi multimediali di Azure v3](assets-concept.md)

## <a name="prerequisites"></a>Prerequisiti

Seguire la procedura disponibile in [Creare un account di Servizi multimediali](./create-account-howto.md) per creare l'account e il gruppo di risorse di Servizi multimediali necessari per creare un asset.

## <a name="methods"></a>Metodi

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [Create an asset with CLI](./includes/task-create-asset-cli.md)]

## <a name="example-script"></a>Script di esempio

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/create-asset/Create-Asset.sh "Create an asset")]

## <a name="rest"></a>[REST](#tab/rest/)

### <a name="using-rest"></a>Uso di REST

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-rest.md)]

### <a name="using-curl"></a>Uso di cURL

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-curl.md)]

## <a name="using-postman"></a>Uso di Postman

[!INCLUDE[Create an asset with Postman](./includes/task-create-asset-postman.md)]

## <a name="net"></a>[.NET](#tab/net/)

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-dotnet.md)]

---

## <a name="next-steps"></a>Passaggi successivi

[Panoramica di servizi multimediali](media-services-overview.md)
