---
title: Technische Profile
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie das TechnicalProfiles-Element einer benutzerdefinierten Richtlinie in Azure Active Directory B2C angeben.
services: active-directory-b2c
author: kengaderdus
manager: CelesteDG
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 05/10/2021
ms.author: kengaderdus
ms.subservice: B2C
ms.openlocfilehash: f9a85604825cc87fa0b77121502bf0901135ab67
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2021
ms.locfileid: "131036350"
---
# <a name="technical-profiles"></a>Technische Profile

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ein *technisches Profil* bietet ein Framework mit einem integrierten Mechanismus für die Kommunikation mit verschiedenen Typen von Seiten. Technische Profile werden verwendet, um mit Ihrem Azure Active Directory B2C-Mandanten (Azure AD B2C) zu kommunizieren, einen Benutzer zu erstellen oder ein Benutzerprofil zu lesen. Ein selbstbestätigtes technisches Profil kann die Interaktion mit dem Benutzer ermöglichen. Beispiel: Ein technisches Profil kann die Anmeldeinformationen des Benutzers für die Anmeldung sammeln und anschließend die Anmeldeseite oder die Seite zum Zurücksetzen des Kennworts rendern.

## <a name="types-of-technical-profiles"></a>Typen technischer Profile

Ein technisches Profil ermöglicht die folgenden Szenarien:

- [Application Insights:](analytics-with-application-insights.md) sendet Ereignisdaten an [Application Insights](../azure-monitor/app/app-insights-overview.md)
- [Azure AD:](active-directory-technical-profile.md) bietet Unterstützung für die Azure AD B2C-Benutzerverwaltung
- [Azure AD Multifactor Authentication:](multi-factor-auth-technical-profile.md) bietet Unterstützung für die Überprüfung einer Telefonnummer mithilfe von Azure AD Multifactor Authentication
- [Anspruchstransformation:](claims-transformation-technical-profile.md) ruft Transformationen für Ausgabeansprüche auf, um Anspruchswerte zu ändern, Ansprüche zu überprüfen oder Standardwerte für Ausgabeansprüche festzulegen
- [ID-Tokenhinweis](id-token-hint.md): überprüft die `id_token_hint`-JWT-Tokensignatur, den Ausstellernamen und die Tokenzielgruppe und extrahiert den Anspruch aus dem eingehenden Token
- [JWT-Tokenaussteller:](jwt-issuer-technical-profile.md) gibt ein JWT-Token aus, das an die Anwendung der vertrauenden Seite zurückgegeben wird
- [OAuth1:](oauth1-technical-profile.md) Verbund mit einem beliebigen Identitätsanbieter für das Protokoll OAuth 1.0
- [OAuth2:](oauth2-technical-profile.md) Verbund mit einem beliebigen Identitätsanbieter für das Protokoll OAuth 2.0
- [Einmalkennwort](one-time-password-technical-profile.md): bietet Unterstützung zum Verwalten der Erstellung und Überprüfung von Einmalkennwörtern
- [OpenID Connect:](openid-connect-technical-profile.md) Verbund mit einem beliebigen Identitätsanbieter für das OpenID Connect-Protokoll
- [MFA per Telefon:](phone-factor-technical-profile.md) unterstützt das Registrieren und Überprüfen von Telefonnummern
- [RESTful-Anbieter:](restful-technical-profile.md) ruft REST-API-Dienste auf, z. B. Überprüfung von Benutzereingaben, Ergänzung von Benutzerdaten oder Integration mit Branchenanwendungen
- [SAML-Identitätsanbieter:](identity-provider-generic-saml.md) Verbund mit einem beliebigen Identitätsanbieter für das SAML-Protokoll
- [SAML-Tokenaussteller:](saml-service-provider.md) gibt ein SAML-Token aus, das an die Anwendung der vertrauenden Seite zurückgegeben wird
- [Selbstbestätigt:](self-asserted-technical-profile.md) interagiert mit dem Benutzer. Beispiel: Die Anmeldeinformationen des Benutzers werden für die Anmeldung, zum Rendern der Anmeldeseite oder für die Kennwortzurücksetzung gesammelt.
- [Sitzungsverwaltung:](custom-policy-reference-sso.md) verarbeitet verschiedene Typen von Sitzungen

## <a name="technical-profile-flow"></a>Fluss technischer Profile

Allen Typen von technischen Profilen liegt das gleiche Konzept zugrunde. Sie beginnen mit dem Lesen der Eingabeansprüche, und führen Anspruchstransformationen aus. Dann kommunizieren sie mit der konfigurierten Seite, z. B. mit einem Identitätsanbieter, der REST-API oder Azure AD-Verzeichnisdiensten. Nach Abschluss des Prozesses gibt das technische Profil die Ausgabeansprüche zurück und führt ggf. Ausgabeanspruchstransformationen aus. Im folgenden Diagramm ist dargestellt, wie die im technischen Profil referenzierten Transformationen und Zuordnungen verarbeitet werden. Unabhängig von der Seite, mit der das technische Profil interagiert, werden die Ausgabeansprüche nach der Ausführung der Anspruchstransformationen sofort im Anspruchsbehälter gespeichert.

![Diagramm: Flow für technische Profile](./media/technical-profiles/technical-profile-flow.png)

