---
title: Blobversionsverwaltung
titleSuffix: Azure Storage
description: Die Blob Storage-Versionsverwaltung verwaltet automatisch vorherige Versionen eines Objekts und identifiziert diese durch Zeitstempel. Sie können eine vorherige Version eines Blobs wiederherstellen, um Daten wiederherzustellen, wenn diese irrtümlich geändert oder gelöscht wurden.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 05/10/2021
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 6807423e59c5877b67a8faaf3aebe4fd60e671fd
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131424929"
---
# <a name="blob-versioning"></a>Blobversionsverwaltung

Sie können die Blob Storage-Versionsverwaltung aktivieren, um frühere Versionen eines Objekts automatisch zu verwalten. Wenn Blobversionsverwaltung aktiviert ist, können Sie eine frühere Version eines Blobs wiederherstellen, um Daten wiederherzustellen, wenn diese irrtümlich geändert oder gelöscht wurden.

## <a name="recommended-data-protection-configuration"></a>Empfohlene Datenschutzkonfiguration

Die Blobversionsverwaltung ist Teil einer umfassenden Datenschutzstrategie für Blobdaten. Microsoft empfiehlt die Aktivierung folgender Datenschutzfeatures, um einen optimalen Schutz Ihrer Blobdaten zu gewährleisten:

- Blobversionsverwaltung zum automatischen Verwalten früherer Versionen eines Blobs. Wenn Blobversionsverwaltung aktiviert ist, können Sie eine frühere Version eines Blobs wiederherstellen, um Daten wiederherzustellen, wenn diese irrtümlich geändert oder gelöscht wurden. Informationen zum Aktivieren der Blobversionsverwaltung finden Sie unter [Aktivieren und Verwalten der Blobversionsverwaltung (Vorschau)](versioning-enable.md).
- Vorläufiges Löschen für Container, um einen gelöschten Container wiederherzustellen. Informationen zum Aktivieren des vorläufigen Löschens für Container finden Sie unter [Aktivieren und Verwalten des vorläufigen Löschens für Container (Vorschau)](soft-delete-container-enable.md).
- Vorläufiges Löschen von Blobs, um ein gelöschtes Blob, eine gelöschte Momentaufnahme oder eine gelöschte Version wiederherzustellen. Informationen zum Aktivieren des vorläufigen Löschens von Blobs finden Sie unter [Aktivieren und Verwalten des vorläufigen Löschens von Blobs](soft-delete-blob-enable.md).

Weitere Informationen zu den Datenschutzempfehlungen von Microsoft finden Sie in der [Datenschutzübersicht](data-protection-overview.md).

## <a name="how-blob-versioning-works"></a>Funktionsweise der Blobversionsverwaltung

Eine Version erfasst den Zustand eines Blobs zu einem bestimmten Zeitpunkt. Jede Version wird durch eine Versions-ID identifiziert. Wenn die Blobversionsverwaltung für ein Speicherkonto aktiviert ist, wird von Azure Storage bei der ursprünglichen Erstellung sowie bei jeder Änderung des Blobs automatisch eine neue Version mit einer eindeutigen ID erstellt.

Eine Versions-ID kann die aktuelle Version oder eine vorherige Version identifizieren. Ein Blob kann immer nur eine aktuelle Version haben.

Wenn Sie ein neues Blob erstellen, ist nur eine einzelne Version (die aktuelle Version) vorhanden. Wenn Sie ein vorhandenes Blob ändern, wird die aktuelle Version zu einer vorherigen Version. Es wird eine neue Version erstellt, um den aktualisierten Zustand zu erfassen, und diese neue Version ist die aktuelle Version. Wenn Sie ein Blob löschen, wird die aktuelle Version des Blobs zu einer vorherigen Version, und es gibt keine aktuelle Version mehr. Alle vorherigen Versionen des Blobs bleiben erhalten.

Die folgende Abbildung zeigt, wie Versionen bei Schreibvorgängen erstellt werden und wie eine vorherige Version zur aktuellen Version heraufgestuft werden kann:

:::image type="content" source="media/versioning-overview/blob-versioning-diagram.png" alt-text="Abbildung zur Veranschaulichung der Funktionsweise der Blobversionsverwaltung":::

Blobversionen sind unveränderlich. Inhalte oder Metadaten einer vorhandenen Blobversion können nicht geändert werden.

Wenn eine große Anzahl von Versionen pro Blob vorhanden ist, kann sich die Latenz bei Auflistungsvorgängen für Blobs erhöhen. Microsoft empfiehlt die Beibehaltung von weniger als 1000 Versionen pro Blob. Sie können die Lebenszyklusverwaltung verwenden, um alte Versionen automatisch zu löschen. Weitere Informationen zur Lebenszyklusverwaltung finden Sie unter [Optimieren der Kosten durch Automatisieren der Azure Blob Storage-Zugriffsebenen](./lifecycle-management-overview.md).

