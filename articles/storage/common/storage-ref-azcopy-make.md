---
title: azcopy make | Microsoft Docs
description: Questo articolo contiene informazioni di riferimento per il comando azcopy make.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: f6d9645e614730d2eb5fad8bca30486e9afef618
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98878342"
---
# <a name="azcopy-make"></a>azcopy make

Crea un contenitore o una condivisione file.

## <a name="synopsis"></a>Riepilogo

Creare un contenitore o una condivisione file rappresentata dall'URL della risorsa specificato.

```azcopy
azcopy make [resourceURL] [flags]
```

## <a name="related-conceptual-articles"></a>Articoli concettuali correlati

- [Introduzione ad AzCopy](storage-use-azcopy-v10.md)
- [Trasferire i dati con AzCopy e l'archiviazione BLOB](./storage-use-azcopy-v10.md#transfer-data)
- [Trasferire dati con AzCopy e l'archivio file](storage-use-azcopy-files.md)
- [Configurare, ottimizzare e risolvere i problemi di AzCopy](storage-use-azcopy-configure.md)

## <a name="examples"></a>Esempio

```azcopy
azcopy make "https://[account-name].[blob,file,dfs].core.windows.net/[top-level-resource-name]"
```

## <a name="options"></a>Opzioni

|Opzione|Descrizione|
|--|--|
|-h, --help|Mostra il contenuto della Guida per il comando make. |
|--quota-GB UInt32|Specifica la dimensione massima della condivisione espressa in gigabyte (GB), zero indica che si accetta la quota predefinita del servizio file.|

## <a name="options-inherited-from-parent-commands"></a>Opzioni ereditate dai comandi padre

|Opzione|Descrizione|
|---|---|
|--Cap-Mbps float|Viene riversata la velocità di trasferimento, in megabit al secondo. Una velocità effettiva momentanea potrebbe variare leggermente rispetto al limite. Se questa opzione è impostata su zero o viene omessa, la velocità effettiva non è limitata.|
|--output-tipo stringa|Formato dell'output del comando. Le scelte includono: text, JSON. Il valore predefinito è "Text".|
|--trusted-Microsoft-suffissi stringa   |Specifica i suffissi di dominio aggiuntivi in cui è possibile inviare i token di accesso Azure Active Directory.  Il valore predefinito è'*. Core.Windows.NET;*. core.chinacloudapi.cn; *. Core.cloudapi.de;*. core.usgovcloudapi.net ". Tutti gli elencati qui vengono aggiunti al valore predefinito. Per la sicurezza, è consigliabile inserire qui solo Microsoft Azure domini. Separare più voci con un punto e virgola.|

## <a name="see-also"></a>Vedi anche

- [azcopy](storage-ref-azcopy.md)
