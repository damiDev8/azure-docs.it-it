---
title: Eseguire il provisioning del dispositivo tramite l'attestazione della chiave simmetrica-Azure IoT Edge
description: Usare l'attestazione della chiave simmetrica per testare il provisioning automatico dei dispositivi per Azure IoT Edge con il servizio Device provisioning
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: mrohera
ms.date: 4/3/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: bfb61a5434089fffab9d8ceb9c7b0fbca528cac5
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99430612"
---
# <a name="create-and-provision-an-iot-edge-device-using-symmetric-key-attestation"></a>Creare ed effettuare il provisioning di un dispositivo IoT Edge usando l'attestazione della chiave simmetrica

È possibile eseguire il provisioning automatico dei dispositivi Azure IoT Edge usando il [servizio Device provisioning](../iot-dps/index.yml) proprio come i dispositivi non abilitati per Edge. Se non si ha familiarità con il processo di provisioning automatico, vedere la panoramica sul [provisioning](../iot-dps/about-iot-dps.md#provisioning-process) prima di continuare.

Questo articolo illustra come creare un servizio Device provisioning singola registrazione con l'attestazione della chiave simmetrica in un dispositivo IoT Edge con i passaggi seguenti:

* Creare un'istanza del servizio Device Provisioning in hub IoT.
* Creare una singola registrazione per il dispositivo.
* Installare il runtime di IoT Edge e connettersi all'hub Internet.

L'attestazione con chiave simmetrica costituisce un approccio semplice per autenticare un dispositivo con un'istanza del servizio Device Provisioning. Questo metodo di attestazione rappresenta un'esperienza di "Hello World" per gli sviluppatori che non hanno familiarità con il provisioning dei dispositivi o che non possiedono requisiti di sicurezza restrittivi. L'attestazione del dispositivo che usa un [TPM](../iot-dps/concepts-tpm-attestation.md) o [certificati X. 509](../iot-dps/concepts-x509-attestation.md) è più sicura e deve essere usata per requisiti di sicurezza più rigorosi.

## <a name="prerequisites"></a>Prerequisiti

* Un hub tutto attivo
* Un dispositivo fisico o virtuale

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>Configurare il servizio Device Provisioning in hub IoT di Azure

Creare una nuova istanza del servizio Device Provisioning in hub IoT in Azure e collegarla all'hub IoT. È possibile seguire le istruzioni riportate in [Configurare il servizio Device Provisioning in hub IoT](../iot-dps/quick-setup-auto-provision.md).

Con il servizio Device Provisioning in esecuzione, copiare il valore di **Ambito ID** dalla pagina di panoramica. Questo valore viene usato quando si configura il runtime IoT Edge.

## <a name="choose-a-unique-registration-id-for-the-device"></a>Scegliere un ID di registrazione univoco per il dispositivo

È necessario definire un ID di registrazione univoco per identificare ogni dispositivo. È possibile usare l'indirizzo MAC, il numero di serie o informazioni univoche dal dispositivo.

In questo esempio viene usata una combinazione di un indirizzo MAC e di un numero di serie che costituisce la stringa seguente per un ID registrazione: `sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6` .

Creare un ID di registrazione univoco per il dispositivo. I caratteri validi sono i caratteri alfanumerici minuscoli e il trattino ("-").

## <a name="create-a-dps-enrollment"></a>Creare una registrazione nel servizio Device Provisioning

Usare l'ID di registrazione del dispositivo per creare una registrazione singola in DPS.

Quando si crea una registrazione nel servizio Device Provisioning, si ha la possibilità di dichiarare un valore di **Stato dispositivo gemello iniziale**. Nel dispositivo gemello è possibile impostare tag per raggruppare i dispositivi in base a una qualsiasi metrica necessaria nella propria soluzione, come l'area, l'ambiente, la località o il tipo di dispositivo. Questi tag vengono usati per creare [distribuzioni automatiche](how-to-deploy-at-scale.md).

> [!TIP]
> Le registrazioni di gruppo sono possibili anche quando si usa l'attestazione della chiave simmetrica e si prevedono le stesse decisioni delle singole registrazioni.

1. Nella [portale di Azure](https://portal.azure.com)passare all'istanza del servizio Device provisioning in hub Internet.

1. In **le impostazioni** selezionare **Gestisci registrazioni**.

1. Selezionare **Aggiungi registrazione singola**, quindi completare la procedura seguente per configurare la registrazione:  

   1. Per **meccanismo** selezionare **chiave simmetrica**.

   1. Selezionare la casella **di controllo genera chiavi automaticamente** .

   1. Fornire l' **ID di registrazione** creato per il dispositivo.

   1. Se lo si desidera, specificare un **ID dispositivo dell'hub** Internet per il dispositivo. È possibile usare gli ID dispositivo per identificare come destinazione un singolo dispositivo per la distribuzione di moduli. Se non si specifica un ID dispositivo, viene usato l'ID di registrazione.

   1. Selezionare **true** per dichiarare che la registrazione è per un dispositivo IOT Edge. Per la registrazione di un gruppo, tutti i dispositivi devono essere IoT Edge dispositivi o nessuno di essi può essere.

   > [!TIP]
   > Nell'interfaccia della riga di comando di Azure è possibile creare un gruppo di registrazione o di [registrazione](/cli/azure/ext/azure-iot/iot/dps/enrollment-group) e usare il flag di **Abilitazione Edge** per specificare che un [dispositivo o un](/cli/azure/ext/azure-iot/iot/dps/enrollment) gruppo di dispositivi è un dispositivo IOT Edge.

   1. Accettare il valore predefinito dal criterio di allocazione del servizio Device provisioning per la **modalità di assegnazione dei dispositivi agli hub** o scegliere un valore diverso specifico per questa registrazione.

   1. Scegliere l'**hub IoT** collegato a cui si vuole connettere il dispositivo. È possibile scegliere più hub e il dispositivo verrà assegnato a uno di essi in base ai criteri di allocazione selezionati.

   1. Scegliere il modo in cui **si vogliono gestire i dati del dispositivo durante il nuovo provisioning** quando i dispositivi richiedono il provisioning dopo la prima volta.

   1. Aggiungere un valore di tag allo **stato dispositivo gemello iniziale**, se si desidera. È possibile usare tag per identificare come destinazione gruppi di dispositivi per la distribuzione di moduli. Ad esempio:

      ```json
      {
         "tags": {
            "environment": "test"
         },
         "properties": {
            "desired": {}
         }
      }
      ```

   1. Assicurarsi che **Abilita voce** sia impostato su **Abilita**.

   1. Selezionare **Salva**.

Ora che è presente una registrazione per questo dispositivo, il runtime di IoT Edge può effettuare automaticamente il provisioning del dispositivo durante l'installazione. Assicurarsi di copiare il valore della **chiave primaria** di registrazione da usare quando si installa il runtime di IOT Edge o se si intende creare chiavi di dispositivo da usare con la registrazione di un gruppo.

## <a name="derive-a-device-key"></a>Derivare una chiave di dispositivo

> [!NOTE]
> Questa sezione è obbligatoria solo se si usa una registrazione di gruppo.

Ogni dispositivo usa la chiave del dispositivo derivata con l'ID di registrazione univoco per eseguire l'attestazione della chiave simmetrica con la registrazione durante il provisioning. Per generare la chiave del dispositivo, usare la chiave copiata dalla registrazione di DPS per calcolare un [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) dell'ID registrazione univoco per il dispositivo e convertire il risultato in formato Base64.

Non includere la chiave primaria o secondaria della registrazione nel codice del dispositivo.

### <a name="linux-workstations"></a>Workstation di Linux

Se si usa una workstation di Linux, è possibile usare openssl per generare la chiave di dispositivo derivata come illustrato nell'esempio seguente.

Sostituire il valore della **CHIAVE** con la **chiave primaria** annotata in precedenza.

Sostituire il valore di **REG_ID** con l'ID registrazione del dispositivo.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```

### <a name="windows-based-workstations"></a>Workstation basate su Windows

Se si usan una workstation basata su Windows, è possibile usare PowerShell per generare le chiavi di dispositivo derivate come illustrato nell'esempio seguente.

Sostituire il valore della **CHIAVE** con la **chiave primaria** annotata in precedenza.

Sostituire il valore di **REG_ID** con l'ID registrazione del dispositivo.

```powershell
$KEY='8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw=='
$REG_ID='sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($KEY)
$sig = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID))
$derivedkey = [Convert]::ToBase64String($sig)
echo "`n$derivedkey`n"
```

```powershell
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```

## <a name="install-the-iot-edge-runtime"></a>Installare il runtime IoT Edge.

Il runtime di IoT Edge viene distribuito in tutti i dispositivi IoT Edge. I relativi componenti vengono eseguiti in contenitori e consentono di distribuire altri contenitori al dispositivo in modo che sia possibile eseguire codice nella rete perimetrale.

Seguire i passaggi in [installare il runtime di Azure IOT Edge](how-to-install-iot-edge.md), quindi tornare a questo articolo per effettuare il provisioning del dispositivo.

## <a name="configure-the-device-with-provisioning-information"></a>Configurare il dispositivo con le informazioni di provisioning

Una volta installato il runtime nel dispositivo, configurare il dispositivo con le informazioni usate per connettersi al servizio Device provisioning e all'hub Internet.

Sono disponibili le informazioni seguenti:

* Valore dell' **ambito dell'ID** DPS
* ID di **registrazione** del dispositivo creato
* **Chiave primaria** copiata dalla registrazione DPS

> [!TIP]
> Per le registrazioni di gruppo, è necessaria la [chiave derivata](#derive-a-device-key) di ogni dispositivo invece della chiave di registrazione DPS.

### <a name="linux-device"></a>Dispositivo Linux

1. Aprire il file di configurazione nel dispositivo IoT Edge.

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

1. Trovare la sezione configurazioni di provisioning del file. Rimuovere il commento dalle righe per il provisioning delle chiavi simmetriche DPS e assicurarsi che tutte le altre righe di provisioning siano impostate come commento.

   La `provisioning:` riga non deve contenere spazi vuoti precedenti ed è necessario rientrare gli elementi annidati di due spazi.

   ```yml
   # DPS TPM provisioning configuration
   provisioning:
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "<SCOPE_ID>"
     attestation:
       method: "symmetric_key"
       registration_id: "<REGISTRATION_ID>"
       symmetric_key: "<SYMMETRIC_KEY>"
   #  always_reprovision_on_startup: true
   #  dynamic_reprovisioning: false
   ```

   Facoltativamente, usare le `always_reprovision_on_startup` `dynamic_reprovisioning` linee o per configurare il comportamento del nuovo provisioning del dispositivo. Se un dispositivo è impostato per eseguire un nuovo provisioning all'avvio, tenterà sempre di eseguire il provisioning con DPS prima e quindi di eseguire il fallback al backup del provisioning in caso di errore. Se un dispositivo è impostato in modo da eseguire un nuovo provisioning dinamico, IoT Edge verrà riavviato ed eseguito un nuovo provisioning se viene rilevato un evento di reprovisioning. Per altre informazioni, vedere [concetti relativi al nuovo provisioning dei dispositivi dell'hub](../iot-dps/concepts-device-reprovision.md).

1. Aggiornare i valori di `scope_id` , `registration_id` e `symmetric_key` con le informazioni sul dispositivo e sul DPS.

1. Riavviare il runtime IoT Edge in modo che accetti tutte le modifiche alla configurazione apportate nel dispositivo.

   ```bash
   sudo systemctl restart iotedge
   ```

### <a name="windows-device"></a>Dispositivo Windows

1. Aprire una finestra di PowerShell in modalità amministratore. Assicurarsi di usare una sessione AMD64 di PowerShell durante l'installazione di IoT Edge, non di PowerShell (x86).

1. Il comando **Initialize-IoTEdge** configura il runtime IoT Edge nel computer. Per impostazione predefinita, il comando esegue il provisioning manuale con i contenitori di Windows, quindi usare il `-DpsSymmetricKey` flag per usare il provisioning automatico con l'autenticazione con chiave simmetrica.

   Sostituire i valori segnaposto per `{scope_id}` , `{registration_id}` e `{symmetric_key}` con i dati raccolti in precedenza.

   Aggiungere il `-ContainerOs Linux` parametro se si usano contenitori Linux in Windows.

   ```powershell
   . {Invoke-WebRequest -useb https://aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -DpsSymmetricKey -ScopeId {scope ID} -RegistrationId {registration ID} -SymmetricKey {symmetric key}
   ```

## <a name="verify-successful-installation"></a>Verificare l'esito positivo dell'installazione

Se il runtime è stato avviato correttamente, è possibile passare all'hub IoT e iniziare la distribuzione di moduli IoT Edge nel dispositivo. Usare i comandi seguenti sul dispositivo per verificare che il runtime sia stato installato e avviato correttamente.

### <a name="linux-device"></a>Dispositivo Linux

Verificare lo stato del servizio IoT Edge.

```cmd/sh
systemctl status iotedge
```

Esaminare i log del servizio.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Elencare i moduli in esecuzione.

```cmd/sh
iotedge list
```

### <a name="windows-device"></a>Dispositivo Windows

Verificare lo stato del servizio IoT Edge.

```powershell
Get-Service iotedge
```

Esaminare i log del servizio.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Elencare i moduli in esecuzione.

```powershell
iotedge list
```

È possibile verificare che sia stata usata la registrazione singola creata nel servizio Device provisioning. Passare all'istanza del servizio Device provisioning nel portale di Azure. Aprire i dettagli di registrazione per la registrazione singola creata. Si noti che lo stato della registrazione viene **assegnato** e viene elencato l'ID del dispositivo.

## <a name="next-steps"></a>Passaggi successivi

Il processo di registrazione del servizio Device Provisioning consente di impostare l'ID dispositivo e i tag del dispositivo gemello mentre si effettua il provisioning del nuovo dispositivo. È possibile usare questi valori per identificare come destinazione singoli dispositivi o gruppi di dispositivi usando la gestione automatica dei dispositivi. Informazioni su come [distribuire e monitorare IOT Edge moduli su larga scala usando l'portale di Azure o l'interfaccia della](how-to-deploy-at-scale.md) riga di comando di [Azure](how-to-deploy-cli-at-scale.md).