Die Blobversionsverwaltung ist für Standardkonten vom Typ „Allgemein v2“, für Premium-Blockblobkonten und für Legacy-Blobspeicherkonten verfügbar. Speicherkonten mit einem hierarchischen Namespace, die für die Verwendung mit Azure Data Lake Storage Gen2 aktiviert sind, werden derzeit nicht unterstützt.

Version 2019-10-10 und höher der Azure Storage-REST-API unterstützt die Blobversionsverwaltung.

> [!IMPORTANT]
> Blobversionsverwaltung kann Ihnen nicht bei der Wiederherstellung nach dem versehentlichen Löschen eines Speicherkontos oder Containers helfen. Um das versehentliche Löschen des Speicherkontos zu verhindern, konfigurieren Sie eine Sperre für die Speicherkontoressource. Weitere Informationen zum Sperren eines Speicherkontos finden Sie unter [Anwenden einer Azure Resource Manager-Sperre auf ein Speicherkonto](../common/lock-account-resource.md).

### <a name="version-id"></a>Versions-ID

Jede Blobversion wird durch eine eindeutige Versions-ID identifiziert. Der Wert der Versions-ID ist der Zeitstempel der Blobaktualisierung. Die Versions-ID wird zum Zeitpunkt der Erstellung der Version zugewiesen.

Sie können Lese- oder Löschvorgänge für eine bestimmte Version eines Blobs durchführen, indem Sie die zugehörige Versions-ID angeben. Ohne Angabe der Versions-ID wird der Vorgang für die aktuelle Version ausgeführt.

Wenn Sie einen Schreibvorgang zum Erstellen oder Ändern eines Blobs aufrufen, gibt Azure Storage den Header *x-ms-version-id* Header in der Antwort zurück. Dieser Header enthält die Versions-ID für die aktuelle Version des Blobs, das durch den Schreibvorgang erstellt wurde.

Die Versions-ID bleibt für die Lebensdauer der Version gleich.

### <a name="versioning-on-write-operations"></a>Versionsverwaltung für Schreibvorgänge

Wenn Blobversionsverwaltung aktiviert ist, erstellt jeder Schreibvorgang in einem Blob eine neue Version. Zu den Schreibvorgängen zählen [Put Blob](/rest/api/storageservices/put-blob), [Put Block List](/rest/api/storageservices/put-block-list), [Copy Blob](/rest/api/storageservices/copy-blob) und [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata).

Wenn der Schreibvorgang ein neues Blob erstellt, entspricht das sich ergebende Blob der aktuellen Version des Blobs. Wenn durch den Schreibvorgang ein vorhandenes Blob geändert wird, wird die aktuelle Version zu einer vorherigen Version, und es wird eine neue aktuelle Version erstellt, um das aktualisierte Blob zu erfassen.

Die folgende Abbildung zeigt, wie sich Schreibvorgänge auf Blobversionen auswirken. Der Einfachheit halber wird in den in diesem Artikel gezeigten Abbildungen die Versions-ID als einfacher Integerwert angezeigt. In Wirklichkeit ist die Versions-ID ein Zeitstempel. Die aktuelle Version ist blau dargestellt, und vorherige Versionen werden grau dargestellt.

:::image type="content" source="media/versioning-overview/write-operations-blob-versions.png" alt-text="Abbildung, die zeigt, wie sich Schreibvorgänge auf Blobs mit Versionsangabe auswirken":::

> [!NOTE]
> Ein Blob, das vor Aktivierung von Versionsverwaltung für das Speicherkonto erstellt wurde, weist keine Versions-ID auf. Wenn dieses Blob geändert wird, wird das geänderte Blob zur aktuellen Version, und es wird eine Version erstellt, um den Zustand des Blobs vor der Aktualisierung zu speichern. Der Version wird eine Versions-ID zugewiesen, die ihre Erstellungszeit angibt.

Wenn die Blobversionsverwaltung für ein Speicherkonto aktiviert ist, wird durch alle Schreibvorgänge für Blockblobs die Erstellung einer neuen Version ausgelöst. Einzige Ausnahme ist der Vorgang [Put Block](/rest/api/storageservices/put-block) (Block festlegen).

Bei Seiten- und Anfügeblobs wird die Versionserstellung nur durch einen Teil der Schreibvorgänge ausgelöst. Dazu zählen die Operationen:

- [Put Blob](/rest/api/storageservices/put-blob)
- [Put Block List](/rest/api/storageservices/put-block-list)
- [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata)
- [Copy Blob](/rest/api/storageservices/copy-blob)

Die folgenden Vorgänge führen nicht zum Erstellen einer neuen Version. Um Änderungen aus diesen Vorgängen aufzuzeichnen, erstellen Sie eine manuelle Momentaufnahme:

- [Put Page](/rest/api/storageservices/put-page) (Seitenblob)
- [Append Block](/rest/api/storageservices/append-block) (Anfügeblob)

