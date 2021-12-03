---
author: eric-urban
ms.service: cognitive-services
ms.topic: include
ms.date: 10/20/2020
ms.author: eur
ms.openlocfilehash: 05d94ddec3c4f5302eb83c430642190759a00561
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131508922"
---
## <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

Zuallererst müssen Sie das <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">Speech SDK für JavaScript </a> installieren. Verwenden Sie dazu die folgenden plattformspezifischen Anleitungen:

- <a href="/azure/cognitive-services/speech-service/speech-sdk?tabs=nodejs#get-the-speech-sdk" target="_blank">Node.js <span 
class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="/azure/cognitive-services/speech-service/speech-sdk?tabs=browser#get-the-speech-sdk" target="_blank">Webbrowser </a>

## <a name="create-voice-signatures"></a>Erstellen von Stimmsignaturen

(Sie können diesen Schritt überspringen, wenn Sie keine vorab registrierten Benutzerprofile für die Identifikation bestimmter Teilnehmer verwenden möchten.)

Wenn Sie Benutzerprofile registrieren möchten, besteht der erste Schritt im Erstellen von Stimmsignaturen für die Teilnehmer der Unterhaltung, damit sie eindeutig als Sprecher erkannt werden können. Die Audioeingabedatei `.wav` zum Erstellen von Stimmsignaturen muss in 16-Bit, mit einer Abtastrate von 16 kHz im Monoformat (einzelner Kanal) vorliegen. Die empfohlene Länge der einzelnen Audiostichproben liegt zwischen 30 Sekunden und zwei Minuten. Ein zu kurzes Audiobeispiel kann bei der Sprechererkennung zu Ungenauigkeiten führen. Bei der Datei `.wav` muss es sich um ein Beispiel für die Stimme **einer Person** handeln, damit ein eindeutiges Sprachprofil erstellt wird.

Das folgende Beispiel zeigt das Erstellen einer Stimmsignatur [mithilfe der REST-API](https://aka.ms/cts/signaturegenservice) in JavaScript. Beachten Sie, dass Sie `subscriptionKey`, `region` und den Pfad der `.wav`-Beispieldatei durch echte Informationen ersetzen müssen.

```javascript
const fs = require('fs');
const axios = require('axios');
const formData = require('form-data');
 
const subscriptionKey = 'your-subscription-key';
const region = 'your-region';
 
async function createProfile() {
    let form = new formData();
    form.append('file', fs.createReadStream('path-to-voice-sample.wav'));
    let headers = form.getHeaders();
    headers['Ocp-Apim-Subscription-Key'] = subscriptionKey;
 
    let url = `https://signature.${region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData`;
    let response = await axios.post(url, form, { headers: headers });
    
    // get signature from response, serialize to json string
    return JSON.stringify(response.data.Signature);
}
 
