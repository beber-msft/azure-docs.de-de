---
title: Authentifizieren einer Anwendung für den Zugriff auf Azure Service Bus-Entitäten
description: Dieser Artikel enthält Informationen zur Authentifizierung einer Anwendung mit Azure Active Directory, um auf Azure Service Bus-Entitäten (Warteschlangen, Themen, usw.) zuzugreifen.
ms.topic: conceptual
ms.date: 06/14/2021
ms.custom: subject-rbac-steps
ms.openlocfilehash: 3ca9284746460b3ab3369b4c9c00d7951784cff8
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131064010"
---
# <a name="authenticate-and-authorize-an-application-with-azure-active-directory-to-access-azure-service-bus-entities"></a>Authentifizieren und Autorisieren einer Anwendung mit Azure Active Directory für den Zugriff auf Azure Service Bus-Entitäten
Azure Service Bus unterstützt die Verwendung von Azure Active Directory (Azure AD), um Anforderungen an Service Bus-Entitäten (Warteschlangen, Themen, Abonnements oder Filter) zu autorisieren. Mit Azure AD können Sie die rollenbasierte Zugriffssteuerung von Azure (Azure RBAC) zum Gewähren von Berechtigungen für einen Sicherheitsprinzipal verwenden, bei dem es sich um einen Benutzer, eine Gruppe oder einen Anwendungsdienstprinzipal handeln kann. Weitere Informationen zu Rollen und Rollenzuweisungen finden Sie unter [Grundlegendes zu den verschiedenen Rollen](../role-based-access-control/overview.md).

## <a name="overview"></a>Übersicht
Wenn ein Sicherheitsprinzipal (ein Benutzer, eine Gruppe oder eine Anwendung) versucht, auf eine Service Bus-Entität zuzugreifen, muss die Anforderung autorisiert werden. Mit Azure AD ist der Zugriff auf eine Ressource ein zweistufiger Prozess. 

 1. Zunächst wird die Identität des Sicherheitsprinzipals authentifiziert, und ein OAuth 2.0-Token wird zurückgegeben. Der Ressourcenname zum Anfordern eines Tokens ist `https://servicebus.azure.net`.
 1. Anschließend wird das Token als Teil einer Anforderung an den Service Bus-Dienst übergeben, um den Zugriff auf die angegebene Ressource zu autorisieren.

Für den Authentifizierungsschritt ist es erforderlich, dass eine Anwendungsanforderung zur Laufzeit ein OAuth 2.0-Zugriffstoken enthält. Wenn eine Anwendung in einer Azure-Entität, z.B. einem virtuellen Azure-Computer, einer VM-Skalierungsgruppe oder einer Azure Functions-App, ausgeführt wird, kann der Zugriff auf die Ressourcen über eine verwaltete Identität erfolgen. Informationen zum Authentifizieren von Anforderungen, die von einer verwalteten Identität an den Service Bus-Dienst übermittelt werden, finden Sie unter [Authentifizieren des Zugriffs auf Azure Service Bus-Ressourcen mit Azure Active Directory und verwalteten Identitäten für Azure Ressourcen](service-bus-managed-service-identity.md). 

