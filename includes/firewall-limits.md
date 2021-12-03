---
title: Datei einfügen
description: include file
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 10/29/2021
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 0f5bafd3bfa25691ee531c5e8aa60b4c70937815
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131520933"
---
| Resource | Begrenzung |
| --- | --- |
| Datendurchsatz |30 GBit/s|
|Regelgrenzwerte|10.000 eindeutige Quellen/Ziele in Netzwerk- und Anwendungsregeln|
|Gesamtgröße von Regeln innerhalb einer einzelnen Regelsammlungsgruppe| 2 MB|
|Anzahl von Regelsammlungsgruppen in einer Firewallrichtlinie|50|
|Maximale Anzahl von DNAT-Regeln|298 (für Firewalls, die mit einer einzigen öffentlichen IP-Adresse konfiguriert sind)<br><br> Die DNAT-Einschränkung ist auf die zugrunde liegende Plattform zurückzuführen. Die maximale Anzahl von DNAT-Regeln beträgt 298. Durch zusätzliche öffentliche IP-Adressen wird die Anzahl verfügbarer DNAT-Regeln jedoch verringert. Beispielsweise lassen zwei öffentliche IP-Adressen 297 DNAT-Regeln zu. Wenn das Protokoll einer Regel sowohl für TCP als auch für UDP konfiguriert ist, zählt dies als zwei Regeln.|
|Mindestgröße für AzureFirewallSubnet |/26|
|Portbereich in Netzwerk- und Anwendungsregeln|1 bis 65535|
|Öffentliche IP-Adressen|Höchstens 250. Alle öffentlichen IP-Adressen können in DNAT-Regeln verwendet werden und zählen zu den verfügbaren SNAT-Ports.|
|IP-Adressen in IP-Gruppen|Maximal 100 IP-Gruppen pro Firewall.<br>Maximal 5000 einzelne IP-Adressen oder IP-Präfixe pro IP-Gruppe.
|Routingtabelle|Standardmäßig verfügt AzureFirewallSubnet über die Route „0.0.0.0/0“, bei der der Wert „NextHopType“ auf **Internet** festgelegt ist.<br><br>Azure Firewall muss über eine direkte Internetverbindung verfügen. Wenn Ihr Subnetz „AzureFirewallSubnet“ eine Standardroute zu Ihrem lokalen Netzwerk über BGP erfasst, müssen Sie diese mit der benutzerdefinierten Route 0.0.0.0/0 überschreiben. Legen Sie dabei den Wert **NextHopType** auf **Internet** fest, um die direkte Internetkonnektivität beizubehalten. Standardmäßig unterstützt Azure Firewall keine Tunnelerzwingung für ein lokales Netzwerk.<br><br>Wenn Ihre Konfiguration jedoch die Tunnelerzwingung für ein lokales Netzwerk erfordert, wird Microsoft dies im Einzelfall unterstützen. Wenden Sie sich in diesem Fall an den Support, damit Ihr Fall überprüft werden kann. Bei einer Annahme wird Ihr Abonnement zugelassen, damit die erforderliche Internetkonnektivität der Firewall auch sicher erhalten bleibt.|
|FQDNs in Netzwerkregeln|Um eine gute Leistung zu erzielen, sollte der Wert von 1.000 FQDNs in allen Netzwerkregeln pro Firewall nicht überschritten werden.|