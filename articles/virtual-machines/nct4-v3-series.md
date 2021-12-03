---
title: NCas T4 v3-Serie
description: Hier finden Sie Spezifikationen für die VMs der NCas T4 v3-Serie.
ms.service: virtual-machines
ms.subservice: vm-sizes-gpu
author: vikancha-MSFT
ms.topic: conceptual
ms.date: 01/12/2021
ms.author: vikancha
ms.openlocfilehash: 8a93d84fea79935f96498eff7d391271e6f4c7ef
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131013097"
---
# <a name="ncast4_v3-series"></a>NCasT4_v3-Serie 

**Gilt für**: :heavy_check_mark: Linux-VMs :heavy_check_mark: Windows-VMs :heavy_check_mark: Flexible Skalierungsgruppen :heavy_check_mark: Einheitliche Skalierungsgruppen

Die virtuellen Computer der NCasT4_v3-Serie basieren auf GPUs vom Typ [Nvidia Tesla T4](https://www.nvidia.com/en-us/data-center/tesla-t4/) sowie auf CPUs vom Typ AMD EPYC 7V12(Rome). Die VMs verfügen über bis zu 4 NVIDIA T4-GPUs mit jeweils 16 GB Arbeitsspeicher, bis zu 64 AMD EPYC 7V12(Rome)-Prozessorkerne ohne Multithreading (Basisfrequenz von 2,45 GHz, Spitzenfrequenz aller Kerne von 3,1 GHz und Spitzenfrequenz von 3,3 GHz für einzelne Kerne) und 440 GiB Systemspeicher. Diese virtuellen Computer eignen sich ideal für das Bereitstellen von KI-Diensten wie Rückschlüsse in Echtzeit im Zusammenhang mit vom Benutzer generierten Anforderungen oder für interaktive Grafiken und Visualisierungsworkloads mit dem GRID-Treiber und der vGPU-Technologie von NVIDIA. GPU-Standardcomputeworkloads, die auf CUDA, TensorRT, Caffe, ONNX und anderen Frameworks basieren, oder auf OpenGL und DirectX basierende GPU-beschleunigte grafische Anwendungen können in der NCasT4_v3-Reihe wirtschaftlich und in unmittelbarer Nähe zu den Benutzern bereitgestellt werden.

<br>

[ACU:](acu.md) 230 – 260<br>
[Storage Premium:](premium-storage-performance.md) Unterstützt<br>
[Storage Premium-Zwischenspeicherung:](premium-storage-performance.md) Unterstützt<br>
[Ultra Disks](disks-types.md#ultra-disks): Unterstützt ([Weitere Informationen](https://techcommunity.microsoft.com/t5/azure-compute/ultra-disk-storage-for-hpc-and-gpu-vms/ba-p/2189312) zur Verfügbarkeit, Verwendung und Leistung) <br>
[Livemigration:](maintenance-and-updates.md) Nicht unterstützt<br>
[Updates mit Speicherbeibehaltung:](maintenance-and-updates.md) Nicht unterstützt<br>
[Unterstützung von VM-Generationen:](generation-2.md) Generation 1 und 2<br>
[Beschleunigter Netzwerkbetrieb](../virtual-network/create-vm-accelerated-networking-cli.md): Unterstützt<br>
[Kurzlebige Betriebssystemdatenträger](ephemeral-os-disks.md): Unterstützt<br>
Nvidia NVLink Interconnect: Nicht unterstützt<br>
<br>

| Size | vCPU | Memory: GiB | Temporärer Speicher (SSD): GiB | GPU | GPU-Arbeitsspeicher: GiB | Max. Anzahl Datenträger | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (MBps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC4as_T4_v3 |4 |28 |180 | 1 | 16 | 8 | 2/8.000 |
| Standard_NC8as_T4_v3 |8 |56 |360 | 1 | 16 | 16 | 4/8000  |
| Standard_NC16as_T4_v3 |16 |110 |360 | 1 | 16 | 32 | 8 / 8000  |
| Standard_NC64as_T4_v3 |64 |440 |2880 | 4 | 64 | 32 | 8 / 32.000  |




## <a name="supported-operating-systems-and-drivers"></a>Unterstützte Betriebssysteme und Treiber

Um die GPU-Funktionen von virtuellen Azure-Computern der NCasT4_v3-Serie unter Windows oder Linux nutzen zu können, müssen Nvidia-GPU-Treiber installiert werden.

Wenn Sie Nvidia-GPU-Treiber manuell installieren möchten, finden Sie Informationen zu unterstützten Betriebssystemen, Treibern und Installation sowie Schritte zur Überprüfung unter [Einrichten von GPU-Treibern der N-Serie für Windows](./windows/n-series-driver-setup.md).

Die Azure-Nvidia-GPU-Treibererweiterung stellt CUDA-Treiber auf den VMs der NCasT4_v3-Serie bereit. Für Grafik- und Visualisierungsworkloads müssen Sie die von Azure unterstützten GRID-Treiber manuell installieren.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Weitere Größen und Informationen

- [Allgemeiner Zweck](sizes-general.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU-optimiert](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

Preisrechner: [Preisrechner](https://azure.microsoft.com/pricing/calculator/)

Weitere Informationen zu Datenträgertypen finden Sie unter [Welche Datenträgertypen stehen in Azure zur Verfügung?](disks-types.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
