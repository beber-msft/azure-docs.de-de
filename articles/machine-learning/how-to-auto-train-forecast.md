---
title: Einrichten von AutoML für die Zeitreihenvorhersage
titleSuffix: Azure Machine Learning
description: Richten Sie automatisiertes ML in Azure Machine Learning ein, um Zeitreihenvorhersagemodelle mit dem Azure Machine Learning Python-SDK zu trainieren.
services: machine-learning
author: nibaccam
ms.author: nibaccam
ms.service: machine-learning
ms.subservice: automl
ms.topic: how-to
ms.custom: contperf-fy21q1, automl, FY21Q4-aml-seo-hack
ms.date: 10/21/2021
ms.openlocfilehash: 45c8f82729bd4cb16e0d0d36d9a9e70b66a7dbe2
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132345347"
---
# <a name="set-up-automl-to-train-a-time-series-forecasting-model-with-python"></a>Einrichten von AutoML zum Trainieren eines Zeitreihenvorhersagemodells mit Python

In diesem Artikel erfahren Sie, wie Sie das AutoML-Training für Zeitreihenvorhersagemodelle mit dem automatisierten ML von Azure Machine Learning im [Azure Machine Learning Python SDK](/python/api/overview/azure/ml/)einrichten.

Dazu gehen Sie wie folgt vor: 

> [!div class="checklist"]
> * Vorbereiten von Daten für die Zeitreihenmodellierung
> * Konfigurieren spezifischer Zeitreihenparameter in einem Objekt vom Typ [`AutoMLConfig`](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)
> * Ausführen von Vorhersagen mit Zeitreihendaten

