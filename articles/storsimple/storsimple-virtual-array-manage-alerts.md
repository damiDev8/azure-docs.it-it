---
title: Visualizzare e gestire gli avvisi per l'array virtuale StorSimple
description: Vengono descritte le condizioni di avviso dell'array virtuale StorSimple e la loro gravità. Viene inoltre illustrato come usare il servizio StorSimple Manager per gestire gli avvisi.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 97ee25a1-0ec3-4883-9a0a-54b722598462
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/12/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 12fcc9996697f3bbba35826d79bec238bfb0f8b3
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/25/2020
ms.locfileid: "95993005"
---
# <a name="use-storsimple-device-manager-to-manage-alerts-for-the-storsimple-virtual-array"></a>Usare Gestione dispositivi StorSimple per gestire gli avvisi per l'array virtuale StorSimple

## <a name="overview"></a>Panoramica

La funzione Avvisi del servizio Gestione dispositivi StorSimple offre un modo per esaminare e cancellare in tempo reale gli avvisi correlati all'array virtuale StorSimple. È possibile usare gli avvisi nel pannello di **riepilogo servizio** per monitorare in modo centralizzato i problemi di integrità degli array virtuali StorSimple e la soluzione Microsoft Azure StorSimple complessiva.

Questa esercitazione descrive come configurare le notifiche di avviso, le condizioni di avviso comuni e i livelli di gravità degli avvisi. Descrive inoltre come visualizzare e tenere traccia degli avvisi. Sono inoltre incluse le tabelle di riferimento rapido degli avvisi che consentono di individuare rapidamente uno specifico avviso e rispondere in modo appropriato.

