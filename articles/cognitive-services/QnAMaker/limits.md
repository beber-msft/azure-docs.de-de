---
title: 'Einschränkungen und Begrenzungen: QnA Maker'
description: QnA Maker weist Metagrenzwerte für Teile der Wissensdatenbank und des Diensts auf. Es ist wichtig, Ihre Wissensdatenbank innerhalb dieser Grenzwerte zu halten, um sie testen und veröffentlichen zu können.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 11/02/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: ac7741a6404f47e3cabdd9eff81f81c6ad39de36
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131011977"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>Grenzwerte und Grenzen für QnA Maker-Wissensdatenbanken

Bei den folgenden QnA Maker-Grenzwerten handelt es sich um eine Kombination aus den [Grenzwerten für den Azure Cognitive Search-Dienst](../../search/search-limits-quotas-capacity.md) und den [Grenzwerten für den QnA Maker-Tarif](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/). Sie müssen mit beiden Grenzwerten vertraut sein, um zu verstehen, wie viele Wissensdatenbanken Sie pro Ressource erstellen können und wie groß die Wissensdatenbanken jeweils werden können.

## <a name="knowledge-bases"></a>Wissensdatenbanken

Die maximale Anzahl von Wissensdatenbanken basiert auf den [Grenzwerten für den Azure Cognitive Search-Dienst](../../search/search-limits-quotas-capacity.md).

|**Azure Cognitive Search-Tarif** | **Free** | **Grundlegend** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maximal zulässige Anzahl der veröffentlichten Wissensdatenbanken|2|14|49|199|199|2\.999|

 Wenn Ihr Tarif beispielsweise 15 zulässige Indizes aufweist, können Sie 14 Wissensdatenbanken veröffentlichen (1 Index pro veröffentlichter Wissensdatenbank). Der 15. Index (`testkb`) wird für alle Wissensdatenbanken zum Erstellen und Testen verwendet.

## <a name="extraction-limits"></a>Grenzwerte für die Extraktion

### <a name="file-naming-constraints"></a>Benennungskonventionen für Dateien

Dateinamen dürfen keines der folgenden Zeichen enthalten:

|Verwenden Sie nicht das Zeichen|
|--|
|Einfache Anführungszeichen `'`|
|Doppelte Anführungszeichen `"`|

### <a name="maximum-file-size"></a>Maximale Dateigröße

|Format|Max. Dateigröße (MB)|
|--|--|
|`.docx`|10|
|`.pdf`|25|
|`.tsv`|10|
|`.txt`|10|
|`.xlsx`|3|

### <a name="maximum-number-of-files"></a>Maximale Anzahl von Dateien

Die maximale Anzahl von Dateien, die extrahiert werden können, und die maximale Dateigröße basieren auf den **[Grenzwerten für Ihren QnA Maker-Tarif](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)** .

### <a name="maximum-number-of-deep-links-from-url"></a>Maximale Anzahl von Deep-Links für die URL

Die maximale Anzahl von Deep-Links, die zum Extrahieren von Fragen und Antworten (QnA) aus einer URL-Seite durchforstet werden können, beträgt **20**.

## <a name="metadata-limits"></a>Grenzwerte für Metadaten

Metadaten werden als textbasiertes Schlüssel-Wert-Paar dargestellt, z. B. `product:windows 10`. Die Metadaten werden in Kleinbuchstaben gespeichert und verglichen. Die maximale Anzahl von Metadatenfeldern basiert auf den **[Grenzwerten für den Azure Cognitive Search-Dienst](../../search/search-limits-quotas-capacity.md)** .

Da der Testindex für alle KBs freigegeben ist, wird der Grenzwert für die GA-Version (allgemeine Verfügbarkeit) auf alle Wissensdatenbanken im QnA Maker angewendet.

|**Azure Cognitive Search-Tarif** | **Free** | **Grundlegend** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maximale Anzahl der Metadatenfelder pro QnA Maker-Dienst (für alle Knowledge Bases)|1\.000|100*|1\.000|1\.000|1\.000|1\.000|

### <a name="by-name-and-value"></a>Nach Name und Wert

Die Länge und die zulässigen Zeichen für den Metadatennamen und -wert sind in der folgenden Tabelle aufgeführt:

|Element|Zulässige Zeichen|RegEx-Musterabgleich|Maximale Anzahl von Zeichen|
|--|--|--|--|
|Name (Schlüssel)|Zulässig:<br>Alphanumerische Zeichen (Buchstaben und Ziffern)<br>`_` (Unterstrich)<br> Darf keine Leerzeichen enthalten.|`^[a-zA-Z0-9_]+$`|100|
|Wert|Alles zulässig mit Ausnahme von:<br>`:` (Doppelpunkt)<br>`|` (senkrechter Strich)<br>Nur ein Wert ist zulässig.|`^[^:|]+$`|500|
|||||

