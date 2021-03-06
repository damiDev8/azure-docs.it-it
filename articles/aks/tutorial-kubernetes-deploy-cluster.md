---
title: Esercitazione su Kubernetes in Azure - Distribuire un cluster
description: In questa esercitazione sul servizio Azure Kubernetes si crea un cluster del servizio Azure Kubernetes e si usa kubectl per connettersi al nodo master Kubernetes.
services: container-service
ms.topic: tutorial
ms.date: 01/12/2021
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: a8e0ddcd77c26a00cf784fb8c2372734314dc0bb
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98250639"
---
# <a name="tutorial-deploy-an-azure-kubernetes-service-aks-cluster"></a>Esercitazione: Distribuire un cluster del servizio Azure Kubernetes

Kubernetes fornisce una piattaforma distribuita per applicazioni in contenitori. Il servizio Azure Kubernetes consente di creare rapidamente un cluster Kubernetes per la produzione. In questa esercitazione, la terza di sette parti, viene distribuito un cluster Kubernetes in servizio Azure Kubernetes. Si apprenderà come:

> [!div class="checklist"]
> * Distribuire un cluster del servizio Azure Kubernetes in grado di eseguire l'autenticazione in un'istanza di Registro Azure Container
> * Installare l'interfaccia della riga di comando di Kubernetes (kubectl)
> * Configurare kubectl per connettersi al cluster servizio Azure Kubernetes

Nelle esercitazioni successive l'applicazione Azure vote viene distribuita nel cluster, ridimensionata e aggiornata.

## <a name="before-you-begin"></a>Prima di iniziare

Nelle esercitazioni precedenti, un'immagine del contenitore è stata creata e caricata in un'istanza di Registro Azure Container. Se questi passaggi non sono stati ancora eseguiti e si vuole procedere, iniziare dall'[Esercitazione 1: Creare immagini del contenitore][aks-tutorial-prepare-app].

Per questa esercitazione è necessario eseguire l'interfaccia della riga di comando di Azure versione 2.0.53 o successiva. Eseguire `az --version` per trovare la versione. Se è necessario eseguire l'installazione o l'aggiornamento, vedere [Installare l'interfaccia della riga di comando di Azure][azure-cli-install].

## <a name="create-a-kubernetes-cluster"></a>Creare un cluster Kubernetes

I cluster del servizio Azure Kubernetes possono usare il controllo degli accessi in base al ruolo di Kubernetes. Questi controlli consentono di definire l'accesso alle risorse in base ai ruoli assegnati agli utenti. È possibile combinare le autorizzazioni, se a un utente sono assegnati più ruoli, e definire l'ambito delle autorizzazioni in un unico spazio dei nomi o nell'intero cluster. Per impostazione predefinita, l'interfaccia della riga di comando di Azure abilita automaticamente il controllo degli accessi in base al ruolo di Kubernetes quando si crea un cluster del servizio Azure Kubernetes.

Creare un cluster servizio Azure Kubernetes usando [az servizio Azure Kubernetes create][]. L'esempio seguente crea un cluster denominato *myAKSCluster* nel gruppo di risorse denominato *myResourceGroup*. Questo gruppo di risorse è stato creato nell'area *eastus* durante l'[esercitazione precedente][aks-tutorial-prepare-acr]. Nell'esempio seguente non viene specificata alcuna area, di conseguenza anche il cluster del servizio Azure Kubernetes viene creato nell'area *eastus*. Per altre informazioni sui limiti delle risorse e sulla disponibilità delle aree per il servizio contenitore di Azure, vedere [quote, restrizioni per le dimensioni delle macchine virtuali e disponibilità di aree in Azure Kubernetes Service (AKS)][quotas-skus-regions] .

Per consentire a un cluster del servizio Azure Kubernetes di interagire con altre risorse di Azure viene creata automaticamente un'entità servizio di Azure Active Directory dal momento che non ne è stata specificata una. A questo punto a tale entità servizio viene [concesso il diritto di eseguire il pull delle immagini ][container-registry-integration] dall'istanza di Registro Azure Container creata nell'esercitazione precedente. Per eseguire correttamente il comando, è necessario avere un **proprietario** o un ruolo di **amministratore dell'account di Azure** nella sottoscrizione di Azure.

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 2 \
    --generate-ssh-keys \
    --attach-acr <acrName>
```

Per evitare di dover disporre di un ruolo di **proprietario** o di **amministratore account Azure** , è anche possibile configurare manualmente un'entità servizio per eseguire il pull delle immagini da ACR. Per altre informazioni, vedere [Autenticazione al Registro Azure Container con entità servizio](../container-registry/container-registry-auth-service-principal.md) o [Eseguire l'autenticazione da Kubernetes con un segreto per il pull](../container-registry/container-registry-auth-kubernetes.md). In alternativa, è possibile usare un' [identità gestita](use-managed-identity.md) anziché un'entità servizio per semplificare la gestione.

Dopo alcuni minuti, la distribuzione viene completata e restituisce le informazioni in formato JSON sulla distribuzione del servizio Azure Kubernetes.

> [!NOTE]
> Per garantire il funzionamento affidabile del cluster, è consigliabile eseguire almeno 2 (due) nodi.

## <a name="install-the-kubernetes-cli"></a>Installare l'interfaccia della riga di comando di Kubernetes

Per connettersi al cluster Kubernetes dal computer locale si usa [kubectl][kubectl], il client da riga di comando di Kubernetes.

Se si usa Azure Cloud Shell, `kubectl` è già installato. È anche possibile installarlo in locale usando il comando [az servizio Azure Kubernetes install-cli][]:

```azurecli
az aks install-cli
```

## <a name="connect-to-cluster-using-kubectl"></a>Connettersi al cluster usando kubectl

Per configurare `kubectl` per la connessione al cluster Kubernetes, usare il comando [az servizio Azure Kubernetes get-credentials][]. L'esempio seguente ottiene le credenziali per il cluster AKS denominato *myAKSCluster* in *myResourceGroup*:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Per verificare la connessione al cluster, usare il comando [kubectl get nodes][kubectl-get] per restituire un elenco di nodi del cluster:

```
$ kubectl get nodes

NAME                                STATUS   ROLES   AGE     VERSION
aks-nodepool1-37463671-vmss000000   Ready    agent   2m37s   v1.18.10
aks-nodepool1-37463671-vmss000001   Ready    agent   2m28s   v1.18.10
```

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato distribuito un cluster Kubernetes in servizio Azure Kubernetes ed è stato configurato `kubectl` per la connessione. Si è appreso come:

> [!div class="checklist"]
> * Distribuire un cluster del servizio Azure Kubernetes in grado di eseguire l'autenticazione in un'istanza di Registro Azure Container
> * Installare l'interfaccia della riga di comando di Kubernetes (kubectl)
> * Configurare kubectl per connettersi al cluster servizio Azure Kubernetes

Passare all'esercitazione successiva per informazioni su come distribuire un'applicazione nel cluster.

> [!div class="nextstepaction"]
> [Distribuire un'applicazione in Kubernetes][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az ad sp create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az acr show]: /cli/azure/acr#az-acr-show
[az role assignment create]: /cli/azure/role/assignment#az-role-assignment-create
[az servizio Azure Kubernetes create]: /cli/azure/aks#az-aks-create
[az servizio Azure Kubernetes install-cli]: /cli/azure/aks#az-aks-install-cli
[az servizio Azure Kubernetes get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-cli-install]: /cli/azure/install-azure-cli
[container-registry-integration]: ./cluster-container-registry-integration.md
[quotas-skus-regions]: quotas-skus-regions.md
