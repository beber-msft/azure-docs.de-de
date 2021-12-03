---
title: Initialisieren von MSAL.NET-Clientanwendungen | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über die Initialisierung öffentlicher und vertraulicher Clientanwendungen mithilfe der Microsoft Authentication Library für .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/18/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 437de129cec6b8ef6f8d9ca572b18c2a181295b3
ms.sourcegitcommit: 838413a8fc8cd53581973472b7832d87c58e3d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2021
ms.locfileid: "132133647"
---
# <a name="initialize-client-applications-using-msalnet"></a>Initialisieren von Clientanwendungen mithilfe von MSAL.NET
Dieser Artikel beschreibt die Initialisierung öffentlicher und vertraulicher Clientanwendungen mithilfe der Microsoft Authentication Library für .NET (MSAL.NET).  Weitere Informationen zu Clientanwendungstypen finden Sie unter [Öffentliche und vertrauliche Clientanwendungen](msal-client-applications.md).

Bei MSAL.NET 3.x besteht die empfohlene Methode zum Instanziieren einer Anwendung darin, die Anwendungsersteller zu verwenden: `PublicClientApplicationBuilder` und `ConfidentialClientApplicationBuilder`. Sie bieten einen leistungsstarken Mechanismus zum Konfigurieren der Anwendung über den Code oder über eine Konfigurationsdatei oder auch durch die Kombination beider Ansätze.

[API-Referenzdokumentation](/dotnet/api/microsoft.identity.client) | [Paket auf NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client/) | [Quellcode der Bibliothek](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Codebeispiele](sample-v2-code.md)

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie eine Anwendung initialisieren, müssen Sie sie zunächst [registrieren](quickstart-register-app.md), damit Ihre App in Microsoft Identity Platform integriert werden kann.  Nach der Registrierung benötigen Sie unter Umständen die folgenden Informationen (die Sie im Azure-Portal finden können):

- Die Client-ID (eine Zeichenfolge, die eine GUID darstellt)
- Die URL des Identitätsanbieters (Namensgeber der Instanz) und die Anmeldezielgruppe für Ihre Anwendung. Diese beiden Parameter werden zusammen als Autorität bezeichnet.
- Die Mandanten-ID, wenn Sie eine Geschäftsanwendung ausschließlich für Ihre Organisation schreiben (auch als Einzelmandantenanwendung bezeichnet).
- Den geheimen Anwendungsschlüssel (geheime Clientzeichenfolge) oder das Zertifikat (vom Typ „X509Certificate2“), wenn es sich um eine vertrauliche Client-App handelt.
- Für Web-Apps und gelegentlich auch für öffentliche Clientanwendungen (insbesondere, wenn Ihre App einen Broker verwenden muss) müssen Sie auch den Umleitungs-URI festlegen, mit dem der Identitätsanbieter Ihrer Anwendung die Sicherheitstoken sendet.

## <a name="ways-to-initialize-applications"></a>Möglichkeiten zum Initialisieren von Anwendungen
Es gibt viele verschiedene Möglichkeiten zum Instanziieren von Clientanwendungen.

### <a name="initializing-a-public-client-application-from-code"></a>Initialisieren einer öffentlichen Clientanwendung über den Code

Der folgende Code instanziiert eine öffentliche Clientanwendung, die Benutzer mit ihrem Geschäfts-, Schul- oder Unikonto oder ihrem persönlichen Microsoft-Konto bei der öffentlichen Microsoft Azure-Cloud anmeldet.

```csharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

### <a name="initializing-a-confidential-client-application-from-code"></a>Initialisieren einer vertraulichen Clientanwendung über den Code

Auf dieselbe Weise instanziiert der folgende Code eine vertrauliche Anwendung (eine Web-App unter `https://myapp.azurewebsites.net`), indem Token von Benutzern in der öffentlichen Microsoft Azure-Cloud über deren Geschäfts-, Schul- oder Unikonto oder deren persönliches Microsoft-Konto verarbeitet werden. Die Anwendung wird anhand des Identitätsanbieters identifiziert, indem ein Clientgeheimnis geteilt wird:

```csharp
string redirectUri = "https://myapp.azurewebsites.net";
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithClientSecret(clientSecret)
    .WithRedirectUri(redirectUri )
    .Build();
```

In der Produktion sollte anstelle eines Clientgeheimnisses eher ein Zertifikat mit Azure AD geteilt werden. In diesem Fall lautet der Code folgendermaßen:

```csharp
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithCertificate(certificate)
    .WithRedirectUri(redirectUri )
    .Build();
```

### <a name="initializing-a-public-client-application-from-configuration-options"></a>Initialisieren einer öffentlichen Clientanwendung über die Konfigurationsoptionen

Der folgende Code instanziiert eine öffentliche Clientanwendung über ein Konfigurationsobjekt, das programmgesteuert ausgefüllt oder aus einer Konfigurationsdatei gelesen werden kann:

```csharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
    .Build();
```

### <a name="initializing-a-confidential-client-application-from-configuration-options"></a>Initialisieren einer vertraulichen Clientanwendung über die Konfigurationsoptionen

Die gleiche Art von Muster gilt für vertrauliche Clientanwendungen. Sie können auch andere Parameter über `.WithXXX`-Modifizierer hinzufügen (hier ein Zertifikat).

```csharp
ConfidentialClientApplicationOptions options = GetOptions(); // your own method
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(options)
    .WithCertificate(certificate)
    .Build();
```

## <a name="builder-modifiers"></a>Erstellermodifizierer

In den Codeausschnitten, in denen Anwendungsersteller verwendet werden, kann eine Reihe von `.With`-Methoden als Modifizierer angewendet werden (z.B. `.WithCertificate` und `.WithRedirectUri`). 

### <a name="modifiers-common-to-public-and-confidential-client-applications"></a>Modifizierer für öffentliche und vertrauliche Clientanwendungen

Folgende Modifizierer können Sie sowohl für einen öffentlichen als auch einen vertraulichen Clientanwendungsersteller festlegen:

|Modifizierer | BESCHREIBUNG|
|--------- | --------- |
|[`.WithAuthority()`](/dotnet/api/microsoft.identity.client.abstractapplicationbuilder-1.withauthority)  | Legt die Standardautorität der Anwendung auf eine Azure AD-Autorität fest, mit der Möglichkeit zur Auswahl der Azure-Cloud, der Zielgruppe, des Mandanten (Mandanten-ID oder Domänenname) oder mit direkter Angabe des Autoritäts-URI.|
|`.WithAdfsAuthority(string)` | Legt die Standardautorität der Anwendung auf eine AD FS-Autorität fest.|
|`.WithB2CAuthority(string)` | Legt die Standardautorität der Anwendung auf eine Azure AD B2C-Autorität fest.|
|`.WithClientId(string)` | Setzt die Client-ID außer Kraft.|
|`.WithComponent(string)` | Legt den Namen der Bibliothek über MSAL.NET fest (aus Telemetriegründen). |
|`.WithDebugLoggingCallback()` | Bei Aufruf ruft die Anwendung `Debug.Write` auf, um die Debugablaufverfolgungen zu aktivieren. Weitere Informationen finden Sie unter [Protokolle](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/logging).|
|`.WithExtraQueryParameters(IDictionary<string,string> eqp)` | Legen Sie die zusätzlichen Abfrageparameter auf Anwendungsebene fest, die in allen Authentifizierungsanforderungen gesendet werden. Dies kann auf jeder Ebene der Tokenabrufmethode außer Kraft gesetzt werden (ebenfalls mit `.WithExtraQueryParameters pattern`).|
|`.WithHttpClientFactory(IMsalHttpClientFactory httpClientFactory)` | Ermöglicht fortgeschrittene Szenarien, z.B. die Konfiguration für einen HTTP-Proxy oder das Erzwingen der Verwendung eines bestimmten HttpClient durch MSAL (z.B. in ASP.NET Core-Web-Apps/-APIs).|
|`.WithLogging()` | Bei Aufruf ruft die Anwendung einen Rückruf mit Debugablaufverfolgungen auf. Weitere Informationen finden Sie unter [Protokolle](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/logging).|
|`.WithRedirectUri(string redirectUri)` | Setzt den standardmäßigen Umleitungs-URI außer Kraft. Im Fall von öffentlichen Clientanwendungen eignet sich diese Option besonders für Szenarien im Zusammenhang mit dem Broker.|
|`.WithTelemetry(TelemetryCallback telemetryCallback)` | Legt den Delegaten fest, der zum Senden von Telemetriedaten verwendet wird.|
|`.WithTenantId(string tenantId)` | Setzt die Mandanten-ID oder die Mandantenbeschreibung außer Kraft.|

### <a name="modifiers-specific-to-xamarinios-applications"></a>Für Xamarin.iOS-Anwendungen spezifische Modifizierer

Folgende Modifizierer können Sie für einen öffentlichen Clientanwendungsersteller in Xamarin.iOS festlegen:

|Modifizierer | BESCHREIBUNG|
|--------- | --------- |
|`.WithIosKeychainSecurityGroup()` | **Nur Xamarin.iOS**: Legt die Sicherheitsgruppe der iOS-Keychain (für die Cachepersistenz) fest.|

### <a name="modifiers-specific-to-confidential-client-applications"></a>Für vertrauliche Clientanwendungen spezifische Modifizierer

Folgende Modifizierer können Sie für einen vertraulichen Clientanwendungsersteller festlegen:

|Modifizierer | BESCHREIBUNG|
|--------- | --------- |
|`.WithCertificate(X509Certificate2 certificate)` | Legt das Zertifikat fest, das die Anwendung bei Azure AD identifiziert.|
|`.WithClientSecret(string clientSecret)` | Legt das Clientgeheimnis (App-Kennwort) fest, das die Anwendung bei Azure AD identifiziert.|

Diese Modifizierer schließen sich gegenseitig aus. Wenn Sie beide angeben, löst die MSAL eine entsprechende Ausnahme aus.

### <a name="example-of-usage-of-modifiers"></a>Beispiel für die Verwendung von Modifizierern

Nehmen wir an, dass es sich bei Ihrer Anwendung um eine Branchenanwendung handelt, die nur für Ihre Organisation bestimmt ist.  In diesem Fall können Sie Folgendes schreiben:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAuthority(AzureCloudInstance.AzurePublic, tenantId)
        .Build();
```

Hierbei ist interessant, dass die Programmierung für nationale Clouds jetzt vereinfacht wurde. Wenn Sie Ihre Anwendung als mehrinstanzenfähige Anwendung in einer nationalen Cloud konzipiert haben, könnten Sie beispielsweise Folgendes schreiben:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAuthority(AzureCloudInstance.AzureUsGovernment, AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

Außerdem gibt es eine Außerkraftsetzung für AD FS (AD FS 2019 wird derzeit nicht unterstützt):
```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Wenn Sie ein Azure AD B2C-Entwickler sind, können Sie Ihren Mandanten wie folgt angeben:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Clientanwendung initialisiert haben, müssen Sie als Nächstes die Unterstützung für die Benutzeranmeldung, den autorisierten API-Zugriff oder beides hinzufügen.

Unsere Anwendungsszenariodokumentation enthält Anleitungen zum Anmelden eines Benutzers und zum Abrufen eines Zugriffstokens für den Zugriff auf eine API im Namen dieses Benutzers:

- [Web-App für Benutzeranmeldungen: An- und Abmeldung](scenario-web-app-sign-user-sign-in.md)
- [Web-App, die Web-APIs aufruft: Abrufen eines Tokens](scenario-web-app-call-api-acquire-token.md)
