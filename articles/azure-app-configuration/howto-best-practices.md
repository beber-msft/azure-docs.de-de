---
title: Bewährte Methoden für Azure App Configuration | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über bewährte Methoden bei der Verwendung von Azure App Configuration. Zu den behandelten Themen gehören Schlüsselgruppierungen, Schlüssel-Wert-Kompositionen, App Configuration-Bootstraps und viele mehr.
services: azure-app-configuration
documentationcenter: ''
author: AlexandraKemperMS
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: alkemper
ms.custom: devx-track-csharp, mvc
ms.openlocfilehash: e75fb11379ccbdff90d1acd1a3bce36b62bd8a1d
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2021
ms.locfileid: "130250861"
---
# <a name="azure-app-configuration-best-practices"></a>Bewährte Methoden für Azure App Configuration

In diesem Artikel werden allgemeine Muster und bewährte Methoden bei der Verwendung von Azure App Configuration erörtert.

## <a name="key-groupings"></a>Schlüsselgruppierungen

App Configuration bietet zwei Optionen zum Organisieren von Schlüsseln:

* Schlüsselpräfixe
* Bezeichnungen

Sie können eine oder beide Optionen verwenden, um Ihre Schlüssel zu gruppieren.

*Schlüsselpräfixe* bilden den Anfang von Schlüsseln. Sie können einen Satz von Schlüsseln logisch gruppieren, indem Sie dasselbe Präfix in ihren Namen verwenden. Präfixe können mehrere Komponenten enthalten, die durch ein Trennzeichen wie z. B. `/` miteinander verbunden sind, ähnlich einem URL-Pfad, um einen Namespace zu bilden. Solche Hierarchien sind hilfreich, wenn Sie Schlüssel für viele Anwendungen, und Microservices in einem App Configuration-Speicher speichern.

Ein wichtiger, zu berücksichtigender Aspekt ist, dass Schlüssel das sind, worauf Ihr Anwendungscode verweist, um die Werte der entsprechenden Einstellungen abzurufen. Schlüssel sollten nicht geändert werden, weil Sie ansonsten bei jeder Änderung auch den Code ändern müssen.

*Bezeichnungen* sind Attribute für Schlüssel. Sie werden zum Erstellen von Varianten eines Schlüssels verwendet. Beispielsweise können Sie mehrere Versionen eines Schlüssels Bezeichnungen zuweisen. Eine Version kann eine Iteration, eine Umgebung oder eine andere kontextbezogene Information sein. Ihre Anwendung kann einen vollständig anderen Satz von Schlüsselwerten anfordern, indem sie eine andere Bezeichnung angibt. Daher bleiben alle Schlüsselverweise in Ihrem Code unverändert.

## <a name="key-value-compositions"></a>Schlüsselwertkompositionen

App Configuration behandelt alle gespeicherten Schlüssel als unabhängige Entitäten. App Configuration versucht nicht, eine Beziehung zwischen Schlüsseln abzuleiten oder Schlüsselwerte basierend auf der Hierarchie zu erben. Sie können jedoch mehrere Sätze von Schlüsseln aggregieren, indem Sie Bezeichnungen verwenden, die mit geeigneter Konfigurationsstapelung in Ihrem Anwendungscode gekoppelt sind.

Schauen wir uns ein Beispiel an. Angenommen, Sie verwenden eine Einstellung namens **Asset1**, deren Wert basierend auf der Entwicklungsumgebung variieren kann. Sie erstellen einen Schlüssel namens „Asset1“ mit einer leeren Bezeichnung und eine Bezeichnung mit dem Namen „Development“. In der ersten Bezeichnung platzieren Sie den Standardwert für **Asset1**, und in der letzteren platzieren Sie einen spezifischen Wert für „Development“.

In Ihrem Code rufen Sie zuerst die Schlüsselwerte ohne Bezeichnungen ab, und dann rufen Sie den gleichen Satz von Schlüsselwerten ein zweites Mal mit der Bezeichnung „Development“ ab. Wenn Sie die Werte das zweite Mal abrufen, werden die vorherigen Werte der Schlüssel überschrieben. Das .NET Core-Konfigurationssystem bietet Ihnen die Möglichkeit, mehrere Sätze von Konfigurationsdaten übereinander zu „stapeln“. Wenn ein Schlüssel in mehreren Sätzen vorhanden ist, wird der letzte Satz verwendet, der den Schlüssel enthält. Bei einem modernen Programmierframework wie .NET Core erhalten Sie diese Stapelfunktion kostenlos, wenn Sie einen nativen Konfigurationsanbieter für den Zugriff auf App Configuration verwenden. Der folgende Codeausschnitt zeigt, wie Sie eine Stapelung in eine .NET Core-Anwendung implementieren können:

```csharp
// Augment the ConfigurationBuilder with Azure App Configuration
// Pull the connection string from an environment variable
configBuilder.AddAzureAppConfiguration(options => {
    options.Connect(configuration["connection_string"])
           .Select(KeyFilter.Any, LabelFilter.Null)
           .Select(KeyFilter.Any, "Development");
});
```

