---
ms.openlocfilehash: 94111ae3b02da897b9544adaad384fd281a91b02
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99531293"
---
1. Clonare il repository da questo percorso: https://github.com/Azure-Samples/live-video-analytics-iot-edge-python.
1. In Visual Studio Code aprire la cartella in cui è stato scaricato il repository.
1. In Visual Studio Code passare alla cartella *src/cloud-to-device-console-app*. In questa cartella creare un file e assegnargli il nome *appsettings.json*. Questo file conterrà le impostazioni necessarie per eseguire il programma.
1. Copiare il contenuto dal file *~/clouddrive/lva-sample/appsettings.json* generato in precedenza in questo argomento di avvio rapido.

    Il testo sarà simile all'output seguente.

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. Passare alla cartella *src/edge* e creare un file denominato *.env*.
1. Copiare il contenuto del file */clouddrive/lva-sample/edge-deployment/.env*. Il testo sarà simile al codice seguente.

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    VIDEO_INPUT_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    VIDEO_OUTPUT_FOLDER_ON_DEVICE="/var/media"
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry password>"      
    ```
    