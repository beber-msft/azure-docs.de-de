---
title: 'Bearbeiten einer Wissensdatenbank: QnA Maker'
description: Mit QnA Maker können Sie die Inhalte Ihrer Wissensdatenbank auf einer benutzerfreundlichen Bearbeitungsoberfläche verwalten.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 07/16/2020
ms.custom: ignite-fall-2021
ms.openlocfilehash: 1b67744b66eaf5563505f72b6b151f40ed44f602
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131069233"
---
# <a name="edit-qna-pairs-in-your-knowledge-base"></a>Bearbeiten von Frage-Antwort-Paaren in Ihrer Wissensdatenbank

Mit QnA Maker können Sie die Inhalte Ihrer Wissensdatenbank auf einer benutzerfreundlichen Bearbeitungsoberfläche verwalten.

Frage-Antwort-Paare werden aus einer Datenquelle (beispielsweise einer Datei oder URL) oder als redaktionelle Quelle hinzugefügt. Eine redaktionelle Quelle deutet darauf hin, dass das Frage-Antwort-Paar manuell im QnA-Portal hinzugefügt wurde. Alle Frage-Antwort-Paare können bearbeitet werden.

[!INCLUDE [Custom question answering](../includes/new-version.md)]

<a name="add-an-editorial-qna-set"></a>

## <a name="question-and-answer-pairs"></a>Frage-Antwort-Paare

Eine Wissensdatenbank besteht aus Frage-Antwort-Paaren (Question and Answer – QnA).  Jedes Paar hat eine Antwort und enthält alle Informationen, die mit dieser _Antwort_ verknüpft sind. Eine Antwort ähnelt einer Datenbankzeile oder einer Datenstrukturinstanz. Die **erforderlichen** Einstellungen in einem Frage-Antwort-Paar (QnA) lauten wie folgt:

* Eine **Frage**: Text der Benutzerabfrage für das maschinelle Lernen von QnA Maker, mit dem der Text der Benutzerfrage mit unterschiedlichen Formulierungen, aber derselben Antwort abgeglichen wird.
* Die **Antwort**: wird zurückgegeben, wenn eine Benutzerabfrage mit der zugeordneten Frage übereinstimmt.

Jedes Paar wird durch eine **ID** dargestellt.

Die **optionalen** Einstellungen für ein Paar umfassen Folgendes:

* **Alternative Formen der Frage:** helfen QnA Maker, die richtige Antwort für eine größere Vielfalt von Frageformulierungen zurückzugeben.
* **Metadaten**: Metadaten sind Tags in Form von Schlüssel-Wert-Paaren, die einem QnA-Paar zugeordnet sind. Metadatentags werden verwendet, um QnA-Paare zu filtern und die Datenmenge einzuschränken, für die ein Abfrageabgleich vorgenommen wird.
* **Mehrfachdurchlauf-Eingabeaufforderungen:** werden verwendet, um eine mehrteilige Unterhaltung fortzusetzen.

![QnA Maker-Wissensdatenbanken](../media/qnamaker-concepts-knowledgebase/knowledgebase.png)

## <a name="add-an-editorial-qna-pair"></a>Hinzufügen eines redaktionellen Frage-Antwort-Paars

Wenn Sie nicht über bereits vorhandene Inhalte zum Füllen der Wissensdatenbank verfügen, können Sie auch redaktionell im QnA Maker-Portal QnA-Paare hinzufügen.

