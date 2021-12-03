---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit AWS Single-Account Access | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und AWS Single-Account Access konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/05/2021
ms.author: jeedes
ms.openlocfilehash: 5e3905c9d15d16641cad8a818719d9335cfcd916
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2021
ms.locfileid: "132324591"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-aws-single-account-access"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit AWS Single-Account Access

In diesem Tutorial erfahren Sie, wie Sie AWS Single-Account Access in Azure Active Directory (Azure AD) integrieren. Die Integration von AWS Single-Account Access in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf AWS Single-Account Access hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihrem Azure AD-Konto automatisch bei AWS Single-Account Access anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="understanding-the-different-aws-applications-in-the-azure-ad-application-gallery"></a>Grundlegendes zu den verschiedenen AWS-Anwendungen im Azure AD-Anwendungskatalog
Verwenden Sie die folgenden Informationen, um sich zwischen den Anwendungen „AWS Single Sign-On“ und „AWS Single-Account Access“ aus dem Azure AD-Anwendungskatalog zu entscheiden.

**AWS Single Sign-On**

[AWS Single Sign-On](./aws-single-sign-on-tutorial.md) wurde dem Azure AD-Anwendungskatalog im Februar 2021 hinzugefügt. Die Anwendung erleichtert die zentrale Zugriffsverwaltung für mehrere AWS-Konten und AWS-Anwendungen (mit Anmeldung über Microsoft Azure AD). Richten Sie einmalig einen Verbund zwischen Microsoft Azure AD und AWS SSO ein, und verwenden Sie AWS SSO, um Berechtigungen für alle Ihre AWS-Konten an einem zentralen Ort zu verwalten. AWS SSO stellt Berechtigungen automatisch bereit und hält sie auf dem neuesten Stand, wenn Sie Richtlinien und Zugriffszuweisungen aktualisieren. Endbenutzer können sich mit ihren Azure AD Anmeldeinformationen authentifizieren, um auf die AWS-Konsole, auf die Befehlszeilenschnittstelle und auf integrierte AWS SSO-Anwendungen zuzugreifen.

**AWS Single-Account Access**

[AWS Single-Account Access]() wird bereits seit einigen Jahren von Kunden verwendet. Die Anwendung ermöglicht die Einrichtung eines Verbunds zwischen Azure AD und einem einzelnen AWS-Konto sowie die Verwaltung des Zugriffs auf AWS-IAM-Rollen mithilfe von Azure AD. AWS-IAM-Administratoren definieren Rollen und Richtlinien in jedem AWS-Konto. Azure AD-Administratoren richten für jedes AWS-Konto einen Verbund mit AWS-IAM ein, weisen dem Konto Benutzer oder Gruppen zu und konfigurieren Azure AD so, dass Assertionen zum Autorisieren des Rollenzugriffs gesendet werden.  

| Funktion | AWS Single Sign-On | AWS Single-Account Access |
|:--- |:---:|:---:|
|Bedingter Zugriff| Unterstützt eine einzelne Richtlinie für bedingten Zugriff für alle AWS-Konten | Unterstützt eine einzelne Richtlinie für bedingten Zugriff für alle Konten oder benutzerdefinierte Richtlinien pro Konto|
| CLI-Zugriff | Unterstützt | Unterstützt|
| Privileged Identity Management | Noch nicht unterstützt | Noch nicht unterstützt |
| Zentralisierte Kontoverwaltung | Zentralisierte Kontoverwaltung in AWS | Zentralisierte Kontoverwaltung in Azure AD (setzt wahrscheinlich eine Azure AD-Unternehmensanwendung pro Konto voraus) |
| SAML-Zertifikat| Einzelnes Zertifikat| Separate Zertifikate pro App/Konto | 