1. **Sitzungsverwaltung für einmaliges Anmelden (Single Sign-On, SSO):** stellt den Sitzungszustand des technischen Profils wieder her, indem die [SSO-Sitzungsverwaltung](custom-policy-reference-sso.md) verwendet wird
1. **Transformation von Eingabeansprüchen:** Bevor das technische Profil gestartet wird, führt Azure AD B2C eine [Transformation](claimstransformations.md) des Eingabeanspruchs aus.
1. **Eingabeansprüche:** Ansprüche werden dem Anspruchsbehälter entnommen, der für das technische Profil verwendet wird.
1. **Ausführung des technischen Profils:** Das technische Profil tauscht die Ansprüche mit der konfigurierten Seite aus. Beispiel:
    - Der Benutzer wird an den Identitätsanbieter umgeleitet, um die Anmeldung abzuschließen. Nach der erfolgreichen Anmeldung kehrt der Benutzer zurück, und die Ausführung des technischen Profils wird fortgesetzt.
    - Eine REST-API wird aufgerufen, während Parameter als InputClaims-Elemente gesendet und Informationen als OutputClaims-Elemente empfangen werden.
    - Das Benutzerkonto wird erstellt oder aktualisiert.
    - Die SMS für Multifactor Authentification wird gesendet und überprüft.
1. **Technische Validierungsprofile:** Ein [selbstbestätigtes technisches Profil](self-asserted-technical-profile.md) kann [technische Validierungsprofile](validation-technical-profile.md) aufrufen, um die Daten zu validieren, für die vom Benutzer ein Profil erstellt wurde.
1. **Ausgabeansprüche:** Ansprüche werden an den Anspruchsbehälter zurückgegeben. Sie können diese Ansprüche im nächsten Orchestrierungsschritt oder Transformationen von Ausgabeansprüchen verwenden.
1. **Transformationen von Ausgabeansprüchen**: Wenn das technische Profil erstellt wurde, führt Azure AD B2C [Transformationen](claimstransformations.md) der Ausgabeansprüche aus.
1. **Sitzungsverwaltung für SSO:** speichert die Daten des technischen Profils mithilfe der [SSO-Sitzungsverwaltung](custom-policy-reference-sso.md) dauerhaft in der Sitzung.

Ein **TechnicalProfiles**-Element enthält mehrere technische Profile, die vom Anspruchsanbieter unterstützt werden. Jeder Anspruchsanbieter muss über mindestens ein technisches Profil verfügen. Das technische Profil bestimmt die Endpunkte und die Protokolle, die für die Kommunikation mit dem Anspruchsanbieter erforderlich sind. Ein Anspruchsanbieter kann über mehrere technische Profile verfügen.

