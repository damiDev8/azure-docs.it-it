---
ms.openlocfilehash: 69fd32c1569dbd1f08815be156585b0cce9f10d4
ms.sourcegitcommit: 31cfd3782a448068c0ff1105abe06035ee7b672a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/10/2021
ms.locfileid: "98061067"
---
Quando si configurano le risorse di Azure, nella VM Linux di Azure usata come dispositivo IoT Edge viene copiato un breve video del traffico in autostrada. In questa guida il file video viene usato per simulare uno streaming live.

Aprire un'applicazione, ad esempio il [lettore multimediale VLC](https://www.videolan.org/vlc/). Selezionare `Ctrl+N` e quindi incollare un collegamento al [video di esempio sull'intersezione dell'autostrada](https://lvamedia.blob.core.windows.net/public/camera-300s.mkv) per avviare la riproduzione. Viene visualizzato il filmato di molti veicoli in transito nel traffico di un'autostrada.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4LTY4]

In questo argomento di avvio rapido si userà Analisi video live in IoT Edge per rilevare oggetti come veicoli e persone. Gli eventi di inferenza associati verranno pubblicati nell'hub di IoT Edge.