async function main() {
    // use this voiceSignature string with conversation transcription calls below
    let voiceSignatureString = await createProfile();
    console.log(voiceSignatureString);
}
main();
```

Beim Ausführen dieses Skripts wird eine Stimmsignatur-Zeichenfolge in der Variable `voiceSignatureString` zurückgegeben. Führen Sie die Funktion zweimal aus, damit Sie über zwei Zeichenfolgen verfügen, die Sie unten als Eingabe für die Variablen `voiceSignatureStringUser1` und `voiceSignatureStringUser2` verwenden können.

> [!NOTE]
> Stimmsignaturen können **nur** mithilfe der REST-API erstellt werden.

## <a name="transcribe-conversations"></a>Transkribieren von Konversationen

Der folgende Beispielcode veranschaulicht das Transkribieren von Konversationen in Echtzeit für zwei Sprecher. Dabei wird davon ausgegangen, dass Sie bereits für alle Sprecher Stimmsignatur-Zeichenfolgen erstellt haben, wie oben gezeigt. Ersetzen Sie `subscriptionKey`, `region` und den Pfad `filepath` für das zu transkribierende Audiomaterial durch echte Informationen.

Wenn Sie keine vorab registrierten Benutzerprofile verwenden, dauert es einige Sekunden, bis die erstmalige Erkennung eines unbekannten Benutzers als Speaker1, Speaker2 usw. abgeschlossen ist.

> [!NOTE]
> Stellen Sie sicher, dass derselbe `subscriptionKey` in Ihrer Anwendung für die Erstellung von Signaturen verwendet wird, da es sonst zu Fehlern kommen kann. 

Der Beispielcode führt die folgenden Aufgaben aus:

* Erstellt einen Pushstream, der für die Transkription verwendet werden soll, und schreibt die Beispieldatei `.wav` dafür.
* Erstellt ein `Conversation`-Element mithilfe von `createConversationAsync()`.
* Erstellt ein `ConversationTranscriber`-Element mit dem Konstruktor.
* Fügt der Unterhaltung Teilnehmer hinzu. Die Zeichenfolgen `voiceSignatureStringUser1` und `voiceSignatureStringUser2` sollten sich als Ausgabe aus den oben dargestellten Schritten ergeben.
* Führt eine Registrierung für Ereignisse durch und beginnt mit der Transkription.
* Wenn Sie die Sprecher unterscheiden möchten, ohne Stimmbeispiele zur Verfügung zu stellen, aktivieren Sie das Feature `DifferentiateGuestSpeakers` wie in der [Übersicht zur Unterhaltungstranskription](../../../conversation-transcription.md). 

```javascript
(function() {
    "use strict";
    var sdk = require("microsoft-cognitiveservices-speech-sdk");
    var fs = require("fs");
    
    var subscriptionKey = "your-subscription-key";
    var region = "your-region";
    var filepath = "audio-file-to-transcribe.wav"; // 8-channel audio
    
    var speechTranslationConfig = sdk.SpeechTranslationConfig.fromSubscription(subscriptionKey, region);
    var audioConfig = sdk.AudioConfig.fromWavFileInput(fs.readFileSync(filepath));
    speechTranslationConfig.setProperty("ConversationTranscriptionInRoomAndOnline", "true");

    // en-us by default. Adding this code to specify other languages, like zh-cn.
    speechTranslationConfig.speechRecognitionLanguage = "en-US";
    
    // create conversation and transcriber
    var conversation = sdk.Conversation.createConversationAsync(speechTranslationConfig, "myConversation");
    var transcriber = new sdk.ConversationTranscriber(audioConfig);
    
    // attach the transcriber to the conversation
    transcriber.joinConversationAsync(conversation,
    function () {
        // add first participant using voiceSignature created in enrollment step
        var user1 = sdk.Participant.From("user1@example.com", "en-us", voiceSignatureStringUser1);
        conversation.addParticipantAsync(user1,
        function () {
            // add second participant using voiceSignature created in enrollment step
            var user2 = sdk.Participant.From("user2@example.com", "en-us", voiceSignatureStringUser2);
            conversation.addParticipantAsync(user2,
            function () {
                transcriber.sessionStarted = function(s, e) {
                console.log("(sessionStarted)");
                };
                transcriber.sessionStopped = function(s, e) {
                console.log("(sessionStopped)");
                };
                transcriber.canceled = function(s, e) {
                console.log("(canceled)");
                };
                transcriber.transcribed = function(s, e) {
                console.log("(transcribed) text: " + e.result.text);
                console.log("(transcribed) speakerId: " + e.result.speakerId);
                };
    
                // begin conversation transcription
                transcriber.startTranscribingAsync(
                function () { },
                function (err) {
                    console.trace("err - starting transcription: " + err);
                });
        },
        function (err) {
            console.trace("err - adding user1: " + err);
        });
    },
    function (err) {
        console.trace("err - adding user2: " + err);
    });
    },
    function (err) {
    console.trace("err - " + err);
    });
}()); 
```
