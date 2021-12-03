---
title: Fsv2-Serie
description: Hier finden Sie Spezifikationen für die virtuellen Computer der Fsv2-Serie.
author: brbell
ms.service: virtual-machines
ms.subservice: vm-sizes-compute
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 4c652a9292caf8e7ac198ac6922b11b78a85781e
ms.sourcegitcommit: e1037fa0082931f3f0039b9a2761861b632e986d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2021
ms.locfileid: "132401419"
---
# <a name="fsv2-series"></a>Fsv2-Serie

**Gilt für**: :heavy_check_mark: Linux-VMs :heavy_check_mark: Windows-VMs :heavy_check_mark: Flexible Skalierungsgruppen :heavy_check_mark: Einheitliche Skalierungsgruppen

Die Fsv2-Serie wird auf dem Intel® Xeon® Platinum 8272CL-Prozessor (Cascade Lake) und auf dem Intel® Xeon® Platinum 8168-Prozessor (Skylake) ausgeführt. Dieser verfügt über eine kontinuierliche Turbo-Taktfrequenz von 3,4 GHz für alle Kerne und eine maximale Turbofrequenz von 3,7 GHz für Einzelkerne. Intel® AVX-512-Anweisungen sind neu auf von Intel skalierbaren Prozessoren. Diese Anweisungen können bei Gleitkommaoperationen mit einfacher und doppelter Genauigkeit mit Vektorverarbeitungsworkloads die Leistung verdoppeln. Sie bewältigen demnach alle Rechenworkloads wirklich schnell.

Virtuelle Computer der Fsv2-Serie verfügen über Hyper-Threading-Technologie von Intel®.

[ACU:](acu.md) 195–210<br>
[Storage Premium:](premium-storage-performance.md) Unterstützt<br>
[Storage Premium-Zwischenspeicherung:](premium-storage-performance.md) Unterstützt<br>
[Livemigration](maintenance-and-updates.md): Unterstützt<br>
[Updates mit Speicherbeibehaltung](maintenance-and-updates.md): Unterstützt<br>
[Unterstützung von VM-Generationen:](generation-2.md) Generation 1 und 2<br>
[Beschleunigter Netzwerkbetrieb](../virtual-network/create-vm-accelerated-networking-cli.md): Unterstützt <br>
[Kurzlebige Betriebssystemdatenträger](ephemeral-os-disks.md): Unterstützt <br>
[Geschachtelte Virtualisierung](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization): Unterstützt <br>
<br>

| Size | vCPUs | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBit/s (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBit/s |  Durchsatz des Datenträgers mit maximalem Burst ohne Cache: IOPS/MBit/s<sup>1</sup> |Maximale Anzahl NICs|Erwartete Netzwerkbandbreite (MBit/s) |
|---|---|---|---|---|---|---|---|---|---|
| Standard_F2s_v2<sup>4</sup>  | 2  | 4   | 16  | 4  | 4000/31 (32)       | 3200/47    | 4000/200 | 2| 5.000   |
| Standard_F4s_v2  | 4  | 8   | 32  | 8  | 8000/63 (64)       | 6400/95    | 8000/200 | 2|10000  |
| Standard_F8s_v2  | 8  | 16  | 64  | 16 | 16000/127 (128)    | 12800/190  | 16000/400 | 4|12500  |
| Standard_F16s_v2 | 16 | 32  | 128 | 32 | 32000/255 (256)    | 25600/380  | 32000/800 | 4|12500  |
| Standard_F32s_v2 | 32 | 64  | 256 | 32 | 64000/512 (512)    | 51200/750  | 64000/1600 | 8|16000 |
| Standard_F48s_v2 | 48 | 96  | 384 | 32 | 96000/768 (768)    | 76800/1100 | 80.000/2.000 | 8|21000 |
| Standard_F64s_v2 | 64 | 128 | 512 | 32 | 128000/1024 (1024) | 80000/1100 | 80.000/2.000 | 8|28000 |
| Standard_F72s_v2<sup>2, 3</sup> | 72 | 144 | 576 | 32 | 144000/1152 (1520) | 80000/1100 | 80.000/2.000 | 8|30.000 |

<sup>1</sup> VMs der Fsv2-Serie können mit einem [Burst](./disk-bursting.md) ihre Datenträgerleistung für jeweils bis zu 30 Minuten auf das maximale Bursting verbessern.

<sup>2</sup> Bei der Verwendung von mehr als 64 vCPUs ist eines dieser unterstützten Gastbetriebssysteme erforderlich:

- Windows Server 2016 oder höher
- Ubuntu 16.04 LTS oder höher mit für Azure optimiertem Kernel (4.15 Kernel oder höher)
- SLES 12 SP2 oder höher
- RHEL oder CentOS-Version 6.7 bis 6.10 mit Installation des von Microsoft bereitgestellten LIS-Pakets 4.3.1 (oder höher)
- RHEL oder CentOS-Version 7.3 mit Installation des von Microsoft bereitgestellten LIS-Pakets 4.2.1 (oder höher)
- RHEL oder CentOS-Version 7.6 oder höher
- Oracle Linux mit UEK4 oder höher
- Debian 9 mit Backports-Kernel, Debian 10 oder höher
- CoreOS mit 4.14-Kernel oder höher

<sup>3</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.<br>
<sup>4</sup> Der beschleunigte Netzwerkbetrieb kann nur auf eine einzelne NIC angewendet werden.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Weitere Größen und Informationen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

Preisrechner: [Preisrechner](https://azure.microsoft.com/pricing/calculator/)

Weitere Informationen zu Datenträgertypen: [Datenträgertypen](./disks-types.md#ultra-disks)


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
