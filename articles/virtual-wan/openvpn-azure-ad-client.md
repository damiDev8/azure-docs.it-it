---
title: 'Gateway VPN: client VPN per le connessioni P2S del protocollo OpenVPN: autenticazione Azure AD'
description: Informazioni su come usare la VPN P2S per connettersi alla VNet usando l'autenticazione Azure AD.
services: vpn-gateway
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: 8e97a2f077efd4d00eec4a91645dc1b65057ebd9
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2020
ms.locfileid: "94565008"
---
# <a name="configure-a-vpn-client-for-p2s-openvpn-protocol-connections-azure-ad-authentication"></a>Configurare un client VPN per le connessioni del protocollo OpenVPN da punto a sito: Autenticazione di Azure AD

Questo articolo illustra come configurare un client VPN per connettersi a una rete virtuale usando la VPN da punto a sito e l'autenticazione Azure Active Directory. Prima di poter connettersi ed eseguire l'autenticazione con Azure AD, è necessario innanzitutto configurare il tenant di Azure AD. Per altre informazioni, vedere [configurare un tenant di Azure ad](openvpn-azure-ad-tenant.md).

> [!NOTE]
> L'autenticazione di Azure AD è supportata solo per le connessioni tramite il protocollo OpenVPN®.
>

## <a name="working-with-client-profiles"></a><a name="profile"></a>Utilizzo dei profili client

Per connettersi, è necessario scaricare il client VPN di Azure e configurare un profilo client VPN in ogni computer che desidera connettersi a VNet. È possibile creare un profilo client in un computer, esportarlo e quindi importarlo in altri computer.

### <a name="to-download-the-azure-vpn-client"></a>Per scaricare il client VPN di Azure

