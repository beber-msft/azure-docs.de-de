---
title: VNET-Regeln – Azure Database for PostgreSQL – Einzelserver
description: Informieren Sie sich, wie Sie VNET-Dienstendpunkte zum Herstellen einer Verbindung mit Azure Database for PostgreSQL (Einzelserver) verwenden.
author: niklarin
ms.author: nlarin
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/17/2020
ms.openlocfilehash: 79645a864d2d9b3652b3b099cc20607148ed871c
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131064998"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-database-for-postgresql---single-server"></a>Verwenden von VNET-Dienstendpunkten und -Regeln für Azure Database for PostgreSQL – Einzelserver

*VNET-Regeln* sind eine Firewallsicherheitsfunktion, die steuert, ob Ihr Azure Database for PostgreSQL-Server Nachrichten zulässt, die von bestimmten Subnetzen in virtuellen Netzwerken gesendet werden. In diesem Artikel erfahren Sie, warum die VNET-Regelfunktion manchmal die sicherste Option darstellt, um Nachrichten an Ihren Azure Database for PostgreSQL-Server zuzulassen.

Zum Erstellen einer VNET-Regel benötigen Sie ein [virtuelles Netzwerk][vm-virtual-network-overview] (VNET) und einen [VNET-Dienstendpunkt][vm-virtual-network-service-endpoints-overview-649d] für die Regel, auf die verwiesen wird. Die folgende Abbildung zeigt, wie ein VNET-Dienstendpunkt mit Azure Database for PostgreSQL funktioniert:

:::image type="content" source="media/concepts-data-access-and-security-vnet/vnet-concept.png" alt-text="Beispielhafte Funktionsweise eines VNET-Dienstendpunkts":::

> [!NOTE]
> Dieses Feature steht in allen Regionen der öffentlichen Azure-Cloud zur Verfügung, in denen Azure Database for PostgreSQL für universelle und arbeitsspeicheroptimierte Server bereitgestellt wird.
> Wenn beim VNET-Peering der Datenverkehr über eine gemeinsame VNet Gateway-Instanz mit Dienstendpunkten fließt und an den Peer fließen soll, erstellen Sie eine ACL/VNET-Regel, damit Azure Virtual Machines im Gateway-VNET auf den Azure Database for PostgreSQL-Server zugreifen kann.

Sie können auch die Verwendung [Private Link](concepts-data-access-and-security-private-link.md) für Verbindungen in Erwägung ziehen. Private Link stellt eine private IP-Adresse in Ihrem VNET für den Azure Database for PostgreSQL-Server bereit.

<a name="anch-terminology-and-description-82f"></a>
## <a name="terminology-and-description"></a>Terminologie und Beschreibung

**Virtuelles Netzwerk:** Sie können Ihrem Azure-Abonnement virtuelle Netzwerke zuordnen.

**Subnetz:** Ein virtuelles Netzwerk enthält **Subnetze**. Alle virtuellen Azure-Computer (VMs) innerhalb des VNet werden einem Subnetz zugewiesen. Ein Subnetz kann mehrere VMs und/oder andere Computeknoten enthalten. Computeknoten, die sich außerhalb Ihres virtuellen Netzwerks befinden, können nicht auf Ihr virtuelles Netzwerk zugreifen, es sei denn, Sie konfigurieren für sie den sicheren Zugriff.

**Virtual Network-Dienstendpunkt:** Ein [VNET-Dienstendpunkt][vm-virtual-network-service-endpoints-overview-649d] ist ein Subnetz, dessen Eigenschaftswerte mindestens einen formalen Azure-Diensttypnamen enthalten. In diesem Artikel beschäftigen wir uns mit dem Typnamen **Microsoft.Sql**, der auf einen Azure-Dienst mit dem Namen „SQL-Datenbank“ verweist. Dieses Diensttag gilt auch für die Azure Database for PostgreSQL- und Azure Database for MySQL-Dienste. Es ist wichtig zu beachten, dass beim Anwenden des Diensttags **Microsoft.Sql** auf einen VNet-Dienstendpunkt der Datenverkehr des Dienstendpunkts für Azure-Datenbankdienste konfiguriert wird: SQL-Datenbank-, Azure Synapse Analytics-, Azure Database for PostgreSQL- und Azure Database for MySQL-Server im Subnetz. 

