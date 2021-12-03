---
title: Azure Virtual Machines – IBM Db2-DBMS-Bereitstellung für SAP-Workload | Microsoft-Dokumentation
description: Azure Virtual Machines – IBM DB2-DBMS-Bereitstellung für SAP-Workload
services: virtual-machines-linux,virtual-machines-windows
author: msjuergent
manager: bburns
tags: azure-resource-manager
keywords: Azure, Db2, SAP, IBM
ms.service: virtual-machines-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/17/2021
ms.author: juergent
ms.custom: H1Hack27Feb2017, ignite-fall-2021
ms.openlocfilehash: 8740e2969030c331ffd5b45fcd88b73975ef3e63
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131013078"
---
# <a name="ibm-db2-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>Azure Virtual Machines – IBM DB2-DBMS-Bereitstellung für SAP-Workload

Mit Microsoft Azure können Sie Ihre bestehende SAP-Anwendung unter IBM Db2 für Linux, UNIX und Windows (LUW) zu Azure-VMs migrieren. Bei SAP unter IBM Db2 für LUW stehen Administratoren und Entwicklern die vertrauten Entwicklungs- und Verwaltungstools aus der lokalen Umgebung zur Verfügung.
Allgemeine Informationen zum Ausführen der SAP Business Suite unter IBM Db2 für LUW finden Sie im SAP Community Network (SCN) unter <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Weitere Informationen und Updates zu SAP unter Db2 für LUW in Azure finden Sie im SAP-Hinweis [2233094]. 

Es gibt verschiedene Artikel zu SAP-Workload in Azure.  Es wird empfohlen, zuerst den Artikel [Erste Schritte mit SAP in Azure](./get-started.md) zu lesen und dann mit dem gewünschten Thema fortzufahren.

Die nachstehenden SAP-Hinweise beziehen sich auf SAP in Azure und die in diesem Dokument behandelten Themen:

| Hinweisnummer |Titel |
| --- |--- |
| [1928533] |SAP-Anwendungen auf Azure: Unterstützte Produkte und Azure VM-Typen |
| [2015553] |SAP auf Microsoft Azure: Voraussetzungen für die Unterstützung |
| [1999351] |Problembehandlung für die erweiterte Azure-Überwachung für SAP |
| [2178632] |Wichtige Überwachungsmetriken für SAP in Microsoft Azure |
| [1409604] |Virtualisierung unter Windows: Erweiterte Überwachung |
| [2191498] |SAP unter Linux mit Azure: Erweiterte Überwachung |
| [2233094] |DB6: SAP-Anwendungen auf Azure mit IBM DB2 für Linux, UNIX und Windows – Weitere Informationen |
| [2243692] |Microsoft Azure (IaaS)-VM: SAP-Lizenzprobleme |
| [1984787] |SUSE LINUX Enterprise Server 12: Installationshinweise |
| [2002167] |Red Hat Enterprise Linux 7.x: Installation und Upgrade |
| [1597355] |Empfehlung zu Auslagerungsbereichen für Linux |

Es ist sinnvoll, im Vorfeld das Dokument [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](dbms_guide_general.md) und andere Leitfäden der [Azure-Dokumentation für SAP-Workload](./get-started.md) zu lesen. 


## <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>Versionsunterstützung für IBM Db2 für Linux, UNIX und Windows
SAP unter IBM Db2 für LUW auf Microsoft Azure Virtual Machine Services wird seit Db2 Version 10.5 unterstützt.

Informationen zu den unterstützten SAP-Produkten und Typen der Azure-VM erhalten Sie in SAP-Hinweis [1928533].

## <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Konfigurationsrichtlinien für IBM Db2 für Linux, UNIX und Windows für SAP-Installationen in Azure-VMs
### <a name="storage-configuration"></a>Speicherkonfiguration
Eine Übersicht über Azure Storage-Typen für die SAP-Workload finden Sie im Artikel [Azure Storage-Typen für die SAP-Workload](./planning-guide-storage.md). Sämtliche Datenbankdateien müssen auf eingebundenen Datenträgern von Azure Block Storage (Windows: NTFS, Linux: xfs oder ext3) gespeichert werden. Freigegebene Remotevolumes wie die Azure-Dienste in den aufgeführten Szenarien werden für Db2-Datenbankdateien **NICHT** unterstützt: 

* [Microsoft Azure-Dateidienst](/archive/blogs/windowsazurestorage/introducing-microsoft-azure-file-service) für alle Gastbetriebssysteme

