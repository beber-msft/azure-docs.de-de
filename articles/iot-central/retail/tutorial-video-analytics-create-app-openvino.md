---
title: 'Tutorial: Erstellen einer Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ in Azure IoT Central (OpenVINO)'
description: In diesem Tutorial wird veranschaulicht, wie Sie in IoT Central eine Anwendung für die Videoanalyse erstellen. Sie erstellen die Anwendung, passen sie an und stellen dafür die Verbindung mit anderen Azure-Diensten her. In diesem Tutorial wird das Intel OpenVINO&trade; Toolkit für die Echtzeitobjekterkennung verwendet.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
author: KishorIoT
ms.author: nandab
ms.date: 09/01/2021
ms.openlocfilehash: a4ac4425a3eeba6dafcaf11eb4c73f900c8fa04d
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2021
ms.locfileid: "131842767"
---
# <a name="tutorial-create-a-video-analytics---object-and-motion-detection-application-in-azure-iot-central-openvinotrade"></a>Tutorial: Erstellen einer Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ in Azure IoT Central (OpenVINO&trade;)

Informieren Sie sich darüber, wie Sie mit der IoT Central-Anwendungsvorlage vom Typ *Videoanalyse: Objekt- und Bewegungserkennung*, Azure IoT Edge-Geräten, Azure Media Services und der hardwareoptimierten OpenVINO-Lösung von Intel&trade; für die Objekt- und Bewegungserkennung eine Anwendung für die Videoanalyse erstellen. Im Rahmen der Lösung wird anhand eines Einzelhandelsgeschäfts (Filiale) veranschaulicht, wie die häufige geschäftliche Anforderung zur Überwachung von Sicherheitskameras erfüllt wird. Für die Lösung wird die automatische Objekterkennung in einem Videofeed genutzt, um interessante Ereignisse schnell identifizieren und den entsprechenden Ort ermitteln zu können.

> [!TIP]
> Informationen zur Verwendung von YOLO v3 anstelle von OpenVINO&trade; für die Objekt- und Bewegungserkennung finden Sie unter [Tutorial: Erstellen einer Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ in Azure IoT Central (YOLO v3)](tutorial-video-analytics-create-app-yolo-v3.md).

[!INCLUDE [iot-central-video-analytics-part1](../../../includes/iot-central-video-analytics-part1.md)]