```xml
<ClaimsProvider>
  <DisplayName>Display name</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Technical profile identifier">
      <DisplayName>Display name of technical profile</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        ...
      </Metadata>
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

Das **TechnicalProfile**-Element enthält die folgenden Attribute:

| attribute | Erforderlich | BESCHREIBUNG |
|---------|---------|---------|
| Id | Ja | Ein eindeutiger Bezeichner des technischen Profils. Auf das technische Profil kann mit diesem Bezeichner von anderen Elementen in der Richtliniendatei verwiesen werden. Beispiele: **OrchestrationSteps** und **ValidationTechnicalProfile**. |

Das **TechnicalProfile**-Element enthält die folgenden Elemente:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| Domain | 0:1 | Der Domänenname für das technische Profil. Wenn das technische Profil z.B. als Identitätsanbieter Facebook angibt, lautet der Domänenname „Facebook.com“. |
| DisplayName | 1:1 | Dies ist der Anzeigename des technischen Profils. |
| BESCHREIBUNG | 0:1 | Dies ist die Beschreibung des technischen Profils. |
| Protocol | 1:1 | Das Protokoll, das für die Kommunikation mit der anderen Seite verwendet wird. |
| Metadaten | 0:1 | Schlüssel und Werte, die das Verhalten des technischen Profils steuern |
| InputTokenFormat | 0:1 | Das Format des Eingabetokens. Mögliche Werte sind `JSON`, `JWT`, `SAML11` oder `SAML2`. Der Wert `JWT` stellt ein JSON Web Token (JWT) gemäß IETF-Spezifikation dar Der Wert `SAML11` stellt ein SAML 1.1-Sicherheitstoken gemäß OASIS-Spezifikation dar Der Wert `SAML2` stellt ein SAML 2.0-Sicherheitstoken gemäß OASIS-Spezifikation dar |
| OutputTokenFormat | 0:1 | Das Format des Ausgabetokens. Mögliche Werte sind `JSON`, `JWT`, `SAML11` oder `SAML2`. |
| CryptographicKeys | 0:1 | Eine Liste kryptografischer Schlüssel, die im technischen Profil verwendet werden. |
| InputClaimsTransformations | 0:1 | Eine Liste zuvor definierter Verweise auf Anspruchstransformationen, die ausgeführt werden sollen, bevor Ansprüche an den Anspruchsanbieter oder die vertrauende Seite gesendet werden. |
| InputClaims | 0:1 | Eine Liste von zuvor definierten Verweisen auf Anspruchstypen, die als Eingabe für das technische Profil verwendet werden |
| PersistedClaims | 0:1 | Eine Liste der zuvor definierten Verweise auf Anspruchstypen, die vom technischen Profil dauerhaft gespeichert werden |
| DisplayClaims | 0:1 | Eine Liste der zuvor definierten Verweise auf Anspruchstypen, die vom [selbstbestätigten technischen Profil](self-asserted-technical-profile.md) dargestellt werden Das DisplayClaims-Feature steht derzeit als Vorschau zur Verfügung. |
| OutputClaims | 0:1 | Eine Liste der zuvor definierten Verweise auf Anspruchstypen, die als Ausgabe für das technische Profil verwendet werden |
| OutputClaimsTransformations | 0:1 | Eine Liste zuvor definierter Verweise auf Anspruchstransformationen, die ausgeführt werden sollen, nachdem Ansprüche vom Anspruchsanbieter empfangen wurden. |
| ValidationTechnicalProfiles | 0:n | Eine Liste der Verweise auf andere technische Profile, die das technische Profil für die Überprüfung verwendet. Weitere Informationen finden Sie unter [Technisches Validierungsprofil](validation-technical-profile.md).|
| SubjectNamingInfo | 0:1 | Steuert die Erzeugung von Antragstellernamen in Token, wenn der Name des Antragstellers getrennt von den Ansprüchen angegeben wird. Beispiele: OAuth oder SAML. |
| IncludeInSso | 0:1 | Gibt an, ob bei Verwendung dieses technischen Profils SSO-Verhalten für die Sitzung verwendet werden soll oder ob stattdessen eine explizite Interaktion erforderlich ist. Dieses Element ist nur in Profilen vom Typ „SelfAsserted“ gültig, die in einem technischen Validierungsprofil verwendet werden. Mögliche Werte sind: `true` (Standard) oder `false`. |
| IncludeClaimsFromTechnicalProfile | 0:1 | Ein Bezeichner eines technischen Profils, von dem alle Eingabe- und Ausgabeansprüche diesem technischen Profil hinzugefügt werden sollen. Das referenzierte technische Profil muss in derselben Richtliniendatei definiert sein. |
| IncludeTechnicalProfile |0:1 | Ein Bezeichner eines technischen Profils, von dem alle Daten diesem technischen Profil hinzugefügt werden sollen. |
| UseTechnicalProfileForSessionManagement | 0:1 | Ein anderes technische Profil, das für die Sitzungsverwaltung verwendet werden soll. |
|EnabledForUserJourneys| 0:1 |Steuert, ob das technische Profil in einer User Journey ausgeführt wird. |

## <a name="protocol"></a>Protocol

Das Element **Protocol** gibt das Protokoll an, das für die Kommunikation mit der anderen Seite verwendet werden soll. Das **Protocol**-Element enthält die folgenden Attribute:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| Name | Ja | Der Name eines gültigen Protokolls, das von Azure AD B2C unterstützt und als Teil des technischen Profils verwendet wird. Mögliche Werte sind `OAuth1`, `OAuth2`, `SAML2`, `OpenIdConnect`, `Proprietary` oder `None`. |
| Handler | Nein | Wenn der Name des Protokolls auf `Proprietary` festgelegt ist, geben Sie den Namen der Assembly an, die von Azure AD B2C zum Bestimmen des Protokollhandlers verwendet wird. |

## <a name="metadata"></a>Metadaten

Das Element **Metadata** enthält die relevanten Konfigurationsoptionen für ein bestimmtes Protokoll. Die Liste der unterstützten Metadaten ist in der Spezifikation des entsprechenden [technischen Profils](#types-of-technical-profiles) dokumentiert. Das **Metadata**-Element enthält das folgende Element:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| Element | 0:n | Die Metadaten zu dem technischen Profil. Jeder Typ von technischem Profil verfügt über einen anderen Satz von Metadatenelementen. Weitere Informationen finden Sie im Abschnitt „Typen technischer Profile“. |

### <a name="item"></a>Element

Das **Item**-Element des **Metadata**-Elements enthält das folgende Attribut:

| Attribut | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| Schlüssel | Ja | Der Metadatenschlüssel. Eine Liste der entsprechenden Metadata-Elemente finden Sie bei den einzelnen [Typen von technischen Profilen](#types-of-technical-profiles). |

Das folgende Beispiel veranschaulicht die Verwendung von Metadaten für ein [technisches OAuth2-Profil](oauth2-technical-profile.md#metadata).

```xml
<TechnicalProfile Id="Facebook-OAUTH">
  ...
  <Metadata>
    <Item Key="ProviderName">facebook</Item>
    <Item Key="authorization_endpoint">https://www.facebook.com/dialog/oauth</Item>
    <Item Key="AccessTokenEndpoint">https://graph.facebook.com/oauth/access_token</Item>
    <Item Key="HttpBinding">GET</Item>
    <Item Key="UsePolicyInRedirectUri">0</Item>
    ...
  </Metadata>
  ...
</TechnicalProfile>
```

Das folgende Beispiel veranschaulicht die Verwendung von Metadaten für ein [technisches REST-API-Profil](restful-technical-profile.md#metadata).

```xml
<TechnicalProfile Id="REST-Validate-Email">
  ...
  <Metadata>
    <Item Key="ServiceUrl">https://api.sendgrid.com/v3/mail/send</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
    ...
  </Metadata>
  ...