* [Azure NetApp Files](https://azure.microsoft.com/services/netapp/) für Db2, ausgeführt in Windows Gastbetriebssystem. 

Freigegebene Remotevolumes wie die Azure-Dienste in den aufgeführten Szenarien werden für Db2-Datenbankdateien unterstützt: 
 
* Das Hosten von auf dem Linux-Gastbetriebssystem basierenden Db2-Daten- und -Protokolldateien auf NFS-Freigaben, die unter Azure NetApp Files gehostet werden, wird unterstützt!

Die in Artikel [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](dbms_guide_general.md) getroffenen Aussagen gelten auch für Bereitstellungen mit dem Db2-DBMS, wenn Sie Datenträger auf Basis von Azure Page Blob Storage oder Managed Disks verwenden.

Wie weiter oben im allgemeinen Teil dieses Dokuments erläutert wurde, gelten für Azure-Datenträger Kontingente für den IOPS-Durchsatz. Die jeweilige Größe der Kontingente hängt vom Typ der verwendeten VM ab. Eine Liste der VM-Typen mit ihren Kontingenten finden Sie [hier (Linux)][virtual-machines-sizes-linux] und [hier (Windows)][virtual-machines-sizes-windows].

Solange das bestehende IOPS-Kontingent pro Datenträger ausreicht, ist es möglich, alle Datenbankdateien auf einem einzelnen eingebundenen Datenträger zu speichern. Dabei sollten Sie immer die Datendateien und Transaktionsprotokolldateien auf unterschiedlichen Datenträgern/VHDs speichern.

Weitere Informationen zur Leistung finden Sie auch im Kapitel „Datensicherheit und Leistung bei Datenbankverzeichnissen“ (in englischer Sprache) in den SAP-Installationshandbüchern.

Sie können auch Windows-Speicherpools (nur verfügbar unter Windows Server 2012 und höher) verwenden (siehe [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](dbms_guide_general.md)) oder LVM oder mdadm unter Linux, um ein einziges großes, logisches Gerät aus mehreren Datenträgern zu erstellen.

<!-- log_dir, sapdata and saptmp are terms in the SAP and DB2 world and now spelling errors -->

Bei Azure-VMs der M-Serie kann die Latenz beim Schreiben in die Transaktionsprotokolle im Vergleich zur Azure Storage Premium-Leistung um Faktoren reduziert werden, wenn Azure-Schreibbeschleunigung verwendet wird. Daher sollten Sie Azure-Schreibbeschleunigung für die VHD(s) bereitstellen, die das Volume für die Db2-Transaktionsprotokolle bilden. Details finden Sie im Dokument [Schreibbeschleunigung](../../how-to-enable-write-accelerator.md).

IBM Db2 LUW 11.5 mit Unterstützung für Sektoren der Größe 4 KB. Bei älteren Db2-Versionen muss eine Sektorgröße von 512 Byte verwendet werden. SSD Premium-Datenträger haben nativ eine Sektorgröße von 4 KB und emulieren 512 Byte. Bei Ultra-Datenträgern wird standardmäßig eine Sektorgröße von 4 KB verwendet. Sie können eine Sektorgröße von 512 Byte bei der Erstellung eines Ultra-Datenträgers aktivieren. Details finden Sie unter [Verwenden von Azure Ultra-Datenträgern](../../disks-enable-ultra-ssd.md#deploy-an-ultra-disk---512-byte-sector-size). Diese Sektorgröße von 512 Byte ist eine Voraussetzung für IBM Db2 LUW-Versionen vor 11.5.

Legen Sie unter Windows für Speicherpools, die die Db2-Speicherpfade für die Verzeichnisse `log_dir`, `sapdata` und `saptmp` enthalten, eine Sektorgröße für physische Datenträger von 512 KB fest. Wenn Sie Windows-Speicherpools verwenden, müssen die Speicherpools manuell über die Befehlszeilenschnittstelle erstellt werden. Der Parameter hierfür lautet `-LogicalSectorSizeDefault`. Weitere Informationen finden Sie unter <https://technet.microsoft.com/itpro/powershell/windows/storage/new-storagepool>.


## <a name="recommendation-on-vm-and-disk-structure-for-ibm-db2-deployment"></a>Empfehlung zur VM- und Datenträgerstruktur für die Bereitstellung von IBM Db2

IBM Db2 für SAP NetWeaver-Anwendungen wird auf allen VM-Typen unterstützt, die im SAP-Supporthinweis [1928533] aufgeführt sind.  Empfohlene VM-Familien für die Ausführung der IBM Db2-Datenbank sind die Esd_v4/Eas_v4/Es_v3- und M/M_v2-Serie für große Datenbanken mit mehreren Terabyte. Die Leistung des Datenträgers beim Schreiben des IBM Db2-Transaktionsprotokolls kann durch Aktivieren der Schreibbeschleunigung für die M-Serie verbessert werden. 

Im Folgenden finden Sie eine Baselinekonfiguration für verschiedene Größen und Verwendungsmöglichkeiten von SAP-Bereitstellungen für Db2, von klein bis groß. Die Liste basiert auf Azure Storage Premium. Azure Ultra Disk wird jedoch auch vollständig mit DB2 unterstützt und kann ebenfalls verwendet werden. Verwenden Sie die Werte für Kapazität, Burstdurchsatz und Burst-IOPS, um die Konfiguration von Ultra-Datenträgern zu definieren. Sie können den IOPS-Wert für „/db2/```<SID>```/log_dir“ bei ungefähr 5.000 IOPS einschränken. 

#### <a name="extra-small-sap-system-database-size-50---200-gb-example-solution-manager"></a>Besonders kleines SAP-System: Datenbankgröße 50–200 GB: Beispiel-Lösungs-Manager
| Name/Größe des virtuellen Computers |Db2-Bereitstellungspunkt |Azure-Premium-Datenträger |Anzahl der Datenträger |IOPS |Durchsatz [MB/s] |Größe [GB] |Burst-IOPS |Burstdurchsatz [GB] | Stripegröße | Caching |
| --- | --- | --- | :---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
|E4ds_v4 |/db2 |P6 |1 |240  |50  |64  |3\.500  |170  ||  |
|vCPU: 4 |/db2/```<SID>```/sapdata |P10 |2 |1\.000  |200  |256  |7\.000  |340  |256 KB |ReadOnly |
|RAM: 32 GiB |/db2/```<SID>```/saptmp |P6 |1 |240  |50  |128  |3\.500  |170  | ||
| |/db2/```<SID>```/log_dir |P6 |2 |480  |100  |128  |7\.000  |340  |64 KB ||
| |/db2/```<SID>```/offline_log_dir |P10 |1 |500  |100  |128  |3\.500  |170  || |

#### <a name="small-sap-system-database-size-200---750-gb-small-business-suite"></a>Kleines SAP-System: Datenbankgröße 200–750 GB: kleine Business Suite
| Name/Größe des virtuellen Computers |Db2-Bereitstellungspunkt |Azure-Premium-Datenträger |Anzahl der Datenträger |IOPS |Durchsatz [MB/s] |Größe [GB] |Burst-IOPS |Burstdurchsatz [GB] | Stripegröße | Caching |
| --- | --- | --- | :---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
|E16ds_v4 |/db2 |P6 |1 |240  |50  |64  |3\.500  |170  || |
|vCPU: 16 |/db2/```<SID>```/sapdata |P15 |4 |4\.400  |500  |1.024  |14.000  |680  |256 KB |ReadOnly |
|RAM: 128 GB |/db2/```<SID>```/saptmp |P6 |2 |480  |100  |128  |7\.000  |340  |128 KB ||
| |/db2/```<SID>```/log_dir |P15 |2 |2.200  |250  |512  |7\.000  |340  |64 KB ||
| |/db2/```<SID>```/offline_log_dir |P10 |1 |500  |100  |128  |3\.500  |170  ||| 

#### <a name="medium-sap-system-database-size-500---1000-gb-small-business-suite"></a>Mittleres SAP-System: Datenbankgröße 500–1000 GB: kleine Business Suite
| Name/Größe des virtuellen Computers |Db2-Bereitstellungspunkt |Azure-Premium-Datenträger |Anzahl der Datenträger |IOPS |Durchsatz [MB/s] |Größe [GB] |Burst-IOPS |Burstdurchsatz [GB] | Stripegröße | Caching |
| --- | --- | --- | :---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
|E32ds_v4 |/db2 |P6 |1 |240  |50  |64  |3\.500  |170  || |
|vCPU: 32 |/db2/```<SID>```/sapdata |P30 |2 |10.000  |400  |2.048  |10.000  |400  |256 KB |ReadOnly |
|RAM: 256 GiB |/db2/```<SID>```/saptmp |P10 |2 |1\.000  |200  |256  |7\.000  |340  |128 KB ||
| |/db2/```<SID>```/log_dir |P20 |2 |4\.600  |300  |1.024  |7\.000  |340  |64 KB ||
| |/db2/```<SID>```/offline_log_dir |P15 |1 |1.100  |125  |256  |3\.500  |170  ||| 

#### <a name="large-sap-system-database-size-750---2000-gb-business-suite"></a>Großes SAP-System: Datenbankgröße 750–2000 GB: Business Suite
| Name/Größe des virtuellen Computers |Db2-Bereitstellungspunkt |Azure-Premium-Datenträger |Anzahl der Datenträger |IOPS |Durchsatz [MB/s] |Größe [GB] |Burst-IOPS |Burstdurchsatz [GB] | Stripegröße | Caching |
| --- | --- | --- | :---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
|E64ds_v4 |/db2 |P6 |1 |240  |50  |64  |3\.500  |170  || |
|vCPU: 64 |/db2/```<SID>```/sapdata |P30 |4 |20.000  |800  |4.096  |20.000  |800  |256 KB |ReadOnly |
|RAM: 504 GiB |/db2/```<SID>```/saptmp |P15 |2 |2.200  |250  |512  |7\.000  |340  |128 KB ||
| |/db2/```<SID>```/log_dir |P20 |4 |9\.200  |600  |2.048  |14.000  |680  |64 KB ||
| |/db2/```<SID>```/offline_log_dir |P20 |1 |2\.300  |150  |512  |3\.500  |170  || |

#### <a name="large-multi-terabyte-sap-system-database-size-2-tb-global-business-suite-system"></a>Großes SAP-System mit mehreren Terabyte: Datenbankgröße mindestens 2 TB: Globales Business Suite-System
| Name/Größe des virtuellen Computers |Db2-Bereitstellungspunkt |Azure-Premium-Datenträger |Anzahl der Datenträger |IOPS |Durchsatz [MB/s] |Größe [GB] |Burst-IOPS |Burstdurchsatz [GB] | Stripegröße | Caching |
| --- | --- | --- | :---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
|M128s |/db2 |P10 |1 |500  |100  |128  |3\.500  |170  || |
|vCPU: 128 |/db2/```<SID>```/sapdata |P40 |4 |30.000  |1,000  |8.192  |30.000  |1,000  |256 KB |ReadOnly |
|RAM:  2048 GiB |/db2/```<SID>```/saptmp |P20 |2 |4\.600  |300  |1.024  |7\.000  |340  |128 KB ||
| |/db2/```<SID>```/log_dir |P30 |4 |20.000  |800  |4.096  |20.000  |800  |64 KB |WriteAccelerator |
| |/db2/```<SID>```/offline_log_dir |P30 |1 |5\.000  |200  |1.024  |5\.000  |200  || |


### <a name="using-azure-netapp-files"></a>Verwenden von Azure NetApp Files
Die Verwendung von NFS v4.1-Volumes auf Basis von Azure NetApp Files (ANF) wird mit IBM Db2 unterstützt, das im Suse- oder Red Hat Linux-Gastbetriebssystem gehostet wird. Sie sollten mindestens vier verschiedene Volumes erstellen, die wie folgt aufgelistet sind:

- Freigegebenes Volume für saptmp1, sapmnt, usr_sap, ```<sid>```_home, db2```<sid>```_home, db2_software
- Ein Datenvolume für sapdata1 bis sapdatan
- Ein Protokollvolume für das Verzeichnis für Wiederholungsprotokolle
- Ein Volume für die Protokollarchive und Sicherungen

Ein fünftes potenzielles Volume könnte ein ANF-Volume sein, das Sie für längerfristige Sicherungen verwenden und die Momentaufnahmen im Azure Blob-Speicher speichern.

Die Konfiguration könnte wie folgt aussehen

![Beispiel für eine Db2-Konfiguration mithilfe von ANF](./media/dbms_guide_ibm/anf-configuration-example.png)


Die Leistungsstufe und die Größe der von ANF gehosteten Volumes müssen auf der Grundlage der Leistungsanforderungen ausgewählt werden. Wir empfehlen jedoch, für die Daten und das Protokollvolumen die Leistungsstufe „Ultra“ zu wählen. Es wird nicht unterstützt, Blockspeicher und gemeinsam genutzte Speichertypen für das Daten- und Protokollvolume zu mischen.

Mit den Einbindungsoptionen könnte das Einbinden dieser Volumes wie folgt aussehen (Sie müssen ```<SID>``` und ```<sid>``` durch die SID Ihres SAP-Systems ersetzen):

```
vi /etc/idmapd.conf   
 # Example
 [General]
 Domain = defaultv4iddomain.com
 [Mapping]
 Nobody-User = nobody
 Nobody-Group = nobody

mount -t nfs -o rw,hard,sync,rsize=1048576,wsize=1048576,sec=sys,vers=4.1,tcp 172.17.10.4:/db2shared /mnt 
mkdir -p /db2/Software /db2/AN1/saptmp /usr/sap/<SID> /sapmnt/<SID> /home/<sid>adm /db2/db2<sid> /db2/<SID>/db2_software
mkdir -p /mnt/Software /mnt/saptmp  /mnt/usr_sap /mnt/sapmnt /mnt/<sid>_home /mnt/db2_software /mnt/db2<sid>
umount /mnt

mount -t nfs -o rw,hard,sync,rsize=1048576,wsize=1048576,sec=sys,vers=4.1,tcp 172.17.10.4:/db2data /mnt
mkdir -p /db2/AN1/sapdata/sapdata1 /db2/AN1/sapdata/sapdata2 /db2/AN1/sapdata/sapdata3 /db2/AN1/sapdata/sapdata4
mkdir -p /mnt/sapdata1 /mnt/sapdata2 /mnt/sapdata3 /mnt/sapdata4
umount /mnt

mount -t nfs -o rw,hard,sync,rsize=1048576,wsize=1048576,sec=sys,vers=4.1,tcp 172.17.10.4:/db2log /mnt 
mkdir /db2/AN1/log_dir
mkdir /mnt/log_dir
umount /mnt

mount -t nfs -o rw,hard,sync,rsize=1048576,wsize=1048576,sec=sys,vers=4.1,tcp 172.17.10.4:/db2backup /mnt
mkdir /db2/AN1/backup
mkdir /mnt/backup
mkdir /db2/AN1/offline_log_dir /db2/AN1/db2dump
mkdir /mnt/offline_log_dir /mnt/db2dump
umount /mnt
```

>[!NOTE]
> Die Einbindungsoptionen „hard“ und „sync“ sind erforderlich.


### <a name="backuprestore"></a>Sichern und Wiederherstellen
Die Funktionen zum Sichern und Wiederherstellen werden für IBM Db2 für LUW genauso unterstützt wie auf standardmäßigen Windows Server-Betriebssystemen und Hyper-V.

Stellen Sie sicher, dass eine gültige Strategie für die Datenbanksicherung vorhanden ist. 

Wie bei der Bare-Metal-Bereitstellung hängt die Leistung bei der Sicherung und Wiederherstellung davon ab, wie viele Volumes parallel ausgelesen werden können und wie hoch deren Durchsatz ist. Bei virtuellen Computern mit bis zu acht CPU-Threads sollte unter Umständen auch der CPU-Verbrauch durch die Sicherungskomprimierung beachtet werden. Es gilt also Folgendes:

* Je geringer die Anzahl an Datenträgern, auf denen sich Datenbankgeräte befinden, desto geringer der Durchsatz beim Auslesen insgesamt.
* Je weniger CPU-Threads in der VM, desto schwerwiegender der Einfluss der Sicherungskomprimierung.
* Je weniger Ziele (Stripesetverzeichnisse, Datenträger), in bzw. auf die die Sicherung geschrieben wird, desto geringer der Durchsatz.

Wenn Sie die Anzahl der Ziele erhöhen möchten, stehen Ihnen je nach Anforderungen zwei Optionen zur Verfügung, die Sie verwenden bzw. kombinieren können:

* Striping des Zielvolumes der Sicherung auf mehrere Datenträger, wodurch der IOPS-Durchsatz auf den betreffenden Datenträgervolumes verbessert wird
* Verwenden von mehr als einem Zielverzeichnis für das Sichern

>[!NOTE]
>Db2 für Windows unterstützt nicht die Windows VSS-Technologie. Daher kann nicht die anwendungskonsistente VM-Sicherung des Azure Backup-Diensts nicht für VMs genutzt werden, in denen Db2-DBMS bereitgestellt wird.

### <a name="high-availability-and-disaster-recovery"></a>Hochverfügbarkeit und Notfallwiederherstellung

#### <a name="linux-pacemaker"></a>Linux Pacemaker

Hochverfügbarkeit und Notfallwiederherstellung (HADR) in Db2 mit Pacemaker wird unterstützt. Die Betriebssysteme SLES und RHEL werden unterstützt. Diese Konfiguration ermöglicht die Hochverfügbarkeit von IBM Db2 für SAP. Bereitstellungsrichtlinien:
* SLES: [Hochverfügbarkeit von IBM Db2 LUW auf Azure-VMs unter SUSE Linux Enterprise Server mit Pacemaker](dbms-guide-ha-ibm.md) 
* RHEL: [Hochverfügbarkeit von IBM Db2 LUW auf Azure-VMs unter Red Hat Enterprise Linux Server](high-availability-guide-rhel-ibm-db2-luw.md)

#### <a name="windows-cluster-server"></a>Windows Cluster Server

Microsoft Cluster Server (MSCS) wird nicht unterstützt.

Hochverfügbarkeit und Notfallwiederherstellung (HADR) in Db2 wird unterstützt. Wenn die an der Hochverfügbarkeitskonfiguration teilnehmenden virtuellen Computer über eine funktionierende Namensauflösung verfügen, unterscheidet sich die Einrichtung in Azure nicht von anderen, lokal ausgeführten Einrichtungen. Es wird nicht empfohlen, ausschließlich die IP-Auflösung zugrunde zu legen.

Verwenden Sie nicht die geografische Replikation für die Speicherkonten, in denen die Datenbankdatenträger gespeichert werden. Weitere Informationen finden Sie im Artikel [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](dbms_guide_general.md). 

### <a name="accelerated-networking"></a>Beschleunigter Netzwerkbetrieb
Für Db2-Bereitstellungen unter Windows wird dringend empfohlen, die Azure-Funktionalität des beschleunigten Netzwerkbetriebs zu nutzen, wie in Artikel [VM-Leistungssteigerung mit beschleunigtem Netzwerkbetrieb – jetzt verfügbar für Windows und Linux](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) (in englischer Sprache) beschrieben. Nützlich sind auch die Empfehlungen unter [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](dbms_guide_general.md). 


### <a name="specifics-for-linux-deployments"></a>Besonderheiten von Linux-Bereitstellungen
Solange das bestehende IOPS-Kontingent pro Datenträger ausreicht, ist es möglich, alle Datenbankdateien auf einem einzelnen Datenträger zu speichern. Dabei sollten Sie immer die Datendateien und Transaktionsprotokolldateien auf unterschiedlichen Datenträgern speichern.

Wenn der IOPS- oder E/A-Durchsatz einer einzelnen Azure-VHD nicht ausreicht, können Sie LVM oder MDADM verwenden (siehe [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](dbms_guide_general.md)), um ein einziges großes, logisches Gerät aus mehreren Datenträgern zu erstellen.
Legen Sie für die Datenträger, die die Db2-Speicherpfade für die Verzeichnisse „sapdata“ und „saptmp“ enthalten, eine physische Datenträgersektorgröße von 512 KB fest.

<!-- sapdata and saptmp are terms in the SAP and DB2 world and now spelling errors -->


### <a name="other"></a>Andere
Alle anderen allgemeinen Themen wie Azure-Verfügbarkeitsgruppen oder SAP-Überwachung (siehe [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](dbms_guide_general.md)) gelten ebenfalls für die Bereitstellung von VMs mit der IBM-Datenbank.

## <a name="next-steps"></a>Nächste Schritte
Artikel lesen 

- [Azure Virtual Machines – DBMS-Bereitstellung für SAP-Workload](dbms_guide_general.md)


[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[dbms-guide]:dbms-guide.md 
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f 
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b 
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d 
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c 
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8
[dbms-guide-managed-disks]:dbms-guide.md#f42c6cb5-d563-484d-9667-b07ae51bce29

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md 
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab 
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e 
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e 
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc 
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d 
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f 
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca 
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b 
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d 
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:https://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md 
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff 
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f 
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a 
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c 
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef 
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a 
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d 
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 

[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd 
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f 

[powershell-install-configure]: /powershell/azure/azurerm/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/management/overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
[sap-pam]:https://support.sap.com/pam 
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fapplication-workloads%2Fsap%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../disks-types.md
