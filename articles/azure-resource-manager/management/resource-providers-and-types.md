---
title: Ressourcenanbieter und Ressourcentypen
description: Hier werden die Ressourcenanbieter beschrieben, die Azure Resource Manager unterstützen. Sie erfahren mehr über die Schemas, verfügbaren API-Versionen und die Regionen, in denen die Ressourcen gehostet werden können.
ms.topic: conceptual
ms.date: 11/15/2021
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: c048da5d7027885bf30b512d9e16e5851697c8b4
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132522648"
---
# <a name="azure-resource-providers-and-types"></a>Azure-Ressourcenanbieter und -typen

Beim Bereitstellen von Ressourcen müssen Sie häufig Informationen zu den Ressourcenanbietern und -typen abrufen. Wenn Sie beispielsweise Schlüssel und Geheimnisse speichern möchten, verwenden Sie den Ressourcenanbieter „Microsoft.KeyVault“. Dieser Ressourcenanbieter bietet einen Ressourcentyp mit dem Namen „vaults“ zum Erstellen des Schlüsseltresors an.

Der Name eines Ressourcentyps hat folgendes Format: **{Ressourcenanbieter}/{Ressourcentyp}**. Der Ressourcentyp für einen Schlüsseltresor ist **Microsoft.KeyVault/vaults**.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

* Anzeigen aller Ressourcenanbieter in Azure
* Überprüfen des Registrierungsstatus eines Ressourcenanbieters
* Registrieren eines Ressourcenanbieters
* Anzeigen von Ressourcentypen für einen Ressourcenanbieter
* Anzeigen gültiger Standorte für einen Ressourcentyp
* Anzeigen gültiger API-Versionen für einen Ressourcentyp

Diese Schritte können über das Azure-Portal, mithilfe von Azure PowerShell oder unter Verwendung der Azure-Befehlszeilenschnittstelle ausgeführt werden.

Eine Liste, die Ressourcenanbieter zu Azure-Diensten zuordnet, finden Sie unter [Ressourcenanbieter für Azure-Dienste](azure-services-resource-providers.md).

## <a name="register-resource-provider"></a>Registrieren des Ressourcenanbieters

Bevor Sie einen Ressourcenanbieter verwenden können, müssen Sie Ihr Azure-Abonnement bei diesem registrieren. Durch die Registrierung wird Ihr Abonnement für die Verwendung mit dem Ressourcenanbieter konfiguriert. 

> [!IMPORTANT]
> Registrieren Sie einen Ressourcenanbieter nur dann, wenn Sie ihn nutzen möchten. Der Registrierungsschritt ermöglicht Ihnen, die geringstmöglichen Berechtigungen innerhalb Ihres Abonnements beizubehalten. Ein böswilliger Benutzer kann nicht registrierte Ressourcenanbieter nicht verwenden.

Einige Ressourcenanbieter sind standardmäßig registriert. Eine Liste der standardmäßig registrierten Anbieter von Azure-Ressourcen finden Sie unter [Ressourcenanbieter für Azure-Dienste](azure-services-resource-providers.md).

Andere werden bei bestimmten Aktionen automatisch registriert. Beim Erstellen einer Ressource über das Portal wird der Ressourcenanbieter in der Regel für Sie registriert. Wenn Sie eine Azure Resource Manager-Vorlage oder Bicep-Datei bereitstellen, werden in der Vorlage definierte Ressourcenanbieter automatisch registriert. Wenn jedoch eine Ressource in der Vorlage unterstützende Ressourcen erstellt, die nicht in der Vorlage enthalten sind, z. B. Überwachungs- oder Sicherheitsressourcen, müssen Sie diese Ressourcenanbieter manuell registrieren.

In anderen Szenarien müssen Sie den Ressourcenanbieter unter Umständen manuell registrieren.

> [!IMPORTANT]
> Ihr Anwendungscode sollte **nicht die Erstellung von Ressourcen für einen Ressourcenanbieter blockieren**, der sich im Status **Wird registriert** befindet. Wenn Sie den Ressourcenanbieter registrieren, wird der Vorgang für jede unterstützte Region einzeln ausgeführt. Um Ressourcen in einer Region zu erstellen, muss die Registrierung nur in dieser Region abgeschlossen werden. Indem Sie den Ressourcenanbieter im Status „Wird registriert“ nicht blockieren, kann die Anwendung viel früher fortfahren und muss nicht auf den Abschluss aller Regionen warten.

Sie benötigen Berechtigungen zum Ausführen des `/register/action`-Vorgangs für den Ressourcenanbieter. Die Berechtigung ist in den Rollen „Mitwirkender“ und „Besitzer“ enthalten.

Sie können die Registrierung eines Ressourcenanbieters nicht aufheben, wenn in Ihrem Abonnement noch Ressourcentypen von diesem Ressourcenanbieter vorhanden sind.

## <a name="azure-portal"></a>Azure-Portal

### <a name="register-resource-provider"></a>Registrieren des Ressourcenanbieters

