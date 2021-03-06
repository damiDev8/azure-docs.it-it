---
title: Visualizzare le prenotazioni per le risorse di Azure
description: Informazioni su come visualizzare le prenotazioni per Azure nel portale di Azure. Visualizzare le prenotazioni e l'utilizzo tramite le API, PowerShell, l'interfaccia della riga di comando e Power BI.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 12/15/2020
ms.author: banders
ms.openlocfilehash: 8c69f477f363654b8bd707949f0a5b4c46a4e8df
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97561105"
---
# <a name="view-azure-reservations"></a>Visualizzare le prenotazioni di Azure

Questo articolo illustra come visualizzare le prenotazioni di Azure nel portale di Azure. È possibile visualizzare e gestire una prenotazione acquistata nel portale di Azure.

## <a name="who-can-manage-a-reservation-by-default"></a>Chi può gestire una prenotazione per impostazione predefinita

Per impostazione predefinita, le prenotazioni possono essere visualizzate e gestite dagli utenti seguenti:

- L'utente che acquista una prenotazione e l'amministratore account della sottoscrizione di fatturazione usata per acquistare la prenotazione vengono aggiunti all'ordine di prenotazione.
- Amministratori fatturazione con Contratto Enterprise e Contratto del cliente Microsoft.

Per consentire ad altre persone di gestire le prenotazioni sono disponibili due opzioni:

- Delegare la gestione degli accessi per un singolo ordine di prenotazione:
    1. Accedere al [portale di Azure](https://portal.azure.com).
    1. Selezionare **Tutti i servizi** > **Prenotazione** per visualizzare l'elenco delle prenotazioni a cui è possibile accedere.
    1. Selezionare la prenotazione per la quale si desidera delegare l'accesso ad altri utenti.
    1. In Dettagli prenotazione selezionare l'ordine di prenotazione.
    1. Selezionare **Controllo di accesso (IAM)** .
    1. Selezionare **Aggiungi assegnazione di ruolo** > **Ruolo** > **Proprietario**. Se si vuole concedere un accesso limitato, selezionare un ruolo diverso.
    1. Digitare l'indirizzo e-mail dell'utente che si vuole aggiungere come proprietario.
    1. Selezionare l'utente e quindi selezionare **Salva**.

- Aggiungere un utente come amministratore della fatturazione a un Contratto Enterprise o a un Contratto del cliente Microsoft:
    - Per un Contratto Enterprise, aggiungere gli utenti con il ruolo di _Amministratore dell'organizzazione_ per visualizzare e gestire tutti gli ordini di prenotazione applicabili al Contratto Enterprise. Gli utenti con il ruolo _Amministratore dell'organizzazione (sola lettura)_ possono solo visualizzare la prenotazione. Gli amministratori del reparto e i proprietari dell'account non possono visualizzare le prenotazioni _a meno che_ non vengano aggiunti ad esse in modo esplicito tramite Controllo di accesso (IAM). Per altre informazioni, vedere [Gestione dei ruoli Enterprise di Azure](../manage/understand-ea-roles.md).

        _Gli amministratori dell'organizzazione possono assumere la proprietà di un ordine di prenotazione e aggiungere altri utenti a una prenotazione tramite Controllo di accesso (IAM)._
    - Per un Contratto del cliente Microsoft, gli utenti con il ruolo di proprietario del profilo di fatturazione o di collaboratore per il profilo di fatturazione possono gestire tutti gli acquisti di prenotazioni effettuati usando il profilo di fatturazione. Gli utenti con il ruolo di lettore profilo di fatturazione e responsabile fatturazione possono visualizzare tutte le prenotazioni pagate con il profilo di fatturazione. Non possono però apportare modifiche alle prenotazioni.
    Per altre informazioni, vedere [Ruoli e attività del profilo di fatturazione](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="how-billing-administrators-view-or-manage-reservations"></a>Visualizzazione o gestione delle prenotazioni da parte degli amministratori della fatturazione

1. Passare a **Gestione dei costi e fatturazione** e quindi sul lato sinistro della pagina selezionare **Transazioni di prenotazione**.
2. Se si hanno le autorizzazioni necessarie per la fatturazione, è possibile visualizzare e gestire le prenotazioni. Se non è visualizzata alcuna prenotazione, verificare di aver eseguito l'accesso con il tenant di Azure AD in cui sono state create le prenotazioni.

## <a name="view-reservation-and-utilization-in-the-azure-portal"></a>Visualizzare la prenotazione e l'utilizzo nel portale di Azure

Per visualizzare una prenotazione come proprietario o lettore

1. Accedere al [portale di Azure](https://portal.azure.com).
2. Passare a [Prenotazioni](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).
3. L'elenco mostra tutte le prenotazioni per le quali si ha il ruolo di Proprietario o Lettore. Ogni prenotazione mostra l'ultima percentuale di utilizzo nota.
4. Selezionare la percentuale di utilizzo per visualizzare la cronologia di utilizzo e i dettagli. Vedere i dettagli nel video seguente.
   > [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4sYwk] 

## <a name="get-reservations-and-utilization-using-apis-powershell-and-cli"></a>Ottenere i dettagli delle prenotazioni e l'utilizzo con le API, PowerShell e l'interfaccia della riga di comando

Ottenere l'elenco di tutte le prenotazioni usando le risorse seguenti:

- [API: ordine di prenotazione/elenco](/rest/api/reserved-vm-instances/reservationorder/list)
- [PowerShell: ordine di prenotazione/elenco](/powershell/module/azurerm.reservations/get-azurermreservationorder)
- [Interfaccia della riga di comando: ordine di prenotazione/elenco](/cli/azure/reservations/reservation-order#az-reservations-reservation-order-list)

È anche possibile ottenere l'[utilizzo della prenotazione](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) usando l'API Utilizzo istanze riservate. 

## <a name="see-reservations-and-utilization-in-power-bi"></a>Visualizzare le prenotazioni e l'utilizzo in Power BI

Per gli utenti di Power BI sono disponibili due opzioni
- Pacchetto di contenuto: la data di acquisto delle prenotazioni e i dati di utilizzo sono disponibili nel [pacchetto di contenuto Informazioni dettagliate sul consumo di Power BI](/power-bi/desktop-connect-azure-cost-management). Creare i report desiderati usando il pacchetto di contenuto. 
- App Gestione costi: usare l'app [Gestione costi](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp) per ottenere i report predefiniti che è possibile personalizzare ulteriormente.

## <a name="need-help-contact-us"></a>Richiesta di assistenza Contatti

In caso di domande o per assistenza, [creare una richiesta di supporto](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Passaggi successivi

- [Gestire le prenotazioni di Azure](manage-reserved-vm-instance.md).
- [Informazioni sull'utilizzo della prenotazione per la sottoscrizione con pagamento in base al consumo](understand-reserved-instance-usage.md).
- [Informazioni sull'utilizzo della prenotazione per l'iscrizione Enterprise](understand-reserved-instance-usage-ea.md).
- [Informazioni sull'utilizzo della prenotazione per sottoscrizioni CSP](/partner-center/azure-reservations).

