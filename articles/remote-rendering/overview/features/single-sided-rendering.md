---
title: Rendering a lato singolo
description: Descrive le impostazioni di rendering a lato singolo e i casi d'uso
author: florianborn71
ms.author: flborn
ms.date: 02/06/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: fea9deae3948b36732b5ea5203fceea6bec07fb9
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2021
ms.locfileid: "99594079"
---
# <a name="no-loc-textsingle-sided-rendering"></a>:::no-loc text="Single-sided"::: rendering

Per migliorare le prestazioni, la maggior parte dei renderer usa il [back-face culling](https://en.wikipedia.org/wiki/Back-face_culling), ossia l'eliminazione delle facce posteriori. Tuttavia, quando le mesh vengono aperte con [piani di taglio](cut-planes.md), gli utenti spesso osservano il lato posteriore dei triangoli. Se questi triangoli non vengono raffigurati, il risultato non è convincente.

Per evitare in modo affidabile questo problema, è necessario eseguire il rendering di *entrambi i lati* dei triangoli. Poiché non usare il back-face culling ha implicazioni sulle prestazioni, per impostazione predefinita Rendering remoto di Azure passa al rendering di entrambi i lati solo per le mesh che intersecano un piano di taglio.

L'impostazione di *:::no-loc text="single-sided"::: rendering* consente di personalizzare questo comportamento.

> [!CAUTION]
> L' :::no-loc text="single-sided"::: impostazione di rendering è una funzionalità sperimentale. Potrebbe essere rimossa in futuro. Non modificare l'impostazione predefinita, a meno che non risolva un problema critico nell'applicazione.

## <a name="prerequisites"></a>Prerequisiti

L' :::no-loc text="single-sided"::: impostazione di rendering ha un effetto solo per le mesh [convertite](../../how-tos/conversion/configure-model-conversion.md) con l' `opaqueMaterialDefaultSidedness` opzione impostata su `SingleSided` . Per impostazione predefinita, questa opzione è impostata su `DoubleSided`.

## <a name="no-loc-textsingle-sided-rendering-setting"></a>:::no-loc text="Single-sided"::: impostazione rendering

Esistono tre modalità diverse:

**Normal:** in questa modalità, le mesh vengono sempre rappresentate così come sono state convertite. In altre parole, il rendering delle mesh convertite con `opaqueMaterialDefaultSidedness` impostato su `SingleSided` viene sempre eseguito con il back-face culling abilitato, anche se intersecano un piano di taglio.

**DynamicDoubleSiding:** in questa modalità, quando un piano di taglio interseca una mesh viene automaticamente impostato il rendering di entrambi i lati. Questa è la modalità predefinita.

**AlwaysDoubleSided:** forza il rendering di entrambi i lati di tutte le geometrie a lato singolo in qualsiasi momento. Questa modalità viene esposta per lo più in modo da poter confrontare facilmente l'effetto sulle prestazioni tra :::no-loc text="single-sided"::: e il :::no-loc text="double-sided"::: rendering.

La modifica delle :::no-loc text="single-sided"::: impostazioni di rendering può essere eseguita come indicato di seguito:

```cs
void ChangeSingleSidedRendering(RenderingSession session)
{
    SingleSidedSettings settings = session.Connection.SingleSidedSettings;

    // Single-sided geometry is rendered as is
    settings.Mode = SingleSidedMode.Normal;

    // Single-sided geometry is always rendered double-sided
    settings.Mode = SingleSidedMode.AlwaysDoubleSided;
}
```

```cpp
void ChangeSingleSidedRendering(ApiHandle<RenderingSession> session)
{
    ApiHandle<SingleSidedSettings> settings = session->Connection()->GetSingleSidedSettings();

    // Single-sided geometry is rendered as is
    settings->SetMode(SingleSidedMode::Normal);

    // Single-sided geometry is always rendered double-sided
    settings->SetMode(SingleSidedMode::AlwaysDoubleSided);
}
```

## <a name="api-documentation"></a>Documentazione dell'API

* [Proprietà C# RenderingConnection. SingleSidedSettings](/dotnet/api/microsoft.azure.remoterendering.renderingconnection.singlesidedsettings)
* [C++ RenderingConnection:: SingleSidedSettings ()](/cpp/api/remote-rendering/renderingconnection#singlesidedsettings)

## <a name="next-steps"></a>Passaggi successivi

* [Piani di taglio](cut-planes.md)
* [Configurare la conversione di modelli](../../how-tos/conversion/configure-model-conversion.md)