Alle Versionen eines Blobs müssen denselben Blobtyp aufweisen. Wenn ein Blob über vorherige Versionen verfügt, kann ein Blob eines Typs nicht mit einem anderen Typ überschrieben werden, es sei denn, Sie löschen zuerst das Blob und alle zugehörigen Versionen.

### <a name="versioning-on-delete-operations"></a>Versionsverwaltung für Löschvorgänge

Wenn Sie den Vorgang [Delete Blob](/rest/api/storageservices/delete-blob) (Blob löschen) ohne Angabe einer Versions-ID aufrufen, wird die aktuelle Version zu einer vorherigen Version, und es gibt keine aktuelle Version mehr. Alle vorhandenen vorherigen Versionen des Blobs bleiben erhalten.

Die folgende Abbildung zeigt die Auswirkung eines Löschvorgangs auf ein Blob mit Versionsangabe:

:::image type="content" source="media/versioning-overview/delete-versioned-base-blob.png" alt-text="Abbildung, die das Löschen eines Blobs mit Versionsangabe zeigt":::

Wenn Sie eine bestimmte Version eines Blobs löschen möchten, müssen Sie für den Löschvorgang die ID der gewünschten Version angeben. Ist für das Speicherkonto vorläufiges Löschen von Blobs aktiviert, bleibt die Version bis zum Ende des Aufbewahrungszeitraums für vorläufig gelöschte Ressourcen im System erhalten.

Wenn neue Daten in das Blob geschrieben werden, wird eine neue aktuelle Version des Blobs erstellt. Alle vorhandenen Versionen sind nicht betroffen, wie in der folgenden Abbildung gezeigt.

:::image type="content" source="media/versioning-overview/recreate-deleted-base-blob.png" alt-text="Abbildung, die die erneute Erstellung eines Blobs mit Versionsangabe nach dem Löschen zeigt":::

### <a name="access-tiers"></a>Zugriffsebenen

Sie können jede beliebige Version eines Blockblobs (einschließlich der aktuellen Version) in eine andere Blobzugriffsebene verschieben, indem Sie den Vorgang [Set Blob Tier](/rest/api/storageservices/set-blob-tier) aufrufen. Sie können niedrigere Kapazitätspreise nutzen, indem Sie ältere Versionen eines Blobs in die kalte Ebene oder die Archivebene verschieben. Weitere Informationen finden Sie unter [Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“ für Blobdaten](access-tiers-overview.md).

Verwenden Sie Bloblebenszyklusverwaltung, um den Prozess des Verschiebens von Blockblobs in die entsprechende Ebene zu automatisieren. Weitere Informationen zur Lebenszyklusverwaltung finden Sie unter [Verwalten des Lebenszyklus von Azure Blob Storage](./lifecycle-management-overview.md).

## <a name="enable-or-disable-blob-versioning"></a>Aktivieren/Deaktivieren von Blobversionsverwaltung

Informationen zum Aktivieren und Deaktivieren der Blobversionsverwaltung finden Sie unter [Aktivieren und Verwalten der Blobversionsverwaltung](versioning-enable.md).

Durch Deaktivierung von Blobversionsverwaltung werden vorhandene Blobs, Versionen oder Momentaufnahmen nicht gelöscht. Wenn Sie Blobversionsverwaltung deaktivieren, kann auf alle vorhandenen Versionen in Ihrem Speicherkonto weiterhin zugegriffen werden. Anschließend werden keine weiteren neuen Versionen erstellt.

Wenn ein Blob erstellt oder geändert wurde, nachdem Versionsverwaltung für das Speicherkonto deaktiviert wurde, wird durch das Überschreiben des Blobs eine neue Version erstellt. Das aktualisierte Blob ist nicht mehr die aktuelle Version und weist keine Versions-ID auf. Alle nachfolgenden Aktualisierungen des Blobs überschreiben seine Daten, ohne den vorherigen Zustand zu speichern.

Sie können Versionen mithilfe der Versions-ID lesen oder löschen, nachdem Versionsverwaltung deaktiviert wurde. Nach Deaktivierung von Versionsverwaltung können Sie auch die Versionen eines Blobs auflisten.

Die folgende Abbildung zeigt, wie durch das Ändern eines Blobs nach dem Deaktivieren von Versionsverwaltung ein Blob ohne Versionsangabe erstellt wird. Alle vorhandenen Versionen, die dem Blob zugeordnet sind, bleiben erhalten.

:::image type="content" source="media/versioning-overview/modify-base-blob-versioning-disabled.png" alt-text="Abbildung, die zeigt, wie das Basisblob nach dem Deaktivieren der Versionsverwaltung geändert wird":::

## <a name="blob-versioning-and-soft-delete"></a>Blobversionsverwaltung und vorläufiges Löschen

Microsoft empfiehlt, sowohl die Versionsverwaltung als auch vorläufiges Löschen von Blobs für Ihre Speicherkonten zu aktivieren, damit Ihre Daten optimal geschützt sind. Weitere Informationen zum vorläufigen Löschen von Blobs finden Sie unter [Vorläufiges Löschen für Azure Storage-Blobs](./soft-delete-blob-overview.md).

