---
title: 'Tutorial: Integrieren von Sentry in das einmalige Anmelden (Single Sign-On, SSO) von Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Sentry konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/23/2020
ms.author: jeedes
ms.openlocfilehash: 6aa1d82982203a3d2c057fc0c4190dfbc7e60e71
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132279878"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sentry"></a>Tutorial: Integrieren von Sentry in das einmalige Anmelden (Single Sign-On, SSO) von Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Sentry in Azure Active Directory (Azure AD) integrieren. Die Integration von Sentry in Azure AD ermöglicht Folgendes:

* Sie können in Azure AD steuern, wer Zugriff auf Sentry haben soll.
* Sie können Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Sentry anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Sentry-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Sentry unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.
* Sentry unterstützt die **Just-In-Time**-Benutzerbereitstellung.

## <a name="adding-sentry-from-the-gallery"></a>Hinzufügen von Sentry aus dem Katalog

Zum Konfigurieren der Integration von Sentry in Azure AD müssen Sie Sentry aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** im Suchfeld den Begriff **Sentry** ein.
1. Wählen Sie im Ergebnisbereich den Eintrag **Sentry** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-sso-for-sentry"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Sentry

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Sentry mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Sentry eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Sentry die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    * **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    * **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Sentry](#configure-sentry-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
    * **[Erstellen eines Sentry-Testbenutzers](#create-sentry-test-user)** , um eine Entsprechung von B. Simon in Sentry zu erhalten, die mit der Benutzerdarstellung in Azure AD verknüpft ist.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Sentry** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://sentry.io/saml/metadata/<ORGANIZATION_SLUG>/`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://sentry.io/saml/acs/<ORGANIZATION_SLUG>/`

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://sentry.io/organizations/<ORGANIZATION_SLUG>/`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächlichen Werte für die Bezeichner, die Antwort-URL und die Anmelde-URL. Weitere Informationen zum Ermitteln dieser Werte finden Sie in der [Sentry-Dokumentation](https://docs.sentry.io/product/accounts/sso/azure-sso/#installation). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Klicken Sie auf der Seite **Einmaliges Anmelden mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf das Kopiersymbol, um den Wert von **App-Metadaten-URL** zu kopieren, und speichern Sie ihn dann auf Ihrem Computer.

   ![Downloadlink für das Zertifikat](common/copy-metadataurl.png)
    
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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Sentry gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste die Option **Sentry** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-sentry-sso"></a>Konfigurieren des einmaligen Anmeldens für Sentry

Navigieren Sie zu den **Unternehmens-Einstellungen** >  **Authentifizierung** (oder navigieren Sie zu`https://sentry.io/settings/<YOUR_ORG_SLUG>/auth/`) und wählen Sie **Konfigurieren** von dem Active Directory, um das einmalige Anmelden auf der **Sentry** Seite zu konfigurieren. Fügen Sie die App-Verbundmetadaten-URL aus Ihrer Azure SAML-Konfiguration ein.

### <a name="create-sentry-test-user"></a>Erstellen eines Sentry-Testbenutzers

In diesem Abschnitt wird in Sentry ein Benutzer namens B. Simon erstellt. Sentry unterstützt die Just-in-Time-Benutzerbereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist noch kein Benutzer in Sentry vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen: 

#### <a name="sp-initiated"></a>SP-initiiert:

1. Wählen Sie im Azure-Portal die Option **Diese Anwendung testen** aus. Sie werden zur Anmelde-URL für Sentry weitergeleitet, wo Sie den Anmeldeabfluss initiieren können.  

1. Rufen Sie direkt die Sentry-Anmelde-URL auf, und initiieren Sie den Anmeldeabfluss.

#### <a name="idp-initiated"></a>IDP-initiiert:

* Wählen Sie im Azure-Portal die Option **Diese Anwendung testen** aus. Dadurch sollten Sie automatisch bei der Sentry-Instanz angemeldet werden, für die Sie das einmalige Anmelden eingerichtet haben. 

#### <a name="either-mode"></a>Beliebiger Modus:

Sie können das „Meine Apps“-Portal verwenden, um die Anwendung in einem beliebigen Modus zu testen. Wenn Sie in „Meine Apps“-Portal auf die Kachel "Sentry" klicken, werden Sie, wenn Sie im SP-Modus konfiguriert sind zur Anmeldeseite der Anwendung umgeleitet, um den Anmeldeabfluss zu initiieren. Wenn Sie im IDP-Modus konfiguriert sind, sollten Sie automatisch bei der Sentry-Anwendung angemeldet werden, für die Sie das einmalige Anmelden eingerichtet haben. Weitere Informationen zum Portal „Meine Apps“ finden Sie unter [Anmelden beim Portal „Meine Apps“ und Starten von Apps über dieses](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von Sentry können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Defender for Cloud Apps erzwingen.](/cloud-app-security/proxy-deployment-any-app)
