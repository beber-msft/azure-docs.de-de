---
title: 'Verwenden von Verwaltungsgruppen: Azure Governance'
description: Hier erfahren Sie, wie Sie die Verwaltungsgruppenhierarchie anzeigen, verwalten, aktualisieren und löschen.
ms.date: 08/17/2021
ms.topic: conceptual
ms.openlocfilehash: 57c4af2d7c3d5d4978d500beaa6e6c8be5d12d4f
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131509395"
---
# <a name="manage-your-resources-with-management-groups"></a>Verwalten von Ressourcen mit Verwaltungsgruppen

Wenn Ihre Organisation über viele Abonnements verfügt, benötigen Sie möglicherweise eine Möglichkeit zur effizienten Verwaltung von Zugriff, Richtlinien und Konformität für diese Abonnements. Azure-Verwaltungsgruppen stellen einen abonnementübergreifenden Bereich dar. Sie organisieren Abonnements in Containern, die als „Verwaltungsgruppen“ bezeichnet werden, und wenden Ihre Governancebedingungen auf die Verwaltungsgruppen an. Alle Abonnements in einer Verwaltungsgruppe erben automatisch die auf die Verwaltungsgruppe angewendeten Bedingungen.

Verwaltungsgruppen ermöglichen Ihnen – unabhängig von den Arten Ihrer Abonnements – die unternehmenstaugliche Verwaltung in großem Umfang. Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organize your resources with Azure Management Groups](./overview.md) (Organisieren von Ressourcen mit Azure-Verwaltungsgruppen).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

> [!IMPORTANT]
> Azure Resource Manager-Benutzertoken und der Verwaltungsgruppencache gelten für 30 Minuten, bevor sie gezwungen werden, sich zu aktualisieren. Nach der Durchführung einer Aktion, wie dem Verschieben einer Verwaltungsgruppe oder eines Abonnements, kann die Anzeige bis zu 30 Minuten dauern. Um die Updates früher anzuzeigen, müssen Sie Ihr Token aktualisieren, indem Sie den Browser aktualisieren, sich an- und abmelden oder ein neues Token anfordern.

> [!IMPORTANT]
> AzManagementGroup-bezogene Az PowerShell-Cmdlets geben an, dass die **-GroupId** der Aliasname des **-GroupName**-Parameters ist, sodass wir beide verwenden können, um die Verwaltungsgruppen-ID als String-Wert zur Verfügung zu stellen. 

## <a name="change-the-name-of-a-management-group"></a>Ändern des Namens einer Verwaltungsgruppe

Der Name einer Verwaltungsgruppe kann über das Portal, mithilfe von PowerShell oder mithilfe der Azure CLI geändert werden.

### <a name="change-the-name-in-the-portal"></a>Ändern des Namens im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Wählen Sie die Verwaltungsgruppe aus, die Sie umbenennen möchten.

1. Wählen Sie **Details** aus.

1. Klicken Sie oben auf der Seite auf die Option **Gruppe umbenennen**.

   :::image type="content" source="./media/detail_action_small.png" alt-text="Screenshot: Aktionsleiste und Schaltfläche „Gruppe umbenennen“ auf der Seite „Verwaltungsgruppe“" border="false":::

1. Wenn das Menü geöffnet wird, geben Sie den neuen Namen ein, der angezeigt werden soll.

   :::image type="content" source="./media/rename_context.png" alt-text="Screenshot: Fenster „Gruppe umbenennen“ und Optionen zum Umbenennen einer Verwaltungsgruppe" border="false":::

1. Wählen Sie **Speichern** aus.

### <a name="change-the-name-in-powershell"></a>Ändern des Namens in PowerShell

Verwenden Sie zum Aktualisieren des Anzeigenamens den Befehl **Update-AzManagementGroup**. Wenn Sie beispielsweise den Anzeigenamen einer Verwaltungsgruppe von „Contoso IT“ in „Contoso Group“ ändern möchten, führen Sie den folgenden Befehl aus:

```azurepowershell-interactive
Update-AzManagementGroup -GroupId 'ContosoIt' -DisplayName 'Contoso Group'
```

### <a name="change-the-name-in-azure-cli"></a>Ändern des Namens über die Azure CLI

Verwenden Sie für die Azure CLI den Aktualisierungsbefehl.

```azurecli-interactive
az account management-group update --name 'Contoso' --display-name 'Contoso Group'
```

## <a name="delete-a-management-group"></a>Löschen einer Verwaltungsgruppe

Um eine Verwaltungsgruppe zu löschen, müssen die folgenden Anforderungen erfüllt sein:

1. Unter der Verwaltungsgruppe gibt es keine untergeordneten Verwaltungsgruppen oder Abonnements. Informationen zum Verschieben eines Abonnements oder einer Verwaltungsgruppe in eine andere Verwaltungsgruppe finden Sie unter [Verschieben von Verwaltungsgruppen und Abonnements in der Hierarchie](#moving-management-groups-and-subscriptions).

1. Sie benötigen Schreibzugriff auf die Verwaltungsgruppe (Rolle „Besitzer“, „Mitwirkender“ oder „Verwaltungsgruppenmitwirkender“). Wählen Sie zum Anzeigen der Ihnen zugewiesenen Berechtigungen die Verwaltungsgruppe aus, und klicken Sie dann auf **IAM**. Weitere Informationen zu Azure-Rollen finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../../role-based-access-control/overview.md).

### <a name="delete-in-the-portal"></a>Löschen im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Wählen Sie die zu löschende Verwaltungsgruppe aus.

1. Wählen Sie **Details** aus.

1. Wählen Sie **Löschen** aus.

   :::image type="content" source="./media/delete.png" alt-text="Screenshot: Seite „Verwaltungsgruppe“, auf der die Schaltfläche „Löschen“ hervorgehoben ist" border="false":::

   > [!TIP]
   > Wenn das Symbol abgeblendet ist, zeigen Sie mit der Maus darauf, um den Grund dafür anzuzeigen.

1. Ein Fenster wird geöffnet, in dem Sie das Löschen der Verwaltungsgruppe bestätigen müssen.

   :::image type="content" source="./media/delete_confirm.png" alt-text="Screenshot: Bestätigungsdialogfeld „Gruppe löschen“ zum Löschen einer Verwaltungsgruppe" border="false":::

1. Wählen Sie **Ja** aus.

### <a name="delete-in-powershell"></a>Löschen in PowerShell

Verwenden Sie zum Löschen von Verwaltungsgruppen in PowerShell den Befehl **Remove-AzManagementGroup**.

```azurepowershell-interactive
Remove-AzManagementGroup -GroupId 'Contoso'
```

### <a name="delete-in-azure-cli"></a>Löschen über die Azure-Befehlszeilenschnittstelle

Verwenden Sie bei der Azure CLI den Befehl „az account management-group delete“.

```azurecli-interactive
az account management-group delete --name 'Contoso'
```

## <a name="view-management-groups"></a>Anzeigen von Verwaltungsgruppen

Sie können jede Verwaltungsgruppe anzeigen, für die Sie eine direkte oder geerbte Azure-Rolle besitzen.

### <a name="view-in-the-portal"></a>Anzeigen im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Die Seite mit der Hierarchie der Verwaltungsgruppe wird geladen. Hier können Sie alle Verwaltungsgruppen und Abonnements untersuchen, auf die Sie Zugriff haben. Durch Auswahl des Gruppennamens gelangen Sie auf eine niedrigere Ebene in der Hierarchie. Die Navigation funktioniert genauso wie im Datei-Explorer.

1. Um die Details der Verwaltungsgruppe zu sehen, wählen Sie den Link **(Details)** neben dem Titel der Verwaltungsgruppe. Wenn dieser Link nicht verfügbar ist, sind Sie zum Anzeigen dieser Verwaltungsgruppe nicht berechtigt.

   :::image type="content" source="./media/main.png" alt-text="Screenshot: Seite „Verwaltungsgruppen“ mit untergeordneten Verwaltungsgruppen und Abonnements" border="false":::

### <a name="view-in-powershell"></a>Anzeigen in PowerShell

Mit dem Befehl „Get-AzManagementGroup“ rufen Sie alle Gruppen ab. Im Modul [Az.Resources](/powershell/module/az.resources/Get-AzManagementGroup) finden Sie die vollständige Liste der GET-Befehle in PowerShell für Verwaltungsgruppen.

```azurepowershell-interactive
Get-AzManagementGroup
```

Verwenden Sie den Parameter „-GroupName“, um die Informationen einer einzelnen Verwaltungsgruppe anzuzeigen.

```azurepowershell-interactive
Get-AzManagementGroup -GroupId 'Contoso'
```

Verwenden Sie die Parameter **-Expand** und **-Recurse**, um eine bestimmte Verwaltungsgruppe und die untergeordnete Hierarchie mit allen Ebenen zurückzugeben.

