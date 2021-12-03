---
title: Leistungsüberwachung für Java-Web-Apps – Azure Application Insights
description: Erweiterte Leistungs- und Nutzungsüberwachung Ihrer Java-Website mit Application Insights.
ms.topic: conceptual
ms.date: 01/10/2019
ms.custom: devx-track-java
author: mattmccleary
ms.author: mmcc
ms.openlocfilehash: aff76a632613da7ea839e4398677d0ab82c6864b
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131078957"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Überwachen von Abhängigkeiten, abgefangene Ausnahmen und Methodenausführungszeiten in Java-Web-Apps

> [!CAUTION]
> Dieses Dokument gilt für Application Insights Java 2.x. Diese Version wird jedoch nicht mehr empfohlen.
>
> Die Dokumentation für die neueste Version finden Sie unter [Application Insights Java 3.x](./java-in-process-agent.md).

Wenn Sie [Ihre Java-Web-App mit dem Application Insights SDK instrumentiert haben][java], können Sie den Java-Agent ohne Codeänderungen verwenden, um tiefer gehende Erkenntnisse zu erhalten:

* **Abhängigkeiten**: Daten über Aufrufe der Anwendung an andere Komponenten, einschließlich:
  * **Ausgehende HTTP-Aufrufe** über Apache HttpClient, OkHttp und `java.net.HttpURLConnection` werden erfasst.
  * **Redis-Aufrufe** über den Jedis-Client werden erfasst.
  * **JDBC-Abfragen**: Für MySQL und PostgreSQL gilt: Wenn der Aufruf länger als zehn Sekunden dauert, wird der Abfrageplan gemeldet.

* **Anwendungsprotokollierung**: Sie können Ihre Anwendungsprotokolle erfassen und mit HTTP-Anforderungen und anderer Telemetrie in Beziehung setzen.
  * **Log4j 1.2**
  * **Log4j2**
  * **Logback**

* **Bessere Vorgangsbenennung:** (für die Aggregation von Anforderungen im Portal verwendet)
  * **Spring** – basiert auf `@RequestMapping`.
  * **JAX-RS** – basiert auf `@Path`. 

Um den Java-Agent zu verwenden, installieren Sie ihn auf Ihrem Server. Ihre Web-Apps müssen mit dem [Application Insights Java SDK][java] instrumentiert werden. 

