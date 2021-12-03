---
title: 'Windows 10-Roamingeinstellungen: Referenz – Azure Active Directory'
description: Einstellungen, für die unter Windows 10 mit ESR Roaming verwendet oder eine Sicherung erstellt wird
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: reference
ms.date: 02/12/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: dcd5f888622b1ce054f5fbb30db7b668f6501c70
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131049898"
---
# <a name="windows-10-roaming-settings-reference"></a>Windows 10-Roamingeinstellungen – Referenz

Die folgende Liste enthält die Einstellungen, für die unter Windows 10 Roaming verwendet oder eine Sicherung erstellt wird. 

## <a name="devices-and-endpoints"></a>Geräte und Endpunkte

Die folgende Tabelle enthält eine Zusammenfassung der Geräte und Kontotypen, die unter Windows 10 vom Framework für die Synchronisierung, Sicherung und Wiederherstellung unterstützt werden.

| Kontotyp und Vorgang | Desktop | Mobil |
| --- | --- | --- |
| Azure Active Directory: Synchronisierung |Ja |Nein |
| Azure Active Directory: Sicherung/Wiederherstellung |Nein |Nein |
| Microsoft-Konto: Synchronisierung |Ja |Ja |
| Microsoft-Konto: Sicherung/Wiederherstellung |Nein |Ja |

## <a name="what-is-backup"></a>Was ist die Sicherung?

Windows-Einstellungen werden normalerweise standardmäßig synchronisiert. Einige Einstellungen werden aber nur gesichert, z. B. die Liste mit den installierten Anwendungen auf einem Gerät. Sicherungen können nur für mobile Geräte angewendet werden und sind zurzeit für Enterprise State Roaming-Benutzer nicht verfügbar. Bei der Sicherung wird ein Microsoft-Konto verwendet, die Einstellungen und Anwendungsdaten werden in OneDrive gespeichert. Wenn ein Benutzer die Synchronisierung auf dem Gerät mit der Einstellungen-App deaktiviert, werden die Anwendungsdaten, die normalerweise synchronisiert werden, nur noch gesichert. Auf Sicherungsdaten kann nur über den Wiederherstellungsvorgang während der ersten Ausführung eines neuen Geräts zugegriffen werden. Sicherungen können über die Geräteeinstellungen deaktiviert und über das OneDrive-Konto des Benutzers verwaltet und gelöscht werden.

## <a name="windows-settings-overview"></a>Übersicht über die Windows-Einstellungen

Die folgenden Einstellungsgruppen sind für Endbenutzer verfügbar, um die Einstellungssynchronisierung auf Windows 10-Geräten zu aktivieren oder zu deaktivieren.

* Design: Desktophintergrund, Benutzerkachel, Taskleistenposition usw. 
* Internet Explorer-Einstellungen: Browserverlauf, eingegebene URLs, Favoriten usw. 
* Kennwörter: Windows-Anmeldeinformationsverwaltung, einschließlich WLAN-Profile 
* Spracheinstellungen: Wörterbuch, Einstellungen für Systemsprache 
* Erleichterte Bedienung: Sprachausgabe, Bildschirmtastatur, Bildschirmlupe 
* Weitere Windows-Einstellungen: siehe Details zu Windows-Einstellungen
* Microsoft Edge-Browser-Einstellung: Microsoft Edge-Favoriten, Leseliste und andere Einstellungen

![Synchronisieren Ihrer Einstellungen](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-syncyoursettings.png)

> [!NOTE]
> Dieser Artikel bezieht sich auf den älteren HTML-basierten Microsoft Edge-Browser, der im Juli 2015 mit Windows 10 veröffentlicht wurde. Der Artikel gilt nicht für den neuen Chromium-basierten Microsoft Edge-Browser, der am 15. Januar 2020 veröffentlicht wurde. Weitere Informationen zum Synchronisierungsverhalten des neuen Microsoft Edge finden Sie im Artikel [Microsoft Edge-Synchronisierung](/deployedge/microsoft-edge-enterprise-sync).

Die Synchronisierung der Einstellungsgruppe des Microsoft Edge-Browsers (Favoriten, Leseliste) kann von Endbenutzern über das Einstellungsmenü des Microsoft Edge-Browsers aktiviert oder deaktiviert werden.

![Konto](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-edge.png)

