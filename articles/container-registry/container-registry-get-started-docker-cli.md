---
title: Push & immagine del contenitore di pull
description: Eseguire il push e il pull delle immagini Docker nel registro contenitori privato in Azure usando l'interfaccia della riga di comando di Docker
ms.topic: article
ms.date: 01/23/2019
ms.custom: seodec18, H1Hack27Feb2017
ms.openlocfilehash: 83ef385313b035f5e5d7d993e7948725906c75a7
ms.sourcegitcommit: 7e117cfec95a7e61f4720db3c36c4fa35021846b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/09/2021
ms.locfileid: "99987768"
---
# <a name="push-your-first-image-to-your-azure-container-registry-using-the-docker-cli"></a>Eseguire il push della prima immagine nel registro contenitori di Azure usando l'interfaccia della riga di comando di Docker

Un registro contenitori di Azure archivia e gestisce le immagini del contenitore privato e altri elementi, in modo analogo a come [Docker Hub](https://hub.docker.com/) archivia le immagini del contenitore Docker pubblico. È possibile usare l' [interfaccia della riga di comando di Docker](https://docs.docker.com/engine/reference/commandline/cli/) per le operazioni di [accesso](https://docs.docker.com/engine/reference/commandline/login/), [push](https://docs.docker.com/engine/reference/commandline/push/), [pull](https://docs.docker.com/engine/reference/commandline/pull/)e altre immagini del contenitore nel registro contenitori.

Nei passaggi seguenti viene scaricata un'immagine di [nginx](https://store.docker.com/images/nginx)pubblica, viene contrassegnata per il registro contenitori di Azure privato, ne viene effettuato il push nel registro di sistema e quindi viene effettuato il pull dal registro di sistema.

## <a name="prerequisites"></a>Prerequisiti

* **Registro contenitori di Azure** : creare un registro contenitori nella sottoscrizione di Azure. Ad esempio usare il [portale di Azure](container-registry-get-started-portal.md) oppure l'[interfaccia della riga di comando di Azure](container-registry-get-started-azure-cli.md).
* **Interfaccia della riga di comando di Docker**: è anche necessario avere Docker installato localmente. Docker offre pacchetti che consentono di configurare facilmente Docker in qualsiasi sistema [macOS][docker-mac], [Windows][docker-windows] o [Linux][docker-linux].

## <a name="log-in-to-a-registry"></a>Accedere a un registro

Esistono [diversi modi per eseguire l'autenticazione](container-registry-authentication.md) nel registro contenitori privato. È il metodo consigliato quando si usa una riga di comando è rappresentato dal comando dell'interfaccia della riga di comando di Azure [az acr login](/cli/azure/acr#az-acr-login). Ad esempio, per accedere a un *registro denominato Registry*, accedere all'interfaccia della riga di comando di Azure e quindi eseguire l'autenticazione al registro di sistema:

```azurecli
az login
az acr login --name myregistry
```

È anche possibile eseguire l'accesso con il comando [docker login](https://docs.docker.com/engine/reference/commandline/login/). Ad esempio, è possibile che sia stata [assegnata un'entità servizio](container-registry-authentication.md#service-principal) al registro per uno scenario di automazione. Quando si esegue questo comando, specificare in modo interattivo l'appID dell'entità servizio (nome utente) e la password quando richiesto. Per le procedure consigliate relative alla gestione delle credenziali di accesso, vedere le informazioni di riferimento sul comando [docker login](https://docs.docker.com/engine/reference/commandline/login/):

```
docker login myregistry.azurecr.io
```

Entrambi i comandi restituiscono `Login Succeeded` una volta completati.
> [!NOTE]
>* Potrebbe essere necessario usare Visual Studio Code con l'estensione Docker per un accesso più veloce e più pratico.

> [!TIP]
> Specificare sempre il nome completo (tutto in maiuscolo) del registro quando si usa `docker login` e quando le immagini vengono contrassegnate per l'esecuzione del push nel registro. Negli esempi riportati in questo articolo il nome completo è *myregistry.azurecr.io*.

## <a name="pull-a-public-nginx-image"></a>Pull an public nginx image

Per prima cosa, effettuare il pull di un'immagine di nginx pubblica nel computer locale. Questo esempio estrae un'immagine da Microsoft Container Registry.

```
docker pull mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
```

## <a name="run-the-container-locally"></a>Eseguire il contenitore in locale

Eseguire il comando [Docker Run](https://docs.docker.com/engine/reference/run/) seguente per avviare un'istanza locale del contenitore nginx in modo interattivo ( `-it` ) sulla porta 8080. L'argomento `--rm` specifica che il contenitore deve essere rimosso quando si arresta.

```
docker run -it --rm -p 8080:80 mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
```

Passare a `http://localhost:8080` per visualizzare la pagina Web predefinita servita da Nginx nel contenitore in esecuzione. Verrà visualizzata una pagina simile alla seguente:

![Nginx sul computer locale](./media/container-registry-get-started-docker-cli/nginx.png)

Poiché il contenitore è stato avviato in modalità interattiva con `-it`, è possibile visualizzare l'output del server Nginx nella riga di comando nel browser in uso.

Per arrestare e rimuovere il contenitore, premere `Control`+`C`.

## <a name="create-an-alias-of-the-image"></a>Creare un alias dell'immagine

Usare [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) per creare un alias dell'immagine, con un percorso completo del registro. Questo esempio specifica lo spazio dei nomi `samples` per evitare confusione nella radice del registro.

```
docker tag mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine myregistry.azurecr.io/samples/nginx
```

Per altre informazioni sull'assegnazione di tag con spazi dei nomi, vedere la sezione [Spazi dei nomi dell'archivio](container-registry-best-practices.md#repository-namespaces) nell'argomento [Procedure consigliate per Registro Azure Container](container-registry-best-practices.md).

## <a name="push-the-image-to-your-registry"></a>Eseguire il push dell'immagine nel registro

Dopo aver contrassegnato l'immagine con il percorso completo del registro privato, è possibile eseguirne il push nel registro con [docker push](https://docs.docker.com/engine/reference/commandline/push/):

```
docker push myregistry.azurecr.io/samples/nginx
```

## <a name="pull-the-image-from-your-registry"></a>Eseguire il pull dell'immagine dal registro

Usare il comando [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) per eseguire il pull dell'immagine dal registro:

```
docker pull myregistry.azurecr.io/samples/nginx
```

## <a name="start-the-nginx-container"></a>Avviare il contenitore Nginx

Usare il comando [docker run](https://docs.docker.com/engine/reference/run/) per eseguire l'immagine di cui è stato eseguito il pull dal registro:

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Passare a `http://localhost:8080` per visualizzare il contenitore in esecuzione.

Per arrestare e rimuovere il contenitore, premere `Control`+`C`.

## <a name="remove-the-image-optional"></a>Rimuovere l'immagine (facoltativo)

Se l'immagine di Nginx non è più necessaria, è possibile eliminarla in locale con il comando [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/).

```
docker rmi myregistry.azurecr.io/samples/nginx
```

Per rimuovere le immagini dal Registro Azure Container, è possibile usare il comando dell'interfaccia della riga di comando di Azure [az acr repository delete](/cli/azure/acr/repository#az-acr-repository-delete). Il comando seguente, ad esempio, elimina il manifesto a cui fa riferimento il tag `samples/nginx:latest`, tutti i dati di livello univoci e tutti gli altri tag che fanno riferimento al manifesto.

```azurecli
az acr repository delete --name myregistry --image samples/nginx:latest
```

## <a name="next-steps"></a>Passaggi successivi

Una volta apprese le nozioni di base, si è pronti per iniziare a usare il registro. È possibile, ad esempio, distribuire le immagini del contenitore dal registro nella posizione seguente:

* [Servizio Azure Kubernetes](../aks/tutorial-kubernetes-prepare-app.md)
* [Istanze di Azure Container](../container-instances/container-instances-tutorial-prepare-app.md)
* [Service Fabric](../service-fabric/service-fabric-tutorial-create-container-images.md)

Installare eventualmente l'[estensione Docker per Visual Studio Code](https://code.visualstudio.com/docs/azure/docker) e l'estensione [Account Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) per l'uso dei registri contenitori di Azure. Eseguire il pull e il push delle immagini in un registro contenitori di Azure o eseguire Attività del Registro Azure Container, il tutto all'interno di Visual Studio Code.


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/
