---
title: Konfigurieren von Azure CNI-Netzwerken in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Azure CNI-Netzwerke – „Advanced“ (Erweitert) – in Azure Kubernetes Service (AKS) konfigurieren, einschließlich der Bereitstellung eines AKS-Clusters in einem vorhandenen virtuellen Netzwerk und Subnetz.
services: container-service
ms.topic: article
ms.date: 06/03/2019
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: 3ab6ce9f955829cb3faf774c5b85c205f5841474
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132551347"
---
# <a name="configure-azure-cni-networking-in-azure-kubernetes-service-aks"></a>Konfigurieren von Azure CNI-Netzwerken in Azure Kubernetes Service (AKS)

Standardmäßig verwenden AKS-Cluster [kubenet][kubenet]. Ein virtuelles Netzwerk und ein Subnetz werden für Sie erstellt. Mit *kubenet* erhalten Knoten eine IP-Adresse aus einem Subnetz des virtuellen Netzwerks. Die Netzwerkadressübersetzung (NAT) wird dann auf den Knoten konfiguriert, und die Pods erhalten eine IP-Adresse, die hinter der Knoten-IP „versteckt“ ist. Dieser Ansatz reduziert die Anzahl der IP-Adressen, die Sie in Ihrem Netzwerkadressraum für die Verwendung von Pods reservieren müssen.

Mit [Azure Container Networking Interface (CNI)][cni-networking] erhält jeder Pod eine IP-Adresse aus dem Subnetz, mit der direkt darauf zugegriffen werden kann. Diese IP-Adressen müssen in Ihrem Netzwerkadressraum eindeutig sein und im Voraus geplant werden. Jeder Knoten verfügt über einen Konfigurationsparameter für die maximale Anzahl von Pods, die er unterstützt. Die entsprechende Anzahl von IP-Adressen pro Knoten wird dann im Voraus für diesen Knoten reserviert. Dieser Ansatz erfordert mehr Planung und führt oft zu einer Erschöpfung der IP-Adresse oder der Notwendigkeit, Cluster in einem größeren Subnetz neu zu erstellen, wenn die Anforderungen Ihrer Anwendung wachsen.

Dieser Artikel veranschaulicht die Verwendung von *Azure CNI*-Netzwerken zum Erstellen und Verwenden eines virtuellen Netzwerksubnetzes für einen AKS-Cluster. Weitere Informationen zu Netzwerkoptionen und -überlegungen finden Sie unter [Netzwerkkonzepte für Kubernetes und AKS][aks-network-concepts].

## <a name="prerequisites"></a>Voraussetzungen

