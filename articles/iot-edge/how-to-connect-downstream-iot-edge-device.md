---
title: Verbinden nachgeschalteter IoT Edge-Geräte – Azure IoT Edge | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie IoT Edge-Geräte für eine Verbindung mit Azure IoT Edge-Gatewaygeräten konfigurieren.
author: kgremban
ms.author: kgremban
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
monikerRange: '>=iotedge-2020-11'
ms.openlocfilehash: c0bbc7bb40c292675374a3198fe0514b720adfca
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2021
ms.locfileid: "130226108"
---
# <a name="connect-a-downstream-iot-edge-device-to-an-azure-iot-edge-gateway"></a>Verbinden eines nachgeschalteten IoT Edge-Geräts mit einem Azure IoT Edge-Gateway

[!INCLUDE [iot-edge-version-202011](../../includes/iot-edge-version-202011.md)]

Dieser Artikel enthält Anweisungen zum Herstellen einer vertrauenswürdigen Verbindung zwischen einem IoT Edge-Gateway und einem nachgeschalteten IoT Edge-Gerät.

In einem Gatewayszenario kann ein IoT Edge-Gerät sowohl ein Gateway als auch ein nachgeschaltetes Gerät sein. Mehrere IoT Edge-Geräte können auf mehreren Ebenen angeordnet werden, um eine Gerätehierarchie zu erstellen. Die nachgeschalteten (oder untergeordneten) Geräte können sich authentifizieren und Nachrichten über das jeweils dazugehörige Gatewaygerät (oder übergeordnete Gerät) senden oder empfangen.

Es gibt zwei verschiedene Konfigurationen für IoT Edge-Geräte in einer Gatewayhierarchie, und in diesem Artikel werden beide thematisiert. Beim ersten handelt es sich um das IoT Edge-Gerät der **obersten Ebene**. Wenn mehrere IoT Edge-Geräte jeweils Verbindungen zueinander herstellen, gilt jedes Gerät, das über kein übergeordnetes Gerät verfügt, sich aber direkt mit IoT Hub verbindet, als ein Gerät der obersten Ebene. Dieses Gerät ist verantwortlich dafür, Anforderungen von allen untergeordneten Geräten zu verarbeiten. Die andere Konfiguration gilt für alle IoT Edge-Geräte auf einer **niedrigeren Ebene** der Hierarchie. Diese Geräte sind möglicherweise ein Gateway für andere nachgeschaltete IoT- und IoT Edge-Geräte. Sie müssen sämtliche Kommunikation jedoch auch über ihre jeweiligen übergeordneten Geräte weiterleiten.

Für manche Netzwerkarchitekturen ist es erforderlich, dass nur das oberste IoT Edge-Gerät einer Hierarchie eine Verbindung zur Cloud herstellen kann. In dieser Konfiguration können alle IoT Edge-Geräte auf niedrigeren Ebenen der Hierarchie nur mit ihrem Gatewaygerät (oder übergeordnetem Gerät) und sämtlichen nachgeschalteten (oder untergeordneten) Geräten kommunizieren.

Alle Schritte in diesem Artikel basieren auf den Schritten im Artikel [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md). Dabei wird ein IoT Edge-Gerät als Gateway für nachgeschaltete IoT-Geräte eingerichtet. Die selben grundlegenden Schritte gelten für alle Gatewayszenarios:

* **Authentifizierung:** Erstellen Sie IoT Hub-Identitäten für alle Geräte in der Gatewayhierarchie.
* **Autorisierung:** Richten Sie die Beziehung zwischen übergeordneten/untergeordneten Geräten in IoT Hub ein, um untergeordnete Geräte für die Herstellung einer Verbindung zum jeweiligen übergeordneten Gerät zu autorisieren, wie wenn eine Verbindung zu IoT Hub hergestellt würde.
* **Gatewayermittlung:** Sorgen Sie dafür, dass das untergeordnete Gerät sein übergeordnetes Gerät im lokalen Netzwerk finden kann.
* **Sichere Verbindung:** Stellen Sie eine sichere Verbindung mit vertrauenswürdigen Zertifikaten her, die Teil derselben Kette sind.

## <a name="prerequisites"></a>Voraussetzungen

* Ein kostenloser oder Standard-IoT-Hub
* Mindestens zwei **IoT Edge-Geräte** (eines muss das Gerät der obersten Ebene sein, das zweite bzw. sämtliche anderen Geräte unterer Ebenen) Wenn keine IoT Edge-Geräte verfügbar sind, erhalten Sie unter [Ausführen von virtuellen Computern vom Typ „Azure IoT Edge unter Ubuntu“](how-to-install-iot-edge-ubuntuvm.md) weitere Informationen.
* Wenn Sie die Azure CLI zum Erstellen und Verwalten von Geräten verwenden, muss die Azure CLI (Version 2.3.1) mit der Azure IoT-Erweiterung (Version 0.10.6 oder höher) installiert sein.

In diesem Artikel finden Sie detaillierte Schritte und Optionen, die Sie beim Erstellen einer geeigneten Gatewayhierarchie für Ihr Szenario unterstützen. Ein Tutorial finden Sie unter [Tutorial: Erstellen einer Hierarchie für IoT Edge-Geräte mit Gateways](tutorial-nested-iot-edge.md).

## <a name="create-a-gateway-hierarchy"></a>Erstellen einer Gatewayhierarchie

