---
title: Centro distribuzione per Azure Kubernetes
description: Il Centro distribuzione in Azure DevOps semplifica la configurazione di una pipeline Azure DevOps affidabile per l'applicazione.
ms.author: puagarw
ms.topic: tutorial
ms.date: 07/12/2019
author: pulkitaggarwl
ms.openlocfilehash: 15b0413eabcfae7e3a4b28243caf2a708260ccae
ms.sourcegitcommit: 1756a8a1485c290c46cc40bc869702b8c8454016
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2020
ms.locfileid: "96932218"
---
# <a name="deployment-center-for-azure-kubernetes"></a>Centro distribuzione per Azure Kubernetes

Il Centro distribuzione in Azure DevOps semplifica la configurazione di una pipeline Azure DevOps affidabile per l'applicazione. Per impostazione predefinita, il Centro distribuzione configura una pipeline Azure DevOps per distribuire gli aggiornamenti dell'applicazione nel cluster Kubernetes. È possibile estendere la pipeline Azure DevOps configurata predefinita e aggiungere anche funzionalità più avanzate, ovvero la possibilità di ottenere l'approvazione prima della distribuzione, eseguire il provisioning di altre risorse di Azure, eseguire script, aggiornare l'applicazione e persino eseguire altri test di convalida.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Configurare una pipeline Azure DevOps per distribuire gli aggiornamenti dell'applicazione nel cluster Kubernetes.
> * Esaminare la pipeline di integrazione continua (CI).
> * Esaminare la pipeline di recapito continuo (CD).
> * Pulire le risorse.

## <a name="prerequisites"></a>Prerequisiti

