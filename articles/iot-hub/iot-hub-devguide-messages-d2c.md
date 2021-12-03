---
title: Grundlegendes zum Azure IoT Hub-Nachrichtenrouting | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Verwenden des Nachrichtenroutings zum Senden von D2C-Nachrichten. Enthält Informationen zum Senden von Telemetriedaten und anderen Daten.'
author: nehsin
manager: mehmet.kucukgoz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/14/2021
ms.author: nehsin
ms.custom:
- 'Role: Cloud Development'
- devx-track-csharp
ms.openlocfilehash: 5a405c50c94a16394a92ba7c4830ab3f3aee072f
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131449714"
---
# <a name="use-iot-hub-message-routing-to-send-device-to-cloud-messages-to-different-endpoints"></a>Verwenden des IoT Hub-Nachrichtenroutings zum Senden von D2C-Nachrichten an verschiedene Endpunkte

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Das Nachrichtenrouting ermöglicht es Ihnen, Nachrichten automatisiert, skalierbar und zuverlässig von Ihren Geräten an Clouddienste zu senden. Das Nachrichtenrouting kann für Folgendes verwendet werden: 

* **Senden von Gerätetelemetrienachrichten und von Ereignissen** – insbesondere von Ereignissen beim Gerätelebenszyklus, Änderungsereignissen bei Gerätezwillingen, Änderungsereignissen bei digitalen Zwillingen sowie von Ereignissen beim Geräteverbindungsstatus im integrierten Endpunkt und in benutzerdefinierten Endpunkten. Erfahren Sie mehr über [Routingendpunkte](#routing-endpoints). Weitere Informationen zu den von IoT Plug & Play-Geräten gesendeten Ereignissen finden Sie unter [Grundlegendes zu digitalen IoT Plug & Play-Zwillingen](../iot-develop/concepts-digital-twin.md).

* **Filtern von Daten vor dem Weiterleiten an verschiedene Endpunkte** durch Anwenden umfassender Abfragen. Mithilfe des Nachrichtenroutings können Sie Abfragen für Nachrichteneigenschaften und Nachrichtentext sowie für Gerätezwillingstags und Gerätezwillingseigenschaften durchführen. Erfahren Sie mehr über die Verwendung von [Abfragen im Nachrichtenrouting](iot-hub-devguide-routing-query-syntax.md).

IoT Hub benötigt Schreibzugriff auf diese Dienstendpunkte, damit das Nachrichtenrouting funktioniert. Wenn Sie Ihre Endpunkte über das Azure-Portal konfigurieren, werden die erforderlichen Berechtigungen für Sie hinzugefügt. Stellen Sie sicher, dass Sie Ihre Dienste zur Unterstützung des erwarteten Durchsatzes konfigurieren. Wenn Sie z. B. Event Hubs als benutzerdefinierten Endpunkt verwenden, müssen Sie die **Durchsatzeinheiten** für den Event Hub so konfigurieren, dass er den Eingang von Ereignissen behandeln kann, die über IoT Hub-Nachrichtenrouting gesendet werden sollen. Wenn Sie eine Service Bus-Warteschlange als Endpunkt verwenden, müssen Sie auch die **maximale Größe** konfigurieren, damit die Warteschlange alle eingegangenen Daten aufnehmen kann, bis sie aus Consumern ausgehen. Nach der Erstkonfiguration Ihrer IoT-Lösung müssen Sie möglicherweise Ihre zusätzlichen Endpunkte überwachen und ggf. Anpassungen an die tatsächliche Last vornehmen.

Der IoT Hub definiert ein [gemeinsames Format](iot-hub-devguide-messages-construct.md) für alle Gerät-zu-Cloud-Nachrichten, um Interoperabilität zwischen Protokollen zu ermöglichen. Wenn eine Nachricht mehreren Routen entspricht, die auf den gleichen Endpunkt verweisen, übermittelt IoT Hub die Nachricht nur einmal an diesen Endpunkt. Aus diesem Grund müssen Sie keine Deduplizierung für Ihre Service Bus-Warteschlange oder Ihr Service Bus-Thema konfigurieren. In diesem Tutorial lernen Sie, wie Sie das [Nachrichtenrouting konfigurieren](tutorial-routing.md).

## <a name="routing-endpoints"></a>Routingendpunkte

Ein IoT Hub verfügt über einen standardmäßigen integrierten Endpunkt (**messages/events**), der mit Event Hubs kompatibel ist. Sie können [benutzerdefinierte Endpunkte](iot-hub-devguide-endpoints.md#custom-endpoints) für die Weiterleitung von Nachrichten erstellen, indem Sie andere Dienste Ihres Abonnements mit dem IoT Hub verknüpfen. 

Jede Nachricht wird an alle Endpunkte weitergeleitet, die mit ihren Routingabfragen übereinstimmen. Anders ausgedrückt: Eine Nachricht kann an mehrere Endpunkte weitergeleitet werden.

Wenn Ihr benutzerdefinierter Endpunkt über Firewallkonfigurationen verfügt, sollten Sie die [Ausnahme vertrauenswürdiger Microsoft-Erstanbieter](./virtual-network-support.md#egress-connectivity-from-iot-hub-to-other-azure-resources) verwenden.

IoT Hub unterstützt derzeit folgende Endpunkte:

 - Integrierter Endpunkt
 - Azure Storage
 - Service Bus-Warteschlangen und Service Bus-Themen
 - Event Hubs

## <a name="built-in-endpoint-as-a-routing-endpoint"></a>Integrierter Endpunkt als Routingendpunkt

Sie können standardmäßige [Event Hubs-Integration und -SDKs](iot-hub-devguide-messages-read-builtin.md) zum Empfangen von D2C-Nachrichten vom integrierten Endpunkt (**messages/events**) verwenden. Sobald eine Route erstellt wird, werden keine Daten mehr an den integrierten Endpunkt gesendet, es sei denn, eine Route zu diesem Endpunkt wird erstellt. Auch wenn keine Routen erstellt werden, muss eine Fallbackroute aktiviert werden, um Nachrichten an den integrierten Endpunkt weiterzuleiten. Die Fallbackfunktion ist standardmäßig aktiviert, wenn Sie Ihren Hub über das Portal oder die Befehlszeilenschnittstelle erstellen.

## <a name="azure-storage-as-a-routing-endpoint"></a>Azure Storage als Routingendpunkt

Es gibt zwei Speicherdienste, an die IoT Hub Nachrichten weiterleiten kann: [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md)- und [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md) (ADLS Gen2)-Konten. Azure Data Lake Storage-Konten sind Speicherkonten mit aktivierten [hierarchischen Namespaces](../storage/blobs/data-lake-storage-namespace.md), die auf Blob-Speicher basieren. Beide verwenden Blobs als Speicher.

IoT Hub unterstützt das Schreiben von Daten in Azure Storage in den Formaten [Apache Avro](https://avro.apache.org/) und „JSON“. Standardwert: AVRO. Wenn Sie die JSON-Codierung verwenden, müssen Sie in der Nachricht [Systemeigenschaften](iot-hub-devguide-routing-query-syntax.md#system-properties) „contentType“ auf **application/json** und „contentEncoding“ auf **UTF-8** festlegen. Bei diesen beiden Werten wird die Groß-/Kleinschreibung nicht beachtet. Wenn die Inhaltscodierung nicht festgelegt ist, schreibt IoT Hub die Nachrichten in Base64-codiertem Format.

Das Codierungsformat kann nur festgelegt werden, wenn der Endpunkt für Blobspeicher konfiguriert wurde. Bei einem vorhandenen Endpunkt kann es nicht bearbeitet werden. Wenn Sie Codierungsformate bei einem vorhandenen Endpunkt wechseln möchten, müssen Sie den benutzerdefinierten Endpunkt löschen und mit dem gewünschten Format neu erstellen. Eine hilfreiche Strategie könnte das Erstellen eines neuen benutzerdefinierten Endpunkts mit Ihrem gewünschten Codierungsformat und das Hinzufügen einer parallelen Route zu diesem Endpunkt sein. Auf diese Weise können Sie Ihre Daten überprüfen, bevor Sie den vorhandenen Endpunkt löschen.

Sie können das Codierungsformat über die IoT Hub-REST-API „Create“ oder „Update“ (insbesondere [RoutingStorageContainerProperties](/rest/api/iothub/iothubresource/createorupdate#routingstoragecontainerproperties)), das [Azure-Portal](https://portal.azure.com), die [Azure CLI](/cli/azure/iot/hub/routing-endpoint) oder [Azure PowerShell](/powershell/module/az.iothub/add-aziothubroutingendpoint) auswählen. Die folgende Abbildung zeigt, wie Sie das Codierungsformat im Azure-Portal auswählen.

![Endpunktcodierung für Blobspeicher](./media/iot-hub-devguide-messages-d2c/blobencoding.png)

IoT Hub verarbeitet Nachrichten batchweise und schreibt Daten in den Speicher, wenn der Batch eine bestimmte Größe erreicht hat oder ein bestimmter Zeitraum verstrichen ist. IoT Hub folgt standardmäßig der nachstehenden Dateibenennungskonvention:

```
{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
```

Sie können eine beliebige Dateibenennungskonvention verwenden, müssen jedoch alle aufgelisteten Tokens verwenden. IoT Hub schreibt in ein leeres Blob, wenn keine Daten zum Schreiben vorhanden sind.

Wir empfehlen, die Blobs oder Dateien aufzulisten und anschließend zu durchlaufen, um sicherzustellen, dass alle Blobs oder Dateien gelesen werden, ohne dass eine Partition vorhanden ist. Der Partitionsbereich könnte sich möglicherweise bei einem [von Microsoft initiierten Failover](iot-hub-ha-dr.md#microsoft-initiated-failover) oder einem [manuellen Failover](iot-hub-ha-dr.md#manual-failover) in Zusammenhang mit IoT Hub ändern. Sie können die Liste der Blobs oder die [Liste der ADLS Gen2-APIs](/rest/api/storageservices/datalakestoragegen2/path) mithilfe der [Liste der Blobs-APIs](/rest/api/storageservices/list-blobs) aufzählen, um die gewünschte Liste von Dateien zu erhalten. Das folgende Beispiel dient als Anleitung.

```csharp
public void ListBlobsInContainer(string containerName, string iothub)
{
    var storageAccount = CloudStorageAccount.Parse(this.blobConnectionString);
    var cloudBlobContainer = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
    if (cloudBlobContainer.Exists())
    {
        var results = cloudBlobContainer.ListBlobs(prefix: $"{iothub}/");
        foreach (IListBlobItem item in results)
        {
            Console.WriteLine(item.Uri);
        }
    }
}
```

Wenn Sie ein Azure Data Lake Gen2-kompatibles Speicherkonto erstellen möchten, erstellen Sie ein neues V2-Speicherkonto, und wählen Sie auf der Registerkarte *Erweitert* im Feld *Hierarchischer Namespace* die Option **aktiviert** aus, wie in der folgenden Abbildung gezeigt wird:

![Auswählen von Azure Data Lake Gen2-Speicher](./media/iot-hub-devguide-messages-d2c/selectadls2storage.png)

## <a name="service-bus-queues-and-service-bus-topics-as-a-routing-endpoint"></a>Service Bus-Warteschlangen und Service Bus-Themen als Routingendpunkt

Für Service Bus-Warteschlangen und -Themen, die als IoT Hub-Endpunkte verwendet werden, dürfen **Sitzungen** oder **Duplikaterkennung** nicht aktiviert werden. Wenn eine dieser Optionen aktiviert ist, wird der Endpunkt im Azure-Portal als **Nicht erreichbar** angezeigt.

## <a name="event-hubs-as-a-routing-endpoint"></a>Event Hubs als Routingendpunkt

Sie können Daten nicht nur an den mit Event Hubs kompatiblen integrierten Endpunkt, sondern auch an benutzerdefinierte Endpunkte vom Typ „Event Hubs“ weiterleiten. 

## <a name="reading-data-that-has-been-routed"></a>Lesen weitergeleiteter Daten

Sie können mit diesem [Tutorial](tutorial-routing.md) eine Route konfigurieren.

Verwenden Sie die folgenden Tutorials, um zu erfahren, wie Sie Nachrichten aus einem Endpunkt lesen:

* Lesen vom [integrierten Endpunkt](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-nodejs)

* Lesen aus [Blob Storage](../storage/blobs/storage-blob-event-quickstart.md)

* Lesen aus [Event Hubs](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

* Lesen aus [Service Bus-Warteschlangen](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)

* Lesen aus [Service Bus-Themen](../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md)


## <a name="fallback-route"></a>Fallbackroute

Die Fallbackroute sendet alle Nachrichten, die die Abfragebedingungen in einer der vorhandenen Routen nicht erfüllen, an den integrierten Endpunkt (**messages/events**), der mit [Event Hubs](../event-hubs/index.yml) kompatibel ist. Wenn das Nachrichtenrouting aktiviert ist, können Sie die Funktion der Fallbackroute verwenden. Sobald eine Route erstellt wird, werden keine Daten mehr an den integrierten Endpunkt gesendet, es sei denn, eine Route zu diesem Endpunkt wird erstellt. Wenn keine Routen zum integrierten Endpunkt vorhanden sind und eine Fallbackroute aktiviert ist, werden nur Nachrichten an den integrierten Endpunkt gesendet, die keinen Abfragebedingungen in Routen entsprechen. Wenn alle vorhandenen Routen gelöscht wurden, muss eine Fallbackroute aktiviert werden, um alle Daten im integrierten Endpunkt zu empfangen. 

Sie können die Fallbackroute im Azure-Portal auf dem Blatt „Nachrichtenrouting“ aktivieren und deaktivieren. Sie können auch Azure Resource Manager verwenden, um [FallbackRouteProperties](/rest/api/iothub/iothubresource/createorupdate#fallbackrouteproperties) für die Nutzung eines benutzerdefinierten Endpunkts für die Fallbackroute festzulegen.

## <a name="non-telemetry-events"></a>Nicht telemetriebezogene Ereignisse

Das Nachrichtenrouting ermöglicht zusätzlich zum Weiterleiten von Gerätetelemetriedaten auch das Senden von Änderungsereignissen bei Gerätezwillingen, Ereignissen beim Gerätelebenszyklus, Änderungsereignissen bei digitalen Zwillingen und Ereignissen beim Geräteverbindungsstatus. Wenn beispielsweise eine Route erstellt wird, deren Datenquelle auf **Änderungsereignisse für Gerätezwillinge** festgelegt ist, sendet IoT Hub Nachrichten an den Endpunkt, der die Änderung im Gerätezwilling enthält. Ebenso gilt: Wenn eine Route erstellt wird, deren Datenquelle auf **Ereignisse im Gerätelebenszyklus** festgelegt ist, sendet IoT Hub eine Nachricht, die mitteilt, ob das Gerät erstellt oder gelöscht wurde. Ein Entwickler kann im Rahmen von [Azure IoT Plug & Play](../iot-develop/overview-iot-plug-and-play.md) Routen erstellen, deren Datenquelle auf **Änderungsereignisse bei digitalen Zwillingen** festgelegt wurde. Dann sendet IoT Hub Nachrichten, wenn eine Eigenschaft des digitalen Zwillings festgelegt oder geändert wird, ein digitaler Zwilling ersetzt wird oder beim zugrunde liegenden Gerätezwilling ein Änderungsereignis eintritt. Und schließlich: Wenn eine Route erstellt wird, deren Datenquelle auf **Ereignisse beim Geräteverbindungsstatus** festgelegt wurde, sendet IoT Hub eine Nachricht, die mitteilt, ob das Gerät verbunden oder die Verbindung getrennt wurde.


[IoT Hub lässt sich auch in Azure Event Grid integrieren](iot-hub-event-grid.md), um Geräteereignisse zu veröffentlichen und so Echtzeitintegrationen und die Automatisierung von Workflows basierend auf diesen Ereignissen zu unterstützen. Informationen dazu, welche Methode sich am besten für Ihr Szenario eignet, finden Sie unter [Vergleichen von Nachrichtenweiterleitung und Event Grid für IoT Hub](iot-hub-event-grid-routing-comparison.md).

## <a name="limitations-for-device-connection-state-events"></a>Einschränkungen für Ereignisse beim Geräteverbindungsstatus

Zum Empfang von Ereignissen beim Geräteverbindungsstatus muss ein Gerät entweder die *Gerät-zu-Cloud-Sendetelemetrie* oder einen *Cloud-zu-Gerät-Empfangsnachrichtenvorgang* mit IoT Hub aufrufen. Wenn aber ein Gerät für die Verbindung mit IoT Hub das AMQP-Protokoll verwendet, empfehlen wir, dass das Gerät den *Cloud-zu-Gerät-Vorgang zum Empfangen einer Nachricht* aufruft. Andernfalls werden die Benachrichtigungen zum Verbindungszustand möglicherweise um einige Minuten verzögert. Wenn Ihr Gerät eine Verbindung mit dem MQTT-Protokoll herstellt, bleibt die Cloud-zu-Gerät-Verbindung in IoT Hub geöffnet. Zum Öffnen des Cloud-zu-Gerät-Links für AMQP rufen Sie die [Receive Async API](/rest/api/iothub/device/receivedeviceboundnotification) (Asynchrone API zum Empfangen) auf.

Die Gerät-zu-Cloud-Verbindung bleibt geöffnet, solange das Gerät Telemetriedaten sendet.

Wenn die Geräteverbindung flackert, d. h., wenn das Gerät eine Verbindung häufig herstellt und trennt, sendet IoT Hub nicht jeden einzelnen Verbindungsstatus, sondern veröffentlicht den aktuellen Verbindungsstatus, der bei einer regelmäßigen Momentaufnahme von 60 Sekunden erfasst wurde, bis das Flackern beendet wird. Wenn dasselbe Verbindungszustandsereignis mit unterschiedlichen Folgenummern oder unterschiedliche Verbindungszustandsereignissen empfangen werden, bedeutet dies, dass sich der Geräteverbindungsstatus geändert hat.

## <a name="testing-routes"></a>Testen von Routen

Wenn Sie eine neue Route erstellen oder eine vorhandene Route bearbeiten, sollten Sie die Routenabfrage mit einer Beispielnachricht testen. Sie können einzelne Routen oder alle Routen gleichzeitig testen. Während des Tests werden keine Nachrichten an die Endpunkte weitergeleitet. Zum Testen können Sie das Azure-Portal, Azure Resource Manager, Azure PowerShell oder die Azure CLI verwenden. Anhand der Ausgaben können Sie herausfinden, ob die Beispielnachricht der Abfrage entspricht oder nicht oder ob der Test nicht ausgeführt werden konnte, weil Beispielnachricht oder Abfragesyntax falsch sind. Weitere Informationen finden Sie unter [Testen einer Route](/rest/api/iothub/iothubresource/testroute) und [Testen aller Routen](/rest/api/iothub/iothubresource/testallroutes).

## <a name="latency"></a>Latency

Wenn Sie Geräte-zu-Cloud-Telemetrienachrichten über integrierte Endpunkte weiterleiten, kommt es nach der Erstellung der ersten Route zu einer leichten Erhöhung der Gesamtlatenz.

In den meisten Fällen beträgt der durchschnittliche Latenzanstieg weniger als 500 ms. Sie können die Latenz mithilfe der IoT Hub-Metriken **Routing: Nachrichtenlatenz für „messages/events“** oder **d2c.endpoints.latency.builtIn.events** überwachen. Das Erstellen oder Löschen einer Route nach der ersten hat keinen Einfluss auf die Gesamtlatenz.

## <a name="monitoring-and-troubleshooting"></a>Überwachung und Problembehandlung

IoT Hub bietet mehrere Metriken in Bezug auf Routing und Endpunkte, um Ihnen einen Überblick über die Integrität Ihres Hubs und der gesendeten Nachrichten zu verschaffen. Eine Liste der IoT Hub-Metriken, aufgeschlüsselt nach Funktionskategorien, finden Sie in der [Referenz zur Überwachung von Daten im Abschnitt „Metriken“](monitor-iot-hub-reference.md#metrics). Mit der [Kategorie **Routen** in den IoT Hub-Ressourcenprotokollen](monitor-iot-hub-reference.md#routes) können Sie Fehler bei der Auswertung einer Routingabfrage und der Endpunktintegrität nachverfolgen, die von IoT Hub registriert werden. Weitere Informationen zur Verwendung von Metriken und Ressourcenprotokollen mit IoT Hub finden Sie unter [Überwachen von IoT Hub](monitor-iot-hub.md).

Sie können die REST-API [Get Endpoint Health](/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) (Endpunktintegrität abrufen) verwenden, um den [Integritätsstatus](iot-hub-devguide-endpoints.md#custom-endpoints) der Endpunkte abzurufen.

Im [Leitfaden zur Problembehandlung beim Routing](troubleshoot-message-routing.md) finden Sie weitere Informationen und Unterstützung bei der Behandlung von Routingproblemen.

## <a name="next-steps"></a>Nächste Schritte

* Wie Sie Nachrichtenrouten erstellen, erfahren Sie unter [Verarbeiten von IoT Hub-D2C-Nachrichten mit Routen](tutorial-routing.md).

* [Senden von D2C-Nachrichten](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-nodejs)

* Informationen zu den SDKs, die Sie zum Senden von D2C-Nachrichten verwenden können, finden Sie unter [Azure IoT-SDKs](iot-hub-devguide-sdks.md).