---
title: Referenz zu Translator V3.0
titleSuffix: Azure Cognitive Services
description: Referenzdokumentation für Translator V3.0 Version 3 von Translator umfasst eine moderne JSON-basierte Web-API.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 09/09/2021
ms.author: lajanuar
ms.openlocfilehash: 777ee0bcbf139c9edc9e4715133faec3318f692b
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131434501"
---
# <a name="translator-v30"></a>Translator v3.0

## <a name="whats-new"></a>Neuigkeiten

Version 3 von Translator umfasst eine moderne JSON-basierte Web-API. Sie ermöglicht eine bessere Nutzung und Leistung, indem vorhandene Features zu einer geringeren Zahl von Vorgängen zusammengefasst und neue Features bereitgestellt werden.

* Transliteration zur Konvertierung von Text in einer Sprache aus einem Skript in ein anderes Skript.
* Übersetzung in mehrere Sprachen innerhalb einer Anforderung.
* Spracherkennung, Übersetzung und Transliteration innerhalb einer Anforderung.
* Wörterbuch zum Nachschlagen von Übersetzungsalternativen für einen Begriff und zum Ermitteln von Rückübersetzungen und Kontextbeispielen für die Begriffe.
* Informativere Ergebnisse für die Spracherkennung.

## <a name="base-urls"></a>Basis-URLs

Anforderungen an Translator werden in den meisten Fällen von dem Rechenzentrum bearbeitet, das dem Ursprungsort der Anforderung am nächsten liegt. Wenn bei Verwendung des globalen Endpunkts ein Rechenzentrumsausfall vorliegt, wird die Anforderung möglicherweise außerhalb der geografischen Region weitergeleitet.

Wenn Sie erzwingen möchten, dass die Anforderung innerhalb einer bestimmten geografischen Region verarbeitet wird, verwenden Sie den gewünschten geografischen Endpunkt. Alle Anforderungen werden zwischen den Rechenzentren innerhalb der geografischen Region verarbeitet. 

|Gebiet|Basis-URL (geografischer Endpunkt)|Rechenzentren|
|:--|:--|:--|
|Global (nicht regional)|    api.cognitive.microsofttranslator.com|Nächstgelegenes verfügbares Rechenzentrum|
|Asien-Pazifik|    api-apc.cognitive.microsofttranslator.com|„Südkorea, Süden“, „Japan, Osten“, „Asien, Südosten“ und „Australien, Osten“|
|Europa|    api-eur.cognitive.microsofttranslator.com|„Europa, Norden“, „Europa, Westen“|
|USA|    api-nam.cognitive.microsofttranslator.com|„USA, Osten“, „USA, Süden-Mitte“, „USA, Westen-Mitte“ und „USA, Westen 2“|

<sup>1</sup> Kunden mit einer Ressource in „Schweiz, Norden“ oder „Schweiz, Westen“ können sicherstellen, dass ihre Text-API-Anforderungen innerhalb der Schweiz verarbeitet werden. Um sicherzustellen, dass Anforderungen in der Schweiz verarbeitet werden, muss die Textübersetzungsressource in der Ressourcenregion „Schweiz, Norden“ oder „Schweiz, Westen“ erstellt und anschließend in den API-Anforderungen der benutzerdefinierte Endpunkt der Ressource verwendet werden. Beispiel: Wenn Sie im Azure-Portal eine Textübersetzungsressource mit der Ressourcenregion „Schweiz, Norden“ erstellen und Ihre Ressource den Namen „my-ch-n“ aufweist, lautet der benutzerdefinierte Endpunkt „https://my-ch-n.cognitiveservices.azure.com“. Eine Beispielanforderung für die Übersetzung wäre etwa:
```curl
// Pass secret key and region using headers to a custom endpoint
curl -X POST " my-ch-n.cognitiveservices.azure.com/translator/text/v3.0/translate?to=fr" \
-H "Ocp-Apim-Subscription-Key: xxx" \
-H "Ocp-Apim-Subscription-Region: switzerlandnorth" \
-H "Content-Type: application/json" \
-d "[{'Text':'Hello'}]" -v
```
<sup>2</sup> Benutzerdefinierter Translator ist zurzeit in der Schweiz nicht verfügbar.

## <a name="authentication"></a>Authentifizierung

