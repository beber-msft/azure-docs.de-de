---
title: 'Tutorial: Azure Active Directory-Integration mit Dropbox Business | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Dropbox Business konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/17/2021
ms.author: jeedes
ms.openlocfilehash: 13a2cdf03923e353f9306ce22c5cfeee87da4f3a
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132314528"
---
# <a name="tutorial-integrate-dropbox-business-with-azure-active-directory"></a>Tutorial: Integrieren von Dropbox Business in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Dropbox Business in Azure Active Directory (Azure AD) integrieren. Bei der Integration von Dropbox Business in Azure AD haben Sie folgende Möglichkeiten:

* Steuern Sie in Azure AD, wer Zugriff auf Dropbox Business hat.
* Ermöglichen Sie Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Dropbox Business anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Abonnement von Dropbox Business, für das einmaliges Anmelden (SSO) aktiviert ist.

> [!NOTE]
> Diese Integration kann auch über die Azure AD-Umgebung für die US Government-Cloud verwendet werden. Sie finden diese Anwendung im Azure AD-Katalog für US Government-Cloudanwendungen und konfigurieren sie auf die gleiche Weise wie in der öffentlichen Cloud.

## <a name="scenario-description"></a>Beschreibung des Szenarios

* In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung. Dropbox Business unterstützt **SP**-initiiertes einmaliges Anmelden.

* Dropbox Business unterstützt die [automatisierte Benutzerbereitstellung und Bereitstellungsaufhebung](dropboxforbusiness-tutorial.md).

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="add-dropbox-business-from-the-gallery"></a>Hinzufügen von Dropbox Business aus dem Katalog

