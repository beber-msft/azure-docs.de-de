---
title: Verwalten der Indizierung in der Azure Cosmos DB-API für MongoDB
description: Dieser Artikel enthält eine Übersicht über die Indizierungsfunktionen von Azure Cosmos DB über die Azure Cosmos DB-API für MongoDB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: how-to
ms.date: 10/13/2021
author: gahl-levy
ms.author: gahllevy
ms.custom: devx-track-js
ms.openlocfilehash: b33e80ad409c20be36a4c743573ea959525f82e8
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131045151"
---
# <a name="manage-indexing-in-azure-cosmos-dbs-api-for-mongodb"></a>Verwalten der Indizierung in der Azure Cosmos DB-API für MongoDB
[!INCLUDE[appliesto-mongodb-api](../includes/appliesto-mongodb-api.md)]

Die Azure Cosmos DB-API für MongoDB nutzt die Kernfunktionen für automatische Indexverwaltung von Azure Cosmos DB. Dieser Artikel konzentriert sich auf das Hinzufügen von Indizes mithilfe der Azure Cosmos DB-API für MongoDB. Indizes sind spezialisierte Datenstrukturen, die das Abfragen Ihrer Daten um etwa eine Größenordnung beschleunigen.

## <a name="indexing-for-mongodb-server-version-36-and-higher"></a>Indizierung für MongoDB-Serverversion 3.6 und höher

Azure Cosmos DB-API für MongoDB Server Version 3.6 und höher indiziert automatisch das Feld `_id` und den Shard-Schlüssel (nur in Shard-Sammlungen). Die API erzwingt automatisch die Eindeutigkeit des Felds `_id` durch einen Shardschlüssel. 

Die API für MongoDB verhält sich anders als die Azure Cosmos DB-SQL-API, die standardmäßig alle Felder indiziert.

### <a name="editing-indexing-policy"></a>Bearbeiten der Indizierungsrichtlinie

Es wird empfohlen, die Indizierungsrichtlinie im Azure-Portal im Daten-Explorer zu bearbeiten.
. Über den Editor für Indizierungsrichtlinien im Daten-Explorer können Sie Einzelfeldindizes und Indizes mit Platzhalterzeichen hinzufügen:

:::image type="content" source="./media/mongodb-indexing/indexing-policy-editor.png" alt-text="Editor für Indizierungsrichtlinien":::

> [!NOTE]
> Mit dem Editor für Indizierungsrichtlinien im Daten-Explorer können Sie keine zusammengesetzten Indizes erstellen.

## <a name="index-types"></a>Indextypen

### <a name="single-field"></a>Einzelfeld

Sie können für jedes einzelne Feld Indizes erstellen. Die Sortierreihenfolge des Einzelfeldindexes ist unerheblich. Der folgende Befehl erstellt einen Index für das Feld `name`:

`db.coll.createIndex({name:1})`

Sie können denselben Einzelfeldindex für `name` im Azure-Portal erstellen:

:::image type="content" source="./media/mongodb-indexing/add-index.png" alt-text="Hinzufügen eines Namensindex im Editor für Indizierungsrichtlinien":::

Bei einer Abfrage werden mehrere Einzelfeldindizes verwendet, soweit verfügbar. Sie können pro Container bis zu 500 Einzelfeldindizes erstellen.

### <a name="compound-indexes-mongodb-server-version-36"></a>Zusammengesetzte Indizes (MongoDB-Serverversion 3.6 und höher)
In der API für MongoDB sind zusammengesetzte Indizes **erforderlich**, wenn Ihre Abfrage mehrere Felder gleichzeitig sortieren kann. Für Abfragen mit mehreren Filtern, die nicht sortiert werden müssen, sollten Sie anstelle eines einzelnen zusammengesetzten Indexes mehrere Einzelfeldindizes erstellen. 

Ein zusammengesetzter Index oder ein einzelner Feldindex für jedes Feld im zusammengesetzten Index führt zu der gleichen Leistung beim Filtern in Abfragen.


