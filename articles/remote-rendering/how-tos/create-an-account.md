---
title: Creare un account di Rendering remoto di Azure
description: Descrive i passaggi per creare un account per Rendering remoto di Azure
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: how-to
ms.openlocfilehash: 83bd4a7ae0082d24f7ac617719e628f4db4baeb9
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2021
ms.locfileid: "98197634"
---
# <a name="create-an-azure-remote-rendering-account"></a>Creare un account di Rendering remoto di Azure

Questo capitolo illustra i passaggi per creare un account per il servizio di **Rendering remoto di Azure**. È obbligatorio un account valido per il completamento di tutti gli avvii rapidi o le esercitazioni.

## <a name="create-an-account"></a>Creare un account

Per creare un account per il servizio di Rendering remoto di Azure, sono necessari i passaggi seguenti:

1. Passare alla [pagina Anteprima di Realtà mista](https://aka.ms/MixedRealityPrivatePreview)
1. Fare clic sul pulsante "Crea una risorsa"
1. Nel campo di ricerca ("Cerca nel marketplace") digitare "Rendering remoto" e premere "Invio".
1. Nell'elenco dei risultati fare clic sul riquadro "Rendering remoto"
1. Nella schermata successiva fare clic sul pulsante "Crea". Si apre un modulo per creare un nuovo account di Rendering remoto:
    1. Impostare "Nome risorsa" sul nome dell'account
    1. Aggiornare la "Sottoscrizione" se necessario
    1. Impostare il "Gruppo di risorse" su un gruppo di risorse a scelta
    1. Selezionare un'area dall'elenco a discesa ' località' in cui deve essere creata la risorsa. Vedere la sezione Osservazioni sulle [aree dell'account](create-an-account.md#account-regions) di seguito.
1. Dopo aver creato l'account, accedere e:
    1. nella scheda *Panoramica* annotare l'ID account
    1. nella scheda *Impostazioni > Chiavi di accesso*, annotare la "Chiave primaria" dell'account (questa è la chiave dell'account privata)

### <a name="account-regions"></a>Aree dell'account
Il percorso specificato durante la creazione dell'account di un account determina l'area a cui è assegnata la risorsa account. Questa operazione non può essere modificata dopo la creazione. Tuttavia, l'account può essere usato per connettersi a una sessione di rendering remoto in qualsiasi [area supportata](./../reference/regions.md), indipendentemente dalla posizione dell'account.

### <a name="retrieve-the-account-information"></a>Recuperare le informazioni dell'account

Per gli esempi e le esercitazioni è necessario fornire l'ID e una chiave dell'account. Ad esempio, nel file **arrconfig.json** usato per gli script di esempio di PowerShell:

```json
"accountSettings": {
    "arrAccountId": "<fill in the account ID from the Azure portal>",
    "arrAccountKey": "<fill in the account key from the Azure portal>",
    "region": "<select from available regions>"
},
```

vedere l'[elenco delle aree disponibili](../reference/regions.md) per compilare l'opzione *area*.

I valori per **`arrAccountId`** e **`arrAccountKey`** sono disponibili nel portale, come descritto nei passaggi seguenti:

* Accedere al [portale di Azure](https://www.portal.azure.com)
* Trovare l' **"Account di Rendering remoto"** , che dovrebbe trovarsi nell'elenco **"Risorse recenti"** . È anche possibile cercarlo nella barra di ricerca nella parte superiore. In tal caso, assicurarsi che la sottoscrizione che si vuole usare sia selezionata nel filtro della sottoscrizione predefinito (icona del filtro accanto alla barra di ricerca):

![Filtro della sottoscrizione](./media/azure-subscription-filter.png)

Facendo clic sull'account viene visualizzata questa schermata, che mostra immediatamente l'**ID dell'account** :

![ID dell'account di Azure](./media/azure-account-id.png)

Per la chiave, selezionare **Chiavi di accesso** nel pannello a sinistra. La pagina successiva mostra una chiave primaria e una chiave secondaria:

![Chiavi di accesso di Azure](./media/azure-account-primary-key.png)

Il valore di **`arrAccountKey`** può essere una chiave primaria o secondaria.

## <a name="link-storage-accounts"></a>Collegare gli account di archiviazione

Questo paragrafo spiega come collegare gli account di archiviazione all'account di Rendering remoto. Quando si collega un account di archiviazione, non è necessario generare un URI di firma di accesso condiviso ogni volta che si vuole interagire con i dati dell'account, ad esempio durante il caricamento di un modello. È invece possibile usare direttamente i nomi degli account di archiviazione, come descritto nella sezione [Caricare una sezione del modello](../concepts/models.md#loading-models).

Per ogni account di archiviazione che deve usare questo metodo di accesso, è necessario eseguire i passaggi descritti in questo paragrafo. Se gli account di archiviazione non sono ancora stati creati, è possibile esaminare il passaggio corrispondente in [Convertire un modello per l'avvio rapido del rendering](../quickstarts/convert-model.md#storage-account-creation).

A questo punto si presuppone che si disponga di un account di archiviazione. Passare all'account di archiviazione nel portale e passare alla scheda **Controllo di accesso (IAM)** per l'account di archiviazione:

![IAM dell'account di archiviazione](./media/azure-storage-account.png)

Assicurarsi di disporre delle autorizzazioni di proprietario per questo account di archiviazione per assicurarsi che sia possibile aggiungere assegnazioni di ruolo. Se non si dispone dell'accesso, l'opzione **Aggiungi assegnazione di ruolo** verrà disabilitata.

Fare clic sul pulsante **Aggiungi** nel riquadro "Aggiungi assegnazione ruolo" per aggiungere il ruolo.

![L'account di archiviazione IAM aggiungere l'assegnazione di ruolo](./media/azure-add-role-assignment.png)

* Assegnare il ruolo di **collaboratore dati BLOB di archiviazione** come illustrato nella schermata precedente.
* Selezionare il sistema di account per il **rendering remoto**  identità gestita assegnata dall'elenco **a discesa assegna accesso a** .
* Selezionare la sottoscrizione e l'account per il Rendering remoto negli ultimi elenchi a discesa.
* Fare clic su "Salva" per salvare le modifiche.

> [!WARNING]
> Nel caso in cui l'account di Rendering remoto non sia elencato, fare riferimento a questa [sezione di risoluzione dei problemi](../resources/troubleshoot.md#cant-link-storage-account-to-arr-account).

> [!IMPORTANT]
> Le assegnazioni di ruolo di Azure vengono memorizzate nella cache da archiviazione di Azure, pertanto potrebbe verificarsi un ritardo di un massimo di 30 minuti tra il momento in cui si concede l'accesso all'account di rendering remoto e quando può essere usato per accedere all'account di archiviazione. Per informazioni dettagliate, vedere la [documentazione sul controllo degli accessi in base al ruolo di Azure (RBAC di Azure)](../../role-based-access-control/troubleshooting.md#role-assignment-changes-are-not-being-detected) .

## <a name="next-steps"></a>Passaggi successivi

* [autenticazione](authentication.md)
* [Usare le API front-end di Azure per l'autenticazione](frontend-apis.md)
* [Script di Azure PowerShell di esempio](../samples/powershell-example-scripts.md)