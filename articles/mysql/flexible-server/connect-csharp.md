---
title: 'Schnellstart: Herstellen einer Verbindung mithilfe von C# – Azure Database for MySQL Flexible Server'
description: Dieser Schnellstart enthält ein C#-Codebeispiel (.NET), das Sie nutzen können, um eine Verbindung mit Azure Database for MySQL Flexible Server herzustellen und Daten daraus abzufragen.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.custom: mvc, devx-track-csharp
ms.devlang: csharp
ms.topic: quickstart
ms.date: 01/16/2021
ms.openlocfilehash: f092a1c2347157561768464c196a3d044b550b43
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131445671"
---
# <a name="quickstart-use-net-c-to-connect-and-query-data-in-azure-database-for-mysql---flexible-server"></a>Schnellstart: Verwenden von .NET (C#) zum Herstellen einer Verbindung und zum Abfragen von Daten in Azure Database for MySQL Flexible Server.

[[!INCLUDE[applies-to-mysql-flexible-server](../includes/applies-to-mysql-flexible-server.md)]

Dieser Schnellstart zeigt, wie Sie mit einer C#-Anwendung eine Verbindung mit einer Azure Database for MySQL-Instanz herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

- Ein Azure-Konto mit einem aktiven Abonnement. 

    [!INCLUDE [flexible-server-free-trial-note](../includes/flexible-server-free-trial-note.md)]
- Erstellen einer Azure Database for MySQL Flexible Server-Instanz mit dem [Azure-Portal](./quickstart-create-server-portal.md) <br/> oder der [Azure CLI](./quickstart-create-server-cli.md), wenn Sie noch keinen besitzen.
- Je nachdem, ob Sie öffentlichen oder privaten Zugriff verwenden, führen Sie **EINE** der folgenden Aktionen aus, um Konnektivität herzustellen:
- [Erstellen einer Datenbank und eines Nicht-Administratorbenutzers](../howto-create-users.md)
- Installieren Sie das [.NET SDK für Ihre Plattform](https://dotnet.microsoft.com/download) (Windows, Ubuntu Linux oder macOS).

[Treten Probleme auf? Informieren Sie uns darüber.](https://github.com/MicrosoftDocs/azure-docs/issues)

## <a name="create-a-c-project"></a>Erstellen eines C#-Projekts
Führen Sie an der Eingabeaufforderung Folgendes aus:

```
mkdir AzureMySqlExample
cd AzureMySqlExample
dotnet new console
dotnet add package MySqlConnector
```

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Azure-Portal im linken Menü **Alle Ressourcen** aus, und suchen Sie dann nach dem Server, den Sie erstellt haben (z.B. **mydemoserver**).
3. Wählen Sie den Servernamen aus.
4. Notieren Sie sich im Bereich **Übersicht** des Servers den **Servernamen** und den **Anmeldenamen des Serveradministrators**. Wenn Sie Ihr Kennwort vergessen haben, können Sie es in diesem Bereich auch zurücksetzen.
 :::image type="content" source="./media/connect-csharp/server-overview-name-login.png" alt-text="Servername für Azure-Datenbank für MySQL":::

## <a name="step-1-connect-and-insert-data"></a>Schritt 1: Herstellen einer Verbindung und Einfügen von Daten
Verwenden Sie den folgenden Code, um die Daten mit SQL-Anweisungen vom Typ `CREATE TABLE` und `INSERT INTO` zu verbinden und zu laden. Für den Code werden die Methoden der `MySqlConnection`-Klasse verwendet:
- [OpenAsync()](/dotnet/api/system.data.common.dbconnection.openasync#System_Data_Common_DbConnection_OpenAsync) zum Herstellen einer Verbindung mit MySQL
- [CreateCommand()](/dotnet/api/system.data.common.dbconnection.createcommand) zum Festlegen der CommandText-Eigenschaft
- [ExecuteNonQueryAsync()](/dotnet/api/system.data.common.dbcommand.executenonqueryasync) zum Ausführen der Datenbankbefehle

Ersetzen Sie die Parameter `Server`, `Database`, `UserID` und `Password` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```csharp
using System;
using System.Threading.Tasks;
using MySqlConnector;

namespace AzureMySqlExample
{
    class MySqlCreate
    {
        static async Task Main(string[] args)
        {
            var builder = new MySqlConnectionStringBuilder
            {
                Server = "YOUR-SERVER.mysql.database.azure.com",
                Database = "YOUR-DATABASE",
                UserID = "USER",
                Password = "PASSWORD",
                SslMode = MySqlSslMode.Required,
            };

            using (var conn = new MySqlConnection(builder.ConnectionString))
            {
                Console.WriteLine("Opening connection");
                await conn.OpenAsync();

                using (var command = conn.CreateCommand())
                {
                    command.CommandText = "DROP TABLE IF EXISTS inventory;";
                    await command.ExecuteNonQueryAsync();
                    Console.WriteLine("Finished dropping table (if existed)");

                    command.CommandText = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
                    await command.ExecuteNonQueryAsync();
                    Console.WriteLine("Finished creating table");

                    command.CommandText = @"INSERT INTO inventory (name, quantity) VALUES (@name1, @quantity1),
                        (@name2, @quantity2), (@name3, @quantity3);";
                    command.Parameters.AddWithValue("@name1", "banana");
                    command.Parameters.AddWithValue("@quantity1", 150);
                    command.Parameters.AddWithValue("@name2", "orange");
                    command.Parameters.AddWithValue("@quantity2", 154);
                    command.Parameters.AddWithValue("@name3", "apple");
                    command.Parameters.AddWithValue("@quantity3", 100);

                    int rowCount = await command.ExecuteNonQueryAsync();
                    Console.WriteLine(String.Format("Number of rows inserted={0}", rowCount));
                }

                // connection will be closed by the 'using' block
                Console.WriteLine("Closing connection");
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

[Treten Probleme auf? Informieren Sie uns darüber.](https://github.com/MicrosoftDocs/azure-docs/issues)

## <a name="step-2-read-data"></a>Schritt 2: Lesen von Daten

Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung vom Typ `SELECT` zu verbinden und zu lesen. Für den Code werden die Methoden der `MySqlConnection`-Klasse verwendet:
- [OpenAsync()](/dotnet/api/system.data.common.dbconnection.openasync#System_Data_Common_DbConnection_OpenAsync) zum Herstellen einer Verbindung mit MySQL
- [CreateCommand()](/dotnet/api/system.data.common.dbconnection.createcommand) zum Festlegen der CommandText-Eigenschaft
- [ExecuteReaderAsync()](/dotnet/api/system.data.common.dbcommand.executereaderasync) zum Ausführen der Datenbankbefehle
- [ReadAsync()](/dotnet/api/system.data.common.dbdatareader.readasync#System_Data_Common_DbDataReader_ReadAsync) für den Zugriff auf die Datensätze in den Ergebnissen Mithilfe von GetInt32 und GetString werden im Code dann die Werte im Datensatz analysiert.


Ersetzen Sie die Parameter `Server`, `Database`, `UserID` und `Password` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```csharp
using System;
using System.Threading.Tasks;
using MySqlConnector;

namespace AzureMySqlExample
{
    class MySqlRead
    {
        static async Task Main(string[] args)
        {
            var builder = new MySqlConnectionStringBuilder
            {
                Server = "YOUR-SERVER.mysql.database.azure.com",
                Database = "YOUR-DATABASE",
                UserID = "USER@YOUR-SERVER",
                Password = "PASSWORD",
                SslMode = MySqlSslMode.Required,
            };

            using (var conn = new MySqlConnection(builder.ConnectionString))
            {
                Console.WriteLine("Opening connection");
                await conn.OpenAsync();

                using (var command = conn.CreateCommand())
                {
                    command.CommandText = "SELECT * FROM inventory;";

                    using (var reader = await command.ExecuteReaderAsync())
                    {
                        while (await reader.ReadAsync())
                        {
                            Console.WriteLine(string.Format(
                                "Reading from table=({0}, {1}, {2})",
                                reader.GetInt32(0),
                                reader.GetString(1),
                                reader.GetInt32(2)));
                        }
                    }
                }

                Console.WriteLine("Closing connection");
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

[Treten Probleme auf? Informieren Sie uns darüber.](https://github.com/MicrosoftDocs/azure-docs/issues)

## <a name="step-3-update-data"></a>Schritt 3: Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung vom Typ `UPDATE` zu verbinden und zu lesen. Durch den Code werden die Methoden der `MySqlConnection`-Klasse verwendet:
- [OpenAsync()](/dotnet/api/system.data.common.dbconnection.openasync#System_Data_Common_DbConnection_OpenAsync) zum Herstellen einer Verbindung mit MySQL
- [CreateCommand()](/dotnet/api/system.data.common.dbconnection.createcommand) zum Festlegen der CommandText-Eigenschaft
- [ExecuteNonQueryAsync()](/dotnet/api/system.data.common.dbcommand.executenonqueryasync) zum Ausführen der Datenbankbefehle


Ersetzen Sie die Parameter `Server`, `Database`, `UserID` und `Password` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```csharp
using System;
using System.Threading.Tasks;
using MySqlConnector;

namespace AzureMySqlExample
{
    class MySqlUpdate
    {
        static async Task Main(string[] args)
        {
            var builder = new MySqlConnectionStringBuilder
            {
                Server = "YOUR-SERVER.mysql.database.azure.com",
                Database = "YOUR-DATABASE",
                UserID = "USER",
                Password = "PASSWORD",
                SslMode = MySqlSslMode.Required,
            };

            using (var conn = new MySqlConnection(builder.ConnectionString))
            {
                Console.WriteLine("Opening connection");
                await conn.OpenAsync();

                using (var command = conn.CreateCommand())
                {
                    command.CommandText = "UPDATE inventory SET quantity = @quantity WHERE name = @name;";
                    command.Parameters.AddWithValue("@quantity", 200);
                    command.Parameters.AddWithValue("@name", "banana");

                    int rowCount = await command.ExecuteNonQueryAsync();
                    Console.WriteLine(String.Format("Number of rows updated={0}", rowCount));
                }

                Console.WriteLine("Closing connection");
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```


## <a name="step-4-delete-data"></a>Schritt 4: Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung vom Typ `DELETE` zu verbinden und zu löschen.

Durch den Code werden die Methoden der `MySqlConnection`-Klasse verwendet:
- [OpenAsync()](/dotnet/api/system.data.common.dbconnection.openasync#System_Data_Common_DbConnection_OpenAsync) zum Herstellen einer Verbindung mit MySQL
- [CreateCommand()](/dotnet/api/system.data.common.dbconnection.createcommand) zum Festlegen der CommandText-Eigenschaft
- [ExecuteNonQueryAsync()](/dotnet/api/system.data.common.dbcommand.executenonqueryasync) zum Ausführen der Datenbankbefehle


Ersetzen Sie die Parameter `Server`, `Database`, `UserID` und `Password` durch die Werte, die Sie beim Erstellen des Servers und der Datenbank angegeben haben.

```csharp
using System;
using System.Threading.Tasks;
using MySqlConnector;

namespace AzureMySqlExample
{
    class MySqlDelete
    {
        static async Task Main(string[] args)
        {
            var builder = new MySqlConnectionStringBuilder
            {
                Server = "YOUR-SERVER.mysql.database.azure.com",
                Database = "YOUR-DATABASE",
                UserID = "USER",
                Password = "PASSWORD",
                SslMode = MySqlSslMode.Required,
            };

            using (var conn = new MySqlConnection(builder.ConnectionString))
            {
                Console.WriteLine("Opening connection");
                await conn.OpenAsync();

                using (var command = conn.CreateCommand())
                {
                    command.CommandText = "DELETE FROM inventory WHERE name = @name;";
                    command.Parameters.AddWithValue("@name", "orange");

                    int rowCount = await command.ExecuteNonQueryAsync();
                    Console.WriteLine(String.Format("Number of rows deleted={0}", rowCount));
                }

                Console.WriteLine("Closing connection");
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit dem folgenden Befehl, um alle in dieser Schnellstartanleitung verwendeten Ressourcen zu bereinigen:

```azurecli
az group delete \
    --name $AZ_RESOURCE_GROUP \
    --yes
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Verwalten eines Azure Database for MySQL-Servers mithilfe des Portals](./how-to-manage-server-portal.md)<br/>

> [!div class="nextstepaction"]
> [Verwalten eines Azure Database for MySQL-Servers mithilfe der CLI](./how-to-manage-server-cli.md)


