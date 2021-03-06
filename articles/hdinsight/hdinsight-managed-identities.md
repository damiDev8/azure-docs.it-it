---
title: Identità gestite in Azure HDInsight
description: Viene fornita una panoramica dell'implementazione di identità gestite in Azure HDInsight.
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/15/2020
ms.openlocfilehash: f2b7f6e8421a735db131bc05605936e8cb2d87eb
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/28/2021
ms.locfileid: "98944121"
---
# <a name="managed-identities-in-azure-hdinsight"></a>Identità gestite in Azure HDInsight

Un'identità gestita è un'identità registrata in Azure Active Directory (Azure AD) le cui credenziali sono gestite da Azure. Con le identità gestite non è necessario registrare le entità servizio in Azure AD. Oppure mantenere le credenziali, ad esempio i certificati.

Le identità gestite vengono usate in Azure HDInsight per accedere ai servizi di Azure AD dominio o ai file di accesso in Azure Data Lake Storage Gen2 quando necessario.

Esistono due tipi di identità gestite: assegnato dall'utente e assegnato dal sistema. Azure HDInsight supporta solo le identità gestite assegnate dall'utente. HDInsight non supporta le identità gestite assegnate dal sistema. Un'identità gestita assegnata dall'utente viene creata come risorsa di Azure autonoma, che può quindi essere assegnata a una o più istanze del servizio di Azure. Al contrario, viene creata un'identità gestita assegnata dal sistema in Azure AD e quindi abilitata direttamente in una specifica istanza del servizio di Azure. Il ciclo di vita dell'identità gestita assegnata dal sistema viene quindi associato alla durata dell'istanza del servizio su cui è abilitata.

## <a name="hdinsight-managed-identity-implementation"></a>Implementazione dell'identità gestita di HDInsight

In Azure HDInsight, le identità gestite sono utilizzabili solo dal servizio HDInsight per i componenti interni. Attualmente non è disponibile alcun metodo supportato per generare token di accesso usando le identità gestite installate nei nodi del cluster HDInsight per l'accesso ai servizi esterni. Per alcuni servizi di Azure, come le macchine virtuali di calcolo, le identità gestite vengono implementate con un endpoint che è possibile usare per acquisire i token di accesso. Questo endpoint non è attualmente disponibile nei nodi HDInsight.

Se è necessario avviare le applicazioni per evitare di inserire segreti/password nei processi di analisi (ad esempio, i processi SCALA), è possibile distribuire i propri certificati ai nodi del cluster usando azioni script e quindi usare tale certificato per acquisire un token di accesso (ad esempio, per accedere ad Azure Vault).

## <a name="create-a-managed-identity"></a>Creare un'identità gestita

È possibile creare identità gestite con uno dei metodi seguenti:

* [Azure portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)
* [Interfaccia della riga di comando di Azure](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)

I passaggi rimanenti per la configurazione dell'identità gestita dipendono dallo scenario in cui verrà usato.

## <a name="managed-identity-scenarios-in-azure-hdinsight"></a>Scenari di identità gestite in Azure HDInsight

Le identità gestite vengono usate in HDInsight di Azure in più scenari. Per istruzioni dettagliate sull'installazione e sulla configurazione, vedere i documenti correlati:

* [Azure Data Lake Storage Gen2](hdinsight-hadoop-use-data-lake-storage-gen2-portal.md#create-a-user-assigned-managed-identity)
* [Enterprise Security Package](domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-and-authorize-a-managed-identity)
* [Crittografia dischi con chiavi gestite dal cliente](disk-encryption.md)

HDInsight rinnoverà automaticamente i certificati per le identità gestite usate per questi scenari. Tuttavia, esiste una limitazione quando più identità gestite diverse vengono usate per i cluster con esecuzione prolungata, il rinnovo del certificato potrebbe non funzionare come previsto per tutte le identità gestite. A causa di questa limitazione, se si prevede di usare cluster con esecuzione prolungata, ad esempio più di 60 giorni, si consiglia di usare la stessa identità gestita per tutti gli scenari precedenti. 

Se è già stato creato un cluster con esecuzione prolungata con più identità gestite diverse ed è in corso uno di questi problemi:
 * Nei cluster ESP, i servizi cluster iniziano a funzionare in caso di errore o di scalabilità verticale e altre operazioni non riescono con errori di autenticazione.
 * Nei cluster ESP, quando si modifica il certificato LDAPs di AAD-DS, il certificato LDAPs non viene aggiornato automaticamente e pertanto la sincronizzazione LDAP e le scale UPS iniziano a non riuscire.
 * Accesso MSI a ADLS Gen2 avvio non riuscito.
 * Non è possibile ruotare le chiavi di crittografia nello scenario CMK.

quindi, è necessario assegnare i ruoli e le autorizzazioni richiesti per gli scenari precedenti a tutte le identità gestite utilizzate nel cluster. Se, ad esempio, sono state usate identità gestite diverse per i cluster ADLS Gen2 e ESP, è necessario che siano presenti i ruoli "proprietario dati BLOB di archiviazione" e "collaboratore servizi di dominio HDInsight" per evitare l'esecuzione di questi problemi.

## <a name="faq"></a>Domande frequenti

### <a name="what-happens-if-i-delete-the-managed-identity-after-the-cluster-creation"></a>Cosa accade se si elimina l'identità gestita dopo la creazione del cluster?

Si verificano problemi nel cluster quando l'identità gestita è necessaria. Attualmente non è possibile aggiornare o modificare un'identità gestita dopo la creazione del cluster. Si consiglia pertanto di verificare che l'identità gestita non venga eliminata durante il runtime del cluster. In alternativa, è possibile ricreare il cluster e assegnare una nuova identità gestita.

## <a name="next-steps"></a>Passaggi successivi

* [Informazioni sulle identità gestite per le risorse di Azure](../active-directory/managed-identities-azure-resources/overview.md)
