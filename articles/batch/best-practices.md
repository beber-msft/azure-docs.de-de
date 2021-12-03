---
title: Bewährte Methoden
description: Erfahren Sie, welche bewährten Methoden und nützlichen Tipps es für das Entwickeln Ihrer Azure Batch-Lösungen gibt.
ms.date: 09/03/2021
ms.topic: conceptual
ms.openlocfilehash: e910fd444fc443ef93d9cc513632ad6ab11a124e
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131461569"
---
# <a name="azure-batch-best-practices"></a>Azure Batch: bewährte Methoden

In diesem Artikel werden bewährte Methoden und nützliche Tipps für die effiziente Verwendung des Azure Batch-Diensts vorgestellt. Diese Tipps können Ihnen helfen, die Leistung zu verbessern und Entwurfsfehler bei Ihren Batch-Lösungen zu vermeiden.

> [!TIP]
> Informationen zur Sicherheit in Azure Batch finden Sie unter [Bewährte Methoden für die Sicherheit und Konformität von Azure Batch](security-best-practices.md).

## <a name="pools"></a>Pools

[Pools](nodes-and-pools.md#pools) sind die Computeressourcen zum Ausführen von Aufträgen im Batch-Dienst. Die folgenden Abschnitte enthalten Empfehlungen zum Arbeiten mit Batch-Pools.

### <a name="pool-configuration-and-naming"></a>Poolkonfiguration und Benennung

- **Poolzuordnungsmodus**: Beim Erstellen eines Batch-Kontos können Sie zwischen zwei Modi der Poolzuordnung wählen: **Batch-Dienst** und **Benutzerabonnement**. In den meisten Fällen sollten Sie den standardmäßigen Batchdienstmodus wählen, in dem Pools im Hintergrund von Batch verwalteten Abonnements zugeordnet werden. Im alternativen Benutzerabonnementmodus werden virtuelle Batchcomputer und andere Ressourcen direkt in Ihrem Abonnement erstellt, wenn ein Pool erstellt wird. Benutzerabonnementkonten werden in erster Linie verwendet, um eine kleine, aber wichtige Teilmenge von Szenarien zu unterstützen. Weitere Informationen finden Sie unter [Zusätzliche Konfiguration für den Benutzerabonnementmodus](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode).

- **„virtualMachineConfiguration“ oder „cloudServiceConfiguration“** : Obwohl Sie Pools derzeit mithilfe einer der beiden Konfigurationen erstellen können, sollten neue Pools mit „virtualMachineConfiguration“ und nicht mit „cloudServiceConfiguration“ konfiguriert werden. Alle aktuellen und neuen Batch-Funktionen werden von den Pools der Konfiguration des virtuellen Computers unterstützt. Pools mit Cloud Services-Konfiguration unterstützen nicht alle Features, und es sind keine neuen Funktionen geplant. Sie können [nach dem 29. Februar 2024](https://azure.microsoft.com/updates/azure-batch-cloudserviceconfiguration-pools-will-be-retired-on-29-february-2024/) weder neue cloudServiceConfiguration-Pools erstellen noch vorhandenen Pools neue Knoten hinzuzufügen. Weitere Informationen finden Sie unter [Migrieren der Batch-Poolkonfiguration von Cloud Services zu Virtual Machines](batch-pool-cloud-service-to-virtual-machine-configuration.md).

- **Überlegungen zu Ausführungszeiten für Aufträge und Aufgaben**: Wenn Sie über Aufträge verfügen, die hauptsächlich aus Aufgaben mit kurzer Ausführungszeit bestehen, und die erwartete Gesamtzahl der Aufgaben niedrig ist, sodass auch die zu erwartende Gesamtausführungszeit des Auftrags nicht lang ist, sollten Sie nicht für jeden Auftrag einen neuen Pool zuordnen. Die Zuordnungszeit der Knoten verringert die Laufzeit des Auftrags.

- **Mehrere Computeknoten**: Es ist nicht garantiert, dass einzelne Knoten immer verfügbar sind. Wenn auch ungewöhnlich, so können Hardwareausfälle, Betriebssystemupdates und eine Menge anderer Probleme dazu führen, dass einzelne Knoten offline sind. Wenn Ihr Batch-Workload deterministischen, garantierten Fortschritt erfordert, sollten Sie Pools mit mehreren Knoten zuordnen.

- **Images mit bevorstehendem EOL-Datum:** Es wird dringend empfohlen, Images mit bevorstehenden EOL-Datum (End of Life, Ende der Lebensdauer) für die Batch-Unterstützung zu vermeiden. Die Datumsangaben für die Unterstützungseinstellung können Sie über die [`ListSupportedImages`-API](/rest/api/batchservice/account/listsupportedimages), [PowerShell](/powershell/module/az.batch/get-azbatchsupportedimage) oder über die [Azure CLI](/cli/azure/batch/pool/supported-images) ermitteln. Es liegt in Ihrer Verantwortung, die Ansicht der für Ihre Pools relevanten EOL-Datumsangaben in regelmäßigen Abständen zu aktualisieren und Ihre Workloads vor dem EOL zu migrieren. Wenn Sie ein benutzerdefiniertes Image mit einem angegebenen Knoten-Agent verwenden, müssen Sie sicherstellen, dass Sie die EOL-Datumsangaben der Batch-Unterstützung für das Image berücksichtigen, von dem Ihr benutzerdefiniertes Image abgeleitet oder an dem es ausgerichtet wird.

- **Eindeutige Ressourcennamen**: Batch-Ressourcen (Aufträge, Pools usw.) ändern sich häufig im Laufe der Zeit. Beispielsweise erstellen Sie vielleicht am Montag einen Pool, den Sie am Dienstag löschen, um dann am Donnerstag einen weiteren ähnlichen Pool zu erstellen. Jeder neue Ressource, die Sie erstellen, sollte einen eindeutigen Name erhalten, den Sie zuvor noch nicht verwendet haben. Hierzu können Sie eine GUID (entweder als vollständiger Ressourcennamen oder als Teil davon) verwenden oder das Datum und die Uhrzeit der Ressourcenerstellung in den Ressourcennamen einbetten. Batch unterstützt [DisplayName](/dotnet/api/microsoft.azure.batch.jobspecification.displayname), sodass Sie einer Ressource einen benutzerfreundlicheren Namen geben können, auch wenn die tatsächliche Ressourcen-ID weniger benutzerfreundlich ist. Durch die Verwendung eindeutiger Namen können Sie leichter unterscheiden, welche spezifische Ressource in Protokollen und Metriken etwas bewirkt hat. Außerdem entfällt hierdurch jede eventuelle Mehrdeutigkeit, wenn Sie jemals eine Supportanfrage für eine Ressource einreichen müssen.

- **Kontinuität bei Wartung und Ausfällen des Pools**: Ihre Aufträge sollten Pools dynamisch verwenden. Wenn Ihre Aufträge denselben Pool für sämtliche Aufgaben verwenden, kann es vorkommen, dass Ihre Aufträge bei einem Problem mit dem Pool nicht ausgeführt werden können. Dies ist besonders wichtig für zeitkritische Workloads. Um dieses Problem zu beheben, wählen oder erstellen Sie einen Pool dynamisch, wenn Sie die einzelnen Aufträge planen, oder implementieren Sie eine Methode, um den Poolnamen außer Kraft zu setzen, sodass fehlerhafte Pools umgangen werden können.

- **Geschäftskontinuität während Poolverwaltung und -ausfall**: Es gibt zahlreiche Gründe, aus denen ein Pool nicht die gewünschte Größe erreichen kann, z. B. interne Fehler oder Kapazitätsbeschränkungen. Stellen Sie sicher, dass Sie Aufträge bei Bedarf einem anderen Pool zuweisen können (möglicherweise mit einer anderen VM-Größe; Batch unterstützt dies über [UpdateJob](/dotnet/api/microsoft.azure.batch.protocol.joboperationsextensions.update)). Verlassen Sie sich nicht auf eine statische Pool-ID in der Erwartung, dass diese nie gelöscht wird und sich nie ändert.

### <a name="pool-lifetime-and-billing"></a>Lebensdauer und Abrechnung für Pools

Die Poollebensdauer kann abhängig von der Zuordnungsmethode und den auf die Poolkonfiguration angewendeten Optionen variieren. Pools können eine beliebige Lebensdauer und zu jedem Zeitpunkt eine unterschiedliche Anzahl von Rechenknoten aufweisen. Es liegt in Ihrer Verantwortung, die Serverknoten im Pool entweder explizit oder durch vom Dienst bereitgestellte Features ([Autoskalierung](nodes-and-pools.md#automatic-scaling-policy) oder [automatischer Pool](nodes-and-pools.md#autopools)) zu verwalten.

- [Aktualität der Pools](https://github.com/Azure/Batch/blob/master/changelogs/nodeagent/CHANGELOG.md): Setzen Sie die Größe Ihrer Pools alle paar Monate auf null zurück, um sicherzustellen, dass Sie die **neuesten Updates und Fehlerbehebungen** für den Knoten-Agent erhalten. Ihr Pool empfängt keine Updates für den Knoten-Agent, es sei denn, er wird neu erstellt (oder seine Größe wird auf 0 Computeknoten geändert). Vor dem erneuten Erstellen des Pools oder dem Ändern der Poolgröße sollten Sie alle Knoten-Agent-Protokolle zu Debuggingzwecken herunterladen, wie im Abschnitt [Knoten](#nodes) erläutert.

- **Erneutes Erstellen von Pools:** Vermeiden Sie es in diesem Zusammenhang auch, Pools täglich zu löschen und neu zu erstellen. Erstellen Sie stattdessen einen neuen Pool, und aktualisieren Sie die vorhandenen Aufträge, sodass sie auf den neuen Pool verweisen. Nachdem alle Aufgaben in den neuen Pool verschoben wurden, löschen Sie den alten Pool.

- **Pooleffizienz und -abrechnung**: Für Batch selbst fallen keine zusätzlichen Gebühren an, aber es fallen Gebühren für die verwendeten Computeressourcen an. Ihnen wird jeder Computeknoten im Pool in Rechnung gestellt, unabhängig davon, in welchem Zustand er sich befindet. Dies schließt alle Gebühren ein, die für die Ausführung des Knotens erforderlich sind, z. B. Speicher- und Netzwerkkosten. Weitere Informationen zu bewährten Methoden finden Sie im Artikel zu [Kostenanalyse und Budgets für Azure Batch](budget.md).

- **Kurzlebige Betriebssystemdatenträger:** VM-Konfigurationspools können [kurzlebige Betriebssystemdatenträger](create-pool-ephemeral-os-disk.md) verwenden. Sie erstellen den Betriebssystemdatenträger im VM-Cache oder temporären SSD, um zusätzliche Kosten im Zusammenhang mit verwalteten Datenträgern zu vermeiden.

### <a name="pool-allocation-failures"></a>Fehler bei der Poolzuordnung

Fehler bei der Poolzuordnung können jederzeit während der ersten Zuordnung oder bei nachfolgenden Größenänderungen auftreten. Dies kann an einer vorübergehenden Kapazitätserschöpfung in einer Region oder an Fehlern in anderen Azure-Diensten liegen, auf denen Batch basiert. Ihr Kernkontingent ist keine Garantie, sondern ein Limit.

### <a name="unplanned-downtime"></a>Ungeplante Ausfallzeiten

Es ist möglich, dass es bei Batch-Pools in Azure zu Ausfallzeiten kommt. Beachten Sie dies, wenn Sie Ihr Szenario oder Ihren Workflow für Batch planen und entwickeln. Wenn Knoten ausfallen, versucht Batch automatisch, diese Computeknoten in Ihrem Auftrag wiederherzustellen. Dadurch wird möglicherweise eine Neuplanung aller laufenden Aufgaben auf dem Knoten ausgelöst, der wiederhergestellt wird. Weitere Informationen zu unterbrochenen Aufgaben finden Sie unter [Entwerfen für Wiederholungsversuche](#design-for-retries-and-re-execution).

### <a name="custom-image-pools"></a>Benutzerdefinierte Imagepools

Wenn Sie einen Azure Batch-Pool mithilfe der Konfiguration des virtuellen Computers erstellen, geben Sie das Image eines virtuellen Computers (VM) an, das das Betriebssystem für jeden Computeknoten im Pool bereitstellt. Sie können den Pool mit einem unterstützten Azure Marketplace-Image erstellen oder [ein benutzerdefiniertes Image aus einem Azure Compute Gallery-Image erstellen](batch-sig-images.md). Sie können zum Erstellen eines benutzerdefinierten Imagepools ein [verwaltetes Image](batch-custom-images.md) verwenden. Es empfiehlt sich jedoch, nach Möglichkeit mit Azure Compute Gallery benutzerdefinierte Images zu erstellen. Durch die Verwendung von Azure Compute Gallery können Sie Pools schneller bereitstellen, eine größere Anzahl von virtuellen Computern skalieren und die Zuverlässigkeit bei der Bereitstellung virtueller Computer verbessern.

### <a name="third-party-images"></a>Images von Drittanbietern

Pools können mithilfe von auf Azure Marketplace veröffentlichten Images von Drittanbietern erstellt werden. Bei Batch-Konten im Benutzerabonnementmodus kann beim Erstellen eines Pools mit bestimmten Images von Drittanbietern der Fehler „Fehler bei der Zuweisung aufgrund der Prüfung der Berechtigung zum Kauf über den Marketplace“ auftreten. Akzeptieren Sie die vom Herausgeber des Images festgelegten Bedingungen, um diesen Fehler zu beheben. Sie können dazu [Azure Powershell](/powershell/module/azurerm.marketplaceordering/set-azurermmarketplaceterms) oder die [Azure CLI](/cli/azure/vm/image/terms) verwenden.

### <a name="azure-region-dependency"></a>Abhängigkeit von Azure-Regionen

Sie sollten sich nicht auf eine einzige Azure-Region verlassen, wenn Sie eine zeitkritische oder eine Produktionsworkload verarbeiten. In seltenen Fällen kann es Probleme geben, die sich auf eine ganze Region auswirken können. Wenn Ihre Verarbeitung beispielsweise zu einem bestimmten Zeitpunkt gestartet werden muss, sollten Sie erwägen, den Pool in ihrer primären Region zentral hochzuskalieren, und zwar *eine ganze Weile im Voraus vor der Startzeit*. Wenn diese Poolskalierung fehlschlägt, können Sie als Plan B einen Pool in einer (oder mehreren) Sicherungsregion(en) zentral hochskalieren.

Pools über mehrere Konten hinweg in unterschiedlichen Regionen bieten eine bereite, leicht zugängliche Sicherung, wenn bei einem anderen Pool etwas schiefgeht. Weitere Informationen finden Sie unter [Entwerfen Ihrer Anwendung für Hochverfügbarkeit](high-availability-disaster-recovery.md).

## <a name="jobs"></a>Aufträge

Ein [Auftrag](jobs-and-tasks.md#jobs) ist ein Container, der auf die Aufnahme Hunderter, Tausender oder sogar von Millionen von Aufgaben ausgelegt ist. Beachten Sie beim Erstellen von Aufträgen die folgenden Richtlinien.

### <a name="fewer-jobs-more-tasks"></a>Weniger Aufträge, mehr Aufgaben

Die Verwendung eines Auftrags zum Ausführen einer einzelnen Aufgabe ist ineffizient. Beispielsweise ist es effizienter, einen einzelnen Auftrag mit 1000 Aufgaben zu verwenden, statt 100 Aufträge zu erstellen, die jeweils 10 Aufgaben enthalten. Das Ausführen von 1000 Aufträgen mit jeweils einer einzelnen Aufgabe wäre der am wenigsten effiziente, langsamste und teuerste Ansatz.

Vermeiden Sie deshalb den Entwurf einer Batch-Lösung, die Tausende gleichzeitig aktiver Aufträge erfordert. Es gibt kein Kontingent für Aufgaben. Wenn Sie also viele Aufgaben unter so wenigen Aufträgen wie möglich ausführen, werden Ihre [Kontingente für Aufträge und die Zeitplanung von Aufträgen](batch-quota-limit.md#resource-quotas) effizient verwendet.

### <a name="job-lifetime"></a>Lebensdauer von Aufträgen

Ein Batch-Auftrag hat eine unbegrenzte Lebensdauer, bis er aus dem System gelöscht wird. Sein Zustand bestimmt, ob er mehr Aufgaben für die Planung annehmen kann oder nicht.

Ein Auftrag wechselt nicht automatisch in den Zustand „Abgeschlossen“, es sei denn, er wurde explizit beendet. Dies kann automatisch über die Eigenschaft [onAllTasksComplete](/dotnet/api/microsoft.azure.batch.common.onalltaskscomplete) oder [maxWallClockTime-](/rest/api/batchservice/job/add#jobconstraints) ausgelöst werden.

Es gibt ein standardmäßiges [Kontingent für aktive Aufträge und die Zeitplanung von Aufträgen](batch-quota-limit.md#resource-quotas). Aufträge und Auftragszeitpläne im Zustand „Abgeschlossen“ werden nicht auf dieses Kontingent angerechnet.

## <a name="tasks"></a>Aufgaben

[Aufgaben](jobs-and-tasks.md#tasks) sind einzelne Arbeitseinheiten, die einen Auftrag bilden. Tasks werden vom Benutzer übermittelt und von Batch auf Computeknoten geplant. Die folgenden Abschnitte enthalten Vorschläge zum Entwurf Ihrer Aufgaben, um sie effizient auszuführen und Probleme zu behandeln.

### <a name="save-task-data"></a>Speichern von Aufgabendaten

Computeknoten sind von Natur aus kurzlebig. Batch-Features wie z. B. [automatische Pools](nodes-and-pools.md#autopools) und [Autoskalierung](nodes-and-pools.md#automatic-scaling-policy) können dazu führen, dass Knoten einfach wegfallen. Wenn Knoten aus einem Pool entfernt werden (aufgrund einer Größenänderung oder der Löschung eines Pools), werden auch alle Dateien auf diesen Knoten gelöscht. Aus diesem Grund sollte die Ausgabe einer Aufgabe aus dem Knoten, auf dem sie ausgeführt wird, in einen permanenten Speicher verschoben werden, bevor sie abgeschlossen wird. Wenn eine Aufgabe fehlschlägt, sollten auch die für die Diagnose des Fehlers erforderlichen Protokolle in einen permanenten Speicher verschoben werden.

Batch verfügt über integrierte Unterstützung für Azure Storage zum Hochladen von Daten über [OutputFiles](batch-task-output-files.md) sowie eine Vielzahl von freigegebenen Dateisystemen, oder Sie können auch den Upload selbst in Ihren Aufgaben durchführen.

### <a name="manage-task-lifetime"></a>Verwalten der Lebensdauer von Aufgaben

Löschen Sie Aufgaben, wenn Sie nicht mehr benötigt werden, oder legen Sie eine Einschränkung durch eine [retentionTime](/dotnet/api/microsoft.azure.batch.taskconstraints.retentiontime)-Aufgabe fest. Wenn eine `retentionTime` festgelegt wird, bereinigt Batch automatisch den von der Aufgabe verwendeten Speicherplatz, wenn die `retentionTime` abläuft.

Durch das Löschen von Aufgaben werden zwei Dinge erreicht. Es wird sichergestellt, dass sich nicht übermäßig Aufgaben im Auftrag ansammeln, was das Abfragen/Auffinden der Aufgabe erschweren kann, an der Sie interessiert sind (da Sie die abgeschlossenen Aufgaben filtern müssen). Es werden ferner die entsprechenden Aufgabendaten auf dem Knoten bereinigt (sofern `retentionTime` nicht bereits erreicht wurde). Dies hilft sicherzustellen, dass Ihre Knoten nicht mit Aufgabendaten aufgefüllt werden und der Speicherplatz knapp wird.

### <a name="submit-large-numbers-of-tasks-in-collection"></a>Übermitteln einer großen Anzahl von Aufgaben in einer Sammlung

Aufgaben können einzeln oder in Sammlungen übermittelt werden. Übermitteln Sie Aufgaben in [Sammlungen](/rest/api/batchservice/task/addcollection) von bis zu 100 Stück gleichzeitig, wenn Sie eine Massenübermittlung von Aufgaben durchführen, um den Aufwand und die Übermittlungszeiten zu verringern.

### <a name="set-max-tasks-per-node-appropriately"></a>Festlegen einer angemessenen maximalen Anzahl von Aufgaben pro Knoten

Batch unterstützt das Überabonnieren von Aufgaben auf Knoten (Ausführen von mehr Aufgaben, als ein Knoten über Kerne verfügt). Es liegt an Ihnen, sicherzustellen, dass ihre Aufgaben in die Knoten in Ihrem Pool „passen“. Es kann beispielsweise zu einer verschlechterten Erfahrung kommen, wenn Sie versuchen, acht Aufgaben zu planen, die jeweils 25 % CPU-Auslastung auf einem Knoten verbrauchen (in einem Pool mit `taskSlotsPerNode = 8`).

### <a name="design-for-retries-and-re-execution"></a>Entwerfen für Wiederholungsversuche und erneute Ausführung

Aufgaben können von Batch automatisch wiederholt werden. Es gibt zwei Arten von Wiederholungsversuchen: benutzergesteuert und intern. Benutzergesteuerte Wiederholungsversuche werden über die [maxTaskRetryCount](/dotnet/api/microsoft.azure.batch.taskconstraints.maxtaskretrycount) der Aufgabe angegeben. Wenn ein in der Aufgabe angegebenes Programm mit einem Exitcode ungleich 0 (null) beendet wird, wird die Aufgabe so oft, wie im Wert `maxTaskRetryCount` angegeben, wiederholt.

Zwar ist dies selten, doch eine Aufgabe kann aufgrund von Fehlern auf dem Computeknoten intern wiederholt werden, z. B. wenn der interne Zustand nicht aktualisiert werden kann oder ein Fehler auf dem Knoten auftritt, während die Aufgabe ausgeführt wird. Die Aufgabe wird, falls möglich, auf demselben Computeknoten bis zu einem internen Limit wiederholt, bevor die Aufgabe aufgegeben und dann zur Neuplanung durch Batch verzögert wird, potenziell auf einem anderen Computeknoten.

Beim Ausführen Ihrer Aufgaben auf dedizierten Knoten oder Knoten mit niedriger Priorität gibt es keine Unterschiede beim Design. Unabhängig davon, ob eine Aufgabe bei der Ausführung auf einem Knoten mit niedriger Priorität vorzeitig entfernt oder aufgrund eines Fehlers auf einem dedizierten Knoten unterbrochen wird, werden beide Situationen durch das Entwerfen einer gegenüber Fehlern robusten Aufgabe abgemildert.

### <a name="build-durable-tasks"></a>Erstellen von permanenten Vorgängen

Aufgaben sollten so entworfen werden, dass Sie Fehlern widerstehen und eine Wiederholung ermöglichen. Dies gilt besonders wichtig für zeitintensive Aufgaben. Stellen Sie zu diesem Zweck sicher, dass Aufgaben dasselbe einzige Ergebnis generieren, auch wenn Sie mehrmals ausgeführt werden. Eine Möglichkeit, dies zu erreichen, besteht darin, Ihre Aufgaben „zielsuchend“ auszuführen. Eine andere Möglichkeit besteht darin sicherzustellen, dass ihre Aufgaben idempotent sind (Aufgaben haben immer dasselbe Ergebnis, egal wie oft Sie ausgeführt werden).

Ein gängiges Beispiel hierfür ist eine Aufgabe zum Kopieren von Dateien auf einen Computeknoten. Ein einfacher Ansatz ist eine Aufgabe, die bei jeder Ausführung alle angegebenen Dateien kopiert, was ineffizient und nicht robust ist. Erstellen Sie stattdessen eine Aufgabe, um sicherzustellen, dass sich die Dateien auf dem Computeknoten befinden, eine Aufgabe, die keine Dateien wiederholt kopiert, die bereits vorhanden sind. Auf diese Weise setzt die Aufgabe an der Stelle fort, an der Sie aufgehört hatte oder unterbrochen wurde.

### <a name="avoid-short-execution-time"></a>Vermeiden kurzer Ausführungszeit

Aufgaben, die nur für eine bis zwei Sekunden ausgeführt werden, sind nicht ideal. Versuchen Sie, eine signifikante Menge an Arbeit in einer einzelnen Aufgabe zu verrichten (mindestens 10 Sekunden, bis zu Stunden oder Tage). Wenn jede Aufgabe für eine Minute (oder mehr) ausgeführt wird, ist der Planungsaufwand als Anteil an der gesamten Computezeit gering.

### <a name="use-pool-scope-for-short-tasks-on-windows-nodes"></a>Verwenden des Poolbereichs für Aufgaben mit kurzer Ausführungszeit in Windows-Knoten

Beim Planen einer Aufgabe in Batch-Knoten können Sie auswählen, ob sie mit dem Aufgabenbereich oder dem Poolbereich ausgeführt werden soll. Wenn die Aufgabe nur für kurze Zeit ausgeführt wird, kann der Aufgabenbereich aufgrund der erforderlichen Ressourcen zum Erstellen des automatischen Benutzerkontos für diese Aufgabe ineffizient sein. Zur Effizienzsteigerung können Sie diese Aufgaben auf den Poolbereich festlegen. Weitere Informationen finden Sie unter [Ausführen einer Aufgabe als automatischer Benutzer mit Poolbereich](batch-user-accounts.md#run-a-task-as-an-auto-user-with-pool-scope).

## <a name="nodes"></a>Nodes

Ein [Serverknoten](nodes-and-pools.md#nodes) ist ein virtueller Azure-Computer (VM) oder ein virtueller Clouddienstcomputer, der für die Verarbeitung eines Teils der Anwendungsworkload fest zugeordnet ist. Beachten Sie beim Arbeiten mit Knoten die folgenden Richtlinien.

### <a name="idempotent-start-tasks"></a>Idempotente Startaufgaben

Wie andere Aufgaben auch sollte die [Knotenstartaufgabe](jobs-and-tasks.md#start-task) idempotent sein, da sie bei jedem Neustart des Knotens erneut ausgeführt wird. Bei einer idempotenten Aufgabe handelt es sich um eine Aufgabe, die bei mehreren Wiederholungen ein konsistentes Ergebnis erzeugt.

### <a name="isolated-nodes"></a>Isolierte Knoten

Ziehen Sie die Verwendung von isolierten VM-Größen für Workloads mit Compliance- oder regulatorischen Anforderungen in Betracht. Zu den unterstützten isolierten Größen im Konfigurationsmodus des virtuellen Computers gehören `Standard_E80ids_v4`, `Standard_M128ms`, `Standard_F72s_v2`, `Standard_G5`, `Standard_GS5` und `Standard_E64i_v3`. Weitere Informationen zu isolierten VM-Größen finden Sie unter [Isolation von virtuellen Computern in Azure](../virtual-machines/isolation.md).

### <a name="manage-long-running-services-via-the-operating-system-services-interface"></a>Verwalten zeitintensiver Dienste über die Betriebssystem-Diensteschnittstelle

In manchen Fällen ist es erforderlich, einen anderen Agent parallel zum Batch-Agent im Knoten auszuführen. Beispielsweise möchten Sie eventuell Daten vom Knoten erfassen und melden. Wir empfehlen, diese Agents als Betriebssystemdienste bereitzustellen, z. B. als Windows-Dienst oder als `systemd`-Dienst von Linux.

Bei der Ausführung dieser Dienste dürfen sie keine Dateisperren für Dateien in von Batch verwalteten Verzeichnissen auf dem Knoten einrichten, da Batch sonst diese Verzeichnisse wegen der Dateisperren nicht löschen kann. Wenn Sie beispielsweise einen Windows-Dienst in einer Startaufgabe installieren, anstatt den Dienst direkt aus dem Arbeitsverzeichnis der Startaufgabe zu starten, kopieren Sie die Dateien an einen anderen Ort (oder wenn die Dateien vorhanden sind, überspringen Sie einfach das Kopieren). Installieren Sie dann den Dienst von diesem Ort aus. Wenn Batch Ihre Startaufgabe erneut ausführt, wird das Arbeitsverzeichnis der Startaufgabe gelöscht und erneut erstellt. Dies funktioniert, weil der Dienst Dateisperren im anderen Verzeichnis und nicht im Arbeitsverzeichnis der Startaufgabe eingerichtet hat.

### <a name="avoid-creating-directory-junctions-in-windows"></a>Vermeiden der Erstellung von Verzeichnisverbindungen

Verzeichnisverbindungen, manchmal auch als feste Verzeichnisverknüpfungen bezeichnet, sind während der Aufgaben- und Auftragsbereinigung schwierig zu behandeln. Verwenden Sie symlinks (symbolische Verknüpfungen) anstelle von festen Verknüpfungen.

### <a name="collect-batch-agent-logs"></a>Erfassen von Batch-Agent-Protokollen

Wenn Sie ein Problem bemerken, das mit dem Verhalten eines Knotens oder einer Aufgabe, die auf einem Knoten ausgeführt wird, zusammenhängt, sollten Sie die Batch-Agent-Protokolle vor dem Aufheben der Zuordnung der fraglichen Knoten erfassen. Die Batch-Agent-Protokolle lassen sich mithilfe der API zum Hochladen von Batch-Dienstprotokollen erfassen. Diese Protokolle können als Teil eines Supporttickets an Microsoft bereitgestellt werden und helfen bei der Behandlung und Behebung von Problemen.

### <a name="manage-os-upgrades"></a>Verwalten von Betriebssystem-Upgrades

Bei Batch-Konten im Modus „Benutzerabonnement“ können automatische Betriebssystem-Upgrades den Taskfortschritt unterbrechen, insbesondere dann, wenn Tasks über einen längeren Zeitraum ausgeführt werden. Das [Entwickeln idempotenter Tasks](#build-durable-tasks) kann zur Reduzierung von Fehlern beitragen, die durch diese Unterbrechungen verursacht werden. Wir empfehlen außerdem, [Upgrades von Betriebssystem-Images für Uhrzeiten zu planen, in denen erwartungsgemäß keine Aufgaben ausgeführt werden](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md#manually-trigger-os-image-upgrades).

Für Windows-Pools ist `enableAutomaticUpdates` standardmäßig auf `true` festgelegt. Das Zulassen automatischer Updates wird zwar empfohlen, aber Sie können diesen Wert auf `false` festlegen, wenn Sie sicherstellen müssen, dass ein Betriebssystemupdate nicht unerwartet durchgeführt wird.

## <a name="isolation-security"></a>Isolierungssicherheit

Falls Ihr Szenario die Isolierung von Aufträgen voneinander erfordert, sollten Sie zu diesem Zwecke die Aufträge in separaten Pools platzieren. Ein Pool ist die Sicherheitsisolierungsgrenze in Batch, und standardmäßig sind zwei Pools nicht gegenseitig sichtbar und können auch nicht miteinander kommunizieren. Vermeiden Sie die Verwendung separater Batch-Konten als Isolierungsmethode.

## <a name="connectivity"></a>Konnektivität

Lesen Sie den folgenden Leitfaden zur Konnektivität in Ihren Batch-Lösungen.

### <a name="network-security-groups-nsgs-and-user-defined-routes-udrs"></a>Netzwerksicherheitsgruppen (NSGs) und benutzerdefinierte Routen (UDRs)

Wenn Sie [Batch-Pools in einem virtuellen Netzwerk](batch-virtual-network.md) bereitstellen, sollten Sie sicherstellen, dass Sie die Richtlinien bezüglich der Verwendung des `BatchNodeManagement`-Diensttags, der Ports, der Protokolle und der Richtung der Regel streng befolgen. Die Verwendung des Diensttags anstelle der zugrunde liegenden IP-Adressen des Batch-Diensts wird dringend empfohlen. Der Grund dafür ist, dass IP-Adressen sich im Laufe der Zeit ändern können. Die direkte Verwendung von IP-Adressen des Batch-Diensts kann zu Instabilität, Unterbrechungen oder Ausfällen für die Batch-Pools führen.

Stellen Sie für benutzerdefinierte Routen (User Defined Routes, UDRs) sicher, dass Sie über einen Prozess verfügen, um die IP-Adressen des Batch-Diensts in regelmäßigen Abständen in der Routingtabelle zu aktualisieren, da sich diese Adressen im Laufe der Zeit ändern. Informationen dazu, wie Sie die Liste mit den IP-Adressen des Batch-Diensts abrufen, finden Sie unter [Lokale Diensttags](../virtual-network/service-tags-overview.md). Die IP-Adressen des Batch-Diensts werden dem `BatchNodeManagement`-Diensttag (oder der regionalen Variante, die Ihrer Batch-Kontoregion entspricht) zugeordnet.

### <a name="honoring-dns"></a>Berücksichtigen von DNS

Stellen Sie sicher, dass Ihre Systeme die DNS-Gültigkeitsdauer (Time-to-Live, TTL) für Ihre Batch-Kontodienst-URL berücksichtigen. Stellen Sie darüber hinaus sicher, dass die Batch-Dienstclients und andere Konnektivitätsmechanismen für den Batch-Dienst nicht auf IP-Adressen basieren (oder [erstellen Sie einen Pool mit statischen öffentlichen IP-Adressen](create-pool-public-ip.md), wie im Folgenden beschrieben).

Wenn Ihre Anforderungen HTTP-Antworten auf 5xx-Ebene empfangen und ein „Connection: close“-Header in der Antwort vorhanden ist, sollte der Batch-Dienstclient die Empfehlung beachten, indem er die vorhandene Verbindung schließt, DNS für die Batch-Kontodienst-URL erneut auflöst und dann die folgenden Anforderungen mit einer neuen Verbindung versucht.

### <a name="retry-requests-automatically"></a>Automatisches Wiederholen von Anforderungen

Stellen Sie sicher, dass für Ihre Batch-Dienstclients geeignete Wiederholungsrichtlinien vorhanden sind, um Ihre Anforderungen auch während des normalen Betriebs und nicht ausschließlich während der Dienstwartungszeiten zu wiederholen. Diese Wiederholungsrichtlinien sollten ein Intervall von mindestens 5 Minuten umfassen. Automatische Wiederholungsfunktionen werden mit verschiedenen Batch-SDKs bereitgestellt, etwa in der [RetryPolicyProvider-Klasse von .NET](/dotnet/api/microsoft.azure.batch.retrypolicyprovider).

### <a name="static-public-ip-addresses"></a>Statische öffentliche IP-Adressen

Normalerweise erfolgt der Zugriff auf virtuelle Computer in einem Batch-Pool über öffentliche IP-Adressen, die sich während der Nutzungsdauer des Pools ändern können. Dies kann die Interaktion mit einer Datenbank oder einem anderen externen Dienst erschweren, bei denen der Zugriff auf bestimmte IP-Adressen beschränkt ist. Um sicherzustellen, dass die öffentlichen IP-Adressen in einem Pool nicht unerwartet geändert werden, können Sie einen Pool mithilfe einer Gruppe von statischen öffentlichen IP-Adressen erstellen, die Sie steuern. Weitere Informationen finden Sie unter [Erstellen eines Azure Batch-Pools mit angegebenen öffentlichen IP-Adressen](create-pool-public-ip.md).

### <a name="testing-connectivity-with-cloud-services-configuration"></a>Testen der Konnektivität mit Cloud Services-Konfiguration

Sie können das normale „ping“/ICMP-Protokoll nicht mit Cloud Services verwenden, da das ICMP-Protokoll nicht über Azure Load Balancer zugelassen wird. Weitere Informationen finden Sie unter [Konnektivität und Netzwerke in Azure Cloud Services](../cloud-services/cloud-services-connectivity-and-networking-faq.yml#can-i-ping-a-cloud-service-).

## <a name="batch-node-underlying-dependencies"></a>Zugrunde liegende Abhängigkeiten von Batch-Knoten

Beachten Sie beim Entwerfen Ihrer Batch-Lösungen die folgenden Abhängigkeiten und Einschränkungen.

### <a name="system-created-resources"></a>Vom System erstellte Ressourcen

Azure Batch erstellt und verwaltet eine Gruppe von Benutzern und Gruppen auf der VM, die nicht geändert werden sollten. Dies sind:

Windows:

- Ein Benutzer mit dem Namen **PoolNonAdmin**
- Eine Benutzergruppe mit dem Namen **WATaskCommon**

Linux:

- Ein Benutzer mit dem Namen **_azbatch**

### <a name="file-cleanup"></a>Dateibereinigung

Batch versucht, das Arbeitsverzeichnis zu bereinigen, in dem Aufgaben ausgeführt werden, sobald die Aufbewahrungsdauer abläuft. Alle außerhalb dieses Verzeichnisses geschriebenen Dateien [müssen von Ihnen bereinigt werden](#manage-task-lifetime), um die Überfüllung des Speicherplatzes zu verhindern.

Wenn Sie einen Dienst unter Windows aus dem Arbeitsverzeichnis „startTask“ ausführen, wird die automatisierte Bereinigung für das Arbeitsverzeichnis blockiert, weil der Ordner noch verwendet wird. Dadurch wird die Leistung beeinträchtigt. Um dieses Problem zu beheben, ändern Sie das Verzeichnis für diesen Dienst in ein anderes Verzeichnis, das nicht von Batch verwaltet wird.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Workflow des Batch-Diensts und primäre Ressourcen](batch-service-workflow-features.md) wie Pools, Knoten, Aufträge und Aufgaben.
- Erfahren Sie mehr über [Azure Batch-Standardkontingente, Grenzwerte und Einschränkungen sowie die Anforderung von Kontingentsteigerungen](batch-quota-limit.md).
- Erfahren Sie, wie Sie [Fehler in Pool- und Knotenhintergrundvorgängen erkennen und vermeiden](batch-pool-node-error-checking.md) können.