Der Autorisierungsschritt erfordert, dass dem Sicherheitsprinzipal mindestens eine Azure-Rolle zugewiesen wird. Azure Service Bus stellt Azure-Rollen bereit, die Berechtigungssätze für Service Bus-Ressourcen enthalten. Die möglichen Berechtigungen eines Sicherheitsprinzipals sind durch die Rollen vorgegeben, die dem Prinzipal zugewiesen sind. Weitere Informationen zur Zuweisung von Azure-Rollen zu Azure Service Bus finden Sie unter [Integrierte Azure-Rollen für Azure Service Bus](#azure-built-in-roles-for-azure-service-bus). 

Native Anwendungen und Webanwendungen, die Anforderungen an Service Bus senden, können die Autorisierung ebenfalls mit Azure AD durchführen. In diesem Artikel erfahren Sie, wie Sie ein Zugriffstoken anfordern und dieses zum Autorisieren von Anforderungen für Service Bus-Ressourcen verwenden. 


## <a name="assigning-azure-roles-for-access-rights"></a>Zuweisen von Azure-Rollen für Zugriffsrechte
Azure Active Directory (Azure AD) autorisiert Rechte für den Zugriff auf geschützte Ressourcen über die [Azure RBAC](../role-based-access-control/overview.md). Azure Service Bus definiert eine Reihe von integrierten Azure-Rollen, die allgemeine Berechtigungen für den Zugriff auf Service Bus-Entitäten umfassen. Sie können auch benutzerdefinierte Rollen für den Zugriff auf die Daten definieren.

Wenn einem Azure AD-Sicherheitsprinzipal eine Azure-Rolle zugewiesen wird, gewährt Azure diesem Sicherheitsprinzipal Zugriff auf diese Ressourcen. Der Zugriff kann sich auf den Bereich des Abonnements, der Ressourcengruppe oder des Namespace von Service Bus beziehen. Eine Azure AD-Sicherheitsprinzipal kann ein Benutzer, eine Gruppe, ein Anwendungsdienstprinzipal oder eine [verwaltete Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) sein.

## <a name="azure-built-in-roles-for-azure-service-bus"></a>Integrierte Azure-Rollen für Azure Service Bus
Bei Azure Service Bus ist die Verwaltung der Namespaces und aller zugehörigen Ressourcen über das Azure-Portal und die Azure-Ressourcenverwaltungs-API bereits durch das Azure RBAC-Modell geschützt. Azure stellt die folgenden integrierten Azure-Rollen zum Autorisieren des Zugriffs auf einen Service Bus-Namespace bereit:

- [Besitzer „Azure Service Bus-Daten“](../role-based-access-control/built-in-roles.md#azure-service-bus-data-owner): Ermöglicht den Datenzugriff auf einen Service Bus-Namespace und seine Entitäten (Warteschlangen, Themen, Abonnements und Filter).
- [Azure Service Bus-Datensender](../role-based-access-control/built-in-roles.md#azure-service-bus-data-sender): Verwenden Sie diese Rolle, um dem Service Bus-Namespace und seinen Entitäten Sendezugriff zu erteilen.
- [Azure Service Bus-Datenempfänger](../role-based-access-control/built-in-roles.md#azure-service-bus-data-receiver): Verwenden Sie diese Rolle, um dem Service Bus-Namespace und seinen Entitäten Empfangszugriff zu erteilen. 

## <a name="resource-scope"></a>Ressourcenumfang 
Bevor Sie einem Sicherheitsprinzipal eine Azure-Rolle zuweisen, legen Sie den Zugriffsbereich fest, den der Sicherheitsprinzipal haben soll. Es hat sich als am besten bewährt, stets nur den kleinstmöglichen Umfang an Zugriffsrechten zu gewähren.

In der folgenden Liste werden die Ebenen beschrieben, auf denen Sie den Zugriff auf Service Bus-Ressourcen einschränken können, beginnend mit dem kleinstmöglichen Bereich:

- **Warteschlange**, **Thema** oder **Abonnement**: Die Rollenzuweisung gilt für die jeweilige Service Bus-Entität. Derzeit wird das Zuweisen von Benutzern/Gruppen/verwalteten Identitäten zu Service Bus-Azure-Rollen auf Abonnementebene vom Azure-Portal nicht unterstützt. 
- **Service Bus-Namespace:** Die Rollenzuweisung umfasst die gesamte Topologie von Service Bus unter dem Namespace und für die zugeordnete Consumergruppe.
- **Ressourcengruppe**: Die Rollenzuweisung gilt für alle Service Bus-Ressourcen unter der Ressourcengruppe.
- **Abonnement**: Die Rollenzuweisung gilt für alle Service Bus-Ressourcen in allen Ressourcengruppen im Abonnement.

> [!NOTE]
> Denken Sie daran, dass die Weitergabe von Azure-Rollenzuweisungen bis zu fünf Minuten dauern kann. 

Weitere Informationen dazu, wie integrierte Rollen definiert sind, finden Sie unter [Grundlegendes zu Rollendefinitionen](../role-based-access-control/role-definitions.md#control-and-data-actions). Informationen zum Erstellen von benutzerdefinierten Azure-Rollen finden Sie unter [Benutzerdefinierte Azure-Rollen](../role-based-access-control/custom-roles.md).


## <a name="assign-azure-roles-using-the-azure-portal"></a>Zuweisen von Azure-Rollen über das Azure-Portal  
Weisen Sie eine der [Service Bus-Rollen](#azure-built-in-roles-for-azure-service-bus) dem Dienstprinzipal der Anwendung im gewünschten Bereich (Service Bus-Namespace, Ressourcengruppe, Abonnement) zu. Ausführliche Informationen finden Sie unter [Zuweisen von Azure-Rollen über das Azure-Portal](../role-based-access-control/role-assignments-portal.md).

Nachdem Sie die Rolle und ihren Bereich definiert haben, können Sie dieses Verhalten mit den [Beispielen auf GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/RoleBasedAccessControl) testen.

## <a name="authenticate-from-an-application"></a>Authentifizieren über eine Anwendung
Ein wesentlicher Vorteil der Verwendung von Azure AD mit Service Bus besteht darin, dass Ihre Anmeldeinformationen nicht mehr im Code gespeichert werden müssen. Stattdessen können Sie ein OAuth 2.0-Zugriffstoken von Microsoft Identity Platform anfordern. Azure AD übernimmt die Authentifizierung des Sicherheitsprinzipals (Benutzer, Gruppe oder Dienstprinzipal), der die Anwendung ausführt. Wenn die Authentifizierung erfolgreich ist, gibt Azure AD das Zugriffstoken an die Anwendung zurück, und die Anwendung kann dann das Zugriffstoken zum Autorisieren von Anforderungen an Azure Service Bus verwenden.

In den folgenden Abschnitten wird gezeigt, wie Sie Ihre native Anwendung oder Webanwendung für die Authentifizierung mit Microsoft Identity Platform 2.0 konfigurieren. Weitere Informationen zu Microsoft Identity Platform 2.0 finden Sie unter [Übersicht über Microsoft Identity Platform (v2.0)](../active-directory/develop/v2-overview.md).

Eine Übersicht über den Datenfluss für OAuth 2.0-Codeberechtigungen finden Sie unter [Autorisieren des Zugriffs auf Azure Active Directory-Webanwendungen mit dem Flow zum Erteilen des OAuth 2.0-Codes](../active-directory/develop/v2-oauth2-auth-code-flow.md).

### <a name="register-your-application-with-an-azure-ad-tenant"></a>Registrieren der Anwendung bei einem Azure AD-Mandanten
Der erste Schritt bei der Verwendung von Azure AD zum Autorisieren von Service Bus-Entitäten ist die Registrierung Ihrer Clientanwendung mit einem Azure AD-Mandanten über das [Azure-Portal](https://portal.azure.com/). Wenn Sie Ihre Clientanwendung registrieren, stellen Sie Azure AD Informationen zu Ihrer Anwendung bereit. Azure AD stellt dann eine Client-ID (auch als Anwendungs-ID bezeichnet) bereit, mit der Sie Ihre Anwendung zur Laufzeit Azure AD zuordnen können. Weitere Informationen zur Client-ID finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory (Azure AD)](../active-directory/develop/app-objects-and-service-principals.md). 

Die folgenden Abbildungen zeigen Schritte zum Registrieren einer Webanwendung:

![Registrieren einer Anwendung](./media/authenticate-application/app-registrations-register.png)

> [!Note]
> Wenn Sie Ihre Anwendung als native Anwendung registrieren, können Sie jeden gültigen URI als Umleitungs-URI angeben. Bei nativen Anwendungen muss dieser Wert keine echte URL sein. Bei Webanwendungen muss der Umleitungs-URI ein gültiger URI sein, da er die URL definiert, für die Token bereitgestellt werden.

Nachdem Sie Ihre Anwendung registriert haben, wird die **Anwendungs-ID (Client-ID)** unter **Einstellungen** angezeigt:

![Anwendungs-ID der registrierten Anwendung](./media/authenticate-application/application-id.png)

Ausführlichere Informationen zum Registrieren einer Anwendung bei Azure AD finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](../active-directory/develop/quickstart-register-app.md).

> [!IMPORTANT]
> Notieren Sie sich die Werte für **TenantId** und **ApplicationId**. Sie benötigen diese Werte, um die Anwendung auszuführen.

### <a name="create-a-client-secret"></a>Erstellen eines Clientgeheimnisses   
Die Anwendung benötigt zum Beweis ihrer Identität einen geheimen Clientschlüssel, wenn sie ein Token anfordert. Führen Sie folgende Schritte aus, um den geheimen Clientschlüssel hinzuzufügen.

1. Navigieren Sie im Azure-Portal zu Ihrer App-Registrierung, wenn diese Seite noch nicht angezeigt wird.
1. Wählen Sie im linken Menü **Zertifikate und Geheimnisse** aus.
1. Wählen Sie unter **Geheime Clientschlüssel** die Option **Neuer geheimer Clientschlüssel** aus, um einen neuen geheimen Schlüssel zu erstellen.

    ![Neuer geheimer Clientschlüssel (Schaltfläche)](./media/authenticate-application/new-client-secret-button.png)
1. Geben Sie eine Beschreibung für den geheimen Schlüssel an, und wählen Sie das gewünschte Ablaufintervall aus. Wählen Sie dann **Hinzufügen** aus.

    ![Clientgeheimnis hinzufügen (Seite)](./media/authenticate-application/add-client-secret-page.png)
1. Kopieren Sie den Wert des neuen geheimen Schlüssels sofort, und legen Sie die Kopie an einem sicheren Speicherort ab. Der Wert wird Ihnen nur ein Mal angezeigt.

    ![Geheimer Clientschlüssel](./media/authenticate-application/client-secret.png)

### <a name="permissions-for-the-service-bus-api"></a>Berechtigungen für die Service Bus-API
Wenn es sich bei Ihrer Anwendung um eine Konsolenanwendung handelt, müssen Sie eine native Anwendung registrieren und den **erforderlichen Berechtigungen** API-Berechtigungen für **Microsoft.ServiceBus** hinzufügen. Native Anwendungen benötigen auch einen **redirect-uri** in Azure AD, der als Bezeichner fungiert. Bei dem URI muss es sich nicht um ein Netzwerkziel handeln. Verwenden Sie in diesem Beispiel `https://servicebus.microsoft.com`, da der Beispielcode diesen URI bereits verwendet.

### <a name="authenticating-the-service-bus-client"></a>Authentifizieren des Service Bus-Clients
Nachdem Sie Ihre Anwendung registriert und ihr Berechtigungen zum Senden/Empfangen von Daten in Azure Service Bus erteilt haben, können Sie Ihren Client mit den Anmeldeinformationen mit geheimem Clientschlüssel authentifizieren, wodurch Sie Anforderungen an Azure Service Bus vornehmen können.

Eine Liste der Szenarien, für die das Abrufen von Token unterstützt wird, finden Sie im GitHub-Repository [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) im Bereich [Scenarios](https://aka.ms/msal-net-scenarios).

Mithilfe der neuesten [Azure.Messaging.ServiceBus](https://www.nuget.org/packages/Azure.Messaging.ServiceBus)-Bibliothek können Sie [ServiceBusClient](/dotnet/api/azure.messaging.servicebus.servicebusclient) mit [ClientSecretCredential](/dotnet/api/azure.identity.clientsecretcredential) authentifizieren (in der [Azure.Identity](https://www.nuget.org/packages/Azure.Identity)-Bibliothek definiert).

```cs
TokenCredential credential = new ClientSecretCredential("<tenant_id>", "<client_id>", "<client_secret>");
var client = new ServiceBusClient("<fully_qualified_namespace>", credential);
```

Wenn Sie die älteren .NET-Pakete verwenden, sehen Sie sich die Beispiele für RoleBasedAccessControl im [Repository mit Beispielen für „azure-service-bus“](https://github.com/Azure/azure-service-bus) an.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zur Azure RBAC finden Sie im Artikel [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../role-based-access-control/overview.md).
- Informationen zum Zuweisen und Verwalten von Azure-Rollenzuweisungen mit Azure PowerShell, der Azure-Befehlszeilenschnittstelle oder der REST-API finden Sie in diesen Artikeln:
    - [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe von Azure PowerShell](../role-based-access-control/role-assignments-powershell.md)  
    - [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe der Azure-Befehlszeilenschnittstelle](../role-based-access-control/role-assignments-cli.md)
    - [Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und der REST-API](../role-based-access-control/role-assignments-rest.md)
    - [Hinzufügen von Azure-Rollenzuweisungen mithilfe von Azure Resource Manager-Vorlagen](../role-based-access-control/role-assignments-template.md)

Weitere Informationen zu Service Bus Messaging finden Sie in folgenden Themen.

- [Service Bus-Azure RBAC-Beispiele](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/RoleBasedAccessControl)
- [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
- [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
- [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)
