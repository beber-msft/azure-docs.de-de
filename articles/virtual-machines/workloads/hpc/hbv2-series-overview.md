---
title: Übersicht über virtuelle Computer der HBv2-Serie – Azure Virtual Machines | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Größe virtueller Computer der HBv2-Serie in Azure.
services: virtual-machines
author: vermagit
tags: azure-resource-manager
ms.service: virtual-machines
ms.subservice: hpc
ms.workload: infrastructure-services
ms.topic: article
ms.date: 09/28/2020
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: 436a4ab3de748b19bd2cad5ae171a754bf9dc955
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132311999"
---
# <a name="hbv2-series-virtual-machine-overview"></a>Übersicht über virtuelle Computer der HBv2-Serie 

**Gilt für**: :heavy_check_mark: Linux-VMs :heavy_check_mark: Windows-VMs :heavy_check_mark: Flexible Skalierungsgruppen :heavy_check_mark: Einheitliche Skalierungsgruppen

Die Maximierung der Leistung von HPC-Anwendungen (High Performance Computing) für AMD EPYC erfordert ein durchdachtes Konzept für die Lokalität des Arbeitsspeichers und die Prozessplatzierung. Im Anschluss finden Sie einen Überblick über die AMD EPYC-Architektur sowie über unsere Implementierung in Azure für HPC-Anwendungen. Der Begriff **pNUMA** bezieht sich hier auf eine physische NUMA-Domäne; **vNUMA** steht für eine virtualisierte NUMA-Domäne. 

Physisch handelt es sich bei einem Server der [HBv2-Serie](../../hbv2-series.md) um zwei EPYC 7742-CPUs mit jeweils 64 Kernen, wodurch sich eine Gesamtanzahl von 128 physischen Kernen ergibt. Diese 128 Kerne sind in 32 pNUMA-Domänen (16 pro Socket) unterteilt, die jeweils vier Kerne umfassen und von AMD als **Core Complex** (oder **CCX**) bezeichnet werden. Jeder CCX verfügt über einen eigenen L3-Cache, der dem Betriebssystem eine pNUMA-/vNUMA-Grenze signalisiert. Vier benachbarte CCX teilen sich den Zugriff auf zwei Kanäle des physischen DRAM. 

Damit der Azure-Hypervisor über genügend Platz verfügt, um ohne Beeinträchtigung des virtuellen Computers agieren zu können, werden die physischen pNUMA-Domänen 0 und 16 (d. h. der erste CCX jedes CPU-Sockets) reserviert. Alle verbleibenden 30 pNUMA-Domänen werden der VM zugewiesen und werden dadurch zu vNUMA. Für den virtuellen Computer steht somit Folgendes zur Verfügung:

`(30 vNUMA domains) * (4 cores/vNUMA) = 120` Kerne pro virtuellem Computer 

Der virtuelle Computer selbst hat keine Kenntnis davon, dass pNUMA 0 und 16 reserviert sind. Er zählt die vNUMA, die er als 0-29 betrachtet, symmetrisch mit 15 vNUMA pro Socket auf, d. h. vNUMA 0-14 an vSocket 0 und vNUMA 15-29 an vSocket 1. Im nächsten Abschnitt finden Sie Anweisungen, wie MPI-Anwendungen bei diesem asymmetrischen NUMA-Layout am besten ausgeführt werden können. 

Eine feste Prozesszuordnung (Process Pinning) funktioniert bei virtuellen Computern der HBv2-Serie, da der zugrunde liegende Chip unverändert für den virtuellen Gastcomputer verfügbar gemacht wird. Aus Leistungs- und Konsistenzgründen wird dringend empfohlen, Prozesse fest zuzuordnen. 


## <a name="hardware-specifications"></a>Hardwarespezifikationen 

| Hardwarespezifikationen          | VM der HBv2-Serie                   | 
|----------------------------------|----------------------------------|
| Kerne                            | 120 (SMT deaktiviert)               | 
| CPU                              | AMD EPYC 7742                    | 
| CPU-Frequenz (ohne AVX)          | ~3.1 GHz (einzeln + alle Kerne)    | 
| Arbeitsspeicher                           | 4 GB/Kern (480 GB insgesamt)         | 
| Lokaler Datenträger                       | 960 GB NVMe (Block), 480 GB SSD (Auslagerungsdatei) | 
| InfiniBand                       | 200  GBit/s EDR Mellanox ConnectX-6 | 
| Netzwerk                          | 50 GBit/s Ethernet (davon 40 GBit/s nutzbar); Azure-SmartNIC der zweiten Generation | 


## <a name="software-specifications"></a>Softwarespezifikationen 

| Softwarespezifikationen     | VM der HBv2-Serie                                            | 
|-----------------------------|-----------------------------------------------------------|
| Maximale MPI-Auftragsgröße            | 36000 Kerne (300 VMs in einer einzelnen VM-Skalierungsgruppe mit singlePlacementGroup=true) |
| MPI-Unterstützung                 | HPC-X, Intel MPI, OpenMPI, MVAPICH2, MPICH, Platform MPI  |
| Zusätzliche Frameworks       | UCX, libfabric und PGAS |
| Azure Storage-Unterstützung       | Standard- und Premium-Datenträger (maximal 8 Datenträger) |
| Betriebssystemunterstützung für SR-IOV/RDMA   | CentOS/RHEL 7.6 und höher, Ubuntu 16.04 und höher, SLES 12 SP4 und höher, Windows Server 2016 und höher  |
| Orchestratorunterstützung        | CycleCloud, Batch und AKS ([Clusterkonfigurationsoptionen](../../sizes-hpc.md#cluster-configuration-options))  |

> [!NOTE] 
> Windows Server 2012 R2 wird auf HBv2 und anderen VMs mit mehr als 64 (virtuellen oder physischen) Kernen nicht unterstützt. Weitere Informationen finden Sie unter [Unterstützte Windows-Gastbetriebssysteme für Hyper-V in Windows Server](/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [Architektur von AMD EPYC](https://bit.ly/2Epv3kC) und die [Architekturen mit mehreren Chips](https://bit.ly/2GpQIMb). Ausführlichere Informationen finden Sie im [HPC-Optimierungsleitfaden für AMD EPYC-Prozessoren](https://bit.ly/2T3AWZ9).
- Informieren Sie sich über die neuesten Ankündigungen, HPC-Workloadbeispiele und Leistungsergebnisse in den [Tech Community-Blogs zu Azure Compute](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute).
- Eine allgemeinere Übersicht über die Architektur für die Ausführung von HPC-Workloads finden Sie unter [High Performance Computing (HPC) in Azure](/azure/architecture/topics/high-performance-computing/).
