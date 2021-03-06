---
title: Comporre configurazioni DSC
description: Questo articolo spiega come creare configurazioni usando risorse composite in State Configuration di Automazione di Azure.
keywords: powershell dsc, Desired State Configuration, powershell dsc azure
services: automation
ms.subservice: dsc
ms.date: 08/21/2018
ms.topic: conceptual
ms.openlocfilehash: 1b1bbb12412deec6ecac8cf1ffd47a00f778862e
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98894729"
---
# <a name="compose-dsc-configurations"></a>Comporre configurazioni DSC

Se è necessario gestire una risorsa con più di una configurazione DSC, è consigliabile usare le [risorse composite](/powershell/scripting/dsc/resources/authoringresourcecomposite). Una risorsa composita è una configurazione annidata e con parametri usata come risorsa DSC all'interno di un'altra configurazione. L'uso di risorse composite consente di creare configurazioni complesse e al tempo stesso gestire e creare singolarmente le risorse composite sottostanti.

Automazione di Azure consente l'[importazione e la compilazione di risorse composite](automation-dsc-compile.md). Dopo aver importato le risorse composite nell'account di automazione, è possibile usare la configurazione dello stato di automazione di Azure tramite la funzionalità di **configurazione dello stato (DSC)** nell'portale di Azure.

## <a name="compose-a-configuration"></a>Creare una configurazione

Prima di poter assegnare una configurazione costituita da risorse composite nel portale di Azure, è necessario crearla. La creazione usa **Crea configurazione** nella pagina State Configuration (DSC), nella scheda **Configurazioni** o **Configurazioni compilate**.

1. Accedere al [portale di Azure](https://portal.azure.com).
1. A sinistra fare clic su **Tutte le risorse** e quindi fare clic sul nome dell'account di Automazione.
1. Nella pagina Account di automazione selezionare **State Configuration (DSC)** in **Gestione della configurazione**.
1. Nella pagina State Configuration (DSC) fare clic sulle schede **Configurazioni** o **Configurazioni compilate**, quindi selezionare **Crea configurazione** nel menu nella parte superiore della pagina.
1. Nell passaggio **Informazioni di base** specificare il nome della configurazione (obbligatorio) e fare clic su un punto qualsiasi nella riga di ogni risorsa composita che si vuole includere nella nuova configurazione, quindi fare clic su **Avanti** oppure fare clic sul passaggio **Codice sorgente**. Per la procedura seguente abbiamo selezionato le risorse composite `PSExecutionPolicy` e `RenameAndDomainJoin`.
   ![Screenshot delle informazioni di base nella pagina di creazione della configurazione](./media/compose-configurationwithcompositeresources/compose-configuration-basics.png)
1. Nel passaggio **Codice sorgente** viene visualizzato l'aspetto della configurazione creata con le risorse composite selezionate. È possibile notare che tutti parametri sono stati uniti e passati alla risorsa composita. Dopo aver esaminato il nuovo codice sorgente, fare clic su **Avanti** oppure fare clic sul passaggio **Parametri**.
   ![Screenshot del codice sorgente nella pagina di creazione della configurazione](./media/compose-configurationwithcompositeresources/compose-configuration-sourcecode.png)
1. Nel passaggio **Parametri** viene esposto il parametro di ogni risorsa composita in modo che sia possibile specificare i valori. Se per il parametro esiste una descrizione, questa viene visualizzata accanto al campo del parametro. Se un parametro è di tipo `PSCredential`, il menu a discesa fornisce un elenco di oggetti di tipo **Credential** nell'account di Automazione corrente. È anche disponibile l'opzione **+ Aggiungi credenziali**. Dopo aver specificato tutti i parametri necessari, fare clic su **Salva e compila**.
   ![Screenshot dei parametri nella pagina di creazione della configurazione](./media/compose-configurationwithcompositeresources/compose-configuration-parameters.png)

## <a name="submit-the-configuration-for-compilation"></a>Inviare la configurazione per la compilazione

Dopo averla salvata, la configurazione viene inoltrata per essere compilata. È possibile visualizzare lo stato del processo di compilazione come per qualsiasi configurazione importata. Per altre informazioni, vedere [Visualizzare un processo di compilazione](automation-dsc-getting-started.md#view-a-compilation-job).

Se la compilazione è stata completata, la nuova configurazione viene visualizzata nella scheda **Configurazioni compilate**. È dunque possibile assegnare la configurazione a un nodo gestito seguendo la procedura descritta in [Riassegnare un nodo a una diversa configurazione di nodo](automation-dsc-getting-started.md#reassign-a-node-to-a-different-node-configuration).

## <a name="next-steps"></a>Passaggi successivi

- Per informazioni su come abilitare i nodi, vedere [Abilitare State Configuration di Automazione di Azure](automation-dsc-onboarding.md).
- Per informazioni sulla compilazione di configurazioni DSC da assegnare ai nodi di destinazione, vedere [Compilare configurazioni DSC in State Configuration di Automazione di Azure](automation-dsc-compile.md).
- Per un esempio dell'uso di State Configuration di Automazione di Azure in una pipeline di distribuzione continua, vedere [Configurare la distribuzione continua con Chocolatey](automation-dsc-cd-chocolatey.md).
- Per informazioni sui prezzi, vedere i [prezzi di State Configuration di Automazione di Azure](https://azure.microsoft.com/pricing/details/automation/).
- Per informazioni di riferimento sui cmdlet di PowerShell, vedere [Az.Automation](/powershell/module/az.automation).
