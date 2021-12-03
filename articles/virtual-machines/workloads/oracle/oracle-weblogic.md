---
title: Lösungen zum Ausführen von Oracle WebLogic Server in Azure Virtual Machines
description: Erfahren Sie, wie Oracle WebLogic Server in Microsoft Azure Virtual Machines ausgeführt wird.
author: rezar
ms.service: virtual-machines
ms.subservice: oracle
ms.collection: linux
ms.topic: article
ms.date: 03/23/2021
ms.author: rezar
ms.custom: devx-track-java, devx-track-javaee, devx-track-javaee-wls, devx-track-javaee-wls-vm
ms.openlocfilehash: 221fd074caeb6354175e0f23a27e691cf0755acb
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131063535"
---
# <a name="what-are-solutions-for-running-oracle-weblogic-server-on-azure-virtual-machines"></a>Welche Lösungen zum Ausführen von Oracle WebLogic Server in Azure Virtual Machines gibt es?

**Gilt für**: :heavy_check_mark: Linux-VMs 

Auf dieser Seite werden die Lösungen zum Ausführen von Oracle WebLogic Server (WLS) auf virtuellen Azure-Computern beschrieben. Diese Lösungen werden gemeinsam von Oracle und Microsoft entwickelt und unterstützt.

WLS kann auch in Azure Kubernetes Service ausgeführt werden. Die dafür verfügbaren Lösungen werden in [diesem Microsoft-Artikel](./weblogic-aks.md) beschrieben.

WLS ist ein führender Java-Anwendungsserver, auf dem einige der weltweit wichtigsten unternehmenskritischen Java-Unternehmensanwendungen ausgeführt werden. WLS bildet die Middleware Foundation für die Oracle-Softwaresuite. Oracle und Microsoft sind bestrebt, WLS-Kunden mit der Auswahl und Flexibilität bei der Durchführung von Workloads in Azure als führende Cloudplattform zu unterstützen.

Die WLS-Lösungen von Azure dienen dazu, die Migration Ihrer Java-Anwendungen zu virtuellen Azure-Computern so einfach wie möglich zu machen. Hierzu werden bereitgestellte Ressourcen für die meisten gängigen Cloudbereitstellungsszenarien generiert. Von den Lösungen werden automatisch VNET, Speicher-, Java-, WLS- und Linux-Ressourcen bereitgestellt. WebLogic Server wird ohne den geringsten Aufwand installiert. Die Lösungen können Sicherheit mit einer Netzwerksicherheitsgruppe, Lastenausgleich mit Azure App Gateway, Authentifizierung mit Azure Active Directory, zentralisierte Protokollierung mit ELK sowie verteiltes Caching mit Oracle Coherence einrichten. Sie können auch automatisch eine Verbindung mit der vorhandenen Datenbank herstellen – einschließlich Azure PostgreSQL, Azure SQL und Oracle DB in der Oracle-Cloud oder in Azure. 

:::image type="content" source="media/oracle-weblogic/wls-on-azure.gif" alt-text="Bereitstellung von WebLogic Server in Azure über das Azure-Portal":::

