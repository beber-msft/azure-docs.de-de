---
title: Migrieren von lokalen SQL Server Integration Services-Aufträgen (SSIS) zu Azure Data Factory
description: In diesem Artikel wird beschrieben, wie Sie SQL Server Integration Services-Aufträge (SSIS) mithilfe von SQL Server Management Studio zu Azure Data Factory-Pipelines, -Aktivitäten oder -Trigger migrieren.
author: chugugrace
ms.author: chugu
ms.service: data-factory
ms.subservice: integration-services
ms.topic: conceptual
ms.date: 10/22/2021
ms.openlocfilehash: d306f5853bee8dcb01cd65d53060b1fa25013176
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2021
ms.locfileid: "131842110"
---
# <a name="migrate-sql-server-agent-jobs-to-adf-with-ssms"></a>Migrieren von SQL Server-Agent-Aufträgen zu ADF mit SSMS

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Beim [Migrieren von lokalen SQL Server Integration Services-Workloads (SSIS) zu SSIS in Azure Data Factory (ADF)](scenario-ssis-migration-overview.md) können Sie nach der Migration von SSIS-Paketen mithilfe des **Assistenten für die SSIS-Auftragsmigration** in SQL Server Management Studio (SSMS) eine Batchmigration von Aufträgen des SQL Server-Agents mit dem Auftragsschritttyp SQL Server Integration Services Package zu Pipelines, Aktivitäten oder Zeitplantrigger in ADF ausführen.

Mit dem **Assistenten für die SSIS-Auftragsmigration** können Sie generell folgende Aktionen für ausgewählte Aufträge des SQL-Agents mit anwendbaren Auftragsschritttypen ausführen:

- Zuordnen des lokalen Speicherorts von SSIS-Paketen zum Zielspeicherort nach der Migration, auf den SSIS in ADF zugreifen kann.
    > [!NOTE]
    > Es werden nur Paketspeicherorte im Dateisystem unterstützt.
- Migrieren von anwendbaren Aufträgen mit anwendbaren Auftragsschritten zu den entsprechenden ADF-Ressourcen:

|Objekt des SQL-Agent-Auftrags  |ADF-Ressource  |Notizen|
|---------|---------|---------|
|SQL-Agent-Auftrag|pipeline     |Der Name der Pipeline lautet *Generiert für \<job name>* . <br> <br> Folgende integrierte Agent-Aufträge sind nicht anwendbar: <li> Wartungsauftrag für SSIS-Server <li> syspolicy_purge_history <li> collection_set_* <li> mdw_purge_data_* <li> sysutility_*|
|SSIS-Auftragsschritt|Aktivität „SSIS-Paket ausführen“|<li> Der Name der Aktivität lautet \<step name>. <li> Das im Auftragsschritt verwendete Proxykonto wird als Windows-Authentifizierung dieser Aktivität migriert. <li> *Ausführungsoptionen*, mit Ausnahme der im Auftragsschritt definierten Option *32-Bit-Runtime verwenden*, werden bei der Migration ignoriert. <li> Die im Auftragsschritt definierte *Überprüfung* wird bei der Migration ignoriert.|
|schedule      |Zeitplantrigger        |Der Name des Zeitplantriggers lautet *Generiert für \<schedule name>* . <br> <br> Die folgenden Optionen im Auftragszeitplan des SQL-Agents werden bei der Migration ignoriert: <li> Intervall der zweiten Ebene <li> *Automatisch starten, wenn der SQL Server-Agent startet* <li> *Starten, wenn sich die CPUs im Leerlauf befinden* <li> *Wochentag* und *Wochenendtag* &gt;Zeitzone&lt; <br> Nach der Migration des Auftragszeitplans des SQL-Agents zum ADF-Zeitplantrigger sind folgende Unterschiede vorhanden: <li> Die nachfolgende Ausführung des ADF-Zeitplantriggers erfolgt unabhängig vom Ausführungszustand der zuvor ausgelösten Ausführung. <li> Die Wiederholungskonfiguration für den ADF-Zeitplantrigger unterscheidet sich von der täglichen Häufigkeit im SQL-Agent-Auftrag.|

- Generieren von Azure Resource Manager-Vorlagen (ARM) im lokalen Ausgabeordner, die direkt oder später manuell in Data Factory bereitgestellt werden. Weitere Informationen zu den Azure Resource Manager-Vorlagen in ADF finden Sie unter [Microsoft.DataFactory-Ressourcentypen](/azure/templates/microsoft.datafactory/allversions).

## <a name="prerequisites"></a>Voraussetzungen

Die in diesem Artikel beschriebene Funktion erfordert Version 18.5 oder höher von SQL Server Management Studio. Die neueste Version von SSMS können Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) abrufen.

## <a name="migrate-ssis-jobs-to-adf"></a>Migrieren von SSIS-Aufträgen zu ADF

1. Klicken Sie in SSMS im Objekt-Explorer auf „SQL Server-Agent“ und dann auf „Aufträge“. Klicken Sie anschließend mit der rechten Maustaste, und wählen Sie die Option **SSIS-Aufträge zu ADF migrieren** aus.
:::image type="content" source="media/how-to-migrate-ssis-job-ssms/menu.png" alt-text="Screenshot des SQL Server Management Studio-Objekt-Explorers für die Auswahl von „Aufträge“ und dann „SSIS-Aufträge zu ADF migrieren“":::