Für ein Vorgehen mit wenig Code folgen Sie dem [Tutorial: Vorhersage des Bedarfs mithilfe von automatisiertem maschinellem Lernen](tutorial-automated-ml-forecast.md) für ein Zeitreihenvorhersagebeispiel mit automatisiertem ML im [Azure Machine Learning Studio](https://ml.azure.com/).

Im Gegensatz zu klassischen Methoden für Zeitreihen werden beim automatisierten maschinellen Lernen Zeitreihenwerte aus der Vergangenheit „pivotiert“ und dienen so zusammen mit anderen Vorhersageelementen als zusätzliche Dimensionen für den Regressor. Dieser Ansatz umfasst mehrere Kontextvariablen und deren Beziehung zueinander beim Training. Da sich mehrere Faktoren auf eine Vorhersage auswirken können, richtet sich diese Methode gut an realen Vorhersageszenarios aus. Wenn z. B. Verkaufszahlen vorhergesagt werden sollen, wird das Ergebnis auf der Grundlage von Wechselwirkungen zwischen historischen Trends, des Wechselkurses und des Preises berechnet. 


## <a name="prerequisites"></a>Voraussetzungen

Für diesen Artikel ist Folgendes erforderlich: 

* Ein Azure Machine Learning-Arbeitsbereich. Informationen zum Erstellen des Arbeitsbereichs finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

* In diesem Artikel werden Grundkenntnisse in der Einrichtung eines Experiments mit automatisiertem maschinellem Lernen vorausgesetzt. Machen Sie sich anhand des [Tutorials](tutorial-auto-train-models.md) oder der [Anleitung](how-to-configure-auto-train.md) mit den wichtigsten Entwurfsmustern für automatisierte ML-Experimente vertraut.

    [!INCLUDE [automl-sdk-version](../../includes/machine-learning-automl-sdk-version.md)]
## <a name="preparing-data"></a>Aufbereiten der Daten

Der wichtigste Unterschied zwischen einem Regressionsaufgabentyp für Vorhersagen und einem Regressionsaufgabentyp im automatisierten ML liegt in der Einbeziehung eines Features in Ihre Daten, das eine gültige Zeitreihe darstellt. Eine reguläre Zeitreihe besitzt ein klar definiertes und konsistentes Intervall sowie einen Wert an jedem Stichprobenpunkt in einem ununterbrochenen Zeitraum. 

Betrachten Sie die folgende Momentaufnahme der Datei `sample.csv`:
Bei diesem Dataset handelt es sich um tägliche Vertriebsdaten für ein Unternehmen, das über zwei Ladengeschäfte verfügt: A und B. 

Außerdem gibt es Features für

 *  `week_of_year`: ermöglicht dem Modell, wöchentliche Saisonalität zu erkennen.
* `day_datetime`: stellt eine saubere Zeitreihe mit täglicher Häufigkeit dar.
* `sales_quantity`: die Zielspalte für das Ausführen von Vorhersagen. 

```output
day_datetime,store,sales_quantity,week_of_year
9/3/2018,A,2000,36
9/3/2018,B,600,36
9/4/2018,A,2300,36
9/4/2018,B,550,36
9/5/2018,A,2100,36
9/5/2018,B,650,36
9/6/2018,A,2400,36
9/6/2018,B,700,36
9/7/2018,A,2450,36
9/7/2018,B,650,36
```


Lesen Sie die Daten in einen Pandas-Datenrahmen ein, und verwenden Sie anschließend die Funktion `to_datetime`, um sicherzustellen, dass eine Zeitreihe vom Typ `datetime` verwendet wird.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["day_datetime"] = pd.to_datetime(data["day_datetime"])
```

In diesem Fall sind die Daten bereits aufsteigend nach dem Zeitfeld `day_datetime` sortiert. Bei der Einrichtung eines Experiments muss jedoch darauf geachtet werden, dass die gewünschte Zeitspalte in aufsteigender Reihenfolge sortiert wird, um eine gültige Zeitreihe zu erstellen. 

Für den folgenden Code gilt: 
* Es wird angenommen, dass die Daten 1.000 Datensätze umfassen, und der Code teilt die Daten deterministisch auf, um Trainings- und Testdatasets zu erstellen. 
* Er identifiziert die Bezeichnungsspalte als `sales_quantity`.
* Er trennt das Bezeichnungsfeld von `test_data`, um die Gruppe `test_target` zu bilden.

```python
train_data = data.iloc[:950]
test_data = data.iloc[-50:]

label =  "sales_quantity"
 
test_labels = test_data.pop(label).values
```

> [!IMPORTANT]
> Stellen Sie beim Trainieren eines Modells für die Vorhersage zukünftiger Werte sicher, dass alle während des Trainings verwendeten Features beim Ausführen von Vorhersagen für Ihren gewünschten Vorhersagehorizont verwendet werden können. <br> <br>Wenn Sie also beispielsweise eine Nachfrageprognose erstellen, lässt sich die Trainingsgenauigkeit durch die Einbeziehung eines Features für den aktuellen Aktienkurs erheblich verbessern. Wenn Sie bei Ihrer Vorhersage allerdings einen Vorhersagehorizont verwenden, der weit in der Zukunft liegt, lassen sich zukünftige Aktienkurse für zukünftige Zeitreihenpunkte ggf. nicht präzise vorhersagen, was sich nachteilig auf die Modellgenauigkeit auswirken kann.

<a name="config"></a>

## <a name="training-and-validation-data"></a>Trainings- und Überprüfungsdaten

Sie können separate Datensätze für Training und Überprüfung direkt im `AutoMLConfig`-Objekt angeben.   Erfahren Sie mehr über [AutoMLConfig](#configure-experiment).

Bei Zeitreihenprognosen wird standardmäßig nur die **Kreuzvalidierung mit rollierendem Ursprung (ROCV)** zur Überprüfung verwendet. Übergeben Sie die Trainings- und Überprüfungsdaten, und legen Sie die Anzahl der Kreuzvalidierungsteilmengen mit dem `n_cross_validations`-Parameter in `AutoMLConfig` fest. Die ROCV teilt die Reihe in Trainings- und Validierungsdaten mit einem Ursprungszeitpunkt auf. Wenn der Ursprung zeitlich verschoben wird, werden Teilmengen für die Kreuzvalidierung erstellt. Mit dieser Strategie wird die Datenintegrität von Zeitreihen beibehalten und das Risiko von Datenlecks vermieden.

![Kreuzvalidierung mit rollierendem Ursprung](./media/how-to-auto-train-forecast/ROCV.svg)

Sie können auch eigene Validierungsdaten importieren. Weitere Informationen finden Sie unter [Konfigurieren von Datenaufteilungen und Kreuzvalidierung in AutoML](how-to-configure-cross-validation-data-splits.md#provide-validation-data).


```python
automl_config = AutoMLConfig(task='forecasting',
                             n_cross_validations=3,
                             ...
                             **time_series_settings)
```

Erfahren Sie mehr darüber, wie AutoML die Kreuzvalidierung anwendet, um eine [Überanpassung von Modellen zu verhindern](concept-manage-ml-pitfalls.md#prevent-overfitting).

## <a name="configure-experiment"></a>Konfigurieren des Experiments

Das Objekt [`AutoMLConfig`](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) definiert die erforderlichen Einstellungen und Daten für eine Aufgabe mit automatisiertem maschinellen Lernen. Die Konfiguration für ein Vorhersagemodell ähnelt der Einrichtung eines Standardregressionsmodells, aber bestimmte Modelle, Konfigurationsoptionen und Featurisierungsschritte gelten speziell für Zeitreihendaten. 

### <a name="supported-models"></a>Unterstützte Modelle
Beim automatisierten maschinellen Lernen werden im Rahmen des Modellerstellungs- und Optimierungsprozesses automatisch verschiedene Modelle und Algorithmen getestet. Als Benutzer müssen Sie den Algorithmus nicht angeben. Bei Vorhersageexperimenten sind sowohl native Zeitreihen- als auch Deep Learning-Modelle Teil des Empfehlungssystems. In der folgenden Tabelle ist diese Teilmenge von Modellen zusammengefasst. 

>[!Tip]
> Herkömmliche Regressionsmodelle werden ebenfalls als Teil des Empfehlungssystems für Vorhersageexperimente getestet. Die vollständige Liste der Modelle finden Sie in der [Tabelle mit unterstützten Modellen](how-to-configure-auto-train.md#supported-models). 

Modelle| BESCHREIBUNG | Vorteile
----|----|---
Prophet (Vorschauversion)|Prophet funktioniert am besten mit Zeitreihen, die starke saisonale Effekte aufweisen und viele Saisons von historischen Daten umfassen. Wenn Sie dieses Modell nutzen möchten, installieren Sie es mithilfe von `pip install fbprophet` lokal. | Schnell und genau, stabil gegenüber Ausreißern, fehlenden Daten und dramatischen Änderungen in den Zeitreihen
Auto-ARIMA (Vorschauversion)|Die ARIMA-Methode (Auto-Regressive Integrated Moving Average, autoregressiver integrierter gleitender Mittelwert) erzielt die beste Leistung, wenn die Daten stationär sind. Das bedeutet, dass die statistischen Eigenschaften wie der Mittelwert und Varianz für das gesamte Dataset konstant sind. Wenn Sie beispielsweise eine Münze werfen, ist Ihre Wahrscheinlichkeit für Kopf 50 %, ganz egal, ob Sie die Münze heute, morgen oder im nächsten Jahr werfen.| Dies eignet sich für univariate Reihen, da vergangene Werte für die Vorhersage zukünftiger Werte verwendet werden.
ForecastTCN (Preview)| ForecastTCN ist ein neuronales Netzwerkmodell, das für die aufwendigsten Vorhersageaufgaben konzipiert wurde. Es erfasst nicht lineare lokale und globale Trends in Ihren Daten sowie Beziehungen zwischen Zeitreihen.|Es kann komplexe Trends in Ihren Daten nutzen und problemlos auf die größten Datasets skaliert werden.

### <a name="configuration-settings"></a>Konfigurationseinstellungen

Sie definieren Standardtrainingsparameter wie Aufgabentyp, Iterationsanzahl, Trainingsdaten und Anzahl von Kreuzvalidierungen (ähnlich wie bei einem Regressionsproblem). Bei Vorhersageaufgaben müssen allerdings noch weitere Parameter für das Experiment festgelegt werden. 

Eine Übersicht über zusätzliche Parameter finden in der folgenden Tabelle. Syntaxentwurfsmuster finden Sie in der [Referenzdokumentation zur ForecastingParameter-Klasse](/python/api/azureml-automl-core/azureml.automl.core.forecasting_parameters.forecastingparameters).

| Parametername&nbsp; | BESCHREIBUNG | Erforderlich |
|-------|-------|-------|
|`time_column_name`|Dient zum Angeben der Datetime-Spalte in den Eingabedaten, die zum Erstellen der Zeitreihe sowie zum Ableiten des Intervalls verwendet wird.|✓|
|`forecast_horizon`|Definiert die Anzahl der Zeiträume, die Sie vorhersagen möchten. Der Horizont wird in Einheiten der Zeitreihenhäufigkeit angegeben. Die Einheiten basieren auf dem Zeitintervall Ihrer Trainingsdaten, z. B. monatlich oder wöchentlich, die vorhergesagt werden sollen.|✓|
|`enable_dnn`|[Aktivieren Sie Vorhersage-DNNs](#enable-deep-learning).||
|`time_series_id_column_names`|Die verwendeten Spaltennamen dienen zum eindeutigen Identifizieren der Zeitreihe in Daten, die mehrere Zeilen mit demselben Zeitstempel aufweisen. Ohne definierte Zeitreihenbezeichner wird bei dem Dataset von einer einzelnen Zeitreihe ausgegangen. Weitere Informationen zu einzelnen Zeitreihen finden Sie unter [energy_demand_notebook](https://github.com/Azure/azureml-examples/blob/main/python-sdk/tutorials/automl-with-azureml/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb).||
|`freq`| Die Häufigkeit der Zeitreihendatasets. Dieser Parameter stellt den Zeitraum dar, in dem Ereignisse zu erwarten sind, z. B. täglich, wöchentlich, jährlich usw. Die Häufigkeit muss ein [Pandas-Offset-Alias](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#dateoffset-objects) sein. Weitere Informationen finden Sie im Abschnitt zur [Häufigkeit](#frequency-target-data-aggregation).||
|`target_lags`|Anzahl der Zeilen, um die die Zielwerte basierend auf der Häufigkeit der Daten verzögert werden sollen. Diese Verzögerung wird als Liste oder als einzelner Integer dargestellt. Die Verzögerung sollte verwendet werden, wenn die Beziehung zwischen den unabhängigen Variablen und der abhängigen Variable standardmäßig nicht übereinstimmt oder korreliert. ||
|`feature_lags`| Welche Features verzögert werden, wird automatisch durch automatisiertes ML festgelegt, wenn `target_lags` festgelegt und `feature_lags` auf `auto` festgelegt ist. Das Aktivieren von Featureverzögerungen kann zur Verbesserung der Genauigkeit beitragen. Featureverzögerungen sind standardmäßig deaktiviert. ||
|`target_rolling_window_size`|*n* Historische Zeiträume zum Generieren der vorhergesagten Werte, < = Größe Trainingsmenge. Wenn nicht angegeben, ist *n* die vollständige Trainingsmenge. Geben Sie diesen Parameter an, wenn Sie beim Trainieren des Modells nur eine bestimmte Menge des Verlaufs beachten möchten. Erfahren Sie mehr über [rollierende Zeitfensteraggregationen als Ziel](#target-rolling-window-aggregation).||
|`short_series_handling_config`| Ermöglicht die Verarbeitung kurzer Zeitreihen, um zu vermeiden, dass diese während des Trainings aufgrund unzureichender Daten fehlschlagen. Die Verarbeitung kurzer Reihen ist standardmäßig auf `auto` festgelegt. Erfahren Sie mehr über die [Verarbeitung kurzer Reihen](#short-series-handling).||
|`target_aggregation_function`| Die Funktion, die zum Aggregieren der Zeitreihenzielspalte verwendet wird, um der mit dem Parameter `freq` angegebenen Häufigkeit zu entsprechen. Der Parameter `freq` muss festgelegt werden, damit die `target_aggregation_function` verwendet werden kann. Der Standardwert lautet `None`. Dieser ist für die meisten Szenarien, in denen `sum` verwendet wird, ausreichend.<br> Erfahren Sie mehr über die [Zielspaltenaggregation](#frequency--target-data-aggregation). 


Für den folgenden Code gilt: 
* Er nutzt die [`ForecastingParameters`](/python/api/azureml-automl-core/azureml.automl.core.forecasting_parameters.forecastingparameters)-Klasse, um die Vorhersageparameter für Ihr Experimenttraining zu definieren.
* Er legt `time_column_name` auf das Feld `day_datetime` im Dataset fest. 
* Er definiert den Parameter `time_series_id_column_names` als `"store"`. Dadurch wird sichergestellt, dass **zwei separate Zeitreihengruppen** für die Daten erstellt werden: eine für Geschäft A und eine für B.
* Er legt `forecast_horizon` auf 50 fest, um die Prognose für den gesamten Testsatz durchzuführen. 
* Er legt ein Vorhersagefenster auf 10 Zeiträume mit `target_rolling_window_size` fest.
* Er gibt mit dem Parameter `target_lags` eine einzelne Verzögerung für die Zielwerte um zwei Zeiträume vorwärts an. 
* Er legt `target_lags` auf die empfohlene Einstellung „Automatisch“ fest, mit der dieser Wert automatisch erkannt wird.

```python
from azureml.automl.core.forecasting_parameters import ForecastingParameters

forecasting_parameters = ForecastingParameters(time_column_name='day_datetime', 
                                               forecast_horizon=50,
                                               time_series_id_column_names=["store"],
                                               freq='W',
                                               target_lags='auto',
                                               target_rolling_window_size=10)
                                              
```

Diese `forecasting_parameters` werden dann zusammen mit dem `forecasting`-Aufgabentyp, der primären Metrik, den Beendigungskriterien und den Trainingsdaten an das standardmäßige `AutoMLConfig`-Objekt weitergeleitet. 

```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment
from azureml.train.automl import AutoMLConfig
import logging

automl_config = AutoMLConfig(task='forecasting',
                             primary_metric='normalized_root_mean_squared_error',
                             experiment_timeout_minutes=15,
                             enable_early_stopping=True,
                             training_data=train_data,
                             label_column_name=label,
                             n_cross_validations=5,
                             enable_ensembling=False,
                             verbosity=logging.INFO,
                             **forecasting_parameters)
```

Die Menge der Daten, die für ein erfolgreiches Training eines Vorhersagemodells mit automatisiertem maschinellen Lernen erforderlich ist, wird durch die `forecast_horizon`-, `n_cross_validations`- und `target_lags`- oder `target_rolling_window_size`-Werte beeinflusst, die bei der Konfiguration Ihrer `AutoMLConfig` angegeben werden. 

Die folgende Formel berechnet die Menge der Verlaufsdaten, die für die Konstruktion von Zeitreihenfeatures benötigt werden.

Mindestens erforderliche Verlaufsdaten: (2x `forecast_horizon`) + #`n_cross_validations` + max(max(`target_lags`), `target_rolling_window_size`)

Es wird für jede Reihe im Dataset eine Fehlerausnahme ausgelöst, die nicht die erforderliche Menge an Verlaufsdaten für die angegebenen relevanten Einstellungen erfüllt. 

### <a name="featurization-steps"></a>Featurisierungsschritte

Standardmäßig werden in jedem Experiment mit automatisiertem maschinellem Lernen automatische Skalierungs- und Normalisierungstechniken auf Ihre Daten angewandt. Bei diesen Techniken handelt es sich um Formen der **Featurisierung**, die für *bestimmte* Algorithmen hilfreich sind, die auf Features unterschiedlicher Größenordnungen reagieren. Weitere Informationen zu den Standardfeaturisierungsschritten finden Sie unter [Featurisierung in AutoML](how-to-configure-auto-features.md#automatic-featurization).

Die folgenden Schritte werden jedoch nur für `forecasting`-Aufgabentypen ausgeführt:

* Erkennen des Intervalls der Zeitreihenstichprobe (z. B. stündlich, täglich, wöchentlich) und Erstellen neuer Datensätze für fehlende Zeitpunkte, um eine ununterbrochene Reihe zu erhalten.
* Imputieren fehlender Werte in der Zielspalte (mittels Forward-Fill) und der Featurespalte (mittels Median-Spaltenwerten)
* Erstellen von Features auf der Basis von Zeitreihenbezeichnern zum Ermöglichen von reihenübergreifenden festen Effekten
* Erstellen zeitbasierter Features zur Ermittlung saisonaler Muster
* Codieren kategorischer Variablen zu numerischen Mengen

Eine Zusammenfassung der Features, die durch diese Schritte erstellt werden, finden Sie unter [Transparenz der Featurisierung](how-to-configure-auto-features.md#featurization-transparency).

> [!NOTE]
> Die Schritte zur Featurebereitstellung bei automatisiertem maschinellen Lernen (Featurenormalisierung, Behandlung fehlender Daten, Umwandlung von Text in numerische Daten usw.) werden Teil des zugrunde liegenden Modells. Bei Verwendung des Modells für Vorhersagen werden die während des Trainings angewendeten Schritte zur Featurebereitstellung automatisch auf Ihre Eingabedaten angewendet.

#### <a name="customize-featurization"></a>Anpassen der Featurisierung

Sie können die Featurisierungseinstellungen auch anpassen, um sicherzustellen, dass die Daten und Features zum Trainieren Ihres ML-Modells zu relevanten Vorhersagen führen. 

Unterstützte Anpassungen für `forecasting`-Aufgaben umfassen:

|Anpassung|Definition|
|--|--|
|**Aktualisierung des Spaltenzwecks**|Außerkraftsetzen des automatisch erkannten Featuretyps für die angegebene Spalte|
|**Aktualisierung von Transformationsparametern** |Aktualisieren der Parameter für den angegebenen Transformator. Unterstützt derzeit *Imputer* (fill_value und median).|
|**Löschen von Spalten** |Gibt Spalten an, die aus der Featureverwendung gelöscht werden sollen.|

Um die Featurisierung mit dem SDK anzupassen, geben Sie `"featurization": FeaturizationConfig` in Ihrem `AutoMLConfig`-Objekt an. Erfahren Sie mehr über [benutzerdefinierte Featurisierungen](how-to-configure-auto-features.md#customize-featurization).

>[!NOTE]
> Die Funktion **Spalten löschen** ist seit SDK-Version 1.19 veraltet. Löschen Sie Spalten im Rahmen der Datenbereinigung aus Ihrem Dataset, bevor Sie es in Ihrem automatisierten ML-Experiment nutzen. 

```python
featurization_config = FeaturizationConfig()

# `logQuantity` is a leaky feature, so we remove it.
featurization_config.drop_columns = ['logQuantitity']

# Force the CPWVOL5 feature to be of numeric type.
featurization_config.add_column_purpose('CPWVOL5', 'Numeric')

# Fill missing values in the target column, Quantity, with zeroes.
featurization_config.add_transformer_params('Imputer', ['Quantity'], {"strategy": "constant", "fill_value": 0})

# Fill mising values in the `INCOME` column with median value.
featurization_config.add_transformer_params('Imputer', ['INCOME'], {"strategy": "median"})
```

Wenn Sie Azure Machine Learning Studio für Ihr Experiment verwenden, finden Sie weitere Informationen unter [Anpassen der Featurisierung in Studio](how-to-use-automated-ml-for-ml-models.md#customize-featurization).

## <a name="optional-configurations"></a>Optionale Konfigurationen

Für Vorhersageaufgaben sind zusätzliche optionale Konfigurationen verfügbar, z. B. das Aktivieren von Deep Learning und das Angeben einer rollierenden Zielfensteraggregation. 

### <a name="frequency--target-data-aggregation"></a>Häufigkeit und Zieldatenaggregation

Verwenden Sie den Häufigkeitsparameter `freq`, um Fehler zu vermeiden, die durch unregelmäßige Daten verursacht werden, d h. Daten, die keinem festgelegten Intervall, z. B. stündliche oder tägliche Daten, entsprechen. 

Für sehr unregelmäßige Daten oder für variierende Geschäftsanforderungen können Benutzer die gewünschte Vorhersagehäufigkeit `freq` festlegen und die `target_aggregation_function` angeben, um die Zielspalte der Zeitreihe zu aggregieren. Wenn Sie diese beiden Einstellungen im `AutoMLConfig`-Objekt verwenden, können Sie bei der Datenaufbereitung Zeit sparen. 

Bei Verwendung des Parameters `target_aggregation_function`:
* Die Zielspaltenwerte werden basierend auf dem angegebenen Vorgang aggregiert. `sum` eignet sich für die meisten Szenarien.

* Numerische Vorhersagespalten in den Daten werden nach Summe, Mittelwert, Minimalwert und Maximalwert aggregiert. Deshalb werden durch automatisiertes ML neue Spalten mit dem Namen der Aggregationsfunktion als Suffix erstellt und der ausgewählte Aggregationsvorgang angewendet. 

* Bei kategorischen Vorhersagespalten werden die Daten nach Modus aggregiert. Dies ist die auffälligste Kategorie im Fenster.

* Datumsvorhersagespalten werden nach Minimalwert, Maximalwert und Modus aggregiert. 

Unterstützte Aggregationsvorgänge für Zielspaltenwerte:

|Funktion | description
|---|---
|`sum`| Summe der Zielwerte
|`mean`| Mittelwert oder Durchschnitt der Zielwerte
|`min`| Minimalwert eines Ziels  
|`max`| Maximalwert eines Ziels  

### <a name="enable-deep-learning"></a>Aktivieren von Deep Learning

> [!NOTE]
> Die DNN-Unterstützung für Vorhersagen in Automated Machine Learning befindet sich in der **Vorschau** und wird für lokale oder in Databricks initiierte Läufe nicht unterstützt.

Sie können auch Deep Learning mit Deep Neural Networks (DNNs) anwenden, um die Scores des Modells zu verbessern. Deep Learning mit automatisiertem maschinellem Lernen ermöglicht das Vorhersagen von ein- und mehrdimensionalen Zeitreihendaten.

Deep Learning-Modelle weisen drei intrinsische Funktionen auf:
1. Sie können von beliebigen Zuordnungen von Eingaben zu Ausgaben lernen.
1. Sie unterstützen mehrere Eingaben und Ausgaben.
1. Sie können automatisch Muster in Eingabedaten extrahieren, die lange Folgen umfassen. 

Um Deep Learning zu aktivieren, legen Sie `enable_dnn=True` im `AutoMLConfig`-Objekt fest.

```python
automl_config = AutoMLConfig(task='forecasting',
                             enable_dnn=True,
                             ...
                             **forecasting_parameters)
```
> [!Warning]
> Wenn Sie DNN für mit dem SDK erstellte Experimente aktivieren, sind [Erläuterungen des besten Modells](how-to-machine-learning-interpretability-automl.md) deaktiviert.

Informationen zum Aktivieren von DNN für ein AutoML-Experiment, das in Azure Machine Learning Studio erstellt wurde, finden Sie in der [Schrittanleitung für Aufgabentypeinstellungen in Studio](how-to-use-automated-ml-for-ml-models.md#create-and-run-experiment).

Ein detailliertes Codebeispiel für die Verwendung von DNNs finden Sie im [Notebook für die Vorhersage der Getränkeproduktion](https://github.com/Azure/azureml-examples/blob/main/python-sdk/tutorials/automl-with-azureml/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb).

### <a name="target-rolling-window-aggregation"></a>Rollierende Zeitfensteraggregationen als Ziel
In vielen Fällen ist die beste Information, über die ein Vorhersagemodell verfügen kann, der aktuelle Wert des Ziels.  Wenn Sie rollierende Zeitfensteraggregationen als Ziel verwenden, können Sie eine rollierende Aggregation von Datenwerten als Features hinzufügen. Durch Erzeugen und Verwenden dieser Features als zusätzliche Kontextdaten wird die Genauigkeit des Trainingsmodells gesteigert.

Angenommen, Sie möchten den Energiebedarf vorhersagen. Sie können ein Feature für rollierende Zeitfenster von drei Tagen hinzufügen, um thermische Änderungen beheizter Räume zu erfassen. In diesem Beispiel erstellen Sie dieses Fenster, indem Sie im `AutoMLConfig`-Konstruktor `target_rolling_window_size= 3` festlegen. 

In der Tabelle wird das resultierende Feature Engineering dargestellt, das auftritt, wenn die Zeitfensteraggregation angewandt wird. Spalten für die Werte **minimum, maximum** und **sum** werden in einem gleitenden Fenster über drei Einträge basierend auf den definierten Einstellungen generiert. Jede Zeile verfügt über ein neues berechnetes Feature. Für den Zeitstempel „8. September 2017, 4:00 Uhr“ werden die Werte „maximum“, „minimum“ und „sum“ mithilfe der **Anforderungswerte** für den 8. September 2017, 1:00 Uhr bis 3:00 Uhr, berechnet. Dieses drei Einträge umfassende Fenster wird verschoben, um die verbleibenden Zeilen mit Daten aufzufüllen.

![Ziel für rollierendes Zeitfenster](./media/how-to-auto-train-forecast/target-roll.svg)

Sehen Sie sich ein Python-Codebeispiel an, in dem das [Feature für rollierende Zeitfensteraggregationen als Ziel](https://github.com/Azure/azureml-examples/blob/main/python-sdk/tutorials/automl-with-azureml/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb) angewendet wird.

### <a name="short-series-handling"></a>Verarbeitung kurzer Reihen

Beim automatisierten maschinellen Lernen gilt eine Zeitreihe als **kurze Reihe**, wenn nicht genügend Datenpunkte vorhanden sind, um die Trainings- und Validierungsphasen der Modellentwicklung durchzuführen. Die Anzahl von Datenpunkten variiert je nach Experiment und hängt vom „max_horizon“-Wert, der Anzahl von Kreuzvalidierungsteilungen und der Länge des Rückblickzeitraums des Modells ab, d. h. dem maximalen Verlauf, der zum Erstellen der Zeitreihenfeatures erforderlich ist. Die genaue Berechnung finden Sie in der [Referenzdokumentation zu „short_series_handling_configuration“](/python/api/azureml-automl-core/azureml.automl.core.forecasting_parameters.forecastingparameters#short-series-handling-configuration).

Automatisiertes maschinelles Lernen bietet mit dem `short_series_handling_configuration`-Parameter im `ForecastingParameters`-Objekt standardmäßig eine Verarbeitung kurzer Reihen. 

Zum Aktivieren der Verarbeitung kurzer Reihen muss auch der `freq`-Parameter definiert werden. Zum Definieren einer stündlichen Häufigkeit legen Sie `freq='H'` fest. [Hier](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#dateoffset-objects) finden Sie die Optionen für Häufigkeitszeichenfolgen. Wenn Sie das Standardverhalten (`short_series_handling_configuration = 'auto'`) ändern möchten, aktualisieren Sie den `short_series_handling_configuration`-Parameter in Ihrem `ForecastingParameter`-Objekt.  

```python
from azureml.automl.core.forecasting_parameters import ForecastingParameters

forecast_parameters = ForecastingParameters(time_column_name='day_datetime', 
                                            forecast_horizon=50,
                                            short_series_handling_configuration='auto',
                                            freq = 'H',
                                            target_lags='auto')
```
In der folgenden Tabelle finden Sie eine Zusammenfassung der verfügbaren Einstellungen für `short_series_handling_config`.
 
|Einstellung|Beschreibung
|---|---
|`auto`| Folgendes ist das Standardverhalten für die Verarbeitung kurzer Reihen: <li> *Wenn alle Reihen kurz sind*, werden die Daten aufgefüllt. <br> <li> *Wenn nicht alle Reihen kurz sind*, werden die kurzen Reihen gelöscht. 
|`pad`| Wenn `short_series_handling_config = pad` festgelegt ist, fügt das automatisierte maschinelle Lernen allen gefundenen kurzen Reihen Zufallswerte hinzu. Im Folgenden sind die Spaltentypen und die Werte aufgeführt, mit denen sie aufgefüllt werden: <li>Objektspalten mit NaN-Werten (Not a Number, keine Zahl) <li> Numerische Spalten mit 0 <li> Boolesche/logische Spalten mit „False“ <li> Die Zielspalte wird mit Zufallswerten mit dem Mittelwert 0 und der Standardabweichung 1 aufgefüllt. 
|`drop`| Wenn `short_series_handling_config = drop` festgelegt ist, werden die kurzen Reihen vom automatisierten maschinellen Lernen gelöscht und nicht für Trainings- oder Vorhersagezwecke verwendet. Bei Vorhersagen für diese Reihen werden NaN-Werte zurückgegeben.
|`None`| Es werden keine Reihen aufgefüllt oder gelöscht.

>[!WARNING]
>Das Auffüllen kann sich auf die Genauigkeit des resultierenden Modells auswirken, da künstliche Daten genutzt werden, um vergangene Trainings ohne Fehler abzurufen. <br> <br> Wenn viele der Reihen kurz sind, kann sich dies auch auf die Erklärbarkeit der Ergebnisse auswirken.

## <a name="run-the-experiment"></a>Ausführen des Experiments 

Wenn das `AutoMLConfig`-Objekt bereit ist, können Sie das Experiment übermitteln. Rufen Sie nach Fertigstellung des Modells die Iteration mit der besten Ausführung ab.

```python
ws = Workspace.from_config()
experiment = Experiment(ws, "Tutorial-automl-forecasting")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
``` 
 
## <a name="forecasting-with-best-model"></a>Vorhersagen mit dem besten Modell

Verwenden Sie die beste Modelliteration, um Werte für das Testdataset vorherzusagen.

Die Funktion [forecast_quantiles()](/python/api/azureml-train-automl-client/azureml.train.automl.model_proxy.modelproxy#forecast-quantiles-x-values--typing-any--y-values--typing-union-typing-any--nonetype----none--forecast-destination--typing-union-typing-any--nonetype----none--ignore-data-errors--bool---false-----azureml-data-abstract-dataset-abstractdataset) ermöglicht anzugeben, wann Vorhersagen gestartet werden sollten. Im Gegensatz dazu wird die `predict()`-Methode in der Regel für Klassifizierungs- und Regressionsaufgaben verwendet. Die Methode „forecast_quantiles()“ generiert standardmäßig eine Punkt- oder eine Mittelwertvorhersage, die nicht von einem Kegel der Unsicherheit umgeben ist. 

Im folgenden Beispiel ersetzen Sie zunächst alle Werte in `y_pred` durch `NaN`. Der Ursprung der Vorhersage liegt in diesem Fall am Ende der Trainingsdaten. Wenn Sie jedoch nur die zweite Hälfte von `y_pred` durch `NaN` ersetzen, lässt die Funktion die numerischen Werte in der ersten Hälfte unverändert, sagt aber die `NaN`-Werte in der zweiten Hälfte voraus. Die Funktion gibt sowohl die vorhergesagten Werte als auch die angepassten Features zurück.

Sie können den Parameter `forecast_destination` in der Funktion `forecast_quantiles()` auch verwenden, um Werte bis zu einem bestimmten Datum vorherzusagen.

```python
label_query = test_labels.copy().astype(np.float)
label_query.fill(np.nan)
label_fcst, data_trans = fitted_model.forecast_quantiles(
    test_data, label_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```

Häufig möchten Kunden die Vorhersagen für ein bestimmtes Quantil der Verteilung verstehen. Wenn z. B. die Vorhersage verwendet wird, um den Bestand an Lebensmitteln oder virtuellen Computern für einen Clouddienst zu kontrollieren. In solchen Fällen lautet der Steuerungspunkt in der Regel: „Wir möchten, dass der Artikel zu 99 % der Zeit auf Lager ist und nicht ausgeht“. Im Folgenden wird gezeigt, wie Sie angeben, welche Quantilen Sie für Ihre Vorhersagen sehen möchten, z. B. das 50. oder 95. Perzentil. Wenn Sie kein Quantil angeben, wie im oben genannten Codebeispiel, werden nur die Vorhersagen für das 50. Perzentil erstellt. 

```python
# specify which quantiles you would like 
fitted_model.quantiles = [0.05,0.5, 0.9]
fitted_model.forecast_quantiles(
    test_data, label_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```
 
Berechnen Sie den RMSE-Wert (Root Mean Squared Error, mittlere quadratische Gesamtabweichung) zwischen den tatsächlichen Werten (`actual_labels`) und den vorhergesagten Werten (`predict_labels`).

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(actual_labels, predict_labels))
rmse
```
 
 
Nach der Ermittlung der allgemeinen Modellgenauigkeit besteht der nächste Schritt in der Regel darin, mithilfe des Modells unbekannte zukünftige Werte vorherzusagen. 

Stellen Sie ein Dataset im gleichen Format wie das Testdataset `test_data`, aber mit zukünftigen Datums-/Uhrzeitwerten bereit, um einen Vorhersagesatz mit Vorhersagewerten für die einzelnen Zeitreihenschritte zu erhalten. Angenommen, die letzten Zeitreihendatensätze im Dataset waren für den 31.12.2018. Wenn Sie die Nachfrage für den Folgetag (oder für beliebig viele Vorhersagezeiträume < = `forecast_horizon`) vorhersagen möchten, erstellen Sie für jede Filiale einen einzelnen Zeitreihendatensatz für den 01.01.2019.

```output
day_datetime,store,week_of_year
01/01/2019,A,1
01/01/2019,A,1
```

Wiederholen Sie die erforderlichen Schritte, um diese zukünftigen Daten in einen Datenrahmen zu laden, und führen Sie anschließend `best_run.forecast_quantiles(test_data)` aus, um zukünftige Werte vorherzusagen.

> [!NOTE]
> In-Sample-Vorhersagen werden für die Vorhersage mit automatisiertem maschinellen Lernen nicht unterstützt, wenn `target_lags` und/oder `target_rolling_window_size` aktiviert sind.

## <a name="forecasting-at-scale"></a>Vorhersagen im großen Stil 

Es gibt Szenarios, in denen ein einzelnes Machine Learning-Modell nicht ausreicht und mehrere Machine Learning-Modelle benötigt werden. Beispiele hierfür sind die Vorhersage des Umsatzes für jedes einzelne Geschäft einer Marke oder das Anpassen einer Benutzeroberfläche an die einzelnen Benutzer*innen. Das Erstellen eines Modells für jede Instanz kann bei vielen Problemen im Bereich des maschinellen Lernens zu besseren Ergebnissen führen. 

Das Gruppieren ist ein Konzept bei der Zeitreihenvorhersage, bei dem Zeitreihen kombiniert werden können, um ein einzelnes Modell pro Gruppe zu trainieren. Dieser Ansatz kann besonders hilfreich sein, wenn Sie über Zeitreihen verfügen, bei denen eine Glättung, eine Füllung oder Entitäten in der Gruppe erforderlich sind, die vom Verlauf oder Trends für andere Entitäten profitieren können. Viele Modelle und hierarchische Zeitreihenvorhersagen sind Lösungen, die durch automatisiertes maschinelles Lernen für diese umfangreichen Vorhersageszenarios unterstützt werden. 

### <a name="many-models"></a>Viele Modelle

Die Azure Machine Learning-Lösung mit vielen Modellen mit automatisiertem maschinellem Lernen ermöglicht Benutzern das gleichzeitige Trainieren und Verwalten von Millionen von Modellen. Der Solution Accelerator verwendet [Azure Machine Learning-Pipelines](concept-ml-pipelines.md) zum Trainieren vieler Modelle. Genauer gesagt werden ein [Pipeline](/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29)-Objekt und [ParallelRunStep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.parallelrunstep) verwendet, und es müssen bestimmte Konfigurationsparameter über [ParallelRunConfig](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.parallelrunconfig) festgelegt werden. 


Das folgende Diagramm zeigt den Workflow für die Lösung mit vielen Modellen. 

![Diagramm mit dem Konzept für viele Modelle](./media/how-to-auto-train-forecast/many-models.svg)

Der folgende Code enthält die wichtigsten Parameter, die Benutzer*innen benötigen, um das Ausführen der vielen Modelle einzurichten.

```python
from azureml.train.automl.runtime._many_models.many_models_parameters import ManyModelsTrainParameters

partition_column_names = ['Store', 'Brand']
automl_settings = {"task" : 'forecasting',
                   "primary_metric" : 'normalized_root_mean_squared_error',
                   "iteration_timeout_minutes" : 10, #This needs to be changed based on the dataset. Explore how long training is taking before setting this value 
                   "iterations" : 15,
                   "experiment_timeout_hours" : 1,
                   "label_column_name" : 'Quantity',
                   "n_cross_validations" : 3,
                   "time_column_name": 'WeekStarting',
                   "max_horizon" : 6,
                   "track_child_runs": False,
                   "pipeline_fetch_max_batch_size": 15,}

mm_paramters = ManyModelsTrainParameters(automl_settings=automl_settings, partition_column_names=partition_column_names)

```

### <a name="hierarchical-time-series-forecasting"></a>Hierarchische Zeitreihenvorhersage

In den meisten Anwendungen müssen Kunden ihre Vorhersagen auf der Makro- und Mikroebene des Unternehmens verstehen, egal, ob es sich nun um die Vorhersage des Umsatzes für Produkte an verschiedenen geografischen Standorten oder das Verstehen des erwarteten Mitarbeiterbedarfs für verschiedene Organisationen in einem Unternehmen handelt. Die Fähigkeit, ein Machine Learning-Modell so zu trainieren, dass es intelligente Vorhersagen basierend auf Hierarchiedaten trifft, ist von zentraler Bedeutung. 

Eine hierarchische Zeitreihe ist eine Struktur, in der alle Reihen basierend auf Dimensionen wie Geografie oder Produkttyp in einer Hierarchie angeordnet sind. Das folgende Beispiel zeigt Daten mit eindeutigen Attributen, die hierarchisch angeordnet sind. Die Hierarchie in diesem Fall wird basierend auf dem Produkttyp (z. B. Kopfhörer oder Tablets), der Produktkategorie, die die Produkttypen weiter in Zubehör und Geräte unterteilt, und der Region definiert, in der die Produkte verkauft werden. 

![Beispieltabelle mit Rohdaten für hierarchische Daten](./media/how-to-auto-train-forecast/hierarchy-data-table.svg)
 
Zur weiteren Veranschaulichung enthalten die Blattebenen der Hierarchie alle Zeitreihen mit eindeutigen Kombinationen aus Attributwerten. Jede nächsthöhere Ebene in der Hierarchie berücksichtigt eine Dimension weniger beim Definieren der Zeitreihe und aggregiert jede Gruppe untergeordneter Knoten auf der Ebene darunter in einem übergeordneten Knoten.
 
![Hierarchievisual für Daten](./media/how-to-auto-train-forecast/data-tree.svg)

Die hierarchische Zeitreihenlösung baut auf der Lösung mit vielen Modellen auf und verwendet ein ähnliches Konfigurationssetup.

Der folgende Code enthält die wichtigsten Parameter, die Sie benötigen, um das Ausführen der hierarchischen Zeitreihenvorhersagen einzurichten. 

```python

from azureml.train.automl.runtime._hts.hts_parameters import HTSTrainParameters

model_explainability = True

engineered_explanations = False # Define your hierarchy. Adjust the settings below based on your dataset.
hierarchy = ["state", "store_id", "product_category", "SKU"]
training_level = "SKU"# Set your forecast parameters. Adjust the settings below based on your dataset.
time_column_name = "date"
label_column_name = "quantity"
forecast_horizon = 7


automl_settings = {"task" : "forecasting",
                   "primary_metric" : "normalized_root_mean_squared_error",
                   "label_column_name": label_column_name,
                   "time_column_name": time_column_name,
                   "forecast_horizon": forecast_horizon,
                   "hierarchy_column_names": hierarchy,
                   "hierarchy_training_level": training_level,
                   "track_child_runs": False,
                   "pipeline_fetch_max_batch_size": 15,
                   "model_explainability": model_explainability,# The following settings are specific to this sample and should be adjusted according to your own needs.
                   "iteration_timeout_minutes" : 10,
                   "iterations" : 10,
                   "n_cross_validations": 2}

hts_parameters = HTSTrainParameters(
    automl_settings=automl_settings,
    hierarchy_column_names=hierarchy,
    training_level=training_level,
    enable_engineered_explanations=engineered_explanations
)
```

## <a name="example-notebooks"></a>Beispielnotebooks

Sehen Sie sich die [Notebooks zum Vorhersagebeispiel](https://github.com/Azure/azureml-examples/tree/main/python-sdk/tutorials/automl-with-azureml) an. Dort finden Sie ausführliche Codebeispiele zu einer erweiterten Vorhersagekonfiguration, einschließlich:

* [Feiertagserkennung und Erstellen zusätzlicher Merkmale (Featurization)](https://github.com/Azure/azureml-examples/blob/main/python-sdk/tutorials/automl-with-azureml/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [Kreuzvalidierung mit rollierendem Ursprung (Rolling Origin Validation)](https://github.com/Azure/azureml-examples/blob/main/python-sdk/tutorials/automl-with-azureml/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [Konfigurierbare Verzögerungen (Lags)](https://github.com/Azure/azureml-examples/blob/main/python-sdk/tutorials/automl-with-azureml/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [Aggregierte Zeitfenstermerkmale (Rolling Window Features)](https://github.com/Azure/azureml-examples/blob/main/python-sdk/tutorials/automl-with-azureml/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [DNN](https://github.com/Azure/azureml-examples/blob/main/python-sdk/tutorials/automl-with-azureml/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)

## <a name="next-steps"></a>Nächste Schritte

* Lernen Sie, [wie und wo Sie Modelle bereitstellen](how-to-deploy-and-where.md) können.
* Informieren Sie sich über [Interpretierbarkeit: Modellerklärungen beim automatisierten maschinellen Lernen (Vorschau)](how-to-machine-learning-interpretability-automl.md). 
* Im [Tutorial: Trainieren von Regressionsmodellen](tutorial-auto-train-models.md) finden Sie ein umfassendes Beispiel für das Erstellen von Experimenten mit automatisiertem Machine Learning.
