---
title: 'Esercitazione: Pubblicare un sito Jekyll in App Web statiche di Azure'
description: Informazioni su come distribuire un'applicazione Jekyll in App Web statiche di Azure.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 06/08/2020
ms.author: cshoe
ms.openlocfilehash: 8c6764ad5b63aa2fde07326ab986404ea4312316
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2021
ms.locfileid: "99585178"
---
# <a name="tutorial-publish-a-jekyll-site-to-azure-static-web-apps-preview"></a>Esercitazione: Pubblicare un sito Jekyll in App Web statiche di Azure (anteprima)

Questo articolo illustra come creare e distribuire un'applicazione Web [Jekyll](https://jekyllrb.com/) in [App Web statiche di Azure](overview.md).

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
>
> - Creare un sito Web Jekyll
> - Configurare una risorsa di App Web statiche di Azure
> - Distribuire l'app Jekyll in Azure

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerequisiti

- Installare [Jekyll](https://jekyllrb.com/docs/installation/)
  - Se necessario, è possibile usare il sottosistema Windows per Linux e seguire le istruzioni di Ubuntu.
- Un account Azure con una sottoscrizione attiva. Se non si ha un account, è possibile [crearne uno gratuito](https://azure.microsoft.com/free/).
- Un account GitHub. Se non si ha un account, è possibile [crearne uno gratuito](https://github.com/join).

## <a name="create-jekyll-app"></a>Crea l'app Jekyll

Creare un'app Jekyll usando l'interfaccia della riga di comando di Jekyll:

1. Dal terminale eseguire l'interfaccia della riga di comando di Jekyll per creare una nuova app.

   ```bash
   jekyll new static-app
   ```

1. Passare all'app appena creata.

   ```bash
   cd static-app
   ```

1. Inizializzare un nuovo repository Git.

   ```bash
    git init
   ```

1. Eseguire il commit delle modifiche.

   ```bash
   git add -A
   git commit -m "initial commit"
   ```

## <a name="push-your-application-to-github"></a>Eseguire il push dell'applicazione in GitHub

App Web statiche di Azure usa GitHub per pubblicare il sito Web. I passaggi seguenti illustrano come creare un repository GitHub.

1. Creare da [https://github.com/new](https://github.com/new) un repository GitHub vuoto denominato **jekyll-azure-static** (non creare un file LEGGIMI).

1. Aggiungere il repository GitHub come repository remoto al repository locale. Assicurarsi di sostituire il segnaposto `<YOUR_USER_NAME>` nel comando seguente con il proprio nome utente di GitHub.

   ```bash
   git remote add origin https://github.com/<YOUR_USER_NAME>/jekyll-static-app
   ```

1. Eseguire il push del repository locale in GitHub.

   ```bash
   git push --set-upstream origin main
   ```

   > [!NOTE]
   > Il ramo Git può essere denominato in modo diverso rispetto a `main` . Sostituire `main` con il valore corretto in questo comando.

## <a name="deploy-your-web-app"></a>Distribuire l'app Web

I passaggi seguenti illustrano come creare una nuova app del sito statica e distribuirla in un ambiente di produzione.

### <a name="create-the-application"></a>Creazione dell'applicazione

1. Passare al [portale di Azure](https://portal.azure.com).

1. Fare clic su **Crea una risorsa**.

1. Cercare **App Web statiche**.

1. Fare clic su **App Web statiche (anteprima)** .

1. Fare clic su **Crea**.

1. Per **Sottoscrizione**, accettare la sottoscrizione elencata o sceglierne una nuova nell'elenco a discesa.

1. Per _Gruppo di risorse_ selezionare **Nuovo**. In _Nome nuovo gruppo di risorse_ immettere **jekyll-static-app** e scegliere **OK**.

1. Immettere quindi un nome per l'app nella casella _Nome_. I caratteri validi includono `a-z`, `A-Z`, `0-9` e `-`.

1. In _Area_ selezionare un'area disponibile nelle vicinanze.

1. In _SKU_ selezionare **Gratuito**.

    :::image type="content" source="./media/publish-jekyll/basic-app-details.png" alt-text="Dettagli inseriti":::

1. Fare clic sul pulsante **Accedi con GitHub**.

1. Selezionare l'**organizzazione** in cui è stato creato il repository.

1. Selezionare **jekyll-static-app** come valore di _Repository_.

1. Per il _ramo_ selezionare **principale**.

    :::image type="content" source="./media/publish-jekyll/completed-github-info.png" alt-text="Informazioni su GitHub inserite":::

### <a name="build"></a>Compilare

Successivamente, verranno aggiunte le impostazioni di configurazione usate dal processo di compilazione per compilare l'app. Le impostazioni seguenti consentono di configurare il file del flusso di lavoro di GitHub Actions.

1. Fare clic sul pulsante **Avanti: Compilazione >** per modificare la configurazione della build.

1. Impostare _Percorso app_ su **/_site**.

1. Lasciare vuota la casella _Percorso artefatto app_.

   Non è necessario specificare alcun valore per _Percorso API_ perché al momento non viene distribuita alcuna API.

### <a name="review-and-create"></a>Rivedi e crea

1. Fare clic sul pulsante **Rivedi e crea** per verificare che tutti i dettagli siano corretti.

1. Fare clic su **Crea** per avviare la creazione della risorsa di App Web statiche di Azure ed effettuare il provisioning di un'azione di GitHub Actions per la distribuzione.

1. La distribuzione avrà esito negativo perché il file del flusso di lavoro necessita di alcune impostazioni specifiche di Jekyll. Per aggiungere queste impostazioni, passare al terminale ed eseguire il pull del commit nel computer con l'azione di GitHub Actions.

   ```bash
   git pull
   ```

1. Aprire l'app Jekyll in un editor di testo e aprire il file _.github/workflows/azure-pages-<NOME_FLUSSO_DI_LAVORO>.yml_.

1. Prima della riga `- name: Build And Deploy` aggiungere il blocco di configurazione seguente.

    ```yml
    - name: Set up Ruby
      uses: ruby/setup-ruby@v1.59.1
      with:
        ruby-version: 2.6
    - name: Install dependencies
      run: bundle install
    - name: Jekyll build
      run: jekyll build
    ```

1. Eseguire il commit del flusso di lavoro aggiornato ed effettuare il push in GitHub.

    ```bash
    git add -A
    git commit -m "Updating GitHub Actions workflow"
    git push
    ```

1. Attendere il completamento dell'azione di GitHub Actions.

1. Nella finestra _Panoramica_ del portale di Azure fare clic sul collegamento _URL_ per aprire l'applicazione distribuita.

   :::image type="content" source="./media/publish-jekyll/deployed-app.png" alt-text="Applicazione distribuita":::

## <a name="clean-up-resources"></a>Pulire le risorse

[!INCLUDE [cleanup-resource](../../includes/static-web-apps-cleanup-resource.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Aggiungere un dominio personalizzato](custom-domain.md)