## <a name="aws-single-account-access-architecture"></a>Architektur von AWS Single-Account Access
![Diagramm der Beziehung zwischen Azure AD und AWS](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Sie können mehrere Bezeichner für mehrere Instanzen konfigurieren. Beispiel:

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Mit diesen Werten entfernt Azure AD den Wert **#** und sendet den korrekten Wert (`https://signin.aws.amazon.com/saml`) als Zielgruppen-URL im SAML-Token.

Dieser Ansatz wird aus folgenden Gründen empfohlen:

- Mit jeder Anwendung erhalten Sie ein individuelles X509-Zertifikat. Jede Instanz einer AWS App-Instanz kann dann ein anderes Zertifikatablaufdatum haben, das auf individueller AWS-Kontenbasis verwaltet werden kann. In diesem Fall gestaltet sich das Zertifikatrollover einfacher.

- Sie können die Benutzerbereitstellung mit der AWS-App in Azure AD aktivieren, woraufhin unser Dienst alle Rollen aus diesem AWS-Konto abruft. Sie müssen die AWS-Rollen nicht manuell in der App hinzufügen oder aktualisieren.

- Sie können den App-Besitzer für die App individuell zuweisen. Diese Person kann die App direkt in Azure AD verwalten.

> [!Note]
> Achten Sie darauf, nur eine Kataloganwendung zu verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein SSO-fähiges AWS-Abonnement

> [!Note]
> Bei Rollenimporten sollten Rollen nicht manuell in Azure AD bearbeitet werden.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* AWS Single-Account Access unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="adding-aws-single-account-access-from-the-gallery"></a>Hinzufügen von AWS Single-Account Access aus dem Katalog

Um die Integration von AWS Single-Account Access in Azure AD zu konfigurieren, müssen Sie AWS Single-Account Access über den Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Suchen Sie im Azure-Portal nach **Azure Active Directory**, und wählen Sie es aus.
1. Wählen Sie im Azure Active Directory-Menü „Übersicht“ die Option **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **AWS Single-Account Access** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **AWS Single-Account Access** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-aws-single-account-access"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für AWS Single-Account Access

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit AWS Single-Account Access mithilfe eines Testbenutzers namens **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in AWS Single-Account Access eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit AWS Single-Account Access die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für AWS Single-Account Access](#configure-aws-single-account-access-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines AWS Single-Account Access-Testbenutzers](#create-aws-single-account-access-test-user)** , um in AWS Single-Account Access ein Pendant von B. Simon zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
    1. **[Konfigurieren der Rollenbereitstellung in AWS Single-Account Access](#how-to-configure-role-provisioning-in-aws-single-account-access)**
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **AWS Single-Account Access** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Aktualisieren Sie im Abschnitt **Grundlegende SAML-Konfiguration** sowohl **Bezeichner (Entitäts-ID)** als auch **Antwort-URL** mit dem gleichen Standardwert: `https://signin.aws.amazon.com/saml`. Wählen Sie **Speichern** aus, um die Änderungen an der Konfiguration zu speichern.

1. Falls Sie mehre Instanzen konfigurieren, geben Sie einen Bezeichnerwert an. Verwenden Sie ab der zweiten Instanz das folgende Format – einschließlich eines **#** -Zeichens zum Angeben eines eindeutigen SPN-Werts:

    `https://signin.aws.amazon.com/saml#2`

1. Die AWS-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute.

    ![image](common/default-attributes.png)

1. Darüber hinaus erwartet die AWS-Anwendung, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden (siehe unten). Diese Attribute werden ebenfalls vorab aufgefüllt, Sie können sie jedoch nach Bedarf überprüfen.
    
    | Name  | Quellattribut  | Namespace |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | `https://aws.amazon.com/SAML/Attributes` |
    | Role | user.assignedroles |  `https://aws.amazon.com/SAML/Attributes` |
    | SessionDuration | „Geben Sie einen Wert zwischen 900 Sekunden (15 Minuten) und 43.200 Sekunden (12 Stunden) an“ |  `https://aws.amazon.com/SAML/Attributes` |

    > [!NOTE]
    > AWS erwartet, dass der Anwendung Rollen für Benutzer zugewiesen werden. Richten Sie diese Rollen in Azure AD ein, damit Benutzern die passenden Rollen zugewiesen werden können. Informationen zum Konfigurieren von Rollen in Azure AD finden Sie [hier](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui).

1. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Dialogfeld **SAML-Signaturzertifikat** (Schritt 3) die Option **Zertifikat hinzufügen** aus.

    ![Erstellen eines neuen SAML-Zertifikats](common/add-saml-certificate.png)

1. Generieren Sie ein neues SAML-Signaturzertifikat, und wählen Sie **Neues Zertifikat** aus. Geben Sie eine E-Mail-Adresse für Zertifikatbenachrichtigungen ein.
   
    ![Neues SAML-Zertifikat](common/new-saml-certificate.png) 

1. Suchen Sie im Abschnitt **SAML-Signaturzertifikat** nach **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](./media/amazon-web-service-tutorial/certificate.png)

1. Kopieren Sie im Abschnitt **AWS Single-Account Access einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Suchen Sie im Azure-Portal nach **Azure Active Directory**, und wählen Sie es aus.
1. Wählen Sie im Azure Active Directory-Menü „Übersicht“ die Optionen **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf AWS Single-Account Access gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **AWS Single-Account Access** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-aws-single-account-access-sso"></a>Konfigurieren des einmaligen Anmeldens für AWS Single-Account Access

1. Melden Sie sich in einem anderen Browserfenster bei Ihrer AWS-Unternehmenswebsite als Administrator an.

2. Wählen Sie **AWS Home** (AWS-Startseite) aus.

    ![Screenshot: AWS-Unternehmenswebsite mit hervorgehobenem Symbol für „AWS Home“][11]

3. Wählen Sie **Identitäts- und Zugriffsverwaltung**.

    ![Screenshot: Seite mit den AWS-Diensten mit Hervorhebung von IAM][12]

4. Wählen Sie **Identity Providers** > **Create Provider** (Identitätsanbieter > Anbieter erstellen) aus.

    ![Screenshot: IAM-Seite mit den hervorgehobenen Optionen „Identity Providers“ (Identitätsanbieter) und „Create Provider“ (Anbieter erstellen)][13]

5. Führen Sie auf der Seite **Configure Provider** (Anbieter konfigurieren) die folgenden Schritte aus:

    ![Screenshot: „Configure Provider“ (Anbieter konfigurieren)][14]

    a. Wählen Sie für **Anbietertyp** die Option **SAML**.

    b. Geben Sie unter **Provider Name** (Anbietername) einen Anbieternamen ein (zum Beispiel: *WAAD*).

    c. Wählen Sie **Datei auswählen**, um die aus dem Azure-Portal heruntergeladene **Metadatendatei** hochzuladen.

    d. Wählen Sie **Nächster Schritt**.

6. Wählen Sie auf der Seite **Verify Provider Information** (Anbieterinformationen überprüfen) die Option **Create** (Erstellen) aus.

    ![Screenshot: „Verify Provider Information“ (Anbieterinformationen überprüfen) mit hervorgehobener Erstellungsoption][15]

7. Wählen Sie **Roles** > **Create role** (Rollen > Rolle erstellen) aus.

    ![Screenshot: Seite „Roles“ (Rollen)][16]

8. Führen Sie auf der Seite **Rolle erstellen** die folgenden Schritte aus:  

    ![Screenshot: Seite „Create role“ (Rolle erstellen)][19]

    a. Wählen Sie unter **Select type of trusted entity** (Art der vertrauenswürdigen Entität auswählen) die Option **SAML 2.0 federation** (SAML 2.0-Verbund) aus.

    b. Wählen Sie unter **Choose a SAML 2.0 Provider** (SAML 2.0-Anbieter auswählen) den **SAML-Anbieter** aus, den Sie zuvor erstellt haben (Beispiel: *WAAD*).

    c. Wählen Sie **Programmgesteuerten und AWS Management Console-Zugriff zulassen** aus.
  
    d. Klicken Sie auf **Weiter: Permissions** (Weiter: Berechtigungen).

9. Fügen Sie im Dialogfeld **Attach permissions policies** (Berechtigungsrichtlinien anfügen) die entsprechende Richtlinie für Ihre Organisation an. Wählen Sie anschließend **Next: Review** (Weiter: Überprüfen).  

    ![Screenshot: Dialogfeld „Attach permissions policies“ (Berechtigungsrichtlinien anfügen)][33]

10. Führen Sie im Dialogfeld **Review** (Überprüfen) die folgenden Schritte aus:

    ![Screenshot: Dialogfeld „Review“ (Überprüfen)][34]

    a. Geben Sie unter **Role name** (Rollenname) Ihren Rollennamen ein.

    b. Geben Sie unter **Role description** (Rollenbeschreibung) die Beschreibung ein.

    c. Wählen Sie **Create role** (Rolle erstellen) aus.

    d. Erstellen Sie so viele Rollen, wie Sie benötigen, und ordnen Sie sie dem Identitätsanbieter zu.

11. Verwenden Sie die Anmeldeinformationen des AWS-Dienstkontos, um die Rollen aus dem AWS-Konto in der Azure AD-Benutzerbereitstellung abzurufen. Öffnen Sie hierzu die Startseite der AWS-Konsole.

12. Klicken Sie auf **Dienste**. Wählen Sie unter **Security, Identity & Compliance** (Sicherheit, Identität und Compliance) die Option **IAM** aus.

    ![Screenshot: Startseite der AWS-Konsole mit den hervorgehobenen Optionen „Services“ (Dienste) und „IAM“](./media/amazon-web-service-tutorial/fetchingrole1.png)

13. Wählen Sie im IAM-Abschnitt die Option **Policies** (Richtlinien) aus.

    ![Screenshot: IAM-Abschnitt mit hervorgehobener Option „Policies“ (Richtlinien)](./media/amazon-web-service-tutorial/fetchingrole2.png)

14. Erstellen Sie durch Auswählen von **Create policy** (Richtlinie erstellen) eine neue Richtlinie, um die Rollen aus dem AWS-Konto in der Azure AD-Benutzerbereitstellung abzurufen.

    ![Screenshot: Seite „Create role“ (Rolle erstellen) mit hervorgehobener Option „Create policy“ (Richtlinie erstellen)](./media/amazon-web-service-tutorial/fetchingrole3.png)

15. Erstellen Sie eine eigene Richtlinie, um alle Rollen aus AWS-Konten abzurufen.

    ![Screenshot: Seite „Create policy“ (Richtlinie erstellen) mit hervorgehobener Registerkarte „JSON“](./media/amazon-web-service-tutorial/policy1.png)

    a. Wählen Sie unter **Create policy** (Richtlinie erstellen) die Registerkarte **JSON** aus.

    b. Fügen Sie in diesem Richtliniendokument den folgenden JSON-Code hinzu:

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                "iam:ListRoles"
                ],
                "Resource": "*"
            }
        ]
    }
    ```

    c. Wählen Sie **Review policy** (Richtlinie überprüfen) aus, um die Richtlinie zu überprüfen.

    ![Screenshot: Seite „Create policy“ (Richtlinie erstellen)](./media/amazon-web-service-tutorial/policy5.png)

16. Definieren Sie die neue Richtlinie.

    ![Screenshot: Seite „Create policy“ (Richtlinie erstellen) mit hervorgehobenen Feldern für Name und Beschreibung](./media/amazon-web-service-tutorial/policy2.png)

    a. Geben Sie unter **Name** die Zeichenfolge **AzureAD_SSOUserRole_Policy** ein.

    b. Geben Sie unter **Description** (Beschreibung) Folgendes ein: **This policy will allow to fetch the roles from AWS accounts** (Diese Richtlinie ermöglicht das Abrufen der Rollen aus AWS-Konten).

    c. Wählen Sie **Create policy** (Richtlinie erstellen) aus.

17. Erstellen Sie im AWS-IAM-Dienst ein neues Benutzerkonto.

    a. Wählen Sie in der AWS-IAM-Konsole die Option **Users** (Benutzer) aus.

    ![Screenshot: AWS-IAM-Konsole mit hervorgehobener Option „Users“ (Benutzer)](./media/amazon-web-service-tutorial/policy3.png)

    b. Wählen Sie zum Erstellen eines neuen Benutzers die Option **Add user** (Benutzer hinzufügen) aus.

    ![Screenshot: Schaltfläche „Add user“ (Benutzer hinzufügen)](./media/amazon-web-service-tutorial/policy4.png)

    c. Gehen Sie im Abschnitt **Add user** (Benutzer hinzufügen) wie folgt vor:

    ![Screenshot: Seite „Add user“ (Benutzer hinzufügen) mit den hervorgehobenen Bereichen für Benutzername und Zugriffstyp](./media/amazon-web-service-tutorial/adduser1.png)

    * Geben Sie **AzureADRoleManager** als Benutzernamen ein.

    * Wählen Sie für den Zugriffstyp die Option **Programmatic access** (Programmgesteuerter Zugriff) aus. Dadurch kann der Benutzer die APIs aufrufen und die Rollen aus dem AWS-Konto abrufen.

    * Wählen Sie **Next: Permissions** (Weiter: Berechtigungen) aus.

18. Erstellen Sie eine neue Richtlinie für diesen Benutzer.

    ![Screenshot: Seite „Benutzer hinzufügen“, auf der Sie eine Richtlinie für den Benutzer erstellen können](./media/amazon-web-service-tutorial/adduser2.png)

    a. Wählen Sie **Attach existing policies directly** (Vorhandene Richtlinien direkt anfügen) aus.

    b. Suchen Sie im Filterabschnitt **AzureAD_SSOUserRole_Policy** nach der neu erstellte Richtlinie.

    c. Wählen Sie die Richtlinie und anschließend **Next: Review** (Weiter: Überprüfen).

19. Überprüfen Sie die Richtlinie für den angefügten Benutzer.

    ![Screenshot: Seite „Add user“ (Benutzer hinzufügen) mit hervorgehobener Option für die Benutzererstellung](./media/amazon-web-service-tutorial/adduser3.png)

    a. Überprüfen Sie den Benutzernamen, den Zugriffstyp und die Richtlinie, die dem Benutzer zugeordnet wurde.

    b. Wählen Sie **Create User** (Benutzer erstellen) aus.

20. Laden Sie die Anmeldeinformationen eines Benutzers herunter.

    ![Screenshot: Seite „Benutzer hinzufügen“ mit einer Schaltfläche zum Herunterladen einer CSV-Datei zum Abrufen von Benutzeranmeldeinformationen](./media/amazon-web-service-tutorial/adduser4.png)

    a. Kopieren Sie **Access key ID** (Zugriffsschlüssel-ID) und **Secret access key** (Geheimer Zugriffsschlüssel) des Benutzers.

    b. Geben Sie diese Anmeldeinformationen im Azure AD-Abschnitt für die Benutzerbereitstellung ein, um die Rollen aus der AWS-Konsole abzurufen.

    c. Klicken Sie auf **Schließen**.

### <a name="how-to-configure-role-provisioning-in-aws-single-account-access"></a>Konfigurieren der Rollenbereitstellung in AWS Single-Account Access

1. Navigieren Sie im Azure AD-Verwaltungsportal in der AWS-App zu **Bereitstellung**.

    ![Screenshot: AWS-App mit hervorgehobener Bereitstellungsoption](./media/amazon-web-service-tutorial/provisioning.png)

2. Geben Sie den Zugriffsschlüssel und das Geheimnis in das Feld **clientsecret** (Clientgeheimnis) bzw. **Geheimes Token** ein.

    ![Screenshot: Dialogfeld „Administratoranmeldeinformationen“](./media/amazon-web-service-tutorial/provisioning1.png)

    a. Geben Sie in das Feld **clientsecret** (Geheimer Clientschlüssel) den AWS-Benutzerzugriffsschlüssel ein.

    b. Geben Sie in das Feld **Secret Token** (Geheimes Token) die geheimen AWS-Benutzerdaten ein.

    c. Wählen Sie **Verbindung testen** aus.

    d. Wählen Sie **Speichern** aus, um das Manifest zu speichern.

3. Wählen Sie im Abschnitt **Einstellungen** für **Bereitstellungsstatus** die Option **Ein** aus. Klicken Sie dann auf **Speichern**.

    ![Screenshot: Abschnitt „Einstellungen“ mit hervorgehobener Option „Ein“](./media/amazon-web-service-tutorial/provisioning2.png)

> [!NOTE]
> Der Bereitstellungsdienst importiert Rollen nur aus AWS in Azure AD. Vom Dienst werden keine Benutzer und Gruppen aus Azure AD in AWS bereitgestellt.

> [!NOTE]
> Nach dem Speichern der Anmeldeinformationen für die Bereitstellung müssen Sie warten, bis der erstmalige Synchronisierungszyklus ausgeführt wird. Die Synchronisierung dauert normalerweise ca. 40 Minuten. Der Status wird unten auf der Seite für die **Bereitstellung** unter **Aktueller Status** angezeigt.

### <a name="create-aws-single-account-access-test-user"></a>Erstellen eines Testbenutzers für AWS Single-Account Access

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens B. Simon in AWS Single-Account Access. Bei AWS Single-Account Access muss für das einmalige Anmelden nicht eigens ein Benutzer im System erstellt werden, daher ist hier keine Aktion erforderlich.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen: 

#### <a name="sp-initiated"></a>SP-initiiert:

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch werden Sie zur Anmelde-URL für AWS Single-Account Access weitergeleitet, wo Sie den Anmeldeflow initiieren können.  

* Rufen Sie direkt die Anmelde-URL für AWS Single-Account Access auf, und initiieren Sie den Anmeldeflow.

#### <a name="idp-initiated"></a>IDP-initiiert:

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch sollten Sie automatisch bei der AWS Single-Account Access-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. 

Sie können auch den Microsoft-Bereich „Meine Apps“ verwenden, um die Anwendung in einem beliebigen Modus zu testen. Beim Klicken auf die Kachel „AWS Single-Account Access“ in „Meine Apps“ geschieht Folgendes: Wenn Sie den SP-Modus konfiguriert haben, werden Sie zum Initiieren des Anmeldeflows zur Anmeldeseite der Anwendung weitergeleitet. Wenn Sie den IDP-Modus konfiguriert haben, sollten Sie automatisch bei der Instanz von AWS Single-Account Access angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).


## <a name="known-issues"></a>Bekannte Probleme

* Die Bereitstellungsintegration von AWS Single-Account Access kann nur zum Herstellen einer Verbindung mit öffentlichen AWS-Cloudendpunkten verwendet werden. Die Bereitstellungsintegration von AWS Single-Account Access kann nicht für den Zugriff auf AWS-Government-Umgebungen verwendet werden.
 
* Im Abschnitt **Bereitstellung** ist im Unterabschnitt **Zuordnungen** eine Lademeldung zu sehen, die Attributzuordnungen werden aber nicht angezeigt. Aktuell wird als Bereitstellungsworkflow lediglich das Importieren von Rollen aus AWS in Azure AD für die Auswahl im Rahmen einer Benutzer- oder Gruppenzuweisung unterstützt. Die Attributzuordnungen hierfür sind vorgegeben und nicht konfigurierbar.

* Im Abschnitt **Bereitstellung** kann immer nur ein einzelner Satz von Anmeldeinformationen für einen einzelnen AWS-Mandanten eingegeben werden. Alle importierten Rollen werden in die Eigenschaft `appRoles` des Azure AD-Objekts [`servicePrincipal`](/graph/api/resources/serviceprincipal)für den AWS-Mandanten geschrieben.

  Azure AD können über den Katalog mehrere AWS-Mandanten (dargestellt durch `servicePrincipals`) für die Bereitstellung hinzugefügt werden. Es gibt jedoch ein bekanntes Problem, das dazu führt, dass nicht alle importierten Rollen aus den verschiedenen, für die Bereitstellung verwendeten AWS-Dienstprinzipalen (`servicePrincipals`) automatisch in den einzelnen Dienstprinzipal (`servicePrincipal`) für das einmalige Anmelden geschrieben werden können.

  Zur Umgehung dieses Problems können Sie mithilfe der [Microsoft Graph-API](/graph/api/resources/serviceprincipal) alle App-Rollen (`appRoles`) extrahieren, die in die einzelnen AWS-Dienstprinzipale (`servicePrincipal`) mit konfigurierter Bereitstellung importiert wurden. Anschließend können Sie diese Rollenzeichenfolgen dem AWS-Dienstprinzipal (`servicePrincipal`) hinzufügen, für den SSO konfiguriert ist.

* Rollen müssen die folgenden Anforderungen erfüllen, damit sie von AWS in Azure AD importiert werden können:

  * Für Rollen muss in AWS genau ein SAML-Anbieter festgelegt sein.
  * Der ARN (Amazon Resource Name) für die Rolle und der ARN für den zugeordneten SAML-Anbieter müssen zusammen weniger als 240 Zeichen umfassen.

## <a name="change-log"></a>Änderungsprotokoll

* 12.01.2020: Der Grenzwert für die Rollenlänge wurde von 119 auf 239 Zeichen erhöht.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von AWS Single-Account Access können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Defender for Cloud Apps erzwingen.](/cloud-app-security/proxy-deployment-aad)

[11]: ./media/amazon-web-service-tutorial/ic795031.png
[12]: ./media/amazon-web-service-tutorial/ic795032.png
[13]: ./media/amazon-web-service-tutorial/ic795033.png
[14]: ./media/amazon-web-service-tutorial/ic795034.png
[15]: ./media/amazon-web-service-tutorial/ic795035.png
[16]: ./media/amazon-web-service-tutorial/ic795022.png
[17]: ./media/amazon-web-service-tutorial/ic795023.png
[18]: ./media/amazon-web-service-tutorial/ic795024.png
[19]: ./media/amazon-web-service-tutorial/ic795025.png
[32]: ./media/amazon-web-service-tutorial/ic7950251.png
[33]: ./media/amazon-web-service-tutorial/ic7950252.png
[35]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/amazon-web-service-tutorial/ic7950253.png
[36]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
