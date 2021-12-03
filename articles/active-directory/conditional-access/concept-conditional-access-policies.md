---
title: Erstellen einer Richtlinie für bedingten Zugriff – Azure Active Directory
description: Welche Optionen stehen für die Erstellung einer Richtlinie für bedingten Zugriff zur Verfügung, und was bedeuten Sie?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/26/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 997b3ec9b784b8b52b826b526102a97bce7d3286
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132301053"
---
# <a name="building-a-conditional-access-policy"></a>Erstellen einer Richtlinie für bedingten Zugriff

Wie im Artikel [Was ist bedingter Zugriff?](overview.md) erläutert, ist eine Richtlinie für bedingten Zugriff eine If-Then-Anweisung mit **Zuweisungen** und **Zugriffssteuerungen**. Eine Richtlinie für bedingten Zugriff führt Signale zusammen, um Entscheidungen zu treffen und Organisationsrichtlinien zu erzwingen.

Wie erstellt eine Organisation diese Richtlinien? Was ist erforderlich? Wie werden sie angewendet?

![Bedingter Zugriff (Signale + Entscheidungen + Erzwingung = Richtlinien)](./media/concept-conditional-access-policies/conditional-access-signal-decision-enforcement.png)

Mehrere Richtlinien für bedingten Zugriff können jederzeit auf einen einzelnen Benutzer angewendet werden. In diesem Fall müssen alle geltenden Richtlinien erfüllt werden. Wenn also beispielsweise eine Richtlinie die Verwendung der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) und eine weitere Richtlinie ein konformes Gerät erfordert, müssen Sie die MFA durchlaufen und ein konformes Gerät verwenden. Alle Zuweisungen sind logisch per **UND**-Operator verbunden. Wenn Sie mehr als eine Zuweisung konfiguriert haben, müssen die Bedingungen aller Zuweisungen erfüllt sein, damit eine Richtlinie ausgelöst wird.

Wenn eine Richtlinie ausgewählt ist, bei der „Eine der ausgewählten Steuerungen anfordern“ ausgewählt ist, wird in der definierten Reihenfolge eine Eingabeaufforderung angezeigt, sobald die Richtlinienanforderungen erfüllt sind. Der Zugriff wird gewährt.

Alle Richtlinien werden in zwei Phasen erzwungen:

- Phase 1: Sammeln von Sitzungsdetails 
   - Sammeln Sie Sitzungsdetails wie Netzwerkstandort und Geräteidentität, die für die Richtlinienauswertung benötigt werden. 
   - Phase 1 der Richtlinienauswertung gilt für aktivierte Richtlinien sowie für Richtlinien im Modus [Nur Bericht](concept-conditional-access-report-only.md).
- Phase 2: Erzwingung 
   - Verwenden Sie die in Phase 1 gesammelten Sitzungsdetails, um alle Anforderungen zu identifizieren, die nicht erfüllt wurden. 
   - Wenn eine Richtlinie zum Blockieren des Zugriffs mit dem Gewährungssteuerelement „Blockieren“ konfiguriert ist, wird die Erzwingung hier angehalten, und der*die Benutzer*in wird blockiert. 
   - Der*die Benutzer*in wird aufgefordert, weitere Gewährungssteuerungsanforderungen auszuführen, die während Phase 1 nicht in der folgenden Reihenfolge erfüllt wurden, bis die Richtlinie erfüllt ist:  
      - Multi-Factor Authentication 
      - Genehmigte Client-App/App-Schutzrichtlinie 
      - Verwaltetes Gerät (kompatibel oder hybrid in Azure AD eingebunden) 
      - Nutzungsbedingungen 
      - Benutzerdefinierte Steuerelemente  
   - Sobald alle Anforderungen der Gewährungssteuerelemente erfüllt wurden, wenden Sie Sitzungssteuerelemente an (von der App erzwungene Berechtigungen, Microsoft Defender für Cloud Apps und Tokengültigkeitsdauer). 
   - Die zweite Phase der Richtlinienauswertung wird für alle aktivierten Richtlinien durchlaufen. 

