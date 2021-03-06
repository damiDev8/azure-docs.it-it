---
title: Sessioni di Rendering remoto
description: Illustra in che cosa consiste una sessione di rendering remoto
author: jakrams
ms.author: jakras
ms.date: 02/21/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: 321d73c78d0192dcb7a303f4aa70a4ff0f18ecea
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/05/2021
ms.locfileid: "99593706"
---
# <a name="remote-rendering-sessions"></a>Sessioni di Rendering remoto

In Rendering remoto di Azure (ARR), una *sessione* rappresenta un concetto chiave. Questo articolo illustra in che cosa consiste esattamente una sessione.

## <a name="overview"></a>Panoramica

Rendering remoto di Azure comporta l'offload di attività di rendering complesse nel cloud. Queste attività di rendering non possono essere eseguite su qualsiasi server, perché la maggior parte dei server cloud non dispone di GPU. A causa della quantità di dati necessari e dei requisiti complessi per la produzione di risultati a frequenze dei fotogrammi interattive, la responsabilità del server che gestisce una richiesta dell'utente non può essere passata a un altro computer in tempo reale, come per il traffico Web più comune.

Ciò significa che, quando si usa Rendering remoto di Azure, un server cloud con le funzionalità hardware necessarie deve essere riservato esclusivamente alla gestione delle richieste di rendering. Una *sessione* fa riferimento a tutti gli elementi interessati dall'interazione con il server. Essa inizia con la richiesta iniziale di riservare un computer per l'uso (*lease*), continua con tutti i comandi per il caricamento e la manipolazione dei modelli e termina con il rilascio del lease nel server cloud.

## <a name="managing-sessions"></a>Gestione delle sessioni

Esistono diversi modi per gestire le sessioni e interagire con esse. Un modo indipendente dal linguaggio per creare, aggiornare e arrestare le sessioni è offerto dall'[API REST di gestione delle sessioni](../how-tos/session-rest-api.md). In C# e C++, queste operazioni vengono esposte tramite le classi `RemoteRenderingClient` e `RenderingSession`. Per le applicazioni Unity, sono disponibili altre funzioni di utilità offerte dal componente `ARRServiceUnity`.

Una volta *stabilita la connessione* a una sessione attiva, le operazioni quali il [caricamento di modelli](models.md) e l'interazione con la scena vengono esposte tramite la classe `RenderingSession`.

### <a name="managing-multiple-sessions-simultaneously"></a>Gestione simultanea di più sessioni

Non è possibile stabilire una *connessione completa* a più sessioni da un dispositivo. Tuttavia, è possibile creare, osservare e arrestare tutte le sessioni desiderate da un'unica applicazione. Fintanto che l'app non è destinata a connettersi a una sessione, non è necessario neanche eseguirla in un dispositivo come HoloLens 2. Un caso d'uso per tale implementazione può essere rappresentato dalla necessità di controllare le sessioni tramite un meccanismo centrale. Ad esempio, è possibile creare un'app Web, in cui è possibile accedere a più tablet e dispositivi HoloLens. Quindi, l'app può visualizzare le opzioni sui tablet, ad esempio il modello CAD da visualizzare. Se un utente effettua una selezione, queste informazioni vengono comunicate a tutti i dispositivi HoloLens per creare un'esperienza condivisa.

## <a name="session-phases"></a>Fasi della sessione

Ogni sessione prevede più fasi.

### <a name="session-startup"></a>Avvio della sessione