Usare questo [collegamento](https://go.microsoft.com/fwlink/?linkid=2117554) per scaricare il client VPN di Azure. Verificare che il client VPN di Azure disponga delle autorizzazioni per l'esecuzione in background. Per selezionare o abilitare l'autorizzazione, attenersi alla procedura seguente:

1. Passare a Start, quindi selezionare impostazioni > privacy > app in background.
2. In app in background verificare che **le app eseguite in background** siano attivate.
3. In scegliere le app che possono essere eseguite in background, attivare le impostazioni per il client VPN di Azure **su on**.

  ![autorizzazione](./media/openvpn-azure-ad-client/backgroundpermission.png)

### <a name="to-create-a-certificate-based-client-profile"></a><a name="cert"></a>Per creare un profilo client basato su certificati

Quando si utilizza un profilo basato su certificato, verificare che nel computer client siano installati i certificati appropriati. Per ulteriori informazioni sui certificati, vedere [Install Client Certificates](certificates-point-to-site.md).

  ![cert](./media/openvpn-azure-ad-client/create/create-cert1.jpg)

### <a name="to-create-a-radius-client-profile"></a><a name="radius"></a>Per creare un profilo client RADIUS

  ![radius](./media/openvpn-azure-ad-client/create/create-radius1.jpg)
  
> [!NOTE]
> Il segreto server può essere esportato nel profilo client VPN P2S.  Le istruzioni su come esportare un profilo client sono disponibili [qui](about-vpn-profile-download.md).
>

### <a name="to-export-and-distribute-a-client-profile"></a><a name="export"></a>Per esportare e distribuire un profilo client

Quando si dispone di un profilo di lavoro ed è necessario distribuirlo ad altri utenti, è possibile esportarlo attenendosi alla procedura seguente:

1. Evidenziare il profilo client VPN che si vuole esportare, selezionare il **...** e quindi selezionare **Esporta**.

    ![Screenshot mostra l'opzione Esporta selezionato dal menu.](./media/openvpn-azure-ad-client/export/export1.jpg)

2. Selezionare il percorso in cui si desidera salvare il profilo, lasciare il nome del file così com'è, quindi selezionare **Salva** per salvare il file XML.

    ![Screenshot mostra una finestra di dialogo Salva con nome in cui è possibile immettere un nome di file.](./media/openvpn-azure-ad-client/export/export2.jpg)

### <a name="to-import-a-client-profile"></a><a name="import"></a>Per importare un profilo client

1. Nella pagina selezionare **Importa**.

    ![Screenshot mostra l'importazione selezionata dal menu più.](./media/openvpn-azure-ad-client/import/import1.jpg)

2. Individuare il file XML del profilo e selezionarlo. Con il file selezionato, selezionare **Apri**.

    ![Screenshot mostra una finestra di dialogo aperta in cui è possibile selezionare un file.](./media/openvpn-azure-ad-client/import/import2.jpg)

3. Specificare il nome del profilo e selezionare **Salva**.

    ![Screenshot mostra il nome della connessione aggiunto e il pulsante Salva selezionato.](./media/openvpn-azure-ad-client/import/import3.jpg)

4. Selezionare **Connetti** per connettersi alla VPN.

    ![Screenshot mostra il pulsante Connetti per la connessione appena creata.](./media/openvpn-azure-ad-client/import/import4.jpg)

5. Una volta stabilita la connessione, l'icona diventerà verde mostrerà lo stato **Connesso**.

    ![Screenshot mostra la connessione in uno stato connesso con l'opzione per la disconnessione.](./media/openvpn-azure-ad-client/import/import5.jpg)

### <a name="to-delete-a-client-profile"></a><a name="delete"></a>Per eliminare un profilo client

1. Selezionare i puntini di sospensione accanto al profilo client che si desidera eliminare. Selezionare quindi **Rimuovi**.

    ![Screenshot mostra Rimuovi selezionato dal menu.](./media/openvpn-azure-ad-client/delete/delete1.jpg)

2. Per procedere all'eliminazione, selezionare **Rimuovi**.

    ![Screenshot Visualizza una finestra di dialogo di conferma con l'opzione per la rimozione o l'annullamento.](./media/openvpn-azure-ad-client/delete/delete2.jpg)

## <a name="create-a-connection"></a><a name="connection"></a>Creare una connessione

1. Nella pagina selezionare **+** , quindi **+ Aggiungi**.

    ![Screenshot mostra Aggiungi selezionati dal menu più.](./media/openvpn-azure-ad-client/create/create1.jpg)

2. Inserire le informazioni di connessione. Se non si è certi dei valori, contattare l'amministratore. Dopo aver compilato i valori, selezionare **Salva**.

    ![Screenshot che mostra il riquadro in cui è possibile immettere i valori necessari.](./media/openvpn-azure-ad-client/create/create2.jpg)

3. Selezionare **Connetti** per connettersi alla VPN.

    ![Screenshot mostra il pulsante Connetti per la connessione.](./media/openvpn-azure-ad-client/create/create3.jpg)

4. Selezionare le credenziali appropriate e quindi fare clic su **continua**.

    ![Screenshot che mostra la finestra di dialogo Accedi.](./media/openvpn-azure-ad-client/create/create4.jpg)

5. Una volta stabilita la connessione, l'icona diventerà verde e si **disconnetterà**.

    ![Screenshot mostra la connessione in uno stato connesso.](./media/openvpn-azure-ad-client/create/create5.jpg)

### <a name="to-connect-automatically"></a><a name="autoconnect"></a>Per connettersi automaticamente

Questi passaggi consentono di configurare la connessione per la connessione automatica con always-on.

1. Nella home page per il client VPN selezionare **Impostazioni VPN**.

    ![Screenshot mostra le connessioni V P N in cui è possibile selezionare le impostazioni V P N.](./media/openvpn-azure-ad-client/auto/auto1.jpg)

2. Selezionare **Sì** nella finestra di dialogo Cambia app.

    ![Screenshot mostra un messaggio di verifica sul cambio di app.](./media/openvpn-azure-ad-client/auto/auto2.jpg)

3. Verificare che la connessione che si vuole impostare non sia già connessa, quindi evidenziare il profilo e selezionare la casella di controllo **Connetti automaticamente** .

    ![Screenshot mostra una finestra di dialogo impostazioni in cui è possibile selezionare Connetti automaticamente.](./media/openvpn-azure-ad-client/auto/auto3.jpg)

4. Selezionare **Connetti** per avviare la connessione VPN.

    ![Screenshot che mostra il pulsante Connetti.](./media/openvpn-azure-ad-client/auto/auto4.jpg)

## <a name="diagnose-connection-issues"></a><a name="diagnose"></a>Diagnosticare i problemi di connessione

1. Per diagnosticare i problemi di connessione, è possibile usare lo strumento **Diagnosi**. Selezionare il **...** accanto alla connessione VPN che si desidera diagnosticare per visualizzare il menu. Selezionare quindi **Diagnosi**.

    ![Screenshot mostra la diagnostica selezionata dal menu.](./media/openvpn-azure-ad-client/diagnose/diagnose1.jpg)

2. Nella pagina **Proprietà connessione** selezionare **Esegui diagnosi**.

    ![Screenshot che mostra il pulsante Esegui diagnosi per una connessione.](./media/openvpn-azure-ad-client/diagnose/diagnose2.jpg)

3. Accedere con le credenziali personali.

    ![Screenshot mostra la finestra di dialogo Accedi per questa azione.](./media/openvpn-azure-ad-client/diagnose/diagnose3.jpg)

4. Visualizzare i risultati della diagnosi.

    ![Screenshot mostra i risultati della diagnosi.](./media/openvpn-azure-ad-client/diagnose/diagnose4.jpg)

## <a name="faq"></a>Domande frequenti

### <a name="how-do-i-add-dns-suffixes-to-the-vpn-client"></a>Ricerca per categorie aggiungere suffissi DNS al client VPN?

È possibile modificare il file XML del profilo scaricato e aggiungere **\<dnssuffixes> \<dnssufix> \</dnssufix> \</dnssuffixes>** i tag

```
<azvpnprofile>
<clientconfig>

    <dnssuffixes>
          <dnssuffix>.mycorp.com</dnssuffix>
          <dnssuffix>.xyz.com</dnssuffix>
          <dnssuffix>.etc.net</dnssuffix>
    </dnssuffixes>
    
</clientconfig>
</azvpnprofile>
```

### <a name="how-do-i-add-custom-dns-servers-to-the-vpn-client"></a>Ricerca per categorie aggiungere server DNS personalizzati al client VPN?

È possibile modificare il file XML del profilo scaricato e aggiungere **\<dnsservers> \<dnsserver> \</dnsserver> \</dnsservers>** i tag

```
<azvpnprofile>
<clientconfig>

    <dnsservers>
        <dnsserver>x.x.x.x</dnsserver>
        <dnsserver>y.y.y.y</dnsserver>
    </dnsservers>
    
</clientconfig>
</azvpnprofile>
```

> [!NOTE]
> Il client Azure AD OpenVPN usa le voci della tabella dei criteri di risoluzione dei nomi DNS, che significa che i server DNS non saranno elencati nell'output di `ipconfig /all` . Per confermare le impostazioni DNS in uso, vedere [Get-DnsClientNrptPolicy](/powershell/module/dnsclient/get-dnsclientnrptpolicy?view=win10-ps) in PowerShell.
>

### <a name="how-do-i-add-custom-routes-to-the-vpn-client"></a>Ricerca per categorie aggiungere route personalizzate al client VPN?

È possibile modificare il file XML del profilo scaricato e aggiungere **\<includeroutes> \<route> \<destination> \<mask> \</destination> \</mask> \</route> \</includeroutes>** i tag

```
<azvpnprofile>
<clientconfig>

    <includeroutes>
        <route>
            <destination>x.x.x.x</destination><mask>24</mask>
        </route>
    </includeroutes>
    
</clientconfig>
</azvpnprofile>
```
### <a name="how-do-i-direct-all-traffic-to-the-vpn-tunnel-force-tunnel"></a>Ricerca per categorie indirizzare tutto il traffico al tunnel VPN (forza tunnel)?

È possibile modificare il file XML del profilo scaricato e aggiungere **\<includeroutes> \<route> \<destination> \<mask> \</destination> \</mask> \</route> \</includeroutes>** i tag

```
<azvpnprofile>
<clientconfig>

    <includeroutes>
        <route>
            <destination>0.0.0.0</destination><mask>1</mask>
        </route>
        <route>
            <destination>128.0.0.0</destination><mask>1</mask>
        </route>
    </includeroutes>
    
</clientconfig>
</azvpnprofile>
```

### <a name="how-do-i-block-exclude-routes-from-the-vpn-client"></a>Ricerca per categorie le route di blocco (escluse) dal client VPN?

È possibile modificare il file XML del profilo scaricato e aggiungere **\<excluderoutes> \<route> \<destination> \<mask> \</destination> \</mask> \</route> \</excluderoutes>** i tag

```
<azvpnprofile>
<clientconfig>

    <excluderoutes>
        <route>
            <destination>x.x.x.x</destination><mask>24</mask>
        </route>
    </excluderoutes>
    
</clientconfig>
</azvpnprofile>
```
### <a name="can-i-import-the-profile-from-a-command-line-prompt"></a>È possibile importare il profilo da un prompt della riga di comando?

È possibile importare il profilo da un prompt della riga di comando inserendo il file di **azurevpnconfig.xml** scaricato nella cartella **%USERPROFILE%\appdata\local\packages\ Microsoft.AzureVpn_8wekyb3d8bbwe \localstate** ed eseguendo il comando seguente:

```
azurevpn -i azurevpnconfig.xml 
```
per forzare l'importazione, usare anche l'opzione **-f**


## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [creare un tenant di Azure Active Directory per le connessioni VPN aperte P2S che usano l'autenticazione Azure ad](openvpn-azure-ad-tenant.md).