Es stehen vier Angebote für verschiedene Szenarien zur Verfügung: [Einzelknoten ohne Verwaltungsserver](https://portal.azure.com/#create/oracle.20191001-arm-oraclelinux-wls20191001-arm-oraclelinux-wls), [Einzelknoten mit Verwaltungsserver](https://portal.azure.com/#create/oracle.20191009-arm-oraclelinux-wls-admin20191009-arm-oraclelinux-wls-admin), [Cluster](https://portal.azure.com/#create/oracle.20191007-arm-oraclelinux-wls-cluster20191007-arm-oraclelinux-wls-cluster) und [dynamischer Cluster](https://portal.azure.com/#create/oracle.20191021-arm-oraclelinux-wls-dynamic-cluster20191021-arm-oraclelinux-wls-dynamic-cluster). Die Angebot sind kostenlos verfügbar. Diese Angebote sind unten beschrieben und verknüpft.

_Für diese Angebote gilt „Bring-Your-Own-License“_ . Es wird davon ausgegangen, dass Sie bereits über die entsprechenden Lizenzen bei Oracle verfügen und mit ordnungsgemäßen Lizenzen ausgestattet sind, um Angebote in Azure auszuführen.

Die Angebote unterstützen eine Reihe von Betriebssystemen sowie Java- und WLS-Versionen über Basisimages (zum Beispiel WebLogic Server 14 und JDK 11 unter Oracle Linux 7.6). Diese Basisimages sind auch eigenständig in Azure verfügbar. Die Basisimages eignen sich für Kunden, die komplexe, angepasste Azure-Bereitstellungen benötigen. Der aktuelle Satz von Basisimages ist im [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?search=WebLogic%20Server%20Base%20Image&page=1) verfügbar.

_Wenn Sie mit dem Ingenieurteam, das diese Angebote entwickelt, eng an Ihren Migrationsszenarios zusammenarbeiten möchten, klicken Sie auf die Schaltfläche [KONTAKTFORMULAR](https://azuremarketplace.microsoft.com/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview)_ auf der [Marketplace-Übersichtsseite](https://azuremarketplace.microsoft.com/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview). Programm-Manager, Architekten und Techniker setzen sich dann in Kürze mit Ihnen in Verbindung und beginnen mit der engen Zusammenarbeit! Während sich die Angebote in der aktiven Entwicklungsphase befinden, haben Sie die Möglichkeit, kostenlos an einem Migrationsszenario zusammenzuarbeiten.

## <a name="oracle-weblogic-server-single-node"></a>Oracle WebLogic Server-Einzelknoten

[Dieses Angebot](https://portal.azure.com/#create/oracle.20191001-arm-oraclelinux-wls20191001-arm-oraclelinux-wls) stellt einen einzelnen virtuellen Computer bereit und installiert WLS darauf. Es wird weder eine Domäne erstellt noch der Verwaltungsserver gestartet. Das Einzelknotenangebot empfiehlt sich für Szenarios mit einer stark angepassten Domänenkonfiguration.

## <a name="oracle-weblogic-server-with-admin-server"></a>Oracle WebLogic Server mit Verwaltungsserver

[Dieses Angebot](https://portal.azure.com/#create/oracle.20191009-arm-oraclelinux-wls-admin20191009-arm-oraclelinux-wls-admin) stellt einen einzelnen virtuellen Computer bereit und installiert WLS darauf. Es wird eine Domäne erstellt und der Verwaltungsserver gestartet. Sie können die Domäne verwalten und sofort mit der Bereitstellung von Anwendungen beginnen.

## <a name="oracle-weblogic-server-cluster"></a>Oracle WebLogic Server-Cluster

Bei [diesem Angebot](https://portal.azure.com/#create/oracle.20191007-arm-oraclelinux-wls-cluster20191007-arm-oraclelinux-wls-cluster) wird ein hochverfügbarer Cluster von virtuellen WLS-Computern erstellt. Der Verwaltungsserver und alle verwalteten Server werden standardmäßig gestartet. Sie können sofort den Cluster verwalten und mit hochverfügbaren Anwendungen loslegen.

## <a name="oracle-weblogic-server-dynamic-cluster"></a>Dynamischer Oracle WebLogic Server-Cluster

Bei [diesem Angebot](https://portal.azure.com/#create/oracle.20191021-arm-oraclelinux-wls-dynamic-cluster20191021-arm-oraclelinux-wls-dynamic-cluster) wird ein hochverfügbarer und skalierbarer dynamischer Cluster von virtuellen WLS-Computern erstellt. Der Verwaltungsserver und alle verwalteten Server werden standardmäßig gestartet.

Die Lösungen ermöglichen eine große Bandbreite an relativ einfachen, produktionsbereiten Bereitstellungsarchitekturen. Sie können die meisten Migrationsfälle auf die produktivste Weise erreichen, indem Sie sich auf die Entwicklung von Geschäftsanwendungen konzentrieren.

:::image type="content" source="media/oracle-weblogic/weblogic-architecture-vms.png" alt-text="Komplexe WebLogic Server-Bereitstellungen sind in Azure aktiviert":::

Neben der automatischen Bereitstellung durch die Lösungen haben Kunden die Flexibilität, ihre Bereitstellungen weiter anzupassen. Ganz oben bei der Bereitstellung von Anwendungen liegt für Kunden wahrscheinlich die Integration weiterer Azure-Ressourcen mit ihren Bereitstellungen. Kunden wird empfohlen, sich mit dem [Entwicklungsteam in Verbindung zu setzen](https://azuremarketplace.microsoft.com/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview) und Feedback zur weiteren Verbesserung der Lösungen bereitzustellen.

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Angebote in Azure.

> [!div class="nextstepaction"]
> [Oracle WebLogic Server-Einzelknoten](https://portal.azure.com/#create/oracle.20191001-arm-oraclelinux-wls20191001-arm-oraclelinux-wls)

> [!div class="nextstepaction"]
> [Oracle WebLogic Server mit Verwaltungsserver](https://portal.azure.com/#create/oracle.20191009-arm-oraclelinux-wls-admin20191009-arm-oraclelinux-wls-admin)

> [!div class="nextstepaction"]
> [Oracle WebLogic Server-Cluster](https://portal.azure.com/#create/oracle.20191007-arm-oraclelinux-wls-cluster20191007-arm-oraclelinux-wls-cluster)

> [!div class="nextstepaction"]
> [Dynamischer Oracle WebLogic Server-Cluster](https://portal.azure.com/#create/oracle.20191021-arm-oraclelinux-wls-dynamic-cluster20191021-arm-oraclelinux-wls-dynamic-cluster)
