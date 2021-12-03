---
title: Überwachen von Azure Files | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Leistung und Verfügbarkeit von Azure Files überwachen können. Überwachen Sie Azure Files-Daten, erfahren Sie mehr über die Konfiguration, und analysieren Sie Metrik- und Protokolldaten.
author: normesta
services: storage
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 11/10/2021
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring, devx-track-csharp, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 79164bedf91a62508d7455104bbbcf00e2eacbee
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132289431"
---
# <a name="monitoring-azure-files"></a>Überwachen von Azure Files

Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen basieren, sollten Sie diese Ressourcen auf Verfügbarkeit, Leistung und Betrieb überwachen. In diesem Artikel wird das Überwachen von Daten beschrieben, die von Azure Files generiert wurden. Außerdem wird erläutert, wie Sie die Funktionen von Azure Monitor verwenden können, um Warnungen für diese Daten zu analysieren.

## <a name="applies-to"></a>Gilt für:
| Dateifreigabetyp | SMB | NFS |
|-|:-:|:-:|
| Standard-Dateifreigaben (GPv2), LRS/ZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Standard-Dateifreigaben (GPv2), GRS/GZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Premium-Dateifreigaben (FileStorage), LRS/ZRS | ![Ja](../media/icons/yes-icon.png) | ![Ja](../media/icons/yes-icon.png) |

## <a name="monitor-overview"></a>Azure Monitor: Übersicht

Im Azure-Portal enthält die Seite **Übersicht** für jede Azure Files-Ressource eine kurze Übersicht über die Ressourcennutzung, beispielsweise Anforderungen und stündliche Abrechnung. Dies sind nützliche Informationen, die aber nur einen kleinen Teil der verfügbaren Überwachungsdaten ausmachen. Einige dieser Daten werden automatisch erfasst und sind für die Analyse verfügbar, sobald Sie die Ressource erstellen. Mit einigen Konfigurationsschritten können Sie zusätzliche Typen von Datensammlungen aktivieren.

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?
Azure Files protokolliert Überwachungsdaten mit [Azure Monitor](../../azure-monitor/overview.md), einem Azure-Dienst zur vollständigen Stapelüberwachung. Azure Monitor bietet einen vollständigen Satz von Funktionen zum Überwachen Ihrer Azure-Ressourcen, von Ressourcen in anderen Clouds sowie von lokalen Ressourcen. 

Beginnen Sie mit dem Artikel [Überwachen von Azure-Ressourcen mit Azure Monitor](../../azure-monitor/essentials/monitor-azure-resource.md), in dem die folgenden Punkte beschrieben werden:

- Was ist Azure Monitor?
- Kosten für die Überwachung
- In Azure gesammelte Überwachungsdaten
- Konfigurieren der Datensammlung
- Standardtools in Azure zum Analysieren von Überwachungsdaten sowie zum Generieren von Warnungen

Die folgenden Abschnitte basieren auf diesem Artikel. Darin werden die spezifischen, von Azure Files erfassten Daten beschrieben. In Beispielen wird gezeigt, wie Sie die Datensammlung konfigurieren und diese Daten mit Azure-Tools analysieren.

## <a name="monitoring-data"></a>Überwachungsdaten

Azure Files erfasst dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die unter [Überwachungsdaten von Azure-Ressourcen](../../azure-monitor/essentials/monitor-azure-resource.md#monitoring-data) beschrieben sind. 

Ausführliche Informationen zu den Metriken und Protokollen, die von Azure Files erstellt werden, finden Sie in der [Überwachungsdatenreferenz zu Azure Files](storage-files-monitoring-reference.md).

Die Metriken und Protokolle in Azure Monitor unterstützen nur Azure Resource Manager-Speicherkonten. Klassische Speicherkonten werden von Azure Monitor nicht unterstützt. Wenn Sie Metriken oder Protokolle für ein klassisches Speicherkonto verwenden möchten, müssen Sie das Konto zu einem Azure Resource Manager-Speicherkonto migrieren. Weitere Informationen finden Sie unter [Migrieren zu Azure Resource Manager](../../virtual-machines/migration-classic-resource-manager-overview.md).

## <a name="collection-and-routing"></a>Sammlung und Routing

Plattformmetriken und das Aktivitätsprotokoll werden automatisch erfasst, können jedoch mithilfe einer Diagnoseeinstellung an andere Speicherorte weitergeleitet werden. 

Damit Sie Ressourcenprotokolle erfassen können, müssen Sie eine Diagnoseeinstellung erstellen. Beim Erstellen einer Diagnoseeinstellung wählen Sie **Datei** als den Speichertyp aus, für den Sie Protokolle aktivieren möchten. Geben Sie dann eine der folgenden Kategorien von Vorgängen an, für die Protokolle erfasst werden sollen. 

| Kategorie | BESCHREIBUNG |
|:---|:---|
| StorageRead | Lesevorgänge für Objekte. |
| StorageWrite | Schreibvorgänge für Objekte. |
| StorageDelete | Löschvorgänge für Objekte. |

Wie Sie die Liste der protokollierten SMB- und REST-Vorgänge abrufen, erfahren Sie unter [Storage Analytics: protokollierte Vorgänge und Statusmeldungen](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) und [Überwachungsdatenreferenz zu Azure Files](storage-files-monitoring-reference.md).

## <a name="creating-a-diagnostic-setting"></a>Erstellen einer Diagnoseeinstellung

Sie können eine Diagnoseeinstellung über das Azure-Portal, PowerShell, die Azure CLI, eine Azure Resource Manager-Vorlage oder Azure Policy erstellen.

> [!NOTE]
> Azure Storage-Protokolle in Azure Monitor befinden sich in der öffentlichen Vorschauphase und stehen in allen Regionen der öffentlichen und der US Government-Cloud für Vorschautests zur Verfügung. Diese Vorschau unterstützt Protokolle für Blobdateien (einschließlich Azure Data Lake Storage Gen2), Dateien, Warteschlangen und Tabellen. Dieses Feature ist für alle Speicherkonten verfügbar, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden. Weitere Informationen finden Sie in der [Speicherkontoübersicht](../common/storage-account-overview.md).

Eine allgemeine Anleitung finden Sie unter [Erstellen einer Diagnoseeinstellung zum Erfassen von Plattformprotokollen und Metriken in Azure](../../azure-monitor/essentials/diagnostic-settings.md).

### <a name="azure-portal"></a>[Azure portal](#tab/azure-portal)

1. Melden Sie sich beim Azure-Portal an.

2. Navigieren Sie zu Ihrem Speicherkonto.

3. Klicken Sie im Abschnitt **Überwachung** auf **Diagnoseeinstellungen (Vorschau)** .

   > [!div class="mx-imgBorder"]
   > ![Portal – Diagnoseprotokolle](media/storage-files-monitoring/diagnostic-logs-settings-pane.png)   

4. Wählen Sie **Datei** als den Speichertyp aus, für den Sie Protokolle aktivieren möchten.

5. Klicken Sie auf **Diagnoseeinstellung hinzufügen**.

   > [!div class="mx-imgBorder"]
   > ![Portal: Ressourcenprotokolle – „Diagnoseeinstellung hinzufügen“](media/storage-files-monitoring/diagnostic-logs-settings-pane-2.png)

   Die Seite **Diagnoseeinstellungen** wird angezeigt.

   > [!div class="mx-imgBorder"]
   > ![Seite „Ressourcenprotokolle“](media/storage-files-monitoring/diagnostic-logs-page.png)

6. Geben Sie im Feld **Name** auf der Seite einen Namen für diese Ressourcenprotokolleinstellung ein. Wählen Sie dann aus, welche Vorgänge (Lese-, Schreib- und Löschvorgänge) protokolliert und wohin die Protokolle gesendet werden sollen.

#### <a name="archive-logs-to-a-storage-account"></a>Archivieren von Protokollen in einem Speicherkonto

Wenn Sie auswählen, dass Ihre Protokolle in einem Speicherkonto archiviert werden sollen, bezahlen Sie für die Menge an Protokolldaten, die an das Speicherkonto gesendet werden. Die genauen Preise finden Sie auf der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) im Abschnitt **Plattformprotokolle**.

