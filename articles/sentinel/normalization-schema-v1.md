---
title: 'Microsoft Sentinel: Netzwerknormalisierungsschema (Legacyversion – Öffentliche Vorschau) | Microsoft-Dokumentation'
description: Dieser Artikel behandelt das Schema zur Datennormalisierung von Microsoft Sentinel.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.service: microsoft-sentinel
ms.subservice: microsoft-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 11/09/2021
ms.author: yelevin
ms.custom: ignite-fall-2021
ms.openlocfilehash: c5f88a7b5234e2a791d26ecc339e6750497ef6a9
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132522857"
---
# <a name="microsoft-sentinel-network-normalization-schema-legacy-version---public-preview"></a>Microsoft Sentinel: Netzwerknormalisierungsschema (Legacyversion – Öffentliche Vorschau)

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Das Netzwerknormalisierungsschema dient zum Beschreiben gemeldeter Netzwerkereignisse und wird von Microsoft Sentinel verwendet, um quellenunabhängige Analysen zu ermöglichen.

Weitere Informationen finden Sie unter [Normalisierung und das erweiterte SIEM-Informationsmodell (ASIM)](normalization.md).

> [!IMPORTANT]
> Dieser Artikel bezieht sich auf Version 0.1 des Netzwerknormalisierungsschemas, die als Vorschauversion veröffentlicht wurde, bevor ASIM verfügbar war. [Version 0.2](network-normalization-schema.md) des Netzwerknormalisierungsschemas ist an ASIM angepasst und bietet weitere Verbesserungen.
>
> Weitere Informationen finden Sie unter [Unterschiede zwischen Versionen des Netzwerknormalisierungsschemas](#changes).
>

## <a name="terminology"></a>Begriff

In den Microsoft Sentinel-Schemas wird folgende Terminologie verwendet:

| Begriff | Definition |
| ---- | ---------- |
| **Meldendes Gerät** | Das System, das die Datensätze an Microsoft Sentinel sendet. Möglicherweise bildet dieses System nicht das Thema des Datensatzes. |
| **Datensatz** | Eine Dateneinheit, die vom meldenden Gerät gesendet wird. Diese Dateneinheit wird häufig als `log`, `event` oder `alert` angegeben, kann aber auch andere Datentypen aufweisen.|
|

## <a name="data-types-and-formats"></a>Datentypen und Formate

Die folgende Tabelle enthält eine Anleitung zur Normalisierung von Datenwerten, die für normalisierte Felder erforderlich ist und für andere Felder empfohlen wird.

| Datentyp | Physischer Typ | Format und Wert |
| --------- | ------------- | ---------------- |
| **Datum/Uhrzeit** | Je nach verwendeter Erfassungsmethode einen der folgenden (mit absteigender Priorität):<ul><li>Integrierter Log Analytics-Typ „DateTime“</li><li>Ein ganzzahliges Feld, das die numerische datetime-Darstellung von Log Analytics verwendet</li><li>Ein Zeichenfolgenfeld, das die numerische datetime-Darstellung von Log Analytics verwendet</li></ul> | datetime-Darstellung von Log Analytics. <br></br>Die Darstellung von Datums- und Uhrzeitwerten in Log Analytics ähnelt prinzipiell der UNIX-Darstellung von Zeitwerten, weist aber Unterschiede auf. Weitere Informationen finden Sie in diesen Konvertierungsrichtlinien. <br><br>Datum und Uhrzeit müssen den Zeitzonen angepasst werden. |
| **MAC-Adresse** | String | Hexadezimalnotation mit Doppelpunkt |
| **IP-Adresse** | IP-Adresse | Das Schema weist keine getrennten IPv4- und IPv6-Adressen auf. Jedes IP-Adressfeld kann entweder eine IPv4-Adresse oder eine IPv6-Adresse enthalten:<ul><li>IPv4 in einer Punkt-Dezimal-Notation</li><li>IPv6 in 8-Hextett-Notation, die die hier beschriebene Kurzform ermöglicht.</li></ul> |
| **Benutzer** | String | Es stehen die folgenden drei Benutzerfelder zur Verfügung:<ul><li>Benutzername</li><li>Benutzer-UPN</li><li>Benutzerdomäne</li></ul> |
| **Benutzer-ID** | String | Die folgenden zwei Benutzer-IDs werden derzeit unterstützt:<ul><li>Benutzer-SID</li><li>Azure Active Directory-ID</li></ul> |
| **Device** | String | Die folgenden drei Geräte-/Host-Spalten werden unterstützt:<ul><li>id</li><li>Name</li><li>Vollqualifizierter Domänenname (FQDN)</li></ul> |
| **Country** | String | Eine Zeichenfolge gemäß ISO 3166-1, entsprechend der folgenden Priorität:<ul><li>Alpha-2-Codes, z. B. `US` für die USA</li><li>Alpha-3-Codes, z. B. `USA` für die USA</li><li>Kurzname</li></ul> |
| **Region** | String | Der Name der Untereinheiten von Ländern nach ISO 3166-2. |
| **City (Ort)** | String | |
| **Longitude** (Längengrad) | Double | ISO 6709-Koordinatendarstellung (Dezimalwert mit Vorzeichen) |
| **Latitude** (Breitengrad) | Double | ISO 6709-Koordinatendarstellung (Dezimalwert mit Vorzeichen) |
| **Hashalgorithmus** | String | Die folgenden vier Hashspalten werden unterstützt:<ul><li>MD5</li><li>SHA1</li><li>SHA256</li><li>SHA512</li></ul> |
| **Dateityp** | String | Der Typ des Dateityps:<ul><li>Durchwahl</li><li>Klasse</li><li>NamedType</li></ul> |
|

## <a name="network-sessions-table-schema"></a>Tabellenschema für Netzwerksitzungen

Unten finden Sie das Schema der Netzwerksitzungstabelle in Version 1.0.0

| Feldname | Werttyp | Beispiel | Beschreibung | Zugeordnete OSSEM-Entitäten |
|-|-|-|-|-|
| **EventType** | String | Verkehr | Der Typ des erfassten Ereignisses | event |
| **EventSubType** | String | Authentifizierung | Zusätzliche Beschreibung des Typs, falls zutreffend. | Ereignis |
| **EventCount** | Integer  | 10 | Die Anzahl der aggregierten Ereignisse, falls zutreffend. | event |
| **EventEndTime** | Date/Time | Lesen Sie dazu „Datentypen“. | Der Zeitpunkt, zu dem das Ereignis geendet hat | event |
| EventMessage | Zeichenfolge |  Zugriff verweigert | Eine allgemeine Nachricht oder Beschreibung, entweder im Datensatz enthalten oder aus ihm generiert | event |
| **DvcIpAddr** | IP-Adresse |  23.21.23.34 | Die IP-Adresse des Geräts, das den Datensatz erzeugt | Gerät,<br>IP |
| **DvcMacAddr** | String | 06:10:9f:eb:8f:14 | Die MAC-Adresse der Netzwerkschnittstelle des meldenden Geräts, von dem das Ereignis gesendet wurde. | Gerät,<br>Mac |
| **DvcHostname** | Gerätename (Zeichenfolge) | syslogserver1.contoso.com | Der Gerätename des Geräts, das die Nachricht erzeugt. | Sicherungsmedium |
| **EventProduct** | String | OfficeSharepoint | Das Produkt, das das Ereignis erzeugt. | event |
| **EventProductVersion** | Zeichenfolge | 9.0 |  Die Version des Produkts, das das Ereignis erzeugt. | event |
| **EventResourceId** | Geräte-ID (Zeichenfolge) | /subscriptions/3c1bb38c-82e3-4f8d-a115-a7110ba70d05 /resourcegroups/contoso77/providers /microsoft.compute/virtualmachines /syslogserver1 | Der Ressourcen-ID des Geräts, das die Nachricht erzeugt. | event |
| **EventReportUrl** | String | https://192.168.1.1/repoerts/ae3-56.htm | Ein Link zum vollständigen Bericht, der vom meldenden Gerät erstellt wurde. | event |
| **EventVendor** | String | Microsoft | Der Hersteller des Produkts, das das Ereignis erzeugt. | event |
| **EventResult** | Mehrwertig: Success, Partial, Failure, [Empty] (Zeichenfolge) | Erfolg | Der für die Aktivität gemeldete Erfolg. Leerer Wert, wenn nicht zutreffend. | event |
| **EventResultDetails** | String | Falsches Kennwort | Grund oder Details für das in EventResult gemeldete Ergebnis | event |
| **EventSchemaVersion** | Real | 0.1 | Microsoft Sentinel Schema Version. Aktuell 0.1. | event |
| **EventSeverity** | String | Niedrig | Beschreibt den Schweregrad der Auswirkungen, falls die gemeldete Aktivität Sicherheitsauswirkungen hat. | event |
| **EventOriginalUid** | String | af6ae8fe-ff43-4a4c-b537-8635976a2b51 | Die Datensatz-ID des meldenden Geräts. | event |
| **EventStartTime** | Date/Time | Lesen Sie dazu „Datentypen“. | Der Zeitpunkt, zu dem das Ereignis begonnen hat | event |
| **TimeGenerated** | Date/Time | Lesen Sie dazu „Datentypen“. | Der Zeitpunkt, zu dem das Ereignis eingetreten ist, wie von der Meldequelle gemeldet. | Benutzerdefiniertes Feld |
| **EventTimeIngested** | Date/Time | Lesen Sie dazu „Datentypen“. | Der Zeitpunkt, zu dem das Ereignis in Microsoft Sentinel erfasst wurde. Wird von Microsoft Sentinel hinzugefügt. | event |
| **EventUid** | GUID (Zeichenfolge) | 516a64e3-8360-4f1e-a67c-d96b3d52df54 | Eindeutiger Bezeichner, mit dem Microsoft Sentinel eine Zeile markiert. | event |
| **NetworkApplicationProtocol** | String | HTTPS | Das Protokoll der Anwendungsschicht, das von der Verbindung oder der Sitzung verwendet wird. | Netzwerk |
| **DstBytes** | INT | 32455 | Die Anzahl Bytes, die für die Verbindung oder Sitzung vom Ziel an die Quelle gesendet werden. | Destination |
| **SrcBytes** | INT | 46536 | Die Anzahl Bytes, die für die Verbindung oder Sitzung von der Quelle an das Ziel gesendet werden. | `Source` |
| **NetworkBytes** | INT | 78991 | Die Anzahl Bytes, die in beide Richtungen gesendet werden. Wenn sowohl BytesReceived als auch BytesSent vorhanden sind, sollte BytesTotal ihrer Summe entsprechen. | Netzwerk |
| **NetworkDirection** | Mehrwertig: Inbound, Outbound (Zeichenfolge) | Eingehende Verbindungen | Die Richtung der Verbindung oder Sitzung, in die bzw. aus der Organisation. | Netzwerk |
| **DstGeoCity** | String | Burlington | Die Stadt, die der IP-Zieladresse zugeordnet ist. | Ziel,<br>geografischer Raum |
| **DstGeoCountry** | Land/Region (Zeichenfolge) | USA | Das Land/die Region, das bzw. die der IP-Quelladresse zugeordnet ist. | Ziel,<br>geografischer Raum |
| **DstDvcHostname** | Gerätename (Zeichenfolge) |  victim_pc | Der Gerätename des Zielgeräts | Destination<br>Sicherungsmedium |
| **DstDvcFqdn** | String | victim_pc.contoso.local | Der vollqualifizierte Domänenname des Hosts, auf dem das Protokoll erstellt wurde | Ziel,<br>Sicherungsmedium |
| **DstDomainHostname** | Zeichenfolge | CONTOSO | Die Domäne des Ziels, die Domäne des Zielhosts (Website, Domänenname usw.), beispielsweise für DNS-Suchen oder NS-Suchen | Destination |
| **DstInterfaceName** | Zeichenfolge | Microsoft Hyper-V-Netzwerkadapter | Die Netzwerkschnittstelle, die vom Zielgerät für die Verbindung oder Sitzung verwendet wird. | Destination |
| **DstInterfaceGuid** | Zeichenfolge | 2BB33827-6BB6-48DB-8DE6-DB9E0B9F9C9B | Die GUID der Netzwerkschnittstelle, die für die Authentifizierungsanforderung verwendet wurde.  | Destination |
| **DstIpAddr** | IP-Adresse | 2001:db8::ff00:42:8329 | Die IP-Adresse des Verbindungs- oder Sitzungsziels, meistens als Ziel-IP im Netzwerkpaket bezeichnet | Ziel,<br>IP |
| **DstDvcIpAddr** | IP-Adresse | 75.22.12.2 | Die IP-Zieladresse eines Geräts, das dem Netzwerkpaket nicht direkt zugeordnet ist | Ziel,<br>Gerät,<br>IP
| **DstGeoLatitude** | Breitengrad (Double) | 44,475833 | Der Breitengrad der geografischen Koordinate, die der IP-Zieladresse zugeordnet ist. | Ziel,<br>geografischer Raum |
| **DstMacAddr** | String | 06:10:9f:eb:8f:14 | Die MAC-Adresse der Netzwerkschnittstelle, an der die Verbindung oder Sitzung endete, meistens als Ziel-MAC im Netzwerkpaket bezeichnet | Ziel,<br>MAC |
| **DstDvcMacAddr** | String | 06:10:9f:eb:8f:14 | Die MAC-Zieladresse eines Geräts, das dem Netzwerkpaket nicht direkt zugeordnet ist | Ziel,<br>Gerät,<br>MAC |
| **DstDvcDomain** | String | CONTOSO | Die Domäne des Zielgeräts | Ziel,<br>Sicherungsmedium |
| **DstPortNumber** | Integer | 443 | Der Ziel-IP-Port | Ziel,<br>Port |
| **DstGeoRegion** | Region (Zeichenfolge) | Vermont | Die Region innerhalb eines Landes, die der IP-Zieladresse zugeordnet ist | Ziel,<br>geografischer Raum |
| **DstResourceId** | Geräte-ID (Zeichenfolge) |  /subscriptions/3c1bb38c-82e3-4f8d-a115-a7110ba70d05 /resourcegroups/contoso77/providers /microsoft.compute/virtualmachines/victim | Die Ressourcen-ID des Zielgeräts. | Destination |
| **DstNatIpAddr** | IP-Adresse | 2::1 | Wenn sie von einem zwischengeschalteten Gerät wie z. B. einer Firewall gemeldet wird, handelt es sich um die vom NAT-Gerät für die Kommunikation mit der Quelle verwendete IP-Adresse. | Ziel-NAT,<br>IP |
| **DstNatPortNumber** | INT | 443 | Wenn er von einem zwischengeschalteten Gerät wie z. B. einer Firewall gemeldet wird, handelt es sich um den vom NAT-Gerät für die Kommunikation mit der Quelle verwendeten Port. | Ziel-NAT,<br>Port |
| **DstUserSid** | Benutzer-SID |  S-12-1445 | Die Benutzer-ID der Identität, die dem Ziel der Sitzung zugeordnet ist. Normalerweise ist dies die Identität, die zum Authentifizieren eines Servers verwendet wird. Weitere Informationen finden Sie unter [Datentypen und Formate](#data-types-and-formats). | Ziel,<br>Benutzer |
| **DstUserAadId** | Zeichenfolge (GUID) | ae92b0b4-cfba-4b42-85a0-fbd862f4df54 | Die Objekt-ID des Azure AD-Kontos des Benutzers am Zielende der Sitzung | Ziel,<br>Benutzer |
| **DstUserName** | Benutzername (Zeichenfolge) | johnd | Der Benutzername der Identität, die dem Ziel der Sitzung zugeordnet ist.  | Ziel,<br>Benutzer |
| **DstUserUpn** | Zeichenfolge | johnd@anon.com | Der UPN der Identität, die dem Ziel der Sitzung zugeordnet ist. | Ziel,<br>Benutzer |
| **DstUserDomain** | Zeichenfolge | WORKGROUP | Der Domänen- oder Computername des Kontos am Ziel der Sitzung | Ziel,<br>Benutzer |
| **DstZone** | String | Dmz | Die Netzwerkzone des Ziels, wie sie vom meldenden Gerät definiert wird. | Destination |
| **DstGeoLongitude** | Längengrad (Double) | -73,211944 | Der Längengrad der geografischen Koordinate, die der IP-Zieladresse zugeordnet ist. | Ziel,<br>geografischer Raum |
| **DvcAction** | Mehrwertig: Allow, Deny, Drop (Zeichenfolge) | Zulassen | Wenn sie von einem zwischengeschalteten Gerät, z. B. einer Firewall, gemeldet wird, die vom Gerät ausgeführte Aktion. | Sicherungsmedium |
| **DvcInboundInterface** | String | eth0 | Wenn sie von einem zwischengeschalteten Gerät, z. B. einer Firewall, gemeldet wird, die von ihm für die Verbindung mit dem Quellgerät verwendete Netzwerkschnittstelle. | Sicherungsmedium |
| **DvcOutboundInterface** | String  | Ethernet-Adapter Ethernet 4 | Wenn sie von einem zwischengeschalteten Gerät, z. B. einer Firewall, gemeldet wird, die von ihm für die Verbindung mit dem Zielgerät verwendete Netzwerkschnittstelle. | Sicherungsmedium |
| **NetworkDuration** | Integer | 1500 | Die Zeitspanne in Millisekunden für den Abschluss der Netzwerksitzung oder Verbindung | Netzwerk |
| **NetworkIcmpCode** | Integer | 34 | Bei ICMP-Nachrichten der numerische Wert für den ICMP-Nachrichtentyp (RFC 2780 oder RFC 4443). | Netzwerk |
| **NetworkIcmpType** | String | Ziel nicht erreichbar | Bei ICMP-Nachrichten die Textdarstellung des ICMP-Nachrichtentyps (RFC 2780 oder RFC 4443). | Netzwerk |
| **DstPackets** | INT  | 446 | Die Anzahl Pakete, die für die Verbindung oder Sitzung vom Ziel an die Quelle gesendet werden. Die Bedeutung eines Pakets wird vom meldenden Gerät definiert. | Destination |
| **SrcPackets** | INT  | 6478 | Die Anzahl Pakete, die für die Verbindung oder Sitzung von der Quelle ans Ziel gesendet werden. Die Bedeutung eines Pakets wird vom meldenden Gerät definiert. | `Source` |
| **NetworkPackets** | INT  | 0 | Die Anzahl Pakete, die in beide Richtungen gesendet werden. Wenn sowohl PacketsReceived als auch PacketsSent vorhanden sind, sollte NetworkPackets ihrer Summe entsprechen. | Netzwerk |
| **HttpRequestTime** | Integer | 700 | Die Zeitspanne, die zum Senden der Anforderung an den Server benötigt wurde, falls zutreffend. | Http |
| **HttpResponseTime** | Integer | 800 | Die Zeitspanne, die zum Empfangen einer Antwort auf dem Server benötigt wurde, falls zutreffend. | Http |
| **NetworkRuleName** | String | AnyAnyDrop | Der Name oder die ID der Regel, anhand derer die Entscheidung für DeviceAction gefallen ist | Netzwerk |
| **NetworkRuleNumber** | INT |  23 | Zugeordnete Regelnummer  | Netzwerk |
| **NetworkSessionId** | Zeichenfolge | 172_12_53_32_4322__123_64_207_1_80 | Der Sitzungsbezeichner, der vom meldenden Gerät gemeldet wird. Beispiel: Sitzungsbezeichner L7 für bestimmte Anwendungen im Anschluss an die Authentifizierung | Netzwerk |
| **SrcGeoCity** | String | Burlington | Die Stadt, die der IP-Quelladresse zugeordnet ist. | Quelle,<br>geografischer Raum |
| **SrcGeoCountry** | Land/Region (Zeichenfolge) | USA | Das Land/die Region, das bzw. die der IP-Quelladresse zugeordnet ist. | Quelle,<br>geografischer Raum |
| **SrcDvcHostname** | Gerätename (Zeichenfolge) |  Bösewicht | Der Gerätename des Quellgeräts | Quelle,<br>Sicherungsmedium |
| **SrcDvcFqdn** | Zeichenfolge | Villain.malicious.com | Der vollqualifizierte Domänenname des Hosts, auf dem das Protokoll erstellt wurde | Quelle,<br>Sicherungsmedium |
| **SrcDvcDomain** | Zeichenfolge | EVILORG | Die Domäne des Geräts, von dem aus die Sitzung initiiert wurde. | Quelle,<br>Sicherungsmedium |
| **SrcDvcOs** | String | iOS | Das Betriebssystem des Quellgeräts | Quelle,<br>Sicherungsmedium |
| **SrcDvcModelName** | String | Samsung Galaxy Note | Der Modellname des Quellgeräts | Quelle,<br>Sicherungsmedium |
| **SrcDvcModelNumber** | String | 10.0 | Die Modellnummer des Quellgeräts | Quelle,<br>Sicherungsmedium |
| **SrcDvcType** | String | Mobile | Der Typ des Quellgeräts | Quelle,<br> Sicherungsmedium |
| **SrcIntefaceName** | String | eth01 | Die Netzwerkschnittstelle, die vom Quellgerät für die Verbindung oder Sitzung verwendet wird. | `Source` |
| **SrcInterfaceGuid** | String | 46ad544b-eaf0-47ef-827c-266030f545a6 | GUID der verwendeten Netzwerkschnittstelle | `Source` |
| **SrcIpAddr** | IP-Adresse | 77.138.103.108 | Die IP-Adresse, von der die Verbindung oder Sitzung stammt. | Quelle,<br>IP |
| **SrcDvcIpAddr** | IP-Adresse | 77.138.103.108 | Die IP-Quelladresse eines Geräts, das nicht direkt mit dem Netzwerkpaket in Verbindung steht (von einem Anbieter erfasst oder explizit berechnet). | Quelle,<br>Gerät,<br>IP |
| **SrcGeoLatitude** | Breitengrad (Double) | 44,475833 | Der Breitengrad der geografischen Koordinate, die der IP-Quelladresse zugeordnet ist. | Quelle,<br>geografischer Raum |
| **SrcGeoLongitude** | Längengrad (Double) | -73,211944 | Der Längengrad der geografischen Koordinate, die der IP-Quelladresse zugeordnet ist. | Quelle,<br>geografischer Raum |
| **SrcMacAddr** | String | 06:10:9f:eb:8f:14 | Die MAC-Adresse der Netzwerkschnittstelle, von der aus die Verbindung oder Sitzung hergestellt wurde. | Quelle,<br>Mac |
| **SrcDvcMacAddr** | String | 06:10:9f:eb:8f:14 | Die MAC-Quelladresse eines Geräts, das dem Netzwerkpaket nicht direkt zugeordnet ist | Quelle,<br>Gerät,<br>Mac |
| **SrcPortNumber** | Integer | 2335 | Der IP-Port, von dem die Verbindung stammt. Ist möglicherweise für Sitzungen, die mehrere Verbindungen umfassen, nicht relevant. | Quelle,<br>Port |
| **SrcGeoRegion** | Region (Zeichenfolge) | Vermont | Die Region innerhalb eines Landes, die der IP-Quelladresse zugeordnet ist | Quelle,<br>geografischer Raum |
| **SrcResourceId** | String | /subscriptions/3c1bb38c-82e3-4f8d-a115-a7110ba70d05 /resourcegroups/contoso77/providers /microsoft.compute/virtualmachines /syslogserver1 | Der Ressourcen-ID des Geräts, das die Nachricht erzeugt. | `Source` |
| **SrcNatIpAddr** | IP-Adresse | 4.3.2.1 | Wenn sie von einem zwischengeschalteten Gerät wie z. B. einer Firewall gemeldet wird, handelt es sich um die vom NAT-Gerät für die Kommunikation mit dem Ziel verwendete IP-Adresse. | Quell-NAT,<br>IP |
| **SrcNatPortNumber** | Integer | 345 | Wenn er von einem zwischengeschalteten Gerät wie z. B. einer Firewall gemeldet wird, handelt es sich um den vom NAT-Gerät für die Kommunikation mit dem Ziel verwendeten Port. | Quell-NAT,<br>Port |
| **SrcUserSid** | Benutzer-ID (Zeichenfolge) | S-15-1445 | Die Benutzer-ID der Identität, die der Quelle der Sitzung zugeordnet ist. Normalerweise ein Benutzer, der eine Aktion auf dem Client ausführt. Weitere Informationen finden Sie unter [Datentypen und Formate](#data-types-and-formats). | Quelle,<br>Benutzer |
| **SrcUserAadId** | Zeichenfolge (GUID) | 16c8752c-7dd2-4cad-9e03-fb5d1cee5477 | Die Objekt-ID des Azure AD-Kontos des Benutzers am Quellende der Sitzung | Quelle,<br>Benutzer |
| **SrcUserName** | Benutzername (Zeichenfolge) | Bernd | Der Benutzername der Identität, die der Quelle der Sitzung zugeordnet ist. Normalerweise ein Benutzer, der eine Aktion auf dem Client ausführt. Weitere Informationen finden Sie unter [Datentypen und Formate](#data-types-and-formats). | `Source`<br>Benutzer |
| **SrcUserUpn** | Zeichenfolge | bob@alice.com | Der UPN des Kontos, das die Sitzung einleitet | Quelle,<br>Benutzer |
| **SrcUserDomain** | Zeichenfolge | DESKTOP | Die Domäne für das Konto, das die Sitzung einleitet | Quelle,<br>Benutzer |
| **SrcZone** | String | Tippen | Die Netzwerkzone der Quelle, wie sie vom meldenden Gerät definiert wird. | `Source` |
| **NetworkProtocol** | String | TCP | Das IP-Protokoll, das von der Verbindung oder der Sitzung verwendet wird. Normalerweise TCP, UDP oder ICMP. | Netzwerk |
| **CloudAppName** | String | Facebook | Der Name der Zielanwendung für eine HTTP-Anwendung, wie er von einem Proxy identifiziert wird. | Cloud |
| **CloudAppId** | String | 124 | Die ID der Zielanwendung für eine HTTP-Anwendung, wie sie von einem Proxy identifiziert wird. Dieser Wert ist in der Regel spezifisch für den verwendeten Proxy. | Cloud |
| **CloudAppOperation** | String | DeleteFile | Der Vorgang, den der Benutzer im Kontext der Zielanwendung für eine HTTP-Anwendung ausgeführt hat, wie er von einem Proxy identifiziert wird. Dieser Wert ist in der Regel spezifisch für den verwendeten Proxy. | Cloud |
| **CloudAppRiskLevel** | String | 3 | Die Risikostufe, die einer HTTP-Anwendung zugeordnet ist, wie sie durch einen Proxy identifiziert wird. Dieser Wert ist in der Regel spezifisch für den verwendeten Proxy. | Cloud |
| **FileName** | String | ImNotMalicious.exe | Der Dateiname, der für Protokolle wie FTP und HTTP (zur Bereitstellung von Dateinameninformationen) über die Netzwerkverbindungen übertragen wird. | Datei |
| **FilePath** | String | C:\Malicious\ImNotMalicious.exe | Der vollständige Pfad der Datei, einschließlich des Dateinamens | File |
| **FileHashMd5** | String | 51BC68715FC7C109DCEA406B42D9D78F | Der MD5-Hashwert der Datei, die über die Netzwerkverbindungen für Protokolle übertragen wird. | File |
| **FileHashSha1** | String | 491AE3…C299821476F4 | Der SHA1-Hashwert der Datei, die über die Netzwerkverbindungen für Protokolle übertragen wird. | File |
| **FileHashSha256** | String | 9B8F8EDB…C129976F03 | Der SHA256-Hashwert der Datei, die über die Netzwerkverbindungen für Protokolle übertragen wird. | File |
| **FileHashSha512** | String | 5E127D…F69F73F01F361 | Der SHA512-Hashwert der Datei, die über die Netzwerkverbindungen für Protokolle übertragen wird. | File |
| **FileExtension** |  String | exe | Der Typ der Datei, die über die Netzwerkverbindungen für Protokolle übertragen wird, z. B. FTP und HTTP. | File
| **FileMimeType** | String | application/msword | Der MIME-Typ der Datei, der über die Netzwerkverbindungen für Protokolle übertragen wird, z. B. FTP und HTTP. | File |
| **FileSize** | Integer | 23500 | Die Dateigröße der Datei in Byte, die über die Netzwerkverbindungen für Protokolle übertragen wird. | File |
| **HttpVersion** | String | 2.0 | Die HTTP-Anforderungsversion für HTTP/HTTPS-Netzwerkverbindungen. | Http |
| **HttpRequestMethod** | String | GET | Die HTTP-Methode für HTTP/HTTPS-Netzwerksitzungen. | Http |
| **HttpStatusCode** | String | 404 | Der HTTP-Statuscode für HTTP/HTTPS-Netzwerksitzungen. | Http |
| **HttpContentType** | String | multipart/form-data; boundary=beliebiger_wert | Der Content-Type-Header der HTTP-Antwort für HTTP/HTTPS-Netzwerksitzungen. | Http |
| **HttpReferrerOriginal** | String | https://developer.mozilla.org/en-US/docs/Web/JavaScript | Der HTTP-Referrer-Header für HTTP/HTTPS-Netzwerksitzungen. | Http |
| **HttpUserAgentOriginal** | String | Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, z. B. Gecko) Chrome/83.0.4103.97 Safari/537.36 | Der HTTP-Benutzer-Agent-Header für HTTP/HTTPS-Netzwerksitzungen. | Http |
| **HttpRequestXff** | String | 120.12.41.1 | Der HTTP X-Forwarded-For-Header für HTTP/HTTPS-Netzwerksitzungen. | Http |
| **UrlCategory** | String | Suchmaschinen | Die definierte Gruppierung einer URL, die auf der Domäne in der URL basieren kann und mit deren Inhalt zusammenhängt. Beispiel: Nicht jugendfreie Inhalte, Nachrichten, Werbung, geparkte Domänen usw.) | URL |
| **UrlOriginal** | String | https:// contoso.com/fo/?k=v&q=u#f | Die HTTP-Anforderungs-URL für HTTP/HTTPS-Netzwerksitzungen. | url |
| **UrlHostname** | String | contoso.com | Der Domänenteil einer HTTP-Anforderungs-URL für HTTP/HTTPS-Netzwerksitzungen. | url |
| **ThreatCategory** | String | Trojaner | Die Kategorie einer Bedrohung, die von einem Sicherheitssystem, z. B. dem Web Security Gateway eines IPS, identifiziert wird und dieser Netzwerksitzung zugeordnet ist. | Bedrohung |
| **ThreatId** | String | Tr.124 | Die ID einer Bedrohung, die von einem Sicherheitssystem, z. B. dem Web Security Gateway eines IPS, identifiziert wird und dieser Netzwerksitzung zugeordnet ist. | Bedrohung |
| **ThreatName** | String | EICAR-Testdatei | Der Name der identifizierten Bedrohung oder Schadsoftware | Bedrohung |
| **AdditionalFields** | Dynamisch (JSON-Behälter) | {<br>Eigenschaft1: „wert1“,<br>Eigenschaft2: „wert2“<br>} | Wenn keine entsprechende Spalte im Schema übereinstimmt, können weitere Felder in einem JSON-Behälter gespeichert werden.<br>Für die Analyse der Abfragezeit wird empfohlen, anstelle eines JSON-Behälters zusätzliche Spalten zu verwenden, da das Packen von Daten in JSON-Code die Abfrageleistung beeinträchtigt. | Benutzerdefiniertes Feld |
|


## <a name="differences-between-the-version-01-and-version-02"></a><a name="changes"></a>Unterschiede zwischen Version 0.1 und Version 0.2

Die ursprüngliche Version des Schemas zur Netzwerksitzungsnormalisierung von Microsoft Sentinel, Version 0.1, wurde als Vorschauversion veröffentlicht, bevor ASIM verfügbar war.

Zu den Unterschieden zwischen Version 0.1, die in diesem Artikel dokumentiert ist, und [Version 0.2](network-normalization-schema.md) gehören:

- In Version 0.2 wurden die quellenunabhängigen und quellenspezifischen Parsernamen geändert, um einer ASIM-Standardnamenskonvention zu entsprechen.
- Version 0.2 fügt spezifische Richtlinien und quellenunabhängige Parser hinzu, um bestimmten Gerätetypen zu entsprechen.

In den folgenden Abschnitten wird beschrieben, wie [Version 0.2](network-normalization-schema.md) bei bestimmten Feldern abweicht.

### <a name="added-fields-in-version-02"></a>Hinzugefügte Felder in Version 0.2

Die folgenden Felder wurden in [Version 0.2](network-normalization-schema.md) hinzugefügt und sind in Version 0.1 nicht vorhanden:

:::row:::
   :::column span="":::
      - DstAppType
      - DstDeviceType
      - DstDomainType
      - DstDvcId
      - DstDvcIdType
      - DstOriginalUserType
      - DstUserIdType
      - DstUsernameType
      - DstUserType
      - DvcActionOriginal
      - DvcDomain
   :::column-end:::
   :::column span="":::
      - DvcDomainType
      - DvcFQDN
      - DvcId
      - DvcIdType
      - DvcIdType
      - EventOriginalSeverity
      - EventOriginalType
      - SrcAppId
      - SrcAppName
      - SrcAppType
   :::column-end:::
   :::column span="":::
      - SrcDeviceType
      - SrcDomainType
      - SrcDvcId
      - SrcDvcIdType
      - SrcOriginalUserType
      - SrcUserIdType
      - SrcUsernameType
      - SrcUserType
      - ThreatRiskLevelOriginal
      - url
   :::column-end:::
:::row-end:::


### <a name="newly-aliased-fields-in-version-02"></a>Felder mit neuem Alias in Version 0.2

Die folgenden Felder erhalten jetzt in [Version 0.2](network-normalization-schema.md) mit der Einführung von ASIM einen Alias:

|Feld in Version 0.1  |Alias in Version 0.2  |
|---------|---------|
|SessionID     |     NetworkSessionId    |
|Duration     |     NetworkDuration    |
|IpAddr     | SrcIpAddr        |
|Benutzer     |     DstUsername    |
|Hostname     |   DstHostname      |
|UserAgent     |     HttpUserAgent    |
|     |         |

### <a name="modified-fields-in-version-02"></a>Geänderte Felder in Version 0.2

Die folgenden Felder sind in Version [0.2](network-normalization-schema.md) aufgeführt und erfordern einen bestimmten Wert aus einer angegebenen Liste.

- EventType
- EventResultDetails
- EventSeverity

### <a name="renamed-fields-in-version-02"></a>Umbenannte Felder in Version 0.2

Die folgenden Felder wurden in [Version 0.2](network-normalization-schema.md) umbenannt:

- **Verwenden Sie in Version 0.2 die integrierten Log Analytics-Felder:**

    Beachten Sie, dass es sich bei `ingestion_time()` um eine KQL-Funktion und nicht um einen Feldnamen handelt.

    |Feld in Version 0.1  |Umbenannt in Version 0.2  |
    |---------|---------|
    |  EventResourceId  |   _ResourceId      |
    | EventUid   |     _ItemId    |
    | EventTimeIngested   |  ingestion_time()       |
    |    |         |

- **Umbenannt, um den Verbesserungen in ASIM und OSSEM zu entsprechen**:

    |Feld in Version 0.1  |Umbenannt in Version 0.2  |
    |---------|---------|
    |  HttpReferrerOriginal  |   HttpReferrer      |
    | HttpUserAgentOriginal   |     HttpUserAgent    |
    |    |         |

- **Umbenannt, um widerzuspiegeln, dass das Ziel der Netzwerksitzung kein Clouddienst sein muss**:

    |Feld in Version 0.1  |Umbenannt in Version 0.2  |
    |---------|---------|
    |  CloudAppId  |   DstAppId      |
    | CloudAppName   |     DstAppName    |
    | CloudAppRiskLevel   |  ThreatRiskLevel       |
    |    |         |

- **Umbenannt, um den Fall zu ändern und der ASIM-Behandlung der Benutzerentität zu entsprechen**:

    |Feld in Version 0.1  |Umbenannt in Version 0.2  |
    |---------|---------|
    |  DstUserName  |   DstUsername      |
    | SrcUserName   |     SrcUsername    |
    |    |         |

- **Umbenannt, um besser der ASIM-Geräteentität zu entsprechen und andere Ressourcen-IDs als die von Azure zu ermöglichen**:

    |Feld in Version 0.1  |Umbenannt in Version 0.2  |
    |---------|---------|
    |  DstResourceId  |   SrcDvcAzureRerouceId      |
    | SrcResourceId   |     SrcDvcAzureRerouceId    |
    |    |         |

- **Umbenannt, um die Zeichenfolge `Dvc` aus Feldnamen zu entfernen, da die Behandlung in Version 0.1 inkonsistent war**.

    |Feld in Version 0.1  |Umbenannt in Version 0.2  |
    |---------|---------|
    |  DstDvcDomain  |   DstDomain      |
    | DstDvcFqdn   |     DstFqdn    |
    |  DstDvcHostname  |   DstHostname      |
    | SrcDvcDomain   |     SrcDomain    |
    |  SrcDvcFqdn  |   SrcFqdn      |
    | SrcDvcHostname   |     SrcHostname    |
    |    |         |

- **Umbenannt, um der Richtlinie für die ASIM-Dateidarstellung zu entsprechen**:

    |Feld in Version 0.1  |Umbenannt in Version 0.2  |
    |---------|---------|
    |  FileHashMd5  |   FileMD5      |
    | FileHashSha1   |     FileSHA1    |
    |  FileHashSha256  |   FileSHA256      |
    | FileHashSha512   |     FileSHA512    |
    |  FileMimeType  |   FileContentType      |
    |    |         |

### <a name="removed-fields-in-version-02"></a>Entfernte Felder in Version 0.2

Die folgenden Felder sind nur in Version 0.1 vorhanden und wurden in [Version 0.2](network-normalization-schema.md) entfernt:

|`Reason`  |Entfernte Felder  |
|---------|---------|
|**Entfernt, da Duplikate vorhanden sind, ohne die Zeichenfolge `Dvc` im Feldnamen**     |  - DstDvcIpAddr<br> - DstDvcMacAddr<br>- SrcDvcIpAddr<br>- SrcDvcMacAddr       |
|**Entfernt, um der ASIM-Behandlung von URLs zu entsprechen**     |  - UrlHostname       |
|**Entfernt, da diese Felder in der Regel nicht als Teil von Netzwerksitzungsereignissen bereitgestellt werden**<br><br>Wenn ein Ereignis diese Felder enthält, verwenden Sie das [Prozessereignisschema](process-events-normalization-schema.md), um zu verstehen, wie Geräteeigenschaften beschrieben werden können. |     - SrcDvcOs<br>-&nbsp;SrcDvcModelName<br>-&nbsp;SrcDvcModelNumber<br>- DvcMacAddr<br>- DvcOs         |
|**Entfernt, um der Richtlinie für die ASIM-Dateidarstellung zu entsprechen**     |   - FilePath<br>- FileExtension      |
|**Entfernt, da dieses Feld angibt, dass ein anderes Schema verwendet werden soll, z. B. das [Authentifizierungsschema](authentication-normalization-schema.md).**     |  - CloudAppOperation       |
|**Entfernt, da es `DstHostname` dupliziert**     |  - DstDomainHostname         |
|     |         |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter

- [Normalisierung in Microsoft Sentinel](normalization.md)
- [Microsoft Sentinel: Referenz zum Schema zur Normalisierung der Authentifizierung (Public Preview)](authentication-normalization-schema.md)
- [Microsoft Sentinel: Referenz zum Schema zur Normalisierung von Dateiereignissen (Public Preview)](file-event-normalization-schema.md)
- [Microsoft Sentinel: Referenz zum DNS-Normalisierungsschema](dns-normalization-schema.md)
- [Microsoft Sentinel: Referenz zum Schema zur Normalisierung von Prozessereignissen](process-events-normalization-schema.md)
- [Microsoft Sentinel: Referenz zum Schema zur Normalisierung von Registrierungsereignissen (Public Preview)](registry-event-normalization-schema.md)
