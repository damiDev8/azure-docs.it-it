---
title: disconnessione azcopy | Microsoft Docs
description: Questo articolo contiene informazioni di riferimento per il comando di disconnessione di azcopy.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 987f5de78edc4c0fe2814259a1461e58018ec270
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98879021"
---
# <a name="azcopy-logout"></a>azcopy logout

Disconnette l'utente e termina l'accesso alle risorse di archiviazione di Azure.

## <a name="synopsis"></a>Riepilogo

Tramite questo comando vengono rimosse tutte le informazioni di accesso memorizzate nella cache per l'utente corrente.

```azcopy
azcopy logout [flags]
```

## <a name="related-conceptual-articles"></a>Articoli concettuali correlati

- [Introduzione ad AzCopy](storage-use-azcopy-v10.md)
- [Trasferire i dati con AzCopy e l'archiviazione BLOB](./storage-use-azcopy-v10.md#transfer-data)
- [Trasferire dati con AzCopy e l'archivio file](storage-use-azcopy-files.md)
- [Configurare, ottimizzare e risolvere i problemi di AzCopy](storage-use-azcopy-configure.md)

## <a name="options"></a>Opzioni

|Opzione|Descrizione|
|--|--|
|-h, --help|Mostra il contenuto della Guida per il comando Disconnetti.|

## <a name="options-inherited-from-parent-commands"></a>Opzioni ereditate dai comandi padre

|Opzione|Descrizione|
|---|---|
|--Cap-Mbps float|Viene riversata la velocità di trasferimento, in megabit al secondo. Una velocità effettiva momentanea potrebbe variare leggermente rispetto al limite. Se questa opzione è impostata su zero o viene omessa, la velocità effettiva non è limitata.|
|--output-tipo stringa|Formato dell'output del comando. Le scelte includono: text, JSON. Il valore predefinito è "Text".|
|--trusted-Microsoft-suffissi stringa   |Specifica i suffissi di dominio aggiuntivi in cui è possibile inviare i token di accesso Azure Active Directory.  Il valore predefinito è'*. Core.Windows.NET;*. core.chinacloudapi.cn; *. Core.cloudapi.de;*. core.usgovcloudapi.net ". Tutti gli elencati qui vengono aggiunti al valore predefinito. Per la sicurezza, è consigliabile inserire qui solo Microsoft Azure domini. Separare più voci con un punto e virgola.|

## <a name="see-also"></a>Vedi anche

- [azcopy](storage-ref-azcopy.md)
