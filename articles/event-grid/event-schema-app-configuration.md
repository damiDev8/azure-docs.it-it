---
title: Configurazione app Azure come origine griglia di eventi
description: Questo articolo descrive come usare la configurazione di app Azure come origine evento di griglia di eventi. Offre lo schema e i collegamenti ad articoli di esercitazione e procedure.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: d305236e8408052be4be28ec003f4e545119fc59
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/04/2021
ms.locfileid: "99550675"
---
# <a name="azure-app-configuration-as-an-event-grid-source"></a>Configurazione app Azure come origine di griglia di eventi
Questo articolo fornisce le proprietà e lo schema per gli eventi di configurazione app Azure. Per un'introduzione agli schemi di eventi, vedere [Schema di eventi di Griglia di eventi di Azure](event-schema.md). Viene inoltre visualizzato un elenco di guide introduttive ed esercitazioni per utilizzare app Azure configurazione come origine evento.

## <a name="event-grid-event-schema"></a>Schema di eventi di Griglia di eventi

### <a name="available-event-types"></a>Tipi di evento disponibili

App Azure configurazione genera i tipi di eventi seguenti:

| Tipo di evento | Descrizione |
| ---------- | ----------- |
| Microsoft. AppConfiguration. KeyValueModified | Generato quando viene creato o sostituito un valore chiave. |
| Microsoft. AppConfiguration. KeyValueDeleted | Generato quando un valore di chiave viene eliminato. |

### <a name="example-event"></a>Evento di esempio

Nell'esempio seguente viene illustrato lo schema di un evento di modifica chiave-valore: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Lo schema per un evento di eliminazione chiave-valore è simile: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueDeleted",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```
 
### <a name="event-properties"></a>Proprietà degli eventi

Un evento presenta i seguenti dati di primo livello:

| Proprietà | Type | Descrizione |
| -------- | ---- | ----------- |
| argomento | string | Percorso risorsa completo dell'origine evento. Questo campo non è scrivibile. Questo valore viene fornito da Griglia di eventi. |
| subject | string | Percorso dell'oggetto dell'evento definito dall'origine di pubblicazione. |
| eventType | string | Uno dei tipi di evento registrati per l'origine evento. |
| eventTime | string | Ora di generazione dell'evento in base all'ora UTC del provider. |
| ID | string | Identificatore univoco dell'evento. |
| data | object | Dati degli eventi di configurazione dell'app. |
| dataVersion | string | Versione dello schema dell'oggetto dati. La versione dello schema è definita dall'origine di pubblicazione. |
| metadataVersion | string | Versione dello schema dei metadati dell'evento. Lo schema delle proprietà di primo livello è definito da Griglia di eventi. Questo valore viene specificato da Griglia di eventi. |

Di seguito sono elencate le proprietà dell'oggetto dati:

| Proprietà | Type | Descrizione |
| -------- | ---- | ----------- |
| Key | string | Chiave del valore della chiave che è stato modificato o eliminato. |
| label | string | Etichetta, se presente, del valore della chiave che è stato modificato o eliminato. |
| etag | string | Per `KeyValueModified` l'ETag del nuovo valore di chiave. Per `KeyValueDeleted` l'ETag del valore della chiave che è stato eliminato. |

## <a name="tutorials-and-how-tos"></a>Esercitazioni e procedure

|Titolo | Descrizione |
|---------|---------|
| [Reagire agli eventi di configurazione di app Azure tramite griglia di eventi](../azure-app-configuration/concept-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Panoramica dell'integrazione di app Azure configurazione con griglia di eventi. |
| [Usa griglia di eventi per le notifiche di modifica dei dati](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Informazioni su come usare le sottoscrizioni di eventi di configurazione app Azure per inviare eventi di modifica chiave-valore a un endpoint Web. |

## <a name="next-steps"></a>Passaggi successivi

* Per un'introduzione a Griglia di eventi di Azure, vedere [Informazioni su Griglia di eventi](overview.md)
* Per altre informazioni sulla creazione di una sottoscrizione di Griglia di eventi di Azure, vedere [Schema di sottoscrizione per Griglia di eventi](subscription-creation-schema.md).
* Per un'introduzione all'uso di eventi di configurazione app Azure, vedere [usare griglia di eventi per le notifiche di modifica dei dati](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json). 