1. Aktivieren Sie das Kontrollkästchen **In ein Speicherkonto archivieren**, und klicken Sie dann auf die Schaltfläche **Konfigurieren**.

   > [!div class="mx-imgBorder"]   
   > ![Seite „Diagnoseeinstellungen“: „In ein Speicherkonto archivieren“](media/storage-files-monitoring/diagnostic-logs-settings-pane-archive-storage.png)

2. Wählen Sie in der Dropdownliste **Speicherkonto** das Speicherkonto aus, in dem Sie Ihre Protokolle archivieren möchten. Klicken Sie auf die Schaltfläche **OK** und dann auf die Schaltfläche **Speichern**.

   [!INCLUDE [no retention policy](../../../includes/azure-storage-logs-retention-policy.md)]

   > [!NOTE]
   > Bevor Sie ein Speicherkonto als Exportziel auswählen, informieren Sie sich unter [Archivieren von Azure-Ressourcenprotokollen](../../azure-monitor/essentials/resource-logs.md#send-to-azure-storage) über die Voraussetzungen für das Speicherkonto.

#### <a name="stream-logs-to-azure-event-hubs"></a>Streamen von Protokollen an Azure Event Hubs

Wenn Sie auswählen, dass Ihre Protokolle an einen Event Hub gestreamt werden sollen, bezahlen Sie für die Menge an Protokolldaten, die an den Event Hub gesendet werden. Die genauen Preise finden Sie auf der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) im Abschnitt **Plattformprotokolle**.

1. Aktivieren Sie das Kontrollkästchen **An einen Event Hub streamen**, und klicken Sie dann auf die Schaltfläche **Konfigurieren**.

2. Wählen Sie im Bereich **Event Hub auswählen** den Namespace, den Namen und den Richtliniennamen der Event Hub-Instanz aus, an die Sie Ihre Protokolle streamen möchten. 

   > [!div class="mx-imgBorder"]
   > ![Seite „Diagnoseeinstellungen“ mit Event Hub](media/storage-files-monitoring/diagnostic-logs-settings-pane-event-hub.png)

3. Klicken Sie auf die Schaltfläche **OK** und dann auf die Schaltfläche **Speichern**.

#### <a name="send-logs-to-azure-log-analytics"></a>Senden von Protokollen an Azure Log Analytics

1. Aktivieren Sie das Kontrollkästchen **An Log Analytics senden**, wählen Sie einen Log Analytics-Arbeitsbereich aus, und klicken Sie dann auf die Schaltfläche **Speichern**.

   > [!div class="mx-imgBorder"]   
   > ![Seite „Diagnoseeinstellungen“ für Log Analytics](media/storage-files-monitoring/diagnostic-logs-settings-pane-log-analytics.png)

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Öffnen Sie ein Windows PowerShell-Befehlsfenster, und melden Sie sich dann mit dem Befehl `Connect-AzAccount` bei Ihrem Azure-Abonnement an. Befolgen Sie dann die Anweisungen auf dem Bildschirm.

   ```powershell
   Connect-AzAccount
   ```

2. Legen Sie Ihr aktives Abonnement auf das Abonnement des Speicherkontos fest, für das Sie die Protokollierung aktivieren möchten.

   ```powershell
   Set-AzContext -SubscriptionId <subscription-id>
   ```

#### <a name="archive-logs-to-a-storage-account"></a>Archivieren von Protokollen in einem Speicherkonto

Wenn Sie auswählen, dass Ihre Protokolle in einem Speicherkonto archiviert werden sollen, bezahlen Sie für die Menge an Protokolldaten, die an das Speicherkonto gesendet werden. Die genauen Preise finden Sie auf der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) im Abschnitt **Plattformprotokolle**.