## <a name="assignments"></a>Zuweisungen

Im Teil „Zuweisungen“ wird das „Wer“, „Was“ und „Wo“ der Richtlinie für bedingten Zugriff gesteuert.

### <a name="users-and-groups"></a>Benutzer und Gruppen

[Benutzer und Gruppen](concept-conditional-access-users-groups.md) legen anhand der Zuweisungen fest, welche Personen in die Richtlinie einbezogen oder von der Richtlinie ausgeschlossen werden sollen. Diese Zuweisung kann alle Benutzer, bestimmte Benutzergruppen, Verzeichnisrollen oder externe Gastbenutzer einschließen. 

### <a name="cloud-apps-or-actions"></a>Cloud-Apps oder -aktionen

[Cloudanwendungen oder -aktionen](concept-conditional-access-cloud-apps.md) können Cloudanwendungen, Benutzeraktionen oder Authentifizierungskontexte, die der Richtlinie unterliegen, beinhalten oder ausschließen.

### <a name="conditions"></a>Bedingungen

Eine Richtlinie kann mehrere [Bedingungen](concept-conditional-access-conditions.md) enthalten.

#### <a name="sign-in-risk"></a>Anmelderisiko

Bei Organisationen mit [Azure AD Identity Protection](../identity-protection/overview-identity-protection.md) können die dort generierten Risikoerkennungen Ihre Richtlinien für den bedingten Zugriff beeinflussen.

#### <a name="device-platforms"></a>Geräteplattformen

Organisationen mit mehreren Betriebssystemplattformen für ihre Geräte möchten möglicherweise bestimmte Richtlinien auf unterschiedlichen Plattformen erzwingen. 

Die zum Berechnen der Geräteplattform verwendeten Informationen stammen aus nicht überprüften Quellen wie Benutzer-Agent-Zeichenfolgen, die geändert werden können.

#### <a name="locations"></a>Positionen

Standortdaten werden von IP-Geolocation-Daten bereitgestellt. Administratoren können bei Bedarf Standorte definieren und einige als vertrauenswürdig kennzeichnen (z.B. die Netzwerkstandorte ihrer Organisation).

#### <a name="client-apps"></a>Client-Apps

Standardmäßig gelten alle neu erstellten Richtlinien für bedingten Zugriff auch dann für alle Client-App-Typen, wenn die Client-Apps-Bedingung nicht konfiguriert ist.

Das Verhalten der Client-Apps-Bedingung wurde im August 2020 aktualisiert. Wenn Sie über Richtlinien für bedingten Zugriff verfügen, bleiben sie unverändert. Wenn Sie jedoch eine vorhandene Richtlinie auswählen, wurde die Umschaltfläche „Konfigurieren“ entfernt, und die Client-Apps, für die die Richtlinie gilt, werden ausgewählt.

#### <a name="device-state"></a>Gerätestatus

Mit diesem Steuerelement können Geräte mit Hybrid-Azure AD-Einbindung oder Geräte, die in Intune als kompatibel gekennzeichnet sind, ausgeschlossen werden. Dieser Ausschluss kann zum Blockieren nicht verwalteter Geräte durchgeführt werden. 

#### <a name="filters-for-devices-preview"></a>Nach Geräten filtern (Vorschau)

Mit diesem Steuerelement können bestimmte Geräte anhand ihrer Attribute in einer Richtlinie adressiert werden.

## <a name="access-controls"></a>Zugriffssteuerung

Die Zugriffssteuerung der Richtlinie für bedingten Zugriff ist der Teil, der steuert, wie eine Richtlinie erzwungen wird.

### <a name="grant"></a>Erteilen

[Erteilen](concept-conditional-access-grant.md) bietet Administratoren eine Möglichkeit zur Richtlinienerzwingung, bei der sie den Zugriff blockieren oder gewähren können.

#### <a name="block-access"></a>Zugriff blockieren

