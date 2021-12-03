---
title: Verwenden von Microsoft Sentinel mit der Azure Web Application Firewall
description: In diesem Abschnitt zeigen wir Ihnen, wie Sie Microsoft Sentinel mit der Azure Web Application Firewall (WAF) verwenden.
services: web-application-firewall
author: TreMansdoerfer
ms.service: web-application-firewall
ms.date: 10/12/2020
ms.author: victorh
ms.topic: how-to
ms.openlocfilehash: c888b364bfac5687996bb88472b5725244ea1bcb
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132305463"
---
# <a name="using-microsoft-sentinel-with-azure-web-application-firewall"></a>Verwenden von Microsoft Sentinel mit der Azure Web Application Firewall

Die Azure Web Application Firewall (WAF) kann in Kombination mit Microsoft Sentinel Security Informtion Event Management (SIEM) für WAF-Ressourcen bereitstellen. Microsoft Sentinel bietet Sicherheitsanalysen mithilfe von Protokollanalysen, mit der Sie Ihre WAF-Daten problemlos aufschlüsseln und anzeigen können. Mithilfe von Microsoft Sentinel können Sie auf vorgefertigte Arbeitsmappen zugreifen und diese so ändern, dass sie den Anforderungen Ihrer Organisation am besten entsprechen. Die Arbeitsmappe kann Analysen für WAF in Azure Content Delivery Network (CDN), WAF auf Azure Front-Door und WAF auf Application Gateway über mehrere Abonnements und Arbeitsbereiche hinweg anzeigen.

## <a name="waf-log-analytics-categories"></a>WAF Log Analytics-Kategorien

WAF Log Analytics werden in folgende Kategorien unterteilt:  

- Alle ausgeführten WAF-Aktionen 
- Top 40 Adressen blockierter Anforderungs-URIs 
- Top 50 Ereignistrigger  
- Nachrichten im zeitlichen Verlauf 
- Vollständige Nachrichtendetails 
- Angriffsereignisse nach Nachrichten  
- Angriffsereignisse im zeitlichen Verlauf 
- Nachverfolgungs-ID-Filter 
- Nachverfolgungs-ID-Nachrichten 
- Top 10 Angriffs-IP-Adressen 
- Angriffsnachrichten von IP-Adressen 

## <a name="waf-workbook-examples"></a>Beispiele für WAF-Arbeitsmappen

Die folgenden WAF-Arbeitsmappenbeispiele zeigen Beispieldaten:

:::image type="content" source="media//waf-sentinel/waf-actions-filter.png" alt-text="WAF-Aktionenfilter":::

:::image type="content" source="media//waf-sentinel/top-50-event-trigger.png" alt-text="Top 50 Ereignisse":::

:::image type="content" source="media//waf-sentinel/attack-events.png" alt-text="Angriffsereignisse":::

:::image type="content" source="media//waf-sentinel/top-10-attacking-ip-address.png" alt-text="Top 10 Angriffs-IP-Adressen":::

## <a name="launch-a-waf-workbook"></a>Starten einer WAF-Arbeitsmappe

Die WAF-Arbeitsmappe funktioniert für alle Azure Front Door-, Application Gateway- und CDN-WAFs. Bevor Sie die Daten aus diesen Ressourcen verbinden, muss Log Analytics für Ihre Ressource aktiviert werden. 

Um Log Analytics für jede Ressource zu aktivieren, wechseln Sie zu Ihre jeweiligen Azure Front Door-, Application Gateway- oder CDN-Ressource:

1. Wählen Sie **Diagnoseeinstellungen** aus.
2. Wählen Sie **+Diagnoseeinstellung hinzufügen**  aus. 
3. Auf der Seite „Diagnoseeinstellung“:
   1. Geben Sie einen Namen ein. 
   1. Wählen Sie **An Log Analytics senden** aus. 
   1. Wählen Sie den Protokollzielarbeitsbereich aus. 
   1. Wählen Sie die Protokolltypen aus, die Sie analysieren möchten:
      1. Application Gateway: „ApplicationGatewayAccessLog“ und „ApplicationGatewayFirewallLog“
      1. Azure Front Door: „FrontDoorAccessLog“ und „FrontDoorFirewallLog“
      1. CDN: „AzureCdnAccessLog“
   1. Wählen Sie **Speichern**.

   :::image type="content" source="media//waf-sentinel/diagnostics-setting.png" alt-text="Diagnoseeinstellung":::

4. Geben Sie auf der Azure-Homepage *Azure Sentinel* in die Suchleiste ein, und wählen Sie die Ressource **Microsoft Sentinel** aus. 
2. Wählen Sie einen bereits aktiven Arbeitsbereich aus, oder erstellen Sie einen neuen Arbeitsbereich. 
3. Wählen Sie im linken Bereich unter **Konfiguration** die Option **Datenconnectors** aus.
4. Suchen Sie nach **Microsoft Web Application Firewall**, und wählen Sie **Microsoft Web Application Firewall (WAF)** aus. Wählen Sie unten rechts die Seite **Connector öffnen** aus.

   :::image type="content" source="media//waf-sentinel/data-connectors.png" alt-text="Datenconnectors":::

8. Befolgen Sie die Anweisungen unter **Konfiguration** für jede WAF-Ressource, für die Sie Protokollanalysedaten haben möchten, falls Sie dies noch nicht getan haben.
6. Nachdem die Konfiguration einzelner WAF-Ressourcen abgeschlossen ist, wählen Sie die Registerkarte **Nächste Schritte** aus. Wählen Sie eine der empfohlenen Arbeitsmappen aus. Diese Arbeitsmappe verwendet alle Protokollanalysedaten, die zuvor aktiviert wurden. Für Ihre WAF-Ressourcen sollte nun eine funktionierende WAF-Arbeitsmappe vorhanden sein.

   :::image type="content" source="media//waf-sentinel/waf-workbooks.png" alt-text="WAF-Arbeitsmappen":::


## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu Microsoft Sentinel](../sentinel/overview.md)
- [Informationen zu Azure Monitor-Arbeitsmappen](../azure-monitor/visualize/workbooks-overview.md)