> [!NOTE]
> Für geschachtelte Eigenschaften oder Arrays können Sie keine zusammengesetzten Indizes erstellen.

Der folgende Befehl erstellt einen zusammengesetzten Index für die Felder `name` und `age`:

`db.coll.createIndex({name:1,age:1})`

Sie können zusammengesetzte Indizes zum gleichzeitigen Sortieren anhand mehrerer Felder verwenden, wie im folgenden Beispiel gezeigt:

`db.coll.find().sort({name:1,age:1})`

Sie können obigen zusammengesetzten Index auch für die effiziente Sortierung einer Abfrage mit der umgekehrten Sortierreihenfolge anhand aller Felder verwenden. Hier sehen Sie ein Beispiel:

`db.coll.find().sort({name:-1,age:-1})`

Allerdings muss die Reihenfolge der Pfade im zusammengesetzten Index exakt mit der Abfrage übereinstimmen. Hier sehen Sie ein Beispiel für eine Abfrage, die einen zusätzlichen zusammengesetzten Index erfordern würde:

`db.coll.find().sort({age:1,name:1})`

> [!NOTE]
> Zusammengesetzte Indizes werden nur in Abfragen verwendet, mit denen Ergebnisse sortiert werden. Für Abfragen mit mehreren Filtern, die nicht sortiert werden müssen, sollten Sie mehrere Einzelfeldindizes erstellen.

### <a name="multikey-indexes"></a>Indizes mit mehreren Schlüsseln

Azure Cosmos DB erstellt Indizes mit mehreren Schlüsseln, um in Arrays gespeicherte Inhalte zu indizieren. Wenn Sie ein Feld mit einem Arraywert indizieren, indiziert Azure Cosmos DB automatisch alle Elemente im Array.

### <a name="geospatial-indexes"></a>Räumliche Indizes

Viele räumliche Operatoren profitieren von räumlichen Indizes. Derzeit unterstützt die API für MongoDB von Azure Cosmos DB `2dsphere`-Indizes. Die API unterstützt `2d`-Indizes noch nicht.

Hier sehen Sie ein Beispiel für das Erstellen eines räumlichen Indexes für das Feld `location`:

`db.coll.createIndex({ location : "2dsphere" })`

### <a name="text-indexes"></a>Textindizes

Derzeit unterstützt die Azure Cosmos DB-API für MongoDB Textindizes noch nicht. Bei Textsuchabfragen für Zeichenfolgen sollten Sie die [Azure Cognitive Search](../../search/search-howto-index-cosmosdb.md)-Integration mit Azure Cosmos DB verwenden. 

## <a name="wildcard-indexes"></a>Platzhalterindizes

Sie können Platzhalterindizes verwenden, um Abfragen für unbekannte Felder zu unterstützen. Angenommen, Sie verfügen über eine Sammlung mit Daten über Familien.

Hier sehen Sie einen Auszug eines Beispieldokuments in dieser Sammlung:

```json
"children": [
   {
     "firstName": "Henriette Thaulow",
     "grade": "5"
   }
]
```

Hier sehen Sie ein weiteres Beispiel, diesmal mit etwas anderen Eigenschaften in `children`:

```json
"children": [
    {
     "familyName": "Merriam",
     "givenName": "Jesse",
     "pets": [
         { "givenName": "Goofy" },
         { "givenName": "Shadow" }
         ]
   },
   {
     "familyName": "Merriam",
     "givenName": "John",
   }
]
```

In dieser Sammlung können Dokumente viele verschiedene Eigenschaften haben. Wenn Sie alle Daten im Array `children` indizieren möchten, haben Sie zwei Möglichkeiten: Sie können separate Indizes für jede einzelne Eigenschaft erstellen oder einen einzelnen Platzhalterindex für das gesamte Array `children`.

### <a name="create-a-wildcard-index"></a>Erstellen eines Platzhalterindex

Mit dem folgenden Befehl wird ein Platzhalterindex für alle Eigenschaften in `children`erstellt:

`db.coll.createIndex({"children.$**" : 1})`

