---
title: Erstellen und Anfügen eines Azure Kubernetes Service
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit Azure Machine Learning einen neuen Azure Kubernetes Service-Cluster erstellen oder einen bestehenden AKS-Cluster an Ihren Arbeitsbereich anfügen können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.custom: devx-track-azurecli
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 11/05/2021
ms.openlocfilehash: 4f72252bf375f9341b08f99b57e540784b47429a
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2021
ms.locfileid: "132486302"
---
# <a name="create-and-attach-an-azure-kubernetes-service-cluster"></a>Erstellen und Anfügen eines Azure Kubernetes Service-Clusters

Mit Azure Machine Learning kann ein trainiertes Machine Learning-Modell im Azure Kubernetes Service bereitgestellt werden. Allerdings müssen Sie zunächst einen AKS-Cluster (Azure Kubernetes Service) in Ihrem Azure ML-Arbeitsbereich __erstellen__ oder einen vorhandenen AKS-Cluster __anfügen__. Dieser Artikel enthält Informationen sowohl zum Erstellen als auch zum Anfügen eines Clusters.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

- Die [Azure CLI-Erweiterung für Machine Learning Service](reference-azure-machine-learning-cli.md), das [Azure Machine Learning Python SDK](/python/api/overview/azure/ml/intro) oder die [Visual Studio Code-Erweiterung für Azure Machine Learning](how-to-setup-vs-code.md).

- Wenn Sie die Verwendung eines Azure-Virtual Network zum Sichern der Kommunikation zwischen Ihrem Azure ML-Arbeitsbereich und dem AKS-Cluster beabsichtigen, lesen Sie den Artikel [Netzwerkisolation während Training und Rückschluss](./how-to-network-security-overview.md).

## <a name="limitations"></a>Einschränkungen

- Wenn in Ihrem Cluster anstelle eines Load Balancer Basic (BLB) ein **Load Balancer Standard (SLB)** bereitgestellt werden muss, erstellen Sie im AKS-Portal, mit der CLI oder über das SDK einen Cluster, den Sie dann an den AML-Arbeitsbereich **anfügen**.

- Wenn Sie über eine Azure Policy verfügen, die die Erstellung von öffentlichen IP-Adressen einschränkt, tritt bei der AKS-Clustererstellung ein Fehler auf. AKS erfordert eine öffentliche IP-Adresse für [ausgehenden Datenverkehr](../aks/limit-egress-traffic.md). Der Artikel zum ausgehenden Datenverkehr enthält außerdem Anleitungen zum Sperren von aus dem Cluster über die öffentliche IP-Adresse ausgehenden Datenverkehr mit Ausnahme einiger vollqualifizierter Domänennamen. Es gibt 2 Möglichkeiten, eine öffentliche IP-Adresse zu aktivieren:
    - Der Cluster kann die öffentliche IP-Adresse verwenden, die standardmäßig mit dem BLB oder SLB erstellt wird, oder
    - der Cluster kann ohne öffentliche IP-Adresse erstellt werden, und anschließend wird eine öffentliche IP-Adresse mit einer Firewall mit einer benutzerdefinierten Route konfiguriert. Weitere Informationen finden Sie unter [Anpassen des ausgehenden Clusterdatenverkehrs mit einer benutzerdefinierten Route](../aks/egress-outboundtype.md).
    
    Die AML-Steuerungsebene kommuniziert nicht mit dieser öffentlichen IP-Adresse. Sie kommuniziert mit der AKS-Steuerungsebene für Bereitstellungen. 