Sie erstellen eine IoT Edge-Gatewayhierarchie, indem Sie Beziehungen zwischen übergeordneten/untergeordneten Geräten für die IoT Edge-Geräte im Szenario definieren. Sie können ein übergeordnetes Gerät beim Erstellen einer neuen Geräteidentität festlegen, oder Sie verwalten das übergeordnete Gerät und untergeordnete Geräte einer vorhandenen Geräteidentität.

Beim Einrichten der Beziehungen zwischen übergeordneten/untergeordneten Geräten werden untergeordnete Geräte dazu autorisiert, eine Verbindung zu ihrem übergeordneten Gerät herzustellen, wie wenn sie eine Verbindung zu IoT Hub herstellen würden.

Nur IoT Edge-Geräte können übergeordnete Geräte sein. Sowohl IoT Edge-Geräte als auch IoT-Geräte können jedoch untergeordnete Geräte sein. Ein übergeordnetes Gerät kann über viele untergeordnete Geräte verfügen. Ein untergeordnetes Gerät kann jedoch nur ein übergeordnetes Gerät haben. Eine Gatewayhierarchie wird erstellt, indem jeweils übergeordnete und untergeordnete Geräte verkettet werden, sodass das untergeordnete Geräte eines Geräts das übergeordnete Gerät eines anderen ist.

<!-- TODO: graphic of gateway hierarchy -->

# <a name="portal"></a>[Portal](#tab/azure-portal)

Im Azure-Portal können Sie die Beziehung zwischen übergeordnetem und untergeordnetem Gerät beim Erstellen neuer Geräteidentitäten verwalten, oder indem Sie vorhandene Geräte bearbeiten.

Wenn Sie ein neues IoT Edge-Gerät erstellen, steht Ihnen die Option zur Verfügung, übergeordnete und untergeordnete Geräte aus einer Liste vorhandener IoT Edge-Geräte im jeweiligen Hub auszuwählen.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.
1. Klicken Sie im Navigationsmenü auf **IoT Edge**.
1. Wählen Sie **IoT Edge-Gerät hinzufügen** aus.
1. Zusammen mit dem Festlegen der Geräte-ID und der Authentifizierungseinstellungen können Sie **ein übergeordnetes Gerät festlegen** oder **untergeordnete Geräte auswählen**.
1. Wählen Sie das Gerät oder die Geräte aus, die übergeordnetes Gerät bzw. untergeordnete Geräte sein sollen.

Sie können auch Beziehungen zwischen übergeordneten und untergeordneten Geräten für vorhandene Geräte erstellen oder sie verwalten.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.
1. Klicken Sie im Navigationsmenü auf **IoT Edge**.
1. Wählen Sie das Gerät, das Sie verwalten möchten, aus der Liste der **IoT Edge-Geräte** aus.
1. Klicken Sie auf **Übergeordnetes Gerät festlegen** oder auf **Untergeordnete Geräte verwalten**.
1. Fügen Sie übergeordnete oder untergeordnete Geräte hinzu, oder entfernen Sie sie.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Die [azure-iot](/cli/azure/iot)-Erweiterung für die Azure CLI bietet Befehle, mit denen Sie Ihre IoT-Ressourcen verwalten können. Sie können die Beziehung zwischen übergeordnetem und untergeordnetem Gerät von IoT- und IoT Edge-Geräten beim Erstellen neuer Geräteidentitäten verwalten, oder Sie bearbeiten vorhandene Geräte.

Die [az iot hub device-identity](/cli/azure/iot/hub/device-identity)-Befehle ermöglichen es Ihnen, die Beziehungen zwischen übergeordneten und untergeordneten Geräten für ein bestimmtes Gerät zu verwalten.

Der `create`-Befehl beinhaltet Parameter für das Hinzufügen von untergeordneten Geräten und das Festlegen eines übergeordneten Geräts bei der Geräteerstellung.

Weitere Befehle für Geräteidentitäten, z. B. `add-children`, `list-children` und `remove-children` oder `get-parent` und `set-parent`, ermöglichen es Ihnen, die Beziehungen zwischen übergeordneten und untergeordneten Geräten für vorhandene Geräte zu verwalten.

---

