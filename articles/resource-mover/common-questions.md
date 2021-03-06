---
title: Domande frequenti su Azure Resource Mover?
description: Risposte alle domande comuni su Azure Resource Mover
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: conceptual
ms.date: 02/04/2021
ms.author: raynew
ms.openlocfilehash: a75cd3c5dbf205f49aa606bfe96623a61bce39db
ms.sourcegitcommit: 49ea056bbb5957b5443f035d28c1d8f84f5a407b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "100007057"
---
# <a name="common-questions"></a>Domande frequenti

Questo articolo risponde a domande comuni su [Azure Resource Mover](overview.md).


## <a name="moving-across-regions"></a>Trasferimento tra aree

### <a name="can-i-move-resources-across-any-regions"></a>È possibile spostare le risorse in tutte le aree?

Attualmente, è possibile spostare le risorse da qualsiasi area pubblica di origine a qualsiasi area pubblica di destinazione, a seconda dei [tipi di risorse disponibili in tale area](https://azure.microsoft.com/global-infrastructure/services/). Il trasferimento delle risorse nelle aree di Azure per enti pubblici non è attualmente supportato.

### <a name="what-resources-can-i-move-across-regions-using-resource-mover"></a>Quali risorse è possibile spostare tra le aree usando il motore di risorse?

Tramite Spostamento risorse è attualmente possibile spostare tra aree le risorse seguenti:

- Macchine virtuali di Azure e dischi associati
- Schede di interfaccia di rete
- Set di disponibilità 
- Reti virtuali di Azure 
- Indirizzi IP pubblici
- Gruppi di sicurezza di rete:
- Servizi di bilanciamento del carico interni e pubblici 
- Database SQL di Azure e pool elastici

### <a name="can-i-move-disks-across-regions"></a>È possibile spostare I dischi tra le aree?

Non è possibile selezionare dischi come risorse per lo spostamento tra le aree. Tuttavia, i dischi vengono spostati come parte di uno spostamento della macchina virtuale.

### <a name="what-does-it-mean-to-move-a-resource-group"></a>Cosa si intende per spostare un gruppo di risorse?

Quando si seleziona una risorsa per lo spostamento, il gruppo di risorse corrispondente viene aggiunto automaticamente per lo spostamento. Questa operazione è necessaria perché la risorsa di destinazione deve essere inserita in un gruppo di risorse come se fosse nella destinazione. È possibile scegliere di personalizzare e fornire un gruppo di risorse exsiting, una volta aggiunto per lo spostamento. Si noti che lo spostamento di un gruppo di risorse **non** significa che tutte le risorse nel gruppo di risorse di origine verranno spostate.

### <a name="can-i-move-resources-across-subscriptions-when-i-move-them-across-regions"></a>È possibile spostare le risorse tra le sottoscrizioni quando si spostano in aree diverse?

È possibile modificare la sottoscrizione dopo aver spostato le risorse nell'area di destinazione. [Altre](../azure-resource-manager/management/move-resource-group-and-subscription.md) informazioni sullo stato di trasferimento delle risorse in una sottoscrizione diversa. 

### <a name="does-azure-resource-move-service-store-customer-data"></a>Il servizio di spostamento delle risorse di Azure archivia i dati dei clienti? 
No. Il servizio di spostamento delle risorse non archivia i dati dei clienti, ma archivia solo le informazioni sui metadati che facilitano il rilevamento e lo stato di avanzamento delle risorse selezionate per lo spostamento da parte del cliente.


### <a name="where-is-the-metadata-for-moving-across-regions-stored"></a>Dove vengono archiviati i metadati per lo stato di trasferimento tra aree?

Viene archiviato in un database di [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md) e nell' [archiviazione BLOB di Azure](../storage/common/storage-service-encryption.md)in una sottoscrizione Microsoft. Attualmente i metadati vengono archiviati nell'area Stati Uniti orientali 2 ed Europa settentrionale. Questa copertura verrà espansa in altre aree. Questo non impedisce di trasferire le risorse in tutte le aree pubbliche.

### <a name="is-the-collected-metadata-encrypted"></a>I metadati raccolti sono crittografati?

Sì, sia in transito che inattivi.
- Durante il transito, i metadati vengono inviati in modo sicuro al servizio Resource Mover tramite Internet tramite HTTPS.
- Nell'archiviazione i metadati sono crittografati.

### <a name="how-is-managed-identity-used-in-resource-mover"></a>In che modo viene usata l'identità gestita nel motore di risorse?

[Identità gestita](../active-directory/managed-identities-azure-resources/overview.md) (precedentemente nota come identità del servizio gestita (MSI)) fornisce servizi di Azure con un'identità gestita automaticamente in Azure ad.
- Il motore di risorse usa l'identità gestita in modo che possa accedere alle sottoscrizioni di Azure per spostare le risorse tra le aree.
- Una raccolta di spostamento richiede un'identità assegnata dal sistema, con accesso alla sottoscrizione che contiene le risorse che si stanno spostando.

- Se si spostano risorse tra aree nel portale, questo processo viene eseguito automaticamente.
- Se si spostano le risorse usando PowerShell, si eseguono i cmdlet per assegnare un'identità assegnata dal sistema alla raccolta e quindi si assegna un ruolo con le autorizzazioni appropriate per la sottoscrizione all'entità Identity. 

### <a name="what-managed-identity-permissions-does-resource-mover-need"></a>Quali autorizzazioni di identità gestite sono necessarie per il motore risorse? 

Per l'identità gestita di Spostamento risorse di Azure sono necessarie almeno le autorizzazioni seguenti: 

- Autorizzazione per la scrittura e la creazione di risorse nella sottoscrizione utente, disponibile con il ruolo *collaboratore* . 
- Autorizzazione a creare assegnazioni di ruolo. Generalmente disponibile con i ruoli *proprietario* o *amministratore accesso utenti* o con un ruolo personalizzato con l' *autorizzazione Microsoft. Authorization/Role/Write* assegnata. Questa autorizzazione non è necessaria se all'identità gestita della risorsa di condivisione dati è già stato concesso l'accesso all'archivio dati di Azure. 
 
Quando si aggiungono risorse nell'hub di Spostamento risorse nel portale, le autorizzazioni vengono gestite automaticamente, purché l'utente disponga delle autorizzazioni descritte in precedenza. Se si aggiungono risorse con PowerShell, si assegnano le autorizzazioni manualmente.

> [!IMPORTANT]
> Si consiglia vivamente di non modificare o rimuovere le assegnazioni di ruolo Identity. 

### <a name="what-should-i-do-if-i-dont-have-permissions-to-assign-role-identity"></a>Cosa fare se non si hanno le autorizzazioni per assegnare l'identità del ruolo?

**Possibile causa** | **Consiglio**
--- | ---
Quando si aggiunge una risorsa per la prima volta, non si è un *collaboratore* e un *amministratore di accesso utente* (o *proprietario*). | Utilizzare un account con autorizzazioni *collaboratore* e *amministratore accesso utenti* (o *proprietario*) per la sottoscrizione.
L'identità gestita del motore di risorse non ha il ruolo richiesto. | Aggiungere i ruoli "collaboratore" e "amministratore accesso utenti".
L'identità gestita del motore di risorse è stata reimpostata su *None*. | Riabilitare un'identità assegnata dal sistema nell' **identità** di spostamento della raccolta >. In alternativa, aggiungere di nuovo la risorsa in **Aggiungi risorse**, che esegue la stessa operazione.  
La sottoscrizione è stata spostata in un tenant diverso. | Disabilitare e quindi abilitare l'identità gestita per la raccolta di spostamento.

### <a name="how-can-i-do-multiple-moves-together"></a>Come è possibile eseguire più spostamenti contemporaneamente?

Modificare le combinazioni di origine/destinazione in base alle esigenze usando l'opzione di modifica nel portale.

### <a name="what-happens-when-i-remove-a-resource-from-a-list-of-move-resources"></a>Cosa accade quando si rimuove una risorsa da un elenco di risorse di spostamento?

È possibile rimuovere le risorse aggiunte all'elenco di spostamento. Il comportamento quando si rimuove una risorsa dall'elenco dipende dallo stato della risorsa. [Altre informazioni](remove-move-resources.md#vm-resource-state-after-removing)



## <a name="next-steps"></a>Passaggi successivi

[Leggere le informazioni](about-move-process.md) relative ai componenti di Spostamento risorse e al processo di spostamento.
