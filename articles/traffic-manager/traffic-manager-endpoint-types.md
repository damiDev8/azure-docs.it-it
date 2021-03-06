---
title: Tipi di endpoint di Gestione traffico | Documentazione Microsoft
description: Questo articolo illustra tipi diversi di endpoint che è possibile usare con Gestione traffico di Azure
services: traffic-manager
documentationcenter: ''
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/21/2021
ms.author: duau
ms.openlocfilehash: 7686f2f97da0113704216dcab741c063a80d3136
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99051228"
---
# <a name="traffic-manager-endpoints"></a>Endpoint di Gestione traffico

Gestione traffico di Microsoft Azure consente di controllare la distribuzione del traffico di rete a distribuzioni di applicazioni in esecuzione in diversi data center. In Gestione traffico ogni distribuzione di applicazioni viene configurata come "endpoint". Quando Gestione traffico riceve una richiesta DNS, sceglie un endpoint disponibile da restituire nella risposta DNS. Gestione traffico basa la scelta sullo stato dell'endpoint corrente e sul metodo di routing del traffico. Per altre informazioni, vedere funzionamento di [Gestione traffico](traffic-manager-how-it-works.md).

Gli endpoint supportati da Gestione traffico sono di tre tipi:

* **Endpoint di Azure**, usati per i servizi ospitati in Azure.
* Gli **endpoint esterni** vengono usati per indirizzi IPv4/IPv6, FQDN o per i servizi ospitati all'esterno di Azure. Questi servizi possono essere locali o con un provider di hosting diverso.
* **Endpoint annidati**, usati per combinare i profili di Gestione traffico e creare schemi di routing del traffico più flessibili, per supportare le esigenze di distribuzioni più grandi e complesse.

Non esiste alcuna restrizione sulla modalità di combinazione degli endpoint di tipi diversi in un singolo profilo di gestione traffico. Ogni profilo può contenere qualsiasi combinazione di tipi di endpoint.

Le sezioni seguenti descrivono i singoli tipi di endpoint in modo più approfondito.

## <a name="azure-endpoints"></a>Endpoint di Azure

In Gestione traffico gli endpoint di Azure vengono usati per i servizi basati su Azure. Sono supportati i tipi di risorse di Azure seguenti:

* Servizi cloud PaaS
* App Web
* Slot per App Web
* Risorse PublicIPAddress, che possono essere collegate alle macchine virtuali direttamente o tramite Azure Load Balancer. È necessario che al valore publicIpAddress sia assegnato un nome DNS, da usare in un profilo di Gestione traffico.

Le risorse PublicIPAddress sono risorse di Azure Resource Manager. Non esistono nel modello di distribuzione classica e sono supportati solo nelle esperienze di Azure Resource Manager di gestione traffico. Gli altri tipi di endpoint sono supportati mediante Resource Manager e il modello di distribuzione classica.