Quando si chiede ad ARR di [creare una nuova sessione](../how-tos/session-rest-api.md#create-a-session), la prima cosa che fa è restituire un [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) di sessione. L'UUID consente di eseguire query sulle informazioni relative alla sessione. L'UUID e alcune informazioni di base sulla sessione vengono resi permanenti per 30 giorni, pertanto è possibile eseguire query su tali informazioni anche dopo l'arresto della sessione. A questo punto, lo **stato della sessione** verrà segnalato come **Avvio in corso**.

Successivamente, il rendering remoto di Azure tenterà di trovare un server in grado di ospitare la sessione. Per questa ricerca sono disponibili due parametri. In primo luogo, vengono riservati solo i server nell'[area di pertinenza](../reference/regions.md). Ciò è dovuto al fatto che la latenza di rete tra aree può essere troppo elevata per garantire un'esperienza soddisfacente. Il secondo fattore è rappresentato dalle *dimensioni* desiderate specificate. In ogni area è disponibile un numero limitato di server che possono soddisfare la richiesta di dimensioni [*standard*](../reference/vm-sizes.md) o [*Premium*](../reference/vm-sizes.md) . Di conseguenza, se tutti i server delle dimensioni richieste sono attualmente in uso nell'area, la creazione della sessione avrà esito negativo. Il motivo dell'errore [può essere sottoposto a query](../how-tos/session-rest-api.md#get-sessions-properties).

> [!IMPORTANT]
> Se si richiede una dimensione del server *standard* e la richiesta ha esito negativo a causa di una richiesta elevata, questo non implica che la richiesta di un server *Premium* avrà esito negativo. Quindi, se si tratta di un'opzione, è possibile provare a eseguire il fallback a una dimensione del server *Premium* .

Quando il servizio individua un server idoneo, deve copiare la macchina virtuale appropriata per trasformarla in un host di rendering remoto di Azure. Questo processo richiede alcuni minuti. Successivamente, la macchina virtuale viene avviata e lo **stato della sessione** passa a **Pronto**.

A questo punto, il server è in attesa esclusivamente dell'input. Questo è anche il punto da cui decorre l'addebito del servizio.

### <a name="connecting-to-a-session"></a>Connessione a una sessione

Una volta *pronta* la sessione, è possibile *connettersi* ad essa. Durante la connessione, il dispositivo può inviare comandi per caricare e modificare i modelli. Ogni host ARR serve un solo dispositivo client alla volta, quindi, quando un client si connette a una sessione, dispone di un controllo esclusivo sul contenuto sottoposto a rendering. Ciò significa anche che le prestazioni di rendering non variano mai per motivi che esulano dal controllo dell'utente.

> [!IMPORTANT]
> Anche se a una sessione può *connettersi* un solo client, è possibile eseguire query sulle informazioni di base sulle sessioni, ad esempio lo stato corrente, senza connettersi.

Quando un dispositivo è connesso a una sessione, i tentativi di connessione di altri dispositivi avranno esito negativo. Tuttavia, una volta che il dispositivo connesso si disconnette, volontariamente o a causa di un errore, la sessione accetterà un'altra richiesta di connessione. Tutti gli stati precedenti (modelli caricati e altro) vengono eliminati, in modo tale che il successivo dispositivo connesso disponga di uno slate libero. Le sessioni possono quindi essere riutilizzate più volte da dispositivi diversi e, nella maggior parte dei casi, è possibile nascondere l'overhead di avvio della sessione all'utente finale.

> [!IMPORTANT]
> Il server remoto non modifica mai lo stato dei dati sul lato client. Eventuali mutazioni dei dati, ad esempio gli aggiornamenti delle trasformazioni e le richieste di caricamento, devono essere eseguite dall'applicazione client. Tutte le azioni aggiornano immediatamente lo stato del client.

### <a name="session-end"></a>Fine sessione

Quando si richiede una nuova sessione, è necessario specificare una *durata del lease massima*, in genere compresa in un intervallo tra una e otto ore. Questa indica il periodo di tempo durante il quale l'host accetterà input.

Esistono di norma due motivi per terminare una sessione. È possibile richiedere manualmente che la sessione venga arrestata o che la durata del lease massima sia scaduta. In entrambi i casi, eventuali connessioni attive all'host vengono chiuse immediatamente e il servizio sul server viene arrestato. Il server viene quindi restituito al pool di Azure e può essere requisito per altri scopi. L'arresto di una sessione non può essere interrotto o annullato. L'esecuzione di query sullo **stato della sessione** in una sessione arrestata restituirà **Interrotto** o **Scaduto**, a seconda che la sessione sia stata arrestata manualmente o che sia stata raggiunta la durata del lease massima.

Una sessione può anche essere arrestata a causa di un errore.

In tutti i casi, dopo l'arresto di una sessione non verrà applicato alcun addebito.

> [!WARNING]
> L'eventuale connessione a una sessione e la relativa durata non influiscono sulla fatturazione. Il pagamento per il servizio dipende dalla *durata della sessione*, ovvero dal tempo in cui un server viene riservato esclusivamente e dalle capacità hardware richieste ( [dimensione allocata](../reference/vm-sizes.md)). Se si avvia una sessione, ci si connette per cinque minuti e quindi non si arresta la sessione, per cui l'esecuzione prosegue fino alla scadenza del lease, verrà addebitata l'intera durata del lease. Viceversa, la *durata del lease massima* è principalmente una rete di sicurezza. Non è importante se si richieda o meno una sessione con una durata del lease di otto ore per poi usarla solo per cinque minuti, se si arresta manualmente la sessione in un secondo momento.

#### <a name="extend-a-sessions-lease-time"></a>Estendere la durata del lease di una sessione

È possibile [estendere la durata del lease](../how-tos/session-rest-api.md#modify-and-query-session-properties) di una sessione attiva nel caso in cui sia necessario più tempo.

## <a name="example-code"></a>Codice di esempio

Il codice riportato di seguito illustra una semplice implementazione di avvio di una sessione, in attesa dello stato *Pronto*, nonché la relativa connessione, disconnessione e chiusura.

```cs
RemoteRenderingInitialization init = new RemoteRenderingInitialization();
// fill out RemoteRenderingInitialization parameters...

RemoteManagerStatic.StartupRemoteRendering(init);

SessionConfiguration sessionConfig = new SessionConfiguration();
// fill out sessionConfig details...

RemoteRenderingClient client = new RemoteRenderingClient(sessionConfig);

RenderingSessionCreationOptions rendererOptions = new RenderingSessionCreationOptions();
// fill out rendererOptions...

CreateRenderingSessionResult result = await client.CreateNewRenderingSessionAsync(rendererOptions);

RenderingSession session = result.Session;
RenderingSessionProperties sessionProperties;
while (true)
{
    var propertiesResult = await session.GetPropertiesAsync();
    sessionProperties = propertiesResult.SessionProperties;
    if (sessionProperties.Status != RenderingSessionStatus.Starting &&
        sessionProperties.Status != RenderingSessionStatus.Unknown)
    {
        break;
    }
    // REST calls must not be issued too frequently, otherwise the server returns failure code 429 ("too many requests"). So we insert the recommended delay of 10s
    await Task.Delay(TimeSpan.FromSeconds(10));
}

if (sessionProperties.Status != RenderingSessionStatus.Ready)
{
    // Do some error handling and either terminate or retry.
}

// Connect to server
ConnectionStatus connectStatus = await session.ConnectAsync(new RendererInitOptions());

// Connected!

while (...)
{
    // per frame update

    session.Connection.Update();
}

// Disconnect
session.Disconnect();

// stop the session
await session.StopAsync();

// shut down the remote rendering SDK
RemoteManagerStatic.ShutdownRemoteRendering();
```

È possibile mantenere, modificare ed eseguire query su più istanze di `RemoteRenderingClient` e `RenderingSession` dal codice. Tuttavia, a `RenderingSession` può connettersi un solo dispositivo alla volta.

La durata di una macchina virtuale non è associata all'istanza di `RemoteRenderingClient` o `RenderingSession`. `RenderingSession.StopAsync` deve essere chiamato per arrestare una sessione.

L'ID sessione persistente può essere sottoposto a query tramite `RenderingSession.SessionUuid()` e memorizzato nella cache in locale. Con questo ID, un'applicazione può chiamare `RemoteRenderingClient.OpenRenderingSessionAsync` per eseguire l'associazione a tale sessione.

Quando `RenderingSession.IsConnected` è true, `RenderingSession.Connection` restituisce un'istanza di `RenderingConnection` che contiene le funzioni per [caricare modelli](models.md), modificare [entità](entities.md) ed [eseguire query di informazioni](../overview/features/spatial-queries.md) sulla scena di cui è stato eseguito il rendering.

## <a name="api-documentation"></a>Documentazione dell'API

* [C# RenderingSession (classe)](/dotnet/api/microsoft.azure.remoterendering.renderingsession)
* [C# RemoteRenderingClient. CreateNewRenderingSessionAsync ()](/dotnet/api/microsoft.azure.remoterendering.remoterenderingclient.createnewrenderingsessionasync)
* [C# RemoteRenderingClient. OpenRenderingSessionAsync ()](/dotnet/api/microsoft.azure.remoterendering.remoterenderingclient.openrenderingsessionasync)
* [Classe C++ RenderingSession](/cpp/api/remote-rendering/renderingsession)
* [C++ RemoteRenderingClient:: CreateNewRenderingSessionAsync](/cpp/api/remote-rendering/remoterenderingclient#createnewrenderingsessionasync)
* [C++ RemoteRenderingClient:: OpenRenderingSession](/cpp/api/remote-rendering/remoterenderingclient#openrenderingsession)

## <a name="next-steps"></a>Passaggi successivi

* [Entità](entities.md)
* [Modelli](models.md)