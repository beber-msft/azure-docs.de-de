---
title: 'Schnellstart: Ihre erste JavaScript-Abfrage'
description: In dieser Schnellstartanleitung führen Sie die Schritte zum Aktivieren der Resource Graph-Bibliothek für JavaScript und zum Ausführen Ihrer ersten Abfrage aus.
ms.date: 07/09/2021
ms.topic: quickstart
ms.custom: devx-track-js, mode-api
ms.openlocfilehash: 9ff7237938deeebee33568fcb176fdde1b2275ef
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131068587"
---
# <a name="quickstart-run-your-first-resource-graph-query-using-javascript"></a>Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage per JavaScript

In dieser Schnellstartanleitung werden Sie durch den Prozess zum Hinzufügen der Bibliotheken zu Ihrer JavaScript-Installation geführt. Wenn Sie Azure Resource Graph verwenden möchten, müssen Sie zuerst eine JavaScript-Anwendung mit den erforderlichen Bibliotheken initialisieren.

Am Ende dieses Prozesses haben Sie Ihrer JavaScript-Installation die Bibliotheken hinzugefügt und Ihre erste Resource Graph-Abfrage ausgeführt.

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement**: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses](https://azure.microsoft.com/free/) Konto erstellen, bevor Sie beginnen.

- **Node.js**: [Node.js](https://nodejs.org/) Version 12 oder höher ist erforderlich.

## <a name="application-initialization"></a>Anwendungsinitialisierung

Um JavaScript zum Abfragen von Azure Resource Graph zu aktivieren, muss die Umgebung konfiguriert werden. Dieses Setup funktioniert überall dort, wo JavaScript verwendet werden kann, einschließlich [Bash unter Windows 10](/windows/wsl/install-win10).

1. Initialisieren Sie ein Node.js-Projekt, indem Sie den folgenden Befehl ausführen.

   ```bash
   npm init -y
   ```

1. Fügen Sie einen Verweis auf das yargs-Modul hinzu.

   ```bash
   npm install yargs
   ```

1. Fügen Sie einen Verweis auf das Azure Resource Graph-Modul hinzu.

   ```bash
   npm install @azure/arm-resourcegraph
   ```

1. Fügen Sie einen Verweis auf die Active Directory-Authentifizierungsbibliothek hinzu.

   ```bash
   npm install @azure/ms-rest-nodeauth
   ```

   > [!NOTE]
   > Überprüfen Sie, in _package.json_, ob `@azure/arm-resourcegraph` in Version **2.0.0** oder höher und `@azure/ms-rest-nodeauth` in Version **3.0.3** oder höher vorliegen.

## <a name="query-the-resource-graph"></a>Abfragen von Resource Graph

1. Erstellen Sie eine neue Datei mit dem Namen _index.js_, und geben Sie den folgenden Code ein.

   ```javascript
   const argv = require("yargs").argv;
   const authenticator = require("@azure/ms-rest-nodeauth");
   const resourceGraph = require("@azure/arm-resourcegraph");

   if (argv.query) {
       const query = async () => {
          const credentials = await authenticator.interactiveLogin();
          const client = new resourceGraph.ResourceGraphClient(credentials);
          const result = await client.resources(
             {
                 query: argv.query
             },
             { resultFormat: "table" }
          );
          console.log("Records: " + result.totalRecords);
          console.log(result.data);
       };

       query();
   }
   ```

   > [!NOTE]
   > Dieser Code erstellt eine mandantenbasierte Abfrage. Das Beschränken einer Abfrage auf eine [Verwaltungsgruppe](../management-groups/overview.md) oder ein Abonnement. Hierzu müssen Sie eine [Abfrageaufforderung](/javascript/api/@azure/arm-resourcegraph/queryrequest) an den `client.resources`-Aufruf definieren und hinzufügen, und entweder `managementGroups` oder `subscriptions` angeben.

1. Geben Sie den folgenden Befehl in das Terminal ein:

   ```bash
   node index.js --query "Resources | project name, type | limit 5"
   ```

   > [!NOTE]
   > Da dieses Abfragebeispiel keinen Sortierungsmodifizierer wie `order by` umfasst, ergibt die mehrfache Ausführung dieser Abfrage unter Umständen pro Anforderung einen anderen Satz von Ressourcen.

1. Ändern Sie den ersten Parameter in `index.js`, und ändern Sie die Abfrage so, dass die Sortierung (`order by`) nach der Eigenschaft **Name** erfolgt:

   ```bash
   node index.js --query "Resources | project name, type | limit 5 | order by name asc"
   ```

   Wenn das Skript versucht, sich zu authentifizieren, wird im Terminal eine Meldung ähnlich der folgenden angezeigt:

   > Verwenden Sie zur Anmeldung einen Webbrowser, um die Seite https://microsoft.com/devicelogin zu öffnen. Geben Sie dann zur Authentifizierung den Code FGB56WJUGK ein.

   Wenn Sie sich beim Browser authentifizieren, wird das Skript weiterhin ausgeführt.

   > [!NOTE]
   > Genau wie bei der ersten Abfrage ergibt die mehrfache Ausführung dieser Abfrage vermutlich pro Anforderung einen anderen Satz von Ressourcen. Die Reihenfolge der Abfragebefehle ist wichtig. In diesem Beispiel kommt `order by` nach `limit`. Durch diese Befehlsreihenfolge werden die Abfrageergebnisse zuerst eingeschränkt und dann sortiert.

1. Ändern Sie den ersten Parameter in `index.js`, und ändern Sie die Abfrage so, dass zuerst die Sortierung (`order by`) nach der Eigenschaft **Name** und dann die Begrenzung (`limit`) auf die ersten fünf Ergebnisse erfolgt:

   ```bash
   node index.js --query "Resources | project name, type | order by name asc | limit 5"
   ```

Wenn die letzte Abfrage mehrmals ausgeführt wird und in Ihrer Umgebung keine Änderungen vorgenommenen werden, sind die zurückgegebenen Ergebnisse konsistent und nach der Eigenschaft **Name** sortiert, aber immer noch auf die ersten fünf Ergebnisse begrenzt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die installierten Bibliotheken aus Ihrer Anwendung entfernen möchten, führen Sie den folgenden Befehl aus.

```bash
npm uninstall @azure/arm-resourcegraph @azure/ms-rest-nodeauth yargs
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Ihrer JavaScript-Umgebung die Resource Graph-Bibliotheken hinzugefügt und Ihre erste Abfrage ausgeführt. Wenn Sie mehr über die Resource Graph-Sprache erfahren möchten, fahren Sie mit der Seite mit den Details zur Abfragesprache fort.

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über die Abfragesprache](./concepts/query-language.md).
