---
title: 'Two-Class Logistic Regression (Logistische Regression mit zwei Klassen): Komponentenreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mit der Komponente „Two-Class Logistic Regression“ (Logistische Regression mit zwei Klassen) in Azure Machine Learning einen binären Klassifizierer erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/22/2020
ms.openlocfilehash: 52bdb0104c33e387aa530d38b492576c90f72201
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2021
ms.locfileid: "131566527"
---
# <a name="two-class-logistic-regression-component"></a>Two-Class Logistic Regression (Logistische Regression mit zwei Klassen) Komponente

Dieser Artikel beschreibt eine Komponente im Azure Machine Learning Designer.

Verwenden Sie diese Komponente, um ein logistisches Regressionsmodell zu erstellen, mit dem zwei (und nur zwei) Ergebnisse vorhergesagt werden können. 

Die logistische Regression ist eine bekannte statistische Methode, die zur Modellierung vieler Arten von Problemen verwendet wird. Dieser Algorithmus ist eine *überwachte Lernmethode*, weshalb Sie ein Dataset bereitstellen müssen, das bereits die Ergebnisse zum Trainieren des Modells enthält.  

### <a name="about-logistic-regression"></a>Informationen zur logistischen Regression  

Die logistische Regression ist eine bekannte Statistikmethode, die zur Vorhersage der Wahrscheinlichkeit eines Ergebnisses verwendet wird und besonders für Klassifizierungsaufgaben geeignet ist. Der Algorithmus prognostiziert die Wahrscheinlichkeit des Vorkommens eines Ereignisses, indem er Daten an eine logistische Funktion anpasst.
  
Bei dieser Komponente wird der Klassifizierungsalgorithmus für dichotome oder binäre Variablen optimiert. Wenn Sie mehrere Ergebnisse klassifizieren müssen, verwenden Sie die Komponente [Multiclass Logistic Regression](./multiclass-logistic-regression.md) (Logisches Regression mit mehreren Klassen).

##  <a name="how-to-configure"></a>Vorgehensweise zur Konfiguration  

Zum Trainieren dieses Modells müssen Sie ein Dataset bereitstellen, das eine Bezeichnungs- oder Klassenspalte enthält. Da diese Komponente für Probleme mit zwei Klassen vorgesehen ist, muss die Bezeichnungs- oder Klassenspalte genau zwei Werte enthalten. 

Beispielsweise kann die Bezeichnungsspalte [Abgestimmt] mit den möglichen Werten „Ja“ oder „Nein“ sein. Oder sie kann [Kreditrisiko] mit den möglichen Werten „Hoch“ oder „Niedrig“ lauten. 
  
1.  Fügen Sie die Komponente **Two-Class Logistic Regression** Ihrer Pipeline hinzu.  
  
2.  Geben Sie an, wie das Modell trainiert werden soll, indem Sie die Option **Create trainer mode** (Trainermodus erstellen) aktivieren.  
  
    -   **Single Parameter** (Einzelner Parameter): Wenn Sie wissen, wie Sie das Modell konfigurieren möchten, können Sie einen bestimmten Satz von Werten als Argumente angeben.  

    -   **Parameter Range** (Parameterbereich): Wenn Sie sich hinsichtlich der besten Parameter nicht sicher sind, können Sie die optimalen Parameter mithilfe der Komponente [Tune Model Hyperparameters](tune-model-hyperparameters.md) (Modell-Hyperparameter optimieren) ermitteln. Sie geben einen Wertebereich an, woraufhin das Training über mehrere Einstellungskombinationen iteriert, um die Wertekombination zu bestimmen, die das beste Ergebnis liefert.
  
3.  Geben Sie für **Optimization tolerance** (Optimierungstoleranz) einen Schwellenwert an, der bei der Optimierung des Modells verwendet werden soll. Wenn die Verbesserung zwischen Iterationen unter den angegebenen Schwellenwert fällt, gilt der Algorithmus als zu einer Lösung konvergiert, woraufhin das Training beendet wird.  
  