**VNET-Regel:** Eine VNET-Regel für Ihren Azure Database for PostgreSQL-Server ist ein Subnetz, das in der Zugriffssteuerungsliste Ihres Azure Database for PostgreSQL-Servers aufgeführt wird. Das Subnetz muss den Typnamen **Microsoft.Sql** enthalten, um in der Zugriffssteuerungsliste für Ihren Azure Database for PostgreSQL-Server zu sein.

Eine VNET-Regel weist Ihren Azure Database for PostgreSQL-Server an, Nachrichten von jedem Knoten im Subnetz anzunehmen.

<a name="anch-details-about-vnet-rules-38q"></a>

## <a name="benefits-of-a-virtual-network-rule"></a>Vorteile einer VNET-Regel

Die virtuellen Computer in Ihren Subnetzen können nicht mit Ihrem Azure Database for PostgreSQL-Server kommunizieren, bis Sie dies einrichten. Eine Aktion zum Herstellen der Kommunikation stellt die Erstellung einer VNET-Regel dar. Die Begründung der Entscheidung für eine VNET-Regel erfordert eine Erörterung der Vor- und Nachteile, die die von der Firewall gebotenen konkurrierenden Sicherheitsoptionen berücksichtigt.

### <a name="allow-access-to-azure-services"></a>Zugriff auf Azure-Dienste erlauben

Der Verbindungssicherheitsbereich hat eine **EIN/AUS** Schaltfläche mit der Bezeichnung **Zugriff auf Azure-Dienste erlauben**. Die Einstellung **EIN** lässt Nachrichten von allen Azure IP-Adressen und aus allen Azure-Subnetzen zu. Diese Azure-IP-Adressen oder -Subnetze gehören möglicherweise nicht Ihnen. Diese Einstellung **EIN** lässt wahrscheinlich einen umfassenderen Zugriff auf Ihre Azure Database for PostgreSQL-Datenbank zu, als von Ihnen gewünscht. Eine VNET-Regel ermöglicht eine präzisere Steuerung.

### <a name="ip-rules"></a>IP-Regeln

Mit der Azure Database for PostgreSQL-Firewall können Sie IP-Adressbereiche bestimmen, aus denen Nachrichten an die Azure Database for PostgreSQL-Datenbank gesendet werden dürfen. Dieser Ansatz eignet sich gut für statische IP-Adressen, die sich außerhalb des privaten Azure-Netzwerks befinden. Doch viele Knoten innerhalb des privaten Azure-Netzwerks sind mit *dynamischen* IP-Adressen konfiguriert. Dynamische IP-Adressen können sich ändern, z.B. wenn Ihre VM neu gestartet wird. Es wäre töricht, eine dynamische IP-Adresse in einer Firewallregel in einer Produktionsumgebung anzugeben.

