---
title: Verteilte Ablaufverfolgung für Telemetriedaten in Azure Application Insights | Microsoft-Dokumentation
description: Enthält Informationen zur Microsoft-Unterstützung für verteilte Ablaufverfolgung durch unsere Partnerschaft im OpenCensus-Projekt
ms.topic: conceptual
ms.custom: devx-track-dotnet
author: nikmd23
ms.author: nimolnar
ms.date: 09/17/2018
ms.reviewer: mbullwin
ms.openlocfilehash: c4f32c0803bb5815599c81ca6c89335fa6e1ffac
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131455167"
---
# <a name="what-is-distributed-tracing"></a>Was ist die verteilte Ablaufverfolgung?

Mit dem Aufkommen moderner Cloud- und [Microservicearchitekturen](https://azure.com/microservices) sind einfache, unabhängig einsetzbare Dienste entstanden, die zur Kostensenkung bei gleichzeitiger Steigerung von Verfügbarkeit und Durchsatz beitragen können. Aber obwohl diese Bewegungen einzelne Dienste als Ganzes besser verständlich gemacht haben, führten sie zu komplizierteren Gesamtsystemen, die schwerer zu debuggen sind.

In monolithischen Architekturen haben wir uns an das Debugging mit Aufruflisten gewöhnt. Aufruflisten sind brillante Werkzeuge, um den Ablauf der Ausführung (Methode A ruft Methode B auf, die Methode C aufruft) sowie Details und Parameter zu jedem dieser Aufrufe anzuzeigen. Dies ist ideal für monolithische Architekturen oder Dienste, die auf einem einzelnen Prozess ausgeführt werden, aber wie erfolgt das Debuggen, wenn der Aufruf über eine Prozessgrenze hinausgeht und nicht nur ein Verweis auf den lokalen Stapel ist? 

Hier kommt die verteilte Ablaufverfolgung ins Spiel.  

Die verteilte Ablaufverfolgung ist das Äquivalent zu Aufruflisten für moderne Cloud- und Microservicesarchitekturen, ergänzt durch einen vereinfachten Leistungsprofiler. In Azure Monitor stehen zwei Oberflächen zur Nutzung der verteilten Ablaufverfolgungsdaten zur Verfügung. Die erste ist unsere [Transaktionsdiagnose](./transaction-diagnostics.md)-Ansicht, die einer Aufrufliste mit zusätzlicher Zeitdimension gleicht. Die Transaktionsdiagnose-Ansicht bietet Einblick in eine einzelne Transaktion/Anforderung und ist hilfreich, um die Ursache für Zuverlässigkeitsprobleme und Leistungsengpässe auf Anforderungsbasis zu ermitteln.

Zudem bietet Azure Monitor eine [Anwendungsübersicht](./app-map.md)-Ansicht, die viele Transaktionen aggregiert, um eine topologische Ansicht der Interaktion der Systeme sowie der durchschnittlichen Leistung und Fehlerraten zu erhalten. 

## <a name="how-to-enable-distributed-tracing"></a>Aktivieren der verteilten Ablaufverfolgung

Das Aktivieren der verteilten Ablaufverfolgung über die Dienste in einer Anwendung hinweg ist so einfach wie das Hinzufügen des richtigen Agents, des richtigen SDK oder der richtigen Bibliothek zu jedem Dienst, basierend auf der Sprache, in der der Dienst implementiert wurde.

## <a name="enabling-via-application-insights-through-auto-instrumentation-or-sdks"></a>Aktivieren über Application Insights per automatischer Instrumentierung oder SDKs

Die verteilte Ablaufverfolgung wird von den Application Insights-Agents und/oder SDKs für .NET, .NET Core, Java, Node.js und JavaScript bereits nativ unterstützt. Anweisungen zum Installieren und Konfigurieren der einzelnen Application Insights SDKs finden Sie unter:

* [.NET](asp-net.md)
* [.NET Core](asp-net-core.md)
* [Java](./java-in-process-agent.md)
* [Node.js](../app/nodejs-quick-start.md)
* [JavaScript](./javascript.md)
* [Python](opencensus-python.md)

Wenn das richtige Application Insights SDK installiert und konfiguriert ist, werden Ablaufverfolgungsinformationen für gängige Frameworks, Bibliotheken und Technologien automatisch von der automatischen Erfassung (Auto-Collectors) für SDK-Abhängigkeiten gesammelt. Die vollständige Liste der unterstützten Technologien finden Sie unter [Automatisches Sammeln von Abhängigkeiten](./auto-collect-dependencies.md).

 Darüber hinaus kann jede beliebige Technologie manuell durch einen Aufruf von [TrackDependency](./api-custom-events-metrics.md) auf dem [TelemetryClient](./api-custom-events-metrics.md) überwacht werden.

## <a name="enable-via-opentelemetry"></a>Aktivieren über OpenTelemetry

Application Insights unterstützt jetzt die verteilte Ablaufverfolgung via [OpenTelemetry](https://opentelemetry.io/). OpenTelemetry bietet eine anbieterneutrale Instrumentierung zum Senden von Ablaufverfolgungen, Metriken und Protokollen an Application Insights. Zunächst hat sich die OpenTelemetry-Gemeinschaft dem Distributed Tracing angenommen. Metriken und Protokolle befinden sich noch in der Entwicklung. Eine vollständige Beobachtungsmöglichkeit umfasst alle drei Säulen, doch derzeit beinhalten unsere [Azure Monitor OpenTelemetry-basierten Exporter-Vorschau-Angebote für .NET, Python und JavaScript](opentelemetry-enable.md) nur Distributed Tracing. Unser Java OpenTelemetry-basiertes Azure Monitor-Angebot ist jedoch unbeschränkt und wird vollständig unterstützt. 

Auf den folgenden Seiten finden Sie für jede Sprache eine Anleitung zur Aktivierung und Konfiguration der OpenTelemetry-basierten Angebote von Microsoft. Wichtig ist, dass wir die verfügbaren Funktionen und Einschränkungen der einzelnen Angebote erläutern, damit Sie entscheiden können, ob OpenTelemetry für Ihr Projekt geeignet ist.

* [.NET](opentelemetry-enable.md?tabs=net)
* [Java](java-in-process-agent.md)
* [Node.js](opentelemetry-enable.md?tabs=nodejs)
* [Python](opentelemetry-enable.md?tabs=python)

## <a name="enable-via-opencensus"></a>Aktivieren über OpenCensus

Zusätzlich zu den Application Insights SDKs unterstützt Application Insights auch die verteilte Ablaufverfolgung durch [OpenCensus](https://opencensus.io/). OpenCensus ist eine herstellerunabhängige, Open Source-Einzelverteilung von Bibliotheken, mit denen Funktionen für das Sammeln von Metriken und für die verteilte Ablaufverfolgung für Dienste bereitgestellt werden. Darüber hinaus ermöglicht es der Open Source-Community, die verteilte Ablaufverfolgung mit gängigen Technologien wie Redis, Memcached oder MongoDB zu durchzuführen. [Microsoft arbeitet bei OpenCensus mit mehreren anderen Überwachungs- und Cloud-Partnern zusammen](https://open.microsoft.com/2018/06/13/microsoft-joins-the-opencensus-project/).

[Python](opencensus-python.md) 

Auf der OpenCensus-Website finden Sie API-Referenzdokumentation für [Python](https://opencensus.io/api/python/trace/usage.html) und [Go](https://godoc.org/go.opencensus.io), sowie verschiedene andere Anleitungen für die Verwendung von OpenCensus. 

## <a name="next-steps"></a>Nächste Schritte

* [OpenCensus Python – Benutzerhandbuch](https://opencensus.io/api/python/trace/usage.html)
* [Anwendungszuordnung](./app-map.md)
* [End-to-End-Leistungsüberwachung](../app/tutorial-performance.md)

