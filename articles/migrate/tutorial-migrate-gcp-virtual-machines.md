---
title: Ermitteln, Bewerten und Migrieren von GCP-VM-Instanzen (Google Cloud Platform) zu Azure
description: In diesem Artikel wird beschrieben, wie Sie virtuelle GCP-Computer mit Azure Migrate zu Azure migrieren.
author: deseelam
ms.author: deseelam
ms.manager: bsiva
ms.topic: tutorial
ms.date: 08/19/2020
ms.custom: MVC
ms.openlocfilehash: 7b826bc1d127f38e716bfcae302a055b941d9b4d
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132331203"
---
# <a name="discover-assess-and-migrate-google-cloud-platform-gcp-vms-to-azure"></a>Ermitteln, Bewerten und Migrieren von GCP-VMs (Google Cloud Platform) zu Azure

In diesem Tutorial erfahren Sie, wie Sie virtuelle GCP-Computer (Google Cloud Platform) mithilfe von Azure Migrate zu Azure-VMs migrieren: Serverbewertung und Azure Migrate: Servermigration“-Tools.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
>
> * Überprüfen der Voraussetzungen für die Migration.
> * Vorbereiten von Azure-Ressourcen mit Azure Migrate: Servermigration. Richten sie Berechtigungen für Ihr Azure-Konto und Ihre Ressourcen ein, um Azure Migrate verwenden zu können.
> * Vorbereiten der GCP-VM-Instanzen für die Migration
> * Fügen Sie das Azure Migrate- Servermigrationstool im Azure Migrate-Hub zu.
> * Richten Sie die Replikationsappliance ein, und stellen Sie den Konfigurationsserver bereit.
> * Installieren des Mobilitätsdiensts auf zu migrierenden GCP-VMs
> * Aktivieren Sie die Replikation für VMs.
> * Verfolgen und überwachen Sie den Replikationsstatus.
> * Führen Sie eine Testmigration aus, um sicherzustellen, dass alles wie erwartet funktioniert.
> * Durchführen einer vollständigen Migration zu Azure

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

## <a name="discover-and-assess"></a>Ermitteln und Bewerten

Bevor Sie zu Azure migrieren, empfehlen wir, dass Sie eine VM-Ermittlung und Migrationsbewertung durchführen. Diese Bewertung hilft bei der korrekten Größenbestimmung Ihrer GCP-VMs für die Migration zu Azure und bei der Abschätzung potenzieller Azure-Ausführungskosten.

Gehen Sie zum Erstellen einer Bewertung wie folgt vor:

1. Führen Sie die Schritte des [Tutorials](./tutorial-discover-gcp.md) aus, um Azure einzurichten und Ihre GCP-VMs auf eine Bewertung vorzubereiten. Beachten Sie dabei Folgendes:

    - Azure Migrate verwendet bei der Ermittlung von GCP-VM-Instanzen die Kennwortauthentifizierung. Die Kennwortauthentifizierung wird von GCP-Instanzen nicht standardmäßig unterstützt. Damit Sie eine Instanz ermitteln können, müssen Sie die Kennwortauthentifizierung aktivieren.
        - Lassen Sie für Windows-Computer WinRM-Port 5985 (HTTP) zu. Dadurch werden WMI-Remoteaufrufe ermöglicht.
        - Für Linux-Computer:
            1. Melden Sie sich bei jedem Linux-Computer an.
            2. Öffnen Sie die Datei „sshd_config“: vi /etc/ssh/sshd_config.
            3. Suchen Sie in der Datei die Zeile **PasswordAuthentication**, und ändern Sie den Wert in **yes**.
            4. Speichern Sie die Datei, und schließen Sie sie. Starten Sie den SSH-Dienst neu.
    - Wenn Sie einen Stammbenutzer zum Ermitteln Ihrer virtuellen Linux-Computer verwenden, stellen Sie sicher, dass die Stammanmeldung auf den virtuellen Computern zulässig ist.
        1. Melden Sie sich bei jedem Linux-Computer an.
        2. Öffnen Sie die Datei „sshd_config“: vi /etc/ssh/sshd_config.
        3. Suchen Sie in der Datei die Zeile **PermitRootLogin**, und ändern Sie den Wert in **yes**.
        4. Speichern Sie die Datei, und schließen Sie sie. Starten Sie den SSH-Dienst neu.

2. Führen Sie anschließend die Schritte dieses [Tutorials](./tutorial-assess-gcp.md) aus, um ein Azure Migrate-Projekt und eine Azure Migrate-Appliance einzurichten, um Ihre GCP-VMs zu ermitteln und zu bewerten.

Wir empfehlen Ihnen zwar, eine Bewertung auszuprobieren, aber die Durchführung einer Bewertung ist kein unbedingt erforderlicher Schritt, um VMs migrieren zu können.



## <a name="prerequisites"></a>Voraussetzungen

