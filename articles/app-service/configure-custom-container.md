---
title: Konfigurieren eines benutzerdefinierten Containers
description: Erfahren Sie, wie Sie einen benutzerdefinierten Container in Azure App Service konfigurieren. In diesem Artikel werden die gängigsten Konfigurationsaufgaben vorgestellt.
ms.topic: how-to
ms.date: 10/22/2021
ms.custom: devx-track-azurepowershell, devx-track-azurecli
zone_pivot_groups: app-service-containers-windows-linux
ms.openlocfilehash: bfcd178a43e7a21ea6ef35d4462956bd49037e6f
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131006865"
---
# <a name="configure-a-custom-container-for-azure-app-service"></a>Konfigurieren eines benutzerdefinierten Containers für Azure App Service

In diesem Artikel erfahren Sie, wie Sie einen benutzerdefinierten Container für die Ausführung unter Azure App Service konfigurieren.

::: zone pivot="container-windows"

Dieser Leitfaden enthält die wichtigsten Konzepte und Anweisungen für die Containerstellung von Windows-Apps in App Service. Wenn Sie Azure App Service noch nie verwendet haben, befolgen Sie zunächst den [Schnellstart zu benutzerdefinierten Containern](quickstart-custom-container.md) und das [Tutorial](tutorial-custom-container.md).

::: zone-end

::: zone pivot="container-linux"

Dieser Leitfaden enthält die wichtigsten Konzepte und Anweisungen für die Containerstellung von Linux-Apps in App Service. Wenn Sie Azure App Service noch nie verwendet haben, befolgen Sie zunächst den [Schnellstart zu benutzerdefinierten Containern](quickstart-custom-container.md) und das [Tutorial](tutorial-custom-container.md). Es gibt auch eine Schnellstartanleitung für [Apps mit mehreren Containern](quickstart-multi-container.md) und ein [Tutorial](tutorial-multi-container-app.md).

::: zone-end

::: zone pivot="container-windows"

## <a name="supported-parent-images"></a>Unterstützte übergeordnete Images