Quando si usano gli endpoint di Azure, gestione traffico rileva quando un'app Web viene arrestata e avviata. Questo stato si riflette nello stato dell'endpoint. Per altri dettagli, vedere [Monitoraggio e failover degli endpoint di Gestione traffico](traffic-manager-monitoring.md#endpoint-and-profile-status). Quando il servizio sottostante viene arrestato, gestione traffico non esegue controlli di integrità degli endpoint o indirizza il traffico all'endpoint. Per l'istanza arrestata non si verifica alcun evento di fatturazione di Gestione traffico. Quando il servizio viene riavviato, la fatturazione riprende e l'endpoint è di nuovo idoneo a ricevere il traffico. Questo rilevamento non si applica agli endpoint PublicIpAddress.

## <a name="external-endpoints"></a>Endpoint esterni

Gli endpoint esterni vengono usati per indirizzi IPv4/IPv6, FQDN o per servizi all'esterno di Azure. L'uso di endpoint per gli indirizzi IPv4/IPv6 consente a Gestione traffico di controllare l'integrità degli endpoint senza che sia necessario specificare un nome DNS. Di conseguenza, Gestione traffico può rispondere alle query con i record A/AAAA quando viene restituito l'endpoint in una risposta. I servizi esterni ad Azure possono includere un servizio ospitato in locale o da un provider diverso. Gli endpoint esterni possono essere usati singolarmente o in combinazione con endpoint di Azure nello stesso profilo di Gestione traffico. L'eccezione riguarda gli endpoint specificati come indirizzi IPv4 o IPv6, che possono essere solo endpoint esterni. La combinazione di endpoint di Azure con endpoint esterni consente un'ampia gamma di scenari:

* Offre maggiore ridondanza per un'applicazione locale esistente in un modello di failover attivo-passivo o attivo-attivo con l'utilizzo di Azure. 
* Instradare il traffico agli endpoint a cui non è associato un nome DNS. Consente inoltre di ridurre la latenza di ricerca DNS complessiva eliminando la necessità di eseguire una seconda query DNS per ottenere un indirizzo IP di un nome DNS restituito.
* Ridurre la latenza dell'applicazione per gli utenti in tutto il mondo, estendere un'applicazione locale esistente ad altre posizioni geografiche in Azure. Per altre informazioni, vedere [Metodo di routing del traffico Prestazioni](traffic-manager-routing-methods.md#performance).
* Offrire maggiore capacità per un'applicazione locale esistente, in modo continuo o come soluzione "da potenziare al cloud" per soddisfare un picco di domanda con Azure.

In alcuni casi, è utile usare endpoint esterni per fare riferimento ai servizi di Azure. Per esempi, vedere le [domande frequenti](traffic-manager-faqs.md#traffic-manager-endpoints) . I controlli di integrità vengono fatturati in base alla tariffa degli endpoint di Azure, non alla tariffa degli endpoint esterni. Diversamente dagli endpoint di Azure, se si arresta o si elimina il servizio sottostante, la fatturazione del controllo integrità continua. La fatturazione viene arrestata dopo la disabilitazione o l'eliminazione dell'endpoint in gestione traffico.

## <a name="nested-endpoints"></a>Endpoint annidati

Gli endpoint annidati combinano più profili di gestione traffico per creare schemi di routing del traffico flessibili per supportare le esigenze di distribuzioni più grandi e complesse. Quando si usano gli endpoint annidati, un profilo "figlio" viene aggiunto come endpoint a un profilo "padre". Entrambi i profili padre e figlio possono contenere altri endpoint di qualsiasi tipo, inclusi altri profili annidati. 

Per altre informazioni, vedere [nested Traffic Manager profiles](traffic-manager-nested-profiles.md)(Profili nidificati di Gestione traffico).

## <a name="web-apps-as-endpoints"></a>App Web come endpoint

Quando si configurano app Web come endpoint in gestione traffico, è necessario tenere presenti alcune considerazioni:

1. Solo le app Web a partire dallo SKU "standard" sono idonee all'uso con Gestione traffico. I tentativi di aggiungere app Web dello SKU di una versione precedente hanno esito negativo. Se si esegue il downgrade dello SKU di un'app Web esistente, Gestione traffico smette di inviare traffico a tale app Web. Per altre informazioni sui piani supportati, vedere i [piani di servizio app](https://azure.microsoft.com/pricing/details/app-service/plans/)
2. Quando un endpoint riceve una richiesta HTTP, usa l'intestazione "host" della richiesta per determinare quale app Web usare per gestirla. L'intestazione host contiene il nome DNS usato per avviare la richiesta, ad esempio ' contosoapp.azurewebsites.net '. Per usare un nome DNS diverso con l'app Web, tale nome DNS deve essere registrato come nome di dominio personalizzato per l'app. Quando si aggiunge un endpoint di app Web come endpoint di Azure, il nome DNS del profilo di Gestione traffico viene registrato automaticamente per l'app. Questa registrazione viene rimossa automaticamente quando l'endpoint viene eliminato.
3. Ogni profilo di Gestione traffico può avere al massimo un endpoint di app Web da ogni area di Azure. Per ovviare a questa limitazione, è possibile configurare un'app Web come endpoint esterno. Per altre informazioni, vedere la sezione [Domande frequenti](traffic-manager-faqs.md#traffic-manager-endpoints).

## <a name="enabling-and-disabling-endpoints"></a>Abilitazione e disabilitazione di endpoint

La disabilitazione di un endpoint in Gestione traffico risulta utile per rimuovere temporaneamente il traffico da un endpoint in modalità di manutenzione o in corso di ridistribuzione. Quando l'endpoint è di nuovo operativo, è possibile abilitarlo nuovamente.

È possibile abilitare o disabilitare gli endpoint tramite il portale di gestione traffico, PowerShell, l'interfaccia della riga di comando o l'API REST.

> [!NOTE]
> La disabilitazione di un endpoint di Azure non ha nulla a che vedere con il relativo stato di distribuzione in Azure. Un servizio di Azure, ad esempio una macchina virtuale o un'app Web, rimane operativo e in grado di ricevere il traffico anche se è disabilitato in Gestione traffico. È possibile indirizzare il traffico direttamente all'istanza del servizio, senza usare il nome DNS del profilo di Gestione traffico. Per altre informazioni, vedere [Modalità di funzionamento di Gestione traffico](traffic-manager-how-it-works.md).

L'idoneità corrente di ogni endpoint a ricevere il traffico dipende dai fattori seguenti:

* Stato del profilo (abilitato/disabilitato)
* Stato dell'endpoint (abilitato/disabilitato)
* Risultati dei controlli di integrità per l'endpoint

Per altre informazioni, vedere [Informazioni sul monitoraggio di Gestione traffico](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> Dal momento che Gestione traffico lavora a livello di DNS, non è in grado di influenzare le connessioni esistenti verso qualsiasi endpoint. Quando un endpoint non è disponibile, Gestione traffico indirizza le nuove connessioni a un altro endpoint disponibile. L'host dietro all'endpoint disabilitato o non integro, tuttavia, può continuare a ricevere il traffico tramite le connessioni esistenti fino a quando le sessioni in questione non vengono terminate. Per consentire lo smaltimento del traffico dalle connessioni esistenti, le applicazioni devono limitare la durata delle sessioni.

Se tutti gli endpoint in un profilo vengono disabilitati o se il profilo stesso viene disabilitato, Traffic Manager invia una risposta ' NXDOMAIN ' a una nuova query DNS.

## <a name="faqs"></a>Domande frequenti

* [È possibile usare Gestione traffico con endpoint di più sottoscrizioni?](./traffic-manager-faqs.md#can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions)

* [È possibile usare Gestione traffico con slot di "staging" del servizio cloud?](./traffic-manager-faqs.md#can-i-use-traffic-manager-with-cloud-service-staging-slots)

* [Gestione traffico supporta endpoint IPv6?](./traffic-manager-faqs.md#does-traffic-manager-support-ipv6-endpoints)

* [È possibile usare Gestione traffico con più app Web nella stessa area?](./traffic-manager-faqs.md#can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region)

* [Qual è la procedura per spostare gli endpoint di Azure del profilo di Gestione traffico in un gruppo di risorse diverso?](./traffic-manager-faqs.md#how-do-i-move-my-traffic-manager-profiles-azure-endpoints-to-a-different-resource-group-or-subscription)

## <a name="next-steps"></a>Passaggi successivi

* Informazioni sul [funzionamento di Gestione traffico](traffic-manager-how-it-works.md).
* Informazioni sul [monitoraggio degli endpoint e sul failover automatico](traffic-manager-monitoring.md)di Gestione traffico.
* Informazioni sui [metodi di routing del traffico](traffic-manager-routing-methods.md)di Gestione traffico.