### <a name="overwriting-a-blob"></a>Überschreiben eines Blobs

Wenn die Blobversionsverwaltung und vorläufiges Löschen von Blobs für ein Speicherkonto aktiviert sind, wird durch das Überschreiben eines Blobs automatisch eine neue Version erstellt. Die neue Version wird nicht vorläufig gelöscht und nicht entfernt, wenn die Beibehaltungsdauer für vorläufiges Löschen abläuft. Es werden keine vorläufig gelöschten Momentaufnahmen erstellt.

### <a name="deleting-a-blob-or-version"></a>Löschen eines Blobs oder einer Version

Wenn die Versionsverwaltung und vorläufiges Löschen für ein Speicherkonto aktiviert sind und Sie ein Blob löschen, wird die aktuelle Version des Blobs zu einer vorherigen Version. Es werden keine neue Version und auch keine vorläufig gelöschten Momentaufnahmen erstellt. Der Aufbewahrungszeitraum für vorläufig gelöschte Ressourcen ist für das gelöschte Blob nicht wirksam.

Vorläufiges Löschen bietet zusätzlichen Schutz beim Löschen von Blobversionen. Wenn Sie eine vorherige Version des Blobs löschen, wird diese Version vorläufig gelöscht. Die vorläufig gelöschte Version wird beibehalten, bis der Aufbewahrungszeitraum für vorläufig gelöschte Ressourcen verstrichen ist, und dann endgültig gelöscht.

Wenn Sie eine vorherige Version eines Blobs löschen möchten, rufen Sie den Vorgang **Delete Blob** (Blob löschen) auf, und geben Sie die Versions-ID an.

Die folgende Abbildung zeigt, was geschieht, wenn Sie ein Blob oder eine Blobversion löschen.

:::image type="content" source="media/versioning-overview/soft-delete-historical-version.png" alt-text="Abbildung, die das Löschen einer Version mit aktiviertem vorläufigem Löschen zeigt":::

### <a name="restoring-a-soft-deleted-version"></a>Wiederherstellen einer vorläufig gelöschten Version

Sie können den Vorgang [Undelete Blob](/rest/api/storageservices/undelete-blob) (Blob wiederherstellen) verwenden, um vorläufig gelöschte Versionen innerhalb des Aufbewahrungszeitraums für vorläufig gelöschte Ressourcen wiederherzustellen. Durch den Vorgang **Undelete Blob** (Blob wiederherstellen) werden immer alle vorläufig gelöschten Versionen des Blobs wiederhergestellt. Es ist nicht möglich, nur eine einzelne vorläufig gelöschte Version wiederherzustellen.

Durch das Wiederherstellen vorläufig gelöschter Versionen mit dem Vorgang **Undelete Blob** wird keine Version in die aktuelle Version höher gestuft. Stellen Sie zum Wiederherstellen der aktuellen Version zunächst alle vorläufig gelöschten Versionen wieder her, und verwenden Sie dann den Vorgang [Copy Blob](/rest/api/storageservices/copy-blob) (Blob kopieren), um eine vorherige Version in eine neue aktuelle Version zu kopieren.

In der folgenden Abbildung wird gezeigt, wie Sie vorläufig gelöschte Blobversionen mit dem Vorgang **Undelete Blob** wiederherstellen, und wie Sie die aktuelle Version des Blobs mit dem Vorgang **Copy Blob** wiederherstellen.

:::image type="content" source="media/versioning-overview/undelete-version.png" alt-text="Abbildung, die zeigt, wie vorläufig gelöschte Versionen wiederhergestellt werden":::

Nachdem die Beibehaltungsdauer für vorläufiges löschen abgelaufen ist, werden alle vorläufig gelöschten Blobversionen dauerhaft gelöscht.

## <a name="blob-versioning-and-blob-snapshots"></a>Blobversionsverwaltung und Blobmomentaufnahmen

Eine Blobmomentaufnahme ist eine schreibgeschützte Kopie eines Blobs, die zu einem bestimmten Zeitpunkt erstellt wird. Blobmomentaufnahmen und Blobversionen sind ähnlich, aber eine Momentaufnahme wird manuell von Ihnen oder Ihrer Anwendung erstellt, während eine Blobversion bei einem Schreib- oder Löschvorgang automatisch erstellt wird, wenn die Blobversionsverwaltung für Ihr Speicherkonto aktiviert ist.

> [!IMPORTANT]
> Microsoft empfiehlt, dass Sie nach der Aktivierung von Blobversionsverwaltung auch Ihre Anwendung aktualisieren, damit keine Momentaufnahmen von Blockblobs mehr erstellt werden. Wenn Versionsverwaltung für Ihr Speicherkonto aktiviert ist, werden alle Blockblobaktualisierungen und -löschungen durch Versionen aufgezeichnet und beibehalten. Die Erstellung von Momentaufnahmen bietet keinen zusätzlichen Schutz für Ihre Blockblobdaten, wenn Blobversionsverwaltung aktiviert ist, und sie erhöht möglicherweise die Kosten und die Komplexität der Anwendung.