Aktivieren Sie Protokolle mit dem PowerShell-Cmdlet [Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting) unter Angabe des Parameters `StorageAccountId`.

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -StorageAccountId <storage-account-resource-id> -Enabled $true -Category <operations-to-log> 
```

Ersetzen Sie den Platzhalter `<storage-service-resource--id>` in diesem Codeausschnitt durch die Ressourcen-ID des Azure-Dateidiensts. Sie finden die Ressourcen-ID im Azure-Portal, indem Sie die Seite **Eigenschaften** Ihres Speicherkontos öffnen.

Sie können `StorageRead`, `StorageWrite` und `StorageDelete` als Wert für den Parameter **Category** angeben.

[!INCLUDE [no retention policy](../../../includes/azure-storage-logs-retention-policy.md)]

Hier sehen Sie ein Beispiel:

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/fileServices/default -StorageAccountId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount -Enabled $true -Category StorageWrite,StorageDelete`

Eine Beschreibung der einzelnen Parameter finden Sie unter [Archivieren von Azure-Ressourcenprotokollen mithilfe von Azure PowerShell](../../azure-monitor/essentials/resource-logs.md#send-to-azure-storage).

#### <a name="stream-logs-to-an-event-hub"></a>Streamen von Protokollen an einen Event Hub

Wenn Sie auswählen, dass Ihre Protokolle an einen Event Hub gestreamt werden sollen, bezahlen Sie für die Menge an Protokolldaten, die an den Event Hub gesendet werden. Die genauen Preise finden Sie auf der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) im Abschnitt **Plattformprotokolle**.

Aktivieren Sie Protokollen mit dem PowerShell-Cmdlet [Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting) und dem Parameter `EventHubAuthorizationRuleId`.

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -EventHubAuthorizationRuleId <event-hub-namespace-and-key-name> -Enabled $true -Category <operations-to-log> -RetentionEnabled <retention-bool> -RetentionInDays <number-of-days>
```

Hier sehen Sie ein Beispiel:

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/fileServices/default -EventHubAuthorizationRuleId /subscriptions/20884142-a14v3-4234-5450-08b10c09f4/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhubnamespace/authorizationrules/RootManageSharedAccessKey -Enabled $true -Category StorageDelete`

