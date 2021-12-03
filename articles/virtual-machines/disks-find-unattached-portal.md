---
title: Identifizieren nicht angefügter Azure-Datenträger – Azure-Portal
description: Informationen zum Ermitteln von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern (VHDs/Seitenblobs) im Azure-Portal.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 06/29/2021
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 0855516610c9712d5f8db597d1de6e53f01ac546
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132301303"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks---azure-portal"></a>Suchen und Löschen von nicht angefügten verwalteten und nicht verwalteten Azure-Datenträgern – Azure-Portal

**Gilt für:** :heavy_check_mark: Linux-VMs :heavy_check_mark: Windows-VMs :heavy_check_mark: Flexible Skalierungsgruppen :heavy_check_mark: Einheitliche Skalierungsgruppen

Beim Löschen eines virtuellen Computers (VM) in Azure werden standardmäßig Datenträger, die an den virtuellen Computer angefügt sind, nicht gelöscht. Dadurch werden Datenverluste durch versehentliche Löschung von virtuellen Computern verhindert. Nach dem Löschen eines virtuellen Computers bezahlen Sie nicht angefügte Datenträger weiterhin. In diesem Artikel erfahren Sie, wie Sie über das Azure-Portal nicht angefügte Datenträger suchen und löschen, um unnötige Kosten zu verringern. Löschvorgänge sind endgültig. Sie können die Daten nicht wiederherstellen, nachdem Sie einen Datenträger gelöscht haben.

## <a name="managed-disks-find-and-delete-unattached-disks"></a>Verwaltete Datenträger: Suchen und Löschen nicht angefügter Datenträger

Falls Sie über nicht angefügte verwaltete Datenträger verfügen und die Daten auf diesen nicht mehr benötigen, wird im folgenden Prozess erläutert, wie Sie diese im Azure-Portal finden:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Suchen Sie nach **Datenträger**, und wählen Sie diese Option aus.

    Auf dem Blatt **Datenträger** wird eine Liste aller Ihrer Datenträger angezeigt.

1. Wählen Sie den Datenträger aus, den Sie löschen möchten. Daraufhin wird der Blade des Datenträgers geöffnet.
1. Auf dem Blade des Datenträgers können Sie bestätigen, dass der Datenträger nicht angefügt ist, und dann **Löschen** auswählen.

    :::image type="content" source="media/disks-find-unattached-portal/delete-managed-disk-unattached.png" alt-text="Screenshot: Blatt mit einem verwalteten Datenträger. Auf diesem Blatt wird „Nicht angefügt“ als Datenträgerstatus angezeigt, wenn dieser nicht angefügt ist. Sie können diesen Datenträger löschen, wenn die Daten nicht länger aufbewahrt werden müssen.":::

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Nicht verwaltete Datenträger: Suchen und Löschen nicht angefügter Datenträger

Nicht verwaltete Datenträger sind VHD-Dateien, die als [Seitenblobs](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) in [Azure Storage-Konten](../storage/common/storage-account-overview.md) gespeichert sind.

Wenn Sie über nicht verwaltete Datenträger verfügen, die nicht an einen virtuellen Computer angefügt sind und die Daten nicht mehr benötigt werden, beschreibt der folgende Prozess, wie Sie Datenträger im Azure-Portal löschen können:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Suchen Sie nach **Datenträger (klassisch)** , und wählen Sie diese Option aus.

    Daraufhin wird eine Liste aller Ihrer nicht verwalteten Datenträger angezeigt. Jeder Datenträger mit „ **-** “ in der Spalte **Angefügt an** ist ein nicht angefügter Datenträger.

    :::image type="content" source="media/disks-find-unattached-portal/unmanaged-disk-unattached-attached-to.png" alt-text="Screenshot: Blatt für nicht verwaltete Datenträger. Die Datenträger auf diesem Blatt mit „-“ der Spalte „Angefügt an“ sind nicht angefügt.":::

1. Wählen Sie den nicht angefügten Datenträger aus, den Sie löschen möchten. Daraufhin wird das Blatt des Datenträgers angezeigt.

1. Auf dem Blatt des Datenträgers können Sie bestätigen, dass er nicht angefügt ist, da für **Angefügt an** weiterhin **-** angezeigt wird.

    :::image type="content" source="media/disks-find-unattached-portal/unmanaged-disk-unattached-select-blade.png" alt-text="Screenshot: Blatt mit einem nicht verwalteten Datenträger. Wenn er nicht angefügt ist, wird in der Spalte „Angefügt an“ der Wert „-“ angezeigt. Wenn Sie die Daten dieses Datenträgers nicht mehr benötigen, können Sie ihn löschen.":::

1. Klicken Sie auf **Löschen**.

    :::image type="content" source="media/disks-find-unattached-portal/delete-unmanaged-disk-unattached.png" alt-text="Screenshot: Blatt mit einem nicht verwalteten Datenträger mit Hervorhebung von Löschen.":::

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie eine automatisierte Methode zum Suchen und Löschen von nicht angefügten Speicherkonten benötigen, finden Sie weitere Informationen in unseren [CLI](linux/find-unattached-disks.md)- oder [PowerShell](windows/find-unattached-disks.md)-Artikeln.

Weitere Informationen finden Sie unter [Löschen von Speicherkonten](../storage/common/storage-account-create.md#delete-a-storage-account) und [Identifizieren verwaister Datenträger mit PowerShell](/archive/blogs/ukplatforms/azure-cost-optimisation-series-identify-orphaned-disks-using-powershell).
