---
title: Gewährungssteuerelemente in Richtlinien für bedingten Zugriff – Azure Active Directory
description: Was sind Gewährungssteuerelemente in einer Richtlinie für bedingten Zugriff in Azure AD?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 06/25/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: calebb, sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 21bf4b8abfac8df9fe5791ccbbf37a619be65c7f
ms.sourcegitcommit: 901ea2c2e12c5ed009f642ae8021e27d64d6741e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2021
ms.locfileid: "132369681"
---
# <a name="conditional-access-grant"></a>Bedingter Zugriff: Erteilen

Innerhalb einer Richtlinie für bedingten Zugriff kann ein Administrator Zugriffssteuerungen verwenden, um den Zugriff auf Ressourcen zu gewähren oder zu blockieren.

![Richtlinie für bedingten Zugriff mit einem Gewährungssteuerelement, durch das eine mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) erforderlich ist](./media/concept-conditional-access-grant/conditional-access-grant.png)

## <a name="block-access"></a>Zugriff blockieren

Beim Blockieren des Zugriffs werden alle Zuweisungen berücksichtigt, und der Zugriff wird basierend auf der Konfiguration der Richtlinie für bedingten Zugriff verhindert.

Das Blockieren des Zugriffs ist ein leistungsstarkes Steuerelement, das nur mit entsprechenden Kenntnissen eingesetzt werden sollte. Richtlinien mit Blockanweisungen können unbeabsichtigte Nebeneffekte haben. Ordnungsgemäße Tests und Validierung sind vor dem Aktivieren im großen Stil unerlässlich. Administratoren sollten bei Änderungen Tools wie den Modus [Nur Bericht](concept-conditional-access-report-only.md) und das [Was-wäre-wenn-Tool](what-if-tool.md) für den bedingten Zugriff verwenden.

## <a name="grant-access"></a>Gewähren von Zugriff

Administratoren können auswählen, ob beim Gewähren des Zugriffs ein oder mehrere Steuerelemente erzwungen werden sollen. Diese Steuerelemente umfassen die folgenden Optionen: 

