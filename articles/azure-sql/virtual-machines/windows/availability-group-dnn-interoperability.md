---
title: Featureinteroperabilität mit Verfügbarkeitsgruppen und DNN-Listener
description: 'Enthält eine Beschreibung der zusätzlichen Erwägungen beim Arbeiten mit bestimmten SQL Server-Features und eines DNN-Listeners (Distributed Network Name, verteilter Netzwerkname) mit einer Always On-Verfügbarkeitsgruppe in SQL Server auf Azure-VMs. '
services: virtual-machines
documentationCenter: na
author: rajeshsetlem
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.subservice: hadr
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/10/2021
ms.author: rsetlem
ms.reviewer: mathoma
ms.openlocfilehash: 79aefd5c4b41e86aeeb2f7f02f2df57397c4b36b
ms.sourcegitcommit: 512e6048e9c5a8c9648be6cffe1f3482d6895f24
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2021
ms.locfileid: "132157493"
---
# <a name="feature-interoperability-with-ag-and-dnn-listener"></a>Featureinteroperabilität mit Verfügbarkeitsgruppe und DNN-Listener 
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!TIP]
> Eliminieren Sie die Notwendigkeit eines verteilten Netzwerknamens für Ihre Always On Availability (AG)-Gruppe, indem Sie Ihre SQL Server-VMs in [mehreren Subnetzen](availability-group-manually-configure-prerequisites-tutorial-multi-subnet.md) innerhalb desselben virtuellen Azure-Netzwerks erstellen.

Es gibt bestimmte SQL Server-Features, für die ein hartcodierter virtueller Netzwerkname (VNN) erforderlich ist. Wenn Sie also den DNN-Listener (Distributed Network Name) mit Ihrer Always On-Verfügbarkeitsgruppe und SQL Server auf Azure-VMs in einem einzigen Subnetz verwenden, müssen Sie möglicherweise einige zusätzliche Überlegungen anstellen. 

In diesem Artikel werden SQL Server-Features und die Interoperabilität mit dem DNN-Listener für Verfügbarkeitsgruppen beschrieben. 

## <a name="behavior-differences"></a>Unterschiede im Verhalten

Es bestehen einige Verhaltensunterschiede zwischen der Funktionalität des VNN-Listeners und des DNN-Listeners, die zu beachten sind: 

- **Failoverzeit**: Die Failoverzeit ist bei Verwendung eines DNN-Listeners schneller, da nicht gewartet werden muss, bis der Netzwerklastenausgleich das Fehlerereignis erkennt und das Routing ändert. 
- **Vorhandene Verbindungen**: Verbindungen, die mit einer *bestimmten Datenbank* innerhalb einer Verfügbarkeitsgruppe hergestellt werden, für die ein Failover ausgeführt wird, werden geschlossen, aber andere Verbindungen mit dem primären Replikat bleiben geöffnet, da der DNN während des Failoverprozesses online bleibt. Dies ist anders als in einer herkömmlichen VNN-Umgebung, in der alle Verbindungen mit dem primären Replikat normalerweise geschlossen werden, wenn für die Verfügbarkeitsgruppe ein Failover ausgeführt wird, der Listener offline geht und das primäre Replikat in die sekundäre Rolle übergeht. Wenn Sie einen DNN-Listener verwenden, müssen Sie möglicherweise Anwendungsverbindungszeichenfolgen anpassen, um sicherzustellen, dass Verbindungen beim Failover an das neue primäre Replikat umgeleitet werden.
- **Offene Transaktionen:** Offene Transaktionen für eine Datenbank in einer Verfügbarkeitsgruppe, für die ein Failover ausgeführt wird, werden geschlossen und es wird ein Rollback durchgeführt. Zudem müssen Sie die Verbindung *manuell* wiederherstellen. Schließen Sie z. B. in SQL Server Management Studio das Abfragefenster, und öffnen Sie ein neues. 

## <a name="client-drivers"></a>Clienttreiber

Bei ODBC-, OLEDB-, ADO.NET-, JDBC-, PHP- und Node.js-Treibern müssen Benutzer den Namen und Port des DNN-Listeners explizit als Servernamen in der Verbindungszeichenfolge angeben. Fügen Sie `MultiSubnetFailover=True` zur Verbindungszeichenfolge hinzu, wenn der SQL-Client dies unterstützt, um bei einem Failover eine schnelle Konnektivität sicherzustellen. 

