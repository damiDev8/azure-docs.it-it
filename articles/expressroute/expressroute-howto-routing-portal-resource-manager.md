---
title: 'Esercitazione: Configurare il peering per un circuito ExpressRoute - Portale di Azure'
description: Questa esercitazione illustra come creare ed effettuare il provisioning di un peering privato di ExpressRoute e un peering Microsoft usando il portale di Azure.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: tutorial
ms.date: 01/11/2021
ms.author: duau
ms.openlocfilehash: f780c8c2f932b612ee42e13906f72983b324eefd
ms.sourcegitcommit: 48e5379c373f8bd98bc6de439482248cd07ae883
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2021
ms.locfileid: "98108535"
---
# <a name="tutorial-create-and-modify-peering-for-an-expressroute-circuit-using-the-azure-portal"></a>Esercitazione: Creare e modificare il peering per un circuito ExpressRoute usando il portale di Azure

Questa esercitazione illustra come creare e gestire la configurazione del routing per un circuito ExpressRoute di Azure Resource Manager usando il portale di Azure. La procedura seguente mostra anche come controllare lo stato e aggiornare, eliminare o effettuare il deprovisioning dei peering per un circuito ExpressRoute. Se si vuole usare un metodo diverso per eseguire operazioni nel circuito, selezionare l'articolo appropriato nell'elenco seguente:

> [!div class="op_single_selector"]
> * [Portale di Azure](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Interfaccia della riga di comando di Azure](howto-routing-cli.md)
> * [Peering pubblico](about-public-peering.md)
> * [Video - Peering privato](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - Peering Microsoft](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (classic)](expressroute-howto-routing-classic.md) (PowerShell (classico))
> 

È possibile configurare il peering privato e il peering Microsoft per un circuito ExpressRoute (il peering pubblico di Azure è deprecato per i nuovi circuiti). I peering possono essere configurati nell'ordine che si preferisce. assicurandosi, tuttavia, di completare la configurazione di un peering per volta. Per altre informazioni sul routing dei domini e il peering , vedere [Domini di routing ExpressRoute](expressroute-circuit-peerings.md). Per informazioni sul peering pubblico, vedere [Creare e gestire il peering pubblico di ExpressRoute](about-public-peering.md).

In questa esercitazione verranno illustrate le procedure per:
> [!div class="checklist"]
> - Configurare, aggiornare ed eliminare il peering Microsoft per un circuito
> - Configurare, aggiornare ed eliminare il peering privato di Azure per un circuito

## <a name="prerequisites"></a>Prerequisiti

* Prima di iniziare la configurazione, rivedere le pagine seguenti:
    * [Prerequisiti](expressroute-prerequisites.md) 
    * [Requisiti di routing](expressroute-routing.md)
    * [Flussi di lavoro](expressroute-workflows.md)
* È necessario avere un circuito ExpressRoute attivo. Seguire le istruzioni per [creare un circuito ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) e chiedere al provider di connettività di abilitarlo prima di continuare. Per configurare i peering, è necessario che sia stato effettuato il provisioning del circuito ExpressRoute e che il circuito sia in stato abilitato. 
* Se si prevede di usare una chiave condivisa o un hash MD5, assicurarsi di usare la chiave su entrambi i lati del tunnel. Il limite massimo è di 25 caratteri alfanumerici. I caratteri speciali non sono supportati. 

Queste istruzioni si applicano solo ai circuiti creati con provider di servizi che offrono servizi di connettività di livello 2. Se si usa un provider di servizi che offre servizi gestiti di livello 3 (in genere un IPVPN, come MPLS), il provider di connettività configura e gestisce il routing per conto dell'utente. 

> [!IMPORTANT]
> Attualmente non vengono annunciati peering configurati da provider di servizi tramite il portale di gestione del servizio. L'abilitazione di questa funzionalità sarà presto disponibile. Rivolgersi al provider di servizi prima di configurare peering BGP.
> 

## <a name="microsoft-peering"></a><a name="msft"></a>Peering Microsoft

Questa sezione consente di creare, ottenere, aggiornare ed eliminare la configurazione del peering Microsoft per un circuito ExpressRoute.

> [!IMPORTANT]
> Per i circuiti ExpressRoute configurati prima del 1 agosto 2017 tutti i prefissi di servizio saranno annunciati tramite il peering Microsoft, anche se non sono definiti i filtri di route. Il peering Microsoft dei circuiti ExpressRoute che vengono configurati dopo il 1° agosto 2017 non avrà alcun prefisso annunciato fino a quando non viene associato un filtro di route per il circuito. Per altre informazioni, vedere [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md) (Configurare un filtro di route per il peering Microsoft).
> 
> 

### <a name="to-create-microsoft-peering"></a>Per creare il peering Microsoft

