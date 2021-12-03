---
title: Automatische Betriebssystemimageupgrades mit Azure-VM-Skalierungsgruppen
description: Es wird beschrieben, wie Sie das Betriebssystemimage auf VM-Instanzen in einer Skalierungsgruppe automatisch aktualisieren.
author: mayanknayar
ms.author: manayar
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: automatic-os-upgrade
ms.date: 07/29/2021
ms.reviewer: jushiman
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: f4b5c58eb8811db9042d92416c4c37fa30234149
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131432945"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-image-upgrades"></a>Automatische Betriebssystemimageupgrades mit Azure-VM-Skalierungsgruppen

**Gilt für**: :heavy_check_mark: Linux-VMs :heavy_check_mark: Windows VMs :heavy_check_mark: Einheitliche Skalierungsgruppen

Das Aktivieren der automatischen Betriebssystemimage-Upgrades auf Ihrer Skalierungsgruppe vereinfacht die Updateverwaltung durch sicheres und automatisches Aktualisieren des Betriebssystem-Datenträgers für alle Instanzen in der Skalierungsgruppe.

Das automatische Betriebssystemupgrade weist folgende Merkmale auf:

- Nach der Konfiguration wird das neueste von den Imageherausgebern veröffentlichte Betriebssystemimage automatisch ohne Benutzereingriff auf die Skalierungsgruppe angewendet.
- Aktualisiert Batches von Instanzen immer parallel, wenn ein neues Image vom Herausgeber veröffentlicht wird.
- Wird in Anwendungsintegritätstests und [Anwendungsintegritätserweiterung](virtual-machine-scale-sets-health-extension.md) integriert.
- Funktioniert für alle Formate von virtuellen Computern, sowohl für Windows- als auch für Linux-Images, einschließlich benutzerdefinierter Images über die [Azure Compute Gallery](../virtual-machines/shared-image-galleries.md).
- Sie können automatische Upgrades jederzeit abwählen (Betriebssystemupgrades können auch manuell initiiert werden).
- Der Betriebssystemdatenträger eines virtuellen Computers wird durch den neuen, mit der neuesten Imageversion erstellten Betriebssystemdatenträger ersetzt. Konfigurierte Erweiterungen und benutzerdefinierte Datenskripts werden ausgeführt, wobei persistente Datenträger weiterhin aufbewahrt werden.
- [Erweiterungssequenzierung](virtual-machine-scale-sets-extension-sequencing.md) wird unterstützt.
- Kann für Skalierungsgruppen beliebiger Größe aktiviert werden

## <a name="how-does-automatic-os-image-upgrade-work"></a>Wie funktioniert das automatische Upgrade von Betriebssystemimages?

Bei einem Upgrade wird der Betriebssystemdatenträger eines virtuellen Computers durch einen neuen Datenträger ersetzt, der mit der neuesten Imageversion erstellt wird. Alle konfigurierten Erweiterungen und benutzerdefinierten Datenskripts werden auf dem Betriebssystemdatenträger ausgeführt, wobei die Datenträger weiterhin aufbewahrt werden. Um die Anwendungsausfallzeiten zu minimieren, erfolgen Upgrades in Batches, wobei jeweils nicht mehr als 20% der Skalierungsgruppe aktualisiert werden.

Sie können einen Azure Load Balancer Anwendungszustandstest oder die [Anwendungszustandserweiterung](virtual-machine-scale-sets-health-extension.md) integrieren, um die Integrität der Anwendung nach einem Upgrade nachzuverfolgen. Sie sollten einen Anwendungstakt integrieren und den Erfolg des Upgrades überprüfen.

### <a name="availability-first-updates"></a>Verfügbarkeitsupdates
Das im Folgenden beschriebene Verfügbarkeitsmodell für plattformorchestrierte Updates stellt sicher, dass die Verfügbarkeitskonfigurationen in Azure für mehrere Verfügbarkeitsstufen berücksichtigt werden.

**Über Regionen hinweg:**
- Ein Update wird in Phasen global in Azure ausgeführt, um Azure-weite Bereitstellungsausfälle zu vermeiden.
- Eine „Phase“ kann sich über eine oder mehrere Regionen erstrecken, und ein Update ist nur dann Phasen übergreifend, wenn die in Betracht kommenden VMs in der vorherigen Phase erfolgreich aktualisiert wurden.
- Georegionspaare werden nicht gleichzeitig aktualisiert und können sich nicht in der gleichen Regionsphase befinden.
- Der Erfolg eines Updates wird durch die Nachverfolgung der Integrität nach dem Update einer VM gemessen.