Sie können die IP-Option weiter nutzen, indem Sie eine *statische* IP-Adresse für Ihre VM abrufen. Einzelheiten finden Sie unter [Konfigurieren von privaten IP-Adressen für einen virtuellen Computer über das Azure-Portal][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Der Ansatz mit statischen IP-Adressen kann jedoch schwierig zu handhaben und aufwendig sein, wenn er in großem Maßstab befolgt wird. VNET-Regeln sind einfacher einzurichten und zu verwalten.


<a name="anch-details-about-vnet-rules-38q"></a>

## <a name="details-about-virtual-network-rules"></a>Details zu VNET-Regeln

In diesem Abschnitt werden verschiedene Details zu VNET-Regeln beschrieben.

### <a name="only-one-geographic-region"></a>Nur eine geografische Region

Jeder Virtual Network-Dienstendpunkt gehört nur zu einer Azure-Region. Der Endpunkt ermöglicht anderen Regionen nicht das Akzeptieren von Nachrichten aus dem Subnetz.

Eine VNET-Regel ist auf die Region beschränkt, zu der der zugrunde liegende Endpunkt gehört.

### <a name="server-level-not-database-level"></a>Auf Serverebene, nicht auf Datenbankebene

Jede VNET-Regel gilt für den gesamten Azure Database for PostgreSQL-Server und nicht nur für eine bestimmte Datenbank auf dem Server. Das heißt, dass VNET-Regeln auf Server- und nicht auf Datenbankebene gelten.

#### <a name="security-administration-roles"></a>Sicherheitsverwaltungsrollen

Bei der Verwaltung der VNET-Dienstendpunkte erfolgt eine Trennung von Sicherheitsrollen. Die folgenden Rollen müssen Aktionen ausführen:

- **Netzwerkadministrator:** &nbsp;Aktivieren des Endpunkts.
- **Datenbankadministrator:** &nbsp;Aktualisieren der Zugriffssteuerungsliste, um das angegebene Subnetz dem Azure Database for PostgreSQL-Server hinzuzufügen.

*Alternative zur rollenbasierten Zugriffssteuerung von Azure (Azure RBAC):*

Die Rollen „Netzwerkadministrator“ und „Datenbankadministrator“ haben mehr Zugriffsrechte, als für die Verwaltung von VNET-Regeln erforderlich ist. Es wird nur eine Teilmenge der Zugriffsrechte benötigt.

Sie können die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)][rbac-what-is-813s] in Azure verwenden, um eine einzelne benutzerdefinierte Rolle zu erstellen, die nur über die erforderliche Teilmenge von Berechtigungen verfügt. Die benutzerdefinierte Rolle kann definiert werden, anstatt den Netzwerk- oder Datenbankadministrator einzubeziehen. Die auf die Sicherheit bezogene Angriffsfläche ist kleiner, wenn Sie einen Benutzer einer benutzerdefinierte Rolle hinzufügen und ihn nicht den beiden anderen wichtigen Administratorrollen hinzufügen.

> [!NOTE]
> In einigen Fällen befinden sich Azure Database for PostgreSQL und das VNET-Subnetz in unterschiedlichen Abonnements. In diesen Fällen müssen Sie folgende Konfigurationen sicherstellen:
> - Beide Abonnements müssen demselben Azure Active Directory-Mandanten zugeordnet sein.
> - Der Benutzer muss über die erforderlichen Berechtigungen zum Initiieren der Vorgänge verfügen. Dazu gehören z.B. das Aktivieren von Dienstendpunkten und das Hinzufügen eines VNET-Subnetzes auf dem angegebenen Server.
> - Stellen Sie sicher, dass für beide Abonnements die Ressourcenanbieter **Microsoft.Sql** und **Microsoft.DBforPostgreSQL** registriert sind. Weitere Informationen finden Sie unter [Azure-Ressourcenanbieter und -typen][resource-manager-portal].

## <a name="limitations"></a>Einschränkungen

Bei Azure Database for PostgreSQL gelten für VNET-Regeln folgende Einschränkungen:

- Eine Web-App kann einer privaten IP in einem VNET/Subnetz zugeordnet werden. Auch wenn Dienstendpunkte im entsprechenden VNET/Subnetz aktiviert sind, haben Verbindungen zwischen der Web-App und dem Server keine VNET-/Subnetzquelle, sondern eine öffentliche Azure-IP-Quelle. Um die Verbindung einer Web-App mit einem Server mit VNET-Firewallregeln zu ermöglichen, müssen Sie auf dem Server Azure-Diensten den Zugriff auf den Server erlauben.

- In der Firewall für Azure Database for PostgreSQL verweist jede VNET-Regel auf ein Subnetz. Alle diese Subnetze müssen in derselben geografischen Region gehostet werden wie Azure Database for PostgreSQL.

- Jeder Azure Database for PostgreSQL-Server kann für ein beliebiges virtuelles Netzwerk maximal 128 Einträge in der Zugriffssteuerungsliste haben.

- VNET-Regeln gelten nur für virtuelle Netzwerke gemäß dem Azure Resource Manager-Modell und nicht gemäß dem [klassischen Bereitstellungsmodell][arm-deployment-model-568f].