Abonnieren Sie Translator oder [Cognitive Services-Multi-Service](https://azure.microsoft.com/pricing/details/cognitive-services/) in Azure Cognitive Services, und verwenden Sie Ihren Abonnementschlüssel (im Azure-Portal) für die Authentifizierung.

Es gibt drei Header, mit denen Sie Ihr Abonnement authentifizieren können. Diese Tabelle beschreibt, wie jeder verwendet wird:

|Header|BESCHREIBUNG|
|:----|:----|
|Ocp-Apim-Subscription-Key|*Verwendung mit Cognitive Services-Abonnement, wenn Sie Ihren geheimen Schlüssel übergeben*.<br/>Der Wert ist der geheime Azure-Schlüssel für Ihr Abonnement für Translator.|
|Authorization|*Verwendung mit dem Cognitive Services-Abonnement, wenn Sie ein Authentifizierungstoken übergeben.*<br/>Der Wert ist das Bearertoken: `Bearer <token>`.|
|Ocp-Apim-Subscription-Region|*Verwendung mit einer Cognitive Services-Ressource für mehrere Dienste und einer regionalen Übersetzerressource.*<br/>Der Wert ist die Region der Ressource für mehrere Dienste bzw. der regionalen Übersetzerressource. Dieser Wert ist bei Verwendung einer globalen Übersetzerressource optional.|

###  <a name="secret-key"></a>Geheimer Schlüssel
Die erste Option ist die Authentifizierung mithilfe des Headers `Ocp-Apim-Subscription-Key`. Fügen Sie Ihrer Anforderung den `Ocp-Apim-Subscription-Key: <YOUR_SECRET_KEY>`-Header hinzu.

#### <a name="authenticating-with-a-global-resource"></a>Authentifizieren mit einer globalen Ressource

Wenn Sie eine [globale Übersetzerressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation) verwenden, müssen Sie einen Header einschließen, um Translator aufzurufen.

|Header|BESCHREIBUNG|
|:-----|:----|
|Ocp-Apim-Subscription-Key| Der Wert ist der geheime Azure-Schlüssel für Ihr Abonnement für Translator.|

Hier sehen Sie eine Beispielanforderung zum Aufrufen von Translator mit der globalen Übersetzerressource.

```curl
// Pass secret key using headers
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=es" \
     -H "Ocp-Apim-Subscription-Key:<your-key>" \
     -H "Content-Type: application/json" \
     -d "[{'Text':'Hello, what is your name?'}]"
```

#### <a name="authenticating-with-a-regional-resource"></a>Authentifizieren mit einer regionalen Ressource

Wenn Sie eine [regionale Übersetzerressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation) verwenden,
Sie benötigen zwei Header, um Translator aufzurufen.

|Header|BESCHREIBUNG|
|:-----|:----|
|Ocp-Apim-Subscription-Key| Der Wert ist der geheime Azure-Schlüssel für Ihr Abonnement für Translator.|
|Ocp-Apim-Subscription-Region| Der Wert ist die Region der Übersetzerressource. |

Hier sehen Sie eine Beispielanforderung zum Aufrufen von Translator mit der regionalen Übersetzerressource.

```curl
// Pass secret key and region using headers
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=es" \
     -H "Ocp-Apim-Subscription-Key:<your-key>" \
     -H "Ocp-Apim-Subscription-Region:<your-region>" \
     -H "Content-Type: application/json" \
     -d "[{'Text':'Hello, what is your name?'}]"
```

#### <a name="authenticating-with-a-multi-service-resource"></a>Authentifizieren mit einer Ressource für mehrere Dienste

Sie können eine Cognitive Services-Ressource für mehrere Dienste verwenden. Auf diese Weise können Sie einen einzigen geheimen Schlüssel verwenden, um Anforderungen für mehrere Dienste zu authentifizieren.

Wenn Sie einen geheimen Multi-Service-Schlüssel verwenden, müssen Sie Ihrer Anforderung zwei Authentifizierungsheader hinzufügen. Sie benötigen zwei Header, um Translator aufzurufen.

|Header|BESCHREIBUNG|
|:-----|:----|
|Ocp-Apim-Subscription-Key| Der Wert ist der geheime Azure-Schlüssel für Ihre Ressource für mehrere Dienste.|
|Ocp-Apim-Subscription-Region| Der Wert ist die Region der Ressource für mehrere Dienste. |

Die Region ist für das Abonnement der Multi-Service-Text-API erforderlich. Die von Ihnen ausgewählte Region ist die einzige Region, die Sie für die Textübersetzung verwenden können, wenn Sie den Multi-Service-Abonnementschlüssel verwenden, und muss sich um die gleiche Region handeln, die Sie bei der Registrierung für Ihr Multi-Service-Abonnement über das Azure-Portal ausgewählt haben.

Verfügbare Regionen sind `australiaeast`, `brazilsouth`, `canadacentral`, `centralindia`, `centralus`, `centraluseuap`, `eastasia`, `eastus`, `eastus2`, `francecentral`, `japaneast`, `japanwest`, `koreacentral`, `northcentralus`, `northeurope`, `southcentralus`, `southeastasia`, `uksouth`, `westcentralus`, `westeurope`, `westus`, `westus2` und `southafricanorth`.

Wenn Sie den geheimen Schlüssel in der Abfragezeichenfolge mit dem Parameter `Subscription-Key` übergeben, dann müssen Sie die Region mit dem Abfrageparameter `Subscription-Region` angeben.

### <a name="authenticating-with-an-access-token"></a>Authentifizieren mit einem Zugriffstoken

Alternativ können Sie Ihren geheimen Schlüssel für ein Zugriffstoken austauschen. Dieses Token ist in jeder Anforderung als `Authorization`-Header enthalten. Senden Sie eine `POST`-Anforderung an die folgende URL, um ein Autorisierungstoken zu erhalten:

| Ressourcentyp     | Authentifizierungsdienst-URL                                |
|-----------------|-----------------------------------------------------------|
| Global          | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |
| Regional oder für mehrere Dienste | `https://<your-region>.api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Hier sind Beispielanforderungen zum Abrufen eines Tokens über einen geheimen Schlüssel angegeben:

```curl
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'

// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Eine erfolgreiche Anforderung gibt das codierte Zugriffstoken im Antworttext im Nur-Text-Format zurück. Das gültige Token wird während der Autorisierung als Bearertoken an den Translator-Dienst übergeben.

```http
Authorization: Bearer <Base64-access_token>
```

Ein Authentifizierungstoken ist zehn Minuten lang gültig. Das Token sollte wiederverwendet werden, wenn Translator mehrfach aufgerufen wird. Wenn Ihr Programm aber über längere Zeit Anforderungen an Translator sendet, muss das Programm regelmäßig (beispielsweise alle acht Minuten) ein neues Zugriffstoken anfordern.

## <a name="authentication-with-azure-active-directory-azure-ad"></a>Authentifizierung mit Azure Active Directory (Azure AD)

 Translator v3.0 unterstützt Authentifizierung mit Azure AD, der cloudbasierten Lösung zur Identitäts- und Zugriffsverwaltung von Microsoft.  Autorisierungsheader ermöglichen es dem Übersetzerdienst, zu überprüfen, ob der anfordernde Client berechtigt ist, die Ressource zu verwenden und die Anforderung abzuschließen.

### <a name="prerequisites"></a>**Voraussetzungen**

* Grundlegendes in Kürze zur [**Authentifizierung mit Azure Active Directory**](../../authentication.md?tabs=powershell#authenticate-with-azure-active-directory).

* Grundlegendes in Kürze zur [**Autorisierung des Zugriffs auf verwaltete Identitäten**](../../authentication.md?tabs=powershell#authorize-access-to-managed-identities).

### <a name="headers"></a>**Headers**

|Header|Wert|
|:-----|:----|
|Authorization| Der Wert ist ein von Azure AD generiertes **Bearertoken** für den Zugriff.</br><ul><li> Das Bearertoken bietet einen Authentifizierungsnachweis und überprüft die Autorisierung des Clients für die Verwendung der Ressource.</li><li> Ein Authentifizierungstoken ist 10 Minuten lang gültig und sollte wiederverwendet werden, wenn mehrere Aufrufe von Translator erfolgen.</br></li>*Lesen Sie dazu* [Authentifizieren mit einem Zugriffstoken](#authenticating-with-an-access-token) oben. </ul>|
|Ocp-Apim-Subscription-Region| Der Wert ist die Region der **Übersetzerressource**.</br><ul><li> Bei einer globalen Ressource ist dieser Wert optional.</li></ul>|
|Ocp-Apim-ResourceId| Der Wert ist die Ressourcen-ID für Ihre Instanz der Übersetzerressource.</br><ul><li>Sie finden die Ressourcen-ID im Azure-Portal unter **Übersetzerressource  → Eigenschaften**. </li><li>Format der Ressourcen-ID: </br>/subscriptions/<**Abonnement-ID**>/resourceGroups/<**Ressourcengruppenname**>/providers/Microsoft.CognitiveServices/accounts/<**Ressourcenname**>/</li></ul>|

##### <a name="translator-property-pageazure-portal"></a>**Translator-Eigenschaftenseite – Azure-Portal**

:::image type="content" source="../media/managed-identities/resource-id-property.png" alt-text="Screenshot: Translator-Eigenschaftenseite im Azure-Portal.":::

### <a name="examples"></a>**Beispiele**

#### <a name="using-the-global-endpoint"></a>**Verwenden des globalen Endpunkts**

```curl
 // Using headers, pass a bearer token generated by Azure AD, resource ID, and the region.

curl -X POST "https://api.cognitive.microsofttranslator.com/translator/text/v3.0/translate?api-version=3.0&to=es" \
     -H "Authorization: Bearer <Base64-access_token>"\
     -H "Ocp-Apim-ResourceId: <Resource ID>" \
     -H "Ocp-Apim-Subscription-Region: <your-region>" \
     -H "Content-Type: application/json" \
     -data-raw "[{'Text':'Hello, friend.'}]"
```

#### <a name="using-your-custom-endpoint"></a>**Verwenden Ihres benutzerdefinierten Endpunkts**

```curl
// Using headers, pass a bearer token generated by Azure AD.

curl -X POST https://<your-custom-domain>.cognitiveservices.azure.com/translator/text/v3.0/translate?api-version=3.0&to=es \
     -H "Authorization: Bearer <Base64-access_token>"\
     -H "Content-Type: application/json" \
     -data-raw "[{'Text':'Hello, friend.'}]"
```

### <a name="examples-using-managed-identities"></a>**Beispiele zur Verwendung verwalteter Identitäten**

Translator v3.0 unterstützt auch die Autorisierung des Zugriffs auf verwaltete Identitäten. Wenn für eine Übersetzerressource eine verwaltete Identität aktiviert ist, können Sie das von der verwalteten Identität generierte Bearertoken im Anforderungsheader übergeben.

#### <a name="with-the-global-endpoint"></a>**Mit dem globalen Endpunkt**

```curl
// Using headers, pass a bearer token generated either by Azure AD or Managed Identities, resource ID, and the region.

curl -X POST https://api.cognitive.microsofttranslator.com/translator/text/v3.0/translate?api-version=3.0&to=es \
     -H "Authorization: Bearer <Base64-access_token>"\
     -H "Ocp-Apim-ResourceId: <Resource ID>" \
     -H "Ocp-Apim-Subscription-Region: <your-region>" \
     -H "Content-Type: application/json" \
     -data-raw "[{'Text':'Hello, friend.'}]"
```

#### <a name="with-your-custom-endpoint"></a>**Mit Ihrem benutzerdefinierten Endpunkt**

```curl
//Using headers, pass a bearer token generated by Managed Identities.

curl -X POST https://<your-custom-domain>.cognitiveservices.azure.com/translator/text/v3.0/translate?api-version=3.0&to=es \
     -H "Authorization: Bearer <Base64-access_token>"\
     -H "Content-Type: application/json" \
     -data-raw "[{'Text':'Hello, friend.'}]"
```

## <a name="virtual-network-support"></a>Unterstützung von Virtual Network

Der Translator-Dienst ist jetzt mit VNET-Funktionen (virtuelles Netzwerk) in allen Regionen der öffentlichen Azure-Cloud verfügbar. Informationen zum Aktivieren von Virtual Network finden Sie unter [Konfigurieren von virtuellen Netzwerken für Azure Cognitive Services](../../cognitive-services-virtual-networks.md?tabs=portal).

Nachdem Sie diese Funktion aktiviert haben, müssen Sie den benutzerdefinierten Endpunkt zum Aufrufen von Translator verwenden. Die Verwendung des globalen Übersetzerendpunkts (api.cognitive.microsofttranslator.com) und die Authentifizierung mit einem Zugriffstoken sind nicht möglich.

Sie finden den benutzerdefinierten Endpunkt, nachdem Sie eine [Übersetzerressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation) erstellt und den Zugriff von ausgewählten Netzwerken und privaten Endpunkten zugelassen haben.

|Header|BESCHREIBUNG|
|:-----|:----|
|Ocp-Apim-Subscription-Key| Der Wert ist der geheime Azure-Schlüssel für Ihr Abonnement für Translator.|
|Ocp-Apim-Subscription-Region| Der Wert ist die Region der Übersetzerressource. Bei einer Ressource vom Typ `global` ist dieser Wert optional.|

Hier sehen Sie eine Beispielanforderung zum Aufrufen von Translator mit dem benutzerdefinierten Endpunkt.

```curl
// Pass secret key and region using headers
curl -X POST "https://<your-custom-domain>.cognitiveservices.azure.com/translator/text/v3.0/translate?api-version=3.0&to=es" \
     -H "Ocp-Apim-Subscription-Key:<your-key>" \
     -H "Ocp-Apim-Subscription-Region:<your-region>" \
     -H "Content-Type: application/json" \
     -d "[{'Text':'Hello, what is your name?'}]"
```

## <a name="errors"></a>Errors

Eine Standardfehlerantwort ist ein JSON-Objekt mit einem Name-Wert-Paar mit dem Namen `error`. Der Wert ist außerdem ein JSON-Objekt mit Eigenschaften:

  * `code`: Ein vom Server definierter Fehlercode.
  * `message`: Eine Zeichenfolge als für Menschen lesbare Darstellung des Fehlers.

Ein Kunde mit einer kostenlose Testversion des Abonnements erhält beispielsweise den folgenden Fehler, nachdem das Kontingent für den Free-Tarif erschöpft ist:

```json
{
  "error": {
    "code":403001,
    "message":"The operation is not allowed because the subscription has exceeded its free quota."
    }
}
```

Der Fehlercode ist eine 6-stellige Zahl, die aus dem 3-stelligen HTTP-Statuscode gefolgt von einer 3-stelligen Zahl zur Kategorisierung des Fehlers besteht. Häufige Fehlercodes sind:

| Code | BESCHREIBUNG |
|:----|:-----|
| 400000| Eine der Anforderungseingaben ist ungültig.|
| 400001| Der scope-Parameter ist ungültig.|
| 400002| Der category-Parameter ist ungültig.|
| 400003| Ein Sprachenspezifizierer fehlt oder ist ungültig.|
| 400004| Ein Zielskriptspezifizierer („To script“) fehlt oder ist ungültig.|
| 400005| Ein Eingabetext fehlt oder ist ungültig.|
| 400006| Die Kombination von Sprache und Skript ist ungültig.|
| 400018| Ein Quellskriptspezifizierer („From script“) fehlt oder ist ungültig.|
| 400019| Eine der angegebenen Sprachen wird nicht unterstützt.|
| 400020| Eines der Elemente im Array des Eingabetexts ist ungültig.|
| 400021| Der API-Versionsparameter fehlt oder ist ungültig.|
| 400023| Eines der angegebenen Sprachpaare wird nicht unterstützt.|
| 400035| Die Ausgangssprache (Feld „From“) ist ungültig.|
| 400036| Der Zielsprache (Feld „To“) fehlt oder ist ungültig.|
| 400042| Eine der angegebenen Optionen (Feld „Optionen“) ist ungültig.|
| 400043| Die Clientablaufverfolgungs-ID (Feld „ClientTraceId“ oder X-ClientTranceId-Header) fehlt oder ist ungültig.|
| 400050| Der Eingabetext ist zu lang. Informieren Sie sich unter [Anforderungsgrenzwerte](../request-limits.md).|
| 400064| Der translation-Parameter fehlt oder ist ungültig.|
| 400070| Die Anzahl der Zielskripts (ToScript-Parameter) entspricht nicht der Anzahl von Zielsprachen (To-Parameter).|
| 400071| Der Wert ist für TextType ungültig.|
| 400072| Das Array mit dem Eingabetext enthält zu viele Elemente.|
| 400073| Der script-Parameter ist ungültig.|
| 400074| Der Anforderungstext ist kein gültiger JSON-Code.|
| 400075| Die Kombination aus Sprachpaar und Kategorie ist ungültig.|
| 400077| Die maximale Anforderungsgröße wurde überschritten. Informieren Sie sich unter [Anforderungsgrenzwerte](../request-limits.md).|
| 400079| Das für die Übersetzung zwischen Ausgangs- und Zielsprache angeforderte benutzerdefinierte System ist nicht vorhanden.|
| 400080| Transliteration wird für die Sprache oder das Skript nicht unterstützt.|
| 401000| Die Anforderung wurde nicht autorisiert, da die Anmeldeinformationen fehlen oder ungültig sind.|
| 401015| „Die angegebenen Anmeldeinformationen gelten für die SAPI. Diese Anforderung erfordert Anmeldeinformationen für die Text-API. Verwenden Sie ein Abonnement für Translator.“|
| 403000| Der Vorgang ist nicht zulässig.|
| 403001| Der Vorgang ist nicht zulässig, da das kostenlose Kontingent für das Abonnement überschritten wurde.|
| 405000| Die Anforderungsmethode wird für die angeforderte Ressource nicht unterstützt.|
| 408001| Das angeforderte Übersetzungssystem wird vorbereitet. Versuchen Sie es in einigen Minuten erneut.|
| 408002| Anforderungstimeout beim Warten auf den eingehenden Datenstrom. Der Client hat innerhalb des Zeitraums, in dem der Server vorbereitet war, zu warten, keine Anforderung erzeugt. Der Client kann die Anforderung ohne Änderungen zu einem späteren Zeitpunkt wiederholen.|
| 415000| Der Content-Type-Header fehlt oder ist ungültig.|
| 429000, 429001, 429002| Der Server hat die Anforderung abgelehnt, da der Client die Anforderungsgrenzwerte überschritten hat.|
| 500000| Ein unerwarteter Fehler ist aufgetreten. Wenn der Fehler weiterhin besteht, melden Sie ihn, und geben Sie dabei Folgendes an: Datum und Zeitpunkt des Fehlers, Anforderungsbezeichner aus dem Antwortheader X-RequestId und Clientbezeichner aus dem Anforderungsheader X-ClientTraceId.|
| 503000| Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht verfügbar.) Wiederholen. Wenn der Fehler weiterhin besteht, melden Sie ihn, und geben Sie dabei Folgendes an: Datum und Zeitpunkt des Fehlers, Anforderungsbezeichner aus dem Antwortheader X-RequestId und Clientbezeichner aus dem Anforderungsheader X-ClientTraceId.|

## <a name="metrics"></a>Metriken
Metriken ermöglichen es Ihnen, die Informationen über die Nutzung und Verfügbarkeit des Translator im Azure-Portal im Abschnitt „Metriken“ anzuzeigen, wie im folgenden Screenshot gezeigt. Weitere Informationen finden Sie unter [Daten- und Plattformmetriken](../../../azure-monitor/essentials/data-platform-metrics.md).

![Translator-Metriken](../media/translatormetrics.png)

In dieser Tabelle sind die verfügbaren Metriken mit einer Beschreibung, wie sie zur Überwachung von Aufrufen der Übersetzungs-API verwendet werden, aufgeführt.

| Metriken | BESCHREIBUNG |
|:----|:-----|
| TotalCalls| Gesamtzahl der API-Aufrufe|
| TotalTokenCalls| Gesamtzahl der API-Aufrufe über den Tokendienst mithilfe des Authentifizierungstokens.|
| SuccessfulCalls| Anzahl erfolgreicher Aufrufe|
| TotalErrors| Anzahl der Aufrufe mit Fehlerantwort.|
| BlockedCalls| Anzahl von Aufrufen, die das Raten- oder Kontingentlimit überschritten haben|
| ServerErrors| Anzahl der Anrufe mit serverinternem Fehler (5XX).|
| ClientErrors| Anzahl der Anrufe mit clientseitigem Fehler (4XX).|
| Latency| Dauer bis zum Abschließen der Anforderung in Millisekunden.|
| CharactersTranslated| Gesamtanzahl von Zeichen in einer eingehenden Textanforderung|