**Innerhalb einer Region:**
- VMs in verschiedenen Verfügbarkeitszonen werden nicht gleichzeitig mit demselben Update aktualisiert.

**Innerhalb einer „Gruppe“:**
- Alle VMs in einer gemeinsamen Skalierungsgruppe werden nicht gleichzeitig aktualisiert.  
- VMs in einer gemeinsamen VM-Skalierungsgruppe werden in Batches gruppiert und innerhalb der Grenzen von Upgradedomänen aktualisiert (siehe folgende Beschreibung).

Der Prozess der plattformorchestrierten Updates wird ausgeführt, um jeden Monat unterstützte Upgrades von Betriebssystemplattformimages auszuführen. Bei benutzerdefinierten Images über Azure Compute Gallery wird ein Imageupgrade nur für eine bestimmte Azure-Region gestartet, wenn das neue Image veröffentlicht und in die Region dieser Skalierungsgruppe [repliziert](../virtual-machines/shared-image-galleries.md#replication) wird.

### <a name="upgrading-vms-in-a-scale-set"></a>Upgraden von virtuellen Computern in einer Skalierungsgruppe

Die Region einer Skalierungsgruppe ist berechtigt, Imageupgrades entweder über den verfügbarkeitsbasierten Prozess für Plattformimages oder die Replikation neuer benutzerdefinierter Imageversionen für die Shared Image Gallery zu erhalten. Das Imageupgrade wird dann wie folgt in Batches auf eine einzelne Skalierungsgruppen angewendet:
1. Bevor mit dem Upgradeprozess begonnen wird, stellt der Orchestrator sicher, dass nicht mehr als 20% der Instanzen in der gesamten Skalierungsgruppe (aus beliebigem Grund) fehlerhaft sind.
2. Der Upgradeorchestrator identifiziert den Batch zu aktualisierender VM-Instanzen, wobei ein Batch maximal 20% der gesamten Instanzenzahl aufweist, abhängig von einer minimalen Batchgröße eines virtuellen Computers. Es gibt keine Mindestanforderungen für die Skalierungsgruppengröße, und Skalierungsgruppen mit mindestens 5 Instanzen verfügen über 1 VM pro Upgradebatch (minimale Batchgröße).
3. Der Betriebssystemdatenträger jeder VM im ausgewählten Upgradebatch wird durch einen neuen Betriebssystemdatenträger ersetzt, der aus dem aktuellen Image erstellt wurde. Alle angegebenen Erweiterungen und Konfigurationen im Skalierungsgruppenmodell werden auf die aktualisierte Instanz angewendet.
4. Für Skalierungsgruppen mit konfigurierten Anwendungsintegritätstests oder Anwendungsintegritätserweiterung wartet das Upgrade bis zu 5 Minuten darauf, dass die Instanz fehlerfrei wird, bevor mit der Aktualisierung des nächsten Batches fortgefahren wird. Wenn für eine Instanz die Integrität innerhalb von fünf Minuten nach einem Upgrade nicht wiederhergestellt werden kann, wird standardmäßig der vorherige Betriebssystemdatenträger für die Instanz wiederhergestellt.
5. Der Upgradeorchestrator verfolgt auch den Prozentsatz der Instanzen nach, die nach einem Upgrade fehlerhaft werden. Das Upgrade wird beendet, wenn mehr als 20% der aktualisierten Instanzen während des Upgradeprozesses fehlerhaft werden.
6. Das oben beschriebene Verfahren wird fortgesetzt, bis alle Instanzen in der Skalierungsgruppe aktualisiert sind.

Der Upgradeorchestrator für das Skalierungsgruppen-Betriebssystem überprüft die allgemeine Skalierungsgruppenintegrität, bevor die einzelnen Batches aktualisiert werden. Beim Aktualisieren eines Batches finden eventuell andere gleichzeitige geplante oder nicht geplante Wartungsaktivitäten statt, die die Integrität Ihrer Skalierungsgruppeninstanzen beeinträchtigen könnten. Wenn in solchen Fällen mehr als 20% der Instanzen der Skalierungsgruppe fehlerhaft werden, endet das Upgrade der Skalierungsgruppe am Ende des aktuellen Batches.

> [!NOTE]
>Das automatische Betriebssystemupgrade führt kein Upgrade der Referenzimage-SKU für die Skalierungsgruppe durch. Um die SKU zu ändern (z. B. Ubuntu 16.04-LTS auf 18.04-LTS), müssen Sie das [Skalierungsgruppenmodell](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model) direkt mit der gewünschten Image-SKU aktualisieren. Imageherausgeber und Angebot können bei einer vorhandenen Skalierungsgruppe nicht geändert werden.  

## <a name="supported-os-images"></a>Unterstützte Betriebssystemimages
Derzeit werden nur bestimmte Betriebssystemplattform-Images unterstützt. Benutzerdefinierte Images [werden unterstützt](virtual-machine-scale-sets-automatic-upgrade.md#automatic-os-image-upgrade-for-custom-images), wenn die Skalierungsgruppe benutzerdefinierte Images über [Azure Compute Gallery](../virtual-machines/shared-image-galleries.md) nutzt.

Derzeit werden die folgenden Plattform-SKUs unterstützt (weitere werden regelmäßig hinzugefügt):

| Herausgeber               | Betriebssystemangebot      |  Sku               |
|-------------------------|---------------|--------------------|
| Canonical               | UbuntuServer  | 16.04-LTS          |
| Canonical               | UbuntuServer  | 18.04-LTS          |
| OpenLogic               | CentOS        | 7,5                |
| MicrosoftWindowsServer  | Windows Server | 2012-R2-Datacenter |
| MicrosoftWindowsServer  | Windows Server | 2016-Datacenter    |
| MicrosoftWindowsServer  | Windows Server | 2016-Datacenter-smalldisk |
| MicrosoftWindowsServer  | Windows Server | 2016-Datacenter-with-Containers |
| MicrosoftWindowsServer  | Windows Server | 2019-Datacenter |
| MicrosoftWindowsServer  | Windows Server | 2019-Datacenter-smalldisk |
| MicrosoftWindowsServer  | Windows Server | 2019-Datacenter-with-Containers |
| MicrosoftWindowsServer  | Windows Server | 2019-Datacenter-Core |
| MicrosoftWindowsServer  | Windows Server | 2019-Datacenter-Core-with-Containers |
| MicrosoftWindowsServer  | Windows Server | 2019-Datacenter-gensecond |


## <a name="requirements-for-configuring-automatic-os-image-upgrade"></a>Anforderungen für das Konfigurieren des automatischen Upgrades von Betriebssystemimages

- Die *version*-Eigenschaft des Images muss auf *latest* festgelegt werden.
- Verwenden Sie Anwendungszustandstest oder die [Application Health-Erweiterung](virtual-machine-scale-sets-health-extension.md) für Nicht-Service Fabric-Skalierungsgruppen oder Service Fabric-Skalierungsgruppen mit Bronze-Dauerhaftigkeit mit zustandslosen Knotentypen.
- Verwenden Sie Compute-API-Version 2018-10-01 oder höher.
- Stellen Sie sicher, dass im Skalierungsgruppenmodell angegebene externe Ressourcen verfügbar und aktualisiert sind. Zu den Beispielen zählen SAS-URI für die Bootstrap-Nutzlast in VM-Erweiterungseigenschaften, Nutzlast im Speicherkonto, Verweis auf Geheimnisse im Modell und Sonstiges.
- Für Skalierungsgruppen mit Verwendung von virtuellen Windows-Computern ab Compute-API-Version 2019-03-01 muss die *virtualMachineProfile.osProfile.windowsConfiguration.enableAutomaticUpdates*-Eigenschaft in der Skalierungsgruppenmodell-Definition auf *false* festgelegt werden. Die Eigenschaft *enableAutomaticUpdates* ermöglicht Patching auf einem virtuellen Computer, bei denen „Windows Update“ Betriebssystempatches anwendet, ohne den Betriebssystemdatenträger zu ersetzen. Wenn für Ihre Skalierungsgruppe automatische Upgrades von Betriebssystemimages aktiviert sind, ist kein zusätzlicher Patchingprozess per Windows Update erforderlich.

### <a name="service-fabric-requirements"></a>Service Fabric-Anforderungen

Stellen Sie bei Verwendung von Service Fabric sicher, dass die folgenden Bedingungen erfüllt sind:
-   Die [Dauerhaftigkeitsstufe](../service-fabric/service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster) von Service Fabric lautet „Silber“ oder „Gold“ und nicht „Bronze“ (mit Ausnahme zustandsloser Knotentypen, die automatische Betriebssystemupgrades unterstützen).
-   Die Service Fabric-Erweiterung in der Skalierungsgruppenmodell-Definition muss über TypeHandlerVersion 1.1 oder höher verfügen.
-   Die Dauerhaftigkeitsstufe sollte im Service Fabric-Cluster und für die Service Fabric-Erweiterung in der Skalierungsgruppenmodell-Definition gleich sein.
- Ein zusätzlicher Integritätstest oder die Verwendung einer Anwendungsintegritätserweiterung ist für Silber- oder Gold-Dauerhaftigkeit nicht erforderlich. Bronze-Dauerhaftigkeit mit zustandslosen Knotentypen erfordert einen zusätzlichen Integritätstest.
- Die Eigenschaft *virtualMachineProfile.osProfile.windowsConfiguration.enableAutomaticUpdates* muss in der Skalierungsgruppenmodell-Definition auf *false* festgelegt werden. Die Eigenschaft *enableAutomaticUpdates* ermöglicht das Patchen in virtuellen VMs mithilfe von „Windows Update“, und wird für Service Fabric-Skalierungsets nicht unterstützt.

Stellen Sie sicher, dass die Dauerhaftigkeitseinstellungen im Service Fabric-Cluster und für die Service Fabric-Erweiterung nicht in Konflikt stehen, weil dies zu Upgradefehlern führt. Dauerhaftigkeitsstufen können mit den Richtlinien geändert werden, die auf [dieser Seite](../service-fabric/service-fabric-cluster-capacity.md#changing-durability-levels) beschrieben sind.


## <a name="automatic-os-image-upgrade-for-custom-images"></a>Automatisches Upgrade von Betriebssystemimages für benutzerdefinierte Images

Das automatische Upgrade von Betriebssystemimages wird für benutzerdefinierte Images unterstützt, die über [Azure Compute Gallery](../virtual-machines/shared-image-galleries.md) bereitgestellt wurden. Andere benutzerdefinierte Images werden für automatische Upgrades von Betriebssystemimages nicht unterstützt.

### <a name="additional-requirements-for-custom-images"></a>Weitere Anforderungen für benutzerdefinierte Images
- Der Einrichtungs- und Konfigurationsprozess für das automatische Upgrade von Betriebssystemimages ist für alle Skalierungsgruppen identisch (siehe Beschreibung im [Konfigurationsabschnitt](virtual-machine-scale-sets-automatic-upgrade.md#configure-automatic-os-image-upgrade) dieser Seite).
- Skalierungsgruppeninstanzen, die für automatische Upgrades von Betriebssystemimages konfiguriert sind, werden auf die neueste Version des Azure-Compute-Gallery-Images aktualisiert, wenn eine neue Version des Images veröffentlicht und in die Region der dieser Skalierungsgruppe [repliziert](../virtual-machines/shared-image-galleries.md#replication) wird. Wenn das neue Image nicht in der Region repliziert wird, in der die Skalierung bereitgestellt wird, erfolgt kein Upgrade der Skalierungsgruppeninstanzen auf die neueste Version. Die regionale Imagereplikation ermöglicht es Ihnen, das Rollout des neuen Images für Ihre Skalierungsgruppen zu steuern.
- Die neue Imageversion sollte nicht aus der aktuellen Version für das betreffende Katalogimage ausgeschlossen werden. Für Imageversionen, die aus der aktuellen Version des Katalogimages ausgeschlossen werden, erfolgt durch das automatische Upgrade von Betriebssystemimages kein Rollout in der Skalierungsgruppe.

> [!NOTE]
>Es kann bis zu drei Stunden dauern, bis eine Skalierungsgruppe den ersten Rollout für das Imageupgrade auslöst, nachdem sie erstmalig für automatische Betriebssystemupgrades konfiguriert wurde. Dies ist eine einmalige Verzögerung pro Skalierungsgruppe. Nachfolgende Imagerollouts werden für die Skalierungsgruppe innerhalb von 30–60 Minuten ausgelöst.


## <a name="configure-automatic-os-image-upgrade"></a>Konfigurieren des automatischen Upgrades von Betriebssystemimages
Stellen Sie zum Konfigurieren des automatischen Upgrades von Betriebssystemimages sicher, dass die Eigenschaft *automaticOSUpgradePolicy.enableAutomaticOSUpgrade* in der Definition des Skalierungsgruppenmodells auf *true* festgelegt ist.

### <a name="rest-api"></a>REST-API
Im folgenden Beispiel wird beschrieben, wie automatische Betriebssystemupgrades auf einem Skalierungsgruppenmodell festgelegt werden:

```
PUT or PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version=2021-03-01`
```

```json
{
  "properties": {
    "upgradePolicy": {
      "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade":  true
      }
    }
  }
}
```

### <a name="azure-powershell"></a>Azure PowerShell
Verwenden Sie das Cmdlet [Update-AzVmss](/powershell/module/az.compute/update-azvmss), um automatische Betriebssystem-Imageupgrades für Ihre Skalierungsgruppe zu konfigurieren. Im folgenden Beispiel werden automatische Upgrades für die Skalierungsgruppe *myScaleSet* in der Ressourcengruppe *myResourceGroup* konfiguriert:

```azurepowershell-interactive
Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -AutomaticOSUpgrade $true
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Verwenden Sie [az vmss update](/cli/azure/vmss#az_vmss_update), um automatische Betriebssystem-Imageupgrades für Ihre Skalierungsgruppe zu konfigurieren. Verwenden Sie Azure CLI 2.0.47 oder höher. Im folgenden Beispiel werden automatische Upgrades für die Skalierungsgruppe *myScaleSet* in der Ressourcengruppe *myResourceGroup* konfiguriert:

```azurecli-interactive
az vmss update --name myScaleSet --resource-group myResourceGroup --set UpgradePolicy.AutomaticOSUpgradePolicy.EnableAutomaticOSUpgrade=true
```

> [!NOTE]
>Nach dem Konfigurieren automatischer Betriebssystem-Imageupgrades für Ihre Skalierungsgruppe müssen Sie die Skalierungsgruppen-VMs auch auf das aktuelle Skalierungsgruppenmodell aktualisieren, wenn Ihre Skalierungsgruppe die [Upgraderichtlinie](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) „Manuell“ verwendet.

## <a name="using-application-health-probes"></a>Verwenden von Anwendungsintegritätstests

Während eines Betriebssystemupgrades werden VM-Instanzen in einer Skalierungsgruppe batchweise nacheinander aktualisiert. Das Upgrade sollte nur fortgesetzt werden, wenn die Kundenanwendung auf den aktualisierten VM-Instanzen fehlerfrei ist. Es wird empfohlen, dass die Anwendung der Upgrade-Engine für das Skalierungsgruppen-Betriebssystem Integritätssignale zur Verfügung stellt. Standardmäßig wertet die Plattform während Betriebssystemupgrades den VM-Energiezustand und den Status der Erweiterungsbereitstellung aus, um festzustellen, ob eine VM-Instanz nach einem Upgrade fehlerfrei ist. Während des Betriebssystemupgrades einer VM-Instanz wird ihr Betriebssystemdatenträger durch einen neuen Datenträger ersetzt, der auf der neuesten Imageversion basiert. Nachdem das Betriebssystemupgrade abgeschlossen wurde, werden die konfigurierten Erweiterungen auf diesen VMs ausgeführt. Die Anwendung wird erst dann als fehlerfrei angesehen, wenn alle Erweiterungen erfolgreich auf der Instanz bereitgestellt wurden.

Eine Skalierungsgruppe kann optional mit Anwendungsintegritätstests konfiguriert werden, um der Plattform genaue Informationen zum fortlaufenden Status der Anwendung bereitzustellen. Anwendungsintegritätstests sind benutzerdefinierte Lastenausgleichs-Prüfpunkte, die als Integritätssignal verwendet werden. Die auf einer Skalierungsgruppen-VM-Instanz ausgeführte Anwendung kann auf externe HTTP- oder TCP-Anforderungen reagieren und dadurch anzeigen, dass sie fehlerfrei ist. Weitere Informationen zur Funktionsweise von benutzerdefinierten Lastenausgleichstests finden Sie unter [Grundlegendes zu Lastenausgleichstests](../load-balancer/load-balancer-custom-probe-overview.md). Anwendungsintegritätstests werden für Service Fabric-Skalierungsgruppen nicht unterstützt. Nicht-Service Fabric-Skalierungsgruppen erfordern entweder Load Balancer-Anwendungsintegritätstests oder eine [Anwendungsintegritätserweiterung](virtual-machine-scale-sets-health-extension.md).

Wenn die Skalierungsgruppe für die Verwendung mehrerer Platzierungsgruppen konfiguriert ist, müssen Tests mit einem [Standardlastenausgleich](../load-balancer/load-balancer-overview.md) verwendet werden.

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Konfigurieren eines benutzerdefinierten Lastenausgleichstests als Anwendungsintegritätstest für eine Skalierungsgruppe
Erstellen Sie als bewährte Methode einen Lastenausgleichstest explizit für die Skalierungsgruppenintegrität. Sie können denselben Endpunkt für einen vorhandenen HTTP-Test oder TCP-Test verwenden. Für einen Integritätstest ist jedoch möglicherweise ein anderes Verhalten als bei einem herkömmlichen Lastenausgleichstest erforderlich. Beispielsweise könnte ein herkömmlicher Lastenausgleichstest den Status „Fehlerhaft“ zurückgeben, wenn die Last der Instanz zu hoch ist, aber zum Ermitteln der Instanzintegrität bei einem automatischen Betriebssystemupgrade wäre dies nicht angemessen. Konfigurieren Sie den Test so, dass eine hohe Stichprobenrate von unter zwei Minuten gilt.

Der Lastenausgleichstest kann im *networkProfile* der Skalierungsgruppe referenziert werden und wie folgt einem internen oder einem öffentlich zugänglichen Lastenausgleich zugeordnet werden:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
}
```

> [!NOTE]
> Wenn Sie automatische Betriebssystemupgrades mit Service Fabric verwenden, wird das neue Betriebssystemimage nacheinander in einzelnen Updatedomänen bereitgestellt. So wird gewährleistet, dass die in Service Fabric ausgeführten Dienste hochverfügbar bleiben. Um automatische Betriebssystemupgrades in Service Fabric nutzen zu können, muss Ihr Clusterknotentyp zur Verwendung der Dauerhaftigkeitsstufe „Silber“ oder höher konfiguriert sein. Für die Dauerhaftigkeitsstufe „Bronze“ wird ein automatisches Betriebssystemupgrade nur für zustandslose Knotentypen unterstützt. Weitere Informationen zu den Dauerhaftigkeitsmerkmalen von Service Fabric-Clustern finden Sie in [dieser Dokumentation](../service-fabric/service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster).

### <a name="keep-credentials-up-to-date"></a>Ihre Anmeldeinformationen müssen aktuell sein
Wenn Ihre Skalierungsgruppe Anmeldeinformationen zum Zugreifen auf externe Ressourcen verwendet – z. B. eine VM-Erweiterung, die für die Verwendung eines SAS-Tokens für das Speicherkonto konfiguriert wurde –, sollten Sie sicherstellen, dass die Anmeldeinformationen aktuell sind. Wenn die verwendeten Anmeldeinformationen (Zertifikate und Token eingeschlossen) abgelaufen sind, kommt es beim Upgrade zu einem Fehler, und der erste VM-Batch wechselt in einen Fehlerzustand.

Führen Sie die folgenden Schritte aus, um nach einem Fehler bei der Ressourcenauthentifizierung die VMs wiederherzustellen und automatische Betriebssystemupgrades erneut zu aktivieren:

* Generieren Sie das Token (oder andere Anmeldeinformationen) neu, die an Ihre Erweiterungen übergeben werden.
* Stellen Sie sicher, dass in der VM verwendete Anmeldeinformationen zur Kommunikation mit externen Entitäten aktuell sind.
* Aktualisieren Sie Erweiterungen im Skalierungsgruppenmodell mit den neuen Token.
* Stellen Sie die aktualisierte Skalierungsgruppe bereit, um alle VM-Instanzen – einschließlich der fehlerhaften – zu aktualisieren.

## <a name="using-application-health-extension"></a>Verwenden der Anwendungsintegritätserweiterung
Die Application Health-Erweiterung wird in einer VM-Skalierungsgruppeninstanz bereitgestellt und berichtet von der Skalierungsgruppeninstanz aus über die VM-Integrität. Sie können die Erweiterung konfigurieren, um einen Anwendungsendpunkt zu prüfen und den Status der Anwendung in dieser Instanz zu aktualisieren. Dieser Instanzstatus wird von Azure geprüft, um festzustellen, ob eine Instanz für Upgradevorgänge geeignet ist.

Da die Erweiterung von einem virtuellen Computer aus über die Integrität berichtet, kann die Erweiterung in Situationen verwendet werden, in denen externe Tests wie z.B. Anwendungsintegritätstests (die benutzerdefinierte Azure Load Balancer-[Tests](../load-balancer/load-balancer-custom-probe-overview.md) verwenden) nicht genutzt werden können.

Es gibt mehrere Möglichkeiten, die Application Health-Erweiterung für Ihre Skalierungsgruppen bereitzustellen, wie in den Beispielen in [diesem Artikel](virtual-machine-scale-sets-health-extension.md#deploy-the-application-health-extension) beschrieben.

## <a name="get-the-history-of-automatic-os-image-upgrades"></a>Abrufen des Verlaufs automatischer Betriebssystemimageupgrades
Sie können den Verlauf des letzten für Ihre Skalierungsgruppe durchgeführten Betriebssystemupgrades mit Azure PowerShell, Azure CLI 2.0 oder der REST-API überprüfen. Sie können den Verlauf für die letzten fünf Betriebssystemupgradeversuche innerhalb der vergangenen zwei Monate abrufen.

### <a name="rest-api"></a>REST-API
Im folgenden Beispiel wird der Status der Skalierungsgruppe *myScaleSet* in der Ressourcengruppe *myResourceGroup* mit der [REST-API](/rest/api/compute/virtualmachinescalesets/getosupgradehistory) überprüft:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osUpgradeHistory?api-version=2021-03-01`
```

Der GET-Aufruf gibt Eigenschaften ähnlich der folgenden Beispielausgabe zurück:

```json
{
    "value": [
        {
            "properties": {
        "runningStatus": {
          "code": "RollingForward",
          "startTime": "2018-07-24T17:46:06.1248429+00:00",
          "completedTime": "2018-04-21T12:29:25.0511245+00:00"
        },
        "progress": {
          "successfulInstanceCount": 16,
          "failedInstanceCount": 0,
          "inProgressInstanceCount": 4,
          "pendingInstanceCount": 0
        },
        "startedBy": "Platform",
        "targetImageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2016-Datacenter",
          "version": "2016.127.20180613"
        },
        "rollbackInfo": {
          "successfullyRolledbackInstanceCount": 0,
          "failedRolledbackInstanceCount": 0
        }
      },
      "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
      "location": "westeurope"
    }
  ]
}
```

### <a name="azure-powershell"></a>Azure PowerShell
Überprüfen Sie mit dem [Get-AzVmss](/powershell/module/az.compute/get-azvmss)-Cmdlet den Verlauf des Betriebssystemupgrades für Ihre Skalierungsgruppe. Das folgende Beispiel zeigt, wie der Status des Betriebssystemupgrades für eine Skalierungsgruppe mit dem Namen *myScaleSet* in der Ressourcengruppe *myResourceGroup* überprüft wird:

```azurepowershell-interactive
Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -OSUpgradeHistory
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Überprüfen Sie mit [az vmss get-os-upgrade-history](/cli/azure/vmss#az_vmss_get_os_upgrade_history) den Verlauf des Betriebssystemupgrades für Ihre Skalierungsgruppe. Verwenden Sie Azure CLI 2.0.47 oder höher. Das folgende Beispiel zeigt, wie der Status des Betriebssystemupgrades für eine Skalierungsgruppe mit dem Namen *myScaleSet* in der Ressourcengruppe *myResourceGroup* überprüft wird:

```azurecli-interactive
az vmss get-os-upgrade-history --resource-group myResourceGroup --name myScaleSet
```

## <a name="how-to-get-the-latest-version-of-a-platform-os-image"></a>Wie erhalte ich die neueste Version eines Plattformimages?

Sie können die verfügbaren Imageversionen für automatische Betriebssystemupgrades von unterstützten SKUs mithilfe der folgenden Beispiele erhalten:

### <a name="rest-api"></a>REST-API
```
GET on `/subscriptions/subscription_id/providers/Microsoft.Compute/locations/{location}/publishers/{publisherName}/artifacttypes/vmimage/offers/{offer}/skus/{skus}/versions?api-version=2021-03-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
```azurepowershell-interactive
Get-AzVmImage -Location "westus" -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
```azurecli-interactive
az vm image list --location "westus" --publisher "Canonical" --offer "UbuntuServer" --sku "16.04-LTS" --all
```

## <a name="manually-trigger-os-image-upgrades"></a>Manuelles Auslösen von Upgrades des Betriebssystemimages
Wenn automatische Upgrades von Betriebssystemimages für Ihre Skalierungsgruppe aktiviert sind, müssen Sie die Imageupdates in Ihrer Skalierungsgruppe nicht manuell auslösen. Der Orchestrator für Betriebssystemupgrades wendet ohne manuellen Eingriff automatisch die neueste verfügbare Imageversion auf Ihre Skalierungsgruppeninstanzen an.

In bestimmten Fällen, in denen Sie nicht auf das Anwenden des aktuellen Images durch den Orchestrator warten möchten, können Sie manuell ein Upgrade des Betriebssystemimages auslösen, indem Sie die unten angegebenen Beispiele verwenden.

> [!NOTE]
> Die manuelle Auslösung von Upgrades des Betriebssystemimages verfügt nicht über Funktionen für den automatischen Rollback. Wenn eine Instanz nach einem Upgradevorgang die Integrität nicht wiedererlangt, kann der vorherige Betriebssystemdatenträger nicht wiederhergestellt werden.

### <a name="rest-api"></a>REST-API
Verwenden Sie den API-Aufruf zum [Starten eines Betriebssystemupgrades](/rest/api/compute/virtualmachinescalesetrollingupgrades/startosupgrade), um ein paralleles Upgrade zu starten, bei dem alle VM-Skalierungsgruppeninstanzen auf die neueste verfügbare Imagebetriebssystemversion aktualisiert werden. Instanzen, auf denen bereits die neueste verfügbare Betriebssystemversion ausgeführt wird, sind nicht betroffen. Das folgende Beispiel zeigt, wie Sie ein paralleles Betriebssystemupgrade für eine Skalierungsgruppe mit dem Namen *myScaleSet* in der Ressourcengruppe *myResourceGroup* starten können:

```
POST on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osRollingUpgrade?api-version=2021-03-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
Verwenden Sie das Cmdlet [Start-AzVmssRollingOSUpgrade](/powershell/module/az.compute/Start-AzVmssRollingOSUpgrade), um den Verlauf der Betriebssystemupgrades für Ihre Skalierungsgruppe zu überprüfen. Das folgende Beispiel zeigt, wie Sie ein paralleles Betriebssystemupgrade für eine Skalierungsgruppe mit dem Namen *myScaleSet* in der Ressourcengruppe *myResourceGroup* starten können:

```azurepowershell-interactive
Start-AzVmssRollingOSUpgrade -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Überprüfen Sie mit [az vmss rolling-upgrade start](/cli/azure/vmss/rolling-upgrade#az_vmss_rolling_upgrade_start) den Verlauf des Betriebssystemupgrades für Ihre Skalierungsgruppe. Verwenden Sie Azure CLI 2.0.47 oder höher. Das folgende Beispiel zeigt, wie Sie ein paralleles Betriebssystemupgrade für eine Skalierungsgruppe mit dem Namen *myScaleSet* in der Ressourcengruppe *myResourceGroup* starten können:

```azurecli-interactive
az vmss rolling-upgrade start --resource-group "myResourceGroup" --name "myScaleSet" --subscription "subscriptionId"
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Weitere Informationen zur Anwendungsintegritätserweiterung](../virtual-machine-scale-sets/virtual-machine-scale-sets-health-extension.md)