![Pagina degli avvisi](./media/storsimple-virtual-array-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Configurare le impostazioni degli avvisi

È possibile scegliere se ricevere le notifiche delle condizioni di avviso tramite posta elettronica per ogni array virtuale StorSimple. È anche possibile identificare altri destinatari delle notifiche di avviso immettendo i relativi indirizzi di posta elettronica nella casella **Altri destinatari di posta elettronica**, separandoli con un punto e virgola.

> [!NOTE]
> È possibile immettere un massimo di 20 indirizzi di posta elettronica per ogni array virtuale.

Dopo aver attivato la notifica di posta elettronica per un array virtuale, i membri dell'elenco delle notifiche riceveranno un messaggio di posta elettronica ogni volta che si verifica un avviso critico. I messaggi verranno inviati da *StorSimple-Alerts-noreply \@ mail.WindowsAzure.com* e descrivono la condizione di avviso. I destinatari possono fare clic su **Annulla sottoscrizione** per rimuovere il proprio indirizzo dall'elenco di notifica tramite posta elettronica.

#### <a name="to-enable-email-notification-for-alerts"></a>Per abilitare la notifica di posta elettronica degli avvisi

1. Accedere al servizio Gestione dispositivi StorSimple e nella sezione **Gestione** selezionare e fare clic su **Dispositivi**. Nell'elenco dei dispositivi visualizzato selezionare e fare clic sul dispositivo.
   
    ![Impostazione avvisi](./media/storsimple-virtual-array-manage-alerts/alerts2.png)
2. Viene visualizzato il pannello **Impostazioni**. Nella sezione **Impostazioni del dispositivo** selezionare **Generale**. Viene visualizzato il pannello **Impostazioni generali**.
   
    ![Screenshot mostra il riquadro Impostazioni dispositivo con l'area Impostazioni avvisi indicata.](./media/storsimple-virtual-array-manage-alerts/alerts4.png)
3. Nel pannello **Impostazioni generali** passare alla sezione **Impostazioni avvisi** e impostare le opzioni seguenti:
   
   1. Nel campo **Abilita notifica e-mail** selezionare **SÌ**.
   2. Nel campo **Invia messaggio di posta elettronica agli amministratori del servizio** selezionare **SÌ** se si vuole che l'amministratore del servizio e tutti i co-amministratori ricevano le notifiche di avviso.
   3. Nel campo **Altri destinatari di posta elettronica** immettere gli indirizzi di posta elettronica di tutti gli altri destinatari che devono ricevere le notifiche di avviso. Immettere i nomi nel formato *\@ Somewhere.com*. Utilizzare il punto e virgola per separare gli indirizzi di posta elettronica. È possibile configurare un massimo di 20 indirizzi di posta elettronica per ogni dispositivo virtuale.
      
       ![Screenshot mostra i dettagli delle impostazioni degli avvisi con le impostazioni descritte in questo passaggio.](./media/storsimple-virtual-array-manage-alerts/alerts6.png)
   4. Per inviare una notifica di posta elettronica di prova, fare clic su **Invia messaggio di posta elettronica di prova**. Il servizio Gestione dispositivi StorSimple visualizza i messaggi di stato e inoltra la notifica di prova.
      
       ![Screenshot mostra una finestra di dialogo informativa che verifica il messaggio di posta elettronica di prova.](./media/storsimple-virtual-array-manage-alerts/alerts7.png)
      
      > [!NOTE]
      > Se non è possibile inviare il messaggio di notifica test, il servizio Gestione dispositivi StorSimple visualizza un messaggio appropriato. Fare clic su **OK**, attendere alcuni minuti e provare a inviare nuovamente il messaggio di notifica di prova.
      >
      >
   5. Fare clic su **Salva** nella parte inferiore della pagina per salvare la configurazione. Quando viene richiesta la conferma, fare clic su **Sì**.
      
      ![Screenshot mostra il riquadro impostazioni con il pulsante Salva selezionato.](./media/storsimple-virtual-array-manage-alerts/alerts10.png)

## <a name="common-alert-conditions"></a>Condizioni di avviso comuni

L'array virtuale StorSimple genera avvisi in risposta a una varietà di condizioni. Di seguito sono indicati i tipi più comuni delle condizioni di avviso:

* **Problemi di connettività** : questi avvisi vengono generati in caso di difficoltà di trasferimento dei dati. Possono verificarsi problemi di comunicazione durante il trasferimento dei dati da un account di archiviazione di Azure e viceversa oppure a causa della mancanza di connettività tra i dispositivi virtuali e il servizio Gestione dispositivi StorSimple. I problemi di comunicazione sono tra i più difficili da risolvere poiché sono presenti numerosi punti di errore. Verificare sempre che la connettività di rete e l'accesso a Internet siano disponibili prima di proseguire con la risoluzione dei problemi più avanzati. Per altre informazioni sulle impostazioni delle porte e del firewall, vedere [Requisiti di sistema dell'array virtuale StorSimple (anteprima)](storsimple-ova-system-requirements.md). Per informazioni sulla risoluzione dei problemi, vedere [Risoluzione dei problemi con il cmdlet Test-Connection](./storsimple-8000-troubleshoot-deployment.md).
* **Problemi di prestazioni**: tali avvisi vengono generati quando le prestazioni del sistema non sono ottimali, ad esempio quando il carico di lavoro è pesante.

Inoltre, possono essere generati avvisi relativi a sicurezza, aggiornamenti o errori di processi.

## <a name="alert-severity-levels"></a>Livelli di gravità dell'avviso

Gli avvisi possono avere diversi livelli di gravità, in base all'impatto determinato dalla situazione di avviso e alla necessità di una risposta all'avviso. I livelli di gravità sono:

* **Critico**: avviso in risposta a una condizione che influenza le prestazioni del sistema. È richiesta un'azione per garantire che il servizio StorSimple non venga interrotto.
* **Avviso**: questa condizione potrebbe diventare critica se non viene risolta. È necessario analizzare la situazione ed eseguire qualsiasi azione necessaria per eliminare il problema.
* **Informazione**: questo avviso contiene informazioni che possono essere utili per monitorare e gestire il sistema.

## <a name="view-and-track-alerts"></a>Visualizzare e tenere traccia degli avvisi

Il pannello di riepilogo servizio di Gestione dispositivi StorSimple offre un rapido riepilogo del numero di avvisi sui dispositivi virtuali, organizzati per livello di gravità.

![Dashboard degli avvisi](./media/storsimple-virtual-array-manage-alerts/alerts14.png)

Fare clic sul livello di gravità per aprire il pannello **Avvisi**. I risultati includono solo gli avvisi che corrispondono al livello di gravità scelto.

![Report avvisi in base al tipo di avviso](./media/storsimple-virtual-array-manage-alerts/alerts15.png)

Fare clic su un avviso nell'elenco per ottenere altri dettagli sull'avviso, inclusi l'ora di segnalazione dell'ultimo avviso, il numero di occorrenze dell'avviso sul dispositivo e l'azione consigliata per risolvere l'avviso.

![Elenco degli avvisi e dettagli](./media/storsimple-virtual-array-manage-alerts/alerts16.png)

Se è necessario inviare le informazioni al supporto tecnico Microsoft, è possibile copiare i dettagli dell'avviso in un file di testo. Dopo avere seguito i suggerimenti e risolto la condizione di avviso in locale, è necessario cancellare l'avviso dall'elenco. Selezionare l'avviso nell'elenco e fare clic su **Cancella**. Per cancellare più avvisi, fare clic su qualsiasi colonna tranne **Avviso**, selezionare tutti gli avvisi da cancellare e fare clic su **Cancella**.

Quando si fa clic su **Cancella**, è possibile fornire commenti sull'avviso e sui passaggi eseguiti per risolvere l'avviso.

![commenti degli avvisi](./media/storsimple-virtual-array-manage-alerts/alerts17.png)

Alcuni eventi verranno cancellati dal sistema se un altro evento viene attivato con nuove informazioni.

## <a name="sort-and-review-alerts"></a>Ordinare e rivedere gli avvisi

Nel pannello **Avvisi** può essere visualizzato un massimo di 250 avvisi. Se è stato superato il numero di avvisi, non tutti gli avvisi verranno inclusi nella visualizzazione predefinita. È possibile combinare i campi seguenti per personalizzare quali avvisi vengono visualizzati:

* **Stato**: è possibile visualizzare gli avvisi attivi o cancellati. Gli avvisi attivi vengono ancora attivati nel sistema, mentre gli avvisi deselezionati sono stati cancellati manualmente da un amministratore o a livello di programmazione, in quanto la condizione di avviso è stata aggiornata dal sistema con nuove informazioni.
* **Gravità**: è possibile visualizzare avvisi di tutti i livelli di gravità (critico, avviso, informazione) o solo di un livello di gravità specifico, ad esempio solo avvisi critici.
* **Origine**: è possibile visualizzare gli avvisi generati da tutte le origini. In alternativa, è possibile visualizzare soltanto quelli che provengono dal servizio oppure da uno o tutti i dispositivi virtuali.
* **Intervallo di tempo**: se si specificano le date di inizio e di fine e i timestamp, è possibile cercare gli avvisi durante il periodo di tempo desiderato.

## <a name="alerts-quick-reference"></a>Riferimento rapido degli avvisi

Nelle tabelle seguenti sono elencati alcuni avvisi di StorSimple che potrebbero essere visualizzati e le raccomandazioni e le informazioni aggiuntive eventualmente disponibili. Gli avvisi relativi all'array virtuale StorSimple rientrano in una delle categorie seguenti:

* [Avvisi di connettività cloud](#cloud-connectivity-alerts)
* [Avvisi di configurazione](#configuration-alerts)
* [Avvisi di errore di processo](#job-failure-alerts)
* [Avvisi di prestazioni](#performance-alerts)
* [Avvisi di sicurezza](#security-alerts)

### <a name="cloud-connectivity-alerts"></a>Avvisi di connettività cloud

| Testo dell'avviso | Evento | Ulteriori informazioni/Azioni consigliate |
|:--- |:--- |:--- |
| Il dispositivo <*nome dispositivo*> non è connesso al cloud. |Il dispositivo denominato non può connettersi al cloud. |Impossibile connettersi al cloud. L'inconveniente potrebbe essere causato da uno dei motivi seguenti:<ul><li>Potrebbe essersi verificato un problema con le impostazioni di rete sul dispositivo.</li><li>Potrebbe essersi verificato un problema con le credenziali dell'account di archiviazione.</li></ul>Per altre informazioni sulle risoluzione dei problemi di connettività, vedere l' [interfaccia utente Web locale](storsimple-ova-web-ui-admin.md) del dispositivo. |

### <a name="configuration-alerts"></a>Avvisi di configurazione

| Testo dell'avviso | Evento | Ulteriori informazioni/Azioni consigliate |
|:--- |:--- |:--- |
| Configurazione del servizio virtuale locale non supportata. |Rallentamento delle prestazioni. |La configurazione corrente può influire negativamente sulle prestazioni. Assicurarsi che il server soddisfi i requisiti minimi di configurazione. Per altre informazioni, vedere [Requisiti di sistema dell'array virtuale StorSimple](storsimple-ova-system-requirements.md). |
| Si sta esaurendo lo spazio su disco di cui è stato effettuato il provisioning in <*nome dispositivo* \> . |Avviso relativo allo spazio su disco. |Sta per esaurirsi lo spazio su disco con provisioning. Per liberare spazio, provare a spostare i carichi di lavoro su un altro volume oppure a condividere o eliminare dei dati. |

### <a name="job-failure-alerts"></a>Avvisi di errore di processo

| Testo dell'avviso | Evento | Ulteriori informazioni/Azioni consigliate |
|:--- |:--- |:--- |
| Non è stato possibile completare il backup del nome del *dispositivo* <\> . |Il processo di backup non è riuscito. |Non è stato possibile creare un backup. Considerare uno degli aspetti seguenti:<ul><li>Alcuni problemi di connettività potrebbero impedire il corretto completamento dell'operazione di backup. Assicurarsi che non siano presenti problemi di connettività. Per altre informazioni sulle risoluzione dei problemi di connettività, vedere l'[interfaccia utente Web locale](storsimple-ova-web-ui-admin.md) del dispositivo virtuale.</li><li>È stato raggiunto il limite di archiviazione disponibile. Provare a eliminare i backup che non sono più necessari per liberare spazio.</li></ul>  Risolvere i problemi, cancellare l'avviso e ripetere l'operazione. |
| Non è stato possibile completare il clone del nome del *dispositivo* <\> . |Il processo di clonazione non è riuscito. |Impossibile creare un clone. Considerare uno degli aspetti seguenti:<ul><li>L'elenco di backup potrebbe non essere valido. Aggiornare l'elenco per verificare che sia ancora valido.</li><li>I problemi di connettività potrebbero impedire il completamento dell'operazione di clonazione. Assicurarsi che non siano presenti problemi di connettività.</li><li>È stato raggiunto il limite di archiviazione disponibile. Provare a eliminare i backup che non sono più necessari per liberare spazio.</li></ul> Risolvere i problemi, cancellare l'avviso e ripetere l'operazione. |

### <a name="networking-alerts"></a>Avvisi di rete

| Testo dell'avviso | Evento | Ulteriori informazioni/Azioni consigliate |
|:--- |:--- |:--- |
| Impossibile connettersi al servizio di autenticazione. |Errore di percorso dati |L'URL usato per l'autenticazione non è raggiungibile. Verificare che nelle regole del firewall siano inclusi i modelli di URL specificati per il dispositivo StorSimple. Per altre informazioni sui modelli di URL nel portale di Azure, vedere [Requisiti di sistema dell'array virtuale StorSimple](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules).|

### <a name="performance-alerts"></a>Avvisi di prestazioni

| Testo dell'avviso | Evento | Ulteriori informazioni/Azioni consigliate |
|:--- |:--- |:--- |
| Si stanno verificando ritardi imprevisti nel trasferimento dei dati. |Trasferimento dati lento. |Gli errori di limitazione si verificano quando si superano gli obiettivi di scalabilità di un servizio di archiviazione. In questo modo il servizio di archiviazione assicura che nessun client o tenant possa utilizzare il servizio a spese di altri. Per altre informazioni sulla risoluzione dei problemi relativi all'account di archiviazione di Azure, vedere [Monitorare, diagnosticare e risolvere i problemi dell'Archiviazione di Microsoft Azure](../storage/common/storage-monitoring-diagnosing-troubleshooting.md). |
| Lo spazio su disco di prenotazione locale è insufficiente sul *nome <dispositivo* \> . |Tempo di risposta lento. |il 10% delle dimensioni totali di cui è stato effettuato il provisioning per <*nome dispositivo* \> è riservato sul dispositivo locale e lo spazio riservato è in esaurimento. Il carico di lavoro sul *nome del dispositivo* <\> genera una percentuale di varianza più elevata oppure è possibile che sia stata eseguita di recente la migrazione di una grande quantità di dati. Questo può comportare una riduzione delle prestazioni. Per risolvere questo problema, provare a eseguire una delle azioni seguenti:<ul><li>Aumentare la larghezza di banda cloud sul dispositivo.</li><li>Ridurre o spostare i carichi di lavoro in un altro volume o condivisione.</li></ul> |

### <a name="security-alerts"></a>Avvisi di sicurezza

| Testo dell'avviso | Evento | Ulteriori informazioni/Azioni consigliate |
|:--- |:--- |:--- |
| La password per <*nome dispositivo* \> scadrà tra <*numero* \> giorni. |Avviso relativo alla password. |La password scadrà tra <*numero* di \> giorni. Provare a modificare la password. Per altre informazioni, vedere [Modificare la password amministratore del dispositivo array virtuale StorSimple](storsimple-virtual-array-change-device-admin-password.md). |

## <a name="next-steps"></a>Passaggi successivi

* [Informazioni sull'array virtuale StorSimple](storsimple-ova-overview.md).