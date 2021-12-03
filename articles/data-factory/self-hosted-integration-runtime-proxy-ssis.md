---
title: Konfigurieren einer selbstgehosteten Integration Runtime als Proxy für SSIS
description: Hier erfahren Sie, wie Sie eine selbstgehostete Integration Runtime als Proxy für eine Azure-SSIS Integration Runtime konfigurieren.
ms.service: data-factory
ms.subservice: integration-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
ms.custom: seo-lt-2019, devx-track-azurepowershell
ms.date: 10/22/2021
ms.openlocfilehash: 776329b23ab4e0fdd11e48da6ec1a7cc3d21a7f7
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2021
ms.locfileid: "131852311"
---
# <a name="configure-a-self-hosted-ir-as-a-proxy-for-an-azure-ssis-ir-in-azure-data-factory"></a>Konfigurieren einer selbstgehosteten IR als Proxy für eine Azure-SSIS IR in Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

In diesem Artikel wird beschrieben, wie Sie SSIS-Pakete (SQL Server Integration Services) in einer Azure-SSIS Integration Runtime (Azure-SSIS IR) in Azure Data Factory (ADF) mit einer selbstgehosteten Integration Runtime (selbstgehostete IR) ausführen, die als Proxy konfiguriert wurde. 

Mit diesem Feature können Sie lokal auf Daten zugreifen und Tasks ausführen, ohne [Ihre Azure-SSIS IR mit einem virtuellen Netzwerk verknüpfen](./join-azure-ssis-integration-runtime-virtual-network.md) zu müssen. Dieses Feature ist nützlich, wenn Ihr Unternehmensnetzwerk eine zu komplexe Konfiguration aufweist oder wenn eine Richtlinie zu restriktiv für Sie ist, um Ihre Azure-SSIS IR darin einfügen zu können.

Dieses Feature kann vorerst nur für den SSIS-Datenflusstask und die „SQL ausführen/verarbeiten“-Tasks aktiviert werden. 

Bei Aktivierung für den Datenflussfask wird der Task mit diesem Feature nach Möglichkeit in zwei Stagingtasks unterteilt: 
* **Lokaler Stagingtask**: Dieser Task führt Ihre Datenflusskomponente aus, die in Ihrer selbstgehosteten IR eine Verbindung mit einem lokalen Datenspeicher herstellt. Dabei werden Daten aus dem lokalen Datenspeicher in einen Stagingbereich in Azure Blob Storage und umgekehrt verschoben.
* **Cloudstagingtask**: Dieser Task führt Ihre Datenflusskomponente aus, die in Ihrer Azure-SSIS IR keine Verbindung mit einem lokalen Datenspeicher herstellt. Dabei werden Daten aus dem Stagingbereich in Azure Blob Storage in einen Clouddatenspeicher oder umgekehrt verschoben.

Wenn Ihr Datenflusstask Daten aus der lokalen in die Cloudumgebung verschiebt, sind der erste und zweite Stagingtask lokale bzw. Cloudstagingtasks. Wenn Ihr Datenflusstask Daten aus der Cloud in die lokale Umgebung verschiebt, sind der erste und zweite Stagingtask Cloud- bzw. lokale Stagingtasks. Wenn Ihr Datenflusstask Daten aus der lokalen Umgebung in die lokale Umgebung verschiebt, sind der erste und zweite Stagingtask beides lokale Stagingtasks. Wenn der Datenflusstask Daten aus der Cloud in die Cloud verschiebt, ist dieses Feature nicht anwendbar.

Bei Aktivierung für die „SQL ausführen/verarbeiten“-Tasks werden die Tasks mit diesem Feature in Ihrer selbstgehosteten IR ausgeführt. 

Weitere Vorteile und Fähigkeiten dieses Features ermöglichen es Ihnen, Ihre selbstgehostete IR in Regionen einzurichten, die noch nicht von einer Azure-SSIS IR unterstützt werden, und die öffentliche statische IP-Adresse Ihrer selbstgehosteten IR auf der Firewall Ihrer Datenquellen zuzulassen.

## <a name="prepare-the-self-hosted-ir"></a>Vorbereiten der selbstgehosteten IR

