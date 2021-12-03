---
title: Verwenden der Clientbibliothek der Formularerkennung für Python
description: Verwenden Sie die Clientbibliothek der Formularerkennung für Python, um eine Formularverarbeitungs-App zu erstellen, die Schlüssel-Wert-Paare und Tabellendaten aus Ihren benutzerdefinierten Dokumenten extrahiert.
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 11/02/2021
ms.author: lajanuar
ms.custom: ignite-fall-2021
ms.openlocfilehash: d60a7dcbbc753ea0e311b51f24f7477139698ae5
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131094929"
---
<!-- markdownlint-disable MD024 -->
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD034 -->

> [!IMPORTANT]
>
> * Dieses Projekt ist auf Formularerkennung REST-API-Version **2.1** ausgerichtet.

[Referenzdokumentation](/python/api/azure-ai-formrecognizer) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/azure/ai/formrecognizer) | [Paket (PyPi)](https://pypi.org/project/azure-ai-formrecognizer/) | [Beispiele](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/samples)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* [Python 3.x](https://www.python.org/)
  * Ihre Python-Installation sollte [pip](https://pip.pypa.io/en/stable/) enthalten. Sie können überprüfen, ob pip installiert ist, indem Sie `pip --version` in der Befehlszeile ausführen. Installieren Sie die aktuelle Python-Version, um pip zu erhalten.
* Trainingsdaten in einem Azure Storage-Blob. Tipps und Optionen für das Zusammenstellen eines Trainingsdatasets finden Sie unter [Erstellen eines Trainingsdatasets für ein benutzerdefiniertes Modell](../../build-training-data-set.md). Sie können die Dateien im Ordner **Trainieren** des [Beispieldatasets](https://go.microsoft.com/fwlink/?linkid=2090451) (herunterladen und extrahieren von *sample_data.zip*) verwenden.
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie über <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer"  title="Erstellen einer Formularerkennungsressource"  target="_blank"> im Azure-Portal eine Formularerkennungsressource </a> erstellen, um Ihren Schlüssel und Endpunkt abzurufen. Wählen Sie nach Abschluss der Bereitstellung **Zu Ressource wechseln** aus.
  * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Formularerkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden unten in den Code eingefügt.
  * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

## <a name="setting-up"></a>Einrichten

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Nach der Installation von Python können Sie die aktuelle Version der Formularerkennungs-Clientbibliothek installieren:

```console
pip install azure-ai-formrecognizer 
```

### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Erstellen Sie eine neue Python-Anwendung namens `form-recognizer.py` in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Importieren Sie dann die folgenden Bibliotheken:

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_imports)]

Erstellen Sie Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_creds)]

## <a name="object-model"></a>Objektmodell

Mit der Formularerkennung können Sie zwei verschiedene Clienttypen erstellen. Der erste (`form_recognizer_client`) wird zum Abfragen des Diensts verwendet, um Formularfelder und -inhalte zu erkennen. Der zweite (`form_training_client`) wird zum Erstellen und Verwalten von benutzerdefinierten Modellen verwendet, mit denen die Erkennung verbessert werden kann.

### <a name="formrecognizerclient"></a>FormRecognizerClient

`form_recognizer_client` stellt Vorgänge für Folgendes bereit:

* Erkennen von Formularfeldern und -inhalten mithilfe von benutzerdefinierten Modellen, die zur Analyse Ihrer benutzerdefinierten Formulare trainiert wurden
* Erkennen von Formularinhalten (einschließlich Tabellen, Zeilen und Wörtern), ohne dass ein Modell trainiert werden muss
* Erkennen allgemeiner Felder in Belegen unter Verwendung eines vorab trainierten Belegmodells für den Formularerkennungsdienst

### <a name="formtrainingclient"></a>FormTrainingClient

`form_training_client` stellt Vorgänge für Folgendes bereit:

* Trainieren benutzerdefinierter Modelle, um alle Felder und Werte in Ihren benutzerdefinierten Formularen zu analysieren. Weitere Informationen finden Sie in der [Dokumentation des Diensts für Modelltraining ohne Bezeichnungen](#train-a-model-without-labels).
* Trainieren benutzerdefinierter Modelle zum Analysieren bestimmter Felder und Werte, die Sie durch Bezeichnen Ihrer benutzerdefinierten Formulare angeben. Weitere Informationen finden Sie in der [Dokumentation des Diensts für Modelltraining mit Bezeichnungen](#train-a-model-with-labels).
* Verwalten der in Ihrem Konto erstellten Modelle
* Kopieren eines benutzerdefinierten Modells aus einer Formularerkennungsressource in eine andere

> [!NOTE]
> Modelle können auch mithilfe einer grafischen Benutzeroberfläche trainiert werden, z. B. mit dem [Formularerkennungstool für die Bezeichnung](../../label-tool.md).

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Hier authentifizieren Sie zwei Clientobjekte mithilfe der oben definierten Abonnementvariablen. Sie verwenden ein **AzureKeyCredential**-Objekt, damit Sie bei Bedarf den API-Schlüssel aktualisieren können, ohne neue Clientobjekte zu erstellen.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_auth)]

## <a name="get-assets-for-testing"></a>Ressourcen zum Testen abrufen

Sie müssen Verweise auf die URLs für Ihre Trainings- und Testdaten hinzufügen.

* [!INCLUDE [get SAS URL](../../includes/sas-instructions.md)]

   :::image type="content" source="../../media/quickstarts/get-sas-url.png" alt-text="SAS-URL-Abruf":::

* Verwenden Sie das Beispielformular und die Belegbilder in den Beispielen weiter unten (auch auf [GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/samples/sample_forms) verfügbar). Alternativ können sie die oben aufgeführten Schritte ausführen, um den SAS-URL eines einzelnen Dokuments im Blobspeicher abzurufen.

> [!NOTE]
> Die Codeausschnitte in diesem Projekt verwenden Remoteformulare, auf die über URLs zugegriffen wird. Wenn Sie stattdessen lokale Formulardokumente verarbeiten möchten, finden Sie weitere Informationen unter den entsprechenden Methoden in der [Referenzdokumentation](/python/api/azure-ai-formrecognizer) und den [Beispielen](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/samples).

## <a name="analyze-layout"></a>Analysieren des Layouts

Mit der Formularerkennung können Sie Tabellen, Zeilen und Wörter in Dokumenten analysieren, ohne ein Modell trainieren zu müssen. Weitere Informationen zur Layoutextraktion finden Sie im [Konzeptleitfaden zum Layout](../../concept-layout.md).

Verwenden Sie die Methode `begin_recognize_content_from_url`, um den Inhalt einer Datei unter einer angegebenen URL zu analysieren. Der zurückgegebene Wert ist eine Sammlung aus `FormPage`-Objekten: eines für jede Seite im übermittelten Dokument. Der folgende Code durchläuft diese Objekte und gibt die extrahierten Schlüssel-Wert-Paare und Tabellendaten aus.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_getcontent)]

> [!TIP]
> Mit Methoden vom Typ [FormRecognizerClient](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient) (beispielsweise `begin_recognize_content`) können Sie auch Inhalte aus lokalen Bildern abrufen. 

### <a name="output"></a>Output

```console
Table found on page 1:
Cell text: Invoice Number
Location: [Point(x=0.5075, y=2.8088), Point(x=1.9061, y=2.8088), Point(x=1.9061, y=3.3219), Point(x=0.5075, y=3.3219)]
Confidence score: 1.0

Cell text: Invoice Date
Location: [Point(x=1.9061, y=2.8088), Point(x=3.3074, y=2.8088), Point(x=3.3074, y=3.3219), Point(x=1.9061, y=3.3219)]
Confidence score: 1.0

Cell text: Invoice Due Date
Location: [Point(x=3.3074, y=2.8088), Point(x=4.7074, y=2.8088), Point(x=4.7074, y=3.3219), Point(x=3.3074, y=3.3219)]
Confidence score: 1.0

Cell text: Charges
Location: [Point(x=4.7074, y=2.8088), Point(x=5.386, y=2.8088), Point(x=5.386, y=3.3219), Point(x=4.7074, y=3.3219)]
Confidence score: 1.0
...

```

## <a name="analyze-receipts"></a>Analysieren von Belegen

In diesem Abschnitt wird veranschaulicht, wie Sie mithilfe eines vorab trainierten Belegmodells gängige Felder in US-Belegen analysieren und extrahieren. Weitere Informationen zur Beleganalyse finden Sie im [Konzeptleitfaden zu Belegen](../../concept-receipt.md). Verwenden Sie die Methode `begin_recognize_receipts_from_url`, um Belege unter einer URL zu analysieren.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_receipts)]

