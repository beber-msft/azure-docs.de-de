---
title: Konfiguration von 1-Klick-SSO (Single Sign-On, einmaliges Anmelden) für Ihre Azure Marketplace-Anwendung
description: Schritte für die 1-Klick-Konfiguration von SSO für Ihre Anwendung aus dem Azure Marketplace.
titleSuffix: Azure AD
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: davidmu
ms.collection: M365-identity-device-management
ms.reviewer: ergreenl
ms.openlocfilehash: d0e0a9fcb3052132df33d6988468cb218c3d9fc2
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132556608"
---
# <a name="one-click-app-configuration-of-single-sign-on"></a>1-Klick-App-Konfiguration für einmaliges Anmelden

 In diesem Tutorial erfahren Sie, wie Sie die 1-Klick-SSO-Konfiguration für Azure Active Directory (Azure AD)-Anwendungen aus dem Azure Marketplace durchführen, die SAML unterstützen.

## <a name="introduction-to-one-click-sso"></a>Einführung in 1-Klick-SSO

Das Feature „1-Klick-SSO“ wurde entwickelt, um einmaliges Anmelden für Azure Marketplace-Anwendungen zu konfigurieren, die das SAML-Protokoll unterstützen. Auf der Azure AD-Seite zur SSO-Konfiguration können Sie mit dieser Option die Azure AD-Metadaten auf der Anwendungsseite automatisch konfigurieren. So können Sie SSO mit minimalem manuellen Aufwand schnell einrichten.

## <a name="advantages-of-one-click-sso"></a>Vorteile von 1-Klick-SSO

- Schnelle SSO-Konfiguration für Azure Marketplace-Anwendungen, die eine manuelle Einrichtung auf der Anwendungsseite erfordern
- Effizientere und präzisere Art der SSO-Konfiguration
- Wegfall der Kommunikation mit dem Partner oder Unterstützung bei der Einrichtung Bereitstellung der Benutzeroberfläche für die SAML-Konfiguration durch die Anwendung

## <a name="prerequisites"></a>Voraussetzungen

- Aktives Abonnement der Anwendung, die Sie mit SSO konfigurieren möchten. Außerdem benötigen Sie Administratoranmeldeinformationen.
- Die **Erweiterung zur sicheren Anmeldung bei „Meine Apps“** von Microsoft, die im Browser installiert ist. Weitere Informationen finden Sie unter [Zugreifen auf und Verwenden von Apps im Portal „Meine Apps“](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="one-click-sso-configuration-steps"></a>Schritte der 1-Klick-SSO-Konfiguration

1. Fügen Sie die Anwendung aus dem Azure Marketplace hinzu.

2. Wählen Sie **Einmaliges Anmelden**.

3. Wählen Sie **Einmaliges Anmelden aktivieren** aus.

4. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die erforderlichen Konfigurationswerte ein.

    > [!NOTE]
    > Wenn die Anwendung eine Konfiguration benutzerdefinierter Ansprüche benötigt, konfigurieren Sie diese, bevor Sie 1-Klick-SSO ausführen.

5. Wenn 1-Klick-SSO für Ihre Azure Marketplace-Anwendung verfügbar ist, sehen Sie den folgenden Bildschirm. Möglicherweise müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie **Erweiterung installieren** auswählen.

   ![Installieren der Browsererweiterung zur sicheren Anmeldung bei „Meine Apps“](./media/one-click-sso-tutorial/install-myappssecure-extension.png)

6. Wählen Sie nach dem Hinzufügen der Erweiterung zum Browser **\<Application Name\> einrichten** aus. Sobald Sie zum Verwaltungsportal der Anwendung umgeleitet wurden, melden Sie sich als Administrator an.

   ![Einrichten des Anwendungsnamens](./media/one-click-sso-tutorial/setup-sso.png)

7. Die Browsererweiterung konfiguriert SSO für die Anwendung automatisch. Bestätigen Sie, indem Sie **Ja** auswählen.

   ![Speichern der automatisch ausgefüllten Daten](./media/one-click-sso-tutorial/save-autopopulate.png)

   > [!NOTE]
   > Wenn die SSO-Konfiguration für Ihre Anwendung zusätzliche Schritte erfordert, folgen Sie den Eingabeaufforderungen zur Ausführung dieser Schritte.

8. Klicken Sie nach Abschluss der Konfiguration auf **OK**, um die Änderungen zu speichern.

   ![Speichern der automatisch ausgefüllten Daten](./media/one-click-sso-tutorial/save-data.png)

9. In einem Bestätigungsfenster wird angezeigt, dass die SSO-Einstellungen erfolgreich konfiguriert wurden.

   ![SSO konfiguriert](./media/one-click-sso-tutorial/sso-configured.png)

10. Nach der erfolgreichen Konfiguration werden Sie von der Anwendung abgemeldet und kehren zum Azure-Portal zurück.

11. Sie können **Testen** auswählen, um das einmalige Anmelden zu testen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](../saas-apps/tutorial-list.md)
- [Was ist die Browsererweiterung zur sicheren Anmeldung bei „Meine Apps“?](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510)