Zum Konfigurieren der Integration von Dropbox Business in Azure AD müssen Sie Dropbox Business aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Dropbox Business** in das Suchfeld ein.
1. Wählen Sie **Dropbox Business** im Ergebnisbereich aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-dropbox-business"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Dropbox Business

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Dropbox Business mithilfe eines Testbenutzers mit dem Namen **Britta Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Dropbox Business eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Dropbox Business zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.    
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Konfigurieren des einmaligen Anmeldens für Dropbox Business](#configure-dropbox-business-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
    1. **[Erstellen eines Dropbox Business-Testbenutzers](#create-dropbox-business-test-user)** , um in Dropbox Business eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Benutzerdarstellung in Azure AD verknüpft ist.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Dropbox Business** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie auf der Seite **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://www.dropbox.com/sso/<id>`.
    
     b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** den folgenden Wert ein: `Dropbox`.
    
    c. Geben Sie im Feld **Antwort-URL** die URL `https://www.dropbox.com/saml_login` ein.
    > [!NOTE]
    > Die **Dropbox-ID für einmaliges Anmelden** finden Sie auf der Dropbox-Website unter „Dropbox“ > „Admin console“ (Administratorkonsole) > „Settings“ (Einstellungen) > „Single sign-on“ (Einmaliges Anmelden) > „SSO sign-in URL“ (Anmelde-URL für SSO).

1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **Dropbox Business einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)


### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal eine Testbenutzerin mit dem Namen Britta Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `Britta Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `BrittaSimon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Dropbox Business gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Dropbox Business** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-dropbox-business-sso"></a>Konfigurieren des einmaligen Anmeldens für Dropbox Business

1. Wenn Sie die Konfiguration in Dropbox Business automatisieren möchten, müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![Erweiterung „Meine Apps“](common/install-myappssecure-extension.png)

2. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **Dropbox Business einrichten**, um zur Anwendung Dropbox Business weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei Dropbox Business anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert die Schritte 3 bis 8.

    ![Einrichtungskonfiguration](common/setup-sso.png)

3. Wenn Sie Dropbox Business manuell einrichten möchten, öffnen Sie ein neues Webbrowserfenster, navigieren Sie zu Ihrem Dropbox Business-Mandanten, und melden Sie sich bei diesem Mandanten an. Führen Sie die folgenden Schritte aus:

    ![Screenshot der Anmeldeseite für Dropbox Business](./media/dropboxforbusiness-tutorial/account.png "Einmaliges Anmelden konfigurieren")

4. Klicken Sie auf das **Benutzersymbol**, und wählen Sie die Registerkarte **Einstellungen** aus.

    ![Screenshot, auf dem die Aktion „Benutzersymbol“ und „Einstellungen“ ausgewählt sind](./media/dropboxforbusiness-tutorial/configure-1.png "Einmaliges Anmelden konfigurieren")

5. Klicken Sie im Navigationsbereich auf der linken Seite auf **Verwaltungskonsole**.

    ![Screenshot, auf dem „Verwaltungskonsole“ ausgewählt ist](./media/dropboxforbusiness-tutorial/configure-2.png "Einmaliges Anmelden konfigurieren")

6. Klicken Sie in der **Verwaltungskonsole** im linken Navigationsbereich auf **Einstellungen**.

    ![Screenshot, auf dem „Einstellungen“ ausgewählt ist](./media/dropboxforbusiness-tutorial/configure-3.png "Einmaliges Anmelden konfigurieren")

7. Wählen Sie im Abschnitt **Authentifizierung** die Option **Einmaliges Anmelden** aus.

    ![Screenshot des Abschnitts „Authentifizierung“, in dem „Einmaliges Anmelden“ ausgewählt ist](./media/dropboxforbusiness-tutorial/configure-4.png "Einmaliges Anmelden konfigurieren")

8. Führen Sie im Abschnitt **Einmaliges Anmelden** die folgenden Schritte aus:  

    ![Screenshot mit den Konfigurationseinstellungen für einmaliges Anmelden](./media/dropboxforbusiness-tutorial/configure-5.png "Einmaliges Anmelden konfigurieren")

    a. Wählen Sie in der Dropdownliste für **Einmaliges Anmelden** die Option **Erforderlich** aus.

    b. Klicken Sie auf **Anmelde-URL hinzufügen**, und fügen Sie den Wert der **Anmelde-URL**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Anmelde-URL des Identitätsanbieters** ein. Anschließend klicken Sie auf **Fertig**.

    ![Konfigurieren von einmaligem Anmelden](./media/dropboxforbusiness-tutorial/sso.png "Einmaliges Anmelden konfigurieren")

    c. Klicken Sie auf **Zertifikat hochladen**, und navigieren Sie dann zu Ihrer **Base64-codierten Zertifikatsdatei**, die Sie aus dem Azure-Portal heruntergeladen haben.

    d. Klicken Sie auf **Link kopieren**, und fügen Sie den kopierten Wert in das Textfeld **Anmelde-URL** im Abschnitt **Domäne und URLs für Dropbox Business** im Azure-Portal ein.

    e. Klicken Sie auf **Speichern**.

### <a name="create-dropbox-business-test-user"></a>Erstellen eines Dropbox Business-Testbenutzers

In diesem Abschnitt wird in Dropbox Business ein Benutzer namens B.Simon erstellt. Dropbox Business unterstützt die Just-In-Time-Benutzerbereitstellung (standardmäßig aktiviert). Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in Dropbox Business vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

>[!Note]
>Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an das [Kundensupportteam von Dropbox Business](https://www.dropbox.com/business/contact).

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen: 

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch werden Sie zur Anmelde-URL für Dropbox Business weitergeleitet, wo Sie den Anmeldeflow initiieren können. 

* Rufen Sie direkt die Dropbox Business-Anmelde-URL auf, und initiieren Sie den Anmeldeflow.

* Sie können „Meine Apps“ von Microsoft verwenden. Wenn Sie in „Meine Apps“auf die Kachel „Dropbox Business“ klicken, werden Sie zur Anmelde-URL für Dropbox Business weitergeleitet. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von Dropbox Business können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Defender for Cloud Apps erzwingen.](/cloud-app-security/proxy-deployment-any-app)