- Wenn Sie einen AKS-Cluster **anfügen**, der über einen [autorisierten IP-Adressbereich mit Zugriff auf den API-Server](../aks/api-server-authorized-ip-ranges.md) verfügt, aktivieren Sie die IP-Adressbereiche der AML-Steuerungsebene für den AKS-Cluster. Die AML-Steuerungsebene wird für Regionspaare bereitgestellt und stellt Rückschlusspods im AKS-Cluster bereit. Ohne Zugriff auf den API-Server können die Rückschlusspods nicht bereitgestellt werden. Verwenden Sie die [IP-Adressbereiche](https://www.microsoft.com/download/confirmation.aspx?id=56519) für beide [Regionspaare](../best-practices-availability-paired-regions.md), wenn Sie die IP-Adressbereiche in einem AKS-Cluster aktivieren.

    Autorisierte IP-Adressbereiche funktionieren nur mit Load Balancer Standard.

- Um einen AKS-Cluster aus einem __anderen Azure-Abonnement__ anfügen zu können, muss Ihnen (Ihrem Azure AD-Konto) die Rolle **Mitwirkender** im AKS-Cluster erteilt werden. Überprüfen Sie Ihren Zugriff im [Azure-Portal](https://ms.portal.azure.com/).

- Wenn Sie einen privaten AKS-Cluster (mit Azure Private Link) verwenden möchten, müssen Sie zuerst den Cluster erstellen und diesen dann **an den Arbeitsbereich anfügen**. Weitere Informationen finden Sie unter [Erstellen eines privaten Azure Kubernetes Service-Clusters](../aks/private-clusters.md).

- Die Verwendung eines [öffentlichen vollqualifizierten Domänennamens (FQDN) mit einem privaten AKS-Cluster](/azure/aks/private-clusters#create-a-private-aks-cluster-with-a-public-fqdn) wird mit Azure Machine Learning __nicht unterstützt__. 

- Der Computename für den AKS-Cluster MUSS innerhalb Ihres Azure ML-Arbeitsbereichs eindeutig sein. Er kann Buchstaben, Ziffern und Bindestriche enthalten. Er muss mit einem Buchstaben beginnen, mit einem Buchstaben oder einer Ziffer enden und zwischen 3 und 24 Zeichen lang sein.
 
 - Wenn Sie Modelle auf **GPU**-Knoten oder **FPGA**-Knoten (oder einer bestimmten SKU) bereitstellen möchten, müssen Sie einen Cluster mit der jeweiligen SKU erstellen. Das Erstellen eines sekundären Knotenpools in einem vorhandenen Cluster und Bereitstellen von Modellen im sekundären Knotenpool wird nicht unterstützt.
 
- Beim Erstellen oder Anfügen eines Clusters können Sie auswählen, ob der Cluster für __Dev/Test__ oder die __Produktion__ erstellt werden soll. Wenn Sie einen AKS-Cluster für __Entwicklung__, __Validierung__ und __Tests__ statt für die Produktion erstellen möchten, legen Sie __Clusterzweck__ auf __Dev/Test__ fest. Wenn Sie den Clusterzweck nicht angeben, wird ein Cluster für die __Produktion__ erstellt. 

    > [!IMPORTANT]
    > Ein __Dev/Test__-Cluster ist nicht für Produktionsdatenverkehr geeignet und kann Rückschlusszeiten erhöhen. Entwicklungs- und Testcluster garantieren auch keine Fehlertoleranz.

- Wenn ein Cluster erstellt oder angehängt wird, muss er mindestens __3 Knoten__ enthalten, wenn er für die __Produktion__ verwendet werden soll. Für einen __Dev/Test__ Cluster muss er mindestens einen Knoten enthalten.

- Das Azure Machine Learning-SDK bietet keine Unterstützung zur Skalierung eines AKS-Clusters. Die Knoten in Ihrem Cluster können Sie über die Benutzeroberfläche Ihres AKS-Clusters in Azure Machine Learning Studio skalieren. Sie können nur die Knotenanzahl ändern, nicht die VM-Größe des Clusters. Weitere Informationen zum Skalieren der Knoten in einem AKS-Cluster finden Sie in den folgenden Artikeln:

    - [Skalieren der Anzahl der Knoten in einem Azure Kubernetes Service-Cluster (AKS)](../aks/scale-cluster.md)
    - [Automatisches Skalieren eines Clusters zur Erfüllung von Anwendungsanforderungen in Azure Kubernetes Service (AKS)](../aks/cluster-autoscaler.md)

- __Aktualisieren Sie den Cluster nicht direkt mithilfe einer YAML-Konfiguration__. Während Azure Kubernetes Services Updates über die YAML-Konfiguration unterstützt, überschreiben Azure Machine Learning-Bereitstellungen Ihre Änderungen. Die einzigen beiden YAML-Felder, die nicht überschrieben werden, sind __Anforderungslimits__ und __CPU und Arbeitsspeicher__.

- Das Erstellen eines AKS-Clusters mithilfe der Azure Machine Learning Studio-Benutzeroberfläche, des Azure Machine Learning Studio-SDKs oder der CLI-Erweiterung ist __nicht__ idempotent. Wenn Sie versuchen, die Ressource erneut zu erstellen, führt dies zu dem Fehler, dass ein Cluster mit demselben Namen bereits vorhanden ist.
    
    - Die Verwendung einer Azure Resource Manager-Vorlage und der Ressource [Microsoft.MachineLearningServices/workspaces/computes](/azure/templates/microsoft.machinelearningservices/2019-11-01/workspaces/computes) zum Erstellen eines AKS-Clusters ist ebenfalls __nicht__ idempotent. Wenn Sie versuchen, die Vorlage erneut zu verwenden, um eine bereits vorhandene Ressource zu aktualisieren, wird derselbe Fehler angezeigt.

## <a name="azure-kubernetes-service-version"></a>Version von Azure Kubernetes Service

Mit Azure Kubernetes Service können Sie einen Cluster mit einer Vielzahl von Kubernetes-Versionen erstellen. Weitere Informationen zu verfügbaren Versionen finden Sie unter [Unterstützte Kubernetes-Versionen in Azure Kubernetes Service](../aks/supported-kubernetes-versions.md).

Wenn Sie einen Azure Kubernetes Service-Cluster mithilfe einer der folgenden Methoden **erstellen**, *können Sie die Version des Clusters nicht auswählen*, der erstellt wird:

* Azure Machine Learning Studio oder den Abschnitt „Azure Machine Learning“ im Azure-Portal.
* Machine Learning-Erweiterung für die Azure CLI.
* Azure Machine Learning SDK.

Diese Methoden zum Erstellen eines AKS-Clusters verwenden die __Standardversion__ des Clusters. *Die Standardversion ändert sich im Laufe der Zeit*, wenn neue Kubernetes-Versionen veröffentlicht werden.

Beim **Anfügen** an einen vorhandenen AKS-Cluster werden alle derzeit unterstützten AKS-Versionen unterstützt.

> [!IMPORTANT]
> Azure Kubernetes Service verwendet den [Blobfuse FlexVolume-Treiber](https://github.com/Azure/kubernetes-volume-drivers/blob/master/flexvolume/blobfuse/README.md) für die Versionen <=1.16 und den [Blob CSI-Treiber](https://github.com/kubernetes-sigs/blob-csi-driver/blob/master/README.md) für die Versionen >=1.17. Daher ist es wichtig, den [Webdienst nach dem Clusterupgrade erneut bereitzustellen oder zu aktualisieren](how-to-deploy-update-web-service.md), um die Bereitstellung für die richtige Blobfuse-Methode für die Clusterversion durchzuführen.

> [!NOTE]
> Es gibt möglicherweise Grenzfälle, in denen Sie über einen älteren Cluster verfügen, der nicht mehr unterstützt wird. In diesem Fall gibt der Anfügevorgang einen Fehler zurück und listet die derzeit unterstützten Versionen auf.
>
> Sie können **Vorschauversionen** anfügen. Vorschaufunktionen werden ohne Vereinbarung zum Servicelevel bereitgestellt und sind nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Die Unterstützung für die Verwendung von Vorschauversionen ist möglicherweise eingeschränkt. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="available-and-default-versions"></a>Verfügbare und Standardversionen

Um die verfügbaren und Standard-AKS-Versionen zu ermitteln, verwenden Sie den [Azure CLI](/cli/azure/install-azure-cli)-Befehl [az aks get-versions](/cli/azure/aks#az_aks_get_versions). Mit dem folgenden Befehl werden beispielsweise die in der Region „USA, Westen“ verfügbaren Versionen zurückgegeben:

```azurecli-interactive
az aks get-versions -l westus -o table
```

Die Ausgabe dieses Befehls ähnelt dem folgenden Text:

```text
KubernetesVersion    Upgrades
-------------------  ----------------------------------------
1.18.6(preview)      None available
1.18.4(preview)      1.18.6(preview)
1.17.9               1.18.4(preview), 1.18.6(preview)
1.17.7               1.17.9, 1.18.4(preview), 1.18.6(preview)
1.16.13              1.17.7, 1.17.9
1.16.10              1.16.13, 1.17.7, 1.17.9
1.15.12              1.16.10, 1.16.13
1.15.11              1.15.12, 1.16.10, 1.16.13
```

Um die Standardversion zu ermitteln, die beim **Erstellen** eines Clusters über Azure Machine Learning verwendet wird, können Sie den `--query`-Parameter verwenden, um die Standardversion auszuwählen:

```azurecli-interactive
az aks get-versions -l westus --query "orchestrators[?default == `true`].orchestratorVersion" -o table
```

Die Ausgabe dieses Befehls ähnelt dem folgenden Text:

```text
Result
--------
1.16.13
```

Wenn Sie die **verfügbaren Versionen programmgesteuert überprüfen** möchten, verwenden Sie die Container Service Client – List Orchestrators-REST-API. Um die verfügbaren Versionen zu finden, sehen Sie sich die Einträge an, in denen `orchestratorType` den Wert `Kubernetes` hat. Die zugehörigen `orchestrationVersion`-Einträge enthalten die verfügbaren Versionen, die an Ihren Arbeitsbereich **angefügt** werden können.

Um die Standardversion zu ermitteln, die beim **Erstellen** eines Clusters über Azure Machine Learning verwendet wird, suchen Sie den Eintrag, bei dem `orchestratorType` den Wert `Kubernetes` und `default` den Wert `true` hat. Der zugeordnete `orchestratorVersion`-Wert ist die Standardversion. Der folgende JSON-Codeausschnitt zeigt einen Beispieleintrag:

```json
...
 {
        "orchestratorType": "Kubernetes",
        "orchestratorVersion": "1.16.13",
        "default": true,
        "upgrades": [
          {
            "orchestratorType": "",
            "orchestratorVersion": "1.17.7",
            "isPreview": false
          }
        ]
      },
...
```

## <a name="create-a-new-aks-cluster"></a>Erstellen eines neuen AKS-Clusters

**Geschätzter Zeitaufwand**: Ca. zehn Minuten.

Das Erstellen oder Anfügen eines AKS-Clusters ist ein für Ihren Arbeitsbereich einmaliger Vorgang. Sie können diesen Cluster für mehrere Bereitstellungen wiederverwenden. Wenn Sie den Cluster oder die Ressourcengruppe löschen, die ihn enthält, müssen Sie bei der nächsten Bereitstellung einen neuen Cluster erstellen. Sie können an Ihren Arbeitsbereich mehrere AKS-Cluster anfügen.

Das folgende Beispiel veranschaulicht, wie ein neuer AKS-Cluster mit dem SDK und der CLI erstellt wird:

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
prov_config = AksCompute.provisioning_configuration()

# Example configuration to use an existing virtual network
# prov_config.vnet_name = "mynetwork"
# prov_config.vnet_resourcegroup_name = "mygroup"
# prov_config.subnet_name = "default"
# prov_config.service_cidr = "10.0.0.0/16"
# prov_config.dns_service_ip = "10.0.0.10"
# prov_config.docker_bridge_cidr = "172.17.0.1/16"

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
```

Weitere Informationen zu den in diesem Beispiel verwendeten Klassen, Methoden und Parametern finden Sie in den folgenden Referenzdokumenten:

* [AksCompute.ClusterPurpose](/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose)
* [AksCompute.provisioning_configuration](/python/api/azureml-core/azureml.core.compute.akscompute#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [ComputeTarget.create](/python/api/azureml-core/azureml.core.compute.computetarget#create-workspace--name--provisioning-configuration-)
* [ComputeTarget.wait_for_completion](/python/api/azureml-core/azureml.core.compute.computetarget#wait-for-completion-show-output-false-)

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli
az ml computetarget create aks -n myaks
```

Weitere Informationen finden Sie unter [az ml computetarget create aks](/cli/azure/ml(v1)/computetarget/create#az_ml_computetarget_create_aks).

# <a name="portal"></a>[Portal](#tab/azure-portal)

Weitere Informationen zum Erstellen eines AKS-Clusters im Portal finden Sie unter [Erstellen von Computezielen in Azure Machine Learning Studio](how-to-create-attach-compute-studio.md#inference-clusters).

---

## <a name="attach-an-existing-aks-cluster"></a>Anfügen eines vorhandenen AKS-Clusters

**Geschätzter Zeitaufwand:** Ca. fünf Minuten.

Wenn Sie in Ihrem Azure-Abonnement bereits über einen AKS-Cluster verfügen, können Sie diesen für Ihren Arbeitsbereich verwenden.

> [!TIP]
> Der vorhandene AKS-Cluster kann sich in einer anderen Azure-Region befinden als Ihr Azure Machine Learning-Arbeitsbereich.


> [!WARNING]
> Erstellen Sie nicht mehrere gleichzeitige Verknüpfungen für einen AKS-Cluster in Ihrem Arbeitsbereich. Verwenden Sie beispielsweise beim Verknüpfen eines AKS-Clusters mit einem Arbeitsbereich nicht zwei unterschiedliche Namen. Jede neue Verknüpfung führt zu einem Fehler der vorherigen vorhandenen Verknüpfungen.
>
> Falls Sie einen AKS-Cluster erneut verknüpfen möchten (etwa zum Ändern der TLS-Einstellung oder einer anderen Clusterkonfigurationseinstellung), müssen Sie zuerst die vorhandene Verknüpfung mithilfe von [AksCompute.detach()](/python/api/azureml-core/azureml.core.compute.akscompute#detach--) entfernen.

Weitere Informationen zum Erstellen eines AKS-Clusters mithilfe der Azure-CLI oder des Portals finden Sie in den folgenden Artikeln:

* [Erstellen eines AKS-Clusters (CLI)](/cli/azure/aks?bc=%2fazure%2fbread%2ftoc.json&toc=%2fazure%2faks%2fTOC.json#az_aks_create)
* [Schnellstart: Bereitstellen eines AKS-Clusters (Azure Kubernetes Service) über das Azure-Portal](../aks/kubernetes-walkthrough-portal.md)
* [Erstellen eines AKS-Clusters (ARM-Vorlage auf Azure-Schnellstartvorlagen)](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.containerinstance/aks-azml-targetcompute)

Das folgende Beispiel veranschaulicht das Anfügen eines vorhandenen AKS-Clusters an Ihren Arbeitsbereich:

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'myexistingcluster'

# Attach the cluster to your workgroup. If the cluster has less than 12 virtual CPUs, use the following instead:
# attach_config = AksCompute.attach_configuration(resource_group = resource_group,
#                                         cluster_name = cluster_name,
#                                         cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'myaks', attach_config)

# Wait for the attach process to complete
aks_target.wait_for_completion(show_output = True)
```

Weitere Informationen zu den in diesem Beispiel verwendeten Klassen, Methoden und Parametern finden Sie in den folgenden Referenzdokumenten:

* [AksCompute.attach_configuration()](/python/api/azureml-core/azureml.core.compute.akscompute#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [AksCompute.ClusterPurpose](/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose)
* [AksCompute.attach](/python/api/azureml-core/azureml.core.compute.computetarget#attach-workspace--name--attach-configuration-)

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Um einen vorhandenen Cluster mithilfe der CLI anzufügen, müssen Sie die Ressourcen-ID des vorhandenen Clusters abrufen. Verwenden Sie zum Abrufen dieses Werts den folgenden Befehl. Ersetzen Sie `myexistingcluster` durch den Namen Ihres AKS-Clusters. Ersetzen Sie `myresourcegroup` durch die Ressourcengruppe, die den Cluster enthält:

```azurecli
az aks show -n myexistingcluster -g myresourcegroup --query id
```

Dieser Befehl gibt einen Wert zurück, der in etwa wie folgt aussieht:

```text
/subscriptions/{GUID}/resourcegroups/{myresourcegroup}/providers/Microsoft.ContainerService/managedClusters/{myexistingcluster}
```

Um den vorhandenen Cluster an Ihren Arbeitsbereich anzufügen, verwenden Sie den folgenden Befehl. Ersetzen Sie `aksresourceid` durch den Wert, der über den vorherigen Befehl zurückgegeben wurde. Ersetzen Sie `myresourcegroup` durch die Ressourcengruppe, die Ihren Arbeitsbereich enthält. Ersetzen Sie `myworkspace` durch den Namen Ihres Arbeitsbereichs.

```azurecli
az ml computetarget attach aks -n myaks -i aksresourceid -g myresourcegroup -w myworkspace
```

Weitere Informationen finden Sie unter [az ml computetarget attach aks](/cli/azure/ml(v1)/computetarget/attach#az_ml_computetarget_attach_aks).

# <a name="portal"></a>[Portal](#tab/azure-portal)

Informationen zum Anfügen eines AKS-Clusters im Portal finden Sie unter [Erstellen von Computezielen in Azure Machine Learning Studio](how-to-create-attach-compute-studio.md#inference-clusters).

---

## <a name="create-or-attach-an-aks-cluster-with-tls-termination"></a>Erstellen oder Anfügen eines AKS-Clusters mit TLS-Abschluss
Wenn Sie [einen AKS-Cluster erstellen oder anfügen](how-to-create-attach-kubernetes.md), können Sie den TLS-Abschluss mit den Konfigurationsobjekten von **[AksCompute.provisioning_configuration()](/python/api/azureml-core/azureml.core.compute.akscompute#provisioning-configuration-agent-count-none--vm-size-none--ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--location-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--service-cidr-none--dns-service-ip-none--docker-bridge-cidr-none--cluster-purpose-none--load-balancer-type-none--load-balancer-subnet-none-)** und **[AksCompute.attach_configuration()](/python/api/azureml-core/azureml.core.compute.akscompute#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)** aktivieren. Bei beiden Methoden wird ein Konfigurationsobjekt zurückgegeben, das über eine **enable_ssl**-Methode verfügt. Mit dieser **enable_ssl**-Methode können Sie TLS aktivieren.

Im folgenden Beispiel wird gezeigt, wie Sie den TLS-Abschluss mit automatischer TLS-Zertifikatgenerierung und -konfiguration aktivieren. Dabei kommt im Hintergrund ein Microsoft-Zertifikat zum Einsatz.
```python
   from azureml.core.compute import AksCompute, ComputeTarget
   
   # Enable TLS termination when you create an AKS cluster by using provisioning_config object enable_ssl method

   # Leaf domain label generates a name using the formula
   # "<leaf-domain-label>######.<azure-region>.cloudapp.azure.com"
   # where "######" is a random series of characters
   provisioning_config.enable_ssl(leaf_domain_label = "contoso")
   
   # Enable TLS termination when you attach an AKS cluster by using attach_config object enable_ssl method

   # Leaf domain label generates a name using the formula
   # "<leaf-domain-label>######.<azure-region>.cloudapp.azure.com"
   # where "######" is a random series of characters
   attach_config.enable_ssl(leaf_domain_label = "contoso")


```
Das folgende Beispiel zeigt, wie Sie den TLS-Abschluss mit benutzerdefiniertem Zertifikat und benutzerdefiniertem Domänennamen aktivieren. Sie müssen Ihren DNS-Eintrag mit der benutzerdefinierten Domäne und dem Zertifikat so aktualisieren, dass er auf die IP-Adresse des Bewertungsendpunkts verweist. Weitere Informationen dazu finden Sie unter [Aktualisieren Ihres DNS](how-to-secure-web-service.md#update-your-dns).

```python
   from azureml.core.compute import AksCompute, ComputeTarget

   # Enable TLS termination with custom certificate and custom domain when creating an AKS cluster
   
   provisioning_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    
   # Enable TLS termination with custom certificate and custom domain when attaching an AKS cluster

   attach_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")


```
>[!NOTE]
> Weitere Informationen zum Schützen von Modellimplementierungen im AKS-Cluster finden Sie unter [Verwenden von TLS zum Absichern eines Webdiensts mit Azure Machine Learning](how-to-secure-web-service.md).

## <a name="create-or-attach-an-aks-cluster-to-use-internal-load-balancer-with-private-ip"></a>Erstellen oder Anfügen eines AKS-Clusters für die Verwendung des internen Lastenausgleichs mit einer privaten IP-Adresse
Wenn Sie einen AKS-Cluster erstellen oder anfügen, können Sie den Cluster für die Verwendung eines internen Lastenausgleichs konfigurieren. Mit einem internen Lastenausgleich nutzen die Bewertungsendpunkte für Ihre Bereitstellungen in AKS eine private IP-Adresse innerhalb des virtuellen Netzwerks. Die folgenden Codeausschnitte zeigen, wie Sie einen internen Lastenausgleich für einen AKS-Cluster konfigurieren.
```python
   
   from azureml.core.compute.aks import AksUpdateConfiguration
   from azureml.core.compute import AksCompute, ComputeTarget
   
   # When you create an AKS cluster, you can specify Internal Load Balancer to be created with provisioning_config object
   provisioning_config = AksCompute.provisioning_configuration(load_balancer_type = 'InternalLoadBalancer')

   # when you attach an AKS cluster, you can update the cluster to use internal load balancer after attach
   aks_target = AksCompute(ws,"myaks")

   # Change to the name of the subnet that contains AKS
   subnet_name = "default"
   # Update AKS configuration to use an internal load balancer
   update_config = AksUpdateConfiguration(None, "InternalLoadBalancer", subnet_name)
   aks_target.update(update_config)
   # Wait for the operation to complete
   aks_target.wait_for_completion(show_output = True)
   
   
```
>[!IMPORTANT]
> Azure Machine Learning unterstützt den TLS-Abschluss mit internem Lastenausgleich nicht. Der interne Lastenausgleich verfügt über eine private IP-Adresse, die sich auch in einem anderen Netzwerk befinden kann. Außerdem kann das Zertifikat wiederverwendet werden. 

>[!NOTE]
> Weitere Informationen zum Schützen einer Rückschlussumgebung finden Sie unter [Schützen einer Azure Machine Learning-Rückschlussumgebung mit virtuellen Netzwerken](how-to-secure-inferencing-vnet.md).

## <a name="detach-an-aks-cluster"></a>Trennen eines AKS-Clusters

Verwenden Sie eine der folgenden Methoden, um einen Cluster von Ihrem Arbeitsbereich zu trennen:

> [!WARNING]
> Wenn Sie einen AKS-Cluster mit dem Azure Machine Learning Studio, dem SDK oder der Azure CLI-Erweiterung für maschinelles Lernen trennen, **wird der AKS-Cluster nicht gelöscht**. Informationen zum Löschen des Clusters finden Sie unter [Schnellstart: Bereitstellen eines Azure Kubernetes Service-Clusters über die Azure-Befehlszeilenschnittstelle](../aks/kubernetes-walkthrough.md#delete-the-cluster).

# <a name="python"></a>[Python](#tab/python)

```python
aks_target.detach()
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Um den vorhandenen Cluster von Ihrem Arbeitsbereich zu trennen, verwenden Sie den folgenden Befehl. Ersetzen Sie `myaks` durch den Namen, unter dem der AKS-Cluster an Ihren Arbeitsbereich angefügt ist. Ersetzen Sie `myresourcegroup` durch die Ressourcengruppe, die Ihren Arbeitsbereich enthält. Ersetzen Sie `myworkspace` durch den Namen Ihres Arbeitsbereichs.

```azurecli
az ml computetarget detach -n myaks -g myresourcegroup -w myworkspace
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

Wählen Sie im Azure Machine Learning Studio __Compute__, __Rückschlusscluster__ und den Cluster aus, den Sie entfernen möchten. Verwenden Sie den Link __Trennen__, um den Cluster zu trennen.

---

## <a name="troubleshooting"></a>Problembehandlung
### <a name="update-the-cluster"></a>Aktualisieren des Clusters

Updates für Azure Machine Learning-Komponenten, die in einem Azure Kubernetes Service-Cluster installiert sind, müssen manuell angewendet werden. 

Sie können diese Updates anwenden, indem Sie den Cluster vom Azure Machine Learning-Arbeitsbereich trennen und ihn dann erneut an den Arbeitsbereich anfügen. Wenn TLS im Cluster aktiviert ist, müssen Sie das TLS/SSL-Zertifikat und den privaten Schlüssel bereitstellen, wenn Sie den Cluster erneut anfügen. 

```python
compute_target = ComputeTarget(workspace=ws, name=clusterWorkspaceName)
compute_target.detach()
compute_target.wait_for_completion(show_output=True)

attach_config = AksCompute.attach_configuration(resource_group=resourceGroup, cluster_name=kubernetesClusterName)

## If SSL is enabled.
attach_config.enable_ssl(
    ssl_cert_pem_file="cert.pem",
    ssl_key_pem_file="key.pem",
    ssl_cname=sslCname)

attach_config.validate_configuration()

compute_target = ComputeTarget.attach(workspace=ws, name=args.clusterWorkspaceName, attach_configuration=attach_config)
compute_target.wait_for_completion(show_output=True)
```

Wenn Sie das TLS/SSL-Zertifikat und den privaten Schlüssel nicht mehr haben, oder wenn Sie ein von Azure Machine Learning generiertes Zertifikat verwenden, können Sie die Dateien vor dem Abtrennen des Clusters abrufen, indem Sie mit `kubectl` eine Verbindung mit dem Cluster herstellen und das Geheimnis `azuremlfessl` abrufen.

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes speichert die Geheimnisse im Base64-codierten Format. Sie müssen die `cert.pem`- und die `key.pem`-Komponente jedes Geheimnisses aus dem Base64-Format decodieren, bevor Sie diese Komponenten für `attach_config.enable_ssl` bereitstellen. 

### <a name="webservice-failures"></a>Webdienstfehler

Viele Webdienstfehler in AKS können debuggt werden, indem mit `kubectl` eine Verbindung mit dem Cluster hergestellt wird. Sie können die Datei `kubeconfig.json` für einen AKS-Cluster abrufen, indem Sie Folgendes ausführen:

```azurecli-interactive
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure RBAC für die Kubernetes-Autorisierung](../aks/manage-azure-rbac.md)
* [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md)
* [Bereitstellen eines Modells in einem Azure Kubernetes Service-Cluster](how-to-deploy-azure-kubernetes-service.md)
