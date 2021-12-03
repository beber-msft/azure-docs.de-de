---
title: Weiterleiten von Warnungsinformation
description: Sie können Weiterleitungsregeln verwenden, um Warnungsinformation an Partnersysteme zu senden.
ms.date: 11/09/2021
ms.topic: how-to
ms.openlocfilehash: d863ab9de5e030ddd54ba1b4b40efc01e844f419
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132278852"
---
# <a name="forward-alert-information"></a>Weiterleiten von Warnungsinformation

Sie können Warnmeldungen an Partner, die Microsoft Defender für IoT integrieren, an Syslog-Server, an E-Mail-Adressen und mehr senden. Mithilfe von Weiterleitungsregeln können Sie Warnungsinformation schnell an Sicherheitsbeteiligte senden.

Definieren Sie Kriterien, nach denen eine Weiterleitungsregel ausgelöst werden soll. Durch Verwenden von Kriterien für Weiterleitungsregeln können Sie das Volumen der vom Sensor an externe Systeme gesendeten Informationen genau bestimmen und verwalten.

Syslog und andere Standardweiterleitungsaktionen werden mit Ihrem System ausgeliefert. Weitere Weiterleitungsaktionen können verfügbar werden, wenn Sie mit Partneranbietern wie Microsoft Sentinel, ServiceNow oder Splunk zusammenarbeiten.

:::image type="content" source="media/how-to-work-with-alerts-sensor/alert-information-screen.png" alt-text="Warnungsinformationen.":::

Defender für IoT-Administratoren verfügen über die Berechtigung, Weiterleitungsregeln zu verwenden.

## <a name="about-forwarded-alert-information"></a>Informationen zu weitergeleiteten Warnungsinformation

Warnungen informieren über ein umfangreiches Spektrum an Sicherheits- und Betriebsereignissen. Beispiel:

- Datum und Uhrzeit der Warnung

- Engine, die das Ereignis erkannt hat

- Warnungstitel und beschreibende Meldung

- Schweregrad der Warnung

- Quell- und -Zielname und IP-Adresse

- Verdächtiger Datenverkehr erkannt

:::image type="content" source="media/how-to-work-with-alerts-sensor/address-scan-detected-screen.png" alt-text="Adressüberprüfung erkannt.":::

Wenn Weiterleitungsregeln erstellt wurden, werden relevante Informationen an Partnersysteme gesendet.

## <a name="about-forwarding-rules-and-certificates"></a>Informationen zu Weiterleitungsregeln und Zertifikaten

Bestimmte Weiterleitungsregeln ermöglichen die Verschlüsselung und Zertifikatüberprüfung zwischen dem Sensor oder der lokalen Verwaltungskonsole sowie dem Server des integrierten Anbieters.

In diesen Fällen ist der Sensor oder die lokale Verwaltungskonsole der Client und Initiator der Sitzung.  Die Zertifikate werden normalerweise vom Server empfangen, oder sie verwenden die asymmetrische Verschlüsselung, bei der ein bestimmtes Zertifikat zum Einrichten der Integration bereitgestellt wird.