</TechnicalProfile>
```

## <a name="cryptographic-keys"></a>Kryptografische Schlüssel

Azure AD B2C speichert Geheimnisse und Zertifikate in Form von [Richtlinienschlüsseln](policy-keys-overview.md), um eine Vertrauensstellung mit den Diensten herzustellen, in die Azure AD B2C integriert ist. Während der Ausführung eines technischen Profils ruft Azure AD B2C die kryptografischen Schlüssel aus Azure AD B2C-Richtlinienschlüsseln ab. Dann verwendet Azure AD B2C die Schlüssel, um eine Vertrauensstellung herzustellen, ein Token zu verschlüsseln oder ein Token zu signieren. Diese Vertrauensstellungen bestehen aus folgenden Elementen:

- Verbund mit den Identitätsanbietern [OAuth1](oauth1-technical-profile.md#cryptographic-keys), [OAuth2](oauth2-technical-profile.md#cryptographic-keys) und [SAML](identity-provider-generic-saml.md)
- Schutz der Verbindung mit [Rest-API-Diensten](secure-rest-api.md)
- Signierung und Verschlüsselung der [JWT](jwt-issuer-technical-profile.md#cryptographic-keys) und [SAML](saml-service-provider.md)-Token

Das **CryptographicKeys**-Element enthält das folgende Element:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| Schlüssel | 1:n | Ein kryptografischer Schlüssel, der in diesem technischen Profil verwendet wird. |

### <a name="key"></a>Schlüssel

Das **Key**-Element enthält das folgende Attribut:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| Id | Nein | Ein eindeutiger Bezeichner eines bestimmten Schlüsselpaars, auf das von anderen Elementen in der Richtliniendatei verwiesen wird. |
| StorageReferenceId | Ja | Ein Bezeichner eines Speicherschlüsselcontainers, auf den von anderen Elementen in der Richtliniendatei verwiesen wird. |

## <a name="input-claims-transformations"></a>Eingabeanspruchstransformationen

Das **InputClaimsTransformations**-Element kann eine Sammlung von Transformationselementen von Eingabeansprüchen enthalten, die zum Ändern von Eingabeansprüchen oder zum Generieren eines neuen Eingabeanspruchs verwendet werden.

Bei den Ausgabeansprüchen einer vorherigen Anspruchstransformation in der Sammlung der Anspruchstransformationen kann es sich um Eingabeansprüche einer nachfolgenden Eingabeanspruchstransformation handeln. Auf diese Weise können Sie eine Sequenz von Anspruchstransformationen verwenden, die voneinander abhängig sind.

Das **InputClaimsTransformations**-Element enthält das folgende Element:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| InputClaimsTransformation | 1:n | Der Bezeichner einer Anspruchstransformation, die ausgeführt werden soll, bevor Ansprüche an den Anspruchsanbieter oder die vertrauende Seite gesendet werden. Eine Anspruchstransformation kann verwendet werden, um vorhandene ClaimsSchema-Ansprüche zu ändern oder neue zu generieren. |

### <a name="inputclaimstransformation"></a>InputClaimsTransformation

Das **InputClaimsTransformation**-Element enthält das folgende Attribut:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| ReferenceId | Ja | Ein Bezeichner einer Anspruchstransformation, die bereits in der Richtliniendatei oder der übergeordneten Richtliniendatei definiert ist. |

Die folgenden technischen Profile beziehen sich auf die Anspruchstransformation **CreateOtherMailsFromEmail**. Die Anspruchstransformation fügt der `otherMails`-Sammlung den Wert des `email`-Anspruchs hinzu, bevor die Daten dauerhaft im Verzeichnis gespeichert werden.

```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
  ...
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="CreateOtherMailsFromEmail" />
  </InputClaimsTransformations>
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="alternativeSecurityId" />
    <PersistedClaim ClaimTypeReferenceId="userPrincipalName" />
    <PersistedClaim ClaimTypeReferenceId="mailNickName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="otherMails" />
    <PersistedClaim ClaimTypeReferenceId="givenName" />
    <PersistedClaim ClaimTypeReferenceId="surname" />
  </PersistedClaims>
  ...