1. Melden Sie sich beim [QnA-Portal](https://www.qnamaker.ai/) an, und wählen Sie die Wissensdatenbank aus, der Sie das Frage-Antwort-Paar hinzufügen möchten.
1. Wählen Sie auf der Seite **BEARBEITEN** der Wissensdatenbank die Option **Add QnA pair** (Frage-Antwort-Paar hinzufügen) aus, um ein neues Frage-Antwort-Paar hinzuzufügen.

    > [!div class="mx-imgBorder"]
    > ![Hinzufügen eines Fragen-und-Antworten-Paars](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

1. Fügen Sie in der Zeile des neuen Frage-Antwort-Paars die erforderlichen Frage- und Antwortfelder hinzu. Die anderen Felder sind optional. Alle Felder können jederzeit geändert werden.

1. Fügen Sie optional **[alternative Formulierungen](../Quickstarts/add-question-metadata-portal.md#add-additional-alternatively-phrased-questions)** hinzu. Eine alternative Formulierung ist eine beliebige Form der Frage, die sich deutlich von der ursprünglichen Frage unterscheidet, aber die gleiche Antwort liefern sollte.

    Wenn Ihre Wissensdatenbank veröffentlicht wurde und Sie [aktives Lernen](use-active-learning.md) aktiviert haben, sammelt QnA Maker alternative Formulierungen, die Sie akzeptieren können. Diese Optionen werden ausgewählt, um die Vorhersagegenauigkeit zu erhöhen.

1. Fügen Sie optional **[Metadaten](../Quickstarts/add-question-metadata-portal.md#add-metadata-to-filter-the-answers)** hinzu. Wählen Sie im Kontextmenü die Option **Ansichtsoptionen** aus, um Metadaten anzuzeigen. Metadaten bieten Filter für die Antworten, die von der Clientanwendung (beispielsweise von einem Chatbot) bereitgestellt werden.

1. Fügen Sie optional **[Folgeaufforderungen](multiturn-conversation.md)** hinzu. Folgeaufforderungen bieten zusätzliche Konversationspfade, die die Clientanwendung dem Benutzer präsentieren kann.

1. Wählen Sie **Speichern und trainieren** aus, um Vorhersagen mit dem neuen Frage-Antwort-Paar zu erhalten.

## <a name="rich-text-editing-for-answer"></a>Rich-Text-Bearbeitung für Antworten

Mit der Rich-Text-Bearbeitung Ihres Antworttexts können Sie die Markdownformatierung über eine einfache Symbolleiste verwenden.

1. Wählen Sie den Textbereich für eine Antwort aus. Die Symbolleiste für den Rich-Text-Editor wird in der Zeile des Frage-Antwort-Paars angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Rich-Text-Editor mit der Frage und der Antwort einer Frage-Antwort-Paarzeile](../media/qnamaker-how-to-edit-kb/rich-text-control-qna-pair-row.png)

    Jeder Text, der sich bereits in der Antwort befindet, wird Benutzern ordnungsgemäß über einen Bot angezeigt.

1. Bearbeiten Sie den Text. Wählen Sie Formatierungsfunktionen aus der Symbolleiste für die Rich-Text-Formatierung aus, oder verwenden Sie das Umschaltfeature, um zur Markdownsyntax zu wechseln.

    > [!div class="mx-imgBorder"]
    > ![Verwenden des Rich-Text-Editors zum Schreiben, Formatieren und Speichern von Text als Markdown](../media/qnamaker-how-to-edit-kb/rich-text-display-image.png)

    |Features des Rich-Text-Editors|Tastenkombinationen|
    |--|--|
    |Wechseln zwischen dem Rich-Text-Editor und Markdown `</>`|STRG+M|
    |Fettdruck **B**|STRG+B|
    |Kursiv (angegeben durch ein kursives **_I_**)|STRG+I|
    |Unsortierte Liste||
    |Sortierte Liste||
    |Absatzformatierung||
    |Bild – Hinzufügen eines Bilds über eine öffentliche URL|STRG+G|
    |Hinzufügen eines Links zu einer öffentlich verfügbaren URL|STRG+K|
    |Emoticon – Hinzufügen einer Auswahl von Emoticons|STRG+E|
    |Menü „Erweitert“ – Rückgängig|STRG+Z|
    |Menü „Erweitert“ – Wiederholen|STRG+Y|

1. Fügen Sie mit dem Bildsymbol auf der Rich-Text-Symbolleiste ein Bild hinzu. Der Editor benötigt die öffentlich zugängliche Bild-URL und einen Alternativtext für das Bild.


    > [!div class="mx-imgBorder"]
    > ![Screenshot: Direkter Editor mit der öffentlich zugänglichen Bild-URL und dem alternativen Text für das eingegebene Bild](../media/qnamaker-how-to-edit-kb/add-image-url-alternate-text.png)

1. Fügen Sie einen Link zu einer URL hinzu, indem Sie entweder den Text in der Antwort auswählen und dann auf das Linksymbol in der Symbolleiste klicken oder indem Sie in der Symbolleiste auf das Linksymbol klicken und dann den neuen Text und die URL eingeben.

    > [!div class="mx-imgBorder"]
    > ![Verwenden des Rich-Text-Editors zum Hinzufügen eines öffentlich zugänglichen Bilds und dessen Alternativtexts](../media/qnamaker-how-to-edit-kb/add-link-to-answer-rich-text-editor.png)

## <a name="edit-a-qna-pair"></a>Bearbeiten eines Frage-Antwort-Paars

Jedes Feld eines Frage-Antwort-Paars kann bearbeitet werden – unabhängig von der ursprünglichen Datenquelle. Aufgrund der aktuellen **Ansichtsoptionen** (auf der Kontextsymbolleiste) werden unter Umständen nicht alle Felder angezeigt.

## <a name="delete-a-qna-pair"></a>Löschen eines Fragen-und-Antworten-Paars

Zum Löschen eines Fragen-und-Antworten-Paars klicken Sie in der Fragen-und-Antworten-Zeile ganz rechts auf das Symbol **Löschen**. Das Paar wird endgültig gelöscht. Dieser Vorgang kann nicht rückgängig gemacht werden. Es ist ratsam, Ihre Wissensdatenbank auf der Seite **Veröffentlichen** zu exportieren, bevor Sie Paare löschen.

![Löschen eines Fragen-und-Antworten-Paars](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="find-the-qna-pair-id"></a>Ermitteln der ID eines Frage-Antwort-Paars

Die ID eines Frage-Antwort-Paars kann auf zwei Arten ermittelt werden:

* Zeigen Sie in der Zeile des gewünschten Frage-Antwort-Paars auf das Löschsymbol. Der daraufhin eingeblendete Text enthält die ID des Frage-Antwort-Paars.
* Exportieren Sie die Wissensdatenbank. Die ID des Frage-Antwort-Paars ist im jeweiligen Frage-Antwort-Paar in der Wissensdatenbank enthalten.

## <a name="add-alternate-questions"></a>Hinzufügen alternativer Fragen

Fügen Sie alternative Fragen zu einem vorhandenen Fragen-und-Antworten-Paar hinzu, um die Wahrscheinlichkeit einer Übereinstimmung mit einer Benutzerabfrage zu erhöhen.

![Hinzufügen alternativer Fragen](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="linking-qna-pairs"></a>Verknüpfen von Frage-Antwort-Paaren

Zur Verknüpfung von Frage-Antwort-Paaren werden [Folgeaufforderungen](multiturn-conversation.md) verwendet. Hierbei handelt es sich um eine logische Verbindung zwischen Frage-Antwort-Paaren, die auf der Ebene der Wissensdatenbank verwaltet wird. Folgeaufforderungen können im QnA Maker-Portal bearbeitet werden.

Frage-Antwort-Paare können nicht in den Metadaten der Antwort verknüpft werden.

## <a name="add-metadata"></a>Hinzufügen von Metadaten

Fügen Sie Metadatenpaare hinzu, indem Sie zuerst **Ansichtsoptionen** und dann **Metadaten anzeigen** auswählen. Dadurch wird die Metadatenspalte angezeigt. Wählen Sie anschließend das Zeichen **+** aus, um ein Metadatenpaar hinzuzufügen. Dieses Paar besteht aus einem Schlüssel und einen Wert.

Weitere Informationen zu Metadaten finden Sie im Schnellstart zum QnA Maker-Portal über Metadaten:
* [Dokumenterstellung: Hinzufügen von Metadaten zu Frage-Antwort-Paaren](../quickstarts/add-question-metadata-portal.md#add-metadata-to-filter-the-answers)
* [Abfragevorhersage: Filtern von Antworten nach Metadaten](../quickstarts/get-answer-from-knowledge-base-using-url-tool.md)

## <a name="save-changes-to-the-qna-pairs"></a>Speichern von Änderungen an Frage-Antwort-Paaren

Wählen Sie bei der Bearbeitung regelmäßig **Save and train** (Speichern und trainieren) aus, damit keine Änderungen verloren gehen.

![Hinzufügen von Metadaten](../media/qnamaker-how-to-edit-kb/add-metadata.png)

## <a name="when-to-use-rich-text-editing-versus-markdown"></a>Vergleich der Anwendungsfälle für Rich-Text-Bearbeitung und Markdown

Mit der [Rich-Text-Bearbeitung](#add-an-editorial-qna-set) von Antworten können Sie als Autor die Formatierungssymbolleiste verwenden, um Text schnell auszuwählen und zu formatieren.

[Markdown](../reference-markdown-format.md) eignet sich besser, wenn Sie Inhalte automatisch generieren müssen, um Wissensdatenbanken zu erstellen, die im Rahmen einer CI/CD-Pipeline oder für [Batchtests](../index.yml) importiert werden.

## <a name="editing-your-knowledge-base-locally"></a>Lokales Bearbeiten Ihrer Wissensdatenbank

Nachdem eine Wissensdatenbank erstellt wurde, wird empfohlen, den Text der Wissensdatenbank im [QnA Maker-Portal](https://qnamaker.ai) zu bearbeiten, anstatt diesen in lokale Dateien zu exportieren und wieder zu importieren. Es kann jedoch vorkommen, dass Sie eine Wissensdatenbank lokal bearbeiten müssen.

Exportieren Sie die Wissensdatenbank auf der Seite **Einstellungen**, und bearbeiten Sie die Wissensdatenbank dann mit Microsoft Excel. Wenn Sie eine andere Anwendung verwenden, um die exportierte Datei zu bearbeiten, fügt die Anwendung eventuell Syntaxfehler ein, da sie nicht vollständig TSV-konform ist. TSV-Dateien von Microsoft Excel fügen in der Regel keine Formatierungsfehler ein.

Wenn Sie mit der Bearbeitung fertig sind, importieren Sie die TSV-Datei auf der Seite **Einstellungen**. Die aktuelle Wissensdatenbank wird dabei vollständig durch die importierte Wissensdatenbank ersetzt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Zusammenarbeiten an einer Wissensdatenbank](../index.yml)

* [Verwalten von QnA Maker-Ressourcen](set-up-qnamaker-service-azure.md)
