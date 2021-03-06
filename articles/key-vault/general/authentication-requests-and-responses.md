---
title: Autenticazione, richieste e risposte
description: Informazioni su come Azure Key Vault usa richieste e risposte in formato JSON e sull'autenticazione necessaria per l'uso di un insieme di credenziali delle chiavi.
services: key-vault
author: amitbapat
manager: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 09/15/2020
ms.author: ambapat
ms.openlocfilehash: 58616b647affd33e96357e556ab61f85d1c62129
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/07/2020
ms.locfileid: "96752278"
---
# <a name="authentication-requests-and-responses"></a>Autenticazione, richieste e risposte

Azure Key Vault fornisce due tipi di contenitori per archiviare e gestire i segreti per le applicazioni cloud:

|Tipo di contenitore|Tipi di oggetto supportati|Endpoint del piano dati|
|--|--|--|
| **Insiemi di credenziali**|<ul><li>Chiavi protette tramite software</li><li>Chiavi protette dal modulo di protezione hardware (con SKU Premium)</li><li>Certificati</li><li>Chiavi dell'account di archiviazione</li></ul> | https://{nome-insiemecredenziali}.vault.azure.net
|**Modulo di protezione hardware gestito** |<ul><li>Chiavi protette dal modulo di protezione hardware</li></ul> | https://{nome-hsm}.managedhsm.azure.net

Ecco i suffissi URL usati per accedere a ogni tipo di oggetto

|Tipo oggetto|Suffisso URL|
|--|--|
|Chiavi protette tramite software| /keys |
|Chiavi protette dal modulo di protezione hardware| /keys |
|Segreti|/secrets|
|Certificati| /certificates|
|Chiavi dell'account di archiviazione|/storageaccounts
||

Azure Key Vault supporta richieste e risposte in formato JSON. Le richieste dirette ad Azure Key Vault vengono indirizzate a un URL di Azure Key Vault valido usando HTTPS con alcuni parametri URL e corpi di richiesta e risposta con codifica JSON.