> [!TIP]
> Mit Methoden vom Typ [FormRecognizerClient](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient) (beispielsweise `begin_recognize_receipts`) können Sie auch lokale Bilder von Belegen analysieren. 

### <a name="output"></a>Output

```console
ReceiptType: Itemized has confidence 0.659
MerchantName: Contoso Contoso has confidence 0.516
MerchantAddress: 123 Main Street Redmond, WA 98052 has confidence 0.986
MerchantPhoneNumber: None has confidence 0.99
TransactionDate: 2019-06-10 has confidence 0.985
TransactionTime: 13:59:00 has confidence 0.968
Receipt Items:
...Item #1
......Name: 8GB RAM (Black) has confidence 0.916
......TotalPrice: 999.0 has confidence 0.559
...Item #2
......Quantity: None has confidence 0.858
......Name: SurfacePen has confidence 0.858
......TotalPrice: 99.99 has confidence 0.386
Subtotal: 1098.99 has confidence 0.964
Tax: 104.4 has confidence 0.713
Total: 1203.39 has confidence 0.774
```

## <a name="analyze-business-cards"></a>Analysieren von Visitenkarten

In diesem Abschnitt wird veranschaulicht, wie Sie mithilfe eines vorab trainierten Modells gängige Felder englischsprachiger Visitenkarten analysieren und extrahieren. Weitere Informationen zur Analyse von Visitenkarten finden Sie im [Konzeptleitfaden zu Visitenkarten](../../concept-business-card.md). 

Verwenden Sie die Methode `begin_recognize_business_cards_from_url`, um Visitenkarten unter einer URL zu analysieren.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart-preview.py?name=snippet_bc)]

> [!TIP]
 > Mit Methoden vom Typ [FormRecognizerClient](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient) (beispielsweise `begin_recognize_business_cards`) können Sie auch lokale Bilder von Visitenkarten analysieren. 

## <a name="analyze-invoices"></a>Analysieren von Rechnungen

In diesem Abschnitt wird veranschaulicht, wie Sie mithilfe eines vorab trainierten Modells gängige Rechnungsfelder analysieren und extrahieren. Weitere Informationen zur Rechnungsanalyse finden Sie im [Konzeptleitfaden zu Rechnungen](../../concept-invoice.md). 

Verwenden Sie die Methode `begin_recognize_invoices_from_url`, um Rechnungen unter einer URL zu analysieren.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart-preview.py?name=snippet_invoice)]

> [!TIP]
> Mit Methoden vom Typ [FormRecognizerClient](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient) (beispielsweise `begin_recognize_invoices`) können Sie auch lokale Bilder von Rechnungen analysieren. 

## <a name="analyze-id-documents"></a>Analysieren von Ausweisdokumenten

In diesem Abschnitt wird veranschaulicht, wie Sie mithilfe des vordefinierten Formularerkennungs-ID-Modells wichtige Informationen aus von staatlichen Behörden ausgestellten Ausweisdokumenten (internationale Reisepässe und US-Führerscheine) analysieren und extrahieren. Weitere Informationen zur Ausweisdokumentanalyse finden Sie unter [Vordefiniertes ID-Modell der Formularerkennung für Ausweise](../../concept-id-document.md).

Verwenden Sie die Methode `begin_recognize_id_documents_from_url`, um Ausweisdokumente anhand einer URL zu analysieren.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart-preview.py?name=snippet_id)]

> [!TIP]
 > Mit Methoden vom Typ [FormRecognizerClient](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient?view=azure-python&preserve-view=true#methods) (beispielsweise `begin_recognize_identity_documents`) können Sie auch Ausweisdokumentbilder analysieren. 

## <a name="train-a-custom-model"></a>Trainieren eines benutzerdefinierten Modells

In diesem Abschnitt wird gezeigt, wie Sie ein Modell mit eigenen Daten trainieren. Ein trainiertes Modell kann strukturierte Daten ausgeben, die die Schlüssel-Wert-Beziehungen im ursprünglichen Formulardokument enthalten. Nachdem das Modell trainiert wurde, können Sie es testen, neu trainieren und schließlich verwenden, um Daten aus weiteren Formularen zuverlässig nach Ihren Bedürfnissen zu extrahieren.

> [!NOTE]
> Sie können Modelle auch mithilfe einer grafischen Benutzeroberfläche trainieren, etwa mit dem [Beispielbezeichnungstool der Formularerkennung](../../label-tool.md).

### <a name="train-a-model-without-labels"></a>Trainieren eines Modells ohne Bezeichnungen

Trainieren Sie benutzerdefinierte Modelle, sodass alle Felder und Werte in Ihren benutzerdefinierten Formularen analysiert werden, ohne dass Sie die Trainingsdokumente manuell beschriften müssen.

Im folgenden Code wird der Trainingsclient mit der `begin_training`-Funktion verwendet, um ein Modell für eine bestimmte Gruppe von Dokumenten zu trainieren. Das zurückgegebene `CustomFormModel`-Objekt enthält Informationen zu den Formulartypen, die vom Modell analysiert werden können, und zu den Feldern, die das Modell aus jedem Formulartyp extrahieren kann. Der folgende Codeblock gibt diese Informationen an der Konsole aus.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_train)]