### <a name="snapshot-a-blob-when-versioning-is-enabled"></a>Momentaufnahme eines Blobs bei aktivierter Versionsverwaltung

Obwohl dies nicht empfohlen wird, können Sie eine Momentaufnahme eines Blobs erstellen, das auch mit einer Versionsangabe versehen ist. Wenn Sie die Anwendung nicht so aktualisieren können, dass keine Momentaufnahmen von Blobs erstellt werden, wenn Sie Versionsverwaltung aktivieren, kann Ihre Anwendung sowohl Momentaufnahmen als auch Versionen unterstützen.

Wenn Sie eine Momentaufnahme eines Blobs mit Versionsangabe erstellen, wird eine neue Version zur gleichen Zeit erstellt, zu der die Momentaufnahme erstellt wird. Eine neue aktuelle Version wird ebenfalls erstellt, wenn eine Momentaufnahme erstellt wird.

Die folgende Abbildung zeigt, was geschieht, wenn Sie eine Momentaufnahme eines Blobs mit Versionsangabe erstellen. In der Abbildung enthalten Blobversionen und Momentaufnahmen mit der Versions-ID 2 und 3 identische Daten.

:::image type="content" source="media/versioning-overview/snapshot-versioned-blob.png" alt-text="Abbildung, die Momentaufnahmen eines Blobs mit Versionsangabe zeigt":::

## <a name="authorize-operations-on-blob-versions"></a>Autorisieren von Vorgängen für Blobversionen

Sie können Zugriff auf Blobversionen mit einer der folgenden Methoden autorisieren:

- Über die rollenbasierte Zugriffssteuerung von Azure (Azure Role-Based Access Control, Azure RBAC), um Berechtigungen für einen Azure Active Directory-Sicherheitsprinzipal (Azure AD) zu erteilen. Microsoft empfiehlt die Verwendung von Azure AD für mehr Sicherheit und Benutzerfreundlichkeit. Weitere Informationen zur Verwendung von Azure AD mit Blobvorgängen finden Sie unter [Autorisieren des Zugriffs auf Daten in Azure Storage](../common/authorize-data-access.md).
- Durch Delegieren des Zugriffs auf Blobversionen mithilfe einer Shared Access Signature (SAS). Geben Sie die Versions-ID für den signierten Ressourcentyp `bv` an, der eine Blobversion darstellt, um ein SAS-Token für Vorgänge mit einer bestimmten Version zu erstellen. Weitere Informationen zu SAS (Shared Access Signatures) finden Sie unter [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](../common/storage-sas-overview.md).
- Durch Verwendung der Kontozugriffsschlüssel zur Autorisierung von Vorgängen für Blobversionen mit gemeinsam verwendetem Schlüssel. Weitere Informationen finden Sie unter [Authentifizieren mit gemeinsam verwendetem Schlüssel](/rest/api/storageservices/authorize-with-shared-key).

Blobversionsverwaltung dient zum Schutz Ihrer Daten vor versehentlichem oder böswilligem Löschen. Um den Schutz zu verbessern, sind für das Löschen einer Blobversion spezielle Berechtigungen erforderlich. In den folgenden Abschnitten werden die Berechtigungen beschrieben, die zum Löschen einer Blobversion erforderlich sind.

### <a name="azure-rbac-action-to-delete-a-blob-version"></a>Azure RBAC-Aktion zum Löschen einer Blobversion

In der folgenden Tabelle wird gezeigt, welche Azure RBAC-Aktionen das Löschen eines Blobs oder einer Blobversion unterstützen:

| BESCHREIBUNG | Vorgang des Blob-Diensts | Erforderliche Azure RBAC-Datenaktion | Unterstützung für integrierte Azure-Rollen |
|----------------------------------------------|------------------------|---------------------------------------------------------------------------------------|-------------------------------|
| Löschen der aktuellen Version | Delete Blob | **Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete** | Mitwirkender an Storage-Blobdaten |
| Löschen einer vorherigen Version | Delete Blob | **Microsoft.Storage/storageAccounts/blobServices/containers/blobs/deleteBlobVersion/action** | Besitzer von Speicherblobdaten |

### <a name="shared-access-signature-sas-parameters"></a>SAS-Parameter (Shared Access Signature)

Die signierte Ressource für eine Blobversion ist `bv`. Weitere Informationen finden Sie unter [Erstellen einer Dienst-SAS](/rest/api/storageservices/create-service-sas) oder [Erstellen einer SAS für die Benutzerdelegierung](/rest/api/storageservices/create-user-delegation-sas).

In der folgenden Tabelle werden die Berechtigungen aufgeführt, die für eine SAS erforderlich sind, um eine Blobversion zu löschen.