1. Configurare il circuito ExpressRoute. Prima di continuare, controllare lo stato di **Stato provider** per assicurarsi che il provider di connettività abbia effettuato il provisioning completo del circuito.

   Se il provider di connettività offre servizi gestiti di livello 3, è possibile chiedere al provider di abilitare il peering Microsoft per l'utente. Non sarà necessario seguire le istruzioni riportate nelle sezioni seguenti. Se invece il provider di connettività non gestisce il routing per conto dell'utente, dopo aver creato il circuito continuare con questa procedura.

   **Circuito - Stato provider: Senza provisioning**

    :::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/not-provisioned.png" alt-text="Screenshot che mostra la pagina di panoramica del circuito demo ExpressRoute con un riquadro rosso che evidenzia l'opzione Stato provider impostata su Senza provisioning":::

   **Circuito - Stato provider: Provisioning eseguito**

    :::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/provisioned.png" alt-text="Screenshot che mostra la pagina di panoramica del circuito demo ExpressRoute con un riquadro rosso che evidenzia l'opzione Stato provider impostata su Provisioning eseguito":::

2. Configurare il peering Microsoft per il circuito. Prima di continuare, verificare di avere le informazioni seguenti.

   * Una coppia di subnet /30 di proprietà dell'utente e registrata in un registro RIR/IRR. Devono essere prefissi IPv4 pubblici validi. Una subnet verrà usata per il collegamento primario e l'altra per il collegamento secondario. Da ognuna di queste subnet si assegnerà al router il primo indirizzo IP utilizzabile, poiché il secondo indirizzo IP utilizzabile per il router viene usato da Microsoft.
   * Un ID VLAN valido su cui stabilire questo peering. Assicurarsi che nessun altro peering nel circuito usi lo stesso ID VLAN. Il collegamento primario e quello secondario devono usare lo stesso ID VLAN.
   * Numero AS per il peering. È possibile usare numeri AS a 2 e a 4 byte.
   * Prefissi annunciati: fornire un elenco di tutti i prefissi che si intende annunciare nella sessione BGP. Sono accettati solo prefissi di indirizzi IP pubblici. Se si intende inviare un set di prefissi, è possibile creare un elenco delimitato da virgole. Questi prefissi devono essere intestati all'utente in un registro RIR o IRR.
   * **Facoltativo -** ASN cliente: se si annunciano prefissi non registrati con l'ASN del peering, è possibile specificare il numero ASN con cui sono registrati.
   * Routing Registry Name: è possibile specificare il registro RIR/IRR in cui sono registrati il numero AS e i prefissi.
   * **Facoltativo:** un hash MD5, se si sceglie di usarne uno.
3. È possibile selezionare il peering che si vuole configurare, come illustrato nell'esempio seguente. Selezionare la riga del peering Microsoft.

   :::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/select-microsoft-peering.png" alt-text="Selezionare la riga del peering Microsoft":::

4. Configurare il peering Microsoft. **Salvare** la configurazione dopo aver specificato tutti i parametri. L'immagine seguente mostra una configurazione di esempio:

   :::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/configuration-m-validation-needed.png" alt-text="Configurare la convalida del peering Microsoft necessaria":::

> [!IMPORTANT]
> Microsoft verifica se all'utente sono assegnati "Prefissi pubblici annunciati" e "ASN peer" (o "ASN cliente") nel registro di sistema di routing Internet. Se si ricevono prefissi pubblici da un'altra entità e l'assegnazione non viene registrata con il registro di sistema di routing, la convalida automatica non verrà completata e sarà richiesta la convalida manuale. Se la convalida automatica ha esito negativo, verrà visualizzato il messaggio "Convalida necessaria". 
>
> Se viene visualizzato il messaggio 'Convalida necessaria', raccogliere i documenti che mostrano che i prefissi pubblici sono assegnati all'organizzazione dall'entità elencata come proprietaria dei prefissi nel registro di routing, quindi inviarli per la convalida manuale aprendo un ticket di supporto. 
>

   Se il circuito raggiunge lo stato 'Convalida necessaria', si dovrà aprire un ticket di supporto per fornire la prova della proprietà dei prefissi al team di supporto. È possibile aprire un ticket di supporto direttamente dal portale, come mostrato nell'esempio seguente:

   :::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/ticket-portal-m.png" alt-text="Convalida necessaria - ticket di supporto":::

5. Dopo che la configurazione è stata accettata, si aprirà una schermata simile all'immagine seguente:

   :::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/microsoft-peering-validation-configured.png" alt-text="Stato peering: Configurato":::

### <a name="to-view-microsoft-peering-details"></a><a name="getmsft"></a>Per visualizzare i dettagli del peering Microsoft

È possibile visualizzare le proprietà del peering Microsoft selezionando la riga corrispondente.

:::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/view-peering-m.png" alt-text="Visualizzare le proprietà del peering Microsoft":::

### <a name="to-update-microsoft-peering-configuration"></a><a name="updatemsft"></a>Per aggiornare la configurazione del peering Microsoft

È possibile selezionare la riga relativa al peering da modificare, quindi modificare le proprietà del peering e salvare le modifiche.

:::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/configuration-m.png" alt-text="Selezionare la riga del peering":::

## <a name="azure-private-peering"></a><a name="private"></a>Peering privato di Azure