4.  Geben Sie für **L1 regularization weight** (Gewichtung von L1-Regularisierung) und **L2 regularization weight** (Gewichtung von L2-Regularisierung) einen Wert für die Regularisierungsparameter L1 und L2 ein. Für beide wird ein Wert ungleich 0 (null) empfohlen.  
     *Regularisierung* ist eine Methode zur Vermeidung von Überpassung durch Zuordnung von Straftermen zu Modellen mit extremen Koeffizientenwerten. Regularisierung funktioniert, indem die Strafterme, die mit Koeffizientenwerten verbunden sind, zum Fehler der Hypothese hinzufügt werden. So würde ein genaues Modell mit extremen Koeffizientenwerten stärker mit Straftermen belegt, während ein weniger genaues Modell mit konservativeren Werten weniger mit Straftermen belegt würde.  
  
     Die L1- und L2-Regularisierung haben unterschiedliche Auswirkungen und Zwecke.  
  
    -   L1 kann auf spärliche Modelle angewendet werden, was bei der Arbeit mit Daten mit hoher Dimensionalität nützlich ist.  
  
    -   Im Gegensatz dazu ist die L2-Regularisierung für Daten vorzuziehen, die nicht spärlich sind.  
  
     Dieser Algorithmus unterstützt eine lineare Kombination von L1- und L2-Regularisierungswerten, d.h. wenn <code>x = L1</code> und <code>y = L2</code>, dann definiert <code>ax + by = c</code> die lineare Spanne der Regalisierungsbegriffe.  
  
    > [!NOTE]
    >  Möchten Sie mehr zur L1- und L2-Regularisierung erfahren? Der folgende Artikel bietet eine Diskussion darüber, inwieweit sich die L1- und L2-Regularisierung unterscheiden und wie sie sich auf die Modellanpassung auswirken, sowie Codebeispiele für logistische Regressions- und neuronale Netzmodelle:  [L1- und L2-Regularisierung für Machine Learning](/archive/msdn-magazine/2015/february/test-run-l1-and-l2-regularization-for-machine-learning)  
    >
    > Für logistische Regressionsmodelle wurden verschiedene lineare Kombinationen von L1- und L2-Begriffen entwickelt: zum Beispiel [Regularisierung mit elastischem Netz](https://wikipedia.org/wiki/Elastic_net_regularization). Wir empfehlen, dass Sie sich auf diese Kombinationen beziehen, um eine lineare Kombination zu definieren, die in Ihrem Modell wirksam ist.
      
5.  Geben Sie für **Memory size for L-BFGS** (Speichergröße für L-BFGS) die Speichergröße an, die für die *L-BFGS*-Optimierung verwendet werden soll.  
  
     L-BFGS steht für „limited memory Broyden-Fletcher-Goldfarb-Shanno“. Es handelt sich um einen Optimierungsalgorithmus, der häufig zur Parameterschätzung verwendet wird. Dieser Parameter gibt die Anzahl der bisherigen Positionen und Gradienten an, die für die Berechnung des nächsten Schritts gespeichert werden sollen.  
  
     Dieser Optimierungsparameter begrenzt die Menge an Speicher, die zur Berechnung des nächsten Schritts und der nächsten Richtung verwendet wird. Wenn Sie weniger Speicher angeben, ist das Training zwar schneller, aber ungenauer.  
  
6.  Geben Sie für **Random number seed** (Zufälliger Startwert) eine ganze Zahl ein. Die Definition eines Startwerts ist wichtig, wenn die Ergebnisse bei mehreren Ausführungen derselben Pipeline reproduzierbar sein sollen.  
  
  
8. Fügen Sie ein gekennzeichnetes Dataset zur Pipeline hinzu, und trainieren Sie das Modell:

    + Wenn Sie **Create trainer mode** (Trainermodus erstellen) auf **Single Parameter** (Einzelner Parameter) festlegen, müssen Sie ein mit Tags versehenes Dataset und die Komponente [Train Model](train-model.md) (Modell trainieren) verbinden.  
  
    + Wenn Sie **Create trainer mode** (Trainermodus erstellen) auf **Parameter Range** (Parameterbereich) festlegen, verbinden Sie ein mit Tags versehenes Dataset, und trainieren Sie das Modell mithilfe von [Tune Model Hyperparameters](tune-model-hyperparameters.md).  
  
    > [!NOTE]
    > 
    > Wenn Sie einen Parameterbereich an [Train Model](train-model.md) übergeben, wird nur der Standardwert in der Liste der Einzelparameter verwendet.  
    > 
    > Wenn Sie einen einzelnen Satz von Parameterwerten an die Komponente [Tune Model Hyperparameters](tune-model-hyperparameters.md) übergeben und ein Bereich von Einstellungen für jeden Parameter erwartet wird, werden die Werte ignoriert und stattdessen die Standardwerte für den Lerner verwendet.  
    > 
    > Wenn Sie die Option **Parameter Range** (Parameterbereich) auswählen und einen einzelnen Wert für einen beliebigen Parameter eingeben, wird dieser angegebene einzelne Wert während des gesamten Löschvorgangs verwendet, auch wenn andere Parameter in einem Wertebereich geändert werden.  
  
9. Übermitteln Sie die Pipeline.  
  
## <a name="results"></a>Ergebnisse

Nach Abschluss des Trainings:
 
  
+ Um Vorhersagen zu neuen Daten zu treffen, verwenden Sie das trainierte Modell und neue Daten als Eingabe für die Komponente [Score Model](./score-model.md) (Modell bewerten). 


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Sammlung der verfügbaren Komponenten](component-reference.md) für Azure Machine Learning an.