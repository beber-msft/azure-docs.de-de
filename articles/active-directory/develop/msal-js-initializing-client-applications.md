---
title: Initialisieren von MSAL.js-Client-Apps | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über die Initialisierung von Clientanwendungen mithilfe der Microsoft Authentication Library für JavaScript (MSAL.js).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/21/2021
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev, devx-track-js
ms.openlocfilehash: 2680f78f7bcd1caaa3d6d1280c51c81b97a52048
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131050639"
---
# <a name="initialize-client-applications-using-msaljs"></a>Initialisieren von Clientanwendungen mithilfe von MSAL.js

Dieser Artikel beschreibt die Initialisierung der Microsoft Authentication Library für JavaScript (MSAL.js) mit einer Instanz einer Benutzer-Agent-Anwendung.

Die Benutzer-Agent-Anwendung ist eine Form der öffentlichen Clientanwendung, bei der der Clientcode in einem Benutzer-Agent wie beispielsweise einem Webbrowser ausgeführt wird. Diese Clients speichern keine Geheimnisse, da der Browserkontext offen zugänglich ist.

Weitere Informationen zu Clientanwendungstypen und Optionen für die Anwendungskonfiguration finden Sie unter [Öffentliche und vertrauliche Client-Apps in MSAL](msal-client-applications.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie eine Anwendung initialisieren, müssen Sie zunächst [die Anwendung im Azure-Portal registrieren](scenario-spa-app-registration.md), um eine Vertrauensstellung zwischen Ihrer Anwendung und Microsoft Identity Platform herzustellen.

Nachdem Sie Ihre App registriert haben, benötigen Sie mindestens einige der folgenden Werte, die im Azure-Portal zu finden sind.

| Wert                   | Erforderlich | BESCHREIBUNG                                                                                                                                                                |
| :---------------------- | :------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Anwendungs-ID (Client) | Erforderlich | Eine GUID, die Ihre Anwendung in Microsoft Identity Platform eindeutig identifiziert.                                                                                   |
| Authority               | Optional | Die URL des Identitätsanbieters (die _Instanz_) und die _Zielgruppe für die Anmeldung_ für Ihre Anwendung. Wenn Instanz und Zielgruppe für die Anmeldung verkettet werden, bilden sie die _Autorität_. |
| Verzeichnis-ID (Mandant)   | Optional | Geben Sie die Verzeichnis-ID (Mandant) an, wenn Sie eine line-of-business-Anwendung ausschließlich für Ihre Organisation erstellen, die häufig als _Anwendung mit nur einem Mandanten_ bezeichnet wird.          |
| Umleitungs-URI            | Optional | Wenn Sie eine Web-App entwickeln, gibt der `redirectUri` an, wohin der Identitätsanbieter (Microsoft Identity Platform) die ausgestellten Sicherheitstoken zurückgeben soll.   |

## <a name="initialize-msaljs-2x-apps"></a>Initialisieren von MSAL.js 2.x-Apps

Initialisieren Sie den MSAL-Authentifizierungskontext, indem Sie [PublicClientApplication][msal-js-publicclientapplication] mit einem [Konfigurations][msal-js-configuration]objekt instanziieren. Für die Konfiguration ist mindestens eine Eigenschaft, die `clientID` Ihrer Anwendung, erforderlich. Diese wird im Azure-Portal auf der **Übersichtsseite** für die App-Registrierung als **Anwendungs-ID (Client)** angezeigt.

Im Folgenden finden Sie ein Beispiel für ein Konfigurationsobjekt und die Instanziierung von `PublicClientApplication`:

```javascript
const msalConfig = {
  auth: {
    clientId: "11111111-1111-1111-111111111111",
    authority: "https://login.microsoftonline.com/common",
    knownAuthorities: [],
    redirectUri: "https://localhost:3001",
    postLogoutRedirectUri: "https://localhost:3001/logout",
    navigateToLoginRequestUrl: true,
  },
  cache: {
    cacheLocation: "sessionStorage",
    storeAuthStateInCookie: false,
  },
  system: {
    loggerOptions: {
      loggerCallback: (
        level: LogLevel,
        message: string,
        containsPii: boolean
      ): void => {
        if (containsPii) {
          return;
        }
        switch (level) {
          case LogLevel.Error:
            console.error(message);
            return;
          case LogLevel.Info:
            console.info(message);
            return;
          case LogLevel.Verbose:
            console.debug(message);
            return;
          case LogLevel.Warning:
            console.warn(message);
            return;
        }
      },
      piiLoggingEnabled: false,
    },
    windowHashTimeout: 60000,
    iframeHashTimeout: 6000,
    loadFrameTimeout: 0,
  },
};

// Create an instance of PublicClientApplication
const msalInstance = new PublicClientApplication(msalConfig);

// Handle the redirect flows
msalInstance
  .handleRedirectPromise()
  .then((tokenResponse) => {
    // Handle redirect response
  })
  .catch((error) => {
    // Handle redirect error
  });
```

### `handleRedirectPromise`

Rufen Sie [handleRedirectPromise][msal-js-handleredirectpromise] auf, wenn Ihre Anwendung die Umleitungsflows verwendet. Bei Verwendung von Umleitungsflows sollte `handleRedirectPromise` bei jedem Laden der Seite ausgeführt werden.

Die Zusage (Promise) kann drei mögliche Ergebnisse beinhalten:

- `.then` wird aufgerufen, und `tokenResponse` ist wahr: Die Anwendung kehrt von einem erfolgreichen Umleitungsvorgang zurück.
- `.then` wird aufgerufen, und `tokenResponse` ist falsch (`null`): Die Anwendung kehrt nicht von einem Umleitungsvorgang zurück.
- `.catch` wird aufgerufen: Die Anwendung kehrt von einem Umleitungsvorgang zurück, und es ist ein Fehler aufgetreten.

## <a name="initialize-msaljs-1x-apps"></a>Initialisieren von MSAL.js 1.x-Apps

Initialisieren Sie den MSAL 1.x-Authentifizierungskontext, indem Sie [UserAgentApplication][msal-js-useragentapplication] mit einem Konfigurationsobjekt instanziieren. Für die Konfiguration ist mindestens eine Eigenschaft, die `clientID` Ihrer Anwendung, erforderlich. Diese wird im Azure-Portal auf der **Übersichtsseite** für die App-Registrierung als **Anwendungs-ID (Client)** angezeigt.

Bei Authentifizierungsmethoden mit Umleitungsflows ([loginRedirect][msal-js-loginredirect] und [acquireTokenRedirect][msal-js-acquiretokenredirect]) in MSAL.js 1.2.x oder früheren Versionen müssen Sie mit der Methode `handleRedirectCallback()` explizit einen Rückruf bei Erfolg oder Fehler registrieren. Die explizite Registrierung des Rückrufs ist in MSAL.js 1.2.x und früheren Versionen erforderlich, da Umleitungsflows keine Zusagen zurückgeben, wie das bei Methoden mit einer Popup-Darstellung der Fall ist. Die Registrierung des Rückrufs ist in MSAL.js 1.3.x und höher _optional_.

```javascript
// Configuration object constructed
const msalConfig = {
  auth: {
    clientId: "11111111-1111-1111-111111111111",
  },
};

// Create UserAgentApplication instance
const msalInstance = new UserAgentApplication(msalConfig);

function authCallback(error, response) {
  // Handle redirect response
}

// Register a redirect callback for Success or Error (when using redirect methods)
// **REQUIRED** in MSAL.js 1.2.x and earlier
// **OPTIONAL** in MSAL.js 1.3.x and later
msalInstance.handleRedirectCallback(authCallback);
```

## <a name="single-instance-and-configuration"></a>Einzelne Instanz und Konfiguration

MSAL.js 1.x und 2.x sind so konzipiert, dass es nur eine Instanz und Konfiguration für `UserAgentApplication` bzw. `PublicClientApplication` gibt, um einen einzigen Authentifizierungskontext darzustellen.

Mehrere Instanzen von `UserAgentApplication` bzw. `PublicClientApplication` werden nicht empfohlen, da sie zu widersprüchlichen Cacheeinträgen und Verhaltensweisen im Browser führen.

## <a name="next-steps"></a>Nächste Schritte

Das folgende MSAL.js 2.x-Codebeispiel auf GitHub veranschaulicht die Instanziierung von [PublicClientApplication][msal-js-publicclientapplication] mit einem [Konfigurations][msal-js-configuration]objekt:

[Azure-Samples/ms-identity-javascript-v2](https://github.com/Azure-Samples/ms-identity-javascript-v2)

<!-- LINKS - External -->

[msal-browser]: https://azuread.github.io/microsoft-authentication-library-for-js/ref/msal-browser/
[msal-core]: https://azuread.github.io/microsoft-authentication-library-for-js/ref/msal-core/
[msal-js-acquiretokenredirect]: https://azuread.github.io/microsoft-authentication-library-for-js/ref/classes/_azure_msal.useragentapplication.html#acquiretokenredirect
[msal-js-configuration]: https://azuread.github.io/microsoft-authentication-library-for-js/ref/modules/_azure_msal.html#configuration
[msal-js-handleredirectpromise]: https://azuread.github.io/microsoft-authentication-library-for-js/ref/classes/_azure_msal_browser.publicclientapplication.html#handleredirectpromise
[msal-js-loginredirect]: https://azuread.github.io/microsoft-authentication-library-for-js/ref/classes/_azure_msal.useragentapplication.html#loginredirect
[msal-js-publicclientapplication]: https://azuread.github.io/microsoft-authentication-library-for-js/ref/classes/_azure_msal_browser.publicclientapplication.html
[msal-js-useragentapplication]: https://azuread.github.io/microsoft-authentication-library-for-js/ref/classes/_azure_msal.useragentapplication.html