</TechnicalProfile>
```

## <a name="input-claims"></a>Eingabeansprüche

Das Element **InputClaims** entnimmt dem Anspruchsbehälter Ansprüche, die für das technische Profil verwendet werden. Ein [selbstbestätigtes technisches Profil](self-asserted-technical-profile.md) verwendet beispielsweise die Eingabeansprüche, um die Ausgabeansprüche im Vorhinein auszufüllen, die der Benutzer angibt. Ein technisches REST-API-Profil verwendet die Eingabeansprüche, um Eingabeparameter an den REST-API-Endpunkt zu senden. Azure AD verwendet einen Eingabeanspruch als eindeutigen Bezeichner zum Lesen, Aktualisieren oder Löschen eines Kontos.

Das **InputClaims**-Element enthält das folgende Element:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| InputClaim | 1:n | Ein erwarteter Eingabeanspruchstyp. |

### <a name="inputclaim"></a>InputClaim

Das **InputClaim**-Element enthält die folgenden Attribute:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Ja | Der Bezeichner eines Anspruchstyps. Der Anspruch ist bereits im Abschnitt für das Anspruchsschema in der Richtliniendatei oder der übergeordneten Richtliniendatei definiert. |
| DefaultValue | Nein | Ein Standardwert, der verwendet wird, um einen Anspruch zu erstellen, wenn der von ClaimTypeReferenceId angegebene Anspruch nicht vorhanden ist, sodass der resultierende Anspruch vom technischen Profil als InputClaim-Element verwendet werden kann. |
| PartnerClaimType | Nein | Der Bezeichner des Anspruchstyps des externen Partners, dem der angegebene Anspruchstyp der Richtlinie zugeordnet ist. Wenn das PartnerClaimType-Attribut nicht angegeben wurde, wird der angegebene Anspruchstyp der Richtlinie dem Partneranspruchstyp mit dem gleichen Namen zugeordnet. Verwenden Sie diese Eigenschaft, wenn sich der Name des Anspruchstyps vom dem der anderen Partei unterscheidet. Der erste Anspruchsname lautet beispielsweise *givenName*, während der Partner einen Anspruch mit dem Namen *first_name* verwendet. |

## <a name="display-claims"></a>Anzeigeansprüche

Das **DisplayClaims**-Element enthält eine Liste mit anzuzeigenden Ansprüchen zum Erfassen von Daten vom Benutzer. In die Sammlung der Anzeigeansprüche können Sie einen Verweis auf einen [Anspruchstyp](claimsschema.md) oder ein von Ihnen erstelltes [Anzeigesteuerelement](display-controls.md) einbeziehen.

- Ein Anspruchstyp ist ein Verweis auf einen Anspruch, der auf dem Bildschirm angezeigt werden soll.
  - Um den Benutzer zur Eingabe eines Werts für einen bestimmten Anspruch zu zwingen, legen Sie das Attribut **Required** des **DisplayClaim**-Elements auf `true` fest.
  - Um Anzeigeansprüche vorab mit Werten aufzufüllen, verwenden Sie die zuvor beschriebenen Eingabeansprüche. Das Element kann darüber hinaus auch einen Standardwert enthalten.
  - Das **ClaimType**-Element in der **DisplayClaims**-Sammlung muss das **UserInputType**-Element auf einen von Azure AD B2C unterstützten Benutzereingabetyp festlegen. Bespiele sind `TextBox` oder `DropdownSingleSelect`.
- Ein Anzeigesteuerelement ist ein Benutzeroberflächenelement mit spezieller Funktionalität, das mit dem Azure AD B2C-Back-End-Dienst interagiert. Es ermöglicht dem Benutzer das Durchführen von Aktionen auf der Seite, die ein technisches Validierungsprofil auf dem Back-End aufrufen. Beispiel: Überprüfen einer E-Mail-Adresse, Telefonnummer oder Kundentreuenummer.

Die Reihenfolge der Elemente in **DisplayClaims** bestimmt die Reihenfolge, in der Azure AD B2C die Ansprüche auf dem Bildschirm rendert.

Das **DisplayClaims**-Element enthält das folgende Element:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| DisplayClaim | 1:n | Ein erwarteter Eingabeanspruchstyp. |

### <a name="displayclaim"></a>DisplayClaim

Das **DisplayClaim**-Element enthält die folgenden Attribute:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Nein | Der Bezeichner eines Anspruchstyps, der bereits im ClaimsSchema-Abschnitt der Richtliniendatei oder der übergeordneten Richtliniendatei definiert ist. |
| DisplayControlReferenceId | Nein | Der Bezeichner eines [Anzeigesteuerelements](display-controls.md), der bereits im ClaimsSchema-Abschnitt der Richtliniendatei oder der übergeordneten Richtliniendatei definiert ist. |
| Erforderlich | Nein | Gibt an, ob der Anzeigeanspruch erforderlich ist. |

Das folgende Beispiel veranschaulicht die Verwendung von Anzeigeansprüchen und -steuerelementen in einem selbstbestätigten technischen Profil.

![Screenshot: selbstbestätigtes technisches Profil mit Anzeigeansprüchen](./media/technical-profiles/display-claims.png)

Für das folgende technische Profil gilt:

- Der erste Anzeigeanspruch verweist auf das `emailVerificationControl`-Anzeigesteuerelement, das die E-Mail-Adresse erfasst und überprüft.
- Der fünfte Anzeigeanspruch verweist auf das `phoneVerificationControl`-Anzeigesteuerelement, das eine Telefonnummer erfasst und überprüft.
- Bei den anderen Anzeigeansprüchen handelt es sich um ClaimTypes-Elemente, die vom Benutzer erfasst werden sollen.

```xml
<TechnicalProfile Id="Id">
  <DisplayClaims>
    <DisplayClaim DisplayControlReferenceId="emailVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="displayName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="givenName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="surName" Required="true" />
    <DisplayClaim DisplayControlReferenceId="phoneVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="newPassword" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
  </DisplayClaims>
