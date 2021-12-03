---
title: 'Schnellstart: Unterhaltungstranskription in Echtzeit: Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie die Unterhaltungstranskription in Echtzeit mit dem Speech SDK verwenden. Die Unterhaltungstranskription ermöglicht Ihnen das Transkribieren von Besprechungen und anderen Unterhaltungen mit der Möglichkeit, mehrere Teilnehmer hinzuzufügen, zu entfernen und zu bestimmen, indem Sie Audiodatenströme an den Speech-Dienste streamen.
services: cognitive-services
author: eric-urban
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/20/2020
ms.author: eur
zone_pivot_groups: acs-js-csharp
ms.custom: ignite-fall-2021
ms.openlocfilehash: 00cfb66da3b3e233e703e74b039be9d6a7d02c4b
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131508924"
---
# <a name="get-started-with-real-time-conversation-transcription"></a>Erste Schritte mit der Unterhaltungstranskription in Echtzeit

Mit der **ConversationTranscriber**-API des Speech SDK können Sie Besprechungen und andere Unterhaltungen transkribieren und dabei mehrere Teilnehmer hinzufügen, entfernen und identifizieren, indem Sie Audio mithilfe von `PullStream` oder `PushStream` an die Speech-Dienste streamen. Sie erstellen zunächst Stimmsignaturen für jeden Teilnehmer mithilfe der REST-API und verwenden dann die Stimmsignaturen mit dem SDK, um Unterhaltungen zu transkribieren. Weitere Informationen finden Sie in der [Übersicht](conversation-transcription.md) zur Unterhaltungstranskription.

## <a name="limitations"></a>Einschränkungen

* Nur in den folgenden Abonnementregionen verfügbar : `centralus`, `eastasia`, `eastus`, `westeurope`
* Es ist ein kreisförmiges Mikrofonarray mit 7 Mikrofonen erforderlich. Das Mikrofonarray sollte [unsere Spezifikation](./speech-sdk-microphone.md) erfüllen.
* Das [Speech-Geräte-SDK](speech-devices-sdk.md) stellt geeignete Geräte und eine Beispiel-App zur Veranschaulichung der Unterhaltungstranskription bereit.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird vorausgesetzt, dass Sie über ein Azure-Konto und über ein Abonnement für den Speech-Dienst verfügen. Falls nicht, können Sie [den Speech-Dienst kostenlos testen](overview.md#try-the-speech-service-for-free).

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript Basics include](includes/how-to/conversation-transcription/real-time-javascript.md)]
::: zone-end

::: zone pivot="programming-language-csharp"
[!INCLUDE [C# Basics include](includes/how-to/conversation-transcription/real-time-csharp.md)]
::: zone-end

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Asynchrone Unterhaltungstranskription](how-to-async-conversation-transcription.md)
> [Beispielcode für ROOBO-Geräte](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Java/Android/Speech%20Devices%20SDK%20Starter%20App/example/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/sdsdkstarterapp/ConversationTranscription.java)
> [Beispielcode für Azure Kinect DK](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Java/Windows_Linux/SampleDemo/src/com/microsoft/cognitiveservices/speech/samples/Cts.java)
