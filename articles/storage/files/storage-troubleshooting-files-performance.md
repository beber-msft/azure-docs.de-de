---
title: Leistungsprobleme mit Azure-Dateifreigaben – Handbuch zur Problembehandlung
description: Hier finden Sie Informationen zur Behandlung bekannter Leistungsprobleme mit Azure-Dateifreigaben. Machen Sie sich mit möglichen Ursachen und entsprechenden Problemumgehungen vertraut.
author: jeffpatt24
ms.service: storage
ms.topic: troubleshooting
ms.date: 07/06/2021
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 44cbae10fc83ddbacf9abc8fd6510667c1f27e00
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132524377"
---
# <a name="troubleshoot-azure-file-shares-performance-issues"></a>Problembehandlung bei Leistungsproblemen mit Azure-Dateifreigaben

In diesem Artikel sind einige allgemeine Probleme aufgeführt, die im Zusammenhang mit Azure-Dateifreigaben auftreten können. Er enthält mögliche Ursachen und Problemumgehungen, wenn diese Probleme bei Ihnen auftreten.

## <a name="applies-to"></a>Gilt für:
| Dateifreigabetyp | SMB | NFS |
|-|:-:|:-:|
| Standard-Dateifreigaben (GPv2), LRS/ZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Standard-Dateifreigaben (GPv2), GRS/GZRS | ![Ja](../media/icons/yes-icon.png) | ![Nein](../media/icons/no-icon.png) |
| Premium-Dateifreigaben (FileStorage), LRS/ZRS | ![Ja](../media/icons/yes-icon.png) | ![Ja](../media/icons/yes-icon.png) |

## <a name="high-latency-low-throughput-and-general-performance-issues"></a>Hohe Latenz, niedriger Durchsatz und allgemeine Leistungsprobleme

### <a name="cause-1-share-was-throttled"></a>Ursache 1: Freigabe wurde gedrosselt

