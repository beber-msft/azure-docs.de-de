---
title: Parquet-Format
titleSuffix: Azure Data Factory & Azure Synapse
description: In diesem Thema wird der Umgang mit dem Parquet-Format in Azure Data Factory- und Azure Synapse Analytics-Pipelines beschrieben.
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 10/18/2021
ms.author: jianleishen
ms.openlocfilehash: 9f481a49016f3f0f07484cb92a4b53295d37671f
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2021
ms.locfileid: "130255354"
---
# <a name="parquet-format-in-azure-data-factory-and-azure-synapse-analytics"></a>Das Parquet-Format in Azure Data Factory and Azure Synapse Analytics
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Beachten Sie diesen Artikel, wenn Sie die **Parquet-Dateien analysieren oder die Daten im Parquet-Format schreiben** möchten. 

Das Parquet-Format wird für folgende Connectors unterstützt: 

- [Amazon S3](connector-amazon-simple-storage-service.md)
- [Amazon S3 Compatible Storage](connector-amazon-s3-compatible-storage.md)
- [Azure-Blob](connector-azure-blob-storage.md)
- [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
- [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md)
- [Azure Files](connector-azure-file-storage.md)
- [Dateisystem](connector-file-system.md)
- [FTP](connector-ftp.md)
- [Google Cloud Storage](connector-google-cloud-storage.md)
- [HDFS](connector-hdfs.md)
- [HTTP](connector-http.md)
- [Oracle Cloud Storage](connector-oracle-cloud-storage.md)
- [SFTP](connector-sftp.md)

Eine Liste der unterstützten Features für alle verfügbaren Connectors finden Sie im Artikel [Übersicht über die Connectors in Azure Data Factory und Azure Synapse Analytics](connector-overview.md).

## <a name="using-self-hosted-integration-runtime"></a>Verwendung der selbstgehosteten Integration Runtime

> [!IMPORTANT]
> Wenn Sie Parquet-Dateien bei Kopiervorgängen mithilfe einer selbstgehosteten Integration Runtime (etwa zwischen lokalen Datenspeichern und der Cloud) nicht **unverändert** kopieren, müssen Sie auf Ihrem IR-Computer die **64-Bit-Version der JRE 8 (Java Runtime Environment) oder OpenJDK** sowie **Microsoft Visual C++ 2010 Redistributable Package** installieren. Weitere Details finden Sie im nächsten Absatz.

Für die Kopiervorgänge in der selbstgehosteten Integration Runtime mit Serialisierung/Deserialisierung von Parquet-Dateien sucht der Service die Java Runtime Environment, indem es zunächst die Registrierung *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* auf JRE überprüft. Wird diese nicht gefunden, wird im nächsten Versuch die Systemvariable *`JAVA_HOME`* auf OpenJDK überprüft.

- **Für JRE:** Die 64-Bit-Integration Runtime erfordert die 64-Bit-JRE. Diese steht [hier](https://go.microsoft.com/fwlink/?LinkId=808605) zur Verfügung.
- **Für OpenJDK:** Die Unterstützung ist seit der IR-Version 3.13 verfügbar. Packen Sie die Datei „jvm.dll“ zusammen mit allen anderen erforderlichen OpenJDK-Assemblys in einem selbstgehosteten IR-Computer, und legen Sie die Umgebungsvariable JAVA_HOME des Systems entsprechend fest.
- **So installieren Sie Microsoft Visual C++ 2010 Redistributable Package:** Visual C++ 2010 Redistributable Package ist bei Installationen mit selbstgehosteter Integration Runtime nicht installiert. Diese steht [hier](https://www.microsoft.com/download/details.aspx?id=26999) zur Verfügung.

> [!TIP]
> Wenn Sie Daten mit der selbstgehosteten Integration Runtime in das/aus dem Parquet-Format kopieren und ein Fehler mit dem Text „Fehler beim Aufrufen von Java, Meldung: **java.lang.OutOfMemoryError:Java-Heapspeicher**“ auftritt, können Sie auf dem Computer, auf dem sich die selbstgehosteten IR befindet, eine Umgebungsvariable `_JAVA_OPTIONS` hinzufügen, um die min./max. Heapgröße für JVM anzupassen, sodass eine solche Kopie möglich ist, und dann die Pipeline erneut ausführen.

:::image type="content" source="./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png" alt-text="JVM-Heapgröße für selbstgehostete IR festlegen":::

Beispiel: Legen Sie für die Variable `_JAVA_OPTIONS` den Wert `-Xms256m -Xmx16g` fest. Das Flag `Xms` gibt den anfänglichen Speicherzuweisungspool für eine Java Virtual Machine (JVM) an, während `Xmx` den maximalen Speicherzuweisungspool angibt. Das bedeutet, dass die JVM mit einer Speichergröße von `Xms` gestartet wird und eine maximale Speichergröße von `Xmx` verwenden kann. Standardmäßig verwendet der Dienst mindestens 64 MB und maximal 1G.


## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der vom Parquet-Dataset unterstützten Eigenschaften.

| Eigenschaft         | BESCHREIBUNG                                                  | Erforderlich |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Die „type“-Eigenschaft des Datasets muss auf **Parquet** festgelegt werden. | Ja      |
| location         | Speicherorteinstellungen der Datei(en) Jeder dateibasierte Connector verfügt unter `location` über seinen eigenen Speicherorttyp und unterstützte Eigenschaften. **Informationen hierzu finden Sie im Abschnitt „Dataset-Eigenschaften“ des Artikels über Connectors**. | Ja      |
| compressionCodec | Der Codec für die Komprimierung, der beim Schreiben in Parquet-Dateien verwendet werden soll. Beim Lesen aus Parquet-Dateien bestimmen Data Factorys den Codec für die Komprimierung automatisch anhand der Dateimetadaten.<br>Unterstützte Typen sind „**none**“, „**gzip**“, „**snappy**“ (Standard) und „**lzo**“. Hinweis: LZO wird derzeit von der Kopieraktivität nicht für das Lesen/Schreiben von Parquet-Dateien unterstützt. | Nein       |

> [!NOTE]
> Ein Leerzeichen im Spaltennamen wird für Parquet-Dateien nicht unterstützt.

Nachfolgend sehen Sie das Beispiel eines Parquet-Datasets in Azure Blob Storage:

```json
{
    "name": "ParquetDataset",
    "properties": {
        "type": "Parquet",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Parquet-Quelle und -Senke unterstützt werden.

### <a name="parquet-as-source"></a>Parquet als Quelle

Die folgenden Eigenschaften werden im Abschnitt ***\*source\**** der Kopieraktivität unterstützt.

| Eigenschaft      | BESCHREIBUNG                                                  | Erforderlich |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **ParquetSource** festgelegt werden. | Ja      |
| storeSettings | Eine Gruppe von Eigenschaften für das Lesen von Daten aus einem Datenspeicher. Jeder dateibasierte Connector verfügt unter `storeSettings` über eigene unterstützte Leseeinstellungen. **Informationen hierzu finden Sie im Abschnitt über die Eigenschaften der Kopieraktivität im Artikel über Connectors**. | Nein       |

### <a name="parquet-as-sink"></a>Parquet als Senke

Die folgenden Eigenschaften werden im Abschnitt ***\*sink\**** der Kopieraktivität unterstützt:

| Eigenschaft      | BESCHREIBUNG                                                  | Erforderlich |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Die „type“-Eigenschaft der Senke der Kopieraktivität muss auf **ParquetSink** festgelegt werden. | Ja      |
| formatSettings | Eine Gruppe von Eigenschaften. Weitere Informationen zu **Parquet-Schreibeinstellungen** finden Sie in der Tabelle unten. |    Nein      |
| storeSettings | Eine Gruppe von Eigenschaften für das Schreiben von Daten in einen Datenspeicher. Jeder dateibasierte Connector verfügt unter `storeSettings` über eigene unterstützte Schreibeinstellungen. **Informationen hierzu finden Sie im Abschnitt über die Eigenschaften der Kopieraktivität im Artikel über Connectors**. | Nein       |

Unterstützte **Parquet-Schreibeinstellungen** unter `formatSettings`:

| Eigenschaft      | BESCHREIBUNG                                                  | Erforderlich                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| Typ          | Der Typ von „formatSettings“ muss auf **ParquetWriteSettings** festgelegt werden. | Ja                                                   |
| maxRowsPerFile | Wenn Sie Daten in einen Ordner schreiben, können Sie in mehrere Dateien zu schreiben und die maximale Anzahl von Zeilen pro Datei angeben.  | Nein |
| fileNamePrefix | Gilt, wenn `maxRowsPerFile` konfiguriert ist.<br> Geben Sie das Dateinamenpräfix beim Schreiben von Daten in mehrere Dateien an, das zu diesem Muster führt: `<fileNamePrefix>_00000.<fileExtension>`. Wenn keine Angabe erfolgt, wird das Dateinamenpräfix automatisch generiert. Diese Eigenschaft findet keine Anwendung, wenn die Quelle ein dateibasierter Speicher oder ein [Datenspeicher mit aktivierter Partitionsoption](copy-activity-performance-features.md) ist.  | Nein |

## <a name="mapping-data-flow-properties"></a>Eigenschaften von Mapping Data Flow

Bei Zuordnungsdatenflüssen können Sie in den folgenden Datenspeichern das Parquet-Format lesen und schreiben: [Azure Blob Storage](connector-azure-blob-storage.md#mapping-data-flow-properties), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) und [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties). Darüber hinaus können Sie das Parquet-Format in [Amazon S3](connector-amazon-simple-storage-service.md#mapping-data-flow-properties) lesen.

### <a name="source-properties"></a>Quelleigenschaften

In der folgenden Tabelle sind die von einer Parquet-Quelle unterstützten Eigenschaften aufgeführt. Sie können diese Eigenschaften auf der Registerkarte **Quelloptionen** bearbeiten.

| Name | BESCHREIBUNG | Erforderlich | Zulässige Werte | Datenflussskript-Eigenschaft |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Format | Das Format muss `parquet` sein | ja | `parquet` | format |
| Platzhalterpfade | Alle Dateien, die dem Platzhalterpfad entsprechen, werden verarbeitet. Überschreibt den Ordner und den Dateipfad, die im Dataset festgelegt sind. | nein | String[] | wildcardPaths |
| Partitionsstammpfad | Für partitionierte Dateidaten können Sie einen Partitionsstammpfad eingeben, um partitionierte Ordner als Spalten zu lesen. | nein | String | partitionRootPath |
| Liste mit den Dateien | Gibt an, ob Ihre Quelle auf eine Textdatei verweist, in der die zu verarbeitenden Dateien aufgelistet sind. | nein | `true` oder `false` | fileList |
| Spalte, in der der Dateiname gespeichert wird | Erstellt eine neue Spalte mit dem Namen und Pfad der Quelldatei. | nein | String | rowUrlColumn |
| Nach Abschluss | Löscht oder verschiebt die Dateien nach der Verarbeitung. Dateipfad beginnt mit dem Containerstamm | nein | Löschen: `true` oder `false` <br> Verschieben: `[<from>, <to>]` | purgeFiles <br> moveFiles |
| Nach der letzten Änderung filtern | Filtern Sie Dateien nach dem Zeitpunkt ihrer letzten Änderung. | nein | Timestamp | modifiedAfter <br> modifiedBefore |
| Finden keiner Dateien zulässig | „true“ gibt an, dass kein Fehler ausgelöst wird, wenn keine Dateien gefunden werden. | nein | `true` oder `false` | ignoreNoFilesFound |

### <a name="source-example"></a>Quellbeispiel

Das folgende Bild ist ein Beispiel für eine Parquet-Quellkonfiguration bei Zuordnungsdatenflüssen.

:::image type="content" source="media/data-flow/parquet-source.png" alt-text="Parquet-Quelle":::

Das zugehörige Datenflussskript ist:

```
source(allowSchemaDrift: true,
    validateSchema: false,
    rowUrlColumn: 'fileName',
    format: 'parquet') ~> ParquetSource
```

### <a name="sink-properties"></a>Senkeneigenschaften

In der folgenden Tabelle sind die von einer Parquet-Senke unterstützten Eigenschaften aufgeführt. Sie können diese Eigenschaften auf der Registerkarte **Einstellungen** bearbeiten.

| Name | BESCHREIBUNG | Erforderlich | Zulässige Werte | Datenflussskript-Eigenschaft |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Format | Das Format muss `parquet` sein | ja | `parquet` | format |
| Ordner löschen | Wenn der Zielordner vor dem Schreiben gelöscht wird. | nein | `true` oder `false` | truncate |
| Dateinamenoption | Das Namensformat der geschriebenen Daten. Standardmäßig eine Datei pro Partition im Format `part-#####-tid-<guid>`. | nein | Muster: String <br> Pro Partition: String[] <br> Wie Daten in Spalte: String <br> Ausgabe in eine einzelne Datei: `['<fileName>']` | filePattern <br> partitionFileNames <br> rowUrlColumn <br> partitionFileNames |

### <a name="sink-example"></a>Senkenbeispiel

Das folgende Bild ist ein Beispiel für eine Parquet-Senkenkonfiguration bei Zuordnungsdatenflüssen.

:::image type="content" source="media/data-flow/parquet-sink.png" alt-text="Parquet-Senke":::

Das zugehörige Datenflussskript ist:

```
ParquetSource sink(
    format: 'parquet',
    filePattern:'output[n].parquet',
    truncate: true,
    allowSchemaDrift: true,
    validateSchema: false,
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> ParquetSink
```

## <a name="data-type-support"></a>Datentypunterstützung

Komplexe Parquet-Datentypen (z. B. MAP, LIST, STRUCT) werden derzeit nur in Datenflüssen, aber nicht in der Kopieraktivität unterstützt. Wenn Sie komplexe Typen in Datenflüssen verwenden möchten, importieren Sie das Dateischema nicht in das Dataset und lassen das Schema im Dataset leer. Importieren Sie dann die Projektion in die Quelltransformation.

## <a name="next-steps"></a>Nächste Schritte

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Mapping Data Flow](concepts-data-flow-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [GetMetadata-Aktivität](control-flow-get-metadata-activity.md)