„Zugriff blockieren“ bewirkt genau dies. Der Zugriff wird unter den angegebenen Zuweisungen blockiert. Das Blockieren des Zugriffs ist ein leistungsstarkes Steuerelement, das nur mit entsprechenden Kenntnissen eingesetzt werden sollte.

#### <a name="grant-access"></a>Gewähren von Zugriff

Das Steuerelement zum Gewähren des Zugriffs kann die Erzwingung von mindestens einem weiteren Steuerelementen auslösen. 

- Mehrstufige Authentifizierung erforderlich (Azure AD Multi-Factor Authentication)
- Als kompatibel markierte Geräte (Intune) erforderlich
- Geräte mit Hybrid-Azure AD-Einbindung erforderlich
- Genehmigte Client-App erforderlich
- App-Schutzrichtlinie erforderlich
- Kennwortänderung anfordern
- Vorschreiben der Verwendung von Nutzungsbedingungen

Administratoren können mithilfe der folgenden Optionen auswählen, ob eins der obigen Steuerelemente oder alle ausgewählten Steuerelemente erforderlich sind. Als Standardwert für mehrere Steuerelemente werden alle als erforderlich ausgewählt.

- Alle ausgewählten Steuerelemente erforderlich (Steuerelement UND Steuerelement)
- Eins der ausgewählten Steuerelemente erforderlich (Steuerelement ODER Steuerelement)

### <a name="session"></a>Sitzung

[Sitzungssteuerelemente](concept-conditional-access-session.md) können die Umgebung einschränken. 

- Verwenden von App-erzwungenen Einschränkungen
   - Funktioniert derzeit nur mit Exchange Online und SharePoint Online.
      - Übergibt Geräteinformationen, um das Gewähren von vollständigem oder eingeschränktem Zugriff auf die Umgebung zu steuern.
- App-Steuerung für bedingten Zugriff verwenden
   - Verwendet Signale von Microsoft Defender für Cloud Apps, um beispielsweise: 
      - Blockiert das Herunterladen, Ausschneiden, Kopieren und Drucken von sensiblen Dokumenten.
      - Überwacht das Verhalten riskanter Sitzungen.
      - Erzwingt das Bezeichnen von sensiblen Dateien.
- Anmeldehäufigkeit
   - Möglichkeit zum Ändern der standardmäßigen Anmeldehäufigkeit für die moderne Authentifizierung.
- Persistente Browsersitzung
   - Benutzer können angemeldet bleiben, nachdem sie ihr Browserfenster geschlossen und erneut geöffnet haben.

## <a name="simple-policies"></a>Einfache Richtlinien

Eine Richtlinie für bedingten Zugriff muss mindestens Folgendes enthalten, um erzwungen werden zu können:

- **Name** der Richtlinie.
- **Zuweisungen**
   - **Benutzer und/oder Gruppen**, auf die die Richtlinie angewendet werden soll.
   - **Cloud-Apps oder Aktionen**, auf die die Richtlinie angewendet werden soll.
- **Steuerelemente**
   - Steuerelemente für **Gewähren** oder **Blockieren**

![Leere Richtlinie für bedingten Zugriff](./media/concept-conditional-access-policies/conditional-access-blank-policy.png)

Der Artikel [Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md) enthält einige Richtlinien, die für die meisten Organisationen nützlich sein könnten.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen der Richtlinie für bedingten Zugriff](../authentication/tutorial-enable-azure-mfa.md?bc=%2fazure%2factive-directory%2fconditional-access%2fbreadcrumb%2ftoc.json&toc=%2fazure%2factive-directory%2fconditional-access%2ftoc.json#create-a-conditional-access-policy)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)

[Planen einer cloudbasierten Azure AD Multi-Factor Authentication-Bereitstellung](../authentication/howto-mfa-getstarted.md)

[Verwalten der Gerätekonformität mit Intune](/intune/device-compliance-get-started)

[Microsoft Defender für Cloud Apps und Bedingter Zugriff](/cloud-app-security/proxy-intro-aad)
