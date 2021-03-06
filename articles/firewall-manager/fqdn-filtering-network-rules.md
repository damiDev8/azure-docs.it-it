---
title: Filtro di Azure Firewall Manager nelle regole di rete (anteprima)
description: Come usare il filtro FQDN nelle regole di rete
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: article
ms.date: 07/30/2020
ms.author: victorh
ms.openlocfilehash: 28cd26532ca5bdf83902854b7910f7d6c18a4eab
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "87460151"
---
# <a name="fqdn-filtering-in-network-rules-preview"></a>Filtro FQDN nelle regole di rete (anteprima)

> [!IMPORTANT]
> Il filtro FQDN nelle regole di rete è attualmente disponibile in anteprima pubblica.
> Questa versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate. Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Un nome di dominio completo (FQDN) rappresenta un nome di dominio di un host o di un indirizzo IP. È possibile usare i nomi di dominio completi nelle regole di rete in base alla risoluzione DNS nei criteri firewall e firewall di Azure. Questa funzionalità consente di filtrare il traffico in uscita con qualsiasi protocollo TCP/UDP (inclusi NTP, SSH, RDP e altro ancora). È necessario abilitare il proxy DNS per l'uso di FQDN nelle regole di rete. Per altre informazioni, vedere [impostazioni DNS di criteri firewall di Azure (anteprima)](dns-settings.md).

## <a name="how-it-works"></a>Funzionamento

Dopo aver definito il server DNS necessario all'organizzazione (DNS di Azure o il proprio DNS personalizzato), il firewall di Azure converte il nome di dominio completo in un indirizzo IP in base al server DNS selezionato. Questa traduzione si verifica sia per l'elaborazione delle regole di rete sia per l'applicazione.

Qual è la differenza tra l'utilizzo dei nomi di dominio nelle regole dell'applicazione rispetto a quella delle regole di rete? 

- Il filtro FQDN nelle regole dell'applicazione per HTTP/S e MSSQL si basa su un proxy trasparente a livello di applicazione e sull'intestazione SNI. Di conseguenza, può discernere tra due FQDN che vengono risolti nello stesso indirizzo IP. Questo non avviene con il filtro FQDN nelle regole di rete. Usare sempre le regole dell'applicazione, quando possibile.
- Nelle regole dell'applicazione è possibile usare HTTP/S e MSSQL come protocolli selezionati. In regole di rete è possibile usare qualsiasi protocollo TCP/UDP con i nomi di dominio completi di destinazione.

## <a name="next-steps"></a>Passaggi successivi

[Impostazioni DNS del firewall di Azure](dns-settings.md)
