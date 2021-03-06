---
title: Come specificare le versioni del runtime per Funzioni di Azure
description: La soluzione Funzioni di Azure supporta più versioni del runtime. Informazioni su come specificare la versione del runtime di un'app per le funzioni ospitata in Azure.
ms.topic: conceptual
ms.date: 07/22/2020
ms.openlocfilehash: 46bf7849888033b2bbb7e9b9669ee3eae4de10e9
ms.sourcegitcommit: 67b44a02af0c8d615b35ec5e57a29d21419d7668
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2021
ms.locfileid: "97916525"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Come specificare le versioni del runtime per Funzioni di Azure

L'app per le funzioni viene eseguita su una versione specifica del runtime di Funzioni di Azure. Sono disponibili tre versioni principali: [1. x, 2. x e 3. x](functions-versions.md). Per impostazione predefinita, le app per le funzioni vengono create nella versione 3. x del runtime. In questo articolo viene illustrato come configurare l'esecuzione di un'app per le funzioni in Azure con la versione scelta. Per informazioni su come configurare un ambiente di sviluppo locale per una versione specifica, vedere [Scrivere codici per Funzioni di Azure e testarle in locale](functions-run-local.md).

La modalità di destinazione manuale di una versione specifica varia a seconda che si esegua Windows o Linux.

## <a name="automatic-and-manual-version-updates"></a>Aggiornamenti di versione automatici e manuali

_Questa sezione non si applica quando si esegue l'app per le funzioni [in Linux](#manual-version-updates-on-linux)._

Funzioni di Azure consente di specificare come destinazione una versione specifica del runtime in Windows usando l' `FUNCTIONS_EXTENSION_VERSION` impostazione dell'applicazione in un'app per le funzioni. L'app per le funzioni viene mantenuta nella versione principale specificata fino a quando non si sceglie esplicitamente di spostarla in una nuova versione. Se si specifica solo la versione principale, l'app per le funzioni viene aggiornata automaticamente alle nuove versioni secondarie del runtime quando diventano disponibili. Le nuove versioni secondarie non devono introdurre modifiche di rilievo. 