**Anders als in MongoDB können von Platzhalterindizes mehrere Felder in Abfrageprädikaten unterstützt werden.** Für die Abfrageleistung ist es unerheblich, ob Sie einen einzelnen Platzhalterindex verwenden oder einen separaten Index für jede Eigenschaft erstellen.

Folgende Indextypen lassen sich mit Platzhaltersyntax erstellen:

* Einzelfeld
* Geodaten

### <a name="indexing-all-properties"></a>Indizieren aller Eigenschaften

So erstellen Sie einen Platzhalterindex für alle Felder:

`db.coll.createIndex( { "$**" : 1 } )`

Mit dem Daten-Explorer im Azure-Portal können Sie auch Indizes mit Platzhalterzeichen erstellen:

:::image type="content" source="./media/mongodb-indexing/add-wildcard-index.png" alt-text="Hinzufügen eines Index mit Platzhalterzeichen im Editor für Indizierungsrichtlinien":::

> [!NOTE]
> Wenn Sie gerade mit der Entwicklung beginnen, wird **dringend** empfohlen, mit einem Platzhalterindex für alle Felder zu beginnen. Dies kann sowohl die Entwicklung als auch die Optimierung von Abfragen vereinfachen.

Dokumente mit vielen Feldern können eine hohe Gebühr für Anforderungseinheiten (Request Unit, RU) für Schreibvorgänge und Updates aufweisen. Bei schreibintensiven Workloads empfiehlt es sich daher, Pfade einzeln zu indizieren, anstatt Platzhalterindizes zu verwenden.

### <a name="limitations"></a>Einschränkungen

Folgende Indextypen oder Eigenschaften werden von Platzhalterindizes nicht unterstützt:

* Verbund
* TTL
* Eindeutig

**Anders als in MongoDB** können Platzhalterindizes in der Azure Cosmos DB-API für MongoDB **nicht** für Folgendes verwendet werden:

* Erstellen eines Platzhalterindex, der mehrere spezifische Felder einschließt

  ```json
  db.coll.createIndex(
      { "$**" : 1 },
      { "wildcardProjection " :
          {
             "children.givenName" : 1,
             "children.grade" : 1
          }
      }
  )
  ```

* Erstellen eines Platzhalterindex, der mehrere spezifische Felder ausschließt

  ```json
  db.coll.createIndex(
      { "$**" : 1 },
      { "wildcardProjection" :
          {
             "children.givenName" : 0,
             "children.grade" : 0
          }
      }
  )
  ```

Alternativ können Sie mehrere Platzhalterindizes erstellen.

## <a name="index-properties"></a>Indexeigenschaften

Die folgenden Vorgänge können für Konten mit der Wire-Protokollversion 4.0 und Konten mit früheren Versionen verwendet werden. Sie können noch mehr über [unterstützte Indizes und indizierte Eigenschaften](feature-support-40.md#indexes-and-index-properties) erfahren.

### <a name="unique-indexes"></a>Eindeutige Indizes

[Eindeutige Indizes](../unique-keys.md) sind nützlich, um zu erzwingen, dass zwei oder mehr Dokumente nicht denselben Wert für indizierte Felder enthalten dürfen.

> [!IMPORTANT]
> Eindeutige Indizes können nur erstellt werden, wenn die Sammlung leer ist (keine Dokumente enthält).

Mit dem folgenden Befehl wird ein eindeutiger Index mit dem Feld `student_id` erstellt:

```shell
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} )
{
    "_t" : "CreateIndexesResponse",
    "ok" : 1,
    "createdCollectionAutomatically" : false,
    "numIndexesBefore" : 1,
    "numIndexesAfter" : 4
}
```

Bei partitionierten Sammlungen muss für die Erstellung eines eindeutigen Indexes der Shardschlüssel (Partitionsschlüssel) angegeben werden. Anders ausgedrückt: Alle eindeutigen Indizes einer Sammlung mit Shards sind zusammengesetzte Indizes, bei denen eines der Felder der Partitionsschlüssel ist.

Mit den folgenden Befehlen erstellen Sie die partitionierte Sammlung ```coll``` (der Shardschlüssel lautet ```university```) mit einem eindeutigen Index für die Felder `student_id` und `university`:

```shell
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "university" : 1, "student_id" : 1 }, {unique:true});
{
    "_t" : "CreateIndexesResponse",
    "ok" : 1,
    "createdCollectionAutomatically" : false,
    "numIndexesBefore" : 3,
    "numIndexesAfter" : 4
}
```

Im vorherigen Beispiel wird beim Weglassen der ```"university":1```-Klausel folgende Fehlermeldung zurückgegeben:

*cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }*