* Una sottoscrizione di Azure. È possibile ottenerne una gratuita tramite [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

* Un cluster del servizio Azure Kubernetes.

## <a name="create-an-aks-cluster"></a>Creare un cluster AKS

1. Accedere al [portale di Azure](https://portal.azure.com/).

1. Selezionare l'opzione [Cloud Shell](../cloud-shell/overview.md) sul lato destro della barra dei menu nel portale di Azure.

1. Per creare il cluster del servizio Azure Kubernetes, eseguire i comandi seguenti:

    ```azurecli
    # Create a resource group in the South India location:

    az group create --name azooaks --location southindia

    # Create a cluster named azookubectl with one node.

    az aks create --resource-group azooaks --name azookubectl --node-count 1 --enable-addons monitoring --generate-ssh-keys
    ```

## <a name="deploy-application-updates-to-a-kubernetes-cluster"></a>Distribuire gli aggiornamenti dell'applicazione in un cluster Kubernetes

1. Passare al gruppo di risorse creato nella sezione precedente.

1. Selezionare il cluster del servizio Azure Kubernetes e quindi **Centro distribuzione (anteprima)** nel pannello sinistro. Selezionare **Attività iniziali**.

   ![Screenshot che mostra il portale di Azure con una freccia che punta al Centro distribuzione.](media/deployment-center-launcher/settings.png)

1. Scegliere il percorso del codice e selezionare **Avanti**. Selezionare quindi uno dei repository attualmente supportati: **[Azure Repos](/azure/devops/repos/index)** o **GitHub**.

    Azure Repos è un set di strumenti di controllo della versione utili per gestire il codice. Indipendentemente dalle dimensioni del progetto software, è consigliabile usare il controllo della versione appena possibile.

    - **Azure Repos**: scegliere un repository dal progetto e dall'organizzazione esistenti.

        ![Azure Repos](media/deployment-center-launcher/azure-repos.gif)

    - **GitHub**: autorizzare e selezionare il repository per l'account GitHub.

        ![Animazione mostra un processo in GitHub per selezionare GitHub come origine e quindi selezionare il repository.](media/deployment-center-launcher/github.gif)


1. Il Centro distribuzione analizza il repository e rileva il Dockerfile. Se si vuole aggiornare il Dockerfile, è possibile modificare il numero di porta identificato.

    ![Impostazioni dell'applicazione](media/deployment-center-launcher/application-settings.png)

    Se il repository non contiene il Dockerfile, il sistema visualizza un messaggio per eseguire il commit di un file.

    ![Screenshot mostra il Centro distribuzione con il messaggio Non è stato possibile trovare alcun Dockerfile nel repository.](media/deployment-center-launcher/dockerfile.png)

1. Selezionare un registro contenitori esistente o crearne uno, quindi selezionare **Fine**. La pipeline viene creata automaticamente e accoda una compilazione in [Azure Pipelines](/azure/devops/pipelines/index).

    Azure Pipelines è un servizio cloud che è possibile usare per compilare e testare automaticamente il progetto di codice e renderlo disponibile ad altri utenti. Azure Pipelines combina integrazione continua e recapito continuo per testare e compilare in modo costante e coerente il codice e distribuirlo in qualsiasi destinazione.

    ![Registro Container](media/deployment-center-launcher/container-registry.png)

1. Selezionare il collegamento per visualizzare la pipeline in corso.

1. I log riusciti verranno visualizzati al termine della distribuzione.

    ![Screenshot mostra il Centro distribuzione con Release-1 contrassegnato da un'icona con segno di spunta verde.](media/deployment-center-launcher/logs.png)

## <a name="examine-the-ci-pipeline"></a>Esaminare la pipeline di integrazione continua

Il Centro distribuzione configura automaticamente la pipeline CI/CD dell'organizzazione di Azure DevOps. La pipeline può essere esplorata e personalizzata.

1. Passare al dashboard del Centro distribuzione.  

1. Selezionare il numero di build nell'elenco dei log riusciti per visualizzare la pipeline di compilazione per il progetto.

1. Selezionare i puntini di sospensione (...) nell'angolo in alto a destra. Viene visualizzato un menu con opzioni per eseguire diverse attività, ad esempio accodare una nuova compilazione, sospendere una compilazione e modificare la pipeline di compilazione. Selezionare **Modifica pipeline**. 

1. In questo riquadro è possibile esaminare le diverse attività disponibili per la pipeline di compilazione. La compilazione esegue diverse attività, ad esempio la raccolta delle origini dal repository GIT, la creazione di un'immagine, l'esecuzione del push di un'immagine nel registro contenitori e la pubblicazione degli output usati per le distribuzioni.

1. Nella parte superiore della pipeline selezionare il nome della pipeline di compilazione.

1. Modificare il nome della pipeline di compilazione scegliendo un testo più descrittivo, selezionare **Salva e accoda** e quindi **Salva**.

1. Sotto la pipeline di compilazione selezionare **Cronologia**. Questo riquadro mostra un audit trail delle modifiche recenti apportate alla compilazione. Azure DevOps monitora tutte le modifiche apportate alla pipeline di compilazione e consente di confrontare le versioni.

1. Selezionare **Trigger**. È possibile includere o escludere rami dal processo di integrazione continua.

1. Selezionare **Conservazione**. A seconda dello scenario, è possibile specificare i criteri per conservare o rimuovere un determinato numero di compilazioni.

## <a name="examine-the-cd-pipeline"></a>Esaminare la pipeline di distribuzione continua

Il Centro distribuzione crea e configura automaticamente la relazione tra l'organizzazione di Azure DevOps e la sottoscrizione di Azure. I passaggi previsti includono la configurazione di una connessione al servizio di Azure per eseguire l'autenticazione della sottoscrizione di Azure con Azure DevOps. Durante il processo automatizzato viene anche creata una pipeline di versione, che fornisce il recapito continuo ad Azure.

1. Selezionare **Pipelines** e quindi **Versioni**.

1. Per modificare la pipeline di versione, selezionare **Modifica**.

1. Nell'elenco **Artefatti** selezionare **Elimina**. Nei passaggi precedenti la pipeline di costruzione esaminata produce l'output usato per l'artefatto. 

1. A destra dell'opzione **Elimina** selezionare il trigger **Distribuzione continua**. Questa pipeline di versione ha un trigger di distribuzione continua abilitato, che esegue una distribuzione ogni volta che è disponibile un nuovo artefatto di compilazione. È anche possibile disabilitare il trigger per richiedere l'esecuzione manuale delle distribuzioni.

1. Per esaminare tutte le attività della pipeline, selezionare **Attività**. La versione imposta l'ambiente Tiller, configura il parametro `imagePullSecrets`, installa gli strumenti Helm e distribuisce i grafici Helm nel cluster Kubernetes.

1. Per visualizzare la cronologia delle versioni, selezionare **Visualizza versioni**.

1. Per visualizzare il riepilogo, selezionare **Versione**. Selezionare su una delle fasi per esplorare più menu, ad esempio un riepilogo della versione, gli elementi di lavoro associati e i test. 

1. Selezionare **Commit**. Questa visualizzazione mostra i commit del codice correlati a questa distribuzione. È possibile confrontare le versioni per visualizzare le differenze di commit tra le distribuzioni.

1. Selezionare **Log**. I log contengono informazioni utili sulla distribuzione, che è possibile visualizzare durante e dopo le distribuzioni.

## <a name="clean-up-resources"></a>Pulire le risorse

Quando non servono più, è possibile eliminare le risorse correlate create in precedenza. Usare la funzionalità di eliminazione nel dashboard di DevOps Projects.

## <a name="next-steps"></a>Passaggi successivi

È possibile modificare queste pipeline di compilazione e di versione in base alle esigenze del team. In alternativa, è possibile usare questo modello di CI/CD come modello per altre pipeline.
