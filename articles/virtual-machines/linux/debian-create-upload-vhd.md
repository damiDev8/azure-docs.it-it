---
title: Preparare un disco rigido virtuale Debian Linux
description: Informazioni su come creare immagini VHD Debian per le distribuzioni di macchine virtuali in Azure.
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: how-to
ms.date: 11/13/2018
ms.author: guybo
ms.openlocfilehash: d822aa30926c473d08bc5d785211c82d038ef652
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2021
ms.locfileid: "98203185"
---
# <a name="prepare-a-debian-vhd-for-azure"></a>Preparare un disco rigido virtuale Debian per Azure
## <a name="prerequisites"></a>Prerequisiti
In questa sezione si presuppone che un sistema operativo Linux Debian sia già stato installato da un file .iso scaricato dal [sito Web di Debian](https://www.debian.org/distrib/) in un disco rigido virtuale. Sono disponibili vari strumenti per creare file con estensione .vhd; Hyper-V è solo un esempio. Per istruzioni sull’uso di Hyper-V, vedere [Installare il ruolo Hyper-V e configurare una macchina virtuale](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh846766(v=ws.11)).

## <a name="installation-notes"></a>Note sull'installazione
* Vedere anche [Note generali sull'installazione di Linux](create-upload-generic.md#general-linux-installation-notes) per altri suggerimenti sulla preparazione di Linux per Azure.
* Il formato VHDX più recente non è supportato in Azure. È possibile convertire il disco in formato VHD usando la console di gestione di Hyper-V o il cmdlet **Convert-VHD** .
* Durante l'installazione del sistema Linux è consigliabile usare partizioni standard anziché LVM, che è spesso la scelta predefinita per numerose installazioni. In questo modo sarà possibile evitare conflitti di nome LVM con le macchine virtuali clonate, in particolare se fosse necessario collegare un disco del sistema operativo a un'altra macchina virtuale per la risoluzione dei problemi. Se si preferisce, su dischi di dati si può usare [LVM](/previous-versions/azure/virtual-machines/linux/configure-lvm) o [RAID](/previous-versions/azure/virtual-machines/linux/configure-raid).
* Non configurare una partizione swap nel disco del sistema operativo. L'agente Linux di Azure può essere configurato in modo da creare un file swap sul disco temporaneo delle risorse. Altre informazioni sono disponibili nei passaggi seguenti.
* Le dimensioni virtuali di tutti i dischi rigidi virtuali su Azure devono essere allineate a 1 MB. Quando si converte un disco non formattato in un disco rigido virtuale, prima della conversione è necessario assicurarsi che le dimensioni del disco non formattato siano un multiplo di 1 MB. Per altre informazioni, vedere [Note sull'installazione di Linux](create-upload-generic.md#general-linux-installation-notes).

## <a name="use-azure-manage-to-create-debian-vhds"></a>Utilizzare Azure-Manage per creare dischi rigidi virtuali Debian
Sono disponibili strumenti per la generazione di dischi rigidi virtuali Debian per Azure, ad esempio gli script di [gestione di Azure](https://github.com/credativ/azure-manage) di [credativ](https://www.credativ.com/). Questo è l'approccio consigliato rispetto alla creazione di un'immagine da zero. Ad esempio, per creare un disco rigido virtuale Debian 8, eseguire i comandi seguenti per scaricare l'utilità `azure-manage` (e le dipendenze) ed eseguire lo script `azure_build_image`:

```console
# sudo apt-get update
# sudo apt-get install git qemu-utils mbr kpartx debootstrap

# sudo apt-get install python3-pip python3-dateutil python3-cryptography
# sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
# git clone https://github.com/credativ/azure-manage.git
# cd azure-manage
# sudo pip3 install .

# sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section
```


## <a name="manually-prepare-a-debian-vhd"></a>Preparare manualmente un disco rigido virtuale Debian
1. Nella console di gestione di Hyper-V selezionare la macchina virtuale.
2. Fare clic su **Connetti** per aprire una finestra della console per la macchina virtuale.
3. Se si è installato il sistema operativo mediante un'immagine ISO, impostare come commento qualsiasi riga correlata a "`deb cdrom`" in `/etc/apt/source.list`.

4. Modificare il file `/etc/default/grub` e il parametro **GRUB_CMDLINE_LINUX** come indicato di seguito per includere parametri aggiuntivi del kernel per Azure.

    ```config-grub
    GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200n8 earlyprintk=ttyS0,115200"
    ```

5. Ricompilare il grub ed eseguire:

    ```console
    # sudo update-grub
    ```

6. Aggiungere i repository di Azure di Debian a/etc/apt/sources.list per Debian 8, 9 o 10:

    **Debian 8.x "Jessie"**

    ```config-grub
    deb http://debian-archive.trafficmanager.net/debian jessie main
    deb-src http://debian-archive.trafficmanager.net/debian jessie main
    deb http://debian-archive.trafficmanager.net/debian-security jessie/updates main
    deb-src http://debian-archive.trafficmanager.net/debian-security jessie/updates
    deb http://debian-archive.trafficmanager.net/debian jessie-updates main
    deb-src http://debian-archive.trafficmanager.net/debian jessie-updates main
    deb http://debian-archive.trafficmanager.net/debian jessie-backports main
    deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
    ```

    **Debian 9.x "Stretch"**

    ```config-grub
    deb http://debian-archive.trafficmanager.net/debian stretch main
    deb-src http://debian-archive.trafficmanager.net/debian stretch main
    deb http://debian-archive.trafficmanager.net/debian-security stretch/updates main
    deb-src http://debian-archive.trafficmanager.net/debian-security stretch/updates main
    deb http://debian-archive.trafficmanager.net/debian stretch-updates main
    deb-src http://debian-archive.trafficmanager.net/debian stretch-updates main
    deb http://debian-archive.trafficmanager.net/debian stretch-backports main
    deb-src http://debian-archive.trafficmanager.net/debian stretch-backports main
    ```
    
    **Debian 10. x "Buster"**
    ```config-grub
    deb http://debian-archive.trafficmanager.net/debian buster main
    deb-src http://debian-archive.trafficmanager.net/debian buster main
    deb http://debian-archive.trafficmanager.net/debian-security buster/updates main
    deb-src http://debian-archive.trafficmanager.net/debian-security buster/updates main
    deb http://debian-archive.trafficmanager.net/debian buster-updates main
    deb-src http://debian-archive.trafficmanager.net/debian buster-updates main
    deb http://debian-archive.trafficmanager.net/debian buster-backports main
    deb-src http://debian-archive.trafficmanager.net/debian buster-backports main
    ```

7. Installare l'agente Linux di Azure:

    ```console
    # sudo apt-get update
    # sudo apt-get install waagent
    ```

8. Per Debian 9+ è consigliabile usare il nuovo kernel Debian Cloud per l'uso con le macchine virtuali in Azure. Per installare questo nuovo kernel, creare innanzitutto un file denominato /etc/apt/preferences.d/linux.pref con il seguente contenuto:

    ```config-pref
    Package: linux-* initramfs-tools
    Pin: release n=stretch-backports
    Pin-Priority: 500
    ```

    Quindi eseguire "sudo apt-get install linux-image-cloud-amd64" per installare il nuovo kernel Debian Cloud.

9. Effettuare il deprovisioning della macchina virtuale e prepararla per il provisioning in Azure ed eseguire:

    ```console
    # sudo waagent –force -deprovision
    # export HISTSIZE=0
    # logout
    ```

10. Fare clic su **azione** -> arrestare la console di gestione di Hyper-V. Il file VHD Linux è ora pronto per il caricamento in Azure.

## <a name="next-steps"></a>Passaggi successivi
È ora possibile usare il disco rigido virtuale Debian per creare nuove macchine virtuali in Azure. Se è la prima volta che si carica il file VHD in Azure, vedere [Creare una macchina virtuale Linux da un disco personalizzato](upload-vhd.md#option-1-upload-a-vhd).