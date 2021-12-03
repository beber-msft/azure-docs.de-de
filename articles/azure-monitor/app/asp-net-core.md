---
title: Azure Application Insights für ASP.NET Core-Anwendungen | Microsoft-Dokumentation
description: Überwachen Sie ASP.NET Core-Webanwendungen auf Verfügbarkeit, Leistung und Auslastung.
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.date: 10/12/2021
ms.openlocfilehash: 521cebd9117c1150e8c2abc2f5ff752e195a1808
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131070050"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights für ASP.NET Core-Anwendungen

In diesem Artikel wird beschrieben, wie Sie Application Insights für eine [ASP.NET Core](/aspnet/core)-Anwendung aktivieren. Nach Abschluss der Schritte in diesem Artikel erfasst Application Insights Anforderungen, Abhängigkeiten, Ausnahmen, Leistungsindikatoren, Heartbeats und Protokolle für Ihre ASP.NET Core-Anwendung.

Sie verwenden im folgenden Beispiel eine [MVC-Anwendung](/aspnet/core/tutorials/first-mvc-app) für `netcoreapp3.0`. Die Anleitung in diesem Artikel können Sie allerdings für alle ASP.NET Core-Anwendungen nutzen. Wenn Sie den [Workerdienst](/aspnet/core/fundamentals/host/hosted-services#worker-service-template) verwenden, befolgen Sie die [hier](./worker-service.md) angegebenen Anweisungen.

> [!NOTE]
> Eine Vorschau zum [OpenTelemetry-basierten .NET-Angebot](opentelemetry-enable.md?tabs=net) ist verfügbar. [Weitere Informationen](opentelemetry-overview.md).

## <a name="supported-scenarios"></a>Unterstützte Szenarios

Mit dem [Application Insights SDK für ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) können Sie Anwendungen unabhängig davon überwachen, wo und wie sie ausgeführt werden. Wenn Ihre Anwendung ausgeführt wird und über eine Netzwerkverbindung mit Azure verfügt, können Telemetriedaten erfasst werden. Die Application Insights-Überwachung wird in allen Umgebungen unterstützt, in denen auch .NET Core unterstützt wird. Die Unterstützung deckt Folgendes ab:
* **Betriebssystem**: Windows, Linux oder Mac
* **Hostingmethode**: Prozessintern oder prozessextern
* **Bereitstellungsmethode**: Abhängig vom Framework oder eigenständig
* **Webserver**: IIS (Internetinformationsdienste) oder Kestrel
* **Hostingplattform**: Das Web-Apps-Feature von Azure App Service, Azure VM, Docker, Azure Kubernetes Service (AKS) usw.
* **.NET Core-Version**: Alle offiziell [unterstützten .NET Core-Versionen](https://dotnet.microsoft.com/download/dotnet-core), die sich nicht in der Vorschauphase befinden
* **IDE**: Visual Studio, Visual Studio Code oder Befehlszeile

> [!NOTE]
> ASP.NET Core 3.1 erfordert [Application Insights 2.8.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/2.8.0) oder höher.

## <a name="prerequisites"></a>Voraussetzungen

- Eine funktionierende ASP.NET Core-Anwendung. Wenn Sie eine ASP.NET Core-Anwendung erstellen müssen, führen Sie die Schritte im entsprechenden [ASP.NET Core-Tutorial](/aspnet/core/getting-started/) aus.
- Ein gültiger Application Insights-Instrumentierungsschlüssel. Dieser ist erforderlich, um Telemetriedaten an Application Insights zu senden. Wenn Sie eine neue Application Insights-Ressource erstellen müssen, um einen Instrumentierungsschlüssel abzurufen, finden Sie unter [Erstellen einer Application Insights-Ressource](./create-new-resource.md) weitere Informationen.

> [!IMPORTANT]
> [Verbindungszeichenfolgen](./sdk-connection-string.md?tabs=net) sind Instrumentierungsschlüsseln vorzuziehen. Neue Azure-Regionen erfordern **zwingend** die Verwendung von Verbindungszeichenfolgen anstelle von Instrumentierungsschlüsseln. Die Verbindungszeichenfolge identifiziert die Ressource, der Sie Ihre Telemetriedaten zuordnen möchten. Hier können Sie die Endpunkte ändern, die Ihre Ressource als Ziel für die Telemetrie verwendet. Sie müssen die Verbindungszeichenfolge kopieren und dem Code Ihrer Anwendung oder einer Umgebungsvariable hinzufügen.


## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Aktivieren der serverseitigen Telemetrie für Application Insights (Visual Studio)

Verwenden Sie für Visual Studio für Mac den [Leitfaden für manuelles Aktivieren](#enable-application-insights-server-side-telemetry-no-visual-studio). Dieses Verfahren wird nur von der Windows-Version von Visual Studio unterstützt.

1. Öffnen Sie Ihr Projekt in Visual Studio.

    > [!TIP]
    > Um alle von Application Insights vorgenommenen Änderungen nachzuverfolgen, können Sie die Quellcodeverwaltung für Ihr Projekt einrichten. Zum Einrichten wählen Sie **Datei** > **Zur Quellcodeverwaltung hinzufügen** aus.

2. Wählen Sie **Projekt** > **Application Insights-Telemetrie hinzufügen** aus.

3. Wählen Sie **Erste Schritte** aus. Je nach Visual Studio-Version kann der Name dieser Schaltfläche abweichen. Bei einigen älteren Versionen heißt die Schaltfläche **Kostenlos starten**.

4. Wählen Sie Ihr Abonnement und dann **Ressource** > **Registrieren** aus.

5. Überprüfen Sie nach dem Hinzufügen von Application Insights zu Ihrem Projekt, ob Sie das neueste stabile Release des SDK verwenden. Wechseln Sie zu **Projekt** > **NuGet-Pakete verwalten** > **Microsoft.ApplicationInsights.AspNetCore**. Bei Bedarf wählen Sie **Aktualisieren** aus.

     ![Screenshot: Auswahl des Application Insights-Pakets, das aktualisiert werden soll](./media/asp-net-core/update-nuget-package.png)

6. Wenn Sie für Ihr Projekt die Quellcodeverwaltung aktiviert haben, wechseln Sie zu **Ansicht** > **Team Explorer** > **Änderungen**. Sie können die einzelnen Dateien auswählen, um sich eine Vergleichsansicht für Änderungen anzeigen zu lassen, die vom Application Insights-Telemetriefeature vorgenommen wurden.

## <a name="enable-application-insights-server-side-telemetry-no-visual-studio"></a>Aktivieren der serverseitigen Telemetrie für Application Insights (ohne Visual Studio)

1. Installieren Sie das [Application Insights SDK-NuGet-Paket für ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). Sie sollten immer die neueste stabile Version verwenden. Vollständige Versionshinweise für das SDK finden Sie im [Open-Source-GitHub-Repository](https://github.com/Microsoft/ApplicationInsights-dotnet/releases).

    Im folgenden Beispielcode wird gezeigt, welche Elemente Sie der `.csproj`-Datei Ihres Projekts hinzufügen müssen.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.16.0" />
        </ItemGroup>
    ```

2. Fügen Sie wie im folgenden Beispiel dargestellt `services.AddApplicationInsightsTelemetry();` der `ConfigureServices()`-Methode in Ihrer `Startup`-Klasse hinzu:

    ```csharp
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // The following line enables Application Insights telemetry collection.
            services.AddApplicationInsightsTelemetry();
    
            // This code adds other services for your application.
            services.AddMvc();
        }
    ```

3. Richten Sie den Instrumentierungsschlüssel ein.

    Sie können einen Instrumentierungsschlüssel als Argument für `AddApplicationInsightsTelemetry` bereitstellen. Empfohlen wird aber, den Instrumentierungsschlüssel in der Konfiguration anzugeben. Im folgenden Beispielcode wird veranschaulicht, wie Sie in `appsettings.json` einen Instrumentierungsschlüssel angeben. Stellen Sie sicher, dass `appsettings.json` während der Veröffentlichung in den Stammordner der Anwendung kopiert wird.

    ```json
        {
          "ApplicationInsights": {
            "InstrumentationKey": "putinstrumentationkeyhere"
          },
          "Logging": {
            "LogLevel": {
              "Default": "Warning"
            }
          }
        }
    ```

    Alternativ können Sie den Instrumentierungsschlüssel auch in einer der folgenden Umgebungsvariablen angeben:

    * `APPINSIGHTS_INSTRUMENTATIONKEY`

    * `ApplicationInsights:InstrumentationKey`

    Beispiel:

    * `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    * `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    * `APPINSIGHTS_INSTRUMENTATIONKEY` wird in der Regel in [Azure-Web-Apps](./azure-web-apps.md?tabs=net) verwendet, kann aber auch überall verwendet werden, wo dieses SDK unterstützt wird. (Wenn Sie eine Überwachung von Web-Apps ohne Code durchführen, ist dieses Format erforderlich, sofern Sie keine Verbindungszeichenfolgen verwenden.)

    Anstatt Instrumentierungsschlüssel festzulegen, können Sie jetzt auch [Verbindungszeichenfolgen](./sdk-connection-string.md?tabs=net) verwenden.

    > [!NOTE]
    > Ein Instrumentierungsschlüssel, der im Code angegeben ist, erhält Vorrang vor der Umgebungsvariable `APPINSIGHTS_INSTRUMENTATIONKEY`, die wiederum Vorrang vor anderen Optionen hat.

### <a name="user-secrets-and-other-configuration-providers"></a>Benutzergeheimnisse und andere Konfigurationsanbieter

Wenn Sie den Instrumentierungsschlüssel in ASP.NET Core-Benutzergeheimnissen speichern oder von einem anderen Konfigurationsanbieter abrufen möchten, können Sie die Überladung mit einem Parameter `Microsoft.Extensions.Configuration.IConfiguration` verwenden. Beispiel: `services.AddApplicationInsightsTelemetry(Configuration);`.
Ab Version [2.15.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) von Microsoft.ApplicationInsights.AspNetCore wird durch Aufrufen von `services.AddApplicationInsightsTelemetry()` automatisch der Instrumentierungsschlüssel aus `Microsoft.Extensions.Configuration.IConfiguration` der Anwendung gelesen. `IConfiguration` muss nicht explizit angegeben werden.

## <a name="run-your-application"></a>Ausführen der Anwendung

Führen Sie Ihre Anwendung aus, und senden Sie Anforderungen an diese. Nun sollten Telemetriedaten an Application Insights übermittelt werden. Mit dem Application Insights SDK werden eingehende Webanforderungen an die Anwendung sowie die folgenden Telemetriedaten automatisch erfasst.

### <a name="live-metrics"></a>Livemetriken

Mit [Livemetriken](./live-stream.md) kann schnell überprüft werden, ob die Application Insights-Überwachung ordnungsgemäß konfiguriert ist. Es kann einige Minuten dauern, bis Telemetriedaten im Portal und in der Analyse angezeigt werden. Livemetriken zeigen die CPU-Auslastung des laufenden Prozesses jedoch nahezu in Echtzeit an. Außerdem können andere Telemetriedaten wie z. B. Anforderungen, Abhängigkeiten und Ablaufverfolgungen angezeigt werden.

### <a name="ilogger-logs"></a>ILogger-Protokolle

Die Standardkonfiguration sammelt `ILogger`-Protokolle mit dem Schweregrad `Warning` und höher. Sie können diese [Konfiguration anpassen](#how-do-i-customize-ilogger-logs-collection).

### <a name="dependencies"></a>Abhängigkeiten

Die Abhängigkeitssammlung ist standardmäßig aktiviert. In [diesem Artikel](asp-net-dependencies.md#automatically-tracked-dependencies) werden die Abhängigkeiten, die automatisch gesammelt werden, sowie die Schritte zur manuellen Nachverfolgung erläutert.

### <a name="performance-counters"></a>Leistungsindikatoren

Für die Unterstützung von [Leistungsindikatoren](./performance-counters.md) in ASP.NET Core gelten die folgenden Einschränkungen:

* Die SDK-Versionen 2.4.1 und höher erfassen Leistungsindikatoren, wenn die Anwendung in Azure-Web-Apps (Windows) ausgeführt wird.
* Die SDK-Versionen 2.7.1 und höher erfassen Leistungsindikatoren, wenn die Anwendung unter Windows läuft und `NETSTANDARD2.0` oder höher als Zielframework verwendet wird.
* Für Anwendungen, die für das .NET Framework bestimmt sind, werden Leistungsindikatoren in allen Versionen des SDK unterstützt.
* Die SDK-Versionen 2.8.0 und höher unterstützen Leistungsindikatoren für CPU und Arbeitsspeicher unter Linux. Es werden kein weiteren Leistungsindikatoren unter Linux unterstützt. Die empfohlene Vorgehensweise für Systemleistungsindikatoren unter Linux (und in anderen Nicht-Windows-Umgebungen) ist die Verwendung von [EventCounters](#eventcounter).

### <a name="eventcounter"></a>EventCounter

`EventCounterCollectionModule` ist standardmäßig aktiviert. Informationen zum Konfigurieren der Liste der zu sammelnden Leistungsindikatoren finden Sie unter [Einführung in EventCounters](eventcounters.md).

## <a name="enable-client-side-telemetry-for-web-applications"></a>Aktivieren der clientseitigen Telemetrie für Webanwendungen

Wenn Sie die vorherigen Schritte ausgeführt haben, können Sie serverseitige Telemetriedaten erfassen. Wenn Ihre Anwendung über clientseitige Komponenten verfügt, sollten Sie die unten angegebenen Schritte ausführen, um mit dem Erfassen von [Nutzungstelemetriedaten](./usage-overview.md) zu beginnen.

1. Nehmen Sie in `_ViewImports.cshtml` eine Einfügung vor:

```cshtml
    @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
```

2. Fügen Sie in `_Layout.cshtml` am Ende des `<head>`-Abschnitts `HtmlHelper` vor allen anderen Skripts ein. Wenn Sie benutzerdefinierte JavaScript-Telemetriedaten für die Seite übermitteln möchten, müssen Sie den Code dafür nach diesem Ausschnitt einfügen:

```cshtml
    @Html.Raw(JavaScriptSnippet.FullScript)
    </head>
```

Als Alternative zur Verwendung von `FullScript` ist ab Version 2.14 des Application Insights SDK für ASP.NET Core `ScriptBody` verfügbar. Verwenden Sie dieses Element, wenn Sie das `<script>`-Tag so steuern müssen, dass eine Inhaltssicherheitsrichtlinie festgelegt wird:

```cshtml
 <script> // apply custom changes to this script tag.
     @Html.Raw(JavaScriptSnippet.ScriptBody)
 </script>
```

Die `.cshtml`-Dateinamen, auf die vorher verwiesen wurde, stammen aus der Standardvorlage für MVC-Anwendungen. Wenn Sie die clientseitige Überwachung für Ihre Anwendung ordnungsgemäß aktivieren möchten, muss der JavaScript-Codeausschnitt im `<head>`-Abschnitt jeder Anwendungsseite vorhanden sein, die Sie überwachen möchten. Fügen Sie hierzu in dieser Anwendungsvorlage den JavaScript-Codeausschnitt zu `_Layout.cshtml` hinzu. 

Sie können für Ihr Projekt die [clientseitige Überwachung](./website-monitoring.md) auch dann aktivieren, wenn dieses nicht `_Layout.cshtml` enthält. Fügen Sie hierzu den JavaScript-Codeausschnitt einer entsprechenden Datei hinzu, die den `<head>`-Abschnitt aller Seiten Ihrer App steuert. Sie können den Codeausschnitt auch mehreren Seiten hinzufügen. Diese Lösung lässt sich allerdings auf Dauer nur schwierig umsetzen und wird daher nicht empfohlen.

> [!NOTE]
> Die JavaScript-Einschleusung bietet eine Standardkonfigurationserfahrung. Wenn Sie eine [Konfiguration](./javascript.md#configuration) benötigen, die über das Festlegen des Instrumentierungsschlüssels hinausgeht, müssen Sie die automatische Einschleusung wie oben beschrieben entfernen und das [JavaScript SDK](./javascript.md#adding-the-javascript-sdk) manuell hinzufügen.

## <a name="configure-the-application-insights-sdk"></a>Konfigurieren des Application Insights SDK

Sie können die Standardkonfiguration des Application Insights SDK für ASP.NET Core anpassen. Benutzer des ASP.NET SDK für Application Insights können die Konfiguration über `ApplicationInsights.config` oder durch das Ändern von `TelemetryConfiguration.Active` anpassen. Falls nicht anders angegeben, werden fast alle Konfigurationsänderungen für ASP.NET Core in der `ConfigureServices()`-Methode Ihrer `Startup.cs`-Klasse vorgenommen. Weitere Informationen finden Sie in den folgenden Abschnitten.

> [!NOTE]
> In ASP.NET Core-Anwendungen wird die Konfiguration durch Änderung von `TelemetryConfiguration.Active` nicht unterstützt.

### <a name="using-applicationinsightsserviceoptions"></a>Verwenden von ApplicationInsightsServiceOptions

Sie können wie im folgenden Beispiel einige allgemeine Einstellungen ändern, indem Sie der `AddApplicationInsightsTelemetry`-Methode `ApplicationInsightsServiceOptions` übergeben:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
    // Disables adaptive sampling.
    aiOptions.EnableAdaptiveSampling = false;

    // Disables QuickPulse (Live Metrics stream).
    aiOptions.EnableQuickPulseMetricStream = false;
    services.AddApplicationInsightsTelemetry(aiOptions);
}
```

Diese Tabelle enthält die vollständige Liste der `ApplicationInsightsServiceOptions`-Einstellungen:

|Einstellung | BESCHREIBUNG | Standard
|---------------|-------|-------
|EnablePerformanceCounterCollectionModule  | `PerformanceCounterCollectionModule` aktivieren/deaktivieren | true
|EnableRequestTrackingTelemetryModule   | `RequestTrackingTelemetryModule` aktivieren/deaktivieren | true
|EnableEventCounterCollectionModule   | `EventCounterCollectionModule` aktivieren/deaktivieren | true
|EnableDependencyTrackingTelemetryModule   | `DependencyTrackingTelemetryModule` aktivieren/deaktivieren | true
|EnableAppServicesHeartbeatTelemetryModule  |  `AppServicesHeartbeatTelemetryModule` aktivieren/deaktivieren | true
|EnableAzureInstanceMetadataTelemetryModule   |  `AzureInstanceMetadataTelemetryModule` aktivieren/deaktivieren | true
|EnableQuickPulseMetricStream | „LiveMetrics“-Feature aktivieren/deaktivieren | true
|EnableAdaptiveSampling | Adaptive Stichprobenerstellung aktivieren/deaktivieren | true
|EnableHeartbeat | Heartbeats-Feature aktivieren/deaktivieren, das in regelmäßigen Abständen (Standardwert: 15 Minuten) eine benutzerdefinierte Metrik namens „HeartbeatState“ mit Informationen zur Laufzeit wie .NET-Version, ggf. Informationen zur Azure-Umgebung usw. sendet. | true
|AddAutoCollectedMetricExtractor | Extraktor für „AutoCollectedMetrics“ aktivieren/deaktivieren, bei dem es sich um einen Telemetrieprozessor handelt, der vorab aggregierte Metriken zu Anforderungen/Abhängigkeiten sendet, bevor die Stichprobenerstellung stattfindet. | true
|RequestCollectionOptions.TrackExceptions | Berichterstellung über die Nachverfolgung von Ausnahmefehlern durch das Anforderungserfassungsmodul aktivieren/deaktivieren. | In NETSTANDARD2.0 „false“ (da Ausnahmen mit „ApplicationInsightsLoggerProvider“ nachverfolgt werden), andernfalls „true“.
|EnableDiagnosticsTelemetryModule | `DiagnosticsTelemetryModule` aktivieren/deaktivieren. Wenn Sie diese Einstellung deaktivieren, werden die folgenden Einstellungen ignoriert: `EnableHeartbeat`, `EnableAzureInstanceMetadataTelemetryModule`, `EnableAppServicesHeartbeatTelemetryModule`. | true

Die aktuelle Liste finden Sie unter den [konfigurierbaren Einstellungen in `ApplicationInsightsServiceOptions`](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/NETCORE/src/Shared/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="configuration-recommendation-for-microsoftapplicationinsightsaspnetcore-sdk-2150-and-later"></a>Konfigurationsempfehlungen für Version 2.15.0 und höher des Microsoft.ApplicationInsights.AspNetCore-SDK

Ab Version [2.15.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/2.15.0) des Microsoft.ApplicationInsights.AspNetCore-SDK wird die Konfiguration aller in `ApplicationInsightsServiceOptions` verfügbaren Einstellungen empfohlen, einschließlich von **InstrumentationKey** unter Verwendung der `IConfiguration`-Instanz der Anwendung. Die Einstellungen müssen sich im Abschnitt „ApplicationInsights“ befinden, wie im folgenden Beispiel zu sehen. Im folgenden Abschnitt aus „appsettings.json“ wird der Instrumentierungsschlüssel konfiguriert, und die adaptive Stichprobenerstellung und das Sammeln von Leistungsindikatoren werden deaktiviert.

```json
{
    "ApplicationInsights": {
    "InstrumentationKey": "putinstrumentationkeyhere",
    "EnableAdaptiveSampling": false,
    "EnablePerformanceCounterCollectionModule": false
    }
}
```

Wenn `services.AddApplicationInsightsTelemetry(aiOptions)` verwendet wird, setzt dies die Einstellungen von `Microsoft.Extensions.Configuration.IConfiguration` außer Kraft.

### <a name="sampling"></a>Stichproben

Das Application Insights SDK für ASP.NET Core unterstützt sowohl die feste als auch die adaptive Stichprobenerstellung. Die adaptive Stichprobenerstellung ist standardmäßig aktiviert.

Weitere Informationen finden Sie unter [Konfigurieren der adaptiven Stichprobenerstellung für ASP.NET Core-Anwendungen](./sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications).

### <a name="adding-telemetryinitializers"></a>Hinzufügen von TelemetryInitializers

Verwenden Sie [Telemetrieinitialisierer](./api-filtering-sampling.md#addmodify-properties-itelemetryinitializer), wenn Sie Telemetriedaten um zusätzliche Informationen erweitern möchten.

Fügen Sie wie im folgenden Code gezeigt dem `DependencyInjection`-Container einen neuen `TelemetryInitializer` hinzu. Das SDK erfasst automatisch alle `TelemetryInitializer`, die dem `DependencyInjection`-Container hinzugefügt werden.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
}
```

> [!NOTE]
> `services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();` kann für einfache Initialisierer verwendet werden. Ansonsten ist Folgendes erforderlich: `services.AddSingleton(new MyCustomTelemetryInitializer() { fieldName = "myfieldName" });`
    
### <a name="removing-telemetryinitializers"></a>Entfernen von TelemetryInitializer-Elementen

Telemetrieinitialisierer sind standardmäßig vorhanden. Wenn Sie alle oder nur bestimmte Telemetrieinitialisierer entfernen möchten, können Sie den folgenden Beispielcode *nach* dem Aufrufen von `AddApplicationInsightsTelemetry()` verwenden.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry();

    // Remove a specific built-in telemetry initializer
    var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                        (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
    if (tiToRemove != null)
    {
        services.Remove(tiToRemove);
    }

    // Remove all initializers
    // This requires importing namespace by using Microsoft.Extensions.DependencyInjection.Extensions;
    services.RemoveAll(typeof(ITelemetryInitializer));
}
```

### <a name="adding-telemetry-processors"></a>Hinzufügen von Telemetrieprozessoren

Sie können `TelemetryConfiguration` benutzerdefinierte Telemetrieprozessoren hinzufügen, indem Sie die Erweiterungsmethode `AddApplicationInsightsTelemetryProcessor` in `IServiceCollection` verwenden. Telemetrieprozessoren werden in [komplexen Filterszenarien](./api-filtering-sampling.md#itelemetryprocessor-and-itelemetryinitializer) verwendet. Verwenden Sie das folgende Beispiel.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddApplicationInsightsTelemetry();
    services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

    // If you have more processors:
    services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
}
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Konfigurieren oder Entfernen von TelemetryModule-Standardelementen

Application Insights verwendet Telemetriemodule, um automatisch nützliche Telemetriedaten zu bestimmten Workloads zu erfassen, ohne dass eine manuelle Nachverfolgung durch den Benutzer erforderlich ist.

Die unten aufgeführten Module für die automatische Sammlung sind standardmäßig aktiviert. Diese sind verantwortlich für die automatische Erfassung von Telemetriedaten. Sie können sie deaktivieren oder ihr Standardverhalten anpassen.

* `RequestTrackingTelemetryModule`: Sammelt Anforderungstelemetriedaten (RequestTelemetry) aus eingehenden Webanforderungen
* `DependencyTrackingTelemetryModule`: Sammelt [Abhängigkeitstelemetriedaten](./asp-net-dependencies.md) (DependencyTelemetry) aus ausgehenden HTTP-Aufrufen und SQL-Aufrufen
* `PerformanceCollectorModule`: Sammelt Windows-Leistungsindikatoren (PerformanceCounters)
* `QuickPulseTelemetryModule`: Sammelt Telemetriedaten zur Anzeige im Live Metrics-Portal
* `AppServicesHeartbeatTelemetryModule`: Sammelt Heartbeats (die als benutzerdefinierte Metriken gesendet werden) über die Azure App Service-Umgebung, in der die Anwendung gehostet wird
* `AzureInstanceMetadataTelemetryModule`: Sammelt Heartbeats (die als benutzerdefinierte Metriken gesendet werden) über die Azure-VM-Umgebung, in der die Anwendung gehostet wird
* `EventCounterCollectionModule`: Sammelt [EventCounters](eventcounters.md). Dieses Modul ist ein neues Feature und in der SDK-Version 2.8.0 und höher verfügbar.

Verwenden Sie zum Konfigurieren von `TelemetryModule`-Standardelementen die Erweiterungsmethode `ConfigureTelemetryModule<T>` in `IServiceCollection`. Dies ist im Beispiel unten dargestellt.

```csharp
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry();

    // The following configures DependencyTrackingTelemetryModule.
    // Similarly, any other default modules can be configured.
    services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
            {
                module.EnableW3CHeadersInjection = true;
            });

    // The following removes all default counters from EventCounterCollectionModule, and adds a single one.
    services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                module.Counters.Add(new EventCounterCollectionRequest("System.Runtime", "gen-0-size"));
            }
        );

    // The following removes PerformanceCollectorModule to disable perf-counter collection.
    // Similarly, any other default modules can be removed.
    var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
    if (performanceCounterService != null)
    {
        services.Remove(performanceCounterService);
    }
}
```

Ab Version 2.12.2 enthält [`ApplicationInsightsServiceOptions`](#using-applicationinsightsserviceoptions) eine Option zum einfachen Deaktivieren beliebiger Standardmodule.

### <a name="configuring-a-telemetry-channel"></a>Konfigurieren eines Telemetriekanals

Der standardmäßige [Telemetriekanal](./telemetry-channels.md) ist `ServerTelemetryChannel`. Im folgenden Beispiel wird gezeigt, wie Sie sie überschreiben.

```csharp
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // Use the following to replace the default channel with InMemoryChannel.
        // This can also be applied to ServerTelemetryChannel.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

### <a name="disable-telemetry-dynamically"></a>Dynamisches Deaktivieren von Telemetrie

Wenn Sie Telemetriedaten bedingt und dynamisch deaktivieren möchten, können Sie eine `TelemetryConfiguration`-Instanz mit einem ASP.NET Core-Abhängigkeitseinschleusungscontainer an einer beliebigen Stelle im Code auflösen und ein `DisableTelemetry`-Flag dafür festlegen.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, TelemetryConfiguration configuration)
    {
        configuration.DisableTelemetry = true;
        ...
    }
```

Das Codebeispiel oben verhindert das Senden von Telemetriedaten an Application Insights. Dadurch wird nicht verhindert, dass Telemetriedaten von Modulen für die automatische Sammlung gesammelt werden. Wenn Sie ein bestimmtes Modul für die automatische Sammlung entfernen möchten, finden Sie weitere Informationen unter [Entfernen des Telemetriemoduls](#configuring-or-removing-default-telemetrymodules).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="does-application-insights-support-aspnet-core-3x"></a>Wird ASP.NET Core 3.X in Application Insights unterstützt?

Ja. Führen Sie ein Update auf [Application Insights SDK für ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) Version 2.8.0 oder höher durch. In älteren Versionen des SDK wird ASP.NET Core 3.X nicht unterstützt.

Wenn Sie [serverseitige Telemetrie für Visual Studio aktivieren](#enable-application-insights-server-side-telemetry-visual-studio), führen Sie außerdem für das Onboarding ein Update auf Visual Studio 2019 (16.3.0) aus. In früheren Versionen von Visual Studio wird das automatische Onboarding für ASP.NET Core 3.X-Apps nicht unterstützt.

### <a name="how-can-i-track-telemetry-thats-not-automatically-collected"></a>Wie kann ich Telemetriedaten nachverfolgen, die nicht automatisch erfasst werden?

Rufen Sie eine Instanz von `TelemetryClient` ab, indem Sie die Abhängigkeit über den Konstruktor übergeben (Constructor Injection) und die erforderliche `TrackXXX()`-Methode aufrufen. Es wird nicht empfohlen, neue `TelemetryClient`- oder `TelemetryConfiguration`-Instanzen in einer ASP.NET Core-Anwendung zu erstellen. Eine Singletoninstanz von `TelemetryClient` ist bereits im `DependencyInjection`-Container registriert, der `TelemetryConfiguration` für alle sonstigen Telemetriedaten verwendet. Sie sollten nur dann eine neue `TelemetryClient`-Instanz erstellen, wenn für diese eine Konfiguration erforderlich ist, die sich von der für die sonstigen Telemetriedaten unterscheidet.

Im folgenden Beispiel wird veranschaulicht, wie Sie zusätzliche Telemetrie über einen Controller nachverfolgen.

```csharp
using Microsoft.ApplicationInsights;

public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // Use constructor injection to get a TelemetryClient instance.
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // Call the required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

Wie Sie Daten in Application Insights benutzerdefiniert erfassen können, erfahren Sie unter [Application Insights-API für benutzerdefinierte Ereignisse und Metriken](./api-custom-events-metrics.md). Ein ähnlicher Ansatz kann verwendet werden, um mithilfe der [GetMetric-API](./get-metric.md) benutzerdefinierte Metriken an Application Insights zu senden.

### <a name="how-do-i-customize-ilogger-logs-collection"></a>Wie passe ich die Sammlung von ILogger-Protokollen an?

Standardmäßig werden nur Protokolle mit dem Schweregrad `Warning` und höher automatisch gesammelt. Um dieses Verhalten zu ändern, überschreiben Sie explizit die Protokollierungskonfiguration für den Anbieter `ApplicationInsights`, wie unten gezeigt.
Mit der folgenden Konfiguration kann Application Insights alle Protokolle mit dem Schweregrad `Information` und höher sammeln.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    },
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "Information"
      }
    }
  }
}
```

Beachten Sie unbedingt, dass das Folgende nicht dazu führt, dass der Application Insights-Anbieter `Information`-Protokolle sammelt. Sie werden nicht gesammelt, da das SDK einen Standardprotokollierungsfilter hinzufügt, der `ApplicationInsights` anweist, nur Protokolle mit dem Schweregrad `Warning` und höher zu sammeln. ApplicationInsights verlangt eine explizite Außerkraftsetzung.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

Weitere Informationen finden Sie unter [ILogger-Konfiguration](ilogger.md#logging-level).

### <a name="some-visual-studio-templates-used-the-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Für einige Visual Studio-Vorlagen wurde die Erweiterungsmethode „UseApplicationInsights()“ in IWebHostBuilder verwendet, um Application Insights zu aktivieren. Ist diese Verwendung noch gültig?

Obwohl die Erweiterungsmethode `UseApplicationInsights()` weiterhin unterstützt wird, ist sie ab der Application Insights SDK-Version 2.8.0 als veraltet markiert. Sie wird in der nächsten Hauptversion des SDK entfernt. Zum Aktivieren von Application Insights-Telemetriedaten wird die Verwendung von `AddApplicationInsightsTelemetry()` empfohlen, da sie Überladungen zum Steuern einiger Konfigurationen bietet. In ASP.NET Core 3.X-Apps ist `services.AddApplicationInsightsTelemetry()` außerdem die einzige Möglichkeit zum Aktivieren von Application Insights.

### <a name="im-deploying-my-aspnet-core-application-to-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>Ich stelle meine ASP.NET Core-Anwendung in Web-Apps bereit. Sollte ich trotzdem die Application Insights-Erweiterung über Web-Apps aktivieren?

Wenn das SDK wie in diesem Artikel gezeigt zur Buildzeit installiert wird, ist es nicht erforderlich, die [Application Insights](./azure-web-apps.md)-Erweiterung über das App Service-Portal zu aktivieren. Auch bei installierter Erweiterung werden keine Schritte ausgeführt, wenn erkannt wird, dass das SDK der Anwendung bereits hinzugefügt wurde. Wenn Sie Application Insights über die Erweiterung aktivieren, müssen Sie das SDK nicht installieren und aktualisieren. Wenn Sie Application Insights jedoch mithilfe der Anweisungen in diesem Artikel aktivieren, sind Sie aus den folgenden Gründen flexibler:

   * Die Application Insights-Telemetriefunktion funktioniert weiterhin
       * auf allen Betriebssystemen einschließlich Windows, Linux und Mac.
       * mit allen Veröffentlichungsmodi einschließlich dem eigenständigen oder frameworkabhängigen Modus.
       * allen Zielframeworks, z. B. vollständigem .NET Framework.
       * mit allen Hostserveroptionen einschließlich Web-Apps, VMs, Linux, Containern, Azure Kubernetes Service und anderer Hostinglösungen als Azure.
       * mit allen .NET Core-Versionen, einschließlich Vorschauversionen.
   * Sie können Telemetriedaten lokal beim Debuggen in Visual Studio anzeigen.
   * Sie können mit der `TrackXXX()`-API zusätzliche benutzerdefinierte Telemetriedaten nachverfolgen.
   * Die vollständige Steuerung der Konfiguration ist Ihnen überlassen.

### <a name="can-i-enable-application-insights-monitoring-by-using-tools-like-azure-monitor-application-insights-agent-formerly-status-monitor-v2"></a>Kann ich die Application Insights-Überwachung mithilfe von Tools wie Azure Monitor Application Insights-Agent (früher Statusmonitor v2) aktivieren?

 Ja. Ab [Application Insights-Agent 2.0.0-beta1](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/2.0.0-beta1) werden in IIS gehostete ASP.NET Core-Anwendungen unterstützt.

### <a name="if-i-run-my-application-in-linux-are-all-features-supported"></a>Werden alle Features unterstützt, wenn ich meine Anwendung unter Linux ausführe?

Ja. Die Featureunterstützung für das SDK ist auf allen Plattformen gleich. Es gelten lediglich die folgenden Ausnahmen:

* Unter Linux sammelt das SDK [EventCounters](./eventcounters.md), da [Leistungsindikatoren](./performance-counters.md) nur unter Windows unterstützt werden. Die meisten Metriken sind identisch.
* Obwohl `ServerTelemetryChannel` standardmäßig aktiviert ist, wird über den Kanal bei der Anwendungsausführung unter Linux oder macOS nicht automatisch ein lokaler Speicherordner erstellt, um die Telemetriedaten bei Netzwerkproblemen vorübergehend zu speichern. Aufgrund dieser Einschränkung gehen Telemetriedaten verloren, wenn vorübergehende Netzwerk- oder Serverprobleme auftreten. Sie können das Problem umgehen, indem Sie einen lokalen Ordner für den Kanal konfigurieren:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure the channel to use the given folder to temporarily
        // store telemetry items during network or Application Insights server issues.
        // User should ensure that the given folder already exists
        // and that the application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```

Diese Einschränkung gilt ab Version [2.15.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/2.15.0) nicht.

### <a name="is-this-sdk-supported-for-the-new-net-core-3x-worker-service-template-applications"></a>Wird dieses SDK für die neuen .NET Core 3.X Worker Service-Vorlagenanwendungen unterstützt?

Dieses SDK erfordert `HttpContext`, daher funktioniert es nicht in Nicht-HTTP-Anwendungen, einschließlich .NET Core 3.X Worker Service-Anwendungen. Informationen zum Aktivieren von Application Insights in solchen Anwendungen mithilfe des neu veröffentlichten Microsoft.ApplicationInsights.WorkerService-SDK finden Sie unter [Application Insights für Workerdienstanwendungen (Anwendungen ohne HTTP)](worker-service.md).

## <a name="open-source-sdk"></a>Open Source SDK

* [Lesen und Hinzufügen von Code](https://github.com/microsoft/ApplicationInsights-dotnet).

Informationen zu den neuesten Updates und Fehlerbehebungen finden Sie in den [Versionshinweisen](./release-notes.md).

## <a name="next-steps"></a>Nächste Schritte

* [Machen Sie sich Benutzerflows vertraut](./usage-flows.md), um die Benutzernavigation in Ihrer App nachzuvollziehen.
* [Konfigurieren Sie eine Momentaufnahmensammlung](./snapshot-debugger.md), um den Status des Quellcodes und der Variablen zu dem Zeitpunkt anzuzeigen, an dem eine Ausnahme ausgelöst wurde.
* [Verwenden Sie die API](./api-custom-events-metrics.md), um Ihre eigenen Ereignisse und Metriken für eine detaillierte Ansicht der Leistung und Nutzung Ihrer App zu senden.
* Überprüfen Sie Ihre App mit [Verfügbarkeitstests](./monitor-web-app-availability.md) fortwährend von jedem Ort der Welt aus.
* [Dependency Injection in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection)
