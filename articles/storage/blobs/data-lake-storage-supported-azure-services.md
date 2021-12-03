---
title: Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen | Microsoft-Dokumentation
description: Hier erfahren Sie, welche Azure-Dienste in Azure Data Lake Storage Gen2 integriert werden.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 10/06/2021
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 284971b1b9a54d21f29cf3c3c81c194be1f5071c
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131022491"
---
# <a name="azure-services-that-support-azure-data-lake-storage-gen2"></a>Azure-Dienste, die Azure Data Lake Storage Gen2 unterstützen

Mit Azure-Diensten können Sie Daten erfassen, Analysen durchführen und visuelle Darstellungen erstellen. Dieser Artikel enthält eine Liste mit unterstützten Azure-Diensten, nennt deren Supportebene und bietet Links zu hilfreichen Artikeln für die Verwendung dieser Dienste mit Azure Data Lake Storage Gen2.

## <a name="supported-azure-services"></a>Unterstützte Azure-Dienste

In dieser Tabelle sind die Azure-Dienste aufgeführt, die mit Azure Data Lake Storage Gen2 verwendet werden können. Die in diesen Tabellen enthaltenen Elemente ändern sich im Laufe der Zeit, da die Unterstützung laufend erweitert wird.

> [!NOTE]
> Die Supportebene bezieht sich lediglich darauf, wie der Dienst mit Data Lake Storage Gen 2 unterstützt wird.

|Azure-Dienst |Supportebene |Azure AD |Gemeinsam verwendeter Schlüssel| Verwandte Artikel |
|---------------|-------------------|---|---|---|
|Azure Data Factory|Allgemein verfügbar|Ja|Ja|[Laden von Daten in Azure Data Lake Storage Gen2 mit Azure Data Factory](../../data-factory/load-azure-data-lake-storage-gen2.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure Databricks|Allgemein verfügbar|Ja|Ja|[Azure Data Lake Storage Gen2](/azure/databricks/data/data-sources/azure/azure-datalake-gen2) <br> [Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Azure Databricks](/azure/databricks/scenarios/databricks-extract-load-sql-data-warehouse) <br>[Tutorial: Zugreifen auf Daten vom Typ „Data Lake Storage Gen2“ mit Azure Databricks unter Verwendung von Spark](data-lake-storage-use-databricks-spark.md)|
|Azure Event Hub|Allgemein verfügbar|Nein|Ja|[Erfassen von Ereignissen über Azure Event Hubs in Azure Blob Storage oder Azure Data Lake Storage](../../event-hubs/event-hubs-capture-overview.md)|
|Azure Event Grid|Allgemein verfügbar|Ja|Ja|[Tutorial: Implementieren des Data Lake-Erfassungsmusters zum Aktualisieren einer Databricks Delta-Tabelle](data-lake-storage-events.md)|
|Azure Logic Apps|Allgemein verfügbar|Nein|Ja|[Übersicht: Was ist Azure Logic Apps?](../../logic-apps/logic-apps-overview.md)|
|Azure Machine Learning|Allgemein verfügbar|Ja|Ja|[Zugreifen auf Daten in Azure Storage-Diensten](../../machine-learning/how-to-access-data.md)|
|Azure Stream Analytics|Allgemein verfügbar|Ja|Ja|[Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](../../stream-analytics/stream-analytics-quick-create-portal.md) <br> [Blobspeicher und Azure Data Lake Gen2](../../stream-analytics/stream-analytics-define-outputs.md)|
|Data Box|Allgemein verfügbar|Nein|Ja|[Verwenden von Azure Data Box zum Migrieren von Daten aus einem lokalen Hadoop Distributed File System-Speicher zu Azure Storage](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|
|HDInsight |Allgemein verfügbar|Ja|Ja|[Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md)<br>[Verwenden der HDFS-CLI mit Data Lake Storage Gen2](data-lake-storage-use-hdfs-data-lake-storage.md) <br>[Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Apache Hive in Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)|
|IoT Hub |Allgemein verfügbar|Ja|Ja|[Verwenden des IoT Hub-Nachrichtenroutings zum Senden von Gerät-zu-Cloud-Nachrichten an verschiedene Endpunkte](../../iot-hub/iot-hub-devguide-messages-d2c.md)|
|Power BI|Allgemein verfügbar|Ja|Ja|[Analysieren von Daten in Azure Data Lake Storage Gen2 mit Power BI](/power-query/connectors/datalakestorage)|
|Azure Synapse Analytics (ehemals SQL Data Warehouse)|Allgemein verfügbar|Ja|Ja|[Analysieren von Daten in einem Speicherkonto](../../synapse-analytics/get-started-analyze-storage.md)|
|SQL Server Integration Services (SSIS)|Allgemein verfügbar|Ja|Ja|[Azure Storage-Verbindungs-Manager](/sql/integration-services/connection-manager/azure-storage-connection-manager)|
|Azure-Daten-Explorer|Allgemein verfügbar|Ja|Ja|[Abfragen von Daten in Azure Data Lake mit Azure Data Explorer](/azure/data-explorer/data-lake-query-data)|
|Azure Cognitive Search|Vorschau|Ja|Ja|[Indizieren von Dokumenten in Azure Data Lake Storage Gen2](../../search/search-howto-index-azure-data-lake-storage.md)|
|Azure Content Delivery Network|Noch nicht unterstützt|Nicht verfügbar|Nicht verfügbar|[Indizieren von Dokumenten in Azure Data Lake Storage Gen2](../../cdn/cdn-overview.md)|
|Azure SQL-Datenbank|Noch nicht unterstützt|Nicht verfügbar|Nicht zutreffend|[Was ist Azure SQL-Datenbank?](../../azure-sql/database/sql-database-paas-overview.md)|

> [!TIP]
> Informationen dazu, wie Dienste in Kategorien wie „Erfassung“, „Download“, „Verarbeitung“ und „Visualisierung“ organisiert sind, finden Sie unter [Erfassung, Verarbeitung und Analyse](./data-lake-storage-best-practices.md#ingest-process-and-analyze).

## <a name="see-also"></a>Weitere Informationen

- [Bekannte Probleme mit Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md)
- [Unterstützung von Blob Storage-Features in Azure Storage-Konten](storage-feature-support-in-storage-accounts.md)
- [Open-Source-Plattformen, die Azure Data Lake Storage Gen2 unterstützen](data-lake-storage-supported-open-source-platforms.md)
- [Multiprotokollzugriff für Azure Data Lake Storage](data-lake-storage-multi-protocol-access.md)
- [Bewährte Methoden zur Verwendung von Azure Data Lake Storage Gen2](data-lake-storage-best-practices.md)