---
title: Transformieren von Daten mit einer Spark-Aktivität
titleSuffix: Azure Data Factory & Azure Synapse
description: Hier erfahren Sie, wie Sie Daten umwandeln können, indem Sie Spark-Programme aus einer Azure Data Factory- oder Synapse Analytics-Pipeline mithilfe der Spark-Aktivität ausführen.
ms.service: data-factory
ms.subservice: tutorials
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: d802b911a462ef077fa69d62d524e763ab13efc2
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131016585"
---
# <a name="transform-data-using-spark-activity-in-azure-data-factory-and-synapse-analytics"></a>Umwandlung von Daten mithilfe von Spark-Aktivitäten in Azure Data Factory und Synapse Analytics
> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-spark.md)
> * [Aktuelle Version](transform-data-using-spark.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Die Spark-Aktivität in einer Datenfabrik und Synapse [Pipelines](concepts-pipelines-activities.md) führt ein Spark-Programm auf [ihrem eigenen](compute-linked-services.md#azure-hdinsight-linked-service) oder [auf Abruf](compute-linked-services.md#azure-hdinsight-on-demand-linked-service)  HDInsight-Cluster aus. Dieser Artikel baut auf dem Artikel zu [Datentransformationsaktivitäten](transform-data.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten bietet. Wenn Sie einen mit Spark verknüpften On-Demand-Dienst verwenden, erstellt der Dienst automatisch einen Spark-Cluster für Sie, um die Daten just-in-time zu verarbeiten, und löscht den Cluster, sobald die Verarbeitung abgeschlossen ist. 


## <a name="spark-activity-properties"></a>Eigenschaften von Spark-Aktivitäten
Dies ist die JSON-Beispieldefinition einer Spark-Aktivität:    

```json
{
    "name": "Spark Activity",
    "description": "Description",
    "type": "HDInsightSpark",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "sparkJobLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "rootPath": "adfspark",
        "entryFilePath": "test.py",
        "sparkConfig": {
            "ConfigItem1": "Value"
        },
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ]
    }
}
```

Die folgende Tabelle beschreibt die JSON-Eigenschaften, die in der JSON-Definition verwendet werden:

| Eigenschaft              | BESCHREIBUNG                              | Erforderlich |
| --------------------- | ---------------------------------------- | -------- |
| name                  | Der Name der Aktivität in der Pipeline.    | Ja      |
| description           | Ein Text, der beschreibt, was mit der Aktivität ausgeführt wird.  | Nein       |
| type                  | Für die Spark-Aktivität ist der Aktivitätstyp „HDInsightSpark“. | Ja      |
| linkedServiceName     | Name des mit HDInsight Spark verknüpften Diensts, in dem das Spark-Programm ausgeführt wird. Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md). | Ja      |
| SparkJobLinkedService | Der verknüpfte Azure Storage-Dienst, der die Datei sowie die Abhängigkeiten und Protokolle für den Spark-Auftrag enthält. Hier werden nur die verknüpften **[Azure Blob Storage](./connector-azure-blob-storage.md)** und **[ADLS Gen2](./connector-azure-data-lake-storage.md)** -Dienste unterstützt. Wenn Sie für diese Eigenschaft keinen Wert angeben, wird der Speicher verwendet, der dem HDInsight-Cluster zugeordnet ist. Der Wert dieser Eigenschaft darf nur ein mit Azure Storage verknüpfter Dienst sein. | Nein       |
| rootPath              | Der Azure-Blobcontainer und -ordner mit der Spark-Datei. Beim Dateinamen muss die Groß-/Kleinschreibung beachtet werden. Details zur Struktur dieses Ordners finden Sie im Abschnitt „Ordnerstruktur“ (nächster Abschnitt). | Ja      |
| entryFilePath         | Der relative Pfad zum Stammordner des Spark-Codes bzw. -Pakets. Die Eingabedatei muss eine Python-Datei oder eine JAR-Datei sein. | Ja      |
| className             | Die Java-/Spark-Hauptklasse der Anwendung.      | Nein       |
| Argumente             | Eine Liste der Befehlszeilenargumente für das Spark-Programm. | Nein       |
| proxyUser             | Das Benutzerkonto, dessen Identität angenommen werden soll, um das Spark-Programm auszuführen. | Nein       |
| sparkConfig           | Geben Sie Werte für die Spark-Konfigurationseigenschaften an, die im Thema [Spark-Konfiguration – Anwendungseigenschaften](https://spark.apache.org/docs/latest/configuration.html#available-properties) aufgeführt sind. | Nein       |
| getDebugInfo          | Gibt an, ob die Spark-Protokolldateien in den Azure-Speicher kopiert werden, der vom HDInsight-Cluster verwendet (oder) von sparkJobLinkedService angegeben wird. Zulässige Werte: „None“, „Always“ oder „Failure“. Standardwert: Keine. | Nein       |

## <a name="folder-structure"></a>Ordnerstruktur
Spark-Aufträge lassen sich besser erweitern als Pig- oder Hive-Aufträge. Bei Spark-Aufträgen können Sie mehrere Abhängigkeiten wie z.B. jar-Pakete (im Java-CLASSPATH platziert), Python-Dateien (im PYTHONPATH platziert) sowie beliebige andere Dateien bereitstellen.

Erstellen Sie folgende Ordnerstruktur in Azure Blob Storage, auf den der verknüpfte HDInsight-Dienst verweist. Laden Sie dann abhängige Dateien in die entsprechenden Unterordner in dem Stammordner hoch, der von **entryFilePath** repräsentiert wird. Python-Dateien werden beispielsweise in den Unterordner „pyFiles“ und jar-Dateien in den Unterordner „jars“ des Stammordners hochgeladen. Zur Laufzeit erwartet der Dienst die folgende Ordnerstruktur im Azure Blob-Speicher:     

| Pfad                  | BESCHREIBUNG                              | Erforderlich | type   |
| --------------------- | ---------------------------------------- | -------- | ------ |
| `.` (Stamm)            | Der Stammpfad des Spark-Auftrags im verknüpften Speicherdienst. | Ja      | Ordner |
| &lt;benutzerdefiniert&gt; | Der Pfad, der auf die Eingabedatei des Spark-Auftrags zeigt. | Ja      | Datei   |
| ./jars                | Alle Dateien in diesem Ordner werden hochgeladen und im Java-CLASSPATH des Clusters platziert. | Nein       | Ordner |
| ./pyFiles             | Alle Dateien in diesem Ordner werden hochgeladen und im PYTHONPATH des Clusters platziert. | Nein       | Ordner |
| ./files               | Alle Dateien in diesem Ordner werden hochgeladen und im Executor-Arbeitsverzeichnis platziert. | Nein       | Ordner |
| ./archives            | Alle Dateien in diesem Ordner sind nicht komprimiert. | Nein       | Ordner |
| ./logs                | Der Ordner, der die Protokolle aus dem Spark-Cluster enthält. | Nein       | Ordner |

Hier finden Sie ein Beispiel für einen Speicher mit zwei Spark-Auftragsdateien in der Azure Blob Storage-Instanz, auf die der verknüpfte HDInsight-Dienst verweist.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs
    
    archives
    
    pyFiles

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
    
    archives
    
    jars
    
    files
    
```
## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Daten auf andere Weisen transformiert werden: 

* [U-SQL-Aktivität](transform-data-using-data-lake-analytics.md)
* [Hive-Aktivität](transform-data-using-hadoop-hive.md)
* [Pig-Aktivität](transform-data-using-hadoop-pig.md)
* [MapReduce-Aktivität](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-Streamingaktivität](transform-data-using-hadoop-streaming.md)
* [Spark-Aktivität](transform-data-using-spark.md)
* [Benutzerdefinierte .NET-Aktivität](transform-data-using-dotnet-custom-activity.md)
* [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md)
