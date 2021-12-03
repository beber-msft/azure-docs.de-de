---
title: Übersicht über Benutzerkonten in Azure Active Directory B2C
description: Lernen Sie die Arten von Benutzerkonten kennen, die in Azure Active Directory B2C verwendet werden können.
services: active-directory-b2c
author: kengaderdus
manager: CelesteDG
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/22/2021
ms.author: kengaderdus
ms.subservice: B2C
ms.custom: b2c-support
ms.openlocfilehash: fe6b7e334352630eb3797cb96b33422a84721813
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2021
ms.locfileid: "130266104"
---
# <a name="overview-of-user-accounts-in-azure-active-directory-b2c"></a>Übersicht über Benutzerkonten in Azure Active Directory B2C

In Azure Active Directory B2C (Azure AD B2C) gibt es mehrere Kontoarten, die erstellt werden können. In Azure Active Directory (Azure AD), Azure Active Directory B2B (Azure AD B2B) und Azure Active Directory B2C (Azure AD B2C) können dieselben Arten von Benutzerkonten verwendet werden.

Die folgenden Arten von Konten sind verfügbar:

- **Geschäftskonto**: Ein Geschäftskonto kann auf Ressourcen in einem Mandanten zugreifen und mit einer Administratorrolle Mandanten verwalten.
- **Gastkonto:** Ein Gastkonto kann nur ein Microsoft-Konto oder ein Azure AD-Benutzerkonto sein, das für die Freigabe von Verwaltungsaufgaben wie das [Verwalten eines Mandanten](tenant-management.md) verwendet werden kann.
- **Consumerkonto**: Ein Consumerkonto wird von einem Benutzer der Anwendungen verwendet, die Sie bei Azure AD B2C registriert haben. Consumerkonten können wie folgt erstellt werden:
  - Durch den Benutzer, der einen Benutzeranmeldeflow in einer Azure AD B2C-Anwendung durchläuft
  - Mithilfe der Microsoft Graph-API
  - Verwenden des Azure-Portals

## <a name="work-account"></a>Geschäftskonto

Ein Geschäftskonto wird für alle Mandanten auf die gleiche Weise basierend auf Azure AD angelegt. Um ein Geschäftskonto zu erstellen, befolgen Sie die Anweisungen unter [Schnellstart: Hinzufügen neuer Benutzer zu Azure Active Directory](../active-directory/fundamentals/add-users-azure-active-directory.md). Ein Geschäftskonto wird im Azure-Portal mit der Option **Neuer Benutzer** erstellt.

Beim Hinzufügen eines neuen Geschäftskontos müssen Sie die folgenden Konfigurationseinstellungen berücksichtigen:

- **Name** und **Benutzername**: Die Eigenschaft **Name** enthält den Vor- und Nachnamen des Benutzers. Der **Benutzername** ist der Bezeichner, den der Benutzer eingibt, um sich anzumelden. Der Benutzername enthält den vollständigen Domänennamen. Der Domänennamensteil des Benutzernamens muss entweder der anfängliche Standarddomänenname *Ihre_Domäne.onmicrosoft.com* oder ein bestätigter, nicht im Verbund konfigurierter [benutzerdefinierter Domänenname](../active-directory/fundamentals/add-custom-domain.md) wie *contoso.com* sein. 
- **E-Mail**: Der neue Benutzer kann sich auch mit einer E-Mail-Adresse anmelden. Sonderzeichen oder Multibytezeichen in E-Mails, z. B. japanische Zeichen, werden nicht unterstützt.
- **Profil**: Das Konto wird mit einem Profil mit Daten des Benutzers eingerichtet. Sie haben die Möglichkeit, einen Vornamen, Nachnamen, eine Position und einen Abteilungsnamen einzugeben. Sie können das Profil bearbeiten, nachdem das Konto erstellt wurde.
- **Gruppen**: Verwenden Sie Gruppen zum Ausführen von Verwaltungsaufgaben, z. B. zum gleichzeitigen Zuweisen von Lizenzen oder Berechtigungen für viele Benutzer oder Geräte. Sie können das neue Konto einer bestehenden [Gruppe](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) in Ihrem Mandanten zuordnen.
- **Verzeichnisrolle**: Sie müssen die Zugriffsebene angeben, die das Benutzerkonto auf Ressourcen in Ihrem Mandanten hat. Die folgenden Berechtigungsebenen sind verfügbar:

    - **Benutzer**: Benutzer können auf zugewiesene Ressourcen zugreifen, aber die meisten Mandantenressourcen nicht verwalten.
    - **Globaler Administrator**: Globale Administratoren haben Vollzugriff auf alle Mandantenressourcen.
    - **Eingeschränkter Administrator**: Wählen Sie die Verwaltungsrollen für den Benutzer aus. Weitere Informationen zu Rollen, die ausgewählt werden können, finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory](../active-directory/roles/permissions-reference.md).

