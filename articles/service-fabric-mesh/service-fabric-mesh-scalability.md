---
title: Scalabilità delle app mesh di Azure Service Fabric
description: Uno dei vantaggi della distribuzione di applicazioni a Service Fabric mesh è la possibilità di ridimensionare facilmente i servizi, manualmente o con i criteri di scalabilità automatica.
author: georgewallace
ms.author: gwallace
ms.date: 10/26/2018
ms.topic: conceptual
ms.openlocfilehash: 3f0e115e596925878bf9fdd43b7074cefdba47b2
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/06/2021
ms.locfileid: "99626859"
---
# <a name="scaling-service-fabric-mesh-applications"></a>Applicazioni Service Fabric Mesh per il ridimensionamento

> [!IMPORTANT]
> L'anteprima di Azure Service Fabric mesh è stata ritirata. Le nuove distribuzioni non saranno più consentite tramite l'API Service Fabric mesh. Il supporto per le distribuzioni esistenti continuerà fino al 28 aprile 2021.
> 
> Per informazioni dettagliate, vedere il [ritiro anteprima di Azure Service Fabric mesh](https://azure.microsoft.com/updates/azure-service-fabric-mesh-preview-retirement/).

Uno dei principali vantaggi derivanti dalla distribuzione di applicazioni a Service Fabric mesh è la possibilità di ridimensionare facilmente i servizi. Questa operazione deve essere usata per la gestione di diverse quantità di carico nei servizi o per migliorare la disponibilità. È possibile ridurre o aumentare manualmente le istanze dei servizi o impostare criteri di ridimensionamento automatico.

## <a name="manual-scaling-instances"></a>Ridimensionamento manuale delle istanze

Nel modello di distribuzione per la risorsa dell'applicazione, ogni servizio ha una proprietà *replicaCount* che può essere usata per impostare il numero di volte in cui si vuole distribuire il servizio. Un'applicazione può essere costituita da più servizi, ognuno dei quali con un valore *replicaCount* univoco, che vengono distribuiti e gestiti insieme. Per ridimensionare il numero di repliche del servizio, modificare il valore *replicaCount* per ogni servizio ridimensionare nel modello o di distribuzione o nel file dei parametri. Aggiornare quindi l'applicazione.

Per esempi di ridimensionamento manualmente delle istanze dei servizi, vedere [Ridurre o aumentare manualmente il numero di istanze dei servizi](service-fabric-mesh-tutorial-template-scale-services.md).

## <a name="autoscaling-service-instances"></a>Ridimensionamento automatico delle istanze del servizio
Il ridimensionamento automatico è una funzionalità aggiuntiva di Service Fabric che consente di ridimensionare dinamicamente il numero di istanze del servizio (scalabilità orizzontale). Offre una notevole elasticità e consente di effettuare il provisioning o la rimozione delle istanze del servizio in base all'utilizzo della CPU o della memoria.  Il ridimensionamento automatico consente di eseguire il numero corretto di istanze del servizio per il carico di lavoro e di ottimizzare i costi.

Per ogni servizio i criteri di ridimensionamento vengono definiti automaticamente nel file di risorse del servizio. Ogni criterio di scalabilità automatica è costituito da due parti:

- Un trigger di ridimensionamento, che indica quando verrà eseguito il ridimensionamento del servizio. Esistono tre fattori che determinano il momento in cui il servizio viene ridimensionato. *Soglia di carico inferiore*: valore che determina quando le istanze del servizio si riducono. Se il carico medio di tutte le istanze delle partizioni è inferiore a questo valore, il servizio verrà ridotto. La *soglia di carico superiore* è un valore che determina quando il servizio verrà scalato orizzontalmente. Se il carico medio di tutte le istanze della partizione è superiore a questo valore, il servizio verrà scalato orizzontalmente. L' *intervallo di scalabilità* determina la frequenza (in secondi) in cui il trigger verrà controllato. Una volta controllato il trigger, se è necessario eseguire un ridimensionamento verrà applicato il meccanismo. Se il ridimensionamento non è necessario, non verrà eseguita alcuna azione. In entrambi i casi il trigger non verrà controllato nuovamente prima dello scadere dell'intervallo di ridimensionamento.

- Un meccanismo di ridimensionamento, che descrive come verrà eseguito il ridimensionamento dopo l'attivazione. *Incremento di scalabilità*: determina il numero di istanze che verranno aggiunte o rimosse quando viene attivato il meccanismo. Il *numero massimo di istanze* definisce il limite superiore per la scalabilità. Se il numero di istanze raggiunge questo limite, non sarà possibile aumentare le istanze del servizio, indipendentemente dal carico. *Numero minimo di istanze*: definisce il limite inferiore per il ridimensionamento. Se il numero di istanze della partizione raggiunge questo limite, non sarà possibile ridurre il servizio, indipendentemente dal carico.

Per le procedure di impostazione dei criteri di scalabilità automatica per il servizio, leggere le informazioni sul [ridimensionamento automatico dei servizi](service-fabric-mesh-howto-auto-scale-services.md).

## <a name="next-steps"></a>Passaggi successivi

Per informazioni sul modello dell'applicazione, consultare [risorse di Service Fabric](service-fabric-mesh-service-fabric-resources.md)
