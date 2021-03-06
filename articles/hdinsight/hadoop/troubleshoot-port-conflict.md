---
title: Conflitto tra porte durante l'avvio dei servizi in Azure HDInsight
description: Procedure di risoluzione dei problemi e possibili soluzioni per i problemi relativi ai conflitti di porta durante l'interazione con i cluster HDInsight di Azure.
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: f42e84d5d9c1dd49d9bf5604fe2f967eae0b6276
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/28/2021
ms.locfileid: "98943104"
---
# <a name="scenario-port-conflict-when-starting-services-in-azure-hdinsight"></a>Scenario: conflitto tra porte durante l'avvio dei servizi in Azure HDInsight

Questo articolo descrive le procedure di risoluzione dei problemi e le possibili soluzioni per i problemi durante l'interazione con i cluster HDInsight di Azure.

## <a name="issue"></a>Problema

Non è possibile avviare un servizio.

## <a name="cause"></a>Causa

È presente un conflitto di porte.

## <a name="resolution"></a>Soluzione

### <a name="method-1"></a>Metodo 1

Usare i comandi seguenti per ottenere/terminare tutti i processi in esecuzione, che sono interessati dal problema di porta.

```bash
netstat -lntp | grep <port>
ps -ef | grep <service>
kill -9 <service>
```

Avviare quindi il servizio.

### <a name="method-2"></a>Metodo 2

Riavviare il nodo.

## <a name="next-steps"></a>Passaggi successivi

Se il problema riscontrato non è presente in questo elenco o se non si riesce a risolverlo, visitare uno dei canali seguenti per ottenere ulteriore assistenza:

* Ricevere risposte dagli esperti di Azure tramite la pagina [Supporto della community per Azure](https://azure.microsoft.com/support/community/).

* Contattare [@AzureSupport](https://twitter.com/azuresupport), l'account ufficiale Microsoft Azure per migliorare l'esperienza del cliente. Mette in contatto la community di Azure con le risorse giuste: risposte, supporto ed esperti.

* Se serve ulteriore assistenza, è possibile inviare una richiesta di supporto dal [portale di Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selezionare **Supporto** nella barra dei menu o aprire l'hub **Guida e supporto**. Per informazioni più dettagliate, vedere [Come creare una richiesta di supporto in Azure](../../azure-portal/supportability/how-to-create-azure-support-request.md). L'accesso al supporto per la gestione delle sottoscrizioni e la fatturazione è incluso nella sottoscrizione di Microsoft Azure e il supporto tecnico viene fornito tramite uno dei [piani di supporto di Azure](https://azure.microsoft.com/support/plans/).