So zeigen Sie alle Ressourcenanbieter und den Registrierungsstatus für Ihr Abonnement an

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie im Menü im Azure-Portal nach **Abonnements**. Wählen Sie es aus den verfügbaren Optionen aus.

   :::image type="content" source="./media/resource-providers-and-types/search-subscriptions.png" alt-text="Suchen von Abonnements":::

1. Wählen Sie das Abonnement aus, das Sie anzeigen möchten.

   :::image type="content" source="./media/resource-providers-and-types/select-subscription.png" alt-text="„Abonnements“ auswählen":::

1. Wählen Sie im linken Menü unter **Einstellungen** die Option **Ressourcenanbieter** aus.

   :::image type="content" source="./media/resource-providers-and-types/select-resource-providers.png" alt-text="Auswählen des Ressourcenanbieters":::

6. Suchen Sie den Ressourcenanbieter, den Sie registrieren möchten, und wählen Sie **Registrieren** aus. Um die geringstmöglichen Berechtigungen innerhalb Ihres Abonnements beizubehalten, registrieren Sie nur die Ressourcenanbieter, die Sie bereit sind zu verwenden.

   :::image type="content" source="./media/resource-providers-and-types/register-resource-provider.png" alt-text="Registrieren von Ressourcenanbietern":::

