---
title: 'Versioni supportate: database di Azure per PostgreSQL-server singolo'
description: Descrive le versioni principale e secondaria di Postgres supportate nel database di Azure per PostgreSQL-server singolo.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/16/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: f0c9a01e43f12e8fbe597c85c22b79c0994305e3
ms.sourcegitcommit: dea56e0dd919ad4250dde03c11d5406530c21c28
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2020
ms.locfileid: "96938869"
---
# <a name="supported-postgresql-major-versions"></a>Versioni principali di PostgreSQL supportate

Per informazioni sui criteri di supporto, vedere [criteri di controllo delle versioni di database di Azure per PostgreSQL](concepts-version-policy.md) .

Database di Azure per PostgreSQL supporta attualmente le versioni principali seguenti:

## <a name="postgresql-version-11"></a>PostgreSQL versione 11
La versione secondaria corrente è 11,6. Per ulteriori informazioni sui miglioramenti e sulle correzioni in questa versione secondaria, vedere la [documentazione di PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-6.html) .

## <a name="postgresql-version-10"></a>PostgreSQL versione 10
La versione secondaria corrente è 10,11. Per ulteriori informazioni sui miglioramenti e sulle correzioni in questa versione secondaria, vedere la [documentazione di PostgreSQL](https://www.postgresql.org/docs/10/static/release-10-11.html) .

## <a name="postgresql-version-96"></a>PostgreSQL versione 9,6
La versione secondaria corrente è 9.6.16. Per ulteriori informazioni sui miglioramenti e sulle correzioni in questa versione secondaria, vedere la [documentazione di PostgreSQL](https://www.postgresql.org/docs/9.6/static/release-9-6-16.html) .

## <a name="postgresql-version-95"></a>PostgreSQL versione 9,5
La versione secondaria corrente è 9.5.20. Per informazioni sui miglioramenti e sulle correzioni in questa versione secondaria, vedere la [documentazione di PostgreSQL](https://www.postgresql.org/docs/9.5/static/release-9-5-20.html) .

> [!NOTE]
> In linea con i criteri di [controllo delle versioni](https://www.postgresql.org/support/versioning/)della community di Postgres, database di Azure per PostgreSQL ritirerà Postgres versione 9,5 l'11 febbraio 2021. Per ulteriori informazioni e restrizioni, vedere [criteri di controllo delle versioni di database di Azure per PostgreSQL](concepts-version-policy.md) .

## <a name="managing-upgrades"></a>Gestione degli aggiornamenti
Il progetto PostgreSQL rilascia periodicamente versioni secondarie per correggere i bug segnalati. Database di Azure per PostgreSQL applica automaticamente patch ai server attraverso release minori durante le distribuzioni mensili del servizio. 

Gli aggiornamenti sul posto automatici per le versioni principali non sono supportati. Per eseguire l'aggiornamento a una versione principale più elevata, è possibile 
   * Usare uno dei metodi descritti negli [aggiornamenti principali della versione tramite dump e Restore](./how-to-upgrade-using-dump-and-restore.md).
   * Utilizzare [pg_dump e pg_restore](./howto-migrate-using-dump-and-restore.md) per spostare un database in un server creato con la nuova versione del motore.
   * Usare il [servizio migrazione del database di Azure](..\dms\tutorial-azure-postgresql-to-azure-postgresql-online-portal.md) per eseguire aggiornamenti online.

### <a name="version-syntax"></a>Sintassi della versione
Prima di PostgreSQL versione 10, i [criteri di controllo delle versioni di PostgreSQL](https://www.postgresql.org/support/versioning/) consideravano un aggiornamento della _versione principale_ come un aumento del primo _o_ del secondo numero. Ad esempio, 9,5 a 9,6 è stato considerato un aggiornamento della versione _principale_ . A partire dalla versione 10, solo una modifica nel primo numero viene considerata un aggiornamento della versione principale. 10,0 a 10,1, ad esempio, è un aggiornamento della versione _secondaria_ . La versione da 10 a 11 è un aggiornamento della versione _principale_ .

## <a name="next-steps"></a>Passaggi successivi
Per informazioni sulle estensioni PostgreSQL supportate, vedere [il documento sulle estensioni](concepts-extensions.md).
