---
title: 'Empfohlene Leistungsbenchmarktests: Azure NetApp Files'
description: Hier finden Sie Informationen zu Benchmarktestempfehlungen für Volumeleistung und -metriken mit Azure NetApp Files.
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: conceptual
ms.date: 11/09/2021
ms.openlocfilehash: 7c53fb0fbbba9c7c0ad40739b754d4b01bbd934b
ms.sourcegitcommit: 838413a8fc8cd53581973472b7832d87c58e3d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2021
ms.locfileid: "132136654"
---
# <a name="performance-benchmark-test-recommendations-for-azure-netapp-files"></a>Testempfehlungen von Leistungsbenchmarks für Azure NetApp Files

Dieser Artikel bietet Benchmarktestempfehlungen für Volumeleistung und -metriken mit Azure NetApp Files.

## <a name="overview"></a>Übersicht

Um die Leistungsmerkmale eines Azure NetApp Files-Volumes zu verstehen, können Sie mit dem Open-Source-Tool [FIO](https://github.com/axboe/fio) eine Reihe von Benchmarktests ausführen, um verschiedene Workloads zu simulieren. FIO kann sowohl auf Linux- als auch auf Windows-basierten Betriebssystemen installiert werden.  Dieses hervorragende Tool bietet sowohl über IOPS als auch Durchsatz eines Volumes einen schnellen Überblick.

> [!IMPORTANT]
> Es wird *nicht* empfohlen, Azure NetApp Files mit dem Hilfsprogramm `dd` als Baseline-Benchmarktool zu verwenden. Sie sollten eine tatsächliche Anwendungsworkload, Workloadsimulation, Benchmarktests und Analysetools (z. B. Oracle AWR mit Oracle oder das IBM-Äquivalent für DB2) verwenden, um eine optimale Infrastrukturleistung zu erzielen und analysieren zu können. Tools wie FIO, vdbench und iometer dienen zur Bestimmung der Speichergrenzwerte von VMs. Sie vergleichen die Parameter des Tests mit den tatsächlichen kombinierten Anwendungsworkloads, um hilfreiche Ergebnisse zu erzielen. Es ist jedoch immer am besten, mit der tatsächlichen Anwendung zu testen.  
### <a name="vm-instance-sizing"></a>Dimensionierung einer VM-Instanz

Um die besten Ergebnisse zu erzielen, stellen Sie sicher, dass Sie eine VM-Instanz (virtueller Computer) verwenden, die für die Durchführung der Tests angemessen dimensioniert ist. Die folgenden Beispiele verwenden eine Standard_D32s_v3-Instanz. Weitere Informationen zu VM-Instanzengrößen finden Sie für Windows-basierte virtuelle Computer unter [Größen für virtuelle Windows-Computer in Azure](../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und für virtuelle Linux-Computer unter [Größen für virtuelle Linux-Computer in Azure](../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="azure-netapp-files-volume-sizing"></a>Volumegrößenanpassung in Azure NetApp Files

Wählen Sie unbedingt den richtigen Servicelevel und die richtige Volumekontingentgröße für die erwartete Leistungsstufe aus. Unter [Dienstebenen für Azure NetApp Files](azure-netapp-files-service-levels.md) finden Sie weitere Informationen.

### <a name="virtual-network-vnet-recommendations"></a>Empfehlungen für virtuelle Netzwerke (VNET)

Sie sollten die Benchmarktests im gleichen VNET ausführen, in dem Sie Azure NetApp Files einsetzen. Im folgenden Beispiel wird die Empfehlung veranschaulicht:

![VNET-Empfehlungen](../media/azure-netapp-files/azure-netapp-files-benchmark-testing-vnet.png)

## <a name="performance-benchmarking-tools"></a>Tools für das Leistungsbenchmarking

Dieser Abschnitt enthält Einzelheiten über einige Benchmarking-Tools. 

### <a name="ssb"></a>SSB

SQL Storage Benchmark (SSB) ist ein in Python geschriebenes Open-Source-Benchmark-Tool. Es wurde entwickelt, um eine "reale" Arbeitslast zu erzeugen, die die Datenbankinteraktion so emuliert, dass die Leistung des Speichersubsystems gemessen werden kann. 

Der Absicht von SSB ist es, Organisationen und Einzelpersonen die Möglichkeit zu geben, die Leistung ihres Speichersubsystems unter der Belastung einer SQL-Datenbankarbeitslast zu messen.

#### <a name="installation-of-ssb"></a>Einbau von SSB 

Folgen Sie dem Abschnitt [Erste Schritte](https://github.com/NetApp/SQL_Storage_Benchmark/blob/main/README.md#getting-started) in der SSB README-Datei, um die Installation für die Plattform Ihrer Wahl durchzuführen.

### <a name="fio"></a>FIO 

Flexible I/O Tester (FIO) ist ein freies und quelloffenes Festplatten-E/A-Tool, das sowohl für Benchmarks als auch für Stress-/Hardwareüberprüfungen verwendet wird. 

FIO ist im Binärformat für Linux und Windows verfügbar. 

#### <a name="installation-of-fio"></a>Installation von FIO

Folgen Sie dem Abschnitt Binärpakete in der README-Datei [FIO](https://github.com/axboe/fio#readme) zur Installation für die Plattform Ihrer Wahl.

#### <a name="fio-examples-for-iops"></a>FIO-Beispiele für IOPS 

Für die FIO-Beispiele in diesem Abschnitt wird folgendes Setup verwendet:
* VM-Instanzgröße: D32s_v3
* Servicelevel und Größe des Kapazitätspools: Premium/50TiB
* Größe des Volumekontingents: 48TiB

Die folgenden Beispiele zeigen die zufälligen Lese- und Schreibvorgänge in FIO.

##### <a name="fio-8k-block-size-100-random-reads"></a>FIO: 8KB Blockgröße 100% zufällige Lesevorgänge

`fio --name=8krandomreads --rw=randread --direct=1 --ioengine=libaio --bs=8k --numjobs=4 --iodepth=128 --size=4G --runtime=600 --group_reporting`

##### <a name="output-68k-read-iops-displayed"></a>Ausgabe: 68KB Lese-IOPS angezeigt

`Starting 4 processes`  
`Jobs: 4 (f=4): [r(4)][84.4%][r=537MiB/s,w=0KiB/s][r=68.8k,w=0 IOPS][eta 00m:05s]`

##### <a name="fio-8k-block-size-100-random-writes"></a>FIO: 8KB Blockgröße 100% zufällige Schreibvorgänge

`fio --name=8krandomwrites --rw=randwrite --direct=1 --ioengine=libaio --bs=8k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

##### <a name="output-73k-write-iops-displayed"></a>Ausgabe: 73KB Schreib-IOPS angezeigt

`Starting 4 processes`  
`Jobs: 4 (f=4): [w(4)][26.7%][r=0KiB/s,w=571MiB/s][r=0,w=73.0k IOPS][eta 00m:22s]`

#### <a name="fio-examples-for-bandwidth"></a>FIO-Beispiele für Bandbreite

Die Beispiele in diesem Abschnitt zeigen die sequenziellen Lese- und Schreibvorgänge in FIO.

##### <a name="fio-64k-block-size-100-sequential-reads"></a>FIO: 64KB Blockgröße 100% sequenzielle Lesevorgänge

`fio --name=64kseqreads --rw=read --direct=1 --ioengine=libaio --bs=64k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

##### <a name="output-118-gbits-throughput-displayed"></a>Ausgabe: 11,8Gbit/s Durchsatz angezeigt

`Starting 4 processes`  
`Jobs: 4 (f=4): [R(4)][40.0%][r=1313MiB/s,w=0KiB/s][r=21.0k,w=0 IOPS][eta 00m:09s]`

##### <a name="fio-64k-block-size-100-sequential-writes"></a>FIO: 64KB Blockgröße 100% sequenzielle Schreibvorgänge

`fio --name=64kseqwrites --rw=write --direct=1 --ioengine=libaio --bs=64k --numjobs=4 --iodepth=128  --size=4G --runtime=600 --group_reporting`

##### <a name="output-122-gbits-throughput-displayed"></a>Ausgabe: 12,2Gbit/s Durchsatz angezeigt

`Starting 4 processes`  
`Jobs: 4 (f=4): [W(4)][85.7%][r=0KiB/s,w=1356MiB/s][r=0,w=21.7k IOPS][eta 00m:02s]`

## <a name="volume-metrics"></a>Volumemetriken

Azure NetApp Files-Leistungsdaten sind über Azure Monitor-Leistungsindikatoren verfügbar. Die Leistungsindikatoren stehen über das Azure-Portal und REST-API-GET-Anforderungen zur Verfügung. 

Sie können Verlaufsdaten für die folgenden Informationen anzeigen:
* Durchschnittliche Wartezeit beim Lesevorgang 
* Durchschnittliche Wartezeit beim Schreibvorgang 
* Lese-IOPS (Durchschnitt)
* Schreib-IOPS (Durchschnitt)
* Logische Größe des Volumes (Durchschnitt)
* Momentaufnahmengröße des Volumes (Durchschnitt)

### <a name="using-azure-monitor"></a>Verwenden von Azure Monitor 

Sie können auf der Seite „Metriken“ wie unten dargestellt auf Volumebasis auf die Azure NetApp Files-Leistungsindikatoren zugreifen:

![Azure Monitor-Metriken](../media/azure-netapp-files/azure-netapp-files-benchmark-monitor-metrics.png)

Sie können auch in Azure Monitor ein Dashboard für Azure NetApp Files erstellen, indem Sie auf der Seite „Metriken“ nach NetApp filtern und die gewünschten Volumeleistungsindikatoren angeben: 

![Azure Monitor-Dashboard](../media/azure-netapp-files/azure-netapp-files-benchmark-monitor-dashboard.png)

### <a name="azure-monitor-api-access"></a>API-Zugriff in Azure Monitor

Sie können mit REST-API-Aufrufen auf die Leistungsindikatoren für Azure NetApp Files zugreifen. Unter [Unterstützte Metriken von Azure Monitor: Microsoft.NetApp/netAppAccounts/capacityPools/Volumes](../azure-monitor/essentials/metrics-supported.md#microsoftnetappnetappaccountscapacitypoolsvolumes) finden Sie Leistungsindikatoren für Kapazitätspools und Volumes.

Das folgende Beispiel zeigt eine GET-URL zur Anzeige der Größe logischer Volumes:

`#get ANF volume usage`  
`curl -X GET -H "Authorization: Bearer TOKENGOESHERE" -H "Content-Type: application/json" https://management.azure.com/subscriptions/SUBIDGOESHERE/resourceGroups/RESOURCEGROUPGOESHERE/providers/Microsoft.NetApp/netAppAccounts/ANFACCOUNTGOESHERE/capacityPools/ANFPOOLGOESHERE/Volumes/ANFVOLUMEGOESHERE/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=VolumeLogicalSize`


## <a name="next-steps"></a>Nächste Schritte

- [Dienstebenen für Azure NetApp Files](azure-netapp-files-service-levels.md)
- [Leistungsbenchmarks für Linux](performance-benchmarks-linux.md)