Wenn Sie dieses Feature verwenden möchten, erstellen Sie zuerst eine Data Factory und richten darin eine Azure-SSIS IR ein. Falls dies noch nicht geschehen ist, führen Sie die Anleitungen unter [Einrichten einer Azure-SSIS IR](./tutorial-deploy-ssis-packages-azure.md) aus.

Anschließend richten Sie Ihre selbstgehostete IR in derselben Data Factory ein, in der Ihre Azure-SSIS IR eingerichtet wurde. Informationen dazu finden Sie unter [Erstellen einer selbstgehosteten IR](./create-self-hosted-integration-runtime.md).

Zum Schluss laden Sie die neueste Version der selbstgehosteten IR sowie die zusätzlichen Treiber und die Runtime auf Ihren lokalen Computer oder Ihre Azure-VM wie folgt herunter und installieren sie dort:
- Laden Sie die neueste Version der [selbstgehosteten IR](https://www.microsoft.com/download/details.aspx?id=39717) herunter, und installieren Sie sie.
- Wenn Sie in Ihren Paketen Connectors für Object Linking and Embedding Database (OLEDB), Open Database Connectivity (ODBC) oder ADO.NET verwenden, laden Sie die relevanten Treiber herunter, und installieren Sie sie auf dem Computer, auf dem Ihre selbstgehostete IR installiert wurde (sofern nicht bereits geschehen).  

  Wenn Sie die frühere Version des OLE DB-Treibers für SQL Server (SQL Server Native Client [SQLNCLI]) verwenden, [laden Sie die 64-Bit-Version herunter](https://www.microsoft.com/download/details.aspx?id=50402).  

  Wenn Sie die neueste Version des OLE DB-Treibers für SQL Server (MSOLEDBSQL) verwenden, [laden Sie die 64-Bit-Version herunter](https://www.microsoft.com/download/details.aspx?id=56730).  
  
  Wenn Sie OLEDB-, ODBC- oder ADO.NET-Treiber für andere Datenbanksysteme wie PostgreSQL, MySQL, Oracle usw. verwenden, können Sie die 64-Bit-Version von der jeweiligen Website herunterladen.
- Wenn Sie Datenflusskomponenten aus dem Azure Feature Pack in Ihren Paketen verwenden, [laden Sie Azure Feature Pack for SQL Server 2017 herunter und installieren Sie es](https://www.microsoft.com/download/details.aspx?id=54798) auf demselben Computer, auf dem Ihre selbstgehostete IR installiert ist, sofern sie dies noch nicht getan haben.
- Falls nicht bereits geschehen, [laden Sie die 64-Bit-Version von Visual C++ (VC) Runtime herunter, und installieren Sie sie](https://www.microsoft.com/en-us/download/details.aspx?id=40784) auf dem Computer, auf dem Ihre selbstgehostete IR installiert wurde.

### <a name="enable-windows-authentication-for-on-premises-tasks"></a>Aktivieren der Windows-Authentifizierung für lokale Tasks

Wenn für lokale Stagingtasks und „SQL ausführen/verarbeiten“-Tasks in Ihrer selbstgehosteten IR die Windows-Authentifizierung erforderlich ist, müssen Sie auch [die Windows-Authentifizierungsfunktion in Ihrer Azure-SSIS-IR konfigurieren](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth). 

Ihre lokalen Stagingtasks und „SQL ausführen/verarbeiten“-Tasks werden mit dem Dienstkonto der selbstgehosteten IR (standardmäßig *NT SERVICE\DIAHostService*) aufgerufen, und der Zugriff auf Ihre Datenspeicher erfolgt über das Windows-Authentifizierungskonto. Beiden Konten müssen bestimmte Sicherheitsrichtlinien zugewiesen werden. Wechseln Sie auf dem Computer mit der selbstgehosteten IR zu **Lokale Sicherheitsrichtlinie** > **Lokale Richtlinien** > **Zuweisen von Benutzerrechten**, und führen Sie dann die folgenden Schritte aus:

1. Weisen Sie die Richtlinien *Arbeitsspeicherkontingente für einen Prozess anpassen* und *Token auf Prozessebene ersetzen* dem Dienstkonto der selbstgehosteten IR zu. Dies sollte automatisch erfolgen, wenn Sie Ihre selbstgehostete IR mit dem Standarddienstkonto installieren. Wenn dies nicht der Fall ist, weisen Sie diese Richtlinien manuell zu. Wenn Sie ein anderes Dienstkonto verwenden, müssen Sie ihm diese Richtlinien zuweisen.

1. Weisen Sie dem Windows-Authentifizierungskonto die Richtlinie *Als Dienst anmelden* zu.

## <a name="prepare-the-azure-blob-storage-linked-service-for-staging"></a>Vorbereiten des mit Azure Blob Storage verknüpften Diensts für das Staging

Erstellen Sie einen mit Azure Blob Storage verknüpften Dienst in der gleichen Data Factory, in der auch Ihre Azure-SSIS IR eingerichtet wurde (sofern noch nicht geschehen). Informationen dazu finden Sie unter [Erstellen eines mit Azure Data Factory verknüpften Diensts](./quickstart-create-data-factory-portal.md#create-a-linked-service). Führen Sie unbedingt die folgenden Schritte aus:
- Wählen Sie **Azure Blob Storage** als **Datenspeicher** aus.  
- Wählen Sie für **Über Integration Runtime verbinden** die Option **AutoResolveIntegrationRuntime** (nicht Ihre selbstgehostete IR) aus, damit diese ignoriert und stattdessen Ihre Azure-SSIS IR zum Abrufen von Zugriffsanmeldeinformationen für Ihre Azure Blob Storage-Instanz verwendet werden kann.
- Wählen Sie für **Authentifizierungsmethode** eine der Optionen **Kontoschlüssel**, **SAS-URI**, **Dienstprinzipal**, **Verwaltete Identität** oder **Benutzerseitig zugewiesene verwaltete Identität** aus.  

>[!TIP]
>Wenn Sie die Methode **Dienstprinzipal** auswählen, gewähren Sie dem Dienstprinzipal mindestens die Rolle *Mitwirkender an Storage-Blobdaten*. Weitere Informationen finden Sie unter [Eigenschaften des verknüpften Diensts](connector-azure-blob-storage.md#linked-service-properties). Wenn Sie die Methode **Verwaltete Identität**/**Benutzerseitig zugewiesene verwaltete Identität** auswählen, gewähren Sie der angegebenen systemseitig/benutzerseitig zugewiesenen verwalteten Identität für Ihre ADF eine geeignete Rolle für den Zugriff auf Azure Blob Storage. Weitere Informationen finden Sie unter [Access Azure Blob Storage using Azure Active Directory (Azure AD) authentication with the specified system/user-assigned managed identity for your ADF](/sql/integration-services/connection-manager/azure-storage-connection-manager#managed-identities-for-azure-resources-authentication) (Zugreifen auf Azure Blob Storage mithilfe von Azure Active Directory (Azure AD)-Authentifizierung mit der angegebenen systemseitig/benutzerseitig zugewiesenen verwalteten Identität für Ihre ADF).

:::image type="content" source="media/self-hosted-integration-runtime-proxy-ssis/shir-azure-blob-storage-linked-service.png" alt-text="Vorbereiten des mit Azure Blob Storage verknüpften Diensts für das Staging":::

## <a name="configure-an-azure-ssis-ir-with-your-self-hosted-ir-as-a-proxy"></a>Konfigurieren einer Azure-SSIS IR mit Ihrer selbstgehosteten IR als Proxy

Nachdem Sie Ihre selbstgehostete IR und den mit Azure Blob Storage verknüpften Dienst für das Staging vorbereitet haben, können Sie jetzt Ihre neue oder vorhandene Azure-SSIS IR mit der selbstgehosteten IR als Proxy in Ihrem Data Factory-Portal bzw. in Ihrer App konfigurieren. Bevor Sie das tun, können Sie jedoch –falls Ihre vorhandene Azure-SSIS IR bereits ausgeführt wird – diese beenden, bearbeiten und dann neu starten.

1. Überspringen Sie im Bereich **Integration Runtime-Setup** die Seiten **Allgemeine Einstellungen** und **Bereitstellungseinstellungen**, indem Sie die Schaltfläche **Weiter** auswählen. 

1. Gehen Sie auf der Seite **Erweiterte Einstellungen** folgendermaßen vor:

   1. Aktivieren Sie das Kontrollkästchen **Selbstgehostete Integration Runtime als Proxy für Ihre Azure-SSIS Integration Runtime**. 

   1. Wählen Sie in der Dropdownliste **Selbstgehostete Integration Runtime** Ihre vorhandene selbstgehostete IR als Proxy für die Azure-SSIS IR aus.

   1. Wählen Sie in der Dropdownliste **Verknüpfter Stagingspeicherdienst** Ihren vorhandenen mit Azure Blob Storage verknüpften Dienst aus, oder erstellen Sie einen neuen Dienst für den Stagingprozess.

   1. Geben Sie im Feld **Stagingpfad** einen Blobcontainer in Ihrem ausgewählten Azure Storage-Konto an, oder lassen Sie das Feld leer, wenn Sie einen Standardpfad für den Stagingprozess verwenden möchten.

   1. Wählen Sie die Schaltfläche **Weiter**.

   :::image type="content" source="./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png" alt-text="Erweiterte Einstellungen bei einer selbstgehosteten IR":::

Sie können auch Ihre neue oder vorhandene Azure-SSIS IR mit der selbstgehosteten IR als Proxy mithilfe von PowerShell konfigurieren.

```powershell
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$AzureSSISName = "[your Azure-SSIS IR name]"
# Self-hosted integration runtime info - This can be configured as a proxy for on-premises data access 
$DataProxyIntegrationRuntimeName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingLinkedServiceName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingPath = "" # OPTIONAL to configure a proxy for on-premises data access 

# Add self-hosted integration runtime parameters if you configure a proxy for on-premises data access
if(![string]::IsNullOrEmpty($DataProxyIntegrationRuntimeName) -and ![string]::IsNullOrEmpty($DataProxyStagingLinkedServiceName))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -DataProxyIntegrationRuntimeName $DataProxyIntegrationRuntimeName `
        -DataProxyStagingLinkedServiceName $DataProxyStagingLinkedServiceName

    if(![string]::IsNullOrEmpty($DataProxyStagingPath))
    {
        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
            -DataFactoryName $DataFactoryName `
            -Name $AzureSSISName `
            -DataProxyStagingPath $DataProxyStagingPath
    }
}
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $DataFactoryName `
    -Name $AzureSSISName `
    -Force
```

## <a name="enable-ssis-packages-to-use-a-proxy"></a>Aktivieren von SSIS-Paketen für die Verwendung eines Proxys

Wenn Sie die neuesten SSDT entweder als Erweiterung „SSIS-Projekte“ für Visual Studio oder ein eigenständiges Installationsprogramm verwenden, gibt es eine neue `ConnectByProxy`-Eigenschaft in den Verbindungs-Managern für unterstützte Datenflusskomponenten und die `ExecuteOnProxy`-Eigenschaft in den „SQL ausführen/verarbeiten“-Tasks.
* [Herunterladen der Erweiterung „SSIS-Projekte“ für Visual Studio](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects)
* [Herunterladen des eigenständigen Installationsprogramms](/sql/ssdt/download-sql-server-data-tools-ssdt#ssdt-for-vs-2017-standalone-installer)   

Wenn Sie neue Pakete entwerfen, die Datenflusstasks mit Komponenten enthalten, die lokal auf Daten zugreifen, können Sie die `ConnectByProxy`-Eigenschaft aktivieren, indem Sie sie im Bereich **Eigenschaften** der entsprechenden Verbindungs-Manager auf *True* festlegen.

Wenn Sie neue Pakete entwerfen, die lokal ausgeführte „SQL ausführen/verarbeiten“-Tasks enthalten, können Sie die `ExecuteOnProxy`-Eigenschaft aktivieren, indem Sie sie im Bereich **Eigenschaften** der entsprechenden Tasks selbst auf *True* festlegen.

:::image type="content" source="media/self-hosted-integration-runtime-proxy-ssis/shir-proxy-properties.png" alt-text="Aktivieren der ConnectByProxy/ExecuteOnProxy-Eigenschaft":::

Sie können die Eigenschaften `ConnectByProxy`/`ExecuteOnProxy` auch aktivieren, wenn Sie vorhandene Pakete ausführen, ohne sie manuell einzeln ändern zu müssen. Es gibt zwei Optionen:
- **Option A**: Öffnen, Neuerstellen und erneutes Bereitstellen des Projekts, das diese Pakete enthält, mit den neuesten SSDT, die in Ihrer Azure-SSIS IR ausgeführt werden sollen. Anschließend können Sie die `ConnectByProxy`-Eigenschaft aktivieren, indem Sie sie für die entsprechenden Verbindungs-Manager auf *True* festlegen, die auf der Registerkarte **Verbindungs-Manager** des Popupfensters **Paket ausführen** angezeigt werden, wenn Sie Pakete aus SSMS ausführen.

  :::image type="content" source="media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssms.png" alt-text="Aktivieren der ConnectByProxy/ExecuteOnProxy-Eigenschaft2":::

  Sie können die `ConnectByProxy`-Eigenschaft auch aktivieren, indem Sie sie für die entsprechenden Verbindungs-Manager auf *True* festlegen, die auf der Registerkarte **Verbindungs-Manager** der Aktivität [SSIS-Paket ausführen](./how-to-invoke-ssis-package-ssis-activity.md) angezeigt werden, wenn Sie Pakete in Data Factory-Pipelines ausführen.
  
  :::image type="content" source="media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssis-activity.png" alt-text="Aktivieren der ConnectByProxy/ExecuteOnProxy-Eigenschaft3":::

- **Option B:** Erneutes Bereitstellen des Projekts, das diese Pakete enthält, für die Ausführung in Ihrer SSIS IR. Sie können dann die Eigenschaften `ConnectByProxy`/`ExecuteOnProxy` aktivieren, indem Sie deren Eigenschaftspfade (`\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`/`\Package\YourExecuteSQLTaskName.Properties[ExecuteOnProxy]`/`\Package\YourExecuteProcessTaskName.Properties[ExecuteOnProxy]`) angeben und sie als *True* für Eigenschaftsüberschreibungen auf der Registerkarte **Erweitert** des Popupfensters **Paket ausführen** festlegen, wenn Sie Pakete aus SSMS ausführen.

  :::image type="content" source="media/self-hosted-integration-runtime-proxy-ssis/shir-advanced-tab-ssms.png" alt-text="Aktivieren der ConnectByProxy/ExecuteOnProxy-Eigenschaft4":::

  Sie können die Eigenschaften `ConnectByProxy`/`ExecuteOnProxy` auch aktivieren, indem Sie deren Eigenschaftspfade (`\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`/`\Package\YourExecuteSQLTaskName.Properties[ExecuteOnProxy]`/`\Package\YourExecuteProcessTaskName.Properties[ExecuteOnProxy]`) angeben und sie als *True* für Eigenschaftsüberschreibungen auf der Registerkarte **Eigenschaftenüberschreibungen** der [Aktivität „SSIS-Paket ausführen“](./how-to-invoke-ssis-package-ssis-activity.md) festlegen, wenn Sie Pakete in Data Factory-Pipelines ausführen.
  
  :::image type="content" source="media/self-hosted-integration-runtime-proxy-ssis/shir-property-overrides-tab-ssis-activity.png" alt-text="Aktivieren der ConnectByProxy/ExecuteOnProxy-Eigenschaft5":::

## <a name="debug-the-on-premises-tasks-and-cloud-staging-tasks"></a>Debuggen der lokalen Tasks und Cloudstagingtasks

In Ihrer selbstgehosteten IR finden Sie die Laufzeitprotokolle im Ordner *C:\ProgramData\SSISTelemetry* und die Ausführungsprotokolle der lokalen Stagingtasks und „SQL ausführen/verarbeiten“-Tasks im Ordner *C:\ProgramData\SSISTelemetry\ExecutionLog*. Die Ausführungsprotokolle von Cloudstagingtasks finden Sie in Ihrer SSISDB, an den angegebenen Protokollierungsdateipfaden oder in Azure Monitor. Dies ist unter anderem abhängig davon, ob Sie Ihre Pakete in SSISDB speichern oder die [Azure Monitor-Integration](./monitor-ssis.md) aktivieren. Die eindeutigen IDs der lokalen Stagingtasks finden Sie auch in den Ausführungsprotokollen der Cloudstagingtasks. 

:::image type="content" source="media/self-hosted-integration-runtime-proxy-ssis/shir-first-staging-task-guid.png" alt-text="Eindeutige ID des ersten Stagingtasks":::

Wenn Sie Kundensupporttickets erstellt haben, können Sie auf der Registerkarte **Diagnose** des **Konfigurations-Managers für Microsoft Integration Runtime**, der in Ihrer selbstgehosteten Integration Runtime installiert ist, die Schaltfläche **Protokolle senden** auswählen, um uns aktuelle Vorgangs-/Ausführungsprotokolle zur Untersuchung zukommen zu lassen.

## <a name="billing-for-the-on-premises-tasks-and-cloud-staging-tasks"></a>Abrechnung für lokale Tasks und Cloudstagingtasks

Die in Ihrer selbstgehosteten IR ausgeführten lokalen Stagingtasks und „SQL ausführen/verarbeiten“-Tasks werden separat abgerechnet, so wie alle Datenverschiebungsaktivitäten in Rechnung gestellt werden, die in einer selbstgehosteten IR erfolgen. Dies wird im Artikel [Azure Data Factory data pipeline pricing](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) (Preise für Azure Data Factory-Datenpipeline) beschrieben.

Die in Ihrer selbstgehosteten IR ausgeführten Cloudstagingtasks werden nicht separat abgerechnet, aber Ihre Ausführung von Azure-SSIS IR wird, wie im Artikel [Preise für Azure-SSIS IR](https://azure.microsoft.com/pricing/details/data-factory/ssis/) beschrieben, in Rechnung gestellt.

## <a name="enable-custom3rd-party-data-flow-components"></a>Aktivieren von benutzerdefinierten Datenflusskomponenten/Drittanbieterkomponenten 

Gehen Sie wie folgt vor, um benutzerdefinierten Datenflusskomponenten/Drittanbieterkomponenten den lokalen Zugriff auf Daten zu ermöglichen und dabei die selbstgehostete Integration Runtime als Proxy für Azure-SSIS IR zu verwenden:

1. Installieren Sie Ihre auf SQL Server 2017 ausgerichteten benutzerdefinierten Datenflusskomponenten/Drittanbieterkomponenten in Azure-SSIS IR per [benutzerdefiniertem Standardsetup/Express-Setup](./how-to-configure-azure-ssis-ir-custom-setup.md).

1. Erstellen Sie die folgenden DTSPath-Registrierungsschlüssel für eine selbstgehostete Integration Runtime, falls sie noch nicht vorhanden sind:
   1. `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\140\SSIS\Setup\DTSPath` auf `C:\Program Files\Microsoft SQL Server\140\DTS\` festgelegt.
   1. `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SQL Server\140\SSIS\Setup\DTSPath` auf `C:\Program Files (x86)\Microsoft SQL Server\140\DTS\` festgelegt.
   
1. Installieren Sie Ihre auf SQL Server 2017 ausgerichteten benutzerdefinierten Datenflusskomponenten/Drittanbieterkomponenten in der selbstgehosteten IR unter dem obigen DTS-Pfad, und stellen Sie sicher, dass Ihr Installationsprozess Folgendes umfasst:

   1. Erstellen der Ordner `<DTSPath>`, `<DTSPath>/Connections`, `<DTSPath>/PipelineComponents` und `<DTSPath>/UpgradeMappings` (sofern noch nicht vorhanden)
   
   1. Erstellen Ihrer eigenen XML-Datei für Erweiterungszuordnungen im Ordner `<DTSPath>/UpgradeMappings`
   
   1. Installieren aller Assemblys, auf die von den Assemblys Ihrer benutzerdefinierten Datenflusskomponenten/Drittanbieterkomponenten im globalen Assemblycache (GAC) verwiesen wird

Im Folgenden finden Sie Beispiele von unseren Partnern [Theobald Software](https://kb.theobald-software.com/xtract-is/XIS-for-Azure-SHIR) und [Aecorsoft](https://www.aecorsoft.com/blog/2020/11/8/using-azure-data-factory-to-bring-sap-data-to-azure-via-self-hosted-ir-and-ssis-ir), die ihre Datenflusskomponenten zur Verwendung unseres benutzerdefinierten Express-Setups und der selbstgehosteten IR als Proxy für Azure-SSIS IR angepasst haben.

## <a name="enforce-tls-12"></a>Erzwingen von TLS 1.2

Wenn Sie auf Datenspeicher zugreifen müssen, die nur für die Verwendung der stärksten Kryptografie bzw. des sichersten Netzwerkprotokolls (TLS 1.2) konfiguriert wurden, z. B. Ihre Azure Blob Storage für Staging, müssen Sie in Ihrer selbstgehosteten IR ausschließlich TLS 1.2 aktivieren und gleichzeitig ältere SSL/TLS-Versionen deaktivieren. Dazu können Sie das Skript *main.cmd* herunterladen und ausführen, das wir im Ordner *CustomSetupScript/UserScenarios/TLS 1.2* unseres Blobcontainers in der öffentlichen Vorschau bereitstellen. Wenn Sie [Azure Storage-Explorer](https://storageexplorer.com/) verwenden, können Sie eine Verbindung mit unserem Blobcontainer in der öffentlichen Vorschau herstellen, indem Sie den folgenden SAS-URI eingeben:

`https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2020-03-25T04:00:00Z&se=2025-03-25T04:00:00Z&sv=2019-02-02&sr=c&sig=WAD3DATezJjhBCO3ezrQ7TUZ8syEUxZZtGIhhP6Pt4I%3D`

## <a name="current-limitations"></a>Aktuelle Einschränkungen

- Aktuell werden nur Datenflusskomponenten unterstützt, die in Azure-SSIS IR Standard Edition integriert/vorinstalliert sind (mit Ausnahme von Hadoop/HDFS/DQS-Komponenten). Weitere Informationen finden Sie unter [Integrierte und vorinstallierte Komponenten für Azure-SSIS Integration Runtime](./built-in-preinstalled-components-ssis-integration-runtime.md).
- Aktuell werden nur benutzerdefinierte/von Drittanbietern erstellte Datenflusskomponenten unterstützt, die in verwaltetem Code (.NET Framework) geschrieben sind. In nativem Code (C++) geschriebene Komponenten werden aktuell nicht unterstützt.
- Das Ändern von Variablenwerten in lokalen und cloudbasierten Stagingtasks wird aktuell nicht unterstützt.
- In lokalen Stagingtasks geänderte Variablenwerte vom Typ „Objekt“ werden in anderen Tasks nicht berücksichtigt.
- *ParameterMapping* in der Quelle „OLEDB“ wird derzeit nicht unterstützt. Verwenden Sie zur Problemumgehung die Option *SQL-Befehl aus Variable* als *AccessMode*, und verwenden Sie die Option *Ausdruck*, um die Variablen/Parameter in einen SQL-Befehl einzufügen. Zur Veranschaulichung sehen Sie sich das Paket *ParameterMappingSample.dtsx* im Ordner *SelfHostedIRProxy/Limitations* unseres Blobcontainers in der öffentlichen Vorschau an. Wenn Sie Azure Storage-Explorer verwenden, können Sie eine Verbindung mit unserem Blobcontainer in der öffentlichen Vorschau herstellen, indem Sie den obigen SAS-URI eingeben.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre selbstgehostete IR als Proxy für Ihre Azure-SSIS IR konfiguriert haben, können Sie Ihre Pakete bereitstellen und ausführen, um in Form von Aktivitäten vom Typ „SSIS-Paket ausführen“ in Data Factory-Pipelines auf lokale Daten zuzugreifen. Weitere Informationen finden Sie unter [Ausführen von SSIS-Paketen als Aktivitäten vom Typ „SSIS-Paket ausführen“ in Data Factory-Pipelines](./how-to-invoke-ssis-package-ssis-activity.md).
