---
title: Kopieren von Daten aus SAP BW
titleSuffix: Azure Data Factory & Azure Synapse
description: Erfahren Sie, wie Daten aus SAP Business Warehouse mithilfe einer Copy-Aktivität in einer Azure Data Factory- oder Synapse Analytics-Pipeline in unterstützte Senkendatenspeicher kopiert werden.
author: linda33wj
ms.author: jingwang
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: 2cbdd3dc237c3f2e7b3cf23bb844a06fdd40d605
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132325845"
---
# <a name="copy-data-from-sap-business-warehouse-using-azure-data-factory-or-synapse-analytics"></a>Kopieren von Daten aus SAP Business Warehouse mithilfe von Azure Data Factory oder Synapse Analytics
> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-sap-business-warehouse-connector.md)
> * [Aktuelle Version](connector-sap-business-warehouse.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel wird beschrieben, wie Sie die Copy-Aktivität in Azure Data Factory- und Azure Synapse Analytics-Pipelines verwenden, um Daten aus einem SAP Business Warehouse (BW) zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

>[!TIP]
>Informationen zur allgemeinen Unterstützung des SAP-Datenintegrationsszenarios durch den Dienst finden Sie im [Whitepaper zur SAP-Datenintegration mit Azure Data Factory](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf). Dort finden Sie auch eine detaillierte Einführung in jeden SAP-Connector, einen Vergleich und Leitfäden.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Der SAP Business Warehouse-Connector wird für die folgenden Aktivitäten unterstützt:

- [Kopieraktivität](copy-activity-overview.md) mit [unterstützter Quellen/Senken-Matrix](copy-activity-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)

Sie können Daten aus SAP Business Warehouse in jeden unterstützten Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Dieser SAP Business Warehouse-Connector unterstützt insbesondere Folgendes:

- SAP Business Warehouse **Version 7.x**
- Kopieren von Daten aus **InfoCubes und QueryCubes** (einschließlich BEx-Abfragen) über MDX-Abfragen
- Kopieren von Daten mithilfe der Standardauthentifizierung

>[!NOTE]
>Der SAP Business Warehouse Connector unterstützt derzeit keine Parameter mit MDX.  Wenn eine Filterung mit MDX-Parametern erforderlich ist, können Sie stattdessen den alternativen [SAP-Open-Hub-Connector](connector-sap-business-warehouse-open-hub.md) verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieses SAP Business Warehouse-Connectors müssen Sie folgende Schritte durchführen:

- Richten Sie eine selbstgehostete Integration Runtime ein. Im Artikel [Selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md) finden Sie Details.
- Installieren der **SAP NetWeaver-Bibliothek** auf dem Computer mit der Integrationslaufzeit. Sie erhalten die SAP Netweaver-Bibliothek vom SAP-Administrator oder direkt aus dem [SAP Software Download Center](https://support.sap.com/swdc). Suchen Sie nach der **SAP Note #1025361**, um den Downloadspeicherort für die neueste Version zu ermitteln. Stellen Sie sicher, dass Sie die **64-Bit**-Version der SAP NetWeaver-Bibliothek auswählen, die der Installation der Integration Runtime entspricht. Installieren Sie dann alle Dateien, die im SAP NetWeaver RFC SDK enthalten sind, entsprechend der SAP-Mitteilung (SAP Note). Die SAP NetWeaver-Bibliothek ist auch in der SAP Client Tools-Installation enthalten.

>[!TIP]
>Um Konnektivitätsprobleme bei SAP BW zu beheben, stellen Sie Folgendes sicher:
>- Alle Abhängigkeitsbibliotheken, die aus dem NetWeaver RFC SDK extrahiert wurden, werden im Ordner „%windir%\system32“ platziert. In der Regel enthält er Folgendes: icudt34.dll, icuin34.dll, icuuc34.dll, libicudecnumber.dll, librfc32.dll, libsapucum.dll, sapcrypto.dll, sapcryto_old.dll, sapnwrfc.dll.
>- Die erforderlichen Ports, die zum Herstellen einer Verbindung mit dem SAP-Server verwendet werden, sind auf dem Computer der selbstgehosteten IR aktiviert. Dies sind üblicherweise Port 3300 und 3201.

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-sap-bw-using-ui"></a>Erstellen eines verknüpften Diensts mit SAP BW über die Benutzeroberfläche

Verwenden Sie die folgenden Schritte, um einen verknüpften Dienst mit SAP BW auf der Azure-Portal Benutzeroberfläche zu erstellen.

1. Navigieren Sie in Ihrem Azure Data Factory- oder Synapse-Arbeitsbereich zu der Registerkarte „Verwalten“, wählen Sie „Verknüpfte Dienste“ aus und klicken Sie dann auf „Neu“:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Erstellen Sie einen neuen verknüpften Dienst mithilfe der Azure Data Factory-Benutzeroberfläche.":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Erstellen Sie einen neuen verknüpften Dienst mithilfe der Azure Synapse Benutzeroberfläche.":::

2. Suchen Sie nach SAP, und wählen Sie die SAP BW über den MDX-Connector aus.

    :::image type="content" source="media/connector-sap-business-warehouse/sap-business-warehouse-connector.png" alt-text="Wählen Sie den SAP BW MDX-Connector aus.":::    

1. Konfigurieren Sie die Dienstdetails, testen Sie die Verbindung und erstellen Sie den neuen verknüpften Dienst.

    :::image type="content" source="media/connector-sap-business-warehouse/configure-sap-business-warehouse-linked-service.png" alt-text="Konfigurieren Sie einen verknüpften Dienst für SAP BW.":::

## <a name="connector-configuration-details"></a>Details zur Connector-Konfiguration

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den SAP Business Warehouse-Connector verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit SAP Business Warehouse (BW) verknüpften Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf Folgendes festgelegt werden: **SapBw** | Ja |
| server | Der Name des Servers, auf dem sich die SAP BW-Instanz befindet. | Ja |
| systemNumber | Die Systemnummer des SAP BW-Systems.<br/>Zulässiger Wert: Zweistellige Dezimalzahl, die als Zeichenfolge angegeben ist. | Ja |
| clientId | Client-ID des Clients im SAP BW-System.<br/>Zulässiger Wert: Dreistellige Dezimalzahl, die als Zeichenfolge angegeben ist. | Ja |
| userName | Der Name des Benutzers, der Zugriff auf den SAP-Server hat. | Ja |
| password | Kennwort für den Benutzer Markieren Sie dieses Feld als einen „SecureString“, um es sicher zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Ja |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Eine selbstgehostete Integrationslaufzeit ist erforderlich, wie unter [Voraussetzungen](#prerequisites) erwähnt wird. |Ja |

**Beispiel:**

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom SAP BW-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus SAP BW die „type“-Eigenschaft des Datasets auf **SapBwCub** fest. Es sind keine typspezifischen Eigenschaften, die für das SAP BW-Dataset des Typs „RelationalTable“ unterstützt werden.

**Beispiel:**

```json
{
    "name": "SAPBWDataset",
    "properties": {
        "type": "SapBwCube",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP BW linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Wenn Sie das Datenset vom Typ `RelationalTable` verwenden, wird es weiterhin unverändert unterstützt. Es wird jedoch empfohlen, zukünftig die neue Version zu verwenden.

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der SAP BW-Quelle unterstützt werden.

### <a name="sap-bw-as-source"></a>SAP BW als Quelle

Beim Kopieren von Daten aus SAP BW werden die folgenden Eigenschaften im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf Folgendes festgelegt werden: **SapBwSource** | Ja |
| Abfrage | Gibt die MDX-Abfrage an, mit der Daten aus der SAP BW-Instanz gelesen werden. | Ja |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromSAPBW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP BW input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SapBwSource",
                "query": "<MDX query for SAP BW>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Wenn Sie eine Quelle vom Typ `RelationalSource` verwenden, wird sie weiterhin unverändert unterstützt. Es wird jedoch empfohlen, zukünftig die neue Version zu verwenden.

## <a name="data-type-mapping-for-sap-bw"></a>Datentypzuordnung für SAP BW

Beim Kopieren von Daten aus SAP BW werden die folgenden Zuordnungen von SAP BW-Datentypen zu den vom Dienst intern verwendeten Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

| SAP BW-Datentyp | Zwischendatentyp des Diensts |
|:--- |:--- |
| ACCP | Int |
| CHAR | String |
| CLNT | String |
| CURR | Decimal |
| CUKY | String |
| DEC | Decimal |
| FLTP | Double |
| INT1 | Byte |
| INT2 | Int16 |
| INT4 | Int |
| LANG | String |
| LCHR | String |
| LRAW | Byte[] |
| PREC | Int16 |
| QUAN | Decimal |
| RAW | Byte[] |
| RAWSTRING | Byte[] |
| STRING | String |
| UNIT | String |
| DATS | String |
| NUMC | String |
| TIMS | String |


## <a name="lookup-activity-properties"></a>Eigenschaften der Lookup-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [Lookup-Aktivität](control-flow-lookup-activity.md).


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quelles und Senken für die Kopieraktivität unterstützt werden, finden Sie in der Dokumentation für [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
