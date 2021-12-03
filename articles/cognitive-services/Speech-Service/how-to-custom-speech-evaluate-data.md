---
title: Bewerten und Verbessern der Custom Speech-Genauigkeit – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: In diesem Dokument erfahren Sie, wie Sie die Qualität unseres Spracherkennungsmodells oder Ihres benutzerdefinierten Modells quantitativ messen und verbessern können. Zum Testen der Genauigkeit sind Audio- und menschenmarkierte Transkriptionsdaten erforderlich, und 30 Minuten bis 5 Stunden an repräsentativem Audio müssen bereitgestellt werden.
services: cognitive-services
author: eric-urban
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: eur
ms.custom: ignite-fall-2021
ms.openlocfilehash: 2a0ad4a926365ffe8bed5ebda5b9848ce72c7845
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131501113"
---
# <a name="evaluate-and-improve-custom-speech-accuracy"></a>Bewerten und Verbessern der Custom Speech-Genauigkeit

In diesem Artikel erfahren Sie, wie Sie die Genauigkeit der Spracherkennungsmodelle von Microsoft oder Ihrer eigenen benutzerdefinierten Modelle quantitativ messen und verbessern können. Zum Testen der Genauigkeit sind Audio- und menschenmarkierte Transkriptionsdaten erforderlich, und 30 Minuten bis 5 Stunden an repräsentativem Audio müssen bereitgestellt werden.

## <a name="evaluate-custom-speech-accuracy"></a>Bewerten der Custom Speech-Genauigkeit