Unter [Verwenden von Bezeichnungen zum Aktivieren verschiedener Konfigurationen für verschiedene Umgebungen](./howto-labels-aspnet-core.md) finden Sie ein vollständiges Beispiel.

## <a name="references-to-external-data"></a>Verweise auf externe Daten

App Configuration dient zum Speichern jeglicher Konfigurationsdaten, die Sie normalerweise in Konfigurationsdateien oder Umgebungsvariablen speichern würden. Einige Datentypen eignen sich jedoch möglicherweise besser für die Speicherung in anderen Quellen. Speichern Sie beispielsweise Geheimnisse in Key Vault, Dateien in Azure Storage, Mitgliedschaftsinformationen in Azure AD-Gruppen oder Kundenlisten in einer Datenbank.

Sie können dennoch die Vorteile von App Configuration nutzen, indem Sie einen Verweis auf externe Daten in einem Schlüsselwert speichern. Sie können den [Inhaltstyp verwenden](./concept-key-value.md#use-content-type), um jede Datenquelle zu unterscheiden. Wenn Ihre Anwendung einen Verweis liest, laden Sie die Daten aus der Quelle, auf die verwiesen wird. Falls Sie den Speicherort Ihrer externen Daten ändern, müssen Sie den Verweis nur in App Configuration aktualisieren, anstatt Ihre gesamte Anwendung zu aktualisieren und erneut bereitzustellen.

Das [Key Vault](use-key-vault-references-dotnet-core.md)-Verweisfeature von App Configuration ist in diesem Fall ein Beispiel. Es gestattet die Aktualisierung der für eine Anwendung erforderlichen Geheimnisse nach Bedarf, während die zugrunde liegenden Geheimnisse selbst in Key Vault verbleiben.

## <a name="app-configuration-bootstrap"></a>App Configuration-Bootstrapping

Um auf den App Configuration-Speicher zuzugreifen, können Sie seine Verbindungszeichenfolge verwenden, die im Azure-Portal verfügbar ist. Weil Verbindungszeichenfolgen Anmeldeinformationen enthalten, gelten sie als Geheimnisse. Diese Geheimnisse müssen in Azure Key Vault gespeichert werden, und Ihr Code muss bei Key Vault authentifiziert werden, um sie abrufen zu können.

Eine bessere Option ist die Verwendung des Features für verwaltete Identitäten in Azure Active Directory. Wenn Sie verwaltete Identitäten verwenden, benötigen Sie nur die URL des App Configuration-Endpunkts, um den Zugriff auf Ihren App Configuration-Speicher zu bootstrappen. Sie können die URL in Ihren Anwendungscode einbetten (z. B. in die Datei *appsettings.json*). Weitere Details finden Sie unter [Integrieren mit verwalteten Azure-Identitäten](howto-integrate-azure-managed-service-identity.md).

## <a name="app-or-function-access-to-app-configuration"></a>App- oder Funktionszugriff auf App Configuration

Sie können Web-Apps oder Azure Functions mit einer der folgenden Methoden Zugriff auf App Configuration gewähren:

* Geben Sie über das Azure-Portal in den Anwendungseinstellungen von App Service die Verbindungszeichenfolge zu Ihrem App Configuration-Speicher ein.
* Speichern Sie die Verbindungszeichenfolge zu Ihrem App Configuration-Speicher in Key Vault, und [verweisen Sie über App Service darauf](../app-service/app-service-key-vault-references.md).
* Verwenden Sie verwaltete Azure-Identitäten für den Zugriff auf den App Configuration-Speicher. Weitere Informationen finden Sie unter [Integrieren mit verwalteten Azure-Identitäten](howto-integrate-azure-managed-service-identity.md).
* Übertragen Sie die Konfiguration aus App Configuration per Push an App Service. App Configuration bietet (im Azure-Portal und in der Azure-Befehlszeilenschnittstelle) eine Exportfunktion, die Daten direkt an App Service sendet. Bei dieser Methode müssen Sie den Anwendungscode gar nicht ändern.

## <a name="reduce-requests-made-to-app-configuration"></a>Verringern der Anzahl der Anforderungen an die App-Konfiguration

Übermäßige Anforderungen an die App-Konfiguration können zur Drosselung oder zu Überschreitungsgebühren führen. So verringern Sie die Anzahl der gesendeten Anforderungen

* Erhöhen Sie das Aktualisierungstimeout, insbesondere wenn sich Ihre Konfigurationswerte nicht häufig ändern. Geben Sie ein neues Aktualisierungstimeout mithilfe der [`SetCacheExpiration`-Methode](/dotnet/api/microsoft.extensions.configuration.azureappconfiguration.azureappconfigurationrefreshoptions.setcacheexpiration) an.

* Überwachen Sie eher einen einzelnen *Sentinel-Schlüssel*, anstatt einzelne Schlüssel zu überwachen. Aktualisieren Sie alle Konfigurationen nur, wenn sich der Sentinel-Schlüssel ändert. Ein Beispiel finden Sie unter [Verwenden der dynamischen Konfiguration in einer ASP.NET Core-App](enable-dynamic-configuration-aspnet-core.md).

* Verwenden Sie Azure Event Grid, um Benachrichtigungen zu empfangen, wenn sich die Konfiguration ändert, anstatt ständig Änderungen abzufragen. Weitere Informationen finden Sie unter [Verwenden von Event Grid für App Configuration-Datenänderungsbenachrichtigungen](./howto-app-configuration-event.md).

* Verteilen Sie Ihre Anforderungen auf mehrere App Configuration-Speicher. Verwenden Sie beispielsweise einen anderen Speicher aus jeder geografischen Region für eine global bereitgestellte Anwendung. Jeder App Configuration-Speicher verfügt über ein eigenes Anforderungskontingent. Diese Einrichtung bietet Ihnen ein Modell für Skalierbarkeit und vermeidet den Single Point of Failure.

## <a name="importing-configuration-data-into-app-configuration"></a>Importieren von Konfigurationsdaten in App Configuration

App Configuration bietet die Möglichkeit, Ihre Konfigurationseinstellungen aus Ihren aktuellen Konfigurationsdateien massenhaft zu [importieren](./howto-import-export-data.md), entweder über das Azure-Portal oder mithilfe der CLI. Sie können auch dieselben Optionen verwenden, um Schlüsselwerte aus App Configuration zu exportieren, z. B. zwischen verwandten Stores. Wenn Sie eine fortlaufende Synchronisierung mit Ihrem Repository in GitHub oder Azure DevOps einrichten möchten, können Sie unsere [GitHub Action](./concept-github-action.md) oder [Azure Pipeline Push-Aufgabe](./push-kv-devops-pipeline.md) verwenden, damit Sie Ihre vorhandenen Quellcodeverwaltungs-Verfahren weiterhin nutzen können und gleichzeitig die Vorteile von App Configuration genießen.

## <a name="multi-region-deployment-in-app-configuration"></a>Bereitstellung in mehreren Regionen in App Configuration

App Configuration ist ein regionaler Dienst. Bei Anwendungen mit unterschiedlichen Konfigurationen pro Region kann das Speichern dieser Konfigurationen in einer Instanz zu einem Single Point of Failure führen. Die Bereitstellung einer App Configuration-Instanz pro Region über mehrere Regionen hinweg ist unter Umständen die bessere Option. Dies kann bei der regionalen Notfallwiederherstellung, zur Sicherstellung der Leistung und bei der Erstellung von Sicherheitssilos hilfreich sein. Durch die Konfiguration nach Region wird auch die Wartezeit verkürzt, und es werden getrennte Drosselungskontingente verwendet, da die Drosselung pro Instanz erfolgt. Zur Anwendung von Notfallwiederherstellungsmaßnahmen können Sie [mehrere Konfigurationsspeicher](./concept-disaster-recovery.md) verwenden. 

## <a name="client-applications-in-app-configuration"></a>Clientanwendungen in App Configuration 

Wenn Sie App Configuration in Clientanwendungen verwenden, stellen Sie sicher, dass Sie zwei Hauptfaktoren berücksichtigen. Zunächst, wenn Sie die Verbindungszeichenfolge in einer Clientanwendung verwenden, riskieren Sie, den Zugriffsschlüssel Ihres App Configuration-Speichers für die Öffentlichkeit verfügbar zu machen. Zweitens kann die typische Skalierung einer Clientanwendung zu übermäßigen Anforderungen an Ihren App Configuration-Speicher führen, was zu Überschreitungsgebühren oder Drosselung führen kann. Weitere Informationen zur Drosselung finden Sie in den [Häufig gestellten Fragen](./faq.yml#are-there-any-limits-on-the-number-of-requests-made-to-app-configuration).

Um diese Bedenken zu berücksichtigen, empfiehlt es sich, einen Proxydienst zwischen Ihren Clientanwendungen und Ihrem App Configuration-Speicher zu verwenden. Der Proxydienst kann sich sicher bei Ihrem App Configuration-Speicher authentifizieren, ohne dass ein Sicherheitsproblem durch offengelegte Authentifizierungsinformationen besteht. Sie können einen Proxydienst erstellen, indem Sie eine der App Configuration-Anbieterbibliotheken verwenden, sodass Sie die integrierten Cache- und Aktualisierungsfunktionen nutzen können, um die Menge der Anforderungen zu optimieren, die an App Configuration gesendet werden. Weitere Informationen zur Verwendung von App Configuration-Anbietern finden Sie in den Artikeln unter „Schnellstarts“ und „Tutorials“. Der Proxydienst stellt die Konfiguration aus seinem Cache für Ihre Clientanwendungen bereit, und Sie vermeiden die beiden potenziellen Probleme, die in diesem Abschnitt erörtert werden.

## <a name="next-steps"></a>Nächste Schritte

* [Schlüssel und Werte](./concept-key-value.md) 