</TechnicalProfile>
```

## <a name="persisted-claims"></a>Persistente Ansprüche

Das Element **PersistedClaims** enthält alle Werte, die vom [technischen Azure AD-Profil](active-directory-technical-profile.md) dauerhaft gespeichert werden sollen, sowie mögliche Zuordnungsinformationen zwischen einem Anspruchstyp, der bereits im Abschnitt [ClaimsSchema](claimsschema.md) in der Richtlinie definiert ist, und dem Azure AD-Attributnamen.

Der Name des Anspruchs ist der Name des [Azure AD-Attributs](user-profile-attributes.md), sofern nicht das **PartnerClaimType**-Attribut angegeben wurde, das den Namen des Azure AD-Attributs enthält.

Das **PersistedClaims**-Element enthält die folgenden Elemente:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| PersistedClaim | 1:n | Der Anspruchstyp, der beibehalten werden soll. |

### <a name="persistedclaim"></a>PersistedClaim

Das **PersistedClaim**-Element enthält die folgenden Attribute:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Ja | Der Bezeichner eines Anspruchstyps, der bereits im ClaimsSchema-Abschnitt der Richtliniendatei oder der übergeordneten Richtliniendatei definiert ist. |
| DefaultValue | Nein | Ein Standardwert, der zum Erstellen eines Anspruchs verwendet wird, wenn der Anspruch nicht vorhanden ist |
| PartnerClaimType | Nein | Der Bezeichner des Anspruchstyps des externen Partners, dem der angegebene Anspruchstyp der Richtlinie zugeordnet ist. Wenn das PartnerClaimType-Attribut nicht angegeben wurde, wird der angegebene Anspruchstyp der Richtlinie dem Partneranspruchstyp mit dem gleichen Namen zugeordnet. Verwenden Sie diese Eigenschaft, wenn sich der Name des Anspruchstyps vom dem der anderen Partei unterscheidet. Der erste Anspruchsname lautet beispielsweise *givenName*, während der Partner einen Anspruch mit dem Namen *first_name* verwendet. |

Im folgenden Beispiel werden die folgenden Ansprüche durch das technische Profil **AAD-UserWriteUsingLogonEmail** oder das [Startpaket](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/SocialAndLocalAccounts) dauerhaft gespeichert, das ein neues lokales Konto erstellt:

```xml
<PersistedClaims>
  <PersistedClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" />
  <PersistedClaim ClaimTypeReferenceId="newPassword" PartnerClaimType="password"/>
  <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
  <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration" />
  <PersistedClaim ClaimTypeReferenceId="givenName" />
  <PersistedClaim ClaimTypeReferenceId="surname" />
</PersistedClaims>
```

## <a name="output-claims"></a>Ausgabeansprüche

Das Element **OutputClaims** ist die Sammlung mit Ansprüchen, die nach Abschluss des technischen Profils an den Anspruchsbehälter zurückgegeben werden. Sie können diese Ansprüche im nächsten Orchestrierungsschritt oder Transformationen von Ausgabeansprüchen verwenden. Das **OutputClaims**-Element enthält das folgende Element:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| OutputClaim | 1:n | Ein erwarteter Ausgabeanspruchstyp. |

### <a name="outputclaim"></a>OutputClaim

Das **OutputClaim**-Element enthält die folgenden Attribute:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Ja | Der Bezeichner eines Anspruchstyps, der bereits im ClaimsSchema-Abschnitt der Richtliniendatei oder der übergeordneten Richtliniendatei definiert ist. |
| DefaultValue | Nein | Ein Standardwert, der zum Erstellen eines Anspruchs verwendet wird, wenn der Anspruch nicht vorhanden ist |
|AlwaysUseDefaultValue |Nein |Erzwingt die Verwendung des Standardwerts |
| PartnerClaimType | Nein | Der Bezeichner des Anspruchstyps des externen Partners, dem der angegebene Anspruchstyp der Richtlinie zugeordnet ist. Wenn das Attribut PartnerClaimType nicht angegeben wurde, wird der angegebene Anspruchstyp der Richtlinie dem Partneranspruchstyp mit demselben Namen zugeordnet. Verwenden Sie diese Eigenschaft, wenn sich der Name des Anspruchstyps vom dem der anderen Partei unterscheidet. Der erste Anspruchsname lautet beispielsweise *givenName*, während der Partner einen Anspruch mit dem Namen *first_name* verwendet. |

## <a name="output-claims-transformations"></a>Transformationen von Ausgabeansprüchen

Das Element **OutputClaimsTransformations** kann eine Sammlung von **OutputClaimsTransformation**-Elementen enthalten. Die Transformationen von Ausgabeansprüchen werden zum Ändern der Ausgabeansprüche oder zum Generieren neuer Ansprüche verwendet. Nach der Ausführung werden die Ausgabeansprüche wieder im Anspruchsbehälter abgelegt. Sie können diese Ansprüche im nächsten Orchestrierungsschritt verwenden.

Bei den Ausgabeansprüchen einer vorherigen Anspruchstransformation in der Sammlung der Anspruchstransformationen kann es sich um Eingabeansprüche einer nachfolgenden Eingabeanspruchstransformation handeln. Auf diese Weise können Sie eine Sequenz von Anspruchstransformationen verwenden, die voneinander abhängig sind.

Das **OutputClaimsTransformations**-Element enthält das folgende Element:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| OutputClaimsTransformation | 1:n | Die Bezeichner von Anspruchstransformationen, die ausgeführt werden sollen, bevor Ansprüche an den Anspruchsanbieter oder die vertrauende Seite gesendet werden. Eine Anspruchstransformation kann verwendet werden, um vorhandene ClaimsSchema-Ansprüche zu ändern oder neue zu generieren. |

### <a name="outputclaimstransformation"></a>OutputClaimsTransformation

Das **OutputClaimsTransformation**-Element enthält das folgende Attribut:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| ReferenceId | Ja | Ein Bezeichner einer Anspruchstransformation, die bereits in der Richtliniendatei oder der übergeordneten Richtliniendatei definiert ist. |

Das folgende technische Profil verweist auf die Anspruchstransformation „AssertAccountEnabledIsTrue“, um nach dem Auslesen des `accountEnabled`-Anspruchs aus dem Verzeichnis zu bewerten, ob das Konto aktiviert ist oder nicht.

```xml
<TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
  ...
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="accountEnabled" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
  </OutputClaims>
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertAccountEnabledIsTrue" />
  </OutputClaimsTransformations>
  ...