Der Branchenstandard zur Messung der Modellgenauigkeit ist die [Wort-Fehler-Rate](https://en.wikipedia.org/wiki/Word_error_rate) (WER). Zur Berechnung der WER wird die Anzahl von bei der Erkennung falsch identifizierten Wörtern ermittelt und durch die Gesamtanzahl von Wörtern im menschenmarkierten Transkript (unten als „N“ zu sehen) dividiert. Diese Zahl wird dann mit 100 % multipliziert, um die WER zu berechnen.

![WER-Formel](./media/custom-speech/custom-speech-wer-formula.png)

Falsch identifizierte Wörter fallen in drei Kategorien:

* Einfügung (I): Wörter, die fälschlicherweise im Hypothesentranskript hinzugefügt werden
* Löschung (D): Wörter, die im Hypothesentranskript nicht erkannt werden
* Ersetzung (S): Wörter, die zwischen Verweis und Hypothese ersetzt wurden

Hier sehen Sie ein Beispiel:

![Beispiel für falsch identifizierte Wörter](./media/custom-speech/custom-speech-dis-words.png)

Wenn Sie WER-Messungen lokal replizieren möchten, können Sie sclite von [SCTK](https://github.com/usnistgov/SCTK) verwenden.

## <a name="resolve-errors-and-improve-wer"></a>Beheben von Fehlern und Verbessern der WER

Sie können die WER aus den Ergebnissen der maschinellen Erkennung verwenden, um die Qualität des mit Ihrer App, Ihrem Tool oder Ihrem Produkt verwendeten Modells zu bewerten. Ein WER von 5 %–10 % gilt als gute Qualität und ist einsatzbereit. Eine WER von 20 % ist akzeptabel, Sie sollten aber weiteres Training in Betracht ziehen. Eine WER von 30 % oder mehr weist auf schlechte Qualität hin und erfordert Anpassung und Training.

Die Verteilung der Fehler spielt eine wichtige Rolle. Wenn viele Löschfehler auftreten, liegt es in der Regel an einem schwachen Audiosignal. Um dieses Problem zu beheben, müssen Sie Audiodaten näher an der Quelle erfassen. Einfügefehler bedeuten, dass der Ton in einer lauten Umgebung aufgenommen wurde und Übersprechen vorhanden sein kann, was zu Erkennungsproblemen führt. Ersetzungsfehler treten häufig auf, wenn eine unzureichende Stichprobe domänenspezifischer Begriffe entweder als menschenmarkierte Transkriptionen oder zugehöriger Text bereitgestellt wurde.

Durch die Analyse einzelner Dateien können Sie feststellen, welche Art von Fehlern vorliegen und welche Fehler nur in einer bestimmten Datei auftreten. Das Verständnis von Problemen auf Dateiebene hilft Ihnen bei gezielten Verbesserungen.

## <a name="create-a-test"></a>Erstellen eines Tests

Wenn Sie die Qualität des Basismodells für Spracherkennung von Microsoft oder eines von Ihnen trainierten benutzerdefinierten Modells testen möchten, können Sie zwei Modelle nebeneinander vergleichen, um die Genauigkeit zu bewerten. Der Vergleich umfasst WER und Erkennungsergebnisse. In der Regel wird ein benutzerdefiniertes Modell mit dem Basismodell von Microsoft verglichen.

Parallele Auswertung von Modellen:

1. Melden Sie sich beim [Custom Speech-Portal](https://speech.microsoft.com/customspeech) an.
2. Navigieren Sie zu **Spracherkennung > Custom Speech > [Projektname] > Testen**.
3. Klicken Sie auf **Test hinzufügen**.
4. Wählen Sie **Bewerten der Genauigkeit** aus. Geben Sie einen Namen und eine Beschreibung für den Test ein, und wählen Sie Ihr Audio- und menschenmarkiertes Transkriptionsdataset aus.
5. Wählen Sie bis zu zwei Modelle aus, die Sie testen möchten.
6. Klicken Sie auf **Erstellen**.

Nachdem der Test erfolgreich erstellt wurde, können Sie die Ergebnisse nebeneinander vergleichen.

### <a name="side-by-side-comparison"></a>Gegenüberstellung

Sobald der Test abgeschlossen ist, was durch den Statuswechsel zu *Erfolgreich* angezeigt wird, erhalten Sie eine WER-Nummer für beide Modelle in Ihrem Test. Klicken Sie auf den Testnamen, um die Seite mit den Testdetails anzuzeigen. Diese Detailseite listet alle Äußerungen in Ihrem Dataset auf und zeigt die Erkennungsergebnisse der beiden Modelle neben der Transkription aus dem übermittelten Dataset an. Zur einfacheren Untersuchung der Gegenüberstellung können Sie verschiedene Fehlertypen wie Einfügungen, Löschungen und Ersetzungen aktivieren oder deaktivieren. Indem Sie sich die Audiodaten anhören und mit den Erkennungsergebnissen in den einzelnen Spalten vergleichen, die die menschenmarkierte Transkription sowie die Ergebnisse für zwei Spracherkennungsmodelle enthalten, können Sie entscheiden, welches Modell Ihre Anforderungen erfüllt und wo weiteres Training und Verbesserungen erforderlich sind.

## <a name="improve-custom-speech-accuracy"></a>Verbessern der Custom Speech-Genauigkeit

Spracherkennungsszenarien unterscheiden sich in Bezug auf die Audioqualität und die Sprache (Vokabular und Sprechweise). Die folgende Tabelle enthält Informationen zu vier gängigen Szenarien:

| Szenario | Audioqualität | Vokabular | Sprechweise |
|----------|---------------|------------|----------------|
| Callcenter | Niedrig, 8 kHz, ggf. zwei Menschen über einen Audiokanal, ggf. komprimiert | Eingeschränkt, auf jeweiliges Thema und Produkte bezogen | Interaktiv, locker strukturiert |
| Sprachassistent (z. B. Cortana oder ein Drive-Through-Fenster) | Hoch, 16 kHz | Viele Entitäten (Songtitel, Produkte, Orte) | Klar gesprochene Wörter und Ausdrücke |
| Diktat (Sofortnachricht, Notizen, Suche) | Hoch, 16 kHz | Verschiedene | Festhalten von Notizen |
| Untertitelung für Videos | Variabel, z. B. vielfältige Mikrofonnutzung, hinzugefügte Musik | Variabel, aus Besprechungen, Rezitationen, Songtexten | Vorgelesen, vorbereitet oder locker strukturiert |

Unterschiedliche Szenarien führen zu Ergebnissen unterschiedlicher Qualität. Die folgende Tabelle enthält Informationen dazu, wie die Inhalte dieser vier Szenarien in Bezug auf die [Wort-Fehler-Rate (Word Error Rate, WER)](how-to-custom-speech-evaluate-data.md) abschneiden. In der Tabelle ist angegeben, welche Fehlertypen in den einzelnen Szenarien am häufigsten vorkommen.

| Szenario | Spracherkennungsqualität | Fehler beim Einfügen | Fehler beim Löschen | Fehler beim Ersetzen |
|----------|----------------------------|------------------|-----------------|---------------------|
| Callcenter | Mittel (WER < 30 %) | Niedrig, sofern keine anderen Personen im Hintergrund sprechen | Ggf. hoch. In Callcentern kann es laut sein, und für das Modell kommt es durch sich überlappende Sprecher ggf. zu Verwirrung | Mittel. Namen von Produkten und Personen können diese Fehler verursachen |
| Sprachassistent | Hoch (WER kann < 10 % sein) | Niedrig | Niedrig | Mittel, aufgrund von Songtiteln, Produktnamen oder Orten |
| Diktieren | Hoch (WER kann < 10 % sein) | Niedrig | Niedrig | High |
| Untertitelung für Videos | Je nach Videotyp (WER kann < 50 % sein) | Niedrig | Kann aufgrund von Musik, Nebengeräuschen, Mikrofonqualität hoch sein | Fachjargon kann zu diesen Fehlern führen |

Durch die Bestimmung der WER-Komponenten (Anzahl von Fehlern beim Einfügen, Löschen und Ersetzen) können Sie ermitteln, welche Art von Daten zur Verbesserung des Modells hinzugefügt werden sollten. Verwenden Sie das [Custom Speech-Portal](https://speech.microsoft.com/customspeech), um die Qualität für ein Basismodell anzuzeigen. Im Portal werden die Raten für Fehler beim Einfügen, Ersetzen und Löschen gemeldet, die in der WER-Qualitätsrate zusammengefasst sind.

## <a name="improve-model-recognition"></a>Verbessern der Modellerkennung

Sie können die Zahl von Erkennungsfehlern reduzieren, indem Sie im [Custom Speech-Portal](https://speech.microsoft.com/customspeech) Trainingsdaten hinzufügen. 

Planen Sie die Wartung Ihres benutzerdefinierten Modells durch das regelmäßige Hinzufügen von Quellmaterial. Sie müssen für Ihr benutzerdefiniertes Modell zusätzliche Trainingsläufe durchführen, damit Sie über Änderungen in Bezug auf Ihre Entitäten informiert sind. Es kann beispielsweise sein, dass Aktualisierungen von Produktnamen, Songtiteln oder neuen Dienstidentifizierungen erforderlich sind.

In den folgenden Abschnitten wird beschrieben, wie mit den einzelnen Arten von zusätzlichen Trainingsdaten Fehler reduziert werden können.

### <a name="add-plain-text-data"></a>Hinzufügen von Nur-Text-Daten

Beginnen Sie beim Training eines neuen benutzerdefinierten Modells, indem Sie verwandten Text hinzufügen, um die Erkennung von themenspezifischen Wörtern und Ausdrücken zu verbessern. Mit passenden Textsätzen können hauptsächlich Ersetzungsfehler reduziert werden, die sich auf die fehlerhafte Erkennung von gängigen und themenspezifischen Wörtern beziehen, indem diese im Kontext angezeigt werden. Bei themenspezifischen Wörtern kann es sich auch um ungewöhnliche oder ausgedachte Wörter handeln, aber ihre Aussprache muss eindeutig sein, damit sie erkannt werden können.

> [!NOTE]
> Vermeiden Sie Textsätze, die „Rauschen“ enthalten, z. B. nicht zu erkennende Buchstaben oder Wörter.

### <a name="add-structured-text-data"></a>Hinzufügen strukturierter Textdaten

Sie können strukturierte Textdaten im Markdownformat ähnlich wie Nur-Text-Sätze verwenden, aber Sie würden strukturierte Textdaten verwenden, wenn Ihre Daten einem bestimmten Muster in bestimmten Äußerungen folgen, die sich nur durch Wörter oder Ausdrücke aus einer Liste unterscheiden. Weitere Informationen finden Sie unter [Strukturierte Textdaten für das Training](how-to-custom-speech-test-and-train.md#structured-text-data-for-training-public-preview). 

> [!NOTE]
> Das Training mit strukturiertem Text wird nur für diese Sprachumgebungen unterstützt: `en-US`, `de-DE`, `en-UK`, `en-IN`, `fr-FR`, `fr-CA`, `es-ES`, `es-MX` und Sie müssen das neueste Basismodell für diese Gebietsschemata verwenden. Eine Liste mit Basismodellen, die das Training mit Audiodaten unterstützen, finden Sie unter [Sprachunterstützung](language-support.md).
> 
> Für Gebietsschemas, die das Training mit strukturiertem Text nicht unterstützen, nimmt der Dienst alle Trainingssätze entgegen, die im Rahmen des Trainings mit Nur-Text-Daten auf keine Klassen verweisen.

### <a name="add-audio-with-human-labeled-transcripts"></a>Hinzufügen von Audiodaten mit menschenmarkierten Transkriptionen

Audiodaten mit menschenmarkierten Transkripts ermöglichen die größten Verbesserungen bei der Genauigkeit, sofern die Audiodaten aus dem Zielanwendungsfall stammen. Beispiele müssen den vollständigen Sprachbereich abdecken. Ein Callcenter für ein Einzelhandelsgeschäft erhält die meisten Anrufe zu Badebekleidung und Sonnenbrillen in den Sommermonaten. Stellen Sie sicher, dass Ihr Beispiel den vollständigen Sprachbereich abdeckt, der erkannt werden soll.

Berücksichtigen Sie Folgendes:

* Das Training mit Audiodaten bietet den größten Nutzen, wenn das Audiomaterial auch für Menschen schwer zu verstehen ist. In den meisten Fällen sollten Sie mit dem Training beginnen, indem Sie nur verwandten Text verwenden.
* Bei Verwendung einer weit verbreiteten Sprache wie „Englisch (USA)“ ist unter Umständen gar kein Training mit Audiodaten erforderlich. Bei diesen Sprachen liefern die Basismodelle in den meisten Szenarien bereits sehr gute Erkennungsergebnisse, und es reicht wahrscheinlich, sie mit verwandtem Text zu trainieren.
* Custom Speech kann den Wortkontext nur erfassen, um Ersetzungsfehler zu reduzieren (keine Einfüge- oder Löschfehler).
* Vermeiden Sie Beispiele, die Transkriptionsfehler enthalten, aber verwenden Sie unterschiedliche Audioqualitäten.
* Vermeiden Sie Sätze, die sich nicht auf Ihre Problembestellung beziehen. Sätze dieser Art können das Modell beeinträchtigen.
* Wenn die Qualität von Transkripts variiert, können Sie besonders gute Sätze (z. B. hervorragende Transkriptionen mit wichtigen Ausdrücken) duplizieren, um ihre Gewichtung zu erhöhen.
* Der Speech-Dienst verwendet die Transkriptionen automatisch, um die Erkennung von themenspezifischen Wörtern und Ausdrücken zu verbessern, als wären sie als verwandter Text hinzugefügt worden.
* Der Abschluss eines Trainingsvorgangs kann mehrere Tage in Anspruch nehmen. Stellen Sie zum Verbessern der Trainingsgeschwindigkeit sicher, dass Sie Ihr Abonnement für den Speech-Dienst in einer [Region mit dedizierter Hardware](custom-speech-overview.md#set-up-your-azure-account) für Training erstellen.

> [!NOTE]
> Nicht alle Basismodelle unterstützen das Training mit Audiodaten. Wenn es von einem Basismodell nicht unterstützt wird, verwendet der Speech-Dienst nur den Text aus den Transkriptionen und ignoriert die Audiodaten. Eine Liste mit Basismodellen, die das Training mit Audiodaten unterstützen, finden Sie unter [Sprachunterstützung](language-support.md#speech-to-text). Selbst wenn ein Basismodell das Training mit Audiodaten unterstützt, verwendet der Dienst möglicherweise nur einen Teil der Audiodaten. Dennoch werden alle Transkriptionen verwendet.

> [!NOTE]
> Wenn Sie das für das Training verwendete Basismodell ändern und das Trainingsdataset Audiodaten enthält, überprüfen Sie *immer*, ob das neue ausgewählte Basismodell [das Training mit Audiodaten unterstützt](language-support.md#speech-to-text). Wenn das zuvor verwendete Basismodell kein Training mit Audiodaten unterstützt hat und das Trainingsdataset Audiodaten enthält, verlängert sich die Trainingsdauer mit dem neuen Basismodell **deutlich** und kann schnell einige Stunden oder sogar mehrere Tage und mehr betragen. Dies ist insbesondere dann der Fall, wenn es sich bei der Region Ihres Abonnements für den Speech-Dienst **nicht** um eine [Region mit dedizierter Hardware](custom-speech-overview.md#set-up-your-azure-account) für Training handelt.
>
> Wenn das im obigen Abschnitt beschriebene Problem auftritt, können Sie die Trainingsdauer schnell verkürzen, indem Sie die Menge der Audiodaten im Dataset verringern oder die Audiodaten ganz daraus entfernen, sodass das Dataset nur noch Text enthält. Letzteres ist sehr empfehlenswert, wenn es sich bei der Region Ihres Abonnements für den Speech-Dienst **nicht** um eine [Region mit dedizierter Hardware](custom-speech-overview.md#set-up-your-azure-account) für Training handelt.

### <a name="add-new-words-with-pronunciation"></a>Hinzufügen von neuen Wörtern mit besonderer Aussprache

Wörter, die ausgedacht oder sehr speziell sind, verfügen unter Umständen über eine besondere Aussprache. Ein Wort dieser Art kann erkannt werden, wenn es zum Aussprechen in kleinere Wörter unterteilt werden kann. Sprechen Sie **Xbox** beispielsweise deutlich als **X box** aus, damit das Wort erkannt wird. Diese Vorgehensweise führt nicht zu einer Erhöhung der Gesamtgenauigkeit, aber die Erkennung dieser Schlüsselwörter kann verbessert werden.

> [!NOTE]
> Diese Methode ist derzeit nur für einige Sprachen verfügbar. Informationen zur Anpassung für die Aussprache finden Sie in der [Tabelle zur Spracherkennung](language-support.md).

## <a name="sources-by-scenario"></a>Quellen nach Szenario

Die folgende Tabelle enthält Szenarien zur Spracherkennung und Quellmaterial für die drei obigen Kategorien für Trainingsinhalte.

| Szenario | Hinzufügen von Nur-Text-Daten und <br> strukturierter Textdaten | Audio + menschenmarkierte Transkripte | Neue Wörter mit besonderer Aussprache |
|----------|------------------------|------------------------------|------------------------------|
| Callcenter             | Marketingdokumente, Website, Produktbewertungen in Bezug auf Callcenteraktivitäten | Von Menschen transkribierte Callcenteranrufe | Begriffe mit mehrdeutiger Aussprache (siehe „Xbox“ oben) |
| Sprachassistent         | Auflisten von Sätzen mit allen Kombinationen von Befehlen und Entitäten | Aufzeichnen von Stimmen, die Befehle in das Gerät sprechen, und Transkribieren in Text | Namen (Filme, Songs, Produkte) mit besonderer Aussprache |
| Diktieren               | Eingaben in Schriftform, z. B. Sofortnachrichten oder E-Mails | Ähnlich wie oben | Ähnlich wie oben |
| Untertitelung für Videos | Skripts von TV-Sendungen, Filme, Marketinginhalte, Videozusammenfassungen | Genaue Transkripts von Videos | Ähnlich wie oben |

## <a name="next-steps"></a>Nächste Schritte

* [Trainieren und Bereitstellen eines Modells](how-to-custom-speech-train-model.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Vorbereiten und Testen Ihrer Daten](./how-to-custom-speech-test-and-train.md)
* [Überprüfen Ihrer Daten](how-to-custom-speech-inspect-data.md)
