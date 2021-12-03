---
title: 'Standorte und Konnektivitätsanbieter: Azure ExpressRoute | Microsoft-Dokumentation'
description: Dieser Artikel enthält eine ausführliche Übersicht über die Standorte, an denen Dienste angeboten werden, sowie Informationen darüber, wie Sie eine Verbindung mit den Azure-Regionen herstellen können. Sortiert nach Standort.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/26/2021
ms.author: duau
ms.openlocfilehash: 0623726022e909c2bc1be405e2afb155142658be
ms.sourcegitcommit: 838413a8fc8cd53581973472b7832d87c58e3d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2021
ms.locfileid: "132135026"
---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute-Partner und Peeringstandorte

> [!div class="op_single_selector"]
> * [Standorte nach Anbieter](expressroute-locations.md)
> * [Anbieter nach Standort](expressroute-locations-providers.md)


In den Tabellen in diesem Artikel finden Sie Informationen zum geografischen Geltungsbereich von ExpressRoute sowie zu ExpressRoute-Standorten, zu ExpressRoute-Konnektivitätsanbietern und zu ExpressRoute-Systemintegratoren (SIs).

> [!Note]
> Azure-Regionen und ExpressRoute-Standorte sind zwei unterschiedliche Konzepte, und das Verstehen der Unterschiede zwischen den beiden ist wichtig für die Untersuchung der Konnektivität von Azure-Hybridnetzwerken. 
>
>

## <a name="azure-regions"></a>Azure-Regionen
Azure-Regionen sind globale Rechenzentren, in denen sich Azure Compute-, Netzwerk- und Speicherressourcen befinden. Beim Erstellen einer Azure-Ressource muss ein Kunde einen Ressourcenspeicherort auswählen. Der Ressourcenspeicherort bestimmt, in welchem Azure-Rechenzentrum (oder in welcher Verfügbarkeitszone) die Ressource erstellt wird.

## <a name="expressroute-locations"></a>ExpressRoute-Standorte
ExpressRoute-Standorte (manchmal auch als „Peeringstandorte“ oder „Meet-Me-Standorte“ bezeichnet) sind Kollokationsumgebungen, in denen sich MSEE-Geräte (Microsoft Enterprise Edge) befinden. ExpressRoute-Standorte sind der Einstiegspunkt in das Netzwerk von Microsoft. Sie sind global verteilt, sodass Kunden die Möglichkeit erhalten, eine Verbindung mit dem Microsoft-Netzwerk weltweit herzustellen. An diesen Standorten stellen ExpressRoute-Partner und ExpressRoute Direct-Kunden Querverbindungen mit dem Microsoft-Netzwerk her. Im Allgemeinen muss der ExpressRoute-Standort der Azure-Region nicht entsprechen. Ein Kunde kann beispielsweise eine ExpressRoute-Leitung mit dem Ressourcenspeicherort *USA, Osten* am Peeringstandort *Seattle* erstellen.

Wenn Sie mit mindestens einem ExpressRoute-Standort innerhalb der geopolitischen Region eine Verbindung hergestellt haben, haben Sie Zugriff auf die Azure-Dienste in allen Regionen dieser geopolitischen Region. 

[!INCLUDE [expressroute-azure-regions-geopolitical-region](../../includes/expressroute-azure-regions-geopolitical-region.md)]

## <a name="expressroute-connectivity-providers"></a><a name="partners"></a>ExpressRoute-Konnektivitätsanbieter

Die folgende Tabelle enthält die Konnektivitätsstandorte und die dazugehörigen Service Providers. Sie können auch eine Tabelle anzeigen ([Standorte nach Service Provider](expressroute-locations.md)), die nach Service Providern mit den jeweiligen Standorten sortiert ist.

* **Lokale Azure-Regionen** sind diejenigen, auf die [ExpressRoute Local](expressroute-faqs.md) an jedem Peeringstandort zugreifen kann. **–** gibt an, dass ExpressRoute Local an diesem Peeringstandort nicht verfügbar ist.