* Das virtuelle Netzwerk des AKS-Clusters muss ausgehende Internetkonnektivität zulassen.
* AKS-Cluster dürfen für den Adressbereich des Kubernetes-Diensts, den Adressbereich für den Pod oder den Adressbereich für das virtuelle Clusternetzwerk nicht `169.254.0.0/16`, `172.30.0.0/16`, `172.31.0.0/16` oder `192.0.2.0/24` verwenden.
* Die vom AKS-Cluster verwendete Clusteridentität muss mindestens Berechtigungen [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md#network-contributor) für das Subnetz in Ihrem virtuellen Netzwerk haben. Wenn Sie eine [benutzerdefinierte Rolle](../role-based-access-control/custom-roles.md) anstelle der integrierten Rolle des Netzwerkmitwirkenden definieren möchten, sind die folgenden Berechtigungen erforderlich:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`
* Das Subnetz, das dem AKS-Knotenpool zugewiesen ist, darf kein [delegiertes Subnetz](../virtual-network/subnet-delegation-overview.md) sein.
* AKS wendet keine Netzwerksicherheitsgruppen (NSGs) auf sein Subnetz an und ändert keine der NSGs, die diesem Subnetz zugeordnet sind. Wenn Sie Ihr eigenes Subnetz bereitstellen und NSGs hinzufügen, die diesem Subnetz zugeordnet sind, müssen Sie sicherstellen, dass die Sicherheitsregeln in den NSGs Datenverkehr zwischen den CIDR-Bereichen der Knoten und Pods zulassen. Weitere Details finden Sie unter [Netzwerksicherheitsgruppen][aks-network-nsg].

## <a name="plan-ip-addressing-for-your-cluster"></a>Planen der IP-Adressierung für Ihren Cluster

Für Cluster, für die Azure CNI-Netzwerke konfiguriert sind, ist eine zusätzliche Planung erforderlich. Die Größe Ihres virtuellen Netzwerks und des zugehörigen Subnetzes muss die Anzahl von Pods, die ausgeführt werden sollen, und die Anzahl von Knoten für den Cluster abdecken.

IP-Adressen für die Pods und die Knoten des Clusters werden über das angegebene Subnetz im virtuellen Netzwerk zugewiesen. Jeder Knoten wird mit einer primären IP-Adresse konfiguriert. Standardmäßig werden 30 zusätzliche IP-Adressen durch Azure CNI vorkonfiguriert, die den im Knoten geplanten Pods zugewiesen werden. Wenn Sie Ihren Cluster aufskalieren, wird jeder Knoten auf ähnliche Weise mit IP-Adressen aus dem Subnetz konfiguriert. Sie können auch die [maximale Anzahl von Pods pro Knoten](#maximum-pods-per-node) anzeigen.

> [!IMPORTANT]
> Die Anzahl der erforderlichen IP-Adressen sollte Überlegungen für Upgrade- und Skalierungsvorgänge beinhalten. Wenn Sie den IP-Adressbereich so einstellen, dass er nur eine feste Anzahl von Knoten unterstützt, können Sie Ihren Cluster nicht aktualisieren oder skalieren.
>
> * Wenn Sie Ihren AKS-Cluster **aktualisieren**, wird im Cluster ein neuer Knoten bereitgestellt. Dienste und Workloads werden auf dem neuen Knoten ausgeführt, und ein älterer Knoten wird aus dem Cluster entfernt. Dieser rollende Aktualisierungsprozess erfordert, dass mindestens ein zusätzlicher Block von IP-Adressen verfügbar ist. Die Knotenanzahl ist dann `n + 1`.
>   * Dieser Aspekt ist besonders wichtig, wenn Sie Windows Server-Knotenpools verwenden. Bei Windows Server-Knoten in AKS erfolgt die Anwendung von Windows Updates nicht automatisch, sondern Sie führen ein Upgrade im Knotenpool durch. Dieses Upgrade stellt neue Knoten mit dem neuesten Window Server 2019-Basisknotenimage und Sicherheitspatches bereit. Weitere Informationen zum Upgrade eines Windows Server-Knotenpools finden Sie unter [Durchführen eines Upgrades für einen Knotenpool in AKS][nodepool-upgrade].
>
> * Wenn Sie Ihren AKS-Cluster **skalieren**, wird im Cluster ein neuer Knoten bereitgestellt. Dienste und Workloads werden auf dem neuen Knoten ausgeführt. Bei Ihrem IP-Adressbereich muss berücksichtigt werden, wie Sie die Anzahl der Knoten und Pods, die Ihr Cluster unterstützen kann, vergrößern können. Es sollte auch ein zusätzlicher Knoten für Aktualisierungsvorgänge integriert werden. Die Knotenanzahl ist dann `n + number-of-additional-scaled-nodes-you-anticipate + 1`.

Wenn Sie erwarten, dass Ihre Knoten die maximale Anzahl von Pods ausführen und regelmäßig Pods zerstören und bereitstellen, sollten Sie auch einige zusätzliche IP-Adressen pro Knoten berücksichtigen. Diese zusätzlichen IP-Adressen berücksichtigen, dass es einige Sekunden dauern kann, bis ein Dienst gelöscht und die IP-Adresse für einen neuen Dienst freigegeben wird und die Adresse erworben wird.

Der IP-Adressplan eines AKS-Clusters enthält neben einem virtuellen Netzwerk mindestens ein Subnetz für Knoten und Pods sowie einen Kubernetes-Dienstadressbereich.

| Adressbereich/Azure-Ressourcen | Grenzen und Kontingente |
| --------- | ------------- |
| Virtuelles Netzwerk | Das virtuelle Azure-Netzwerk kann eine Größe von /8 haben, ist aber auf 65.536 konfigurierte IP-Adressen beschränkt. Berücksichtigen Sie alle Netzwerkanforderungen, einschließlich der Kommunikation mit Diensten in anderen virtuellen Netzwerken, bevor Sie den Adressraum konfigurieren. Wenn Sie z. B. einen zu großen Adressraum konfigurieren, treten möglicherweise Probleme mit überlappenden Adressräumen in Ihrem Netzwerk auf.|
| Subnet | Muss groß genug für die Knoten, Pods und alle Kubernetes- und Azure-Ressourcen sein, die in Ihrem Cluster bereitgestellt werden können. Wenn Sie beispielsweise einen internen Azure Load Balancer bereitstellen, werden dessen Front-End-IP-Adressen aus dem Clusternetzwerk zugeordnet, nicht die öffentlichen IP-Adressen. Die Größe des Subnetzes sollte auch Aktualisierungsvorgänge oder zukünftige Skalierungsanforderungen berücksichtigen.<p />So berechnen Sie die *Mindestsubnetzgröße* mit einem zusätzlichen Knoten für Aktualisierungsvorgänge: `(number of nodes + 1) + ((number of nodes + 1) * maximum pods per node that you configure)`<p/>Beispiel für einen Cluster mit 50 Knoten: `(51) + (51  * 30 (default)) = 1,581` (/ 21 oder mehr)<p/>Beispiel für einen 50-Knoten-Cluster, der auch die Möglichkeit beinhaltet, ihn um weitere 10 Knoten hochzuskalieren: `(61) + (61 * 30 (default)) = 1,891` (/21 oder größer)<p>Wenn Sie beim Erstellen des Clusters keine maximale Anzahl von Pods pro Knoten angeben, wird diese auf *30* festgelegt. Die mindestens erforderliche Anzahl von IP-Adressen basiert auf diesem Wert. Wenn Sie Ihre mindestens erforderliche Anzahl von IP-Adressen anhand eines anderen Maximalwerts berechnen, finden Sie Informationen zum Festlegen dieses Werts beim Bereitstellen Ihres Clusters unter [Konfigurieren der maximalen Anzahl von Pods pro Knoten](#configure-maximum---new-clusters). |
| Kubernetes-Dienstadressbereich | Dieser Bereich darf nicht von Netzwerkelementen verwendet werden, die sich in diesem virtuellen Netzwerk befinden oder damit verbunden sind. Das Dienstadress-CIDR darf höchstens eine Größe von /12 aufweisen. Sie können diesen Bereich für unterschiedliche AKS-Cluster wiederverwenden. |
| Kubernetes-DNS-Dienst – IP-Adresse | Die IP-Adresse im Kubernetes-Dienstadressbereich wird bei der Clusterdienstermittlung verwendet. Verwenden Sie nicht die erste IP-Adresse Ihres Adressbereichs. Die erste Adresse Ihres Subnetzbereichs wird für die Adresse *kubernetes.default.svc.cluster.local* genutzt. |
| Docker-Bridge-Adresse | Die Docker-Bridge-Netzwerkadresse stellt die Standardnetzwerkadresse der Bridge *docker0* dar, die in allen Docker-Installationen vorhanden ist. Die Bridge *docker0* wird zwar nicht von AKS-Clustern oder den Pods selbst verwendet, die Adresse muss aber trotzdem festgelegt werden, damit Szenarien wie *docker build* innerhalb des AKS-Clusters weiterhin unterstützt werden. Für die Docker-Bridge-Netzwerkadresse muss ein CIDR-Wert ausgewählt werden, da Docker andernfalls automatisch ein Subnetz auswählt, was zu Konflikten mit anderen CIDRs führen kann. Wählen Sie einen Adressraum aus, der nicht mit den übrigen CIDRs in Ihrem Netzwerk in Konflikt steht (einschließlich Dienst-CIDR und Pod-CIDR des Clusters). Standard 172.17.0.1/16. Sie können diesen Bereich für unterschiedliche AKS-Cluster wiederverwenden. |

## <a name="maximum-pods-per-node"></a>Maximale Pods pro Knoten

Die maximale Anzahl von Pods pro Knoten in einem AKS-Cluster ist 250. *Standardmäßig* variiert die maximale Anzahl von Pods pro Knoten zwischen *kubenet*- und *Azure CNI*-Netzwerken sowie je nach Methode für die Clusterbereitstellung.

| Bereitstellungsmethode | Kubenet-Standardeinstellung | Azure CNI-Standardeinstellung | Bei der Bereitstellung konfigurierbar |
| -- | :--: | :--: | -- |
| Azure CLI | 110 | 30 | Ja (bis zu 250) |
| Resource Manager-Vorlage | 110 | 30 | Ja (bis zu 250) |
| Portal | 110 | 110 (konfiguriert auf der Registerkarte „Knotenpools“) | Nein |

### <a name="configure-maximum---new-clusters"></a>Konfigurieren des Höchstwerts: Neue Cluster

Sie können die maximale Anzahl von Pods pro Knoten bei der Clusterbereitstellung oder beim Hinzufügen von neuen Knotenpools konfigurieren. Wenn Sie die Bereitstellung mit der Azure CLI oder mit einer Resource Manager-Vorlage durchführen, können Sie die maximale Anzahl von Pods pro Knoten auf bis zu 250 festlegen.

Wenn Sie die maximale Anzahl von Pods (maxPods) beim Erstellen neuer Knotenpools nicht angeben, wird der Standardwert 30 für Azure CNI verwendet.

Ein Mindestwert für maximale Pods pro Knoten wird erzwungen, um Platz für Systempods zu garantieren, die für die Clusterintegrität entscheidend sind. Der Mindestwert, der für maximale Pods pro Knoten festgelegt werden kann, ist nur dann 10, wenn die Konfiguration der einzelnen Knotenpools Platz für mindestens 30 Pods bietet. Wenn Sie z. B. die maximalen Pods pro Knoten auf mindestens 10 festlegen, muss jeder einzelne Knotenpool mindestens drei Knoten aufweisen. Diese Anforderung gilt auch für jeden neu erstellten Knotenpool, d. h. wenn für die maximalen Pods pro Knoten der Wert 10 definiert ist, muss jeder weitere hinzugefügte Knotenpool mindestens drei Knoten aufweisen.

| Netzwerk | Minimum | Maximum |
| -- | :--: | :--: |
| Azure CNI | 10 | 250 |
| Kubenet | 10 | 110 |

> [!NOTE]
> Der Mindestwert in der obigen Tabelle wird vom AKS-Dienst strikt erzwungen. Sie können für maxPods keinen niedrigeren Wert als den angezeigten Mindestwert festlegen, da dadurch der Start des Clusters verhindert werden kann.

* **Azure CLI**: Geben Sie das `--max-pods`-Argument an, wenn Sie einen Cluster mit dem Befehl [az aks create][az-aks-create] bereitstellen. Der Höchstwert ist 250.
* **Resource Manager-Vorlage**: Geben Sie die `maxPods`-Eigenschaft im Objekt [ManagedClusterAgentPoolProfile] an, wenn Sie einen Cluster mit einer Resource Manager-Vorlage bereitstellen. Der Höchstwert ist 250.
* **Azure-Portal**: Sie können die maximale Anzahl von Pods pro Knoten nicht ändern, wenn Sie einen Cluster über das Azure-Portal bereitstellen. Die Cluster in Azure CNI-Netzwerken sind, wenn die Bereitstellung über das Azure-Portal erfolgt, auf 110 Pods pro Knoten beschränkt.

### <a name="configure-maximum---existing-clusters"></a>Konfigurieren des Höchstwerts: Vorhandene Cluster

Die Einstellung für die maximale Anzahl von Pods pro Knoten kann beim Erstellen eines neuen Knotenpools definiert werden. Wenn Sie die maximale Anzahl von Pods pro Knoten in einem vorhandenen Cluster erhöhen müssen, fügen Sie einen neuen Knotenpool mit dem neuen gewünschten Maximalwert hinzu. Nachdem Sie Ihre Pods in den neuen Pool migriert haben, löschen Sie den älteren Pool. Wenn Sie einen älteren Pool in einem Cluster löschen möchten, müssen Sie sicherstellen, dass Sie die Modi der Knotenpools gemäß der Definition im Artikel [Verwalten von Systemknotenpools in Azure Kubernetes Service (AKS)][system-node-pools] festlegen.

## <a name="deployment-parameters"></a>Bereitstellungsparameter

Beim Erstellen eines AKS-Clusters können folgende Parameter für Azure CNI-Netzwerke konfiguriert werden:

**Virtuelles Netzwerk:** Das virtuelle Netzwerk, in dem Sie den Kubernetes-Cluster bereitstellen möchten. Wenn Sie ein neues virtuelles Netzwerk für Ihren Cluster erstellen möchten, können Sie *Neu erstellen* wählen und die Schritte im Abschnitt *Erstellen eines virtuellen Netzwerks* ausführen. Weitere Informationen zu Grenzwerten und Kontingenten für virtuelle Azure-Netzwerke finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

**Subnetz**: Das Subnetz im virtuellen Netzwerk, in dem Sie den Cluster bereitstellen möchten. Wenn Sie ein neues Subnetz im virtuellen Netzwerk für Ihren Cluster erstellen möchten, können Sie *Neu erstellen* wählen und die Schritte im Abschnitt *Erstellen eines Subnetzes* ausführen. Bei Hybridkonnektivität sollte sich der Adressbereich nicht mit anderen virtuellen Netzwerken in Ihrer Umgebung überschneiden.

**Azure-Netzwerk-Plug-In:** Wenn das Azure-Netzwerk-Plug-In verwendet wird, kann nicht von VMs mit einer IP-Adresse in clusterCIDR außerhalb des AKS-Clusters mit „externalTrafficPolicy=Local“ auf den internen Lastenausgleich zugegriffen werden.

**Kubernetes-Dienstadressbereich**: Dieser Parameter enthält die virtuellen IP-Adressen, die Kubernetes den internen [Diensten][services] in Ihrem Cluster zuweist. Sie können jeden privaten Adressbereich verwenden, der die folgenden Anforderungen erfüllen:

* Darf nicht innerhalb des IP-Adressbereichs des virtuellen Netzwerk Ihres Clusters liegen
* Darf sich nicht mit anderen virtuellen Netzwerken überlappen, die Peers des virtuellen Netzwerks des Clusters sind
* Er darf sich nicht mit lokalen IP-Adressen überlappen.
* Er darf sich nicht in den Bereichen `169.254.0.0/16`, `172.30.0.0/16`, `172.31.0.0/16` oder `192.0.2.0/24` befinden.

Obwohl es technisch möglich ist, einen Dienstadressbereich im gleichen virtuellen Netzwerk wie Ihr Cluster anzugeben, wird dies nicht empfohlen. Bei Verwendung sich überlappender IP-Adressbereiche kann es zu unvorhersehbarem Verhalten kommen. Weitere Informationen finden Sie im Abschnitt [Häufig gestellte Fragen](#frequently-asked-questions) in diesem Artikel. Weitere Informationen zu Kubernetes-Diensten finden Sie in der Kubernetes-Dokumentation unter [Dienste][services].

**Kubernetes-DNS-Dienst – IP-Adresse**:  Die IP-Adresse für den DNS-Dienst des Clusters. Diese Adresse muss innerhalb des *Kubernetes-Dienstadressbereichs* liegen. Verwenden Sie nicht die erste IP-Adresse Ihres Adressbereichs. Die erste Adresse Ihres Subnetzbereichs wird für die Adresse *kubernetes.default.svc.cluster.local* genutzt.

**Docker-Bridge-Adresse**: Die Docker-Bridge-Netzwerkadresse stellt die Standardnetzwerkadresse der Bridge *docker0* dar, die in allen Docker-Installationen vorhanden ist. Die Bridge *docker0* wird zwar nicht von AKS-Clustern oder den Pods selbst verwendet, die Adresse muss aber trotzdem festgelegt werden, damit Szenarien wie *docker build* innerhalb des AKS-Clusters weiterhin unterstützt werden. Für die Docker-Bridge-Netzwerkadresse muss ein CIDR-Wert ausgewählt werden, da Docker andernfalls automatisch ein Subnetz auswählt, was zu Konflikten mit anderen CIDRs führen kann. Wählen Sie einen Adressraum aus, der nicht mit den übrigen CIDRs in Ihrem Netzwerk in Konflikt steht (einschließlich Dienst-CIDR und Pod-CIDR des Clusters).

## <a name="configure-networking---cli"></a>Konfigurieren der Netzwerkeinstellungen – CLI

Wenn Sie einen AKS-Cluster mit Azure CLI erstellen, können Sie auch Azure CLI-Netzwerkeinstellungen konfigurieren. Verwenden Sie die folgenden Befehle, um einen neuen AKS-Cluster mit aktivierten Azure CLI-Netzwerken zu erstellen.

Ermitteln Sie zunächst die Subnetzressourcen-ID für das vorhandene Subnetz, mit dem der AKS-Cluster verknüpft werden soll:

```azurecli-interactive
$ az network vnet subnet list \
    --resource-group myVnet \
    --vnet-name myVnet \
    --query "[0].id" --output tsv

/subscriptions/<guid>/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Verwenden Sie den Befehl [az aks create][az-aks-create] mit dem Argument `--network-plugin azure`, um einen Cluster mit erweiterten Netzwerkeinstellungen zu erstellen. Aktualisieren Sie den `--vnet-subnet-id`-Wert mit der im vorherigen Schritt erfassten Subnetz-ID:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --generate-ssh-keys
```

## <a name="configure-networking---portal"></a>Konfigurieren der Netzwerkeinstellungen – Portal

Im folgenden Screenshot des Azure-Portals ist ein Beispiel für die Konfiguration dieser Einstellungen während der AKS-Clustererstellung dargestellt:

![Erweiterte Netzwerkkonfiguration im Azure-Portal][portal-01-networking-advanced]

## <a name="dynamic-allocation-of-ips-and-enhanced-subnet-support-preview"></a>Dynamische IP-Adressenzuordnung und erweiterte Subnetzunterstützung (Vorschauphase)

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

Ein Nachteil des herkömmlichen CNI liegt darin, dass die IP-Adressen für Pods mit zunehmender AKS-Clustergröße erschöpfen und der gesamte Cluster in diesem Fall in einem größeren Subnetz neu erstellt werden muss. Die neue Azure CNI-Funktion, IP-Adressen dynamisch zuzuordnen, löst dieses Problem, indem IP-Adressen für Pods aus einem Subnetz zugeordnet werden, das von dem Subnetz getrennt ist, in dem der AKS-Cluster gehostet wird. Dies hat folgende Vorteile:

* **Bessere Auslastung von IP-Adressen:** IP-Adressen werden Clusterpods dynamisch aus dem Podsubnetz zugeordnet. Dies führt zu einer besseren Auslastung der IP-Adressen im Cluster im Vergleich zur herkömmlichen CNI-Lösung, bei der die IP-Adressen für jeden Knoten statisch zugeordnet werden.  

* **Skalierbar und flexibel:** Knoten- und Podsubnetze können unabhängig voneinander skaliert werden. Ein einzelnes Podsubnetz kann für mehrere Knotenpools eines Clusters oder mehrere AKS-Cluster im selben VNET freigegeben werden. Sie können auch ein separates Podsubnetz für einen Knotenpool konfigurieren.  

* **Höchstleistung:** Da Pods VNET-IP-Adressen zugewiesen sind, besteht eine direkte Verbindung zu anderen Clusterpods und -ressourcen im VNET. Die Lösung unterstützt selbst größte Cluster ohne Leistungseinbußen.

* **Separate VNET-Richtlinien für Pods:** Da für Pods ein separates Subnetz besteht, können Sie auch separate VNET-Richtlinien konfigurieren, die von den Knotenrichtlinien abweichen. Das ermöglicht viele nützliche Anwendungsfälle wie die Beschränkung der Internetkonnektivität auf Pods statt auf Pods und Knoten, das Korrigieren der Quell-IP für Pods in einem Knotenpool mithilfe einer Netzwerkadressenübersetzung für VNETs und den Einsatz von Netzwerksicherheitsgruppen, um Datenverkehr zwischen Knotenpools zu filtern.  

* **Netzwerkrichtlinien für Kubernetes:** Sowohl die Azure-Netzwerkrichtlinien als auch Calico sind mit dieser neuen Lösung kompatibel.  

### <a name="additional-prerequisites"></a>Zusätzliche Voraussetzungen

Die bereits für das Azure CNI aufgeführten [Voraussetzungen][prerequisites] gelten weiterhin, es gibt jedoch einige zusätzliche Einschränkungen:

* Es werden nur Linux-Knotencluster und -Knotenpools unterstützt.
* AKS-Engine-Cluster und selbst erstellte Cluster werden nicht unterstützt.

### <a name="install-the-aks-preview-azure-cli"></a>Installieren der Azure-Befehlszeilenschnittstelle `aks-preview`

Sie benötigen die Azure CLI-Erweiterung *aks-preview*. Installieren Sie die Erweiterung *aks-preview* der Azure-Befehlszeilenschnittstelle mithilfe des Befehls [az extension add][az-extension-add]. Alternativ können Sie verfügbare Updates mithilfe des Befehls [az extension update][az-extension-update] installieren.

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-the-podsubnetpreview-preview-feature"></a>Registrieren der Previewfunktion `PodSubnetPreview`

Sie müssen darüber hinaus das Featureflag `PodSubnetPreview` in Ihrem Abonnement aktivieren, um dieses Feature zu verwenden.

Registrieren Sie das Featureflag `PodSubnetPreview` mithilfe des Befehls [az feature register][az-feature-register], wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "PodSubnetPreview"
```

Es dauert einige Minuten, bis der Status *Registered (Registriert)* angezeigt wird. Überprüfen Sie den Registrierungsstatus mithilfe des Befehls [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/PodSubnetPreview')].{Name:name,State:properties.state}"
```

Wenn der Vorgang abgeschlossen ist, können Sie die Registrierung des *Microsoft.ContainerService*-Ressourcenanbieters mit dem Befehl [az provider register][az-provider-register] aktualisieren:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="planning-ip-addressing"></a>Planen der IP-Adressenvergabe

Dieses Feature erleichtert die Planung erheblich. Da die Knoten und Pods unabhängig voneinander skaliert werden, können auch ihre Adressräume separat geplant werden. Da Podsubnetze auf die Granularität eines Knotenpools konfiguriert werden können, können Kunden jederzeit ein neues Subnetz hinzufügen, wenn sie einen Knotenpool hinzufügen. Die Systempods in einem Cluster- bzw. Knotenpool erhalten die IP-Adressen ebenfalls aus dem Podsubnetz, sodass dieses Verhalten berücksichtigt werden muss.

Die IP-Adressenplanung für Kubernetes-Dienste und Docker-Bridges bleibt unverändert.

### <a name="maximum-pods-per-node-in-a-cluster-with-dynamic-allocation-of-ips-and-enhanced-subnet-support"></a>Maximale Pods pro Knoten in einem Cluster mit dynamischer IP-Adressenzuordnung und erweiterter Subnetzunterstützung

Die Pods pro Knotenwerte im Azure CNI mit dynamischer IP-Adressenzuordnung unterscheiden sich nun geringfügig vom Verhalten des herkömmlichen CNI:

|CNI|Bereitstellungsmethode|Standard|Bei der Bereitstellung konfigurierbar|
|--|--| :--: |--|
|Herkömmliches Azure CNI|Azure CLI|30|Ja (bis zu 250)|
|Azure CNI mit dynamischer IP-Adressenzuordnung|Azure CLI|250|Ja (bis zu 250)|

Davon abgesehen bleiben die Anleitungen zum Konfiguration der maximalen Knoten pro Pool unverändert.

### <a name="additional-deployment-parameters"></a>Zusätzliche Bereitstellungsparameter

Alle oben genannten Bereitstellungsparameter außer einem sind weiterhin gültig:

* Der Parameter **subnet** verweist jetzt auf das Subnetz für die Knoten des Clusters.
* Zusätzlich wird der Parameter **pod subnet** verwendet, um das Subnetz anzugeben, dessen IP-Adressen den Pods dynamisch zugeordnet werden.

### <a name="configure-networking---cli-with-dynamic-allocation-of-ips-and-enhanced-subnet-support"></a>Netzwerkkonfiguration: CLI mit dynamischer IP-Adressenzuordnung und erweiterter Subnetzunterstützung

Die Verwendung der dynamischen IP-Adressenzuordnung und der erweiterten Subnetzunterstützung in Ihrem Cluster ähnelt der Standardmethode der Azure CNI-Konfiguration für Cluster. Das folgende Beispiel veranschaulicht die Erstellung eines neuen virtuellen Netzwerks mit einem Subnetz für Knoten und einem weiteren Subnetz für Pods und die Erstellung eines Clusters, der das Azure CNI mit dynamischer IP-Adressenzuordnung und erweiterter Subnetzunterstützung verwendet. Ersetzen Sie Variablen wie `$subscription` durch eigene Werte:

Erstellen Sie zunächst das virtuelle Netzwerk mit zwei Subnetzen:

```azurecli-interactive
resourceGroup="myResourceGroup"
vnet="myVirtualNetwork"
location="westcentralus"

# Create the resource group
az group create --name $resourceGroup --location $location

# Create our two subnet network 
az network vnet create -g $resourceGroup --location $location --name $vnet --address-prefixes 10.0.0.0/8 -o none 
az network vnet subnet create -g $resourceGroup --vnet-name $vnet --name nodesubnet --address-prefixes 10.240.0.0/16 -o none 
az network vnet subnet create -g $resourceGroup --vnet-name $vnet --name podsubnet --address-prefixes 10.241.0.0/16 -o none 
```

Erstellen Sie dann den Cluster, indem Sie mithilfe von `--vnet-subnet-id` auf das Knotensubnetz und mithilfe von `--pod-subnet-id` auf das Podsubnetz verweisen:

```azurecli-interactive
clusterName="myAKSCluster"
subscription="aaaaaaa-aaaaa-aaaaaa-aaaa"

az aks create -n $clusterName -g $resourceGroup -l $location \
  --max-pods 250 \
  --node-count 2 \
  --network-plugin azure \
  --vnet-subnet-id /subscriptions/$subscription/resourceGroups/$resourceGroup/providers/Microsoft.Network/virtualNetworks/$vnet/subnets/nodesubnet \
  --pod-subnet-id /subscriptions/$subscription/resourceGroups/$resourceGroup/providers/Microsoft.Network/virtualNetworks/$vnet/subnets/podsubnet  
```

#### <a name="adding-node-pool"></a>Hinzufügen des Knotenpools

Verweisen Sie beim Hinzufügen des Knotenpools mithilfe von `--vnet-subnet-id` auf das Knotensubnetz und mithilfe von `--pod-subnet-id` auf das Podsubnetz. Im folgenden Beispiel werden zwei neue Subnetze erstellt, auf die dann bei der Erstellung eines neuen Knotenpools verwiesen wird:

```azurecli-interactive
az network vnet subnet create -g $resourceGroup --vnet-name $vnet --name node2subnet --address-prefixes 10.242.0.0/16 -o none 
az network vnet subnet create -g $resourceGroup --vnet-name $vnet --name pod2subnet --address-prefixes 10.243.0.0/16 -o none 

az aks nodepool add --cluster-name $clusterName -g $resourceGroup  -n newnodepool \
  --max-pods 250 \
  --node-count 2 \
  --vnet-subnet-id /subscriptions/$subscription/resourceGroups/$resourceGroup/providers/Microsoft.Network/virtualNetworks/$vnet/subnets/node2subnet \
  --pod-subnet-id /subscriptions/$subscription/resourceGroups/$resourceGroup/providers/Microsoft.Network/virtualNetworks/$vnet/subnets/pod2subnet \
  --no-wait 
```

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Die folgenden Fragen und Antworten gelten für die **Azure CLI**-Netzwerkkonfiguration.

* *Kann ich VMs in meinem Clustersubnetz bereitstellen?*

  Ja.

* *Welche Quell-IP-Adressen sind für externe Systeme bei Datenverkehr sichtbar, der aus einem Azure CNI-fähigen Pod stammt?*

  Systeme im gleichen virtuellen Netzwerk wie der AKS-Cluster sehen die Pod-IP als Quelladresse für jeglichen Datenverkehr aus dem Pod. Systeme, die sich außerhalb des virtuellen Netzwerks des AKS-Clusters befinden, sehen die Knoten-IP als Quelladresse für jeglichen Datenverkehr aus dem Pod.

* *Kann ich Netzwerkrichtlinien pro Pod konfigurieren?*

  Ja, die Kubernetes-Netzwerkrichtlinie ist in AKS verfügbar. Informationen zu den ersten Schritten finden Sie unter [Sicherer Datenverkehr zwischen Pods durch Netzwerkrichtlinien in Azure Kubernetes Service][network-policy].

* *Ist die maximale Anzahl von Pods, die auf einem Knoten bereitgestellt werden können, konfigurierbar?*

  Ja, wenn Sie einen Cluster mit der Azure CLI oder einer Resource Manager-Vorlage bereitstellen. Weitere Informationen finden Sie unter [Maximale Pods pro Knoten](#maximum-pods-per-node).

  Sie können die maximalen Anzahl von Pods pro Knoten für einen vorhandenen Cluster nicht ändern.

* *Wie kann ich zusätzliche Eigenschaften für das Subnetz konfigurieren, das ich während der Erstellung des AKS-Clusters erstellt habe? Beispiel: Dienstendpunkte.*

  Die vollständige Liste mit Eigenschaften für das virtuelle Netzwerk und Subnetze, die Sie während der Erstellung des AKS-Clusters erstellen, kann im Azure-Portal auf der Standardseite für die Konfiguration des virtuellen Netzwerks konfiguriert werden.

* *Kann ich im virtuellen Netzwerk meines Clusters ein anderes Subnetz für den*  **Kubernetes-Dienstadressbereich** verwenden?

  Es wird zwar nicht empfohlen, diese Konfiguration ist jedoch möglich. Der Dienstadressbereich ist ein Satz von virtuellen IP-Adressen (VIPs), die Kubernetes internen Diensten in Ihrem Cluster zuweist. Das Azure-Netzwerk hat keinen Einblick in den Dienst-IP-Adressbereich des Kubernetes-Clusters. Aufgrund fehlender Einblicke in den Dienstadressbereich des Clusters ist es möglich, später ein neues Subnetz im virtuellen Netzwerk des Clusters zu erstellen, das mit dem Dienstadressbereich überlappt. Im Falle einer solchen Überlappung weist Kubernetes einem Dienst ggf. eine IP zu, die bereits von einer anderen Ressource im Subnetz verwendet wird. Dies führt zu unvorhersehbarem Verhalten oder Fehlern. Wenn Sie einen Adressbereich außerhalb des virtuellen Netzwerk des Clusters verwenden, können Sie dieses Überlappungsrisiko umgehen.

### <a name="dynamic-allocation-of-ip-addresses-and-enhanced-subnet-support-faqs"></a>FAQ zur dynamischen IP-Adressenzuordnung und zur erweiterten Subnetzunterstützung

Die folgenden Fragen und Antworten gelten für die **Azure CNI-Netzwerkkonfiguration mit dynamischer IP-Adressenzuordnung und erweiterter Subnetzunterstützung**.

* *Kann ich mehrere Podsubnetze zu einem Cluster- bzw. Knotenpool hinzufügen?*

  Einem Cluster oder Knotenpool kann nur ein Subnetz zugewiesen werden. Mehrere Cluster- oder Knotenpools können jedoch ein einzelnes Subnetz gemeinsam nutzen.

* *Kann ich Podsubnetze auch aus unterschiedlichen VNETs zuweisen?*

  Das Podsubnetz sollte demselben VNET wie der Cluster angehören.  

* *Können einige Knotenpools in einem Cluster das herkömmliche CNI, andere jedoch das neue CNI nutzen?*

  Für den gesamten Cluster sollte nur eine CNI-Variante verwendet werden.

## <a name="aks-engine"></a>AKS Engine

Die [Azure Kubernetes Service-Engine (AKS-Engine)][aks-engine] ist ein Open-Source-Projekt, über das Azure Resource Manager-Vorlagen generiert werden, die Sie zum Bereitstellen von Kubernetes-Clustern in Azure verwenden können.

Per AKS-Engine erstellte Kubernetes-Cluster unterstützen sowohl das [kubenet][kubenet]- als auch das [Azure CNI][cni-networking]-Plug-In. Für die AKS Engine werden also beide Netzwerkszenarien unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Netzwerken in AKS finden Sie in den folgenden Artikeln:

* [Verwenden einer statischen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)](static-ip.md)
* [Verwenden eines internen Lastenausgleichs mit Azure Container Service (AKS)](internal-lb.md)

* [Erstellen eines einfachen Eingangscontrollers mit Konnektivität mit einem externen Netzwerk][aks-ingress-basic]
* [Aktivieren des Add-Ons für das HTTP-Anwendungsrouting][aks-http-app-routing]
* [Erstellen eines Eingangscontrollers, der ein internes, privates Netzwerk und eine IP-Adresse verwendet][aks-ingress-internal]
* [Erstellen eines Eingangscontrollers mit einer dynamischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-tls]
* [Erstellen eines Eingangscontrollers mit einer statischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-static-tls]
<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png [portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[aks-engine]: https://github.com/Azure/aks-engine
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks#az_aks_create
[aks-ssh]: ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object
[aks-network-concepts]: concepts-network.md
[aks-network-nsg]: concepts-network.md#network-security-groups
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-internal]: ingress-internal-ip.md
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az_extension_update
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-provider-register]: /cli/azure/provider#az_provider_register
[network-policy]: use-network-policies.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[network-comparisons]: concepts-network.md#compare-network-models
[system-node-pools]: use-system-pools.md
[prerequisites]: configure-azure-cni.md#prerequisites
