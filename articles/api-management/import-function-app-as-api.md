---
title: Importare un'app per le funzioni di Azure come API in Gestione API
titleSuffix: Azure API Management
description: Questo articolo illustra come importare un'app per le funzioni di Azure in Gestione API di Azure come API.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/22/2020
ms.author: apimpm
ms.openlocfilehash: f66395b1e0f45f1e80cd0ac93bf8c9ae8674a0f2
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2021
ms.locfileid: "98732963"
---
# <a name="import-an-azure-function-app-as-an-api-in-azure-api-management"></a>Importare un'app per le funzioni di Azure come API in Gestione API di Azure

Gestione API di Azure supporta l'importazione di app per le funzioni di Azure come nuove API o aggiungendole ad API esistenti. Il processo genera automaticamente una chiave host nell'app per le funzioni di Azure, a cui viene quindi assegnato un valore denominato in Gestione API di Azure.

Questo articolo illustra l'importazione di un'app per le funzioni di Azure come API in Gestione API di Azure. Descrive inoltre il processo di test.

Si apprenderà come:

> [!div class="checklist"]
> * Importare un'app per le funzioni di Azure come API
> * Aggiungere un'app per le funzioni di Azure a un'API
> * Visualizzare la chiave host e il valore denominato di Gestione API di Azure della nuova app per le funzioni di Azure
> * Testare l'API nel portale di Azure
> * Testare l'API nel portale per sviluppatori

## <a name="prerequisites"></a>Prerequisiti

* Completare la guida di avvio rapido [Creare un'istanza di Gestione API di Azure](get-started-create-service-instance.md).
* Verificare che nella sottoscrizione sia disponibile un'app Funzioni di Azure. Per altre informazioni, vedere [come creare un'app per le funzioni di Azure](../azure-functions/functions-get-started.md). Deve contenere funzioni con trigger HTTP e livello di autorizzazione impostato su *Anonimo* o su *Funzione*.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-an-azure-function-app-as-a-new-api"></a><a name="add-new-api-from-azure-function-app"></a> Importare un'app per le funzioni di Azure come nuova API

Per creare una nuova API da un'app per le funzioni di Azure, seguire questa procedura.

1. Passare al servizio Gestione API nel portale di Azure e selezionare **API** dal menu.

2. Nell'elenco **Add a new API** (Aggiungere una nuova API) selezionare **App per le funzioni**.

    ![Screenshot che mostra il riquadro App per le funzioni.](./media/import-function-app-as-api/add-01.png)

3. Fare clic su **Sfoglia** per selezionare le funzioni da importare.

    ![Screenshot con il pulsante Salva evidenziato.](./media/import-function-app-as-api/add-02.png)

4. Fare clic sulla sezione **App per le funzioni** per scegliere dall'elenco di App per le funzioni disponibili.

    ![Screenshot con la sezione App per le funzioni evidenziata.](./media/import-function-app-as-api/add-03.png)

5. Individuare l'app per le funzioni da cui si desidera importare funzioni, selezionarla e fare clic su **Seleziona**.

    ![Screenshot con la sezione App per le funzioni evidenziata da cui importare funzioni e il pulsante Seleziona.](./media/import-function-app-as-api/add-04.png)

6. Selezionare le funzioni che si desidera importare e fare clic su **Seleziona**.

    ![Screenshot con le funzioni evidenziate da importare e il pulsante Seleziona.](./media/import-function-app-as-api/add-05.png)

    > [!NOTE]
    > È possibile importare solo funzioni basate su un trigger HTTP e il cui livello di autorizzazione è impostato su *Anonimo* o *Funzione*.

7. Passare alla visualizzazione **Completa** e assegnare **Prodotto** alla nuova API. Se necessario, specificare altri campi durante la creazione o configurarli successivamente passando alla scheda **Impostazioni**. Le impostazioni sono illustrate nell'esercitazione [Importare e pubblicare la prima API](import-and-publish.md#import-and-publish-a-backend-api).
8. Fare clic su **Crea**.

## <a name="append-azure-function-app-to-an-existing-api"></a><a name="append-azure-function-app-to-api"></a>Aggiungere l'app per le funzioni di Azure a un'API esistente

Per aggiungere l'app per le funzioni di Azure a un'API esistente, seguire questa procedura.

1. Nell'istanza del servizio **Gestione API di Azure** selezionare **API** dal menu a sinistra.

2. Scegliere un'API in cui importare un'app per le funzioni di Azure. Fare clic su **...** e selezionare **Importa** dal menu di scelta rapida.

    ![Screenshot con la voce di menu Importa evidenziata.](./media/import-function-app-as-api/append-01.png)

