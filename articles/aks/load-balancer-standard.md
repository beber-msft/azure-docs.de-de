---
title: Verwenden eines öffentlichen Lastenausgleichsmoduls
titleSuffix: Azure Kubernetes Service
description: Hier erfahren Sie, wie Sie ein öffentliches Lastenausgleichsmodul mit einer Standard-SKU verwenden, um Ihre Dienste mit Azure Kubernetes Service (AKS) verfügbar zu machen.
services: container-service
ms.topic: article
ms.date: 11/14/2020
ms.author: jpalma
author: palma21
ms.openlocfilehash: 1da7c5f189b3ffee7f74a4af94bda7fe2d755c74
ms.sourcegitcommit: 4cd97e7c960f34cb3f248a0f384956174cdaf19f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2021
ms.locfileid: "132025777"
---
# <a name="use-a-public-standard-load-balancer-in-azure-kubernetes-service-aks"></a>Verwenden einer öffentlichen Instanz von Load Balancer Standard in Azure Kubernetes Service (AKS)

Azure Load Balancer befindet sich auf L4 des OSI-Modells (Open Systems Interconnection), wofür sowohl Eingangs- als auch Ausgangsszenarien unterstützt werden. Hierbei werden Datenflüsse, die beim Front-End des Lastenausgleichs eingehen, auf die Instanzen des Back-End-Pools verteilt.

Ein **öffentlicher** Lastenausgleich, der in AKS integriert ist, hat zwei Zwecke:

1. Bereitstellen von ausgehenden Verbindungen mit den Clusterknoten im virtuellen AKS-Netzwerk. Dies wird erreicht, indem die private IP-Adresse eines Knotens in eine öffentliche IP-Adresse übersetzt wird, die Teil des zugehörigen *Ausgangspools* ist.
2. Ermöglichen des Zugriffs auf Anwendungen über Kubernetes-Dienste vom Typ `LoadBalancer`. Hiermit ist es einfach, Ihre Anwendung zu skalieren und hoch verfügbare Dienste zu erstellen.

Ein **internes (oder privates)** Lastenausgleichsmodul wird verwendet, wenn nur private IP-Adressen als Front-End zulässig sind. Interne Lastenausgleichsmodule werden verwendet, um einen Lastausgleich für Datenverkehr innerhalb eines virtuellen Netzwerks vorzunehmen. Auf ein Lastenausgleichs-Front-End kann auch über ein lokales Netzwerk in einem Hybridszenario zugegriffen werden.

In diesem Dokument wird die Integration mit dem öffentlichen Lastenausgleichsmodul beschrieben. Informationen zur Integration mit dem internen Lastenausgleichsmodul finden Sie in der [Dokumentation zum internen AKS-Lastenausgleichsmodul](internal-lb.md).

## <a name="before-you-begin"></a>Voraussetzungen

Azure Load Balancer ist in zwei SKUs verfügbar: *Basic* und *Standard*. Beim Erstellen eines AKS-Clusters wird standardmäßig die SKU *Standard* verwendet. Verwenden Sie die SKU *Standard*, um Zugriff auf weitere Funktionen zu erhalten, z. B. einen größeren Back-End-Pool, [**Pools mit mehreren Knoten**](use-multiple-node-pools.md) und [**Verfügbarkeitszonen**](availability-zones.md). Dies ist die empfohlene Load Balancer-SKU für AKS.

Weitere Informationen zu den SKUs *Basic* und *Standard* finden Sie unter [Vergleich der Load Balancer-SKUs][azure-lb-comparison].

In diesem Artikel wird davon ausgegangen, dass Sie über einen AKS-Cluster mit der SKU *Standard* verfügen. Es wird Schritt für Schritt beschrieben, wie Sie einige Funktionen und Features des Lastenausgleichsmoduls nutzen und konfigurieren. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

> [!IMPORTANT]
> Falls Sie Azure Load Balancer nicht zum Bereitstellen einer ausgehenden Verbindung nutzen möchten, sondern hierfür lieber Ihr eigenes Gateway, eine Firewall oder einen Proxy verwenden möchten, können Sie die Erstellung des Ausgangspools für den Lastenausgleich und der entsprechenden Front-End-IP-Adresse überspringen. Fahren Sie in diesem Fall mit [**Anpassen des ausgehenden Clusterdatenverkehrs mit einer benutzerdefinierten Route**](egress-outboundtype.md) fort. Mit dem „Ausgangstyp“ (outboundType) wird die Ausgangsmethode für einen Cluster definiert, und standardmäßig wird der Typ „loadBalancer“ verwendet.

## <a name="use-the-public-standard-load-balancer"></a>Verwenden der öffentlichen Load Balancer Standard-Instanz

Nachdem Sie einen AKS-Cluster mit „outboundType: loadBalancer“ (Standardeinstellung) erstellt haben, kann das Lastenausgleichsmodul vom Cluster verwendet werden, um auch Dienste verfügbar zu machen.

Hierfür können Sie einen öffentlichen Dienst vom Typ `LoadBalancer` erstellen. Dies ist im folgenden Beispiel dargestellt. Erstellen Sie zunächst ein Dienstmanifest mit dem Namen `public-svc.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: public-svc
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: public-app
```

Stellen Sie das öffentliche Dienstmanifest bereit, indem Sie [kubectl apply][kubectl-apply] verwenden und den Namen Ihres YAML-Manifests angeben:

```azurecli-interactive
kubectl apply -f public-svc.yaml
```

Azure Load Balancer wird mit einer neuen öffentlichen IP-Adresse als Front-End für diesen neuen Dienst konfiguriert. Da Azure Load Balancer über mehrere Front-End-IP-Adressen verfügen kann, erhält jeder neue bereitgestellte Dienst eine neue dedizierte Front-End-IP-Adresse, damit der eindeutige Zugriff sichergestellt ist.

