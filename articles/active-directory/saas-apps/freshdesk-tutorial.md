---
title: 'Tutorial: Azure Active Directory-Integration mit Freshdesk | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Freshdesk konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/20/2021
ms.author: jeedes
ms.openlocfilehash: 7678efc256e0c533ac82f8eee4abf86b76328b55
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132338001"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Tutorial: Azure Active Directory-Integration mit Freshdesk

In diesem Tutorial erfahren Sie, wie Sie FreshDesk in Azure Active Directory (Azure AD) integrieren. Die Integration von FreshDesk in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf FreshDesk hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei FreshDesk anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:
 
* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein FreshDesk-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* FreshDesk unterstützt **SP**-initiiertes einmaliges Anmelden.

## <a name="add-freshdesk-from-the-gallery"></a>Hinzufügen von FreshDesk aus dem Katalog

Zum Konfigurieren der Integration von Freshdesk in Azure AD müssen Sie Freshdesk aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **FreshDesk** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **FreshDesk** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-freshdesk"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für FreshDesk

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit FreshDesk mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in FreshDesk eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit FreshDesk die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Konfigurieren des einmaligen Anmeldens für FreshDesk](#configure-freshdesk-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines FreshDesk-Testbenutzers](#create-freshdesk-test-user)** , um eine Entsprechung von Britta Simon in FreshDesk zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **FreshDesk** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

    1. Geben Sie im Textfeld **Anmelde-URL** eine URL im Format `https://<tenant-name>.freshdesk.com` oder einen anderen von FreshDesk vorgeschlagenen Wert an.

    1. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im Format `https://<tenant-name>.freshdesk.com` oder einen anderen von FreshDesk vorgeschlagenen Wert an.
     
    1. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<tenant-name>.freshdesk.com/login/saml`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächlichen Werte für die Anmelde-URL, den Bezeichner und die Antwort-URL. Wenden Sie sich an das [Supportteam von FreshDesk](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Die FreshDesk-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot enthält die Liste der Standardattribute: **Eindeutige Benutzer-ID** ist **user.userprincipalname** zugeordnet, FreshDesk erwartet jedoch, dass dieser Anspruch **user.mail** zugeordnet wird. Daher müssen Sie die Attributzuordnung bearbeiten, indem Sie auf das Symbol „Bearbeiten“ klicken und die Attributzuordnung ändern.

    ![image](common/edit-attribute.png)

1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **FreshDesk einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf FreshDesk gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Freshdesk** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-freshdesk-sso"></a>Konfigurieren des einmaligen Anmeldens für FreshDesk

1. Melden Sie sich in einem anderen Webbrowserfenster bei der Freshdesk-Unternehmenswebsite als Administrator an.

2. Wählen Sie das Symbol für **Sicherheit** aus, und führen Sie im Abschnitt **Security** (Sicherheit) die folgenden Schritte aus:

    ![Einmaliges Anmelden](./media/freshdesk-tutorial/configure-1.png "Einmaliges Anmelden")
  
    1. Wählen Sie unter **Single Sign On** (Einmaliges Anmelden) die Einstellung **On** (Ein) aus.

    1. Wählen Sie unter **Login Method** (Anmeldemethode) die Option **SAML SSO** (SAML-SSO) aus.

    1. Fügen Sie im Textfeld **Entity ID provided by the IdP** (Vom IdP bereitgestellte Entitäts-ID) den Wert der **Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    1. Fügen Sie im Textfeld **SAML SSO URL** (SAML-SSO-URL) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    1. Wählen Sie unter **Signing Options** (Signaturoptionen) im Dropdownmenü die Option **Only Signed Assertions** (Nur signierte Assertionen) aus.

    1. Fügen Sie im Textfeld **Abmelde-URL** den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    1. Fügen Sie im Textfeld **Security Certificate** (Sicherheitszertifikat) den Wert für **Zertifikat (Base64)** ein, den Sie zuvor erhalten haben.
  
    1. Klicken Sie auf **Speichern**.

## <a name="create-freshdesk-test-user"></a>Erstellen eines FreshDesk-Testbenutzers

Damit sich Azure AD-Benutzer bei Freshdesk anmelden können, müssen sie in Freshdesk bereitgestellt werden.  
Im Fall von Freshdesk ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **Freshdesk** -Mandanten an.

1. Klicken Sie im Menü auf der linken Seite auf **Admin** (Verwaltung) und auf der Registerkarte **General Settings** (Allgemeine Einstellungen) auf **Agents**.
  
    ![Agents](./media/freshdesk-tutorial/create-user-1.png "Agents")

1. Klicken Sie auf **Neuer Agent**.

    ![New Agent (Neuer Agent)](./media/freshdesk-tutorial/create-user-2.png "Neuer Agent")

1. Füllen Sie im Dialogfeld mit den Agent-Informationen die Pflichtfelder aus, und klicken Sie auf **Create agent** (Agent erstellen).

    ![Agent Information (Agent-Informationen)](./media/freshdesk-tutorial/create-user-3.png "Agent-Informationen")

    >[!NOTE]
    >Der Besitzer des Azure AD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
    >
    >[!NOTE]
    >Sie können Azure AD-Benutzerkonten auch mit anderen Tools zum Erstellen von FreshDesk-Benutzerkonten oder mit den APIs von FreshDesk bereitstellen.

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen: 

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch werden Sie zur Anmelde-URL für FreshDesk weitergeleitet, wo Sie den Anmeldeflow initiieren können. 

* Rufen Sie direkt die Anmelde-URL für FreshDesk auf, und initiieren Sie den Anmeldeflow.

* Sie können „Meine Apps“ von Microsoft verwenden. Wenn Sie unter „Meine Apps“ auf die Kachel „FreshDesk“ klicken, sollten Sie automatisch bei der FreshDesk-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von FreshDesk können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Defender for Cloud Apps erzwingen.](/cloud-app-security/proxy-deployment-any-app)
