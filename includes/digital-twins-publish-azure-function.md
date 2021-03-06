---
author: baanders
description: file di inclusione per il processo di pubblicazione di una funzione di Azure da Visual Studio
ms.service: digital-twins
ms.topic: include
ms.date: 1/21/2021
ms.author: baanders
ms.openlocfilehash: 63b393f519ad29baa05fef046ee1e8ba9e5330d8
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98701161"
---
Per pubblicare il progetto in un'app per le funzioni in Azure, fare clic con il pulsante destro del mouse sul progetto in *Esplora soluzioni* e scegliere **pubblica**.

> [!IMPORTANT] 
> La pubblicazione in un'app per le funzioni in Azure comporta costi aggiuntivi per la sottoscrizione, indipendentemente dai dispositivi gemelli digitali di Azure.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-1.png" alt-text="Screenshot di Visual Studio che mostra il menu della soluzione di selezione a destra. La pubblicazione viene evidenziata nel menu.":::

Nella pagina *Pubblica* che segue lasciare la selezione di destinazione predefinita, **Azure**, e premere *Avanti*. 

Per una destinazione specifica, scegliere **App per le funzioni di Azure (Windows)** e premere *Avanti*.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-2.png" alt-text="Screenshot di Visual Studio nella finestra di dialogo pubblica funzione di Azure. Azure app per le funzioni (Windows) è selezionato nella pagina destinazione specifica.":::

Nella pagina *Istanza di Funzioni* scegliere la propria sottoscrizione. Si dovrebbe popolare una finestra con i *gruppi di risorse* della sottoscrizione.

Selezionare il gruppo di risorse dell'istanza e quindi premere *+* per creare una nuova funzione di Azure.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-3.png" alt-text="Screenshot di Visual Studio nella finestra di dialogo pubblica funzione di Azure. Il pulsante + per creare una nuova funzione viene evidenziato nella pagina dell'istanza di funzioni.":::

Nella finestra *App per le funzioni (Windows) - Crea nuova* compilare i campi come segue:
* **Nome** è il nome del piano a consumo che verrà usato da Azure per ospitare l'app di Funzioni di Azure. Diventerà anche il nome dell'app per le funzioni che contiene la funzione effettiva. È possibile scegliere un valore univoco o lasciare il suggerimento predefinito.
* Assicurarsi che **Sottoscrizione** corrisponda alla sottoscrizione da usare 
* Assicurarsi che **Gruppo di risorse** corrisponda al gruppo di risorse da usare
* Lasciare **Consumo** per *Tipo di piano*
* In **Località** selezionare la località che corrisponde al gruppo di risorse
* Creare una nuova risorsa **Archiviazione di Azure** usando il collegamento *Nuovo*. Impostare la località che corrisponde al gruppo di risorse, usare gli altri valori predefiniti e premere "OK".

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-4.png" alt-text="Screenshot di Visual Studio nella finestra di dialogo pubblica funzione di Azure. Vengono compilati i dettagli di una nuova app per le funzioni, inclusi nome, sottoscrizione, gruppo di risorse, tipo di piano, posizione e archiviazione di Azure.":::

Scegliere quindi **Create** (Crea).

Verrà visualizzata di nuovo la pagina *Istanza di Funzioni*, in cui è ora visibile la nuova app per le funzioni sotto il gruppo di risorse. Premere *Fine*.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-5.png" alt-text="Pubblicare la funzione di Azure in Visual Studio: Istanza di Funzioni (dopo l'app per le funzioni)":::

Nel riquadro *Pubblica* visualizzato di nuovo nella finestra principale di Visual Studio verificare che tutte le informazioni siano corrette e selezionare **Pubblica**.

:::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-6.png" alt-text="Screenshot di Visual Studio nella finestra di dialogo pubblica funzione di Azure. La nuova app per le funzioni viene visualizzata nell'elenco delle app per le funzioni ed è disponibile un pulsante fine.":::

> [!NOTE]
> Se viene visualizzato un popup simile al seguente: :::image type="content" source="../articles/digital-twins/media/tutorial-end-to-end/publish-azure-function-7.png" alt-text="screenshot della finestra popup di Visual Studio denominata Publish Credentials. Contiene i campi per un nome utente e una password e un pulsante per tentare di recuperare le credenziali da Azure." border="false":::
> Selezionare **Tentativo di recuperare le credenziali da Azure** e quindi **Salva**.
>
> Se viene visualizzato un avviso per *aggiornare la versione di Funzioni in Azure* o che indica che *la versione del runtime di Funzioni non corrisponde alla versione in esecuzione in Azure*:
>
> seguire le istruzioni per eseguire l'aggiornamento alla versione più recente del runtime di Funzioni di Azure. Questo problema può verificarsi se si usa una versione precedente di Visual Studio.

Per consentire all'app per le funzioni di accedere ai dispositivi gemelli digitali di Azure, è necessario disporre di un'identità gestita dal sistema e avere le autorizzazioni per accedere all'istanza di Azure Digital gemelli. Che verrà impostato successivamente.
