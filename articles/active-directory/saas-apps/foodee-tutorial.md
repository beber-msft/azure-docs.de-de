---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Foodee | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Foodee konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/27/2021
ms.author: jeedes
ms.openlocfilehash: 84cb6886c802208d414cb3b8f5281971e0b1d8aa
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132317811"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-foodee"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Foodee

In diesem Tutorial erfahren Sie, wie Sie Foodee in Azure Active Directory (Azure AD) integrieren. Die Integration von Foodee in Azure AD ermöglicht Folgendes:

* Sie können in Azure AD steuern, wer Zugriff auf Foodee hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Foodee anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Foodee-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Foodee unterstützt **SP- und IDP**-initiiertes einmaliges Anmelden.
* Foodee unterstützt **Just-in-Time**-Benutzerbereitstellung.
* Foodee unterstützt [Automatisierte Benutzerbereitstellung](foodee-provisioning-tutorial.md).

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="adding-foodee-from-the-gallery"></a>Hinzufügen von Foodee aus dem Katalog

Zum Konfigurieren der Integration von Foodee in Azure AD müssen Sie Foodee aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Foodee** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Foodee** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-foodee"></a>Konfigurieren und Testen von Azure AD-SSO für Foodee

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Foodee mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Foodee eingerichtet werden.

Führen Sie zum Konfigurieren und Testen von Azure AD-SSO mit Foodee die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Foodee](#configure-foodee-sso)**, um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines Foodee-Testbenutzers](#create-foodee-test-user)**, um eine Entsprechung von B. Simon in Foodee zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Foodee** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://concierge.food.ee/sso/saml/<INSTANCENAME>/consume`.

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://concierge.food.ee/sso/saml/<INSTANCENAME>`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Antwort-URL und die tatsächliche Anmelde-URL ersetzt werden. Diese Werte erhalten Sie vom [Supportteam für den Foodee-Client](mailto:dev@food.ee). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **Foodee einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Foodee gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste den Eintrag **Foodee** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="configure-foodee-sso"></a>Konfigurieren des einmaligen Anmeldens für Foodee

1. Wenn Sie die Konfiguration in Foodee automatisieren möchten, müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![Erweiterung „Meine Apps“](common/install-myappssecure-extension.png)

2. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **Foodee einrichten**, um zur Anwendung Foodee weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei Foodee anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert die Schritte 3 und 4.

    ![Einrichtungskonfiguration](common/setup-sso.png)

3. Wenn Sie Foodee manuell einrichten möchten, melden Sie sich in einem neuen Webbrowserfenster bei der Foodee-Unternehmenswebsite als Administrator an, und führen Sie die folgenden Schritte aus:

4. Klicken Sie oben rechts auf der Seite auf **profile logo** (Profillogo), navigieren Sie zu **Single Sign On** (Einmaliges Anmelden), und führen Sie die folgenden Schritte aus:

   ![Foodee-Konfiguration](./media/foodee-tutorial/profile-logo.png)

   1. Geben Sie im Textfeld **IDP NAME** (IDP-NAME) den Namen ein, etwa „Azure“.
   1. Öffnen Sie die Verbundmetadaten-XML im Editor, kopieren Sie den Inhalt, und fügen Sie ihn ins Textfeld **IDP METADATA XML** (IDP-METADATEN-XML) ein.
   1. Klicken Sie auf **Speichern**.

### <a name="create-foodee-test-user"></a>Erstellen eines Foodee-Testbenutzers

In diesem Abschnitt wird in Foodee ein Benutzer namens B. Simon erstellt. Foodee unterstützt die Just-In-Time-Bereitstellung (standardmäßig aktiviert). Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls ein Benutzer nicht bereits in Foodee vorhanden ist, wird beim Versuch, auf Foodee zuzugreifen, ein neuer Benutzer erstellt.

Außerdem unterstützt Foodee automatische Benutzerbereitstellung. Weitere Informationen zum Konfigurieren der automatischen Benutzerbereitstellung finden Sie [hier](./foodee-provisioning-tutorial.md).

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen:

SP-initiiert:

* Klicken Sie im Azure-Portal auf Diese Anwendung testen. Dadurch werden Sie zur Anmelde-URL für Foodee weitergeleitet. Dort können Sie den Anmeldeflow initiieren.

* Rufen Sie die Anmelde-URL für Foodee direkt auf, und initiieren Sie den Anmeldeflow.

IDP-initiiert:

* Klicken Sie im Azure-Portal auf „Diese Anwendung testen“. Dadurch sollten Sie automatisch bei der Foodee-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben.

Sie können auch den Microsoft-Bereich „Meine Apps“ verwenden, um die Anwendung in einem beliebigen Modus zu testen. Beim Klicken auf die Kachel „Foodee“ in „Meine Apps“ geschieht Folgendes: Wenn Sie die Anwendung im SP-Modus konfiguriert haben, werden Sie zum Initiieren des Anmeldeflows zur Anmeldeseite der Anwendung weitergeleitet. Wenn Sie die Anwendung im IDP-Modus konfiguriert haben, sollten Sie automatisch bei der Foodee-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](../user-help/my-apps-portal-end-user-access.md).


## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von Foodee können Sie Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Defender for Cloud Apps erzwingen.](/cloud-app-security/proxy-deployment-any-app)
