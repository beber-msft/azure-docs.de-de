---
title: Installieren des C#-basierten Agents auf einem Windows-Gerät
description: Hier erfahren Sie, wie Sie den Defender für IoT-Agent auf Geräten unter Windows (32 Bit oder 64 Bit) installieren.
ms.topic: conceptual
ms.date: 11/09/2021
ms.openlocfilehash: 1bd5017f7c5e6a59aac29dfd35834ef5768682e8
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132306071"
---
# <a name="deploy-a-defender-for-iot-c-based-security-agent-for-windows"></a>Bereitstellen eines C#-basierten Sicherheits-Agents von Defender für IoT unter Windows

In dieser Anleitung erfahren Sie, wie Sie den C#-basierten Sicherheits-Agent von Defender für IoT unter Windows installieren.

In diesem Artikel lernen Sie Folgendes:

- Installieren
- Überprüfen der Bereitstellung
- Deinstallieren des Agents
- Problembehandlung

## <a name="prerequisites"></a>Voraussetzungen

Informationen zu anderen Plattformen und Agent-Varianten finden Sie unter [Choose the right security agent](how-to-deploy-agent.md) (Auswählen des richtigen Sicherheits-Agents).

1. Lokale Administratorrechte auf dem Computer, auf dem Sie installieren möchten.

1. [Erstellen Sie einen Defender für IoT-Micro-Agent](quickstart-create-security-twin.md) für das Gerät.

## <a name="installation"></a>Installation

Verwenden Sie zum Installieren des Sicherheits-Agents den folgenden Workflow:

1. Installieren Sie den C#-basierten Defender für IoT-Windows-Agent auf dem Gerät. Laden Sie die neueste Version aus dem [GitHub-Repository](https://github.com/Azure/Azure-IoT-Security-Agent-CS) für Defender für IoT auf Ihren Computer herunter.

1. Extrahieren Sie den Inhalt des Pakets, und navigieren Sie zum Ordner „/Install“.

1. Öffnen Sie Windows PowerShell als Administrator.
1. Führen Sie Folgendes aus, um dem Skript „InstallSecurityAgent“ Ausführungsberechtigungen hinzuzufügen:

    ```
    Unblock-File .\InstallSecurityAgent.ps1
    ```

    Führen Sie anschließend Folgendes aus:

    ```
    .\InstallSecurityAgent.ps1 -Install -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -cl <certificate location kind>
    ```

    Beispiel:

    ```
    .\InstallSecurityAgent.ps1 -Install -aui Device -aum SymmetricKey -f c:\Temp\Key.txt -hn MyIotHub.azure-devices.net -di Mydevice1 -cl store
    ```

    Weitere Informationen zu Authentifizierungsparametern finden Sie unter [Authentifizierungsmethoden des Sicherheits-Agents](concept-security-agent-authentication-methods.md).

Das Skript bewirkt Folgendes:

* Installieren der erforderlichen Komponenten
* Hinzufügen eines Dienstbenutzers (mit deaktivierter interaktiver Anmeldung)
* Installieren des Agents als **Systemdienst**
* Konfigurieren des Agents mit den angegebenen Authentifizierungsparametern

Weitere Hilfe erhalten Sie bei Bedarf in PowerShell mithilfe des Befehls „Get-Help“.

Beispiel für „Get-Help“: ```Get-Help .\InstallSecurityAgent.ps1```

### <a name="verify-deployment-status"></a>Überprüfen des Bereitstellungsstatus

Führen Sie Folgendes aus, um den Bereitstellungsstatus des Agents zu überprüfen:

`sc.exe query "ASC IoT Agent"`

### <a name="uninstall-the-agent"></a>Deinstallieren des Agents

So deinstallieren Sie den Agent:

1. Führen Sie das folgende PowerShell-Skript aus, und legen Sie dabei den Parameter **-mode** auf **Uninstall** fest:

    ```
    .\InstallSecurityAgent.ps1 -Uninstall
    ```

## <a name="troubleshooting"></a>Problembehandlung

Sollte der Agent nicht starten, aktivieren Sie die Protokollierung, um weitere Informationen zu erhalten. (Die Protokollierung ist standardmäßig *deaktiviert*.)

So aktivieren Sie die Protokollierung:

1. Öffnen Sie die Konfigurationsdatei (General.config) für die Bearbeitung mit einem standardmäßigen Datei-Editor.

1. Bearbeiten Sie die folgenden Werte:

   ```xml
   <add key="logLevel" value="Debug" />
   <add key="fileLogLevel" value="Debug"/>
   <add key="diagnosticVerbosityLevel" value="Some" />
   <add key="logFilePath" value="IoTAgentLog.log" />
   ```

    > [!NOTE]
    > Es empfiehlt sich, die Protokollierung nach Abschluss der Problembehandlung zu **deaktivieren**. Bei **aktivierter** Protokollierung erhöhen sich Protokolldateigröße und Datennutzung.

1. Starten Sie den Agent über PowerShell oder über die Befehlszeile neu:

    **PowerShell**

     ```
     Restart-Service "ASC IoT Agent"
     ```

   oder

    **Befehlszeile**

     ```
     sc.exe stop "ASC IoT Agent"
     sc.exe start "ASC IoT Agent"
     ```

1. Sehen Sie sich die Protokolldatei an, um mehr über den Fehler zu erfahren. Die Protokolldatei befindet sich in dem Arbeitsverzeichnis, in dem das Skript ausgeführt wird. 

   Speicherort der Protokolldatei: `.\IoTAgentLog.log`

## <a name="next-steps"></a>Nächste Schritte

* Lesen der [Übersicht](overview.md) über den Defender für IoT-Dienst
* Weitere Informationen zu Defender für IoT: [Info zu Agent-basierten Lösungen für Gerätehersteller](architecture-agent-based.md)
* Aktivieren Sie den [Dienst](quickstart-onboard-iot-hub.md).
* Lesen Sie [häufig gestellte Fragen zum Microsoft Defender für IoT-Agent](resources-agent-frequently-asked-questions.md).
* Machen Sie sich mit [Warnungen](concept-security-alerts.md) vertraut.