### <a name="output"></a>Output

Hier sehen Sie die Ausgabe eines Modells, das mit den Trainingsdaten aus dem [Python SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/samples/sample_forms/training) trainiert wurde.

```console
Model ID: 628739de-779c-473d-8214-d35c72d3d4f7
Status: ready
Training started on: 2020-08-20 23:16:51+00:00
Training completed on: 2020-08-20 23:16:59+00:00

Recognized fields:
The submodel with form type 'form-0' has recognized the following fields: Additional Notes:, Address:, Company Name:, Company Phone:, Dated As:, Details, Email:, Hero Limited, Name:, Phone:, Purchase Order, Purchase Order #:, Quantity, SUBTOTAL, Seattle, WA 93849 Phone:, Shipped From, Shipped To, TAX, TOTAL, Total, Unit Price, Vendor Name:, Website:
Document name: Form_1.jpg
Document status: succeeded
Document page count: 1
Document errors: []
Document name: Form_2.jpg
Document status: succeeded
Document page count: 1
Document errors: []
Document name: Form_3.jpg
Document status: succeeded
Document page count: 1
Document errors: []
Document name: Form_4.jpg
Document status: succeeded
Document page count: 1
Document errors: []
Document name: Form_5.jpg
Document status: succeeded
Document page count: 1
Document errors: []
```

### <a name="train-a-model-with-labels"></a>Trainieren eines Modells mit Bezeichnungen

Sie können benutzerdefinierte Modelle auch trainieren, indem Sie die Trainingsdokumente manuell bezeichnen. Das Training mit Bezeichnungen führt in einigen Szenarien zu einer besseren Leistung. Das zurückgegebene `CustomFormModel` gibt die Felder an, die das Modell extrahieren kann, und bietet Informationen zur geschätzten Genauigkeit in jedem Feld. Der folgende Codeblock gibt diese Informationen an der Konsole aus.

> [!IMPORTANT]
> Zum Training mit Bezeichnungen benötigen Sie zusätzlich zu den Trainingsdokumenten spezielle Informationsdateien mit Bezeichnungen (`\<filename\>.pdf.labels.json`) in Ihrem Blobspeichercontainer. Das [Beispielbezeichnungstool der Formularerkennung](../../label-tool.md) bietet eine Benutzeroberfläche, auf der Sie diese Bezeichnungsdateien erstellen können. Sobald Sie darüber verfügen, können Sie die `begin_training`-Funktion mit dem auf `true` festgelegten Parameter *use_training_labels* aufrufen.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_trainlabels)]

### <a name="output"></a>Output

Hier sehen Sie die Ausgabe eines Modells, das mit den Trainingsdaten aus dem [Python SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/samples/sample_forms/training) trainiert wurde.

```console
Model ID: ae636292-0b14-4e26-81a7-a0bfcbaf7c91

Status: ready
Training started on: 2020-08-20 23:20:56+00:00
Training completed on: 2020-08-20 23:20:57+00:00

Recognized fields:
The submodel with form type 'form-ae636292-0b14-4e26-81a7-a0bfcbaf7c91' has recognized the following fields: CompanyAddress, CompanyName, CompanyPhoneNumber, DatedAs, Email, Merchant, PhoneNumber, PurchaseOrderNumber, Quantity, Signature, Subtotal, Tax, Total, VendorName, Website
Document name: Form_1.jpg
Document status: succeeded
Document page count: 1
Document errors: []
Document name: Form_2.jpg
Document status: succeeded
Document page count: 1
Document errors: []
Document name: Form_3.jpg
Document status: succeeded
Document page count: 1
Document errors: []
Document name: Form_4.jpg
Document status: succeeded
Document page count: 1
Document errors: []
Document name: Form_5.jpg
Document status: succeeded
Document page count: 1
Document errors: []
```

