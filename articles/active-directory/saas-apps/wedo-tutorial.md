---
title: 'Tutorial: Integration des einmaligen Anmeldens von Azure Active Directory (Single Sign-On, SSO) mit WEDO | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und WEDO konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/02/2021
ms.author: jeedes
ms.openlocfilehash: 49bccfa7a5a3cc90038c5068841c75c972616fe2
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132316646"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-wedo"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit WEDO

In diesem Tutorial erfahren Sie, wie Sie WEDO in Azure Active Directory (Azure AD) integrieren. Die Integration von WEDO in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf WEDO hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei WEDO anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein WEDO-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist Ein SSO-Abonnement erhalten Sie vom [Supportteam für den WEDO-Client](mailto:info@wedo.swiss).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* WEDO unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.
* WEDO unterstützt die [automatisierte Benutzerbereitstellung](wedo-provisioning-tutorial.md).

## <a name="add-wedo-from-the-gallery"></a>Hinzufügen von WEDO über den Katalog

Zum Konfigurieren der Integration von WEDO in Azure AD müssen Sie WEDO aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **WEDO** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **WEDO** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-wedo"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für WEDO

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit WEDO mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in WEDO eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit WEDO die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für WEDO](#configure-wedo-sso)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines WEDO-Testbenutzers](#create-wedo-test-user)**, um in WEDO eine Entsprechung von B. Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **WEDO** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten** Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<SUBDOMAIN>.wedo.swiss/sp/acs`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<SUBDOMAIN>.wedo.swiss/sp/acs`

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<SUBDOMAIN>.wedo.swiss/`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem tatsächlichen Bezeichner, der Antwort-URL und der Anmelde-URL aktualisieren. Diese Werte erhalten Sie vom [Supportteam für den WEDO-Client](mailto:info@wedo.swiss). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Die Anwendung WEDO erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute.

    | Name | Quellattribut|
    | ------------ | --------- |
    | email | user.email |
    | firstName | user.firstName |
    | lastName | user.lasttName |
    | userName | user.userName |

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **WEDO einrichten** die entsprechenden URLs basierend auf Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf WEDO gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste die Option **WEDO** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-wedo-sso"></a>Konfigurieren des einmaligen Anmeldens für WEDO

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD in WEDO zu aktivieren:

1. Melden Sie sich bei [WEDO](https://login.wedo.swiss/) an. Ihnen muss die **Administratorrolle** zugewiesen sein.
1. Wählen Sie in den Profileinstellungen im Abschnitt **Network settings** (Netzwerkeinstellungen) das Menü **Authentication** (Authentifizierung) aus.
1. Führen Sie auf der Seite **SAML Authentication** (SAML-Authentifizierung) die folgenden Schritte aus:

   ![Link für die SAML-Authentifizierung](media/wedo-tutorial/network-security-authentification.png)

   a. Aktivieren Sie **SAML Authentication** (SAML-Authentifizierung).

   b. Wählen Sie die Registerkarte **Identity Provider Metadata (XML)** (Metadaten des Identitätsanbieters (XML)) aus.

   c. Öffnen Sie die heruntergeladene **Verbundmetadaten-XML** aus dem Azure-Portal im Editor, kopieren Sie ihren Inhalt, und fügen Sie ihn in das Textfeld **X.509 Certificate** (X.509-Zertifikat) ein.

   d. Klicken Sie auf **Speichern**.

### <a name="create-wedo-test-user"></a>Erstellen eines WEDO-Testbenutzers

In diesem Abschnitt erstellen Sie in WEDO einen Testbenutzer mit dem Namen Bob Simon. Die Informationen müssen mit den Angaben unter **Erstellen eines Azure AD-Testbenutzers** übereinstimmen.

1. Wählen Sie in WEDO in der Profileinstellung im Abschnitt **Network settings** (Netzwerkeinstellungen) die Option **Users** (Benutzer) aus.
1. Klicken Sie auf **Benutzer hinzufügen**.
1. Geben Sie im Popupfenster „Add user“ (Benutzer*in hinzufügen) die Informationen zum Benutzer bzw. zur Benutzerin ein.

    a. Vorname: `B`

    b. Nachname: `Simon`

    c. Geben Sie die E-Mail-Adresse im Format `username@companydomain.extension` ein. Beispiel: `B.Simon@contoso.com`. Die Domäne der E-Mail-Adresse muss dem Kurznamen Ihres Unternehmens entsprechen.

    d. Benutzertyp: `User`

    e. Klicken Sie auf **Benutzer erstellen**.

    f. Klicken Sie auf der Seite **Select teams** (Teams auswählen) auf **Save** (Speichern).

    g.  Klicken Sie auf der Seite **Invite user** (Benutzer einladen) auf **Yes** (Ja).

1. Überprüfen Sie den Benutzer anhand des per E-Mail erhaltenen Links.

> [!NOTE]
> Wenn Sie einen Pseudobenutzer erstellen möchten (die oben genannte E-Mail-Adresse ist in Ihrem Netzwerk nicht vorhanden), wenden Sie sich an den [Support](mailto:info@wedo.swiss), um den Benutzer zu überprüfen*.

> [!NOTE]
> Außerdem unterstützt WEDO die automatische Benutzerbereitstellung. Weitere Informationen zum Konfigurieren der automatischen Benutzerbereitstellung finden Sie [hier](./wedo-provisioning-tutorial.md).

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen: 

#### <a name="sp-initiated"></a>SP-initiiert:

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch werden Sie zur Anmelde-URL für WEDO weitergeleitet, wo Sie den Anmeldeflow initiieren können.  

* Rufen Sie direkt die WEDO-Anmelde-URL auf, und initiieren Sie den Anmeldeflow.

#### <a name="idp-initiated"></a>IDP-initiiert:

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch sollten Sie automatisch bei der WEDO-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. 

Sie können auch den Microsoft-Bereich „Meine Apps“ verwenden, um die Anwendung in einem beliebigen Modus zu testen. Beim Klicken auf die Kachel „WEDO“ unter „Meine Apps“ geschieht Folgendes: Wenn Sie die Anwendung im SP-Modus konfiguriert haben, werden Sie zum Initiieren des Anmeldeflows zur Anmeldeseite der Anwendung weitergeleitet. Wenn Sie die Anwendung im IDP-Modus konfiguriert haben, sollten Sie automatisch bei der WEDO-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von WEDO können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Defender for Cloud Apps erzwingen.](/cloud-app-security/proxy-deployment-aad)
