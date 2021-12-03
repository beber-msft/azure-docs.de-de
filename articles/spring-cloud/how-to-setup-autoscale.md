---
title: Einrichten von Autoskalierung für Anwendungen
description: In diesem Artikel wird beschrieben, wie Sie über das Microsoft Azure-Portal oder die Azure CLI Autoskalierungseinstellungen für Ihre Anwendungen einrichten.
author: karlerickson
ms.author: karler
ms.service: spring-cloud
ms.topic: how-to
ms.date: 11/03/2021
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 67b5976e8df650ac8961cdb428338ca8147878d4
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2021
ms.locfileid: "131846632"
---
# <a name="set-up-autoscale-for-applications"></a>Einrichten von Autoskalierung für Anwendungen

**Dieser Artikel gilt für:** ✔️ Java ✔️ C#

In diesem Artikel wird beschrieben, wie Sie über das Microsoft Azure-Portal oder die Azure CLI Autoskalierungseinstellungen für Ihre Anwendungen einrichten.

Die in Azure Spring Cloud integrierte Autoskalierung trägt dazu bei, dass Anwendungen auch dann optimal funktionieren, wenn sich der Bedarf ändert. Azure Spring Cloud unterstützt horizontale Skalierung und Abskalieren, einschließlich der Änderung der Anzahl von App-Instanzen und des Lastenausgleichs.
 
## <a name="prerequisites"></a>Voraussetzungen

Um die folgenden Verfahren ausführen zu können, benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
* Eine bereitgestellte Azure Spring Cloud-Dienstinstanz. Informationen zu den ersten Schritten finden Sie unter [Schnellstart: Starten einer Java Spring-Anwendung mit der Azure CLI](./quickstart.md).
* Mindestens eine bereits in Ihrer Dienstinstanz erstellte Anwendung.

## <a name="navigate-to-the-autoscale-page-in-the-azure-portal"></a>Navigieren zur Seite „Autoskalierung“ im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Navigieren Sie zur Seite **Übersicht** von Azure Spring Cloud.
3. Wählen Sie die Ressourcengruppe aus, die Ihren Dienst enthält.
4. Wählen Sie im Menü im linken Navigationsbereich unter **Einstellungen** die Registerkarte **Apps** aus.
5. Wählen Sie die Anwendung aus, für die Sie die Autoskalierung einrichten möchten. In diesem Beispiel wird die Anwendung mit dem Namen **demo** ausgewählt. Dann sollte die Seite **Übersicht** der Anwendung angezeigt werden.
6. Navigieren Sie im Menü im linken Navigationsbereich unter **Einstellungen** zur Registerkarte **Aufskalieren**.
7. Wählen Sie die Bereitstellung aus, für die Sie die Autoskalierung einrichten möchten. Es sollten Optionen für die Autoskalierung angezeigt werden. Diese werden im folgenden Abschnitt veranschaulicht.

![Menü für die Autoskalierung](./media/spring-cloud-autoscale/autoscale-menu.png)

## <a name="set-up-autoscale-settings-for-your-application-in-the-azure-portal"></a>Einrichten von Einstellungen für die Autoskalierung für Ihre Anwendung über das Azure-Portal

Es gibt zwei Optionen für die Autoskalierungsbedarfsverwaltung:

* Manuelles Skalieren: Beibehalten einer festen Anzahl von Instanzen Im Standard-Tarif können Sie auf maximal 500 Instanzen aufskalieren. Dieser Wert ändert die Anzahl separat ausgeführter Instanzen der Anwendung.
* Benutzerdefinierte Autoskalierung: Skalieren nach einem beliebigen Zeitplan basierend auf Metriken

Wählen Sie im Azure-Portal aus, wie Sie skalieren möchten.  In der folgenden Abbildung sind die Option **Benutzerdefinierte Autoskalierung** und Moduseinstellungen dargestellt.

![Benutzerdefinierte Autoskalierung](./media/spring-cloud-autoscale/custom-autoscale.png)

## <a name="set-up-autoscale-settings-for-your-application-in-azure-cli"></a>Einrichten von Autoskalierungseinstellungen für Ihre Anwendung über die Azure CLI

Sie können Autoskalierungsmodi auch mithilfe der Azure CLI festlegen.  Die folgenden Befehle erstellen eine Einstellung und eine Regel für die Autoskalierung.

* Erstellen der Autoskalierungseinstellung:

   ```azurecli
   az monitor autoscale create -g demo-rg --resource /subscriptions/ffffffff-ffff-ffff-ffff-ffffffffffff/resourcegroups/demo-rg/providers/Microsoft.AppPlatform/Spring/autoscale/apps/demo/deployments/default --name demo-setting --min-count 1 --max-count 5 --count 1
   ```

* Erstellen der Autoskalierungsregel:

   ```azurecli
   az monitor autoscale rule create -g demo-rg --autoscale-name demo-setting --scale out 1 --cooldown 1 --condition "tomcat.global.request.total.count > 100 avg 1m where AppName == demo and Deployment == default"
   ```

## <a name="upgrade-to-the-standard-tier"></a>Upgrade auf den Standard-Tarif

Wenn Sie sich im Tarif „Basic“ befinden und durch diese Grenzwerte eingeschränkt sind, können Sie ein Upgrade auf den Tarif „Standard“ durchführen. Wechseln Sie zu diesem Zweck zum Menü **Preise**, indem Sie zuerst die Spalte **Standard-Tarif** auswählen und dann auf die Schaltfläche **Upgrade** klicken.

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über die Autoskalierung in Microsoft Azure](../azure-monitor/autoscale/autoscale-overview.md)
* [Überwachen der Autoskalierung mithilfe der Azure CLI](/cli/azure/monitor/autoscale)