### <a name="ttl-indexes"></a>TTL-Indizes

Um den Dokumentablauf in einer bestimmten Sammlung zu aktivieren, müssen Sie einen [TTL-Index](../time-to-live.md) (Time To Live, Gültigkeitsdauer) erstellen. Ein TTL-Index ist ein Index für das Feld `_ts` mit einem Wert für `expireAfterSeconds`.

Beispiel:

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Der obige Befehl löscht alle Dokumente in der Sammlung ```db.coll```, die nicht innerhalb der letzten zehn Sekunden geändert wurden.

> [!NOTE]
> **_ts** ist ein Azure Cosmos DB-spezifisches Feld, auf das nicht über MongoDB-Clients zugegriffen werden kann. Dies ist eine reservierte Eigenschaft (Systemeigenschaft), die den Zeitstempel der letzten Änderung eines Dokuments enthält.

## <a name="track-index-progress"></a>Nachverfolgen des Indizierungsfortschritts

Version 3.6 und höher der Azure Cosmos DB-API für MongoDB unterstützt den Befehl `currentOp()` zum Nachverfolgen des Indizierungsfortschritts für eine Datenbankinstanz. Dieser Befehl gibt ein Dokument zurück, das Informationen zu den aktuell in Bearbeitung befindlichen Vorgängen in einer Datenbankinstanz enthält. Verwenden Sie den Befehl `currentOp`, um alle laufenden Vorgänge in der nativen MongoDB-Datenbank zu verfolgen. In der Azure Cosmos DB-API für MongoDB unterstützt dieser Befehl nur die Nachverfolgung des Indizierungsvorgangs.

Im Folgenden werden einige Beispiele zur Verwendung des Befehls `currentOp` zur Nachverfolgung des Indizierungsfortschritts gezeigt:

* Abrufen des Indexfortschritts für eine Sammlung:

   ```shell
   db.currentOp({"command.createIndexes": <collectionName>, "command.$db": <databaseName>})
   ```

* Abrufen des Indizierungsfortschritts für alle Sammlungen in einer Datenbank:

  ```shell
  db.currentOp({"command.$db": <databaseName>})
  ```

* Abrufen des Indizierungsfortschritts für alle Datenbanken und Sammlungen in einem Azure Cosmos-Konto:

  ```shell
  db.currentOp({"command.createIndexes": { $exists : true } })
  ```

### <a name="examples-of-index-progress-output"></a>Beispiele für die Ausgabe des Indizierungsfortschritts

Die Details zum Indizierungsfortschritt zeigen den Fortschritt des aktuellen Indizierungsvorgangs in Prozent an. Im folgenden Beispiel wird das Format des ausgegebenen Dokuments für verschiedene Phasen des Indizierungsfortschritts veranschaulicht:

* Wenn ein Indizierungsvorgang für die Sammlung „foo“ und die Datenbank „bar“ einen Fortschritt von 60 % aufweist, wird das folgende Dokument ausgegeben. Im Feld `Inprog[0].progress.total` wird 100 als Zielprozentsatz für den Abschluss angezeigt.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 60 %",
                "progress" : {
                        "done" : 60,
                        "total" : 100
                },
                …………..…..
        }
        ],
        "ok" : 1
   }
   ```

* Bei einem Indizierungsvorgang, der erst für die Sammlung „foo“ und die Datenbank „bar“ gestartet wurde, zeigt das Ausgabedokument einen Fortschritt von 0 % an, bis ein messbarer Wert erreicht wurde.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 0 %",
                "progress" : {
                        "done" : 0,
                        "total" : 100
                },
                …………..…..
        }
        ],
       "ok" : 1
   }
   ```