## <a name="install-the-application-insights-agent-for-java"></a>Installieren des Application Insights-Agents für Java
1. Laden Sie auf dem Computer, auf dem Ihr Java-Server ausgeführt wird, [den Agent der Version 2.x herunter](https://github.com/microsoft/ApplicationInsights-Java/releases/tag/2.6.2). Stellen Sie sicher, dass die verwendete Version des 2.x-Java-Agents mit der Version Ihres 2.x-Application Insights-Java SDK übereinstimmt.
2. Bearbeiten Sie das Startskript des Anwendungsservers, und fügen Sie das folgende JVM-Argument hinzu:
   
    `-javaagent:<full path to the agent JAR file>`
   
    Beispielsweise in Tomcat auf einem Linux-Computer:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Starten Sie den Anwendungsserver neu.

## <a name="configure-the-agent"></a>Konfigurieren des Agents
Erstellen Sie eine Datei mit dem Namen `AI-Agent.xml` , und speichern Sie sie im selben Ordner wie die JAR-Datei des Agents.

Legen Sie den Inhalt der XML-Datei fest. Bearbeiten Sie das folgende Beispiel, um Features Ihrer Wahl aufzunehmen oder nicht.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsightsAgent>
   <Instrumentation>
      <BuiltIn enabled="true">

         <!-- capture logging via Log4j 1.2, Log4j2, and Logback, default is true -->
         <Logging enabled="true" />

         <!-- capture outgoing HTTP calls performed through Apache HttpClient, OkHttp,
              and java.net.HttpURLConnection, default is true -->
         <HTTP enabled="true" />

         <!-- capture JDBC queries, default is true -->
         <JDBC enabled="true" />

         <!-- capture Redis calls, default is true -->
         <Jedis enabled="true" />

         <!-- capture query plans for JDBC queries that exceed this value (MySQL, PostgreSQL),
              default is 10000 milliseconds -->
         <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>

      </BuiltIn>
   </Instrumentation>
</ApplicationInsightsAgent>
```

## <a name="additional-config-spring-boot"></a>Zusätzliche Konfiguration (Spring Boot)

`java -javaagent:/path/to/agent.jar -jar path/to/TestApp.jar`

Führen Sie für Azure App Services die folgenden Schritte aus:

* Wählen Sie „Einstellungen“ > „Anwendungseinstellungen“ aus.
* Fügen Sie unter „App-Einstellungen“ ein neues Schlüssel-Wert-Paar hinzu:

Schlüssel: `JAVA_OPTS` Wert: `-javaagent:D:/home/site/wwwroot/applicationinsights-agent-2.6.2.jar`

Der Agent muss als Ressource in Ihrem Projekt enthalten sein, sodass er sich letztendlich im Verzeichnis „D:/home/site/wwwroot/“ befindet. Sie können sich vergewissern, dass der Agent im richtigen App Service-Verzeichnis enthalten ist, indem Sie zu **Entwicklungstools** > **Erweiterte Tools** > **Debugging-Konsole** wechseln und den Inhalt des Siteverzeichnisses prüfen.    

* Speichern Sie die Einstellungen, und starten Sie die App neu. (Diese Schritte gelten nur für App Services unter Windows.)

> [!NOTE]
> „AI-Agent.xml“ und die Agent-JAR-Datei sollten sich im selben Ordner befinden. Sie werden häufig zusammen im `/resources`-Ordner des Projekts platziert.  

#### <a name="enable-w3c-distributed-tracing"></a>Aktivieren der verteilten W3C-Ablaufverfolgung

Fügen Sie der Datei „AI-Agent.xml“ Folgendes hinzu:

```xml
<Instrumentation>
   <BuiltIn enabled="true">
      <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
   </BuiltIn>
</Instrumentation>
```

> [!NOTE]
> Der Abwärtskompatibilitätsmodus ist standardmäßig aktiviert, und der Parameter „enableW3CBackCompat“ ist optional und sollte nur verwendet werden, wenn Sie diese Funktion deaktivieren möchten. 

Idealerweise wäre dies der Fall, wenn alle Dienste auf neuere Versionen der SDKs aktualisiert wurden, die das W3C-Protokoll unterstützen. Es wird dringend empfohlen, so bald wie möglich auf neuere Versionen der SDKs mit W3C-Unterstützung umzustellen.

Stellen Sie sicher, dass die **Konfigurationen für [eingehende](correlation.md#enable-w3c-distributed-tracing-support-for-java-apps) und ausgehende Vorgänge (Agent)** exakt gleich sind.

## <a name="view-the-data"></a>Anzeigen der Daten
In der Application Insights-Ressource werden aggregierte Remoteabhängigkeiten und Methodenausführungszeiten [auf der Kachel „Leistung“][metrics] angezeigt.

Um nach den einzelnen Instanzen der Abhängigkeits-, Ausnahmen- und Methodenberichte zu suchen, öffnen Sie die [Suche][diagnostic].

[Diagnostizieren von Problemen mit Abhängigkeiten – weitere Informationen](./asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Fragen? Probleme?
* Sie sehen keine Daten? [Festlegen von Firewallausnahmen](./ip-addresses.md)
* [Problembehandlung für Java](java-2x-troubleshoot.md)

<!--Link references-->

[api]: ./api-custom-events-metrics.md
[apiexceptions]: ./api-custom-events-metrics.md#track-exception
[availability]: ./monitor-web-app-availability.md
[diagnostic]: ./diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-in-process-agent.md
[javalogs]: java-2x-trace-logs.md
[metrics]: ../essentials/metrics-charts.md

