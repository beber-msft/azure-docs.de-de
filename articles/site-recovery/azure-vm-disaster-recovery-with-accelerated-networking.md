---
title: Aktivieren des beschleunigten Netzwerkbetriebs für die Notfallwiederherstellung für virtuelle Azure-Computer mit Azure Site Recovery
description: Dieser Artikel beschreibt, wie Sie den beschleunigten Netzwerkbetrieb mit Azure Site Recovery für die Notfallwiederherstellung von virtuellen Azure-Computern verwenden.
services: site-recovery
documentationcenter: ''
author: rishjai-msft
manager: gaggupta
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: rishjai
ms.openlocfilehash: d3495625da0b039a5e75bf3973600b16f802b263
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131463482"
---
# <a name="accelerated-networking-with-azure-virtual-machine-disaster-recovery"></a>Beschleunigter Netzwerkbetrieb mit Notfallwiederherstellung für virtuelle Azure-Computer

Der beschleunigte Netzwerkbetrieb ermöglicht die E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) auf einem virtuellen Computer und somit eine erhebliche Steigerung der Netzwerkleistung. Über diesen für Hochleistung konzipierten Pfad wird der Host des Datenpfads umgangen, um Latenzen, Jitter und CPU-Auslastung zu verringern. So können mit unterstützten VM-Typen die anspruchsvollsten Netzwerkworkloads genutzt werden. Die folgende Abbildung zeigt die Kommunikation zwischen zwei VMs mit und ohne beschleunigten Netzwerkbetrieb:

![Vergleich](./media/azure-vm-disaster-recovery-with-accelerated-networking/accelerated-networking-benefit.png)

Mit Azure Site Recovery können Sie bei virtuellen Azure-Computern, für die ein Failover in eine andere Azure-Region durchgeführt wurde, von den Vorteilen des beschleunigten Netzwerkbetriebs profitieren. Dieser Artikel beschreibt, wie Sie den beschleunigten Netzwerkbetrieb für virtuelle Azure-Computer aktivieren können, die mit Azure Site Recovery repliziert wurden.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass Sie mit folgenden Konzepten vertraut sind:
-   [Architektur der Replikation](azure-to-azure-architecture.md) von virtuellen Azure-Computern
-   [Einrichten der Replikation](azure-to-azure-tutorial-enable-replication.md) für virtuelle Azure-Computer
-   [Ausführen des Failovers](azure-to-azure-tutorial-failover-failback.md) von virtuellen Azure-Computern

## <a name="accelerated-networking-with-windows-vms"></a>Beschleunigter Netzwerkbetrieb mit virtuellen Windows-Computern

Azure Site Recovery unterstützt den beschleunigten Netzwerkbetrieb für replizierte virtuelle Computer nur dann, wenn dieser auch auf dem virtuellen Quellcomputer aktiviert ist. Wenn der beschleunigte Netzwerkbetrieb auf dem virtuellen Quellcomputer nicht aktiviert ist, erfahren Sie [hier](../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms), wie Sie diesen für virtuelle Windows-Computer aktivieren.

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
Die folgenden Distributionen werden standardmäßig aus dem Azure-Katalog unterstützt:
* **Windows Server 2016 Datacenter**
* **Windows Server 2012 R2 Datacenter**

### <a name="supported-vm-instances"></a>Unterstützte VM-Instanzen
Der beschleunigte Netzwerkbetrieb wird in den meisten universellen, computeoptimierten Instanzgrößen mit mindestens 2 vCPUs unterstützt.  Folgende Reihen werden unterstützt: D/DSv2 und F/Fs

Bei Instanzen, die Hyperthreading unterstützen, wird der beschleunigte Netzwerkbetrieb auf VM-Instanzen mit mindestens 4 vCPUs unterstützt. Folgende Reihen werden unterstützt: D/DSv3, E/ESv3, Fsv2 und Ms/Mms

