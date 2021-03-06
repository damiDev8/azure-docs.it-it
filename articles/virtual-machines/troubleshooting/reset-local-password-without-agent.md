---
title: Reimpostare una password di Windows locale senza l'agente di Azure | Documentazione Microsoft
description: Come reimpostare la password di un account utente di Windows locale quando l'agente guest di Azure non è installato o funzionante in una VM
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: genli
ms.openlocfilehash: 2cc6ef9b1d9ca8336162b524356ea6e0d1bf5fd2
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2021
ms.locfileid: "98197660"
---
# <a name="reset-local-windows-password-for-azure-vm-offline"></a>Reimpostare una password di Windows locale per una VM di Azure offline
È possibile reimpostare la password di Windows locale di una VM in Azure tramite il [portale di Azure o Azure PowerShell](reset-rdp.md) a condizione che l'agente guest di Azure sia installato. Questo è il metodo principale per reimpostare una password per una VM di Azure. In mancanza di risposta da parte dell'agente guest di Azure, o in caso di errore di installazione dopo il caricamento di un'immagine personalizzata, è possibile reimpostare la password di Windows manualmente. Questo articolo illustra come reimpostare la password di un account locale collegando il disco virtuale del sistema operativo di origine a un'altra VM. I passaggi descritti in questo articolo non si applicano ai controller di dominio Windows. 

> [!WARNING]
> Questo processo viene usato solo come ultima risorsa. Provare sempre a reimpostare la password usando prima il [portale di Azure o Azure PowerShell](reset-rdp.md).

## <a name="overview-of-the-process"></a>Panoramica della procedura
Di seguito sono elencati i passaggi fondamentali della reimpostazione di una password locale per una VM Windows in Azure quando non è possibile accedere all'agente guest di Azure:

1. Arrestare la VM interessata.
1. Creare uno snapshot per il disco del sistema operativo della VM.
1. Creare una copia del disco del sistema operativo dallo snapshot.
1. Collegare e montare il disco del sistema operativo copiato in un'altra VM Windows, quindi creare alcuni file di configurazione sul disco. I file consentiranno di reimpostare la password.
1. Smontare e scollegare il disco rigido copiato del sistema operativo dalla VM per la risoluzione dei problemi.
1. Scambiare il disco del sistema operativo per la VM interessata.

## <a name="detailed-steps-for-the-vm-with-resource-manager-deployment"></a>Procedura dettagliata per le VM con la distribuzione Resource Manager

> [!NOTE]
> I passaggi non si applicano ai controller di dominio Windows. Si applicano solo a un server autonomo o a un server membro di un dominio.

Provare sempre a reimpostare la password usando il [portale di Azure o Azure PowerShell](reset-rdp.md) prima di procedere ai passaggi seguenti. Assicurarsi di disporre di un backup della VM prima di iniziare.

1. Acquisire uno snapshot per il disco del sistema operativo della VM interessata, creare un disco dallo snapshot, quindi collegare il disco a una VM per la risoluzione dei problemi. Per altre informazioni, vedere [Risolvere i problemi relativi a una macchina virtuale Windows collegando il disco del sistema operativo a una macchina virtuale di ripristino usando il portale Azure](troubleshoot-recovery-disks-portal-windows.md).
2. Connettersi alla VM per la risoluzione dei problemi usando Desktop remoto.
3. Creare `gpt.ini` in `\Windows\System32\GroupPolicy` sull'unità della VM di origine (in presenza del file gpt.ini, rinominarlo in gpt.ini.bak):
   
   > [!WARNING]
   > Assicurarsi di non creare accidentalmente i seguenti file in C:\Windows, l'unità del sistema operativo per la VM per la risoluzione dei problemi. Creare i seguenti file nell'unità del sistema operativo per la VM di origine collegata come disco dati.
   
   * Aggiungere le righe seguenti al file `gpt.ini` creato:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     :::image type="content" source="./media/reset-local-password-without-agent/create-gpt-ini.png" alt-text="Screenshot che Mostra gli aggiornamenti apportati al file di gpt.ini.":::