Für Windows 10 Version 1803 oder höher, Internet Explorer-Einstellungengruppe (Favoriten, typisierte URLs) kann das Synchronisieren von Endbenutzern über die Menüoption „Internet Explorer-Einstellungen“ aktiviert oder deaktiviert werden. 

![Einstellungen](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-ie.png)

## <a name="windows-settings-details"></a>Details zu Windows-Einstellungen

In der folgenden Tabelle bezieht sich „Sonstige“ in der Spalte „Gruppe“ auf Einstellungen, die unter „Einstellungen“ > „Konten“ > „Einstellungen synchronisieren“ > „Weitere Windows-Einstellungen“ deaktiviert werden können. 

Die Einträge „Intern“ in der Spalte „Gruppe“ beziehen sich auf Einstellungen und Apps, für die die Synchronisierung nur in der App selbst oder für das gesamte Gerät per MDM- oder Gruppenrichtlinien-Einstellungen deaktiviert werden kann.
Einstellungen, für die kein Roaming oder keine Synchronisierung durchgeführt wird, gehören keiner Gruppe an.

| Einstellungen | Desktop | Mobil | Group |
| --- | --- | --- | --- |
| **Konten**: Kontobild |sync |X |Design |
| **Konten**: weitere Kontoeinstellungen |X |X | |
| **Erweitertes mobiles Breitband**: Netzwerkname der Internetverbindungsfreigabe (ermöglicht automatische Ermittlung von mobilen WLAN-Hotspots per Bluetooth) |X |X |Kennwörter |
| **App-Daten**: einzelne Apps können Daten synchronisieren |Sync.sicherung |Sync.sicherung |internal |
| **App-Liste**: Liste der installierten Apps |X |Sicherung |Andere |
| **Bluetooth**: alle Bluetooth-Einstellungen |X |X | |
| **Eingabeaufforderung:** Standardeinstellungen für die Eingabeaufforderung |sync |X |internal |
| **Anmeldeinformationen**: Schließfach für Anmeldeinformationen |sync |sync |password |
| **Datum, Uhrzeit und Region**: automatische Uhrzeit (Internetzeitsynchronisierung) |sync |sync |language |
| **Datum, Uhrzeit und Region**: 24-Stunden-Format |sync |X |language |
| **Datum, Uhrzeit und Region**: Datum und Uhrzeit |sync |X |language |
| **Datum, Uhrzeit und Region**: Zeitzone | |X |language |
| **Datum, Uhrzeit und Region**: Sommerzeit |sync |X |language |
| **Datum, Uhrzeit und Region**: Land/Region |sync |X |language |
| **Datum, Uhrzeit und Region**: erster Tag der Woche |sync |X |language |
| **Datum, Uhrzeit und Region**: regionales Format (Gebietsschema) |sync |X |language |
| **Datum, Uhrzeit und Region**: kurzes Datum |sync |X |language |
| **Datum, Uhrzeit und Region**: langes Datum |sync |X |language |
| **Datum, Uhrzeit und Region**: kurze Uhrzeit |sync |X |language |
| **Datum, Uhrzeit und Region**: lange Uhrzeit |sync |X |language |
| **Desktoppersonalisierung**: Desktopdesign (Hintergrund, Systemfarbe, standardmäßige Systemsounds, Bildschirmschoner) |sync |X |Design |
| **Desktoppersonalisierung**: Diashow-Hintergrundbild |sync |X |Design |
| **Desktoppersonalisierung**: Einstellungen der Taskleiste (Position, automatisches Ausblenden usw.) |sync |X |Design |
| **Desktoppersonalisierung**: Layout des Startbildschirms |X |Sicherung | |
| **Geräte**: verbundene freigegebene Drucker |X |X |andere |
| **Microsoft Edge-Browser:** Leseliste |sync |sync |internal |
| **Microsoft Edge-Browser:** Favoriten |sync |sync |internal |
| **Microsoft Edge-Browser:** beste Websites <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** eingegebene URLs <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** Einstellungen der Favoritenleiste <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** Schaltfläche „Startseite“ anzeigen <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** Popupfenster blockieren <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** fragen, wie mit jedem Download verfahren werden soll <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** Speichern von Kennwörtern anbieten <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** „Do Not Track“-Anforderungen (nicht nachverfolgen) senden <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** Formulareinträge speichern <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** Such- und Websitevorschläge während der Eingabe anzeigen <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** Voreinstellung für Cookies <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** Websites das Speichern geschützter Medienlizenzen auf meinem Gerät erlauben <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Microsoft Edge-Browser:** Einstellung für die Sprachausgabe <sup>[[1]](#footnote-1)</sup> |sync |sync |internal |
| **Hoher Kontrast**: Ein/Aus |sync |X |Erleichterte Bedienung |
| **Hoher Kontrast**: Designeinstellungen |sync |X |Erleichterte Bedienung |
| **Internet Explorer**: geöffnete Registerkarten (URL und Titel) |sync |sync |Internet Explorer |
| **Internet Explorer**: Leseliste |sync |sync |Internet Explorer |
| **Internet Explorer**: eingegebene URLs |sync |sync |Internet Explorer |
| **Internet Explorer**: Browserverlauf |sync |sync |Internet Explorer |
| **Internet Explorer**: Favoriten |sync |sync |Internet Explorer |
| **Internet Explorer**: ausgeschlossene URLs |sync |sync |Internet Explorer |
| **Internet Explorer**: Startseiten |sync |sync |Internet Explorer |
| **Internet Explorer**: Domänenvorschläge |sync |sync |Internet Explorer |
| **Tastatur**: Benutzer können Bildschirmtastatur ein-/ausschalten |sync |X |Erleichterte Bedienung |
| **Tastatur**: Einrastfunktion einschalten (standardmäßig deaktiviert) |sync |X |Erleichterte Bedienung |
| **Tastatur**: Anschlagverzögerung einschalten (standardmäßig deaktiviert) |sync |X |Erleichterte Bedienung |
| **Tastatur**: Umschalttasten einschalten (standardmäßig deaktiviert) |sync |X |Erleichterte Bedienung |
| **Internet Explorer**: Domänensprache: Chinesisch (CHS) QWERTY – Selbstlernfunktion aktivieren |sync |X |Sprache |
| **Sprache**: CHS QWERTY – dynamische Kandidateneinstufung aktivieren |sync |X |Sprache |
| **Sprache**: CHS QWERTY – Zeichensatz Chinesisch (vereinfacht) |sync |X |Sprache |
| **Sprache**: CHS QWERTY – Zeichensatz Chinesisch (traditionell) |sync |X |Sprache |
| **Sprache**: CHS QWERTY – Fuzzy Pinyin |sync |Sicherung |Sprache |
| **Sprache**: CHS QWERTY – Fuzzy Pairs |sync |Sicherung |Sprache |
| **Sprache**: CHS QWERTY – Pinyin (vollständig) |sync |X |Sprache |
| **Sprache**: CHS QWERTY – Doppel-Pinyin |sync |X |Sprache |
| **Sprache**: CHS QWERTY – Autokorrektur beim Lesen |sync |X |Sprache |
| **Sprache**: CHS QWERTY – C/E-Umschalttaste, UMSCHALT |sync |X |Sprache |
| **Sprache**: CHS QWERTY – C/E-Umschalttaste, STRG |sync |X |Sprache |
| **Sprache**: CHS WUBI – Eingabemodus für einzelne Zeichen |sync |X |Sprache |
| **Sprache**: CHS WUBI – verbleibende Codierung des Kandidaten anzeigen |sync |X |Sprache |
| **Sprache**: CHS WUBI – Signalton bei ungültigem 4-Code |sync |X |Sprache |
| **Sprache:** CHT Bopomofo – CJK-Erweiterung A einschließen |sync |X |Sprache |
| **Sprache**: Japanisches IME – Eingabevorhersage und benutzerdefinierte Wörter |sync |sync |Sprache |
| **Sprache**: Koreanisches (KOR) IME |X |X |Sprache |
| **Sprache**: Handschrifterkennung |X |X |Sprache |
| **Sprache**: Sprachprofil |sync |Sicherung |Sprache |
| **Sprache**: Rechtschreibprüfung – AutoKorrektur und Hervorhebung von Rechtschreibfehlern |sync |Sicherung |Sprache |
| **Sprache**: Liste der Tastaturen |sync |Sicherung |Sprache |
| **Sperrbildschirm**: alle Einstellungen des Sperrbildschirms |X |X | |
| **Bildschirmlupe**: Ein/Aus (Masterumschalter) |X |X |Erleichterte Bedienung |
| **Bildschirmlupe**: Inversionsfarbe ein/aus (standardmäßig deaktiviert) |sync |X |Erleichterte Bedienung |
| **Bildschirmlupe**: Nachverfolgung – dem Tastaturfokus folgen |sync |X |Erleichterte Bedienung |
| **Bildschirmlupe**: Nachverfolgung – dem Mauszeiger folgen |sync |X |Erleichterte Bedienung |
| **Bildschirmlupe**: beim Anmelden von Benutzern starten (standardmäßig deaktiviert) |sync |X |Erleichterte Bedienung |
| **Maus**: Größe des Mauszeigers ändern |sync |X |andere |
| **Maus**: Farbe des Mauszeigers ändern |sync |X |andere |
| **Maus**: alle anderen Einstellungen |X |X | |
| **Sprachausgabe**: Schnellstart |sync |X |Erleichterte Bedienung |
| **Sprachausgabe**: Benutzer können Tonhöhe der Sprachausgabe ändern |sync |X |Erleichterte Bedienung |
| **Sprachausgabe**: Benutzer können Lesehinweise der Sprachausgabe für häufige Elemente ein- oder ausschalten (standardmäßig aktiviert) |sync |X |Erleichterte Bedienung |
| **Sprachausgabe**: Benutzer können aktivieren oder deaktivieren, ob eingegebene Zeichen zu hören sein sollen (standardmäßig aktiviert) |sync |X |Erleichterte Bedienung |
| **Sprachausgabe**: Benutzer können aktivieren oder deaktivieren, ob eingegebene Wörter zu hören sein sollen (standardmäßig aktiviert) |sync |X |Erleichterte Bedienung |
| **Sprachausgabe**: Einfügecursor nach Sprachausgabe (standardmäßig aktiviert) |sync |X |Erleichterte Bedienung |
| **Sprachausgabe**: visuelle Hervorhebung des Sprachausgabecursors aktivieren (standardmäßig aktiviert) |sync |X |Erleichterte Bedienung |
| **Sprachausgabe**: Audiohinweise wiedergeben (standardmäßig aktiviert) |sync |X |Erleichterte Bedienung |
| **Sprachausgabe**: aktivieren Sie Tasten auf der Bildschirmtastatur beim Heben Ihres Fingers (standardmäßig deaktiviert) |sync |X |Erleichterte Bedienung |
| **Erleichterte Bedienung**: legen Sie die Breite des blinkenden Cursors fest |sync |X |Erleichterte Bedienung |
| **Erleichterte Bedienung**: Hintergrundbilder entfernen (standardmäßig deaktiviert) |sync |X |Erleichterte Bedienung |
| **Netzschalter und Energiesparen**: alle Einstellungen |X |X | |
| **Personalisierung des Startbildschirms:** Akzentfarbe (nur Phone) |X |sync |Design |
| **Eingabe**: Wörterbuch |sync |Sicherung |Sprache |
| **Eingabe**: falsch geschriebenes Wort automatisch korrigieren |sync |Sicherung |Sprache |
| **Eingabe**: Rechtschreibfehler hervorheben |sync |Sicherung |Sprache |
| **Eingabe**: Textvorschläge bei der Eingabe anzeigen |sync |Sicherung |Sprache |
| **Eingabe**: nach Auswahl eines Textvorschlags Leerzeichen einfügen |sync |Sicherung |Sprache |
| **Eingabe**: nach Doppeltippen auf die LEERTASTE Punkt einfügen |sync |Sicherung |Sprache |
| **Eingabe**: Großbuchstaben am Satzanfang |sync |Sicherung |Sprache |
| **Eingabe**: beim Doppeltippen auf die UMSCHALTTASTE Großbuchstaben verwenden |sync |Sicherung |Sprache |
| **Eingabe**: Tastentöne bei der Eingabe |sync |Sicherung |Sprache |
| **Eingabe**: Personalisierungsdaten für Bildschirmtastatur |sync |Sicherung |Sprache |
| **WLAN**: WLAN-Profile (nur WPA) |sync |sync |Kennwörter |

###### <a name="footnote-1"></a>Fußnote 1

Minimale unterstützte Betriebssystemversion des Windows Creators Updates (Build 15063). 

## <a name="next-steps"></a>Nächste Schritte

Grundlegendes erfahren Sie unter [Überblick über Enterprise State Roaming](enterprise-state-roaming-overview.md).
