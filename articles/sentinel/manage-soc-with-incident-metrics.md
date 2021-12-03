---
title: Bessere Verwaltung Ihres SOC mit Incidentmetriken in Microsoft Sentinel | Microsoft-Dokumentation
description: Verwenden Sie Informationen aus dem Microsoft Sentinel-Bildschirm und der -Arbeitsmappe mit Incidentmetriken, um Ihr Security Operations Center (SOC) besser zu verwalten.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: microsoft-sentinel
ms.subservice: microsoft-sentinel
ms.devlang: na
ms.topic: how-to
ms.custom: mvc, ignite-fall-2021
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2021
ms.author: yelevin
ms.openlocfilehash: 9e20c875068387342edf6fd70fe93689e26fb1ad
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132523598"
---
# <a name="manage-your-soc-better-with-incident-metrics"></a>Bessere Verwaltung von SOC mit Incidentmetriken

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

Als Security Operations Center-Manager benötigen Sie Metriken und Measures zur Gesamteffizienz, um die Leistung Ihres Teams messen zu können. Sie müssen Incidentvorgänge im Lauf der Zeit anhand einer Vielzahl verschiedener Kriterien anzeigen, wie z. B. Schweregrad, MITRE-Taktiken, mittlere Zeit bis zur Selektierung und mittlere Lösungszeit. Microsoft Sentinel stellt diese Daten jetzt mit der neuen **SecurityIncident**-Tabelle und dem entsprechenden Schema in Log Analytics und der zugehörigen Arbeitsmappe **Effizienz des Sicherheitsbetriebs** zur Verfügung. Sie können die Leistung Ihres Teams im zeitlichen Verlauf visualisieren und diese Einblicke verwenden, um die Effizienz zu verbessern. Sie können auch eigene KQL-Abfragen für die Incidenttabelle schreiben und verwenden, um benutzerdefinierte Arbeitsmappen zu erstellen, die genau Ihren speziellen Überprüfungsanforderungen und KPIs entsprechen.

## <a name="use-the-security-incidents-table"></a>Verwenden der Tabelle mit Sicherheitsincidents

Die Tabelle **SecurityIncident** ist in Microsoft Sentinel integriert. Sie finden Sie zusammen mit anderen Tabellen in der Auflistung **SecurityInsights** unter **Protokolle**. Sie können diese Tabelle wie jede andere auch in Log Analytics abfragen.

:::image type="content" source="./media/manage-soc-with-incident-metrics/security-incident-table.png" alt-text="Tabelle mit Sicherheitsincidents":::

Jedes Mal, wenn Sie einen Incident erstellen oder aktualisieren, wird der Tabelle ein neuer Protokolleintrag hinzugefügt. So können Sie die Änderungen nachverfolgen, die an Incidents vorgenommen werden, und noch leistungsfähigere SOC-Metriken erstellen. Allerdings müssen Sie diesen Aspekt berücksichtigen, wenn Sie Abfragen für diese Tabelle erstellen, da Sie je nach Abfrage möglicherweise doppelte Einträge für einen Incident entfernen müssen. 

Ein Beispiel: Wenn Sie eine Liste aller Incidents sortiert nach Incidentnummer benötigen, aber für jeden Incident nur das neueste Protokoll zurückgeben möchten, können Sie dafür den [summarize-Operator](/azure/data-explorer/kusto/query/summarizeoperator) von KQL mit der [Aggregationsfunktion](/azure/data-explorer/kusto/query/arg-max-aggfunction) `arg_max()` verwenden:

```Kusto
SecurityIncident
| summarize arg_max(LastModifiedTime, *) by IncidentNumber
```
### <a name="more-sample-queries"></a>Weitere Beispielabfragen

Incidentstatus: Alle Incidents nach Status und Schweregrad in einem bestimmten Zeitraum:

```Kusto
let startTime = ago(14d);
let endTime = now();
SecurityIncident
| where TimeGenerated >= startTime
| summarize arg_max(TimeGenerated, *) by IncidentNumber
| where LastModifiedTime  between (startTime .. endTime)
| where Status in  ('New', 'Active', 'Closed')
| where Severity in ('High','Medium','Low', 'Informational')
```

