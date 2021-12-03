---
title: Was ist Azure Active Directory Identity Protection?
description: Erkennen, Behandeln, Untersuchen und Analysieren von Risiken mit Azure AD Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: overview
ms.date: 06/15/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: sahandle
ms.custom: contperf-fy21q1
ms.collection: M365-identity-device-management
ms.openlocfilehash: b624194285dd5ba7e0a54b8a15d3885a845d64dc
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132283238"
---
# <a name="what-is-identity-protection"></a>Was ist Identity Protection?

Identity Protection ist ein Tool, mit dem Organisationen drei wichtige Aufgaben erledigen können:

- [Automatisieren der Erkennung und Behandlung identitätsbasierter Risiken](howto-identity-protection-configure-risk-policies.md)
- [Untersuchen von Risiken](howto-identity-protection-investigate-risk.md) unter Verwendung von Daten im Portal
- [Exportieren von Risikoerkennungsdaten in Ihre SIEM-Lösung](../../sentinel/data-connectors-reference.md#azure-active-directory-identity-protection)

Identity Protection nutzt die Erkenntnisse, die Microsoft dank seiner Position in Organisationen (Azure AD), im Verbraucherbereich (Microsoft-Konten) und im Spielebereich (Xbox) gewonnen hat, um Ihre Benutzer zu schützen. Microsoft analysiert 6,5 Billionen Signale pro Tag, um Bedrohungen zu erkennen und Kunden vor Bedrohungen zu schützen.

Die Signale, die von Identity Protection generiert und Identity Protection zugeführt werden, können auch im Rahmen des bedingten Zugriffs genutzt werden, um Zugriffsentscheidungen zu treffen. Darüber hinaus können die Signale zur weiteren Untersuchung auf der Grundlage der erzwungenen Richtlinien Ihrer Organisation an ein SIEM-Tool (Security Information & Event Management) übergeben werden.

## <a name="why-is-automation-important"></a>Warum ist eine Automatisierung wichtig?

In seinem [Blogbeitrag vom Oktober 2018](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Eight-essentials-for-hybrid-identity-3-Securing-your-identity/ba-p/275843) erklärt Alex Weinert (Leiter des Microsoft-Teams für Identitätssicherheit und -schutz), warum die Automatisierung angesichts des Ereignisumfangs so wichtig ist:

> Von unseren Machine Learning- und Heuristiksystemen werden Tag für Tag Risikobewertungen für 18 Milliarden Anmeldeversuche für über 800 Millionen verschiedenen Konten generiert. 300 Millionen dieser Anmeldeversuche stammen erkennbar von Angreifern (Kriminelle, Hacker usw.).
>
> Auf der letztjährigen Ignite habe ich über die drei wichtigsten Angriffe auf unsere Identitätssysteme gesprochen. Diese Angriffe hatten zuletzt folgenden Umfang:
>   
>   - **Breach Replay:** 4,6 Milliarden erkannte Angriffe im Mai 2018
>   - **Kennwortspray:** 350.000 im April 2018
>   - **Phishing:** Diese Angriffe lassen sich nur schwer exakt quantifizieren. Im März 2018 wurden jedoch 23 Millionen Risikoereignisse erfasst, von denen viele mit Phishing in Zusammenhang stehen.

## <a name="risk-detection-and-remediation"></a>Risikoerkennung und -behandlung

Identity Protection erkennt verschiedene Risikotypen, einschließlich:

- Verwendung anonymer IP-Adressen
- Ungewöhnlicher Ortswechsel
- Mit Schadsoftware verknüpfte IP-Adresse
- Ungewöhnliche Anmeldeeigenschaften
- Kompromittierte Anmeldeinformationen
- Kennwortspray
- und vieles mehr...

Weitere Einzelheiten zu diesen und anderen Risiken sowie dazu, wann und wie sie berechnet werden, finden Sie im Artikel [Was bedeutet Risiko?](concept-identity-protection-risks.md).

Die Risikosignale können Korrekturmaßnahmen zur Folge haben, die unter Umständen dazu führen, dass Benutzer die Azure AD Multi-Factor Authentication verwenden oder ihr Kennwort per Self-Service-Kennwortzurücksetzung zurücksetzen müssen oder blockiert werden, bis ein Administrator aktiv wird.

## <a name="risk-investigation"></a>Risikountersuchung

Administratoren können Erkennungen überprüfen und bei Bedarf manuelle Maßnahmen ergreifen. In Identity Protection stehen drei wichtige Berichte zur Verfügung, die Administratoren bei ihren Untersuchungen nutzen können:

- Riskante Benutzer
- Riskante Anmeldungen
- Risikoerkennungen

Weitere Informationen finden Sie im Artikel [Untersuchen von Risiken](howto-identity-protection-investigate-risk.md).

### <a name="risk-levels"></a>Risikostufen

Mit Identity Protection werden Risiken in drei Stufen eingeteilt: niedrig, mittel und hoch. 

Microsoft macht zwar keine spezifischen Angaben zur Berechnung von Risiken, mit den einzelnen Stufen wird jedoch jeweils eine höhere Zuverlässigkeit bei der Einschätzung erreicht, ob eine Kompromittierung des Benutzers oder der Anmeldung vorliegt. Beispielsweise sind einmalige ungewöhnliche Anmeldeeigenschaften eines Benutzers unter Umständen nicht so riskant wie kompromittierte Anmeldeinformationen eines anderen Benutzers.

## <a name="exporting-risk-data"></a>Exportieren von Risikodaten

Daten aus Identity Protection können zur Archivierung sowie für weitere Untersuchungen und Korrelationsschritte in andere Tools exportiert werden. Dank der Microsoft Graph-basierten APIs können Organisationen diese Daten zur weiteren Verarbeitung in einem Tool (beispielsweise SIEM) sammeln. Der Zugriff auf die Identity Protection-API wird im Artikel [Erste Schritte mit Azure Active Directory Identity Protection und Microsoft Graph](howto-identity-protection-graph-api.md) erläutert.

Informationen zur Integration von Identity Protection-Informationen in Microsoft Sentinel finden Sie im Artikel [Verknüpfen von Daten aus Azure AD Identity Protection](../../sentinel/data-connectors-reference.md#azure-active-directory-identity-protection).

Darüber hinaus können Organisationen Daten für längere Zeiträume speichern, indem sie die Diagnoseeinstellungen in Azure AD so ändern, dass RiskyUsers- und UserRiskEvents-Daten an einen Log Analytics-Arbeitsbereich gesendet, Daten in einem Speicherkonto archiviert, Daten an einen Event Hub gestreamt oder Daten an eine Partnerlösung gesendet werden. Ausführliche Informationen dazu finden Sie im Artikel [Anleitung: Exportieren von Risikodaten](howto-export-risk-data.md).

## <a name="permissions"></a>Berechtigungen

Benutzer müssen über die Rolle „Benutzer mit Leseberechtigung für Sicherheitsfunktionen“, „Sicherheitsoperator“, „Sicherheitsadministrator“, „Globaler Leser“ oder „Globaler Administrator“ verfügen, um auf Identity Protection zugreifen zu können.

| Role | Möglich | Nicht möglich |
| --- | --- | --- |
| Globaler Administrator | Vollzugriff auf Identity Protection |   |
| Sicherheitsadministrator | Vollzugriff auf Identity Protection | Zurücksetzen des Kennworts für einen Benutzer |
| Sicherheitsoperator | Anzeigen aller Identity Protection-Berichte und des Blatts „Übersicht“ <br><br> Ignorieren des Benutzerrisikos, Bestätigen der sicheren Anmeldung, Bestätigen der Kompromittierung | Konfigurieren oder Ändern von Richtlinien <br><br> Zurücksetzen des Kennworts für einen Benutzer <br><br> Konfigurieren von Warnungen |
| Sicherheitsleseberechtigter | Anzeigen aller Identity Protection-Berichte und des Blatts „Übersicht“ | Konfigurieren oder Ändern von Richtlinien <br><br> Zurücksetzen des Kennworts für einen Benutzer <br><br> Konfigurieren von Warnungen <br><br> Bereitstellen von Feedback zu Erkennungen |

Derzeit kann die Rolle „Sicherheitsoperator“ nicht auf den Bericht „Riskante Anmeldungen“ zugreifen.

Administratoren für bedingten Zugriff können auch Richtlinien erstellen, bei denen das Anmelderisiko als Bedingung berücksichtigt wird. Weitere Informationen finden Sie im Artikel [Bedingter Zugriff: Bedingungen](../conditional-access/concept-conditional-access-conditions.md#sign-in-risk).

## <a name="license-requirements"></a>Lizenzanforderungen

[!INCLUDE [Active Directory P2 license](../../../includes/active-directory-p2-license.md)]

| Funktion | Details  | Azure AD Free/Microsoft 365-Apps | Azure AD Premium P1|Azure AD Premium P2 |
| --- | --- | --- | --- | --- |
| Risikorichtlinien | Benutzerrisiko-Richtlinie (über Identity Protection)  | Nein | Nein |Ja | 
| Risikorichtlinien | Anmelderisiko-Richtlinie (über Identity Protection oder bedingten Zugriff)  | Nein |  Nein |Ja |
| Sicherheitsberichte | Übersicht |  Nein | Nein |Ja |
| Sicherheitsberichte | Riskante Benutzer  | Eingeschränkte Informationen. Nur Benutzer mit mittlerem und hohem Risiko werden angezeigt. Kein Drawer „Details“ oder Risikoverlauf. | Eingeschränkte Informationen. Nur Benutzer mit mittlerem und hohem Risiko werden angezeigt. Kein Drawer „Details“ oder Risikoverlauf. | Vollzugriff|
| Sicherheitsberichte | Riskante Anmeldungen  | Eingeschränkte Informationen. Es werden keine Risikodetails oder Risikostufen angezeigt. | Eingeschränkte Informationen. Es werden keine Risikodetails oder Risikostufen angezeigt. | Vollzugriff|
| Sicherheitsberichte | Risikoerkennungen   | Nein | Eingeschränkte Informationen. Kein Drawer „Details“.| Vollzugriff|
| Benachrichtigungen | Warnungen zu erkannten gefährdeten Benutzern  | Nein | Nein |Ja |
| Benachrichtigungen | Wöchentliche Übersicht| Nein | Nein | Ja | 
| | Richtlinie für MFA-Registrierung | Nein | Nein | Ja |

Weitere Informationen zu diesen umfassenden Berichten finden Sie im Artikel zu den [ Risiken](howto-identity-protection-investigate-risk.md#navigating-the-reports).

## <a name="next-steps"></a>Nächste Schritte

- [Sicherheitsübersicht](concept-identity-protection-security-overview.md)

- [Was bedeutet Risiko?](concept-identity-protection-risks.md)

- [Verfügbare Richtlinien zum Mindern von Risiken](concept-identity-protection-policies.md)
