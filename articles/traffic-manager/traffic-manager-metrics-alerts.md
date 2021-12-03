---
title: Metriken und Warnungen in Azure Traffic Manager
description: In diesem Artikel erfahren Sie mehr über die Metriken und Warnungen, die für Traffic Manager in Azure verfügbar sind.
services: traffic-manager
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: duau
ms.openlocfilehash: eaa1ef35432a0e31376bcff8f50bc8bdffbfaf58
ms.sourcegitcommit: 61f87d27e05547f3c22044c6aa42be8f23673256
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2021
ms.locfileid: "132054662"
---
# <a name="traffic-manager-metrics-and-alerts"></a>Traffic Manager-Metriken und -Warnungen

Traffic Manager bietet Ihnen DNS-basierten Lastenausgleich, der mehrere Routingmethoden und Optionen für die Endpunktüberwachung umfasst. Dieser Artikel beschreibt die Metriken und die dazugehörigen Warnungen, die für Kunden zur Verfügung stehen. 

## <a name="metrics-available-in-traffic-manager"></a>In Traffic Manager verfügbare Metriken 

Traffic Manager bietet die folgenden Metriken auf der Basis von Profilen, die Kunden nutzen können, um ihre Verwendung von Traffic Manager und den Status ihrer Endpunkte unter diesem Profil zu verstehen.  

### <a name="queries-by-endpoint-returned"></a>Abfragen nach zurückgegebenem Endpunkt
Verwenden Sie [diese Metrik](../azure-monitor/essentials/metrics-supported.md), um die Anzahl der Abfragen anzuzeigen, die ein Traffic Manager-Profil in einem bestimmten Zeitraum verarbeitet. Sie können die gleichen Informationen auch mit Granularität auf Endpunktebene anzeigen, um besser zu verstehen, wie oft ein Endpunkt in den Abfrageantworten von Traffic Manager zurückgegeben wurde.

Im folgenden Beispiel zeigt die Abbildung 1 alle Abfrageantworten, die das Traffic Manager-Profil zurückgibt. 

  
![Aggregatansicht aller Abfragen](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Abbildung 1: Aggregatansicht mit allen Abfragen*
  
Abbildung 2 zeigt die gleichen Informationen, sie sind allerdings nach Endpunkten unterteilt. So können Sie das Volumen der Abfrageantworten erkennen, in denen ein bestimmter Endpunkt zurückgegeben wurde.

![Traffic Manager-Metriken – unterteilte Ansicht des Abfragevolumens pro Endpunkt](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Abbildung 2: Unterteilte Ansicht mit Abfragevolumen, dargestellt pro zurückgegebenem Endpunkt*

## <a name="endpoint-status-by-endpoint"></a>Endpunktstatus nach Endpunkt
Verwenden Sie [diese Metrik](../azure-monitor/essentials/metrics-supported.md#microsoftnetworktrafficmanagerprofiles), um den Integritätsstatus der Endpunkte im Profil zu verstehen. Zwei Werte sind möglich:
 - Verwenden Sie **1**, wenn der Endpunkt aktiv ist.
 - Verwenden Sie **0**, wenn der Endpunkt ausgefallen ist.

Diese Metrik kann als ein aggregierter Wert angezeigt werden, der den Status aller Metriken darstellt (Abbildung 3). Er kann aber auch unterteilt werden (siehe Abbildung 4), um den Status von bestimmten Endpunkten anzuzeigen. Wenn im ersten Fall als Aggregationsebene **Mittelwert** ausgewählt wird, ist der Wert dieser Metrik der arithmetische Mittelwert des Status aller Endpunkte. Beispiel: Wenn ein Profil zwei Endpunkte aufweist und nur einer fehlerfrei ist, hat diese Metrik den Wert **0,50**, wie in Abbildung 3 gezeigt. 


![Traffic Manager-Metriken – zusammengesetzte Ansicht des Endpunktstatus](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Abbildung 3: Zusammengesetzte Ansicht der Metrik zum Endpunktstatus – Aggregation „Mittelwert“ ausgewählt*


![Traffic Manager-Metriken – unterteilte Ansicht des Endpunktstatus](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Abbildung 4: Unterteilte Ansicht der Metrik zum Endpunktstatus*

Sie können diese Metriken über das Portal des [Azure Monitor-Diensts](../azure-monitor/essentials/metrics-supported.md), [REST-API](/rest/api/monitor/), [Azure CLI](/cli/azure/monitor) und [Azure PowerShell](/powershell/module/az.applicationinsights) oder über den Abschnitt „Metriken“ in der Traffic Manager-Portalumgebung nutzen.

## <a name="alerts-on-traffic-manager-metrics"></a>Warnungen für Traffic Manager-Metriken
Zusätzlich zum Verarbeiten und Anzeigen von Metriken aus Traffic Manager ermöglicht Azure Monitor den Kunden, Warnungen zu konfigurieren und zu empfangen, die mit diesen Metriken verknüpft sind. Sie können auswählen, welche Bedingungen in diesen Metriken erfüllt sein müssen, damit eine Warnung auftritt, wie oft diese Bedingungen beobachtet werden müssen und wie die Warnungen an Sie gesendet werden sollen. Weitere Informationen finden Sie in der [Dokumentation zu Azure Monitor-Warnungen](../azure-monitor/alerts/alerts-metric.md).

Die Warnungsüberwachung ist wichtig, um sicherzustellen, dass das System benachrichtigt, wenn Tests nicht erfolgreich sind. Eine übermäßig empfindliche Überwachung kann ablenkend wirken. Traffic Manager stellt mehrere Tests bereit, um die Resilienz zu erhöhen. Der Schwellenwert für den Status von Tests sollte niedriger als 0,5 sein. Wenn der Durchschnitt für den Status **Erfolgreich** unter 0,5 fällt (d. h. weniger als 50 % der Tests sind erfolgreich), sollte eine Warnung für einen Endpunktfehler ausgegeben werden.

> [!NOTE]
> Es werden mehrere Tests bereitgestellt, um die Resilienz zu erhöhen. Wenn ein Test von den vielen, die gesendet werden, nicht erfolgreich ist, spiegelt dies nicht unbedingt wider, dass der Endpunkt ausgefallen ist. Der Endpunkt wird nur als ausgefallen klassifiziert, wenn der Großteil der zurückgegebenen Tests nicht erfolgreich ist.

Die folgende Konfiguration ist ein Beispiel für eine Warnungseinrichtung.

:::image type="content" source="./media/traffic-manager-metrics-alerts/alert-example.png" alt-text="Screenshot des Beispiels für eine testschwellenwertabhängige Warnung.":::

Weitere Informationen zu Tests und Überwachung finden Sie unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md).

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über den [Azure Monitor-Dienst](../azure-monitor/essentials/metrics-supported.md).
- Erfahren Sie, wie Sie [ein Diagramm mithilfe von Azure Monitor erstellen](../azure-monitor/essentials/metrics-getting-started.md#create-your-first-metric-chart).