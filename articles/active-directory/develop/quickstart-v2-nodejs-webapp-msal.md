---
title: 'Schnellstart: Hinzufügen von Authentifizierung zu einer Node.js-Web-App mit MSAL Node | Azure'
titleSuffix: Microsoft identity platform
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Authentifizierung bei einer Node.js-Web-App und der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) für Node.js implementieren.
services: active-directory
author: mmacy
manager: celested
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/22/2020
ms.author: marsma
ms.custom: aaddev, scenarios:getting-started, languages:js, devx-track-js
ms.openlocfilehash: a593aef3ff99edb498e899b0d8db46c7b4636683
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131427701"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-node-web-app-using-the-auth-code-flow"></a>Schnellstart: Anmelden von Benutzern und Abrufen eines Zugriffstokens in einer Node-Web-App mithilfe des Autorisierungscodeflows

In dieser Schnellstartanleitung laden Sie ein Codebeispiel herunter und führen es aus, das zeigt, wie eine Node.js-Web-App Benutzer mithilfe des Autorisierungscodeflows anmelden kann. Das Codebeispiel veranschaulicht außerdem das Abrufen eines Zugriffstokens zum Aufrufen der Microsoft Graph-API.

Eine Abbildung finden Sie unter [Funktionsweise des Beispiels](#how-the-sample-works).

In dieser Schnellstartanleitung wird die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) für Node.js (MSAL Node) beim Autorisierungscodeflow verwendet.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. [Erstellen Sie ein kostenloses Azure-Abonnement.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) oder ein anderer Code-Editor

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Registrieren und Herunterladen Ihrer Schnellstartanwendung
>
> #### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren
>
> 1. Melden Sie sich beim <a href="https://portal.azure.com/" target="_blank">Azure-Portal</a> an.
> 1. Wenn Sie Zugriff auf mehrere Mandanten haben, verwenden Sie im Menü am oberen Rand den Filter **Verzeichnis + Abonnement** :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false":::, um den Mandanten auszuwählen, in dem Sie die Anwendung registrieren möchten.
> 1. Wählen Sie unter **Verwalten** Folgendes aus: **App-Registrierungen** > **Neue Registrierung**.
> 1. Geben Sie einen **Namen** für Ihre Anwendung ein. Benutzern Ihrer App wird wahrscheinlich dieser Namen angezeigt. Sie können ihn später ändern.
> 1. Wählen Sie unter **Unterstützte Kontotypen** **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** aus.
> 1. Legen Sie den Wert für **Umleitungs-URI** auf `http://localhost:3000/redirect` fest.
> 1. Wählen Sie **Registrieren**.
> 1. Notieren Sie sich für die spätere Verwendung auf der Seite **Übersicht** den Wert von **Anwendungs-ID (Client)** .
> 1. Wählen Sie unter **Verwalten** die Optionen **Zertifikate und Geheimnisse** > **Geheime Clientschlüssel** > **Neuer geheimer Clientschlüssel** aus.  Lassen Sie die Beschreibung und den Standardablauf leer, und wählen Sie **Hinzufügen** aus.
> 1. Notieren Sie sich den Wert von **Geheimer Clientschlüssel** zur späteren Verwendung.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-the-application-in-azure-portal"></a>Schritt 1: Konfigurieren der Anwendung im Azure-Portal
> Damit das Codebeispiel für diese Schnellstartanleitung funktioniert, müssen Sie einen geheimen Clientschlüssel erstellen und die folgende Antwort-URL hinzufügen: `http://localhost:3000/redirect`.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Diese Änderung für mich vornehmen]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Bereits konfiguriert](media/quickstart-v2-windows-desktop/green-check.png): Ihre Anwendung ist mit diesen Attributen konfiguriert.

#### <a name="step-2-download-the-project"></a>Schritt 2: Herunterladen des Projekts

