---
title: Configurazione TLS-portale di Azure-database di Azure per MySQL
description: Informazioni su come impostare la configurazione di TLS usando portale di Azure per il database di Azure per MySQL
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 06/02/2020
ms.openlocfilehash: 290752c0e577e6c2cd58d83f77fea8a5406388e4
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2020
ms.locfileid: "93240631"
---
# <a name="configuring-tls-settings-in-azure-database-for-mysql-using-azure-portal"></a>Configurazione delle impostazioni TLS nel database di Azure per MySQL con portale di Azure

Questo articolo descrive come configurare un database di Azure per il server MySQL per applicare la versione minima di TLS consentita per le connessioni e negare tutte le connessioni con una versione di TLS inferiore rispetto alla versione minima configurata di TLS, migliorando così la sicurezza della rete.

È possibile applicare la versione TLS per la connessione al database di Azure per MySQL. I clienti hanno ora la possibilità di impostare la versione minima di TLS per il server di database. Se ad esempio si imposta la versione minima di TLS su 1,0, sarà necessario consentire ai client di connettersi tramite TLS 1.0, 1.1 e 1,2. In alternativa, se si imposta questa opzione su 1,2 si consente solo ai client che si connettono usando TLS 1.2 + e tutte le connessioni in ingresso con TLS 1,0 e TLS 1,1 verranno rifiutate.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa guida, è necessario:

* Un [database di Azure per MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-mysql"></a>Impostare le configurazioni TLS per database di Azure per MySQL

Per impostare la versione minima di TLS del server MySQL, seguire questa procedura:

1. Nella [portale di Azure](https://portal.azure.com/)selezionare il database di Azure per il server MySQL esistente.

1. Nella pagina server MySQL, in **Impostazioni** , fare clic su **sicurezza connessione** per aprire la pagina Configurazione sicurezza connessione.

1. Nella **versione minima di TLS** selezionare **1,2** per negare le connessioni con la versione tls inferiore a TLS 1,2 per il server MySQL.

    :::image type="content" source="./media/howto-tls-configurations/setting-tls-value.png" alt-text="Configurazione TLS per database di Azure per MySQL":::

1. Fare clic su **Salva** per salvare le modifiche.

1. Una notifica conferma che l'impostazione di sicurezza della connessione è stata abilitata correttamente.

    :::image type="content" source="./media/howto-tls-configurations/setting-tls-value-success.png" alt-text="Configurazione TLS per database di Azure per MySQL":::

## <a name="next-steps"></a>Passaggi successivi

- Informazioni su [come creare avvisi](howto-alert-on-metric.md) per le metriche