## <a name="analyze-forms-with-a-custom-model"></a>Analysieren von Formularen mit einem benutzerdefinierten Modell

In diesem Abschnitt wird veranschaulicht, wie Sie mithilfe von Modellen, die Sie mit Ihren eigenen Formularen trainiert haben, Schlüssel-Wert-Informationen und andere Inhalte aus Ihren benutzerdefinierten Formulartypen extrahieren.

> [!IMPORTANT]
> Um dieses Szenario zu implementieren, müssen Sie bereits ein Modell trainiert haben, sodass Sie seine ID an die unten stehende Methode übergeben können. Weitere Informationen finden Sie im Abschnitt [Trainieren eines Modells](#train-a-model-without-labels).

Sie verwenden die Methode `begin_recognize_custom_forms_from_url`. Der zurückgegebene Wert ist eine Sammlung aus `RecognizedForm`-Objekten: eines für jede Seite im übermittelten Dokument. Der folgende Code gibt die Analyseergebnisse an der Konsole aus. Der Code gibt jedes erkannte Feld und den zugehörigen Wert sowie eine Zuverlässigkeitsbewertung aus.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_analyze)]

> [!TIP]
> Auch lokale Bilder können analysiert werden. Mehr dazu erfahren Sie bei den [FormRecognizerClient](/python/api/azure-ai-formrecognizer/azure.ai.formrecognizer.formrecognizerclient)-Methoden, z. B. `begin_recognize_custom_forms`. Alternativ finden Sie im Beispielcode auf [GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/formrecognizer/azure-ai-formrecognizer/samples) Szenarien zu lokalen Bildern.

### <a name="output"></a>Output

Das Modell aus dem vorherigen Beispiel ergibt die folgende Ausgabe:

```console
Form type: form-ae636292-0b14-4e26-81a7-a0bfcbaf7c91
Field 'Merchant' has label 'Merchant' with value 'Invoice For:' and a confidence score of 0.116
Field 'CompanyAddress' has label 'CompanyAddress' with value '1 Redmond way Suite 6000 Redmond, WA' and a confidence score of 0.258
Field 'Website' has label 'Website' with value '99243' and a confidence score of 0.114
Field 'VendorName' has label 'VendorName' with value 'Charges' and a confidence score of 0.145
Field 'CompanyPhoneNumber' has label 'CompanyPhoneNumber' with value '$56,651.49' and a confidence score of 0.249
Field 'CompanyName' has label 'CompanyName' with value 'PT' and a confidence score of 0.245
Field 'DatedAs' has label 'DatedAs' with value 'None' and a confidence score of None
Field 'Email' has label 'Email' with value 'None' and a confidence score of None
Field 'PhoneNumber' has label 'PhoneNumber' with value 'None' and a confidence score of None
Field 'PurchaseOrderNumber' has label 'PurchaseOrderNumber' with value 'None' and a confidence score of None
Field 'Quantity' has label 'Quantity' with value 'None' and a confidence score of None
Field 'Signature' has label 'Signature' with value 'None' and a confidence score of None
Field 'Subtotal' has label 'Subtotal' with value 'None' and a confidence score of None
Field 'Tax' has label 'Tax' with value 'None' and a confidence score of None
Field 'Total' has label 'Total' with value 'None' and a confidence score of None
```

## <a name="manage-custom-models"></a>Verwalten benutzerdefinierter Modelle

In diesem Abschnitt wird veranschaulicht, wie Sie die in Ihrem Konto gespeicherten benutzerdefinierten Modelle verwalten.

### <a name="check-the-number-of-models-in-the-formrecognizer-resource-account"></a>Überprüfen der Anzahl von Modellen im FormRecognizer-Ressourcenkonto

Der folgende Codeblock überprüft, wie viele Modelle in Ihrem Formularerkennungskonto gespeichert sind, und vergleicht diese Zahl mit dem Kontolimit.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_manage_count)]

### <a name="output"></a>Output

```console
Our account has 5 custom models, and we can have at most 5000 custom models
```

### <a name="list-the-models-currently-stored-in-the-resource-account"></a>Auflisten der zurzeit im Ressourcenkonto gespeicherten Modelle

Der folgende Codeblock listet die aktuell in Ihrem Konto vorhandenen Modelle auf und gibt ihre Details an der Konsole aus. Außerdem speichert er einen Verweis auf das erste Modell.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_manage_list)]

