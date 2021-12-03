---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Firmex VDR | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Firmex VDR konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/21/2020
ms.author: jeedes
ms.openlocfilehash: 9454456fb366b5c97f69f16cbc20b1739f8cfb32
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132334300"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-firmex-vdr"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Firmex VDR

In diesem Tutorial wird beschrieben, wie Sie Firmex VDR in Azure Active Directory (Azure AD) integrieren. Die Integration von Firmex VDR in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Firmex VDR hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Firmex VDR anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Firmex VDR-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Firmex VDR unterstützt **SP- und IDP**-initiiertes einmaliges Anmelden.

* Nach dem Konfigurieren von Firmex VDR können Sie Sitzungssteuerungen erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützen. Sitzungssteuerungen basieren auf bedingtem Zugriff. [Erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Defender for Cloud Apps erzwingen.](/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-firmex-vdr-from-the-gallery"></a>Hinzufügen von Firmex VDR aus dem Katalog

Zum Konfigurieren der Integration von Firmex VDR in Azure AD müssen Sie Firmex VDR aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Firmex VDR** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Firmex VDR** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-single-sign-on-for-firmex-vdr"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Firmex VDR

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Firmex VDR mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Firmex VDR eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Firmex VDR die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    * **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    * **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Firmex VDR](#configure-firmex-vdr-sso)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
    * **[Erstellen eines Firmex VDR-Testbenutzers](#create-firmex-vdr-test-user)**, um eine Entsprechung von B. Simon in Firmex VDR zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Firmex VDR** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Im Abschnitt **Grundlegende SAML-Konfiguration** muss der Benutzer keine Schritte ausführen, weil die App bereits in Azure integriert ist.

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** die URL ein: `https://login.firmex.com`.

1. Klicken Sie auf **Speichern**.

1. Die Firmex VDR-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute.

    ![image](common/default-attributes.png)

1. Darüber hinaus wird von der Firmex VDR-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden (siehe unten). Diese Attribute werden ebenfalls vorab aufgefüllt, Sie können sie jedoch nach Bedarf überprüfen.

    | Name | Quellattribut|
    | ------------ | --------- |
    | email | user.mail |

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **Firmex VDR einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie diesem Benutzer Zugriff auf Firmex VDR gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste den Eintrag **Firmex VDR** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-firmex-vdr-sso"></a>Konfigurieren des einmaligen Anmeldens für Firmex VDR

### <a name="before-you-get-started"></a>Bevor Sie beginnen

#### <a name="what-youll-need"></a>Voraussetzungen

-   Aktives Firmex-Abonnement
-   Azure AD als SSO-Dienst
-   SSO-Konfiguration durch Ihren IT-Administrator
-   Nachdem SSO aktiviert wurde, müssen sich alle Benutzer Ihres Unternehmens per SSO bei Firmex anmelden. Die Anmeldung per Benutzername und Kennwort wird nicht mehr verwendet.

#### <a name="how-long-will-this-take"></a>Wie lange dauert dieser Vorgang?

Das Implementieren von SSO dauert einige Minuten. Es kommt praktisch zu keinen Ausfallzeiten zwischen dem Zeitpunkt, zu dem der Firmex-Support SSO für Ihre Site aktiviert, und dem Beginn der SSO-Authentifizierung durch die Benutzer Ihres Unternehmens. Führen Sie einfach die unten angegebenen Schritte aus.

### <a name="step-1---identify-your-companys-domains"></a>Schritt 1: Identifizieren der Domänen Ihres Unternehmens

Identifizieren Sie die Domänen, über die sich die Benutzer Ihres Unternehmens anmelden.

Beispiel:

- @firmex.com
- @firmex.ca

### <a name="step-2---contact-firmex-support-with-your-domains"></a>Schritt 2: Bereitstellen Ihrer Domänen für den Firmex-Support

Senden Sie eine E-Mail an das [Firmex-Supportteam](mailto:support@firmex.com), oder wenden Sie sich unter 1888 688 4042 x.11 telefonisch an den Firmex-Support. Geben Sie die Informationen zu Ihren Domänen an. Vom Firmex-Support werden die Domänen VDR als **in Anspruch genommene Domänen** (Claimed Domains) hinzugefügt. Ihr Administrator muss nun das einmalige Anmelden (SSO) konfigurieren.

Warnung: Die Benutzer Ihres Unternehmens können die Anmeldung bei VDR erst durchführen, nachdem Ihr Websiteadministrator die in Anspruch genommenen Domänen konfiguriert hat. Andere Benutzer (z. B. Gastbenutzer) können sich weiterhin per E-Mail-Adresse und Kennwort anmelden. Die Konfiguration dauert einige Minuten.

### <a name="step-3---configure-the-claimed-domains"></a>Schritt 3: Konfigurieren der in Anspruch genommenen Domänen

1. Melden Sie sich als Websiteadministrator bei Firmex an.
1. Klicken Sie oben links auf das Logo Ihres Unternehmens.
1. Wählen Sie die Registerkarte **SSO** aus. Wählen Sie anschließend **SSO Configuration** (SSO-Konfiguration) aus. Klicken Sie auf die Domäne, die Sie konfigurieren möchten.

    ![In Anspruch genommene Domänen](./media/firmex-vdr-tutorial/edit-sso.png)  

1. Lassen Sie von Ihrem IT-Administrator die folgenden Felder ausfüllen. Die Einträge der Felder sollten von Ihrem Identitätsanbieter übernommen werden:  

    ![SSO Configuration](./media/firmex-vdr-tutorial/SSO-config.png)

    a. Fügen Sie in das Textfeld **Entity ID** (Entitäts-ID) den Wert für den **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie in das Textfeld **Identity Provider URL** (Identitätsanbieter-URL) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. **Öffentliches Schlüsselzertifikat**: Eine SAML-Nachricht kann vom Aussteller zu Authentifizierungszwecken digital signiert werden. Zum Überprüfen der Signatur der Nachricht verwendet der Empfänger einen öffentlichen Schlüssel, der im Besitz des Ausstellers ist. Ebenso gilt Folgendes: Zum Verschlüsseln einer Nachricht muss der Aussteller den öffentlichen Verschlüsselungsschlüssel kennen, der dem letztendlichen Empfänger gehört. In beiden Fällen – Signierung und Verschlüsselung – müssen im Voraus vertrauenswürdige öffentliche Schlüssel ausgetauscht werden.  Dies ist das **X509Certificate** aus der **Verbundmetadaten-XML**-Datei.

    d. Klicken Sie auf **Speichern**, um die SSO-Konfiguration abzuschließen. Änderungen sind sofort wirksam.

1. SSO ist jetzt für Ihre Website aktiviert.

### <a name="create-firmex-vdr-test-user"></a>Erstellen eines Firmex VDR-Testbenutzers

In diesem Abschnitt erstellen Sie in Firmex einen Benutzer mit dem Namen „B. Simon“. Arbeiten Sie mit dem [Firmex-Supportteam](mailto:support@firmex.com) zusammen, um die Benutzer der Firmex-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Firmex VDR“ klicken, sollten Sie automatisch bei Ihrer Firmex VDR-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](./tutorial-list.md)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Was ist der bedingte Zugriff in Azure Active Directory?](../conditional-access/overview.md)

- [Verwenden von Firmex VDR mit Azure AD](https://aad.portal.azure.com/)

- [Was ist die Sitzungssteuerung in Microsoft Defender for Cloud Apps?](/cloud-app-security/proxy-intro-aad)

- [Schützen von Apps mit Microsoft Cloud App Security Conditional Access App Control](/cloud-app-security/proxy-intro-aad)
