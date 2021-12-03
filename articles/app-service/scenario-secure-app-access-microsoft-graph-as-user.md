---
title: 'Tutorial: Zugreifen auf Microsoft Graph als Benutzer durch eine Web-App | Azure'
description: In diesem Tutorial wird beschrieben, wie Sie für einen angemeldeten Benutzer auf Daten in Microsoft Graph zugreifen.
services: microsoft-graph, app-service-web
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 11/02/2021
ms.author: ryanwi
ms.reviewer: stsoneff
ms.custom: azureday1
ms.openlocfilehash: b20ae4e6cec7ad3ce710c6e9670ff580b30f2638
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131462308"
---
# <a name="tutorial-access-microsoft-graph-from-a-secured-app-as-the-user"></a>Tutorial: Zugreifen auf Microsoft Graph über eine geschützte App als Benutzer

Es wird beschrieben, wie Sie über eine Web-App, die in Azure App Service ausgeführt wird, auf Microsoft Graph zugreifen.

:::image type="content" alt-text="Diagramm: Zugreifen auf Microsoft Graph" source="./media/scenario-secure-app-access-microsoft-graph/web-app-access-graph.svg" border="false":::

Sie möchten den Zugriff auf Microsoft Graph über Ihre Web-App hinzufügen und als angemeldeter Benutzer eine Aktion durchführen. In diesem Abschnitt wird beschrieben, wie Sie der Web-App delegierte Berechtigungen gewähren und die Profilinformationen des angemeldeten Benutzers aus Azure Active Directory (Azure AD) abrufen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Gewähren von delegierten Berechtigungen für eine Web-App
> * Aufrufen von Microsoft Graph über eine Web-App für einen angemeldeten Benutzer

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Eine unter Azure App Service ausgeführte Webanwendung, für die das [App Service-Modul für die Authentifizierung/Autorisierung aktiviert ist](scenario-secure-app-authentication-app-service.md).

## <a name="grant-front-end-access-to-call-microsoft-graph"></a>Gewähren von Front-End-Zugriff zum Aufrufen von Microsoft Graph

Nachdem Sie nun die Authentifizierung und Autorisierung für Ihre Web-App aktiviert haben, wird die Web-App bei der Microsoft Identity Platform registriert und durch eine Azure AD-Anwendung unterstützt. In diesem Schritt gewähren Sie der Web-App die Berechtigungen für den Zugriff auf Microsoft Graph für den Benutzer. (In technischer Hinsicht gewähren Sie der Azure AD-Anwendung der Web-App die Berechtigungen zum Zugreifen auf die AD-Anwendung von Microsoft Graph für den Benutzer.)