>[!NOTE]
>Wenn Sie Beziehungen zwischen über- und untergeordneten Elementen programmgesteuert einrichten möchten, können Sie das [IoT Hub Service SDK](../iot-hub/iot-hub-devguide-sdks.md) für C#, Java oder Node.js verwenden.
>
>Dies ist ein [Beispiel für die Zuordnung von untergeordneten Geräten](https://github.com/Azure/azure-iot-sdk-csharp/blob/main/e2e/test/iothub/service/RegistryManagerE2ETests.cs) mit dem C#-SDK. Die Aufgabe `RegistryManager_AddAndRemoveDeviceWithScope()` zeigt, wie Sie programmgesteuert eine Hierarchie mit drei Ebenen erstellen. Ein IoT Edge-Gerät befindet sich als übergeordnetes Element auf der ersten Ebene. Ein weiteres IoT Edge-Gerät befindet sich auf der zweiten Ebene und dient sowohl als untergeordnetes als auch als übergeordnetes Element. Schließlich befindet sich ein IoT-Gerät auf der dritten Ebene als untergeordnetes Gerät der untersten Ebene.

## <a name="prepare-certificates"></a>Vorbereiten von Zertifikaten

Auf den Geräten einer Gatewayhierarchie muss eine konsistente Zertifikatkette installiert sein, damit eine sichere Kommunikation zwischen diesen Geräten möglich ist. Jedes Gerät in der Hierarchie, ob IoT Edge-Gerät oder IoT-Blattgerät, benötigt eine Kopie desselben Zertifikats der Stammzertifizierungsstelle. Die einzelnen IoT Edge-Geräte in der Hierarchie verwenden das Zertifikat der Stammzertifizierungsstelle dann als Stamm für ihr jeweiliges Stammzertifizierungsstellenzertifikat.

Mit dieser Einrichtung kann jedes nachgeschaltete IoT Edge-Gerät oder IoT-Blattgerät die Identität des jeweiligen übergeordneten Geräts überprüfen, indem überprüft wird, ob der Edgehub, zu dem die Verbindung hergestellt wird, über ein Serverzertifikat verfügt, das vom gemeinsamen Stammzertifizierungsstellenzertifikat signiert ist.

<!-- TODO: certificate graphic -->

Erstellen Sie die folgenden Zertifikate:

* Ein **Zertifikat der Stammzertifizierungsstelle:** Dabei handelt es sich um das oberste gemeinsam genutzte Zertifikat für alle Geräte in einer bestimmten Gatewayhierarchie. Dieses Zertifikat ist auf allen Geräten installiert.
* Alle **Zwischenzertifikate**, die in die Stammzertifikatkette eingeschlossen werden sollen.
* Ein **Gerätezertifizierungsstellenzertifikat** und der dazugehörige **private Schlüssel**, die vom Stamm- und den Zwischenzertifikaten erstellt werden. Sie benötigen ein eindeutiges Gerätezertifizierungsstellenzertifikat für jedes einzelne IoT Edge-Gerät in der Gatewayhierarchie.

Sie können entweder eine selbstsignierte Zertifizierungsstelle verwenden oder ein Zertifikat von einer vertrauenswürdigen kommerziellen Zertifizierungsstelle wie Baltimore, Verisign, DigiCert oder GlobalSign erwerben.

Sollten Sie über keine eigenen Zertifikate verfügen, können Sie [Demozertifikate zum Testen von IoT Edge-Gerätefeatures erstellen](how-to-create-test-certificates.md). Befolgen Sie die Schritte in diesem Artikel, um Stamm- und Zwischenzertifikate zu erstellen. Erstellen Sie dann Zertifizierungsstellenzertifikate für IoT Edge-Geräte für Ihre einzelnen Geräte.

## <a name="configure-iot-edge-on-devices"></a>Konfigurieren von IoT Edge auf Geräten

Die Schritte für das Einrichten von IoT Edge als Gateway sind den Schritten für das Einrichten von IoT Edge als nachgeschaltetes Gerät sehr ähnlich.

Damit die Gatewayermittlung möglich ist, muss jedes IoT Edge-Gatewaygerät mit einem **Hostname** konfiguriert werden, den die untergeordneten Geräte verwenden können, um es im lokalen Netzwerk zu finden. Jedes nachgeschaltete IoT Edge-Gerät muss mit einem Wert für **parent_hostname** konfiguriert werden, zu dem die Verbindung hergestellt werden kann. Wenn ein einzelnes IoT Edge-Gerät sowohl übergeordnetes als auch untergeordnetes Gerät ist, benötigt es beide Parameter.

Für sichere Verbindungen muss jedes IoT Edge-Gerät in einem Gatewayszenario mit einem eindeutigen Gerätezertifizierungsstellenzertifikat und einer Kopie des Stammzertifizierungsstellenzertifikats konfiguriert werden, das von allen Geräten in der Gatewayhierarchie genutzt wird.

Auf Ihrem Gerät sollte IoT Edge bereits installiert sein. Falls nicht, führen Sie die Schritte unter [Manuelles Bereitstellen eines einzelnen Linux-IoT Edge aus](how-to-provision-single-device-linux-symmetric.md).

Die Schritte in diesem Abschnitt verweisen auf das **Zertifikat der Stammzertifizierungsstelle** und auf das **Gerätezertifizierungsstellenzertifikat und den privaten Schlüssel**, die in diesem Artikel an früherer Stelle bereits erläutert wurden. Wenn Sie diese Zertifikate auf einem anderen Gerät erstellt haben, sorgen Sie dafür, dass sie auf diesem Gerät verfügbar sind. Sie können die Dateien physisch übertragen, z. B. mit einem USB-Datenträger, mit einem Dienst wie [Azure Key Vault](../key-vault/general/overview.md) oder mit einer Funktion zum [sicheren Kopieren von Dateien](https://www.ssh.com/ssh/scp/).

Verwenden Sie die folgenden Schritte zum Konfigurieren von IoT Edge auf Ihrem Gerät.

Stellen Sie sicher, dass der Benutzer **iotedge** Leseberechtigungen für das Verzeichnis mit den Zertifikaten und Schlüsseln hat.

1. Installieren Sie das **Stammzertifizierungsstellenzertifikat** auf diesem IoT Edge-Gerät.

   ```bash
   sudo cp <path>/<root ca certificate>.pem /usr/local/share/ca-certificates/<root ca certificate>.pem.crt
   ```

1. Aktualisieren Sie den Zertifikatspeicher.

   ```bash
   sudo update-ca-certificates
   ```

   Dieser Befehl sollte als Ausgabe anzeigen, dass ein Zertifikat unter /etc/ssl/certs hinzugefügt wurde.

1. Öffnen Sie die IoT-Edge-Konfigurationsdatei.

   ```bash
   sudo nano /etc/aziot/config.toml
   ```

   >[!TIP]
   >Wenn die Konfigurationsdatei auf Ihrem Gerät noch nicht vorhanden ist, erstellen Sie sie mit dem folgenden Befehl basierend auf der Vorlagendatei:
   >
   >```bash
   >sudo cp /etc/aziot/config.toml.edge.template /etc/aziot/config.toml
   >```

1. Suchen Sie in der Konfigurationsdatei den Abschnitt **Hostname**. Heben Sie die Auskommentierung der Zeile mit dem Parameter `hostname` auf, und aktualisieren Sie den Wert auf den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) oder die IP-Adresse des IoT Edge-Geräts.

   Den Wert dieses Parameters verwenden nachgeschaltete Geräte zum Herstellen einer Verbindung zu diesem Gateway. Der Hostname verwendet den Computernamen standardmäßig, der FQDN oder die IP-Adresse ist jedoch erforderlich, um eine Verbindung zu nachgeschalteten Geräten herstellen zu können.

   Verwenden Sie einen Hostnamen mit weniger als 64 Zeichen. Dies ist das Zeichenlimit für einen allgemeinen Namen für ein Serverzertifikat.

   Nutzen Sie das Muster des Hostnamens für die gesamte Gatewayhierarchie konsistent. Verwenden Sie entweder FQDNs oder IP-Adressen, nicht beides.

1. *Wenn dieses Gerät ein untergeordnetes Gerät ist*, suchen Sie den Abschnitt **Parent hostname** (Übergeordneter Hostname). Heben Sie die Auskommentierung des Parameters `parent_hostname` auf, und aktualisieren Sie ihn auf den FQDN oder die IP-Adresse des übergeordneten Geräts. Der Wert muss mit dem Wert übereinstimmen, der in der Konfigurationsdatei des übergeordneten Geräts als Hostname angegeben wurde.

1. Suchen Sie den Abschnitt **Trust bundle cert** (Vertrauensstellungszertifikatbündel). Heben Sie die Auskommentierung des Parameters `trust_bundle_cert` auf, und aktualisieren Sie ihn mit dem Datei-URI auf das Zertifikat der Stammzertifizierungsstelle auf Ihrem Gerät.

1. Überprüfen Sie, ob Ihr IoT Edge-Gerät beim Start die richtige Version des IoT Edge-Agents verwendet.

   Suchen Sie den Abschnitt **Standard-Edge-Agent**, und vergewissern Sie sich, dass der Imagewert IoT Edge 1.2 lautet. Falls nicht, aktualisieren Sie es:

   ```toml
   [agent.config]
   image: "mcr.microsoft.com/azureiotedge-agent:1.2"
   ```

1. Suchen Sie in der Konfigurationsdatei den Abschnitt **Edge CA certificate** (Edge-Zertifikat der Zertifizierungsstelle). Heben Sie die Auskommentierung der Zeilen in diesem Abschnitt auf, und geben Sie die Datei-URI-Pfade für die Zertifikat- und Schlüsseldateien auf dem IoT Edge Gerät an.

   ```toml
   [edge_ca]
   cert = "file:///<path>/<device CA cert>"
   pk = "file:///<path>/<device CA key>"
   ```

1. Speichern (`Ctrl+O`) und schließen (`Ctrl+X`) Sie die Konfigurationsdatei.

1. Wenn Sie zuvor bereits andere Zertifikate für IoT Edge verwendet haben, löschen Sie die Dateien in den folgenden beiden Verzeichnissen, um dafür zu sorgen, dass Ihre neuen Zertifikate verwendet werden:

   * `/var/lib/aziot/certd/certs`
   * `/var/lib/aziot/keyd/keys`

1. Übernehmen Sie Ihre Änderungen.

   ```bash
   sudo iotedge config apply
   ```

1. Überprüfen Sie, ob Fehler in der Konfiguration vorhanden sind.

   ```bash
   sudo iotedge check --verbose
   ```

   >[!TIP]
   >Das Prüftool für IoT Edge verwendet einen Container, um einige der Diagnoseüberprüfungen durchzuführen. Wenn Sie dieses Tool auf nachgeschalteten IoT Edge-Geräten verwenden möchten, sorgen Sie dafür, dass diese auf `mcr.microsoft.com/azureiotedge-diagnostics:latest` zugreifen können. Sorgen Sie alternativ dafür, dass sich das Containerimage in Ihrer privaten Containerregistrierung befindet.

## <a name="network-isolate-downstream-devices"></a>Isolieren von nachgeschalteten Geräten im Netzwerk

Mit den bisherigen Schritten dieses Artikels wurden IoT Edge-Geräte entweder als Gateway oder als nachgeschaltetes Gerät eingerichtet. Außerdem wurde eine vertrauenswürdige Verbindung zwischen ihnen erstellt. Das Gatewaygerät verarbeitet Interaktionen zwischen dem nachgeschalteten Gerät und IoT Hub, einschließlich der Authentifizierung und Nachrichtenrouting. Auf nachgeschalteten IoT Edge-Geräten bereitgestellte Module können weiterhin ihre eigenen Verbindungen zu Clouddiensten herstellen.

Bei einigen Netzwerkarchitekturen, z. B. solchen, die den ISA-95-Standard befolgen, wird versucht, die Anzahl an Internetverbindungen zu minimieren. In diesen Szenarios können Sie nachgeschaltete IoT Edge-Geräte ohne direkte Internetkonnektivität konfigurieren. Zusätzlich zum Routing der IoT Hub-Kommunikation über das Gatewaygerät können IoT Edge-Geräte alle Cloudverbindungen über das Gatewaygerät herstellen.

Für diese Netzwerkkonfiguration ist erforderlich, dass nur das IoT Edge-Gerät der obersten Ebene einer Gatewayhierarchie direkte Verbindungen zur Cloud aufweist. IoT Edge-Geräte in den unteren Ebenen können jeweils nur mit ihrem übergeordneten Gerät oder untergeordneten Geräten kommunizieren. Sondermodule auf den Gatewaygeräten ermöglichen dieses Szenario. Dazu gehören die folgenden:

* Das **API-Proxymodul** ist für jedes IoT Edge-Gateway erforderlich, das ein weiteres untergeordnetes IoT Edge-Gerät aufweist. Das bedeutet, das Modul muss sich auf *jeder Ebene* einer Gatewayhierarchie mit Ausnahme der untersten Ebene befinden. Dieses Modul verwendet einen [nginx](https://nginx.org)-Reverseproxy, um HTTP-Daten über Netzwerkebenen über einen einzelnen Port weiterzuleiten. Über den dazugehörigen Modulzwilling und die Umgebungsvariablen ist eine umfangreiche Konfiguration möglich. Das Modul kann also exakt an Ihre Gatewayszenarioanforderungen angepasst werden.

* Das **Docker-Registrierungsmodul** kann auf dem IoT Edge-Gatewaygerät auf der *obersten Ebene* einer Gatewayhierarchie bereitgestellt werden. Dieses Modul ist verantwortlich für das Abrufen und Zwischenspeichern von Containerimages im Auftrag aller IoT Edge-Geräte unterer Ebenen. Die Alternative zum Bereitstellen dieses Moduls auf der obersten Ebene ist die Verwendung einer lokalen Registrierung oder das manuelle Laden von Containerimages auf Geräte bei Festlegung der Pullrichtlinie des Moduls auf **Nie**.

* Das Modul **Azure Blob Storage für IoT Edge** kann auf dem IoT Edge-Gatewaygerät auf der *obersten Ebene* einer Gatewayhierarchie bereitgestellt werden. Dieses Modul ist verantwortlich für das Hochladen von Blobs im Auftrag aller IoT Edge-Geräte unterer Ebenen. Die Möglichkeit, Blobs hochzuladen, ermöglicht auch hilfreiche Troubleshootingfunktionen für IoT Edge-Geräte auf unteren Ebenen, z. B. Hochladen von Modulprotokollen und Unterstützung für das Hochladen von Paketen.

### <a name="network-configuration"></a>Netzwerkkonfiguration

Für jedes Gatewaygerät der oberen Ebene müssen Netzwerkbetreiber Folgendes tun:

* Angeben einer statischen IP-Adresse oder eines vollqualifizierten Domänennamens (FQDN)
* Autorisieren der ausgehenden Kommunikation von dieser IP-Adresse zu Ihrem Azure IoT Hub-Hostname über die Ports 443 (HTTPS) und 5671 (AMQP)
* Autorisieren ausgehender Kommunikation von dieser IP-Adresse zu Ihrem Azure Container Registry-Hostname über Port 443 (HTTPS)

  Das API-Proxymodul kann jeweils nur Verbindungen zu einer Containerregistrierung zur selben Zeit verarbeiten. Es wird empfohlen, alle Containerimages, einschließlich der öffentlichen von der Containerregistrierung von Microsoft (mcr.microsoft.com) bereitgestellten Images, in Ihrer privaten Containerregistrierung zu speichern.

Für jedes Gatewaygerät einer unteren Ebene müssen Netzwerkbetreiber Folgendes tun:

* Bereitstellen einer statischen IP-Adresse
* Autorisieren ausgehender Kommunikation von dieser IP-Adresse zur IP-Adresse des übergeordneten Gateway über die Ports 443 (HTTPS) und 5671 (AMQP)

### <a name="deploy-modules-to-top-layer-devices"></a>Bereitstellen von Modulen auf Geräten oberer Ebenen

Das IoT Edge-Gerät auf der obersten Ebene einer Gatewayhierarchie verfügt über mehrere erforderliche Module, die zusätzlich zu Workloadmodulen, die Sie möglicherweise auf dem Gerät ausführen, für es bereitgestellt werden müssen.

Das API-Proxymodul wurde so entworfen, dass es angepasst werden kann, um die häufigsten Gatewayszenarios zu verarbeiten. In diesem Artikel finden Sie ein Beispiel für das Einrichten der Module in einer einfachen Konfiguration. Unter [Konfigurieren des API-Proxymoduls für Ihr Gatewayhierarchieszenario](how-to-configure-api-proxy-module.md) finden Sie detailliertere Informationen und Beispiele.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.
1. Klicken Sie im Navigationsmenü auf **IoT Edge**.
1. Wählen Sie das Gerät der obersten Ebene aus der Liste mit **IoT Edge-Geräten** aus, das Sie konfigurieren.
1. Wählen Sie **Module festlegen** aus.
1. Klicken Sie im Abschnitt **IoT Edge-Modul** auf **Hinzufügen** und dann auf **Marketplace-Modul**.
1. Suchen Sie nach dem Modul **IoT Edge-API-Proxy**, und klicken Sie darauf.
1. Wählen Sie den Namen des API-Proxymoduls aus der Liste bereitgestellter Module aus, und aktualisieren Sie die folgenden Moduleinstellungen:
   1. Aktualisieren Sie auf der Registerkarte **Umgebungsvariablen** den Wert von **NGINX_DEFAULT_PORT** in `443`.
   1. Aktualisieren Sie auf der Registerkarte mit **Optionen zum Erstellen von Containern** die Portbindungen in den Referenzport 443.

      ```json
      {
        "HostConfig": {
          "PortBindings": {
            "443/tcp": [
              {
                "HostPort": "443"
              }
            ]
          }
        }
      }
      ```

   Diese Änderungen konfigurieren das API-Proxymodul so, dass es an Port 443 lauscht. Zur Vermeidung von Portbindungskollisionen müssen Sie das edgeHub-Modul so konfigurieren, dass es nicht an Port 443 lauscht. Stattdessen leitet das API-Proxymodul sämtlichen edgeHub-Datenverkehr für Port 443 weiter.

1. Klicken Sie auf **Runtimeeinstellungen**, und suchen Sie nach den Optionen zum Erstellen des edgeHub-Moduls. Löschen Sie die Portbindung für Port 443, übernehmen Sie jedoch die Bindungen für die Ports 5671 und 8883.

   ```json
   {
     "HostConfig": {
       "PortBindings": {
         "5671/tcp": [
           {
             "HostPort": "5671"
           }
         ],
         "8883/tcp": [
           {
             "HostPort": "8883"
           }
         ]
       }
     }
   }
   ```

1. Klicken Sie auf **Speichern** zum Speichern der Änderungen an den Runtimeeinstellungen.
1. Klicken Sie noch mal auf **Hinzufügen**, und klicken Sie dann auf das **IoT Edge-Modul**.
1. Geben Sie die folgenden Werte an, um das Docker-Registrierungsmodul Ihrer Bereitstellung hinzuzufügen:
   1. **Name des IoT Edge-Moduls:** `registry`
   1. **Image-URI** (Auf der Registerkarte **Moduleinstellungen**): `registry:latest`.
   1. Fügen Sie auf der Registerkarte **Umgebungsvariablen** die folgenden Umgebungsvariablen hinzu:

      * **Name:** `REGISTRY_PROXY_REMOTEURL` **Value**: Die URL für die Containerregistrierung, der dieses Registrierungsmodul zugeordnet werden soll. Beispiel: `https://myregistry.azurecr`.

        Das Registrierungsmodul kann nur einer Containerregistrierung zugeordnet werden, es wird also empfohlen, dass alle Containerimages in einer einzelnen privaten Containerregistrierung gespeichert werden.

      * **Name:** `REGISTRY_PROXY_USERNAME` **Value**: Benutzername für die Authentifizierung bei der Containerregistrierung

      * **Name:** `REGISTRY_PROXY_PASSWORD` **Value**: Kennwort für die Authentifizierung bei der Containerregistrierung

   1. Öffnen Sie die Registerkarte mit **Optionen für die Containererstellung**. Fügen Sie dort Folgendes ein:

      ```json
      {
          "HostConfig": {
              "PortBindings": {
                  "5000/tcp": [
                      {
                          "HostPort": "5000"
                      }
                  ]
              }
          }
      }
      ```

1. Klicken Sie auf **Hinzufügen**, um das Modul der Bereitstellung hinzuzufügen.
1. Klicken Sie auf **Weiter: Routen**, um mit dem nächsten Schritt fortzufahren.
1. Wenn Gerät-zu-Cloud-Nachrichten von nachgeschalteten Geräten IoT Hub erreichen sollen, schließen Sie eine Route ein, die alle Nachrichten an IoT Hub übergibt. Zum Beispiel:
    1. **Name**: `Route`
    1. **Wert**: `FROM /messages/* INTO $upstream`
1. Klicken Sie auf **Überprüfen + erstellen**, um zum letzten Schritt zu gelangen.
1. Klicken Sie auf **Erstellen**, um die Bereitstellung für Ihr Gerät durchzuführen.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

1. Erstellen Sie in [Azure Cloud Shell](https://shell.azure.com/) eine JSON-Datei zur Bereitstellung. Beispiel:

   ```json
   {
       "modulesContent": {
           "$edgeAgent": {
               "properties.desired": {
                   "modules": {
                       "dockerContainerRegistry": {
                           "settings": {
                               "image": "registry:latest",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5000/tcp\":[{\"HostPort\":\"5000\"}]}}}"
                           },
                           "type": "docker",
                           "version": "1.0",
                           "env": {
                               "REGISTRY_PROXY_REMOTEURL": {
                                   "value": "The URL for the container registry you want this registry module to map to. For example, https://myregistry.azurecr"
                               },
                               "REGISTRY_PROXY_USERNAME": {
                                   "value": "Username to authenticate to the container registry."
                               },
                               "REGISTRY_PROXY_PASSWORD": {
                                   "value": "Password to authenticate to the container registry."
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       },
                       "IoTEdgeAPIProxy": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-api-proxy:1.0",
                               "createOptions": "{\"HostConfig\": {\"PortBindings\": {\"443/tcp\": [{\"HostPort\":\"443\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "NGINX_DEFAULT_PORT": {
                                   "value": "443"
                               },
                               "DOCKER_REQUEST_ROUTE_ADDRESS": {
                                   "value": "registry:5000"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always",
                           "version": "1.0"
                       }
                   },
                   "runtime": {
                       "settings": {
                           "minDockerVersion": "v1.25"
                       },
                       "type": "docker"
                   },
                   "schemaVersion": "1.1",
                   "systemModules": {
                       "edgeAgent": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-agent:1.2",
                               "createOptions": "{}"
                           },
                           "type": "docker"
                       },
                       "edgeHub": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-hub:1.2",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}]}}}"
                           },
                           "type": "docker",
                           "env": {},
                           "status": "running",
                           "restartPolicy": "always"
                       }
                   }
               }
           },
           "$edgeHub": {
               "properties.desired": {
                   "routes": {
                       "route": "FROM /messages/* INTO $upstream"
                   },
                   "schemaVersion": "1.1",
                   "storeAndForwardConfiguration": {
                       "timeToLiveSecs": 7200
                   }
               }
           }
       }
   }
   ```

   Diese Bereitstellungsdatei konfiguriert das API-Proxymodul so, dass es an Port 443 lauscht. Konfigurieren Sie das edgeHub-Modul so, dass es nicht an Port 443 lauscht, um Portbindungskollisionen zu vermeiden. Stattdessen leitet das API-Proxymodul sämtlichen edgeHub-Datenverkehr für Port 443 weiter.

1. Geben Sie den folgenden Befehl ein, um eine Bereitstellung auf ein IoT Edge-Gerät zu erstellen:

   ```azurecli
   az iot edge set-modules --device-id <device_id> --hub-name <iot_hub_name> --content ./<deployment_file_name>.json
   ```

---

### <a name="deploy-modules-to-lower-layer-devices"></a>Bereitstellen von Modulen auf Geräten unterer Ebenen

IoT Edge-Geräte auf niedrigeren Ebenen einer Gatewayhierarchie verfügen über ein erforderliches Modul, das zusätzlich zu Workloadmodulen, die Sie möglicherweise auf dem Gerät ausführen, für sie bereitgestellt werden muss.

#### <a name="route-container-image-pulls"></a>Weiterleiten von Pullvorgängen für Containerimages

Bevor Sie Informationen zum erforderlichen Proxymodul für IoT Edge-Geräte in Gatewayhierarchien erhalten, ist es wichtig, zu verstehen, wie IoT Edge-Geräte auf unteren Ebenen ihre Modulimages abrufen.

Wenn Ihre Geräte auf unteren Ebenen keine Verbindung zur Cloud herstellen können, Sie jedoch möchten, dass sie wie gewohnt Modulimages pullen können, muss das Gerät der obersten Ebene der Gatewayhierarchie so konfiguriert werden, dass diese Anforderungen verarbeitet werden. Das Gerät der obersten Ebene muss ein **Docker-Registrierungsmodul** ausführen, das Ihrer Containerregistrierung zugeordnet ist. Konfigurieren Sie das API-Proxymodul dann so, dass Containeranforderungen dorthin weitergeleitet werden. Diese Details werden in den oberen Abschnitten dieses Artikels erläutert. In dieser Konfiguration sollten die Geräte der unteren Ebenen nicht auf Cloudcontainerregistrierungen verweisen, sondern auf Registrierungen, die auf der oberen Ebene ausgeführt werden.

Anstatt `mcr.microsoft.com/azureiotedge-api-proxy:1.0` aufzurufen, sollten Geräte niedrigerer Ebenen beispielsweise `$upstream:443/azureiotedge-api-proxy:1.0` aufrufen.

Der **$upstream**-Parameter verweist auf das übergeordnete Gerät eines Geräts auf einer niedrigeren Ebene. Die Anforderung wird also über alle Ebenen weitergeleitet, bis sie die oberste Ebene erreicht, die über eine Proxyumgebung verfügt, die Containeranforderungen an das Registrierungsmodul weiterleitet. Der `:443`-Port in diesem Beispiel sollte durch den Port ersetzt werden, an dem das API-Proxymodul des übergeordneten Geräts lauscht.

Das API-Proxymodul kann Weiterleitungen nur zu einem Registrierungsmodul durchführen, und die einzelnen Registrierungsmodule können nur einer Containerregistrierung zugeordnet werden. Deshalb müssen alle Images, die von Geräten auf niedrigeren Ebenen benötigt werden, in einer einzelnen Containerregistrierung gespeichert werden.

Wenn Sie nicht möchten, dass Geräte niedrigerer Ebenen Pull Requests für Module über eine Gatewayhierarchie durchführen können, besteht eine andere Option darin, eine lokale Registrierungslösung zu verwalten. Alternativ können Sie die Modulimages auf die Geräte pushen, bevor Sie Bereitstellungen erstellen. Anschließend legen Sie **imagePullPolicy** auf **Nie** fest.

#### <a name="bootstrap-the-iot-edge-agent"></a>Bootstrapping für den IoT Edge-Agent

Der IoT Edge-Agent ist die erste Runtimekomponente, die auf einem IoT Edge-Gerät gestartet wird. Sie müssen dafür sorgen, dass alle nachgeschalteten IoT Edge-Geräte beim Start auf das edgeAgent-Modulimage zugreifen können. Danach können sie auf Bereitstellungen zugreifen und die restlichen Modulimages starten.

Wenn Sie die Konfigurationsdatei auf einem IoT Edge-Gerät aufrufen, um dessen Authentifizierungsinformationen, Zertifikate und den übergeordneten Hostnamen anzugeben, aktualisieren Sie auch das edgeAgent-Containerimage.

Wenn das Gatewaygerät der obersten Ebene so konfiguriert ist, Containerimageanforderungen zu verarbeiten, ersetzen Sie `mcr.microsoft.com` durch den übergeordneten Hostname und den API-Proxyport, an dem gelauscht wird. Im Bereitstellungsmanifest können Sie `$upstream` als Verknüpfung verwenden. Dafür ist aber erforderlich, dass das edgeHub-Modul das Routing verarbeitet und dass das Modul zu diesem Zeitpunkt noch nicht gestartet wurde. Zum Beispiel:

```toml
[agent]
name = "edgeAgent"
type = "docker"

[agent.config]
image: "{Parent FQDN or IP}:443/azureiotedge-agent:1.2"
```

Wenn Sie eine lokale Containerregistrierung verwenden oder die Containerimages auf dem Gerät manuell bereitstellen, aktualisieren Sie die Konfigurationsdatei entsprechend.

#### <a name="configure-runtime-and-deploy-proxy-module"></a>Konfigurieren der Runtime und Bereitstellen des Proxymoduls

Das **API-Proxymodul** ist für das Weiterleiten der gesamten Kommunikation zwischen der Cloud und allen nachgeschalteten IoT Edge-Geräten erforderlich. Ein IoT Edge-Gerät auf der untersten Ebene der Hierarchie, das keine nachgeschalteten IoT Edge-Geräte aufweist, benötigt dieses Modul nicht.

Das API-Proxymodul wurde so entworfen, dass es angepasst werden kann, um die häufigsten Gatewayszenarios zu verarbeiten. In diesem Artikel werden die Schritte zum Einrichten der Module in einer einfachen Konfiguration kurz erläutert. Unter [Konfigurieren des API-Proxymoduls für Ihr Gatewayhierarchieszenario](how-to-configure-api-proxy-module.md) finden Sie detailliertere Informationen und Beispiele.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem IoT Hub.
1. Klicken Sie im Navigationsmenü auf **IoT Edge**.
1. Wählen Sie das Gerät der niedrigeren Ebene aus der Liste mit **IoT Edge-Geräten** aus, das Sie konfigurieren.
1. Wählen Sie **Module festlegen** aus.
1. Klicken Sie im Abschnitt **IoT Edge-Modul** auf **Hinzufügen** und dann auf **Marketplace-Modul**.
1. Suchen Sie nach dem Modul **IoT Edge-API-Proxy**, und klicken Sie darauf.
1. Wählen Sie den Namen des API-Proxymoduls aus der Liste bereitgestellter Module aus, und aktualisieren Sie die folgenden Moduleinstellungen:
   1. Aktualisieren Sie auf der Registerkarte **Moduleinstellungen** den Wert für **Image-URI**. Ersetzen Sie `mcr.microsoft.com` durch `$upstream:443`.
   1. Aktualisieren Sie auf der Registerkarte **Umgebungsvariablen** den Wert von **NGINX_DEFAULT_PORT** in `443`.
   1. Aktualisieren Sie auf der Registerkarte mit **Optionen zum Erstellen von Containern** die Portbindungen in den Referenzport 443.

      ```json
      {
        "HostConfig": {
          "PortBindings": {
            "443/tcp": [
              {
                "HostPort": "443"
              }
            ]
          }
        }
      }
      ```

   Diese Änderungen konfigurieren das API-Proxymodul so, dass es an Port 443 lauscht. Zur Vermeidung von Portbindungskollisionen müssen Sie das edgeHub-Modul so konfigurieren, dass es nicht an Port 443 lauscht. Stattdessen leitet das API-Proxymodul sämtlichen edgeHub-Datenverkehr für Port 443 weiter.

1. Wählen Sie **Runtimeeinstellungen** aus.
1. Aktualisieren Sie die edgeHub-Moduleinstellungen:

   1. Ersetzen Sie im Feld **Image** `mcr.microsoft.com` durch `$upstream:443`.
   1. Löschen Sie im Feld mit **Erstelloptionen** die Portbindung für Port 443, übernehmen Sie jedoch die Bindungen für die Ports 5671 und 8883.

   ```json
   {
     "HostConfig": {
       "PortBindings": {
         "5671/tcp": [
           {
             "HostPort": "5671"
           }
         ],
         "8883/tcp": [
           {
             "HostPort": "8883"
           }
         ]
       }
     }
   }
   ```

1. Aktualisieren Sie die edgeAgent-Moduleinstellungen:
   1. Ersetzen Sie im Feld **Image** `mcr.microsoft.com` durch `$upstream:443`.

1. Klicken Sie auf **Speichern** zum Speichern der Änderungen an den Runtimeeinstellungen.
1. Klicken Sie auf **Weiter: Routen**, um mit dem nächsten Schritt fortzufahren.
1. Wenn Gerät-zu-Cloud-Nachrichten von nachgeschalteten Geräten IoT Hub erreichen sollen, schließen Sie eine Route ein, die alle Nachrichten an `$upstream` übergibt. Der Upstreamparameter verweist im Fall von Geräten niedrigerer Ebenen auf das übergeordnete Gerät. Zum Beispiel:
    1. **Name**: `Route`
    1. **Wert**: `FROM /messages/* INTO $upstream`
1. Klicken Sie auf **Überprüfen + erstellen**, um zum letzten Schritt zu gelangen.
1. Klicken Sie auf **Erstellen**, um die Bereitstellung für Ihr Gerät durchzuführen.

## <a name="next-steps"></a>Nächste Schritte

[Verwendung eines IoT Edge-Geräts als Gateway](iot-edge-as-gateway.md)

[Konfigurieren des API-Proxymoduls für Ihr Gatewayhierarchieszenario (Vorschau)](how-to-configure-api-proxy-module.md)