Mittlere Zeit bis zum Abschluss:
```Kusto
SecurityIncident
| summarize arg_max(TimeGenerated,*) by IncidentNumber 
| extend TimeToClosure =  (ClosedTime - CreatedTime)/1h
| summarize 5th_Percentile=percentile(TimeToClosure, 5),50th_Percentile=percentile(TimeToClosure, 50), 
  90th_Percentile=percentile(TimeToClosure, 90),99th_Percentile=percentile(TimeToClosure, 99)
```

Mittlere Zeit bis zur Selektierung:
```Kusto
SecurityIncident
| summarize arg_max(TimeGenerated,*) by IncidentNumber 
| extend TimeToTriage =  (FirstModifiedTime - CreatedTime)/1h
| summarize 5th_Percentile=max_of(percentile(TimeToTriage, 5),0),50th_Percentile=percentile(TimeToTriage, 50), 
  90th_Percentile=percentile(TimeToTriage, 90),99th_Percentile=percentile(TimeToTriage, 99) 
```

## <a name="security-operations-efficiency-workbook"></a>Arbeitsmappe zur Effizienz des Sicherheitsbetriebs

Zur Ergänzung der Tabelle **SecurityIncidents** haben wir eine sofort einsatzbereite Arbeitsmappenvorlage namens **Effizienz des Sicherheitsbetriebs** bereitgestellt, die Sie zum Überwachen Ihrer SOC-Vorgänge verwenden können. Die Arbeitsmappe enthält die folgenden Metriken: 
- Erstellte Incidents im Lauf der Zeit 
- Erstellte Incidents nach Abschlussklassifizierung, Schweregrad, Besitzer und Status 
- Mittlere Zeit bis zur Selektierung 
- Mittlere Zeit bis zum Abschluss 
- Erstellte Incidents nach Schweregrad, Besitzer, Status, Produkt und Taktik im zeitlichen Verlauf 
- Perzentile für die Zeit bis zur Selektierung 
- Perzentile für die Zeit bis zum Abschluss 
- Mittlere Zeit bis zur Selektierung pro Besitzer 
- Neueste Aktivitäten 
- Neueste Abschlussklassifizierungen  

Sie finden diese neue Arbeitsmappenvorlage, indem Sie im Microsoft Sentinel-Navigationsmenü **Arbeitsmappen** und dann die Registerkarte **Vorlagen** auswählen. Wählen Sie **Effizienz von Sicherheitsvorgängen** aus dem Katalog aus, und klicken Sie auf eine der Schaltflächen **Gespeicherte Arbeitsmappe anzeigen** und **Vorlage anzeigen**.

:::image type="content" source="./media/manage-soc-with-incident-metrics/security-incidents-workbooks-gallery.png" alt-text="Katalog mit Arbeitsmappen zu Sicherheitsincidents":::

:::image type="content" source="./media/manage-soc-with-incident-metrics/security-operations-workbook-1.png" alt-text="Vollständige Arbeitsmappe zu Sicherheitsincidents":::

Sie können die Vorlage verwenden, um eigene, auf Ihre Anforderungen zugeschnittene Arbeitsmappen zu erstellen.

## <a name="securityincidents-schema"></a>SecurityIncidents-Schema

[!INCLUDE [SecurityIncidents schema](../../includes/sentinel-schema-security-incident.md)]

## <a name="next-steps"></a>Nächste Schritte

- Für den Einstieg in Microsoft Sentinel benötigen Sie ein Microsoft Azure-Abonnement. Wenn Sie nicht über ein Abonnement verfügen, können Sie sich für ein [kostenloses Testabonnement](https://azure.microsoft.com/free/)registrieren.
- Hier erfahren Sie, wie Sie [ein Onboarding Ihrer Daten in Microsoft Sentinel durchführen](quickstart-onboard.md) und [Einblicke in Daten und potenzielle Bedrohungen erhalten](get-visibility.md).
