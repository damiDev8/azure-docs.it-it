---
title: Programma di installazione di driver GPU NVIDIA serie N di Azure per Windows
description: Informazioni su come configurare i driver GPU NVIDIA per macchine virtuali serie N che eseguono Windows Server o Windows in Azure
author: vikancha-MSFT
manager: jkabat
ms.service: virtual-machines-windows
ms.topic: how-to
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: vikancha
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 38d9727cadd925b944809956eaee51103499a2df
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2021
ms.locfileid: "98200907"
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-windows"></a>Installare i driver GPU NVIDIA sulle macchine virtuali serie N eseguite in Windows 

Per usufruire delle funzionalità GPU delle macchine virtuali serie N di Azure che si servono di GPU NVIDIA, è necessario installare i driver GPU NVIDIA. L'[estensione del driver NVIDIA GPU](../extensions/hpccompute-gpu-windows.md) consente di installare i driver NVIDIA CUDA o GRID appropriati in una macchina virtuale serie N. Installare o gestire l'estensione usando il portale di Azure o strumenti come i modelli di Azure PowerShell Azure o Azure Resource Manager. Vedere la [documentazione dell'estensione dei driver GPU NVIDIA](../extensions/hpccompute-gpu-windows.md) per informazioni sui sistemi operativi supportati e sui passaggi di distribuzione.

Se si sceglie di installare manualmente i driver GPU NVIDIA, questo articolo fornisce i sistemi operativi supportati, i driver e i passaggi di installazione e verifica. Le informazioni di configurazione manuale dei driver sono disponibili anche per le [VM Linux](../linux/n-series-driver-setup.md).

Per conoscere le specifiche base, le capacità di archiviazione e i dettagli relativi ai dischi, vedere [Dimensioni delle macchine virtuali Windows GPU](../sizes-gpu.md?toc=/azure/virtual-machines/windows/toc.json). 

[!INCLUDE [virtual-machines-n-series-windows-support](../../../includes/virtual-machines-n-series-windows-support.md)]

## <a name="driver-installation"></a>Installazione del driver

1. Connettersi tramite Desktop remoto a ciascuna VM serie N.

2. Scaricare, estrarre e installare il driver supportato per il sistema operativo Windows.

Dopo l'installazione del driver GRID su una macchina virtuale, è necessario eseguire il riavvio. Dopo l'installazione del driver CUDA, non è necessario eseguire il riavvio.

## <a name="verify-driver-installation"></a>Verificare l'installazione del driver

Si noti che il pannello di controllo NVIDIA è accessibile solo con l'installazione del Driver GRID. Se sono stati installati driver CUDA, il pannello di controllo NVIDIA non sarà visibile.

È possibile verificare l'installazione del driver in Gestione dispositivi. L'esempio seguente illustra la corretta configurazione della scheda Tesla K80 in una VM NC Azure.

![Proprietà del driver GPU](./media/n-series-driver-setup/GPU_driver_properties.png)

Per eseguire una query sullo stato del dispositivo GPU, eseguire l'utilità della riga di comando [smi nvidia](https://developer.nvidia.com/nvidia-system-management-interface) installata con il driver.

1. Aprire un prompt dei comandi e passare alla directory **C:\Programmi\NVIDIA Corporation\NVSMI**.

2. Eseguire `nvidia-smi`. Se il driver è installato, l'output sarà simile al seguente. **GPU-Util** mostra **0%** a meno che nella macchina virtuale non sia attualmente in esecuzione un carico di lavoro di GPU. La versione del driver e i dettagli GPU possono essere diversi da quelli riportati.

![Stato del dispositivo NVIDIA](./media/n-series-driver-setup/smi.png)  

## <a name="rdma-network-connectivity"></a>Connettività di rete RDMA

La connettività di rete RDMA può essere abilitata nelle macchine virtuali serie N con supporto per RDMA, come le macchine virtuali NC24r distribuite nello stesso set di disponibilità o in un unico gruppo di selezione in un set di scalabilità di macchine virtuali. È necessario aggiungere l'estensione HpcVmDrivers per installare i driver dei dispositivi di rete Windows che consentono la connettività RDMA. Per aggiungere l'estensione di VM a una macchina virtuale serie N abilitata per RDMA, usare i cmdlet di [Azure PowerShell](/powershell/azure/) per Azure Resource Manager.

Per installare l'ultima versione 1.1 dell'estensione HpcVMDrivers in una VM esistente con supporto per RDMA denominata myVM negli Stati Uniti occidentali:
  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  Per altre informazioni, vedere [Estensioni e funzionalità della macchina virtuale per Windows](../extensions/features-windows.md).

La rete RDMA supporta il traffico Message Passing Interface (MPI) per le applicazioni in esecuzione con [Microsoft MPI](/message-passing-interface/microsoft-mpi) o Intel MPI 5. x. 


## <a name="next-steps"></a>Passaggi successivi

* Gli sviluppatori che creano applicazioni con accelerazione GPU per GPU NVIDIA Tesla possono anche scaricare e installare il [CUDA Toolkit](https://developer.nvidia.com/cuda-downloads) più recente. Per altre informazioni, vedere la [guida di installazione di CUDA](https://docs.nvidia.com/cuda/cuda-installation-guide-microsoft-windows/index.html#axzz4ZcwJvqYi).