Questo argomento illustra le caratteristiche del servizio Azure Key Vault. Per informazioni generali sull'uso delle interfacce REST di Azure, tra cui l'autenticazione/autorizzazione e come acquisire un token di accesso, vedere [Informazioni di riferimento sull'API REST di Azure](/rest/api/azure).

## <a name="request-url"></a>URL richiesta  
 Le operazioni di gestione delle chiavi usano HTTP DELETE, GET, PATCH, PUT e HTTP POST, mentre le operazioni crittografiche su oggetti chiave esistenti usano HTTP POST. I client che non possono supportare verbi HTTP specifici possono anche usare HTTP POST con l'intestazione X-HTTP-REQUEST per specificare il verbo desiderato. Le richieste che normalmente non richiedono un corpo devono includere un corpo vuoto quando si usa HTTP POST, ad esempio quando si usa POST invece di DELETE.  

 Di seguito sono riportati gli URL di esempio per lavorare con gli oggetti in Azure Key Vault:  

- Per creare una chiave denominata TESTKEY in Key Vault usare - `PUT /keys/TESTKEY?api-version=<api_version> HTTP/1.1`  

- Per importare una chiave denominata IMPORTEDKEY in Key Vault usare- `POST /keys/IMPORTEDKEY/import?api-version=<api_version> HTTP/1.1`  

- Per ottenere un segreto denominato MYSECRET in Key Vault usare - `GET /secrets/MYSECRET?api-version=<api_version> HTTP/1.1`  

- Per firmare un digest usando una chiave denominata TESTKEY in Key Vault usare - `POST /keys/TESTKEY/sign?api-version=<api_version> HTTP/1.1`  

- L'autorità per una richiesta a Key Vault è sempre la seguente: 
  - Per gli insiemi di credenziali: `https://{keyvault-name}.vault.azure.net/`
  - Per HSM gestiti: `https://{HSM-name}.managedhsm.azure.net/`

  Le chiavi vengono sempre archiviate nel percorso /keys, i segreti vengono sempre archiviati nel percorso /secrets.  

## <a name="api-version"></a>Versione API  
 Il servizio Azure Key Vault supporta il controllo delle versioni del protocollo per garantire la compatibilità con i client di livello inferiore, anche se non tutte le funzionalità saranno disponibili per tali client. I client devono usare il parametro di stringa della query `api-version` per specificare la versione del protocollo supportato, dato che non ci sono valori predefiniti.  

 Le versioni del protocollo di Azure Key Vault seguono uno schema di numerazione con data nel formato {AAAA}.{MM}.{GG}.  

## <a name="request-body"></a>Corpo della richiesta  
 Secondo la specifica HTTP, le operazioni GET NON devono avere un corpo per la richiesta, mentre le operazioni POST e PUT devono avere un corpo per la richiesta. Il corpo nelle operazioni DELETE è facoltativo in HTTP.  

 Se non diversamente specificato nella descrizione dell'operazione, il tipo di contenuto del corpo della richiesta deve essere application/json e contenere un oggetto JSON serializzato conforme al tipo di contenuto.  

 Se non diversamente specificato nella descrizione del funzionamento, l'intestazione Accept della richiesta deve contenere il tipo di elemento multimediale application/json.  

## <a name="response-body"></a>Corpo della risposta  
 Se non diversamente specificato nella descrizione dell'operazione, il tipo di contenuto del corpo della risposta delle operazioni riuscite e non riuscite sarà application/json e conterrà informazioni dettagliate sugli errori.  

## <a name="using-http-post"></a>Uso di HTTP POST  
 Alcuni client potrebbero non essere in grado di usare determinati verbi HTTP, come PATCH o DELETE. Azure Key Vault supporta HTTP POST come alternativa per questi client, a condizione che il client includa anche l'intestazione "X-HTTP-METHOD" per specificare il verbo HTTP originale. Il supporto per HTTP POST è indicato per ognuna delle API definite in questo documento.  

## <a name="error-responses"></a>Risposte di errore  
 La gestione degli errori usa i codici di stato HTTP. I risultati tipici sono:  

- 2xx – Success (Operazione riuscita): usato per un'operazione normale. Il corpo della risposta conterrà il risultato previsto  

- 3xx – Redirection (Reindirizzamento): può essere restituito il messaggio 304 "Not Modified" (Non modificato) per soddisfare un'operazione GET condizionale. Altri codici 3xx potranno essere usati in futuro per indicare modifiche di DNS e percorso.  

- 4xx-Errore del client: usato per richieste non corrette, chiavi mancanti, errori di sintassi, parametri non validi, errori di autenticazione e così via. Il corpo della risposta conterrà una spiegazione dettagliata dell'errore.  

- 5xx – Server Error (Errore server): usato per errori interni del server. L'organo di risposta contiene informazioni riassuntive sull'errore.  

  Il sistema è progettato per funzionare dietro un proxy o un firewall. Il client potrebbe quindi ricevere altri codici di errore.  

  Azure Key Vault restituisce anche le informazioni sull'errore nel corpo della risposta quando si verifica un problema. Il corpo della risposta è in formato JSON e assume il formato seguente:  

```  

{  
  "error":  
  {  
    "code": "BadArgument",  
    "message":  

      "’Foo’ is not a valid argument for ‘type’."  
    }  
  }  
}  

```  

## <a name="authentication"></a>Authentication  
 Tutte le richieste inviate ad Azure Key Vault DEVONO essere autenticate. Azure Key Vault supporta i token di accesso di Azure Active Directory che possono essere ottenuti tramite OAuth2 [[RFC6749](https://tools.ietf.org/html/rfc6749)]. 
 
 Per altre informazioni sulla registrazione dell'applicazione e l'autenticazione per l'uso di Azure Key Vault, vedere [Registrare l'applicazione client con Azure AD](/rest/api/azure/index#register-your-client-application-with-azure-ad).
 
 I token di accesso devono essere inviati al servizio usando l'intestazione di autorizzazione HTTP:  

```  
PUT /keys/MYKEY?api-version=<api_version>  HTTP/1.1  
Authorization: Bearer <access_token>  

```  

 Se non viene fornito un token di accesso o se un token non è accettato dal servizio, al client viene restituito un errore HTTP 401 che include l'intestazione WWW-Authenticate, ad esempio:  

```  
401 Not Authorized  
WWW-Authenticate: Bearer authorization="…", resource="…"  

```  

 I parametri nell'intestazione WWW-Authenticate sono i seguenti:  

-   authorization: indirizzo del servizio di autorizzazione OAuth2 che può essere usato per ottenere un token di accesso per la richiesta.  

-   Resource: nome della risorsa ( `https://vault.azure.net` ) da usare nella richiesta di autorizzazione.

> [!NOTE]
> Key Vault Client SDK per i segreti, i certificati e le chiavi nella prima chiamata a Key Vault non forniscono un token di accesso per recuperare le informazioni sul tenant. Si prevede di ricevere un HTTP 401 usando Key Vault Client SDK, in cui il Key Vault Mostra all'applicazione l'intestazione WWW-Authenticate che contiene la risorsa e il tenant in cui deve andare e richiedere il token. Se tutti gli elementi sono configurati correttamente, la seconda chiamata dall'applicazione a Key Vault conterrà un token valido e avrà esito positivo. 