### <a name="output"></a>Output

Hier sehen Sie eine Beispielausgabe für das Testkonto.

```console
We have models with the following ids:
453cc2e6-e3eb-4e9f-aab6-e1ac7b87e09e
628739de-779c-473d-8214-d35c72d3d4f7
ae636292-0b14-4e26-81a7-a0bfcbaf7c91
b4b5df77-8538-4ffb-a996-f67158ecd305
c6309148-6b64-4fef-aea0-d39521452699
```

### <a name="get-a-specific-model-using-the-models-id"></a>Abrufen eines bestimmten Modells anhand der Modell-ID

Der folgende Codeblock verwendet die im vorherigen Abschnitt gespeicherte Modell-ID und verwendet sie zum Abrufen von Details über das Modell.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_manage_getmodel)]

### <a name="output"></a>Output

Hier sehen Sie die Beispielausgabe für das im vorherigen Beispiel erstellte benutzerdefinierte Modell.

```console
Model ID: ae636292-0b14-4e26-81a7-a0bfcbaf7c91
Status: ready
Training started on: 2020-08-20 23:20:56+00:00
Training completed on: 2020-08-20 23:20:57+00:00
```

### <a name="delete-a-model-from-the-resource-account"></a>Löschen eines Modells aus dem Ressourcenkonto

Sie können ein Modell auch aus Ihrem Konto löschen, indem Sie auf die ID des Modells verweisen. Dieser Code löscht das im vorherigen Abschnitt verwendete Modell.

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerQuickstart.py?name=snippet_manage_delete)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit Befehl `python` unten aus:

```console
python form-recognizer.py
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../../../cognitive-services/cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../../../cognitive-services/cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="troubleshooting"></a>Problembehandlung

### <a name="general"></a>Allgemein

Die Clientbibliothek der Formularerkennung löst in [Azure Core](https://aka.ms/azsdk-python-azure-core) definierte Ausnahmen aus.

### <a name="logging"></a>Protokollierung

Diese Bibliothek verwendet für die Protokollierung die [Standardprotokollierungsbibliothek](https://docs.python.org/3/library/logging.html). Grundlegende Informationen zu HTTP-Sitzungen (URLs, Header usw.) werden auf der Ebene INFO protokolliert.

Eine ausführliche Protokollierung auf der Ebene DEBUG, einschließlich Anforderungs-/Antworttexten und vollständiger Header, kann auf einem Client mit dem Schlüsselwortargument `logging_enable` aktiviert werden:

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerLogging.py?name=snippet_logging)]

Ebenso kann über `logging_enable` die ausführliche Protokollierung für einen einzelnen Vorgang aktiviert werden, auch wenn diese Funktion für den Client nicht aktiviert ist:

[!code-python[](~/cognitive-services-quickstart-code/python/FormRecognizer/FormRecognizerLogging.py?name=snippet_example)]

## <a name="rest-samples-on-github"></a>REST-Beispiele auf GitHub

* Extrahieren von Text, Auswahlmarkierungen und Tabellenstruktur aus Dokumenten
  * [Extrahieren von Layoutdaten: Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-layout.md)
* Trainieren von benutzerdefinierten Modellen und Extrahieren von Formulardaten
  * [Trainieren ohne Beschriftungen: Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-train-extract.md)
  * [Trainieren mit Beschriftungen: Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-labeled-data.md)
* Extrahieren von Daten aus Rechnungen
  * [Extrahieren von Rechnungsdaten: Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-invoices.md)
* Extrahieren von Daten aus Verkaufsbelegen
  * [Extrahieren von Verkaufsbelegdaten: Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-receipts.md)
* Extrahieren von Daten aus Visitenkarten
  * [Extrahieren von Visitenkartendaten – Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-business-cards.md)

## <a name="next-steps"></a>Nächste Schritte

Für dieses Projekt haben Sie die Clientbibliothek der Formularerkennung für Python verwendet, um auf unterschiedliche Weise Modelle zu trainieren und Formulare zu analysieren. Lesen Sie im Anschluss die Tipps zum Erstellen eines besseren Trainingsdatasets und zum Generieren genauerer Modelle.

> [!div class="nextstepaction"]
> [Erstellen eines Trainingsdatasets](../../build-training-data-set.md)

* [Was ist die Formularerkennung?](../../overview.md)

* Den Beispielcode aus diesem Projekt finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/FormRecognizerQuickstart.py).