- [Scratchpad.txt](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/Scratchpad.txt): Diese Datei hilft Ihnen dabei, die verschiedenen Konfigurationsoptionen zu erfassen, die Sie im Rahmen dieser Tutorials benötigen.
- [deployment.openvino.amd64.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/deploymentManifests/deployment.openvino.amd64.json)
- [LvaEdgeGatewayDcm.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/LvaEdgeGatewayDcm.json)
- [state.json](https://raw.githubusercontent.com/Azure/live-video-analytics/master/ref-apps/lva-edge-iot-central-gateway/setup/state.json): Diese Datei muss nur heruntergeladen werden, wenn Sie das Intel NUC-Gerät im zweiten Tutorial verwenden möchten.

[!INCLUDE [iot-central-video-analytics-part2](../../../includes/iot-central-video-analytics-part2.md)]

## <a name="edit-the-deployment-manifest"></a>Bearbeiten des Bereitstellungsmanifests

Sie stellen ein IoT Edge-Modul bereit, indem Sie ein Bereitstellungsmanifest verwenden. In IoT Central können Sie das Manifest als Gerätevorlage importieren und die Verwaltung der Modulbereitstellung dann IoT Central überlassen.

Bereiten Sie das Bereitstellungsmanifest wie folgt vor:

1. Öffnen Sie die Datei *deployment.openvino.amd64.json*, die Sie im Ordner *lva-configuration* gespeichert haben, in einem Text-Editor.

1. Suchen Sie nach den Einstellungen vom Typ `LvaEdgeGatewayModule`, und vergewissern Sie sich, dass der Imagename wie im folgenden Codeausschnitt lautet:

    ```json
    "LvaEdgeGatewayModule": {
        "settings": {
            "image": "mcr.microsoft.com/lva-utilities/lva-edge-iotc-gateway:1.0-amd64",
    ```

1. Fügen Sie den Namen Ihres Media Services-Kontos im Abschnitt `LvaEdgeGatewayModule` dem Knoten `env` hinzu. Den Namen Ihres Media Services-Kontos haben Sie sich in der Datei *scratchpad.txt* notiert:

    ```json
    "env": {
        "lvaEdgeModuleId": {
            "value": "lvaEdge"
        },
        "amsAccountName": {
            "value": "<YOUR_AZURE_MEDIA_SERVICES_ACCOUNT_NAME>"
        }
    }
    ```

1. Über die Vorlage werden diese Eigenschaften nicht in IoT Central verfügbar gemacht. Daher müssen Sie die Media Services-Konfigurationswerte dem Bereitstellungsmanifest hinzufügen. Suchen Sie nach dem Modul `lvaEdge`, und ersetzen Sie die Platzhalter durch die Werte, die Sie sich beim Erstellen Ihres Media Services-Kontos in der Datei *scratchpad.txt* notiert haben.

    `azureMediaServicesArmId` ist die **Ressourcen-ID**, die Sie sich beim Erstellen des Media Services-Kontos in der Datei *scratchpad.txt* notiert haben.

    Die folgende Tabelle enthält die Werte der **Verbindungsherstellung mit der Media Services-API (JSON)** in der Datei *scratchpad.txt*, die im Bereitstellungsmanifest verwendet werden sollten:

    | Bereitstellungsmanifest       | Scratchpad  |
    | ------------------------- | ----------- |
    | aadTenantId               | AadTenantId |
    | aadServicePrincipalAppId  | AadClientId |
    | aadServicePrincipalSecret | AadSecret   |

    > [!CAUTION]
    > Stellen Sie mithilfe der obigen Tabelle sicher, dass Sie die richtigen Werte im Bereitstellungsmanifest hinzufügen. Andernfalls funktioniert das Gerät nicht.

    ```json
    {
        "lvaEdge":{
        "properties.desired": {
            "applicationDataDirectory": "/var/lib/azuremediaservices",
            "azureMediaServicesArmId": "[Resource ID from scratchpad]",
            "aadTenantId": "[AADTenantID from scratchpad]",
            "aadServicePrincipalAppId": "[AadClientId from scratchpad]",
            "aadServicePrincipalSecret": "[AadSecret from scratchpad]",
            "aadEndpoint": "https://login.microsoftonline.com",
            "aadResourceId": "https://management.core.windows.net/",
            "armEndpoint": "https://management.azure.com/",
            "diagnosticsEventsOutputName": "AmsDiagnostics",
            "operationalMetricsOutputName": "AmsOperational",
            "logCategories": "Application,Event",
            "AllowUnsecuredEndpoints": "true",
            "TelemetryOptOut": false
            }
        }
    }
    ```

1. Speichern Sie die Änderungen.

In diesem Tutorial wird Ihre Lösung für die Verwendung des OpenVINO&trade;-Moduls für Objekt- und Bewegungserkennung konfiguriert. Der folgende Ausschnitt zeigt die Konfiguration des Moduls:

```json
"OpenVINOModelServerEdgeAIExtensionModule": {
    "settings": {
        "image": "marketplace.azurecr.io/intel_corporation/open_vino",
        "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"4000/tcp\":[{\"HostPort\":\"4000\"}]}},\"Cmd\":[\"/ams_wrapper/start_ams.py\",\"--ams_port=4000\",\"--ovms_port=9000\"]}"
    },
    "type": "docker",
    "version": "1.0",
    "status": "running",
    "restartPolicy": "always"
}
```

[!INCLUDE [iot-central-video-analytics-part3](../../../includes/iot-central-video-analytics-part3.md)]

### <a name="edit-the-manifest"></a>Bearbeiten des Manifests

Wählen Sie auf der Seite **LVA-Edgegateway v2** die Option **Manifest bearbeiten** aus.

:::image type="content" source="./media/tutorial-video-analytics-create-app-openvino/replace-manifest.png" alt-text="Manifest ersetzen":::

Wählen Sie **durch eine neue Datei ersetzen** aus, navigieren Sie zum Ordner *lva-configuration*, und wählen Sie die Manifestdatei *deployment.openvino.amd64.json* aus, die Sie zuvor bearbeitet haben. Wählen Sie anschließend **Speichern** aus.

[!INCLUDE [iot-central-video-analytics-part4](../../../includes/iot-central-video-analytics-part4.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Anwendung nicht mehr benötigen, können Sie alle erstellten Ressourcen wie folgt entfernen:

1. Navigieren Sie in der IoT Central-Anwendung zur Seite **Ihre Anwendung** im Abschnitt **Verwaltung**. Wählen Sie anschließend die Option **Löschen**.
1. Löschen Sie im Azure-Portal die Ressourcengruppe **lva-rg**.
1. Beenden Sie auf Ihrem lokalen Computer den Docker-Container **amp-viewer**.

## <a name="next-steps"></a>Nächste Schritte

Sie haben jetzt mit der Anwendungsvorlage **Videoanalyse: Objekt- und Bewegungserkennung** eine IoT Central-Anwendung und dann eine Gerätevorlage für das Gatewaygerät erstellt und der Anwendung anschließend ein Gatewaygerät hinzugefügt.

Gehen Sie wie folgt vor, falls Sie die Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ mit IoT Edge-Modulen ausprobieren möchten, für die eine Cloud-VM mit simulierten Videodatenströmen ausgeführt wird:

> [!div class="nextstepaction"]
> [Erstellen einer IoT Edge-Instanz für die Videoanalyse (Linux-VM)](tutorial-video-analytics-iot-edge-vm.md)

Gehen Sie wie folgt vor, falls Sie die Anwendung vom Typ „Videoanalyse: Objekt- und Bewegungserkennung“ mit IoT Edge-Modulen ausprobieren möchten, für die ein echtes Gerät mit einer echten **ONVIF**-Kamera ausgeführt wird:

> [!div class="nextstepaction"]
> [Erstellen einer IoT Edge-Instanz für die Videoanalyse (Intel NUC)](tutorial-video-analytics-iot-edge-nuc.md)
