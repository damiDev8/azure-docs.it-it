---
title: Indicizzazione in Azure Cosmos DB
description: Informazioni sul funzionamento dell'indicizzazione in Azure Cosmos DB, diversi tipi di indici, ad esempio intervallo, spaziale e indici compositi supportati.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/21/2020
ms.author: tisande
ms.openlocfilehash: b7349a08b93810dcc3befd6058302d6c4573ab8d
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98019234"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>Indicizzazione in Azure Cosmos DB - Panoramica
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Azure Cosmos DB è un database senza schema che consente di eseguire l'iterazione sull'applicazione senza dover gestire schemi o indici. Per impostazione predefinita, Azure Cosmos DB indicizza automaticamente ogni proprietà per tutti gli elementi nel [contenitore](account-databases-containers-items.md#azure-cosmos-containers) senza dover definire schemi né configurare indici secondari.

Lo scopo di questo articolo è spiegare come Azure Cosmos DB indicizza i dati e come usa gli indici per migliorare le prestazioni delle query. È consigliabile leggere questa sezione prima di esplorare come personalizzare i [criteri di indicizzazione](index-policy.md).

## <a name="from-items-to-trees"></a>Da elementi ad alberi

Ogni volta che un elemento viene archiviato in un contenitore, il relativo contenuto viene proiettato come documento JSON e quindi convertito in una rappresentazione ad albero. Ciò significa che ogni proprietà di tale elemento viene rappresentata sotto forma di nodo in un albero. Un nodo pseudoradice viene creato come elemento padre per tutte le proprietà di primo livello dell'elemento. I nodi foglia contengono i valori scalari effettivi di un elemento.

Ad esempio, considerare questo elemento:

```json
    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 },
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }
```

Verrà rappresentato dall'albero seguente:

:::image type="content" source="./media/index-overview/item-as-tree.png" alt-text="Elemento precedente rappresentato sotto forma di albero" border="false":::

Si noti il modo in cui le matrici vengono codificate nell'albero: ogni voce in una matrice ottiene un nodo intermedio etichettato con l'indice di tale voce all'interno della matrice (0, 1 e così via).

## <a name="from-trees-to-property-paths"></a>Da alberi a percorsi delle proprietà

Azure Cosmos DB trasforma gli elementi in alberi perché ciò consente di fare riferimento alle proprietà in base ai relativi percorsi all'interno di tali alberi. Per ottenere il percorso di una proprietà, è possibile eseguire l'attraversamento dell'albero dal nodo radice a tale proprietà e concatenare le etichette di ogni nodo attraversato.

Ecco i percorsi per ogni proprietà dell'elemento di esempio descritto in precedenza:

- /Locations/0/Country: "Germania"
- /Locations/0/City: "Berlino"
- /Locations/1/Country: "Francia"
- /Locations/1/City: "Parigi"
- /Headquarters/Country: "Belgio"
- /Headquarters/Employees: 250
- /exports/0/City: "Mosca"
- /exports/1/City: "Atene"

Quando viene scritto un elemento, Azure Cosmos DB indicizza in modo efficace il percorso di ogni proprietà e il relativo valore corrispondente.

## <a name="types-of-indexes"></a><a id="index-types"></a>Tipi di indici

Azure Cosmos DB supporta attualmente tre tipi di indici. È possibile configurare questi tipi di indice quando si definiscono i criteri di indicizzazione.

### <a name="range-index"></a>Indice di intervallo

L'indice di **intervallo** è basato su una struttura ad albero ordinata. Il tipo di indice di intervallo viene usato per:

- Query di uguaglianza:

    ```sql
   SELECT * FROM container c WHERE c.property = 'value'
   ```

   ```sql
   SELECT * FROM c WHERE c.property IN ("value1", "value2", "value3")
   ```

   Corrispondenza di uguaglianza su un elemento di matrice
   ```sql
    SELECT * FROM c WHERE ARRAY_CONTAINS(c.tags, "tag1")
    ```

- Query su intervallo:

   ```sql
   SELECT * FROM container c WHERE c.property > 'value'
   ```
  (per `>`, `<`, `>=`, `<=``!=`)

- Verifica della presenza di una proprietà:

   ```sql
   SELECT * FROM c WHERE IS_DEFINED(c.property)
   ```

- Funzioni di sistema di stringa:

   ```sql
   SELECT * FROM c WHERE CONTAINS(c.property, "value")
   ```

   ```sql
   SELECT * FROM c WHERE STRINGEQUALS(c.property, "value")
   ```

- Query `ORDER BY`:

   ```sql
   SELECT * FROM container c ORDER BY c.property
   ```

- Query `JOIN`:

   ```sql
   SELECT child FROM container c JOIN child IN c.properties WHERE child = 'value'
   ```

Gli indici di intervallo possono essere usati in valori scalari (stringa o numero). I criteri di indicizzazione predefiniti per i contenitori appena creati applicano indici di intervallo per qualsiasi stringa o numero. Per informazioni su come configurare gli indici di intervallo, vedere [esempi di criteri di indicizzazione di intervalli](how-to-manage-indexing-policy.md#range-index)

### <a name="spatial-index"></a>Indice spaziale

Gli indici **spaziali** consentono query efficienti su oggetti geospaziali , ad esempio punti, linee, poligoni e multipoligoni. Queste query usano le parole chiave ST_DISTANCE, ST_WITHIN, ST_INTERSECTS. Di seguito sono riportati alcuni esempi in cui viene utilizzato il tipo di indice spaziale:

- Query geospaziale distance:

   ```sql
   SELECT * FROM container c WHERE ST_DISTANCE(c.property, { "type": "Point", "coordinates": [0.0, 10.0] }) < 40
   ```

- Query geospaziale within:

   ```sql
   SELECT * FROM container c WHERE ST_WITHIN(c.property, {"type": "Point", "coordinates": [0.0, 10.0] })
   ```

- Query geospaziale intersect:

   ```sql
   SELECT * FROM c WHERE ST_INTERSECTS(c.property, { 'type':'Polygon', 'coordinates': [[ [31.8, -5], [32, -5], [31.8, -5] ]]  })  
   ```

Gli indici spaziali possono essere usati su oggetti [GeoJSON](./sql-query-geospatial-intro.md) con formato corretto. Attualmente sono supportati gli oggetti Point, LineString, Polygon e MultiPolygon. Per usare questo tipo di indice, impostare usando la `"kind": "Range"` proprietà quando si configurano i criteri di indicizzazione. Per informazioni su come configurare gli indici spaziali, vedere [esempi di criteri di indicizzazione spaziale](how-to-manage-indexing-policy.md#spatial-index)

### <a name="composite-indexes"></a>Indici composti

Gli indici **composti** aumentano l'efficienza quando si eseguono operazioni su più campi. Il tipo di indice composito viene usato per:

- Query `ORDER BY` su più proprietà:

```sql
 SELECT * FROM container c ORDER BY c.property1, c.property2
```

- Query con un filtro e `ORDER BY`. Queste query possono usare un indice composto se la proprietà di filtro viene aggiunta alla clausola `ORDER BY`.

```sql
 SELECT * FROM container c WHERE c.property1 = 'value' ORDER BY c.property1, c.property2
```

- Query con un filtro su due o più proprietà in cui almeno una proprietà è un filtro di uguaglianza

```sql
 SELECT * FROM container c WHERE c.property1 = 'value' AND c.property2 > 'value'
```

Fino a quando un predicato del filtro usa uno dei tipi di indice, il motore di query lo valuterà prima di analizzare il resto. Se si dispone ad esempio di una query SQL come `SELECT * FROM c WHERE c.firstName = "Andrew" and CONTAINS(c.lastName, "Liu")`

* La query precedente filtra innanzitutto le voci in cui firstName = "Andrew" usando l'indice. Passa quindi tutte le voci firstName = "Andrew" tramite una pipeline successiva per valutare il predicato del filtro CONTAINS.

* È possibile velocizzare le query ed evitare analisi complete del contenitore quando ci si serve di funzioni che non usano l'indice, ad esempio CONTAINS, aggiungendo altri predicati di filtro che usano l'indice. L'ordine delle clausole di filtro non è importante. Il motore di query consente di individuare i predicati più selettivi ed eseguire la query di conseguenza.

Per informazioni su come configurare gli indici compositi, vedere [esempi di criteri di indicizzazione composita](how-to-manage-indexing-policy.md#composite-index)

## <a name="querying-with-indexes"></a>Esecuzione di query con indici

I percorsi estratti durante l'indicizzazione dei dati facilitano la ricerca dell'indice durante l'elaborazione di una query. Abbinando la clausola `WHERE` di una query con l'elenco dei percorsi indicizzati, è possibile identificare molto rapidamente gli elementi che corrispondono al predicato della query.

Si consideri ad esempio la query seguente: `SELECT location FROM location IN company.locations WHERE location.country = 'France'`. Il predicato della query (filtro sugli elementi la cui posizione contiene "France" come paese/area geografica) corrisponderà al percorso evidenziato in rosso sotto:

:::image type="content" source="./media/index-overview/matching-path.png" alt-text="Corrispondenza di un percorso specifico all'interno di un albero" border="false":::

> [!NOTE]
> Una clausola `ORDER BY` che ordina in base a una singola proprietà necessita *sempre* di un indice di intervallo e avrà esito negativo se il percorso a cui fa riferimento non ne ha uno. In modo analogo, una query `ORDER BY` che ordina in base a più proprietà necessita *sempre* di un indice composto.

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sull'indicizzazione sono disponibili negli articoli seguenti:

- [Criterio di indicizzazione](index-policy.md)
- [Come gestire i criteri di indicizzazione](how-to-manage-indexing-policy.md)