Se si specifica una versione secondaria, ad esempio "2.0.12345", l'app per le funzioni viene bloccata alla versione specifica fino a quando non viene modificata in modo esplicito. Le versioni secondarie precedenti vengono rimosse periodicamente dall'ambiente di produzione. Al termine di questa operazione, l'app per le funzioni viene eseguita sulla versione più recente anziché sulla versione impostata in `FUNCTIONS_EXTENSION_VERSION` . Per questo motivo, è necessario risolvere rapidamente eventuali problemi con l'app per le funzioni che richiedono una versione secondaria specifica, in modo che sia possibile fare riferimento alla versione principale. Le rimozioni della versione secondaria sono annunciate negli [annunci del servizio app](https://github.com/Azure/app-service-announcements/issues).

> [!NOTE]
> Se si aggiunge una versione principale specifica di funzioni di Azure e quindi si prova a eseguire la pubblicazione in Azure tramite Visual Studio, viene visualizzata una finestra di dialogo che richiede di eseguire l'aggiornamento alla versione più recente o di annullare la pubblicazione. Per evitare questo problema, aggiungere la `<DisableFunctionExtensionVersionUpdate>true</DisableFunctionExtensionVersionUpdate>` proprietà nel `.csproj` file.

Quando una nuova versione risulta disponibile pubblicamente, nel portale viene visualizzata una richiesta che consente di eseguire l'aggiornamento alla nuova versione. Dopo il passaggio a una nuova versione, è sempre possibile usare l'impostazione `FUNCTIONS_EXTENSION_VERSION` dell'applicazione per tornare a una versione precedente.

La tabella seguente mostra i `FUNCTIONS_EXTENSION_VERSION` valori per ogni versione principale per abilitare gli aggiornamenti automatici:

| Versione principale | Valore della proprietà `FUNCTIONS_EXTENSION_VERSION` |
| ------------- | ----------------------------------- |
| 3.x  | `~3` |
| 2.x  | `~2` |
| 1.x  | `~1` |

Ogni modifica alla versione del runtime comporta il riavvio dell'app per le funzioni.

## <a name="view-and-update-the-current-runtime-version"></a>Visualizzare e aggiornare la versione corrente del runtime

_Questa sezione non si applica quando si esegue l'app per le funzioni [in Linux](#manual-version-updates-on-linux)._

È possibile modificare la versione del runtime usata dall'app per le funzioni. A causa del potenziale di modifiche di rilievo, è possibile modificare solo la versione di runtime prima di creare funzioni nell'app per le funzioni. 

> [!IMPORTANT]
> Anche se la versione del runtime è determinata dall'impostazione `FUNCTIONS_EXTENSION_VERSION`, è necessario apportare questa modifica nel portale di Azure e non modificando l'impostazione direttamente, perché il portale convalida le modifiche e apporta altre modifiche correlate in base alle esigenze.

# <a name="portal"></a>[Portale](#tab/portal)

[!INCLUDE [Set the runtime version in the portal](../../includes/functions-view-update-version-portal.md)]

> [!NOTE]
> Utilizzando la portale di Azure, non è possibile modificare la versione di runtime per un'app per le funzioni che contiene già funzioni.

# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azurecli)

È anche possibile visualizzare e configurare `FUNCTIONS_EXTENSION_VERSION` dall'interfaccia della riga di comando di Azure.  

Tramite l'interfaccia della riga di comando di Azure, visualizzare la versione corrente del runtime con il comando [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings).

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

In questo codice sostituire `<function_app>` con il nome dell'app per le funzioni. Sostituire anche `<my_resource_group>` con il nome del gruppo di risorse per l'app per le funzioni. 

Verrà visualizzato `FUNCTIONS_EXTENSION_VERSION` nell'output seguente, troncato per maggiore chiarezza:

```output
[
  {
    "name": "FUNCTIONS_EXTENSION_VERSION",
    "slotSetting": false,
    "value": "~2"
  },
  {
    "name": "FUNCTIONS_WORKER_RUNTIME",
    "slotSetting": false,
    "value": "dotnet"
  },
  
  ...
  
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "8.11.1"
  }
]
```

È possibile aggiornare l'impostazione `FUNCTIONS_EXTENSION_VERSION` nell'app per le funzioni con il comando [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings).

```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP> \
--resource-group <RESOURCE_GROUP> \
--settings FUNCTIONS_EXTENSION_VERSION=<VERSION>
```

Sostituire `<FUNCTION_APP>` con il nome dell'app per le funzioni. Sostituire anche `<RESOURCE_GROUP>` con il nome del gruppo di risorse per l'app per le funzioni. Sostituire anche `<VERSION>` con una versione specifica o `~3` , o `~2` `~1` .

È possibile eseguire questo comando da [Azure Cloud Shell](../cloud-shell/overview.md) scegliendo **Prova** nell'esempio di codice precedente. È anche possibile usare l'[interfaccia della riga di comando di Azure in locale](/cli/azure/install-azure-cli) per eseguire questo comando dopo l'esecuzione del comando [az login](/cli/azure/reference-index#az-login) per eseguire l'accesso.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Per controllare il runtime di funzioni di Azure, usare il cmdlet seguente: 

```powershell
Get-AzFunctionAppSetting -Name "<FUNCTION_APP>" -ResourceGroupName "<RESOURCE_GROUP>"
```

Sostituire `<FUNCTION_APP>` con il nome dell'app per le funzioni e `<RESOURCE_GROUP>` . Il valore corrente dell' `FUNCTIONS_EXTENSION_VERSION` impostazione viene restituito nella tabella hash.

Usare lo script seguente per modificare il runtime di funzioni:

```powershell
Update-AzFunctionAppSetting -Name "<FUNCTION_APP>" -ResourceGroupName "<RESOURCE_GROUP>" -AppSetting @{"FUNCTIONS_EXTENSION_VERSION" = "<VERSION>"} -Force
```

Come in precedenza, sostituire `<FUNCTION_APP>` con il nome dell'app per le funzioni e `<RESOURCE_GROUP>` con il nome del gruppo di risorse. Sostituire anche `<VERSION>` con la versione specifica o la versione principale, ad esempio `~2` o `~3` . È possibile verificare il valore aggiornato dell' `FUNCTIONS_EXTENSION_VERSION` impostazione nella tabella hash restituita. 

---

L'app per le funzioni viene riavviata dopo la modifica apportata all'impostazione dell'applicazione.

## <a name="manual-version-updates-on-linux"></a>Aggiornamenti manuali delle versioni in Linux

Per aggiungere un'app per le funzioni di Linux a una versione specifica dell'host, è necessario specificare l'URL dell'immagine nel campo ' LinuxFxVersion ' nella configurazione del sito. Ad esempio, se si vuole aggiungere un'app per le funzioni node 10 per indicare la versione host 3.0.13142-

Per le app di **servizio app Linux/Premium elastico** : impostare `LinuxFxVersion` su `DOCKER|mcr.microsoft.com/azure-functions/node:3.0.13142-node10-appservice` .

Per le **app a consumo Linux** : impostare `LinuxFxVersion` su `DOCKER|mcr.microsoft.com/azure-functions/mesh:3.0.13142-node10` .


# <a name="azure-cli"></a>[Interfaccia della riga di comando di Azure](#tab/azurecli-linux)

È possibile visualizzare e impostare `LinuxFxVersion` dall'interfaccia della riga di comando di Azure.  

Usando l'interfaccia della riga di comando di Azure, visualizzare la versione corrente del runtime con il comando [AZ functionapp config Show](/cli/azure/functionapp/config) .

```azurecli-interactive
az functionapp config show --name <function_app> \
--resource-group <my_resource_group>
```

In questo codice sostituire `<function_app>` con il nome dell'app per le funzioni. Sostituire anche `<my_resource_group>` con il nome del gruppo di risorse per l'app per le funzioni. 

Verrà visualizzato `linuxFxVersion` nell'output seguente, troncato per maggiore chiarezza:

```output
{
  ...

  "kind": null,
  "limits": null,
  "linuxFxVersion": <LINUX_FX_VERSION>,
  "loadBalancing": "LeastRequests",
  "localMySqlEnabled": false,
  "location": "West US",
  "logsDirectorySizeLimit": 35,
   ...
}
```

È possibile aggiornare l' `linuxFxVersion` impostazione nell'app per le funzioni con il comando [AZ functionapp config set](/cli/azure/functionapp/config) .

```azurecli-interactive
az functionapp config set --name <FUNCTION_APP> \
--resource-group <RESOURCE_GROUP> \
--linux-fx-version <LINUX_FX_VERSION>
```

Sostituire `<FUNCTION_APP>` con il nome dell'app per le funzioni. Sostituire anche `<RESOURCE_GROUP>` con il nome del gruppo di risorse per l'app per le funzioni. Sostituire inoltre `<LINUX_FX_VERSION>` con i valori descritti in precedenza.

È possibile eseguire questo comando da [Azure Cloud Shell](../cloud-shell/overview.md) scegliendo **Prova** nell'esempio di codice precedente. È anche possibile usare l'[interfaccia della riga di comando di Azure in locale](/cli/azure/install-azure-cli) per eseguire questo comando dopo l'esecuzione del comando [az login](/cli/azure/reference-index#az-login) per eseguire l'accesso.


Analogamente, l'app per le funzioni viene riavviata dopo la modifica apportata alla configurazione del sito.

> [!NOTE]
> Si noti che l'impostazione `LinuxFxVersion` dell'URL dell'immagine direttamente per le app a consumo può escludere i segnaposto e altre ottimizzazioni di avvio a freddo.

---

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Usare il runtime 2.0 nell'ambiente di sviluppo locale](functions-run-local.md)

> [!div class="nextstepaction"]
> [Vedere le note sulla versione per le versioni del runtime](https://github.com/Azure/azure-webjobs-sdk-script/releases)