```azurepowershell-interactive
PS C:\> $response = Get-AzManagementGroup -GroupId TestGroupParent -Expand -Recurse
PS C:\> $response

Id                : /providers/Microsoft.Management/managementGroups/TestGroupParent
Type              : /providers/Microsoft.Management/managementGroups
Name              : TestGroupParent
TenantId          : 00000000-0000-0000-0000-000000000000
DisplayName       : TestGroupParent
UpdatedTime       : 2/1/2018 11:15:46 AM
UpdatedBy         : 00000000-0000-0000-0000-000000000000
ParentId          : /providers/Microsoft.Management/managementGroups/00000000-0000-0000-0000-000000000000
ParentName        : 00000000-0000-0000-0000-000000000000
ParentDisplayName : 00000000-0000-0000-0000-000000000000
Children          : {TestGroup1DisplayName, TestGroup2DisplayName}

PS C:\> $response.Children[0]

Type        : /managementGroup
Id          : /providers/Microsoft.Management/managementGroups/TestGroup1
Name        : TestGroup1
DisplayName : TestGroup1DisplayName
Children    : {TestRecurseChild}

PS C:\> $response.Children[0].Children[0]

Type        : /managementGroup
Id          : /providers/Microsoft.Management/managementGroups/TestRecurseChild
Name        : TestRecurseChild
DisplayName : TestRecurseChild
Children    :
```

### <a name="view-in-azure-cli"></a>Anzeigen in der Azure CLI

Verwenden Sie den Befehl „list“, um alle Gruppen abzurufen.

```azurecli-interactive
az account management-group list
```

Verwenden Sie den Befehl „show“, um die Informationen einer einzelnen Verwaltungsgruppe anzuzeigen.

```azurecli-interactive
az account management-group show --name 'Contoso'
```

Verwenden Sie die Parameter **-Expand** und **-Recurse**, um eine bestimmte Verwaltungsgruppe und die untergeordnete Hierarchie mit allen Ebenen zurückzugeben.

```azurecli-interactive
az account management-group show --name 'Contoso' -e -r
```

## <a name="moving-management-groups-and-subscriptions"></a>Verschieben von Verwaltungsgruppen und Abonnements

Ein Grund zum Erstellen einer Verwaltungsgruppe ist das Bündeln von Abonnements. Nur Verwaltungsgruppen und Abonnements können als untergeordnete Elemente einer anderen Verwaltungsgruppe festgelegt werden. Ein Abonnement, das in eine Verwaltungsgruppe verschoben wird, erbt den gesamten Benutzerzugriff und alle Richtlinien von der übergeordneten Verwaltungsgruppe.

Wenn eine Verwaltungsgruppe oder ein Abonnement verschoben und einer anderen Verwaltungsgruppe untergeordnet wird, müssen drei Regeln als TRUE ausgewertet werden.

Wenn Sie eine solche Verschiebung durchführen, müssen Ihnen auf jeder der folgenden Ebenen die entsprechenden Berechtigungen erteilt werden:

- Untergeordnetes Abonnement / Untergeordnete Verwaltungsgruppe
  - `Microsoft.management/managementgroups/write`
  - `Microsoft.management/managementgroups/subscription/write` (nur für Abonnements)
  - `Microsoft.Authorization/roleAssignments/write`
  - `Microsoft.Authorization/roleAssignments/delete`
  - `Microsoft.Management/register/action`
- Übergeordnete Zielverwaltungsgruppe
  - `Microsoft.management/managementgroups/write`
- Aktuelle übergeordnete Verwaltungsgruppe
  - `Microsoft.management/managementgroups/write`

**Ausnahme**: Wenn die Zielverwaltungsgruppe oder die vorhandene übergeordnete Verwaltungsgruppe die Stammverwaltungsgruppe ist, gelten die Berechtigungsanforderungen nicht. Da die Stammverwaltungsgruppe die standardmäßige Landing-Gruppe für alle neuen Verwaltungsgruppen und Abonnements ist, benötigen Sie keine Berechtigungen für diese Gruppe, wenn ein Element hierhin verschoben werden soll.

Wenn die Rolle „Besitzer“ in Ihrem Abonnement von der aktuellen Verwaltungsgruppe geerbt wurde, sind Ihre Verschiebeziele eingeschränkt. Sie können das Abonnement nur in eine andere Verwaltungsgruppe verschieben, für das Sie die Rolle „Besitzer“ innehaben. Sie können das Abonnement nicht in eine Verwaltungsgruppe verschieben, in der Sie nur „Mitwirkender“ sind, da Sie den Besitz an Ihrem Abonnement verlieren würden. Wenn Ihnen die Besitzerrolle für das Abonnement direkt zugewiesen wurde, können Sie es in jede Verwaltungsgruppe verschieben, bei der Sie Mitwirkender sind.

Wählen Sie zum Anzeigen Ihrer Berechtigungen im Azure-Portal die Verwaltungsgruppe und dann **IAM** aus. Weitere Informationen zu Azure-Rollen finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../../role-based-access-control/overview.md).

## <a name="move-subscriptions"></a>Verschieben von Abonnements

### <a name="add-an-existing-subscription-to-a-management-group-in-the-portal"></a>Hinzufügen eines vorhandenen Abonnements zu einer Verwaltungsgruppe im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Wählen Sie die Verwaltungsgruppe aus, die als übergeordnete Gruppe festgelegt werden soll.

1. Klicken Sie am oberen Rand der Seite auf **Abonnement hinzufügen**.

1. Wählen Sie in der Liste das Abonnement mit der richtigen ID aus.

   :::image type="content" source="./media/add_context_sub.png" alt-text="Screenshot: Optionen unter „Abonnement hinzufügen“ zur Auswahl eines vorhandenen Abonnements, das einer Verwaltungsgruppe hinzugefügt werden soll" border="false":::

1. Wählen Sie „Speichern“ aus.

### <a name="remove-a-subscription-from-a-management-group-in-the-portal"></a>Entfernen eines Abonnements aus einer Verwaltungsgruppe im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Wählen Sie die geplante Verwaltungsgruppe aus. Hierbei handelt es sich um die aktuelle übergeordnete Gruppe.

1. Klicken Sie in der Liste am Ende der Zeile des zu verschiebenden Abonnements auf die Ellipse.

   :::image type="content" source="./media/move_small.png" alt-text="Screenshot: Alternatives Menü für ein Abonnement zum Auswählen der Option „Verschieben“" border="false":::

1. Klicken Sie auf **Verschieben**.

1. Klicken Sie im angezeigten Menü auf **Übergeordnete Verwaltungsgruppe**.

   :::image type="content" source="./media/move_small_context.png" alt-text="Screenshot: Fenster „Verschieben“ und Optionen zum Verschieben eines Abonnements in eine andere Verwaltungsgruppe" border="false":::

1. Wählen Sie **Speichern** aus.

### <a name="move-subscriptions-in-powershell"></a>Verschieben von Abonnements in PowerShell

Verschieben Sie Abonnements in PowerShell mit dem Befehl „New-AzManagementGroupSubscription“.

```azurepowershell-interactive
New-AzManagementGroupSubscription -GroupId 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

Verwenden Sie zum Entfernen der Verknüpfung zwischen dem Abonnement und der Verwaltungsgruppe den Befehl „Remove-AzManagementGroupSubscription“.

```azurepowershell-interactive
Remove-AzManagementGroupSubscription -GroupId 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-azure-cli"></a>Verschieben von Abonnements in der Azure CLI

Verwenden Sie zum Verschieben eines Abonnements in der CLI den Befehl „add“.

```azurecli-interactive
az account management-group subscription add --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

Verwenden Sie zum Entfernen des Abonnements aus der Verwaltungsgruppe den Befehl „remove“.

```azurecli-interactive
az account management-group subscription remove --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-arm-template"></a>Verschieben von Abonnements in einer ARM-Vorlage