* Wenn der aktive Indizierungsvorgang abgeschlossen wird, zeigt das Ausgabedokument leere `inprog`-Vorgänge an.

   ```json
   {
      "inprog" : [],
      "ok" : 1
   }
   ```

## <a name="background-index-updates"></a>Indexaktualisierungen im Hintergrund

Unabhängig von dem für die **Hintergrund**-Indexeigenschaft angegebenen Wert werden Indexaktualisierungen immer im Hintergrund durchgeführt. Da Indexaktualisierungen Anforderungseinheiten (Request Units, RUs) mit einer niedrigeren Priorität als andere Datenbankvorgänge nutzen, führen Indexänderungen nicht zu Ausfallzeiten bei Schreib-, Update- oder Löschvorgängen.

Das Hinzufügen eines neuen Indexes hat keine Auswirkung auf die Leseverfügbarkeit. Abfragen verwenden neue Indizes erst dann, wenn die Indextransformation abgeschlossen ist. Während der Indextransformation werden von der Abfrage-Engine weiterhin vorhandene Indizes verwendet, sodass Sie während der Indextransformation eine ähnliche Leseleistung beobachten werden wie vor dem Einleiten der Indexänderung. Beim Hinzufügen neuer Indizes besteht auch kein Risiko, unvollständige oder inkonsistente Abfrageergebnisse zu erhalten.

Wenn Indizes entfernt und sofort Abfragen ausgeführt werden, die nach den gelöschten Indizes filtern, können die Ergebnisse inkonsistent und unvollständig sein, solange die Indextransformation nicht abgeschlossen ist. Wenn Sie Indizes entfernen, bietet die Abfrage-Engine keine konsistenten oder vollständigen Ergebnisse, falls Abfragen nach diesen soeben entfernten Indizes filtern. Die meisten Entwickler löschen keine Indizes und versuchen dann sofort, Abfragen dafür auszuführen, sodass diese Situation in der Praxis eher unwahrscheinlich ist.

