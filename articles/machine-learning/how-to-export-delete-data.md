---
title: Exportieren oder Löschen von Arbeitsbereichsdaten
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Ihren Arbeitsbereich mit Azure Machine Learning Studio, der Befehlszeilenschnittstelle, dem SDK und authentifizierten REST-APIs exportieren oder löschen.
services: machine-learning
ms.service: machine-learning
ms.subservice: mldata
author: lobrien
ms.author: laobri
ms.date: 10/21/2021
ms.topic: how-to
ms.openlocfilehash: 6f4a09153702e98068034fa5b5866a4baa18de03
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2021
ms.locfileid: "131561516"
---
# <a name="export-or-delete-your-machine-learning-service-workspace-data"></a>Exportieren oder Löschen Ihrer Arbeitsbereichsdaten im Machine Learning-Dienst

In Azure Machine Learning können Sie Ihre Arbeitsbereichsdaten entweder über die grafische Benutzeroberfläche des Portals oder das Python-SDK exportieren oder löschen. Dieser Artikel beschreibt beide Optionen.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-workspace-data"></a>Steuern Ihrer Arbeitsbereichsdaten

Die von Azure Machine Learning gespeicherten, im Produkt enthaltenen Daten können exportiert und gelöscht werden. Der Export- und Löschvorgang kann über Azure Machine Learning Studio, die Befehlszeilenschnittstelle und das SDK erfolgen. Auf Telemetriedaten können Sie über das Azure Privacy-Portal zugreifen. 

Bei Azure Machine Learning bestehen personenbezogene Daten aus Benutzerinformationen in Ausführungsverlaufsdokumenten. 

## <a name="delete-high-level-resources-using-the-portal"></a>Löschen von allgemeinen Ressourcen über das Portal

Wenn Sie einen Arbeitsbereich erstellen, erzeugt Azure mehrere Ressourcen innerhalb der Ressourcengruppe:

- Den Arbeitsbereich selbst
- Ein Speicherkonto
- Eine Containerregistrierung
- Eine Applications Insights-Instanz
- Einen Schlüsseltresor

Diese Ressourcen können gelöscht werden, indem Sie sie aus der Liste und dann **Löschen** auswählen. 

:::image type="content" source="media/how-to-export-delete-data/delete-resource-group-resources.png" alt-text="Screenshot des Portals mit hervorgehobenem Löschsymbol":::

Ausführungsverlaufsdokumente, die persönliche Benutzerinformationen enthalten können, werden im Speicherkonto im Blobspeicher in Unterordnern von `/azureml` gespeichert. Sie können die Daten aus dem Portal herunterladen und löschen.

:::image type="content" source="media/how-to-export-delete-data/storage-account-folders.png" alt-text="Screenshot des azureml-Verzeichnisses im Speicherkonto, innerhalb des Portals":::

## <a name="export-and-delete-machine-learning-resources-using-azure-machine-learning-studio"></a>Exportieren und Löschen von Machine Learning-Ressourcen mit Azure Machine Learning Studio

Azure Machine Learning Studio bietet eine einheitliche Übersicht über Ihre Machine Learning-Ressourcen, z. B. Notebooks, Datasets, Modelle und Experimente. Azure Machine Learning Studio legt Wert auf die Bewahrung einer Aufzeichnung Ihrer Daten und Experimente. Rechenressourcen wie Pipelines und Computeressourcen können über den Browser gelöscht werden. Navigieren Sie für diese Ressourcen zu der betreffenden Ressource, und wählen Sie **Löschen** aus. 

Die Registrierung von Datasets kann aufgehoben und Experimente können archiviert werden, aber diese Vorgänge löschen die Daten nicht. Zum vollständigen Entfernen der Daten müssen Datasets und Experimentdaten auf Speicherebene gelöscht werden. Das Löschen auf der Speicherebene erfolgt, wie zuvor beschrieben, über das Portal. Eine einzelne Ausführung kann direkt in Studio gelöscht werden. Beim Löschen einer Ausführung werden auch die Ausführungsdaten gelöscht. 

> [!NOTE]
> Verwenden Sie vor dem Aufheben der Registrierung eines Datasets dessen **Datenquellenlink**, um die spezifische Daten-URL für das Löschen zu finden. 

Sie können Trainingsartefakte aus experimentellen Ausführungen mit dem Studio herunterladen. Wählen Sie das gewünschte **Experiment** und die **Ausführung** aus. Wählen Sie **Ausgabe + Protokolle** aus, und navigieren Sie zu den spezifischen Artefakten, die Sie herunterladen möchten. Wählen Sie **...** und **Herunterladen** aus.

Sie können ein registriertes Modell herunterladen, indem Sie zu dem **Modell** navigieren und **Herunterladen** auswählen. 

:::image type="contents" source="media/how-to-export-delete-data/model-download.png" alt-text="Screenshot der Studiomodellseite mit hervorgehobener Option „Herunterladen“":::

## <a name="export-and-delete-resources-using-the-python-sdk"></a>Exportieren und Löschen von Ressourcen mit dem Python SDK

Sie können die Ausgaben einer bestimmten Ausführung mit Folgendem herunterladen: 

```python
# Retrieved from Azure Machine Learning web UI
run_id = 'aaaaaaaa-bbbb-cccc-dddd-0123456789AB'
experiment = ws.experiments['my-experiment']
run = next(run for run in ex.get_runs() if run.id == run_id)
metrics_output_port = run.get_pipeline_output('metrics_output')
model_output_port = run.get_pipeline_output('model_output')

metrics_output_port.download('.', show_progress=True)
model_output_port.download('.', show_progress=True)
```

Die folgenden Machine Learning-Ressourcen können mit dem Python SDK gelöscht werden: 

| type | Funktionsaufruf | Notizen | 
| --- | --- | --- |
| `Workspace` | [`delete`](/python/api/azureml-core/azureml.core.workspace.workspace#delete-delete-dependent-resources-false--no-wait-false-) | Verwenden von `delete-dependent-resources` zum Überlappen des Löschvorgangs |
| `Model` | [`delete`](/python/api/azureml-core/azureml.core.model%28class%29#delete--) | | 
| `ComputeTarget` | [`delete`](/python/api/azureml-core/azureml.core.computetarget#delete--) | |
| `WebService` | [`delete`](/python/api/azureml-core/azureml.core.webservice%28class%29) | |