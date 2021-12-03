---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Prezi | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und Prezi konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/23/2021
ms.author: jeedes
ms.openlocfilehash: f10befe849a2a907526f28bdb8c11a150c44cda8
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132287531"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-prezi"></a>Tutorial: Integration des einmaligen Anmeldens von Azure Active Directory mit Prezi

In diesem Tutorial erfahren Sie, wie Sie Prezi in Azure Active Directory (Azure AD) integrieren. Die Integration von Prezi in Azure AD ermöglicht Folgendes:

* Steuern Sie, wer in Azure AD Zugriff auf Prezi hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihrem Azure AD-Konto automatisch bei Prezi anzumelden.
* Verwalten Sie Ihre Konten im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Prezi-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Prezi unterstützt SP- und IDP-initiiertes einmaliges Anmelden.
* Prezi unterstützt die Just-In-Time-Benutzerbereitstellung.

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="add-prezi-from-the-gallery"></a>Hinzufügen von Prezi aus dem Katalog

Um die Integration von Prezi in Azure AD zu konfigurieren, müssen Sie Prezi über den Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im ganz linken Bereich **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Prezi** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Prezi** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-prezi"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Prezi

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Prezi mithilfe einer Testbenutzerin mit dem Namen „B. Simon“. Damit einmaliges Anmelden funktioniert, richten Sie eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Prezi ein.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Prezi die folgenden Schritte aus:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso), um Ihren Benutzern die Verwendung dieses Features zu ermöglichen
    1. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden von Azure AD mit B.Simon zu testen
    1. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um B.Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen
1. [Konfigurieren des einmaliges Anmeldens für Prezi](#configure-prezi-sso), um die Einstellungen für das einmalige Anmelden auf der Anwendungsseite zu konfigurieren.
    1. [Erstellen eines Prezi-Testbenutzers](#create-a-prezi-test-user), um in Prezi eine Entsprechung von B. Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. [Testen des einmaligen Anmeldens](#test-sso), um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

So aktivieren Sie einmaliges Anmelden von Azure AD im Azure-Portal

1. Suchen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Prezi** den Abschnitt **Verwalten** und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Wählen Sie auf der Seite **Einmaliges Anmelden mit SAML einrichten** das Stiftsymbol aus, um die Einstellungen in der **Grundlegenden SAML-Konfiguration** zu bearbeiten.

   ![Bearbeiten von Einstellungen für die grundlegende SAML-Konfiguration](common/edit-urls.png)

1. Im Abschnitt **Grundlegende SAML-Konfiguration** muss der Benutzer keine Schritte ausführen, weil die App bereits in Azure integriert ist.

1. Wählen Sie **Zusätzliche URLs festlegen** aus, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten** Modus konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** die URL ein: `https://prezi.com/login/sso/`.

1. Wählen Sie **Speichern** aus.

1. Die Prezi-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute.

    ![Benutzerattribute und Ansprüche](common/default-attributes.png)

1. Von der Prezi-Anwendung wird außerdem die Rückgabe einiger weiterer Attribute in der SAML-Antwort erwartet, wie hier gezeigt. Diese Attribute werden ebenfalls vorab aufgefüllt, Sie können sie jedoch gemäß Ihren Anforderungen überarbeiten.
    
    | Name | Quellattribut|
    | ---------------| --------------- |
    | given_name | user.givenname |
    | family_name | user.surname |

1. Suchen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** nach **Zertifikat (Base64)** . Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **Prezi einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im ganz linken Bereich des Azure-Portals die Option **Azure Active Directory** aus. Wechseln Sie zu **Benutzer**, und wählen Sie dann **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie in den Eigenschaften für Benutzer die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** den Namen **B.Simon** ein.
   1. Geben Sie im Feld **Benutzername** `username@companydomain.extension` ein, z. B. `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**. Notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B.Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Prezi gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Prezi** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie die Schaltfläche **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf **Auswählen**.
1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.

## <a name="configure-prezi-sso"></a>Konfigurieren des einmaligen Anmeldens für Prezi

1. Melden Sie sich in einem anderen Webbrowserfenster mit Ihrem Teamkonto bei Prezi an, und navigieren Sie zur [Verwaltungskonsole](https://prezi.com/organizations/manage).

1. Wählen Sie in der **Verwaltungskonsole** die Registerkarte **Einstellungen** aus.

    ![Registerkarte "Einstellungen"](./media/prezi-tutorial/settings-image.png)

1. Wechseln Sie zum Abschnitt **Einmaliges Anmelden (SSO)** , und aktivieren Sie die Umschaltfläche, um einmaliges Anmelden zu aktivieren.
    
    ![Umschaltfläche „Einmaliges Anmelden (SSO)“](./media/prezi-tutorial/single-sign-on.png)

1. Führen Sie im Abschnitt **Einmaliges Anmelden (SSO)** die folgenden Schritte aus:

    ![Abschnitt „Einmaliges Anmelden (SSO)“](./media/prezi-tutorial/configuration.png)

    1. Fügen Sie im Feld **Bezeichner oder Aussteller-URL** den Wert für **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    1. Fügen Sie im Feld **SAML 2.0-Endpunkt (HTTP)** den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    1. Öffnen Sie das aus dem Azure-Portal heruntergeladene **Zertifikat (Base64)** in Editor. Kopieren Sie den Inhalt des Zertifikats, und fügen Sie ihn ins Feld **Zertifikat (X.509)** ein.

    1. Wählen Sie **Speichern** aus.

### <a name="create-a-prezi-test-user"></a>Erstellen eines Prezi-Testbenutzers

In diesem Abschnitt wird in Prezi ein Benutzer mit dem Namen Britta Simon erstellt. Prezi unterstützt die Just-In-Time-Benutzerbereitstellung (standardmäßig aktiviert). Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in Prezi vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen: 

#### <a name="sp-initiated"></a>SP-initiiert:

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch werden Sie zur Anmelde-URL für Prezi weitergeleitet, wo Sie den Anmeldeflow initiieren können.  

* Navigieren Sie direkt zur Anmelde-URL für Prezi, und initiieren Sie den Anmeldeablauf.

#### <a name="idp-initiated"></a>IDP-initiiert:

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch sollten Sie automatisch bei der Prezi-Instanz angemeldet werden, für die Sie das einmalige Anmelden eingerichtet haben. 

Sie können auch den Microsoft-Bereich „Meine Apps“ verwenden, um die Anwendung in einem beliebigen Modus zu testen. Beim Klicken auf die Kachel „Prezi“ in „Meine Apps“ geschieht Folgendes: Wenn Sie die Anwendung im SP-Modus konfiguriert haben, werden Sie zum Initiieren des Anmeldeablaufs zur Anmeldeseite der Anwendung weitergeleitet. Wenn Sie die Anwendung im IDP-Modus konfiguriert haben, sollten Sie automatisch bei der Prezi-Instanz angemeldet werden, für die Sie das einmalige Anmelden eingerichtet haben. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von Prezi können Sie die Sitzungssteuerung erzwingen, die Ihre vertraulichen Unternehmensdaten in Echtzeit vor der Exfiltration und Infiltration schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Defender for Cloud Apps erzwingen.](/cloud-app-security/proxy-deployment-aad)
