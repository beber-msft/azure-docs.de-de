---
title: 'Schnellstart: Erstellen einer Registrierung – PowerShell'
description: Hier erfahren Sie, wie Sie schnell eine private Docker-Registrierung über PowerShell in Azure Container Registry erstellen.
ms.date: 06/03/2021
ms.topic: quickstart
ms.custom: devx-track-azurepowershell, mvc, mode-api
ms.openlocfilehash: 448b692adb603c884f1034bf5d7234e459ce5f66
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131043488"
---
# <a name="quickstart-create-a-private-container-registry-using-azure-powershell"></a>Schnellstart: Erstellen einer privaten Containerregistrierung mit Azure PowerShell

Azure Container Registry ist ein privater Registrierungsdienst zum Erstellen, Speichern und Verwalten von Containerimages und verwandten Artefakten. In dieser Schnellstartanleitung erstellen Sie eine Azure Container Registry-Instanz über Azure PowerShell. Übertragen Sie anschließend mithilfe von Docker-Befehlen ein Containerimage per Push in die Registrierung. Rufen Sie abschließend das Image per Pull aus der Registrierung ab, und führen Sie es aus.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Für diese Schnellstartanleitung ist das Azure PowerShell-Modul erforderlich. Führen Sie `Get-Module -ListAvailable Az` aus, um Ihre installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-az-ps) (Installieren des Azure PowerShell-Moduls) Informationen dazu.

Darüber hinaus muss Docker lokal installiert sein. Docker stellt Pakete für [macOS][docker-mac]-, [Windows][docker-windows]- und [Linux][docker-linux]-Systeme bereit.

Da Azure Cloud Shell nicht alle erforderlichen Docker-Komponenten (z.B. den `dockerd`-Daemon) enthält, können Sie Cloud Shell für diesem Schnellstart nicht verwenden.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich mit dem Befehl [Connect-AzAccount][Connect-AzAccount] bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Connect-AzAccount
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie nach Abschluss der Authentifizierung bei Azure mit [New-AzResourceGroup][New-AzResourceGroup] eine Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Sie Azure-Ressourcen bereitstellen und verwalten.

```powershell
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-container-registry"></a>Erstellen einer Containerregistrierung

Erstellen Sie nun mit dem Befehl [New-AzContainerRegistry][New-AzContainerRegistry] eine Containerregistrierung in Ihrer neuen Ressourcengruppe.

Der Registrierungsname muss innerhalb von Azure eindeutig sein und zwischen 5 und 50 alphanumerische Zeichen enthalten. Das folgende Beispiel erstellt eine Registrierung mit dem Namen „myContainerRegistry007“. Ersetzen Sie *myContainerRegistry007* im folgenden Befehl, und führen Sie diesen dann zum Erstellen der Registrierung aus:

```powershell
$registry = New-AzContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

[!INCLUDE [container-registry-quickstart-sku](../../includes/container-registry-quickstart-sku.md)]

## <a name="log-in-to-registry"></a>Anmelden bei der Registrierung

Bevor Sie Push- und Pullvorgänge für Containerimages ausführen können, müssen Sie sich mit dem Cmdlet [Connect-AzContainerRegistry][connect-azcontainerregistry] bei der Registrierung anmelden. Im folgenden Beispiel werden die gleichen Anmeldeinformationen verwendet, mit denen Sie sich bei der Authentifizierung bei Azure mit dem Cmdlet `Connect-AzAccount` angemeldet haben.

> [!NOTE]
> Im folgenden Beispiel ist der Wert von `$registry.Name` der Ressourcenname, nicht der vollqualifizierte Registrierungsname.

```powershell
Connect-AzContainerRegistry -Name $registry.Name
```

Der Befehl gibt nach Abschluss des Vorgangs `Login Succeeded` zurück.

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die in dieser Schnellstartanleitung erstellten Ressourcen nicht mehr benötigen, verwenden Sie den Befehl [Remove-AzResourceGroup][Remove-AzResourceGroup], um die Ressourcengruppe, die Containerregistrierung und die darin gespeicherten Containerimages zu entfernen:

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie mit Azure PowerShell eine Azure Container Registry-Instanz erstellt, ein Containerimage per Push übertragen und das Image per Pull aus der Registrierung abgerufen und ausgeführt. Fahren Sie mit den Azure Container Registry-Tutorials fort, um eingehendere Informationen zu ACR zu erhalten.

> [!div class="nextstepaction"]
> [Tutorials zu Azure Container Registry][container-registry-tutorial-prepare-registry]

> [!div class="nextstepaction"]
> [Tutorials zu Azure Container Registry Tasks][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzAccount]: /powershell/module/az.accounts/connect-azaccount
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzContainerRegistry]: /powershell/module/az.containerregistry/New-AzContainerRegistry
[New-AzResourceGroup]: /powershell/module/az.resources/new-azresourcegroup
[Remove-AzResourceGroup]: /powershell/module/az.resources/remove-azresourcegroup
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[container-registry-tutorial-prepare-registry]: container-registry-tutorial-prepare-registry.md
[connect-azcontainerregistry]: /powershell/module/az.containerregistry/connect-azcontainerregistry