* **Zone** bezieht sich auf [Preise](https://azure.microsoft.com/pricing/details/expressroute/).

### <a name="global-commercial-azure"></a>Globales Azure Commercial
| **Location** | **Adresse** | **Zone** | **Lokale Azure-Regionen** | **ER Direct** | **Service Providers** |
| --- | --- | --- | --- | --- | --- |
| **Amsterdam** | [Equinix AM5](https://www.equinix.com/locations/europe-colocation/netherlands-colocation/amsterdam-data-centers/am5/) | 1 | Europa, Westen | 10G, 100G | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Interxion, KPN, IX Reach, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, Telefonica, Telenor, Telia Carrier, Verizon, Zayo |
| **Amsterdam2** | [Interxion AMS8](https://www.interxion.com/Locations/amsterdam/schiphol/) | 1 | Europa, Westen | 10G, 100G | BICS, British Telecom, CenturyLink Cloud Connect, Colt, DE-CIX, Equinix, euNetworks, GÉANT, Interxion, NOS, NTT Global DataCenters EMEA, Orange, Vodafone |
| **Atlanta** | [Equinix AT2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/atlanta-data-centers/at2/) | 1 | – | 10G, 100G | Equinix, Megaport |
| **Auckland** | [Vocus Group NZ Albany](https://www.vocus.co.nz/business/cloud-data-centres) | 2 | – | 10G | Devoli, Kordia, Megaport, REANNZ, Spark NZ, Vocus Group NZ |
| **Bangkok** | [AIS](https://business.ais.co.th/solution/en/azure-expressroute.html) | 2 | – | 10G | AIS, National Telecom UIH |
| **Berlin** | [NTT GDC](https://www.e-shelter.de/en/location/berlin-1-data-center) | 1 | Deutschland, Norden | 10G | Colt, Equinix, NTT Global DataCenters EMEA|
| **Bogota** | [Equinix BG1](https://www.equinix.com/locations/americas-colocation/colombia-colocation/bogota-data-centers/bg1/) | 4 | – | 10G | Equinix |
| **Busan** | [LG CNS](https://www.lgcns.com/En/Service/DataCenter) | 2 | Korea, Süden | – | LG CNS |
| **Campinas** | [Ascenty](https://www.ascenty.com/en/data-centers-en/campinas/) | 3 | Brasilien Süd | 10G, 100G | Ascenty |
| **Canberra** | [CDC](https://cdcdatacentres.com.au/about-us/) | 1 | Australien, Mitte | 10G, 100G | CDC |
| **Canberra2** | [CDC](https://cdcdatacentres.com.au/about-us/) | 1 | Australien, Mitte 2| 10G, 100G | CDC, Equinix |
| **Kapstadt** | [Teraco CT1](https://www.teraco.co.za/data-centre-locations/cape-town/) | 3 | Südafrika, Westen | 10G | BCX, Internet Solutions – Cloud Connect, Liquid Telecom, MTN Global Connect, Teraco, Vodacom |
| **Chennai** | Tata Communications | 2 | Indien (Süden) | 10G | BSNL, Global CloudXchange (GCX), SIFY, Tata Communications, VodafoneIdea |
| **Chennai2** | Airtel | 2 | Indien (Süden) | 10G | Airtel |
| **Chicago** | [Equinix CH1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/chicago-data-centers/ch1/) | 1 | USA Nord Mitte | 10G, 100G | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Cologix, Colt, Comcast, Coresite, Equinix, InterCloud, Internet2, Level 3 Communications, Megaport, PacketFabric, PCCW Global Limited, Sprint, Telia Carrier, Verizon, Zayo |
| **Chicago2** | [CoreSite CH1](https://www.coresite.com/data-center/ch1-chicago-il) | 1 | USA Nord Mitte | 10G, 100G | CoreSite |
| **Kopenhagen** | [Interxion CPH1](https://www.interxion.com/Locations/copenhagen/) | 1 | – | 10G | Interxion |
| **Dallas** | [Equinix DA3](https://www.equinix.com/locations/americas-colocation/united-states-colocation/dallas-data-centers/da3/) | 1 | – | 10G, 100G | Aryaka Networks, AT&T NetBond, Cologix, Equinix, Internet2, Level 3 Communications, Megaport, Neutrona Networks, PacketFabric, Telmex Uninet, Telia Carrier, Transtelco, Verizon, Zayo|
| **Denver** | [CoreSite DE1](https://www.coresite.com/data-centers/locations/denver/de1) | 1 | USA, Westen-Mitte | 10G, 100G | CoreSite, Megaport, PacketFabric, Zayo |
| **Dubai** | [PCCS](https://www.pacificcontrols.net/cloudservices/index.html) | 3 | Vereinigte Arabische Emirate, Norden | – | Etisalat (VAE) |
| **Dubai2** | [du datamena](http://datamena.com/solutions/data-centre) | 3 | Vereinigte Arabische Emirate, Norden | – | DE-CIX, du datamena, Equinix, GBI, Megaport, Orange, Orixcom |
| **Dublin** | [Equinix DB3](https://www.equinix.com/locations/europe-colocation/ireland-colocation/dublin-data-centers/db3/) | 1 | Nordeuropa | 10G, 100G | CenturyLink Cloud Connect, Colt, eir, Equinix, GEANT, euNetworks, Interxion, Megaport |
| **Dublin2** | [Interxion DUB2](https://www.interxion.com/locations/europe/dublin) | 1 | Nordeuropa | 10G, 100G | Interxion |
| **Frankfurt** | [Interxion FRA11](https://www.interxion.com/Locations/frankfurt/) | 1 | Deutschland, Westen-Mitte | 10G, 100G | AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, DE-CIX, Equinix, euNetworks, GBI, GEANT, InterCloud, Interxion, Megaport, NTT Global DataCenters EMEA, Orange, Telia Carrier, T-Systems |
| **Frankfurt2** | [Equinix FR7](https://www.equinix.com/locations/europe-colocation/germany-colocation/frankfurt-data-centers/fr7/) | 1 | Deutschland, Westen-Mitte | 10G, 100G | Deutsche Telekom AG, Equinix |
| **Genf** | [Equinix GV2](https://www.equinix.com/locations/europe-colocation/switzerland-colocation/geneva-data-centers/gv2/) | 1 | Schweiz, Westen | 10G, 100G | Colt, Equinix, Megaport, Swisscom |
| **Hongkong** | [Equinix HK1](https://www.equinix.com/data-centers/asia-pacific-colocation/hong-kong-colocation/hong-kong-data-centers/hk1) | 2 | Asien, Osten | 10G | Aryaka Networks, British Telecom, CenturyLink Cloud Connect, Chief Telecom, China Telecom Global, China Unicom, Colt, Equinix, InterCloud, Megaport, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telia Carrier, Verizon |
| **Hongkong2** | [iAdvantage MEGA-i](https://www.iadvantage.net/index.php/locations/mega-i) | 2 | Asien, Osten | 10G | China Mobile International, China Telecom Global, iAdvantage, Megaport, PCCW Global Limited, SingTel |
| **Jakarta** | [Telin](https://www.telin.net/) | 4 | – | 10G | NTT Communications, Telin, XL Axiata |
| **Johannesburg** | [Teraco JB1](https://www.teraco.co.za/data-centre-locations/johannesburg/#jb1) | 3 | Südafrika, Norden | 10G | BCX, British Telecom, Internet Solutions – Cloud Connect, Liquid Telecom, MTN Global Connect, Orange, Teraco, Vodacom |
| **Kuala Lumpur** | [TIME dotCom Menara AIMS](https://www.time.com.my/enterprise/connectivity/direct-cloud) | 2 | – | – | TIME dotCom |
| **Las Vegas** | [Switch LV](https://www.switch.com/las-vegas) | 1 | – | 10G, 100G | CenturyLink Cloud Connect, Megaport, PacketFabric |
| **London** | [Equinix LD5](https://www.equinix.com/locations/europe-colocation/united-kingdom-colocation/london-data-centers/ld5/) | 1 | UK, Süden | 10G, 100G | AT&T NetBond, British Telecom, CenturyLink, Colt, Equinix, euNetworks, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telehouse - KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **London2** | [Telehouse North Two](https://www.telehouse.net/data-centres/emea/uk-data-centres/london-data-centres/north-two) | 1 | UK, Süden | 10G, 100G | BICS, British Telecom, CenturyLink Cloud Connect, Colt, GTT, IX Reach, Equinix, JISC, Megaport, NTT Global DataCenters EMEA, SES, Sohonet, Telehouse – KDDI |
| **Los Angeles** | [CoreSite LA1](https://www.coresite.com/data-centers/locations/los-angeles/one-wilshire) | 1 | – | 10G, 100G | CoreSite, Equinix*, Megaport, Neutrona Networks, NTT, Zayo</br></br> **Neue ExpressRoute-Leitungen werden mit Equinix in Los Angeles nicht mehr unterstützt. Erstellen Sie neue Leitungen in Los Angeles2.* |
| **Los Angeles2** | [Equinix LA1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/los-angeles-data-centers/la1/) | 1 | – | 10G, 100G | Equinix |
| **Madrid** | [Interxion MAD1](https://www.interxion.com/es/donde-estamos/europa/madrid) | 1 | Europa, Westen | 10G, 100G | Interxion, Megaport |
| **Marseille** |[Interxion MRS1](https://www.interxion.com/Locations/marseille/) | 1 | Frankreich, Süden | – | Colt, DE-CIX, GEANT, Interxion, Jaguar Network, Ooredoo Cloud Connect |
| **Melbourne** | [NextDC M1](https://www.nextdc.com/data-centres/m1-melbourne-data-centre) | 2 | Australien, Südosten | 10G, 100G | AARNet, Devoli, Equinix, Megaport, NEXTDC, Optus, Telstra Corporation, TPG Telecom |
| **Miami** | [Equinix MI1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/miami-data-centers/mi1/) | 1 | – | 10G, 100G | Claro, C3ntro, Equinix, Megaport, Neutrona Networks |
| **Mailand** | [IRIDEOS](https://irideos.it/en/data-centers/) | 1 | – | 10G | Colt, Equinix, Fastweb, IRIDEOS, Retelit |
| **Minneapolis** | [Cologix MIN1](https://www.cologix.com/data-centers/minneapolis/min1/) | 1 | – | 10G, 100G | Cologix, Megaport |
| **Montreal** | [Cologix MTL3](https://www.cologix.com/data-centers/montreal/mtl3/) | 1 | – | 10G, 100G | Bell Canada, Cologix, Fibrenoire, Megaport, Telus, Zayo |
| **Mumbai** | Tata Communications | 2 | Indien, Westen | 10G | BSNL, DE-CIX, Global CloudXchange (GCX), Reliance Jio, Sify, Tata Communications, Verizon |
| **Mumbai2** | Airtel | 2 | Indien, Westen | 10G | Airtel, Sify, Vodafone Idea |
| **München** | [EdgeConneX](https://www.edgeconnex.com/locations/europe/munich/) | 1 | – | 10G | Colt, DE-CIX, Megaport |
| **New York** | [Equinix NY5](https://www.equinix.com/locations/americas-colocation/united-states-colocation/new-york-data-centers/ny5/) | 1 | – | 10G, 100G | CenturyLink Cloud Connect, Colt, Coresite, DE-CIX, Equinix, InterCloud, Megaport, Packet, Zayo |
| **Newport(Wales)** | [Daten der nächsten Generation](https://www.nextgenerationdata.co.uk) | 1 | UK, Westen | 10G, 100G | British Telecom, Colt, Jisc, Level 3 Communications, Next Generation Data |
| **Osaka** | [Equinix OS1](https://www.equinix.com/locations/asia-colocation/japan-colocation/osaka-data-centers/os1/) | 2 | Japan, Westen | 10G, 100G | AT TOKYO, BBIX, Colt, Equinix, Internet Initiative Japan Inc. – IIJ, Megaport, NTT Communications, NTT SmartConnect, Softbank, Tokai Communications |
| **Oslo** | [DigiPlex Ulven](https://www.digiplex.com/locations/oslo-datacentre) | 1 | Norwegen, Osten | 10G, 100G | GlobalConnect, Megaport, Telenor, Telia Carrier |
| **Paris** | [Interxion PAR5](https://www.interxion.com/Locations/paris/) | 1 | Frankreich, Mitte | 10G, 100G | British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Intercloud, Interxion, Jaguar Network, Megaport, Orange, Telia Carrier, Zayo |
| **Perth** | [NextDC P1](https://www.nextdc.com/data-centres/p1-perth-data-centre) | 2 | – | 10G | Megaport, NextDC |
| **Phoenix** | [EdgeConneX PHX01](https://www.edgeconnex.com/locations/north-america/phoenix-az/) | 1 | USA, Westen 3 | 10G, 100G | Megaport, Zayo |
| **Pune** | [STT GDC Pune DC1](https://www.sttelemediagdc.in/our-data-centres-in-india) | 2 | Indien, Mitte| 10G | |
| **Quebec City** | [Vantage](https://vantage-dc.com/data_centers/quebec-city-data-center-campus/) | 1 | Kanada, Osten | 10G, 100G | Bell Canada, Equinix, Megaport, Telus |
| **Queretaro (Mexiko)** | [KIO Networks QR01](https://www.kionetworks.com/es-mx/) | 4 | – | 10G | Transtelco|
| **Quincy** | [Sabey Datacenter – Building A](https://sabeydatacenters.com/data-center-locations/central-washington-data-centers/quincy-data-center) | 1 | USA, Westen 2 | 10G, 100G | |
| **Rio de Janeiro** | [Equinix-RJ2](https://www.equinix.com/locations/americas-colocation/brazil-colocation/rio-de-janeiro-data-centers/rj2/) | 3 | Brasilien, Südosten | 10G | Equinix |
| **San Antonio** | [CyrusOne SA1](https://cyrusone.com/locations/texas/san-antonio-texas/) | 1 | USA Süd Mitte | 10G, 100G | CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** | [Equinix SP2](https://www.equinix.com/locations/americas-colocation/brazil-colocation/sao-paulo-data-centers/sp2/) | 3 | Brasilien Süd | 10G, 100G | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica, UOLDIVEO |
| **Sao Paulo2** | [TIVIT TSM](https://www.tivit.com/en/tivit/) | 3 | Brasilien Süd | 10G, 100G | |
| **Seattle** | [Equinix SE2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/seattle-data-centers/se2/) | 1 | USA, Westen 2 | 10G, 100G | Aryaka Networks, Equinix, Level 3 Communications, Megaport, Telus, Zayo |
| **Seoul** | [KINX Gasan IDC](https://www.kinx.net/?lang=en) | 2 | Korea, Mitte | 10G, 100G | KINX, KT, LG CNS, LGUplus, Equinix, Sejong Telecom, SK Telecom |
| **Seoul2** | [KT IDC](https://www.kt-idc.com/eng/introduce/sub1_4_10.jsp#tab) | 2 | Korea, Mitte | – | |
| **Silicon Valley** | [Equinix SV1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/silicon-valley-data-centers/sv1/) | 1 | USA (Westen) | 10G, 100G | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, Comcast, Coresite, Equinix, InterCloud, Internet2, IX Reach, Packet, PacketFabric, Level 3 Communications, Megaport, Orange, Sprint, Tata Communications, Telia Carrier, Verizon, Zayo |
| **Silicon Valley2** | [Coresite SV7](https://www.coresite.com/data-centers/locations/silicon-valley/sv7) | 1 | USA (Westen) | 10G, 100G | Colt, Coresite | 
| **Singapur** | [Equinix SG1](https://www.equinix.com/data-centers/asia-pacific-colocation/singapore-colocation/singapore-data-center/sg1) | 2 | Asien, Südosten | 10G, 100G | Aryaka Networks, AT&T NetBond, British Telecom, China Mobile International, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon, Vodafone |
| **Singapur2** | [Global Switch Tai Seng](https://www.globalswitch.com/locations/singapore-data-centres/) | 2 | Asien, Südosten | 10G, 100G | CenturyLink Cloud Connect, China Unicom Global, Colt, Epsilon Global Communications, Equinix, Megaport, PCCW Global Limited, SingTel, Telehouse – KDDI |
| **Stavanger** | [Green Mountain DC1](https://greenmountain.no/dc1-stavanger/) | 1 | Norwegen, Westen | 10G, 100G |GlobalConnect, Megaport |
| **Stockholm** | [Equinix SK1](https://www.equinix.com/locations/europe-colocation/sweden-colocation/stockholm-data-centers/sk1/) | 1 | – | 10G | Equinix, Megaport, Telia Carrier |
| **Sydney** | [Equinix SY2](https://www.equinix.com/locations/asia-colocation/australia-colocation/sydney-data-centers/sy2/) | 2 | Australien (Osten) | 10G, 100G | AARNet, AT&T NetBond, British Telecom, Devoli, Equinix, Kordia, Megaport, NEXTDC, NTT Communications, Optus, Orange, Spark NZ, Telstra Corporation, TPG Telecom, Verizon, Vocus Group NZ |
| **Sydney2** | [NextDC S1](https://www.nextdc.com/data-centres/s1-sydney-data-centre) | 2 | Australien (Osten) | 10G, 100G | Megaport, NextDC |
| **Taipeh** | Chief Telecom | 2 | – | 10G | Chief Telecom, Chunghwa Telecom, FarEasTone |
| **Tokio** | [Equinix TY4](https://www.equinix.com/locations/asia-colocation/japan-colocation/tokyo-data-centers/ty4/) | 2 | Japan, Osten | 10G, 100G | Aryaka Networks, AT&T NetBond, BBIX, British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Intercloud, Internet Initiative Japan Inc. – IIJ, Megaport, NTT Communications, NTT EAST, Orange, Softbank, Telehouse – KDDI, Verizon |
| **Tokio2** | [AT TOKYO](https://www.attokyo.com/) | 2 | Japan, Osten | 10G, 100G | AT TOKYO, China Unicom Global, Megaport, Tokai Communications |
| **Toronto** | [Cologix TOR1](https://www.cologix.com/data-centers/toronto/tor1/) | 1 | Kanada, Mitte | 10G, 100G | AT&T NetBond, Bell Canada, CenturyLink Cloud Connect, Cologix, Equinix, IX Reach Megaport, Telus, Verizon, Zayo |
| **Toronto2** | [Allied REIT](https://www.alliedreit.com/property/905-king-st-w/) | 1 | Kanada, Mitte | 10G, 100G | |
| **Vancouver** | [Cologix VAN1](https://www.cologix.com/data-centers/vancouver/van1/) | 1 | – | 10G | Bell Canada, Cologix, Megaport, Telus |
| **Washington DC** | [Equinix DC2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/washington-dc-data-centers/dc2/), [Equinix DC6](https://www.equinix.com/data-centers/americas-colocation/united-states-colocation/washington-dc-data-centers/dc6) | 1 | USA, Osten, USA, Osten 2 | 10G, 100G | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Cologix, Colt, Comcast, Coresite, Equinix, Internet2, InterCloud, Iron Mountain, IX Reach, Level 3 Communications, Megaport, Neutrona Networks, NTT Communications, Orange, PacketFabric, SES, Sprint, Tata Communications, Telia Carrier, Verizon, Zayo |
| **Washington DC2** | [CoreSite VA2](https://www.coresite.com/data-center/va2-reston-va) | 1 | USA, Osten, USA, Osten 2 | 10G, 100G | CenturyLink Cloud Connect, Coresite, Intelsat, Megaport, Viasat, Zayo | 
| **Zürich** | [Interxion ZUR2](https://www.interxion.com/Locations/zurich/) | 1 | Schweiz, Norden | 10G, 100G | Colt, Eqinix, Intercloud, Interxion, Megaport, Swisscom |

 **+** steht für "In Kürze"

### <a name="national-cloud-environments"></a>Nationale Cloudumgebungen

Nationale Azure-Clouds sind voneinander und vom globalen Azure Commercial isoliert. ExpressRoute für eine Azure-Cloud kann keine Verbindung mit den Azure-Regionen in den anderen Clouds herstellen.

### <a name="us-government-cloud"></a>US-Government Cloud
| **Location** | **Adresse** | **Lokale Azure-Regionen**| **ER Direct** | **Service Providers** |
| --- | --- | --- | --- | --- |
| **Atlanta** | [Equinix AT1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/atlanta-data-centers/at1/) | – | 10G, 100G | Equinix |
| **Chicago** | [Equinix CH1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/chicago-data-centers/ch1/) | – | 10G, 100G | AT&T NetBond, British Telecom, Equinix, Level 3 Communications, Verizon |
| **Dallas** | [Equinix DA3](https://www.equinix.com/locations/americas-colocation/united-states-colocation/dallas-data-centers/da3/) | – | 10G, 100G | Equinix, Internet2, Megaport, Verizon |
| **New York** | [Equinix NY5](https://www.equinix.com/locations/americas-colocation/united-states-colocation/new-york-data-centers/ny5/) | – | 10G, 100G | Equinix, CenturyLink Cloud Connect, Verizon |
| **Phoenix** | [CyrusOne Chandler](https://cyrusone.com/locations/arizona/phoenix-arizona-chandler/) | US Gov Arizona | 10G, 100G | AT&T NetBond, CenturyLink Cloud Connect, Megaport |
| **San Antonio** | [CyrusOne SA2](https://cyrusone.com/locations/texas/san-antonio-texas-ii/) | US Gov Texas | 10G, 100G | CenturyLink Cloud Connect, Megaport |
| **Silicon Valley** | [Equinix SV4](https://www.equinix.com/locations/americas-colocation/united-states-colocation/silicon-valley-data-centers/sv4/) | – | 10G, 100G | AT&T, Equinix, Level 3 Communications, Verizon |
| **Seattle** | [Equinix SE2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/seattle-data-centers/se2/) | – | 10G, 100G | Equinix, Megaport |
| **Washington DC** | [Equinix DC2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/washington-dc-data-centers/dc2/) | US DoD, Osten; US Gov Virginia | 10G, 100G | AT&T NetBond, CenturyLink Cloud Connect, Equinix, Level 3 Communications, Megaport, Verizon |

### <a name="china"></a>China
| **Location** | **Adresse** | **Lokale Azure-Regionen** | **ER Direct** | **Service Providers** |
| --- | --- | --- | --- | --- |
| **Peking (Beijing)** | China Telecom | – | 10G | China Telecom |
| **Peking2** | GDS | – | 10G | China Telecom, China Unicom, GDS |
| **Shanghai** | China Telecom | – | 10G | China Telecom |
| **Shanghai2** | GDS | – | 10G | China Telecom, China Unicom, GDS |

Weitere Informationen finden Sie unter [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Deutschland
| **Location** | **Service Providers** |
| --- | --- |
| **Berlin** |e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="connectivity-through-exchange-providers"></a><a name="c1partners"></a>Konnektivität über Exchange-Anbieter
Wenn Ihr Konnektivitätsanbieter nicht in den vorherigen Abschnitten enthalten ist, können Sie dennoch eine Verbindung erstellen.

* Fragen Sie Ihren Konnektivitätsanbieter, ob eine Verbindung mit einem der Exchange-Standorte aus der vorherigen Tabelle bereitgestellt werden kann. Sie können auch die folgenden Links überprüfen, um weitere Informationen über die von den Exchange-Anbietern angebotenen Dienste zu erhalten. Viele Konnektivitätsanbieter sind bereits mit Ethernet-Exchanges verbunden.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [DE-CIX](https://www.de-cix.net/en/de-cix-service-world/cloud-exchange)
  * [Equinix Cloud Exchange](https://www.equinix.com/resources/videos/cloud-exchange-overview)
  * [InterXion](https://www.interxion.com/)
  * [NextDC](https://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [PacketFabric](https://www.packetfabric.com/cloud-connectivity/microsoft-azure)
  * [Teraco](https://www.teraco.co.za/platform-teraco/africa-cloud-exchange/)
  
* Bitten Sie Ihren Konnektivitätsanbieter, Ihr Netzwerk auf den Peeringstandort Ihrer Wahl zu erweitern.
  * Stellen Sie sicher, dass Ihr Konnektivitätsanbieter Ihre Konnektivität mit hoher Verfügbarkeit erweitert, sodass es keine einzelnen Fehlerquellen mehr gibt.
* Bestellen Sie eine ExpressRoute-Verbindung mit der Exchange als Konnektivitätsanbieter für die Verbindung mit Microsoft.
  * Führen Sie die Schritte in [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md) aus, um die Verbindung einzurichten.

## <a name="connectivity-through-satellite-operators"></a>Konnektivität durch Satellitenoperatoren
Wenn Sie an einem Remotestandort nicht über Glasfaserkonnektivität verfügen oder andere Konnektivitätsoptionen untersuchen möchten, können Sie die folgenden Satellitenoperatoren überprüfen. 

* Intelsat
* [SES](https://www.ses.com/networks/signature-solutions/signature-cloud/ses-and-azure-expressroute)
* [Viasat](http://www.directcloud.viasatbusiness.com/)

## <a name="connectivity-through-additional-service-providers"></a><a name="c1partners"></a>Konnektivität über zusätzliche Dienstanbieter
| **Location** | **Exchange** | **Konnektivitätsanbieter** |
| --- | --- | --- |
| **Amsterdam** | Equinix, Interxion, Level 3 Communications | BICS, CloudXpress, Eurofiber, Fastweb S.p.A, Gulf Bridge International, Kalaam Telecom Bahrain B.S.C, MainOne, Nianet, POST Telecom Luxembourg, Proximus, RETN, TDC Erhverv, Telecom Italia Sparkle, Telekom Deutschland GmbH, Telia |
| **Atlanta** | Equinix| Crown Castle
| **Kapstadt** | Teraco | MTN |
| **Chennai** | Tata Communications | Tata Teleservices |
| **Chicago** | Equinix| Crown Castle, Spectrum Enterprise, Windstream |
| **Dallas** | Equinix, Megaport | Axtel, C3ntro Telecom, Cox Business, Crown Castle, Data Foundry, Spectrum Enterprise, Transtelco |
| **Frankfurt** | Interxion | BICS, Cinia, Equinix, Nianet, QSC AG, Telekom Deutschland GmbH |
| **Hamburg** | Equinix | Cinia |
| **Hongkong SAR** | Equinix | Chief, Macroview Telecom |
| **Johannesburg** | Teraco | MTN |
| **London** | BICS, Equinix, euNetworks| Bezeq International Ltd., CoreAzure, Epsilon Telecommunications Limited, Exponential E, HSO, NexGen Networks, Proximus, Tamares Telecom, Zain |
| **Los Angeles** | Equinix |Crown Castle, Spectrum Enterprise, Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix| Airgate Technologies, Inc. Aptum Technologies, Rogers, Zirro |
| **Mumbai** | Tata Communications | Tata Teleservices |
| **New York** |Equinix, Megaport | Altice Business, Crown Castle, Spectrum Enterprise, Webair |
| **Paris** | Equinix | Proximus |
| **Quebec City** | Megaport | Fibrenoire |
| **Sao Paulo** | Equinix | Venha Pra Nuvem |
| **Seattle** |Equinix | Alaska Communications |
| **Silicon Valley** |Coresite, Equinix | Cox Business, Spectrum Enterprise, Windstream, X2nsat Inc. |
| **Singapur** |Equinix |1CLOUDSTAR, BICS, CMC Telecom, Epsilon Telecommunications Limited, LGA Telecom, United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sydney** | Megaport | Macquarie Telecom Group|
| **Tokio** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix, Megaport | Airgate Technologies Inc., Beanfield Metroconnect, Aptum Technologies, IVedha Inc, Oncore Cloud Services Inc., Rogers, Thinktel, Zirro|
| **Washington DC** |Equinix | Altice Business, BICS, Cox Business, Crown Castle, Gtt Communications Inc, Epsilon Telecommunications Limited, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>ExpressRoute-Systemintegratoren
Je nach Ihrer Netzwerkgröße kann das Aktivieren privater Verbindungen für Ihre Anforderungen problematisch sein. Alle Systemintegratoren, die in der folgenden Tabelle aufgeführt sind, können Ihnen beim Integrieren von ExpressRoute helfen.

| **Kontinent** | **Systemintegratoren** |
| --- | --- |
| **Asien** |Avanade Inc., OneAs1a |
| **Australien** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Europa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **Nordamerika** |Avanade Inc., Equinix Professional Services, FlexManage, Lightstream, Perficient, Presidio |
| **Südamerika** |Avanade Inc., Venha Pra Nuvem |
## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).
* Stellen Sie sicher, dass alle Voraussetzungen erfüllt sind. Informationen finden Sie unter [ExpressRoute-Voraussetzungen](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Standortkarte"
