---
title: Come usare l'archiviazione di Accodamento da PHP-archiviazione di Azure
description: Informazioni su come usare il servizio di archiviazione di Accodamento di Azure per creare ed eliminare code e per inserire, ottenere ed eliminare messaggi. Gli esempi sono scritti in PHP.
author: mhopkins-msft
ms.author: mhopkins
ms.reviewer: dineshm
ms.date: 01/11/2018
ms.topic: how-to
ms.service: storage
ms.subservice: queues
ms.openlocfilehash: 69369d81892a10c390aa31a2c46f79fdfa41206d
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97592025"
---
# <a name="how-to-use-queue-storage-from-php"></a>Come usare l'archiviazione di Accodamento da PHP

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

Questa guida illustra come eseguire scenari comuni usando il servizio di archiviazione di Accodamento di Azure. Gli esempi sono scritti tramite le classi della [libreria client di archiviazione di Azure per php](https://github.com/Azure/azure-storage-php). Gli scenari presentati includono l'inserimento, la visualizzazione, il recupero e l'eliminazione dei messaggi in coda, oltre alle procedure di creazione ed eliminazione di code.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Creare un'applicazione PHP

L'unico requisito per la creazione di un'applicazione PHP che accede all'archiviazione code di Azure è il riferimento delle classi nella [libreria client di archiviazione di Azure per php](https://github.com/Azure/azure-storage-php) dall'interno del codice. Per creare l'applicazione, è possibile usare qualsiasi strumento di sviluppo, incluso il Blocco note.

In questa guida vengono usate le funzionalità del servizio di archiviazione di accodamento che possono essere chiamate in un'applicazione PHP in locale o nel codice in esecuzione all'interno di un'applicazione Web in Azure.

## <a name="get-the-azure-client-libraries"></a>Acquisire le librerie client di Azure

### <a name="install-via-composer"></a>Installare tramite Composer

1. Creare un file denominato `composer.json` nella radice del progetto e aggiungervi il codice seguente:

    ```json
    {
      "require": {
        "microsoft/azure-storage-queue": "*"
      }
    }
    ```

2. Scaricare [`composer.phar`](https://getcomposer.org/composer.phar) nella radice del progetto.

3. Aprire un prompt dei comandi ed eseguire il comando seguente nella radice del progetto:

    ```console
    php composer.phar install
    ```

In alternativa, passare alla [libreria client PHP di archiviazione di Azure](https://github.com/Azure/azure-storage-php) su GitHub per clonare il codice sorgente.

## <a name="configure-your-application-to-access-queue-storage"></a>Configurazione dell'applicazione per l’accesso ad Archiviazione di accodamento

Per usare le API per l'archiviazione code di Azure, è necessario:

1. Usare l'istruzione per fare riferimento al file autoloader [`require_once`](https://www.php.net/manual/en/function.require-once.php) .
2. Fare riferimento a tutte le eventuali classi utilizzabili.

Nell'esempio seguente viene indicato come includere il file autoloader e fare riferimento alla classe `QueueRestProxy`.

```php
require_once 'vendor/autoload.php';
use MicrosoftAzure\Storage\Queue\QueueRestProxy;
```

Negli esempi seguenti l' `require_once` istruzione è sempre visualizzata, ma si fa riferimento solo alle classi necessarie per eseguire l'esempio.

## <a name="set-up-an-azure-storage-connection"></a>Configurare una connessione di archiviazione di Azure

Per creare un'istanza di un client di archiviazione code di Azure, è necessario innanzitutto disporre di una stringa di connessione valida. Il formato della stringa di connessione dell'archiviazione di accodamento è il seguente.

Per accedere a un servizio attivo:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Per accedere alla memoria dell'emulatore:

```php
UseDevelopmentStorage=true
```

Per creare un client di archiviazione code di Azure, è necessario usare la `QueueRestProxy` classe. È possibile utilizzare le tecniche seguenti:

- Passare la stringa di connessione direttamente.
- Usare le variabili di ambiente nell'app Web per archiviare la stringa di connessione. Vedere il documento sulle [impostazioni di configurazione di app Web di Azure](../../app-service/configure-common.md) per la configurazione delle stringhe di connessione.

Per gli esempi illustrati in questo articolo, la stringa di connessione viene passata direttamente.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";
$queueClient = QueueRestProxy::createQueueService($connectionString);
```

## <a name="create-a-queue"></a>Creare una coda

Un `QueueRestProxy` oggetto consente di creare una coda usando il `CreateQueue` metodo. Quando si crea una coda, è possibile impostare le opzioni per la coda, anche se tale operazione non è necessaria. Questo esempio illustra come impostare i metadati in una coda.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateQueueOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// OPTIONAL: Set queue metadata.
$createQueueOptions = new CreateQueueOptions();
$createQueueOptions->addMetaData("key1", "value1");
$createQueueOptions->addMetaData("key2", "value2");

try    {
    // Create queue.
    $queueClient->createQueue("myqueue", $createQueueOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> Non basarsi sulla distinzione maiuscole/minuscole nelle chiavi di metadati. Il servizio legge tutte le chiavi come scritte in minuscolo.

## <a name="add-a-message-to-a-queue"></a>Aggiungere un messaggio a una coda

Per aggiungere un messaggio a una coda, usare `QueueRestProxy->createMessage` . Il metodo utilizza il nome della coda, il testo del messaggio e le opzioni messaggio (facoltative).

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateMessageOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

try    {
    // Create message.
    $queueClient->createMessage("myqueue", "Hello, World");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="peek-at-the-next-message"></a>Visualizzare il messaggio successivo

È possibile visualizzare uno o più messaggi all'inizio di una coda senza rimuoverli dalla coda chiamando `QueueRestProxy->peekMessages` . Per impostazione predefinita, il `peekMessage` metodo restituisce un singolo messaggio, ma è possibile modificare tale valore utilizzando il `PeekMessagesOptions->setNumberOfMessages` metodo.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\PeekMessagesOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// OPTIONAL: Set peek message options.
$message_options = new PeekMessagesOptions();
$message_options->setNumberOfMessages(1); // Default value is 1.

try    {
    $peekMessagesResult = $queueClient->peekMessages("myqueue", $message_options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$messages = $peekMessagesResult->getQueueMessages();

// View messages.
$messageCount = count($messages);
if($messageCount <= 0){
    echo "There are no messages.<br />";
}
else{
    foreach($messages as $message)    {
        echo "Peeked message:<br />";
        echo "Message Id: ".$message->getMessageId()."<br />";
        echo "Date: ".date_format($message->getInsertionDate(), 'Y-m-d')."<br />";
        echo "Message text: ".$message->getMessageText()."<br /><br />";
    }
}
```

## <a name="de-queue-the-next-message"></a>Rimuovere il messaggio successivo dalla coda

Il codice consente di rimuovere un messaggio da una coda in due passaggi. In primo luogo, si chiama `QueueRestProxy->listMessages` , che rende il messaggio invisibile a qualsiasi altro codice che legge dalla coda. Per impostazione predefinita, il messaggio rimane invisibile per 30 secondi. Se il messaggio non viene eliminato in questo periodo di tempo, diventa nuovamente visibile nella coda. Per completare la rimozione del messaggio dalla coda, è necessario chiamare `QueueRestProxy->deleteMessage` . Questo processo in due passaggi di rimozione di un messaggio assicura che, qualora l'elaborazione di un messaggio abbia esito negativo a causa di errori hardware o software, un'altra istanza del codice sia in grado di ottenere lo stesso messaggio e di riprovare. Il codice chiama `deleteMessage` subito dopo l'elaborazione del messaggio.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// Get message.
$listMessagesResult = $queueClient->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

/* ---------------------
    Process message.
   --------------------- */

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Delete message.
    $queueClient->deleteMessage("myqueue", $messageId, $popReceipt);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="change-the-contents-of-a-queued-message"></a>Cambiare il contenuto di un messaggio in coda

È possibile modificare il contenuto di un messaggio sul posto nella coda chiamando `QueueRestProxy->updateMessage` . Se il messaggio rappresenta un'attività di lavoro, è possibile utilizzare questa funzionalità per aggiornarne lo stato. Il codice seguente consente di aggiornare il messaggio in coda con nuovo contenuto e di impostarne il timeout di visibilità per prolungarlo di altri 60 secondi. In questo modo lo stato del lavoro associato al messaggio viene salvato e il client ha a disposizione un altro minuto per continuare l'elaborazione del messaggio. È possibile utilizzare questa tecnica per tenere traccia dei flussi di lavoro in più passaggi nei messaggi in coda, senza dover ricominciare dall'inizio se un passaggio di elaborazione non riesce a causa di un errore hardware o software. In genere, è consigliabile mantenere anche un conteggio dei tentativi, in modo da eliminare i messaggi per cui vengono effettuati più di *n* tentativi. In questo modo è possibile evitare che un messaggio attivi un errore dell'applicazione ogni volta che viene elaborato.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Get message.
$listMessagesResult = $queueClient->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

// Define new message properties.
$new_message_text = "New message text.";
$new_visibility_timeout = 5; // Measured in seconds.

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Update message.
    $queueClient->updateMessage("myqueue",
                                $messageId,
                                $popReceipt,
                                $new_message_text,
                                $new_visibility_timeout);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="additional-options-for-dequeuing-messages"></a>Opzioni aggiuntive per rimuovere i messaggi dalla coda

È possibile personalizzare il recupero di messaggi da una coda in due modi. Innanzitutto, è possibile recuperare un batch di messaggi (massimo 32). In secondo luogo, è possibile impostare un timeout di visibilità più lungo o più breve assegnando al codice più o meno tempo per l'elaborazione completa di ogni messaggio. Nell'esempio di codice seguente viene usato il `getMessages` metodo per ottenere 16 messaggi in una sola chiamata. Elabora quindi ogni messaggio usando un `for` ciclo. Per ogni messaggio, inoltre, il timeout di invisibilità viene impostato su cinque minuti.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\ListMessagesOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// Set list message options.
$message_options = new ListMessagesOptions();
$message_options->setVisibilityTimeoutInSeconds(300);
$message_options->setNumberOfMessages(16);

// Get messages.
try{
    $listMessagesResult = $queueClient->listMessages("myqueue",
                                                     $message_options);
    $messages = $listMessagesResult->getQueueMessages();

    foreach($messages as $message){

        /* ---------------------
            Process message.
        --------------------- */

        // Get message Id and pop receipt.
        $messageId = $message->getMessageId();
        $popReceipt = $message->getPopReceipt();

        // Delete message.
        $queueClient->deleteMessage("myqueue", $messageId, $popReceipt);
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="get-queue-length"></a>Recuperare la lunghezza della coda

È possibile ottenere una stima sul numero di messaggi presenti in una coda. Il `QueueRestProxy->getQueueMetadata` metodo recupera i metadati relativi alla coda. La chiamata al `getApproximateMessageCount` metodo sull'oggetto restituito fornisce un conteggio del numero di messaggi presenti in una coda. Il conteggio è solo approssimativo perché i messaggi possono essere aggiunti o rimossi dopo che l'archiviazione code risponde alla richiesta.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

try    {
    // Get queue metadata.
    $queue_metadata = $queueClient->getQueueMetadata("myqueue");
    $approx_msg_count = $queue_metadata->getApproximateMessageCount();
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

echo $approx_msg_count;
```

## <a name="delete-a-queue"></a>Eliminare una coda

Per eliminare una coda e tutti i messaggi in essa contenuti, chiamare il `QueueRestProxy->deleteQueue` metodo.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

try    {
    // Delete queue.
    $queueClient->deleteQueue("myqueue");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Passaggi successivi

Ora che sono state apprese le nozioni di base dell'archiviazione code di Azure, seguire i collegamenti seguenti per informazioni sulle attività di archiviazione più complesse:

- Vedere le informazioni [di riferimento sulle API per la libreria client PHP di archiviazione di Azure](https://azure.github.io/azure-storage-php/)
- Vedere l' [esempio di coda avanzata](https://github.com/Azure/azure-storage-php/blob/master/samples/QueueSamples.php).

Per ulteriori informazioni, vedere il [centro per sviluppatori PHP](https://azure.microsoft.com/develop/php/).
