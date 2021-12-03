---
title: Einrichten eines Flows für Kennwortanmeldeinformationen von Ressourcenbesitzern
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie den Flow für Kennwortanmeldeinformationen von Ressourcenbesitzern in Azure Active Directory B2C einrichten.
services: active-directory-b2c
author: kengaderdus
manager: CelesteDG
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/12/2021
ms.custom: project-no-code
ms.author: kengaderdus
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 0acca4a87ea72157a11862ec448483008af87526
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131008224"
---
# <a name="set-up-a-resource-owner-password-credentials-flow-in-azure-active-directory-b2c"></a>Einrichten eines Flows für Kennwortanmeldeinformationen von Ressourcenbesitzern in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

In Azure Active Directory B2C (Azure AD B2C) ist der Flow für Kennwortanmeldeinformationen des Ressourcenbesitzers (Resource Owner Password Credentials, ROPC) ein OAuth-Standardauthentifizierungsflow. Bei diesem Flow tauscht eine Anwendung, die auch als die vertrauende Seite bezeichnet wird, gültige Anmeldeinformationen gegen Token aus. Die Anmeldeinformationen enthalten eine Benutzer-ID und ein Kennwort. Die zurückgegebenen Token sind ein ID-Token, ein Zugriffstoken und ein Aktualisierungstoken.

## <a name="ropc-flow-notes"></a>Hinweise zum ROPC-Flow

In Azure Active Directory B2C (Azure AD B2C) werden die folgenden Optionen unterstützt:

- **Nativer Client**: Die Benutzerinteraktion während der Authentifizierung erfolgt, wenn Code auf einem Gerät auf Benutzerseite ausgeführt wird. Bei dem Gerät kann es sich um eine mobile Anwendung handeln, die unter einem nativen Betriebssystem wie Android oder iOS ausgeführt wird.
- **Flow für öffentlichen Client**: Im API-Aufruf werden nur die von einer Anwendung gesammelten Benutzeranmeldeinformationen gesendet. Die Anmeldeinformationen der Anwendung werden nicht gesendet.
- **Neue Ansprüche hinzufügen**: Der Inhalt des ID-Tokens kann geändert werden, um neue Ansprüche hinzuzufügen.

Folgende Flüsse werden nicht unterstützt:

- **Server-zu-Server**: Das Identitätsschutzsystem benötigt eine zuverlässige IP-Adresse, die vom Aufrufer (dem nativen Client) im Rahmen der Interaktion erfasst wurde. Bei einem serverseitigen API-Aufruf wird nur die IP-Adresse des Servers verwendet. Wenn der dynamische Schwellenwert einer fehlgeschlagenen Authentifizierung überschritten wird, identifiziert das System für den Identitätsschutz eine wiederholt auftretende IP-Adresse als Angreifer.
- **Flow für vertraulichen Client**: Die Anwendungsclient-ID wird überprüft, der geheime Anwendungsschlüssel wird jedoch nicht überprüft.

Beachten Sie bei der Verwendung des ROPC-Flows Folgendes:

- ROPC funktioniert nicht, wenn der Authentifizierungsflow für eine Benutzerinteraktion unterbrochen wird. Zum Beispiel, wenn ein Kennwort abgelaufen ist oder geändert werden muss, wenn die [mehrstufige Authentifizierung](multi-factor-authentication.md) erforderlich ist, oder wenn bei der Anmeldung weitere Informationen gesammelt werden müssen (z. B. Benutzereinwilligung).
- ROPC unterstützt nur lokale Konten. Benutzer können sich nicht mit [Verbundidentitätsanbietern](add-identity-provider.md) wie Microsoft, Google+, Twitter, AD FS oder Facebook anmelden.
- Die [Sitzungsverwaltung](session-behavior.md), einschließlich [Angemeldet bleiben](session-behavior.md#enable-keep-me-signed-in-kmsi), ist nicht anwendbar.


## <a name="register-an-application"></a>Registrieren einer Anwendung

[!INCLUDE [active-directory-b2c-appreg-ropc](../../includes/active-directory-b2c-appreg-ropc.md)]

::: zone pivot="b2c-user-flow"

##  <a name="create-a-resource-owner-user-flow"></a>Erstellen eines Benutzerflows für Ressourcenbesitzer

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als **globaler Administrator** Ihres Azure AD B2C-Mandanten an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält:
    1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnisse und Abonnements** aus.
    1. Suchen Sie auf der Seite **Portaleinstellungen > Verzeichnisse und Abonnements** das Azure AD B2C-Verzeichnis in der Liste **Verzeichnisname**, und klicken Sie dann auf **Wechseln**.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie **Benutzerflows** und dann **Neuer Benutzerflow** aus.
1. Wählen Sie **Mit Ressourcenbesitzer-Kennwortanmeldeinformationen (ROPC) anmelden** aus.
1. Vergewissern Sie sich, dass unter **Version** die Option **Vorschau** aktiviert ist, und wählen Sie dann **Erstellen** aus.
1. Geben Sie einen Namen für den Benutzerflow an, z. B. *ROPC_Auth*.
1. Klicken Sie unter **Anwendungsansprüche** auf **Mehr anzeigen**.
1. Wählen Sie die Anwendungsansprüche aus, die Sie für Ihre Anwendung benötigen, z. B. „Anzeigename“, „E-Mail-Adresse“ und „Identitätsanbieter“.
1. Wählen Sie **OK** und anschließend **Erstellen**.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="pre-requisite"></a>Voraussetzung 
Wenn Sie dies noch nicht getan haben, informieren Sie sich unter [Erste Schritte mit benutzerdefinierten Richtlinien in Active Directory B2C](tutorial-create-user-flows.md) über das Starter Pack für benutzerdefinierte Richtlinien.

##  <a name="create-a-resource-owner-policy"></a>Erstellen von Richtlinien für Ressourcenbesitzer

1. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*.
2. Falls es nicht bereits vorhanden ist, fügen Sie ein **ClaimsSchema**-Element und seine untergeordneten Elemente als erstes Element unter dem **BuildingBlocks**-Element hinzu:

    ```xml
    <ClaimsSchema>
      <ClaimType Id="logonIdentifier">
        <DisplayName>User name or email address that the user can use to sign in</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="resource">
        <DisplayName>The resource parameter passes to the ROPC endpoint</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="refreshTokenIssuedOnDateTime">
        <DisplayName>An internal parameter used to determine whether the user should be permitted to authenticate again using their existing refresh token.</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="refreshTokensValidFromDateTime">
        <DisplayName>An internal parameter used to determine whether the user should be permitted to authenticate again using their existing refresh token.</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
    </ClaimsSchema>
    ```

3. Fügen Sie dem **BuildingBlocks**-Element hinter dem **ClaimsSchema**-Element ein **ClaimsTransformations**-Element und dessen untergeordnete Elemente hinzu:

    ```xml
    <ClaimsTransformations>
      <ClaimsTransformation Id="CreateSubjectClaimFromObjectID" TransformationMethod="CreateStringClaim">
        <InputParameters>
          <InputParameter Id="value" DataType="string" Value="Not supported currently. Use oid claim." />
        </InputParameters>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="sub" TransformationClaimType="createdClaim" />
        </OutputClaims>
      </ClaimsTransformation>

      <ClaimsTransformation Id="AssertRefreshTokenIssuedLaterThanValidFromDate" TransformationMethod="AssertDateTimeIsGreaterThan">
        <InputClaims>
          <InputClaim ClaimTypeReferenceId="refreshTokenIssuedOnDateTime" TransformationClaimType="leftOperand" />
          <InputClaim ClaimTypeReferenceId="refreshTokensValidFromDateTime" TransformationClaimType="rightOperand" />
        </InputClaims>
        <InputParameters>
          <InputParameter Id="AssertIfEqualTo" DataType="boolean" Value="false" />
          <InputParameter Id="AssertIfRightOperandIsNotPresent" DataType="boolean" Value="true" />
        </InputParameters>
      </ClaimsTransformation>
    </ClaimsTransformations>
    ```

4. Suchen Sie das **ClaimsProvider**-Element, bei dem **DisplayName** den Wert `Local Account SignIn` aufweist, und fügen Sie das folgende technische Profil hinzu:

    ```xml
    <TechnicalProfile Id="ResourceOwnerPasswordCredentials-OAUTH2">
      <DisplayName>Local Account SignIn</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <Metadata>
        <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">We can't seem to find your account</Item>
        <Item Key="UserMessageIfInvalidPassword">Your password is incorrect</Item>
        <Item Key="UserMessageIfOldPasswordUsed">Looks like you used an old password</Item>
        <Item Key="DiscoverMetadataByTokenIssuer">true</Item>
        <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item>
        <Item Key="METADATA">https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration</Item>
        <Item Key="authorization_endpoint">https://login.microsoftonline.com/{tenant}/oauth2/token</Item>
        <Item Key="response_types">id_token</Item>
        <Item Key="response_mode">query</Item>
        <Item Key="scope">email openid</Item>
        <Item Key="grant_type">password</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="logonIdentifier" PartnerClaimType="username" Required="true" DefaultValue="{OIDC:Username}"/>
        <InputClaim ClaimTypeReferenceId="password" Required="true" DefaultValue="{OIDC:Password}" />
        <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="password" />
        <InputClaim ClaimTypeReferenceId="scope" DefaultValue="openid" />
        <InputClaim ClaimTypeReferenceId="nca" PartnerClaimType="nca" DefaultValue="1" />
        <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppId" />
        <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppId" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="oid" />
        <OutputClaim ClaimTypeReferenceId="userPrincipalName" PartnerClaimType="upn" />
      </OutputClaims>
      <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromObjectID" />
      </OutputClaimsTransformations>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>
    ```

    Ersetzen Sie **DefaultValue** von **client_id** durch die Anwendungs-ID der ProxyIdentityExperienceFramework-Anwendung, die Sie im Tutorial unter den Voraussetzungen erstellt haben. Ersetzen Sie **DefaultValue** von **resource_id** durch die Anwendungs-ID der IdentityExperienceFramework-Anwendung, die Sie ebenfalls im Tutorial unter den Voraussetzungen erstellt haben.

5. Fügen Sie die folgenden **ClaimsProvider**-Elemente mit ihren technischen Profilen dem **ClaimsProviders**-Element hinzu:

    ```xml
    <ClaimsProvider>
      <DisplayName>Azure Active Directory</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="AAD-UserReadUsingObjectId-CheckRefreshTokenDate">
          <Metadata>
            <Item Key="Operation">Read</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" />
            <OutputClaim ClaimTypeReferenceId="refreshTokensValidFromDateTime" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="AssertRefreshTokenIssuedLaterThanValidFromDate" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromObjectID" />
          </OutputClaimsTransformations>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

    <ClaimsProvider>
      <DisplayName>Session Management</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SM-RefreshTokenReadAndSetup">
          <DisplayName>Trustframework Policy Engine Refresh Token Setup Technical Profile</DisplayName>
          <Protocol Name="None" />
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" />
            <OutputClaim ClaimTypeReferenceId="refreshTokenIssuedOnDateTime" />
          </OutputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

    <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="JwtIssuer">
          <Metadata>
            <!-- Point to the redeem refresh token user journey-->
            <Item Key="RefreshTokenUserJourneyId">ResourceOwnerPasswordCredentials-RedeemRefreshToken</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

6. Fügen Sie dem **TrustFrameworkPolicy**-Element ein **UserJourneys**-Element und dessen untergeordnete Elemente hinzu:

    ```xml
    <UserJourney Id="ResourceOwnerPasswordCredentials">
      <PreserveOriginalAssertion>false</PreserveOriginalAssertion>
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="ResourceOwnerFlow" TechnicalProfileReferenceId="ResourceOwnerPasswordCredentials-OAUTH2" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
    </UserJourney>
    <UserJourney Id="ResourceOwnerPasswordCredentials-RedeemRefreshToken">
      <PreserveOriginalAssertion>false</PreserveOriginalAssertion>
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="RefreshTokenSetupExchange" TechnicalProfileReferenceId="SM-RefreshTokenReadAndSetup" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="CheckRefreshTokenDateFromAadExchange" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId-CheckRefreshTokenDate" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
    </UserJourney>
    ```

7. Wählen Sie in Ihrem Azure AD B2C-Mandanten auf der Seite **Benutzerdefinierte Richtlinien** die Option **Richtlinie hochladen** aus.
8. Aktivieren Sie **Richtlinie überschreiben, sofern vorhanden**, navigieren Sie dann zur Datei *TrustFrameworkExtensions.xml*, und wählen Sie die Datei aus.
9. Klicken Sie auf **Hochladen**.

## <a name="create-a-relying-party-file"></a>Erstellen einer Datei der vertrauenden Seite

Aktualisieren Sie als Nächstes die Datei der vertrauenden Seite, die die User Journey initiiert, die Sie erstellt haben:

1. Erstellen Sie in Ihrem Arbeitsverzeichnis eine Kopie der Datei *SignUpOrSignIn.xml*, und benennen Sie sie in *ROPC_Auth.xml* um.
2. Öffnen Sie die neue Datei, und ändern Sie den Wert des **PolicyId**-Attributs für **TrustFrameworkPolicy** in einen eindeutigen Wert. Die Richtlinien-ID ist der Name Ihrer Richtlinie. Beispiel: **B2C_1A_ROPC_Auth**.
3. Ändern Sie den Wert des **ReferenceId**-Attributs unter **DefaultUserJourney** in `ResourceOwnerPasswordCredentials`.
4. Ändern Sie das **OutputClaims**-Element so, dass es nur die folgenden Ansprüche enthält:

    ```xml
    <OutputClaim ClaimTypeReferenceId="sub" />
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="displayName" DefaultValue="" />
    <OutputClaim ClaimTypeReferenceId="givenName" DefaultValue="" />
    <OutputClaim ClaimTypeReferenceId="surname" DefaultValue="" />
    ```

5. Wählen Sie in Ihrem Azure AD B2C-Mandanten auf der Seite **Benutzerdefinierte Richtlinien** die Option **Richtlinie hochladen** aus.
6. Aktivieren Sie **Richtlinie überschreiben, sofern vorhanden**, navigieren Sie dann zur Datei *ROPC_Auth.xml*, und wählen Sie sie aus.
7. Klicken Sie auf **Hochladen**.


::: zone-end

## <a name="test-the-ropc-flow"></a>Testen des ROPC-Flows

Verwenden Sie Ihre bevorzugte API-Entwicklungsanwendung, um einen API-Aufruf zu generieren, und überprüfen Sie die Antwort, um Ihre Richtlinie zu debuggen. Erstellen Sie einen Aufruf wie das folgende Beispiel, und verwenden Sie die folgenden Informationen als Hauptteil der POST-Anforderung:

`https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/B2C_1A_ROPC_Auth/oauth2/v2.0/token`

- Ersetzen Sie `<tenant-name>` durch den Namen des Azure AD B2C-Mandanten.
- Ersetzen Sie `B2C_1A_ROPC_Auth` durch den vollständigen Namen der Richtlinie für Kennwortanmeldeinformationen des Ressourcenbesitzers.

| Schlüssel | Wert |
| --- | ----- |
| username | `user-account` |
| password | `password1` |
| grant_type | password |
| scope | openid `application-id` offline_access |
| client_id | `application-id` |
| response_type | token id_token |

- Ersetzen Sie `user-account` durch den Namen eines Benutzerkontos in Ihrem Mandanten.
- Ersetzen Sie `password1` durch das Kennwort des Benutzerkontos.
- Ersetzen Sie `application-id` durch die Anwendungs-ID aus der *ROPC_Auth_app*-Registrierung.
- *Offline_access* ist optional, wenn Sie ein Aktualisierungstoken erhalten möchten.

Die tatsächliche POST-Anforderung sieht wie im folgenden Beispiel aus:

```https
POST /<tenant-name>.onmicrosoft.com/B2C_1A_ROPC_Auth/oauth2/v2.0/token HTTP/1.1
Host: <tenant-name>.b2clogin.com
Content-Type: application/x-www-form-urlencoded

username=contosouser.outlook.com.ws&password=Passxword1&grant_type=password&scope=openid+bef22d56-552f-4a5b-b90a-1988a7d634ce+offline_access&client_id=bef22d56-552f-4a5b-b90a-1988a7d634ce&response_type=token+id_token
```

Eine erfolgreiche Antwort mit Offlinezugriff ähnelt dem folgenden Beispiel:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlki...",
    "token_type": "Bearer",
    "expires_in": "3600",
    "refresh_token": "eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3REVk1EVFBLbUJLb0FUcWQ1ZWFja1hBIiwidmVyIjoiMS4wIiwiemlwIjoiRGVmbGF0ZSIsInNlciI6Ij...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlki..."
}
```

## <a name="redeem-a-refresh-token"></a>Einlösen eines Aktualisierungstokens

Erstellen Sie einen POST-Aufruf ähnlich dem hier gezeigten. Verwenden Sie die Informationen aus der folgenden Tabelle als Hauptteil der Anforderung:

`https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/B2C_1A_ROPC_Auth/oauth2/v2.0/token`

- Ersetzen Sie `<tenant-name>` durch den Namen des Azure AD B2C-Mandanten.
- Ersetzen Sie `B2C_1A_ROPC_Auth` durch den vollständigen Namen der Richtlinie für Kennwortanmeldeinformationen des Ressourcenbesitzers.

| Schlüssel | Wert |
| --- | ----- |
| grant_type | refresh_token |
| response_type | id_token |
| client_id | `application-id` |
| resource | `application-id` |
| refresh_token | `refresh-token` |

- Ersetzen Sie `application-id` durch die Anwendungs-ID aus der *ROPC_Auth_app*-Registrierung.
- Ersetzen Sie `refresh-token` durch das **refresh_token** (Aktualisierungstoken), das in der vorherigen Antwort zurückgesendet wurde.

Eine erfolgreiche Antwort ähnelt dem folgenden Beispiel:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrNHh5b2pORnVtMWtsMll0djhkbE5QNC1jNTdkTzZRR1RWQndhT...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrNHh5b2pORnVtMWtsMll0djhkbE5QNC1jNTdkTzZRR1RWQn...",
    "token_type": "Bearer",
    "not_before": 1533672990,
    "expires_in": 3600,
    "expires_on": 1533676590,
    "resource": "bef2222d56-552f-4a5b-b90a-1988a7d634c3",
    "id_token_expires_in": 3600,
    "profile_info": "eyJ2ZXIiOiIxLjAiLCJ0aWQiOiI1MTZmYzA2NS1mZjM2LTRiOTMtYWE1YS1kNmVlZGE3Y2JhYzgiLCJzdWIiOm51bGwsIm5hbWUiOiJEYXZpZE11IiwicHJlZmVycmVkX3VzZXJuYW1lIjpudWxsLCJpZHAiOiJMb2NhbEFjY291bnQifQ",
    "refresh_token": "eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMCIsInppcCI6IkRlZmxhdGUiLCJzZXIiOiIxLjAi...",
    "refresh_token_expires_in": 1209600
}
```

## <a name="troubleshooting"></a>Problembehandlung

### <a name="the-provided-application-is-not-configured-to-allow-the-oauth-implicit-flow"></a>Die angegebene Anwendung ist nicht zum Zulassen des impliziten Flows „OAuth“ konfiguriert.

* **Symptom:** Sie führen den ROPC-Flow aus und erhalten die folgende Meldung: *AADB2C90057: Die angegebene Anwendung ist nicht zum Zulassen des impliziten Flows „OAuth“ konfiguriert.*
* **Mögliche Ursachen:** Der implizite Flow ist für Ihre Anwendung nicht zulässig.
* **Lösung:** Beim Erstellen der [App-Registrierung](#register-an-application) in Azure AD B2C müssen Sie das Anwendungsmanifest manuell bearbeiten und den Wert der Eigenschaft `oauth2AllowImplicitFlow` auf `true` festlegen. Nach dem Konfigurieren der Eigenschaft `oauth2AllowImplicitFlow` kann es einige Minuten (in der Regel nicht länger als fünf Minuten) dauern, bis die Änderung wirksam wird. 

## <a name="use-a-native-sdk-or-app-auth"></a>Verwenden eines nativen SDK oder einer App-Authentifizierung

Azure AD B2C entspricht den OAuth 2.0-Standards für Kennwortanmeldeinformationen von Ressourcenbesitzern öffentlicher Clients und sollte mit den meisten Client-SDKs kompatibel sein. Die neuesten Informationen finden Sie unter [Natives App-SDK für OAuth 2.0 und OpenID Connect für die Implementierung moderner Best Practices](https://appauth.io/).

## <a name="next-steps"></a>Nächste Schritte

Laden Sie funktionierende Beispiele, die für die Verwendung mit Azure AD B2C konfiguriert sind, von GitHub unter [für Android](https://aka.ms/aadb2cappauthropc) und [für iOS](https://aka.ms/aadb2ciosappauthropc) herunter.