Sie können überprüfen, ob Ihr Dienst erstellt und der Lastenausgleich konfiguriert wurde, indem Sie beispielsweise Folgendes ausführen:

```azurecli-interactive
kubectl get service public-svc
```

```console
NAMESPACE     NAME          TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)         AGE
default       public-svc    LoadBalancer   10.0.39.110    52.156.88.187   80:32068/TCP    52s
```

Wenn Sie die Dienstdetails anzeigen, finden Sie die öffentliche IP-Adresse, die für diesen Dienst auf dem Lastenausgleich erstellt wurde, in der Spalte *EXTERNAL-IP*. Es kann ein oder zwei Minuten dauern, bis sich die IP-Adresse wie im obigen Beispiel von *\<pending\>* in eine tatsächliche öffentliche IP-Adresse ändert.

## <a name="configure-the-public-standard-load-balancer"></a>Konfigurieren der öffentlichen Load Balancer Standard-Instanz

Bei Verwendung der SKU „Standard“ für den öffentlichen Lastenausgleich können Sie bei der Erstellung oder beim Aktualisieren des Clusters einige Optionen anpassen. Da Sie mit diesen Optionen den Lastenausgleich so anpassen können, dass er Ihre Workloadanforderungen erfüllt, sollten Sie sie entsprechend überprüfen. Mit Load Balancer Standard haben Sie folgende Möglichkeiten:

* Festlegen oder Skalieren der Anzahl von verwalteten ausgehenden IP-Adressen
* Verwenden von eigenen benutzerdefinierten [IP-Ausgangsadressen oder entsprechenden Präfixen](#provide-your-own-outbound-public-ips-or-prefixes)
* Anpassen der Anzahl von zugeordneten ausgehenden Ports an die einzelnen Knoten des Clusters
* Konfigurieren der Timeouteinstellung für Verbindungen im Leerlauf

> [!IMPORTANT]
> Nur eine ausgehende IP-Option (verwaltete IPs, Bring Your Own IP oder IP-Präfix) kann zu einem bestimmten Zeitpunkt verwendet werden.

### <a name="scale-the-number-of-managed-outbound-public-ips"></a>Skalieren der Anzahl von verwalteten öffentlichen IP-Ausgangsadressen

Azure Load Balancer unterstützt zusätzlich zu eingehenden Verbindungen auch ausgehende Verbindungen eines virtuellen Netzwerks. Mit Ausgangsregeln können Sie die Netzwerkadressenübersetzung (NAT) einer öffentlichen Load Balancer Standard-Instanz für ausgehenden Datenverkehr ganz einfach konfigurieren.

Wie alle Load Balancer-Regeln folgen auch die Ausgangsregeln der gleichen vertrauten Syntax wie Lastenausgleichsregeln und NAT-Eingangsregeln:

***Front-End-IP-Adressen + Parameter + Back-End-Pool***

Eine Ausgangsregel konfiguriert die NAT für ausgehenden Datenverkehr für alle VMs, die vom Back-End-Pool für die Front-End-Übersetzung identifiziert wurden. Darüber hinaus ermöglichen Parameter eine zusätzliche differenzierte Steuerung des NAT-Algorithmus für ausgehenden Datenverkehr.

Ausgangsregeln können jeweils mit nur einer einzigen öffentlichen IP-Adresse verwendet werden und erleichtern die Konfiguration beim Skalieren der NAT für ausgehenden Datenverkehr. Sie können mehrere IP-Adressen verwenden, um umfangreiche Szenarien zu planen. Mithilfe von Ausgangsregeln lassen sich außerdem die für die SNAT-Überlastung anfälligen Muster reduzieren. Jede zusätzliche IP-Adresse, die von einem Front-End bereitgestellt wird, stellt 64.000 kurzlebige Ports zur Verfügung, die Load Balancer als SNAT-Ports verwenden kann. 

Wenn Sie einen Lastenausgleich mit der SKU *Standard* mit verwalteten ausgehenden öffentlichen IP-Adressen verwenden, die standardmäßig erstellt werden, können Sie die Anzahl verwalteter ausgehender öffentlicher IP-Adressen mit dem Parameter **`load-balancer-managed-ip-count`** skalieren.

Zum Aktualisieren eines vorhandenen Clusters führen Sie den unten angegebenen Befehl aus. Dieser Parameter kann auch zum Zeitpunkt der Clustererstellung festgelegt werden, um mehrere verwaltete ausgehende öffentliche IP-Adressen zu erhalten.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-managed-outbound-ip-count 2
```

Im obigen Beispiel wird die Anzahl der verwalteten ausgehenden öffentlichen IP-Adressen für den *myAKSCluster*-Cluster in *myResourceGroup* auf *2* festgelegt. 

Sie können auch den Parameter **`load-balancer-managed-ip-count`** verwenden, um die anfängliche Anzahl von verwalteten ausgehenden öffentlichen IP-Adressen beim Erstellen des Clusters festzulegen. Hierfür fügen Sie den Parameter **`--load-balancer-managed-outbound-ip-count`** an und legen ihn auf den gewünschten Wert fest. Die Standardanzahl von verwalteten ausgehenden öffentlichen IP-Adressen beträgt 1.

### <a name="provide-your-own-outbound-public-ips-or-prefixes"></a>Verwenden von eigenen öffentlichen IP-Ausgangsadressen oder Präfixen

Bei Verwendung eines Lastenausgleichs mit der SKU *Standard* wird vom AKS-Cluster standardmäßig automatisch eine öffentliche IP-Adresse in der von AKS verwalteten Infrastrukturressourcengruppe erstellt und dem Ausgangspool des Lastenausgleichs zugewiesen.

Eine öffentliche IP-Adresse, die von AKS erstellt wird, wird als verwaltete AKS-Ressource angesehen. Dies bedeutet, dass der Lebenszyklus dieser öffentlichen IP-Adresse von AKS verwaltet werden sollte und dass direkt auf der öffentlichen IP-Ressource kein Benutzereingriff erforderlich ist. Alternativ können Sie Ihre eigene benutzerdefinierte öffentliche IP-Adresse bzw. das zugehörige Präfix bei der Erstellung des Clusters zuweisen. Ihre benutzerdefinierten IP-Adressen können auch in den Lastenausgleichseigenschaften eines vorhandenen Clusters aktualisiert werden.

Anforderungen für die Verwendung Ihrer eigenen öffentlichen IP-Adresse oder Ihres Präfixes:

- Benutzerdefinierte öffentliche IP-Adressen müssen vom Benutzer erstellt werden und sich in seinem Besitz befinden. Verwaltete öffentliche IP-Adressen, die von AKS erstellt werden, können nicht für die Bereitstellung einer eigenen benutzerdefinierten IP-Adresse wiederverwendet werden, weil dies zu Verwaltungskonflikten führen kann.
- Sie müssen sicherstellen, dass die AKS-Clusteridentität (Dienstprinzipal oder verwaltete Identität) über Berechtigungen für den Zugriff auf ausgehende IP-Adressen verfügt. Gemäß der [Liste der erforderlichen Berechtigungen für die öffentliche IP-Adresse](kubernetes-service-principal.md#networking).
- Stellen Sie sicher, dass Sie die vorgegebenen [Voraussetzungen und Einschränkungen](../virtual-network/ip-services/public-ip-address-prefix.md#limitations) erfüllen, die für die Konfiguration von ausgehenden IP-Adressen und der zugehörigen Präfixe gelten.

#### <a name="update-the-cluster-with-your-own-outbound-public-ip"></a>Aktualisieren des Clusters mit Ihrer eigenen ausgehenden öffentlichen IP-Adresse

Verwenden Sie den Befehl [az network public-ip show][az-network-public-ip-show], um die IDs Ihrer öffentlichen IP-Adressen aufzulisten.

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIP --query id -o tsv
```

Der obige Befehl zeigt die ID für die öffentliche IP-Adresse *myPublicIP* in der Ressourcengruppe *myResourceGroup* an.

Verwenden Sie den Befehl `az aks update` mit dem Parameter **`load-balancer-outbound-ips`** , um Ihren Cluster mit Ihren öffentlichen IP-Adressen zu aktualisieren.

Im folgenden Beispiel wird der Parameter `load-balancer-outbound-ips` mit den IDs aus dem vorherigen Befehl verwendet.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

#### <a name="update-the-cluster-with-your-own-outbound-public-ip-prefix"></a>Aktualisieren des Clusters mit Ihrem eigenen Präfix für eine ausgehende öffentliche IP-Adresse

Sie können auch öffentliche IP-Präfixe für ausgehenden Datenverkehr mit Ihrem *Standard*-SKU-Lastenausgleich verwenden. Im folgenden Beispiel wird der Befehl [az network public-ip prefix show][az-network-public-ip-prefix-show] verwendet, um die IDs Ihrer öffentlichen IP-Präfixe aufzulisten:

```azurecli-interactive
az network public-ip prefix show --resource-group myResourceGroup --name myPublicIPPrefix --query id -o tsv
```

Der obige Befehl zeigt die ID für das öffentliche IP-Präfix *myPublicIPPrefix* in der Ressourcengruppe *myResourceGroup* an.

Im folgenden Beispiel wird der Parameter *load-balancer-outbound-ip-prefixes* mit den IDs aus dem vorherigen Befehl verwendet.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

#### <a name="create-the-cluster-with-your-own-public-ip-or-prefixes"></a>Erstellen des Clusters mit Ihren eigenen öffentlichen IP-Adressen oder Präfixen

Möglicherweise möchten Sie zum Zeitpunkt der Clustererstellung Ihre eigenen IP-Adressen oder IP-Präfixe für den ausgehenden Datenverkehr einbinden, um Szenarien wie das Hinzufügen von Endpunkten für ausgehenden Datenverkehr zu einer Positivliste zu unterstützen. Fügen Sie die oben gezeigten Parameter im Schritt zur Clustererstellung an, um Ihre eigenen öffentlichen IP-Adressen und IP-Präfixe zu Beginn des Lebenszyklus eines Clusters zu definieren.

Verwenden Sie den Befehl *az aks create* mit dem Parameter *load-balancer-outbound-ips*, um einen neuen Cluster mit Ihren öffentlichen IP-Adressen zu Beginn zu erstellen.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

Verwenden Sie den Befehl *az aks create* mit dem Parameter *load-balancer-outbound-ip-prefixes*, um einen neuen Cluster mit Ihren öffentlichen IP-Präfixen zu Beginn zu erstellen.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

### <a name="configure-the-allocated-outbound-ports"></a>Konfigurieren der zugeordneten ausgehenden Ports

> [!IMPORTANT]
> Bei Anwendungen in Ihrem Cluster, die eine große Anzahl von Verbindungen mit kleinen Zielen herstellen können (etwa zahlreiche Instanzen einer Front-End-Anwendung, die eine Verbindung mit einer Datenbank herstellen), kann ein Szenario auftreten, das sehr anfällig für die SNAT-Portauslastung ist. Die SNAT-Portauslastung tritt auf, wenn eine Anwendung nicht mehr über ausgehende Ports zum Herstellen einer Verbindung mit einer anderen Anwendung oder einem anderen Host verfügt. Bei einem Szenario, in dem eine SNAT-Portauslastung auftreten kann, wird dringend empfohlen, die zugeordneten ausgehenden Ports und die Front-End-IP-Ausgangsadressen im Lastenausgleichsmodul zu erhöhen, um eine SNAT-Portauslastung zu verhindern. Weiter unten finden Sie Informationen zum ordnungsgemäßen Berechnen von ausgehenden Ports und Front-End-IP-Ausgangsadressen.

AKS legt *AllocatedOutboundPorts* für den Lastenausgleich standardmäßig auf `0` fest. Dadurch wird die [automatische Zuweisung ausgehender Ports basierend auf der Größe des Back-End-Pools][azure-lb-outbound-preallocatedports] ermöglicht, wenn Sie einen Cluster erstellen. Wenn ein Cluster beispielsweise über 50 oder weniger Knoten verfügt, werden jedem Knoten 1.024 Ports zugeordnet. Wenn sich die Anzahl der Knoten im Cluster erhöht, sind weniger Ports pro Knoten verfügbar. Verwenden Sie `az network lb outbound-rule list`, um den Wert von *AllocatedOutboundPorts* für den Lastenausgleich des AKS-Clusters anzuzeigen. Beispiel:

```azurecli-interactive
NODE_RG=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
az network lb outbound-rule list --resource-group $NODE_RG --lb-name kubernetes -o table
```

Die folgende Beispielausgabe zeigt, dass die automatische Zuweisung ausgehender Ports basierend auf der Größe des Back-End-Pools für den Cluster aktiviert ist:

```console
AllocatedOutboundPorts    EnableTcpReset    IdleTimeoutInMinutes    Name             Protocol    ProvisioningState    ResourceGroup
------------------------  ----------------  ----------------------  ---------------  ----------  -------------------  -------------
0                         True              30                      aksOutboundRule  All         Succeeded            MC_myResourceGroup_myAKSCluster_eastus  
```

Verwenden Sie `load-balancer-outbound-ports` und entweder `load-balancer-managed-outbound-ip-count`, `load-balancer-outbound-ips` oder `load-balancer-outbound-ip-prefixes`, um einen bestimmten Wert für *AllocatedOutboundPorts* und die ausgehende IP-Adresse zu konfigurieren. Bevor Sie einen bestimmten Wert festlegen oder einen vorhandenen Wert für ausgehende Ports und IP-Adressen erhöhen, müssen Sie die entsprechende Anzahl ausgehender Ports und IP-Adressen berechnen. Verwenden Sie für diese Berechnung die folgende Gleichung, gerundet auf die nächste ganze Zahl: `64,000 ports per IP / <outbound ports per node> * <number of outbound IPs> = <maximum number of nodes in the cluster>`.

Beachten Sie Folgendes, wenn Sie die Anzahl ausgehender Ports und IP-Adressen berechnen und die Werte festlegen:
* Die Anzahl ausgehender Ports pro Knoten hängt von dem Wert ab, den Sie festlegen.
* Der Wert für ausgehende Ports muss ein Vielfaches von 8 sein.
* Durch das Hinzufügen weiterer IP-Adressen werden einem Knoten keine weiteren Ports hinzugefügt. Es wird Kapazität für weitere Knoten im Cluster geschaffen.
* Sie müssen Knoten berücksichtigen, die unter Umständen im Rahmen von Upgrades hinzugefügt werden, einschließlich der Anzahl von Knoten, die über [maxSurge-Werte][maxsurge] angegeben werden.

Die folgenden Beispiele zeigen, wie die Anzahl der ausgehenden Ports und IP-Adressen von den von Ihnen festgelegten Werten beeinflusst wird:
- Wenn die Standardwerte verwendet werden und der Cluster über 48 Knoten verfügt, stehen jedem Knoten 1.024 Ports zur Verfügung.
- Wenn die Standardwerte verwendet werden und der Cluster von 48 auf 52 Knoten skaliert wird, wird bei jedem Knoten die Anzahl verfügbarer Ports von 1.024 auf 512 aktualisiert.
- Wenn Sie die Anzahl ausgehender Ports auf 1.000 und die Anzahl ausgehender IP-Adressen auf 2 festlegen, kann der Cluster maximal 128 Knoten unterstützen: `64,000 ports per IP / 1,000 ports per node * 2 IPs = 128 nodes`.
- Wenn Sie die Anzahl ausgehender Ports auf 1.000 und die Anzahl ausgehender IP-Adressen auf 7 festlegen, kann der Cluster maximal 448 Knoten unterstützen: `64,000 ports per IP / 1,000 ports per node * 7 IPs = 448 nodes`.
- Wenn Sie die Anzahl ausgehender Ports auf 4.000 und die Anzahl ausgehender IP-Adressen auf 2 festlegen, kann der Cluster maximal 32 Knoten unterstützen: `64,000 ports per IP / 4,000 ports per node * 2 IPs = 32 nodes`.
- Wenn Sie die Anzahl ausgehender Ports auf 4.000 und die Anzahl ausgehender IP-Adressen auf 7 festlegen, kann der Cluster maximal 112 Knoten unterstützen: `64,000 ports per IP / 4,000 ports per node * 7 IPs = 112 nodes`.

> [!IMPORTANT]
> Nachdem Sie die Anzahl ausgehender Ports und IP-Adressen berechnet haben, überprüfen Sie, ob Sie über zusätzliche Kapazität für ausgehende Ports verfügen, um den Knotenanstieg während Upgrades zu bewältigen. Es ist sehr wichtig, eine ausreichende Zahl überschüssiger Ports für zusätzliche Knoten zuzuordnen, die für Upgrades und andere Vorgänge benötigt werden. AKS verwendet standardmäßig einen Pufferknoten für Upgradevorgänge. Bei Verwendung von [maxSurge-Werten][maxsurge] müssen Sie die ausgehenden Ports pro Knoten mit Ihrem maxSurge-Wert multiplizieren, um die Anzahl der erforderlichen Ports zu bestimmen. Sie haben beispielsweise berechnet, dass Sie 4.000 Ports pro Knoten mit 7 IP-Adressen in einem Cluster mit maximal 100 Knoten und einem maximalen Anstieg von 2 benötigen:
> * 2 Surge-Knoten · 4.000 Ports pro Knoten = 8.000 Ports für den Knotenanstieg während Upgrades erforderlich
> * 100 Knoten · 4.000 Ports pro Knoten = 400.000 Ports für Ihren Cluster erforderlich
> * 7 IP-Adressen · 64.000 Ports pro IP-Adresse = 448.000 Ports für Ihren Cluster verfügbar
>
> Das obige Beispiel zeigt, dass der Cluster über eine überschüssige Kapazität von 48.000 Ports verfügt. Dies ist ausreichend, um die 8.000 Ports zu verarbeiten, die für den Knotenanstieg während Upgrades erforderlich sind.

Nachdem die Werte berechnet und überprüft wurden, können Sie diese Werte mit `load-balancer-outbound-ports` und entweder `load-balancer-managed-outbound-ip-count`, `load-balancer-outbound-ips` oder `load-balancer-outbound-ip-prefixes` beim Erstellen oder Aktualisieren eines Clusters anwenden. Beispiel:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-managed-outbound-ip-count 7 \
    --load-balancer-outbound-ports 4000
```

### <a name="configure-the-load-balancer-idle-timeout"></a>Konfigurieren des Leerlauftimeouts für den Load Balancer

Sobald die SNAT-Portressourcen erschöpft sind, sind ausgehende Datenflüsse erst wieder möglich, wenn SNAT-Ports von vorhandenen Datenflüssen freigegeben werden. Der Load Balancer gibt die SNAT-Ports wieder frei, wenn der Datenflussvorgang abgeschlossen ist. Für den von AKS konfigurierten Lastenausgleich wird ein Leerlauftimeout von 30 Minuten verwendet, um SNAT-Ports für im Leerlauf befindliche Datenflüsse wieder freizugeben.
Sie können auch einen Datentransport (z. B. **`TCP keepalives`** ) oder **`application-layer keepalives`** verwenden, um einen im Leerlauf befindlichen Datenfluss zu aktualisieren, und diesen Leerlauftimeout bei Bedarf zurücksetzen. Sie können diesen Timeout anhand dieses Beispiels konfigurieren: 


```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-idle-timeout 4
```

Erwägen Sie die Verwendung eines niedrigen Timeoutwerts, z. B. vier Minuten, falls Sie mit vielen kurzlebigen Verbindungen rechnen – nicht mit Verbindungen, die langlebig sind und ggf. lange Leerlaufdauern aufweisen, z. B. bei der Nutzung von `kubectl proxy` oder `kubectl port-forward`. Wenn TCP-Keepalives verwendet werden, genügt es zudem, sie auf einer Seite der Verbindung zu aktivieren. Beispielsweise reicht es aus, sie nur auf der Serverseite zu aktivieren, um den Leerlauftimer des Datenflusses zurückzusetzen, und es ist nicht erforderlich, TCP-Keepalives auf beiden Seiten zu starten. Ähnliche Konzepte gibt es für die Anwendungsschicht, einschließlich Client/Server-Konfigurationen für Datenbanken. Überprüfen Sie auf der Serverseite, welche Optionen es für anwendungsspezifische Keepalives gibt.

> [!IMPORTANT]
> AKS ermöglicht bei einem Leerlauf standardmäßig die TCP-Zurücksetzung. Es wird empfohlen, diese Konfiguration beizubehalten und zu nutzen, um für Ihre Szenarien ein besser vorhersagbares Anwendungsverhalten zu erzielen.
> „TCP RST“ wird nur während der TCP-Verbindung im Status „ESTABLISHED“ gesendet. Weitere Informationen hierzu finden Sie [hier](../load-balancer/load-balancer-tcp-reset.md).

Wenn Sie *IdleTimeoutInMinutes* auf einen anderen Wert als den Standardwert von 30 Minuten festlegen, sollten Sie berücksichtigen, wie lange Sie für Ihre Workloads eine ausgehende Verbindung benötigen. Berücksichtigen Sie auch, dass der Standardtimeoutwert für einen Load Balancer mit der SKU *Standard*, der außerhalb von AKS verwendet wird, 4 Minuten beträgt. Ein *IdleTimeoutInMinutes*-Wert, der Ihre spezifische AKS-Workload genauer widerspiegelt, kann zu einer Verringerung der SNAT-Auslastung beitragen. Hierzu kann es kommen, wenn nicht mehr verwendete Verbindungen vorhanden sind.

> [!WARNING]
> Durch das Ändern der Werte für *AllocatedOutboundPorts* und *IdleTimeoutInMinutes* kann sich das Verhalten der Ausgangsregel für Ihren Lastenausgleich erheblich ändern. Dies sollte nicht leichtfertig durchgeführt werden, ohne die möglichen Nachteile und die Verbindungsmuster Ihrer Anwendung zu kennen. Lesen Sie unten den Abschnitt zur [SNAT-Problembehandlung][troubleshoot-snat] und die Informationen zu [Load Balancer-Ausgangsregeln][azure-lb-outbound-rules-overview] und [ausgehenden Verbindungen in Azure][azure-lb-outbound-connections], bevor Sie diese Werte aktualisieren, damit Sie mit den Auswirkungen Ihrer Änderungen vertraut sind.

## <a name="restrict-inbound-traffic-to-specific-ip-ranges"></a>Beschränken des eingehenden Datenverkehrs auf bestimmte IP-Adressbereiche

Im folgenden Manifest wird *loadBalancerSourceRanges* verwendet, um einen neuen IP-Adressbereich für eingehenden externen Datenverkehr anzugeben:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
  loadBalancerSourceRanges:
  - MY_EXTERNAL_IP_RANGE
```

In diesem Beispiel wird die Regel aktualisiert, um eingehenden externen Datenverkehr nur aus dem `MY_EXTERNAL_IP_RANGE`-Bereich zuzulassen. Wenn Sie `MY_EXTERNAL_IP_RANGE` durch die IP-Adresse des internen Subnetzes ersetzen, wird der Datenverkehr nur auf die internen IP-Adressen des Clusters beschränkt. Wenn der Datenverkehr auf interne Cluster-IPs beschränkt ist, können Clients außerhalb Ihres Kubernetes-Clusters nicht auf den Lastenausgleich zugreifen.

> [!NOTE]
> Eingehender, externer Datenverkehr wird vom Lastenausgleich an das virtuelle Netzwerk für Ihren AKS-Cluster geleitet. Das virtuelle Netzwerk verfügt über eine Netzwerksicherheitsgruppe (NSG), die den gesamten eingehenden Datenverkehr vom Lastenausgleich zulässt. Diese NSG verwendet ein [Diensttag][service-tags] vom Typ *LoadBalancer*, um den Datenverkehr vom Lastenausgleich zuzulassen.

## <a name="maintain-the-clients-ip-on-inbound-connections"></a>Beibehalten der IP-Adresse des Clients bei eingehenden Verbindungen

Standardmäßig behält ein Dienst vom Typ `LoadBalancer` [in Kubernetes](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-type-loadbalancer) und in AKS die IP-Adresse des Clients für die Verbindung zum Pod nicht bei. Die Quell-IP-Adresse des Pakets, das an den Pod übermittelt wird, ist die private IP-Adresse des Knotens. Sie müssen in der Dienstdefinition `service.spec.externalTrafficPolicy` auf `local` festlegen, um die IP-Adresse des Clients beizubehalten. Das folgende Manifest zeigt ein Beispiel:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  externalTrafficPolicy: Local
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

## <a name="additional-customizations-via-kubernetes-annotations"></a>Weitere Anpassungen mit Kubernetes-Anmerkungen

Hier ist eine Liste mit Anmerkungen angegeben, die für Kubernetes-Dienste vom Typ `LoadBalancer` unterstützt werden. Diese Anmerkungen gelten nur für eingehende Datenflüsse (**INBOUND**):

| Anmerkung | Wert | BESCHREIBUNG
| ----------------------------------------------------------------- | ------------------------------------- | ------------------------------------------------------------ 
| `service.beta.kubernetes.io/azure-load-balancer-internal`         | `true` oder `false`                     | Geben Sie an, ob es ein interner Lastenausgleich sein soll. Wenn Sie nichts angeben, wird standardmäßig „Öffentlich“ verwendet.
| `service.beta.kubernetes.io/azure-load-balancer-internal-subnet`  | Name des Subnetzes                    | Geben Sie an, an welches Subnetz der interne Lastenausgleich gebunden werden soll. Wenn Sie nichts angeben, wird standardmäßig das in der Cloudkonfigurationsdatei konfigurierte Subnetz verwendet.
| `service.beta.kubernetes.io/azure-dns-label-name`                 | Name der DNS-Bezeichnung von öffentlichen IP-Adressen   | Geben Sie den Namen der DNS-Bezeichnung für den **öffentlichen** Dienst an. Wenn die Zeichenfolge leer ist, wird der DNS-Eintrag in der öffentlichen IP-Adresse nicht verwendet.
| `service.beta.kubernetes.io/azure-shared-securityrule`            | `true` oder `false`                     | Geben Sie an, dass der Dienst mit einer Azure-Sicherheitsregel verfügbar gemacht werden sollte, die mit einem anderen Dienst gemeinsam verwendet werden kann. Dies ermöglicht einen Austausch in Bezug auf die Spezifizität von Regeln, um die Anzahl von Diensten zu erhöhen, die verfügbar gemacht werden können. Diese Anmerkung basiert auf dem Azure-Feature [Ergänzte Sicherheitsregeln](../virtual-network/network-security-groups-overview.md#augmented-security-rules) von Netzwerksicherheitsgruppen. 
| `service.beta.kubernetes.io/azure-load-balancer-resource-group`   | Name der Ressourcengruppe            | Geben Sie die Ressourcengruppe von öffentlichen IP-Adressen des Lastenausgleichs an, die sich nicht in derselben Ressourcengruppe wie die Clusterinfrastruktur (Knotenressourcengruppe) befinden.
| `service.beta.kubernetes.io/azure-allowed-service-tags`           | Liste mit zulässigen Diensttags          | Geben Sie eine Liste mit den zulässigen [Diensttags][service-tags] an (mit Kommas als Trennzeichen).
| `service.beta.kubernetes.io/azure-load-balancer-tcp-idle-timeout` | TCP-Leerlauftimeouts in Minuten          | Geben Sie die Dauer in Minuten für Leerlauftimeouts von TCP-Verbindungen ein, die für den Lastenausgleich auftreten. Der Standard- und Minimalwert ist 4. Der Maximalwert ist 30. Dieser Wert muss eine ganze Zahl sein.
|`service.beta.kubernetes.io/azure-load-balancer-disable-tcp-reset` | `true`                                | Deaktivieren von `enableTcpReset` für SLB In Kubernetes 1.18 als veraltet gekennzeichnet und in 1.20 entfernt. 


## <a name="troubleshooting-snat"></a>Problembehandlung für SNAT

Wenn Sie wissen, dass Sie viele ausgehende TCP- oder UDP-Verbindungen zu derselben IP-Zieladresse und demselben Port starten, und Fehler bei ausgehenden Verbindungen feststellen oder vom Support darauf hingewiesen werden, dass Sie zu viele SNAT-Ports (vorab zugeordnete kurzlebige Ports, die für PAT verwendet werden) in Anspruch nehmen, stehen Ihnen mehrere Lösungsmöglichkeiten zur Verfügung. Überprüfen Sie diese Optionen, und entscheiden Sie, welche für Ihr Szenario verfügbar und am besten geeignet sind. Möglicherweise kann die ein oder andere die Verwaltung dieses Szenarios erleichtern. Ausführliche Informationen finden Sie unter [Problembehandlung für Fehler bei ausgehenden Verbindungen](../load-balancer/troubleshoot-outbound-connection.md).

Die Grundursache für eine SNAT-Auslastung ist häufig ein Antimuster bei der Einrichtung und Verwaltung der ausgehenden Konnektivität oder bei der Änderung des Standardwerts von konfigurierbaren Zeitgebern. Lesen Sie diesem Abschnitt sorgfältig.

### <a name="steps"></a>Schritte
1. Überprüfen Sie, ob Ihre Verbindungen längere Zeit im Leerlauf verbleiben und für die Freigabe des Ports der Standard-Leerlauftimeout genutzt wird. Wenn dies der Fall ist, müssen Sie den Standard-Timeoutwert von 30 Minuten für Ihr Szenario unter Umständen reduzieren.
2. Ermitteln Sie, wie von Ihrer Anwendung ausgehende Konnektivität erstellt wird (z. B. Code Review oder Paketerfassung).
3. Ermitteln Sie, ob es sich bei dieser Aktivität um ein erwartetes Verhalten handelt oder die Anwendung ein Fehlverhalten aufweist. Verwenden Sie [Metriken](../load-balancer/load-balancer-standard-diagnostics.md) und [Protokolle](../load-balancer/monitor-load-balancer.md) in Azure Monitor, um Ihre Ergebnisse zu untermauern. Verwenden Sie beispielsweise die Kategorie „Fehler“ für die Metrik „SNAT-Verbindungen“.
4. Prüfen Sie, ob die richtigen [Muster](#design-patterns) eingehalten werden.
5. Prüfen Sie, ob die SNAT-Portüberlastung durch [zusätzliche ausgehende IP-Adressen und zugeordnete ausgehende Ports](#configure-the-allocated-outbound-ports) beseitigt werden muss.

### <a name="design-patterns"></a>Entwurfsmuster
Nutzen Sie nach Möglichkeit immer die Wiederverwendung von Verbindungen und Verbindungspools. Bei diesen Mustern werden Probleme aufgrund einer hohen Ressourcenauslastung vermieden, und es ergibt sich ein vorhersagbares Verhalten. Primitive für diese Muster finden Sie in vielen Entwicklungsbibliotheken und Frameworks.

- Atomische Anforderungen (eine Anforderung pro Verbindung) sind meist kein guter Entwurfsansatz. Antimuster dieser Art führen zu einer Begrenzung der Skalierung und zu einer Verringerung der Leistung und Zuverlässigkeit. Nutzen Sie stattdessen die Wiederverwendung von HTTP/S-Verbindungen, um die Anzahl von Verbindungen und zugeordneten SNAT-Ports zu reduzieren. Die Anwendungsskalierung und die Leistung werden verbessert, weil Handshakes, Mehraufwand und Kosten für kryptografische Vorgänge reduziert werden, wenn TLS verwendet wird.
- Berücksichtigen Sie bei Verwendung eines clusterexternen oder benutzerdefinierten DNS oder von benutzerdefinierten Upstream-Servern im coreDNS, dass vom DNS ggf. viele einzelne Datenflüsse mit großen Datenmengen genutzt werden, wenn der Client das Ergebnis der DNS-Auflösung nicht zwischenspeichert. Passen Sie das coreDNS zuerst an, anstatt benutzerdefinierte DNS-Server zu verwenden, und definieren Sie einen guten Wert für die Zwischenspeicherung.
- Bei UDP-Datenflüssen (z. B. DNS-Suchen) werden für die Dauer des Leerlauftimeouts SNAT-Ports zugeordnet. Je länger der Leerlauftimeout dauert, desto höher ist der Druck auf die SNAT-Ports. Verwenden Sie ein kurzes Leerlauftimeout (z. B. vier Minuten).
Nutzen Sie Verbindungspools, um Ihre Verbindungsdatenmenge zu steuern.
- Vermeiden Sie den Abbruch eines TCP-Datenflusses im Hintergrund, und verlassen Sie sich nicht darauf, dass dies über TCP-Timer bereinigt wird. Wenn Sie die explizite Verbindungstrennung durch TCP nicht zulassen, bleibt der Zuordnungszustand bei Zwischensystemen und -endpunkten erhalten, und SNAT-Ports stehen nicht für andere Verbindungen zur Verfügung. Dieses Muster kann zu einer Auslösung von Anwendungsausfällen und zu einer SNAT-Überlastung führen.
- Ändern Sie Timerwerte für die TCP-Verbindungstrennung auf der Betriebssystemebene nur, wenn Sie mit den Auswirkungen vertraut sind, die sich dadurch ergeben. Der TCP-Stapel wird zwar wiederhergestellt, aber Ihre Anwendungsleistung kann beeinträchtigt werden, wenn für die Endpunkte einer Verbindung unterschiedliche Erwartungen bestehen. Der Wunsch nach einer Änderung von Timern ist normalerweise ein Zeichen für ein zugrunde liegendes Entwurfsproblem. Sehen Sie sich die folgenden Empfehlungen an.

## <a name="moving-from-a-basic-sku-load-balancer-to-standard-sku"></a>Wechseln von einem Lastenausgleich mit der SKU „Basic“ zur SKU „Standard“

Wenn Sie bereits über einen Cluster mit einem Lastenausgleich mit einer Basic-SKU verfügen, sind bei der Migration zur Verwendung eines Clusters mit einem Lastenausgleich mit einer Standard-SKU wichtige Verhaltensunterschiede zu beachten.

Beispielsweise ist das Erstellen von Blau/Grün-Bereitstellungen zum Migrieren von Clustern eine gängige Vorgehensweise, da der `load-balancer-sku`-Typ eines Clusters nur zum Zeitpunkt der Clustererstellung definiert werden kann. Allerdings verwendet ein Lastenausgleich mit einer *Basic-SKU* IP-Adressen der *Basic-SKU*, die nicht mit einem Lastenausgleich mit einer *Standard-SKU* kompatibel sind, da hierfür IP-Adressen der *Standard-SKU* benötigt werden. Beim Migrieren von Clustern zum Upgraden von Load Balancer-SKUs ist eine neue IP-Adresse mit einer kompatiblen IP-Adressen-SKU erforderlich.

Weitere Informationen zum Migrieren von Clustern finden Sie in der [Dokumentation mit Überlegungen zur Migration](aks-migration.md), die eine Liste wichtiger Themen enthält, die bei der Migration zu berücksichtigen sind. Die folgenden Einschränkungen sind ebenfalls wichtige Verhaltensunterschiede, die bei der Verwendung eines Lastenausgleichs mit einer Standard-SKU in AKS zu beachten sind.

## <a name="limitations"></a>Einschränkungen

Wenn Sie AKS-Cluster erstellen und verwalten, die einen Lastenausgleich mit der SKU *Standard* unterstützen, gelten folgende Einschränkungen:

* Mindestens eine öffentliche IP-Adresse oder ein IP-Präfix ist erforderlich, um ausgehenden Datenverkehr aus dem AKS-Cluster zuzulassen. Darüber hinaus wird die öffentliche IP-Adresse oder das IP-Präfix benötigt, um die Konnektivität zwischen der Steuerungsebene und den Agent-Knoten sowie die Kompatibilität mit früheren Versionen von AKS zu gewährleisten. Sie haben die folgenden Optionen zum Angeben von öffentlichen IP-Adressen oder IP- Präfixen mit einem *Standard*-SKU-Lastenausgleichsmodul:
    * Geben Sie Ihre eigenen öffentlichen IP-Adressen an.
    * Geben Sie Ihre eigenen öffentlichen IP-Präfixe an.
    * Geben Sie eine Zahl bis 100 an, damit der AKS-Cluster so viele öffentliche *Standard*-SKU-IP-Adressen in derselben Ressourcengruppe erstellen kann, in der sich der AKS-Cluster befindet, in der Regel mit *MC_* am Anfang benannt. AKS weist die öffentliche IP-Adresse dem Lastenausgleich mit der SKU *Standard* zu. Standardmäßig wird automatisch eine öffentliche IP-Adresse in derselben Ressourcengruppe wie der AKS-Cluster erstellt, wenn weder eine öffentliche IP-Adresse noch ein öffentliches IP-Präfix oder eine Anzahl von IP-Adressen angegeben wird. Sie müssen auch öffentliche Adressen zulassen und dürfen keine Azure-Richtlinie erstellen, die die Erstellung von IP-Adressen unterbindet.
* Eine von AKS erstellte öffentliche IP-Adresse kann nicht als eigene benutzerdefinierte öffentliche IP-Adresse wiederverwendet werden. Alle benutzerdefinierten IP-Adressen müssen vom Benutzer erstellt und verwaltet werden.
* Das Definieren der Lastenausgleichs-SKU kann nur durchgeführt werden, wenn Sie einen AKS-Cluster erstellen. Nach der Erstellung eines AKS-Clusters kann die Lastenausgleichs-SKU nicht mehr geändert werden.
* Pro Cluster kann immer nur eine Art von Lastenausgleichs-SKU („Basic“ oder „Standard“) verwendet werden.
* Ein Lastenausgleich mit einer *Standard*-SKU unterstützt nur IP-Adressen der *Standard*-SKU.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Kubernetes-Diensten finden Sie in der entsprechenden [Dokumentation][kubernetes-services].

Informieren Sie sich in der [Dokumentation zum internen AKS-Lastenausgleich](internal-lb.md) weiter über die Verwendung eines internen Lastenausgleichs für eingehenden Datenverkehr.

<!-- LINKS - External -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[az-aks-show]: /cli/azure/aks#az_aks_show
[az-aks-create]: /cli/azure/aks#az_aks_create
[az-aks-get-credentials]: /cli/azure/aks#az_aks_get_credentials
[az-aks-install-cli]: /cli/azure/aks#az_aks_install_cli
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-group-create]: /cli/azure/group#az_group_create
[az-provider-register]: /cli/azure/provider#az_provider_register
[az-network-lb-outbound-rule-list]: /cli/azure/network/lb/outbound-rule#az_network_lb_outbound_rule_list
[az-network-public-ip-show]: /cli/azure/network/public-ip#az_network_public_ip_show
[az-network-public-ip-prefix-show]: /cli/azure/network/public-ip/prefix#az_network_public_ip_prefix_show
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/skus.md
[azure-lb-outbound-rules]: ../load-balancer/load-balancer-outbound-connections.md#outboundrules
[azure-lb-outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md
[azure-lb-outbound-preallocatedports]: ../load-balancer/load-balancer-outbound-connections.md#preallocatedports
[azure-lb-outbound-rules-overview]: ../load-balancer/load-balancer-outbound-connections.md#outboundrules
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az_extension_update
[use-multiple-node-pools]: use-multiple-node-pools.md
[troubleshoot-snat]: #troubleshooting-snat
[service-tags]: ../virtual-network/network-security-groups-overview.md#service-tags
[maxsurge]: upgrade-cluster.md#customize-node-surge-upgrade