Eine Beschreibung der einzelnen Parameter finden Sie unter [Streamen von Daten an Event Hubs mithilfe von PowerShell-Cmdlets](../../azure-monitor/essentials/resource-logs.md#send-to-azure-event-hubs).

#### <a name="send-logs-to-log-analytics"></a>Senden von Protokollen an Log Analytics

Aktivieren Sie Protokollen mit dem PowerShell-Cmdlet [Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting) und dem Parameter `WorkspaceId`.

```powershell
Set-AzDiagnosticSetting -ResourceId <storage-service-resource-id> -WorkspaceId <log-analytics-workspace-resource-id> -Enabled $true -Category <operations-to-log> -RetentionEnabled <retention-bool> -RetentionInDays <number-of-days>
```

Hier sehen Sie ein Beispiel:

`Set-AzDiagnosticSetting -ResourceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/fileServices/default -WorkspaceId /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.OperationalInsights/workspaces/my-analytic-workspace -Enabled $true -Category StorageDelete`

Weitere Informationen finden Sie unter [Streamen von Azure-Ressourcenprotokollen an einen Log Analytics-Arbeitsbereich in Azure Monitor](../../azure-monitor/essentials/resource-logs.md#send-to-log-analytics-workspace).

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

1. Öffnen Sie zunächst [Azure Cloud Shell](../../cloud-shell/overview.md), oder falls Sie die Azure-CLI lokal [installiert](/cli/azure/install-azure-cli) haben, öffnen Sie eine Befehlskonsolenanwendung wie Windows PowerShell.

2. Wenn Ihre Identität mehreren Abonnements zugeordnet ist, legen Sie das aktive Abonnement auf das Abonnement des Speicherkontos fest, für das Sie Protokolle aktivieren möchten.

   ```azurecli-interactive
   az account set --subscription <subscription-id>
   ```

   Ersetzen Sie den Platzhalterwert `<subscription-id>` durch die ID Ihres Abonnements.

#### <a name="archive-logs-to-a-storage-account"></a>Archivieren von Protokollen in einem Speicherkonto

Wenn Sie auswählen, dass Ihre Protokolle in einem Speicherkonto archiviert werden sollen, bezahlen Sie für die Menge an Protokolldaten, die an das Speicherkonto gesendet werden. Die genauen Preise finden Sie auf der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) im Abschnitt **Plattformprotokolle**.

Aktivieren Sie Protokolle mit dem Befehl [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create).

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --storage-account <storage-account-name> --resource <storage-service-resource-id> --resource-group <resource-group> --logs '[{"category": <operations>, "enabled": true}]'
```

Ersetzen Sie den Platzhalter `<storage-service-resource--id>` in diesem Codeausschnitt durch die Ressourcen-ID des Blobspeicherdiensts. Sie finden die Ressourcen-ID im Azure-Portal, indem Sie die Seite **Eigenschaften** Ihres Speicherkontos öffnen.

Sie können `StorageRead`, `StorageWrite` und `StorageDelete` als Wert für den Parameter **Category** angeben.

[!INCLUDE [no retention policy](../../../includes/azure-storage-logs-retention-policy.md)]

Hier sehen Sie ein Beispiel:

`az monitor diagnostic-settings create --name setting1 --storage-account mystorageaccount --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/fileServices/default --resource-group myresourcegroup --logs '[{"category": StorageWrite, "enabled": true}]'`

Eine Beschreibung der einzelnen Parameter finden Sie unter [Archivieren von Ressourcenprotokollen mithilfe der Azure-Befehlszeilenschnittstelle](../../azure-monitor/essentials/resource-logs.md#send-to-azure-storage).

#### <a name="stream-logs-to-an-event-hub"></a>Streamen von Protokollen an einen Event Hub

Wenn Sie auswählen, dass Ihre Protokolle an einen Event Hub gestreamt werden sollen, bezahlen Sie für die Menge an Protokolldaten, die an den Event Hub gesendet werden. Die genauen Preise finden Sie auf der Seite [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/#platform-logs) im Abschnitt **Plattformprotokolle**.

Verwenden Sie für das Aktivieren von Protokollen den Befehl [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create).

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --event-hub <event-hub-name> --event-hub-rule <event-hub-namespace-and-key-name> --resource <storage-account-resource-id> --logs '[{"category": <operations>, "enabled": true "retentionPolicy": {"days": <number-days>, "enabled": <retention-bool}}]'
```

Hier sehen Sie ein Beispiel:

`az monitor diagnostic-settings create --name setting1 --event-hub myeventhub --event-hub-rule /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhubnamespace/authorizationrules/RootManageSharedAccessKey --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/fileServices/default --logs '[{"category": StorageDelete, "enabled": true }]'`

Eine Beschreibung der einzelnen Parameter finden Sie unter [Streamen von Daten an Event Hubs mithilfe der Azure-Befehlszeilenschnittstelle](../../azure-monitor/essentials/resource-logs.md#send-to-azure-event-hubs).

#### <a name="send-logs-to-log-analytics"></a>Senden von Protokollen an Log Analytics

Aktivieren Sie Protokolle mit dem Befehl [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create).

```azurecli-interactive
az monitor diagnostic-settings create --name <setting-name> --workspace <log-analytics-workspace-resource-id> --resource <storage-account-resource-id> --logs '[{"category": <category name>, "enabled": true "retentionPolicy": {"days": <days>, "enabled": <retention-bool}}]'
```

Hier sehen Sie ein Beispiel:

`az monitor diagnostic-settings create --name setting1 --workspace /subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.OperationalInsights/workspaces/my-analytic-workspace --resource /subscriptions/938841be-a40c-4bf4-9210-08bcf06c09f9/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/myloggingstorageaccount/fileServices/default --logs '[{"category": StorageDelete, "enabled": true ]'`

 Weitere Informationen finden Sie unter [Streamen von Azure-Ressourcenprotokollen an einen Log Analytics-Arbeitsbereich in Azure Monitor](../../azure-monitor/essentials/resource-logs.md#send-to-log-analytics-workspace).

### <a name="template"></a>[Vorlage](#tab/template)

Eine Azure Resource Manager-Vorlage zum Erstellen einer Diagnoseeinstellung finden Sie unter [Diagnoseeinstellung für Azure Storage](../../azure-monitor/essentials/resource-manager-diagnostic-settings.md#diagnostic-setting-for-azure-storage).

### <a name="azure-policy"></a>[Azure Policy](#tab/policy)

Sie können eine Diagnoseeinstellung mithilfe einer Richtliniendefinition erstellen. Auf diese Weise können Sie sicherstellen, dass für jedes erstellte oder aktualisierte Konto eine Diagnoseeinstellung erstellt wird. Siehe hierzu [Integrierte Azure Policy-Definitionen für Azure Storage](../common/policy-reference.md).

---

## <a name="analyzing-metrics"></a>Analysieren von Metriken

Mit dem Metrik-Explorer können Sie Metriken für Azure Storage mit Metriken aus anderen Azure-Diensten analysieren. Öffnen Sie den Metrik-Explorer, indem Sie im Menü **Azure Monitor** die Option **Metriken** auswählen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../../azure-monitor/essentials/metrics-getting-started.md). 

Bei Metriken mit Dimensionsunterstützung können Sie die Metrik nach dem gewünschten Dimensionswert filtern.  Eine vollständige Liste der von Azure Storage unterstützten Dimensionen finden Sie unter [Metrikdimensionen](storage-files-monitoring-reference.md#metrics-dimensions). Metriken für Azure Files befinden sich in diesen Namespaces: 

- Microsoft.Storage/storageAccounts
- Microsoft.Storage/storageAccounts/fileServices

Eine Liste aller von Azure Monitor unterstützten Metriken (einschließlich Azure Files) finden Sie unter [Unterstützte Metriken von Azure Monitor](../../azure-monitor/essentials/metrics-supported.md#microsoftstoragestorageaccountsfileservices).

### <a name="accessing-metrics"></a>Zugreifen auf Metriken

> [!TIP]
> Wählen Sie zum Anzeigen von Azure CLI- oder .NET-Beispielen die hier aufgeführten entsprechenden Registerkarten aus.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="list-the-metric-definition"></a>Auflisten der Metrikdefinition

Sie können die Metrikdefinition Ihres Speicherkontos oder des Azure Files-Diensts auflisten. Verwenden Sie das Cmdlet [Get-AzMetricDefinition](/powershell/module/az.monitor/get-azmetricdefinition).

Ersetzen Sie in diesem Beispiel den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder die Ressourcen-ID des Azure Files-Diensts.  Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### <a name="reading-metric-values"></a>Lesen von Metrikwerten

Sie können die Metrikwerte auf der Kontoebene Ihres Speicherkontos oder des Azure Files-Diensts lesen. Verwenden Sie das Cmdlet [Get-AzMetric](/powershell/module/Az.Monitor/Get-AzMetric).

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

#### <a name="reading-metric-values-with-dimensions"></a>Lesen von Metrikwerten mit Dimensionen

Wenn eine Metrik Dimensionen unterstützt, können Sie Metrikwerte lesen und sie mithilfe von Dimensionswerten filtern. Verwenden Sie das Cmdlet [Get-AzMetric](/powershell/module/Az.Monitor/Get-AzMetric).

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```
```powershell
$resourceId = "<resource-ID>"
$dimFilter = [String](New-AzMetricFilter -Dimension ApiName -Operator eq -Value "GetFile" 3> $null)
Get-AzMetric -ResourceId $resourceId -MetricName Transactions -TimeGrain 01:00:00 -MetricFilter $dimFilter -AggregationType "Total"
```


### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

#### <a name="list-the-account-level-metric-definition"></a>Auflisten der Metrikdefinition auf Kontoebene

Sie können die Metrikdefinition Ihres Speicherkontos oder des Azure Files-Diensts auflisten. Verwenden Sie den Befehl [az monitor metrics list-definitions](/cli/azure/monitor/metrics#az_monitor_metrics_list_definitions).
 
Ersetzen Sie in diesem Beispiel den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder die Ressourcen-ID des Azure Files-Diensts. Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### <a name="read-account-level-metric-values"></a>Lesen der Metrikwerte auf der Kontoebene

Sie können die Metrikwerte Ihres Speicherkontos oder des Azure Files-Diensts lesen. Verwenden Sie den Befehl [az monitor metrics list](/cli/azure/monitor/metrics#az_monitor_metrics_list).

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```
#### <a name="reading-metric-values-with-dimensions"></a>Lesen von Metrikwerten mit Dimensionen

Wenn eine Metrik Dimensionen unterstützt, können Sie Metrikwerte lesen und sie mithilfe von Dimensionswerten filtern. Verwenden Sie den Befehl [az monitor metrics list](/cli/azure/monitor/metrics#az_monitor_metrics_list).

```azurecli
az monitor metrics list --resource <resource-ID> --metric "Transactions" --interval PT1H --filter "ApiName eq 'GetFile' " --aggregation "Total" 
```
### <a name="net-sdk"></a>[.NET SDK](#tab/azure-portal)

Azure Monitor bietet das [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) zum Lesen von Metrikdefinition und -werten. Die [Beispielcode](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) zeigt, wie das SDK mit unterschiedlichen Parametern verwendet wird. Sie benötigen `0.18.0-preview` oder eine höhere Version für Speichermetriken.
 
Ersetzen Sie in diesen Beispielen den Platzhalter `<resource-ID>` durch die Ressourcen-ID des gesamten Speicherkontos oder des Azure Files-Diensts. Sie finden diese Ressourcen-IDs im Azure-Portal auf der jeweiligen Seite **Eigenschaften** Ihres Speicherkontos.

Ersetzen Sie die Variable `<subscription-ID>` durch die ID Ihres Abonnements. Eine Anleitung zum Abrufen von Werten für `<tenant-ID>`, `<application-ID>`, und `<AccessKey>` finden Sie unter [Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](../../active-directory/develop/howto-create-service-principal-portal.md). 

#### <a name="list-the-account-level-metric-definition"></a>Auflisten der Metrikdefinition auf Kontoebene

Das folgende Beispiel zeigt, wie Sie die Metrikdefinition auf der Kontoebene auflisten:

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";


        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            // Enumrate metric definition:
            //    Id
            //    ResourceId
            //    Name
            //    Unit
            //    MetricAvailabilities
            //    PrimaryAggregationType
            //    Dimensions
            //    IsDimensionRequired
        }
    }

```

#### <a name="reading-account-level-metric-values"></a>Lesen von Metrikwerten auf Kontoebene

Das folgende Beispiel zeigt, wie Sie `UsedCapacity`-Daten auf der Kontoebene lesen:

```csharp
    public static async Task ReadStorageMetricValue()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metricnames: "UsedCapacity",

            aggregation: "Average",
            resultType: ResultType.Data,
            cancellationToken: CancellationToken.None);

        foreach (var metric in Response.Value)
        {
            // Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

#### <a name="reading-multidimensional-metric-values"></a>Lesen von Werten mehrdimensionaler Metriken

Für mehrdimensionale Metriken müssen Sie Metadatenfilter definieren, wenn Metrikdaten für bestimmte Dimensionswerte gelesen werden sollen.

Im folgenden Beispiel wird gezeigt, wie Metrikdaten bei Metriken mit Unterstützung für mehrere Dimensionen gelesen werden:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for Azure Files
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/fileServices/default";
        var subscriptionId = "<subscription-ID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metricnames: "BlobCapacity",
                        odataQuery: odataFilterMetrics,
                        aggregation: "Average",
                        resultType: ResultType.Data);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

# <a name="template"></a>[Vorlage](#tab/template)

N/V.

### <a name="azure-policy"></a>[Azure Policy](#tab/policy)

N/V.

---

## <a name="analyzing-logs"></a>Analysieren von Protokollen

Sie können auf Ressourcenprotokolle entweder als Blob in einem Speicherkonto, als Ereignisdaten oder über Log Analytics-Abfragen zugreifen.

Wie Sie die Liste der protokollierten SMB- und REST-Vorgänge abrufen, erfahren Sie unter [Storage Analytics: protokollierte Vorgänge und Statusmeldungen](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) und [Überwachungsdatenreferenz zu Azure Files](storage-files-monitoring-reference.md).

> [!NOTE]
> Azure Storage-Protokolle in Azure Monitor befinden sich in der öffentlichen Vorschauphase und stehen in allen Regionen der öffentlichen und der US Government-Cloud für Vorschautests zur Verfügung. Diese Vorschauversion ermöglicht Protokolle für Blobs (einschließlich Azure Data Lake Storage Gen2), Dateien, Warteschlangen, Tabellen, Universell V1-Premium-Speicherkonten und Universell V2-Speicherkonten. Klassische Speicherkonten werden nicht unterstützt.

Protokolleinträge werden nur erstellt, wenn Anforderungen für den Dienstendpunkt gestellt wurden. Wenn beispielsweise ein Speicherkonto Aktivität im Dateiendpunkt, jedoch nicht im Tabellen- oder Warteschlangenendpunkt aufweist, werden nur Protokolle für den Azure-Dateidienst erstellt. Azure Storage-Protokolle enthalten ausführliche Informationen über erfolgreiche und fehlgeschlagene Anforderungen an einen Speicherdienst. Anhand dieser Informationen können einzelne Anforderungen überwacht und Probleme mit einem Speicherdienst untersucht werden. Anforderungen werden auf Grundlage der besten Leistung protokolliert.

### <a name="log-authenticated-requests"></a>Protokollieren von authentifizierten Anforderungen

 Die folgenden Typen authentifizierter Anforderungen werden protokolliert:

- Erfolgreiche Anforderungen
- Fehlerhafte Anforderungen, einschließlich Timeout-, Drosselungs-, Netzwerk- und Autorisierungsfehler sowie anderer Fehler
- Anforderungen, die Kerberos, NTLM oder eine SAS (Shared Access Signature) verwenden, einschließlich fehlerhafter und erfolgreicher Anforderungen
- Anforderungen an Analysedaten (klassische Protokolldaten im Container **$logs** und klassische Metrikdaten in den **$metric**-Tabellen)

Anforderungen, die durch den Azure Files-Dienst selbst erfolgen, wie z. B. Protokollerstellungs- oder -löschvorgänge, werden nicht protokolliert. Eine vollständige Liste der protokollierten SMB- und REST-Anforderungen finden Sie unter [Storage Analytics: protokollierte Vorgänge und Statusmeldungen](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) und [Überwachungsdatenreferenz zu Azure Files](storage-files-monitoring-reference.md).

### <a name="accessing-logs-in-a-storage-account"></a>Zugreifen auf Protokolle in einem Speicherkonto

Protokolle werden als Blobs angezeigt, die im Zielspeicherkonto in einem Container gespeichert sind. Die erfassten Daten werden in einem einzelnen Blob als durch Zeilen getrennte JSON-Nutzlast gespeichert. Für den Namen des Blobs wird diese Benennungskonvention verwendet:

`https://<destination-storage-account>.blob.core.windows.net/insights-logs-<storage-operation>/resourceId=/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<source-storage-account>/fileServices/default/y=<year>/m=<month>/d=<day>/h=<hour>/m=<minute>/PT1H.json`

Hier sehen Sie ein Beispiel:

`https://mylogstorageaccount.blob.core.windows.net/insights-logs-storagewrite/resourceId=/subscriptions/`<br>`208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/fileServices/default/y=2019/m=07/d=30/h=23/m=12/PT1H.json`

### <a name="accessing-logs-in-an-event-hub"></a>Zugreifen auf Protokolle in einem Event Hub

An einen Event Hub gesendete Protokolle werden nicht als Datei gespeichert. Sie können jedoch überprüfen, ob der Event Hub die Protokollinformationen empfangen hat. Wechseln Sie im Azure-Portal zu Ihrem Event Hub, und vergewissern Sie sich, dass die Anzahl **eingehender Nachrichten** größer als Null ist. 

![Überwachungsprotokolle](media/storage-files-monitoring/event-hub-log.png)

Mithilfe von SIEM- (Security Information & Event Management) und Überwachungstools können Sie auf die an Ihren Event Hub gesendeten Protokolldaten zugreifen und diese lesen. Unter [Wie kann ich die an meinen Event Hub gesendeten Überwachungsdaten nutzen?](../../azure-monitor/essentials/stream-monitoring-data-event-hubs.md) finden Sie weitere Informationen dazu.

### <a name="accessing-logs-in-a-log-analytics-workspace"></a>Zugreifen auf Protokolle in einem Log Analytics-Arbeitsbereich

Mithilfe von Azure Monitor-Protokollabfragen können Sie auf Protokolle zugreifen, die an einen Log Analytics-Arbeitsbereich gesendet wurden. Die Daten werden in der **StorageFileLogs**-Tabelle gespeichert. 

Weitere Informationen finden Sie im [Log Analytics-Tutorial](../../azure-monitor/logs/log-analytics-tutorial.md).

#### <a name="sample-kusto-queries"></a>Kusto-Beispielabfragen

Im Folgenden finden Sie einige Abfragen, die Sie in die **Protokollsuchleiste** eingeben können, um die Überwachung Ihrer Azure Files-Instanz zu vereinfachen. Diese Abfragen arbeiten mit der [neuen Sprache](../../azure-monitor/logs/log-query-overview.md).

> [!IMPORTANT]
> Wenn Sie **Protokolle** im Speicherkonto-Ressourcengruppenmenü auswählen, wird Log Analytics geöffnet, wobei der Abfragebereich auf die aktuelle Ressourcengruppe festgelegt ist. Dies bedeutet, dass Protokollabfragen nur Daten aus dieser Ressourcengruppe umfassen. Wenn Sie eine Abfrage ausführen möchten, die Daten aus anderen Ressourcen oder Daten aus anderen Azure-Diensten enthält, wählen Sie im Menü **Azure Monitor** die Option **Protokolle** aus. Ausführliche Informationen finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](../../azure-monitor/logs/scope.md).

Verwenden Sie diese Abfragen, um Ihre Azure-Dateifreigaben zu überwachen:

- Anzeigen von SMB-Fehlern in der letzten Woche

```Kusto
StorageFileLogs
| where Protocol == "SMB" and TimeGenerated >= ago(7d) and StatusCode contains "-"
| sort by StatusCode
```
- Erstellen eines Kreisdiagramms der SMB-Vorgänge in der letzten Woche

```Kusto
StorageFileLogs
| where Protocol == "SMB" and TimeGenerated >= ago(7d) 
| summarize count() by OperationName
| sort by count_ desc
| render piechart
```

- Anzeigen von REST-Fehlern in der letzten Woche

```Kusto
StorageFileLogs
| where Protocol == "HTTPS" and TimeGenerated >= ago(7d) and StatusText !contains "Success"
| sort by StatusText asc
```

- Erstellen eines Kreisdiagramms der REST-Vorgänge in der letzten Woche

```Kusto
StorageFileLogs
| where Protocol == "HTTPS" and TimeGenerated >= ago(7d) 
| summarize count() by OperationName
| sort by count_ desc
| render piechart
```

Informationen zum Anzeigen der Liste der Spaltennamen und Beschreibungen für Azure Files finden Sie unter [StorageFileLogs](/azure/azure-monitor/reference/tables/storagefilelogs).

Weitere Informationen zum Schreiben von Abfragen finden Sie im [Log Analytics-Tutorial](../../azure-monitor/logs/log-analytics-tutorial.md).

## <a name="alerts"></a>Alerts

Azure Monitor-Warnungen informieren Sie proaktiv, wenn wichtige Bedingungen in Ihren Überwachungsdaten gefunden werden. Sie ermöglichen Ihnen, Probleme in Ihrem System zu identifizieren und zu beheben, bevor Ihre Kunden sie bemerken. Sie können Warnungen für [Metriken](../../azure-monitor/alerts/alerts-metric-overview.md), [Protokolle](../../azure-monitor/alerts/alerts-unified-log.md) und das [Aktivitätsprotokoll](../../azure-monitor/alerts/activity-log-alerts.md) festlegen. 

In der folgenden Tabelle sind einige Beispielszenarios für die Überwachung und die jeweils geeignete Metrik für die Warnung aufgeführt:

| Szenario | Für Warnung zu verwendende Metrik |
|-|-|
| Dateifreigabe wird gedrosselt. | Metrik: Transaktionen<br>Dimensionsname: Antworttyp <br>Dimensionsname: FileShare (nur Freigabe von Premiumdateien) |
| Die Größe der Dateifreigabe beträgt 80 % der Kapazität. | Metrik: Dateikapazität<br>Dimensionsname: FileShare (nur Freigabe von Premiumdateien) |
| Der Dateifreigabeausgang hat 500 GiB an einem Tag überschritten. | Metrik: Ausgehende Daten<br>Dimensionsname: FileShare (nur Freigabe von Premiumdateien) |

### <a name="how-to-create-alerts-for-azure-files"></a>Erstellen von Warnungen für Azure Files

1. Navigieren Sie im **Azure-Portal** zu Ihrem **Speicherkonto**. 

2. Klicken Sie auf **Warnungen** und dann auf **+ Neue Warnungsregel**.

3. Klicken Sie auf **Ressource bearbeiten**, wählen Sie den **Dateiressourcentyp** aus, und klicken Sie anschließend auf **Fertig**. 

4. Klicken Sie auf **Bedingung hinzufügen**, und geben Sie die folgenden Informationen zur Warnung an: 

    - **Metrik**
    - **Dimensionsname**
    - **Warnungslogik**

5. Klicken Sie auf **Aktionsgruppe hinzufügen**, und fügen Sie der Warnung eine Aktionsgruppe (E-Mail, SMS usw.) hinzu, indem Sie entweder eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.

6. Geben Sie die **Warnungsdetails** wie **Warnungsregelname**, **Beschreibung** und **Schweregrad** ein.

7. Klicken Sie auf **Warnungsregel erstellen**, um die Warnung zu erstellen.

> [!NOTE]  
> Wenn Sie eine Warnung erstellen und zu viele Warnungen ausgelöst werden, passen Sie den Schwellenwert und die Warnungslogik an.

### <a name="how-to-create-an-alert-if-a-file-share-is-throttled"></a>Erstellen einer Warnung, wenn eine Dateifreigabe gedrosselt ist

1. Navigieren Sie im **Azure-Portal** zu Ihrem **Speicherkonto**.
2. Klicken Sie im Abschnitt **Überwachung** auf **Warnungen** und anschließend auf **+ Neue Warnungsregel**.
3. Klicken Sie auf **Ressource bearbeiten**, wählen Sie unter **Ressourcentyp** den Ressourcentyp für das Speicherkonto aus, und klicken Sie anschließend auf **Fertig**. Wenn der Name des Speicherkontos beispielsweise `contoso` ist, wählen Sie die `contoso/file`-Ressource aus.
4. Klicken Sie auf **Bedingung hinzufügen**, um eine Bedingung hinzuzufügen.
5. Wählen Sie in der daraufhin angezeigten Liste der für das Speicherkonto unterstützten Signale die Metrik **Transaktionen** aus.
6. Klicken Sie auf dem Blatt **Signallogik konfigurieren** auf das Dropdownmenü **Dimensionsname**, und wählen Sie **Antworttyp** aus.
7. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie die entsprechenden Antworttypen für die Dateifreigabe aus.

    Wählen Sie für Standard-Dateifreigaben die folgenden Antworttypen aus:

    - SuccessWithShareIopsThrottling
    - SuccessWithThrottling
    - ClientShareIopsThrottlingError

    Wählen Sie für Premium-Dateifreigaben die folgenden Antworttypen aus:

    - SuccessWithShareEgressThrottling
    - SuccessWithShareIngressThrottling
    - SuccessWithShareIopsThrottling
    - ClientShareEgressThrottlingError
    - ClientShareIngressThrottlingError
    - ClientShareIopsThrottlingError

   > [!NOTE]
   > Wenn die Antworttypen nicht in der Dropdownliste **Dimensionswerte** aufgeführt werden, bedeutet dies, dass die Ressource nicht gedrosselt wurde. Wählen Sie zum Hinzufügen der Dimensionswerte neben der Dropdownliste **Dimensionswerte** die Option **Benutzerdefinierten Wert hinzufügen** aus, geben Sie den Typ ein (z. B. **SuccessWithThrottling**), und wählen Sie **OK** aus. Wiederholen Sie diese Schritte zum Hinzufügen aller betreffenden Antworttypen für die Dateifreigabe.

8. Klicken Sie bei **Premium-Dateifreigaben** auf die Dropdownliste **Dimensionsname**, und wählen Sie **Dateifreigabe** aus. Springen Sie bei **Standarddateifreigaben** zu **Schritt #10**.

   > [!NOTE]
   > Wenn die Dateifreigabe eine Standarddateifreigabe ist, wird/werden die Dateifreigabe(n) in der Dimension **Dateifreigabe** nicht aufgeführt, weil Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind. Die Drosselung von Warnungen für Standarddateifreigaben wird ausgelöst, wenn eine Dateifreigabe im Speicherkonto gedrosselt wird und die Warnung nicht identifiziert, welche Freigabe gedrosselt wurde. Da Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind, wird empfohlen, dass es eine einzige Dateifreigabe pro Speicherkonto gibt.

9. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie die Dateifreigaben aus, für die Sie eine Warnung einrichten möchten.
10. Definieren Sie die **Warnungsparameter** (Schwellenwert, Operator, Aggregationsgranularität und Häufigkeit der Auswertung), und klicken Sie auf **Fertig**.

    > [!TIP]
    > Bei Verwendung eines statischen Schwellenwerts kann das Metrikdiagramm bei der Ermittlung eines sinnvollen Schwellenwerts helfen, wenn die Dateifreigabe gerade gedrosselt wird. Falls Sie einen dynamischen Schwellenwert verwenden, zeigt das Metrikdiagramm die berechneten Schwellenwerte basierend auf aktuellen Daten an.

11. Klicken Sie auf **Aktionsgruppe hinzufügen**, und fügen Sie der Warnung eine **Aktionsgruppe** (E-Mail, SMS usw.) hinzu, indem Sie eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.
12. Geben Sie die **Warnungsdetails** wie **Warnungsregelname**, **Beschreibung** und **Schweregrad** ein.
13. Klicken Sie auf **Warnungsregel erstellen**, um die Warnung zu erstellen.

### <a name="how-to-create-an-alert-if-the-azure-file-share-size-is-80-of-capacity"></a>Erstellen einer Warnung, wenn die Größe der Azure-Dateifreigabe 80 % der Kapazität beträgt

1. Navigieren Sie im **Azure-Portal** zu Ihrem **Speicherkonto**.
2. Klicken Sie im Abschnitt **Überwachung** auf **Warnungen** und anschließend auf **+ Neue Warnungsregel**.
3. Klicken Sie auf **Ressource bearbeiten**, wählen Sie unter **Ressourcentyp** den Ressourcentyp für das Speicherkonto aus, und klicken Sie anschließend auf **Fertig**. Wenn der Name des Speicherkontos beispielsweise `contoso` ist, wählen Sie die `contoso/file`-Ressource aus.
4. Klicken Sie auf **Bedingung hinzufügen**, um eine Bedingung hinzuzufügen.
5. Wählen Sie in der daraufhin angezeigten Liste der für das Speicherkonto unterstützten Signale die Metrik **Dateikapazität** aus.
6. Klicken Sie bei **Premium-Dateifreigaben** auf die Dropdownliste **Dimensionsname**, und wählen Sie **Dateifreigabe** aus. Springen Sie bei **Standarddateifreigaben** zu **Schritt #8**.

   > [!NOTE]
   > Wenn die Dateifreigabe eine Standarddateifreigabe ist, wird/werden die Dateifreigabe(n) in der Dimension **Dateifreigabe** nicht aufgeführt, weil Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind. Warnungen für Standarddateifreigaben basieren auf allen Dateifreigaben im Speicherkonto. Da Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind, wird empfohlen, dass es eine einzige Dateifreigabe pro Speicherkonto gibt.

7. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie die Dateifreigaben aus, für die Sie eine Warnung einrichten möchten.
8. Geben Sie den **Schwellenwert** in Bytes ein. Wenn die Größe der Dateifreigabe z. B. 100 TiB beträgt und Sie eine Warnung erhalten möchten, wenn die Größe der Dateifreigabe 80 % der Kapazität beträgt, ist der Schwellenwert in Bytes 87960930222080.
9. Definieren Sie die übrigen **Warnungsparameter** (Aggregationsgranularität und Häufigkeit der Auswertung), und klicken Sie auf **Fertig**.
10. Klicken Sie auf **Aktionsgruppe hinzufügen**, und fügen Sie der Warnung eine **Aktionsgruppe** (E-Mail, SMS usw.) hinzu, indem Sie eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.
11. Geben Sie die **Warnungsdetails** wie **Warnungsregelname**, **Beschreibung** und **Schweregrad** ein.
12. Klicken Sie auf **Warnungsregel erstellen**, um die Warnung zu erstellen.

### <a name="how-to-create-an-alert-if-the-azure-file-share-egress-has-exceeded-500-gib-in-a-day"></a>Erstellen einer Warnung, wenn der Ausgang der Azure-Dateifreigabe 500 GiB pro Tag überschritten hat

1. Navigieren Sie im **Azure-Portal** zu Ihrem **Speicherkonto**.
2. Klicken Sie im Abschnitt „Überwachung“ auf **Warnungen** und anschließend auf **+ Neue Warnungsregel**.
3. Klicken Sie auf **Ressource bearbeiten**, wählen Sie unter **Ressourcentyp** den Ressourcentyp für das Speicherkonto aus, und klicken Sie anschließend auf **Fertig**. Wenn der Name des Speicherkontos z. B. „contoso“ lautet, wählen Sie die Ressource „contoso/datei“ aus.
4. Klicken Sie auf **Bedingung hinzufügen**, um eine Bedingung hinzuzufügen.
5. Wählen Sie in der daraufhin angezeigten Liste der für das Speicherkonto unterstützten Signale die Metrik **Ausgehend** aus.
6. Klicken Sie bei **Premium-Dateifreigaben** auf die Dropdownliste **Dimensionsname**, und wählen Sie **Dateifreigabe** aus. Springen Sie bei **Standarddateifreigaben** zu **Schritt #8**.

   > [!NOTE]
   > Wenn die Dateifreigabe eine Standarddateifreigabe ist, wird/werden die Dateifreigabe(n) in der Dimension **Dateifreigabe** nicht aufgeführt, weil Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind. Warnungen für Standarddateifreigaben basieren auf allen Dateifreigaben im Speicherkonto. Da Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind, wird empfohlen, dass es eine einzige Dateifreigabe pro Speicherkonto gibt.

7. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie die Dateifreigaben aus, für die Sie eine Warnung einrichten möchten.
8. Geben Sie **536870912000** Bytes für den Schwellenwert ein. 
9. Klicken Sie auf die Dropdownliste **Aggregationsgranularität**, und wählen Sie **24 Stunden** aus.
10. Wählen Sie die **Häufigkeit der Auswertung** aus, und klicken Sie auf **Fertig**.
11. Klicken Sie auf **Aktionsgruppe hinzufügen**, und fügen Sie der Warnung eine **Aktionsgruppe** (E-Mail, SMS usw.) hinzu, indem Sie eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.
12. Geben Sie die **Warnungsdetails** wie **Warnungsregelname**, **Beschreibung** und **Schweregrad** ein.
13. Klicken Sie auf **Warnungsregel erstellen**, um die Warnung zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen von Azure Files](storage-files-monitoring-reference.md)
- [Überwachen von Azure-Ressourcen mit Azure Monitor](../../azure-monitor/essentials/monitor-azure-resource.md)
- [Migration von Azure Storage-Metriken](../common/storage-metrics-migration.md)
- [Planung für eine Azure Files-Bereitstellung](./storage-files-planning.md)
- [Bereitstellen von Azure Files](./storage-how-to-create-file-share.md)
- [Problembehandlung für Azure Files unter Windows](./storage-troubleshoot-windows-file-connection-problems.md)
- [Problembehandlung für Azure Files unter Linux](./storage-troubleshoot-linux-file-connection-problems.md)
