---
title: 'Select Columns Transform (Auswählen der Spaltentransformation): Komponentenreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit der Komponente „Select Columns Transform“ im Azure Machine Learning-Designer eine Auswahltransformation ausführen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2020
ms.openlocfilehash: e29f8ba9224390be51c3f29bc255a554b96591ad
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2021
ms.locfileid: "131566452"
---
# <a name="select-columns-transform"></a>Select Columns Transform

In diesem Artikel wird die Verwendung der Komponente „Select Columns Transform“ (Auswählen der Spaltentransformation) im Azure Machine Learning-Designer beschrieben. Der Zweck der Komponente „Select Columns Transform“ besteht darin sicherzustellen, dass für alle weiteren Machine Learning-Vorgänge ein vorhersagbarer, einheitlicher Satz von Spalten verwendet wird.

Diese Komponente ist nützlich für Aufgaben wie Bewertungen, für die spezifische Spalten erforderlich sind. Änderungen in den verfügbaren Spalten können die Pipeline unterbrechen oder die Ergebnisse ändern.

Sie verwenden das Modul „Select Columns Transform“, um einen Satz von Spalten zu erstellen und zu speichern. Danach verwenden Sie die Komponente [Apply Transformation](apply-transformation.md) (Anwenden einer Transformation), um diese Auswahl auf neue Daten anzuwenden.

## <a name="how-to-use-select-columns-transform"></a>Verwenden von „Select Columns Transform“

In diesem Szenario wird davon ausgegangen, dass Sie die Featureauswahl verwenden möchten, um einen dynamischen Satz von Spalten zu generieren, die zum Trainieren eines Modells verwendet werden sollen. Um sicherzustellen, dass die Spaltenauswahl für den Bewertungsprozess identisch ist, verwenden Sie die Komponente „Select Columns Transform“, um die Spaltenauswahl zu erfassen und diese Auswahl an anderer Stelle in der Pipeline anzuwenden.

1. Fügen Sie Ihrer Pipeline im Designer ein Eingabedataset hinzu.

2. Fügen Sie eine Instanz von [Filter Based Feature Selection](filter-based-feature-selection.md) (Filterbasierte Featureauswahl) hinzu.

3. Verbinden Sie die Komponenten und konfigurieren Sie die Komponente „Feature Selection“, damit im Eingabedataset automatisch eine Reihe von optimalen Features gefunden wird.

4. Fügen Sie eine Instanz von [Train Model](train-model.md) (Trainieren eines Modells) hinzu, und verwenden Sie die Ausgabe von [Filter Based Feature Selection](filter-based-feature-selection.md) als Eingabe für das Training.

    > [!IMPORTANT]
    > Weil die Featurerelevanz auf den Werten in der Spalte basiert, können Sie nicht im Voraus wissen, welche Spalten unter Umständen als Eingabe für [Train Model](train-model.md) verfügbar sind.  
5. Fügen Sie eine Instanz der Komponente „Select Columns Transform“ hinzu. 

    Mit diesem Schritt wird eine Spaltenauswahl als Transformation generiert, die gespeichert oder auf andere Datasets angewendet werden kann. In diesem Schritt wird sichergestellt, dass die Spalten, die bei der Featureauswahl ermittelt wurden, gespeichert werden, damit sie von anderen Komponenten wiederverwendet werden können.

6. Hinzufügen der Komponente [Score Model](score-model.md) (Modell bewerten). 

   *Stellen Sie keine Verbindung mit dem Eingabedataset her.* Fügen Sie stattdessen die Komponente [Apply Transformation](apply-transformation.md) hinzu und verbinden Sie die Ausgabe der Featureauswahl-Transformation.

   Die Pipelinestruktur sollte wie folgt aussehen:

   > [!div class="mx-imgBorder"]
   > ![Beispiel-Pipeline](media/module/filter-based-feature-selection-score.png)

   > [!IMPORTANT]
   > Wenn Sie [Filter Based Feature Selection](filter-based-feature-selection.md) auf das Bewertungsdataset anwenden, können Sie nicht erwarten, dass dieselben Ergebnisse erzielt werden. Da die Featureauswahl auf Werten basiert, wird dabei unter Umständen ein anderer Satz von Spalten ausgewählt, was dazu führt, dass der Bewertungsvorgang fehlschlägt.
    
7. Übermitteln Sie die Pipeline.

Durch diese Vorgehensweise mit Speichern und anschließendem Anwenden einer Spaltenauswahl wird sichergestellt, dass für das Training und die Bewertung dasselbe Datenschema verfügbar ist.


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Sammlung der verfügbaren Komponenten](component-reference.md) für Azure Machine Learning an. 
