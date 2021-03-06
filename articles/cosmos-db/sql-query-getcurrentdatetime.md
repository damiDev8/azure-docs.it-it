---
title: GetCurrentDateTime in linguaggio di query Azure Cosmos DB
description: Informazioni sulla funzione di sistema SQL GetCurrentDateTime in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: b48237b5a7eb836c495612758eeb9eaa45029b26
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99526586"
---
# <a name="getcurrentdatetime-azure-cosmos-db"></a>GetCurrentDateTime (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Restituisce la data e l'ora UTC (Coordinated Universal Time) correnti come stringa ISO 8601.
  
## <a name="syntax"></a>Sintassi
  
```sql
GetCurrentDateTime ()
```

## <a name="return-types"></a>Tipi restituiti
  
  Restituisce la data e l'ora UTC correnti del valore stringa ISO 8601 nel formato in `YYYY-MM-DDThh:mm:ss.fffffffZ` cui:
  
  |Formato|Descrizione|
  |-|-|
  |AAAA|anno a quattro cifre|
  |MM|mese a due cifre (01 = gennaio e così via)|
  |GG|giorno del mese a due cifre (da 01 a 31)|
  |T|significato per l'inizio degli elementi Time|
  |hh|ora a due cifre (da 00 a 23)|
  |MM|minuti a due cifre (da 00 a 59)|
  |ss|secondi a due cifre (da 00 a 59)|
  |. fffffff|secondi frazionari a sette cifre|
  |Z|Indicatore UTC (Coordinated Universal Time)||
  
  Per ulteriori informazioni sul formato ISO 8601, vedere [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601)

## <a name="remarks"></a>Commenti

GetCurrentDateTime () è una funzione non deterministica. Il risultato restituito è UTC. La precisione è 7 cifre, con un'accuratezza di 100 nanosecondi.

> [!NOTE]
> Questa funzione di sistema non utilizzerà l'indice. Se è necessario confrontare i valori con l'ora corrente, ottenere l'ora corrente prima dell'esecuzione della query e usare tale valore stringa costante nella `WHERE` clausola.

## <a name="examples"></a>Esempio
  
Nell'esempio seguente viene illustrato come ottenere la data e ora UTC correnti utilizzando la funzione predefinita GetCurrentDateTime ().
  
```sql
SELECT GetCurrentDateTime() AS currentUtcDateTime
```  
  
 Di seguito è riportato un esempio di set di risultati.
  
```json
[{
  "currentUtcDateTime": "2019-05-03T20:36:17.1234567Z"
}]  
```  

## <a name="next-steps"></a>Passaggi successivi

- [Funzioni di data e ora Azure Cosmos DB](sql-query-date-time-functions.md)
- [Funzioni di sistema in Azure Cosmos DB](sql-query-system-functions.md)
- [Introduzione ad Azure Cosmos DB](introduction.md)