| **Berechtigung** | **URI-Symbol** | **Zulässige Vorgänge** |
|----------------|----------------|------------------------|
| Löschen         | x              | Löschen einer Blobversion. |

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Wenn Sie Blobversionsverwaltung aktivieren, kann dies zu zusätzlichen Kosten für Datenspeicher für Ihr Konto führen. Wenn Sie beim Entwurf Ihrer Anwendung berücksichtigen, wie diese Gebühren auflaufen, können Sie die Kosten minimieren.

Blobversionen werden wie Blobmomentaufnahmen mit den gleichen Gebühren abgerechnet wie aktive Daten. Die Abrechnung von Versionen hängt davon ab, ob Sie die Dienstebene explizit für das Basisblob oder für eine seiner Versionen (oder Momentaufnahmen) festgelegt haben. Weitere Informationen zu Blobebenen finden Sie unter [Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“ für Blobdaten](access-tiers-overview.md).

Wenn Sie die Dienstebene eines Blobs oder einer Version nicht geändert haben, werden Ihnen die eindeutigen Datenblöcke in diesem Blob, seinen Versionen und allen ggf. vorhandenen Momentaufnahmen in Rechnung gestellt. Weitere Informationen finden Sie unter [Abrechnung bei nicht explizit festgelegter Blobebene](#billing-when-the-blob-tier-has-not-been-explicitly-set).

Wenn Sie die Dienstebene eines Blobs oder einer Version geändert haben, wird Ihnen das gesamte Objekt in Rechnung gestellt, unabhängig davon, ob sich das Blob und die Version wieder auf derselben Dienstebene befinden. Weitere Informationen finden Sie unter [Abrechnung bei explizit festgelegter Blobebene](#billing-when-the-blob-tier-has-been-explicitly-set).

> [!NOTE]
> Das Aktivieren von Versionsverwaltung für häufig überschriebene Daten kann zu höheren Gebühren für die Speicherkapazität und einer höheren Latenz bei Auflistungsvorgängen führen. Um diese Probleme zu vermeiden, speichern Sie häufig überschrieben Daten in einem separaten Speicherkonto mit deaktivierter Versionsverwaltung.

Weitere Informationen zu Abrechnungsdetails für Blobmomentaufnahmen finden Sie unter [Blobmomentaufnahmen](snapshots-overview.md).

### <a name="billing-when-the-blob-tier-has-not-been-explicitly-set"></a>Abrechnung bei nicht explizit festgelegter Blobebene

Wenn Sie die Blobebene für ein Basisblob oder eine der zugehörigen Versionen nicht explizit festgelegt haben, werden Ihnen die eindeutigen Blöcke oder Seiten im Blob, seinen Versionen und ggf. vorhandenen Momentaufnahmen in Rechnung gestellt. Daten, die von einem Blob und seinen Versionen gemeinsam genutzt werden, werden nur einmal in Rechnung gestellt. Wenn ein Blob aktualisiert wird, unterscheiden sich die Daten im Basisblob von den in den zugehörigen Versionen gespeicherten Daten, und die eindeutigen Daten werden pro Block oder Seite abgerechnet.

Wenn Sie einen Block innerhalb eines Block-BLOBs ersetzen, wird dieser Block anschließend als eindeutiger Block berechnet. Dies gilt auch, wenn der Block dieselbe Block-ID und dieselben Daten enthält wie in der früheren Version. Nachdem der Block erneut committet wurde, weicht er von seinem Pendant in den früheren Versionen ab, und Ihnen werden die Daten des Blocks berechnet. Gleiches gilt für eine Seite in einem Seitenblob, die mit identischen Daten aktualisiert wird.

Blob Storage kann nicht feststellen, ob zwei Blöcke identische Daten enthalten. Jeder hochgeladene Block, für den ein Commit ausgeführt wird, wird als eindeutig behandelt, selbst wenn die enthaltenen Daten und die Block-ID identisch sind. Da Gebühren jeweils für eindeutige Blöcke anfallen, sollten Sie berücksichtigen, dass beim Aktualisieren eines Blobs mit aktivierter Versionsverwaltung zusätzliche eindeutige Blöcke generiert werden, für die zusätzliche Kosten entstehen.

Wenn die Blobversionsverwaltung aktiviert ist, rufen Sie Aktualisierungsvorgänge für Blockblobs auf, damit Sie die geringstmögliche Anzahl von Blöcken aktualisieren. Die Schreibvorgänge, die eine differenzierte Steuerung von Blöcken ermöglichen, sind [Put Block](/rest/api/storageservices/put-block) und [Put Block List](/rest/api/storageservices/put-block-list). Der Vorgang [Put Blob](/rest/api/storageservices/put-blob) hingegen ersetzt den gesamten Inhalt eines Blobs und kann zu zusätzlichen Kosten führen.

Die folgenden Szenarien veranschaulichen, wie Gebühren für ein Blockblob und zugehörige Versionen berechnet werden, wenn die Blobebene nicht explizit festgelegt wurde.

#### <a name="scenario-1"></a>Szenario 1

In Szenario 1 ist eine frühere Version des Blobs vorhanden. Das Blob wurde nicht aktualisiert, seit die Version erstellt wurde, sodass Gebühren nur für die eindeutigen Blöcke 1, 2 und 3 anfallen.

![Abbildung 1: Abrechnung für eindeutige Blöcke im Basisblob und in der vorherigen Version](./media/versioning-overview/versions-billing-scenario-1.png)

#### <a name="scenario-2"></a>Szenario 2

In Szenario 2 wurde ein Block (Block 3 in der Abbildung) im Blob aktualisiert. Obwohl der aktualisierte Block die gleichen Daten und dieselbe ID enthält, ist er nicht identisch mit Block 3 in der vorherigen Version. Daher wird das Konto mit Gebühren für vier Blöcke belastet:

![Abbildung 2: Abrechnung für eindeutige Blöcke im Basisblob und in der vorherigen Version](./media/versioning-overview/versions-billing-scenario-2.png)

#### <a name="scenario-3"></a>Szenario 3

In Szenario 3 wurde das Blob aktualisiert, die Version jedoch nicht. Block 3 wurde im Basisblob durch Block 4 ersetzt, die vorherige Version enthält aber immer noch Block 3. Daher wird das Konto mit Gebühren für vier Blöcke belastet:

![Abbildung 3: Abrechnung für eindeutige Blöcke im Basisblob und in der vorherigen Version](./media/versioning-overview/versions-billing-scenario-3.png)

#### <a name="scenario-4"></a>Szenario 4

In Szenario 4 wurde das Basis-Blob vollständig aktualisiert und enthält keinen der ursprünglichen Blöcke. Folglich werden dem Konto alle acht eindeutigen Blöcke in Rechnung gestellt: vier im Basisblob und vier in der vorherigen Version. Dieses Szenario kann eintreten, wenn Sie in ein Blob mit dem Vorgang [Put Blob](/rest/api/storageservices/put-blob) schreiben, da dabei der gesamte Inhalt des Basisblobs ersetzt wird.

![Abbildung 4: Abrechnung für eindeutige Blöcke im Basisblob und in der vorherigen Version](./media/versioning-overview/versions-billing-scenario-4.png)

### <a name="billing-when-the-blob-tier-has-been-explicitly-set"></a>Abrechnung bei explizit festgelegter Blobebene

Wenn Sie die Dienstebene für ein Blob oder eine Version (oder Momentaufnahme) explizit festgelegt haben, wird Ihnen der gesamte Inhalt des Objekts auf der neuen Dienstebene in Rechnung gestellt, unabhängig davon, ob es Blöcke mit einem Objekt auf der ursprünglichen Dienstebene gemeinsam nutzt. Ihnen wird auch der vollständige Inhalt der ältesten Version auf der ursprünglichen Dienstebene in Rechnung gestellt. Bei allen anderen früheren Versionen oder Momentaufnahmen, die auf der ursprünglichen Dienstebene verbleiben, erfolgt die Abrechnung nach eindeutigen Blöcken, die sie eventuell gemeinsam nutzen, wie unter [Abrechnung bei nicht explizit festgelegter Blobebene](#billing-when-the-blob-tier-has-not-been-explicitly-set) beschrieben.

#### <a name="moving-a-blob-to-a-new-tier"></a>Verschieben eines Blobs auf eine neue Dienstebene

In der folgenden Tabelle wird das Abrechnungsverhalten für ein Blob oder eine Version beschrieben, wenn dieses oder diese auf eine neue Dienstebene verschoben wird.

| Wenn die Blobebene explizit festgelegt wird für: | Wird Folgendes in Rechnung gestellt: |
|-|-|
| Ein Basisblob mit einer früheren Version | Das Basisblob auf der neuen Dienstebene und die älteste Version auf der ursprünglichen Dienstebene sowie alle eindeutigen Blöcke in anderen Versionen.<sup>1</sup> |
| Ein Basisblob mit einer früheren Version und einer Momentaufnahme | Das Basisblob auf der neuen Dienstebene, die älteste Version auf der ursprünglichen Dienstebene und die älteste Momentaufnahme sowie alle eindeutigen Blöcke in anderen Versionen oder Momentaufnahmen.<sup>1</sup> |
| Eine frühere Version | Die Version auf der neuen Dienstebene und das Basisblob auf der ursprünglichen Dienstebene sowie alle eindeutigen Blöcke in anderen Versionen.<sup>1</sup> |

<sup>1</sup> Wenn andere frühere Versionen oder Momentaufnahmen nicht von der ursprünglichen Dienstebene verschoben wurden, werden diese Versionen oder Momentaufnahmen basierend auf der Anzahl der eindeutigen Blöcke abgerechnet, wie in [Abrechnung bei nicht explizit festgelegter Blobebene](#billing-when-the-blob-tier-has-not-been-explicitly-set) beschrieben.

Im folgenden Diagramm wird veranschaulicht, wie Objekte in Rechnung gestellt werden, wenn ein Blob mit Versionsangabe auf eine andere Dienstebene verschoben wird.

:::image type="content" source="media/versioning-overview/versioning-billing-tiers.png" alt-text="Diagramm der Abrechnung von Objekten, wenn ein Blob mit Versionsangabe explizit auf eine Dienstebene verschoben wird":::

Das explizite Festlegen der Dienstebene für ein Blob, eine Version oder eine Momentaufnahme kann nicht rückgängig gemacht werden. Wenn Sie ein Blob auf eine neue Dienstebene verschieben und dann wieder auf die ursprüngliche Dienstebene zurück verschieben, wird Ihnen der gesamte Inhalt des Objekts in Rechnung gestellt, auch wenn es Blöcke mit anderen Objekten auf der ursprünglichen Dienstebene gemeinsam nutzt.

Vorgänge, mit denen die Dienstebene eines Blobs, einer Version oder einer Momentaufnahme explizit festgelegt wird:

- [Set Blob Tier](/rest/api/storageservices/set-blob-tier)
- [Put Blob](/rest/api/storageservices/put-blob) mit der angegebenen Dienstebene
- [Put Block List](/rest/api/storageservices/put-block-list) mit der angegebenen Dienstebene
- [Copy Blob](/rest/api/storageservices/copy-blob) mit der angegebenen Dienstebene

#### <a name="deleting-a-blob-when-soft-delete-is-enabled"></a>Löschen eines Blobs mit aktiviertem vorläufigem Löschen

Wenn das vorläufige Löschen für Blobs aktiviert ist und Sie ein Basisblob löschen oder überschreiben, dessen Dienstebene explizit festgelegt wurde, werden alle früheren Versionen des vorläufig gelöschten Blobs mit ihrem vollständigen Inhalt abgerechnet. Weitere Informationen zur gemeinsamen Verwendung von Blobversionsverwaltung und vorläufigem Löschen finden Sie unter [Blobversionsverwaltung und vorläufiges Löschen](#blob-versioning-and-soft-delete).

In der folgenden Tabelle wird das Abrechnungsverhalten für ein vorläufig gelöschtes Blob in den beiden Fällen mit aktivierter und deaktivierter Versionsverwaltung beschrieben. Wenn die Versionsverwaltung aktiviert ist, wird beim vorläufigen Löschen eines Blobs eine Version erstellt. Wenn die Versionsverwaltung deaktiviert ist, wird durch das vorläufige Löschen eines Blobs eine Momentaufnahme mit vorläufigem Löschen erstellt.

| Wenn Sie ein Basisblob überschreiben, dessen Dienstebene explizit festgelegt wurde: | Wird Folgendes in Rechnung gestellt: |
|-|-|
| Vorläufiges Löschen und Versionsverwaltung wurden für das Blob aktiviert. | Alle vorhandenen Versionen mit ihrem vollständigen Inhalt – unabhängig von der Dienstebene |
| Vorläufiges Löschen für das Blob wurde aktiviert, aber die Versionsverwaltung wurde deaktiviert. | Alle vorhandenen Momentaufnahmen mit vorläufigem Löschen mit ihrem vollständigen Inhalt – unabhängig von der Dienstebene |

## <a name="feature-support"></a>Featureunterstützung

In der folgenden Tabelle wird gezeigt, wie dieses Feature in Ihrem Konto unterstützt wird und welche Auswirkungen die Aktivierung bestimmter Funktionen auf die Unterstützung hat.

| Speicherkontotyp                | Blob Storage (Standardunterstützung)   | Data Lake Storage Gen2 <sup>1</sup>                        | NFS 3.0 <sup>1</sup>
|-----------------------------|---------------------------------|------------------------------------|--------------------------------------------------|
| Standard, Universell V2 | ![Ja](../media/icons/yes-icon.png) |![Nein](../media/icons/no-icon.png)              | ![Nein](../media/icons/no-icon.png) |
| Premium-Blockblobs          | ![Ja](../media/icons/yes-icon.png) |![Nein](../media/icons/no-icon.png)              | ![Nein](../media/icons/no-icon.png) |

<sup>1</sup>    Für Data Lake Storage Gen2 und das NFS 3.0-Protokoll (Network File System) ist ein Speicherkonto mit aktiviertem hierarchischem Namespace erforderlich.

## <a name="see-also"></a>Siehe auch

- [Aktivieren und Verwalten der Blobversionsverwaltung](versioning-enable.md)
- [Erstellen einer Momentaufnahme eines Blobs](/rest/api/storageservices/creating-a-snapshot-of-a-blob)
- [Vorläufiges Löschen für Azure Storage-Blobs](./soft-delete-blob-overview.md)
