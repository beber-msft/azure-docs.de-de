---
title: Beheben von Problemen beim Registrieren für ein neues Konto im Azure-Portal
description: Lösen eines Problems beim Registrieren für ein neues Konto im Microsoft Azure-Portal.
services: cost-management-billing
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: troubleshooting
ms.date: 10/07/2021
ms.author: banders
ms.openlocfilehash: ae47b3e96dcb8bd7e7e07297313892dcb24405d0
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2021
ms.locfileid: "131464735"
---
# <a name="troubleshoot-issues-when-you-sign-up-for-a-new-account-in-the-azure-portal"></a>Beheben von Problemen beim Registrieren für ein neues Konto im Azure-Portal

Unter Umständen tritt ein Problem auf, wenn Sie sich im Microsoft Azure-Portal für ein neues Konto registrieren. In dieser Kurzanleitung werden Sie durch den Anmeldevorgang geleitet und einige häufig auftretende Probleme bei den einzelnen Schritten erörtert.

> [!NOTE]
> Wenn Sie bereits über ein Konto verfügen und Anleitungen zur Behandlung von Anmeldeproblemen suchen, lesen Sie die Informationen unter [Beheben von Problemen bei der Anmeldung für ein Azure-Abonnement](./troubleshoot-sign-in-issue.md).

## <a name="before-you-begin"></a>Voraussetzungen

Überprüfen Sie vor Beginn der Registrierung Folgendes:

- Die Informationen Ihres Azure-Profils (einschließlich Kontakt-E-Mail-Adresse, Anschrift und Telefonnummer) stimmen.
- Ihre Kreditkarteninformationen stimmen.
- Sie verfügen nicht bereits über ein Microsoft-Konto mit denselben Informationen.

## <a name="guided-walkthrough-of-azure-sign-up"></a>Exemplarische Vorgehensweisen zur Registrierung bei Azure

Die Umgebung zur Registrierung bei Azure besteht aus vier Abschnitten:

- Persönliche Informationen
- Identitätsüberprüfung per Telefon
- Identitätsüberprüfung per Karte
- Vereinbarung

Diese exemplarische Vorgehensweise enthält Beispiele der ordnungsgemäßen Informationen zur Registrierung für ein Azure-Konto. In jedem Abschnitt werden auch einige häufig auftretende Probleme und deren Behebung erläutert.

## <a name="about-you"></a>Persönliche Informationen

Wenn Sie sich zum ersten Mal für Azure registrieren, müssen Sie einige Informationen zur eigenen Person bereitstellen. Dazu gehören die folgenden Angaben:

- Land oder Region
- Vorname
- Nachname
- E-Mail-Adresse
- Telefonnummer
- Kreditkarteninformationen
 
### <a name="common-issues-and-solutions"></a>Bekannte Probleme und Lösungen

#### <a name="you-see-the-message-we-cannot-proceed-with-sign-up-due-to-an-issue-with-your-account-please-contact-billing-support"></a>Die Meldung „Die Registrierung kann aufgrund eines Problems mit Ihrem Konto nicht fortgesetzt werden. Wenden Sie sich an den Abrechnungssupport“ wird angezeigt.

Gehen Sie folgendermaßen vor, um diesen Fehler zu beheben:

1. Melden Sie sich beim [Microsoft-Kontocenter](https://account.microsoft.com/) an.
1. Wählen Sie oben auf der Seite **Ihre Informationen** aus.
1. Vergewissern Sie sich, dass Ihre Abrechnungs- und Versanddetails vollständig und gültig sind.
1.  Vergewissern Sie sich bei der Registrierung für das Azure-Abonnement, dass die beim Registrieren der Kreditkarte eingegebene Rechnungsadresse mit den Angaben der Bank übereinstimmt.

Wenn Sie die Fehlermeldung weiterhin erhalten, führen Sie die Registrierung in einem anderen Browser aus.

Was gilt für InPrivate-Browsen?

#### <a name="free-trial-is-not-available"></a>Kostenlose Testversion nicht verfügbar

Haben Sie bereits einmal ein Azure-Abonnement verwendet? Laut Vereinbarung in den Azure-Nutzungsbedingungen ist die Aktivierung einer kostenlosen Testversion nur für Benutzer eingeschränkt, die neu bei Azure sind. Wenn Sie bereits einen anderen Typ von Azure-Abonnement verwendet haben, können Sie keine kostenlose Testversion aktivieren. Sie könnten sich auch für ein [Abonnement mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) registrieren.

#### <a name="you-see-the-message-you-are-not-eligible-for-an-azure-subscription"></a>Die Meldung „Sie besitzen keine Berechtigung für ein Azure-Abonnement“ wird angezeigt.

Um dieses Problem zu beheben, überprüfen Sie, ob die folgenden Punkte zutreffen:

- Die für Ihr Azure-Kontoprofil angegebenen Informationen (einschließlich Kontakt-E-Mail-Adresse, Anschrift und Telefonnummer) sind richtig.
- Die Kreditkarteninformationen sind richtig.
- Sie verfügen nicht bereits über ein Microsoft-Konto, das dieselben Informationen verwendet.

#### <a name="you-see-the-message-your-current-account-type-is-not-supported"></a>Die Meldung „Ihr aktueller Kontotyp wird nicht unterstützt“ wird angezeigt.

Dieses Problem kann auftreten, wenn das Konto in einem [nicht verwalteten Azure AD-Verzeichnis](../../active-directory/enterprise-users/directory-self-service-signup.md) registriert ist und nicht im Azure AD-Verzeichnis Ihrer Organisation vorhanden ist.
Um dieses Problem zu beheben, registrieren Sie das Azure-Konto unter Verwendung eines anderen Kontos, oder übernehmen Sie das nicht verwaltete AD-Verzeichnis. Weitere Informationen finden Sie unter [Übernehmen eines nicht verwalteten Verzeichnisses als Administrator in Azure Active Directory](../../active-directory/enterprise-users/domains-admin-takeover.md).

## <a name="identity-verification-by-phone"></a>Identitätsüberprüfung per Telefon

![Identitätsüberprüfung per Telefon](./media/troubleshoot-azure-sign-up/2.png)
 
Wenn Sie die Textnachricht oder den Telefonanruf erhalten haben, geben Sie den empfangenen Code in das Textfeld ein.

### <a name="common-issues-and-solutions"></a>Bekannte Probleme und Lösungen

#### <a name="no-verification-text-message-or-phone-call"></a>Weder SMS noch Telefonanruf mit Überprüfungscode

Der Überprüfungsprozess bei der Registrierung ist zwar in der Regel schnell, es kann aber bis zu vier Minuten dauern, bis ein Überprüfungscode übermittelt wird.

Hier sind einige zusätzlichen Tipps:

- Sie können eine beliebige Telefonnummer zur Überprüfung nutzen, solange sie den Anforderungen entspricht. Die zur Überprüfung eingegebene Telefonnummer wird nicht als Kontaktnummer für das Konto gespeichert.
  - Für den Überprüfungsvorgang per Telefon kann keine VoIP-Telefonnummer (Voice-over-IP) verwendet werden.
  - Stellen Sie sicher, dass Ihr Telefon Anrufe oder SMS-Nachrichten von einer Telefonnummer aus den USA empfangen kann.
- Überprüfen Sie im Dropdownmenü die eingegebene Telefonnummer einschließlich Landesvorwahl.
- Wenn Ihr Telefon keine Textnachrichten (SMS) empfängt, versuchen Sie es mit der Option **Mich anrufen**.

## <a name="identity-verification-by-card"></a>Identitätsüberprüfung per Karte

![Identitätsüberprüfung per Karte](./media/troubleshoot-azure-sign-up/3.png)
 
### <a name="common-issues-and-solutions"></a>Bekannte Probleme und Lösungen

#### <a name="credit-card-declined-or-not-accepted"></a>Kreditkarte abgelehnt bzw. nicht akzeptiert

Virtuelle oder Prepaid-Kreditkarten oder -Debitkarten werden als Zahlungsmittel für Azure-Abonnements nicht akzeptiert. Weitere Informationen zu möglichen Ursachen für die Ablehnung Ihrer Karte finden Sie unter [Behandeln von Problemen mit einer abgelehnten Karte bei der Azure-Registrierung](./troubleshoot-declined-card.md).

#### <a name="credit-card-form-doesnt-support-my-billing-address"></a>Das Kreditkartenformular akzeptiert meine Rechnungsadresse nicht.

Ihre Rechnungsadresse muss sich in dem Land befinden, das Sie im Abschnitt **Persönliche Informationen** ausgewählt haben. Vergewissern Sie sich, dass Sie das richtige Land angegeben haben.

#### <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Statusanzeige hängt im Abschnitt „Identitätsüberprüfung per Karte“

Um die Identitätsprüfung mithilfe der Karte abzuschließen, müssen Cookies von Drittanbietern in Ihrem Browser zugelassen sein.

Führen Sie die folgenden Schritte aus, um die Cookie-Einstellungen Ihres Browsers zu aktualisieren.

1. Aktualisieren Sie die Cookie-Einstellungen.
   - Wenn Sie eine **Chrome** verwenden:
     - Wählen Sie **Einstellungen** > **Erweiterte Einstellungen anzeigen** > **Datenschutz** > **Inhaltseinstellungen** aus. Deaktivieren Sie **Cookies und Websitedaten von Drittanbietern blockieren**.

   - Wenn Sie **Microsoft Edge** verwenden:
     - Wählen Sie **Einstellungen** > **Erweiterte Einstellungen anzeigen** > **Cookies** > **Keine Cookies blockieren** aus.

1. Aktualisieren Sie die Azure-Registrierungsseite, und überprüfen Sie dann, ob das Problem behoben ist.
1. Wenn das Problem durch die Aktualisierung nicht behoben wurde, beenden Sie den Browser, starten Sie ihn neu, und wiederholen Sie den Vorgang.

### <a name="i-saw-a-charge-on-my-free-trial-account"></a>In meinem kostenlosen Testkonto werden Gebühren angezeigt

Nach der Registrierung wird möglicherweise eine kurze temporäre Prüfsperre in Ihrem Kreditkartenkonto angezeigt. Diese Sperre wird innerhalb von 3 bis 5 Tagen aufgehoben. Wenn Sie Bedenken bezüglich der Verwaltung von Kosten haben, informieren Sie sich über das [Analysieren unerwarteter Gebühren](../understand/analyze-unexpected-charges.md).

## <a name="agreement"></a>Vereinbarung

Schließen Sie die Vereinbarung ab.

## <a name="other-issues"></a>Andere Probleme

### <a name="cant-activate-azure-benefit-plan-like-visual-studio-bizspark-bizsparkplus-or-mpn"></a>Ich kann kein Azure-Vorteilsprogramm wie Visual Studio, BizSpark, BizSparkPlus oder MPN aktivieren

Überprüfen Sie, ob Sie die richtigen Anmeldeinformationen verwenden. Überprüfen Sie anschließend das Vorteilsprogramm, um sich zu vergewissern, dass Sie berechtigt sind.
- Visual Studio
  - Überprüfen Sie auf der [Visual Studio-Kontoseite](https://my.visualstudio.com/Benefits) Ihren Berechtigungsstatus.
  - Falls Sie Ihren Status nicht überprüfen können, sollten Sie sich an den [Support für Visual Studio-Abonnements](https://visualstudio.microsoft.com/subscriptions/support/) wenden.
- Microsoft für Startups
  - Melden Sie sich beim [Microsoft für Startups-Portal](https://startups.microsoft.com/#start-two) an, um Ihren Berechtigungsstatus für Microsoft für Startups zu überprüfen.
  - Wenn Sie Ihren Status nicht überprüfen können, erhalten Sie in den [Microsoft für Startups-Foren](https://www.microsoftpartnercommunity.com/t5/Microsoft-for-Startups/ct-p/Microsoft_Startups) Hilfe.
- MPN
  - Melden Sie sich beim [MPN-Portal](https://mspartner.microsoft.com/Pages/Locale.aspx) an, um Ihren Berechtigungsstatus zu überprüfen. Ihnen stehen ggf. weitere Leistungen zu, wenn Sie über die entsprechenden [Kompetenzen für Cloudplattformen](https://mspartner.microsoft.com/pages/membership/cloud-platform-competency.aspx) verfügen.
  - Wenn Sie Ihren Status nicht überprüfen können, wenden Sie sich an den [MPN-Support](https://mspartner.microsoft.com/Pages/Support/Premium/contact-support.aspx).

### <a name="cant-activate-new-azure-in-open-subscription"></a>Ich kann kein neues Azure In Open-Abonnement aktivieren

Um ein Azure In Open-Abonnement zu erstellen, müssen Sie über einen gültigen OSA-Schlüssel (Online Service Activation) verfügen, dem mindestens ein Azure In Open-Token zugeordnet ist. Wenn Sie keinen OSA-Schlüssel haben, wenden Sie sich an einen der unter [Microsoft Pinpoint](https://pinpoint.microsoft.com/) aufgeführten Microsoft-Partner.

## <a name="additional-help-resources"></a>Zusätzliche Hilferessourcen

Weitere Artikel für die Problembehandlung bei Azure-Abrechnung und -Abonnements

- [Karte abgelehnt](./troubleshoot-declined-card.md)
- [Probleme bei der Abonnementanmeldung](./troubleshoot-sign-in-issue.md)
- [Keine Abonnements gefunden](./no-subscriptions-found.md)
- [Enterprise-Kostenansicht deaktiviert](./enterprise-mgmt-grp-troubleshoot-cost-view.md)

## <a name="contact-us-for-help"></a>Setzen Sie sich mit uns in Verbindung, um Hilfe zu erhalten.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [Dokumentation zu Azure Cost Management + Billing](../index.yml).