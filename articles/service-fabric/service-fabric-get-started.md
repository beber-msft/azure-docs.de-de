---
title: Einrichten einer Windows-Entwicklungsumgebung
description: Installieren Sie Laufzeit, SDK und Tools, und erstellen Sie einen lokalen Entwicklungscluster. Nach Abschluss des Setups können Sie mit der Erstellung von Anwendungen unter Windows beginnen.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/16/2020
ms.custom: sfrev, devx-track-azurepowershell
ms.openlocfilehash: 8f520d74f8b3ccac8c31fce6fbf4af2016cb51ac
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2021
ms.locfileid: "130217713"
---
# <a name="prepare-your-development-environment-on-windows"></a>Vorbereiten Ihrer Entwicklungsumgebung unter Windows

> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [Mac OS X](service-fabric-get-started-mac.md)
>
>

Zum Erstellen und Ausführen von [Azure Service Fabric-Anwendungen][1] müssen auf dem Windows-Entwicklungscomputer die Service Fabric-Laufzeit, das SDK und Tools installiert werden. Außerdem müssen Sie die [Ausführung der im SDK enthaltenen Windows PowerShell-Skripts aktivieren](#enable-powershell-script-execution).

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie eine unterstützte [Windows-Version](service-fabric-versions.md#supported-windows-versions-and-support-end-date) verwenden:

## <a name="install-the-sdk-and-tools"></a>Installieren des SDKs und der Tools

Für die Installation des SDK und der Tools wird der Webplattform-Installer (WebPI) empfohlen. Wenn bei der Verwendung des WebPI Laufzeitfehler auftreten, finden Sie in den Anmerkungen zu einer bestimmten Service Fabric-Version direkte Links zu den Installern. Die Anmerkungen zur Version finden Sie in den verschiedenen Versionsankündigungen im [Service Fabric-Teamblog](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric).

> [!NOTE]
> Upgrades eines lokalen Service Fabric-Entwicklungsclusters werden nicht unterstützt.

### <a name="to-use-visual-studio-2017-or-2019"></a>So verwenden Sie Visual Studio 2017 oder 2019

Die Service Fabric-Tools sind Bestandteil der Workload für die Azure-Entwicklung in Visual Studio 2019 und 2017. Aktivieren Sie diese Workload im Rahmen der Visual Studio-Installation.
Zudem müssen Sie das Microsoft Azure Service Fabric SDK und die Laufzeit mithilfe des Webplattform-Installers installieren.

* [Installieren Sie das Microsoft Azure Service Fabric-SDK][core-sdk].

### <a name="sdk-installation-only"></a>Nur SDK-Installation

Wenn Sie nur das SDK benötigen, können Sie dieses Paket installieren:

* [Installieren Sie das Microsoft Azure Service Fabric-SDK][core-sdk].

Aktuelle Versionen:

* Service Fabric-SDK und -Tools 5.2.1235
* Service Fabric-Runtime 8.2.1235

Eine Liste mit unterstützten Versionen finden Sie unter [Unterstützte Service Fabric-Versionen](service-fabric-versions.md).

> [!NOTE]
> Cluster mit einem Computer (OneBox) werden für Anwendungs- oder Clusterupgrades nicht unterstützt. Löschen Sie den OneBox-Cluster, und erstellen Sie ihn neu, wenn Sie ein Clusterupgrade ausführen müssen oder wenn Probleme beim Ausführen eines Anwendungsupgrades auftreten. 

## <a name="enable-powershell-script-execution"></a>Aktivieren der PowerShell-Skriptausführung

Service Fabric verwendet Windows PowerShell-Skripts zum Erstellen eines lokalen Entwicklungsclusters und zum Bereitstellen von Anwendungen aus Visual Studio. Die Ausführung dieser Skripts wird von Windows standardmäßig blockiert. Um die Skripts zu aktivieren, müssen Sie die PowerShell-Ausführungsrichtlinie ändern. Öffnen Sie PowerShell als Administrator, und geben Sie folgenden Befehl ein:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="install-docker-optional"></a>Installieren von Docker (optional)

[Service Fabric ist der Containerorchestrator](service-fabric-containers-overview.md), der Microservices für einen Cluster von Computern bereitstellt. Zum Ausführen von Windows-Containeranwendungen in Ihrem lokalen Entwicklungscluster müssen Sie zunächst Docker für Windows installieren. Laden Sie [Docker CE für Windows (stabil)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) herunter. Klicken Sie nach dem Installieren und Starten von Docker mit der rechten Maustaste auf das Taskleistensymbol, und wählen Sie **Switch to Windows containers** (Zu Windows-Containern wechseln). Dieser Schritt ist für die Ausführung Windows-basierter Docker-Images erforderlich.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Entwicklungsumgebung eingerichtet haben, können Sie nun mit dem Erstellen und Ausführen von Apps beginnen.

* [Informationen zum Erstellen, Bereitstellen und Verwalten von Anwendungen](service-fabric-tutorial-create-dotnet-app.md)
* [Weitere Informationen zu den Programmiermodellen: Reliable Services und Reliable Actors](service-fabric-choose-framework.md)
* [Service Fabric-Codebeispiele auf GitHub](/samples/browse/?products=azure)
* [Visualisieren des Clusters mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Vorbereiten einer Linux-Entwicklungsumgebung unter Windows](service-fabric-local-linux-cluster-windows.md)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).

[1]: https://azure.microsoft.com/campaigns/service-fabric/ "Service Fabric-Kampagnenseite"
[2]: https://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "WebPI-Link für VS 2015"
[full-bundle-dev15]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "WebPI-Link für Dev15"
[core-sdk]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "WebPI-Link für Core SDK"
[powershell5-download]:https://www.microsoft.com/download/details.aspx?id=54616