## <a name="knowledge-base-content-limits"></a>Grenzwerte für die Inhalte einer Knowledge Base
Allgemeine Grenzwerte für die Inhalte in der Knowledge Base:
* Länge des Antworttexts: 25.000 Zeichen
* Länge des Fragentexts: 1.000 Zeichen
* Länge des Texts für den Metadatenschlüssel: 100 Zeichen
* Länge des Texts für den Metadatenwert: 500 Zeichen
* Unterstützte Zeichen für den Metadatennamen: Buchstaben, Ziffern und `_`
* Unterstützte Zeichen für den Metadatenwerte: Alle Zeichen außer `:` und `|`
* Länge des Dateinamens: 200
* Unterstützte Dateiformate: „.tsv“, „.pdf“, „.txt“, „.docx“, „.xlsx“.
* Maximale Anzahl von alternativen Fragen: 300
* Maximale Anzahl von Frage-Antwort-Paaren: Abhängig vom ausgewählten **[Azure Cognitive Search-Tarif](../../search/search-limits-quotas-capacity.md#document-limits)** . Ein Frage-Antwort-Paar wird einem Dokument im Azure Cognitive Search-Index zugeordnet.
* URL/HTML-Seite: Eine Million Zeichen

## <a name="create-knowledge-base-call-limits"></a>Grenzwerte für Aufrufe zum Erstellen einer Knowledge Base
Dabei handelt es sich um die Grenzwerte für die einzelnen Aktionen zum Erstellen einer Knowledge Base, d.h. Klicken auf *Create KB* (Knowledge Base erstellen) oder Aufrufen der CreateKnowledgeBase-API.
* Empfohlene maximale Anzahl von alternativen Fragen pro Antwort: 300
* Maximale Anzahl von URLs: 10
* Maximale Anzahl von Dateien: 10
* Maximale Anzahl zulässiger QnAs pro Aufruf: 1000

## <a name="update-knowledge-base-call-limits"></a>Grenzwerte für Aufrufe zum Aktualisieren einer Knowledge Base
Dabei handelt es sich um die Grenzwerte für die einzelnen Aktualisierungsaktionen, d.h. Klicken auf *Save and train* (Speichern und trainieren) oder Aufrufen der UpdateKnowledgeBase-API.
* Länge jedes Quellnamens: 300
* Empfohlene maximale Anzahl hinzugefügter oder gelöschter alternativer Fragen: 300
* Maximale Anzahl hinzugefügter oder gelöschter Metadatenfelder: 10
* Maximale Anzahl der URLs, die aktualisiert werden können: 5
* Maximale Anzahl zulässiger QnAs pro Aufruf: 1000

## <a name="add-unstructured-file-limits"></a>Hinzufügen von Grenzwerten für unstrukturierte Dateien

> [!NOTE]
> * Wenn Sie größere Dateien verwenden müssen, als der Grenzwert zulässt, können Sie die Datei in kleinere Dateien aufteilen, bevor Sie sie an die API senden. 

Diese stellen die Grenzwerte dar, wenn unstrukturierte Dateien zum *Erstellen einer Wissensdatenbank* oder zum Aufrufen der CreateKnowledgeBase-API verwendet werden:
* Länge der Datei: Wir extrahieren die ersten 32.000 Zeichen.
* Maximal drei Antworten pro Datei

## <a name="prebuilt-question-answering-limits"></a>Vordefinierte Grenzwerte für die Beantwortung von Fragen

> [!NOTE]
> * Wenn Sie größere Dokumente verwenden müssen, als der Grenzwert zulässt, können Sie den Text in kleinere Textabschnitte aufteilen, bevor Sie ihn an die API senden. 
> * Ein Dokument ist eine einzelne aus Textzeichen bestehende Zeichenfolge.  

Diese stellen die Grenzwerte dar, wenn die vordefinierte API verwendet wird, um *eine Antwort zu generieren* oder die GenerateAnswer-API aufzurufen:
* Anzahl der Dokumente: 5
* Maximale Größe eines einzelnen Dokuments: 5.120 Zeichen
* Maximal drei Antworten pro Dokument

> [!IMPORTANT]
> Unterstützung für unstrukturierte Dateien/Inhalte, nur in „Fragen und Antworten“ verfügbar

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wann und wie [Diensttarife](How-To/set-up-qnamaker-service-azure.md#upgrade-qna-maker-sku) geändert werden:
