---
title: Erstellen eines Azure Service Bus-Namespacethemas mithilfe einer Vorlage
description: 'Schnellstart: Erstellen eines Service Bus-Namespace mit einem Thema und einem Abonnement anhand einer Azure Resource Manager-Vorlage'
documentationcenter: .net
author: spelluru
ms.author: spelluru
ms.date: 09/27/2021
ms.topic: quickstart
ms.tgt_pltfrm: dotnet
ms.custom: devx-track-azurepowershell, mode-arm
ms.openlocfilehash: 2f274f1d179e4e59f83f867b282c8aa96b8aaeab
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131054987"
---
# <a name="quickstart-create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a>Schnellstart: Erstellen eines Service Bus-Namespace mit einem Thema und einem Abonnement anhand einer Azure Resource Manager-Vorlage

In diesem Artikel wird beschrieben, wie Sie eine Azure Resource Manager-Vorlage verwenden, die einen Service Bus-Namespace und ein Thema sowie ein Abonnement innerhalb dieses Namespace erstellt. Dieser Artikel zeigt Ihnen, wie Sie angeben, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder Ihren Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen][Authoring Azure Resource Manager templates].

Die vollständige Vorlage finden Sie unter [Service Bus namespace with topic and subscription][Service Bus namespace with topic and subscription] (Service Bus-Namespace mit Thema und Abonnement).

> [!NOTE]
> Die folgenden Azure Resource Manager-Vorlagen sind zum Download und zur Bereitstellung verfügbar.
>
> * [Erstellen eines Service Bus-Namespaces](service-bus-resource-manager-namespace.md)
> * [Create a Service Bus namespace and a queue using an Azure Resource Manager template](service-bus-resource-manager-namespace-queue.md)
> * [Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template](service-bus-resource-manager-namespace-auth-rule.md)
> * [Create a Service Bus namespace with topic, subscription, and rule (Erstellen eines Service Bus-Namespace mit Thema, Abonnement und Regel)](service-bus-resource-manager-namespace-topic-with-rule.md)
>
> Die neuesten Vorlagen finden Sie, indem Sie im Katalog [Azure-Schnellstartvorlagen][Azure Quickstart Templates] nach **Service Bus** suchen.

## <a name="what-do-you-deploy"></a>Was stellen Sie bereit?

Mit dieser Vorlage können Sie einen Service Bus-Namespace mit Thema und Abonnement bereitstellen.

[Service Bus-Themen und -Abonnements](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) bieten eine 1:n-Kommunikation in einem Muster vom Typ *Veröffentlichen/Abonnieren*.

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![In Azure bereitstellen](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.servicebus%2Fservicebus-create-topic-and-subscription%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure Resource Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters` , der alle Parameterwerte enthält. Definieren Sie einen Parameter für die Werte, die basierend auf dem bereitgestellten Projekt oder der bereitgestellten Umgebung variieren. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert wird in der Vorlage verwendet, um die bereitgestellten Ressourcen zu definieren.

Die Vorlage definiert die folgenden Parameter:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Der Name des zu erstellenden Service Bus-Namespace.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

Der Name des Themas, das im Service Bus-Namespace erstellt wird.

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

Der Name des Abonnements, das im Service Bus-Namespace erstellt wird.

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Die Service Bus-API-Version der Vorlage.

```json
"serviceBusApiVersion": {
       "type": "string",
       "defaultValue": "2017-04-01",
       "metadata": {
           "description": "Service Bus ApiVersion used by the template"
       }
```

## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen

Erstellt einen standardmäßigen Service Bus-Namespace des Typs **Messaging** mit Thema und Abonnement.

```json
"resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "Standard",
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

JSON-Syntax und Eigenschaften finden Sie unter [Namespaces](/azure/templates/microsoft.servicebus/namespaces), [Themen](/azure/templates/microsoft.servicebus/namespaces/topics) und [Abonnements](/azure/templates/microsoft.servicebus/namespaces/topics/subscriptions).

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```powershell-interactive
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/quickstarts/microsoft.servicebus/servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az deployment group create \<my-resource-group\> --name \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/quickstarts/microsoft.servicebus/servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun Ressourcen mit Azure Resource Manager erstellt und bereitgestellt haben, lesen Sie die folgenden Artikel, und erfahren Sie, wie Sie diese Ressourcen verwalten können:

* [Verwalten von Service Bus mit PowerShell](service-bus-manage-with-ps.md)
* [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/templates/syntax.md
[Azure Quickstart Templates]: https://azure.microsoft.com/resources/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/management/manage-resources-powershell.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/management/manage-resources-cli.md
[Service Bus namespace with topic and subscription]: https://azure.microsoft.com/resources/templates/servicebus-create-topic-and-subscription/