Anforderungen werden gedrosselt, wenn die E/A-Vorgänge pro Sekunde (I/O Operations Per Second, IOPS) und Grenzwerte für „eingehend“ oder „ausgehend“ für eine Dateifreigabe erreicht werden. Informationen zu den Grenzwerten für Standard- und Premium-Dateifreigaben finden Sie unter [Skalierungsziele für Dateifreigaben und Dateien](./storage-files-scale-targets.md#azure-file-share-scale-targets).

Wenn Sie überprüfen möchten, ob Ihre Freigabe gerade gedrosselt wird, können Sie im Portal auf Azure-Metriken zugreifen und sie verwenden.

1. Wechseln Sie im Azure-Portal zu Ihrem Speicherkonto.

1. Wählen Sie im linken Bereich unter **Überwachung** die Option **Metriken** aus.

1. Wählen Sie **Datei** als Metriknamespace für Ihren Speicherkontobereich aus.

1. Wählen Sie **Transaktionen** als Metrik aus.

1. Fügen Sie einen Filter für den **Antworttyp** hinzu, und überprüfen Sie dann, ob Anforderungen gedrosselt wurden. 

    Bei Standard-Dateifreigaben werden die folgenden Antworttypen protokolliert, wenn eine Anforderung gedrosselt wird:

    - SuccessWithThrottling
    - SuccessWithShareIopsThrottling
    - ClientShareIopsThrottlingError

    Bei Premium-Dateifreigaben werden die folgenden Antworttypen protokolliert, wenn eine Anforderung gedrosselt wird:

    - SuccessWithShareEgressThrottling
    - SuccessWithShareIngressThrottling
    - SuccessWithShareIopsThrottling
    - ClientShareEgressThrottlingError
    - ClientShareIngressThrottlingError
    - ClientShareIopsThrottlingError

    Weitere Informationen zu den einzelnen Antworttypen finden Sie unter [Metrikdimensionen](./storage-files-monitoring-reference.md#metrics-dimensions).

    ![Der Screenshot der Metrikoptionen für Premium-Dateifreigaben zeigt einen Eigenschaftsfilter „Antworttyp“.](media/storage-troubleshooting-premium-fileshares/metrics.png)

    > [!NOTE]
    > Informationen zum Empfangen einer Warnung finden Sie weiter unten in diesem Artikel im Abschnitt [„Erstellen einer Warnung, wenn eine Dateifreigabe gedrosselt ist“](#how-to-create-an-alert-if-a-file-share-is-throttled).

#### <a name="solution"></a>Lösung

- Wenn Sie eine Standarddateifreigabe verwenden, [aktivieren Sie große Dateifreigaben](storage-how-to-create-file-share.md#enable-large-files-shares-on-an-existing-account) in Ihrem Speicherkonto, und [erhöhen Sie das Dateifreigabekontingent, um die Unterstützung für große Dateifreigaben zu nutzen](storage-how-to-create-file-share.md#expand-existing-file-shares). Große Dateifreigaben unterstützen hohe Grenzwerte für IOPS und Bandbreite. Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Files](storage-files-scale-targets.md).
- Wenn Sie eine Premium-Dateifreigabe verwenden, erhöhen Sie die Größe der bereitgestellten Dateifreigabe, um den IOPS-Grenzwert heraufzusetzen. Weitere Informationen finden Sie unter [Grundlegendes zur Bereitstellung für Premium-Dateifreigaben](./understanding-billing.md#provisioned-model).

### <a name="cause-2-metadata-or-namespace-heavy-workload"></a>Ursache 2: Hohe Arbeitsauslastung bei Metadaten oder Namespace

Wenn der größte Teil Ihrer Anforderungen metadatenzentriert ist (z. B. `createfile`, `openfile`, `closefile`, `queryinfo` oder `querydirectory`), wird die Latenz schlimmer als diejenige von Lese-/Schreibvorgängen sein.

Wenn Sie ermitteln möchten, ob die meisten Ihrer Anforderungen metadatenzentriert sind, beginnen Sie mit der Ausführung der Schritte 1–4, wie oben in „Ursache 1“ beschrieben. Wenn Sie in Schritt 5 keinen Filter für **Antworttyp** hinzufügen möchten, fügen Sie stattdessen einen Eigenschaftsfilter für **API-Name** hinzu.

![Der Screenshot der Metrikoptionen für Premium-Dateifreigaben zeigt einen Eigenschaftsfilter „API-Name“.](media/storage-troubleshooting-premium-fileshares/MetadataMetrics.png)

#### <a name="workaround"></a>Problemumgehung

- Überprüfen Sie, ob die Anwendung so geändert werden kann, dass sich die Anzahl von Metadatenvorgängen verringert.
- Fügen Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) für die Dateifreigabe hinzu, und binden Sie die VHD vom Client ein, um Dateivorgänge für die Daten durchzuführen. Dieser Ansatz funktioniert bei Szenarios mit einem einzelnen Writer/Reader oder bei Szenarios mit mehreren Readern und keinen Writern. Weil das Dateisystem im Besitz des Clients (und nicht von Azure Files) ist, dürfen Metadatenvorgänge lokal sein. Das-Setup bietet eine ähnliche Leistung wie bei einem lokalen, direkt angeschlossenen Speicher.
    -   Verwenden Sie zum Einbinden einer VHD auf einem Windows-Client das PowerShell-Cmdlet [Mount-DiskImage](/powershell/module/storage/mount-diskimage).
    -   Informationen zum Einbinden einer VHD unter Linux finden Sie in der Dokumentation für Ihre Linux-Distribution.     

### <a name="cause-3-single-threaded-application"></a>Ursache 3: Singlethread-Anwendung

Wenn die von Ihnen verwendete Anwendung eine Singlethread-Anwendung ist, kann dieses Setup dazu führen, dass der IOPS-Durchsatz deutlich niedriger ist als derjenige, der je nach Ihrer bereitgestellten Freigabegröße maximal möglich wäre.

#### <a name="solution"></a>Lösung

- Erhöhen Sie die Parallelität Ihrer Anwendung, indem Sie die Anzahl von Threads erhöhen.
- Wechseln Sie zu Anwendungen, in denen Parallelität möglich ist. Für Kopiervorgänge könnten Sie beispielsweise „AzCopy“ oder „RoboCopy“ von Windows-Clients oder den Befehl **parallel** von Linux-Clients verwenden.

### <a name="cause-4-number-of-smb-channels-exceeds-four"></a>Ursache 4: mehr als vier SMB-Kanäle

Wenn Sie SMB MultiChannel verwenden und die Anzahl Ihrer Kanäle vier überschreitet, führt dies zu einer schlechteren Leistung. Verwenden Sie das PowerShell-Cmdlet `get-SmbClientConfiguration`, um die aktuellen Einstellungen für die Verbindungsanzahl anzuzeigen und zu ermitteln, ob die Anzahl der Verbindungen vier überschreitet.

#### <a name="solution"></a>Lösung

Legen Sie unter Windows die NIC-Einstellung für SMB fest, damit die Gesamtanzahl der Kanäle vier nicht überschreitet. Wenn Sie beispielsweise über zwei NICs verfügen, können Sie die maximale Anzahl pro NIC mithilfe des folgenden PowerShell-Cmdlets auf zwei festlegen: `Set-SmbClientConfiguration -ConnectionCountPerRssNetworkInterface 2`.

## <a name="very-high-latency-for-requests"></a>Sehr hohe Latenz für Anforderungen

### <a name="cause"></a>Ursache

Der virtuelle Clientcomputer (Virtual Machine, VM) könnte sich in einer anderen Region als die Dateifreigabe befinden. Eine andere mögliche Ursache für hohe Wartezeit kann die client- oder netzwerkbedingte Wartezeit sein.

### <a name="solution"></a>Lösung

- Führen Sie die Anwendung auf einem virtuellen Computer aus, der sich in derselben Region wie die Dateifreigabe befindet.
- Überprüfen Sie für Ihr Speicherkonto die Transaktionsmetriken **SuccessE2ELatency** und **SuccessServerLatency** über **Azure Monitor** im Azure-Portal. Eine große Differenz zwischen den Metriken „SuccessE2ELatency“ und „SuccessServerLatency“ deutet auf Wartezeit hin, die wahrscheinlich durch das Netzwerk oder den Client verursacht wird. Weitere Informationen finden Sie in der Referenz zur Überwachung von Daten in Azure Files unter [Transaktionsmetriken](storage-files-monitoring-reference.md#transaction-metrics).

## <a name="client-unable-to-achieve-maximum-throughput-supported-by-the-network"></a>Client kann den maximalen Durchsatz des Netzwerks nicht erreichen

### <a name="cause"></a>Ursache
Eine mögliche Ursache ist das Fehlen von SMB-Unterstützung für mehrere Kanäle bei Standarddateifreigaben. Zurzeit unterstützt Azure Files nur Einkanalübertragung, sodass es nur eine einzige Verbindung zwischen dem virtuellen Clientcomputer und dem Server gibt. Diese einzelne Verbindung ist an einen einzelnen Kern auf dem virtuellen Clientcomputer gebunden, weshalb der maximal erreichbare Durchsatz für den virtuellen Computer durch den einzelnen Kern beschränkt ist.

### <a name="workaround"></a>Problemumgehung

- [Aktivieren Sie SMB Multichannel](files-smb-protocol.md#smb-multichannel) für Premium-Dateifreigaben.
- Durch Bereitstellen eines virtuellen Computers mit einem größeren Kern könnte der Durchsatz möglicherweise erhöht werden.
- Durch Ausführen der Clientanwendung auf mehreren virtuellen Computern wird der Durchsatz erhöht.
- Verwenden Sie nach Möglichkeit REST-APIs.
- Für NFS-Dateifreigaben ist nconnect in der Vorschau verfügbar. Die Verwendung wird für Produktionsworkloads nicht empfohlen.

## <a name="throughput-on-linux-clients-is-significantly-lower-than-that-of-windows-clients"></a>Der Durchsatz auf Linux-Clients ist deutlich geringer als auf Windows-Clients

### <a name="cause"></a>Ursache

Dies ist ein bekanntes Problem bei der Implementierung des SMB-Clients unter Linux.

### <a name="workaround"></a>Problemumgehung

- Verteilen Sie die Last auf mehrere virtuelle Computer.
- Verwenden Sie auf demselben virtuellen Computer mehrere Bereitstellungspunkte mit der Option `nosharesock`, und verteilen Sie die Last auf diese Punkte.
- Versuchen Sie unter Linux, die Einbindung mit einer Option `nostrictsync` durchzuführen, um eine SMB-Leerung bei jedem Aufruf von `fsync` zu vermeiden. Bei Azure Files beeinträchtigt diese Option nicht die Datenkonsistenz. Sie könnte aber in Verzeichnisauflistungen zu veralteten Dateimetadaten (Befehl `ls -l`) führen. Durch direktes Abfragen von Dateimetadaten mit dem Befehl `stat` werden die aktuellsten Dateimetadaten zurückgegeben.

## <a name="high-latencies-for-metadata-heavy-workloads-involving-extensive-openclose-operations"></a>Hohe Latenzen bei metadatenorientierten hohen Arbeitslasten, die sich aus umfangreichen Öffnen-/Schließen-Vorgängen ergeben

### <a name="cause"></a>Ursache

Es werden keine Verzeichnis-Leasedauern unterstützt.

### <a name="workaround"></a>Problemumgehung

- Vermeiden Sie nach Möglichkeit, dass Öffnen-/Schließen-Vorgänge in demselben Verzeichnis innerhalb kurzer Zeit verarbeitet werden müssen.
- Erhöhen Sie bei virtuellen Linux-Computern das Cachetimeout für Verzeichniseinträge, indem Sie die Einbindungsoption `actimeo=<sec>` angeben. Standardmäßig beträgt das Timeout 1 Sekunde, sodass ein größerer Wert, z. B. 3 oder 5 Sekunden, hilfreich sein könnte.
- Aktualisieren Sie das System bei virtuellen CentOS Linux- oder Red Hat Enterprise Linux (RHEL)-Computern auf CentOS Linux 8.2 bzw. RHEL 8.2. Aktualisieren Sie bei anderen virtuellen Linux-Computern den Kernel auf mindestens 5.0.

## <a name="low-iops-on-centos-linux-or-rhel"></a>Wenige IOPS unter CentOS Linux oder RHEL

### <a name="cause"></a>Ursache

Eine E/A-Tiefe von mehr als 1 wird unter CentOS Linux oder RHEL nicht unterstützt.

### <a name="workaround"></a>Problemumgehung

- Führen Sie ein Upgrade auf CentOS Linux 8 oder RHEL 8 durch.
- Wechseln Sie zu Ubuntu.

## <a name="slow-file-copying-to-and-from-azure-file-shares-in-linux"></a>Langsames Kopieren von Dateien in und aus Azure-Dateifreigaben unter Linux

Wenn bei Ihnen Dateien nur langsam kopiert werden, lesen Sie im [Handbuch zur Problembehandlung für Linux](storage-troubleshoot-linux-file-connection-problems.md#slow-file-copying-to-and-from-azure-files-in-linux) den Abschnitt „Langsames Kopieren von Dateien in und aus Azure-Dateifreigaben unter Linux“.

## <a name="jittery-or-sawtooth-pattern-for-iops"></a>Unregelmäßiges oder sägezahnartiges Muster bei IOPS

### <a name="cause"></a>Ursache

Die Clientanwendung überschreitet durchgängig den IOPS-Grundwert. Zurzeit gibt es keine dienstseitige Glättung der Anforderungslast. Wenn der Client den IOPS-Grundwert überschreitet, wird er vom Dienst gedrosselt. Die Drosselung kann dazu führen, dass sich für den Client ein unregelmäßiges oder sägezahnartiges IOPS-Muster ergibt. In diesem Fall kann der vom Client erreichte durchschnittliche IOPS niedriger als der IOPS-Grundwert sein.

### <a name="workaround"></a>Problemumgehung
- Verringern Sie die Anforderungslast aus der Clientanwendung, sodass die Freigabe nicht gedrosselt wird.
- Erhöhen Sie das Kontingent für die Freigabe, sodass sie nicht gedrosselt wird.

## <a name="excessive-directoryopendirectoryclose-calls"></a>Übermäßig viele DirectoryOpen/DirectoryClose-Aufrufe

### <a name="cause"></a>Ursache

Wenn sich die Anzahl von **DirectoryOpen/DirectoryClose**-Aufrufen unter den häufigsten API-Aufrufen befindet und Sie nicht erwarten, dass der Client so viele Aufrufe ausführt, könnte die Problemursache die auf dem virtuellen Azure-Client installierte Antivirensoftware sein.

### <a name="workaround"></a>Problemumgehung

- Eine Behebung dieses Problems ist im [Plattformupdate für Windows aus April](https://support.microsoft.com/help/4052623/update-for-windows-defender-antimalware-platform) verfügbar.

## <a name="file-creation-is-slower-than-expected"></a>Dateierstellung ist langsamer als erwartet

### <a name="cause"></a>Ursache

Bei Workloads, die sich aus der Erstellung einer großen Anzahl von Dateien ergeben, gibt es keinen wesentlichen Leistungsunterschied zwischen Premium-Dateifreigaben und Standarddateifreigaben.

### <a name="workaround"></a>Problemumgehung

- Keine.

## <a name="slow-performance-from-windows-81-or-server-2012-r2"></a>Niedrige Leistung von Windows 8.1 oder Server 2012 R2

### <a name="cause"></a>Ursache

Zugriffe auf Azure-Dateifreigaben haben für E/A-intensive Workloads eine höhere Latenz als erwartet.

### <a name="workaround"></a>Problemumgehung

- Installieren Sie den verfügbaren [Hotfix](https://support.microsoft.com/help/3114025/slow-performance-when-you-access-azure-files-storage-from-windows-8-1).

## <a name="smb-multichannel-is-not-being-triggered"></a>SMB Multichannel wird nicht ausgelöst.

### <a name="cause"></a>Ursache

Letzte Änderungen an SMB Multichannel-Konfigurationseinstellungen ohne eine erneute Bereitstellung.

### <a name="solution"></a>Lösung
 
-   Nach Änderungen an den SMB Multichannel-Konfigurationseinstellungen für den Windows SMB-Client oder das Multichannel-Konto müssen Sie die Bereitstellung der Freigabe aufheben, 60 Sekunden lang warten und dann die Freigabe erneut bereitstellen, um den Multichannel-Vorgang auszulösen.
-   Generieren Sie beim Windows-Clientbetriebssystem die E/A-Auslastung mit hoher Warteschlangentiefe (Queue Depth) wie „QD=8“, beispielsweise durch das Kopieren einer Datei zum Auslösen von SMB Multichannel.  Beim Serverbetriebssystem wird SMB Multichannel mit „QD=1“ ausgelöst, also sobald Sie eine E/A für die Freigabe starten.

## <a name="high-latency-on-web-sites-hosted-on-file-shares"></a>Hohe Latenz bei Websites, die auf Dateifreigaben gehostet werden 

### <a name="cause"></a>Ursache  

Eine hohe Anzahl von Dateiänderungsbenachrichtigungen für Dateifreigaben kann zu erheblichen hohen Latenzen führen. Dieses Problem tritt normalerweise bei Websites auf, die auf Dateifreigaben mit einer tief geschachtelten Verzeichnisstruktur gehostet werden. Ein typisches Szenario ist eine IIS-gehostete Webanwendung, bei der eine Dateiänderungsbenachrichtigung für jedes Verzeichnis in der Standardkonfiguration eingerichtet wird. Bei jeder Änderung ([ReadDirectoryChangesW](/windows/win32/api/winbase/nf-winbase-readdirectorychangesw)) für die Freigabe, für die der Client registriert ist, wird eine Änderungsbenachrichtigung vom Dateidienst an den Client gepusht. Dadurch werden Systemressourcen beansprucht, und das Problem verschlimmert sich mit zunehmender Anzahl von Änderungen. Dies kann eine Freigabedrosselung verursachen und folglich zu einer höheren clientseitigen Latenz führen. 

Zur Überprüfung können Sie die Azure-Metriken im Portal folgendermaßen verwenden: 

1. Wechseln Sie im Azure-Portal zu Ihrem Speicherkonto. 
1. Wählen Sie im linken Menü unter „Überwachung“ die Option „Metriken“ aus. 
1. Wählen Sie „Datei“ als Metriknamespace für Ihren Speicherkontobereich aus. 
1. Wählen Sie „Transaktionen“ als Metrik aus. 
1. Fügen Sie einen Filter für „ResponseType“ hinzu, und überprüfen Sie, ob es Anforderungen gibt, die den Antwortcode „SuccessWithThrottling“ (für SMB oder NFS) oder „ClientThrottlingError“ (für REST) enthalten.

### <a name="solution"></a>Lösung 

- Wenn keine Dateiänderungsbenachrichtigung verwendet wird, deaktivieren Sie diese Benachrichtigung (bevorzugt).
    - [Deaktivieren Sie die Dateiänderungsbenachrichtigung](https://support.microsoft.com/help/911272/fix-asp-net-2-0-connected-applications-on-a-web-site-may-appear-to-sto) durch Aktualisieren von „FCNMode“. 
    - Aktualisieren Sie das Abrufintervall des IIS-Workerprozesses (W3WP) auf „0“, indem Sie in Ihrer Registrierung `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\W3SVC\Parameters\ConfigPollMilliSeconds ` festlegen. Starten Sie dann den W3WP-Prozess neu. Informationen zu dieser Einstellung finden Sie unter [Common registry keys that are used by many parts of IIS](/troubleshoot/iis/use-registry-keys#registry-keys-that-apply-to-iis-worker-process-w3wp) (Allgemeine Registrierungsschlüssel, die von vielen Teilen von IIS verwendet werden).
- Erhöhen Sie die Häufigkeit des Abrufintervalls für Dateiänderungsbenachrichtigungen, um das Volumen zu verringern.
    - Aktualisieren Sie das Abrufintervall für den W3WP-Workerprozess je nach Anforderung auf einen höheren Wert (z. B. 10 Minuten oder 30 Minuten). Legen Sie `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\W3SVC\Parameters\ConfigPollMilliSeconds ` [in Ihrer Registrierung](/troubleshoot/iis/use-registry-keys#registry-keys-that-apply-to-iis-worker-process-w3wp) fest, und starten Sie den W3WP-Prozess neu.
- Wenn das zugeordnete physische Verzeichnis Ihrer Website eine geschachtelte Verzeichnisstruktur hat, können Sie versuchen, den Bereich der Dateiänderungsbenachrichtigung einzuschränken, um das Benachrichtigungsvolumen zu verringern. IIS verwendet standardmäßig die Konfiguration aus „Web.config“-Dateien in dem physischen Verzeichnis, dem das virtuelle Verzeichnis zugeordnet ist, sowie in allen untergeordneten Verzeichnissen in diesem physischen Verzeichnis. Wenn Sie keine „Web.config“-Dateien in untergeordneten Verzeichnissen verwenden möchten, geben Sie für das „allowSubDirConfig“-Attribut im virtuellen Verzeichnis den Wert „false“ an. Weitere Informationen finden Sie [hier](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories). 
    - Legen Sie in „Web.config“ die Einstellung „allowSubDirConfig“ für das virtuelle IIS-Verzeichnis auf *false* fest, um zugeordnete physische Unterverzeichnisse aus dem Bereich auszuschließen.  

## <a name="how-to-create-an-alert-if-a-file-share-is-throttled"></a>Erstellen einer Warnung, wenn eine Dateifreigabe gedrosselt ist

1. Navigieren Sie im **Azure-Portal** zu Ihrem **Speicherkonto**.
2. Klicken Sie im Abschnitt **Überwachung** auf **Warnungen** und anschließend auf **+ Neue Warnungsregel**.
3. Klicken Sie auf **Ressource bearbeiten**, wählen Sie unter **Ressourcentyp** den Ressourcentyp für das Speicherkonto aus, und klicken Sie anschließend auf **Fertig**. Wenn der Name des Speicherkontos beispielsweise `contoso` ist, wählen Sie die `contoso/file`-Ressource aus.
4. Klicken Sie auf **Bedingung hinzufügen**, um eine Bedingung hinzuzufügen.
5. Wählen Sie in der daraufhin angezeigten Liste der für das Speicherkonto unterstützten Signale die Metrik **Transaktionen** aus.
6. Klicken Sie auf dem Blatt **Signallogik konfigurieren** auf das Dropdownmenü **Dimensionsname**, und wählen Sie **Antworttyp** aus.
7. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie die entsprechenden Antworttypen für die Dateifreigabe aus.

    Wählen Sie für Standard-Dateifreigaben die folgenden Antworttypen aus:

    - SuccessWithThrottling
    - SuccessWithShareIopsThrottling
    - ClientShareIopsThrottlingError

    Wählen Sie für Premium-Dateifreigaben die folgenden Antworttypen aus:

    - SuccessWithShareEgressThrottling
    - SuccessWithShareIngressThrottling
    - SuccessWithShareIopsThrottling
    - ClientShareEgressThrottlingError
    - ClientShareIngressThrottlingError
    - ClientShareIopsThrottlingError

   > [!NOTE]
   > Wenn die Antworttypen nicht in der Dropdownliste **Dimensionswerte** aufgeführt werden, bedeutet dies, dass die Ressource nicht gedrosselt wurde. Wählen Sie zum Hinzufügen der Dimensionswerte neben der Dropdownliste **Dimensionswerte** die Option **Benutzerdefinierten Wert hinzufügen** aus, geben Sie den Typ ein (z. B. **SuccessWithThrottling**), und wählen Sie **OK** aus. Wiederholen Sie diese Schritte zum Hinzufügen aller betreffenden Antworttypen für die Dateifreigabe.

8. Klicken Sie bei **Premium-Dateifreigaben** auf die Dropdownliste **Dimensionsname**, und wählen Sie **Dateifreigabe** aus. Springen Sie bei **Standarddateifreigaben** zu **Schritt #10**.

   > [!NOTE]
   > Wenn die Dateifreigabe eine Standarddateifreigabe ist, wird/werden die Dateifreigabe(n) in der Dimension **Dateifreigabe** nicht aufgeführt, weil Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind. Die Drosselung von Warnungen für Standarddateifreigaben wird ausgelöst, wenn eine Dateifreigabe im Speicherkonto gedrosselt wird und die Warnung nicht identifiziert, welche Freigabe gedrosselt wurde. Da Metriken pro Freigabe für Standarddateifreigaben nicht verfügbar sind, wird empfohlen, dass es eine einzige Dateifreigabe pro Speicherkonto gibt.

9. Klicken Sie auf die Dropdownliste **Dimensionswerte**, und wählen Sie die Dateifreigaben aus, für die Sie eine Warnung einrichten möchten.
10. Definieren Sie die **Warnungsparameter** (Schwellenwert, Operator, Aggregationsgranularität und Häufigkeit der Auswertung), und klicken Sie auf **Fertig**.

    > [!TIP]
    > Bei Verwendung eines statischen Schwellenwerts kann das Metrikdiagramm bei der Ermittlung eines sinnvollen Schwellenwerts helfen, wenn die Dateifreigabe gerade gedrosselt wird. Falls Sie einen dynamischen Schwellenwert verwenden, zeigt das Metrikdiagramm die berechneten Schwellenwerte basierend auf aktuellen Daten an.

11. Klicken Sie auf **Aktionsgruppe hinzufügen**, und fügen Sie der Warnung eine **Aktionsgruppe** (E-Mail, SMS usw.) hinzu, indem Sie eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.
12. Geben Sie die **Warnungsdetails** wie **Warnungsregelname**, **Beschreibung** und **Schweregrad** ein.
13. Klicken Sie auf **Warnungsregel erstellen**, um die Warnung zu erstellen.

Weitere Informationen zum Konfigurieren von Warnungen in Azure Monitor finden Sie unter [Übersicht über Warnungen in Microsoft Azure](../../azure-monitor/alerts/alerts-overview.md).

## <a name="how-to-create-alerts-if-a-premium-file-share-is-trending-toward-being-throttled"></a>Erstellen von Warnungen, wenn sich eine Premium-Dateifreigabe der Drosselung nähert

1. Wechseln Sie im Azure-Portal zu Ihrem Speicherkonto.
2. Wählen Sie im Abschnitt **Überwachung** die Option **Warnungen** und dann **Neue Warnungsregel** aus.
3. Wählen Sie nacheinander **Ressource bearbeiten**, den **Dateiressourcentyp** für das Speicherkonto und dann **Fertig** aus. Wenn der Name des Speicherkontos beispielsweise *contoso* lautet, wählen Sie die Ressource „contoso/datei“ aus.
4. Wählen Sie **Bedingung auswählen** aus, um eine Bedingung hinzuzufügen.
5. Wählen Sie in der Liste der für das Speicherkonto unterstützten Signale die Metrik **Ausgehend** aus.

   > [!NOTE]
   > Sie müssen drei separate Warnungen erstellen, um benachrichtigt zu werden, wenn die Werte für eingehenden Datenverkehr, ausgehenden Datenverkehr oder Transaktionen die von Ihnen festgelegten Schwellenwerte überschreiten. Dies liegt daran, dass eine Warnung nur dann ausgelöst wird, wenn alle Bedingungen erfüllt sind. Wenn Sie beispielsweise alle Bedingungen in eine einzige Warnung einfügen, werden Sie nur benachrichtigt, wenn die Schwellenwerte für „Eingehend“, „Ausgehend“ und „Transaktionen“ überschritten werden.

6. Scrollen Sie nach unten. Wählen Sie in der Dropdownliste **Dimensionsname** den Eintrag **Dateifreigabe** aus.
7. Wählen Sie in der Dropdownliste **Dimensionswerte** die Dateifreigabe oder Freigaben aus, für die Sie eine Warnung einrichten möchten.
8. Definieren Sie die Warnungsparameter, indem Sie in den Dropdownlisten **Operator**, **Schwellenwert**, **Aggregationsgranularität** und **Häufigkeit der Auswertung** die gewünschten Werte und dann **Fertig** auswählen.

   Die Metriken „Eingehend“, „Ausgehend“ und „Transaktionen“ werden in Minuten ausgedrückt, obwohl die Abrechnung für „Eingehend“, „Ausgehend“ und „E/A“ nach Sekunden erfolgt. Wenn Ihr bereitgestellter ausgehender Datenverkehr beispielsweise 90&nbsp;MiB/Sekunde ist und Ihr Schwellenwert 80&nbsp;% des bereitgestellten ausgehenden Datenverkehrs sein soll, wählen Sie die folgenden Warnungsparameter aus: 
   - Für **Fehlerschwellenwert**: *75497472* 
   - Für **Operator**: *Größer als oder gleich*
   - Für **Aggregationstyp**: *Durchschnitt*
   
   Abhängig davon, wie oft Ihre Warnung gesendet werden soll, können Sie auch Werte für **Aggregationsgranularität** und **Auswertungshäufigkeit** auswählen. Wenn Sie beispielsweise möchten, dass Ihre Warnung den durchschnittlichen eingehenden Datenverkehr im Zeitraum von einer Stunde untersucht und Ihre Warnungsregel stündlich ausgeführt werden soll, wählen Sie Folgendes aus:
   - Für **Aggregationsgranularität**: *1 Stunde*
   - Für **Häufigkeit der Auswertung**: *1 Stunde*

9. Wählen Sie **Aktionsgruppe hinzufügen** aus, und fügen Sie der Warnung eine Aktionsgruppe (z. B. E-Mail oder SMS) hinzu, indem Sie entweder eine vorhandene Aktionsgruppe auswählen oder eine neue erstellen.
10. Geben Sie die Warnungsdetails ein, z. B. **Warnungsregelname**, **Beschreibung** und **Schweregrad**.
11. Wählen Sie **Benachrichtigungsregel erstellen** aus, um die Benachrichtigung zu erstellen.

    > [!NOTE]
    > - Wenn Sie benachrichtigt werden möchten, dass Ihre Premium-Dateifreigabe *aufgrund von bereitgestelltem eingehendem Datenverkehr* bald gedrosselt wird, führen Sie die vorstehenden Anweisungen aus, aber mit der folgenden Änderung:
    >    - Wählen Sie in Schritt 5 die Metrik **Eingehend** statt **Ausgehend** aus.
    >
    > - Wenn Sie benachrichtigt werden möchten, dass Ihre Premium-Dateifreigabe *aufgrund von bereitgestelltem IOPS* bald gedrosselt wird, führen Sie die vorstehenden Anweisungen aus, aber mit der folgenden Änderung:
    >    - Wählen Sie in Schritt 5 die Metrik **Transaktionen** statt **Ausgehend** aus.
    >    - Außerdem ist in Schritt 10 die einzige Option für den **Aggregationstyp** der *Gesamtwert*. Daher ist der Schwellenwert abhängig von Ihrer ausgewählten Aggregationsgranularität. Wenn Sie z. B. als Schwellenwert 80&nbsp;% des bereitgestellten IOPS-Grundwerts verwenden möchten und für *Aggregationsgranularität* den Wert **1 Stunde** ausgewählt haben, wäre Ihr **Schwellenwert** Ihr IOPS-Grundwert (in Byte) gleich &times;&nbsp;0,8 &times;&nbsp;3.600. 

Weitere Informationen zum Konfigurieren von Warnungen in Azure Monitor finden Sie unter [Übersicht über Warnungen in Microsoft Azure](../../azure-monitor/alerts/alerts-overview.md).

## <a name="see-also"></a>Weitere Informationen
- [Behandeln von Azure Files-Problemen unter Windows](storage-troubleshoot-windows-file-connection-problems.md)  
- [Behandeln von Azure Files-Problemen unter Linux](storage-troubleshoot-linux-file-connection-problems.md)  
- [Häufig gestellte Fragen (FAQ) zu Azure Files](storage-files-faq.md)