- Vergewissern Sie sich, dass auf den zu migrierenden GCP-VMs eine unterstützte Betriebssystemversion ausgeführt wird. GCP-VMs werden zum Zweck der Migration wie physische Computer behandelt. Überprüfen Sie die [unterstützten Betriebssysteme und Kernelversionen](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) für den Workflow der Migration physischer Server. Sie können Standardbefehle wie *hostnamectl* oder *uname -a* verwenden, um die Betriebssysteme und Kernelversionen für Ihre virtuellen Linux-Computer zu überprüfen.  Es wird empfohlen, eine Testmigration durchzuführen, um zu überprüfen, ob die VM erwartungsgemäß funktioniert, bevor Sie mit der tatsächlichen Migration fortfahren.
- Stellen Sie sicher, dass Ihre GCP-VMs die [unterstützten Konfigurationen](./migrate-support-matrix-physical-migration.md#physical-server-requirements) für die Migration zu Azure erfüllen.
- Stellen Sie sicher, dass die GCP-VMs, die Sie in Azure replizieren möchten, die [Anforderungen für Azure-VMs](./migrate-support-matrix-physical-migration.md#azure-vm-requirements) erfüllen.
- Auf den VMs sind einige Änderungen erforderlich, bevor Sie sie zu Azure migrieren.
    - Bei einigen Betriebssystemen führt Azure Migrate diese Änderungen automatisch durch.
    - Es ist wichtig, diese Änderungen vorzunehmen, bevor Sie mit der Migration beginnen. Wenn Sie den virtuellen Computer migrieren, bevor Sie die Änderung vorgenommen haben, wird der virtuelle Computer in Azure unter Umständen nicht gestartet.
Überprüfen Sie die erforderlichen Änderungen für [Windows](prepare-for-migration.md#windows-machines) und [Linux](prepare-for-migration.md#linux-machines).

### <a name="prepare-azure-resources-for-migration"></a>Vorbereiten von Azure-Ressourcen für die Migration

Vorbereiten von Azure für die Migration mit Azure Migrate: Servermigration“ kommunizieren kann.

**Aufgabe** | **Details**
--- | ---
**Erstellen eines Azure Migrate-Projekts** | Ihr Azure-Konto benötigt zum [Erstellen eines neuen Projekts](./create-manage-projects.md) Berechtigungen vom Typ „Mitwirkender“ oder „Besitzer“.
**Überprüfen der Berechtigungen für Ihr Azure-Konto** | Ihr Azure-Konto benötigt Berechtigungen zum Erstellen eines virtuellen Computers sowie zum Schreiben auf einen verwalteten Azure-Datenträger.

### <a name="assign-permissions-to-create-project"></a>Zuweisen von Berechtigungen für die Projekterstellung

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** nach dem relevanten Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Rolle zuzuweisen.

### <a name="assign-azure-account-permissions"></a>Zuweisen der Azure-Kontoberechtigungen

Weisen Sie dem Azure-Konto die Rolle „Mitwirkender für virtuelle Computer“ zu. Dadurch werden Berechtigungen für folgende Aktionen erteilt:

- Erstellen einer VM in der ausgewählten Ressourcengruppe
- Erstellen einer VM im ausgewählten virtuellen Netzwerk
- Schreiben auf einen verwalteten Azure-Datenträger

### <a name="create-an-azure-network"></a>Erstellen eines Azure-Netzwerks

Sie müssen ein virtuelles Azure-Netzwerk (VNET) [einrichten](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Wenn Sie in Azure replizieren, werden die virtuellen Azure-Computer, die erstellt werden, in das Azure-VNet eingebunden, das Sie beim Einrichten der Migration angeben.

## <a name="prepare-gcp-instances-for-migration"></a>Vorbereiten der GCP-Instanzen für die Migration

Sie müssen zur Vorbereitung der Migration von GCP zu Azure eine Replikationsappliance für die Migration vorbereiten und bereitstellen.

### <a name="prepare-a-machine-for-the-replication-appliance"></a>Vorbereiten eines Computers für die Replikationsappliance

Von der Azure Bei der Servermigration wird eine Replikationsappliance verwendet, um Computer in Azure zu replizieren. Die Replikationsappliance führt die hier angegebenen Komponenten aus.

- **Konfigurationsserver**: Der Konfigurationsserver koordiniert die Kommunikation zwischen den GCP-VMs und Azure und verwaltet die Datenreplikation.
- **Prozessserver** Der Prozessserver fungiert als Replikationsgateway. Er empfängt Replikationsdaten, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an ein Cachespeicherkonto in Azure.

Bereiten Sie die Bereitstellung der Appliance wie folgt vor:

- Richten Sie eine gesonderte GCP-VM für das Hosten der Replikationsappliance ein. Auf dieser Instanz muss Windows Server 2012 R2 oder Windows Server 2016 ausgeführt werden. [Überprüfen](./migrate-replication-appliance.md#appliance-requirements) Sie die Hardware-, Software- und Netzwerkanforderungen für die Appliance.
- Die Appliance sollte nicht auf einer zu replizierenden Quell-VM oder auf der Ermittlungs- und Bewertungsappliance von Azure Migrate installiert werden, die Sie unter Umständen bereits installiert haben. Sie sollte auf einem anderen virtuellen Computer bereitgestellt werden.
- Die zu migrierenden virtuellen GCP-Quellcomputer sollten über eine Sichtverbindung zur Replikationsappliance im Netzwerk verfügen. Konfigurieren Sie die erforderlichen Firewallregeln, um dies zu ermöglichen. Es wird empfohlen, die Replikationsappliance in demselben VPC-Netzwerk wie die zu migrierenden virtuellen Quellcomputer bereitzustellen. Wenn sich die Replikationsappliance in einem anderen VPC befinden muss, müssen die VPCs mittels VPC-Peering verbunden werden.
- Die virtuellen GCP-Quellcomputer kommunizieren mit der Replikationsappliance an den eingehenden Ports HTTPS 443 (Steuerkanalorchestrierung) und TCP 9443 (Datentransport) für die Replikationsverwaltung und die Replikationsdatenübertragung. Die Replikationsappliance wiederum orchestriert und sendet Replikationsdaten an Azure über den ausgehenden Port HTTPS 443. Um diese Regeln zu konfigurieren, bearbeiten Sie die Eingangs-/Ausgangsregeln der Sicherheitsgruppe mit den entsprechenden Ports und Quell-IP-Informationen.

   ![GCP-Firewallregeln](./media/tutorial-migrate-gcp-virtual-machines/gcp-firewall-rules.png)


   ![Bearbeiten von Firewallregeln](./media/tutorial-migrate-gcp-virtual-machines/gcp-edit-firewall-rule.png)

- Die Replikationsappliance verwendet MySQL. Überprüfen Sie die [Optionen](migrate-replication-appliance.md#mysql-installation) für die Installation von MySQL auf der Appliance.
- Überprüfen Sie die Azure-URLs, die die Replikationsappliance für den Zugriff auf [öffentliche](migrate-replication-appliance.md#url-access) und [behördliche](migrate-replication-appliance.md#azure-government-url-access) Clouds benötigt.

## <a name="set-up-the-replication-appliance"></a>Einrichten der Replikationsappliance

Der erste Schritt bei der Migration besteht darin, die Replikationsappliance einzurichten. Sie müssen zum Einrichten der Appliance für die Migration von GCP-VMs die Installationsdatei für die Appliance herunterladen und diese dann auf dem [vorbereiteten virtuellen Computer](#prepare-a-machine-for-the-replication-appliance) ausführen.

### <a name="download-the-replication-appliance-installer"></a>Herunterladen des Installationsprogramms für die Replikationsappliance

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Ermitteln**.

    ![VMs ermitteln](./media/tutorial-migrate-physical-virtual-machines/migrate-discover.png)

2. Klicken Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** auf **Nicht virtualisiert/Andere**.
3. Wählen Sie unter **Zielregion** die Azure-Region aus, zu der Sie die Computer migrieren möchten.
4. Wählen Sie **Bestätigen Sie, dass die Zielregion für die Migration \<region-name\> lautet.** aus.
5. Klicken Sie auf **Ressourcen erstellen**. Daraufhin wird im Hintergrund ein Azure Site Recovery-Tresor erstellt.
    - Falls Sie die Migration bereits mit dem Tool für die Azure Migrate-Servermigration eingerichtet haben, kann die Zieloption nicht konfiguriert werden, da die Ressourcen bereits vorher eingerichtet wurden.
    - Nach dem Klicken auf diese Schaltfläche kann die Zielregion für dieses Projekt nicht mehr geändert werden.
    - Um ihre VMs in eine andere Region zu migrieren, müssen Sie ein neues/anderes Azure Migrate-Projekt erstellen.
    > [!NOTE]
    > Wenn Sie bei der Erstellung des Azure Migrate-Projekts den privaten Endpunkt als Konnektivitätsmethode ausgewählt haben, wird der Recovery Services-Tresor auch für die Konnektivität mit privaten Endpunkten konfiguriert. Stellen Sie sicher, dass die privaten Endpunkte von der Replikationsappliance aus erreichbar sind: [**Weitere Informationen**](troubleshoot-network-connectivity.md)

6. Wählen Sie unter **Möchten Sie eine neue Replikationsappliance installieren oder ein vorhandenes Setup horizontal hochskalieren?** die Option **Replikationsappliance installieren**.
7. Führen Sie unter **Laden Sie die Software für die Replikationsappliance herunter, und installieren Sie sie** den Download des Installationsprogramms für die Appliance und des Registrierungsschlüssels durch. Sie benötigen den Schlüssel zum Registrieren der Appliance. Der Schlüssel ist nach dem Herunterladen fünf Tage lang gültig.

    ![Anbieter herunterladen](media/tutorial-migrate-physical-virtual-machines/download-provider.png)

8. Kopieren Sie die Setupdatei für die Appliance und die Schlüsseldatei auf die GCP-VM mit Windows Server 2016 oder Windows Server 2012, die Sie für die Replikationsappliance erstellt haben.
9. Führen Sie die Setupdatei für die Replikationsappliance aus, wie dies im nächsten Schritt beschrieben ist.  
    9.1. Wählen Sie unter **Vorbereitung** die Option **Install the configuration server and process server** (Konfigurationsserver und Prozessserver installieren) und dann **Weiter**.   
    9.2 Wählen Sie unter **Third-Party Software License** (Drittanbietersoftware-Lizenz) die Option **I accept the third-party license agreement** (Ich akzeptiere die Drittanbieter-Lizenzvereinbarung) und dann **Weiter**.   
    9.3 Wählen Sie unter **Registrierung** die Option **Durchsuchen**, und navigieren Sie dann zu dem Ort, an dem Sie die Schlüsseldatei für die Tresorregistrierung abgelegt haben. Wählen Sie **Weiter** aus.  
    9.4 Wählen Sie unter **Interneteinstellungen** die Option **Connect to Azure Site Recovery without a proxy server** (Ohne Proxyserver mit Azure Site Recovery verbinden) und dann **Weiter**.  
    9.5 Auf der Seite **Prüfung der erforderlichen Komponenten** werden verschiedene Komponenten überprüft. Wählen Sie **Weiter**, wenn der Vorgang abgeschlossen ist.  
    9.6 Geben Sie unter **MySQL Configuration** (MySQL-Konfiguration) ein Kennwort für die MySQL-Datenbank an, und wählen Sie anschließend **Weiter** aus.  
    9.7 Wählen Sie unter **Umgebungsdetails** die Option **Nein**. Es ist nicht erforderlich, Ihre VMs zu schützen. Klicken Sie anschließend auf **Weiter**.  
    9.8 Wählen Sie unter **Installationspfad** die Option **Weiter**, um den Standardwert zu übernehmen.  
    9.9 Wählen Sie unter **Netzwerkauswahl** die Option **Weiter**, um den Standardwert zu übernehmen.  
    9.10 Wählen Sie unter **Zusammenfassung** die Option **Installieren**.   
    9.11 Unter **Installationsfortschritt** werden Informationen zum Installationsvorgang angezeigt. Wählen Sie **Fertig stellen**, wenn der Vorgang abgeschlossen ist. In einem Fenster wird eine Meldung zu einem Neustart angezeigt. Klicken Sie auf **OK**.   
    9.12 Als Nächstes wird in einem Fenster eine Meldung zur Passphrase für die Konfigurationsserververbindung angezeigt. Kopieren Sie die Passphrase in die Zwischenablage, und speichern Sie die Passphrase in einer temporären Textdatei auf den Quell-VMs. Sie benötigen diese Passphrase später während des Installationsprozesses für den Mobilitätsdienst.
10. Nach Abschluss der Installation wird automatisch der Assistent zur Appliancekonfiguration gestartet. (Sie können den Assistenten auch manuell über die cspsconfigtool-Verknüpfung starten, die auf dem Desktop der Appliance erstellt wird.) In diesem Tutorial installieren wir den Mobilitätsdienst manuell auf den Quell-VMs, die repliziert werden sollen. Erstellen Sie daher in diesem Schritt ein Dummykonto, und fahren Sie fort. Sie können die folgenden Details zum Erstellen des Dummykontos angeben: „guest“ als Anzeigename, „username“ als Benutzername und „password“ als Kennwort für das Konto. Sie verwenden dieses Dummykonto in der Phase zum Aktivieren der Replikation.

    ![Registrierung abschließen](./media/tutorial-migrate-physical-virtual-machines/finalize-registration.png)

## <a name="install-the-mobility-service"></a>Installieren des Mobilitätsdiensts

Ein Mobilitätsdienst-Agent muss auf den GCP-VMs installiert sein, die migriert werden sollen. Die Installationsprogramme für den Agent sind auf der Replikationsappliance verfügbar. Sie können nach dem richtigen Installationsprogramm suchen und den Agent auf jedem Computer installieren, den Sie migrieren möchten. Gehen Sie wie folgt vor:

1. Melden Sie sich an der Replikationsappliance an.
2. Navigieren Sie zu **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository**.
3. Suchen Sie nach dem richtigen Installationsprogramm für das Betriebssystem und die Version der virtuellen GCP-Quellcomputer. Lesen Sie den Artikel zu den [unterstützten Betriebssystemen](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines).
4. Kopieren Sie die Installationsprogrammdatei auf den virtuellen GCP-Quellcomputer, den Sie migrieren möchten.
5. Stellen Sie sicher, dass Sie über die gespeicherte Textdatei mit der Passphrase verfügen, die bei der Installation der Replikationsappliance erstellt wurde.
    - Wenn Sie vergessen haben, die Passphrase zu speichern, können Sie die Passphrase auf der Replikationsappliance mit diesem Schritt anzeigen. Führen Sie in der Befehlszeile **C:\ProgramData\ASR\home\svsystems\bin\genpassphrase.exe -v** aus, um die aktuelle Passphrase anzuzeigen.
    - Kopieren Sie nun die Passphrase in die Zwischenablage, und speichern Sie sie in einer temporären Textdatei auf den Quell-VMs.

### <a name="installation-guide-for-windows-gcp-vms"></a>Installationshandbuch für GCP-VMs unter Windows

1. Extrahieren Sie den Inhalt der Installationsprogrammdatei wie folgt in einen lokalen Ordner auf der GCP-VM (z. B. „C:\Temp“):

    ```
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```  

2. Führen Sie das Mobility Service-Installationsprogramm aus:
    ```
   UnifiedAgent.exe /Role "MS" /Silent
    ```  

3. Registrieren Sie den Agent bei der Replikationsappliance:
    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <replication appliance IP address> /PassphraseFilePath <Passphrase File Path>
    ```

### <a name="installation-guide-for-linux-gcp-vms"></a>Installationshandbuch für GCP-VMs unter Linux

1. Extrahieren Sie den Inhalt des Installationsprogramm-Tarballs wie folgt in einen lokalen Ordner auf der GCP-VM (z. B. „/tmp/MobSvcInstaller“):
    ```
    mkdir /tmp/MobSvcInstaller
    tar -C /tmp/MobSvcInstaller -xvf <Installer tarball>
    cd /tmp/MobSvcInstaller
    ```  

2. Führen Sie das Installationsskript aus:
    ```
    sudo ./install -r MS -q
    ```  

3. Registrieren Sie den Agent bei der Replikationsappliance:
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <replication appliance IP address> -P <Passphrase File Path>
    ```

## <a name="enable-replication-for-gcp-vms"></a>Aktivieren der Replikation für GCP-VMs

> [!NOTE]
> Im Portal können Sie bis zu 10 virtuelle Computer gleichzeitig für die Replikation hinzufügen. Um mehr VMs gleichzeitig zu replizieren, können Sie sie in Batches von 10 hinzufügen.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Replizieren**.

    ![Replizieren von VMs](./media/tutorial-migrate-physical-virtual-machines/select-replicate.png)

2. Wählen Sie unter **Replizieren** > **Quelleinstellungen** > **Sind Ihre Computer virtualisiert?** die Option **Nicht virtualisiert/Andere** aus.
3. Wählen Sie unter **Lokale Appliance** den Namen der Azure Migrate-Appliance aus, die Sie eingerichtet haben.
4. Wählen Sie unter **Prozessserver** den Namen der Replikationsappliance aus.
5. Wählen Sie unter **Gastanmeldeinformationen** das Dummykonto aus, das Sie zuvor während des [Replikationsinstaller-Setups](#download-the-replication-appliance-installer) erstellt haben, um den Mobilitätsdienst manuell zu installieren. (Die Pushinstallation wird nicht unterstützt.) Klicken Sie anschließend auf **Next: Virtuelle Computer**.   

    ![Replizieren von Einstellungen](./media/tutorial-migrate-physical-virtual-machines/source-settings.png)
6. Übernehmen Sie im Bereich **Virtuelle Computer** unter **Migrationseinstellungen aus einer Bewertung importieren?** die Standardeinstellung **Nein, ich gebe die Migrationseinstellungen manuell an**.
7. Wählen Sie alle VMs aus, die Sie migrieren möchten. Klicken Sie anschließend auf **Next: Zieleinstellungen**.

    ![Auswählen von VMs](./media/tutorial-migrate-physical-virtual-machines/select-vms.png)

8. Wählen Sie unter **Zieleinstellungen** das Abonnement und die Zielregion für die Migration aus, und geben Sie die Ressourcengruppe an, in der sich die Azure-VMs nach der Migration befinden.
9. Wählen Sie unter **Virtuelles Netzwerk** das Azure-VNET/-Subnetz aus, in das die Azure-VMs nach der Migration eingebunden werden.  
10. Behalten Sie im **Cachespeicherkonto** die Standardoption bei, um das Cachespeicherkonto zu verwenden, das automatisch für das Projekt erstellt wird. Verwenden Sie die Dropdownliste, wenn Sie ein anderes Speicherkonto angeben möchten, das als Cachespeicherkonto für die Replikation verwendet werden soll. <br/>
    > [!NOTE]
    >
    > - Wenn Sie den privaten Endpunkt als Konnektivitätsmethode für das Azure Migrate-Projekt ausgewählt haben, gewähren Sie dem Recovery Services-Tresor Zugriff auf das Cachespeicherkonto. [**Weitere Informationen**](how-to-use-azure-migrate-with-private-endpoints.md#grant-access-permissions-to-the-recovery-services-vault)
    > - Erstellen Sie einen privaten Endpunkt für das Cachespeicherkonto, um die Replikation mithilfe von ExpressRoute mit privatem Peering zu ermöglichen. [**Weitere Informationen**](how-to-use-azure-migrate-with-private-endpoints.md#create-a-private-endpoint-for-the-storage-account-optional)
11. Wählen Sie unter **Verfügbarkeitsoptionen** Folgendes aus:
    -  Verfügbarkeitszone, um den migrierten Computer einer bestimmten Verfügbarkeitszone in der Region anzuheften. Verteilen Sie mit dieser Option Server, die eine Anwendungsebene mit mehreren Knoten bilden, über Verfügbarkeitszonen. Wenn Sie diese Option auswählen, müssen Sie die Verfügbarkeitszone angeben, die für jeden auf der Registerkarte „Compute“ ausgewählten Computer verwendet werden soll. Diese Option ist nur verfügbar, wenn die für die Migration ausgewählte Zielregion Verfügbarkeitszonen unterstützt.
    -  Verfügbarkeitsgruppe, um den migrierten Computer in einer Verfügbarkeitsgruppe zu platzieren. Um diese Option verwenden zu können, muss die ausgewählte Zielressourcengruppe über mindestens eine Verfügbarkeitsgruppe verfügen.
    - Die Infrastrukturredundanzoption ist nicht erforderlich, wenn Sie keine dieser Verfügbarkeitskonfigurationen für die migrierten Computer benötigen.
12. Wählen Sie unter **Datenträgerverschlüsselungstyp** Folgendes aus:
    - Verschlüsselung ruhender Daten mit plattformseitig verwaltetem Schlüssel
    - Verschlüsselung ruhender Daten mit kundenseitig verwaltetem Schlüssel
    - Mehrfachverschlüsselung mit plattformseitig und kundenseitig verwalteten Schlüsseln

   > [!NOTE]
   > Um VMs mit CMK zu replizieren, müssen Sie unter der Zielressourcengruppe einen [Datenträgerverschlüsselungssatz erstellen](../virtual-machines/disks-enable-customer-managed-keys-portal.md#set-up-your-disk-encryption-set). Ein Datenträgerverschlüsselungssatz-Objekt ordnet verwaltete Datenträger einer Key Vault-Instanz zu, die den für SSE zu verwendenden CMK enthält.

13. Wählen Sie unter **Azure-Hybridvorteil**

    - die Option **Nein** aus, falls Sie den Azure-Hybridvorteil nicht anwenden möchten. Klicken Sie dann auf **Weiter**.
    - Wählen Sie **Ja** aus, wenn Sie über Windows Server-Computer verfügen, die durch aktive Software Assurance- oder Windows Server-Abonnements abgedeckt sind, und den Vorteil auf die zu migrierenden Computer anwenden möchten. Klicken Sie dann auf **Weiter**.

    ![Zieleinstellungen](./media/tutorial-migrate-vmware/target-settings.png)

14. Überprüfen Sie in **Compute** Name, Größe, Betriebssystem- Datenträger und Verfügbarkeitskonfiguration der VM (falls im vorherigen Schritt ausgewählt). Die VMs müssen die [Azure-Anforderungen](migrate-support-matrix-physical-migration.md#azure-vm-requirements) erfüllen.

    - **VM-Größe**: Bei Verwendung von Bewertungsempfehlungen zeigt die Dropdownliste für die VM-Größe die empfohlene Größe. Andernfalls wählt Azure Migrate eine Größe basierend auf der höchsten Übereinstimmung im Azure-Abonnement aus. Alternativ können Sie unter **Azure-VM-Größe** manuell eine Größe auswählen.
    - **Betriebssystemdatenträger**: Geben Sie den Betriebssystemdatenträger (Startdatenträger) für die VM an. Der Betriebssystemdatenträger enthält den Bootloader und das Installationsprogramm des Betriebssystems.
    - **Verfügbarkeitszone**: Geben Sie die zu verwendende Verfügbarkeitszone an.
    - **Verfügbarkeitsgruppe**: Geben Sie die zu verwendende Verfügbarkeitsgruppe an.

![Computeeinstellungen](./media/tutorial-migrate-physical-virtual-machines/compute-settings.png)

15. Geben Sie unter **Datenträger** an, ob die VM-Datenträger in Azure repliziert werden sollen, und wählen Sie den Datenträgertyp (SSD Standard/HDD Standard oder Managed Disks Premium) in Azure aus. Klicken Sie dann auf **Weiter**.
    - Sie können Datenträger von der Replikation ausschließen.
    - Wenn Sie Datenträger ausschließen, sind diese nach der Migration nicht auf der Azure-VM vorhanden.

    ![Datenträgereinstellungen](./media/tutorial-migrate-physical-virtual-machines/disks.png)

16. Überprüfen Sie unter **Replikation prüfen und starten** die Einstellungen, und klicken Sie auf **Replizieren**, um die erste Replikation für die Server zu starten.

> [!NOTE]
> Sie können die Replikationseinstellungen vor Beginn der Replikation jederzeit unter **Verwalten** > **Aktuell replizierte Computer** aktualisieren. Die Einstellungen können nach dem Beginn der Replikation nicht mehr geändert werden.

## <a name="track-and-monitor-replication-status"></a>Verfolgen und überwachen des Replikationsstatus

- Wenn Sie auf **Replizieren** klicken, beginnt die Durchführung des Auftrags „Replikation starten“.
- Nachdem der Auftrag „Replikation starten“ erfolgreich abgeschlossen wurde, beginnen die VMs mit der anfänglichen Replikation in Azure.
- Nach Abschluss der ersten Replikation beginnt die Deltareplikation. Inkrementelle Änderungen an Datenträgern von GCP-VMs werden regelmäßig auf die Replikatdatenträger in Azure repliziert.

Sie haben die Möglichkeit, den Auftragsstatus über die Portalbenachrichtigungen nachzuverfolgen.

Sie können den Replikationsstatus überwachen, indem Sie auf **Server werden repliziert** klicken (unter **Azure Migrate: Servermigration**).  

![Überwachen der Replikation](./media/tutorial-migrate-physical-virtual-machines/replicating-servers.png)

## <a name="run-a-test-migration"></a>Ausführen einer Testmigration

Wenn die Deltareplikation beginnt, können Sie zunächst eine Testmigration für die VMs ausführen, bevor eine vollständige Migration zu Azure erfolgt. Die Testmigration wird dringend empfohlen und bietet die Möglichkeit, mögliche Probleme zu ermitteln und zu beheben, bevor Sie mit der tatsächlichen Migration fortfahren. Es empfiehlt sich, diesen Schritt vor der Migration mindestens einmal für jede VM auszuführen.

- Bei einer Testmigration wird überprüft, ob die Migration wie erwartet funktioniert, ohne die GCP-VMs zu beeinträchtigen, und ob diese während der Replikation betriebsbereit bleiben.
- Mit der Testmigration wird die Migration simuliert, indem eine Azure-VM mit replizierten Daten erstellt wird (normalerweise per Migration zu einem nicht für die Produktion bestimmten VNET unter Ihrem Azure-Abonnement).
- Sie können den replizierten virtuellen Azure-Testcomputer verwenden, um die Migration zu überprüfen, App-Tests durchzuführen und Probleme zu beheben, bevor die vollständige Migration erfolgt.

Führen Sie die Testmigration wie folgt durch:

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Servermigration** auf **Migrierte Server testen**.

     ![Testen der migrierten Server](./media/tutorial-migrate-physical-virtual-machines/test-migrated-servers.png)

2. Klicken Sie mit der rechten Maustaste auf die zu testende VM, und klicken Sie anschließend auf **Testmigration**.

    :::image type="content" source="./media/tutorial-migrate-physical-virtual-machines/test-migrate-inline.png" alt-text="Screenshot: Ergebnis nach dem Klicken auf „Testmigration“" lightbox="./media/tutorial-migrate-physical-virtual-machines/test-migrate-expanded.png":::

3. Wählen Sie unter **Testmigration** das Azure VNET aus, in dem sich die Azure-VM nach der Migration befindet. Es empfiehlt sich, ein nicht für die Produktion bestimmtes VNET zu verwenden.
4. Der Auftrag **Testmigration** wird gestartet. Überwachen Sie den Auftrag anhand der Portalbenachrichtigungen.
5. Zeigen Sie die migrierte Azure-VM nach Abschluss der Migration im Azure-Portal unter **Virtuelle Computer** an. Der Computername enthält das Suffix **-Test**.
6. Klicken Sie nach Abschluss des Tests mit der rechten Maustaste unter **Aktuell replizierte Computer** auf die Azure-VM, und klicken Sie anschließend auf **Testmigration bereinigen**.

    :::image type="content" source="./media/tutorial-migrate-physical-virtual-machines/clean-up-inline.png" alt-text="Screenshot: Ergebnis nach der Bereinigung der Testmigration" lightbox="./media/tutorial-migrate-physical-virtual-machines/clean-up-expanded.png":::

    > [!NOTE]
    > Sie können jetzt Ihre Server, auf denen SQL Server ausgeführt wird, bei SQL VM RP registrieren, um die Vorteile automatisierter Patches, der automatisierten Sicherung und vereinfachten Lizenzverwaltung mit der Erweiterung für den SQL-IaaS-Agent zu nutzen.
    >- Wählen Sie für die Registrierung bei SQL VM RP die Optionen **Verwaltung** > **Server werden repliziert** > **Computer mit SQL Server** > **Compute und Netzwerk** und dann **Ja** aus.
    >- Wählen Sie „Azure-Hybridvorteil“ aus, wenn Sie über SQL Server-Instanzen verfügen, die durch aktive Software Assurance- oder SQL Server-Abonnements abgedeckt sind, und den Vorteil auf die zu migrierenden Computer anwenden möchten.

## <a name="migrate-gcp-vms"></a>Migrieren von GCP-VMs

Nachdem Sie sich vergewissert haben, dass die Testmigration wie erwartet funktioniert, können Sie die GCP-VMs migrieren.

1. Klicken Sie im Azure Migrate-Projekt unter **Server** > **Azure Migrate: Servermigration** auf **Server werden repliziert**.

    ![Replizieren der Server](./media/tutorial-migrate-physical-virtual-machines/replicate-servers.png)

2. Klicken Sie unter **Aktuell replizierte Computer** mit der rechten Maustaste auf die VM und dann auf **Migrieren**.
3. Wählen Sie unter **Migrieren** > **Virtuelle Computer herunterfahren und eine geplante Migration ohne Datenverlust durchführen?** die Option **Ja** >  und anschließend **OK** aus.
    - Falls Sie die VM nicht herunterfahren möchten, wählen Sie **Nein** aus.
4. Für den virtuellen Computer wird ein Migrationsauftrag gestartet. Sie können den Auftragsstatus anzeigen, indem Sie oben rechts auf der Portalseite auf das Benachrichtigungsglockensymbol klicken, oder indem Sie zur Seite mit den Aufträgen des Servermigrationstools wechseln (Klicken Sie auf „Übersicht“, und wählen Sie dann auf der Toolkachel im linken Menü „Aufträge auswählen“ aus).
5. Nach Abschluss des Auftrags können Sie die VM auf der Seite „Virtuelle Computer“ anzeigen und verwalten.

### <a name="complete-the-migration"></a>Fertigstellen der Migration

1. Klicken Sie nach Abschluss der Migration mit der rechten Maustaste auf die VM und dann auf **Migration beenden**. Die folgenden Schritte werden ausgeführt:
    - Die Replikation für die GCP-VM wird beendet.
    - Die GCP-VM wird aus dem Zähler **Server werden repliziert.** in Azure Migrate entfernt: Servermigration.
    - Bereinigt die Replikationsstatusinformationen für den virtuellen Computer.
1. Überprüfen und [beheben Sie alle Windows-Aktivierungsprobleme auf dem virtuellen Azure-Computer](/troubleshoot/azure/virtual-machines/troubleshoot-activation-problems).
1. Führen Sie App-Anpassungen nach der Migration durch, z. B. die Aktualisierung von Hostnamen, Datenbankverbindungszeichenfolgen und Webserverkonfigurationen.
1. Führen Sie endgültige Anwendungs- und Migrationsakzeptanztests für die migrierte Anwendung durch, die nun in Azure ausgeführt wird.
1. Leiten Sie den Datenverkehr auf die migrierte Instanz der Azure-VM um.
1. Aktualisieren Sie die interne Dokumentation zum Anzeigen des neuen Speicherorts und der IP-Adresse der Azure-VMs.



## <a name="post-migration-best-practices"></a>Bewährte Methoden nach der Migration

- Beachten Sie zur Steigerung der Resilienz Folgendes:
    - Schützen Sie Daten, indem Sie Azure-VMs mit dem Azure Backup-Dienst sichern. [Weitere Informationen](../backup/quick-backup-vm-portal.md)
    - Sorgen Sie für die kontinuierliche Ausführung und Verfügbarkeit von Workloads, indem Sie Azure-VMs mithilfe von Site Recovery in eine sekundäre Region replizieren. [Weitere Informationen](../site-recovery/azure-to-azure-tutorial-enable-replication.md)
- Beachten Sie zur Steigerung der Sicherheit Folgendes:
    - Sperren und beschränken Sie den Zugriff von eingehendem Datenverkehr mit der [Just-In-Time-Verwaltung in Microsoft Defender für Cloud](../security-center/security-center-just-in-time.md).
    - Beschränken Sie den Netzwerkdatenverkehr mithilfe von [Netzwerksicherheitsgruppen](../virtual-network/network-security-groups-overview.md) auf Verwaltungsendpunkte.
    - Stellen Sie [Azure Disk Encryption](../security/fundamentals/azure-disk-encryption-vms-vmss.md) bereit, um Datenträger und Daten vor Diebstahl und unbefugtem Zugriff zu schützen.
    - Erfahren Sie mehr über das [Schützen von IaaS-Ressourcen](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/), und besuchen Sie die Website [Microsoft Defender für Cloud](https://azure.microsoft.com/services/security-center/).
- Beachten Sie zur Überwachung und Verwaltung Folgendes:
    - Ziehen Sie die Bereitstellung von [Azure Cost Management](../cost-management-billing/cost-management-billing-overview.md) in Erwägung, um den Ressourceneinsatz und die Ausgaben zu überwachen.



## <a name="troubleshooting--tips"></a>Problembehandlung/Tipps

**Frage:** Warum kann ich meine GCP-VMs nicht in der Liste der ermittelten Server für die Migration finden?   
**Antwort:** Überprüfen Sie, ob Ihre Replikationsappliance die Anforderungen erfüllt. Stellen Sie sicher, dass der Mobilitäts-Agent auf dem virtuellen Quellcomputer installiert ist, der migriert werden soll, und dass er als Konfigurationsserver registriert ist. Überprüfen Sie die Firewallregeln, um einen Netzwerkpfad zwischen der Replikationsappliance und den virtuellen GCP-Quellcomputern zu ermöglichen.  

**Frage:** Woran sehe ich, ob meine VM erfolgreich migriert wurde?   
**Antwort:** Nach Abschluss der Migration können Sie die VM auf der Seite „Virtuelle Computer“ anzeigen und verwalten. Stellen Sie eine Verbindung mit der migrierten VM her, um dies zu überprüfen.  

**Frage:** Ich kann keine VMs für die Migration aus meinen zuvor erstellten Ergebnissen der Serverbewertung importieren.   
**Antwort:** Zurzeit unterstützen wir keinen Import von Bewertungen für diesen Workflow. Um dieses Problem zu umgehen, können Sie die Bewertung exportieren und dann die VM-Empfehlung während des Schritts „Replikation aktivieren“ manuell auswählen.

**Frage:** Warum erhalte ich die Fehlermeldung "Failed to fetch BIOS GUID" (Fehler beim Abrufen der BIOS-GUID) bei dem Versuch, meine GCP-VMs zu ermitteln?   
**Antwort:** Verwenden Sie die Root-Anmeldung und keinen Pseudobenutzer für die Authentifizierung. Wenn Sie keinen Root-Benutzer verwenden können, stellen Sie sicher, dass die erforderlichen Funktionen gemäß den Instruktionen in der [Unterstützungsmatrix](migrate-support-matrix-physical.md#physical-server-requirements) für den Benutzer festgelegt sind. Überprüfen Sie außerdem die unterstützten Betriebssysteme für GCP-VMs.  

**Frage:** Mein Replikationsstatus zeigt keinen Fortschritt.   
**Antwort:** Überprüfen Sie, ob Ihre Replikationsappliance die Anforderungen erfüllt. Stellen Sie sicher, dass Sie die erforderlichen Ports auf Ihrer Replikationsappliance, TCP-Port 9443 und HTTPS 443 für den Datentransport, aktiviert haben. Stellen Sie sicher, dass keine veralteten doppelten Versionen der Replikationsappliance mit demselben Projekt verbunden sind.   

**Frage:** Warum kann ich wegen des HTTP-Statuscodes 504 vom Windows-Remoteverwaltungsdienst mithilfe von Azure Migrate keine GCP-Instanzen ermitteln?    
**Antwort:** Vergewissern Sie sich, dass Sie die Anforderungen für die Azure Migrate-Appliance und den URL-Zugriff kennen. Stellen Sie sicher, dass die Applianceregistrierung nicht durch Proxyeinstellungen blockiert wird.

**Frage:** Muss ich Änderungen vornehmen, bevor ich meine GCP-VMs zu Azure migriere?   
**Antwort:** Möglicherweise müssen Sie diese Änderungen vornehmen, bevor Sie Ihre EC2-VMs zu Azure migrieren:

- Wenn Sie cloud-init für die VM-Bereitstellung verwenden, können Sie cloud-init auf der VM deaktivieren, bevor Sie sie in Azure replizieren. Die von cloud-init auf dem virtuellen Computer ausgeführten Bereitstellungsschritte sind möglicherweise GCP-spezifisch und nach der Migration zu Azure nicht mehr gültig. 
- Lesen Sie den Abschnitt [Voraussetzungen](#prerequisites), um herauszufinden, ob für das verwendete Betriebssystem Änderungen erforderlich sind, bevor Sie zu Azure migrieren.
- Sie sollten vor der abschließenden Migration stets eine Testmigration ausführen.  

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Artikel zur [Cloudmigration](/azure/architecture/cloud-adoption/getting-started/migrate) des Frameworks für die Cloudeinführung (Cloud Adoption Framework) an.
