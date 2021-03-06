---
title: Rotazione dei certificati per database di Azure per MySQL
description: Informazioni sulle modifiche imminenti delle modifiche apportate al certificato radice che avranno effetto sul database di Azure per MySQL
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2021
ms.openlocfilehash: a65ac8d52c17a288447193fb8c0fba2c6e6c5554
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201264"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-mysql"></a>Informazioni sulle modifiche apportate alla modifica della CA radice per database di Azure per MySQL

Il database di Azure per MySQL modificherà il certificato radice per l'applicazione client o il driver abilitato con SSL, usato per [connettersi al server di database](concepts-connectivity-architecture.md). Il certificato radice attualmente disponibile è impostato in modo da scadere il 15 febbraio 2021 (02/15/2021) come parte delle procedure consigliate per la manutenzione e la sicurezza standard. In questo articolo vengono fornite ulteriori informazioni sulle modifiche imminenti, sulle risorse interessate e sui passaggi necessari per garantire che l'applicazione mantenga la connettività al server di database.

>[!NOTE]
> In base ai commenti e suggerimenti dei clienti, è stata estesa la deprecazione del certificato radice per la CA radice Baltimore esistente dal 26 ottobre 2020 fino al 15 febbraio 2021. Ci auguriamo che questa estensione fornisca lead time sufficienti per consentire agli utenti di implementare le modifiche apportate al client, se interessate.