Verwenden Sie die folgende Vorlage, um ein Abonnement in eine Azure Resource Manager-Vorlage (ARM-Vorlage) zu verschieben, und stellen Sie sie auf [Mandantenebene](../../azure-resource-manager/templates/deploy-to-tenant.md) bereit.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "targetMgId": {
            "type": "string",
            "metadata": {
                "description": "Provide the ID of the management group that you want to move the subscription to."
            }
        },
        "subscriptionId": {
            "type": "string",
            "metadata": {
                "description": "Provide the ID of the existing subscription to move."
            }
        }
    },
    "resources": [
        {
            "scope": "/",
            "type": "Microsoft.Management/managementGroups/subscriptions",
            "apiVersion": "2020-05-01",
            "name": "[concat(parameters('targetMgId'), '/', parameters('subscriptionId'))]",
            "properties": {
            }
        }
    ],
    "outputs": {}
}
```

## <a name="move-management-groups"></a>Verschieben von Verwaltungsgruppen

### <a name="move-management-groups-in-the-portal"></a>Verschieben von Verwaltungsgruppen im Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie auf **Alle Dienste** > **Verwaltungsgruppen**.

1. Wählen Sie die Verwaltungsgruppe aus, die als übergeordnete Gruppe festgelegt werden soll.

1. Klicken Sie am oberen Rand der Seite auf **Verwaltungsgruppe hinzufügen**.

1. Ein Menü wird geöffnet, in dem Sie auswählen, ob Sie eine neue oder vorhandene Verwaltungsgruppe verwenden möchten.

   - Durch die Auswahl von „Neu“ wird eine neue Verwaltungsgruppe erstellt.
   - Wenn Sie eine vorhandene Verwaltungsgruppe auswählen, wird eine Dropdownliste mit allen Verwaltungsgruppen angezeigt, die Sie in diese Verwaltungsgruppe verschieben können.

   :::image type="content" source="./media/add_context_MG.png" alt-text="Screenshot: Optionen unter „Verwaltungsgruppe hinzufügen“ zum Erstellen einer neuen Verwaltungsgruppe" border="false":::

1. Wählen Sie **Speichern** aus.

### <a name="move-management-groups-in-powershell"></a>Verschieben von Verwaltungsgruppen in PowerShell

Verwenden Sie den Befehl „Update-AzureRmManagementGroup“ in PowerShell, um eine Verwaltungsgruppe in eine andere Gruppe zu verschieben.

```azurepowershell-interactive
$parentGroup = Get-AzManagementGroup -GroupId ContosoIT
Update-AzManagementGroup -GroupId 'Contoso' -ParentId $parentGroup.id
```

### <a name="move-management-groups-in-azure-cli"></a>Verschieben von Verwaltungsgruppen in der Azure CLI

Verwenden Sie den Befehl „update“, um eine Verwaltungsgruppe mit der Azure CLI zu verschieben.

```azurecli-interactive
az account management-group update --name 'Contoso' --parent ContosoIT
```

## <a name="audit-management-groups-using-activity-logs"></a>Überwachen von Verwaltungsgruppen mithilfe von Aktivitätsprotokollen

Verwaltungsgruppen werden im [Azure-Aktivitätsprotokoll](../../azure-monitor/essentials/platform-logs-overview.md) unterstützt. Alle Ereignisse, die für eine Verwaltungsgruppe auftreten, können am gleichen zentralen Ort wie bei anderen Azure-Ressourcen abgefragt werden. Sie können beispielsweise alle Änderungen an Rollen- oder Richtlinienzuweisungen anzeigen, die für eine bestimmte Verwaltungsgruppe vorgenommen wurden.

:::image type="content" source="./media/al-mg.png" alt-text="Screenshot: Aktivitätsprotokolle und -vorgänge im Zusammenhang mit der ausgewählten Verwaltungsgruppe" border="false":::

Wenn Sie Verwaltungsgruppen außerhalb des Azure-Portals abfragen möchten, sieht der Zielbereich für Verwaltungsgruppen wie folgt aus: **"/providers/Microsoft.Management/managementGroups/{yourMgID}"** .

## <a name="referencing-management-groups-from-other-resource-providers"></a>Verweisen auf Verwaltungsgruppen von anderen Ressourcenanbietern

Wenn Sie von den Aktionen eines anderen Ressourcenanbieters auf Verwaltungsgruppen verweisen, verwenden Sie den folgenden Pfad als Bereich. Dieser Pfad wird sowohl in PowerShell, als auch an der Azure-Befehlszeilenschnittstelle (CLI) und in REST-APIs verwendet.

`/providers/Microsoft.Management/managementGroups/{yourMgID}`

Das Zuweisen einer neuen Rollenzuweisung für eine Verwaltungsgruppe in PowerShell ist ein Beispiel für die Verwendung dieses Pfads:

```azurepowershell-interactive
New-AzRoleAssignment -Scope "/providers/Microsoft.Management/managementGroups/Contoso"
```

Der gleiche Bereichspfad wird verwendet, wenn Sie eine Richtliniendefinition in einer Verwaltungsgruppe abrufen.

```http
GET https://management.azure.com/providers/Microsoft.Management/managementgroups/MyManagementGroup/providers/Microsoft.Authorization/policyDefinitions/ResourceNaming?api-version=2019-09-01
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Verwaltungsgruppen finden Sie unter folgenden Links:

- [Erstellen von Verwaltungsgruppen zum Organisieren von Azure-Ressourcen](./create-management-group-portal.md)
- [Ändern, Löschen oder Verwalten Ihrer Verwaltungsgruppen](./manage.md)
- [Überprüfen von Verwaltungsgruppen im Azure PowerShell-Ressourcenmodul](/powershell/module/az.resources#resources)
- [Überprüfen von Verwaltungsgruppen in der REST-API](/rest/api/managementgroups/managementgroups)
- [Überprüfen von Verwaltungsgruppen in der Azure CLI](/cli/azure/account/management-group)
