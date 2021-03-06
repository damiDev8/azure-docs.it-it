---
title: Configurare protocolli API-IoT Edge di griglia di eventi di Azure | Microsoft Docs
description: Informazioni sulle possibili configurazioni di protocollo di un modulo di griglia di eventi.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: a9cf6088201ffeaed76d99a9b211e5bcd1ea139a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91322581"
---
# <a name="configure-event-grid-api-protocols"></a>Configurare i protocolli API di griglia di eventi

Questa guida fornisce esempi delle possibili configurazioni di protocollo di un modulo di griglia di eventi. Il modulo di griglia di eventi espone l'API per le operazioni di gestione e di Runtime. Nella tabella seguente vengono acquisiti i protocolli e le porte.

| Protocollo | Porta | Descrizione |
| ---------------- | ------------ | ------------ |
| HTTP | 5888 | Disabilitato per impostazione predefinita. Utile solo durante i test. Non adatto per carichi di lavoro di produzione.
| HTTPS | 4438 | Predefinito

Vedere la guida per la [sicurezza e l'autenticazione](security-authentication.md) per tutte le configurazioni possibili.

## <a name="expose-https-to-iot-modules-on-the-same-edge-network"></a>Esporre HTTPS ai moduli di Internet delle cose nella stessa rete perimetrale

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ]
}
 ```

## <a name="enable-https-to-other-iot-modules-and-non-iot-workloads"></a>Abilitare HTTPS ad altri moduli e carichi di lavoro non-Internet

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> La sezione **PortBindings** consente di eseguire il mapping delle porte interne alle porte dell'host contenitore. Questa funzionalità consente di raggiungere il modulo di griglia di eventi dall'esterno della rete IoT Edge contenitore, se il dispositivo Edge è raggiungibile pubblicamente.

## <a name="expose-http-and-https-to-iot-modules-on-the-same-edge-network"></a>Esporre HTTP e HTTPS ai moduli di Internet delle cose nella stessa rete perimetrale

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=enabled",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ]
}
 ```

## <a name="enable-http-and-https-to-other-iot-modules-and-non-iot-workloads"></a>Abilitare HTTP e HTTPS ad altri moduli e carichi di lavoro non-Internet

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=enabled",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ],
      "5888/tcp": [
        {
          "HostPort": "5888"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> Per impostazione predefinita, ogni modulo delle cose è parte del runtime IoT Edge creato dalla rete Bridge. Consente la comunicazione tra i diversi moduli dell'it nella stessa rete. **PortBindings** consente di eseguire il mapping di una porta interna del contenitore nel computer host in modo da consentire a chiunque di accedere alla porta del modulo di griglia di eventi dall'esterno.

>[!IMPORTANT]
> Mentre le porte possono essere rese accessibili all'esterno della rete IoT Edge, l'autenticazione client impone a chi è effettivamente consentito effettuare chiamate al modulo.
