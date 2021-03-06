---
title: Schema di definizione di servizi cloud di Azure (versione classica) (file con estensione csdef) | Microsoft Docs
description: Un file di definizione del servizio (con estensione csdef) definisce un modello di servizio per un'applicazione, contenente i ruoli disponibili, gli endpoint e i valori di configurazione per il servizio.
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: de81b6ffb5b4dc944f3d538a116383d06145661b
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2021
ms.locfileid: "98739802"
---
# <a name="azure-cloud-services-classic-definition-schema-csdef-file"></a>Schema di definizione di servizi cloud di Azure (versione classica) (file con estensione csdef)

> [!IMPORTANT]
> [Servizi cloud di Azure (supporto esteso)](../cloud-services-extended-support/overview.md) è un nuovo modello di distribuzione basato su Azure Resource Manager per il prodotto servizi cloud di Azure.Con questa modifica, i servizi cloud di Azure in esecuzione nel modello di distribuzione basato su Service Manager di Azure sono stati rinominati come servizi cloud (versione classica) e tutte le nuove distribuzioni devono usare i [servizi cloud (supporto esteso)](../cloud-services-extended-support/overview.md).

Il file di definizione del servizio definisce il modello di servizio per un'applicazione. Il file contiene le definizioni per i ruoli disponibili per un servizio cloud, specifica gli endpoint di servizio e stabilisce le impostazioni di configurazione per il servizio. I valori delle impostazioni di configurazione vengono impostati nel file di configurazione del servizio, come descritto in [Cloud Service (classic) Configuration Schema](/previous-versions/azure/reference/ee758710(v=azure.100)) (Schema di configurazione del servizio Cloud (classico)).

Per impostazione predefinita, il file dello schema di configurazione di Diagnostica di Azure viene installato nella directory `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas`. Sostituire `<version>` con la versione installata di [Azure SDK](https://www.windowsazure.com/develop/downloads/).

L'estensione predefinita per il file di definizione del servizio è csdef.

## <a name="basic-service-definition-schema"></a>Schema di definizione del servizio di base
Il file di definizione del servizio deve contenere un elemento `ServiceDefinition`. La definizione del servizio deve contenere almeno un elemento ruolo (`WebRole` o `WorkerRole`). Può contenere fino a 25 ruoli definiti in una singola definizione ed è possibile combinare i tipi di ruolo. La definizione del servizio contiene inoltre l'elemento facoltativo `NetworkTrafficRules` che limita i ruoli che possono comunicare con endpoint interni specificati. La definizione del servizio contiene inoltre l'elemento facoltativo `LoadBalancerProbes` che contiene i probe di integrità degli endpoint definiti dal cliente.

Il formato di base del file di definizione del servizio è il seguente.

```xml
<ServiceDefinition name="<service-name>" topologyChangeDiscovery="<change-type>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" upgradeDomainCount="<number-of-upgrade-domains>" schemaVersion="<version>">
  
  <LoadBalancerProbes>
         …
  </LoadBalancerProbes>
  
  <WebRole …>
         …
  </WebRole>
  
  <WorkerRole …>
         …
  </WorkerRole>
  
  <NetworkTrafficRules>
         …
  </NetworkTrafficRules>

</ServiceDefinition>
```

## <a name="schema-definitions"></a>Definizioni dello schema
Gli argomenti seguenti descrivono lo schema:

- [Schema LoadBalancerProbe](schema-csdef-loadbalancerprobe.md)
- [Schema WebRole](schema-csdef-webrole.md)
- [Schema WorkerRole](schema-csdef-workerrole.md)
- [Schema NetworkTrafficRules](schema-csdef-networktrafficrules.md)

##  <a name="servicedefinition-element"></a><a name="ServiceDefinition"></a> Elemento ServiceDefinition
L'elemento `ServiceDefinition` è l'elemento di livello superiore del file di definizione del servizio.

La tabella seguente descrive gli attributi dell'elemento `ServiceDefinition`.

| Attributo               | Descrizione |
| ----------------------- | ----------- |
| name                    |Obbligatorio. Il nome del servizio. Il nome deve essere univoco all'interno dell'account del servizio.|
| topologyChangeDiscovery | facoltativo. Specifica il tipo di notifica di modifica della topologia. I valori possibili sono:<br /><br /> -   `Blast` - Invia l'aggiornamento appena possibile a tutte le istanze del ruolo. Se si sceglie l'opzione, il ruolo deve essere in grado di gestire l'aggiornamento della topologia senza necessità di riavvio.<br />-   `UpgradeDomainWalk` - Invia l'aggiornamento a ogni istanza del ruolo in modo sequenziale dopo che l'istanza precedente ha accettato correttamente l'aggiornamento.|
| schemaVersion           | facoltativo. Specifica la versione dello schema di definizione del servizio. La versione dello schema consente a Visual Studio di selezionare gli strumenti SDK corretti da usare per la convalida dello schema se più di una versione dell'SDK è installata side-by-side.|
| upgradeDomainCount      | facoltativo. Specifica il numero di domini di aggiornamento in cui vengono allocati i ruoli nel servizio. Le istanze del ruolo vengono allocate a un dominio di aggiornamento quando viene distribuito il servizio. Per altre informazioni, vedere [aggiornare un ruolo o una distribuzione del servizio cloud](cloud-services-how-to-manage-portal.md#update-a-cloud-service-role-or-deployment), [gestire la disponibilità delle macchine virtuali](../virtual-machines/manage-availability.md) e informazioni su [un modello di servizio cloud](./cloud-services-model-and-package.md).<br /><br /> È possibile specificare fino a 20 domini di aggiornamento. Il numero predefinito di domini di aggiornamento è 5.|