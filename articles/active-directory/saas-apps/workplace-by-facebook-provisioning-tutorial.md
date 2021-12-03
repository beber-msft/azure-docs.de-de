---
title: 'Tutorial: Konfigurieren von Workplace by Facebook für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Hier werden die Schritte beschrieben, die Sie sowohl in Workplace by Facebook als auch in Azure Active Directory (Azure AD) ausführen müssen, um die automatische Benutzerbereitstellung zu konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/22/2021
ms.author: jeedes
ms.openlocfilehash: f2fb2358da5b30d450ad0a63e18aca9d8c6ddb6b
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131479789"
---
# <a name="tutorial-configure-workplace-by-facebook-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Workplace by Facebook für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte beschrieben, die Sie sowohl in Workplace by Facebook als auch in Azure Active Directory (Azure AD) ausführen müssen, um die automatische Benutzerbereitstellung zu konfigurieren. Nach der Konfiguration stellt Azure AD mithilfe des Azure AD-Bereitstellungsdiensts automatisch Benutzer für [Workplace by Facebook](https://work.workplace.com/) bereit bzw. hebt deren Bereitstellung automatisch auf. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="capabilities-supported"></a>Unterstützte Funktionen
> [!div class="checklist"]
> * Erstellen von Benutzern in Workplace by Facebook
> * Entfernen von Benutzern aus Workplace by Facebook, wenn diese keinen Zugriff mehr benötigen
> * Synchronisieren von Benutzerattributen zwischen Azure AD and Workplace by Facebook
> * [Einmaliges Anmelden](./workplacebyfacebook-tutorial.md) für Workplace by Facebook (empfohlen)