- [Mehrstufige Authentifizierung erforderlich (Azure AD Multi-Factor Authentication)](../authentication/concept-mfa-howitworks.md)
- [Markieren des Geräts als kompatibel erforderlich (Microsoft Intune)](/intune/protect/device-compliance-get-started)
- [Gerät mit Hybrid-Azure AD-Einbindung erforderlich](../devices/concept-azure-ad-join-hybrid.md)
- [Genehmigte Client-App erforderlich](app-based-conditional-access.md)
- [App-Schutzrichtlinie erforderlich](app-protection-based-conditional-access.md)
- [Kennwortänderung erforderlich](#require-password-change)

Wenn sich Administratoren dafür entscheiden, diese Optionen zu kombinieren, können sie die folgenden Methoden auswählen:

- Alle ausgewählten Steuerungen anfordern (Steuerelement **UND** Steuerelement)
- Eine der ausgewählten Steuerungen anfordern (Steuerelement **ODER** Steuerelement)

Für den bedingten Zugriff sind standardmäßig alle ausgewählten Steuerelemente erforderlich.

### <a name="require-multi-factor-authentication"></a>Mehrstufige Authentifizierung erforderlich

Wenn Sie dieses Kontrollkästchen aktivieren, müssen Benutzer Azure AD Multi-Factor Authentication ausführen. Weitere Informationen zum Bereitstellen von Azure AD Multi-Factor Authentication finden Sie im Artikel [Planen einer cloudbasierten Bereitstellung von Azure AD Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md).

[Windows Hello for Business](/windows/security/identity-protection/hello-for-business/hello-overview) erfüllt die Anforderungen für Multifactor Authentication in Richtlinien für bedingten Zugriff. 

### <a name="require-device-to-be-marked-as-compliant"></a>Markieren des Geräts als kompatibel erforderlich

Organisationen, die Microsoft Intune bereitstellen, können die von ihren Geräten zurückgegebenen Informationen verwenden, um Geräte zu identifizieren, die bestimmte Kompatibilitätsanforderungen erfüllen. Diese Informationen zur Richtliniencompliance werden von Intune an Azure AD weitergeleitet, sodass der bedingte Zugriff Entscheidungen zum Gewähren oder Blockieren des Zugriffs auf Ressourcen treffen kann. Weitere Informationen zu Konformitätsrichtlinien finden Sie im Artikel [Legen Sie mit Intune Regeln auf Geräten fest, um Zugriff auf Ressourcen in Ihrer Organisation zu gewähren](/intune/protect/device-compliance-get-started).

Ein Gerät kann von Intune (beliebiges Gerätebetriebssystem) oder vom MDM-System eines Drittanbieters für Windows 10-Geräte als konform markiert werden. Eine Liste der unterstützten MDM-Systeme von Drittanbietern finden Sie im Artikel [Unterstützen von Drittanbieter-Gerätekonformitätspartnern in Intune](/mem/intune/protect/device-compliance-partners).

Geräte müssen in Azure AD registriert werden, damit Sie als kompatibel gekennzeichnet werden können. Weitere Informationen zur Geräteregistrierung finden Sie im Artikel [Was ist eine Geräteidentität?](../devices/overview.md).

**Anmerkungen**

- Die **Anforderung, dass das Gerät als konform gekennzeichnet werden muss**:
   - Unterstützt nur Windows aktuelle (Windows 10+), iOS-, Android- und macOS-Geräte, die bei Azure AD intune registriert sind.
   - Für Geräte, die bei MDM-Systemen von Drittanbietern registriert sind, siehe [Unterstützung von Drittanbieter-Geräte-Compliance-Partnern in Intune](/mem/intune/protect/device-compliance-partners).
   - Der bedingte Zugriff kann Microsoft Edge im InPrivate-Modus einer genehmigten Client-App nicht berücksichtigen.


### <a name="require-hybrid-azure-ad-joined-device"></a>Gerät mit Hybrid-Azure AD-Einbindung erforderlich

Organisationen können die Geräteidentität als Teil ihrer Richtlinie für bedingten Zugriff verwenden. Mit diesem Kontrollkästchen können Organisationen festlegen, dass Geräte in Hybrid-Azure AD eingebunden sein müssen. Im Artikel [Was ist eine Geräteidentität?](../devices/overview.md) finden Sie weitere Informationen zu Geräteidentitäten.

Bei Verwendung des [OAuth-Gerätecodeflows](../develop/v2-oauth2-device-code.md) werden das Gewährungssteuerelement „Verwaltetes Gerät erforderlich“ oder eine Gerätestatusbedingung nicht unterstützt. Dies liegt daran, dass das Gerät, das die Authentifizierung ausführt, seinen Gerätestatus nicht für das Gerät bereitstellen kann, das einen Code bereitstellt. Zudem ist der Gerätestatus im Token für das Gerät, das die Authentifizierung ausführt, gesperrt. Verwenden Sie stattdessen das Gewährungssteuerelement „Mehrstufige Authentifizierung erforderlich“.

**Anmerkungen**

- Die Anforderung **Hybrid Azure AD eingebundenes Gerät** ist 
erforderlich:
   - Unterstützt nur in die Domäne eingebundene Windows (vor Windows 10) und Windows aktuelle Geräten (Windows 10+).
   - Conditional Access kann Microsoft Edge im InPrivate-Modus nicht als hybrides, mit Azure AD verbundenes Gerät berücksichtigen.

### <a name="require-approved-client-app"></a>Genehmigte Client-App erforderlich

Organisationen können festlegen, dass ein versuchter Zugriff auf die ausgewählten Cloud-Apps über eine genehmigte Client-App erfolgen muss. Diese genehmigten Client-Apps unterstützen [Intune-App-Schutzrichtlinien](/intune/app-protection-policy) unabhängig von jeglicher MDM-Lösung (Mobile-Device Management, Verwaltung mobiler Geräte).

Um dieses Gewährungssteuerelement zu nutzen, muss für den bedingten Zugriff das Gerät in Azure Active Directory registriert sein. Dafür ist eine Broker-App erforderlich. Die Broker-App kann entweder Microsoft Authenticator für iOS, Microsoft Authenticator oder das Microsoft-Unternehmensportal für Android-Geräte sein. Wenn der Benutzer versucht, sich zu authentifizieren, und auf dem Gerät keine Broker-App installiert ist, wird er zum entsprechenden App Store umgeleitet, um die erforderliche Broker-App zu installieren.

Für die folgenden Client-Apps wurde bestätigt, dass diese Einstellung unterstützt wird:

- Microsoft Azure Information Protection
- Microsoft Bookings
- Microsoft Cortana
- Microsoft Dynamics 365
- Microsoft Edge
- Microsoft Excel
- Microsoft Power Automate
- Microsoft Invoicing
- Microsoft Kaizala
- Microsoft Launcher
- Microsoft-Listen
- Microsoft Office
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft Power Apps
- Microsoft Power BI
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype for Business
- Microsoft StaffHub
- Microsoft Stream
- Microsoft Teams
- Microsoft To-Do
- Microsoft Visio
- Microsoft Word
- Microsoft Yammer
- Microsoft Whiteboard
- Microsoft 365 Admin

**Anmerkungen**

- Die genehmigten Client-Apps unterstützen das Intune-Feature für die mobile Anwendungsverwaltung.
- Anforderung **Genehmigte Client-App erforderlich**:
   - Unterstützt als Geräteplattformbedingung nur iOS und Android.
   - Zum Registrieren des Geräts ist eine Broker-App erforderlich. Die Broker-App kann entweder Microsoft Authenticator für iOS, Microsoft Authenticator oder das Microsoft-Unternehmensportal für Android-Geräte sein.
- Der bedingte Zugriff kann Microsoft Edge im InPrivate-Modus einer genehmigten Client-App nicht berücksichtigen.
- Die Verwendung eines Azure AD-Anwendungsproxys zum Aktivieren der mobilen Power BI-App zum Herstellen einer Verbindung mit dem lokalen Power BI-Berichtsserver wird nicht mit Richtlinien für den bedingten Zugriff unterstützt, die die Microsoft Power BI-App als genehmigte Client-App benötigen.

Konfigurationsbeispiele finden Sie im Artikel [Gewusst wie: Vorschreiben der Verwendung von genehmigten Client-Apps für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs](app-based-conditional-access.md).

### <a name="require-app-protection-policy"></a>App-Schutzrichtlinie erforderlich

In Ihrer Richtlinie für bedingten Zugriff können Sie festlegen, dass eine [Intune-App-Schutzrichtlinie](/intune/app-protection-policy) für die Client-App vorhanden sein muss, um auf die ausgewählten Cloud-Apps zugreifen zu können. 

Um dieses Gewährungssteuerelement zu nutzen, muss für den bedingten Zugriff das Gerät in Azure Active Directory registriert sein. Dafür ist eine Broker-App erforderlich. Die Broker-App kann entweder Microsoft Authenticator für iOS oder das Microsoft-Unternehmensportal für Android-Geräte sein. Wenn der Benutzer versucht, sich zu authentifizieren, und auf dem Gerät keine Broker-App installiert ist, wird er zum App Store umgeleitet, um die Broker-App zu installieren.

Anwendungen müssen über das **Intune SDK** mit implementierter **Richtliniensicherung** verfügen und bestimmte andere Anforderungen erfüllen, um diese Einstellung zu unterstützen. Entwickler, die Anwendungen mit dem Intune SDK implementieren möchten, finden in der SDK-Dokumentation weitere Informationen zu diesen Anforderungen.

Für die folgenden Client-Apps wurde bestätigt, dass diese Einstellung unterstützt wird:

- Microsoft Cortana
- Microsoft Edge
- Microsoft Excel
- Microsoft Lists (iOS)
- Microsoft Office
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft Power BI
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Teams
- Microsoft To Do
- Microsoft Word
- MultiLine for Intune
- Nine Mail – Email & Calendar

> [!NOTE]
> Microsoft Kaizala, Microsoft Skype for Business und Microsoft Visio unterstützen den Gewährungstyp **App-Schutzrichtlinie erforderlich** nicht. Wenn Sie mit diesen Apps arbeiten müssen, verwenden Sie exklusiv den Gewährungstyp **Genehmigte Apps erforderlich**. Die Verwendung der `or`Klausel zwischen den beiden Gewährungstypen funktioniert für diese drei Anwendungen nicht.

**Anmerkungen**

- Apps für die App-Schutzrichtlinie unterstützen die mobile Anwendungsverwaltung mit Intune mit Richtlinienschutz.
- Die Voraussetzung **App-Schutzrichtlinie erforderlich**:
    - Unterstützt als Geräteplattformbedingung nur iOS und Android.
    - Zum Registrieren des Geräts ist eine Broker-App erforderlich. Bei iOS ist Microsoft Authenticator die Broker-App, und bei Android ist es die Intune-Unternehmensportal-App.

Konfigurationsbeispiele finden Sie im Artikel [Gewusst wie: Erzwingen einer App-Schutzrichtlinie und einer genehmigten Client-App für den Zugriff auf Cloud-Apps mithilfe des bedingten Zugriffs](app-protection-based-conditional-access.md).

### <a name="require-password-change"></a>Kennwortänderung anfordern 

Wenn Benutzerrisiken erkannt werden, können Administratoren mithilfe der Benutzerrisiko-Richtlinienbedingungen festlegen, dass der Benutzer sein Kennwort mithilfe der Self-Service-Kennwortzurücksetzung von Azure AD sicher ändern kann. Wenn Benutzerrisiken erkannt werden, können die Benutzer die Self-Service-Kennwortzurücksetzung zur Eigenwartung durchführen. Dieser Prozess schließt das Benutzerrisikoereignis, um unnötigen Aufwand für Administratoren zu vermeiden. 

Wenn Benutzer aufgefordert werden, ihr Kennwort zu ändern, müssen sie zunächst die mehrstufige Authentifizierung durchführen. Sie müssen sicherstellen, dass alle Ihre Benutzer sich für die mehrstufige Authentifizierung registriert haben, damit sie vorbereitet sind, wenn ein Risiko für ihr Konto erkannt werden sollte.  

> [!WARNING]
> Benutzer müssen sich vor Auslösen der Anmelderisikorichtlinie bereits für die Self-Service-Kennwortzurücksetzung registriert haben. 

Wenn Sie eine Richtlinie mithilfe der Kennwortänderungssteuerung konfigurieren, sind Einschränkungen wirksam.  

1. Diese Richtlinie muss „allen Cloud-Apps“ zugewiesen sein. Diese Anforderung verhindert, dass ein Angreifer eine andere App verwendet, um das Kennwort des Benutzers zu ändern und so das Kontorisiko zurückzusetzen, indem er sich bei einer anderen App anmeldet. 
1. „Kennwortänderung erforderlich“ kann nicht mit anderen Steuerungen verwendet werden, zum Beispiel der Anforderung, ein kompatibles Gerät zu verwenden.  
1. Die Kennwortänderungssteuerung kann nur mit der Benutzer- und Gruppenzuweisungsbedingung, mit der Cloud-App-Zuweisung (die auf „Alle“ festgelegt sein muss) und den Benutzerrisikobedingungen verwendet werden. 

### <a name="terms-of-use"></a>Nutzungsbedingungen

Wenn Ihre Organisation Nutzungsbedingungen erstellt hat, werden unter den Steuerelementen zur Rechteerteilung ggf. andere Optionen angezeigt. Mit diesen Optionen können Administratoren die Bestätigung von Nutzungsbedingungen als Bedingung für den Zugriff auf die Ressourcen festlegen, die durch die Richtlinie geschützt werden. Weitere Informationen zu Nutzungsbedingungen finden Sie im Artikel [Nutzungsbedingungen für Azure Active Directory](terms-of-use.md).

## <a name="next-steps"></a>Nächste Schritte

- [Bedingter Zugriff: Sitzung](concept-conditional-access-session.md)

- [Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

- [Modus „Nur Bericht“](concept-conditional-access-report-only.md)
