---
title: "Soluzione VMware di Azure: l'invio di DNS dal cloud privato al sito locale"
description: Viene descritto come abilitare il server DNS del cloud privato di CloudSimple per l'invio della ricerca di risorse locali
author: Ajayan1008
ms.author: v-hborys
ms.date: 02/29/2020
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3481e21a93fbf5f658c5ca00513ea179c8f95400
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/05/2021
ms.locfileid: "97899202"
---
# <a name="enable-cloudsimple-private-cloud-dns-servers-to-forward-dns-lookup-of-on-premises-resources-to-your-dns-servers"></a>Abilitare i server DNS del cloud privato CloudSimple per l'invio della ricerca DNS delle risorse locali ai server DNS

I server DNS del cloud privato possono inviare la ricerca DNS per tutte le risorse locali ai server DNS.  L'abilitazione della ricerca consente ai componenti del cloud privato di individuare tutti i servizi in esecuzione nell'ambiente locale e di comunicare con essi usando nomi di dominio completi (FQDN).

## <a name="scenarios"></a>Scenari 

L'invio della ricerca DNS per il server DNS locale consente di usare il cloud privato per gli scenari seguenti:

* Usare il cloud privato come configurazione per il ripristino di emergenza per la soluzione VMware locale
* Usare Active Directory locali come origine di identità per il cloud privato vSphere
* Usare HCX per la migrazione di macchine virtuali da locale a cloud privato

## <a name="before-you-begin"></a>Prima di iniziare

Una connessione di rete deve essere presente dalla rete cloud privata alla rete locale per il funzionamento dell'invio DNS.  Per configurare la connessione di rete, è possibile usare:

* [Connettersi da locale a CloudSimple usando ExpressRoute](on-premises-connection.md)
* [Configurare un gateway VPN da sito a sito](./vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)

Le porte del firewall devono essere aperte in questa connessione per il funzionamento dell'invio DNS.  Le porte utilizzate sono la porta TCP 53 o la porta UDP 53.

> [!NOTE]
> Se si usa una VPN da sito a sito, è necessario aggiungere la subnet del server DNS locale come parte dei prefissi locali.

## <a name="request-dns-forwarding-from-private-cloud-to-on-premises"></a>Richiedere l'invio DNS dal cloud privato al sito locale

Per abilitare l'inoltro DNS dal cloud privato in locale, inviare una richiesta di [supporto](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest), fornendo le informazioni seguenti.

* Tipo di problema: **tecnico**
* Sottoscrizione: **sottoscrizione in cui è distribuito il servizio CloudSimple**
* Servizio: **soluzione VMware di CloudSimple**
* Tipo di problema: **Advisory o ricerca per categorie...**
* Sottotipo di problema: è **necessaria assistenza con NW**
* Specificare il nome di dominio del dominio locale nel riquadro dei dettagli.
* Fornire l'elenco dei server DNS locali a cui verrà trasmessa la ricerca dal cloud privato nel riquadro dei dettagli.

## <a name="next-steps"></a>Passaggi successivi

* [Altre informazioni sulla configurazione del firewall locale](on-premises-firewall-configuration.md)
* [Configurazione del server DNS locale](on-premises-dns-setup.md)
