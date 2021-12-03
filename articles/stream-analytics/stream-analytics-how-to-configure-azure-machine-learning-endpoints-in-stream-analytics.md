---
title: Verwendung von Machine Learning Studio (classic) Endpunkten in Azure Stream Analytics
description: In diesem Artikel wird beschrieben, wie benutzerdefinierte Machine Language-Funktionen in Azure Stream Analytics verwendet werden.
author: jseb225
ms.author: jeanb
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/11/2019
ms.openlocfilehash: d16ad0ea147343b0880ba0b50e2365ac47ace31d
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131003521"
---
# <a name="machine-learning-studio-classic-integration-in-stream-analytics"></a>Integration von Machine Learning Studio (klassisch) in Stream Analytics
Stream Analytics unterstützt benutzerdefinierte Funktionen, die Machine Learning Studio (classic) Endpunkte aufrufen. Die REST-API-Unterstützung für dieses Feature ist in der [Stream Analytics-REST-API-Bibliothek](/rest/api/streamanalytics/)ausführlich beschrieben. Dieser Artikel enthält zusätzliche Informationen, die für die erfolgreiche Implementierung dieser Funktion in Stream Analytics erforderlich sind. Es wurde ein Tutorial bereitgestellt, das [hier](stream-analytics-machine-learning-integration-tutorial.md)verfügbar ist.

## <a name="overview-machine-learning-studio-classic-terminology"></a>Überblick: Machine Learning Studio (klassische) Terminologie
Microsoft Machine Learning Studio (classic) ist ein kollaboratives Drag-and-Drop-Tool, mit dem Sie Predictive-Analytics-Lösungen für Ihre Daten erstellen, testen und bereitstellen können. Dieses Werkzeug heißt *Machine Learning Studio (classic)* . Studio (klassisch) wird zum Interagieren mit den Machine Learning-Ressourcen und zum einfachen Erstellen, Testen und Durchlaufen Ihres Entwurfs verwendet. Unten sind diese Ressourcen und die dazugehörigen Definitionen angegeben.

* **Arbeitsbereich**: Der *Arbeitsbereich* ist ein Container, in dem alle anderen Machine Learning-Ressourcen zur Verwaltung und Steuerung zusammengefasst sind.
* **Experiment**: *Experimente* werden von Data Scientists erstellt, um Datasets zu nutzen und ein Machine Learning-Modell zu trainieren.
* **Endpunkt**: Bei *Endpunkten* handelt es sich um das Studio-Objekt (klassisch), das zum Verwenden von Features als Eingabe, Anwenden eines angegebenen Machine Learning-Modells und Zurückgeben der erzielten Ausgabe eingesetzt wird.
* **Bewertungswebdienst**: Ein *Bewertungswebdienst* ist eine Sammlung von Endpunkten (wie oben beschrieben).

Jeder Endpunkt verfügt über APIs für die Batchausführung und synchrone Ausführung. Für Stream Analytics wird die synchrone Ausführung verwendet. Der spezifische Dienst wird in Machine Learning Studio (classic) als [Request/Response Service](../machine-learning/classic/consume-web-services.md) bezeichnet.

## <a name="studio-classic-resources-needed-for-stream-analytics-jobs"></a>Für Stream Analytics-Aufträge erforderliche Studio-Ressourcen (klassisch)
Bei der Verarbeitung von Stream Analytics-Aufträgen sind für eine erfolgreiche Ausführung ein Anforderung/Antwort-Endpunkt, ein [apikey](../machine-learning/classic/consume-web-services.md)und eine Swagger-Definition erforderlich. Stream Analytics verfügt über einen zusätzlichen Endpunkt, der die URL für den Swagger-Endpunkt erstellt, nach der Schnittstelle sucht und eine UDF-Standarddefinition für den Benutzer zurückgibt.

## <a name="configure-a-stream-analytics-and-studio-classic-udf-via-rest-api"></a>Konfigurieren einer Stream Analytics- und Studio-UDF (klassisch) per REST-API
Mit REST-APIs können Sie Ihren Auftrag so konfigurieren, dass er Studio-Funktionen (klassisch) aufruft. Die Schritte lauten wie folgt:

1. Erstellen eines Stream Analytics-Auftrags
2. Definieren einer Eingabe
3. Definieren einer Ausgabe
4. Erstellen einer benutzerdefinierten Funktion (UDF)
5. Schreiben einer Stream Analytics-Transformation zum Aufrufen der UDF
6. Starten des Auftrags

## <a name="creating-a-udf-with-basic-properties"></a>Erstellen einer UDF mit grundlegenden Eigenschaften
Im folgenden Beispielcode wird eine skalare UDF mit dem Namen *newudf* erstellt, die an einen Machine Learning Studio (classic)-Endpunkt gebunden ist. Beachten Sie, dass sich der *Endpunkt* (Dienst-URI) auf der API-Hilfeseite für den gewählten Dienst und der *apiKey* auf der Hauptseite der Dienste befindet.

```
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

Beispiel für Anforderungstext:

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
```

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Aufrufen des RetrieveDefaultDefinition-Endpunkts für standardmäßige UDF
Nachdem das UDF-Gerüst erstellt wurde, wird die vollständige Definition der UDF benötigt. Mit dem Endpunkt RetrieveDefaultDefinition können Sie die Standarddefinition für eine skalare Funktion abrufen, die an einen Endpunkt von Machine Learning Studio (classic) gebunden ist. Für die unten angegebene Nutzlast ist es erforderlich, dass Sie die UDF-Standarddefinition für eine Skalarfunktion abrufen, die an einen Studio-Endpunkt (klassisch) gebunden ist. Hierbei wird der eigentliche Endpunkt nicht angegeben, da er bereits während der PUT-Anforderung bereitgestellt wurde. Stream Analytics ruft den in der Anforderung angegebenen Endpunkt auf, falls er explizit bereitgestellt wird. Andernfalls wird der ursprünglich angegebene Endpunkt verwendet. Hier verwendet die UDF einen einzelnen Zeichenfolgenparameter (einen Satz) und gibt eine Einzelausgabe einer Typzeichenfolge zurück, mit der die Bezeichnung „sentiment“ (Stimmung) für diesen Satz angegeben wird.

```
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
```

Beispiel für Anforderungstext:

```json
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
```

Eine Beispielausgabe sieht hierfür ungefähr wie folgt aus:

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
```

## <a name="patch-udf-with-the-response"></a>Patchen der UDF mit der Antwort
Jetzt muss die UDF mit der vorherigen Antwort wie unten gezeigt gepatcht werden.

```
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

Anforderungstext (Ausgabe von „RetrieveDefaultDefinition“):

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
```

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Implementieren der Stream Analytics-Transformation für den Aufruf der UDF
Fragen Sie die UDF (hier: scoreTweet) jetzt in Bezug auf alle Eingabeereignisse ab, und schreiben Sie eine Antwort für das Ereignis in eine Ausgabe.

```json
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
```


## <a name="get-help"></a>Hier erhalten Sie Hilfe
Weitere Unterstützung finden Sie auf der [Frageseite von Microsoft Q&A (Fragen und Antworten) zu Azure Stream Analytics](/answers/topics/azure-stream-analytics.html).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](/stream-analytics-query/stream-analytics-query-language-reference)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](/rest/api/streamanalytics/)
