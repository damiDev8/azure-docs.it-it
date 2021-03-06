---
title: Notebook di sinapsi Studio
description: Questo articolo illustra come creare e sviluppare notebook di Azure sinapsi Studio per la preparazione e la visualizzazione dei dati.
services: synapse analytics
author: ruixinxu
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: spark
ms.date: 10/19/2020
ms.author: ruxu
ms.reviewer: ''
ms.custom: devx-track-python
ms.openlocfilehash: 57999ce53e536d422e6502a77aaccdc66b4c5077
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/27/2021
ms.locfileid: "98898495"
---
# <a name="create-develop-and-maintain-synapse-studio-notebooks-in-azure-synapse-analytics"></a>Creare, sviluppare e gestire notebook di sinapsi studio in Azure sinapsi Analytics

Un notebook di sinapsi studio è un'interfaccia Web che consente di creare file che contengono codice in tempo reale, visualizzazioni e testo descrittivo. I notebook possono essere usati per convalidare idee ed eseguire esperimenti rapidi per ottenere informazioni cognitive dettagliate dai dati. I notebook sono anche ampiamente usati per la preparazione e la visualizzazione dei dati, l'apprendimento automatico e altri scenari di Big Data.

Con un notebook di Azure Synapse Studio è possibile:

* Iniziare senza attività di configurazione.
* Mantenere i dati protetti con le funzionalità di sicurezza aziendali predefinite.
* Analizzare i dati in formati non elaborati (CSV, txt, JSON e così via), formati di file elaborati (parquet, Delta Lake, ORC e così via) e file di dati tabulari SQL in Spark e SQL.
* Ottenere produttività con funzionalità avanzate di creazione e visualizzazione dei dati predefinite.

Questo articolo descrive come usare i notebook in Azure Synapse Studio.

## <a name="preview-of-the-new-notebook-experience"></a>Anteprima della nuova esperienza notebook
Il team sinapsi ha portato il nuovo componente notebooks in sinapsi Studio per offrire un'esperienza coerente con i notebook per i clienti Microsoft e massimizzare l'individuabilità, la produttività, la condivisione e la collaborazione. La nuova esperienza notebook è pronta per l'anteprima. Selezionare il pulsante **funzionalità di anteprima** nella barra degli strumenti del notebook per attivarlo. La tabella seguente acquisisce il confronto delle funzionalità tra un notebook esistente (denominato "notebook classico") con la nuova versione di anteprima.  

|Funzionalità|Notebook classico|Anteprima notebook|
|--|--|--|
|% esecuzione| Non supportato | &#9745;|
|% cronologia| Non supportato |&#9745;
|% carico| Non supportato |&#9745;|
|%% HTML| Non supportato |&#9745;|
|Trascinare e rilasciare per spostare una cella| Non supportato |&#9745;|
|Output di visualizzazione permanente ()|&#9745;| Non disponibile |
|Annulla tutto| &#9745;| Non disponibile|
|Esegui tutte le celle sopra|&#9745;| Non disponibile |
|Esegui tutte le celle sotto|&#9745;| Non disponibile |
|Formattare la cella di testo con i pulsanti della barra degli strumenti|&#9745;| Non disponibile |
|Annulla operazione cella| &#9745;| Non disponibile |


## <a name="create-a-notebook"></a>Creare un notebook

È possibile creare un notebook in due modi. È possibile creare un nuovo notebook o importarne uno esistente in un'area di lavoro Azure Synapse da **Esplora oggetti**. I notebook di Azure Synapse Studio sono in grado di riconoscere i file con estensione ipynb standard di Jupyter Notebook.

![Crea notebook di importazione](./media/apache-spark-development-using-notebooks/synapse-create-import-notebook-2.png)

## <a name="develop-notebooks"></a>Sviluppare i notebook

I notebook sono costituiti da celle, ovvero singoli blocchi di codice o testo che possono essere eseguiti in modo indipendente o come gruppo.

### <a name="add-a-cell"></a>Aggiungere una cella

