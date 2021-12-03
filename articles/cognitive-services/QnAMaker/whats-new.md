---
title: Neuerungen im QnA Maker-Dienst
titleSuffix: Azure Cognitive Services
description: Dieser Artikel enthält Neuigkeiten zu QnA Maker.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: overview
ms.date: 07/16/2020
ms.custom: ignite-fall-2021
ms.openlocfilehash: 959a6d5c5ed4b606c5a5850264422b6e460eb8f5
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131020443"
---
# <a name="whats-new-in-qna-maker"></a>Neuerungen in QnA Maker

Informieren Sie sich über die Neuerungen im Dienst. Dabei kann es sich um Versionshinweise, Videos, Blogbeiträge und andere Informationen handeln. Legen Sie ein Lesezeichen für diese Seite an, um über den Dienst auf dem Laufenden zu bleiben.

[!INCLUDE [Custom question answering](./includes/new-version.md)]

## <a name="release-notes"></a>Versionshinweise

Informieren Sie sich über Neuigkeiten zu QnA Maker.

### <a name="may-2021"></a>Mai 2021

* „Mit QnA Maker verwaltet“ wurde in der [Textanalyse-Ressource](https://ms.portal.azure.com/?quickstart=true#create/Microsoft.CognitiveServicesTextAnalytics) als „Benutzerdefinierte Fragen und Antworten“ wieder eingeführt.
* „Benutzerdefinierte Fragen und Antworten“ unterstützt unstrukturierte Dokumente.
* Die [vordefinierte API](how-to/using-prebuilt-api.md) wurde eingeführt, um Antworten für Benutzerabfragen aus Dokumenttext zu generieren, der über die API übergeben wird.

### <a name="november-2020"></a>November 2020

* Es wurde eine neue Version von QnA Maker in der kostenlosen Public Preview gestartet. Weitere Informationen finden Sie [hier](https://techcommunity.microsoft.com/t5/azure-ai/introducing-qna-maker-managed-now-in-public-preview/ba-p/1845575).

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Introducing-QnA-managed-Now-in-Public-Preview/player]
* Vereinfachte Ressourcenerstellung
* End-to-End-Unterstützung für Regionen
* Bewertungsmodell mit Deep Learning
* Machine Reading Comprehension für präzise Antworten
  
### <a name="july-2020"></a>Juli 2020

* [Metadaten: `OR` logische Kombination mehrerer Metadatenpaare](how-to/query-knowledge-base-with-metadata.md#logical-or-using-strictfilterscompoundoperationtype-property)
* [Schritte](how-to/network-isolation.md) zum Konfigurieren der Cognitive Search-Endpunkte, dass sie privat, aber dennoch für QnA Maker zugänglich sind.
* Kostenlose Cognitive Search-Ressourcen werden nach [90 Tagen Inaktivität](how-to/set-up-qnamaker-service-azure.md#inactivity-policy-for-free-search-resources) entfernt.

### <a name="june-2020"></a>Juni 2020

* Die Schritte unter [Tutorial: Hinzufügen Ihrer Wissensdatenbank zu Power Virtual Agents](tutorials/integrate-with-power-virtual-assistant-fallback-topic.md) wurden verkürzt und vereinfacht.

### <a name="may-2020"></a>Mai 2020

* [Rollenbasierte Zugriffssteuerung von Azure (Azure RBAC)](concepts/role-based-access-control.md)
* [Rich-Text-Bearbeitung](how-to/edit-knowledge-base.md#rich-text-editing-for-answer) für Antworten

### <a name="march-2020"></a>März 2020

* TLS 1.2 wird nun für alle HTTP-Anforderungen erzwungen, die an diesen Dienst gerichtet werden. Weitere Informationen finden Sie unter [Sicherheit von Azure Cognitive Services](../cognitive-services-security.md).

### <a name="february-2020"></a>Februar 2020

* [NPM-Paket](https://www.npmjs.com/package/@azure/cognitiveservices-qnamaker) mit GenerateAnswer-API

### <a name="november-2019"></a>November 2019

* [US Government-Cloudunterstützung](../../azure-government/compare-azure-government-global-azure.md#guidance-for-developers) für QnA Maker
* [Mehrfachdurchlauf](./how-to/multiturn-conversation.md) allgemein verfügbar
* [Geplauderunterstützung](./how-to/chit-chat-knowledge-base.md#language-support) in Sprachen der Ebene 1 verfügbar

### <a name="october-2019"></a>Oktober 2019

* Explizites Festlegen der Sprache für alle Wissensdatenbanken im QnA Maker-Dienst

### <a name="september-2019"></a>September 2019

* Importieren und Exportieren mit dem XLS-Dateiformat

### <a name="june-2019"></a>Juni 2019

* Verbessertes [Rangfolgemodell](concepts/query-knowledge-base.md#ranker-process) für Französisch, Italienisch, Deutsch, Spanisch und Portugiesisch

### <a name="april-2019"></a>April 2019

* Extraktion des Inhalts von Supportwebsites
* Unterstützung von [SharePoint-Dokumenten](how-to/add-sharepoint-datasources.md) über authentifizierten Zugriff

### <a name="march-2019"></a>März 2019

* [Aktives Lernen](how-to/improve-knowledge-base.md) stellt Vorschläge für Alternativen für neue Fragen basierend auf echten Benutzerfragen bereit.
* Verbessertes [Rangfolgemodell](concepts/query-knowledge-base.md#ranker-process) für die Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) für Englisch

> [!div class="nextstepaction"]
> [Create a QnA Maker service](how-to/set-up-qnamaker-service-azure.md) (Erstellen eines QnA Maker-Diensts)

## <a name="cognitive-service-updates"></a>Cognitive Services-Updates

[Azure-Updateankündigungen für Cognitive Services](https://azure.microsoft.com/updates/?product=cognitive-services)
