---
title: Librerie di connessione-database di Azure per PostgreSQL-server singolo
description: Questo articolo descrive diverse librerie e driver che è possibile usare quando si codificano le applicazioni per connettersi ed eseguire query su database di Azure per PostgreSQL-server singolo.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 45081c6ba161686498398f2c4ccae8b4cff4c0d1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2020
ms.locfileid: "91704311"
---
# <a name="connection-libraries-for-azure-database-for-postgresql---single-server"></a>Librerie di connessione per database di Azure per PostgreSQL-server singolo
Questo argomento elenca le librerie e i driver che gli sviluppatori possono usare per creare applicazioni per la connessione e l'esecuzione di query in Database di Azure per PostgreSQL.

## <a name="client-interfaces"></a>Interfacce client
La maggior parte delle librerie client di linguaggio usate per connettersi al server PostgreSQL è costituita da progetti esterni e viene distribuita in modo indipendente. Le librerie elencate sono supportate dalle piattaforme Windows, Linux e Mac per la connessione a Database di Azure per PostgreSQL. Nella sezione Passaggi successivi sono elencati alcuni esempi introduttivi.

| **Lingua** | **Interfaccia del client** | **Informazioni aggiuntive** | **Download** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | Compatibile con l'API DB 2.0 | [Download](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://secure.php.net/manual/en/book.pgsql.php) | Estensione del database | [Installazione](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [Pg npm package](https://www.npmjs.com/package/pg) | Client non bloccante JavaScript puro | [Installazione](https://www.npmjs.com/package/pg) |
| Java | [JDBC](https://jdbc.postgresql.org/) | Driver JDBC tipo 4 | [Download](https://jdbc.postgresql.org/download.html)  |
| Ruby | [Pg gem](https://deveiate.org/code/pg/) | Interfaccia Ruby | [Download](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Go | [Package pq](https://godoc.org/github.com/lib/pq) | Driver postgres Go puro | [Installazione](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](https://www.npgsql.org/) | Provider di dati ADO.NET | [Download](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | Driver ODBC | [Download](https://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Interfaccia primaria di linguaggio C | Incluso |
| C++ | [libpqxx](http://pqxx.org/) | Interfaccia nel nuovo stile C++ | [Download](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Passaggi successivi
Leggere queste guide introduttive per informazioni su come connettersi ed eseguire query in Database di Azure per PostgreSQL usando il linguaggio scelto:

[Python](./connect-python.md)  |  [Node.JS](./connect-nodejs.md)  |  [Java](./connect-java.md)  |  [Ruby](./connect-ruby.md)  |  [Php](./connect-php.md)  |  [.NET (C#)](./connect-csharp.md)  |  [Vai](./connect-go.md)