## <a name="tools"></a>Tools

Benutzer von [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms), [sqlcmd](/sql/tools/sqlcmd-utility), [Azure Data Studio](/sql/azure-data-studio/what-is) und [SQL Server Data Tools](/sql/ssdt/sql-server-data-tools) müssen den Namen und Port des DNN-Listeners explizit als Servernamen in der Verbindungszeichenfolge angeben, damit die Verbindung mit dem Listener hergestellt werden kann. 

Die Erstellung des DNN-Listeners über die grafische Benutzeroberfläche von SQL Server Management Studio (SSMS) wird derzeit nicht unterstützt. 


## <a name="availability-groups-and-fci"></a>Verfügbarkeitsgruppen und FCI

Sie können eine Always On-Verfügbarkeitsgruppe konfigurieren, indem Sie eine Failoverclusterinstanz (FCI) als eines der Replikate verwenden. Damit diese Konfiguration für den DNN-Listener funktioniert, [muss für die Failoverclusterinstanz ebenfalls der DNN verwendet werden](failover-cluster-instance-distributed-network-name-dnn-configure.md), da es keine Möglichkeit gibt, die virtuelle IP-Adresse der FCI in die Liste mit den IP-Adressen des Verfügbarkeitsgruppen-DNN einzufügen. 

Bei dieser Konfiguration muss die Spiegelungsendpunkt-URL für das FCI-Replikat den FCI-DNN verwenden. Wenn die FCI als schreibgeschütztes Replikat verwendet wird, muss für das schreibgeschützte Routing zum FCI-Replikat ebenfalls der FCI-DNN verwendet werden. 

Das Format für den Spiegelungsendpunkt ist: `ENDPOINT_URL = 'TCP://<FCI DNN DNS name>:<mirroring endpoint port>'`. 

Wenn der DNN-DNS-Name der FCI beispielsweise `dnnlsnr` lautet und `5022` der Port des Spiegelungsendpunkts der FCI ist, sieht der Transact-SQL-Codeausschnitt (T-SQL) zum Erstellen der Endpunkt-URL wie folgt aus: 

```sql
ENDPOINT_URL = 'TCP://dnnlsnr:5022'
```

Das Format für die URL für das schreibgeschützte Routing ist dann: `TCP://<FCI DNN DNS name>:<SQL Server instance port>`. 

Wenn der DNN-DNS-Name beispielsweise `dnnlsnr` ist und `1444` der Port ist, der von der schreibgeschützten Ziel-SQL Server-FCI verwendet wird, sieht der T-SQL-Codeausschnitt zum Erstellen der URL für das schreibgeschützte Routing wie folgt aus: 

```sql
READ_ONLY_ROUTING_URL = 'TCP://dnnlsnr:1444'
```

Sie können den Port in der URL weglassen, wenn es sich um den Standardport 1433 handelt. Bei einer benannten Instanz konfigurieren Sie einen statischen Port für die benannte Instanz und geben diesen in der URL für das schreibgeschützte Routing an.  

## <a name="distributed-availability-group"></a>Verteilte Verfügbarkeitsgruppe

Verteilte Verfügbarkeitsgruppen werden für den DNN-Listener derzeit nicht unterstützt. 

## <a name="replication"></a>Replikation

Für die Transaktions-, Merge- und Momentaufnahmereplikation wird jeweils die Ersetzung des VNN-Listeners durch den DNN-Listener und den zugehörigen Port in Replikationsobjekten unterstützt, die eine Verbindung mit dem Listener herstellen. 

Weitere Informationen zur Verwendung der Replikation mit Verfügbarkeitsgruppen finden Sie unter [Konfigurieren der Replikation mit Always On-Verfügbarkeitsgruppen](/sql/database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server), [Replikationsabonnenten und Always On-Verfügbarkeitsgruppen (SQL Server)](/sql/database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server) und [Einrichten der Verteilungsdatenbank für die Replikation in einer Always On-Verfügbarkeitsgruppe](/sql/relational-databases/replication/configure-distribution-availability-group).

## <a name="msdtc"></a>MSDTC