> [!div renderon="docs"]
> Um das Projekt mit einem Webserver unter Verwendung von Node.js auszuführen, [laden Sie die Kernprojektdateien herunter](https://github.com/Azure-Samples/ms-identity-node/archive/main.zip).

> [!div renderon="portal" class="sxs-lookup"]
> Führen Sie das Projekt mit einem Webserver unter Verwendung von Node.js aus:

> [!div renderon="portal" class="sxs-lookup" id="autoupdate" class="nextstepaction"]
> [Laden Sie das Codebeispiel herunter](https://github.com/Azure-Samples/ms-identity-node/archive/main.zip).

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-node-app"></a>Schritt 3: Konfigurieren Ihrer Node-App
>
> Extrahieren Sie das Projekt, und öffnen Sie den Ordner *ms-identity-node-main* und dann die Datei *index.js*.
>
> Legen Sie den Wert `clientID` mit der Anwendungs-ID (Client) und dann den Wert `clientSecret` mit dem geheimen Clientschlüssel fest.
>
>```javascript
>const config = {
>    auth: {
>        clientId: "Enter_the_Application_Id_Here",
>        authority: "https://login.microsoftonline.com/common",
>        clientSecret: "Enter_the_Client_Secret_Here"
>    },
>    system: {
>        loggerOptions: {
>            loggerCallback(loglevel, message, containsPii) {
>                console.log(message);
>            },
>            piiLoggingEnabled: false,
>            logLevel: msal.LogLevel.Verbose,
>        }
>    }
>};
> ```

> [!div renderon="docs"]
>
> Ändern Sie die Werte im Abschnitt `config`:
>
> - `Enter_the_Application_Id_Here` ist die Anwendungs-ID (Client) für die von Ihnen registrierte Anwendung.
>
>    Die Anwendungs-ID (Client) finden Sie im Azure-Portal auf der Seite **Übersicht** der App-Registrierung.
> - `Enter_the_Client_Secret_Here` ist der geheime Clientschlüssel für die von Ihnen registrierte Anwendung.
>
>    Wählen Sie zum Abrufen oder Generieren eines neuen geheimen Clientschlüssels unter **Verwalten** die Option **Zertifikate und Geheimnisse** aus.
>
> Der Standardwert `authority` stellt die Azure-Hauptcloud (global) dar:
>
> ```javascript
> authority: "https://login.microsoftonline.com/common",
> ```
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Schritt 3: Ihre App ist konfiguriert und betriebsbereit
>
> [!div renderon="docs"]
>
> #### <a name="step-4-run-the-project"></a>Schritt 4: Ausführen des Projekts

Führen Sie das Projekt mithilfe von Node.js aus.

1. Führen Sie im Projektverzeichnis die folgenden Befehle aus, um den Server zu starten:

    ```console
    npm install
    npm start
    ```

1. Wechseln Sie zu `http://localhost:3000/`.

1. Wählen Sie **Anmelden** aus, um den Anmeldeprozess zu starten.

    Bei der ersten Anmeldung werden Sie aufgefordert, einzuwilligen, dass die Anwendung auf Ihr Profil zugreifen und Sie anmelden darf. Nachdem Sie sich erfolgreich angemeldet haben, wird in der Befehlszeile eine Protokollmeldung angezeigt.

## <a name="more-information"></a>Weitere Informationen

### <a name="how-the-sample-works"></a>Funktionsweise des Beispiels

Das Beispiel hostet einen Webserver auf „localhost“, Port 3000. Wenn ein Webbrowser auf diese Website zugreift, leitet das Beispiel den Benutzer sofort zu einer Microsoft-Authentifizierungsseite weiter. Aus diesem Grund enthält das Beispiel keine HTML- oder Anzeigeelemente. Bei erfolgreicher Authentifizierung wird die Meldung „OK“ angezeigt.

### <a name="msal-node"></a>MSAL Node

Über die Bibliothek „MSAL Node“ werden Benutzer angemeldet und die Token angefordert, die für den Zugriff auf eine durch Microsoft Identity Platform geschützte API verwendet werden. Sie können die neueste Version mithilfe von Node.js-Paket-Manager (npm) herunterladen:

```console
npm install @azure/msal-node
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Auth zu einer vorhandenen Web-App – GitHub-Codebeispiel >](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/samples/msal-node-samples/auth-code)