Wählen Sie im Menü [Azure-Portal](https://portal.azure.com) die Option **Azure Active Directory** aus. Sie können auch auf einer beliebigen Seite **Azure Active Directory** suchen und auswählen.

Wählen Sie **App-Registrierungen** > **Anwendungen mit Besitzer** > **Alle Anwendungen im Verzeichnis anzeigen** aus. Wählen Sie den Namen Ihrer Web-App und dann die Option **API-Berechtigungen** aus.

Wählen Sie **Berechtigung hinzufügen** und dann „Microsoft-APIs“ und „Microsoft Graph“ aus.

Wählen Sie **Delegierte Berechtigungen** und in der Liste dann **User.Read** aus. Wählen Sie **Berechtigungen hinzufügen** aus.

## <a name="configure-app-service-to-return-a-usable-access-token"></a>Konfigurieren von App Service für die Rückgabe eines verwendbaren Zugriffstokens

Die Web-App verfügt nun über die erforderlichen Berechtigungen für den Zugriff auf Microsoft Graph als angemeldeter Benutzer. In diesem Schritt konfigurieren Sie die App Service-Authentifizierung und -Autorisierung, um ein verwendbares Zugriffstoken für den Zugriff auf Microsoft Graph zu erhalten. Für diesen Schritt müssen Sie den Bereich „User.Read“ für den Downstreamdienst (Microsoft Graph) hinzufügen: `https://graph.microsoft.com/User.Read`.

> [!IMPORTANT]
> Wenn Sie App Service nicht so konfigurieren, dass ein verwendbares Zugriffstoken zurückgegeben wird, erhalten Sie beim Aufrufen von Microsoft Graph-APIs in Ihrem Code den Fehler ```CompactToken parsing failed with error code: 80049217```.

# <a name="azure-resource-explorer"></a>[Azure-Ressourcen-Explorer](#tab/azure-resource-explorer)
Navigieren Sie zu [Azure-Ressourcen-Explorer](https://resources.azure.com/), und suchen Sie in der Ressourcenstruktur nach Ihrer Web-App. Die Ressourcen-URL sollte in etwa wie folgt lauten: `https://resources.azure.com/subscriptions/subscriptionId/resourceGroups/SecureWebApp/providers/Microsoft.Web/sites/SecureWebApp20200915115914`.

Der Azure-Ressourcen-Explorer wird nun mit der in der Ressourcenstruktur ausgewählten Web-App geöffnet. Wählen Sie am oberen Rand der Seite die Option **Lesen/Schreiben** aus, um die Bearbeitung Ihrer Azure-Ressourcen zu ermöglichen.

Führen Sie im Browser links einen Drilldown für **config** > **authsettingsV2** aus.

Wählen Sie in der Ansicht **authsettingsV2** die Option **Bearbeiten** aus. Suchen Sie den Abschnitt **login** von **identityProviders** ->  **azureActiveDirectory** und fügen Sie die folgenden **loginParameter**-Einstellungen hinzu: `"loginParameters":[ "response_type=code id_token","scope=openid offline_access profile https://graph.microsoft.com/User.Read" ]`.

```json
"identityProviders": {
    "azureActiveDirectory": {
      "enabled": true,
      "login": {
        "loginParameters":[
          "response_type=code id_token",
          "scope=openid offline_access profile https://graph.microsoft.com/User.Read"
        ]
      }
    }
  }
},
```

Speichern Sie Ihre Einstellungen, indem Sie **PUT** auswählen. Es kann mehrere Minuten dauern, bis die Einstellung wirksam wird. Ihre Web-App ist jetzt so konfiguriert, dass auf Microsoft Graph mit einem richtigen Zugriffstoken zugegriffen wird. Wenn nicht, wird von Microsoft Graph ein Fehler mit dem Hinweis zurückgegeben, dass das Format des kompakten Tokens fehlerhaft ist.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie die Azure-Befehlszeilenschnittstelle (CLI), um die Service Web App REST APIs aufzurufen, um die Authentifizierungskonfigurationseinstellungen [abzurufen](/rest/api/appservice/web-apps/get-auth-settings) und zu [aktualisieren](/rest/api/appservice/web-apps/update-auth-settings), damit Ihre Webanwendung Microsoft Graph aufrufen kann. Öffnen Sie ein Befehlsfenster, und melden Sie sich bei Azure CLI an:

```azurecli
az login
```

Rufen Sie Ihre bestehenden „config/authsettingsv2“-Einstellungen ab und speichern Sie sie in einer lokalen *authsettings.json*-Datei.

```azurecli
az rest --method GET --url '/subscriptions/{SUBSCRIPTION_ID}/resourceGroups/{RESOURCE_GROUP}/providers/Microsoft.Web/sites/{WEBAPP_NAME}/config/authsettingsv2/list?api-version=2020-06-01' > authsettings.json
```

Öffnen Sie die authsettings.jsin- Datei mit Ihrem bevorzugten Text-Editor. Suchen Sie den Abschnitt **login** von **identityProviders** ->  **azureActiveDirectory** und fügen Sie die folgenden **loginParameter**-Einstellungen hinzu: `"loginParameters":[ "response_type=code id_token","scope=openid offline_access profile https://graph.microsoft.com/User.Read" ]`.

```json
"identityProviders": {
    "azureActiveDirectory": {
      "enabled": true,
      "login": {
        "loginParameters":[
          "response_type=code id_token",
          "scope=openid offline_access profile https://graph.microsoft.com/User.Read"
        ]
      }
    }
  }
},
```

Speichern Sie Ihre Änderungen in der *authsettings.json*-Datei und laden Sie die lokalen Einstellungen in Ihre Webanwendung hoch:

```azurecli
az rest --method PUT --url '/subscriptions/{SUBSCRIPTION_ID}/resourceGroups/{RESOURCE_GROUP}/providers/Microsoft.Web/sites/{WEBAPP_NAME}/config/authsettingsv2?api-version=2020-06-01' --body @./authsettings.json
```
---

## <a name="call-microsoft-graph"></a>Aufrufen von Microsoft Graph

Ihre Web-App verfügt jetzt über die erforderlichen Berechtigungen, und den Anmeldeparametern wird die Client-ID von Microsoft Graph hinzugefügt.

# <a name="c"></a>[C#](#tab/programming-language-csharp)
Mit der [Microsoft.Identity.Web-Bibliothek](https://github.com/AzureAD/microsoft-identity-web/) ruft die Web-App ein Zugriffstoken für die Authentifizierung bei Microsoft Graph ab. In Version 1.2.0 und höher ist die Microsoft.Identity.Web-Bibliothek mit dem App Service-Modul für die Authentifizierung/Autorisierung integriert und kann parallel dazu ausgeführt werden. Microsoft.Identity.Web erkennt, dass die Web-App in App Service gehostet wird, und ruft das Zugriffstoken über das App Service-Modul für die Authentifizierung/Autorisierung ab. Das Zugriffstoken wird dann mit der Microsoft Graph-API an authentifizierte Anforderungen weitergeleitet.

Im [Beispiel auf GitHub](https://github.com/Azure-Samples/ms-identity-easyauth-dotnet-storage-graphapi/tree/main/2-WebApp-graphapi-on-behalf) können Sie sich diesen Code in einer Beispielanwendung ansehen.

> [!NOTE]
> Die Microsoft.Identity.Web-Bibliothek ist in Ihrer Web-App für die Standardauthentifizierung bzw. -autorisierung oder zum Authentifizieren von Anforderungen mit Microsoft Graph nicht erforderlich. Es ist möglich, [Downstream-APIs auf sichere Weise aufzurufen](tutorial-auth-aad.md#call-api-securely-from-server-code), wenn nur das App Service-Modul für die Authentifizierung/Autorisierung aktiviert ist.
> 
> Die Authentifizierung/Autorisierung von App Service ist für grundlegendere Authentifizierungsszenarios ausgelegt. Für komplexere Szenarien (z. B. die Verarbeitung von benutzerdefinierten Ansprüchen) benötigen Sie die Microsoft.Identity.Web-Bibliothek oder die [Microsoft Authentication Library](../active-directory/develop/msal-overview.md). Anfänglich müssen einige zusätzliche Einrichtungs- und Konfigurationsschritte ausgeführt werden, aber die Microsoft.Identity.Web-Bibliothek kann parallel zum App Service-Modul für die Authentifizierung/Autorisierung ausgeführt werden. Wenn mit Ihrer Web-App später dann komplexere Szenarien verarbeitet werden sollen, können Sie das App Service-Modul für die Authentifizierung/Autorisierung deaktivieren, und Microsoft.Identity.Web ist bereits Teil Ihrer App.

### <a name="install-client-library-packages"></a>Installieren von Clientbibliothekspaketen

Installieren Sie die NuGet-Pakete [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web/) und [Microsoft.Identity.Web.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph) in Ihrem Projekt, indem Sie die .NET Core-Befehlszeilenschnittstelle oder die Paket-Manager-Konsole in Visual Studio verwenden.

#### <a name="net-core-command-line"></a>.NET Core-Befehlszeile

Öffnen Sie eine Befehlszeile, und wechseln Sie zu dem Verzeichnis, in dem Ihre Projektdatei enthalten ist.

Führen Sie die Installationsbefehle aus.

```dotnetcli
dotnet add package Microsoft.Identity.Web.MicrosoftGraph

dotnet add package Microsoft.Identity.Web
```

#### <a name="package-manager-console"></a>Paket-Manager-Konsole

Öffnen Sie das Projekt bzw. die Projektmappe in Visual Studio, und navigieren Sie zu **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**, um die Konsole zu öffnen.

Führen Sie die Installationsbefehle aus.
```powershell
Install-Package Microsoft.Identity.Web.MicrosoftGraph

Install-Package Microsoft.Identity.Web
```

### <a name="startupcs"></a>Startup.cs

In der Datei *Startup.cs* wird Microsoft.Identity.Web mit der ```AddMicrosoftIdentityWebApp```-Methode Ihrer Web-App hinzugefügt. Mit der ```AddMicrosoftGraph```-Methode wird die Microsoft Graph-Unterstützung hinzugefügt.

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Identity.Web;
using Microsoft.AspNetCore.Authentication.OpenIdConnect;

// Some code omitted for brevity.
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
                .AddMicrosoftIdentityWebApp(Configuration.GetSection("AzureAd"))
                    .EnableTokenAcquisitionToCallDownstreamApi()
                        .AddMicrosoftGraph(Configuration.GetSection("Graph"))
                        .AddInMemoryTokenCaches();

        services.AddRazorPages();
    }
}

```

### <a name="appsettingsjson"></a>appsettings.json

Mit *AzureAd* wird die Konfiguration für die Microsoft.Identity.Web-Bibliothek angegeben. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Portalmenü die Option **Azure Active Directory** und dann die Option **App-Registrierungen** aus. Wählen Sie die App-Registrierung aus, die erstellt wurde, als Sie das App Service-Modul für die Authentifizierung/Autorisierung aktiviert haben. (Die App-Registrierung sollte den gleichen Namen wie Ihre Web-App haben.) Sie finden die Mandanten-ID und die Client-ID auf der Übersichtsseite der App-Registrierung. Den Domänennamen finden Sie auf der Seite mit der Azure AD-Übersicht für Ihren Mandanten.

Mit *Graph* werden der Microsoft Graph-Endpunkt und die anfänglichen Bereiche angegeben, die von der App benötigt werden.

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "fourthcoffeetest.onmicrosoft.com",
    "TenantId": "[tenant-id]",
    "ClientId": "[client-id]",
    // To call an API
    "ClientSecret": "[secret-from-portal]", // Not required by this scenario
    "CallbackPath": "/signin-oidc"
  },

  "Graph": {
    "BaseUrl": "https://graph.microsoft.com/v1.0",
    "Scopes": "user.read"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="indexcshtmlcs"></a>Index.cshtml.cs

Im folgenden Beispiel ist dargestellt, wie Sie Microsoft Graph als angemeldeter Benutzer aufrufen und Benutzerinformationen abrufen. Das Objekt ```GraphServiceClient``` wird in den Controller eingefügt, und die Authentifizierung wurde für Sie von der Microsoft.Identity.Web-Bibliothek konfiguriert.

```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc.RazorPages;
using Microsoft.Graph;
using System.IO;
using Microsoft.Identity.Web;
using Microsoft.Extensions.Logging;

// Some code omitted for brevity.

[AuthorizeForScopes(Scopes = new[] { "user.read" })]
public class IndexModel : PageModel
{
    private readonly ILogger<IndexModel> _logger;
    private readonly GraphServiceClient _graphServiceClient;

    public IndexModel(ILogger<IndexModel> logger, GraphServiceClient graphServiceClient)
    {
        _logger = logger;
        _graphServiceClient = graphServiceClient;
    }

    public async Task OnGetAsync()
    {
        try
        {
            var user = await _graphServiceClient.Me.Request().GetAsync();
            ViewData["Me"] = user;
            ViewData["name"] = user.DisplayName;

            using (var photoStream = await _graphServiceClient.Me.Photo.Content.Request().GetAsync())
            {
                byte[] photoByte = ((MemoryStream)photoStream).ToArray();
                ViewData["photo"] = Convert.ToBase64String(photoByte);
            }
        }
        catch (Exception ex)
        {
            ViewData["photo"] = null;
        }
    }
}
```

# <a name="nodejs"></a>[Node.js](#tab/programming-language-nodejs)

Die Web-App erhält das Zugriffstoken des Benutzers aus dem Header für eingehende Anforderungen. Anschließend wird das Token an den Microsoft Graph-Client übergeben, um eine authentifizierte Anforderung an den `/me`-Endpunkt zu senden.

In *graphController.js* im [Beispiel auf GitHub](https://github.com/Azure-Samples/ms-identity-easyauth-nodejs-storage-graphapi/tree/main/2-WebApp-graphapi-on-behalf) können Sie sich diesen Code in einer Beispielanwendung ansehen.

```nodejs
const graphHelper = require('../utils/graphHelper');

// Some code omitted for brevity.

exports.getProfilePage = async(req, res, next) => {

    try {
        const graphClient = graphHelper.getAuthenticatedClient(req.session.protectedResources["graphAPI"].accessToken);

        const profile = await graphClient
            .api('/me')
            .get();

        res.render('profile', { isAuthenticated: req.session.isAuthenticated, profile: profile, appServiceName: appServiceName });   
    } catch (error) {
        next(error);
    }
}
```

Verwenden Sie zum Abfragen von Microsoft Graph das [Microsoft Graph JavaScript SDK](https://github.com/microsoftgraph/msgraph-sdk-javascript). Der Code dafür befindet sich in [utils/graphHelper.js](https://github.com/Azure-Samples/ms-identity-easyauth-nodejs-storage-graphapi/blob/main/2-WebApp-graphapi-on-behalf/utils/graphHelper.js):

```nodejs
const graph = require('@microsoft/microsoft-graph-client');

// Some code omitted for brevity.

getAuthenticatedClient = (accessToken) => {
    // Initialize Graph client
    const client = graph.Client.init({
        // Use the provided access token to authenticate requests
        authProvider: (done) => {
            done(null, accessToken);
        }
    });

    return client;
}
```
---

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie dieses Tutorial abgeschlossen haben und die Web-App und die zugehörigen Ressourcen nicht mehr benötigen, sollten Sie [die von Ihnen erstellten Ressourcen bereinigen](scenario-secure-app-clean-up-resources.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
>
> * Gewähren von delegierten Berechtigungen für eine Web-App
> * Aufrufen von Microsoft Graph über eine Web-App für einen angemeldeten Benutzer

> [!div class="nextstepaction"]
> [Zugreifen auf Microsoft Graph durch App Service als App](scenario-secure-app-access-microsoft-graph-as-app.md)