</TechnicalProfile>
```

## <a name="validation-technical-profiles"></a>Verwenden von technischen Validierungsprofilen

Ein technisches Validierungsprofil wird zum Überprüfen von Ausgabeansprüchen in einem [selbstbestätigten technischen Profil](self-asserted-technical-profile.md#validation-technical-profiles) verwendet. Ein technisches Validierungsprofil ist ein ganz einfaches technisches Profil aus jedem beliebigen Protokoll, z. B. [Azure AD](active-directory-technical-profile.md) oder eine [REST-API](restful-technical-profile.md). Das technische Validierungsprofil gibt Ausgabeansprüche oder einen Fehlercode zurück. Die Fehlermeldung wird dem Benutzer auf dem Bildschirm angezeigt, damit er den Vorgang wiederholen kann.

Im folgenden Diagramm wird veranschaulicht, wie Azure AD B2C die Benutzeranmeldeinformationen mit einem technischen Validierungsprofil überprüft.

![Diagramm: Validierungsflow für ein technisches Profil](./media/technical-profiles/validation-technical-profile.png)

Das **ValidationTechnicalProfiles**-Element enthält das folgende Element:

| Element | Vorkommen | BESCHREIBUNG |
| ------- | ----------- | ----------- |
| ValidationTechnicalProfile | 1:n | Die Bezeichner von technischen Profilen, mit denen einige oder alle Ausgabeansprüche des verweisenden technischen Profils überprüft werden. Alle Eingabeansprüche des technischen Profils, auf das verwiesen wird, müssen in den Ausgabeansprüchen des verweisenden technischen Profils enthalten sein. |

### <a name="validationtechnicalprofile"></a>ValidationTechnicalProfile

Das **ValidationTechnicalProfile**-Element enthält das folgende Attribut:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| ReferenceId | Ja | Ein Bezeichner eines technischen Profils, das bereits in der Richtliniendatei oder der übergeordneten Richtliniendatei definiert ist. |

## <a name="subjectnaminginfo"></a>SubjectNamingInfo

Das Element **SubjectNamingInfo** definiert den Antragstellernamen, der in einer [Richtlinie für die vertrauende Seite](relyingparty.md#subjectnaminginfo) in Token verwendet wird. Das **SubjectNamingInfo**-Element enthält das folgende Attribut:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| ClaimType | Ja | Ein Bezeichner eines Anspruchstyps, der bereits im ClaimsSchema-Abschnitt der Richtliniendatei definiert ist. |

## <a name="include-technical-profile"></a>Einschließen eines technischen Profils

Ein technisches Profil kann ein anderes technisches Profil enthalten, um Einstellungen zu ändern oder neue Funktionalität hinzuzufügen. Das Element **IncludeTechnicalProfile** ist ein Verweis auf das allgemeine technische Profil, von dem ein technisches Profil abgeleitet wird. Um die Redundanz und Komplexität von Richtlinienelementen zu verringern, schließen Sie Profile in andere Profile ein, wenn Sie über mehrere technische Profile mit den gleichen Kernelementen verfügen. Verwenden Sie ein allgemeines technisches Profil mit einem gemeinsamen Konfigurationssatz sowie technische Profile für bestimmte Aufgaben, die das allgemeine Profil einschließen.

Angenommen, Sie verfügen über ein [technisches REST-API-Profil](restful-technical-profile.md) mit einem einzigen Endpunkt, an den Sie verschiedene Ansprüche für unterschiedliche Szenarios senden müssen. Erstellen Sie ein allgemeines technisches Profil mit den gemeinsamen Funktionen wie REST-API-Endpunkt-URL, Metadaten, Authentifizierungstyp und Kryptografieschlüsseln. Erstellen Sie spezielle technische Aufgabenprofile, die das allgemeine technische Profil enthalten. Fügen Sie dann die Eingabe- und Ausgabeansprüche hinzu, oder überschreiben Sie den REST-API-Endpunkt-URI, der für dieses technische Profil relevant ist.

Das **IncludeTechnicalProfile**-Element enthält das folgende Attribut:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| ReferenceId | Ja | Ein Bezeichner eines technischen Profils, das bereits in der Richtliniendatei oder der übergeordneten Richtliniendatei definiert ist. |

Das folgende Beispiel veranschaulicht das Einschließen von Profilen:

- **REST-API-Common:** ein allgemeines technisches Profil mit der Basiskonfiguration
- **REST-ValidateProfile:** schließt das technische Profil **REST-API-Common** ein und gibt die Eingabe- und Ausgabeansprüche an
- **REST-UpdateProfile:** schließt das technische Profil **REST-API-Common** ein, gibt die Eingabe- und Ausgabeansprüche an und überschreibt die `ServiceUrl`-Metadaten

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-API-Common">
      <DisplayName>Base REST API configuration</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity</Item>
        <Item Key="AuthenticationType">Basic</Item>
        <Item Key="SendClaimsIn">Body</Item>
      </Metadata>
      <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
      </CryptographicKeys>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>

    <TechnicalProfile Id="REST-ValidateProfile">
      <DisplayName>Validate the account and return promo code</DisplayName>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" />
        <InputClaim ClaimTypeReferenceId="email" />
        <InputClaim ClaimTypeReferenceId="userLanguage" PartnerClaimType="lang" DefaultValue="{Culture:LCID}" AlwaysUseDefaultValue="true" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="promoCode" />
      </OutputClaims>
      <IncludeTechnicalProfile ReferenceId="REST-API-Common" />
    </TechnicalProfile>

    <TechnicalProfile Id="REST-UpdateProfile">
      <DisplayName>Update the user profile</DisplayName>  
      <Metadata>
        <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/update</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" />
        <InputClaim ClaimTypeReferenceId="email" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="REST-API-Common" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

### <a name="multilevel-inclusion"></a>Einschluss auf mehreren Ebenen

Ein technisches Profil kann ein einzelnes technisches Profil einschließen. Die Anzahl von Einschlussebenen ist nicht begrenzt. Das technische Profil **AAD-UserReadUsingAlternativeSecurityId-NoError** enthält z. B. das Profil **AAD-UserReadUsingAlternativeSecurityId**. Dieses technische Profil legt das Metadatenelement `RaiseErrorIfClaimsPrincipalDoesNotExist` auf `true` fest und löst einen Fehler aus, wenn ein Social Media-Konto nicht im Verzeichnis vorhanden ist. **AAD-UserReadUsingAlternativeSecurityId-NoError** setzt dieses Verhalten außer Kraft und deaktiviert die Fehlermeldung.

```xml
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
  <Metadata>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">false</Item>
  </Metadata>
  <IncludeTechnicalProfile ReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
