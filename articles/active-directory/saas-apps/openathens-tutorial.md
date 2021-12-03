---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit OpenAthens | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und OpenAthens konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/03/2021
ms.author: jeedes
ms.openlocfilehash: 906eab30ca99da98ed795bdc747628831907b3a9
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132280125"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-openathens"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit OpenAthens

In diesem Tutorial erfahren Sie, wie Sie OpenAthens in Azure Active Directory (Azure AD) integrieren. Die Integration von OpenAthens in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf OpenAthens hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei OpenAthens anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* OpenAthens-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* OpenAthens unterstützt **IDP**-initiiertes einmaliges Anmelden.
* OpenAthens unterstützt die **Just-in-Time**-Benutzerbereitstellung.

## <a name="add-openathens-from-the-gallery"></a>Hinzufügen von OpenAthens aus dem Katalog

Zum Konfigurieren der Integration von OpenAthens in Azure AD müssen Sie OpenAthens aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **OpenAthens** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **OpenAthens** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-openathens"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für OpenAthens

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit OpenAthens mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in OpenAthens eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit OpenAthens die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    * **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    * **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für OpenAthens](#configure-openathens-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    * **[Erstellen eines OpenAthens-Testbenutzers](#create-openathens-test-user)** , um eine Entsprechung von B. Simon in OpenAthens zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **OpenAthens** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Laden Sie im Abschnitt **Grundlegende SAML-Konfiguration** die **Dienstanbieter-Metadatendatei** hoch. Die Schritte hierfür sind weiter unten in diesem Tutorial beschrieben.

    a. Klicken Sie auf **Metadatendatei hochladen**.

    ![OpenAthens – Hochladen von Metadaten](common/upload-metadata.png)

    b. Klicken Sie auf das **Ordnerlogo**, wählen Sie die Metadatendatei aus, und klicken Sie auf **Hochladen**.

    ![OpenAthens – Durchsuchen des Uploads von Metadaten](common/browse-upload-metadata.png)

    c. Nach dem erfolgreichen Upload der Metadatendatei wird der Wert **Bezeichner** automatisch in das Textfeld im Abschnitt **Grundlegende SAML-Konfiguration** eingefügt:

    ![SSO-Informationen zur Domäne und zu den URLs für OpenAthens](common/idp-identifier.png)

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **OpenAthens einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf OpenAthens gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **OpenAthens** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-openathens-sso"></a>Konfigurieren des einmaligen Anmeldens für OpenAthens

1. Melden Sie sich in einem anderen Webbrowserfenster bei der OpenAthens-Unternehmenswebsite als Administrator an.

1. Wählen Sie in der Liste auf der Registerkarte **Management** (Verwaltung) die Option **Connections** (Verbindungen) aus.

    ![Screenshot: Seite der Unternehmenswebsite von „OpenAthens“, auf der auf der Registerkarte „Management“ (Verwaltung) die Option „Connections“ (Verbindungen) ausgewählt ist](./media/openathens-tutorial/connections.png)

1. Wählen Sie **SAML 1.1/2.0** aus, und klicken Sie anschließend auf die Schaltfläche **Configure** (Konfigurieren).

    ![Screenshot: Dialogfeld „Select local authentication system type.“ (Typ des lokalen Authentifizierungssystems auswählen), in dem „SAML 1.1/2.0“ und die Schaltfläche „Configure“ (Konfigurieren) ausgewählt sind](./media/openathens-tutorial/saml.png)

1. Klicken Sie zum Hinzufügen der Konfiguration auf die Schaltfläche **Browse** (Durchsuchen), um die Metadaten-XML-Datei hochzuladen, die Sie aus dem Azure-Portal heruntergeladen haben, und klicken Sie anschließend auf **Add** (Hinzufügen).

    ![Screenshot: Dialogfeld „Add SAML authentication system.“ (SAML-Authentifizierungssystem hinzufügen), in dem die Aktion „Browse“ (Durchsuchen) und die Schaltfläche „Add“ (Hinzufügen) ausgewählt sind](./media/openathens-tutorial/configure.png)

1. Führen Sie auf der Registerkarte **Details** die folgenden Schritte aus.

    ![Einmaliges Anmelden konfigurieren](./media/openathens-tutorial/add.png)

    a. Wählen Sie für **Display name mapping** (Zuordnung des Anzeigenamens) die Option **Use Attribute** (Attribut verwenden) aus.

    b. Geben Sie im Textfeld **Display name attribute** (Anzeigenamenattribut) den Wert `http://schemas.microsoft.com/identity/claims/displayname` ein.

    c. Wählen Sie für **Unique user mapping** (Eindeutige Benutzerzuordnung) die Option **Use Attribute** (Attribut verwenden) aus.

    d. Geben Sie im Textfeld **Unique user attribute** (Eindeutiges Benutzerattribut) den Wert `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.

    e. Aktivieren Sie unter **Status** alle drei Kontrollkästchen.

    f. Wählen Sie in **Create local accounts** (Lokale Konten erstellen) die Option **automatically** (automatisch) aus.

    g. Klicken Sie auf **Save changes** (Änderungen speichern).

    h. Kopieren Sie auf der Registerkarte **</> Vertrauende Seite** die **Metadaten-URL**, und öffnen Sie die Seite im Browser, um die Datei mit der **SP-Metadaten-XML** herunterzuladen. Laden Sie diese SP-Metadatendatei im Abschnitt **Grundlegende SAML-Konfiguration** in Azure AD hoch.

    ![Screenshot: Ausgewählte Registerkarte „Relying party“ (Vertrauende Seite) und hervorgehobene Option „Metadata URL“ (Metadaten-URL)](./media/openathens-tutorial/metadata.png)

### <a name="create-openathens-test-user"></a>Erstellen eines OpenAthens-Testbenutzers

In diesem Abschnitt wird in OpenAthens eine Benutzerin namens „Britta Simon“ erstellt. OpenAthens unterstützt die **Just-in-Time-Benutzerbereitstellung**, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in OpenAthens vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen:

* Klicken Sie im Azure-Portal auf „Diese Anwendung testen“. Dadurch sollten Sie automatisch bei der OpenAthens-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben.

* Sie können „Meine Apps“ von Microsoft verwenden. Wenn Sie unter „Meine Apps“ auf die Kachel „OpenAthens“ klicken, sollten Sie automatisch bei der OpenAthens-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von OpenAthens können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Defender for Cloud Apps erzwingen.](/cloud-app-security/proxy-deployment-any-app)