- Wenn Sie die VNET-Dienstendpunkte für Azure Database for PostgreSQL mit dem Diensttag **Microsoft.Sql** aktivieren, werden auch die Endpunkte für alle Azure-Datenbankdienste aktiviert: Azure Database for MySQL, Azure Database for PostgreSQL, Azure SQL-Datenbank und Azure Synapse Analytics.

- VNET-Dienstendpunkte werden nur für Server vom Typ „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.

- Wenn **Microsoft.Sql** in einem Subnetz aktiviert ist, weist dies darauf hin, dass Sie für Verbindungen nur VNET-Regeln nutzen möchten. [VNET-fremde Firewallregeln](concepts-firewall-rules.md) von Ressourcen in diesem Subnetz funktionieren nicht.

- In der Firewall gelten zwar IP-Adressbereiche für die folgenden Netzwerkelemente, VNET-Regeln jedoch nicht:
    - [Virtuelles privates S2S-Netzwerk (Site-to-Site-VPN)][vpn-gateway-indexmd-608y]
    - Lokal über [ExpressRoute][expressroute-indexmd-744v]

## <a name="expressroute"></a>ExpressRoute

Wenn Ihr Netzwerk über [ExpressRoute][expressroute-indexmd-744v] mit Ihrem Azure-Netzwerk verbunden ist, wird jede Verbindung mit zwei öffentlichen IP-Adressen im Microsoft-Edgebereich konfiguriert. Die beiden IP-Adressen werden zum Herstellen der Verbindung mit Microsoft-Diensten, z.B. Azure Storage, mithilfe von öffentlichem Azure-Peering verwendet.

Sie müssen IP-Netzwerkregeln für die öffentlichen IP-Adressen Ihrer Verbindungen erstellen, um die Kommunikation zwischen Ihrer Verbindung und Azure Database for PostgreSQL zu ermöglichen. Öffnen Sie über das Azure-Portal ein Supportticket für ExpressRoute, um die öffentlichen IP-Adressen Ihrer ExpressRoute-Verbindung zu ermitteln.

## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>Hinzufügen einer VNET-Firewallregel zu Ihrem Server ohne Aktivierung von VNET-Dienstendpunkten

Allein das Festlegen einer VNET-Firewallregel trägt nicht zur Sicherung des Servers im VNET bei. Sie müssen auch VNET-Dienstendpunkte **aktivieren** , damit der Server gesichert wird. Wenn Sie Dienstendpunkte **aktivieren** , fällt das VNET-Subnetz solange aus, bis der Übergang von **„deaktiviert“** zu **„aktiviert“** abgeschlossen ist. Dies gilt vor allem für sehr umfangreiche VNETs. Mithilfe des Flags **IgnoreMissingServiceEndpoint** können Sie die Ausfallzeit während des Übergangs reduzieren bzw. vermeiden.

Verwenden Sie die Azure CLI oder das Azure-Portal, um das Flag **IgnoreMissingServiceEndpoint** festzulegen.

## <a name="related-articles"></a>Verwandte Artikel
- [Virtuelle Azure-Netzwerke][vm-virtual-network-overview]
- [Azure-VNET-Dienstendpunkte][vm-virtual-network-service-endpoints-overview-649d]

## <a name="next-steps"></a>Nächste Schritte
Hier finden Sie weitere Artikel zum Erstellen von VNET-Regeln:
- [Erstellen und Verwalten von VNET-Regeln für Azure Database for PostgreSQL über das Azure-Portal](howto-manage-vnet-using-portal.md)
- [Erstellen und Verwalten von VNET-Regeln für Azure Database for PostgreSQL über Azure CLI](howto-manage-vnet-using-cli.md)


<!-- Link references, to text, Within this same GitHub repo. -->
[arm-deployment-model-568f]: ../azure-resource-manager/management/deployment-models.md

[vm-virtual-network-overview]: ../virtual-network/virtual-networks-overview.md

[vm-virtual-network-service-endpoints-overview-649d]: ../virtual-network/virtual-network-service-endpoints-overview.md

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/ip-services/virtual-networks-static-private-ip-arm-pportal.md

[rbac-what-is-813s]: ../role-based-access-control/overview.md

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml

[expressroute-indexmd-744v]: ../expressroute/index.yml

[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md