Ihr Defender für IoT-System wurde so eingerichtet, dass entweder Zertifikate überprüft oder die Zertifikatüberprüfung ignoriert wird.  Informationen zum Aktivieren und Deaktivieren der Überprüfung finden Sie unter [Informationen zur Zertifikatüberprüfung](how-to-deploy-certificates.md#about-certificate-validation).

Wenn die Überprüfung aktiviert wurde und das Zertifikat nicht überprüft werden kann, wird die Kommunikation zwischen Defender für IoT und dem Server angehalten.  Der Sensor zeigt eine Fehlermeldung zum Überprüfungsfehler an.  Wenn die Überprüfung deaktiviert wurde und das Zertifikat ungültig ist, wird die Kommunikation weiterhin durchgeführt.

Die folgenden Weiterleitungsregeln ermöglichen die Verschlüsselung und Zertifikatüberprüfung:
- Syslog CEF
- Microsoft Sentinel
- QRadar

## <a name="create-forwarding-rules"></a>Erstellen von Weiterleitungsregeln

**So erstellen Sie eine neue Weiterleitungsregel für einen Sensor**

1. Melden Sie sich beim Sensor an.

1. Wählen Sie im Seitenmenü **Weiterleitung** aus.

1. Wählen Sie **Weiterleitungsregel erstellen** aus.

   :::image type="content" source="media/how-to-work-with-alerts-sensor/create-forwarding-rule-screen.png" alt-text="Erstellen Sie das Symbol „Weiterleitungsregel“.":::

1. Geben Sie einen Namen für die Weiterleitungsregel ein.

1. Wählen Sie den Schweregrad aus.

   Vorfälle, die mindestens diesen Schweregrad aufweisen, werden weitergeleitet. Wenn Sie z. B. **Minor** auswählen, werden Warnungen mit einem geringen Schweregrad und alle Warnungen mit höheren Schweregraden weitergeleitet. Die Schweregrade sind vordefiniert.

1. Wählen Sie die gewünschten Protokolle aus.

   Die Weiterleitungsregel nur auslösen, wenn der erkannte Datenverkehr über bestimmte Protokolle ausgeführt wurde. Wählen Sie die gewünschten oder alle Protokolle in der Dropdownliste aus.

1. Wählen Sie aus, für welche Engines die Regel gelten soll.

   Wählen Sie die erforderlichen oder alle Engines aus. Warnungen von ausgewählten Engines werden gesendet. 

1. Wählen Sie eine gewünschte Aktion aus, und geben Sie die für die ausgewählte Aktion erforderlichen Parameter ein.

   Weiterleitungsregelaktionen weisen den Sensor an, Warnungsinformation an Partnerhersteller oder Server weiterzuleiten. Sie können für jede Weiterleitungsregel mehrere Aktionen erstellen.

1. Fügen Sie bei Bedarf eine weitere Aktion hinzu.

1. Klicken Sie auf **Senden**.

### <a name="email-address-action"></a>E-Mail-Adressen-Aktion

Senden einer E-Mail, die die Warnungsinformationen enthält. Sie können pro Regel eine E-Mail-Adresse eingeben.

So richten Sie eine E-Mail für die Weiterleitungsregel ein

1. Geben Sie eine E-Mail-Adresse ein. Wenn Sie mehr als eine E-Mail-Adresse hinzufügen möchten, müssen Sie für jede E-Mail-Adresse eine weitere Aktion erstellen.

    :::image type="content" source="media/how-to-forward-alert-information-to-partners/forward-email.png" alt-text="Screenshot: Bildschirm „Warnungsweiterleitung“ zum Weiterleiten der Warnungen an eine E-Mail-Adresse":::

1. Geben Sie die Zeitzone für den Zeitstempel für die Warnungserkennung in SIEM ein.

1. Klicken Sie auf **Submit** (Senden).

### <a name="syslog-server-actions"></a>Syslog-Server-Aktionen

Die folgenden Formate werden unterstützt:

- Textnachrichten

- CEF-Nachrichten

- LEEF-Nachrichten

- Objektnachrichten

:::image type="content" source="media/how-to-work-with-alerts-sensor/create-actions-rule.png" alt-text="Erstellen von Weiterleitungsregelaktionen.":::

Legen Sie die folgenden Parameter fest:

- Syslog-Hostname und -Port.

- Protokoll: TCP und UDP.

- Zeitzone für den Zeitstempel für die Warnungserkennung in SIEM.

- TLS-Verschlüsselungszertifikatdatei und Schlüsseldatei für CEF-Server (optional).

:::image type="content" source="media/how-to-work-with-alerts-sensor/configure-encryption.png" alt-text="Konfigurieren der Verschlüsselung für die Weiterleitungsregel.":::

| Ausgabefelder für Syslog-Textnachrichten | BESCHREIBUNG |
|--|--|
| Datum und Uhrzeit | Datum und Uhrzeit des Empfangs der Informationen durch den Syslog-Servercomputer. |
| Priorität | User.Alert |
| Hostname | IP-Adresse des Sensors |
| Protocol | TCP oder UDP |
| `Message` | Sensor: Der Name des Sensors.<br /> Warnung: Der Titel der Warnung.<br /> Typ: Der Typ der Warnung. Kann **Protocol Violation**, **Policy Violation**, **Malware**, **Anomaly** oder **Operational** lauten.<br /> Schweregrad: Der Schweregrad der Warnung. Kann **Warning**, **Minor**, **Major** oder **Critical** lauten.<br /> Quelle: Der Namen des Quellgeräts.<br /> Quell-IP: Die IP-Adresse des Quellgeräts.<br /> Ziel: Der Name des Zielgeräts.<br /> Ziel-IP: Die IP-Adresse des Zielgeräts.<br /> Meldung: Der Meldungstext der Warnung.<br /> Warnungsgruppe: Die der Warnung zugeordnete Warnungsgruppe. |

| Syslog-Objektausgabe | BESCHREIBUNG |
|--|--|
| Datum und Uhrzeit | Datum und Uhrzeit des Empfangs der Informationen durch den Syslog-Servercomputer. |  
| Priorität | User.Alert |
| Hostname | Sensor-IP |
| `Message` | Sensorname: Der Name der Appliance. <br /> Zeitpunkt der Warnung: Der Zeitpunkt, zu dem die Warnung erkannt wurde: Kann von der Zeit des Syslog-Servercomputers abweichen und hängt von der Zeitzonenkonfiguration der Weiterleitungsregel ab. <br /> Titel der Warnung: Der Titel der Warnung. <br /> Warnmeldung: Der Meldungstext der Warnung. <br /> Schweregrad der Warnung: Der Schweregrad der Warnung: **Warning**, **Minor**, **Major** oder **Critical**. <br /> Warnungstyp: **Protocol Violation**, **Policy Violation**, **Malware**, **Anomaly** oder **Operational**. <br /> Protokoll: Protokoll der Warnung.  <br /> **Source_MAC**: IP-Adresse, Name, Hersteller oder Betriebssystem des Quellgeräts. <br /> Destination_MAC: IP-Adresse, Name, Hersteller oder Betriebssystem des Ziels. Wenn Daten fehlen, lautet der Wert **N/A**. <br /> alert_group: Die der Warnung zugeordnete Warnungsgruppe. |

| Syslog-CEF-Ausgabeformat | BESCHREIBUNG |
|--|--|
| Datum und Uhrzeit | Datum und Uhrzeit des Empfangs der Informationen durch den Syslog-Servercomputer. |
| Priorität | User.Alert |
| Hostname | IP-Adresse des Sensors |
| `Message` | CEF:0 <br />Microsoft Defender für IoT <br />Sensorname: Der Name der Sensor-Appliance. <br />Sensorversion <br />Titel der Warnung: Der Titel der Warnung. <br />msg: Der Meldungstext der Warnung. <br />Protokoll: Protokoll der Warnung. <br />Schweregrad:  **Warning**, **Minor**, **Major** oder **Critical**. <br />Typ:  **Protocol Violation**, **Policy Violation**, **Malware**, **Anomaly** oder **Operational**. <br /> start: Der Zeitpunkt, zu dem die Warnung erkannt wurde. <br />Kann von der Zeit des Syslog-Servercomputers abweichen und hängt von der Zeitzonenkonfiguration der Weiterleitungsregel ab. <br />src_ip: IP-Adresse des Quellgeräts.  <br />dst_ip: IP-Adresse des Zielgeräts.<br />cat: Die der Warnung zugeordnete Warnungsgruppe.  |

| Syslog-LEEF-Ausgabeformat | BESCHREIBUNG |
|--|--|
| Datum und Uhrzeit | Datum und Uhrzeit des Empfangs der Informationen durch den Syslog-Servercomputer. |  
| Priorität | User.Alert |
| Hostname | Sensor-IP |
| `Message` | Sensorname: Der Name der Microsoft Defender für IoT-Appliance. <br />LEEF:1.0 <br />Microsoft Defender für IoT <br />Sensor  <br />Sensorversion <br />Microsoft Defender für IoT-Warnung <br />Titel: Der Titel der Warnung. <br />msg: Der Meldungstext der Warnung. <br />Protokoll: Protokoll der Warnung.<br />Schweregrad:  **Warning**, **Minor**, **Major** oder **Critical**. <br />Typ: Der Typ der Warnung: **Protocol Violation**, **Policy Violation**, **Malware**, **Anomaly** oder **Operational**. <br />start: Der Zeitpunkt der Warnung.Der Zeitpunkt kann von der Uhrzeit des Syslog-Servercomputers abweichen. (Dies hängt von der Zeitzonenkonfiguration ab.) <br />src_ip: IP-Adresse des Quellgeräts.<br />dst_ip: IP-Adresse des Zielgeräts. <br />cat: Die der Warnung zugeordnete Warnungsgruppe. |

Nachdem Sie die Informationen eingegeben haben, wählen Sie **Senden** aus.

### <a name="webhook-server-action"></a>Webhookserveraktion

Senden Sie Warnungsinformationen an einen Webhookserver. Mit Webhookservern können Sie Integrationen einrichten, die Warnungsereignisse mit Defender für IoT abonnieren. Wenn ein Warnungsereignis ausgelöst wird, sendet die Verwaltungskonsole entsprechende HTTP POST-Nutzdaten an die konfigurierte URL des Webhooks. Webhooks können verwendet werden, um ein externes SIEM-System, SOAR-Systeme, Incident Management-Systeme usw. zu aktualisieren.

**So definieren Sie eine Webhookaktion**

1. Wählen Sie die Webhookaktion aus.

   :::image type="content" source="media/how-to-work-with-alerts-sensor/webhook.png" alt-text="Definieren Sie eine Webhookweiterleitungsregel.":::

1. Geben Sie die Serveradresse in das Feld **URL** ein.

1. Passen Sie in den Feldern **Schlüssel** und **Wert** den HTTP-Header mit einer Schlüssel- und Wertdefinition an. Dieses Feld darf nur Buchstaben, Ziffern, Bindestriche und Unterstriche enthalten. Werte dürfen nur ein führendes und/oder ein nachstehendes Leerzeichen enthalten.

1. Wählen Sie **Speichern** aus.

### <a name="webhook-extended"></a>Webhook erweitert

Erweiterte Webhooks können zum Senden von zusätzlichen Daten an den Endpunkt verwendet werden. Das erweiterte Feature enthält alle Informationen in der Webhookwarnung und fügt dem Bericht die folgenden Informationen hinzu:

- sensorID
- sensorName
- zoneID
- zoneName
- siteID
- siteName
- sourceDeviceAddress
- destinationDeviceAddress
- remediationSteps
- handled (verarbeitet)
- additionalInformation

**So definieren Sie eine erweiterte Webhookaktion**:

1. Wählen Sie in der Verwaltungskonsole im linken Bereich **Weiterleitung** aus.

1. Fügen Sie eine Weiterleitungsregel hinzu, indem Sie die Schaltfläche :::image type="icon" source="media/how-to-forward-alert-information-to-partners/add-icon.png" border="false"::: auswählen.

1. Fügen Sie einen aussagekräftigen Namen für die Weiterleitungswarnung hinzu.

1. Wählen Sie einen Schweregrad aus.

1. Wählen Sie **Hinzufügen**.

1. Wählen Sie im Dropdownfenster „Typ auswählen“ die Option **Webhook erweitert** aus.

   :::image type="content" source="media/how-to-forward-alert-information-to-partners/webhook-extended.png" alt-text="Wählen Sie im Dropdownmenü mit Optionen für „Typ auswählen“ die Option „Webhook erweitert“ aus.":::

1. Fügen Sie im Feld „URL“ die Endpunktdaten-URL ein.

1. (Optional) Passen Sie den HTTP-Header mit einer Schlüssel- und Wertdefinition an. Fügen Sie zusätzliche Header hinzu, indem Sie die Schaltfläche :::image type="icon" source="media/how-to-forward-alert-information-to-partners/add-header.png" border="false"::: auswählen.

1. Wählen Sie **Speichern** aus.

Nachdem die Weiterleitungsregel „Webhook erweitert“ konfiguriert wurde, können Sie die Warnung in der Verwaltungskonsole über den Bildschirm „Weiterleitung“ testen.

**So testen Sie die Weiterleitungsregel „Webhook erweitert“** :

1. Wählen Sie in der Verwaltungskonsole im linken Bereich **Weiterleitung** aus.

1. Wählen Sie die Schaltfläche **Ausführen** aus, um Ihre Warnung zu testen.

    :::image type="content" source="media/how-to-forward-alert-information-to-partners/run-button.png" alt-text="Wählen Sie die Schaltfläche „Ausführen“ aus, um Ihre Weiterleitungsregel zu testen.":::

Wenn die Erfolgsmeldung angezeigt wird, wissen Sie, dass die Weiterleitungsregel funktioniert.

:::image type="content" source="media/how-to-forward-alert-information-to-partners/success.png" alt-text="Wenn die Erfolgsmeldung angezeigt wird, hat die Warnung ordnungsgemäß funktioniert.":::

### <a name="netwitness-action"></a>NetWitness-Aktion

Senden von Warnungsinformationen an einen NetWitness-Server.

So definieren Sie NetWitness-Weiterleitungsparameter:

1. Geben Sie Informationen zum NetWitness-**Hostnamen** und -**Port** ein.

1. Geben Sie die Zeitzone für den Zeitstempel für die Warnungserkennung in SIEM ein.

   :::image type="content" source="media/how-to-work-with-alerts-sensor/add-timezone.png" alt-text="Fügen Sie der Weiterleitungsregel eine Zeitzone hinzu.":::

1. Klicken Sie auf **Submit** (Senden).

### <a name="integrated-vendor-actions"></a>Integrierte Hersteller-Aktionen

Vielleicht haben Sie Ihr System mit Sicherheits-, Gerätemanagement- oder anderen Branchenherstellern integriert. Diese Integrationen ermöglichen Folgendes:

  - Senden von Warnungsinformationen.

  - Senden von Gerätebestandsinformationen.

  - Kommunikation mit herstellerseitigen Firewalls.

Integrationen helfen bei der Überbrückung von zuvor abgeschotteten Sicherheitslösungen, der Verbesserung der Gerätesichtbarkeit sowie der Beschleunigung der systemweiten Reaktion zur schnelleren Risikominimierung.

Verwenden Sie den Abschnitt „Aktionen“, um die Anmeldeinformationen und andere Informationen einzugeben, die für die Kommunikation mit integrierten Herstellern erforderlich sind.

Details zum Einrichten von Weiterleitungsregeln für die Integrationen finden Sie in den entsprechenden Artikeln zur Partnerintegration.

## <a name="test-forwarding-rules"></a>Testen von Weiterleitungsregeln

Testen Sie die Verbindung zwischen dem Sensor und dem Partnerserver, der in Ihren Weiterleitungsregeln definiert ist:

1. Wählen Sie die Regel im Dialogfeld **Weiterleitungsregel** aus.

1. Wählen Sie das Feld **Mehr** aus.

1. Wählen Sie **Testnachricht senden** aus.

1. Wechseln Sie zu Ihrem Partnersystem, um zu überprüfen, ob die vom Sensor gesendeten Informationen empfangen wurden.

## <a name="edit-and-delete-forwarding-rules"></a>Bearbeiten und Löschen von Weiterleitungsregeln 

**So bearbeiten Sie eine Weiterleitungsregel**

- Wählen Sie auf dem Bildschirm **Weiterleitungsregel** die Option **Bearbeiten** aus dem Dropdownmenü **Mehr** aus. Nehmen Sie die gewünschten Änderungen vor, und wählen Sie **Senden** aus.

**So entfernen Sie eine Weiterleitungsregel**

- Wählen Sie auf dem Bildschirm **Weiterleitungsregel** die Option **Entfernen** aus dem Dropdownmenü **Mehr** aus. Wählen Sie im Dialogfeld **Warnung** die Option **OK** aus.

## <a name="forwarding-rules-and-alert-exclusion-rules"></a>Weiterleitungsregeln und Warnungsausschlussregeln

Der Administrator hat möglicherweise Warnungsausschlussregeln definiert. Diese Regeln ermöglichen Administratoren eine präzisere Kontrolle über die Auslösung von Warnungen, indem sie den Sensor anweisen, Warnungsereignisse basierend auf verschiedenen Parametern zu ignorieren. Diese Parameter können Geräteadressen, Warnungsnamen oder bestimmte Sensoren enthalten.

Das bedeutet, dass die von Ihnen definierten Weiterleitungsregeln möglicherweise aufgrund von Ausschlussregeln, die Ihr Administrator erstellt hat, ignoriert werden. Ausschlussregeln werden in der lokalen Verwaltungskonsole definiert.

## <a name="see-also"></a>Weitere Informationen

[Beschleunigen des Warnungsworkflows](how-to-accelerate-alert-incident-response.md)