> [!NOTE]
> Comunicazione senza distorsione
>
> Microsoft supporta un ambiente diversificato ed inclusivo. Questo articolo contiene riferimenti alle parole _Master_ e _slave_. La [Guida di stile Microsoft per la comunicazione senza distorsione](https://github.com/MicrosoftDocs/microsoft-style-guide/blob/master/styleguide/bias-free-communication.md) li riconosce come parole di esclusione. Le parole vengono usate in questo articolo per coerenza perché sono attualmente parole visualizzate nel software. Quando il software viene aggiornato per rimuovere le parole, questo articolo verrà aggiornato in modo da essere in linea.
>

## <a name="what-update-is-going-to-happen"></a>Quale aggiornamento verrà eseguita?

In alcuni casi, le applicazioni utilizzano un file di certificato locale generato da un file di certificato dell'autorità di certificazione (CA) attendibile per la connessione sicura. Attualmente i clienti possono usare solo il certificato predefinito per connettersi a un database di Azure per il server MySQL, disponibile [qui](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem). Tuttavia, il [Forum del browser dell'autorità di certificazione (CA)](https://cabforum.org/)ha pubblicato di   recente report di più certificati rilasciati dai fornitori di CA come non conformi.

In base ai requisiti di conformità del settore, i fornitori di CA hanno iniziato a revocare i certificati della CA per le CA non conformi, che richiedono che i server usino certificati rilasciati da CA conformi e firmati da certificati della CA da tali CA conformi. Poiché il database di Azure per MySQL usa attualmente uno di questi certificati non conformi, che le applicazioni client usano per convalidare le connessioni SSL, è necessario assicurarsi che vengano eseguite le azioni appropriate (descritte più avanti in questo argomento) per ridurre al minimo il potenziale impatto sui server MySQL.

Il nuovo certificato verrà usato a partire dal 15 febbraio 2021 (02/15/2021). Se si usa la convalida della CA o la convalida completa del certificato del server quando ci si connette da un client MySQL (sslmode = Verify-CA o sslmode = Verify-Full), è necessario aggiornare la configurazione dell'applicazione prima del 15 febbraio 2021 (03/15/2021).

## <a name="how-do-i-know-if-my-database-is-going-to-be-affected"></a>Ricerca per categorie verificare se il database sarà interessato?

Tutte le applicazioni che usano SSL/TLS e verificano che il certificato radice debba aggiornare il certificato radice. È possibile stabilire se le connessioni verificano il certificato radice rivedendo la stringa di connessione.

* Se la stringa di connessione include `sslmode=verify-ca` o `sslmode=verify-identity` , è necessario aggiornare il certificato.
* Se la stringa di connessione include `sslmode=disable` ,, `sslmode=allow` `sslmode=prefer` o `sslmode=require` , non è necessario aggiornare i certificati.
* Se si usano i connettori Java e la stringa di connessione include useSSL = false o requireSSL = false, non è necessario aggiornare i certificati.
* Se la stringa di connessione non specifica sslmode, non è necessario aggiornare i certificati.

Se si usa un client che astrae la stringa di connessione, esaminare la documentazione del client per capire se i certificati sono verificati.
Per informazioni su database di Azure per MySQL sslmode, vedere le [descrizioni della modalità SSL](concepts-ssl-connection-security.md#ssl-default-settings).

Per evitare che la disponibilità dell'applicazione venga interrotta a causa di un arresto imprevisto dei certificati o per aggiornare un certificato revocato, vedere la sezione [**"cosa devo fare per mantenere la connettività"**](concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity) .

## <a name="what-do-i-need-to-do-to-maintain-connectivity"></a>Cosa è necessario fare per mantenere la connettività

Per evitare che la disponibilità dell'applicazione venga interrotta a causa di un arresto imprevisto dei certificati o per l'aggiornamento di un certificato revocato, attenersi alla procedura riportata di seguito. L'idea è creare un nuovo file con *estensione PEM* , che combina il certificato corrente e quello nuovo e durante la convalida del certificato SSL verrà usato uno dei valori consentiti. Vedere i passaggi seguenti:

* Scaricare BaltimoreCyberTrustRoot & DigiCertGlobalRootG2 CA radice dai collegamenti seguenti:

  * [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)
  * [https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem)

* Sono inclusi un archivio certificati CA combinato con certificati **BaltimoreCyberTrustRoot** e **DigiCertGlobalRootG2** .

  * Per gli utenti Java (MySQL Connector/J), eseguire:

    ```console
    keytool -importcert -alias MySQLServerCACert -file D:\BaltimoreCyberTrustRoot.crt.pem -keystore truststore -storepass password -noprompt
    ```

    ```console
    keytool -importcert -alias MySQLServerCACert2 -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password -noprompt
    ```

    Sostituire quindi il file dell'archivio chiavi originale con quello nuovo generato:

    * System. SetProperty ("javax. NET. SSL. trustStore", "path_to_truststore_file");
    * System. SetProperty ("javax. NET. SSL. trustStorePassword", "password");

  * Per gli utenti di .NET (MySQL Connector/NET, MySQLConnector), assicurarsi che **BaltimoreCyberTrustRoot** e **DigiCertGlobalRootG2** siano presenti nell'archivio certificati di Windows, autorità di certificazione radice attendibili. Se non esistono certificati, importare il certificato mancante.

        ![Azure Database for MySQL .net cert](media/overview/netconnecter-cert.png)

  * Per gli utenti .NET in Linux che usano SSL_CERT_DIR, assicurarsi che **BaltimoreCyberTrustRoot** e **DigiCertGlobalRootG2** siano presenti nella directory indicata da SSL_CERT_DIR. Se non esistono certificati, creare il file del certificato mancante.

  * Per altri utenti (client MySQL/MySQL Workbench/C/C++/Go/Python/Ruby/PHP/NodeJS/Perl/Swift), è possibile unire due file di certificato CA nel formato seguente:</b>

     </br>-----INIZIO CERTIFICATO-----  </br>(Radice CA1: BaltimoreCyberTrustRoot. CRT. PEM)  </br>-----CERTIFICATO-----END  </br>-----INIZIO CERTIFICATO-----  </br>(CA2 radice: DigiCertGlobalRootG2. CRT. PEM)  </br>-----CERTIFICATO-----END

* Sostituire il file PEM CA radice originale con il file CA radice combinato e riavviare l'applicazione/client.
* In futuro, dopo la distribuzione del nuovo certificato sul lato server, è possibile modificare il file di CA PEM in DigiCertGlobalRootG2. CRT. pem.

## <a name="what-can-be-the-impact-of-not-updating-the-certificate"></a>Quale può essere l'effetto del mancato aggiornamento del certificato?

Se si usa il certificato rilasciato da database di Azure per MySQL, come descritto in questo articolo, la disponibilità dell'applicazione potrebbe essere interrotta perché il database non sarà raggiungibile. A seconda dell'applicazione, è possibile che vengano visualizzati diversi messaggi di errore, tra cui:

* Certificato/certificato revocato non valido
* Timeout della connessione

> [!NOTE]
> Non eliminare o modificare il **certificato Baltimore** fino a quando non viene apportata la modifica del certificato. Al termine della modifica verrà inviata una comunicazione, dopo la quale è possibile eliminare il certificato Baltimore.

## <a name="frequently-asked-questions"></a>Domande frequenti

### <a name="1-if-im-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. se non si usa SSL/TLS, è ancora necessario aggiornare la CA radice?

  Non sono necessarie azioni se non si usa SSL/TLS.

### <a name="2-if-im-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. Se si usa SSL/TLS, è necessario riavviare il server di database per aggiornare la CA radice?

No, non è necessario riavviare il server di database per iniziare a usare il nuovo certificato. Questo certificato radice è una modifica sul lato client e le connessioni client in ingresso devono usare il nuovo certificato per assicurarsi che possano connettersi al server di database.

### <a name="3-what-will-happen-if-i-dont-update-the-root-certificate-before-february-15-2021-02152021"></a>3. cosa accade se non si aggiorna il certificato radice prima del 15 febbraio 2021 (02/15/2021)?

Se non si aggiorna il certificato radice prima del 15 febbraio 2021 (02/15/2021), le applicazioni che si connettono tramite SSL/TLS e la verifica del certificato radice non saranno in grado di comunicare con il server di database MySQL e l'applicazione subirà problemi di connettività al server di database MySQL.

### <a name="4-what-is-the-impact-if-using-app-service-with-azure-database-for-mysql"></a>4. quali sono le conseguenze dell'uso del servizio app con database di Azure per MySQL?

Per i servizi app di Azure che si connettono al database di Azure per MySQL, esistono due scenari possibili e a seconda di come si usa SSL con l'applicazione.

* Questo nuovo certificato è stato aggiunto al servizio app a livello di piattaforma. Se si usano i certificati SSL inclusi nella piattaforma del servizio app nell'applicazione, non è necessaria alcuna azione.
* Se si include in modo esplicito il percorso del file del certificato SSL nel codice, è necessario scaricare il nuovo certificato e aggiornare il codice per usare il nuovo certificato. Un esempio valido di questo scenario è quando si usano contenitori personalizzati nel servizio app come condivisi nella [documentazione del servizio app](../app-service/tutorial-multi-container-app.md#configure-database-variables-in-wordpress)

### <a name="5-what-is-the-impact-if-using-azure-kubernetes-services-aks-with-azure-database-for-mysql"></a>5. quali sono le conseguenze dell'uso di Azure Kubernetes Services (AKS) con database di Azure per MySQL?

Se si sta provando a connettersi al database di Azure per MySQL usando i servizi di Azure Kubernetes (AKS), è simile all'accesso da un ambiente host dedicato ai clienti. Fare riferimento ai passaggi [qui](../aks/ingress-own-tls.md).

### <a name="6-what-is-the-impact-if-using-azure-data-factory-to-connect-to-azure-database-for-mysql"></a>6. quali sono le conseguenze dell'uso di Azure Data Factory per connettersi a database di Azure per MySQL?

Per un connettore con Azure Integration Runtime, il connettore usa i certificati nell'archivio certificati di Windows nell'ambiente ospitato da Azure. Questi certificati sono già compatibili con i nuovi certificati applicati e pertanto non è necessaria alcuna azione.

Per un connettore che usa Integration Runtime self-hosted in cui si include in modo esplicito il percorso del file del certificato SSL nella stringa di connessione, è necessario scaricare il [nuovo certificato](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) e aggiornare la stringa di connessione per usarlo.

### <a name="7-do-i-need-to-plan-a-database-server-maintenance-downtime-for-this-change"></a>7. è necessario pianificare un tempo di inattività per la manutenzione del server di database per questa modifica?

No. Poiché la modifica è solo sul lato client per la connessione al server di database, non è necessario alcun tempo di inattività di manutenzione per il server di database per questa modifica.

### <a name="8--what-if-i-cannot-get-a-scheduled-downtime-for-this-change-before-february-15-2021-02152021"></a>8. cosa accade se non è possibile ottenere un tempo di inattività pianificato per questa modifica prima del 15 febbraio 2021 (02/15/2021)?

Poiché i client usati per la connessione al server devono aggiornare le informazioni del certificato come descritto [nella sezione correzione](./concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity), in questo caso non è necessario un tempo di inattività per il server.

### <a name="9-if-i-create-a-new-server-after-february-15-2021-02152021-will-i-be-impacted"></a>9. Se si crea un nuovo server dopo il 15 febbraio 2021 (02/15/2021), si avrà un effetto?

Per i server creati dopo il 15 febbraio 2021 (02/15/2021), è possibile usare il certificato appena emesso per le applicazioni per la connessione tramite SSL.

### <a name="10-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>10. con quale frequenza Microsoft aggiorna i certificati o quali sono i criteri di scadenza?

Questi certificati usati da database di Azure per MySQL sono forniti da autorità di certificazione (CA) attendibili. Il supporto di questi certificati nel database di Azure per MySQL è quindi associato al supporto di questi certificati da parte dell'autorità di certificazione. Tuttavia, come in questo caso, in questi certificati predefiniti possono essere presenti bug non previsti, che devono essere corretti al più presto.

### <a name="11-if-im-using-read-replicas-do-i-need-to-perform-this-update-only-on-source-server-or-the-read-replicas"></a>11. Se si usano le repliche di lettura, è necessario eseguire questo aggiornamento solo sul server di origine o sulle repliche di lettura?

Poiché questo aggiornamento è una modifica lato client, se il client utilizzato per leggere i dati dal server di replica, è necessario applicare anche le modifiche per tali client.

### <a name="12-if-im-using-data-in-replication-do-i-need-to-perform-any-action"></a>12. Se si usa la replica dati, è necessario eseguire qualsiasi azione?

Se si usa la [replica dei dati](concepts-data-in-replication.md) per connettersi a database di Azure per MySQL, è necessario prendere in considerazione due aspetti:

* Se la replica dei dati viene eseguita da una macchina virtuale (locale o da una macchina virtuale di Azure) al database di Azure per MySQL, è necessario verificare se SSL è usato per creare la replica. Eseguire **show slave status** e verificare l'impostazione seguente.  

    ```azurecli-interactive
    Master_SSL_Allowed            : Yes
    Master_SSL_CA_File            : ~\azure_mysqlservice.pem
    Master_SSL_CA_Path            :
    Master_SSL_Cert               : ~\azure_mysqlclient_cert.pem
    Master_SSL_Cipher             :
    Master_SSL_Key                : ~\azure_mysqlclient_key.pem
    ```

    Se si nota che il certificato viene fornito per il CA_file, SSL_Cert e SSL_Key, è necessario aggiornare il file aggiungendo il [nuovo certificato](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem).

* Se la replica dei dati è tra due database di Azure per MySQL, sarà necessario reimpostare la replica eseguendo **CALL MySQL.az_replication_change_master** e fornire il nuovo certificato radice duale come ultimo parametro [master_ssl_ca](howto-data-in-replication.md#link-source-and-replica-servers-to-start-data-in-replication)

### <a name="13-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>13. esiste una query sul lato server per verificare se è in uso SSL?

Per verificare se si sta usando la connessione SSL per la connessione al server, fare riferimento alla [verifica SSL](howto-configure-ssl.md#step-4-verify-the-ssl-connection).

### <a name="14-is-there-an-action-needed-if-i-already-have-the-digicertglobalrootg2-in-my-certificate-file"></a>14. esiste un'azione necessaria se si dispone già di DigiCertGlobalRootG2 nel file di certificato?

No. Non è necessario alcun intervento se il file del certificato dispone già del **DigiCertGlobalRootG2**.

###    <a name="15-what-if-i-have-further-questions"></a>15. cosa accade se si hanno altre domande?

In caso di domande, ottenere le risposte dagli esperti della community in [Microsoft Q&a](mailto:AzureDatabaseforMySQL@service.microsoft.com). Se si dispone di un piano di supporto ed è necessario assistenza tecnica, [contattare](mailto:AzureDatabaseforMySQL@service.microsoft.com)Microsoft.