Esistono diversi modi per aggiungere una nuova cella al notebook.

# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

1. Espandere il pulsante in alto a sinistra **+ Cella**, quindi selezionare **Aggiungi cella di codice** o **Aggiungi cella di testo**.

    ![add-cell-with-cell-button](./media/apache-spark-development-using-notebooks/synapse-add-cell-1.png)

2. Passare con il mouse sullo spazio tra due celle e selezionare **Aggiungi codice** o **Aggiungi testo**.

    ![add-cell-between-space](./media/apache-spark-development-using-notebooks/synapse-add-cell-2.png)

3. Usare la [combinazione di tasti in modalità comando](#shortcut-keys-under-command-mode). Premere **A** per inserire una cella al di sopra della cella corrente. Premere **B** per inserire una cella sotto la cella corrente.


# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

1. Espandere il pulsante in alto a sinistra **+ cella** e selezionare **cella codice** o **cella Markdown**.
    ![Add-Azure-notebook-Cell-with-Cell-Button](./media/apache-spark-development-using-notebooks/synapse-azure-notebook-add-cell-1.png)
2. Selezionare il segno più all'inizio di una cella e selezionare **cella codice** o **cella Markdown**.

    ![Aggiungi-Azure-notebook-cella-tra-spazio](./media/apache-spark-development-using-notebooks/synapse-azure-notebook-add-cell-2.png)

3. Usare i [tasti di scelta rapida aznb in modalità comando](#shortcut-keys-under-command-mode). Premere **A** per inserire una cella al di sopra della cella corrente. Premere **B** per inserire una cella sotto la cella corrente.

---

### <a name="set-a-primary-language"></a>Impostare il linguaggio primario

I notebook di Azure Synapse Studio supportano quattro linguaggi Apache Spark:

* pySpark (Python)
* Spark (Scala)
* SparkSQL
* .NET per Apache Spark (C#)

È possibile impostare il linguaggio primario per le nuove celle aggiunte dall'elenco a discesa nella barra dei comandi superiore.

   ![default-synapse-language](./media/apache-spark-development-using-notebooks/synapse-default-language.png)

### <a name="use-multiple-languages"></a>Usare più linguaggi

È possibile usare più linguaggi in un notebook specificando il comando magic corretto per il linguaggio all'inizio di una cella. Nella tabella seguente sono elencati i comandi magic per passare da un linguaggio all'altro nelle celle.

|Comando magic |Linguaggio | Descrizione |  
|---|------|-----|
|%%pyspark| Python | Eseguire una query **Python** sul contesto Spark.  |
|%%spark| Scala | Eseguire una query **Scala** sul contesto Spark.  |  
|%%sql| SparkSQL | Eseguire una query **SparkSQL** sul contesto Spark.  |
|%%csharp | .NET per Spark C# | Eseguire una query **.NET per Spark C#** sul contesto Spark. |

L'immagine seguente è un esempio di come è possibile scrivere una query PySpark usando il comando magic **%% PySpark** o una query SparkSQL con il comando magic **%% SQL** in un notebook **Spark(Scala)** . Si noti che il linguaggio primario per il notebook è impostato su pySpark.

   ![Comandi Magic di sinapsi Spark](./media/apache-spark-development-using-notebooks/synapse-spark-magics.png)

### <a name="use-temp-tables-to-reference-data-across-languages"></a>Usare tabelle temporanee per fare riferimento ai dati tra linguaggi

Non è possibile fare riferimento a dati o variabili direttamente tra linguaggi diversi in un notebook di Synapse Studio. In Spark è possibile fare riferimento a una tabella temporanea tra i vari linguaggi. Di seguito è riportato un esempio di come leggere un DataFrame `Scala` in `PySpark` e `SparkSQL` usando una tabella temporanea di Spark come soluzione alternativa.

1. Nella cella 1 leggere un dataframe da un connettore del pool SQL usando scala e creare una tabella temporanea.

   ```scala
   %%scala
   val scalaDataFrame = spark.read.sqlanalytics("mySQLPoolDatabase.dbo.mySQLPoolTable")
   scalaDataFrame.createOrReplaceTempView( "mydataframetable" )
   ```

2. Nella cella 2 eseguire una query sui dati usando Spark SQL.
   
   ```sql
   %%sql
   SELECT * FROM mydataframetable
   ```

3. Nella cella 3 usare i dati in PySpark.

   ```python
   %%pyspark
   myNewPythonDataFrame = spark.sql("SELECT * FROM mydataframetable")
   ```

### <a name="ide-style-intellisense"></a>IntelliSense di tipo IDE

I notebook di Azure Synapse Studio sono integrati nell'editor Monaco e consentono di aggiungere la funzionalità IntelliSense di tipo IDE all'editor di celle. L'evidenziazione della sintassi, il marcatore di errore e i completamenti automatici del codice consentono di scrivere codice e identificare più rapidamente i problemi.

Le funzionalità di IntelliSense hanno livelli di maturità diversi per i diversi linguaggi. Usare la tabella seguente per visualizzare gli elementi supportati.

|Languages| Evidenziazione della sintassi | Generatore di errori della sintassi  | Completamento del codice della sintassi | Completamento del codice della variabile| Completamento del codice della funzione di sistema| Completamento del codice della funzione utente| Rientro automatico | Riduzione del codice|
|--|--|--|--|--|--|--|--|--|
|PySpark (Python)|Sì|Sì|Sì|Sì|Sì|Sì|Sì|Sì|
|Spark (Scala)|Sì|Sì|Sì|Sì|-|-|-|Sì|
|SparkSQL|Sì|Sì|-|-|-|-|-|-|
|.NET per Spark (C#)|Sì|-|-|-|-|-|-|-|

### <a name="format-text-cell-with-toolbar-buttons"></a>Formattare la cella di testo con i pulsanti della barra degli strumenti

# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

È possibile usare i pulsanti di formato nella barra degli strumenti delle celle di testo per eseguire azioni di markdown comuni, tra cui l'applicazione di grassetto o corsivo al testo, l'inserimento di frammenti di codice, l'inserimento di elenchi non ordinati, l'inserimento di elenchi ordinati e l'inserimento di immagini dall'URL.

  ![Barra degli strumenti cella testo sinapsi](./media/apache-spark-development-using-notebooks/synapse-text-cell-toolbar.png)

# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

La barra degli strumenti del pulsante formato non è ancora disponibile per l'esperienza di anteprima notebook. 

---

### <a name="undo-cell-operations"></a>Annullare operazioni sulle celle

# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

Selezionare il pulsante **Annulla** oppure premere **CTRL + Z** per revocare l'operazione più recente sulle celle. A questo punto è possibile annullare le ultime 20 azioni cronologiche sulle celle. 

   ![Celle di annullamento della sinapsi](./media/apache-spark-development-using-notebooks/synapse-undo-cells.png)
# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

L'operazione di annullamento della cella non è ancora disponibile per l'esperienza di anteprima del notebook. 

---

### <a name="move-a-cell"></a>Spostare una cella

# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

Selezionare i puntini di sospensione (...) per accedere al menu aggiuntivo delle azioni sulle celle all'estrema destra. Selezionare quindi **Sposta la cella in alto** o **Sposta la cella in basso** per spostare la cella corrente. 

È anche possibile usare la [combinazione di tasti in modalità comando](#shortcut-keys-under-command-mode). Premere **CTRL + ALT + ↑** per spostare verso l'alto la cella corrente. Premere **CTRL + ALT + ↓** per spostare verso il basso la cella corrente.

   ![move-a-cell](./media/apache-spark-development-using-notebooks/synapse-move-cells.png)

# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

Fare clic sul lato sinistro di una cella e trascinarlo nella posizione desiderata. 
    ![Celle spostamento sinapsi](./media/apache-spark-development-using-notebooks/synapse-azure-notebook-drag-drop-cell.gif)

---

### <a name="delete-a-cell"></a>Eliminare una cella

# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

Per eliminare una cella, selezionare i puntini di sospensione (...) per accedere al menu aggiuntivo delle azioni sulle celle all'estrema destra e selezionare **Elimina cella**. 

È anche possibile usare la [combinazione di tasti in modalità comando](#shortcut-keys-under-command-mode). Premere **D,D** per eliminare la cella corrente.
  
   ![delete-a-cell](./media/apache-spark-development-using-notebooks/synapse-delete-cell.png)

# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

Per eliminare una cella, fare clic sul pulsante Elimina nella parte destra della cella. 

È anche possibile usare la [combinazione di tasti in modalità comando](#shortcut-keys-under-command-mode). Premere **MAIUSC + D** per eliminare la cella corrente. 

   ![Azure-notebook-Delete-a-Cell](./media/apache-spark-development-using-notebooks/synapse-azure-notebook-delete-cell.png)

---

### <a name="collapse-a-cell-input"></a>Comprimere l'input di una cella

# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

Selezionare il pulsante freccia nella parte inferiore della cella corrente per comprimerlo. Per espanderlo, selezionare il pulsante freccia mentre la cella è compressa.

   ![collapse-cell-input](./media/apache-spark-development-using-notebooks/synapse-collapse-cell-input.gif)

# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

Selezionare **più** puntini di sospensione (...) sulla barra degli strumenti della cella e **input** per comprimere l'input della cella corrente. Per espanderlo, selezionare l' **input nascosto** mentre la cella è compressa.

   ![Azure-notebook-Comprimi-cella-input](./media/apache-spark-development-using-notebooks/synapse-azure-notebook-collapse-cell-input.gif)

---

### <a name="collapse-a-cell-output"></a>Comprimere l'output di una cella

# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

Selezionare il pulsante **Comprimi output** nella parte superiore sinistra dell'output della cella corrente per comprimerlo. Per espanderlo, selezionare **Mostra output celle** mentre l'output della cella è compresso.

   ![collapse-cell-output](./media/apache-spark-development-using-notebooks/synapse-collapse-cell-output.gif)

# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

Selezionare **più** puntini di sospensione (...) sulla barra degli strumenti della cella e **output** per comprimere l'output della cella corrente. Per espanderlo, selezionare lo stesso pulsante mentre l'output della cella è nascosto.

   ![Azure-notebook-Comprimi-cella-output](./media/apache-spark-development-using-notebooks/synapse-azure-notebook-collapse-cell-output.gif)


---

## <a name="run-notebooks"></a>Eseguire i notebook

È possibile eseguire le celle di codice nel notebook singolarmente o tutte insieme. Lo stato e l'avanzamento di ogni cella sono rappresentati nel notebook.

### <a name="run-a-cell"></a>Eseguire una cella

Esistono diversi modi per eseguire il codice in una cella.

1. Passare con il puntatore del mouse sulla cella che si desidera eseguire e selezionare il pulsante **Esegui cella** oppure premere **CTRL + INVIO**.

   ![run-cell-1](./media/apache-spark-development-using-notebooks/synapse-run-cell.png)
  
2. Usare la [combinazione di tasti in modalità comando](#shortcut-keys-under-command-mode). Premere **MAIUSC + INVIO** per eseguire la cella corrente e selezionare la cella al di sotto. Premere **ALT + INVIO** per eseguire la cella corrente e inserire una nuova cella al di sotto.

---

### <a name="run-all-cells"></a>Eseguire tutte le celle
Selezionare il pulsante **Esegui tutto** per eseguire tutte le celle del notebook corrente in sequenza.

   ![run-all-cells](./media/apache-spark-development-using-notebooks/synapse-run-all.png)


# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

### <a name="run-all-cells-above-or-below"></a>Eseguire tutte le celle al di sopra o al di sotto

Selezionare i puntini di sospensione **(...)** all'estrema destra per accedere al menu aggiuntivo delle azioni sulle celle. Selezionare quindi **Esegui celle sopra** per eseguire tutte le celle sopra quella corrente in sequenza. Selezionare **Esegui celle sotto** per eseguire tutte le celle sotto quella corrente in sequenza.

   ![run-cells-above-or-below](./media/apache-spark-development-using-notebooks/synapse-run-cells-above-or-below.png)


### <a name="cancel-all-running-cells"></a>Annulla tutte le celle in esecuzione
Selezionare il pulsante **Annulla tutto** per annullare le celle o le celle in attesa nella coda. 
   ![Annulla-tutte le celle](./media/apache-spark-development-using-notebooks/synapse-cancel-all.png) 

# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

Annulla tutte le celle in esecuzione non è ancora disponibile per l'esperienza di anteprima del notebook. 

---



### <a name="reference-notebook"></a>Notebook di riferimento

# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

Non supportata.

# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

È possibile usare ```%run <notebook path>``` il comando Magic per fare riferimento a un altro notebook nel contesto del notebook corrente. Tutte le variabili definite nel notebook di riferimento sono disponibili nel notebook corrente. ```%run``` il comando Magic supporta le chiamate annidate ma non supporta le chiamate ricorsive. Se la profondità dell'istruzione è maggiore di cinque, verrà generata un'eccezione. ```%run``` il comando attualmente supporta solo per passare un percorso del notebook come parametro. 

Esempio: ``` %run /path/notebookA ```.

---


### <a name="cell-status-indicator"></a>Indicatore di stato delle celle

Sotto la cella viene visualizzato il relativo stato di esecuzione dettagliato, che indica lo stato di avanzamento corrente. Al termine dell'esecuzione della cella, viene visualizzato un riepilogo dell'esecuzione con la durata totale e l'ora di fine, informazioni che verranno mantenute per riferimento futuro.

![cell-status](./media/apache-spark-development-using-notebooks/synapse-cell-status.png)

### <a name="spark-progress-indicator"></a>Indicatore di avanzamento Spark

Il notebook di Azure Synapse Studio è basato esclusivamente su Spark. Le celle di codice vengono eseguite nel pool di Apache Spark senza server in modalità remota. Viene fornito un indicatore di stato del processo Spark con una barra di avanzamento in tempo reale che consente di comprendere lo stato di esecuzione del processo.
Il numero di attività per ogni processo o fase consente di identificare il livello parallelo del processo Spark. È anche possibile esaminare più a fondo l'interfaccia utente di Spark di un processo specifico o di una fase mediante la selezione del collegamento al nome del processo o della fase.


![spark-progress-indicator](./media/apache-spark-development-using-notebooks/synapse-spark-progress-indicator.png)

### <a name="spark-session-config"></a>Configurazione della sessione Spark

È possibile specificare la durata del timeout, il numero e le dimensioni degli executor da assegnare alla sessione Spark corrente in **Configura sessione**. Riavviare la sessione di Spark per rendere effettive le modifiche alla configurazione. Tutte le variabili del notebook memorizzate nella cache vengono cancellate.

[![gestione della sessione](./media/apache-spark-development-using-notebooks/synapse-azure-notebook-spark-session-management.png)](./media/apache-spark-development-using-notebooks/synapse-azure-notebook-spark-session-management.png#lightbox)

#### <a name="spark-session-config-magic-command"></a>Comando Magic Session config Spark
È anche possibile specificare le impostazioni della sessione Spark tramite un comando Magic **%% Configure**. Per rendere effettive le impostazioni, è necessario riavviare la sessione di Spark. Si consiglia di eseguire **%% Configure** all'inizio del notebook. Di seguito è riportato un esempio, fare riferimento a https://github.com/cloudera/livy#request-body per l'elenco completo dei parametri validi 

```
%%configure -f
{
    to config the session.
    "driverMemory":"2g",
    "driverCores":3,
    "executorMemory":"2g",
    "executorCores":2,
    "jars":["myjar1.jar","myjar.jar"],
    "conf":{
        "spark.driver.maxResultSize":"10g"
    }
}
```


## <a name="bring-data-to-a-notebook"></a>Importare i dati in un notebook

È possibile caricare i dati da Archiviazione BLOB di Azure, Azure Data Lake Store Gen 2 e dal pool SQL, come illustrato negli esempi di codice riportati di seguito.

### <a name="read-a-csv-from-azure-data-lake-store-gen2-as-a-spark-dataframe"></a>Leggere un CSV da Azure Data Lake Store Gen2 come un DataFrame di Spark

```python
from pyspark.sql import SparkSession
from pyspark.sql.types import *
account_name = "Your account name"
container_name = "Your container name"
relative_path = "Your path"
adls_path = 'abfss://%s@%s.dfs.core.windows.net/%s' % (container_name, account_name, relative_path)

df1 = spark.read.option('header', 'true') \
                .option('delimiter', ',') \
                .csv(adls_path + '/Testfile.csv')

```

#### <a name="read-a-csv-from-azure-blob-storage-as-a-spark-dataframe"></a>Leggere un CSV da Archiviazione BLOB di Azure come un DataFrame di Spark

```python

from pyspark.sql import SparkSession

# Azure storage access info
blob_account_name = 'Your account name' # replace with your blob name
blob_container_name = 'Your container name' # replace with your container name
blob_relative_path = 'Your path' # replace with your relative folder path
linked_service_name = 'Your linked service name' # replace with your linked service name

blob_sas_token = mssparkutils.credentials.getConnectionStringOrCreds(linked_service_name)

# Allow SPARK to access from Blob remotely

wasb_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)

spark.conf.set('fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name), blob_sas_token)
print('Remote blob path: ' + wasb_path)

df = spark.read.option("header", "true") \
            .option("delimiter","|") \
            .schema(schema) \
            .csv(wasbs_path)
```

### <a name="read-data-from-the-primary-storage-account"></a>Leggere i dati dall'account di archiviazione primario

È possibile accedere direttamente ai dati nell'account di archiviazione primario. Non è necessario fornire le chiavi private. In Esplora dati fare clic con il pulsante destro del mouse su un file e selezionare **Nuovo notebook** per visualizzare un nuovo notebook con l'estrazione dati generata automaticamente.

![data-to-cell](./media/apache-spark-development-using-notebooks/synapse-data-to-cell.png)

## <a name="save-notebooks"></a>Salvare i notebook

È possibile salvare un singolo notebook o tutti i notebook nell'area di lavoro.

1. Per salvare le modifiche apportate a un singolo notebook, selezionare il pulsante **Pubblica** sulla barra dei comandi del notebook.

   ![publish-notebook](./media/apache-spark-development-using-notebooks/synapse-publish-notebook.png)

2. Per salvare tutti i notebook nell'area di lavoro, selezionare il pulsante **Pubblica tutti i** sulla barra dei comandi dell'area di lavoro. 

   ![publish-all](./media/apache-spark-development-using-notebooks/synapse-publish-all.png)

Nelle proprietà del notebook è possibile specificare se includere l'output della cella durante il salvataggio.

   ![notebook-properties](./media/apache-spark-development-using-notebooks/synapse-notebook-properties.png)

## <a name="magic-commands"></a>Comandi magic
Nei notebook di Azure sinapsi studio è possibile usare i comandi Magic Jupyter noti. Esaminare l'elenco seguente come i comandi Magic disponibili correnti. Inviaci [i tuoi casi d'uso su GitHub](https://github.com/MicrosoftDocs/azure-docs/issues/new) per poter continuare a sviluppare più comandi magici per soddisfare le tue esigenze.

# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

Comandi magic disponibili per le righe: [%lsmagic](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-lsmagic), [%time](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-time), [%timeit](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit)

Funzionalità magiche disponibili per le celle: [%% tempo](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-time), [%% timeit](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit), [%% Capture](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cellmagic-capture), [%% WriteFile](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cellmagic-writefile), [%% SQL](#use-multiple-languages), [%% pyspark](#use-multiple-languages), [%% Spark](#use-multiple-languages), [%% CSharp](#use-multiple-languages),[%% Configure](#spark-session-config-magic-command)



# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

Le magie della riga disponibili sono: [% lsmagic](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-lsmagic), [% Time](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-time), [% timeit](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit), [% History](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-history), [% Run](#reference-notebook), [% Load](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-load)

Funzionalità magiche disponibili per le celle: [%% tempo](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-time), [%% timeit](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit), [%% Capture](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cellmagic-capture), [%% WriteFile](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cellmagic-writefile), [%% SQL](#use-multiple-languages), [%% pyspark](#use-multiple-languages), [%% Spark](#use-multiple-languages), [%% CSharp](#use-multiple-languages), [%% HTML](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cellmagic-html), [%% Configure](#spark-session-config-magic-command)

--- 

## <a name="integrate-a-notebook"></a>Integrare un notebook

### <a name="add-a-notebook-to-a-pipeline"></a>Aggiungere un notebook a una pipeline

Selezionare il pulsante **Aggiungi a pipeline** nell'angolo superiore destro per aggiungere un notebook a una pipeline esistente o creare una nuova pipeline.

![Aggiungi notebook alla pipeline](./media/apache-spark-development-using-notebooks/add-to-pipeline.png)

### <a name="designate-a-parameters-cell"></a>Designare una cella parametri

# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

Per parametrizzare il notebook, selezionare i puntini di sospensione (...) per accedere al menu delle azioni delle celle aggiuntivo all'estrema destra. Selezionare quindi **Imposta/Rimuovi cella parametro** per indicare la cella come cella parametri.

![interruttore-parametro](./media/apache-spark-development-using-notebooks/toggle-parameter-cell.png)

# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

Per parametrizzare il notebook, selezionare i puntini di sospensione (...) per accedere a **altri comandi** sulla barra degli strumenti della cella. Selezionare quindi **Imposta/Rimuovi cella parametro** per indicare la cella come cella parametri.

![Azure-notebook-interruttore-parametro](./media/apache-spark-development-using-notebooks/azure-notebook-toggle-parameter-cell.png)

---

Azure Data Factory cerca la cella Parameters e considera questa cella come impostazioni predefinite per i parametri passati in fase di esecuzione. Il motore di esecuzione aggiungerà una nuova cella sotto la cella Parameters con i parametri di input per sovrascrivere i valori predefiniti. Quando non viene designata una cella dei parametri, la cella inserita viene inserita nella parte superiore del notebook.


### <a name="assign-parameters-values-from-a-pipeline"></a>Assegnare i valori dei parametri da una pipeline

Dopo aver creato un notebook con i parametri, è possibile eseguirlo da una pipeline con l'attività di Azure sinapsi notebook. Dopo aver aggiunto l'attività nell'area di disegno della pipeline, sarà possibile impostare i valori dei parametri nella sezione **parametri di base** della scheda **Impostazioni** . 

![Assegnare un parametro](./media/apache-spark-development-using-notebooks/assign-parameter.png)

Quando si assegnano valori di parametro, è possibile usare il [linguaggio delle espressioni della pipeline](../../data-factory/control-flow-expression-language-functions.md) o le variabili di [sistema](../../data-factory/control-flow-system-variables.md).



## <a name="shortcut-keys"></a>Combinazioni di tasti

Analogamente a Jupyter Notebook, i notebook di Azure Synapse Studio hanno un'interfaccia utente modale. La tastiera esegue diverse operazioni a seconda della modalità in cui si trova la cella del notebook. I notebook di Synapse Studio supportano le due modalità seguenti per una cella di codice specificata, ovvero la modalità di comando e la modalità di modifica.

1. Una cella è in modalità di comando quando non è presente un cursore di testo che richiede di digitare. Quando una cella è in modalità di comando, è possibile modificare il notebook nel suo complesso, ma non digitare in singole celle. Immettere la modalità comando premendo `ESC` o usando il mouse per selezionare all'esterno dell'area dell'editor di una cella.

   ![command-mode](./media/apache-spark-development-using-notebooks/synapse-command-mode-2.png)

2. La modalità di modifica è indicata da un cursore di testo che richiede di digitare nell'area dell'editor. Quando una cella è in modalità di modifica, è possibile digitare nella cella. Per passare alla modalità di modifica, premere `Enter` o usare il mouse per selezionare l'area dell'editor di una cella.
   
   ![edit-mode](./media/apache-spark-development-using-notebooks/synapse-edit-mode-2.png)

### <a name="shortcut-keys-under-command-mode"></a>Combinazione di tasti in modalità comando

# <a name="classical-notebook"></a>[Notebook classico](#tab/classical)

Usando i tasti di scelta rapida seguenti, è possibile esplorare ed eseguire più facilmente il codice nei notebook di Azure Synapse.

| Azione |Scelte rapide del notebook di Synapse Studio  |
|--|--|
|Eseguire la cella corrente e selezionare in basso | MAIUSC+INVIO |
|Eseguire la cella corrente e inserire in basso | ALT+INVIO |
|Selezionare la cella in alto| Su |
|Selezionare la cella in basso| Giù |
|Inserire la cella in alto| Una |
|Inserire la cella in basso| b |
|Estendere le celle selezionate in alto| MAIUSC+freccia SU |
|Estendere le celle selezionate in basso| Shift+Down|
|Spostare la cella in alto| CTRL + ALT + ↑ |
|Spostare la cella in basso| CTRL+Alt+↓ |
|Eliminare le celle selezionate| D, D |
|Passare alla modalità di modifica| Immettere |

# <a name="preview-notebook"></a>[Anteprima notebook](#tab/preview)

| Azione |Scelte rapide del notebook di Synapse Studio  |
|--|--|
|Eseguire la cella corrente e selezionare in basso | MAIUSC+INVIO |
|Eseguire la cella corrente e inserire in basso | ALT+INVIO |
|Esegui cella corrente| CTRL+INVIO |
|Selezionare la cella in alto| Su |
|Selezionare la cella in basso| Giù |
|Seleziona cella precedente| K |
|Seleziona cella successiva| J |
|Inserire la cella in alto| Una |
|Inserire la cella in basso| b |
|Eliminare le celle selezionate| MAIUSC + D |
|Passare alla modalità di modifica| Immettere |

---

### <a name="shortcut-keys-under-edit-mode"></a>Tasti di scelta rapida in modalità di modifica


Usando i tasti di scelta rapida seguenti, è possibile esplorare ed eseguire più facilmente il codice nei notebook di Azure Synapse in modalità di modifica.

| Azione |Scelte rapide del notebook di Synapse Studio  |
|--|--|
|Spostare il cursore in alto | Su |
|Spostare in cursore in basso|Giù|
|Annullamento|CTRL + Z|
|Ripristinare|CTRL + Y|
|Inserimento/Rimozione di commenti|CTRL + /|
|Eliminare la parola prima|CTRL + BACKSPACE|
|Eliminare la parola dopo|CTRL + CANC|
|Andare all'inizio della cella|CTRL + Home|
|Andare alla fine della cella |CTRL + Fine|
|Andare a sinistra di una parola|CTRL + freccia sinistra|
|Andare a destra di una parola|CTRL + freccia destra|
|Selezionare tutto|CTRL + A|
|Impostare un rientro| CTRL +]|
|Annullare l'impostazione di un rientro|CTRL + [|
|Passare alla modalità comandi| ESC |

---

## <a name="next-steps"></a>Passaggi successivi
- [Estrai notebook di esempio sinapsi](https://github.com/Azure-Samples/Synapse/tree/master/Notebooks)
- [Avvio rapido: Creare un pool di Apache Spark in Azure Synapse Analytics con gli strumenti Web](../quickstart-apache-spark-notebook.md)
- [Che cos'è Apache Spark in Azure Synapse Analytics](apache-spark-overview.md)
- [Usare .NET per Apache Spark con Azure Synapse Analytics](spark-dotnet.md)
- [Documentazione di .NET per Apache Spark](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Azure Synapse Analytics](../index.yml)