> [!NOTE]
> Sie können [den Indizierungsfortschritt nachverfolgen](#track-index-progress).

## <a name="reindex-command"></a>ReIndex-Befehl

Mit dem `reIndex`-Befehl werden alle Indizes in einer Sammlung neu erstellt. In einigen seltenen Fällen können Probleme mit der Abfrageleistung oder andere Indexprobleme in Ihrer Sammlung durch Ausführen des Befehls `reIndex` gelöst werden. Bei Problemen mit der Indizierung wird empfohlen, die Indizes mit dem Befehl `reIndex` neu zu erstellen. 

Verwenden Sie für die Ausführung des `reIndex`-Befehls die folgende Syntax:

`db.runCommand({ reIndex: <collection> })`

Mithilfe der folgenden Syntax können Sie überprüfen, ob die Ausführung des Befehls `reIndex` die Abfrageleistung in Ihrer Sammlung verbessern würde:

`db.runCommand({"customAction":"GetCollection",collection:<collection>, showIndexes:true})`

Beispielausgabe:

```
{
        "database" : "myDB",
        "collection" : "myCollection",
        "provisionedThroughput" : 400,
        "indexes" : [
                {
                        "v" : 1,
                        "key" : {
                                "_id" : 1
                        },
                        "name" : "_id_",
                        "ns" : "myDB.myCollection",
                        "requiresReIndex" : true
                },
                {
                        "v" : 1,
                        "key" : {
                                "b.$**" : 1
                        },
                        "name" : "b.$**_1",
                        "ns" : "myDB.myCollection",
                        "requiresReIndex" : true
                }
        ],
        "ok" : 1
}
```

Wenn `reIndex` die Abfrageleistung verbessert, ist für **requiresReIndex** der Wert „true“ angegeben. Wenn `reIndex` die Abfrageleistung nicht verbessert, wird diese Eigenschaft ausgelassen.

## <a name="migrate-collections-with-indexes"></a>Migrieren von Sammlungen mit Indizes

Derzeit ist die Erstellung von eindeutigen Indizes nur möglich, wenn die Sammlung keine Dokumente enthält. Bei gängigen MongoDB-Migrationstools wird versucht, die eindeutigen Indizes nach dem Importieren der Daten zu erstellen. Um dieses Problem zu umgehen, können Sie die entsprechenden Sammlungen und eindeutigen Indizes manuell erstellen, damit das Migrationstool dies nicht versucht. (Sie können dieses Verhalten für ```mongorestore``` erzielen, indem Sie das `--noIndexRestore`-Flag an der Befehlszeile verwenden.)

## <a name="indexing-for-mongodb-version-32"></a>Indizierung für MongoDB-Version 3.2

Bei Azure Cosmos-Konten, die mit Version 3.2 des MongoDB-Wire-Protokolls kompatibel sind, weichen die verfügbaren Indizierungsfeatures und Standardwerte ab. Sie können [die Version Ihres Kontos überprüfen](feature-support-36.md#protocol-support) und [eine Upgrade auf Version 3.6 durchführen](upgrade-mongodb-version.md).

Wenn Sie Version 3.2 verwenden, beachten Sie die in diesem Abschnitt erläuterten wichtigen Unterschiede zu den Versionen 3.6 und höher.

### <a name="dropping-default-indexes-version-32"></a>Löschen der Standardindizes (Version 3.2)

Anders als bei den Versionen 3.6 und höher der Azure Cosmos DB-API für MongoDB werden in der Version 3.2 standardmäßig alle Eigenschaften indiziert. Mit dem folgenden Befehl können Sie diese Standardindizes für eine Sammlung (```coll```) löschen:

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

Nachdem Sie die Standardindizes gelöscht haben, können Sie zusätzliche Indizes wie in Version 3.6 und höher hinzufügen.

### <a name="compound-indexes-version-32"></a>Zusammengesetzte Indizes (Version 3.2)

Zusammengesetzte Indizes enthalten Verweise auf mehrere Felder eines Dokuments. Wenn Sie einen zusammengesetzten Index erstellen möchten, [führen Sie ein Upgrade auf Version 3.6 oder 4.0 durch](upgrade-mongodb-version.md).

### <a name="wildcard-indexes-version-32"></a>Platzhalterindizes (Version 3.2)

Wenn Sie einen Platzhalterindex erstellen möchten, [führen Sie ein Upgrade auf Version 4.0 oder 3.6 durch](upgrade-mongodb-version.md).

## <a name="next-steps"></a>Nächste Schritte

* [Indizierung in Azure Cosmos DB](../index-policy.md)
* [Festlegen einer Gültigkeitsdauer für den automatischen Ablauf von Daten in Azure Cosmos DB](../time-to-live.md)
* Informationen zur Beziehung zwischen Partitionierung und Indizierung finden Sie im Artikel [Abfragen eines Azure Cosmos-Containers](../how-to-query-container.md).
* Versuchen Sie, die Kapazitätsplanung für eine Migration zu Azure Cosmos DB durchzuführen? Sie können Informationen zu Ihrem vorhandenen Datenbankcluster für die Kapazitätsplanung verwenden.
    * Wenn Sie nur die Anzahl der virtuellen Kerne und Server in Ihrem vorhandenen Datenbankcluster kennen, lesen Sie die Informationen zum [Schätzen von Anforderungseinheiten mithilfe von virtuellen Kernen oder virtuellen CPUs](../convert-vcore-to-request-unit.md) 
    * Wenn Sie die typischen Anforderungsraten für Ihre aktuelle Datenbankworkload kennen, lesen Sie die Informationen zum [Schätzen von Anforderungseinheiten mit dem Azure Cosmos DB-Kapazitätsplaner](estimate-ru-capacity-planner.md)