>[!VIDEO https://www.youtube.com/embed/oF7I0jjCfrY]

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* [Azure AD-Mandant](../develop/quickstart-create-new-tenant.md) 
* Ein Benutzerkonto in Azure AD mit der [Berechtigung](../roles/permissions-reference.md) zum Konfigurieren von Bereitstellungen (z. B. Anwendungsadministrator, Cloudanwendungsadministrator, Anwendungsbesitzer oder globaler Administrator)
* Ein Workplace by Facebook-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

> [!NOTE]
> Diese Integration kann auch über die Azure AD-Umgebung für die US Government-Cloud verwendet werden. Sie finden diese Anwendung im Azure AD-Katalog für US Government-Cloudanwendungen und konfigurieren sie auf die gleiche Weise wie in der öffentlichen Cloud.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="step-1-plan-your-provisioning-deployment"></a>Schritt 1: Planen der Bereitstellung
1. Erfahren Sie, [wie der Bereitstellungsdienst funktioniert](../app-provisioning/user-provisioning.md).
2. Bestimmen Sie, wer [in den Bereitstellungsbereich](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) einbezogen werden soll.
3. Bestimmen Sie, welche Daten [zwischen Azure AD und Workplace by Facebook zugeordnet werden sollen](../app-provisioning/customize-application-attributes.md).

## <a name="step-2-configure-workplace-by-facebook-to-support-provisioning-with-azure-ad"></a>Schritt 2: Konfigurieren von Workplace by Facebook zur Unterstützung der Bereitstellung mit Azure AD

Vor dem Konfigurieren und Aktivieren des Bereitstellungsdiensts müssen Sie entscheiden, welche Benutzer in Azure AD die Benutzer darstellen, die Zugriff auf Ihre Workplace by Facebook-App benötigen. Anschließend können Sie diese Benutzer Ihrer Workplace by Facebook-App zuweisen, indem Sie diese Anweisungen befolgen:

*   Es wird empfohlen, Workplace by Facebook einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der Bereitstellung zu testen. Später können weitere Benutzer zugewiesen werden.

*   Wenn Sie Workplace by Facebook einen Benutzer zuweisen, müssen Sie eine gültige Benutzerrolle auswählen. Die Rolle „Standardzugriff“ ist für Bereitstellungen nicht geeignet.

## <a name="step-3-add-workplace-by-facebook-from-the-azure-ad-application-gallery"></a>Schritt 3: Hinzufügen von Workplace by Facebook aus dem Azure AD-Anwendungskatalog

Fügen Sie Workplace by Facebook aus dem Azure AD-Anwendungskatalog hinzu, um die Verwaltung der Bereitstellung für Workplace by Facebook zu beginnen. Wenn Sie Workplace by Facebook zuvor für einmaliges Anmelden (Single Sign-On, SSO) eingerichtet haben, können Sie dieselbe Anwendung verwenden. Es ist jedoch empfehlenswert, beim erstmaligen Testen der Integration eine separate App zu erstellen. [Hier](../manage-apps/add-application-portal.md) erfahren Sie mehr über das Hinzufügen einer Anwendung aus dem Katalog.

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Schritt 4. Definieren der Benutzer für den Bereitstellungsbereich 

Mit dem Azure AD-Bereitstellungsdienst können Sie anhand der Zuweisung zur Anwendung oder aufgrund von Attributen für den Benutzer/die Gruppe festlegen, wer in die Bereitstellung einbezogen werden soll. Wenn Sie sich dafür entscheiden, anhand der Zuweisung festzulegen, wer für Ihre App bereitgestellt werden soll, können Sie der Anwendung mithilfe der folgenden [Schritte](../manage-apps/assign-user-or-group-access-portal.md) Benutzer zuweisen. Wenn Sie allein anhand der Attribute des Benutzers oder der Gruppe auswählen möchten, wer bereitgestellt wird, können Sie einen [hier](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) beschriebenen Bereichsfilter verwenden. 

* Beim Zuweisen von Benutzern zu Workplace by Facebook müssen Sie eine andere Rolle als **Standardzugriff** auswählen. Benutzer mit der Rolle „Standardzugriff“ werden von der Bereitstellung ausgeschlossen und in den Bereitstellungsprotokollen als „nicht effektiv berechtigt“ gekennzeichnet. Wenn für die Anwendung nur die Rolle „Standardzugriff“ verfügbar ist, können Sie das [Anwendungsmanifest aktualisieren](../develop/howto-add-app-roles-in-azure-ad-apps.md) und weitere Rollen hinzufügen. 

* Fangen Sie klein an. Testen Sie die Bereitstellung mit einer kleinen Gruppe von Benutzern, bevor Sie sie für alle freigeben. Wenn der Bereitstellungsbereich auf zugewiesene Benutzer festgelegt ist, können Sie dies durch Zuweisen von einem oder zwei Benutzern zur App steuern. Ist der Bereich auf alle Benutzer und Gruppen festgelegt, können Sie einen [attributbasierten Bereichsfilter](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) angeben. 

## <a name="step-5-configure-automatic-user-provisioning-to-workplace-by-facebook"></a>Schritt 5: Konfigurieren der automatischen Benutzerbereitstellung für Workplace by Facebook
In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts für das Erstellen, Aktualisieren und Deaktivieren von Benutzern in der Workplace by Facebook-App auf der Grundlage von Benutzerzuweisungen in Azure AD erläutert.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Workplace by Facebook** aus.

    ![Der Link von Workplace by Facebook in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Screenshot der Optionen zum Verwalten mit aufgerufener Bereitstellungsoption](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Screenshot der Dropdownliste „Bereitstellungsmodus“ mit aufgerufener Option „Automatisch“](common/provisioning-automatic.png)

5. Klicken Sie im Abschnitt **Administratoranmeldeinformationen** auf **Autorisieren**. Sie werden auf die Autorisierungsseite von Workplace by Facebook umgeleitet. Geben Sie Ihren Benutzernamen für Workplace by Facebook ein, und klicken Sie auf die Schaltfläche **Weiter**. Klicken Sie auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Workplace by Facebook herstellen kann. Wenn die Verbindung nicht hergestellt werden kann, sollten Sie sicherstellen, dass Ihr Workplace by Facebook-Konto über Administratorberechtigungen verfügt. Wiederholen Sie dann den Vorgang.

    ![Screenshot: Dialogfeld „Administratoranmeldeinformationen“ mit der Option „Autorisieren“](./media/workplace-by-facebook-provisioning-tutorial/provisioning.png)

    ![Autorisieren](./media/workplace-by-facebook-provisioning-tutorial/workplace-login.png)

6. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

7. Wählen Sie **Speichern** aus.

8. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Workplace by Facebook synchronisieren** aus.

9. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Workplace by Facebook synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in Workplace by Facebook für Updatevorgänge verwendet. Wenn Sie sich dafür entscheiden, das [übereinstimmende Zielattribut](../app-provisioning/customize-application-attributes.md) zu ändern, müssen Sie sicherstellen, dass die Workplace by Facebook-API das Filtern von Benutzern anhand dieses Attributs unterstützt. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

   |attribute|type|
   |---|---|
   |userName|String|
   |displayName|String|
   |aktiv|Boolean|
   |title|Boolean|
   |emails[type eq "work"].value|String|
   |name.givenName|String|
   |name.familyName|String|
   |name.formatted|String|
   |addresses[type eq "work"].formatted|String|
   |addresses[type eq "work"].streetAddress|String|
   |addresses[type eq "work"].locality|String|
   |addresses[type eq "work"].region|String|
   |addresses[type eq "work"].country|String|
   |addresses[type eq "work"].postalCode|String|
   |addresses[type eq "other"].formatted|String|
   |phoneNumbers[type eq "work"].value|String|
   |phoneNumbers[type eq "mobile"].value|String|
   |phoneNumbers[type eq "fax"].value|String|
   |externalId|String|
   |preferredLanguage|String|
   |urn:scim:schemas:extension:enterprise:1.0.manager|String|
   |urn:scim:schemas:extension:enterprise:1.0.department|String|
   |urn:scim:schemas:extension:enterprise:1.0.division|String|
   |urn:scim:schemas:extension:enterprise:1.0.organization|String|
   |urn:scim:schemas:extension:enterprise:1.0.costCenter|String|
   |urn:scim:schemas:extension:enterprise:1.0.employeeNumber|String|
   |urn:scim:schemas:extension:facebook:auth_method:1.0:auth_method|String|
   |urn:scim:schemas:extension:facebook:frontline:1.0.is_frontline|Boolean|
   |urn:scim:schemas:extension:facebook:starttermdates:1.0.startDate|Integer|


10. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Ändern Sie im Abschnitt **Einstellungen** den **Bereitstellungsstatus** in **Ein**, um den Azure AD-Bereitstellungsdienst für Workplace by Facebook zu aktivieren.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

12. Legen Sie die Benutzer fest, die in Workplace by Facebook bereitgestellt werden sollen, indem Sie im Abschnitt **Einstellungen** unter **Bereich** die entsprechenden Werte auswählen.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

13. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Mit diesem Vorgang wird der erste Synchronisierungszyklus für alle Benutzer gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert wurden. Der erste Zyklus dauert länger als nachfolgende Zyklen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. 

## <a name="step-6-monitor-your-deployment"></a>Schritt 6: Überwachen der Bereitstellung
Nachdem Sie die Bereitstellung konfiguriert haben, können Sie mit den folgenden Ressourcen die Bereitstellung überwachen:

1. Mithilfe der [Bereitstellungsprotokolle](../reports-monitoring/concept-provisioning-logs.md) können Sie ermitteln, welche Benutzer erfolgreich bzw. nicht erfolgreich bereitgestellt wurden.
2. Anhand der [Fortschrittsleiste](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) können Sie den Status des Bereitstellungszyklus überprüfen und den Fortschritt der Bereitstellung verfolgen.
3. Wenn sich die Bereitstellungskonfiguration in einem fehlerhaften Zustand zu befinden scheint, wird die Anwendung unter Quarantäne gestellt. Weitere Informationen zu den verschiedenen Quarantänestatus finden Sie [hier](../app-provisioning/application-provisioning-quarantine-status.md).

## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung
*  Wenn Sie feststellen, dass ein Benutzer nicht erfolgreich erstellt wurde und ein Überwachungsprotokollereignis mit dem Code „1789003“ vorliegt, bedeutet dies, dass der Benutzer aus einer nicht überprüften Domäne stammt.
*  In einigen Fällen erhalten Benutzer folgenden Fehler: „FEHLER: Fehlendes Feld ‚Email‘: Geben Sie den von Facebook zurückgegebenen E-Mail-Fehler an: Ausnahme bei der Verarbeitung der HTTP-Anforderung. Weitere Informationen finden Sie in der HTTP-Antwort, die von der Response-Eigenschaft dieser Ausnahme zurückgegeben wurde. Dieser Vorgang wurde kein einziges Mal wiederholt. Er wird nach diesem Datum erneut wiederholt.“ Die Ursache dieses Fehler ist, dass Kunden der Facebook-E-Mail „mail“ anstelle von „userPrincipalName“ zuordnen, einige Benutzer jedoch kein mail-Attribut haben. Ändern Sie die Attributzuordnung in Workplace vom mail-Attribut von Facebook in Coalesce([mail],[userPrincipalName]), heben Sie die Zuordnung des Benutzers von Workplace zu Facebook auf, oder stellen Sie eine E-Mail-Adresse für den Benutzer bereit, um die Fehler zu vermeiden und die fehlerhaften Benutzer erfolgreich über Facebook in Workplace bereitstellen zu können.  
*  Es gibt eine Option in Workplace, die das Vorhandensein von [Benutzern ohne E-Mail-Adressen](https://www.workplace.com/resources/tech/account-management/email-less#enable) ermöglicht. Wenn diese Einstellung auf der Workplace-Seite umschaltet wird, muss die Bereitstellung auf der Azure-Seite neu gestartet werden, damit Benutzer ohne E-Mail-Adressen erfolgreich in Workplace erstellt werden können.  


## <a name="change-log"></a>Änderungsprotokoll

* 10.09.2020: Unterstützung der Attribute „division“, „organization“, „costCenter“ und „employeeNumber“ hinzugefügt. Unterstützung der benutzerdefinierten Attribute „startDate“, „auth_method“ und „frontline“ hinzugefügt.
* 22.07.2021: Tipps zur Problembehandlung für Kunden mit einer Zuordnung von E-Mail-Adressen zu Facebook-E-Mail-Adressen aktualisiert, da einige Benutzer nicht über ein mail-Attribut verfügen.

## <a name="more-resources"></a>Weitere Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../app-provisioning/check-status-user-account-provisioning.md)