Questa sezione fornisce le istruzioni per creare, ottenere, aggiornare ed eliminare la configurazione del peering privato di Azure per un circuito ExpressRoute.

### <a name="to-create-azure-private-peering"></a>Per creare un peering privato di Azure

1. Configurare il circuito ExpressRoute. Prima di continuare, assicurarsi che il provider di connettività abbia effettuato il provisioning completo del circuito. 

   Se il provider di connettività offre servizi gestiti di livello 3, è possibile chiedere al provider di connettività di abilitare il peering privato di Azure per l'utente. Non sarà necessario seguire le istruzioni riportate nelle sezioni seguenti. Se invece il provider di connettività non gestisce il routing per conto dell'utente, dopo aver creato il circuito continuare con la procedura seguente.

   **Circuito - Stato provider: Senza provisioning**

   :::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-private.png" alt-text="Screenshot che mostra la pagina di panoramica del circuito demo ExpressRoute con un riquadro rosso che evidenzia l'opzione Stato provider impostata su Senza provisioning":::

   **Circuito - Stato provider: Provisioning eseguito**

   :::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/provisioned-private-peering.png" alt-text="Screenshot che mostra la pagina di panoramica del circuito demo ExpressRoute con un riquadro rosso che evidenzia l'opzione Stato provider impostata su Provisioning eseguito":::

2. Configurare il peering privato di Azure per il circuito. Prima di continuare con i passaggi successivi, verificare di avere gli elementi seguenti:

   * Una coppia di subnet /30 di proprietà dell'utente. Una subnet verrà usata per il collegamento primario e l'altra per il collegamento secondario. Da ognuna di queste subnet si assegnerà al router il primo indirizzo IP utilizzabile, poiché il secondo indirizzo IP utilizzabile per il router viene usato da Microsoft.
   * Un ID VLAN valido su cui stabilire questo peering. Assicurarsi che nessun altro peering nel circuito usi lo stesso ID VLAN. Il collegamento primario e quello secondario devono usare lo stesso ID VLAN.
   * Numero AS per il peering. È possibile usare numeri AS a 2 e a 4 byte. È possibile usare un numero AS privato per questo peering, ad eccezione dell'intervallo da 65515 a 65520, inclusi questi numeri.
   * Quando si configura il peering privato, è necessario annunciare le route dal router perimetrale locale ad Azure tramite BGP.
   * **Facoltativo:** un hash MD5, se si sceglie di usarne uno.
3. Selezionare la riga corrispondente al peering privato di Azure, come illustrato nell'esempio seguente:

   :::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/select-private-peering.png" alt-text="Selezionare la riga del peering privato":::

4. Configurare il peering privato. **Salvare** la configurazione dopo aver specificato tutti i parametri.

   :::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/private-peering-configuration.png" alt-text="Configurare il peering privato":::

5. Dopo che la configurazione è stata accettata, viene visualizzata una schermata simile all'esempio seguente:

   :::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/save-private-peering.png" alt-text="Peering privato salvato":::

### <a name="to-view-azure-private-peering-details"></a><a name="getprivate"></a>Per visualizzare i dettagli relativi al peering privato di Azure

È possibile visualizzare le proprietà del peering privato di Azure selezionandolo.

:::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/view-peering-m.png" alt-text="Visualizzare le proprietà del peering privato":::

### <a name="to-update-azure-private-peering-configuration"></a><a name="updateprivate"></a>Per aggiornare la configurazione del peering privato di Azure

È possibile selezionare la riga per il peering e modificare le relative proprietà. Al termine dell'aggiornamento, salvare le modifiche.

:::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/update-private-peering.png" alt-text="Aggiornare il peering privato":::

## <a name="clean-up-resources"></a>Pulire le risorse

### <a name="to-delete-microsoft-peering"></a><a name="deletemsft"></a>Per eliminare il peering Microsoft

Per rimuovere la configurazione del peering Microsoft, fare clic con il pulsante destro del mouse sul peering e scegliere **Elimina**, come illustrato nell'immagine seguente:

:::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/delete-microsoft-peering.png" alt-text="Eliminare il peering Microsoft":::

### <a name="to-delete-azure-private-peering"></a><a name="deleteprivate"></a>Per eliminare un peering privato di Azure

Per rimuovere la configurazione del peering privato, fare clic con il pulsante destro del mouse sul peering e scegliere **Elimina**, come illustrato nell'immagine seguente:

> [!WARNING]
> Prima di eseguire questa operazione, verificare che tutte le reti virtuali e le connessioni di Copertura globale ExpressRoute siano state rimosse. 
> 

:::image type="content" source="./media/expressroute-howto-routing-portal-resource-manager/delete-private-peering.png" alt-text="Eliminare il peering privato":::

## <a name="next-steps"></a>Passaggi successivi

Dopo la configurazione del peering privato di Azure, è possibile creare un gateway ExpressRoute per collegare una rete virtuale al circuito. 

> [!div class="nextstepaction"]
> [Configurare un gateway di rete virtuale per ExpressRoute](expressroute-howto-add-gateway-resource-manager.md)
