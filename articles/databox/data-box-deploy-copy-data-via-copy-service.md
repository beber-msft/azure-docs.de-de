---
title: 'Tutorial: Kopieren auf Ihr Gerät mithilfe des Datenkopierdiensts'
titleSuffix: Azure Data Box
description: In diesem Tutorial erfahren Sie, wie Sie mit dem Datenkopierdienst Daten auf Ihr Azure Data Box-Gerät kopieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 11/08/2021
ms.author: alkohli
ms.openlocfilehash: 0fd3a8ed38d55580cfe56e6ee4501616771a2b48
ms.sourcegitcommit: 838413a8fc8cd53581973472b7832d87c58e3d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2021
ms.locfileid: "132135756"
---
# <a name="tutorial-use-the-data-copy-service-to-copy-data-into-azure-data-box-preview"></a>Tutorial: Kopieren von Daten in Azure Data Box (Vorschauversion) mithilfe des Datenkopierdiensts

In diesem Tutorial erfahren Sie, wie Sie Daten mithilfe des Datenkopierdiensts ohne Zwischenhost erfassen. Der Datenkopierdienst wird lokal in Microsoft Azure Data Box ausgeführt, stellt eine SMB-Verbindung mit Ihrem NAS-Gerät (Network Attached Storage) her und kopiert Daten in Data Box.

Der Datenkopierdienst eignet sich für Folgendes:

- NAS-Umgebungen, in denen unter Umständen keine Zwischenhosts verfügbar sind
- Kleine Dateien, bei denen das Erfassen und Hochladen von Daten mehrere Wochen dauert. Der Datenkopierdienst verkürzt die Erfassungs- und Uploadzeit für kleine Dateien erheblich.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Kopieren von Daten auf die Data Box

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben das folgende Tutorial abgeschlossen: [Tutorial: Verkabeln und Herstellen einer Verbindung mit der Azure Data Box](data-box-deploy-set-up.md).
2. Sie haben Ihr Data Box-Gerät erhalten, und die Bestellung wird im Portal mit dem Status **Übermittelt** angezeigt.
3. Sie verfügen über die Anmeldeinformationen des NAS-Quellgeräts, mit dem Sie eine Verbindung für den Datenkopiervorgang herstellen.
4. Sie verfügen über eine Verbindung mit einem Hochgeschwindigkeitsnetzwerk. Mindestens eine 10-GbE-Verbindung (10-Gigabit Ethernet) wird dringend empfohlen. Falls keine 10-GbE-Verbindung verfügbar ist, können Sie eine 1-GbE-Datenverbindung verwenden, die Geschwindigkeit der Kopiervorgänge wird dadurch jedoch beeinträchtigt.

## <a name="copy-data-to-data-box"></a>Kopieren von Daten auf die Data Box

Nachdem Sie die Verbindung mit dem NAS-Gerät hergestellt haben, kopieren Sie im nächsten Schritt Ihre Daten. Bevor Sie mit dem Kopieren der Daten beginnen, sollten Sie folgende Aspekte beachten:

* Stellen Sie beim Kopieren der Daten sicher, dass für die Datengröße die Größenbeschränkungen eingehalten werden, die im Artikel zu den [Grenzwerten für Azure Storage und Data Box](data-box-limits.md) beschrieben sind.

* Falls von Data Box hochgeladene Daten gleichzeitig von anderen Anwendungen außerhalb von Data Box hochgeladen werden, kann dies zu Fehlern bei Uploadaufträgen und zu Datenbeschädigungen führen.

* Wenn die Daten geändert werden, während sie gerade vom Datenkopierdienst gelesen werden, kommt es ggf. zu Fehlern oder Datenbeschädigungen.

> [!IMPORTANT]
> Bevor Sie bestätigen können, dass Data Box Ihre Daten nach Azure Storage übertragen hat, müssen Sie sicherstellen, dass Sie über eine Kopie der Quelldaten verfügen.

Sie müssen einen Auftrag erstellen, um Daten mit dem Datenkopierdienst zu kopieren:

1. Navigieren Sie auf der lokalen Webbenutzeroberfläche Ihres Data Box-Geräts zu **Verwalten** > **Daten kopieren**.
2. Wählen Sie auf der Seite **Daten kopieren** die Option **Erstellen**.

    ![Wählen von „Erstellen“ auf der Seite „Daten kopieren“](media/data-box-deploy-copy-data-via-copy-service/click-create.png)

3. Füllen Sie im Dialogfeld **Auftrag konfigurieren und starten** die folgenden Felder aus:
    
    |Feld                          |Wert    |
    |-------------------------------|---------|
    |**Auftragsname**                       |Eindeutiger Name für den Auftrag (weniger als 230 Zeichen). Auftragsnamen dürfen keines der folgenden Zeichen enthalten: \<, \>, \|, \?, \*, \\, \:, \/ und \\\.         |
    |**Quellpfad**                |Geben Sie den SMB-Pfad der Datenquelle im Format `\\<ServerIPAddress>\<ShareName>` oder `\\<ServerName>\<ShareName>` an.        |
    |**Benutzername**                       |Benutzername im Format `\\<DomainName><UserName>` für den Zugriff auf die Datenquelle. Wenn ein lokaler Administrator eine Verbindung herstellt, benötigt er explizite Sicherheitsberechtigungen. Klicken Sie mit der rechten Maustaste auf den Ordner, wählen Sie **Eigenschaften** aus und dann **Sicherheit**. Damit wird der lokale Administrator auf der Registerkarte **Sicherheit** hinzugefügt.       |
    |**Kennwort**                       |Kennwort für den Zugriff auf die Datenquelle.           |
    |**Zielspeicherkonto**    |Wählen Sie in der Liste das Zielspeicherkonto aus, in das die Daten hochgeladen werden sollen.         |
    |**Zieltyp**       |Wählen Sie den Zielspeichertyp in der Liste aus: **Blockblob**, **Seitenblob** oder **Azure Files**.        |
    |**Zielcontainer/-freigabe**    |Geben Sie den Namen des Containers oder der Freigabe an, in den bzw. die Daten in Ihrem Zielspeicherkonto hochgeladen werden sollen. Hierbei kann es sich um einen Freigabenamen oder um einen Containernamen handeln. Verwenden Sie beispielsweise `myshare` oder `mycontainer`. Der Name kann auch das Format `sharename\directory_name` oder `containername\virtual_directory_name` haben.        |
    |**Mit Muster übereinstimmende Dateien kopieren**    | Sie können das gewünschte Dateinamenmuster eingeben. Dabei haben Sie zwei Optionen:<ul><li>**Verwenden von Platzhalterausdrücken:** In Platzhalterausdrücken werden nur `*` und `?` unterstützt. Beispiel: Der Ausdruck `*.vhd` entspricht allen Dateien mit der Erweiterung `.vhd`. Analog dazu entspricht `*.dl?` allen Dateien, die entweder die Erweiterung `.dl` haben oder mit `.dl` beginnen, etwa `.dll`. `*foo` entspricht allen Dateien, deren Name mit `foo` endet.<br>Platzhalterausdrücke können direkt in das Feld eingegeben werden. Der in das Feld eingegebene Wert wird standardmäßig als Platzhalterausdruck behandelt.</li><li>**Verwenden von regulären Ausdrücken:** POSIX-basierte reguläre Ausdrücke werden unterstützt. Der reguläre Ausdruck `.*\.vhd` entspricht beispielsweise allen Dateien mit der Erweiterung `.vhd`. Wenn Sie regulärer Ausdrücke verwenden möchten, geben Sie das Muster (`<pattern>`) direkt als `regex(<pattern>)` an. Weitere Informationen zu regulären Ausdrücken finden Sie unter [Sprachelemente für reguläre Ausdrücke – Kurzübersicht](/dotnet/standard/base-types/regular-expression-language-quick-reference).</li><ul>|
    |**Dateioptimierung**              |Wenn dieses Feature aktiviert ist, werden Dateien, die kleiner als 1 MB sind, während der Erfassung gepackt. Dieses Packen beschleunigt das Kopieren kleiner Dateien. Es spart darüber hinaus viel Zeit, wenn die Anzahl von Dateien deutlich über der Anzahl von Verzeichnissen liegt.</br>Bei Verwendung der Dateioptimierung:<ul><li>Nach den Versandvorbereitungen können Sie eine [BOM-Datei herunterladen](data-box-logs.md#inspect-bom-during-prepare-to-ship), in der die ursprünglichen Dateinamen aufgeführt sind, um sicherzustellen, dass die richtigen Dateien kopiert werden.</li><li>Löschen Sie nicht die gepackten Dateien, deren Dateinamen mit „ADB_PACK_“ beginnen. Wenn Sie eine gepackte Datei löschen, wird die ursprüngliche Datei bei zukünftigen Datenkopiervorgängen nicht hochgeladen.</li><li>Kopieren Sie nicht die gleichen Dateien, die Sie mit dem Kopierdienst über andere Protokolle wie SMB, NFS oder REST-API kopieren. Die Verwendung verschiedener Protokolle kann zu Konflikten und Fehlern bei Datenuploads führen. </li></ul>    |
 
4. Wählen Sie **Starten** aus. Die Eingaben werden überprüft. Ist die Überprüfung erfolgreich, wird der Auftrag gestartet. Es dauert möglicherweise ein paar Minuten, bis der Auftrag gestartet wird.

    ![Starten eines Auftrags über das Dialogfeld „Auftrag konfigurieren und starten“](media/data-box-deploy-copy-data-via-copy-service/configure-and-start.png)

5. Ein Auftrag mit den angegebenen Einstellungen wird erstellt. Sie können einen Auftrag anhalten, fortsetzen, abbrechen oder neu starten. Aktivieren Sie das Kontrollkästchen neben dem Auftragsnamen, und wählen Sie die entsprechende Schaltfläche.

    ![Verwalten eines Auftrags über die Seite „Daten kopieren“](media/data-box-deploy-copy-data-via-copy-service/select-job.png)
    
    - Sie können einen Auftrag anhalten, wenn er die Ressourcen des NAS-Geräts während Spitzenzeiten beeinträchtigt:

        ![Anhalten eines Auftrags über die Seite „Daten kopieren“](media/data-box-deploy-copy-data-via-copy-service/pause-job.png)

        Sie können den Auftrag später fortsetzen, wenn die Auslastung geringer ist:

        ![Fortsetzen eines Auftrags über die Seite „Daten kopieren“](media/data-box-deploy-copy-data-via-copy-service/resume-job.png)

    - Ein Auftrag kann jederzeit abgebrochen werden:

        ![Abbrechen eines Auftrags über die Seite „Daten kopieren“](media/data-box-deploy-copy-data-via-copy-service/cancel-job.png)
        
        Beim Abbrechen eines Auftrags wird eine Bestätigung angefordert:

        ![Bestätigen eines Auftragsabbruchs](media/data-box-deploy-copy-data-via-copy-service/confirm-cancel-job.png)

        Wenn Sie einen Auftrag abbrechen, werden die bereits kopierten Daten nicht gelöscht. Setzen Sie das Gerät zurück, um die auf Ihr Data Box-Gerät kopierten Daten zu löschen.

        ![Zurücksetzen eines Geräts](media/data-box-deploy-copy-data-via-copy-service/reset-device.png)

        >[!NOTE]
        > Beim Abbrechen oder Anhalten eines Auftrags werden große Dateien unter Umständen nur teilweise kopiert. Diese teilweise kopierten Dateien werden im gleichen Zustand in Azure hochgeladen. Vergewissern Sie sich, dass Ihre Dateien ordnungsgemäß kopiert wurden, wenn Sie einen Auftrag abbrechen oder anhalten. Sehen Sie sich zur Überprüfung der Dateien die SMB-Freigaben an, oder laden Sie die BOM-Datei herunter.

    - Sie können einen Auftrag neu starten, wenn dieser aufgrund eines vorübergehenden Fehlers (etwa aufgrund einer Netzwerkstörung) nicht erfolgreich war. Aufträge, die einen Endzustand (etwa **Erfolgreich** oder **Mit Fehlern abgeschlossen**) erreicht haben, können nicht erneut gestartet werden. Auftragsfehler können durch Probleme mit der Dateibenennung oder Dateigröße verursacht werden. Solche Fehler werden zwar protokolliert, der Auftrag kann jedoch nicht erneut gestartet werden, nachdem er abgeschlossen wurde.

        ![Neustarten eines nicht erfolgreichen Auftrags](media/data-box-deploy-copy-data-via-copy-service/restart-failed-job.png)

        Sollte ein Fehler aufgetreten sein und der Auftrag nicht erneut gestartet werden können, laden Sie die Fehlerprotokolle herunter, und sehen Sie sich den Fehler in den Protokolldateien an. Erstellen Sie einen neuen Auftrag zum Kopieren der Dateien, wenn Sie das Problem behoben haben. Alternativ können Sie die [Dateien über SMB kopieren](data-box-deploy-copy-data.md).
    
    - In diesem Release können Sie keine Aufträge löschen.
    
    - Sie können eine unbegrenzte Anzahl von Aufträgen erstellen, jedoch immer nur maximal zehn Aufträge parallel ausführen.
    - Bei aktivierter **Dateioptimierung** werden die kleinen Dateien im Rahmen der Erfassung gepackt, um die Kopierleistung zu verbessern. In diesen Fällen wird eine gepackte Datei angezeigt (mit einer GUID als Dateiname). Löschen Sie diese Datei nicht. Sie wird während des Uploads entpackt.

6. Während der Auftragsausführung gilt für die Seite **Daten kopieren** Folgendes:

    - In der Spalte **Status** wird der Status des Kopierauftrags angezeigt. Der Status kann Folgendes sein:
        - **Wird ausgeführt**
        - **Fehler**
        - **Erfolgreich**
        - **Wird angehalten**
        - **Angehalten**
        - **Wird abgebrochen**
        - **Canceled**
        - **Mit Fehlern abgeschlossen**
    - In der Spalte **Dateien** werden die Anzahl und die Gesamtgröße der kopierten Dateien angezeigt.
    - In der Spalte **Verarbeitet** werden Anzahl und Gesamtgröße der verarbeiteten Dateien angezeigt.
    - Wählen Sie in der Spalte **Auftragsdetails** die Option **Anzeigen**, um die Auftragsdetails anzuzeigen.
    - Sollten während des Kopiervorgangs Fehler auftreten (siehe Spalte **Anzahl von Fehlern**), navigieren Sie zur Spalte **Fehlerprotokoll**, und laden Sie zur Problembehandlung die Fehlerprotokolle herunter.

Warten Sie, bis der Kopierauftrag abgeschlossen ist. Da einige Fehler nur auf der Seite **Verbindung herstellen und Daten kopieren** protokolliert werden, vergewissern Sie sich, dass der Kopierauftrag fehlerfrei abgeschlossen wurde, bevor Sie mit dem nächsten Schritt fortfahren.

![Keine Fehler auf der Seite „Verbindung herstellen und Daten kopieren“](media/data-box-deploy-copy-data-via-copy-service/verify-no-errors-on-connect-and-copy.png)

Um die Datenintegrität zu gewährleisten, wird inline eine Prüfsumme berechnet, während die Daten kopiert werden. Wählen Sie nach Abschluss des Kopiervorgangs die Option **Dashboard anzeigen**, um den belegten Speicherplatz und den freien Speicherplatz auf Ihrem Gerät zu überprüfen.
    
![Überprüfen des freien und belegten Speicherplatzes im Dashboard](media/data-box-deploy-copy-data-via-copy-service/verify-used-space-dashboard.png)

Nach Abschluss des Kopierauftrags können Sie **Für Versand vorbereiten** auswählen.

>[!NOTE]
> **Für Versand vorbereiten** kann nicht ausgeführt werden, solange noch Kopieraufträge ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihr Data Box-Gerät zurück an Microsoft senden.

> [!div class="nextstepaction"]
> [Tutorial: Zurücksenden der Azure Data Box und Überprüfen des Datenuploads in Azure](./data-box-deploy-picked-up.md)