</TechnicalProfile>
```

**AAD-UserReadUsingAlternativeSecurityId** enthält das technische Profil `AAD-Common`.

```xml
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">Read</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">User does not exist. Please sign up before you can sign in.</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId" PartnerClaimType="alternativeSecurityId" Required="true" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surname" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

**AAD-UserReadUsingAlternativeSecurityId-NoError** und **AAD-UserReadUsingAlternativeSecurityId** geben das erforderliche **Protocol**-Element nicht an, weil es im technischen Profil **AAD-Common** angegeben ist.

```xml
<TechnicalProfile Id="AAD-Common">
  <DisplayName>Azure Active Directory</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
</TechnicalProfile>
```

## <a name="use-technical-profile-for-session-management"></a>Verwenden eines technischen Profils für die Sitzungsverwaltung

Das Element **UseTechnicalProfileForSessionManagement** verweist auf die [SSO-Sitzung für das technische Profil](custom-policy-reference-sso.md). Das **UseTechnicalProfileForSessionManagement**-Element enthält das folgende Attribut:

| attribute | Erforderlich | BESCHREIBUNG |
| --------- | -------- | ----------- |
| ReferenceId | Ja | Ein Bezeichner eines technischen Profils, das bereits in der Richtliniendatei oder der übergeordneten Richtliniendatei definiert ist. |

## <a name="enabled-for-user-journeys"></a>Aktiviert für User Journeys

Das [ClaimsProviderSelections](userjourneys.md#claims-provider-selection)-Element in einer User Journey definiert die Liste der Auswahloptionen für Anspruchsanbieter und deren Reihenfolge. Mit dem Element **EnabledForUserJourneys** filtern Sie, welcher Anspruchsanbieter für den Benutzer verfügbar ist. Das **EnabledForUserJourneys**-Element enthält einen der folgenden Werte:

- **Immer:** Das technische Profil wird ausgeführt.
- **Nie:** Das technische Profil wird übersprungen.
- **OnClaimsExistence:** Die Ausführung erfolgt nur, wenn ein angegebener Anspruch im technischen Profil vorhanden ist.
- **OnItemExistenceInStringCollectionClaim:** Die Ausführung erfolgt nur, wenn ein Element in einem Anspruch mit einer Zeichenfolgensammlung vorhanden ist.
- **OnItemAbsenceInStringCollectionClaim:** Die Ausführung erfolgt nur, wenn ein Element in einem Anspruch mit einer Zeichenfolgensammlung nicht enthalten ist.

Bei Verwendung von **OnClaimsExistence**, **OnItemExistenceInStringCollectionClaim** oder **OnItemAbsenceInStringCollectionClaim** müssen Sie die folgenden Metadaten angeben:

- **ClaimTypeOnWhichToEnable:** gibt den Anspruchstyp zurück, der bewertet werden soll
- **ClaimValueOnWhichToEnable**: gibt den Wert zurück, der verglichen werden soll

Das folgende technische Profil wird nur ausgeführt, wenn die **identityProviders**-Zeichenfolgensammlung den Wert `facebook.com` enthält:

```xml
<TechnicalProfile Id="UnLink-Facebook-OAUTH">
  <DisplayName>Unlink Facebook</DisplayName>
...
    <Metadata>
      <Item Key="ClaimTypeOnWhichToEnable">identityProviders</Item>
      <Item Key="ClaimValueOnWhichToEnable">facebook.com</Item>
    </Metadata>
...
  <EnabledForUserJourneys>OnItemExistenceInStringCollectionClaim</EnabledForUserJourneys>
</TechnicalProfile>
```