1. Melden Sie sich bei Azure an, und wählen Sie ein Azure-Abonnement, eine Data Factory-Instanz und eine Integration Runtime aus. Die Auswahl eines Azure Storage-Kontos ist optional. Dieses Konto wird im Schritt der Zuordnung des Paketspeicherorts verwendet, wenn die SSIS-Aufträge, die migriert werden sollen, über SSIS-Dateisystempakete verfügen.
:::image type="content" source="media/how-to-migrate-ssis-job-ssms/step1.png" alt-text="Menü":::

1. Ordnen Sie die Pfade der SSIS-Pakete und Konfigurationsdateien in SSIS-Aufträgen den Zielpfaden zu, über die die migrierten Pipelines zugreifen können. Im Zuordnungsschritt können Sie folgende Aktionen ausführen:

    1. Sie können einen Quellordner auswählen und auf **Zuordnung hinzufügen** klicken.
    1. Sie können den Pfad zum Quellordner aktualisieren. Gültige Pfade sind Ordnerpfade oder übergeordnete Ordnerpfade von Paketen.
    1. Sie können den Pfad des Zielordners aktualisieren. Die Standardeinstellung ist der relative Pfad zum Azure Storage-Standardkonto, das in Schritt 2 ausgewählt wird.
    1. Sie können eine ausgewählte Zuordnung über **Zuordnung löschen** löschen.
:::image type="content" source="media/how-to-migrate-ssis-job-ssms/step2.png" alt-text="Screenshot: Seite für die Zuordnung von SSIS-Paketen und Konfigurationen, auf der eine Zuordnung hinzugefügt werden kann.":::
:::image type="content" source="media/how-to-migrate-ssis-job-ssms/step2-1.png" alt-text="Screenshot: Seite für die Zuordnung von SSIS-Paketen und Konfigurationen, auf der die Pfade der Quell- und Zielordner aktualisiert werden können.":::

1. Wählen Sie die anwendbaren Aufträge aus, die migriert werden sollen, und konfigurieren Sie die Einstellungen der entsprechenden Aktivität *SSIS-Paket ausführen*.

    - Die *Standardeinstellung* gilt standardmäßig für alle ausgewählten Schritte. Weitere Informationen zu den einzelnen Eigenschaften finden Sie auf der Registerkarte *Einstellungen* der [Aktivität „SSIS-Paket ausführen“](how-to-invoke-ssis-package-ssis-activity.md), wenn der Paketspeicherort auf *Dateisystem (Paket)* festgelegt ist.
    :::image type="content" source="media/how-to-migrate-ssis-job-ssms/step3-1.png" alt-text="Screenshot der Seite „SSIS-Aufträge auswählen“ für das Konfigurieren der Einstellungen der entsprechenden ausgeführten SSIS-Paketaktivität":::
    - Über die Einstellung *Schritt* können Sie die Einstellung für einen ausgewählten Schritt konfigurieren.
        
        Das Kontrollkästchen bei **Standardeinstellung anwenden** ist standardmäßig aktiviert. Deaktivieren Sie das Kontrollkästchen, wenn Sie nur die Einstellung für den ausgewählten Schritt konfigurieren möchten.  
        Weitere Informationen zu anderen Eigenschaften finden Sie auf der Registerkarte *Einstellungen* der [Aktivität „SSIS-Paket ausführen“](how-to-invoke-ssis-package-ssis-activity.md), wenn der Paketspeicherort auf *Dateisystem (Paket)* festgelegt ist.
    :::image type="content" source="media/how-to-migrate-ssis-job-ssms/step3-2.png" alt-text="Screenshot der Seite „SSIS-Aufträge auswählen“ für das Anwenden der Standardeinstellungen":::

1. Generieren Sie eine ARM-Vorlage, und stellen Sie diese bereit.
    1. Wählen Sie den Ausgabepfad für die ARM-Vorlagen der migrierten ADF-Pipelines aus, oder geben Sie ihn ein. Sofern der Ordner noch nicht vorhanden ist, wird er automatisch erstellt.
    2. Aktivieren Sie das Kontrollkästchen bei **ARM-Vorlagen in Data Factory bereitstellen**.
        - Standardmäßig ist das Kontrollkästchen nicht aktiviert. Generierte ARM-Vorlagen können Sie später manuell bereitstellen.
        - Aktivieren Sie das Kontrollkästchen, um generierte ARM-Vorlagen direkt in Data Factory bereitzustellen.
    :::image type="content" source="media/how-to-migrate-ssis-job-ssms/step4.png" alt-text="Screenshot der Seite „Migration konfigurieren“ für das Auswählen oder Eingeben des Ausgabepfads für die ARM-Vorlagen der migrierten ADF-Pipelines und das Aktivieren der Option zum Bereitstellen von ARM-Vorlagen in Ihrer Data Factory":::

1. Führen Sie die Migration aus, und überprüfen Sie die Ergebnisse.
:::image type="content" source="media/how-to-migrate-ssis-job-ssms/step5.png" alt-text="Screenshot der Seite „Migrationsergebnisse“ mit dem Status der Migration":::

## <a name="next-steps"></a>Nächste Schritte

[Ausführen und Überwachen der Pipeline](how-to-invoke-ssis-package-ssis-activity.md)