> [!IMPORTANT]
> Wie [bereits erwähnt](#register-resource-provider), sollten Sie die Erstellung von Ressourcen für einen Ressourcenanbieter, der sich im Status **Wird registriert** befindet, **nicht blockieren**. Indem Sie den Ressourcenanbieter im Status „Wird registriert“ nicht blockieren, kann die Anwendung viel früher fortfahren und muss nicht auf den Abschluss aller Regionen warten.


### <a name="view-resource-provider"></a>Anzeigen des Ressourcenanbieters

So zeigen Sie Informationen für einen bestimmten Ressourcenanbieter an

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü des Azure-Portals die Option **Alle Dienste** aus.
3. Geben Sie im Feld **Alle Dienste****Ressourcen-Explorer** ein, und wählen Sie dann **Ressourcen-Explorer** aus.

    ![Auswahl von „Alle Dienste“](./media/resource-providers-and-types/select-resource-explorer.png)

4. Erweitern Sie **Anbieter**, indem Sie den Pfeil nach rechts auswählen.

    ![„Anbieter“ auswählen](./media/resource-providers-and-types/select-providers.png)

5. Erweitern Sie einen Ressourcenanbieter und Ressourcentyp, die Sie anzeigen möchten.

    ![Ressourcentyp auswählen](./media/resource-providers-and-types/select-resource-type.png)

6. Der Ressourcen-Manager wird in allen Regionen unterstützt, aber die Ressourcen, die Sie bereitstellen, werden möglicherweise nicht in allen Regionen unterstützt. Darüber hinaus liegen möglicherweise Einschränkungen Ihres Abonnements vor, die die Verwendung einiger Regionen verhindern, die die Ressource unterstützen. Der Ressourcen-Explorer zeigt gültige Standorte für den Ressourcentyp an.

    ![Standorte anzeigen](./media/resource-providers-and-types/show-locations.png)

7. Die API-Version entspricht einer Version von REST-API-Vorgängen, die vom Ressourcenanbieter freigegeben werden. Wenn ein Ressourcenanbieter neue Features aktiviert, wird eine neue Version der REST-API veröffentlicht. Der Ressourcen-Explorer zeigt gültige API-Versionen für den Ressourcentyp an.

    ![API-Versionen anzeigen](./media/resource-providers-and-types/show-api-versions.png)

## <a name="azure-powershell"></a>Azure PowerShell

Verwenden Sie Folgendes, um alle Ressourcenanbieter in Azure und den Registrierungsstatus für Ihr Abonnement anzuzeigen:

```azurepowershell-interactive
Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Der Befehl gibt Folgendes zurück:

```output
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Verwenden Sie zum Anzeigen aller für Ihr Abonnement registrierten Ressourcenanbieter Folgendes:

```azurepowershell-interactive
 Get-AzResourceProvider -ListAvailable | Where-Object RegistrationState -eq "Registered" | Select-Object ProviderNamespace, RegistrationState | Sort-Object ProviderNamespace
```

Um die geringstmöglichen Berechtigungen innerhalb Ihres Abonnements beizubehalten, registrieren Sie nur die Ressourcenanbieter, die Sie bereit sind zu verwenden. Verwenden Sie zum Registrieren eines Ressourcenanbieters Folgendes:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Der Befehl gibt Folgendes zurück:

```output
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

> [!IMPORTANT]
> Wie [bereits erwähnt](#register-resource-provider), sollten Sie die Erstellung von Ressourcen für einen Ressourcenanbieter, der sich im Status **Wird registriert** befindet, **nicht blockieren**. Indem Sie den Ressourcenanbieter im Status „Wird registriert“ nicht blockieren, kann die Anwendung viel früher fortfahren und muss nicht auf den Abschluss aller Regionen warten.

Verwenden Sie Folgendes, um Informationen für einen bestimmten Ressourcenanbieter anzuzeigen:

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.Batch
```

Der Befehl gibt Folgendes zurück:

```output
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

Verwenden Sie Folgendes, um die Ressourcentypen für einen Ressourcenanbieter anzuzeigen:

```azurepowershell-interactive
(Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Der Befehl gibt Folgendes zurück:

```output
batchAccounts
operations
locations
locations/quotas
```

Die API-Version entspricht einer Version von REST-API-Vorgängen, die vom Ressourcenanbieter freigegeben werden. Wenn ein Ressourcenanbieter neue Features aktiviert, wird eine neue Version der REST-API veröffentlicht.

Verwenden Sie Folgendes, um die verfügbaren API-Versionen für einen Ressourcentyp abzurufen:

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Der Befehl gibt Folgendes zurück:

```output
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Der Ressourcen-Manager wird in allen Regionen unterstützt, aber die Ressourcen, die Sie bereitstellen, werden möglicherweise nicht in allen Regionen unterstützt. Darüber hinaus liegen möglicherweise Einschränkungen Ihres Abonnements vor, die die Verwendung einiger Regionen verhindern, die die Ressource unterstützen.

Verwenden Sie Folgendes, um die unterstützten Standorte für einen Ressourcentyp abzurufen:

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Der Befehl gibt Folgendes zurück:

```output
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure CLI

Verwenden Sie Folgendes, um alle Ressourcenanbieter in Azure und den Registrierungsstatus für Ihr Abonnement anzuzeigen:

```azurecli-interactive
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Der Befehl gibt Folgendes zurück:

```output
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Verwenden Sie zum Anzeigen aller für Ihr Abonnement registrierten Ressourcenanbieter Folgendes:

```azurecli-interactive
az provider list --query "sort_by([?registrationState=='Registered'].{Provider:namespace, Status:registrationState}, &Provider)" --out table
```

Um die geringstmöglichen Berechtigungen innerhalb Ihres Abonnements beizubehalten, registrieren Sie nur die Ressourcenanbieter, die Sie bereit sind zu verwenden. Verwenden Sie zum Registrieren eines Ressourcenanbieters Folgendes:

```azurecli-interactive
az provider register --namespace Microsoft.Batch
```

Der Befehl gibt eine Meldung zurück, dass die Registrierung durchgeführt wird.

Verwenden Sie Folgendes, um Informationen für einen bestimmten Ressourcenanbieter anzuzeigen:

```azurecli-interactive
az provider show --namespace Microsoft.Batch
```

Der Befehl gibt Folgendes zurück:

```output
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

> [!IMPORTANT]
> Wie [bereits erwähnt](#register-resource-provider), sollten Sie die Erstellung von Ressourcen für einen Ressourcenanbieter, der sich im Status **Wird registriert** befindet, **nicht blockieren**. Indem Sie den Ressourcenanbieter im Status „Wird registriert“ nicht blockieren, kann die Anwendung viel früher fortfahren und muss nicht auf den Abschluss aller Regionen warten.

Verwenden Sie Folgendes, um die Ressourcentypen für einen Ressourcenanbieter anzuzeigen:

```azurecli-interactive
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Der Befehl gibt Folgendes zurück:

```output
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

Die API-Version entspricht einer Version von REST-API-Vorgängen, die vom Ressourcenanbieter freigegeben werden. Wenn ein Ressourcenanbieter neue Features aktiviert, wird eine neue Version der REST-API veröffentlicht.

Verwenden Sie Folgendes, um die verfügbaren API-Versionen für einen Ressourcentyp abzurufen:

```azurecli-interactive
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Der Befehl gibt Folgendes zurück:

```output
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Der Ressourcen-Manager wird in allen Regionen unterstützt, aber die Ressourcen, die Sie bereitstellen, werden möglicherweise nicht in allen Regionen unterstützt. Darüber hinaus liegen möglicherweise Einschränkungen Ihres Abonnements vor, die die Verwendung einiger Regionen verhindern, die die Ressource unterstützen.

Verwenden Sie Folgendes, um die unterstützten Standorte für einen Ressourcentyp abzurufen:

```azurecli-interactive
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Der Befehl gibt Folgendes zurück:

```output
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Erstellen von Ressourcen-Manager-Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../templates/syntax.md). 
* Informationen zum Anzeigen der Vorlagenschemata für Ressourcenanbieter finden Sie unter [Vorlagenreferenz](/azure/templates/).
* Eine Liste, die Ressourcenanbieter zu Azure-Diensten zuordnet, finden Sie unter [Ressourcenanbieter für Azure-Dienste](azure-services-resource-providers.md).
* Informationen zum Anzeigen der Vorgänge für einen Ressourcenanbieter finden Sie unter [Azure-REST-API](/rest/api/).