### <a name="create-a-work-account"></a>Erstellen eines Geschäftskontos

Befolgen Sie zum Erstellen eines neuen Geschäftskontos die folgenden Angaben:

- [Azure portal](../active-directory/fundamentals/add-users-azure-active-directory.md)
- [Microsoft Graph](/graph/api/user-post-users)

### <a name="update-a-user-profile"></a>Aktualisieren eines Benutzerprofils

Befolgen Sie zum Aktualisieren des Profils eines Benutzers die folgenden Angaben:

- [Azure portal](../active-directory/fundamentals/active-directory-users-profile-azure-portal.md)
- [Microsoft Graph](/graph/api/user-update)

### <a name="reset-a-password-for-a-user"></a>Zurücksetzen eines Kennworts für einen Benutzer

Befolgen Sie zum Zurücksetzen des Kennworts eines Benutzers die folgenden Angaben:

- [Azure portal](../active-directory/fundamentals/active-directory-users-reset-password-azure-portal.md)
- [Microsoft Graph](/graph/api/user-update)

## <a name="guest-user"></a>Gastbenutzer

Sie können in Ihrem Mandanten externe Benutzer als Gastbenutzer einladen. Ein typisches Szenario für das Einladen von Gastbenutzern in Ihren Azure AD B2C-Mandanten ist das Teilen von Systemadministratoraufgaben. Ein Beispiel der Nutzung eines Gastkontos finden Sie unter [Eigenschaften eines Azure Active Directory B2B-Zusammenarbeitsbenutzers](../active-directory/external-identities/user-properties.md).

Wenn Sie Gastbenutzer in Ihren Mandanten eingeladen haben, geben Sie die E-Mail-Adresse des Empfängers und eine Nachricht ein, die die Einladung beschreibt. Über den Einladungslink gelangt der Benutzer zur Zustimmungsseite. Wenn an die E-Mail-Adresse kein Posteingang angefügt ist, kann der Benutzer zur Zustimmungsseite navigieren, indem er mit den Anmeldeinformationen der Einladung zu einer Microsoft-Seite wechselt. Der Benutzer ist dann gezwungen, die Einladung in gleicher Weise wie beim Klicken auf den Link in der E-Mail einzulösen. Beispiel: `https://myapps.microsoft.com/B2CTENANTNAME`.

Sie können auch mit der [Microsoft Graph-API](/graph/api/invitation-post) Gastbenutzer einladen.

## <a name="consumer-user"></a>Consumerbenutzer

Der Consumerbenutzer kann sich bei Anwendungen anmelden, die mit Azure AD B2C geschützt sind, kann aber nicht auf Azure-Ressourcen wie das Azure-Portal zugreifen. Der Consumerbenutzer kann ein lokales oder Verbundkonto wie Facebook oder Twitter verwenden. Ein Consumerkonto wird durch Verwendung eines [Registrierungs- oder Anmeldebenutzerflows](user-flow-overview.md), mithilfe der Microsoft Graph-API oder im Azure-Portal erstellt.

Sie können die Daten angeben, die beim Erstellen eines Consumerbenutzerkontos gesammelt werden. Weitere Informationen finden Sie unter [Hinzufügen von Benutzerattributen und Anpassen der Benutzereingabe](configure-user-input.md).

Weitere Informationen zum Verwalten von Benutzerkonten finden Sie unter [Verwalten von Azure AD B2C-Benutzerkonten mit Microsoft Graph](./microsoft-graph-operations.md).

### <a name="migrate-consumer-user-accounts"></a>Migrieren von Benutzerkonten für Consumer

Möglicherweise müssen Sie bestehende Benutzerkonten von Consumern von Identitätsanbietern in Azure AD B2C migrieren. Weitere Informationen finden Sie unter [Migrieren von Benutzern zu Azure Active Directory B2C](user-migration.md).
