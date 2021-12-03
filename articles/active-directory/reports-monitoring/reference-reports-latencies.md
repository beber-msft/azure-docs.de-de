---
title: Latenzen bei Azure Active Directory-Berichten | Microsoft-Dokumentation
description: Erfahren Sie etwas über den erforderliche Zeitraum, bis Ereignisse in Ihrem Azure-Portal angezeigt werden.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: karenhoran
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/13/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 807f3efbb76269a59b6c5f974e036754be197868
ms.sourcegitcommit: 27ddccfa351f574431fb4775e5cd486eb21080e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2021
ms.locfileid: "131995348"
---
# <a name="azure-active-directory-reporting-latencies"></a>Latenzen bei Azure Active Directory-Berichten

Latenz ist die Dauer bis zur Anzeige von Azure AD-Berichtsdaten (Azure Active Directory) im [Azure-Portal](https://portal.azure.com). Dieser Artikel beschreibt die erwartete Latenz für die verschiedenen Typen von Berichten. 

## <a name="activity-reports"></a>Aktivitätsberichte

Es gibt zwei Arten von Aktivitätsberichten:

- [Anmeldungen](concept-sign-ins.md): Bietet Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung.
- [Überwachungsprotokolle](concept-audit-logs.md): Bietet Informationen zu Systemaktivitäten von Benutzern und Gruppen, zu verwalteten Anwendungen und Verzeichnisaktivitäten

Die folgende Tabelle enthält Latenzzeitinformationen für Aktivitätsberichte. 

> [!NOTE]
> **Latenz (95. Perzentil)** bezieht sich auf die Zeit, bis zu der 95 % der Protokolle gemeldet werden, und **Latenz (99. Perzentil)** bezieht sich auf die Zeit, bis zu der 99 % der Protokolle gemeldet werden. 
>

| Bericht | Latenz (95. Perzentil) |Latenz (99. Perzentil)|
| :-- | --- | --- |
| Überwachungsprotokolle | 2 Min.  | 5 Min.  |
| Anmeldungen | 2 Min.  | 5 Min. |

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>Wie schnell werden nach Erwerb einer Premium-Lizenz Daten zu Aktivitäten angezeigt?

Wenn Ihnen bereits mit einer kostenlosen Lizenz Daten zu Aktivitäten angezeigt werden, können Sie diese sofort nach dem Upgrade sehen. Wenn Sie noch keine Daten haben, dauert es ein bis zwei Tage, bis die Daten in den Berichten nach dem Upgrade auf eine Premium-Lizenz angezeigt werden.

## <a name="security-reports"></a>Sicherheitsberichte

Es gibt zwei Arten von Sicherheitsberichten:

- [Riskante Anmeldungen:](../identity-protection/overview-identity-protection.md) Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. 
- [Benutzer mit Risikomarkierung:](../identity-protection/overview-identity-protection.md) Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. 

Die folgende Tabelle enthält Latenzzeitinformationen für Sicherheitsberichte.

| Bericht | Minimum | Average | Maximum |
| :-- | --- | --- | --- |
| Gefährdete Benutzer          | 5 Minuten   | 15 Minuten  | 2 Stunden  |
| Riskante Anmeldungen         | 5 Minuten   | 15 Minuten  | 2 Stunden  |

## <a name="risk-detections"></a>Risikoerkennungen

Azure AD verwendet adaptive Machine Learning-Algorithmen und -Heuristiken, um verdächtige Aktivitäten im Zusammenhang mit Ihren Benutzerkonten zu erkennen. Jede erkannte verdächtige Aktion wird in einem Datensatz gespeichert, der als **Risikoerkennung** bezeichnet wird.

Die folgende Tabelle enthält Latenzzeitinformationen für Risikoerkennungen.

| Bericht | Minimum | Average | Maximum |
| :-- | --- | --- | --- |
| Anmeldungen über anonyme IP-Adressen |5 Minuten |15 Minuten |2 Stunden |
| Anmeldungen über unbekannte Standorte |5 Minuten |15 Minuten |2 Stunden |
| Benutzer mit kompromittierten Anmeldeinformationen |2 Stunden |4 Stunden |8 Stunden |
| Unmöglicher Ortswechsel zu atypischen Orten |5 Minuten |1 Stunde |8 Stunden  |
| Anmeldungen über infizierte Geräte |2 Stunden |4 Stunden |8 Stunden  |
| Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten |2 Stunden |4 Stunden |8 Stunden  |


## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über Azure AD-Berichte](overview-reports.md)
* [Programmgesteuerter Zugriff auf Azure AD-Berichte](concept-reporting-api.md)
* [Azure Active Directory-Risikoerkennungen](../identity-protection/overview-identity-protection.md)