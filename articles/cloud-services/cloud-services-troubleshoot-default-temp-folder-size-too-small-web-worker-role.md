---
title: Dimensioni della cartella TEMP predefinita ridotte per un ruolo | Documentazione Microsoft
description: Un ruolo del servizio cloud dispone di una quantità limitata di spazio per la cartella TEMP. Questo articolo fornisce alcuni suggerimenti su come evitare l'esaurimento dello spazio.
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 1b7bfb47168c31f9e2e1b7e40764439118c00805
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2021
ms.locfileid: "98743203"
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-classic-webworker-role"></a>Dimensioni della cartella temporanea predefinite troppo piccole in un ruolo Web/di lavoro del servizio cloud (classico)

> [!IMPORTANT]
> [Servizi cloud di Azure (supporto esteso)](../cloud-services-extended-support/overview.md) è un nuovo modello di distribuzione basato su Azure Resource Manager per il prodotto servizi cloud di Azure.Con questa modifica, i servizi cloud di Azure in esecuzione nel modello di distribuzione basato su Service Manager di Azure sono stati rinominati come servizi cloud (versione classica) e tutte le nuove distribuzioni devono usare i [servizi cloud (supporto esteso)](../cloud-services-extended-support/overview.md).

La directory temporanea predefinita di un ruolo Web o di lavoro del servizio cloud ha una dimensione massima di 100 MB, che può esaurirsi. Questo articolo descrive come evitare l'esaurimento dello spazio della directory temporanea.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Perché ho esaurito lo spazio?
Le variabili di ambiente Windows standard, TEMP e TMP, sono disponibili per il codice in esecuzione nell'applicazione. Sia TEMP che TMP puntano a una singola directory con una dimensione massima di 100 MB. Tutti i dati archiviati in questa directory non sono persistenti nel ciclo di vita del servizio cloud. Se le istanze del ruolo in un servizio cloud vengono riciclate, la directory viene pulita.

## <a name="suggestion-to-fix-the-problem"></a>Suggerimento per risolvere il problema
Implementare una delle alternative seguenti:

* Configurare una risorsa di archiviazione locale e accedervi direttamente invece che usando TEMP o TMP. Per accedere a una risorsa di archiviazione locale dal codice in esecuzione nell'applicazione, chiamare il metodo [RoleEnvironment.GetLocalResource](/previous-versions/azure/reference/ee772845(v=azure.100)) .
* Configurare una risorsa di archiviazione locale e definire le directory TEMP e TMP in modo che puntino al percorso della risorsa di archiviazione locale. Questa modifica deve essere eseguita nel metodo [RoleEntryPoint.OnStart](/previous-versions/azure/reference/ee772851(v=azure.100)) .

L'esempio di codice seguente illustra come modificare le directory di destinazione per TEMP e TMP nel metodo OnStart:

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Passaggi successivi
Vedere il blog [How to increase the size of the Windows Azure Web Role ASP.NET Temporary Folder](/archive/blogs/kwill/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder)(Come aumentare le dimensioni della cartella temporanea ASP.NET del ruolo Web di Azure).

Altri [articoli sulla risoluzione dei problemi](/visualstudio/azure/vs-azure-tools-debugging-cloud-services-overview) per i servizi cloud.

Per informazioni su come risolvere i problemi dei ruoli del servizio cloud usando i dati di diagnostica del computer Azure PaaS, vedere la [serie di blog di Kevin Williamson](/archive/blogs/kwill/windows-azure-paas-compute-diagnostics-data).