Sowohl lokale als auch gruppierte MS DTCs werden unterstützt. Für MS DTC wird aber ein dynamischer Port verwendet, sodass zum Konfigurieren des Ports für Hochverfügbarkeit eine Azure Load Balancer Standard-Instanz verwendet werden muss. Daher muss für die VM eine IP-Standardreservierung verwendet werden, da sie sonst nicht für den Zugriff über das Internet verfügbar gemacht werden kann. 

Definieren Sie zwei Regeln: eine für Port 135 der RPC-Endpunktzuordnung und eine für den tatsächlichen MS DTC-Port. Führen Sie nach dem Failover für die LB-Regel die Änderung in den neuen MS DTC-Port durch, nachdem dieser auf dem neuen Knoten geändert wurde. 

Wenn es sich um einen lokalen MS DTC handelt, müssen Sie die ausgehende Kommunikation zulassen. 

## <a name="distributed-query"></a>Verteilte Abfrage 

Für eine verteilte Abfrage muss ein Verbindungsserver verwendet werden, für den ein DNN-Listener und der zugehörige Port für die Verfügbarkeitsgruppe konfiguriert werden kann. Wenn der Port nicht 1433 lautet, sollten Sie beim Konfigurieren Ihres Verbindungsservers in SQL Server Management Studio (SSMS) die Option **Andere Datenquelle** auswählen. 

## <a name="filestream"></a>FileStream

Filestream wird unterstützt. Dies gilt aber nicht für Szenarien, in denen Benutzer mit der Windows-Datei-API auf die bereichsbezogene Dateifreigabe zugreifen. 

## <a name="filetable"></a>Dateitabelle

Dateitabelle wird unterstützt. Dies gilt aber nicht für Szenarien, in denen Benutzer mit der Windows-Datei-API auf die bereichsbezogene Dateifreigabe zugreifen. 

## <a name="linked-servers"></a>Verbindungsserver

Konfigurieren Sie den Verbindungsserver mit dem Namen und Port des DNN-Listeners für die Verfügbarkeitsgruppe. Wenn der Port nicht 1433 lautet, sollten Sie beim Konfigurieren Ihres Verbindungsservers in SQL Server Management Studio (SSMS) die Option **Andere Datenquelle** auswählen. 


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen


- Welche SQL Server-Version verfügt über Unterstützung für den DNN-Listener für Verfügbarkeitsgruppen? 

   SQL Server 2019 CU8 und höher.

- Welche Failoverzeit ist zu erwarten, wenn der DNN-Listener verwendet wird?

   Bei Verwendung eines DNN-Listeners entspricht die Failoverzeit lediglich der Failoverzeit für Verfügbarkeitsgruppen. Es kommt keine zusätzliche Zeit hinzu (wie die Testzeit bei Verwendung von Azure Load Balancer).

- Gibt es eine Versionsanforderung für SQL-Clients für die Unterstützung eines DNN mit OLEDB und ODBC?

   Wir empfehlen Ihnen, für DNN-Listener die Verbindungszeichenfolgen-Unterstützung vom Typ `MultiSubnetFailover=True` zu verwenden. Diese steht ab SQL Server 2012 (11.x) zur Verfügung.

- Sind in SQL Server für die Verwendung des DNN-Listeners Konfigurationsänderungen erforderlich? 

   Für die Verwendung eines DNN sind keine Konfigurationsänderungen in SQL Server erforderlich. Bei einigen SQL Server-Features müssen jedoch möglicherweise weitere Aspekte berücksichtigt werden. 

- Unterstützen DNNs Cluster mit mehreren Subnetzen?

   Ja. Der Cluster verknüpft den DNN im DNS mit den physischen IP-Adressen aller Replikate in der Verfügbarkeitsgruppe unabhängig vom Subnetz. Der SQL-Client probiert unabhängig vom Subnetz alle IP-Adressen des DNS-Namens aus. 

- Unterstützt der DNN-Listener der Verfügbarkeitsgruppe schreibgeschütztes Routing? 

   Ja. Schreibgeschütztes Routing wird vom DNN-Listener unterstützt. 


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter:

- [Always On-Verfügbarkeitsgruppen mit SQL Server auf Azure-VMs](availability-group-overview.md)
- [Windows Server-Failovercluster mit SQL Server auf Azure-VMs](hadr-windows-server-failover-cluster-overview.md)
- [Übersicht über Always On-Verfügbarkeitsgruppen](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)
- [HADR-Einstellungen für SQL Server auf Azure-VMs](hadr-cluster-best-practices.md)

