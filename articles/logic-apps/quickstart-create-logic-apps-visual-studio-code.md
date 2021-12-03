---
title: 'Schnellstart: Erstellen von Integrationsworkflows mit Azure Logic Apps in Visual Studio Code'
description: Erstellen und Verwalten von Workflowdefinitionen mit mehrinstanzenfähigen Azure Logic Apps in Visual Studio Code.
services: logic-apps
ms.suite: integration
ms.reviewer: azla
ms.topic: quickstart
ms.custom: mvc
ms.date: 05/25/2021
ms.openlocfilehash: 15f3481e42c543bead8ccf5f01654912cce5085f
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131078425"
---
# <a name="quickstart-create-and-manage-logic-app-workflow-definitions-with-multi-tenant-azure-logic-apps-and-visual-studio-code"></a>Schnellstart: Erstellen und Verwalten von Logik-App-Workflowdefinitionen mit mehrinstanzenfähigen Azure Logic Apps und Visual Studio Code

In dieser Schnellstartanleitung erfahren Sie, wie Sie Logik-App-Workflows erstellen und verwalten, mit denen Sie Aufgaben und Prozesse automatisieren können, die Apps, Daten, Systeme und Dienste in Organisationen und Unternehmen integrieren, indem Sie multiinstanzenfähige [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und Visual Studio Code verwenden. Sie können die zugrunde liegenden Workflowdefinitionen, die JavaScript Object Notation (JSON) verwenden, für Logik-Apps in einer codebasierten Umgebung erstellen und bearbeiten. Sie können auch vorhandene Logik-Apps bearbeiten, die bereits in Azure bereitgestellt wurden. Weitere Informationen zum Mehrinstanzenmodell im Vergleich zu einem Einzelinstanzenmodell finden Sie unter [Einzelinstanz im Vergleich zu Mehrinstanzen und Integrationsdienstumgebung](single-tenant-overview-compare.md).

Obwohl Sie dieselben Aufgaben im [Azure-Portal](https://portal.azure.com) und in Visual Studio ausführen können, können Sie in Visual Studio Code schneller starten, wenn Sie bereits mit Logik-App-Definitionen vertraut sind und direkt im Code arbeiten möchten. Beispielsweise können Sie bereits erstellte Logik-Apps deaktivieren, aktivieren, löschen und aktualisieren. Sie können zudem auf jeder Entwicklungsplattform, auf der Visual Studio Code ausgeführt wird – z.B. Linux, Windows und Mach, an Logik-Apps und Integrationskonten arbeiten.

Für diesen Artikel können Sie die Logik-App aus [dieser Schnellstartanleitung](../logic-apps/quickstart-create-first-logic-app-workflow.md) erstellen, die sich eher mit den grundlegenden Konzepten befasst. Sie können sich auch darüber informieren, wie Sie [die Beispiel-App in Visual Studio erstellen](quickstart-create-logic-apps-with-visual-studio.md) und [Apps über die Azure-Befehlszeilenschnittstelle (Azure Command-Line Interface, Azure CLI) erstellen und verwalten](quickstart-logic-apps-azure-cli.md). In Visual Studio Code sieht die Logik-App wie in diesem Beispiel aus:

![Beispieldefinition für Logik-App-Workflows](./media/quickstart-create-logic-apps-visual-studio-code/visual-studio-code-overview.png)

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie über die folgenden Elemente verfügen:

* Wenn Sie nicht über ein Azure-Konto und ein Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Sie benötigen Grundkenntnisse über die [Workflowdefinitionen von Logik-Apps](../logic-apps/logic-apps-workflow-definition-language.md) und deren JSON-basierte Struktur.

  Wenn Sie noch nicht mit Logik-Apps vertraut sind, probieren Sie [diese Schnellstartanleitung](../logic-apps/quickstart-create-first-logic-app-workflow.md) aus, in der Sie Ihre erste Logik-App im Azure-Portal erstellen und die sich eher mit den grundlegenden Konzepten befasst.

* Sie benötigen Zugriff auf das Web zum Anmelden bei Azure und in Ihrem Azure-Abonnement.

* Laden Sie diese Tools herunter, und installieren Sie sie, falls sie noch nicht vorhanden sind:

  * [Visual Studio Code, Version 1.25.1 oder höher](https://code.visualstudio.com/) (kostenlos)

  * Visual Studio Code-Erweiterung für Azure Logic Apps

    Sie können diese Erweiterung im [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-logicapps) oder direkt in Visual Studio Code herunterladen und installieren. Laden Sie Visual Studio Code nach der Installation unbedingt neu.

    ![Suche nach „Visual Studio Code-Erweiterung für Azure Logic Apps“](./media/quickstart-create-logic-apps-visual-studio-code/find-install-logic-apps-extension.png)

    Wählen Sie das Azure-Symbol auf der Visual Studio Code-Symbolleiste aus, um zu überprüfen, ob die Erweiterung ordnungsgemäß installiert wurde.

    ![Überprüfen der ordnungsgemäßen Installation der Erweiterung](./media/quickstart-create-logic-apps-visual-studio-code/confirm-installed-visual-studio-code-extension.png)

    Weitere Informationen finden Sie im [Marketplace für Erweiterungen](https://code.visualstudio.com/docs/editor/extension-gallery). Unter [Azure Logic Apps-Erweiterung für Visual Studio Code auf GitHub](https://github.com/Microsoft/vscode-azurelogicapps) können Sie Beiträge zur Open-Source-Version dieser Erweiterung einreichen.

* Falls Ihre Logik-App über eine Firewall kommunizieren muss, mit der der Datenverkehr auf bestimmte IP-Adressen beschränkt wird, muss Folgendes sichergestellt sein: In der Firewall muss der Zugriff für IP-Adressen, die vom Logic Apps-Dienst oder der Runtime in der Azure-Region Ihrer Logik-App genutzt werden, in [eingehender](logic-apps-limits-and-config.md#inbound) *und* [ausgehender](logic-apps-limits-and-config.md#outbound) Richtung zugelassen sein. Falls für Ihre Logik-App auch [verwaltete Connectors](../connectors/managed.md), z. B. Office 365 Outlook-Connector oder SQL-Connector, oder [benutzerdefinierte Connectors](/connectors/custom-connectors/) verwendet werden, muss in der Firewall in der Azure-Region Ihrer Logik-App zusätzlich der Zugriff für *alle* [ausgehenden IP-Adressen des verwalteten Connectors](logic-apps-limits-and-config.md#outbound) zulässig sein.

<a name="access-azure"></a>

## <a name="access-azure-from-visual-studio-code"></a>Zugreifen auf Azure aus Visual Studio Code

1. Öffnen Sie Visual Studio Code. Wählen Sie in der Visual Studio Code-Symbolleiste das Azure-Symbol aus.

   ![Auswählen des Azure-Symbols auf der Visual Studio Code-Symbolleiste](./media/quickstart-create-logic-apps-visual-studio-code/open-extensions-visual-studio-code.png)

1. Wählen Sie im Azure-Fenster unter **Logik-Apps** die Option **Bei Azure anmelden** aus. Wenn Sie von der Microsoft-Anmeldeseite dazu aufgefordert werden, melden Sie sich mit Ihrem Azure-Konto an.

   ![Auswahl von „Bei Azure anmelden“](./media/quickstart-create-logic-apps-visual-studio-code/sign-in-azure-visual-studio-code.png)

   1. Wenn die Anmeldung länger als üblich dauert, werden Sie von Visual Studio Code aufgefordert, sich über eine Microsoft-Authentifizierungswebsite anzumelden, indem Sie einen Gerätecode angeben. Wählen Sie **Gerätecode verwenden** aus, um sich stattdessen mit dem Code anzumelden.

      ![Stattdessen fortfahren mit Gerätecode](./media/quickstart-create-logic-apps-visual-studio-code/use-device-code-prompt.png)

   1. Um den Code zu kopieren, wählen Sie **Kopieren und öffnen** aus.

      ![Kopieren von Code für die Azure-Anmeldung](./media/quickstart-create-logic-apps-visual-studio-code/sign-in-prompt-authentication.png)

   1. Um ein neues Browserfenster zu öffnen und auf der Authentifizierungswebsite fortzufahren, wählen Sie **Link öffnen** aus.

      ![Bestätigen des Öffnens eines Browsers und Wechseln zur Authentifizierungswebsite](./media/quickstart-create-logic-apps-visual-studio-code/confirm-open-link.png)

   1. Geben Sie auf der Seite **Bei Ihrem Konto anmelden** Ihren Authentifizierungscode ein, und wählen Sie **Weiter** aus.

      ![Eingeben des Authentifizierungscodes für die Azure-Anmeldung](./media/quickstart-create-logic-apps-visual-studio-code/authentication-code-azure-sign-in.png)

1. Wählen Sie Ihr Azure-Konto aus. Nach dem Anmelden können Sie den Browser schließen und zu Visual Studio Code zurückkehren.

   Im Azure-Bereich werden in den Abschnitten **Logik-Apps** und **Integrationskonten** nun die mit Ihrem Konto verknüpften Azure-Abonnements angezeigt. Wenn die erwarteten Abonnements jedoch nicht angezeigt werden, oder wenn in den Abschnitten zu viele Abonnements angezeigt werden, führen Sie die folgenden Schritte aus:

   1. Bewegen Sie den Mauszeiger über die Bezeichnung **Logik-Apps**. Wenn die Symbolleiste angezeigt wird, wählen Sie **Abonnements auswählen** (Filtersymbol) aus.

      ![Suchen oder Filtern von Azure-Abonnements](./media/quickstart-create-logic-apps-visual-studio-code/find-or-filter-subscriptions.png)

   1. Wählen Sie in der angezeigten Liste die Abonnements aus, die angezeigt werden sollen.

1. Wählen Sie unter **Logik-Apps** das gewünschte Abonnement aus. Der Abonnementknoten wird erweitert und zeigt alle Logik-Apps an, die in diesem Abonnement vorhanden sind.

   ![Auswählen des Azure-Abonnements](./media/quickstart-create-logic-apps-visual-studio-code/select-azure-subscription.png)

   > [!TIP]
   > Unter **Integrationskonten** werden bei der Auswahl Ihres Abonnements alle Integrationskonten angezeigt, die in diesem Abonnement vorhanden sind.

<a name="create-logic-app"></a>

## <a name="create-new-logic-app"></a>Erstellen einer neuen Logik-App

1. Wenn Sie sich noch nicht über Visual Studio Code bei Ihrem Azure-Konto und -Abonnement angemeldet haben, befolgen Sie die [vorherigen Schritte, um sich jetzt anzumelden](#access-azure).

1. Öffnen Sie in Visual Studio Code unter **Logik-Apps** das Kontextmenü Ihres Abonnements, und wählen Sie **Logik-App erstellen** aus.

   ![Auswählen von „Logik-App erstellen“ im Abonnementmenü](./media/quickstart-create-logic-apps-visual-studio-code/create-logic-app-visual-studio-code.png)

   Eine Liste wird angezeigt, in der alle Azure-Ressourcengruppen in Ihrem Abonnement angezeigt werden.

1. Wählen Sie in der Ressourcengruppenliste entweder **Neue Ressourcengruppe erstellen** oder eine vorhandene Ressourcengruppe aus. Erstellen Sie für dieses Beispiel eine neue Ressourcengruppe.

   ![Erstellen einer neuen Azure-Ressourcengruppe](./media/quickstart-create-logic-apps-visual-studio-code/select-or-create-azure-resource-group.png)

1. Geben Sie einen Namen für Ihre Azure-Ressourcengruppe ein, und drücken Sie die EINGABETASTE.

   ![Angeben eines Namens für Ihre Ressourcengruppe](./media/quickstart-create-logic-apps-visual-studio-code/enter-name-resource-group.png)

1. Wählen Sie die Azure-Region aus, in der Sie die Metadaten Ihrer Logik-App speichern möchten.

   ![Auswählen des Azure-Standorts zum Speichern von Logik-App-Metadaten](./media/quickstart-create-logic-apps-visual-studio-code/select-azure-location-new-resources.png)

1. Geben Sie einen Namen für Ihre Logik-App ein, und drücken Sie die EINGABETASTE.

   ![Angeben eines Namens für Ihre Logik-App](./media/quickstart-create-logic-apps-visual-studio-code/enter-name-logic-app.png)

   Ihre neue und leere Logik-App wird nun im Azure-Fenster unter Ihrem Azure-Abonnement angezeigt. Visual Studio Code öffnet auch eine JSON-Datei (.logicapp.json), die eine Skelettworkflowdefinition für Ihre Logik-App enthält. Nun können Sie die Erstellung der Workflowdefinition ihrer Logik-App in dieser JSON-Datei manuell starten. Eine technische Referenz zu Struktur und Syntax einer Workflowdefinition finden Sie im [Schemareferenzleitfaden für die Workflowdefinitionssprache in Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md).

   ![Leere JSON-Datei zur Definition des Logik-App-Workflows](./media/quickstart-create-logic-apps-visual-studio-code/empty-logic-app-workflow-definition.png)

   Im Folgenden sehen Sie eine Beispieldefinition für einen Logik-App-Workflow, der mit einem RSS-Trigger und einer Office 365 Outlook-Aktion beginnt. JSON-Elemente werden in der Regel alphabetisch in jedem Abschnitt angezeigt. In diesem Beispiel werden diese Elemente jedoch ungefähr in der Reihenfolge angezeigt, in dem die Schritte der Logik-App im Designer aufgeführt sind.

   > [!IMPORTANT]
   > Wenn Sie dieses Beispiel einer Logik-App-Definition wiederverwenden möchten, benötigen Sie ein Organisationskonto, z. B. @fabrikam.com. Stellen Sie sicher, dass Sie die fiktive E-Mail-Adresse durch Ihre eigene E-Mail-Adresse ersetzen. Um einen anderen E-Mail-Connector wie z. B. Outlook.com oder Gmail zu verwenden, ersetzen Sie die `Send_an_email_action`-Aktion durch eine ähnliche Aktion, die bei einem [von Azure Logic Apps unterstützten E-Mail-Connector](../connectors/apis-list.md) verfügbar ist.
   >
   > Wenn Sie den Gmail-Connector verwenden möchten, können nur G-Suite-Geschäftskonten diesen Connector ohne Einschränkung in Logik-Apps verwenden. 
   > Wenn Sie über ein Gmail-Consumerkonto verfügen, können Sie diesen Connector nur mit bestimmten von Google genehmigten Diensten verwenden, oder Sie können [eine Google-Client-App erstellen, die für die Authentifizierung mit Ihrem Gmail-Connector verwendet werden soll](/connectors/gmail/#authentication-and-bring-your-own-application). 
   > Weitere Informationen finden Sie unter [Datensicherheit und Datenschutzrichtlinien für Google-Connectors in Azure Logic Apps](../connectors/connectors-google-data-security-privacy-policy.md).

   ```json
   {
      "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "$connections": {
            "defaultValue": {},
            "type": "Object"
         }
      },
      "triggers": {
         "When_a_feed_item_is_published": {
            "recurrence": {
               "frequency": "Minute",
               "interval": 1
            },
            "splitOn": "@triggerBody()?['value']",
            "type": "ApiConnection",
            "inputs": {
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['rss']['connectionId']"
                  }
               },
               "method": "get",
               "path": "/OnNewFeed",
               "queries": {
                  "feedUrl": "http://feeds.reuters.com/reuters/topNews"
               }
            }
         }
      },
      "actions": {
         "Send_an_email_(V2)": {
            "runAfter": {},
            "type": "ApiConnection",
            "inputs": {
               "body": {
                  "Body": "<p>Title: @{triggerBody()?['title']}<br>\n<br>\nDate published: @{triggerBody()?['updatedOn']}<br>\n<br>\nLink: @{triggerBody()?['primaryLink']}</p>",
                  "Subject": "RSS item: @{triggerBody()?['title']}",
                  "To": "sophia-owen@fabrikam.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['office365']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/v2/Mail"
            }
         }
      },
      "outputs": {}
   }
   ```

1. Wenn Sie fertig sind, speichern Sie die Workflowdefinition Ihrer Logik-App. (Menü „Datei“ > „Speichern“, oder drücken Sie STRG+S)

1. Wenn Sie zum Hochladen Ihrer Logik-App in Ihr Azure-Abonnement aufgefordert werden, wählen Sie **Hochladen** aus.

   In diesem Schritt wird Ihre Logik-App im [Azure-Portal](https://portal.azure.com) veröffentlicht, sodass sie in Azure in Betrieb geht.

   ![Hochladen einer neuen Logik-App in Ihr Azure-Abonnement](./media/quickstart-create-logic-apps-visual-studio-code/upload-new-logic-app.png)

## <a name="view-logic-app-in-designer"></a>Anzeigen der Logik-App im Designer

In Visual Studio Code können Sie Ihre Logik-App in der schreibgeschützten Entwurfsansicht öffnen. Obwohl Sie Ihre Logik-App im Designer nicht bearbeiten können, können Sie den Workflow ihrer Logik-App mithilfe der Designeransicht visuell überprüfen.

Öffnen Sie im Azure-Fenster unter **Logik-Apps** das Kontextmenü ihrer Logik-App, und wählen Sie **Im Designer öffnen** aus.

Der schreibgeschützte Designer wird in einem separaten Fenster geöffnet und zeigt den Workflow ihrer Logik-App an, z. B.:

![Anzeigen der Logik-App im schreibgeschützten Designer](./media/quickstart-create-logic-apps-visual-studio-code/logic-app-designer-view.png)

## <a name="view-in-azure-portal"></a>Ansicht im Azure-Portal

Führen Sie die folgenden Schritte aus, um Ihre Logik-App im Azure-Portal zu überprüfen:

1. Melden Sie sich mit demselben Azure-Konto und -Abonnement, die Ihrer Logik-App zugeordnet sind, beim [Azure-Portal](https://portal.azure.com) an.

1. Geben Sie im Suchfeld des Azure-Portals den Namen Ihrer Logik-App ein. Wählen Sie in der Ergebnisliste ihre Logik-App aus.

   ![Ihre neue Logik-App im Azure-Portal](./media/quickstart-create-logic-apps-visual-studio-code/published-logic-app-in-azure.png)

<a name="edit-logic-app"></a>

## <a name="edit-deployed-logic-app"></a>Bearbeiten der bereitgestellten Logik-App

In Visual Studio Code können Sie die Workflowdefinition für eine bereits in Azure bereitgestellte Logik-App öffnen und bearbeiten.

> [!IMPORTANT] 
> Bevor Sie eine aktive, in der Produktionsumgebung ausgeführte Logik-App bearbeiten, vermeiden Sie das Risiko, diese Logik-App zu beschädigen, und minimieren Sie die Unterbrechung, indem Sie [Ihre Logik-App deaktivieren](#disable-enable-logic-apps).

1. Wenn Sie sich noch nicht über Visual Studio Code bei Ihrem Azure-Konto und -Abonnement angemeldet haben, befolgen Sie die [vorherigen Schritte, um sich jetzt anzumelden](#access-azure).

1. Erweitern Sie im Azure-Fenster unter **Logik-Apps** Ihr Azure-Abonnement, und wählen Sie die gewünschte Logik-App aus.

1. Öffnen Sie das Logik-App-Menü, und wählen Sie **In Editor öffnen** aus. Alternativ können Sie neben dem Namen Ihrer Logik-App das Bearbeitungssymbol auswählen.

   ![Öffnen des Editors für eine vorhandene Logik-App](./media/quickstart-create-logic-apps-visual-studio-code/open-editor-existing-logic-app.png)

   Visual Studio Code öffnet die .logicapp.json-Datei in Ihrem lokalen temporären Ordner, damit Sie die Workflowdefinition Ihrer Logik-App anzeigen können.

   ![Anzeigen der Workflowdefinition einer veröffentlichten Logik-App](./media/quickstart-create-logic-apps-visual-studio-code/edit-published-logic-app-workflow-definition.png)

1. Nehmen Sie die gewünschten Änderungen an der Workflowdefinition Ihrer Logik-App vor.

1. Speichern Sie Ihre Änderungen, wenn Sie fertig sind. (Menü „Datei“ > „Speichern“, oder drücken Sie STRG+S)

1. Wenn Sie aufgefordert werden, die Änderungen hochzuladen und Ihre vorhandene Logik-App im Azure-Portal zu *überschreiben*, wählen Sie **Hochladen** aus.

   In diesem Schritt werden Ihre Updates der Logik-App im [Azure-Portal](https://portal.azure.com) veröffentlicht.

   ![Hochladen der Änderungen an der Logik-App-Definition in Azure](./media/quickstart-create-logic-apps-visual-studio-code/upload-logic-app-changes.png)

## <a name="view-or-promote-other-versions"></a>Anzeigen oder Höherstufen anderer Versionen

In Visual Studio Code können Sie die früheren Versionen der Logik-App öffnen und überprüfen. Sie können auch eine frühere Version auf die aktuelle Version höher stufen.

> [!IMPORTANT] 
> Bevor Sie eine aktive, in der Produktionsumgebung ausgeführte Logik-App ändern, vermeiden Sie das Risiko, diese Logik-App zu beschädigen, und minimieren Sie die Unterbrechung, indem Sie [Ihre Logik-App deaktivieren](#disable-enable-logic-apps).

1. Erweitern Sie im Azure-Fenster unter **Logik-Apps** Ihr Azure-Abonnement, sodass Sie alle Logik-Apps in diesem Abonnement sehen können.

1. Erweitern Sie Ihre Logik-App in Ihrem Abonnement, und erweitern Sie **Versionen**.

   In der Liste **Versionen** werden – sofern vorhanden – die früheren Versionen ihrer Logik-App angezeigt.

   ![Die früheren Versionen Ihrer Logik-App](./media/quickstart-create-logic-apps-visual-studio-code/view-previous-versions.png)

1. Um eine frühere Version anzuzeigen, wählen Sie einen der folgenden Schritte aus:

   * Um die JSON-Definition anzuzeigen, wählen Sie unter **Versionen** die Versionsnummer für diese Definition aus. Öffnen Sie alternativ das Kontextmenü der Version, und wählen Sie **Im Editor öffnen** aus.

     Auf dem lokalen Computer wird eine neue Datei geöffnet und die JSON-Definition dieser Version angezeigt.

   * Um die Version in der schreibgeschützten Designeransicht anzuzeigen, öffnen Sie das Kontextmenü dieser Version, und wählen Sie **Im Designer öffnen** aus.

1. Um eine frühere Version auf die aktuelle Version höherzustufen, führen Sie diese Schritte aus:

   1. Öffnen Sie unter **Versionen** das Kontextmenü der früheren Version, und wählen Sie **Höher stufen** aus.

      ![Höherstufen der früheren Version](./media/quickstart-create-logic-apps-visual-studio-code/promote-earlier-version.png)

   1. Um fortzufahren, nachdem Visual Studio Code Sie zur Bestätigung aufgefordert hat, wählen Sie **Ja** aus.

      ![Bestätigen des Höherstufens der früheren Version](./media/quickstart-create-logic-apps-visual-studio-code/confirm-promote-version.png)

      Visual Studio Code stuft die ausgewählte Version auf die aktuelle Version herauf und weist der höher gestuften Version eine neue Nummer zu. Die zuvor aktuelle Version wird jetzt unter der höher gestuften Version angezeigt.

<a name="disable-enable-logic-apps"></a>

## <a name="disable-or-enable-logic-apps"></a>Deaktivieren oder Aktivieren von Logik-Apps

Wenn Sie in Visual Studio Code eine veröffentlichte Logik-App bearbeiten und die Änderungen speichern, *überschreiben* Sie Ihre bereits bereitgestellte App. Deaktivieren Sie Ihre Logik-App zunächst, um eine Beschädigung Ihrer Logik-App zu vermeiden und die Unterbrechung zu minimieren. Anschließend können Sie Ihre Logik-App wieder aktivieren, nachdem Sie sich vergewissert haben, dass sie weiterhin funktioniert.

> [!NOTE]
> Das Deaktivieren einer Logik-App wirkt sich wie folgt auf Workflowinstanzen aus:
>
> * Der Logic Apps-Dienst setzt alle aktiven und ausstehenden Ausführungen fort, bis sie abgeschlossen sind. Basierend auf dem Volume oder Backlog kann es einige Zeit dauern, bis dieser Prozess abgeschlossen ist.
>
> * Der Logic Apps-Dienst erstellt keine neuen Workflowinstanzen und führt keine neuen Workflowinstanzen aus.
>
> * Der Trigger wird nicht ausgelöst, wenn die definierten Bedingungen beim nächsten Mal erfüllt werden. Der Triggerstatus merkt sich den Punkt, an dem die Logik-App angehalten wurde. Wenn Sie die Logik-App erneut aktivieren, wird der Trigger für alle nicht verarbeiteten Elemente seit der letzten Ausführung ausgelöst.
>
>   Wenn Sie die Auslösung eines Triggers für nicht verarbeitete Elemente seit der letzten Ausführung verhindern möchten, löschen Sie den Triggerstatus, bevor Sie die Logik-App wieder aktivieren:
>
>   1. Bearbeiten Sie in der Logik-App einen beliebigen Teil des Workflowtriggers.
>   1. Speichern Sie die Änderungen. Durch diesen Schritt wird der aktuelle Status Ihres Triggers zurückgesetzt.
>   1. Aktivieren Sie Ihre Logik-App wieder.

1. Wenn Sie sich noch nicht über Visual Studio Code bei Ihrem Azure-Konto und -Abonnement angemeldet haben, befolgen Sie die [vorherigen Schritte, um sich jetzt anzumelden](#access-azure).

1. Erweitern Sie im Azure-Fenster unter **Logik-Apps** Ihr Azure-Abonnement, sodass Sie alle Logik-Apps in diesem Abonnement sehen können.

   1. Um die gewünschte Logik-App zu deaktivieren, öffnen Sie das Menü der Logik-App, und wählen Sie **Deaktivieren** aus.

      ![Ihre Logik-App deaktivieren](./media/quickstart-create-logic-apps-visual-studio-code/disable-published-logic-app.png)

   1. Wenn Sie bereit sind, Ihre Logik-App wieder zu aktivieren, öffnen Sie das Logik-App-Menü, und wählen Sie **Aktivieren** aus.

      ![Ihre Logik-App aktivieren](./media/quickstart-create-logic-apps-visual-studio-code/enable-published-logic-app.png)

<a name="delete-logic-apps"></a>

## <a name="delete-logic-apps"></a>Löschen von Logik-Apps

Das Löschen einer Logik-App wirkt sich wie folgt auf Workflowinstanzen aus:

* Der Logic Apps-Dienst unterbricht alle aktiven und ausstehenden Ausführungen so gut wie möglich.

  Selbst bei einer großen Menge oder einem umfangreichen Backlog werden die meisten Ausführungen abgebrochen, bevor sie abgeschlossen oder gestartet werden. Es kann jedoch einige Zeit dauern, bis der Abbruchvorgang abgeschlossen ist. In der Zwischenzeit werden möglicherweise einige Ausführungen gestartet, während der Dienst den Abbruchprozess durchläuft.

* Der Logic Apps-Dienst erstellt keine neuen Workflowinstanzen und führt keine neuen Workflowinstanzen aus.

* Wenn Sie einen Workflow löschen und dann denselben Workflow neu erstellen, hat der neu erstellte Workflow nicht die gleichen Metadaten wie der gelöschte Workflow. Sie müssen jeden Workflow, der den gelöschten Workflow aufgerufen hat, neu speichern. Auf diese Weise ruft der Aufrufer die richtigen Informationen für den neu erstellten Workflow ab. Andernfalls schlagen Aufrufe des neu erstellten Workflows mit einem `Unauthorized`-Fehler fehl. Dieses Verhalten gilt auch für Workflows, die Artefakte in Integrationskonten verwenden, sowie Workflows, die Azure-Funktionen aufrufen.

1. Wenn Sie sich noch nicht über Visual Studio Code bei Ihrem Azure-Konto und -Abonnement angemeldet haben, befolgen Sie die [vorherigen Schritte, um sich jetzt anzumelden](#access-azure).

1. Erweitern Sie im Azure-Fenster unter **Logik-Apps** Ihr Azure-Abonnement, sodass Sie alle Logik-Apps in diesem Abonnement sehen können.

1. Navigieren Sie zu der Logik-App, die Sie löschen möchten, öffnen Sie das Menü der Logik-App, und wählen Sie **Löschen** aus.

   ![Löschen Ihrer Logik-App](./media/quickstart-create-logic-apps-visual-studio-code/delete-logic-app.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einzelinstanzenbasierter Logik-App-Workflows in Visual Studio Code](../logic-apps/create-single-tenant-workflows-visual-studio-code.md)
