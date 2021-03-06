---
title: File di inclusione
description: includere file
services: data-factory
author: lrtoyou1223
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: lle
ms.openlocfilehash: 45c6bbb88ef1f01f729451af27ecc73244483216
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2021
ms.locfileid: "98121668"
---
| Nomi di dominio                                          | Porte in uscita | Descrizione                |
| ----------------------------------------------------- | -------------- | ---------------------------|
| Cloud pubblico: `*.servicebus.windows.net` <br> Azure per enti pubblici: `*.servicebus.usgovcloudapi.net` <br> Cina `*.servicebus.chinacloudapi.cn`   | 443            | Richiesta dal runtime di integrazione self-hosted per la creazione interattiva. |
| Cloud pubblico: `{datafactory}.{region}.datafactory.azure.net`<br> o `*.frontend.clouddatahub.net` <br> Azure per enti pubblici: `{datafactory}.{region}.datafactory.azure.us` <br> Cina `{datafactory}.{region}.datafactory.azure.cn` | 443            | Richiesta dal runtime di integrazione self-hosted per connettersi al servizio Data Factory. <br>Per i nuovi Data Factory creati nel cloud pubblico, trovare il nome di dominio completo dalla chiave di Integration Runtime self-hosted nel formato {DataFactory}. {Region}. DataFactory. Azure. NET. Per i data factory precedenti, se il nome di dominio completo non è visibile nella chiave di integrazione self-hosted, usare invece *.frontend.clouddatahub.net. |
| `download.microsoft.com`    | 443            | Richiesta dal runtime di integrazione self-hosted per il download degli aggiornamenti. Se l'aggiornamento automatico è stato disabilitato, è possibile evitare di configurare questo dominio. |
| URL dell'insieme di credenziali delle chiavi | 443           | Richiesto da Azure Key Vault se si archiviano le credenziali in Key Vault. |