Weitere Informationen zu VM-Instanzen finden Sie unter [Größen für virtuelle Windows-Computer](../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="accelerated-networking-with-linux-vms"></a>Beschleunigter Netzwerkbetrieb mit virtuellen Linux-Computern

Azure Site Recovery unterstützt den beschleunigten Netzwerkbetrieb für replizierte virtuelle Computer nur dann, wenn dieser auch auf dem virtuellen Quellcomputer aktiviert ist. Wenn der beschleunigte Netzwerkbetrieb auf dem virtuellen Quellcomputer nicht aktiviert ist, erfahren Sie [hier](../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms), wie Sie diesen für virtuelle Linux-Computer aktivieren.

### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
Die folgenden Distributionen werden standardmäßig aus dem Azure-Katalog unterstützt:
* **Ubuntu 16.04**
* **SLES 12 SP3**
* **RHEL 7.4**
* **CentOS 7.4**
* **CoreOS Linux**
* **Debian „Stretch“ mit Backports-Kernel**
* **Oracle Linux 7.4**

### <a name="supported-vm-instances"></a>Unterstützte VM-Instanzen
Der beschleunigte Netzwerkbetrieb wird in den meisten universellen, computeoptimierten Instanzgrößen mit mindestens 2 vCPUs unterstützt.  Folgende Reihen werden unterstützt: D/DSv2 und F/Fs

Bei Instanzen, die Hyperthreading unterstützen, wird der beschleunigte Netzwerkbetrieb auf VM-Instanzen mit mindestens 4 vCPUs unterstützt. Folgende Reihen werden unterstützt: D/DSv3, E/ESv3, Fsv2 und Ms/Mms.

Weitere Informationen zu VM-Instanzen finden Sie unter [Größen für virtuelle Linux-Computer in Azure](../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="enabling-accelerated-networking-for-replicated-vms"></a>Aktivieren des beschleunigten Netzwerkbetriebs für replizierte VMs

Wenn Sie die [Replikation für virtuelle Azure-Computer aktivieren](azure-to-azure-tutorial-enable-replication.md), erkennt Site Recovery automatisch, ob der beschleunigte Netzwerkbetrieb für die Netzwerkschnittstellen der virtuellen Computer aktiviert ist. Wenn der beschleunigte Netzwerkbetrieb bereits aktiviert ist, konfiguriert Site Recovery diesen automatisch in den Netzwerkschnittstellen des replizierten virtuellen Computers.

Der Status des beschleunigten Netzwerkbetriebs kann auf der entsprechenden NIC-Registerkarte in den **Netzwerk**-Einstellungen des replizierten virtuellen Computers überprüft werden.

![Einstellungen des beschleunigten Netzwerkbetriebs](./media/azure-vm-disaster-recovery-with-accelerated-networking/compute-network-accelerated-networking.png)

Wenn Sie den beschleunigten Netzwerkbetrieb erst nach dem Aktivieren der Replikation auf dem virtuellen Quellcomputer aktiviert haben, können Sie den Betrieb für die Netzwerkschnittstellen des virtuellen Computers mithilfe des folgenden Prozesses aktivieren:
1. Öffnen Sie die **Netzwerk**-Einstellungen des replizierten virtuellen Computers.
2. Klicken Sie im Abschnitt **Netzwerkschnittstellen** auf den Namen der Netzwerkschnittstelle.
3. Wählen Sie aus der Dropdownliste für den beschleunigten Netzwerkbetrieb in der Spalte **Ziel** die Option **Aktiviert** aus.

![Aktivieren des beschleunigten Netzwerkbetriebs](./media/azure-vm-disaster-recovery-with-accelerated-networking/network-interface-accelerated-networking-enabled.png)

Der oben beschriebene Prozess gilt auch für vorhandene virtuelle Computer, für die der beschleunigte Netzwerkbetrieb nicht automatisch von Site Recovery aktiviert wurde.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über die [Vorteile des beschleunigten Netzwerkbetriebs](../virtual-network/create-vm-accelerated-networking-powershell.md#benefits).
- Erfahren Sie mehr über die Einschränkungen und Grenzwerte des beschleunigten Netzwerkbetriebs für [virtuelle Windows-Computer](../virtual-network/create-vm-accelerated-networking-powershell.md#limitations-and-constraints) und [virtuelle Linux-Computer](../virtual-network/create-vm-accelerated-networking-cli.md#limitations-and-constraints).
- Informieren Sie sich ausführlicher über [Wiederherstellungspläne](site-recovery-create-recovery-plans.md) zum Automatisieren des Anwendungsfailovers.