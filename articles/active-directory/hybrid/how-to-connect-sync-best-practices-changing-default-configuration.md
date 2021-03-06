---
title: 'Servizio di sincronizzazione Azure AD Connect: modifica della configurazione predefinita | Documentazione Microsoft'
description: Fornisce le procedure consigliate per modificare la configurazione predefinita del servizio di sincronizzazione Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 7638a031-1635-4942-94c3-fce8f09eed5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/29/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70e91ff8fa3666a2dfc5aaad07be7927852b08bd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "85357699"
---
# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Servizio di sincronizzazione Azure AD Connect: procedure consigliate per modificare la configurazione predefinita
Questo argomento descrive le modifiche supportate e non supportate del servizio di sincronizzazione Azure AD Connect.

La configurazione creata da Azure AD Connect funziona "così com'è" per la maggior parte degli ambienti che sincronizzano l'istanza di Active Directory locale con Azure AD. In alcuni casi, tuttavia, è necessario applicare alcune modifiche a una configurazione per soddisfare un'esigenza o un requisito specifico.

## <a name="changes-to-the-service-account"></a>Modifiche apportate all'account del servizio
Il servizio di sincronizzazione Azure AD Connect viene eseguito tramite un account del servizio creato dall'installazione guidata. Questo account del servizio include le chiavi di crittografia per il database utilizzato dalla sincronizzazione. Viene creato con una password di 127 caratteri e la password è impostata in modo da non scadere.

* La modifica o la reimpostazione della password dell'account del servizio **non è supportata** . Questa operazione comporta l'eliminazione delle chiavi di crittografia impedendo così l'avvio del servizio e il relativo accesso al database.

## <a name="changes-to-the-scheduler"></a>Modifiche apportate all'utilità di pianificazione
A partire dalle versioni rilasciate con la build 1.1 (febbraio 2016), è possibile configurare l' [utilità di pianificazione](how-to-connect-sync-feature-scheduler.md) per ottenere un ciclo di sincronizzazione diverso dai 30 minuti predefiniti.

## <a name="changes-to-synchronization-rules"></a>Modifiche apportate alle regole di sincronizzazione
L'installazione guidata fornisce una configurazione valida per gli scenari più comuni. Nel caso in cui sia necessario apportare modifiche alla configurazione, seguire queste regole per disporre ancora di una configurazione supportata.

> [!WARNING]
> Se si apportano modifiche alle regole di sincronizzazione predefinite, tali modifiche saranno sovrascritte al successivo aggiornamento di Azure AD Connect, con la possibilità di ottenere risultati di sincronizzazione inattesi e indesiderati.

* È possibile [modificare i flussi degli attributi](how-to-connect-sync-change-the-configuration.md#other-common-attribute-flow-changes) se quelli diretti e predefiniti non sono adatti alla propria organizzazione.
* Per [non trasmettere un attributo](how-to-connect-sync-change-the-configuration.md#do-not-flow-an-attribute) e rimuovere i valori degli attributi esistenti in Azure AD, è necessario creare una regola per questo scenario.
* [Disabilitare una regola di sincronizzazione indesiderata](#disable-an-unwanted-sync-rule) invece di eliminarla. Una regola eliminata viene ricreata durante un aggiornamento.
* Per [modificare una regola predefinita](#change-an-out-of-box-rule), creare una copia della regola originale e disabilitare quella predefinita. L'editor delle regole di sincronizzazione visualizza istruzioni e fornisce informazioni.
* Esportare le regole di sincronizzazione personalizzate usando l'Editor regole di sincronizzazione. L'editor fornisce uno script di PowerShell che è possibile usare per ricrearle facilmente in uno scenario di ripristino di emergenza.

> [!WARNING]
> Le regole di sincronizzazione predefinite hanno un'identificazione personale associata. Se si apporta una modifica a queste regole, l'identificazione personale non sarà più corrispondente. È possibile che si verifichino problemi quando in futuro si proverà ad applicare una nuova versione di Azure AD Connect. Apportare modifiche solo nel modo descritto in questo articolo.

### <a name="disable-an-unwanted-sync-rule"></a>Disabilitare una regola di sincronizzazione indesiderata
Non eliminare una regola di sincronizzazione predefinita. Verrà ricreata durante l'aggiornamento successivo.

In alcuni casi l'installazione guidata ha generato una configurazione che non funziona per la topologia dell'utente. Ad esempio, se è disponibile una topologia di foresta di account o di risorse, ma lo schema nella foresta di account è stato esteso con lo schema di Exchange, per le foresta di account e per quella di risorse vengono quindi create regole per Exchange. In questo caso è necessario disabilitare la regola di sincronizzazione per Exchange.

![Regola di sincronizzazione disabilitata](./media/how-to-connect-sync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Nella figura precedente l'installazione guidata ha rilevato un vecchio schema di Exchange 2003 nella foresta di account. Questa estensione dello schema è stata aggiunta prima dell'introduzione della foresta di risorse nell'ambiente di Fabrikam. Per assicurarsi che non vengano sincronizzati gli attributi dall'implementazione di Exchange precedente, la regola di sincronizzazione deve essere disabilitata come illustrato.

### <a name="change-an-out-of-box-rule"></a>modificare una regola predefinita
È necessario modificare una regola predefinita solo se è necessario modificare la regola join. Se si vuole modificare un flusso di attributi, creare una regola di sincronizzazione con una precedenza superiore rispetto alle regole predefinite. L'unica regola che è in pratica necessario clonare è la regola **In from AD – User Join**. È possibile eseguire l'override di tutte le altre regole con una regola di precedenza superiore.

Se è necessario apportare modifiche a una regola predefinita, effettuarne una copia e disabilitare la regola originale. Quindi apportare le modifiche alla regola clonata. L'editor delle regole di sincronizzazione facilita l'esecuzione di questa procedura. Quando si apre una regola predefinita, viene visualizzata questa finestra di dialogo:   
![Avviso regola predefinita](./media/how-to-connect-sync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Selezionare **Sì** per creare una copia della regola. Viene quindi aperta la regola clonata.  
![Regola clonata](./media/how-to-connect-sync-best-practices-changing-default-configuration/clonedrule.png)

In questa regola clonata, apportare le modifiche necessarie all'ambito, al join e alle trasformazioni.

## <a name="next-steps"></a>Passaggi successivi
**Argomenti generali**

* [Servizio di sincronizzazione Azure AD Connect: comprendere e personalizzare la sincronizzazione](how-to-connect-sync-whatis.md)
* [Integrazione delle identità locali con Azure Active Directory](whatis-hybrid-identity.md)
