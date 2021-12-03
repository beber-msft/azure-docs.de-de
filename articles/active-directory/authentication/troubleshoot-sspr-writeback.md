---
title: Problembehandlung beim Rückschreiben der Self-Service-Kennwortzurücksetzung – Azure Active Directory
description: Lernen Sie häufige Probleme und die entsprechenden Lösungsschritte beim Rückschreiben der Self-Service-Kennwortzurücksetzung in Azure Active Directory kennen.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: troubleshooting
ms.date: 08/25/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8bdf8f914aaebff678d0d4e1a30823c18460f3d8
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131018298"
---
# <a name="troubleshoot-self-service-password-reset-writeback-in-azure-active-directory"></a>Problembehandlung beim Rückschreiben der Self-Service-Kennwortzurücksetzung in Azure Active Directory

Mit der Self-Service-Kennwortzurücksetzung (SSPR) in Azure Active Directory (Azure AD) können Benutzer ihre Kennwörter in der Cloud zurücksetzen. Kennwortrückschreiben ist eine Funktion, die mit [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) aktiviert wurde und es ermöglicht, Kennwortänderungen in der Cloud in Echtzeit in ein vorhandenes lokales Verzeichnis zurückzuschreiben.

Wenn Sie Probleme beim Rückschreiben der Self-Service-Kennwortzurücksetzung haben, können Ihnen die folgende Beschreibung häufiger Fehler und die Schritte zur Problembehandlung helfen. Wenn Sie keine Antwort für Ihr Problem finden, [stehen Ihnen unsere Supportteams stets für weitere Unterstützung zur Verfügung](#contact-microsoft-support).

## <a name="troubleshoot-connectivity"></a>Behandeln von Konnektivitätsproblemen

Wenn Sie Probleme beim Kennwortrückschreiben für Azure AD Connect haben, helfen Ihnen möglicherweise die folgenden Schritte, das Problem zu beheben. Um den Dienst wiederherzustellen, sollten Sie diese Schritte in der folgenden Reihenfolge ausführen:

* [Bestätigen der Netzwerkkonnektivität](#confirm-network-connectivity)
* [Neustarten des Azure AD Connect-Synchronisierungsdiensts](#restart-the-azure-ad-connect-sync-service)
* [Deaktivieren und erneutes Aktivieren des Kennwortrückschreibenfeatures](#disable-and-re-enable-the-password-writeback-feature)
* [Installieren der aktuellen Azure AD Connect-Version](#install-the-latest-azure-ad-connect-release)
* [Problembehandlung: Kennwortrückschreiben](#common-password-writeback-errors)

### <a name="confirm-network-connectivity"></a>Bestätigen der Netzwerkkonnektivität

Die häufigste Fehlerursache besteht darin, dass die Firewall, Proxyports oder Leerlauftimeouts falsch konfiguriert sind.

Für Azure AD Connect, Version *1.1.443.0* und höher, ist *ausgehender HTTPS-Zugriff* auf die folgenden Adressen erforderlich:

* *\*.passwordreset.microsoftonline.com*
* *\*.servicebus.windows.net*

[Azure Government-Endpunkte:](../../azure-government/compare-azure-government-global-azure.md#guidance-for-developers)

* *\*.passwordreset.microsoftonline.us*
* *\*.servicebus.usgovcloudapi.net*

Wenn Sie eine höhere Granularität benötigen, konsultieren Sie die [Liste der IP-Bereiche der Microsoft Azure-Rechenzentren](https://www.microsoft.com/download/details.aspx?id=41653). Diese Liste wird jeden Mittwoch aktualisiert und tritt am jeweils nächsten Montag in Kraft.

Führen Sie das folgende Cmdlet aus, um festzustellen, ob der Zugriff auf eine URL und einen Port in einer Umgebung eingeschränkt ist:

```powershell
Test-NetConnection -ComputerName ssprdedicatedsbprodscu.servicebus.windows.net -Port 443
```

Oder führen Sie Folgendes aus:

```powershell
Invoke-WebRequest -Uri https://ssprdedicatedsbprodscu.servicebus.windows.net -Verbose
```

Weitere Informationen finden Sie unter [Voraussetzungen für Azure AD Connect](../hybrid/how-to-connect-install-prerequisites.md).

### <a name="restart-the-azure-ad-connect-sync-service"></a>Neustarten des Azure AD Connect-Synchronisierungsdiensts

Führen Sie zum Beheben von Konnektivitätsproblemen oder anderen vorübergehenden Problemen die folgenden Schritte aus, um den Azure AD Connect-Synchronisierungsdienst neu zu starten:

1. Wählen Sie als Administrator auf dem Server, auf dem Azure AD Connect ausgeführt wird, **Start** aus.
1. Geben Sie *services.msc* in das Suchfeld ein, und drücken Sie die **EINGABETASTE**.
1. Suchen Sie nach dem Eintrag *Microsoft Azure AD Sync*.
1. Klicken Sie mit der rechten Maustaste auf den Diensteintrag, klicken Sie auf **Neu starten**, und warten Sie, bis der Vorgang abgeschlossen wurde.

    :::image type="content" source="./media/troubleshoot-sspr-writeback/service-restart.png" alt-text="Neustarten des Azure AD Sync-Diensts mit der grafischen Benutzeroberfläche" border="false":::

Mit diesen Schritten wird die Verbindung mit Azure AD wiederhergestellt, und die Konnektivitätsprobleme sollten behoben sein.

Wenn das Problem durch einen Neustart des Azure AD Connect-Synchronisierungsdiensts nicht behoben wurde, deaktivieren Sie das Kennwortrückschreibenfeature, und aktivieren Sie es anschließend wieder, wie im nächsten Abschnitt beschrieben.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Deaktivieren und erneutes Aktivieren des Kennwortrückschreibenfeatures

Um mit der Problembehandlung fortzufahren, führen Sie die folgenden Schritte aus, um das Kennwortrückschreibenfeature zu deaktivieren und dann erneut zu aktivieren:

1. Öffnen Sie als Administrator auf dem Server, auf dem Azure AD Connect ausgeführt wird, den **Konfigurations-Assistenten für Azure AD Connect**.
1. Geben Sie unter **Mit Azure AD verbinden** Ihre Anmeldeinformationen für globale Azure AD-Administratoren ein.
1. Geben Sie unter **Mit AD DS verbinden** Ihre Anmeldeinformationen für lokale Administratoren der Active Directory Domain Services ein.
1. Klicken Sie unter **Eindeutige Identifizierung der Benutzer** auf die Schaltfläche **Weiter**.
1. Deaktivieren Sie unter **Optionale Features** das Kontrollkästchen **Kennwortrückschreiben**.
1. Klicken Sie auf den restlichen Seiten des Assistenten jeweils auf **Weiter**, ohne Änderungen vorzunehmen, bis Sie zur Seite **Bereit zur Konfiguration** gelangen.
1. Überprüfen Sie, ob auf der Seite **Bereit für Konfiguration** die Option *Kennwortrückschreiben* als *deaktiviert* angezeigt wird. Wählen Sie die grüne Schaltfläche **Konfigurieren** aus, um die Änderungen zu committen.
1. Deaktivieren Sie unter **Fertig** die Option **Jetzt synchronisieren**, und klicken Sie auf **Fertig stellen**, um den Assistenten zu schließen.
1. Öffnen Sie den **Konfigurations-Assistenten für Azure AD Connect** erneut.
1. Wiederholen Sie die Schritte 2 bis 8, aktivieren Sie diesmal jedoch auf der Seite **Optionale Features** die Option *Kennwortrückschreiben*, um den Dienst wieder zu aktivieren.

Mit diesen Schritten wird die Verbindung mit Azure AD wiederhergestellt, und die Konnektivitätsprobleme sollten behoben sein.

Wenn das Problem durch Deaktivieren und erneutes Aktivieren des Kennwortrückschreibenfeatures nicht behoben wurde, installieren Sie Azure AD Connect neu, wie im nächsten Abschnitt beschrieben.

### <a name="install-the-latest-azure-ad-connect-release"></a>Installieren der aktuellen Azure AD Connect-Version

Eine Neuinstallation von Azure AD Connect kann Konfigurations- und Konnektivitätsprobleme zwischen Azure AD und Ihrer lokalen Active Directory Domain Services-Umgebung beseitigen. Es wird empfohlen, diesen Schritt erst auszuführen, nachdem Sie die vorherigen Schritte ausgeführt haben, um die Konnektivität zu überprüfen und Konnektivitätsprobleme zu beheben.

> [!WARNING]
> Falls Sie die integrierten Synchronisierungsregeln angepasst haben, *sichern Sie diese, bevor Sie mit dem Upgrade fortfahren, und stellen Sie die Anpassungen nach dem Upgrade manuell wieder bereit*.

1. Laden Sie die neueste Version von Azure AD Connect aus dem [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkId=615771) herunter.
1. Da Sie Azure AD Connect bereits installiert haben, führen Sie ein direktes Upgrade Ihrer Azure AD Connect-Installation auf die aktuelle Version durch.

    Führen Sie das heruntergeladene Paket aus, und folgen Sie den Bildschirmanweisungen zum Aktualisieren von Azure AD Connect.

Mit diesen Schritten sollte die Verbindung mit Azure AD wiederhergestellt werden, und die Konnektivitätsprobleme sollten behoben sein.

Wenn das Problem durch Installieren der aktuellen Version des Azure AD Connect-Servers nicht behoben wurde, deaktivieren Sie als Letztes nach dem Installieren der aktuellen Version das Kennwortrückschreiben, und aktivieren Sie es anschließend wieder.

## <a name="verify-that-azure-ad-connect-has-the-required-permissions"></a>Überprüfen, ob Azure AD Connect über die erforderlichen Berechtigungen verfügt

Azure AD Connect benötigt für das Kennwortrückschreiben die AD DS-Berechtigung **Kennwort zurücksetzen**. Ob Azure AD Connect diese Berechtigung für ein bestimmtes lokales AD DS-Benutzerkonto besitzt, können Sie mithilfe des **Windows-Features „Effektive Berechtigung“** ermitteln:

1. Melden Sie sich beim Azure AD Connect-Server an, und starten Sie den **Synchronization Service Manager**, indem Sie auf **Start** > **Synchronisierungsdienst** klicken.
1. Wählen Sie auf der Registerkarte **Connectors** den lokalen Connector **Active Directory Domain Services** aus, und klicken Sie anschließend auf **Eigenschaften**.

    :::image type="content" source="./media/troubleshoot-sspr-writeback/synchronization-service-manager.png" alt-text="Synchronization Service Manager zeigt das Bearbeiten von Eigenschaften" border="false":::
  
1. Klicken Sie im Popupfenster auf die Registerkarte **Mit Active Directory-Gesamtstruktur verbinden**, und notieren Sie sich die Eigenschaft **Benutzername**. Bei dieser Eigenschaft handelt es sich um das AD DS-Konto, das von Azure AD Connect für die Verzeichnissynchronisierung verwendet wird.

    Damit Azure AD Connect das Kennwortrückschreiben durchführen kann, muss das AD DS-Konto über die Berechtigung „Kennwort zurücksetzen“ verfügen. Mit den folgenden Schritten überprüfen Sie die Berechtigungen für dieses Benutzerkonto.

    :::image type="content" source="./media/troubleshoot-sspr-writeback/synchronization-service-manager-properties.png" alt-text="Suchen des Active Directory-Benutzerkontos für den Synchronisierungsdienst" border="false":::
  
1. Melden Sie sich bei einem lokalen Domänencontroller an, und starten Sie die Anwendung **Active Directory-Benutzer und -Computer**.
1. Klicken Sie auf **Ansicht**, und vergewissern Sie sich, dass die Option **Erweiterte Features** aktiviert ist.  

    :::image type="content" source="./media/troubleshoot-sspr-writeback/view-advanced-features.png" alt-text="Active Directory-Benutzer und -Computer zeigen erweiterte Features." border="false":::
  
1. Suchen Sie nach dem AD DS-Benutzerkonto, das Sie überprüfen möchten. Klicken Sie mit der rechten Maustaste auf das Konto, und klicken Sie anschließend auf **Eigenschaften**.  
1. Navigieren Sie im Popupfenster zur Registerkarte **Sicherheit**, und klicken Sie auf **Erweitert**.  
1. Navigieren Sie im Popupfenster mit den erweiterten **Sicherheitseinstellungen für den Administratorzur** Registerkarte **Effektiver Zugriff**.
1. Wählen Sie **Benutzer auswählen** aus, wählen Sie das von Azure AD Connect verwendete AD DS-Konto aus, und klicken Sie anschließend auf **Effektiven Zugriff anzeigen**.

    :::image type="content" source="./media/troubleshoot-sspr-writeback/view-effective-access.png" alt-text="Registerkarte „Effektiver Zugriff“ zeigt das Synchronisierungskonto" border="false":::
  
1. Scrollen Sie nach unten, und suchen Sie nach **Kennwort zurücksetzen**. Ist der Eintrag mit einem Häkchen versehen, ist das AD DS-Konto zum Zurücksetzen des Kennworts für das ausgewählte Active Directory-Benutzerkonto berechtigt.  

    :::image type="content" source="./media/troubleshoot-sspr-writeback/check-permissions.png" alt-text="Überprüfen, ob das Synchronisierungskonto die Berechtigung zum Zurücksetzen des Kennworts hat" border="false":::

## <a name="common-password-writeback-errors"></a>Häufige Fehler bei der Kennwortrückschreibung

Bei der Kennwortrückschreibung können die folgenden speziellen Probleme auftreten. Wenn einer dieser Fehler aufgetreten ist, verwenden Sie die vorgeschlagene Lösung, und überprüfen Sie dann, ob die Kennwörter ordnungsgemäß zurückgeschrieben werden.

| Fehler | Lösung |
| --- | --- |
| Der Dienst für die Kennwortzurücksetzung startet lokal nicht. Das Anwendungsereignisprotokoll des Azure AD Connect-Computers enthält den Fehler 6800. <br> <br> Nach der Integration können Verbundbenutzer oder Benutzer mit Pass-Through-Authentifizierung bzw. Kennworthashsynchronisierung ihre Kennwörter nicht zurücksetzen. | Bei aktiviertem Kennwortrückschreiben ruft das Synchronisierungsmodul die Bibliothek für das Rückschreiben auf, um die Konfiguration (Integration) mittels Kommunikation mit dem Cloudintegrationsdienst durchzuführen. Fehler bei der Integration oder beim Start des WCF-Endpunkts ( Windows Communication Foundation) für das Kennwortrückschreiben führen zu Fehlern im Ereignisprotokoll auf Ihrem Azure AD Connect-Computer. <br> <br> Im Rahmen des Neustarts des ADSync-Diensts (Azure AD Sync) wird der WCF-Endpunkt gestartet, sofern das Kennwortrückschreiben konfiguriert wurde. Falls beim Starten des Endpunkts jedoch ein Fehler auftritt, wird der Fehler 6800 protokolliert, und der Synchronisierungsdienst wird gestartet. Dieses Ereignis bedeutet, dass der Endpunkt für das Kennwortrückschreiben nicht gestartet wurde. Die Ereignisprotokolldetails für dieses Ereignis (6800) geben – zusammen mit den von der PasswordResetService-Komponente generierten Ereignisprotokolleinträgen – an, warum der Endpunkt nicht gestartet werden kann. Überprüfen Sie diese Ereignisprotokollfehler, und versuchen Sie Azure AD Connect neu zu starten, wenn das Kennwortrückschreiben weiterhin nicht funktioniert. Sollte das Problem bestehen bleiben, versuchen Sie, das Kennwortrückschreiben zu deaktivieren und erneut zu aktivieren.
| Wenn ein Benutzer versucht, ein Kennwort zurückzusetzen oder ein Konto mit aktiviertem Kennwortrückschreiben zu entsperren, tritt dabei ein Fehler auf. <br> <br> Darüber hinaus wird nach dem Entsperren ein Ereignis im Azure AD Connect-Ereignisprotokoll mit folgendem Inhalt angezeigt: „Synchronization Engine returned an error hr=800700CE, message=The filename or extension is too long“ (Das Synchronisierungsmodul hat einen Fehler zurückgegeben: hr=800700CE, message=Der Dateiname oder die Dateierweiterung ist zu lang.). | Suchen Sie das Active Directory-Konto für Azure AD Connect, und setzen Sie das Kennwort zurück, sodass es maximal 256 Zeichen umfasst. Öffnen Sie dann über das **Startmenü** den **Synchronisierungsdienst**. Navigieren Sie zu **Connectors**, und suchen Sie **Active Directory Connector**. Wählen Sie diesen Connector aus, und klicken Sie anschließend auf **Eigenschaften**. Navigieren Sie zur Seite **Anmeldeinformationen**, und geben Sie das neue Kennwort ein. Wählen Sie **OK**, um die Seite zu schließen. |
| Im letzten Schritt des Azure AD Connect-Installationsvorgangs wird ein Fehler mit dem Hinweis angezeigt, dass das Kennwortrückschreiben nicht konfiguriert werden konnte. <br> <br> Das Azure AD Connect-Anwendungsereignisprotokoll enthält den Fehler 32009 mit dem Text „Error getting auth token“ (Fehler beim Abrufen des Authentifizierungstokens). | Dieser Fehler tritt in den folgenden beiden Fällen auf: <br><ul><li>Sie haben zu Beginn der Azure AD Connect-Installation ein falsches Kennwort für das globale Administratorkonto angegeben.</li><li>Sie haben versucht, zu Beginn der Azure AD Connect-Installation einen Verbundbenutzer für das globale Administratorkonto zu verwenden.</li></ul> Stellen Sie zum Beheben dieses Problems sicher, dass Sie zu Beginn der Installation kein Verbundkonto für den globalen Administrator angegeben haben und dass das angegebene Kennwort richtig ist. |
| Das Ereignisprotokoll für den Azure AD Connect-Computer enthält den Fehler 32002, der durch Ausführen von „PasswordResetService“ ausgelöst wurde. <br> <br> Der Fehler lautet: „Error Connecting to ServiceBus. The token provider was unable to provide a security token.“ (Fehler beim Herstellen einer Verbindung mit „ServiceBus“. Der Tokenanbieter konnte kein Sicherheitstoken bereitstellen.) | Ihre lokale Umgebung kann keine Verbindung mit dem Azure Service Bus-Endpunkt in der Cloud herstellen. Dieser Fehler wird durch eine Firewallregel verursacht, die eine ausgehende Verbindung mit einem bestimmten Port oder einer Webadresse blockiert. Weitere Informationen finden Sie unter [Voraussetzungen für Azure AD Connect](../hybrid/how-to-connect-install-prerequisites.md). Starten Sie nach dem Aktualisieren dieser Regeln den Azure AD Connect-Server neu. Anschließend sollte das Kennwortrückschreiben wieder funktionieren. |
| Nachdem sie eine Weile gearbeitet haben, können Verbundbenutzer oder Benutzer mit Pass-Through-Authentifizierung bzw. Kennworthashsynchronisierung ihre Kennwörter nicht zurücksetzen. | In seltenen Fällen kann der Dienst für das Kennwortrückschreiben möglicherweise nicht neu gestartet werden, wenn Azure AD Connect neu gestartet wurde. Prüfen Sie in diesen Fällen zunächst, ob das Kennwortrückschreiben lokal aktiviert ist. Hierzu können Sie entweder den Azure AD Connect-Assistenten oder PowerShell verwenden. Wenn das Feature aktiviert ist, versuchen Sie erneut, es zu aktivieren oder zu deaktivieren. Sollte dieser Problembehandlungsschritt nicht funktionieren, deinstallieren Sie Azure AD Connect vollständig, und installieren Sie es neu. |
| Verbundbenutzern oder Benutzern mit Pass-Through-Authentifizierung bzw. Kennworthashsynchronisierung, die versuchen, ihre Kennwörter zurückzusetzen, wird ein Fehler angezeigt, wenn sie versuchen, ihre Kennwörter zu übermitteln. Der Fehler deutet auf ein Dienstproblem hin. <br ><br> Zusätzlich zu diesem Problem wird in den lokalen Ereignisprotokollen während der Vorgänge zur Kennwortzurücksetzung möglicherweise ein Fehler mit dem Hinweis angezeigt, dass dem Verwaltungs-Agent der Zugriff verweigert wurde. | Sollten diese Fehler in Ihrem Ereignisprotokoll enthalten sein, stellen Sie sicher, dass das ADMA-Konto (Active Directory Management Agent, Active Directory-Verwaltungs-Agent), das im Rahmen der Konfiguration im Assistenten angegeben wurde, über die erforderlichen Berechtigungen für das Kennwortrückschreiben verfügt. <br> <br> Nach Erteilung dieser Berechtigung kann es bis zu einer Stunde dauern, bis die Berechtigungen über den Hintergrundtask `sdprop` auf dem Domänencontroller (DC) angewendet wurden. <br> <br> Damit die Kennwortzurücksetzung funktioniert, müssen die Berechtigungen in die Sicherheitsbeschreibung des Benutzerobjekts geschrieben werden, dessen Kennwort zurückgesetzt wird. Bis diese Berechtigung im Benutzerobjekt angezeigt werden, treten bei der Kennwortzurücksetzung weiterhin Zugriffsverweigerungsfehler auf. |
| Verbundbenutzern oder Benutzern mit Pass-Through-Authentifizierung bzw. Kennworthashsynchronisierung, die versuchen, ihre Kennwörter zurückzusetzen, wird nach der Kennwortübermittlung ein Fehler angezeigt. Der Fehler deutet auf ein Dienstproblem hin. <br> <br> Darüber hinaus wird in den Ereignisprotokollen bei Vorgängen zur Kennwortzurücksetzung für den Azure AD Connect-Dienst möglicherweise ein Fehler mit dem Hinweis auf ein nicht gefundenes Objekt angezeigt. | Dieser Fehler deutet üblicherweise darauf hin, dass das Synchronisierungsmodul entweder das Benutzerobjekt im Azure AD-Connectorbereich oder das verknüpfte Metaverse- oder Azure AD-Connectorbereichsobjekt nicht gefunden hat. <br> <br> Stellen Sie zur Behandlung dieses Problems sicher, dass für den Benutzer tatsächlich eine Synchronisierung der lokalen Umgebung über die aktuelle Azure AD Connect-Instanz zu Azure AD erfolgt, und untersuchen Sie den Zustand des Objekts in den Connectorbereichen und im Metaverse (MV). Vergewissern Sie sich, dass das AD CS-Objekt (Active Directory-Zertifikatdienste) über die Regel „Microsoft.InfromADUserAccountEnabled.xxx“ mit dem MV-Objekt verbunden ist.|
| Verbundbenutzern oder Benutzern mit Pass-Through-Authentifizierung bzw. Kennworthashsynchronisierung, die versuchen, ihre Kennwörter zurückzusetzen, wird nach der Kennwortübermittlung ein Fehler angezeigt. Der Fehler deutet auf ein Dienstproblem hin. <br> <br> Darüber hinaus wird in den Ereignisprotokollen bei Vorgängen zur Kennwortzurücksetzung für den Azure AD Connect-Dienst möglicherweise ein Fehler mit dem Hinweis angezeigt, dass mehrere Übereinstimmungen gefunden wurden. | Dies deutet darauf hin, dass das Synchronisierungsmodul ermittelt hat, dass das MV-Objekt über die Regel „Microsoft.InfromADUserAccountEnabled.xxx“ mit mehr als einem AD CS-Objekt verbunden ist. Dies bedeutet, dass das Benutzerkonto in mehr als einer Gesamtstruktur über ein aktives Konto verfügt. Dieses Szenario wird für das Kennwortrückschreiben nicht unterstützt. |
| Bei Kennwortvorgängen treten Konfigurationsfehler auf. Das Anwendungsereignisprotokoll enthält den Azure AD Connect-Fehler 6329 mit dem Text „0x8023061f (The operation failed because password synchronization is not enabled on this Management Agent)“ (0x8023061f (Der Vorgang konnte nicht ausgeführt werden, da die Kennwortsynchronisierung für diesen Verwaltungs-Agent nicht aktiviert ist.)). | Dieser Fehler tritt auf, wenn die Azure AD Connect-Konfiguration geändert wird, um eine neue Active Directory-Gesamtstruktur hinzuzufügen (oder eine vorhandene Gesamtstruktur zu entfernen und erneut hinzuzufügen), nachdem die Funktion zum Kennwortrückschreiben bereits aktiviert wurde. Kennwortvorgänge für Benutzer in diesen kürzlich hinzugefügten Gesamtstrukturen sind nicht erfolgreich. Deaktivieren Sie zur Behebung dieses Problems das Kennwortrückschreibenfeature nach Abschluss der Konfigurationsänderungen an der Gesamtstruktur, und aktivieren Sie es anschließend wieder.
| SSPR_0029: Wir können Ihr Kennwort aufgrund eines Fehlers in Ihrer lokalen Konfiguration nicht zurücksetzen. Wenden Sie sich an Ihren Administrator, und bitten Sie ihn, nachzuforschen. | Problem: Es wurden alle erforderlichen Schritte zum Aktivieren des Kennwortrückschreibens ausgeführt, aber wenn Sie versuchen, ein Kennwort zu ändern, erhalten Sie die Meldung „SSPR_0029“: Ihre Organisation hat die lokale Konfiguration für die Kennwortzurücksetzung nicht ordnungsgemäß eingerichtet." Wenn Sie die Ereignisprotokolle auf dem Azure AD Connect-System überprüfen, wird angezeigt, dass den Anmeldeinformationen des Verwaltungs-Agents der Zugriff verweigert wurde. Mögliche Lösung: Verwenden Sie RSOP auf dem Azure AD Connect-System und Ihren Domänencontrollern, um festzustellen, ob die Richtlinie „Netzwerkzugriff: Clients einschränken, die Remoteaufrufe an SAM ausführen dürfen“ unter Computerkonfiguration > Windows-Einstellungen > Sicherheitseinstellungen > Lokale Richtlinien > Sicherheitsoptionen aktiviert ist. Bearbeiten Sie die Richtlinie so, dass auch das MSOL_XXXXXXX-Verwaltungskonto als zulässiger Benutzer enthalten ist. |

## <a name="password-writeback-event-log-error-codes"></a>Ereignisprotokoll-Fehlercodes für das Kennwortrückschreiben

Eine bewährte Methode bei der Problembehandlung für das Kennwortrückschreiben besteht darin, das Anwendungsereignisprotokoll auf Ihrem Azure AD Connect-Computer zu überprüfen. Dieses Ereignisprotokoll enthält Ereignisse aus zwei Quellen für das Kennwortrückschreiben. Die Quelle *PasswordResetService* beschreibt Vorgänge und Probleme im Zusammenhang mit dem Kennwortrückschreiben. Die Quelle *ADSync* beschreibt Vorgänge und Probleme im Zusammenhang mit dem Zurücksetzen von Kennwörtern in Ihrer Active Directory Domain Services-Umgebung.

### <a name="if-the-source-of-the-event-is-adsync"></a>Ereignisquelle „ADSync“

| Code | Name oder Meldung | BESCHREIBUNG |
| --- | --- | --- |
| 6329 | BAIL: MMS(4924) 0x80230619: „A restriction prevents the password from being changed to the current one specified.“ (BAIL: MMS(4924) 0x80230619: Eine Einschränkung verhindert, dass das Kennwort in die aktuelle Angabe geändert wird.) | Dieses Ereignis tritt auf, wenn der Dienst für das Kennwortrückschreiben versucht, ein Kennwort für Ihr lokales Verzeichnis festzulegen, das die in der Domäne geltenden Anforderungen im Hinblick auf Alter, Verlauf, Komplexität oder Filterung für Kennwörter nicht erfüllt. <br> <br> Wenn Sie ein Mindestkennwortalter festgelegt haben und das Kennwort kürzlich geändert wurde, können Sie das Kennwort erst wieder ändern, wenn es das für Ihre Domäne festgelegte Kennwortalter erreicht hat. Zu Testzwecken sollte das Mindestalter auf 0 festgelegt werden. <br> <br> Wenn Sie Anforderungen für den Kennwortverlauf aktiviert haben, müssen Sie ein Kennwort auswählen, das die letzten *n* Male nicht verwendet wurde, wobei *n* für die Einstellung des Kennwortverlaufs steht. Wenn Sie ein Kennwort auswählen, das die letzten *n* Male verwendet wurde, tritt ein Fehler auf. Zu Testzwecken sollte der Kennwortverlauf auf 0 festgelegt werden. <br> <br> Wenn Anforderungen an die Kennwortkomplexität gelten, werden diese erzwungen, wenn der Benutzer versucht, ein Kennwort zu ändern oder zurückzusetzen. <br> <br> Wenn Sie Kennwortfilter aktiviert haben und ein Benutzer versucht, ein Kennwort auszuwählen, das nicht den Filterkriterien entspricht, tritt bei der Kennwortänderung oder -zurücksetzung ein Fehler auf. |
| 6329 | MMS(3040): admaexport.cpp(2837): Die LDAP-Kennwortrichtliniensteuerung ist auf dem Server nicht vorhanden. | Dieses Problem tritt auf, wenn die Steuerung „LDAP_SERVER_POLICY_HINTS_OID“ (1.2.840.113556.1.4.2066) auf den DCs nicht aktiviert ist. Die Steuerung muss aktiviert werden, um das Kennwortrückschreiben-Feature verwenden zu können. Hierzu müssen die DCs mindestens über Windows Server 2008 R2 oder höher verfügen. |
| HR 8023042 | Synchronization Engine returned an error hr=80230402, message=An attempt to get an object failed because there are duplicated entries with the same anchor. (Das Synchronisierungsmodul hat einen Fehler zurückgegeben: hr=80230402, message=Fehler beim Abrufen eines Objekts aufgrund von doppelten Einträgen mit dem gleichen Anker.) | Dieser Fehler tritt auf, wenn die gleiche Benutzer-ID in mehreren Domänen aktiviert ist. So tritt er beispielsweise auf, wenn Sie Konto- und Ressourcengesamtstrukturen synchronisieren und dabei in den einzelnen Gesamtstrukturen die gleiche Benutzer-ID vorhanden und aktiviert ist. <br> <br> Der Fehler kann auch auftreten, wenn Sie ein nicht eindeutiges Ankerattribut (etwa einen Alias oder UPN) verwenden und sich zwei Benutzer dieses Ankerattribut teilen. <br> <br> Stellen Sie zur Behebung dieses Problems sicher, dass kein Benutzer in Ihren Domänen doppelt vorhanden ist und für jeden Benutzer ein eindeutiges Ankerattribut verwendet wird. |

### <a name="if-the-source-of-the-event-is-passwordresetservice"></a>Ereignisquelle „PasswordResetService“

| Code | Name oder Meldung | BESCHREIBUNG |
| --- | --- | --- |
| 31001 | PasswordResetStart | Dieses Ereignis gibt an, dass der lokale Dienst eine aus der Cloud stammende Anforderung zur Kennwortzurücksetzung für einen Verbundbenutzer oder einen Benutzer mit Pass-Through-Authentifizierung bzw. Kennworthashsynchronisierung erkannt hat. Hierbei handelt es sich jeweils um das erste Ereignis eines Rückschreibvorgangs für Kennwortzurücksetzungen. |
| 31002 | PasswordResetSuccess | Dieses Ereignis gibt an, dass ein Benutzer im Zuge einer Kennwortzurücksetzung ein neues Kennwort ausgewählt hat. Das Kennwort wurde überprüft und erfüllt die Kennwortanforderungen des Unternehmens. Es wurde erfolgreich in die lokale Active Directory-Umgebung zurückgeschrieben. |
| 31003 | PasswordResetFail | Dieses Ereignis gibt an, dass ein Benutzer ein Kennwort ausgewählt hat und das Kennwort erfolgreich in der lokalen Umgebung eingegangen ist. Aufgrund eines Fehlers konnte das Kennwort jedoch nicht in der lokalen Active Directory-Umgebung festgelegt werden. Dieser Fehler kann verschiedene Ursachen haben: <br><ul><li>Das Benutzerkennwort entspricht nicht den für die Domäne geltenden Anforderungen in Bezug auf Alter, Verlauf, Komplexität oder Kennwortfilter. Erstellen Sie zur Behebung dieses Problems ein neues Kennwort.</li><li>Das ADMA-Dienstkonto verfügt nicht über die Berechtigungen, die zum Festlegen des neuen Kennworts für das betreffende Benutzerkonto erforderlich sind.</li><li>Das Benutzerkonto befindet sich in einer geschützten Gruppe (beispielsweise in der Gruppe „Domänenadministrator“ oder „Unternehmensadministrator“). Für diese Gruppen ist keine Kennwortverwaltung zulässig.</li></ul>|
| 31004 | OnboardingEventStart | Dieses Ereignis tritt auf, wenn Sie das Kennwortrückschreiben mit Azure AD Connect aktivieren und die Integration Ihrer Organisation in den Webdienst für das Kennwortrückschreiben gestartet wurde. |
| 31005 | OnboardingEventSuccess | Dieses Ereignis gibt an, dass der Integrationsvorgang erfolgreich war und das Kennwortrückschreibenfeature einsatzbereit ist. |
| 31006 | ChangePasswordStart | Dieses Ereignis gibt an, dass der lokale Dienst eine aus der Cloud stammende Anforderung zur Kennwortänderung für einen Verbundbenutzer oder einen Benutzer mit Pass-Through-Authentifizierung bzw. Kennworthashsynchronisierung erkannt hat. Hierbei handelt es sich jeweils um das erste Ereignis eines Rückschreibvorgangs für Kennwortänderungen. |
| 31007 | ChangePasswordSuccess | Dieses Ereignis gibt an, dass ein Benutzer im Zuge einer Kennwortänderung ein neues Kennwort angefordert hat. Es wurde ermittelt, dass dieses Kennwort die Unternehmensanforderungen für Kennwörter erfüllt, und das Kennwort wurde erfolgreich in die lokale Active Directory-Umgebung zurückgeschrieben. |
| 31008 | ChangePasswordFail | Dieses Ereignis gibt an, dass ein Benutzer ein Kennwort ausgewählt hat und dass dieses Kennwort erfolgreich in der lokalen Umgebung eingegangen ist. Beim Versuch, das Kennwort in der lokalen Active Directory-Umgebung festzulegen, ist jedoch ein Fehler aufgetreten. Dieser Fehler kann verschiedene Ursachen haben: <br><ul><li>Das Benutzerkennwort entspricht nicht den für die Domäne geltenden Anforderungen in Bezug auf Alter, Verlauf, Komplexität oder Kennwortfilter. Erstellen Sie zur Behebung dieses Problems ein neues Kennwort.</li><li>Das ADMA-Dienstkonto verfügt nicht über die Berechtigungen, die zum Festlegen des neuen Kennworts für das betreffende Benutzerkonto erforderlich sind.</li><li>Das Benutzerkonto befindet sich in einer geschützten Gruppe, z. B. „Domänenadministratoren“ oder „Unternehmensadministratoren“, für die eine Kennwortverwaltung nicht möglich ist.</li></ul> |
| 31009 | ResetUserPasswordByAdminStart | Der lokale Dienst hat eine Anforderung zur Kennwortzurücksetzung für einen Verbundbenutzer oder einen Benutzer mit Pass-Through-Authentifizierung bzw. Kennworthashsynchronisierung ermittelt, die vom Administrator im Namen eines Benutzers initiiert wurde. Hierbei handelt es sich jeweils um das erste Ereignis eines Rückschreibvorgangs für Kennwortzurücksetzungen, die von einem Administrator initiiert werden. |
| 31010 | ResetUserPasswordByAdminSuccess | Der Administrator hat im Zuge eines vom Administrator initiierten Kennwortzurücksetzungsvorgangs ein neues Kennwort ausgewählt. Das Kennwort wurde überprüft und erfüllt die Kennwortanforderungen des Unternehmens. Es wurde erfolgreich in die lokale Active Directory-Umgebung zurückgeschrieben. |
| 31011 | ResetUserPasswordByAdminFail | Der Administrator hat im Namen eines Benutzers ein Kennwort ausgewählt. Das Kennwort ist erfolgreich in der lokalen Umgebung eingegangen. Aufgrund eines Fehlers konnte das Kennwort jedoch nicht in der lokalen Active Directory-Umgebung festgelegt werden. Dieser Fehler kann verschiedene Ursachen haben: <br><ul><li>Das Benutzerkennwort entspricht nicht den für die Domäne geltenden Anforderungen in Bezug auf Alter, Verlauf, Komplexität oder Kennwortfilter. Versuchen Sie es mit einem neuen Kennwort, um dieses Problem zu beheben.</li><li>Das ADMA-Dienstkonto verfügt nicht über die Berechtigungen, die zum Festlegen des neuen Kennworts für das betreffende Benutzerkonto erforderlich sind.</li><li>Das Benutzerkonto befindet sich in einer geschützten Gruppe, z. B. „Domänenadministratoren“ oder „Unternehmensadministratoren“, für die eine Kennwortverwaltung nicht möglich ist.</li></ul>  |
| 31012 | OffboardingEventStart | Dieses Ereignis tritt auf, wenn Sie das Kennwortrückschreiben mit Azure AD Connect deaktivieren. Es gibt an, dass der Vorgang zum Entfernen Ihrer Organisation aus dem Webdienst für das Kennwortrückschreiben gestartet wurde. |
| 31013| OffboardingEventSuccess| Dieses Ereignis gibt an, dass der Vorgang zum Entfernen erfolgreich war und die Kennwortrückschreibenfunktion erfolgreich deaktiviert wurde. |
| 31014| OffboardingEventFail| Dieses Ereignis gibt an, dass der Vorgang zum Entfernen nicht erfolgreich war. Dies kann auf einen Berechtigungsfehler für das Cloudadministratorkonto oder für das lokale Administratorkonto zurückzuführen sein, das bei der Konfiguration angegeben wurde. Der Fehler kann auch auftreten, wenn Sie beim Deaktivieren des Kennwortrückschreibens versuchen, einen globalen Cloudverbundadministrator zu verwenden. Prüfen Sie zur Behebung dieses Problems Ihre Administratorberechtigungen, und vergewissern Sie sich, dass Sie zum Konfigurieren des Kennwortrückschreibens kein Verbundkonto verwenden.|
| 31015| WriteBackServiceStarted| Dieses Ereignis gibt an, dass der Dienst zum Kennwortrückschreiben erfolgreich gestartet wurde. Er kann nun Kennwortverwaltungsanforderungen aus der Cloud entgegennehmen.|
| 31016| WriteBackServiceStopped| Dieses Ereignis gibt an, dass der Dienst zum Kennwortrückschreiben beendet wurde. Kennwortverwaltungsanforderungen aus der Cloud haben keinen Erfolg.|
| 31017| AuthTokenSuccess| Dieses Ereignis weist darauf hin, dass für den globalen Administrator, der während der Azure AD Connect-Einrichtung angegeben wurde, erfolgreich ein Autorisierungstoken abgerufen wurde, um den Vorgang zur Integration oder zum Entfernen zu starten.|
| 31018| KeyPairCreationSuccess| Dieses Ereignis gibt an, dass der Kennwortverschlüsselungsschlüssel erfolgreich erstellt wurde. Der Schlüssel dient zum Verschlüsseln von Kennwörtern aus der Cloud, die an Ihre lokale Umgebung gesendet werden.|
| 31034| ServiceBusListenerError| Dieses Ereignis gibt an, dass beim Herstellen einer Verbindung mit dem Service Bus-Listener Ihres Mandanten ein Fehler aufgetreten ist. Wenn die Fehlermeldung „Das Remotezertifikat ist laut Validierungsverfahren ungültig“ enthält, stellen Sie sicher, dass der Azure AD Connect-Server wie unter [TLS-Zertifikatänderungen für Azure](../../security/fundamentals/tls-certificate-changes.md) beschrieben über alle erforderlichen Stammzertifizierungsstellen verfügt. |
| 31044| PasswordResetService| Dieses Ereignis gibt an, dass das Kennwortrückschreiben nicht funktioniert. Der Service Bus lauscht aus Redundanzgründen auf zwei separaten Relays auf Anforderungen. Jede Relayverbindung wird von einem eindeutigen Diensthost verwaltet. Der Rückschreibeclient gibt einen Fehler zurück, wenn einer der Diensthosts nicht ausgeführt wird.|
| 32000| UnknownError| Dieses Ereignis gibt an, dass während eines Kennwortverwaltungsvorgangs ein unbekannter Fehler aufgetreten ist. Lesen Sie den Ausnahmetext im Ereignis, um weitere Informationen zu erhalten. Deaktivieren Sie bei Problemen das Kennwortrückschreiben, und aktivieren Sie es anschließend wieder. Sollte das Problem weiterhin auftreten, senden Sie eine Kopie des Ereignisprotokolls zusammen mit der angegebenen Nachverfolgungs-ID, wenn Sie eine Supportanfrage öffnen.|
| 32001| ServiceError| Dieses Ereignis gibt an, dass beim Herstellen einer Verbindung mit dem Kennwortzurücksetzungsdienst ein Fehler aufgetreten ist. Dieser Fehler tritt in der Regel auf, wenn der lokale Dienst keine Verbindung mit dem Webdienst für die Kennwortzurücksetzung herstellen konnte.|
| 32002| ServiceBusError| Dieses Ereignis gibt an, dass beim Herstellen einer Verbindung mit der Service Bus-Instanz Ihres Mandanten ein Fehler aufgetreten ist. Dieser Fall kann eintreten, wenn Sie in Ihrer lokalen Umgebung ausgehende Verbindungen blockieren. Prüfen Sie Ihre Firewall, und vergewissern Sie sich, dass Verbindungen über TCP 443 und mit https://ssprdedicatedsbprodncu.servicebus.windows.net möglich sind. Versuchen Sie es anschließend erneut. Sollten weiterhin Probleme auftreten, deaktivieren Sie das Kennwortrückschreiben, und aktivieren Sie es anschließend wieder.|
| 32003| InPutValidationError| Dieses Ereignis weist darauf hin, dass die an die Webdienst-API übergebene Eingabe ungültig war. Wiederholen Sie den Vorgang.|
| 32004| DecryptionError| Dieses Ereignis weist darauf hin, dass beim Entschlüsseln des aus der Cloud empfangenen Kennworts ein Fehler aufgetreten ist. Dieser Fall kann eintreten, wenn die Entschlüsselungsschlüssel von Clouddienst und lokaler Umgebung nicht übereinstimmen. Deaktivieren Sie zur Behebung dieses Problems das Kennwortrückschreiben in der lokalen Umgebung, und aktivieren Sie es anschließend wieder.|
| 32005| ConfigurationError| Während der Integration wurden mandantenspezifische Informationen in einer Konfigurationsdatei in Ihrer lokalen Umgebung gespeichert. Dieses Ereignis gibt an, dass beim Speichern dieser Datei ein Fehler aufgetreten ist oder dass beim Starten des Diensts ein Fehler beim Lesen der Datei aufgetreten ist. Deaktivieren Sie zur Behebung dieses Problems das Kennwortrückschreiben, und aktivieren Sie es anschließend wieder, um die Neuerstellung dieser Konfigurationsdatei zu erzwingen.|
| 32007| OnBoardingConfigUpdateError| Während der Integration werden Daten vom Clouddienst für die Kennwortzurücksetzung an den lokalen Dienst für die Kennwortzurücksetzung gesendet. Diese Daten werden dann in eine Datei im Arbeitsspeicher geschrieben, bevor sie an den Synchronisierungsdienst gesendet und sicher auf der Festplatte gespeichert werden. Dieses Ereignis gibt an, dass beim Schreiben oder Aktualisieren der Daten im Arbeitsspeicher ein Problem aufgetreten ist. Deaktivieren Sie zur Behebung dieses Problems das Kennwortrückschreiben, und aktivieren Sie es anschließend wieder, um die Neuerstellung dieser Konfigurationsdatei zu erzwingen.|
| 32008| ValidationError| Dieses Ereignis gibt an, dass eine ungültige Antwort vom Webdienst für die Kennwortzurücksetzung empfangen wurde. Deaktivieren Sie zur Behebung dieses Problems das Kennwortrückschreiben, und aktivieren Sie es anschließend wieder.|
| 32009| AuthTokenError| Dieses Ereignis gibt an, dass kein Autorisierungstoken für das globale Administratorkonto abgerufen werden konnte, das bei der Einrichtung von Azure AD Connect angegeben wurde. Dieser Fehler kann durch einen ungültigen Benutzernamen oder ein ungültiges Kennwort verursacht werden, der bzw. das für das globale Administratorkonto angegeben wurde. Er kann aber auch auftreten, wenn es sich bei dem globalen Administratorkonto um ein Verbundadministratorkonto handelt. Wiederholen Sie zur Behebung dieses Problems die Konfiguration mit dem korrekten Benutzernamen und Kennwort, und vergewissern Sie sich, dass es sich bei dem Administratorkonto um ein verwaltetes Konto (nur Cloud oder Kennwortsynchronisierung) handelt.|
| 32010| CryptoError| Dieses Ereignis gibt an, dass beim Generieren des Kennwortverschlüsselungsschlüssels oder beim Entschlüsseln eines vom Clouddienst empfangenen Kennworts ein Fehler aufgetreten ist. Dieser Fehler ist meist auf ein Problem mit der Umgebung zurückzuführen. Prüfen Sie die Details im Ereignisprotokoll, um weitere Informationen zum aufgetretenen Problem und zu dessen Lösung zu erhalten. Sie können auch versuchen, das Kennwortrückschreiben zu deaktivieren und wieder zu aktivieren.|
| 32011| OnBoardingServiceError| Dieses Ereignis gibt an, dass der lokale Dienst nicht ordnungsgemäß mit dem Webdienst für die Kennwortzurücksetzung kommunizieren konnte, um den Integrationsvorgang zu initiieren. Dieses Problem kann auf eine Firewallregel oder auf Probleme beim Abrufen eines Authentifizierungstokens für Ihren Mandanten zurückzuführen sein. Vergewissern Sie sich zur Behebung dieses Problems, dass ausgehende Verbindungen über TCP 443 und TCP 9350-9354 oder mit https://ssprdedicatedsbprodncu.servicebus.windows.net möglich sind. Stellen Sie außerdem sicher, dass es sich bei dem für die Integration verwendeten Azure AD-Administratorkonto nicht um ein Verbundkonto handelt.|
| 32013| OffBoardingError| Dieses Ereignis gibt an, dass der lokale Dienst nicht ordnungsgemäß mit dem Webdienst für die Kennwortzurücksetzung kommunizieren konnte, um den Vorgang zum Entfernen zu initiieren. Dieses Problem kann auf eine Firewallregel oder auf Probleme beim Abrufen eines Autorisierungstokens für Ihren Mandanten zurückzuführen sein. Vergewissern Sie sich zur Behebung dieses Problems, dass ausgehende Verbindungen über Port 443 oder mit https://ssprdedicatedsbprodncu.servicebus.windows.net möglich sind, und dass es sich bei dem für das Offboarding verwendeten Azure Active Directory-Administratorkonto nicht um ein Verbundkonto handelt.|
| 32014| ServiceBusWarning| Dieses Ereignis gibt an, dass erneut versucht werden musste, eine Verbindung mit der Service Bus-Instanz Ihres Mandanten herzustellen. Unter normalen Umständen ist das kein Problem, aber wenn dieses Ereignis häufiger auftritt, sollten Sie ggf. Ihre Netzwerkverbindung mit Service Bus prüfen – insbesondere bei Verwendung einer Verbindung mit hoher Wartezeit oder geringer Bandbreite.|
| 32015| ReportServiceHealthError| Zur Überwachung der Integrität des Diensts für das Kennwortrückschreiben werden alle fünf Minuten Taktdaten an den Webdienst für die Kennwortzurücksetzung gesendet. Dieses Ereignis weist darauf hin, dass beim Senden dieser Integritätsinformationen zurück an den Cloudwebdienst ein Fehler aufgetreten ist. Diese Integritätsinformationen enthalten keine personenbezogenen Daten. Es handelt sich lediglich um ein Taktsignal und grundlegende Dienststatistiken, um Dienststatusinformationen in der Cloud bereitzustellen.|
| 33001| ADUnKnownError| Dieses Ereignis gibt an, dass von Active Directory ein unbekannter Fehler zurückgegeben wurde. Prüfen Sie das Ereignisprotokoll des Azure AD Connect-Servers auf Ereignisse mit der Quelle „ADSync“, um weitere Informationen zu erhalten.|
| 33002| ADUserNotFoundError| Dieses Ereignis weist darauf hin, dass der Benutzer, der ein Kennwort zurücksetzen oder ändern möchte, nicht im lokalen Verzeichnis gefunden wurde. Dieser Fehler kann auftreten, wenn der Benutzer lokal, aber nicht in der Cloud gelöscht wurde. Er kann aber auch auf ein Problem mit der Synchronisierung zurückzuführen sein. Prüfen Sie Ihre Synchronisierungsprotokolle und die Details zu den letzten Synchronisierungsvorgängen, um weitere Informationen zu erhalten.|
| 33003| ADMutliMatchError| Wenn eine Anforderung zur Kennwortzurücksetzung oder -änderung aus der Cloud stammt, wird mithilfe des bei der Einrichtung von Azure AD Connect angegeben Cloudankers ermittelt, wie diese Anforderung mit einem Benutzer in Ihrer lokalen Umgebung verknüpft wird. Dieses Ereignis weist darauf hin, dass zwei Benutzer mit demselben Cloudankerattribut im lokalen Verzeichnis gefunden wurden. Prüfen Sie Ihre Synchronisierungsprotokolle und die Details zu den letzten Synchronisierungsvorgängen, um weitere Informationen zu erhalten.|
| 33004| ADPermissionsError| Dieses Ereignis gibt an, dass das ADMA-Dienstkonto (Active Directory Management Agent, Active Directory-Verwaltungs-Agent) über keine ausreichenden Berechtigungen verfügt, um für das betreffende Konto ein neues Kennwort festzulegen. Stellen Sie sicher, dass das ADMA-Konto in der Gesamtstruktur des Benutzers über die Berechtigungen zum Zurücksetzen von Kennwörtern für alle Objekte in der Gesamtstruktur verfügt. Weitere Informationen zum Festlegen von Berechtigungen finden Sie in Schritt 4: Einrichten der geeigneten Active Directory-Berechtigungen. Dieser Fehler kann auch auftreten, wenn das „AdminCount“-Attribut des Benutzers auf „1“ festgelegt ist.|
| 33005| ADUserAccountDisabled| Dieses Ereignis gibt an, dass versucht wurde, ein Kennwort für ein lokal deaktiviertes Konto zurückzusetzen oder zu ändern. Aktivieren Sie das Konto, und versuchen Sie es erneut.|
| 33006| ADUserAccountLockedOut| Dieses Ereignis gibt an, dass versucht wurde, ein Kennwort für ein lokal gesperrtes Konto zurückzusetzen oder zu ändern. Sperrungen können auftreten, wenn ein Benutzer innerhalb eines kurzen Zeitraums zu häufig versucht hat, ein Kennwort zu ändern oder zurückzusetzen. Entsperren Sie das Konto, und versuchen Sie es erneut.|
| 33007| ADUserIncorrectPassword| Dieses Ereignis weist darauf hin, dass der Benutzer beim Ausführen einer Kennwortänderung ein falsches aktuelles Kennwort angegeben hat. Geben Sie das richtige aktuelle Kennwort an, und versuchen Sie es erneut.|
| 33008| ADPasswordPolicyError| Dieses Ereignis tritt auf, wenn der Dienst für das Kennwortrückschreiben versucht, ein Kennwort für Ihr lokales Verzeichnis festzulegen, das die in der Domäne geltenden Anforderungen im Hinblick auf Alter, Verlauf, Komplexität oder Filterung für Kennwörter nicht erfüllt. <br> <br> Wenn Sie ein Mindestkennwortalter festgelegt haben und das Kennwort kürzlich geändert wurde, können Sie das Kennwort erst wieder ändern, wenn es das für Ihre Domäne festgelegte Kennwortalter erreicht hat. Zu Testzwecken sollte das Mindestalter auf 0 festgelegt werden. <br> <br> Wenn Sie Anforderungen für den Kennwortverlauf festgelegt haben, müssen Sie ein Kennwort auswählen, das die letzten *n* Male nicht verwendet wurde, wobei *n* für die Einstellung des Kennwortverlaufs steht. Wenn Sie ein Kennwort auswählen, das die letzten *n* Male verwendet wurde, tritt ein Fehler auf. Zu Testzwecken sollte der Kennwortverlauf auf 0 festgelegt werden. <br> <br> Wenn Anforderungen an die Kennwortkomplexität gelten, werden diese erzwungen, wenn der Benutzer versucht, ein Kennwort zu ändern oder zurückzusetzen. <br> <br> Wenn Sie Kennwortfilter aktiviert haben und ein Benutzer versucht, ein Kennwort auszuwählen, das nicht den Filterkriterien entspricht, tritt bei der Kennwortänderung oder -zurücksetzung ein Fehler auf.|
| 33009| ADConfigurationError| Dieses Ereignis gibt an, dass aufgrund eines Konfigurationsproblems mit Active Directory ein Fehler beim Zurückschreiben eines Kennworts in das lokale Verzeichnis aufgetreten ist. Überprüfen Sie das Anwendungsereignisprotokoll für den Azure AD Connect-Computer auf Meldungen des ADSync-Diensts, um weitere Informationen zur Fehlerursache zu erhalten.|

## <a name="azure-ad-forums"></a>Azure AD-Foren

Wenn Sie allgemeine Fragen zu Azure AD und zur Self-Service-Kennwortzurücksetzung haben, können Sie die Community auf der [Microsoft-Frageseite „Q & A“ für Azure Active Directory](/answers/topics/azure-active-directory.html) um Unterstützung bitten. Zu den Mitgliedern der Community gehören Techniker, Produktmanager MVPs und andere IT-Experten.

## <a name="contact-microsoft-support"></a>Microsoft-Support kontaktieren

Wenn Sie keine Antwort auf ein Problem finden, stehen Ihnen unsere Supportteams immer für weitere Unterstützung zur Verfügung.

Damit wir Sie bestmöglich unterstützen können, geben Sie bitte so viele Details wie möglich an, wenn Sie eine Anfrage erstellen. Hierzu gehören die folgenden Angaben:

* **Allgemeine Beschreibung des Fehlers**: Welcher Fehler liegt vor? Welches Verhalten haben Sie festgestellt? Wie können wir den Fehler reproduzieren? Geben Sie bitte so viele Details wie möglich an.
* **Seite**: Auf welcher Seite befanden Sie sich, als der Fehler aufgetreten ist? Geben Sie möglichst die URL an, und erstellen Sie einen Screenshot der Seite.
* **Unterstützungscode**: Welcher Unterstützungscode wurde generiert, als der Fehler aufgetreten ist?
   * Reproduzieren Sie zum Ermitteln des Unterstützungscodes den Fehler, klicken Sie im unteren Bildschirmbereich auf den Link **Unterstützungscode**, und senden Sie die generierte GUID an den Supportmitarbeiter.

    :::image type="content" source="./media/troubleshoot-sspr-writeback/view-support-code.png" alt-text="Der Unterstützungscode befindet sich unten rechts im Webbrowserfenster.":::

  * Wenn Sie sich auf einer Seite ohne Unterstützungscode befinden, drücken Sie F12, suchen Sie nach der SID und der CID, und senden Sie beide Ergebnisse an den Supportmitarbeiter.
* **Datum, Uhrzeit und Zeitzone**: Geben Sie das genaue Datum und die exakte Uhrzeit des Fehlers an (*einschließlich der Zeitzone*).
* **Benutzer-ID**: Bei welchem Benutzer ist der Fehler aufgetreten? Ein Beispiel hierfür ist *Benutzer\@contoso.com*.
   * Handelt es sich um einen Verbundbenutzer?
   * Ist dies ein Benutzer mit Pass-Through-Authentifizierung?
   * Handelt es sich um einen Benutzer mit Kennworthashsynchronisierung?
   * Handelt es sich um einen reinen Cloudbenutzer?
* **Lizenzierung**: Ist dem Benutzer eine Azure AD-Lizenz zugewiesen?
* **Anwendungsereignisprotokoll**: Wenn Sie das Kennwortrückschreiben verwenden und der Fehler in Ihrer lokalen Infrastruktur auftritt, fügen Sie eine Kopie des Anwendungsereignisprotokolls von Ihrem Azure AD Connect-Server als ZIP-Datei bei.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu SSPR finden Sie unter [So funktioniert's: Self-Service-Kennwortzurücksetzung in Azure AD](concept-sspr-howitworks.md) oder [Funktionsweise des Rückschreibens von Self-Service-Kennwortzurücksetzungen in Azure AD](concept-sspr-writeback.md).
