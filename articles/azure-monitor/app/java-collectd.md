---
title: 'Monitorare le prestazioni delle app Web in Linux: Azure | Documentazione Microsoft'
description: Monitoraggio esteso delle prestazioni delle applicazioni del sito Web Java con il plug-in CollectD per Application Insights.
ms.topic: conceptual
ms.date: 03/14/2019
author: MS-jgol
ms.custom: devx-track-java
ms.author: jgol
ms.openlocfilehash: 5ec928a0dc3cbcde3c6dd50b1795a05b5e092bde
ms.sourcegitcommit: c4246c2b986c6f53b20b94d4e75ccc49ec768a9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2020
ms.locfileid: "96601272"
---
# <a name="collectd-linux-performance-metrics-in-application-insights-deprecated"></a>collectd: metriche delle prestazioni di Linux in Application Insights [deprecato]

> [!IMPORTANT]
> L' **approccio consigliato** per il monitoraggio delle applicazioni Java consiste nell'usare la strumentazione automatica senza modificare il codice. Seguire le linee guida per **[Application Insights agente Java 3,0](./java-in-process-agent.md)**.

Per esplorare le metriche delle prestazioni del sistema Linux in [Application Insights](./app-insights-overview.md), installare [collectd](https://collectd.org/) insieme al rispettivo plug-in di Application Insights. Questa soluzione open source raccoglie diverse che relative al sistema e alla rete.

In genere, si usa collectd se è già stato [instrumentato il servizio Web Java con Application Insights][java]. Fornisce una maggiore quantità di dati che consentono di migliorare le prestazioni dell'app o diagnosticare i problemi. 

## <a name="get-your-instrumentation-key"></a>Ottenere la chiave di strumentazione
Nel [Portale di Microsoft Azure](https://portal.azure.com) aprire la risorsa [Application Insights](./app-insights-overview.md) in cui devono essere visualizzati i dati. In alternativa, [creare una nuova risorsa](./create-new-resource.md).

Copiare la chiave di strumentazione, che identifica la risorsa.

![Visualizzare tutto, aprire la risorsa e quindi nell'elenco a discesa Informazioni di base selezionare e copiare la chiave di strumentazione](./media/java-collectd/instrumentation-key-001.png)

## <a name="install-collectd-and-the-plug-in"></a>Installare collectd e il plug-in
Nei computer server Linux:

1. Installare [collectd](https://collectd.org/) versione 5.4.0 o successive.
2. Scaricare il [plug-in di scrittura collectd di Application Insights](https://github.com/microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal). Annotare il numero di versione.
3. Copiare il file JAR del plug-in in `/usr/share/collectd/java`.
4. Modificare `/etc/collectd/collectd.conf`:
   * Assicurarsi che il [plug-in Java](https://collectd.org/wiki/index.php/Plugin:Java) sia abilitato.
   * Aggiornare JVMArg per java.class.path in modo da includere il file JAR seguente. Aggiornare il numero di versione in modo che corrisponda a quello scaricato:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Aggiungere questo frammento di codice usando la chiave di strumentazione dalla risorsa:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Di seguito è riportata una parte di un file di configurazione di esempio:

```XML

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"

      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
    ...
```

Configurare altri [plug-in collectd](https://collectd.org/wiki/index.php/Table_of_Plugins), che possono raccogliere diversi dati da origini diverse.

Riavviare collectd, come indicato nel rispettivo [manuale](https://collectd.org/wiki/index.php/First_steps).

## <a name="view-the-data-in-application-insights"></a>Visualizzare i dati in Application Insights
Nella risorsa Application Insights aprire [metriche e aggiungere grafici][metrics], selezionando le metriche da visualizzare nella categoria personalizzata.

Per impostazione predefinita, le metriche vengono aggregate per tutti i computer host da cui vengono raccolte le metriche. Per visualizzare le metriche dei singoli host, nel pannello di dettagli del grafico attivare l'opzione Raggruppamento e quindi scegliere di eseguire il raggruppamento in base a CollectD-Host.

## <a name="to-exclude-upload-of-specific-statistics"></a>Per escludere il caricamento di statistiche specifiche
Per impostazione predefinita, il plug-in di Application Insights invia tutti i dati raccolti da tutti i plug-in di tipo 'read' di collectd. 

Per escludere dati da plug-in specifici oppure origini dati specifiche:

* Modificare il file di configurazione. 
* In `<Plugin ApplicationInsightsWriter>`aggiungere righe di direttive analoghe alle seguenti:

| Direttiva | Effetto |
| --- | --- |
| `Exclude disk` |Esclusione di tutti i dati raccolti dal plug-in `disk` . |
| `Exclude disk:read,write` |Esclusione delle origini denominate `read` e `write` dal plug-in `disk`. |

Separare le direttive con un valore NewLine.

## <a name="problems"></a>Problemi?
*I dati non vengono visualizzati nel portale*

* Aprire [Cerca][diagnostic] per verificare se gli eventi non elaborati sono stati ricevuti. In alcuni casi necessitano di più tempo per la visualizzazione in Esplora metriche.
* Potrebbe essere necessario [impostare le eccezioni del firewall per i dati in uscita](./ip-addresses.md)
* Abilitare la traccia nel plug-in di Application Insights. Aggiungere questa riga in `<Plugin ApplicationInsightsWriter>`:
  * `SDKLogger true`
* Aprire un terminale e avviare collectd in modalità dettagliata, per visualizzare eventuali problemi segnalati:
  * `sudo collectd -f`

## <a name="known-issue"></a>Problema noto

Il plug-in di scrittura di Application Insights non è compatibile con alcuni plug-in di lettura. Alcuni plug-in a volte inviano un errore "non un numero" quando il plug-in di Application Insights prevede un numero a virgola mobile.

Sintomo: il log collectd Mostra gli errori che includono "AI:... SyntaxError: token imprevisto N ".

Soluzione alternativa: escludere i dati raccolti dal plug-in di scrittura che causa il problema. 

<!--Link references-->

[api]: ./api-custom-events-metrics.md
[apiexceptions]: ./api-custom-events-metrics.md#track-exception
[availability]: ./monitor-web-app-availability.md
[diagnostic]: ./diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../platform/metrics-charts.md