3. Fare clic sul riquadro **App per le funzioni**.

    ![Screenshot con il riquadro App per le funzioni evidenziato.](./media/import-function-app-as-api/append-02.png)

4. Nella finestra popup fare clic su **Sfoglia**.

    ![Screenshot del pulsante Sfoglia.](./media/import-function-app-as-api/append-03.png)

5. Fare clic sulla sezione **App per le funzioni** per scegliere dall'elenco di App per le funzioni disponibili.

    ![Screenshot con l'elenco di app per le funzioni evidenziato.](./media/import-function-app-as-api/add-03.png)

6. Individuare l'app per le funzioni da cui si desidera importare funzioni, selezionarla e fare clic su **Seleziona**.

    ![Screenshot con la sezione App per le funzioni evidenziata da cui importare funzioni.](./media/import-function-app-as-api/add-04.png)

7. Selezionare le funzioni che si desidera importare e fare clic su **Seleziona**.

    ![Schermata con le funzioni da importare evidenziate.](./media/import-function-app-as-api/add-05.png)

8. Fare clic su **Importa**.

    ![Aggiungere da app per le funzioni](./media/import-function-app-as-api/append-04.png)

## <a name="authorization"></a><a name="authorization"></a> Autorizzazione

L'importazione di un'app per le funzioni di Azure genera automaticamente:

* la chiave host nell'app per le funzioni, con il nome apim-{*nome istanza del servizio Gestione API di Azure*},
* il valore denominato nell'istanza di Gestione API di Azure, con il nome {*nome istanza dell'app per le funzioni di Azure*}-key, che contiene la chiave host creata.

Per le API create dopo il 4 aprile 2019, la chiave host viene passata nelle richieste HTTP da Gestione API all'app per le funzioni in un'intestazione. Le API precedenti passano la chiave host come [parametro di query](../azure-functions/functions-bindings-http-webhook-trigger.md#api-key-authorization). Questo comportamento può essere modificato tramite la [chiamata API REST](/rest/api/apimanagement/2019-12-01/backend/update#backendcredentialscontract) `PATCH Backend` sull'entità *Back-end* associata all'app per le funzioni.

> [!WARNING]
> Se si rimuove o si modifica il valore della chiave host dell'app per le funzioni di Azure o il valore denominato di Gestione API di Azure, la comunicazione tra i servizi sarà interrotta. I valori non vengono sincronizzati automaticamente.
>
> Se è necessario ruotare la chiave host, assicurarsi che venga modificato anche il valore denominato in Gestione API di Azure.

### <a name="access-azure-function-app-host-key"></a>Accedere alla chiave host dell'app per le funzioni di Azure

1. Passare all'istanza dell'app per le funzioni di Azure.

2. Selezionare **Impostazioni dell'app per le funzioni** dalla panoramica.

    ![Screenshot con l'opzione Impostazioni dell'app per le funzioni evidenziata.](./media/import-function-app-as-api/keys-02-a.png)

3. La chiave si trova nella sezione **Chiavi host**.

    ![Screenshot con la sezione Chiavi host evidenziata.](./media/import-function-app-as-api/keys-02-b.png)

### <a name="access-the-named-value-in-azure-api-management"></a>Accedere al valore denominato in Gestione API di Azure

Passare all'istanza di Gestione API di Azure e selezionare **Valori denominati** nel menu a sinistra. Qui è archiviata la chiave dell'app per le funzioni di Azure.

![Aggiungere da app per le funzioni](./media/import-function-app-as-api/keys-01.png)

## <a name="test-the-new-api-in-the-azure-portal"></a><a name="test-in-azure-portal"></a> Testare la nuova API nel portale di Azure

È possibile chiamare le operazioni direttamente dal portale di Azure. Il portale di Azure offre un sistema pratico per visualizzare e testare le operazioni di un'API.  

1. Selezionare l'API creata nella sezione precedente.

2. Selezionare la scheda **Test**.

3. Selezionare un'operazione.

    La pagina visualizza campi per le intestazioni e campi per i parametri di query. Una delle intestazioni è **Ocp-Apim-Subscription-Key**, per la chiave di sottoscrizione del prodotto associato all'API. Se si è creata l'istanza di Gestione API, si è già un amministratore, quindi la chiave viene inserita automaticamente. 

4. Selezionare **Send** (Invia).

    Il back-end risponde con **200 OK** e alcuni dati.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Trasformare e proteggere un'API pubblicata](transform-api.md)