Für Ihr benutzerdefiniertes Windows-Image müssen Sie das passende [übergeordnete Image (Basisimage)](https://docs.docker.com/develop/develop-images/baseimages/) für das gewünschte Framework auswählen:

- Verwenden Sie zum Bereitstellen von .NET Framework-Apps ein übergeordnetes Image, das auf dem [LTSC](/windows-server/get-started/servicing-channels-comparison#long-term-servicing-channel-ltsc)-Release (Long-Term Servicing Channel, langfristiger Wartungskanal) von Windows Server 2019 Core basiert. 
- Verwenden Sie zum Bereitstellen von .NET Core-Apps ein übergeordnetes Image, das auf dem Release [Semi-Annual Servicing Channel (SAC)](/windows-server/get-started/servicing-channels-comparison#semi-annual-channel) von Windows Server 2019 Nano basiert. 

Während des App-Starts dauert das Herunterladen eines übergeordneten Images eine Weile. Sie können die Startzeit jedoch reduzieren, indem Sie eins der folgenden übergeordneten Images verwenden, die bereits in Azure App Service zwischengespeichert sind:

- [mcr.microsoft.com/windows/servercore](https://hub.docker.com/_/microsoft-windows-servercore):20H2
- [mcr.microsoft.com/windows/servercore](https://hub.docker.com/_/microsoft-windows-servercore):ltsc2019
- [mcr.microsoft.com/dotnet/framework/aspnet](https://hub.docker.com/_/microsoft-dotnet-framework-aspnet/):4.8-windowsservercore-20H2
- [mcr.microsoft.com/dotnet/framework/aspnet](https://hub.docker.com/_/microsoft-dotnet-framework-aspnet/):4.8-windowsservercore-ltsc2019
- [mcr.microsoft.com/dotnet/runtime](https://hub.docker.com/_/microsoft-dotnet-runtime/):5.0-nanoserver-20H2
- [mcr.microsoft.com/dotnet/runtime](https://hub.docker.com/_/microsoft-dotnet-runtime/):5.0-nanoserver-1809
- [mcr.microsoft.com/dotnet/aspnet](https://hub.docker.com/_/microsoft-dotnet-aspnet/):5.0-nanoserver-20H2
- [mcr.microsoft.com/dotnet/aspnet](https://hub.docker.com/_/microsoft-dotnet-aspnet/):5.0-nanoserver-1809
- [mcr.microsoft.com/dotnet/runtime](https://hub.docker.com/_/microsoft-dotnet-runtime/):3.1-nanoserver-20H2
- [mcr.microsoft.com/dotnet/runtime](https://hub.docker.com/_/microsoft-dotnet-runtime/):3.1-nanoserver-1809
- [mcr.microsoft.com/dotnet/aspnet](https://hub.docker.com/_/microsoft-dotnet-aspnet/):3.1-nanoserver-20H2
- [mcr.microsoft.com/dotnet/aspnet](https://hub.docker.com/_/microsoft-dotnet-aspnet/):3.1-nanoserver-1809

::: zone-end

## <a name="change-the-docker-image-of-a-custom-container"></a>Ändern des Docker-Images eines benutzerdefinierten Containers

Wenn Sie eine vorhandene benutzerdefinierte Container-App vom aktuellen Docker-Image auf ein neues Image umstellen möchten, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group <group-name> --docker-custom-image-name <docker-hub-repo>/<image>
```

## <a name="use-an-image-from-a-private-registry"></a>Verwenden eines Images aus einer privaten Registrierung

Führen Sie den folgenden Befehl aus, um ein Image aus einer privaten Registrierung wie Azure Container Registry zu verwenden:

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group <group-name> --docker-custom-image-name <image-name> --docker-registry-server-url <private-repo-url> --docker-registry-server-user <username> --docker-registry-server-password <password>
```

Geben Sie für *\<username>* und *\<password>* die Anmeldeinformationen für das Konto Ihrer privaten Registrierung an.

## <a name="use-managed-identity-to-pull-image-from-azure-container-registry"></a>Verwenden Sie eine verwaltete Identität zum Pullen eines Image aus Azure Container Registry.

Folgen Sie den folgenden Schritte, um Ihre Web-App zum Pullen aus ACR mithilfe einer verwalteten Identität zu konfigurieren. In diesen Schritten wird eine systemseitig zugewiesene verwaltete Identität verwendet, aber Sie können auch eine benutzerseitig zugewiesene verwaltete Identität verwenden.

1. Aktivieren Sie mithilfe des Befehls [`az webapp identity assign`](/cli/azure/webapp/identity#az_webapp_identity-assign) eine [ dem System zugewiesene verwaltete Identität](./overview-managed-identity.md) für die Web-App:

    ```azurecli-interactive
    az webapp identity assign --resource-group <group-name> --name <app-name> --query principalId --output tsv
    ```
    Ersetzen Sie `<app-name>` durch den Namen, den Sie im vorherigen Schritt verwendet haben. Die Ausgabe des Befehls (gefiltert nach den Argumenten --query und --output) ist die Dienstprinzipal-ID der zugewiesenen Identität, den Sie in Kürze verwenden.
1. Abrufen der Ressourcen-ID von Azure Container Registry:
    ```azurecli-interactive
    az acr show --resource-group <group-name> --name <registry-name> --query id --output tsv
    ```
    Ersetzen Sie `<registry-name>` durch den Namen Ihrer Registrierung. Die Ausgabe des Befehls (gefiltert nach den Argumenten --query und --output) ist die Ressourcen-ID von Azure Container Registry.
1. Erteilen Sie der verwalteten Identität die Berechtigung für den Zugriff auf die Containerregistrierung:

    ```azurecli-interactive
    az role assignment create --assignee <principal-id> --scope <registry-resource-id> --role "AcrPull"
    ```

    Ersetzen Sie die folgenden Werte:
    - `<principal-id>` durch die Dienstprinzipal-ID aus dem Befehl `az webapp identity assign`
    - `<registry-resource-id>` durch die ID Ihrer Containerregistrierung aus dem `az acr show`-Befehl

    Weitere Informationen zu diesen Berechtigungen finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung von Azure?](../role-based-access-control/overview.md).

1. Konfigurieren Sie Ihre App so, dass sie die verwaltete Identität zum Abrufen aus der Azure Container Registry verwendet.

    ```azurecli-interactive
    az webapp config set --resource-group <group-name> --name <app-name> --generic-configurations '{"acrUseManagedIdentityCreds": true}'
    ```

    Ersetzen Sie die folgenden Werte:
    - `<app-name>` durch den Namen Ihrer Web-App
    >[!Tip]
    > Wenn Sie die Befehle mithilfe der PowerShell-Konsole ausführen, müssen Sie die Zeichenfolgen im `--generic-configurations`-Argument in diesem und im nächsten Schritt escapen. Beispiel: `--generic-configurations '{\"acrUseManagedIdentityCreds\": true'`
1. (Optional) Wenn Ihre App eine [benutzerseitig zugewiesene verwaltete Identität](overview-managed-identity.md#add-a-user-assigned-identity) verwendet, stellen Sie sicher, dass diese für die Web-App konfiguriert ist, und legen Sie dann eine zusätzliche Eigenschaft `acrUserManagedIdentityID` fest, um ihre Client-ID anzugeben:
    
    ```azurecli-interactive
    az identity show --resource-group <group-name> --name <identity-name> --query clientId --output tsv
    ```
    Ersetzen Sie `<identity-name>` Ihrer benutzerseitig zugewiesenen verwalteten Identität, und verwenden Sie die Ausgabe `<client-id>`, um die ID der benutzerseitig zugewiesenen verwalteten Identität zu konfigurieren.

    ```azurecli-interactive
    az  webapp config set --resource-group <group-name> --name <app-name> --generic-configurations '{"acrUserManagedIdentityID": "<client-id>"}'
    ```

Jetzt ist alles festgelegt, und die Web-App verwendet jetzt die verwaltete Identität, um aus Azure Container Registry zu pullen. 

::: zone pivot="container-linux"

## <a name="use-an-image-from-a-network-protected-registry"></a>Verwenden eines Images aus einer netzwerkgeschützten Registrierung

Um eine Verbindung mit einer Registrierung in einem virtuellen Netzwerk oder lokal herzustellen und zu pullen, muss Ihre App mithilfe des VNet-Integrationsfeatures mit einem virtuellen Netzwerk verbunden sein. Dies ist auch für Azure Container Registry mit privatem Endpunkt erforderlich. Wenn Ihre Netzwerk- und DNS-Lösung konfiguriert sind, aktivieren Sie das Routing des Image-Pulls über das VNet, indem Sie die App-Einstellung `WEBISTE_PULL_IMAGE_OVER_VNET=true` festlegen:

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --settings WEBISTE_PULL_IMAGE_OVER_VNET=true
```

::: zone-end

## <a name="i-dont-see-the-updated-container"></a>Ich kann den aktualisierten Container nicht sehen

Wenn Sie Ihre Docker-Containereinstellungen so ändern, dass sie auf einen neuen Container verweisen, kann es ein paar Minuten dauern, bis die App HTTP-Anforderungen aus dem neuen Container verarbeitet. Während der neue Container abgerufen und gestartet wird, bedient App Service weiterhin Anforderungen aus dem alten Container. Nur wenn der neue Container gestartet wird und zum Empfangen von Anforderungen bereit ist, beginnt App Service mit dem Senden von Anforderungen an ihn.

## <a name="how-container-images-are-stored"></a>Speichern von Containerimages

Wenn Sie ein benutzerdefiniertes Docker-Image zum ersten Mal in App Service ausführen, führt App Service ein `docker pull` aus und ruft alle Imageebenen ab. Diese Ebenen werden auf dem Datenträger gespeichert, so als ob Sie Docker lokal verwendeten. Bei jedem Neustart der App führt App Service ein `docker pull` aus, ruft aber nur Schichten ab, die sich geändert haben. Wenn keine Änderungen vorliegen, verwendet App Service vorhandene Ebenen auf dem lokalen Datenträger.

Wenn die App Compute-Instanzen aus irgendeinem Grund ändert (z. B. durch zentrales Hoch- oder Herunterskalieren der Tarife), muss App Service alle Ebenen erneut abrufen. Dasselbe trifft zu, wenn Sie eine Aufskalierung durchführen, um zusätzliche Instanzen hinzuzufügen. Es gibt auch seltene Fälle, in denen sich App-Instanzen ohne einen Skalierungsvorgang ändern können.

## <a name="configure-port-number"></a>Konfigurieren der Portnummer

App Service geht standardmäßig davon aus, dass der benutzerdefinierte Container an Port 80 lauscht. Wenn Ihr Container an einem anderen Port lauscht, legen Sie die App-Einstellung `WEBSITES_PORT` in Ihrer App Service-App fest. Sie können sie über die [Cloud Shell](https://shell.azure.com) festlegen. In Bash:

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --settings WEBSITES_PORT=8000
```

PowerShell:

```azurepowershell-interactive
Set-AzWebApp -ResourceGroupName <group-name> -Name <app-name> -AppSettings @{"WEBSITES_PORT"="8000"}
```

App Service ermöglicht es Ihrem Container zurzeit, nur einen Port für HTTP-Anforderungen verfügbar zu machen. 

## <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Ihr benutzerdefinierter Container kann Umgebungsvariablen verwenden, die extern bereitgestellt werden müssen. Sie können sie über die [Cloud Shell](https://shell.azure.com) übergeben. In Bash:

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --settings DB_HOST="myownserver.mysql.database.azure.com"
```

PowerShell:

```azurepowershell-interactive
Set-AzWebApp -ResourceGroupName <group-name> -Name <app-name> -AppSettings @{"DB_HOST"="myownserver.mysql.database.azure.com"}
```

Wenn Ihre App ausgeführt wird, werden die Einstellungen der App Service-App automatisch als Umgebungsvariablen in den Prozess eingefügt. Sie können Containerumgebungsvariablen mit der URL `https://<app-name>.scm.azurewebsites.net/Env)` überprüfen.

Wenn Ihre App Images aus einer privaten Registrierung oder von Docker Hub verwendet, werden die Anmeldeinformationen für den Zugriff auf das Repository in den Umgebungsvariablen `DOCKER_REGISTRY_SERVER_URL`, `DOCKER_REGISTRY_SERVER_USERNAME` und `DOCKER_REGISTRY_SERVER_PASSWORD` gespeichert. Aufgrund von Sicherheitsrisiken wird keiner dieser reservierten Variablennamen in der Anwendung verfügbar gemacht.

::: zone pivot="container-windows"
Für Container, die auf IIS oder .NET Framework (4.0 oder höher) basieren, werden sie von App Service automatisch als .NET-App-Einstellungen und -Verbindungszeichenfolgen in `System.ConfigurationManager` eingefügt. Für alle anderen Sprachen oder Frameworks werden sie als Umgebungsvariablen für den Prozess bereitgestellt, wobei eines der folgenden entsprechenden Präfixe verwendet wird:

- `APPSETTING_`
- `SQLCONTR_`
- `MYSQLCONTR_`
- `SQLAZURECOSTR_`
- `POSTGRESQLCONTR_`
- `CUSTOMCONNSTR_`

::: zone-end

::: zone pivot="container-linux"

Diese Methode funktioniert sowohl für Apps mit einem einzelnen Container als auch für Apps mit mehreren Containern, wobei die Umgebungsvariablen in der Datei *docker-compose.yml* angegeben sind.

::: zone-end

## <a name="use-persistent-shared-storage"></a>Verwenden von beständig freigegebenem Speicher

::: zone pivot="container-windows"

Sie können das Verzeichnis *C:\home* im Dateisystem Ihrer App verwenden, damit Dateien auch nach einem Neustart erhalten bleiben und instanzübergreifend freigegeben werden können. Das `C:\home` wird in Ihrer App bereitgestellt, damit Ihre Container-App auf den beständigen Speicher zugreifen kann.

Wenn der beständige Speicher deaktiviert ist, bleiben Schreibvorgänge in das Verzeichnis `C:\home` nicht erhalten. [Docker-Hostprotokolle und -Containerprotokolle](#access-diagnostic-logs) werden in einem standardmäßigen permanenten freigegebenen Speicher gespeichert, der nicht an den Container angefügt ist. Wenn der beständige Speicher aktiviert ist, bleiben alle Schreibvorgänge in das Verzeichnis `C:\home` erhalten, alle Instanzen einer horizontal skalierten App können darauf zugreifen, und das Protokoll ist unter `C:\home\LogFiles` verfügbar.

::: zone-end

::: zone pivot="container-linux"

Sie können das Verzeichnis */home* im Dateisystem Ihrer App verwenden, damit Dateien auch nach einem Neustart erhalten bleiben und instanzübergreifend freigegeben werden können. Das `/home` wird in Ihrer App bereitgestellt, damit Ihre Container-App auf den beständigen Speicher zugreifen kann.

Wenn der beständige Speicher deaktiviert ist, bleiben Schreibvorgänge in das Verzeichnis `/home` nicht über App-Neustarts oder mehrere Instanzen hinweg erhalten. Die einzige Ausnahme ist das Verzeichnis `/home/LogFiles`, das zum Speichern der Docker- und Containerprotokolle verwendet wird. Wenn der beständige Speicher aktiviert ist, bleiben alle Schreibvorgänge in das Verzeichnis `/home` erhalten und können von allen Instanzen einer horizontal skalierten App aufgerufen werden.

::: zone-end

Standardmäßig ist der beständige Speicher deaktiviert, und die Einstellung wird in den App-Einstellungen nicht verfügbar gemacht. Um ihn zu aktivieren, legen Sie die App-Einstellung `WEBSITES_ENABLE_APP_SERVICE_STORAGE` über die [Cloud Shell](https://shell.azure.com) fest. In Bash:

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=true
```

PowerShell:

```azurepowershell-interactive
Set-AzWebApp -ResourceGroupName <group-name> -Name <app-name> -AppSettings @{"WEBSITES_ENABLE_APP_SERVICE_STORAGE"=true}
```

> [!NOTE]
> Sie können auch [Ihren eigenen beständigen Speicher konfigurieren](configure-connect-to-azure-storage.md).

## <a name="detect-https-session"></a>Erkennen einer HTTPS-Sitzung

App Service beendet TLS/SSL auf den Front-Ends. Dies bedeutet, dass TLS/SSL-Anforderungen nie bis zu Ihrer App gelangen. Sie müssen und sollten keine Unterstützung für TLS/SSL in Ihrer App implementieren. 

Die Front-Ends befinden sich in Azure-Rechenzentren. Wenn Sie TLS/SSL mit ihrer App verwenden, wird Ihr Datenverkehr über das Internet immer sicher verschlüsselt.

::: zone pivot="container-windows"

## <a name="customize-aspnet-machine-key-injection"></a>Anpassen des Einfügens des ASP.NET-Computerschlüssels

 Während des Containerstarts werden automatisch generierte Schlüssel als Computerschlüssel für ASP.NET-Kryptografieroutinen in den Container eingefügt. Sie können [diese Schlüssel in Ihrem Container finden](#connect-to-the-container), indem Sie nach den folgenden Umgebungsvariablen suchen: `MACHINEKEY_Decryption`, `MACHINEKEY_DecryptionKey`, `MACHINEKEY_ValidationKey`, `MACHINEKEY_Validation`. 

Die neuen Schlüssel können bei jedem Neustart die ASP.NET-Formularauthentifizierung und den Ansichtszustand zurücksetzen, wenn Ihre App von ihnen abhängig ist. Um die automatische Neugenerierung von Schlüsseln zu verhindern, [legen Sie sie manuell als App Service-App-Einstellungen](#configure-environment-variables) fest. 

## <a name="connect-to-the-container"></a>Herstellen einer Verbindung mit dem Container

Sie können eine direkte Verbindung mit Ihrem Windows-Container für Diagnoseaufgaben herstellen, indem Sie zu `https://<app-name>.scm.azurewebsites.net/DebugConsole` navigieren. Funktionsweise:

- Mit der Debugkonsole können Sie interaktive Befehle ausführen, z. B. das Starten von PowerShell-Sitzungen, das Untersuchen von Registrierungsschlüsseln und das Navigieren im gesamten Containerdateisystem.
- Sie funktioniert gesondert vom darüber angesiedelten grafischen Browser, in dem nur die Dateien in Ihrem [freigegebenen Speicher](#use-persistent-shared-storage) angezeigt werden.
- In einer horizontal skalierten App ist die Debugkonsole mit einer der Containerinstanzen verbunden. Sie können eine andere Instanz aus dem Dropdown **Instanz** im obersten Menü auswählen.
- Jede Änderung, die Sie an dem Container innerhalb der Konsole vornehmen, bleibt *nicht* erhalten, wenn Ihre App neu gestartet wird (mit Ausnahme von Änderungen im freigegebenen Speicher), weil sie nicht Teil des Docker-Images ist. Um Ihre Änderungen beizubehalten, wie z. B. Registrierungseinstellungen und Softwareinstallation, machen Sie sie zu einem Teil der Dockerfile-Datei.

## <a name="access-diagnostic-logs"></a>Zugreifen auf Diagnoseprotokolle

App Service protokolliert Aktionen des Docker-Hosts sowie aus dem Container stammende Aktivitäten. Protokolle vom Docker-Host (Plattformprotokolle) werden standardmäßig ausgeliefert, aber Anwendungsprotokolle oder Webserverprotokolle aus dem Container müssen manuell aktiviert werden. Weitere Informationen finden Sie unter [Aktivieren der Diagnoseprotokollierung](troubleshoot-diagnostic-logs.md#enable-application-logging-linuxcontainer) und [Aktivieren der Webserverprotokollierung](troubleshoot-diagnostic-logs.md#enable-web-server-logging). 

Es gibt verschiedene Möglichkeiten, um auf Docker-Protokolle zuzugreifen:

- [Im Azure-Portal](#in-azure-portal)
- [In der Kudu-Konsole](#from-the-kudu-console)
- [Mit der Kudu-API](#with-the-kudu-api)
- [Senden von Protokollen an Azure Monitor](troubleshoot-diagnostic-logs.md#send-logs-to-azure-monitor-preview)

### <a name="in-azure-portal"></a>Im Azure-Portal

Docker-Protokolle werden im Portal auf der Seite **Containereinstellungen** Ihrer App angezeigt. Die Protokolle werden abgeschnitten, aber Sie können alle Protokolle herunterladen, indem Sie auf **Herunterladen** klicken. 

### <a name="from-the-kudu-console"></a>In der Kudu-Konsole

Navigieren Sie zu `https://<app-name>.scm.azurewebsites.net/DebugConsole`, und klicken Sie auf den Ordner **LogFiles**, um die einzelnen Protokolldateien anzuzeigen. Um das gesamte Verzeichnis **LogFiles** herunterzuladen, klicken Sie auf das Symbol **Herunterladen** links neben dem Verzeichnisnamen. Sie können auch über einen FTP-Client auf diesen Ordner zugreifen.

Im Konsolenterminal können Sie nicht standardmäßig auf den Ordner `C:\home\LogFiles` zugreifen, da der persistente freigegebene Speicher nicht aktiviert ist. Um dieses Verhalten im Konsolenterminal zu aktivieren, [aktivieren Sie den persistenten freigegebenen Speicher](#use-persistent-shared-storage).

Wenn Sie versuchen, das Docker-Protokoll, das zurzeit verwendet wird, mithilfe eines FTP-Clients herunterzuladen, erhalten Sie möglicherweise einen Fehler aufgrund einer Dateisperre.

### <a name="with-the-kudu-api"></a>Mit der Kudu-API

Navigieren Sie direkt zu `https://<app-name>.scm.azurewebsites.net/api/logs/docker`, um die Metadaten für die Docker-Protokolle anzuzeigen. Möglicherweise wird mehr als eine Protokolldatei aufgelistet, und mithilfe der `href`-Eigenschaft können Sie die Protokolldatei direkt herunterladen. 

Um alle Protokolle zusammen in einer ZIP-Datei herunterzuladen, greifen Sie auf `https://<app-name>.scm.azurewebsites.net/api/logs/docker/zip` zu.

## <a name="customize-container-memory"></a>Anpassen des Containerarbeitsspeichers

Standardmäßig sind alle in Azure App Service bereitgestellten Windows-Container auf 1 GB RAM beschränkt. Sie können diesen Wert ändern, indem Sie die App-Einstellung `WEBSITE_MEMORY_LIMIT_MB` über die [Cloud Shell](https://shell.azure.com) bereitstellen. In Bash:

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --settings WEBSITE_MEMORY_LIMIT_MB=2000
```

PowerShell:

```azurepowershell-interactive
Set-AzWebApp -ResourceGroupName <group-name> -Name <app-name> -AppSettings @{"WEBSITE_MEMORY_LIMIT_MB"=2000}
```

Der Wert wird in MB definiert und muss kleiner oder gleich dem gesamten physischen Arbeitsspeicher des Hosts sein. Beispielsweise darf in einem App Service-Plan mit 8 GB RAM der kumulative Gesamtwert von `WEBSITE_MEMORY_LIMIT_MB` für alle Apps 8 GB nicht überschreiten. Informationen dazu, wie viel Arbeitsspeicher für jeden Tarif verfügbar ist, finden Sie in der [App Service-Preisübersicht](https://azure.microsoft.com/pricing/details/app-service/windows/) im Abschnitt **Premium V3-Serviceplan**.

## <a name="customize-the-number-of-compute-cores"></a>Anpassen der Anzahl der Compute-Kerne

Standardmäßig wird ein Windows-Container mit allen verfügbaren Kernen für Ihren ausgewählten Tarif ausgeführt. Sie können z. B. die Anzahl der Kerne verringern, die ihr Stagingslot verwendet. Um die Anzahl der von einem Container verwendeten Kerne zu verringern, legen Sie die App-Einstellung `WEBSITE_CPU_CORES_LIMIT` auf die bevorzugte Anzahl von Kernen fest. Sie können sie über die [Cloud Shell](https://shell.azure.com) festlegen. In Bash:

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --slot staging --settings WEBSITE_CPU_CORES_LIMIT=1
```

PowerShell:

```azurepowershell-interactive
Set-AzWebApp -ResourceGroupName <group-name> -Name <app-name> -AppSettings @{"WEBSITE_CPU_CORES_LIMIT"=1}
```

> [!NOTE]
> Durch Aktualisieren der App-Einstellung wird ein automatischer Neustart ausgelöst, was zu minimalen Ausfallzeiten führt. Bei einer Produktions-App sollten Sie erwägen, sie in einen Stagingslot zu wechseln, die App-Einstellung im Stagingslot zu ändern, und die App dann wieder zurück in die Produktion zu wechseln.

Überprüfen Sie Ihre angepasste Anzahl, indem Sie zur Kudu-Konsole wechseln (`https://<app-name>.scm.azurewebsites.net`) und die folgenden Befehle mithilfe von PowerShell eingeben. Jeder Befehl gibt eine Zahl aus.

```PowerShell
Get-ComputerInfo | ft CsNumberOfLogicalProcessors # Total number of enabled logical processors. Disabled processors are excluded.
Get-ComputerInfo | ft CsNumberOfProcessors # Number of physical processors.
```

Bei den Prozessoren kann es sich um Mehrkern- oder Hyperthreading-Prozessoren handeln. Informationen dazu, wie viele Kerne für jeden Tarif verfügbar ist, finden Sie in der [App Service-Preisübersicht](https://azure.microsoft.com/pricing/details/app-service/windows/) im Abschnitt **Premium V3-Serviceplan**.

## <a name="customize-health-ping-behavior"></a>Anpassen des Integritäts-Ping-Verhaltens

In App Service wird ein Container als erfolgreich gestartet betrachtet, wenn der Container gestartet wird und auf einen HTTP-Ping antwortet. Die Integritäts-Ping-Anforderung enthält den Header `User-Agent= "App Service Hyper-V Container Availability Check"`. Wenn der Container gestartet wird, aber nach einem bestimmten Zeitraum nicht auf einen Ping antwortet, protokolliert App Service ein Ereignis im Docker-Protokoll, das besagt, dass der Container nicht gestartet wurde. 

Wenn Ihre Anwendung ressourcenintensiv ist, antwortet der Container möglicherweise nicht rechtzeitig auf den HTTP-Ping. Um die Aktionen zu kontrollieren, wenn HTTP-Pings fehlschlagen, legen Sie die App-Einstellung `CONTAINER_AVAILABILITY_CHECK_MODE` fest. Sie können sie über die [Cloud Shell](https://shell.azure.com) festlegen. In Bash:

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --settings CONTAINER_AVAILABILITY_CHECK_MODE="ReportOnly"
```

PowerShell:

```azurepowershell-interactive
Set-AzWebApp -ResourceGroupName <group-name> -Name <app-name> -AppSettings @{"CONTAINER_AVAILABILITY_CHECK_MODE"="ReportOnly"}
```

In der folgenden Tabelle sind die möglichen Werte aufgeführt:

| Wert | Beschreibungen |
| - | - |
| **Repair** | Neustarten des Containers nach drei aufeinanderfolgenden Verfügbarkeitsprüfungen |
| **ReportOnly** | Der Standardwert. Den Container nicht neu starte, aber den Container nach drei aufeinanderfolgenden Verfügbarkeitsprüfungen in den Docker-Protokollen protokollieren. |
| **Deaktiviert** | Keine Verfügbarkeitsprüfung durchführen. |

## <a name="support-for-group-managed-service-accounts"></a>Unterstützung für gruppenverwaltete Dienstkonten

Gruppenverwaltete Dienstkonten (Group Managed Service Accounts, gMSAs) werden zurzeit in Windows-Containern in App Service nicht unterstützt.

::: zone-end

::: zone pivot="container-linux"

## <a name="enable-ssh"></a>Aktivieren von SSH

SSH ermöglicht die sichere Kommunikation zwischen einem Container und einem Client. Damit ein benutzerdefinierter Container SSH unterstützt, müssen Sie ihn dem Docker-Image selbst hinzufügen.

> [!TIP]
> In allen integrierten Linux-Containern in App Service wurden die SSH-Anweisungen in ihren Imagerepositorys hinzugefügt. Sie können die folgenden Anweisungen mit dem [Node.js 10.14-Repository](https://github.com/Azure-App-Service/node/blob/master/10.14) durchgehen, um zu prüfen, wie es dort aktiviert ist. Die Konfiguration im integrierten Node.js-Image unterscheidet sich geringfügig, ist jedoch im Wesentlichen identisch.

- Fügen Sie dem Repository [die Datei „sshd_config“](https://man.openbsd.org/sshd_config) hinzu, wie im folgenden Beispiel gezeigt.

    ```
    Port            2222
    ListenAddress       0.0.0.0
    LoginGraceTime      180
    X11Forwarding       yes
    Ciphers aes128-cbc,3des-cbc,aes256-cbc,aes128-ctr,aes192-ctr,aes256-ctr
    MACs hmac-sha1,hmac-sha1-96
    StrictModes         yes
    SyslogFacility      DAEMON
    PasswordAuthentication  yes
    PermitEmptyPasswords    no
    PermitRootLogin     yes
    Subsystem sftp internal-sftp
    ```

    > [!NOTE]
    > Diese Datei konfiguriert OpenSSH und muss die folgenden Elemente enthalten:
    > - `Port` muss auf 2222 festgelegt werden.
    > - `Ciphers` muss mindestens ein Element aus dieser Liste enthalten: `aes128-cbc,3des-cbc,aes256-cbc`.
    > - `MACs` muss mindestens ein Element aus dieser Liste enthalten: `hmac-sha1,hmac-sha1-96`.

- Fügen Sie eine ssh_setup-Skriptdatei hinzu, um die SSH-Schlüssel über [ssh-keygen](https://man.openbsd.org/ssh-keygen.1) in Ihrem Repository zu erstellen.

    ```
    #!/bin/sh

    if [ ! -f "/etc/ssh/ssh_host_rsa_key" ]; then
        # generate fresh rsa key
        ssh-keygen -f /etc/ssh/ssh_host_rsa_key -N '' -t rsa
    fi

    if [ ! -f "/etc/ssh/ssh_host_dsa_key" ]; then
        # generate fresh dsa key
        ssh-keygen -f /etc/ssh/ssh_host_dsa_key -N '' -t dsa
    fi

    if [ ! -f "/etc/ssh/ssh_host_ecdsa_key" ]; then
        # generate fresh ecdsa key
        ssh-keygen -f /etc/ssh/ssh_host_ecdsa_key -N '' -t dsa
    fi

    if [ ! -f "/etc/ssh/ssh_host_ed25519_key" ]; then
        # generate fresh ecdsa key
        ssh-keygen -f /etc/ssh/ssh_host_ed25519_key -N '' -t dsa
    fi

    #prepare run dir
        if [ ! -d "/var/run/sshd" ]; then
        mkdir -p /var/run/sshd
    fi
    ```

- Fügen Sie im Dockerfile die folgenden Befehle hinzu:

    ```Dockerfile
    # Install OpenSSH and set the password for root to "Docker!". In this example, "apk add" is the install instruction for an Alpine Linux-based image.
    RUN apk add openssh \
         && echo "root:Docker!" | chpasswd 

    # Copy the sshd_config file to the /etc/ssh/ directory
    COPY sshd_config /etc/ssh/

    # Copy and configure the ssh_setup file
    RUN mkdir -p /tmp
    COPY ssh_setup.sh /tmp
    RUN chmod +x /tmp/ssh_setup.sh \
        && (sleep 1;/tmp/ssh_setup.sh 2>&1 > /dev/null)

    # Open port 2222 for SSH access
    EXPOSE 80 2222
    ```

    > [!NOTE] 
    > Das Root-Passwort muss exakt `Docker!` lauten, da es vom App Service verwendet wird, um Ihnen den Zugriff auf die SSH-Sitzung mit dem Container zu ermöglichen. Diese Konfiguration erlaubt keine externen Verbindungen zum Container. Port 2222 des Containers ist nur innerhalb des Brückennetzes eines privaten virtuellen Netzes zugänglich und für einen Angreifer aus dem Internet nicht erreichbar.

- Starten Sie im Startskript für Ihren Container den SSH-Server.

    ```bash
    /usr/sbin/sshd
    ```

## <a name="access-diagnostic-logs"></a>Zugreifen auf Diagnoseprotokolle

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-linux-no-h.md)]

## <a name="configure-multi-container-apps"></a>Konfigurieren von Apps mit mehreren Containern

- [Verwenden von beständigem Speicher in Docker Compose](#use-persistent-storage-in-docker-compose)
- [Einschränkungen der Vorschau](#preview-limitations)
- [Optionen von Docker Compose](#docker-compose-options)

### <a name="use-persistent-storage-in-docker-compose"></a>Verwenden von beständigem Speicher in Docker Compose

Apps mit mehreren Containern wie WordPress benötigen einen beständigen Speicher, um ordnungsgemäß zu funktionieren. Um dies zu ermöglichen, muss Ihre Docker Compose-Konfiguration auf einen Speicherort *außerhalb* Ihres Containers verweisen. Speicherorte in Ihrem Container bleiben nach dem Neustart der App nicht mehr unverändert.

Aktivieren Sie den beständigen Speicher, indem Sie die App-Einstellung `WEBSITES_ENABLE_APP_SERVICE_STORAGE` mit dem Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) in der [Cloud Shell](https://shell.azure.com) festlegen.

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

Ordnen Sie in Ihrer Datei *docker-compose.yml* die `volumes`-Option zu `${WEBAPP_STORAGE_HOME}` zu. 

`WEBAPP_STORAGE_HOME` ist eine Umgebungsvariable in App Service, die dem beständigen Speicher für Ihre App zugeordnet wird. Beispiel:

```yaml
wordpress:
  image: <image name:tag>
  volumes:
  - ${WEBAPP_STORAGE_HOME}/site/wwwroot:/var/www/html
  - ${WEBAPP_STORAGE_HOME}/phpmyadmin:/var/www/phpmyadmin
  - ${WEBAPP_STORAGE_HOME}/LogFiles:/var/log
```

### <a name="preview-limitations"></a>Einschränkungen der Vorschau

Apps mit mehreren Containern befinden sich derzeit in der Vorschauphase. Die folgenden Features der App Service-Plattform werden nicht unterstützt:

- Authentifizierung/Autorisierung
- Verwaltete Identitäten
- CORS
- VNET-Integration wird für Docker Compose-Szenarien nicht unterstützt.
- Docker Compose unter Azure App Service verfügt derzeit über ein Limit von 4.000 Zeichen.

### <a name="docker-compose-options"></a>Optionen von Docker Compose

In den folgenden Listen werden unterstützte und nicht unterstützte Docker Compose-Konfigurationsoptionen angezeigt:

#### <a name="supported-options"></a>Unterstützte Optionen

- command
- entrypoint
- Environment
- image
- ports
- restart
- services
- volumes

#### <a name="unsupported-options"></a>Nicht unterstützte Optionen

- build (unzulässig)
- [depends_on](faq-app-service-linux.yml#how-do-i-use-depends-on-) (ignoriert)
- networks (ignoriert)
- secrets (ignoriert)
- andere Ports als 80 und 8080 (ignoriert)

> [!NOTE]
> Alle weiteren Optionen, die nicht ausdrücklich aufgeführt sind, werden in der Public Preview ignoriert.

[!INCLUDE [robots933456](../../includes/app-service-web-configure-robots933456.md)]

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Migrieren benutzerdefinierter Software zu Azure App Service mithilfe eines benutzerdefinierten Containers](tutorial-custom-container.md)

::: zone pivot="container-linux"

> [!div class="nextstepaction"]
> [Tutorial: WordPress-App mit mehreren Containern](tutorial-multi-container-app.md)

::: zone-end

Oder siehe in zusätzlichen Ressourcen:

- [Referenz zu Umgebungsvariablen und App-Einstellungen](reference-app-settings.md)
- [Laden eines Zertifikats in Windows-/Linux-Containern](configure-ssl-certificate-in-code.md#load-certificate-in-linuxwindows-containers)