4. Creare `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts\`. Accertarsi che vengano visualizzate le cartelle nascoste. Se necessario, creare le cartelle `Machine` o `Scripts`. 
   
   * Aggiungere le righe seguenti al file `scripts.ini` creato:
     
     ```
     [Startup]
     0CmdLine=FixAzureVM.cmd
     0Parameters=
     ```
     
     :::image type="content" source="./media/reset-local-password-without-agent/create-scripts-ini-1.png" alt-text="Screenshot che Mostra gli aggiornamenti apportati al file di script.ini.":::

5. Creare `FixAzureVM.cmd` in `\Windows\System32\GroupPolicy\Machine\Scripts\Startup\` con i contenuti seguenti, sostituendo `<username>` e `<newpassword>` con i propri valori:
   
    ```
    net user <username> <newpassword> /add /Y
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add
    ```

    :::image type="content" source="./media/reset-local-password-without-agent/create-fixazure-cmd-1.png" alt-text="Screenshot che mostra il file FixAzureVM. cmd appena creato in cui si aggiornano il nome utente e la password.":::
   
    Quando si definisce la nuova password, è necessario soddisfare i requisiti di complessità delle password configurate per la VM.

6. Nel portale di Azure scollegare il disco dalla VM per la risoluzione dei problemi.

7. [Modificare il disco del sistema operativo per la VM interessata](troubleshoot-recovery-disks-portal-windows.md#swap-the-os-disk-for-the-vm).

8. Quando è in esecuzione, connettersi alla nuova VM tramite Desktop remoto con la nuova password specificata nello script `FixAzureVM.cmd`.

9. Dalla sessione remota per la nuova VM, rimuovere i file seguenti per pulire l'ambiente:
    
    * Da%windir%\System32\GroupPolicy\Machine\Scripts\Startup
      * rimuovere FixAzureVM.cmd
    * Da %windir%\System32\GroupPolicy\Machine\Scripts
      * rimuovere scripts.ini
    * Da %windir%\System32\GroupPolicy
      * rimuovere il file gpt.ini (se gpt.ini era presente in precedenza, ed era stato rinominato in gpt.ini.bak, modificare il nome del file con estensione bak nuovamente in gpt.ini)

## <a name="detailed-steps-for-classic-vm"></a>Procedura dettagliata per le macchine virtuali classiche

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

> [!NOTE]
> I passaggi non si applicano ai controller di dominio Windows. Si applicano solo a un server autonomo o a un server membro di un dominio.

Provare sempre a reimpostare la password usando il [portale di Azure o Azure PowerShell](/previous-versions/azure/virtual-machines/windows/classic/reset-rdp) prima di procedere ai passaggi seguenti. Assicurarsi di disporre di un backup della VM prima di iniziare. 

1. Eliminare la VM interessata nel portale di Azure. Questa operazione determina l'eliminazione solo dei metadati, il riferimento della VM in Azure. Quando la VM viene eliminata, i dischi virtuali vengono mantenuti:
   
   * Selezionare la VM nel portale di Azure, quindi fare clic su *Elimina*:
     
     :::image type="content" source="./media/reset-local-password-without-agent/delete-vm-classic.png" alt-text="Elimina macchina virtuale classica esistente":::

2. Collegare il disco del sistema operativo della VM di origine alla VM per la risoluzione dei problemi. La VM per la risoluzione dei problemi deve essere nella stessa area del disco del sistema operativo della VM di origine (ad esempio `West US`):
   
   1. Selezionare la VM per la risoluzione dei problemi nel portale di Azure. Fare clic su *Dischi* | *Collega esistente*:
     
      :::image type="content" source="./media/reset-local-password-without-agent/disks-attach-existing-classic.png" alt-text="Connetti disco esistente-classico":::
     
   2. Selezionare *File VHD*, quindi scegliere l'account di archiviazione contenente la VM di origine:
     
      :::image type="content" source="./media/reset-local-password-without-agent/disks-select-storage-account-classic.png" alt-text="Selezione dell'account di archiviazione-versione classica":::
     
   3. Selezionare la casella contrassegnata *Mostra account di archiviazione della versione classica*, quindi selezionare il contenitore di origine. Il contenitore di origine è in genere *vhd*:
     
      :::image type="content" source="./media/reset-local-password-without-agent/disks-select-container-classic.png" alt-text="Selezionare il contenitore di archiviazione-versione classica":::

      :::image type="content" source="./media/reset-local-password-without-agent/disks-select-container-vhds-classic.png" alt-text="Selezionare il contenitore di archiviazione-VHD-classico":::
     
   4. Selezionare il disco rigido virtuale del sistema operativo da collegare. Fare clic su *Seleziona* per completare il processo:
     
      :::image type="content" source="./media/reset-local-password-without-agent/disks-select-source-vhd-classic.png" alt-text="Selezionare il disco virtuale di origine-classico":::

   5. Fare clic su OK per collegare il disco

      :::image type="content" source="./media/reset-local-password-without-agent/disks-attach-okay-classic.png" alt-text="Connetti disco esistente-finestra di dialogo OK-classico":::

3. Connettersi alla VM per la risoluzione dei problemi tramite Desktop remoto e assicurarsi che il disco del sistema operativo della VM di origine sia visibile:

   1. Selezionare la VM per la risoluzione dei problemi nel portale di Azure e fare clic su *Connetti*.

   2. Aprire il file RDP scaricato. Immettere il nome utente e la password della VM per la risoluzione dei problemi.

   3. In Esplora file cercare il disco dati collegato. Se il disco rigido virtuale della VM di origine è l'unico disco dati collegato alla VM per la risoluzione dei problemi, questo deve corrispondere all'unità F:
     
      :::image type="content" source="./media/reset-local-password-without-agent/troubleshooting-vm-file-explorer-classic.png" alt-text="Visualizzare il disco dati collegato":::

4. Creare `gpt.ini` in `\Windows\System32\GroupPolicy` sull'unità della VM di origine (in presenza del file `gpt.ini`, rinominarlo in `gpt.ini.bak`):
   
   > [!WARNING]
   > Assicurarsi di non creare accidentalmente i seguenti file in `C:\Windows`, l'unità del sistema operativo per la VM per la risoluzione dei problemi. Creare i seguenti file nell'unità del sistema operativo per la VM di origine collegata come disco dati.
   
   * Aggiungere le righe seguenti al file `gpt.ini` creato:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     :::image type="content" source="./media/reset-local-password-without-agent/create-gpt-ini-classic.png" alt-text="Creazione di gpt.ini-versione classica":::

5. Creare `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts\`. Accertarsi che vengano visualizzate le cartelle nascoste. Se necessario, creare le cartelle `Machine` o `Scripts`.
   
   * Aggiungere le righe seguenti al file `scripts.ini` creato:

     ```
     [Startup]
     0CmdLine=FixAzureVM.cmd
     0Parameters=
     ```
     
     :::image type="content" source="./media/reset-local-password-without-agent/create-scripts-ini-classic-1.png" alt-text="Creazione di scripts.ini-versione classica":::

6. Creare `FixAzureVM.cmd` in `\Windows\System32\GroupPolicy\Machine\Scripts\Startup\` con i contenuti seguenti, sostituendo `<username>` e `<newpassword>` con i propri valori:
   
    ```
    net user <username> <newpassword> /add /Y
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add
    ```

    :::image type="content" source="./media/reset-local-password-without-agent/create-fixazure-cmd-1.png" alt-text="Creazione di FixAzureVM. cmd-versione classica":::
   
    Quando si definisce la nuova password, è necessario soddisfare i requisiti di complessità delle password configurate per la VM.

7. Nel portale di Azure scollegare il disco dalla VM per la risoluzione dei problemi:
   
   1. Selezionare la VM per la risoluzione dei problemi nel portale di Azure, fare clic su *Dischi*.
   
   2. Selezionare il disco dati collegato nel passaggio 2, fare clic su **Scollega**, quindi su **OK**.

     :::image type="content" source="./media/reset-local-password-without-agent/data-disks-classic.png" alt-text="Scollegare il disco-risoluzione dei problemi di VM-classico":::
     
     :::image type="content" source="./media/reset-local-password-without-agent/detach-disk-classic.png" alt-text="Scollegare il disco-risoluzione dei problemi della macchina virtuale-finestra di dialogo OK-classico":::

8. Creare una VM dal disco del sistema operativo della VM di origine:
   
     :::image type="content" source="./media/reset-local-password-without-agent/create-new-vm-from-template-classic.png" alt-text="Creare una macchina virtuale dal modello classico":::

     :::image type="content" source="./media/reset-local-password-without-agent/choose-subscription-classic.png" alt-text="Creare una macchina virtuale da un modello: scegliere sottoscrizione-classica":::

     :::image type="content" source="./media/reset-local-password-without-agent/create-vm-classic.png" alt-text="Creare una macchina virtuale da un modello: creare una VM-classica":::

## <a name="complete-the-create-virtual-machine-experience"></a>Completare l'esperienza di creazione di una macchina virtuale

1. Quando è in esecuzione, connettersi alla nuova VM tramite Desktop remoto con la nuova password specificata nello script `FixAzureVM.cmd`.

2. Dalla sessione remota per la nuova VM, rimuovere i file seguenti per pulire l'ambiente:
    
    * Da `%windir%\System32\GroupPolicy\Machine\Scripts\Startup\`
      * rimuovere `FixAzureVM.cmd`
    * Da `%windir%\System32\GroupPolicy\Machine\Scripts`
      * rimuovere `scripts.ini`
    * Da `%windir%\System32\GroupPolicy`
      * rimuovere `gpt.ini` (se `gpt.ini` esisteva già ed era stato rinominato in `gpt.ini.bak`, rinominare il file `.bak` in `gpt.ini`)

## <a name="next-steps"></a>Passaggi successivi
Se non è ancora possibile connettersi tramite Desktop remoto, vedere la [guida alla risoluzione dei problemi di RDP](troubleshoot-rdp-connection.md). La [guida alla risoluzione dei problemi di RDP](detailed-troubleshoot-rdp.md) esamina i metodi di risoluzione dei problemi anziché i passaggi specifici. È anche possibile [aprire una richiesta di supporto tecnico di Azure](https://azure.microsoft.com/support/options/) per ricevere un supporto pratico.
