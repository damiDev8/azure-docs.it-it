---
title: Come usare l'archiviazione di Accodamento (C++)-archiviazione di Azure
description: Informazioni su come usare il servizio di archiviazione di Accodamento in Azure. Gli esempi sono scritti in C++.
author: mhopkins-msft
ms.author: mhopkins
ms.reviewer: dineshm
ms.date: 07/16/2020
ms.topic: how-to
ms.service: storage
ms.subservice: queues
ms.openlocfilehash: 44d64c54049c02b6602f01b97effcc33b03dbcfe
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97591328"
---
# <a name="how-to-use-queue-storage-from-c"></a>Come usare l'archiviazione delle code da C++

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Panoramica

Questa guida illustra come eseguire scenari comuni usando il servizio di archiviazione di Accodamento di Azure. Gli esempi sono scritti in C++ e usano la [libreria client di archiviazione di Azure per c++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). Gli scenari illustrati includono **inserimento**, **visualizzazione**, **recupero** ed **eliminazione** dei messaggi in coda, oltre alla **creazione ed eliminazione di code**.

> [!NOTE]
> Questa guida è destinata alla libreria client di archiviazione di Azure per C++ v 1.0.0 e versioni successive. La versione consigliata è la libreria client di archiviazione di Azure v 2.2.0, disponibile tramite [NuGet](https://www.nuget.org/packages/wastorage) o [GitHub](https://github.com/Azure/azure-storage-cpp/).

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Creazione di un’applicazione C++

In questa guida verranno utilizzate le funzionalità di archiviazione che è possibile eseguire all’interno di un’applicazione C++.

A tale scopo, sarà necessario installare la libreria client di archiviazione di Azure per C++ e creare un account di archiviazione di Azure nella sottoscrizione di Azure.

<!-- docutune:casing "Getting Started on Linux" -->

Per installare la libreria client di archiviazione di Azure per C++, è possibile usare i metodi seguenti:

- **Linux:** Seguire le istruzioni fornite nella pagina del [file Leggimi della libreria client di archiviazione di Azure per C++: introduzione in Linux](https://github.com/Azure/azure-storage-cpp#getting-started-on-linux) .
- **Windows:** in Windows usare [vcpkg](https://github.com/microsoft/vcpkg) come utilità di gestione dipendenze. Seguire la [Guida introduttiva](https://github.com/microsoft/vcpkg#quick-start) per inizializzare `vcpkg` . Quindi usare il comando seguente per installare la libreria:

```powershell
.\vcpkg.exe install azure-storage-cpp
```

È possibile trovare una guida su come compilare il codice sorgente ed esportare in NuGet nel file [Leggimi](https://github.com/Azure/azure-storage-cpp#download--install) .

## <a name="configure-your-application-to-access-queue-storage"></a>Configurazione dell'applicazione per l’accesso ad Archiviazione di accodamento

Aggiungere le istruzioni include seguenti all'inizio del file C++ in cui si desidera usare le API di archiviazione di Azure per accedere alle code:

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Configurare una stringa di connessione di archiviazione di Azure

I client di archiviazione di Azure usano le stringhe di connessione di archiviazione per archiviare endpoint e credenziali per l'accesso ai servizi di gestione dati. Quando si esegue un'applicazione client, è necessario specificare la stringa di connessione di archiviazione nel formato seguente, usando il nome dell'account di archiviazione e la chiave di accesso alle archiviazione per l'account di archiviazione elencato nella [portale di Azure](https://portal.azure.com) per i `AccountName` `AccountKey` valori e. Per informazioni sugli account di archiviazione e sulle chiavi di accesso, vedere [informazioni sugli account di archiviazione di Azure](../common/storage-account-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json). In questo esempio viene illustrato come dichiarare un campo statico per memorizzare la stringa di connessione:

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

Per testare l'applicazione nel computer Windows locale, è possibile usare l' [emulatore di archiviazione azzurrite](../common/storage-use-azurite.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json). Azzurrite è un'utilità che simula l'archiviazione BLOB di Azure e l'archiviazione di accodamento nel computer di sviluppo locale. Nell’esempio seguente viene illustrato come dichiarare un campo statico per memorizzare la stringa di connessione all’emulatore di archiviazione locale:

```cpp
// Define the connection-string with Azurite.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Per avviare azzurrite, vedere [usare l'emulatore di azzurrite per lo sviluppo locale di archiviazione di Azure](../common/storage-use-azurite.md).

Gli esempi seguenti presumono che sia stato usato uno di questi due metodi per ottenere la stringa di connessione di archiviazione.

## <a name="retrieve-your-connection-string"></a>Recuperare la stringa di connessione

Per visualizzare le informazioni dell'account di archiviazione, è possibile usare la classe `cloud_storage_account`. Per recuperare le informazioni sull'account di archiviazione dalla stringa di connessione di archiviazione, è possibile usare il `parse` metodo.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a>Procedura: creare una coda

Un `cloud_queue_client` oggetto consente di ottenere oggetti di riferimento per le code. Il codice seguente crea un `cloud_queue_client` oggetto.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

Utilizzare l' `cloud_queue_client` oggetto per ottenere un riferimento alla coda che si desidera utilizzare. È possibile creare la coda se non esiste già.

```cpp
// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedura: inserire un messaggio in una coda

Per inserire un messaggio in una coda esistente, creare innanzitutto un nuovo oggetto `cloud_queue_message` . Chiamare quindi il `add_message` metodo. `cloud_queue_message`È possibile creare un oggetto da una stringa (in formato UTF-8) o da una matrice di byte. Ecco il codice che crea una coda (se non esiste) e inserisce il messaggio `Hello, World` :

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
queue.create_if_not_exists();

// Create a message and add it to the queue.
azure::storage::cloud_queue_message message1(U("Hello, World"));
queue.add_message(message1);  
```

## <a name="how-to-peek-at-the-next-message"></a>Procedura: visualizzare il messaggio successivo

È possibile visualizzare il messaggio nella parte anteriore di una coda senza rimuoverlo dalla coda chiamando il `peek_message` metodo.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Peek at the next message.
azure::storage::cloud_queue_message peeked_message = queue.peek_message();

// Output the message content.
std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedura: cambiare il contenuto di un messaggio accodato

È possibile cambiare il contenuto di un messaggio inserito nella coda. Se il messaggio rappresenta un'attività di lavoro, è possibile utilizzare questa funzionalità per aggiornarne lo stato. Il codice seguente consente di aggiornare il messaggio in coda con nuovo contenuto e di impostarne il timeout di visibilità per prolungarlo di altri 60 secondi. In questo modo lo stato del lavoro associato al messaggio viene salvato e il client ha a disposizione un altro minuto per continuare l'elaborazione del messaggio. È possibile utilizzare questa tecnica per tenere traccia dei flussi di lavoro in più passaggi nei messaggi in coda, senza dover ricominciare dall'inizio se un passaggio di elaborazione non riesce a causa di un errore hardware o software. In genere, è consigliabile mantenere anche un conteggio dei tentativi, in modo da eliminare i messaggi per cui vengono effettuati più di n tentativi. In questo modo è possibile evitare che un messaggio attivi un errore dell'applicazione ogni volta che viene elaborato.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the message from the queue and update the message contents.
// The visibility timeout "0" means make it visible immediately.
// The visibility timeout "60" means the client can get another minute to continue
// working on the message.
azure::storage::cloud_queue_message changed_message = queue.get_message();

changed_message.set_content(U("Changed message"));
queue.update_message(changed_message, std::chrono::seconds(60), true);

// Output the message content.
std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  
```

## <a name="how-to-dequeue-the-next-message"></a>Procedura: rimuovere il messaggio successivo dalla coda

Il codice consente di rimuovere un messaggio da una coda in due passaggi. Quando si chiama `get_message` , si ottiene il messaggio successivo in una coda. Un messaggio restituito da `get_message` diventa invisibile a qualsiasi altro elemento di codice che legge i messaggi di questa coda. Per completare la rimozione del messaggio dalla coda, è necessario chiamare anche `delete_message` . Questo processo in due passaggi di rimozione di un messaggio assicura che, qualora l'elaborazione di un messaggio non riesca a causa di errori hardware o software, un'altra istanza del codice sia in grado di ottenere lo stesso messaggio e di riprovare. Il codice chiama `delete_message` subito dopo l'elaborazione del messaggio.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the next message.
azure::storage::cloud_queue_message dequeued_message = queue.get_message();
std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

// Delete the message.
queue.delete_message(dequeued_message);
```

## <a name="how-to-use-additional-options-for-dequeuing-messages"></a>Procedura: utilizzare opzioni aggiuntive per la rimozione dalla coda dei messaggi

È possibile personalizzare il recupero di messaggi da una coda in due modi. Innanzitutto, è possibile recuperare un batch di messaggi (massimo 32). In secondo luogo, è possibile impostare un timeout di invisibilità più lungo o più breve assegnando al codice più o meno tempo per l'elaborazione completa di ogni messaggio. Nell'esempio di codice seguente viene usato il metodo `get_messages` per recuperare 20 messaggi con una sola chiamata. Ogni messaggio viene poi elaborato con un ciclo `for`. Per ogni messaggio, inoltre, il timeout di invisibilità viene impostato su cinque minuti. Si noti che i cinque minuti iniziano per tutti i messaggi contemporaneamente, quindi dopo che sono trascorsi cinque minuti dalla chiamata a `get_messages` , tutti i messaggi che non sono stati eliminati diventeranno nuovamente visibili.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
// 5 minutes (300 seconds).
azure::storage::queue_request_options options;
azure::storage::operation_context context;

// Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

for (auto it = messages.cbegin(); it != messages.cend(); ++it)
{
    // Display the contents of the message.
    std::wcout << U("Get: ") << it->content_as_string() << std::endl;
}
```

## <a name="how-to-get-the-queue-length"></a>Procedura: recuperare la lunghezza delle code

È possibile ottenere una stima sul numero di messaggi presenti in una coda. Il `download_attributes` metodo restituisce le proprietà della coda, incluso il numero di messaggi. Il `approximate_message_count` metodo ottiene il numero approssimativo di messaggi nella coda.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Fetch the queue attributes.
queue.download_attributes();

// Retrieve the cached approximate message count.
int cachedMessageCount = queue.approximate_message_count();

// Display number of messages.
std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  
```

## <a name="how-to-delete-a-queue"></a>Procedura: eliminare una coda

Per eliminare una coda e tutti i messaggi che contiene, chiamare il `delete_queue_if_exists` metodo sull'oggetto Queue.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// If the queue exists and delete it.
queue.delete_queue_if_exists();  
```

## <a name="next-steps"></a>Passaggi successivi

Ora che sono state apprese le nozioni di base dell'archiviazione di Accodamento, seguire questi collegamenti per altre informazioni su archiviazione di Azure.

- [Come usare l'archiviazione BLOB da C++](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
- [Come usare l’archiviazione tabelle da C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
- [Elenco delle risorse di archiviazione di Azure in C++](../common/storage-c-plus-plus-enumeration.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
- [Informazioni di riferimento sulla libreria client di archiviazione di Azure per C++](https://azure.github.io/azure-storage-cpp)
- [Documentazione di Archiviazione di Azure](https://azure.microsoft.com/documentation/services/storage/)
