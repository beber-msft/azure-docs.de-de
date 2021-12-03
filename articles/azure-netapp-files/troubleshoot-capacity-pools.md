---
title: Problembehandlung bei Kapazitätspoolfehlern in Azure NetApp Files | Microsoft-Dokumentation
description: Hier werden mögliche Probleme bei der Verwaltung von Kapazitätspools beschrieben und Lösungen für diese Probleme angeboten.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/14/2021
ms.author: b-juche
ms.openlocfilehash: e637c80f864592f89252e52f27325762cd43582b
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2021
ms.locfileid: "130219771"
---
# <a name="troubleshoot-capacity-pool-errors"></a>Behandeln von Kapazitätspoolfehlern

In diesem Artikel werden Lösungen für möglicherweise beim Verwalten von Kapazitätspools auftretende Probleme einschließlich des Pooländerungsvorgangs beschrieben. 

## <a name="issues-managing-a-capacity-pool"></a>Probleme beim Verwalten eines Kapazitätspools 

|     Fehlerzustand    |     Lösung    |
|-|-|
| Probleme beim Erstellen eines Kapazitätspools |  Stellen Sie sicher, dass die Anzahl der Kapazitätspools den Grenzwert nicht überschreitet. Weitere Informationen finden Sie unter [Ressourcenlimits für Azure NetApp Files](azure-netapp-files-resource-limits.md).  Wenn die Anzahl kleiner als das Limit ist und weiterhin Probleme auftreten, können Sie ein Supportticket einreichen und den Namen des Kapazitätspools angeben. |
| Probleme beim Löschen eines Kapazitätspools  |  Stellen Sie sicher, dass Sie alle Volumes und Momentaufnahmen von Azure NetApp Files aus dem Abonnement entfernen, in dem Sie den Kapazitätspool löschen möchten. <br> Wenn Sie bereits alle Volumes und Momentaufnahmen entfernt haben und den Kapazitätspool immer noch nicht löschen können, sind möglicherweise weiterhin Verweise auf Ressourcen vorhanden, ohne im Portal angezeigt zu werden. Reichen Sie in diesem Fall ein Supportticket ein, und geben Sie an, dass Sie die oben beschriebenen Schritte ausgeführt haben. |
| Fehler `Requested throughput not available` bei Volumeerstellung oder -änderung | Der verfügbare Durchsatz für ein Volume wird durch die Größe des Kapazitätspools und den Servicelevel festgelegt. Wenn Sie nicht über genügend Durchsatz verfügen, sollten Sie die Poolgröße heraufsetzen oder den vorhandenen Volumedurchsatz anpassen. | 

## <a name="issues-when-changing-the-capacity-pool-of-a-volume"></a>Probleme beim Ändern des Kapazitätspools eines Volumes 

|     Fehlerzustand    |     Lösung    |
|-|-|
| Das Ändern des Kapazitätspools für ein Volume ist nicht zulässig. | Sie sind möglicherweise noch nicht autorisiert, dieses Feature zu verwenden. <br> Die Funktion zum Verschieben eines Volumes in einen anderen Kapazitätspool befindet sich derzeit in der Vorschauphase. Wenn Sie dieses Feature zum ersten Mal verwenden, müssen Sie es zuerst registrieren und `-FeatureName ANFTierChange` festlegen. Die Registrierungsschritte finden Sie unter [Dynamisches Ändern der Dienstebene eines Volumes](dynamic-change-volume-service-level.md). |
| Die Kapazitätspoolgröße ist für die Gesamtvolumegröße zu gering. |  Der Fehler ist darauf zurückzuführen, dass der Zielkapazitätspool nicht über die verfügbare Kapazität für das Volume verfügt, das verschoben wird.  <br> Setzen Sie die Größe des Zielpools herauf, oder wählen Sie einen anderen Pool aus, der größer ist.  Weitere Informationen finden Sie unter [Ändern der Größe eines Kapazitätspools oder Volumes](azure-netapp-files-resize-capacity-pools-or-volumes.md).   |
|  Die Pooländerung kann nicht abgeschlossen werden, da ein Volume mit dem Namen `'{source pool name}'` bereits im Zielpool `'{target pool name}'` vorhanden ist. | Dieser Fehler tritt auf, weil das Volume mit demselben Namen bereits im Zielkapazitätspool vorhanden ist.  Wählen Sie einen anderen Kapazitätspool aus, der nicht über ein Volume mit demselben Namen verfügt.   | 

## <a name="next-steps"></a>Nächste Schritte  

* [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)
* [Verwalten eines manuellen QoS-Kapazitätspools](manage-manual-qos-capacity-pool.md)
* [Dynamisches Ändern der Dienstebene eines Volumes](dynamic-change-volume-service-level.md)
* [Ändern der Größe eines Kapazitätspools oder Volumes](azure-netapp-files-resize-capacity-pools-or-volumes.md)
