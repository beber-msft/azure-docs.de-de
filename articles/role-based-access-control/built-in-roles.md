---
title: In Azure integrierte Rollen – Azure RBAC
description: In diesem Artikel werden die in Azure integrierten Rollen für die auf Azure-Rollen basierte Zugriffssteuerung (Azure RBAC) beschrieben. Es werden „Actions“, „NotActions“, „DataActions“ und „NotDataActions“ aufgelistet.
services: active-directory
ms.service: role-based-access-control
ms.topic: reference
ms.workload: identity
author: rolyon
ms.author: rolyon
ms.date: 11/12/2021
ms.custom: generated
ms.openlocfilehash: 81b12ad1ae52c290e5c3ef573bf091fb64595701
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2021
ms.locfileid: "132494408"
---
# <a name="azure-built-in-roles"></a>Integrierte Azure-Rollen

Die auf [Azure-Rollen basierte Zugriffssteuerung (Azure RBAC)](overview.md) verfügt über mehrere integrierte Azure-Rollen, die Sie Benutzern, Gruppen, Dienstprinzipalen und verwalteten Identitäten zuweisen können. Durch Rollenzuweisungen wird die Art und Weise gesteuert, wie Sie auf Azure-Ressourcen zugreifen. Wenn die integrierten Rollen den Ansprüchen Ihrer Organisation nicht entsprechen, können Sie Ihre eigenen [benutzerdefinierten Azure-Rollen](custom-roles.md) erstellen. Informationen zum Zuweisen von Rollen finden Sie unter [Schritte zum Zuweisen einer Azure-Rolle](role-assignments-steps.md).

In diesem Artikel werden die integrierten Azure-Rollen aufgeführt. Administratorrollen für Azure Active Directory (Azure AD) finden Sie unter [Integrierte Rollen in Azure AD](../active-directory/roles/permissions-reference.md).

Die folgende Tabelle enthält eine kurze Beschreibung aller integrierten Rollen. Klicken Sie auf den Rollennamen, um die Liste der `Actions`, `NotActions`, `DataActions` und `NotDataActions` für jede Rolle anzuzeigen. Informationen zur Bedeutung dieser Aktionen und ihrer Anwendung auf die Steuerung und die Datenebenen finden Sie unter [Grundlegendes zu Azure-Rollendefinitionen](role-definitions.md).

## <a name="all"></a>All

> [!div class="mx-tableFixed"]
> | Integrierte Rolle | BESCHREIBUNG | id |
> | --- | --- | --- |
> | **Allgemein** |  |  |
> | [Mitwirkender](#contributor) | Hiermit wird Vollzugriff zum Verwalten aller Ressourcen gewährt, allerdings nicht zum Zuweisen von Rollen in Azure RBAC, zum Verwalten von Zuweisungen in Azure Blueprints oder zum Teilen von Imagekatalogen. | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | [Besitzer](#owner) | Hiermit wird Vollzugriff zum Verwalten aller Ressourcen gewährt, einschließlich der Möglichkeit, Rollen in Azure RBAC zuzuweisen. | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | [Leser](#reader) | Hiermit können Sie alle Ressourcen anzeigen, aber keine Änderungen vornehmen. | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | [Benutzerzugriffsadministrator](#user-access-administrator) | Ermöglicht Ihnen die Verwaltung von Benutzerzugriffen auf Azure-Ressourcen. | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **Compute** |  |  |
> | [Mitwirkender von klassischen virtuellen Computern](#classic-virtual-machine-contributor) | Ermöglicht Ihnen das Verwalten klassischer virtueller Computer, aber weder den Zugriff darauf noch auf die mit ihnen verbundenen virtuellen Netzwerke oder Speicherkonten. | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | [VM-Administratoranmeldung](#virtual-machine-administrator-login) | Anzeigen von virtuellen Computern im Portal und Anmelden als Administrator | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | [Mitwirkender von virtuellen Computern](#virtual-machine-contributor) | Erstellen und Verwalten von VMs, Verwalten von Datenträgern und Datenträgermomentaufnahmen, Installieren und Ausführen von Software, Zurücksetzen des Kennworts des Stammbenutzers der VM mit VM-Erweiterungen und Verwalten lokaler Benutzerkonten mit VM-Erweiterungen. Diese Rolle gewährt Ihnen keinen Verwaltungszugriff auf das virtuelle Netzwerk oder das Speicherkonto, mit dem die VMs verbunden sind. Diese Rolle ermöglicht es Ihnen nicht, Rollen in Azure RBAC zuzuweisen. | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | [VM-Benutzeranmeldung](#virtual-machine-user-login) | Anzeigen von virtuellen Computern im Portal und Anmelden als regulärer Benutzer. | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Netzwerk** |  |  |
> | [Mitwirkender für den CDN-Endpunkt](#cdn-endpoint-contributor) | Diese Rolle kann CDN-Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen. | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | [CDN-Endpunktleser](#cdn-endpoint-reader) | Diese Rolle kann CDN-Endpunkte anzeigen, aber keine Änderungen vornehmen. | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | [Mitwirkender für das CDN-Profil](#cdn-profile-contributor) | Diese Rolle kann CDN-Profile und deren Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen. | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | [CDN-Profilleser](#cdn-profile-reader) | Diese Rolle kann CDN-Profile und deren Endpunkte anzeigen, aber keine Änderungen vornehmen. | 8f96442b-4075-438f-813d-ad51ab4019af |
> | [Mitwirkender von klassischem Netzwerk](#classic-network-contributor) | Ermöglicht Ihnen das Verwalten von klassischen Netzwerken, nicht aber den Zugriff darauf. | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | [DNS Zone Contributor](#dns-zone-contributor) | Ermöglicht Ihnen die Verwaltung von DNS-Zonen und Ressourceneintragssätzen in Azure DNS, aber nicht zu steuern, wer darauf Zugriff hat. | befefa01-2a29-4197-83a8-272ff33ce314 |
> | [Mitwirkender von virtuellem Netzwerk](#network-contributor) | Ermöglicht Ihnen das Verwalten von Netzwerken, nicht aber den Zugriff darauf. | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | [Mitwirkender für private DNS-Zone](#private-dns-zone-contributor) | Ermöglicht Ihnen das Verwalten privater DNS-Zonenressourcen, aber nicht der virtuellen Netzwerke, mit denen sie verknüpft sind. | b12aa53e-6015-4669-85d0-8515ebb3ae7f |
> | [Traffic Manager-Mitwirkender](#traffic-manager-contributor) | Ermöglicht Ihnen die Verwaltung von Traffic Manager-Profilen, aber nicht die Steuerung des Zugriffs darauf. | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Storage** |  |  |
> | [Avere-Mitwirkender](#avere-contributor) | Kann einen Avere vFXT-Cluster erstellen und verwalten. | 4f8fab4f-1852-4a58-a46a-8eaf358af14a |
> | [Avere-Bediener](#avere-operator) | Wird vom Avere vFXT-Cluster zum Verwalten des Clusters verwendet | c025889f-8102-4ebf-b32c-fc0c6f0c6bd9 |
> | [Mitwirkender für Sicherungen](#backup-contributor) | Ermöglicht Ihnen die Verwaltung des Sicherungsdiensts. Sie können jedoch keine Tresore erstellen oder anderen Benutzern Zugriff gewähren. | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | [Sicherungsoperator](#backup-operator) | Ermöglicht Ihnen das Verwalten von Sicherungsdiensten, jedoch nicht das Entfernen der Sicherung, die Tresorerstellung und das Erteilen von Zugriff an andere Benutzer. | 00c29273-979b-4161-815c-10b084fb9324 |
> | [Sicherungsleser](#backup-reader) | Kann Sicherungsdienste anzeigen, aber keine Änderungen vornehmen. | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | [Mitwirkender von klassischem Speicherkonto](#classic-storage-account-contributor) | Ermöglicht Ihnen das Verwalten klassischer Speicherkonten, nicht aber den Zugriff darauf. | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | [Klassische Dienstrolle „Speicherkonto-Schlüsseloperator“](#classic-storage-account-key-operator-service-role) | Klassische Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für klassische Speicherkonten auflisten und neu generieren. | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | [Data Box-Mitwirkender](#data-box-contributor) | Ermöglicht Ihnen das Verwalten aller Komponenten unter dem Data Box-Dienst, mit Ausnahme der Gewährung des Zugriffs für andere Benutzer. | add466c9-e687-43fc-8d98-dfcf8d720be5 |
> | [Data Box-Leser](#data-box-reader) | Ermöglicht Ihnen das Verwalten des Data Box-Diensts, mit Ausnahme der Erstellung von Aufträgen oder der Bearbeitung von Auftragsdetails und der Gewährung des Zugriffs für andere Benutzer. | 028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027 |
> | [Data Lake Analytics-Entwickler](#data-lake-analytics-developer) | Ermöglicht Ihnen das Übermitteln, Überwachen und Verwalten Ihrer eigenen Aufträge, aber nicht das Erstellen oder Löschen von Data Lake Analytics-Konten. | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | [Lese- und Datenzugriff](#reader-and-data-access) | Ermöglicht Ihnen das Anzeigen sämtlicher Aspekte, jedoch nicht das Löschen oder Erstellen eines Speicherkontos oder einer darin enthaltenen Ressource. Sie können auch Lese-/Schreibzugriff auf alle Daten in einem Speicherkonto durch Zugriff auf Speicherkontoschlüssel gewähren. | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | [Mitwirkender von Speicherkonto](#storage-account-contributor) | Erlaubt die Verwaltung von Speicherkonten. Ermöglicht den Zugriff auf den Kontoschlüssel, der für den Datenzugriff über die Autorisierung mit einem gemeinsam verwendetem Schlüssel genutzt werden kann. | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | [Dienstrolle „Speicherkonto-Schlüsseloperator“](#storage-account-key-operator-service-role) | Ermöglicht das Auflisten und erneute Generieren von Zugriffsschlüsseln für Speicherkonten. | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | [Mitwirkender an Speicherblobdaten](#storage-blob-data-contributor) | Lesen, Schreiben und Löschen von Azure Storage-Containern und -Blobs. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | [Besitzer von Speicherblobdaten](#storage-blob-data-owner) | Bietet Vollzugriff auf Azure Storage-Blobcontainer und -daten, einschließlich POSIX-Zugriffssteuerung. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | b7e6dc6d-f1e8-4753-8033-0f276bb0955b |
> | [Leser von Speicherblobdaten](#storage-blob-data-reader) | Lesen und Auflisten von Azure Storage-Containern und -Blobs. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | [Storage Blob-Delegator](#storage-blob-delegator) | Abrufen eines Benutzerdelegierungsschlüssels, mit dem dann eine SAS (Shared Access Signature) für einen Container oder Blob erstellt werden kann, die mit Azure AD-Anmeldeinformationen signiert ist. Weitere Informationen finden Sie unter [Erstellen einer SAS für die Benutzerdelegierung](/rest/api/storageservices/create-user-delegation-sas). | db58b8e5-c6ad-4a2a-8342-4190687cbf4a |
> | [Speicherdateidaten-SMB-Freigabemitwirkender](#storage-file-data-smb-share-contributor) | Ermöglicht den Lese-, Schreib- und Löschzugriff auf Dateien/Verzeichnisse in Azure-Dateifreigaben. Für diese Rolle gibt es keine integrierte Entsprechung auf Windows-Dateiservern. | 0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb |
> | [Speicherdateidaten-SMB-Freigabemitwirkender mit erhöhten Rechten](#storage-file-data-smb-share-elevated-contributor) | Ermöglicht das Lesen, Schreiben, Löschen und Bearbeiten von Zugriffssteuerungslisten für Dateien/Verzeichnisse in Azure-Dateifreigaben. Diese Rolle entspricht einer Dateifreigabe-ACL für das Bearbeiten auf Windows-Dateiservern. | a7264617-510b-434b-a828-9731dc254ea7 |
> | [Speicherdateidaten-SMB-Freigabeleser](#storage-file-data-smb-share-reader) | Ermöglicht den Lesezugriff auf Dateien/Verzeichnisse in Azure-Dateifreigaben. Diese Rolle entspricht einer Dateifreigabe-ACL für das Lesen auf Windows-Dateiservern. | aba4ae5f-2193-4029-9191-0cb91df5e314 |
> | [Mitwirkender an Storage-Warteschlangendaten](#storage-queue-data-contributor) | Lesen, Schreiben und Löschen von Azure Storage-Warteschlangen und -Warteschlangennachrichten. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 974c5e8b-45b9-4653-ba55-5f855dd0fb88 |
> | [Verarbeiter von Speicherwarteschlangen-Datennachrichten](#storage-queue-data-message-processor) | Einsehen, Abrufen und Löschen einer Nachricht aus einer Azure Storage-Warteschlange. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 8a0f0c08-91a1-4084-bc3d-661d67233fed |
> | [Absender der Speicherwarteschlangen-Datennachricht](#storage-queue-data-message-sender) | Hinzufügen von Nachrichten zu einer Azure Storage-Warteschlange. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | c6a89b2d-59bc-44d0-9896-0f6e12d7b80a |
> | [Storage-Warteschlangendatenleser](#storage-queue-data-reader) | Lesen und Auflisten von Azure Storage-Warteschlangen und -Warteschlangennachrichten. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 19e7f393-937e-4f77-808e-94535e297925 |
> | [Mitwirkender an Storage-Tabellendaten](#storage-table-data-contributor) | Ermöglicht den Lese-, Schreib- und Löschzugriff auf Azure Storage-Tabellen und -Entitäten. | 0a9a7e1f-b9d0-4cc4-a60d-0319b160aaa3 |
> | [Storage-Tabellendatenleser](#storage-table-data-reader) | Ermöglicht den Lesezugriff auf Azure Storage-Tabellen und -Entitäten. | 76199698-9eea-4c19-bc75-cec21354c6b6 |
> | **Web** |  |  |
> | [Azure Maps-Datenmitwirkender](#azure-maps-data-contributor) | Gewährt Lese-, Schreib- und Löschzugriff auf kartenbezogene Daten von einem Azure Maps-Konto. | 8f5e0ce6-4f7b-4dcf-bddf-e6f48634a204 |
> | [Azure Maps-Datenleser](#azure-maps-data-reader) | Gewährt Lesezugriff auf kartenbezogene Daten von einem Azure Maps-Konto. | 423170ca-a8f6-4b0f-8487-9e4eb8f49bfa |
> | [Azure Spring Cloud Config Server-Mitwirkender](#azure-spring-cloud-config-server-contributor) | Gewährt Lese-, Schreib- und Löschzugriff auf den Azure Spring Cloud Config Server | a06f5c24-21a7-4e1a-aa2b-f19eb6684f5b |
> | [Azure Spring Cloud Config Server-Leser](#azure-spring-cloud-config-server-reader) | Gewährt Lesezugriff auf den Azure Spring Cloud Config Server | d04c6db6-4947-4782-9e91-30a88feb7be7 |
> | [Azure Spring Cloud-Datenleser](#azure-spring-cloud-data-reader) | Gewährt Lesezugriff auf Azure Spring Cloud-Daten. | b5537268-8956-4941-a8f0-646150406f0c |
> | [Azure Spring Cloud Service Registry-Mitwirkender](#azure-spring-cloud-service-registry-contributor) | Gewährt Lese-, Schreib- und Löschzugriff auf die Azure Spring Cloud Service Registry | f5880b48-c26d-48be-b172-7927bfa1c8f1 |
> | [Azure Spring Cloud Service Registry-Leser](#azure-spring-cloud-service-registry-reader) | Gewährt Lesezugriff auf die Azure Spring Cloud Service Registry | cff1b556-2399-4e7e-856d-a8f754be7b65 |
> | [Media Services-Kontoadministrator](#media-services-account-administrator) | Erstellen, Lesen, Ändern und Löschen von Media Services Konten; schreibgeschützter Zugriff auf andere Media Services-Ressourcen. | 054126f8-9a2b-4f1c-a9ad-eca461f08466 |
> | [Media Services-Administrator für Liveereignisse](#media-services-live-events-administrator) | Erstellen, Lesen, Ändern und Löschen von Liveereignissen, Medienobjekten, Medienobjektfiltern und Streaminglocators; schreibgeschützter Zugriff auf andere Media Services-Ressourcen. | 532bc159-b25e-42c0-969e-a1d439f60d77 |
> | [Media Services-Medienoperator](#media-services-media-operator) | Erstellen, Lesen, Ändern und Löschen von Medienobjekten, Medienobjektfiltern, Streaminglocators und Aufträgen; schreibgeschützter Zugriff auf andere Media Services-Ressourcen. | e4395492-1534-4db2-bedf-88c14621589c |
> | [Media Services-Richtlinienadministrator](#media-services-policy-administrator) | Erstellen, Lesen, Ändern und Löschen von Kontofiltern, Streamingrichtlinien, Richtlinien für symmetrische Schlüssel und Transformationen; schreibgeschützter Zugriff auf andere Media Services-Ressourcen. Aufträge, Medienobjekte oder Streamingressourcen können nicht erstellt werden. | c4bba371-dacd-4a26-b320-7250bca963ae |
> | [Media Services-Administrator für Streamingendpunkte](#media-services-streaming-endpoints-administrator) | Erstellen, Lesen, Ändern und Löschen von Streamingendpunkten; schreibgeschützter Zugriff auf andere Media Services-Ressourcen. | 99dba123-b5fe-44d5-874c-ced7199a5804 |
> | [Mitwirkender an Suchindexdaten](#search-index-data-contributor) | Gewährt Vollzugriff auf Azure Cognitive Search-Indexdaten. | 8ebe5a00-799e-43f5-93ac-243d3dce84a7 |
> | [Suchindexdatenleser](#search-index-data-reader) | Gewährt Lesezugriff auf Azure Cognitive Search-Indexdaten. | 1407120a-92aa-4202-b7e9-c0e197c71c8f |
> | [Mitwirkender von Suchdienst](#search-service-contributor) | Ermöglicht Ihnen das Verwalten von Search-Diensten, nicht aber den Zugriff darauf. | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | [SignalR-AccessKey-Leser](#signalr-accesskey-reader) | Ermöglicht das Lesen von SignalR Service-Zugriffsschlüsseln. | 04165923-9d83-45d5-8227-78b77b0a687e |
> | [SignalR-App-Server (Vorschau)](#signalr-app-server-preview) | Ermöglicht Ihrem App-Server den Zugriff auf SignalR Service mit AAD-Authentifizierungsoptionen. | 420fcaa2-552c-430f-98ca-3264be4806c7 |
> | [SignalR-REST-API-Besitzer](#signalr-rest-api-owner) | Bietet Vollzugriff auf Azure SignalR Service-REST-APIs. | fd53cd77-2268-407a-8f46-7e7863d0f521 |
> | [SignalR-REST-API-Leser](#signalr-rest-api-reader) | Bietet schreibgeschützten Zugriff auf Azure SignalR Service-REST-APIs. | ddde6b66-c0df-4114-a159-3618637b3035 |
> | [SignalR Service-Besitzer](#signalr-service-owner) | Bietet Vollzugriff auf Azure SignalR Service-REST-APIs. | 7e4f1700-ea5a-4f59-8f37-079cfe29dce3 |
> | [SignalR-/Web PubSub-Mitwirkender](#signalrweb-pubsub-contributor) | Ermöglicht das Erstellen, Lesen, Aktualisieren und Löschen von SignalR Service-Ressourcen. | 8cf5e20a-e4b2-4e9d-b3a1-5ceb692c2761 |
> | [Mitwirkender von Webplan](#web-plan-contributor) | Dieser verwaltet die Webpläne für Websites. Ermöglicht es Ihnen nicht, Rollen in Azure RBAC zuzuweisen. | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | [Mitwirkender von Website](#website-contributor) | Verwalten von Websites, aber nicht von Webplänen. Ermöglicht es Ihnen nicht, Rollen in Azure RBAC zuzuweisen. | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Container** |  |  |
> | [AcrDelete](#acrdelete) | Löschen von Repositorys, Tags oder Manifesten aus einer Containerregistrierung | c2f4ef07-c644-48eb-af81-4b1b4947fb11 |
> | [AcrImageSigner](#acrimagesigner) | Pushen oder Pullen vertrauenswürdiger Images in einer Containerregistrierung, die für Inhaltsvertrauen aktiviert ist | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | [AcrPull](#acrpull) | Pullen von Artefakten aus einer Containerregistrierung | 7f951dda-4ed3-4680-a7ca-43fe172d538d |
> | [AcrPush](#acrpush) | Pushen oder Pullen von Artefakten in einer Containerregistrierung | 8311e382-0749-4cb8-b61a-304f252e45ec |
> | [AcrQuarantineReader](#acrquarantinereader) | Pullen von Images in Quarantäne aus einer Containerregistrierung | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | [AcrQuarantineWriter](#acrquarantinewriter) | Pushen oder Pullen von Images in Quarantäne in einer Containerregistrierung | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | [Administratorrolle für Azure Kubernetes Service-Cluster](#azure-kubernetes-service-cluster-admin-role) | Listet die Aktion für Anmeldeinformationen des Clusteradministrators auf. | 0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8 |
> | [Benutzerrolle für Azure Kubernetes Service-Cluster](#azure-kubernetes-service-cluster-user-role) | Listet die Aktion für Anmeldeinformationen des Clusterbenutzer auf. | 4abbcc35-e782-43d8-92c5-2d3f1bd2253f |
> | [Rolle „Mitwirkender“ für Azure Kubernetes Service](#azure-kubernetes-service-contributor-role) | Gewährt Lese- und Schreibzugriff auf Azure Kubernetes Service-Cluster. | ed7f3fbd-7b88-4dd4-9017-9adb7ce333f8 |
> | [RBAC-Administrator von Azure Kubernetes Service](#azure-kubernetes-service-rbac-admin) | Ermöglicht Ihnen das Verwalten aller Ressourcen unter einem Cluster/Namespace, außer das Aktualisieren oder Löschen von Ressourcenkontingenten und Namespaces. | 3498e952-d568-435e-9b2c-8d77e338d7f7 |
> | [RBAC-Clusteradministrator von Azure Kubernetes Service](#azure-kubernetes-service-rbac-cluster-admin) | Ermöglicht Ihnen das Verwalten aller Ressourcen im Cluster. | b1ff04bb-8a4e-4dc4-8eb5-8693973ce19b |
> | [RBAC-Leser von Azure Kubernetes Service](#azure-kubernetes-service-rbac-reader) | Ermöglicht schreibgeschützten Zugriff, um die meisten Objekte in einem Namespace anzuzeigen. Es ist nicht möglich, Rollen oder Rollenbindungen anzuzeigen. Diese Rolle lässt das Anzeigen von Geheimnissen nicht zu, da das Lesen des Inhalts von Geheimnissen den Zugriff auf ServiceAccount-Anmeldeinformationen im Namespace ermöglicht, was den API-Zugriff als beliebiges Dienstkonto im Namespace ermöglichen würde (eine Form von Berechtigungsausweitung). Wenn Sie diese Rolle im Clusterumfang anwenden, wird der Zugriff auf alle Namespaces ermöglicht. | 7f6c6a51-bcf8-42ba-9220-52d62157d7db |
> | [RBAC-Writer von Azure Kubernetes Service](#azure-kubernetes-service-rbac-writer) | Ermöglicht den Lese-/Schreibzugriff auf die meisten Objekte in einem Namespace. Diese Rolle gestattet nicht das Anzeigen oder Ändern von Rollen oder Rollenbindungen. Diese Rolle ermöglicht jedoch den Zugriff auf Geheimnisse und das Ausführen von Pods als beliebiges Dienstkonto im Namespace, sodass sie verwendet werden kann, um die API-Zugriffsebenen eines beliebigen ServiceAccount im Namespace zu erhalten. Wenn Sie diese Rolle im Clusterumfang anwenden, wird der Zugriff auf alle Namespaces ermöglicht. | a7ffa36f-339b-4b5c-8bdf-e2c188b2c0eb |
> | **Datenbanken** |  |  |
> | [Azure Connected SQL Server Onboarding](#azure-connected-sql-server-onboarding) | Ermöglicht Lese- und Schreibzugriff auf Azure-Ressourcen für SQL Server auf Arc-fähigen Servern. | e8113dce-c529-4d33-91fa-e9b972617508 |
> | [Cosmos DB-Rolle „Kontoleser“](#cosmos-db-account-reader-role) | Kann Azure Cosmos DB-Kontodaten lesen. Informationen zum Verwalten von Azure Cosmos DB-Konten finden Sie unter [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor). | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | [Cosmos DB-Operator](#cosmos-db-operator) | Ermöglicht das Verwalten von Azure Cosmos DB-Konten, aber nicht das Zugreifen auf deren Daten. Verhindert den Zugriff auf Kontoschlüssel und Verbindungszeichenfolgen. | 230815da-be43-4aae-9cb4-875f7bd000aa |
> | [CosmosBackupOperator](#cosmosbackupoperator) | Kann eine Wiederherstellungsanforderung für eine Cosmos DB-Datenbank oder einen Container für ein Konto übermitteln. | db7b14f2-5adf-42da-9f96-f2ee17bab5cb |
> | [CosmosRestoreOperator](#cosmosrestoreoperator) | Kann eine Wiederherstellungsaktion für ein Cosmos DB-Datenbankkonto im fortlaufendem Sicherungsmodus ausführen | 5432c526-bc82-444a-b7ba-57c5b0b5b34f |
> | [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor) | Kann Azure Cosmos DB-Konten verwalten. Azure Cosmos DB wurde früher als DocumentDB bezeichnet. | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | [Mitwirkender von Redis-Cache](#redis-cache-contributor) | Ermöglicht Ihnen das Verwalten von Redis Caches, nicht aber den Zugriff darauf. | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | [Mitwirkender von SQL DB](#sql-db-contributor) | Ermöglicht Ihnen das Verwalten von SQL-Datenbanken, nicht aber den Zugriff darauf. Darüber hinaus können Sie deren sicherheitsbezogenen Richtlinien oder übergeordneten SQL-Server nicht verwalten. | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | [Verwaltete SQL-Instanz: Mitwirkender](#sql-managed-instance-contributor) | Diese Rolle ermöglicht Ihnen das Verwalten verwalteter SQL-Instanzen und der erforderlichen Netzwerkkonfiguration, jedoch nicht das Erteilen des Zugriffs an andere. | 4939a1f6-9ae0-4e48-a1e0-f2cbe897382d |
> | [SQL-Sicherheits-Manager](#sql-security-manager) | Ermöglicht Ihnen das Verwalten von sicherheitsbezogenen Richtlinien von SQL-Server und Datenbanken, jedoch nicht den Zugriff darauf. | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | [Mitwirkender von SQL Server](#sql-server-contributor) | Diese Rolle ermöglicht es Ihnen, SQL-Server und -Datenbanken zu verwalten, gewährt Ihnen jedoch keinen Zugriff darauf und auch nicht auf deren sicherheitsbezogenen Richtlinien. | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Analyse** |  |  |
> | [Azure Event Hubs-Datenbesitzer](#azure-event-hubs-data-owner) | Ermöglicht den uneingeschränkten Zugriff auf die Azure Event Hubs-Ressourcen. | f526a384-b230-433a-b45c-95f59c4a2dec |
> | [Azure Event Hubs-Datenempfänger](#azure-event-hubs-data-receiver) | Ermöglicht Empfängern den Zugriff auf die Azure Event Hubs-Ressourcen. | a638d3c7-ab3a-418d-83e6-5f17a39d4fde |
> | [Azure Event Hubs-Datensender](#azure-event-hubs-data-sender) | Ermöglicht Absendern den Zugriff auf die Azure Event Hubs-Ressourcen. | 2b629674-e913-4c01-ae53-ef4638d8f975 |
> | [Mitwirkender von Data Factory](#data-factory-contributor) | Erstellen und verwalten Sie Data Factorys sowie die darin enthaltenen untergeordneten Ressourcen. | 673868aa-7521-48a0-acc6-0f60742d39f5 |
> | [Datenpurger](#data-purger) | Löschen privater Daten aus einem Log Analytics-Arbeitsbereich. | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | [HDInsight-Clusteroperator](#hdinsight-cluster-operator) | Ermöglicht Ihnen das Lesen und Ändern von HDInsight-Clusterkonfigurationen. | 61ed4efc-fab3-44fd-b111-e24485cc132a |
> | [Mitwirkender für die HDInsight-Domänendienste](#hdinsight-domain-services-contributor) | Ermöglicht Ihnen, Vorgänge im Zusammenhang mit Domänendiensten, die für das HDInsight Enterprise-Sicherheitspaket erforderlich sind, zu lesen, zu erstellen, zu ändern und zu löschen. | 8d8d5a11-05d3-4bda-a417-a08778121c7c |
> | [Log Analytics-Mitwirkender](#log-analytics-contributor) | Ein Log Analytics-Mitwirkender kann alle Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Das Bearbeiten von Überwachungseinstellungen schließt folgende Aufgaben ein: Hinzufügen der VM-Erweiterung zu VMs, Lesen von Speicherkontoschlüsseln zum Konfigurieren von Protokollsammlungen aus Azure Storage, Hinzufügen von Lösungen, Konfigurieren der Azure-Diagnose für alle Azure-Ressourcen. | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | [Log Analytics-Leser](#log-analytics-reader) | Ein Log Analytics-Leser kann alle Überwachungsdaten anzeigen und durchsuchen sowie Überwachungseinstellungen anzeigen. Hierzu zählt auch die Anzeige der Konfiguration von Azure-Diagnosen für alle Azure-Ressourcen. | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | [Purview-Datenkurator (Legacy)](#purview-data-curator-legacy) | Datenkurator für Microsoft Purview ist eine Rolle, die Katalogdatenobjekte erstellen, lesen, ändern und löschen und Beziehungen zwischen Objekten herstellen kann. Diese Rolle wurde vor Kurzem für den rollenbasierten Zugriff in Azure als veraltet gekennzeichnet und durch eine neue Datenkuratorrolle auf der Azure Purview-Datenebene ersetzt. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Purview: Rollen](../purview/catalog-permissions.md#roles). | 8a3c2885-9b38-4fd2-9d99-91af537c1347 |
> | [Purview-Datenleseberechtigter (Legacy)](#purview-data-reader-legacy) | Die Rolle Microsoft Purview-Datenleser ist eine Legacyrolle, die Katalogdatenobjekte lesen kann. Diese Rolle wurde vor Kurzem für den rollenbasierten Zugriff in Azure als veraltet gekennzeichnet und durch eine neue Datenleserrolle auf der Azure Purview-Datenebene ersetzt. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Purview: Rollen](../purview/catalog-permissions.md#roles). | ff100721-1b9d-43d8-af52-42b69c1272db |
> | [Purview-Datenquellenadministrator (Legacy)](#purview-data-source-administrator-legacy) | Der Datenquellenadministrator für Microsoft Purview ist eine Legacyrolle, die Datenquellen und Datenüberprüfungen verwalten kann. Diese Rolle wurde vor Kurzem für den rollenbasierten Zugriff in Azure als veraltet gekennzeichnet und durch eine neue Datenquellenadministratorrolle auf der Azure Purview-Datenebene ersetzt. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Purview: Rollen](../purview/catalog-permissions.md#roles). | 200bba9e-f0c8-430f-892b-6f0794863803 |
> | [Mitwirkender der Schemaregistrierung (Vorschau)](#schema-registry-contributor-preview) | Lesen, Schreiben und Löschen von Schemaregistrierungsgruppen und Schemas. | 5dffeca3-4936-4216-b2bc-10343a5abb25 |
> | [Schemaregistrierungsleser (Vorschau)](#schema-registry-reader-preview) | Lesen und Auflisten von Schemaregistrierungsgruppen und Schemas. | 2c56ea50-c6b3-40a6-83c0-9d98858bc7d2 |
> | **Blockchain** |  |  |
> | [Zugriff auf Blockchainmitgliedsknoten (Vorschauversion)](#blockchain-member-node-access-preview) | Ermöglicht den Zugriff auf Blockchainmitgliedsknoten. | 31a002a1-acaf-453e-8a5b-297c9ca1ea24 |
> | **KI und Machine Learning** |  |  |
> | [AzureML Data Scientist](#azureml-data-scientist) | Kann alle Aktionen innerhalb eines Azure Machine Learning-Arbeitsbereichs ausführen, mit Ausnahme des Erstellens oder Löschens von Computeressourcen und des Änderns des Arbeitsbereichs selbst. | f6c7c914-8db3-469d-8ca1-694a8f32e121 |
> | [Mitwirkender für Cognitive Services](#cognitive-services-contributor) | Ermöglicht Ihnen das Erstellen, Lesen, Aktualisieren, Löschen und Verwalten von Cognitive Services-Schlüsseln. | 25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68 |
> | [Cognitive Services: Custom Vision-Mitwirkender](#cognitive-services-custom-vision-contributor) | Vollzugriff auf Projekte, einschließlich der Möglichkeit zum Anzeigen, Erstellen, Bearbeiten oder Löschen von Projekten | c1ff6cc2-c111-46fe-8896-e0ef812ad9f3 |
> | [Cognitive Services: Custom Vision-Bereitstellung](#cognitive-services-custom-vision-deployment) | Veröffentlichen, Aufheben der Veröffentlichung oder Exportieren von Modellen. Mit der Bereitstellungsrolle kann das Projekt angezeigt, aber nicht aktualisiert werden. | 5c4089e1-6d96-4d2f-b296-c1bc7137275f |
> | [Cognitive Services: Custom Vision-Beschriftungsersteller](#cognitive-services-custom-vision-labeler) | Anzeigen, Bearbeiten von Trainingsbildern und Erstellen, Hinzufügen, Entfernen oder Löschen von Bildtags. Beschriftungsersteller können Projekte anzeigen, jedoch nichts außer Trainingsbildern und Tags aktualisieren. | 88424f51-ebe7-446f-bc41-7fa16989e96c |
> | [Cognitive Services: Custom Vision-Leser](#cognitive-services-custom-vision-reader) | Schreibgeschützte Aktionen im Projekt. Leser können das Projekt weder erstellen noch aktualisieren. | 93586559-c37d-4a6b-ba08-b9f0940c2d73 |
> | [Cognitive Services: Custom Vision-Trainer](#cognitive-services-custom-vision-trainer) | Anzeigen, Bearbeiten von Projekten und Trainieren der Modelle, einschließlich der Möglichkeit zum Veröffentlichen, Aufheben der Veröffentlichung und Exportieren der Modelle. Trainer können das Projekt weder erstellen noch löschen. | 0a5ae4ab-0d65-4eeb-be61-29fc9b54394b |
> | [Cognitive Services-Datenleser (Vorschau)](#cognitive-services-data-reader-preview) | Ermöglicht das Lesen von Cognitive Services-Daten. | b59867f0-fa02-499b-be73-45a86b5b3e1c |
> | [Cognitive Services: Gesichtserkennung](#cognitive-services-face-recognizer) | Ermöglicht Ihnen das Ausführen von Vorgängen zum Erkennen, Überprüfen, Identifizieren und Gruppieren sowie zum Auffinden ähnlicher Gesichter in der Gesichtserkennungs-API. Diese Rolle lässt keine Erstellungs- oder Löschvorgänge zu. Daher eignet sie sich gut für Endpunkte, die nur Rückschlussfunktionen basierend auf der Regel der geringsten Rechte benötigen. | 9894cab4-e18a-44aa-828b-cb588cd6f2d7 |
> | [Cognitive Services Metrics Advisor-Administrator](#cognitive-services-metrics-advisor-administrator) | Vollzugriff auf das Projekt, einschließlich der Konfiguration auf Systemebene. | cb43c632-a144-4ec5-977c-e80c4affc34a |
> | [Cognitive Services QnA Maker-Editor](#cognitive-services-qna-maker-editor) | Erstellen, Bearbeiten, Importieren und Exportieren einer Wissensdatenbank. Sie können keine Wissensdatenbank veröffentlichen oder löschen. | f4cc2bf9-21be-47a1-bdf1-5c5804381025 |
> | [Cognitive Services QnA Maker-Leseberechtigter](#cognitive-services-qna-maker-reader) | Sie können Wissensdatenbanken nur lesen und testen. | 466ccd10-b268-4a11-b098-b4849f024126 |
> | [Cognitive Services-Benutzer](#cognitive-services-user) | Ermöglicht Ihnen das Lesen und Auflisten von Cognitive Services-Schlüsseln. | a97b65f3-24c7-4388-baec-2e87135dc908 |
> | **Internet der Dinge (IoT)** |  |  |
> | [Device Update-Administrator](#device-update-administrator) | Mit dieser Rolle erhalten Sie Vollzugriff auf Verwaltungs- und Inhaltsvorgänge. | 02ca0879-e8e4-47a5-a61e-5c618b76e64a |
> | [Device Update-Inhaltsadministrator](#device-update-content-administrator) | Mit dieser Rolle erhalten Sie Vollzugriff auf Inhaltsvorgänge. | 0378884a-3af5-44ab-8323-f5b22f9f3c98 |
> | [Device Update-Inhaltsleser](#device-update-content-reader) | Mit dieser Rolle erhalten Sie Lesezugriff auf Inhaltsvorgänge, können jedoch keine Änderungen vornehmen. | d1ee9a80-8b14-47f0-bdc2-f4a351625a7b |
> | [Device Update-Bereitstellungsadministrator](#device-update-deployments-administrator) | Mit dieser Rolle erhalten Sie Vollzugriff auf Verwaltungsvorgänge. | e4237640-0e3d-4a46-8fda-70bc94856432 |
> | [Device Update-Bereitstellungsleser](#device-update-deployments-reader) | Mit dieser Rolle erhalten Sie Lesezugriff auf Verwaltungsvorgänge, können jedoch keine Änderungen vornehmen. | 49e2f5d2-7741-4835-8efa-19e1fe35e47f |
> | [Device Update-Leser](#device-update-reader) | Mit dieser Rolle erhalten Sie Lesezugriff auf Verwaltungs- und Inhaltsvorgänge, können jedoch keine Änderungen vornehmen. | e9dba6fb-3d52-4cf0-bce3-f06ce71b9e0f |
> | [Mitwirkender an IoT Hub-Daten](#iot-hub-data-contributor) | Ermöglicht Vollzugriff für Vorgänge auf der IoT Hub-Datenebene | 4fc6c259-987e-4a07-842e-c321cc9d413f |
> | [IoT Hub-Datenleser](#iot-hub-data-reader) | Ermöglicht vollständigen Lesezugriff auf Eigenschaften auf der IoT Hub-Datenebene | b447c946-2db7-41ec-983d-d8bf3b1c77e3 |
> | [Mitwirkender an IoT Hub-Registrierung](#iot-hub-registry-contributor) | Ermöglicht Vollzugriff auf die IoT Hub-Geräteregistrierung | 4ea46cd5-c1b2-4a8e-910b-273211f9ce47 |
> | [Mitwirkender an IoT Hub-Zwillingen](#iot-hub-twin-contributor) | Ermöglicht Lese- und Schreibzugriff auf alle IoT Hub-Geräte- und -Modulzwillinge. | 494bdba2-168f-4f31-a0a1-191d2f7c028c |
> | **Mixed Reality** |  |  |
> | [Remote Rendering-Administrator](#remote-rendering-administrator) | Bietet dem Benutzer Konvertierungs-, Sitzungsverwaltungs-, Rendering- und Diagnosefunktionen für Azure Remote Rendering. | 3df8b902-2a6f-47c7-8cc5-360e9b272a7e |
> | [Remote Rendering-Client](#remote-rendering-client) | Bietet dem Benutzer Sitzungsverwaltungs-, Rendering- und Diagnosefunktionen für Azure Remote Rendering. | d39065c4-c120-43c9-ab0a-63eed9795f0a |
> | [Spatial Anchors-Kontomitwirkender](#spatial-anchors-account-contributor) | Ermöglicht Ihnen das Verwalten von Raumankern in Ihrem Konto, nicht jedoch das Löschen von Ankern. | 8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827 |
> | [Spatial Anchors-Kontobesitzer](#spatial-anchors-account-owner) | Ermöglicht Ihnen das Verwalten von Raumankern in Ihrem Konto, einschließlich der Löschung von Ankern. | 70bbe301-9835-447d-afdd-19eb3167307c |
> | [Spatial Anchors-Kontoleser](#spatial-anchors-account-reader) | Ermöglicht Ihnen das Ermitteln und Lesen von Eigenschaften für Raumanker in Ihrem Dokument. | 5d51204f-eb77-4b1c-b86a-2ec626c49413 |
> | **Integration** |  |  |
> | [Mitwirkender des API-Verwaltungsdienstes](#api-management-service-contributor) | Kann Dienst und APIs verwalten. | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | [Operatorrolle des API Management-Diensts](#api-management-service-operator-role) | Kann den Dienst, aber nicht die APIs verwalten. | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | [Leserrolle des API Management-Diensts](#api-management-service-reader-role) | Schreibgeschützter Zugriff auf Dienst und APIs | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | [App Configuration-Datenbesitzer](#app-configuration-data-owner) | Ermöglicht den Vollzugriff auf App Configuration-Daten. | 5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b |
> | [App Configuration-Datenleser](#app-configuration-data-reader) | Ermöglicht den Lesezugriff auf App Configuration-Daten. | 516239f1-63e1-4d78-a4de-a74fb236a071 |
> | [Azure Relay-Listener](#azure-relay-listener) | Ermöglicht Lauschzugriff auf Azure Relay-Ressourcen. | 26e0b698-aa6d-4085-9386-aadae190014d |
> | [Azure Relay-Besitzer](#azure-relay-owner) | Ermöglicht Vollzugriff auf Azure Relay-Ressourcen. | 2787bf04-f1f5-4bfe-8383-c8a24483ee38 |
> | [Azure Relay-Absender](#azure-relay-sender) | Ermöglicht Sendezugriff auf Azure Relay-Ressourcen. | 26baccc8-eea7-41f1-98f4-1762cc7f685d |
> | [Azure Service Bus-Datenbesitzer](#azure-service-bus-data-owner) | Ermöglicht den uneingeschränkten Zugriff auf die Azure Service Bus-Ressourcen. | 090c5cfd-751d-490a-894a-3ce6f1109419 |
> | [Azure Service Bus-Datenempfänger](#azure-service-bus-data-receiver) | Ermöglicht Empfängern den Zugriff auf die Azure Service Bus-Ressourcen. | 4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0 |
> | [Azure Service Bus-Datensender](#azure-service-bus-data-sender) | Ermöglicht Absendern den Zugriff auf die Azure Service Bus-Ressourcen. | 69a216fc-b8fb-44d8-bc22-1f3c2cd27a39 |
> | [Besitzer der Azure Stack-Registrierung](#azure-stack-registration-owner) | Ermöglicht Ihnen die Verwaltung von Azure Stack-Registrierungen. | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | [EventGrid Contributor](#eventgrid-contributor) | Ermöglicht die Verwaltung von EventGrid-Vorgängen. | 1e241071-0855-49ea-94dc-649edcd759de |
> | [EventGrid-Datenabsender](#eventgrid-data-sender) | Ermöglicht Sendezugriff auf Event Grid-Ereignisse. | d5a91429-5739-47e2-a06b-3470a27159e7 |
> | [EventGrid EventSubscription-Mitwirkender](#eventgrid-eventsubscription-contributor) | Ermöglicht die Verwaltung von EventGrid-Ereignisabonnementvorgängen. | 428e0ff0-5e57-4d9c-a221-2c70d0e0a443 |
> | [EventGrid EventSubscription-Leser](#eventgrid-eventsubscription-reader) | Ermöglicht das Lesen von EventGrid-Ereignisabonnements. | 2414bbcf-6497-4faf-8c65-045460748405 |
> | [Mitwirkender an FHIR-Daten](#fhir-data-contributor) | Die Rolle ermöglicht dem Benutzer oder Prinzipal vollen Zugriff auf FHIR-Daten. | 5a1fc7df-4bf1-4951-a576-89034ee01acd |
> | [FHIR-Datenexportierer](#fhir-data-exporter) | Die Rolle ermöglicht dem Benutzer oder Prinzipal das Lesen und Exportieren von FHIR-Daten. | 3db33094-8700-4567-8da5-1501d4e7e843 |
> | [FHIR-Datenleser](#fhir-data-reader) | Die Rolle ermöglicht dem Benutzer oder Prinzipal das Lesen von FHIR-Daten. | 4c8d0bbc-75d3-4935-991f-5f3c56d81508 |
> | [FHIR-Datenschreiber](#fhir-data-writer) | Die Rolle ermöglicht dem Benutzer oder Prinzipal das Lesen und Schreiben von FHIR-Daten. | 3f88fce4-5892-4214-ae73-ba5294559913 |
> | [Mitwirkender für Integrationsdienstumgebungen](#integration-service-environment-contributor) | Hiermit wird das Verwalten von Integrationsdienstumgebungen ermöglicht, nicht aber der Zugriff auf diese. | a41e2c5b-bd99-4a07-88f4-9bf657a760b8 |
> | [Entwickler für Integrationsdienstumgebungen](#integration-service-environment-developer) | Hiermit wird Entwicklern das Erstellen und Aktualisieren von Workflows, Integrationskonten und API-Verbindungen in Integrationsdienstumgebungen ermöglicht. | c7aa55d3-1abb-444a-a5ca-5e51e485d6ec |
> | [Mitwirkender von Intelligent Systems-Konto](#intelligent-systems-account-contributor) | Ermöglicht Ihnen das Verwalten von Intelligent Systems-Konten, nicht aber den Zugriff darauf. | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | [Logik-App-Mitwirkender](#logic-app-contributor) | Ermöglicht Ihnen die Verwaltung von Logik-Apps. Sie können aber nicht den App-Zugriff ändern. | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | [Logik-App-Operator](#logic-app-operator) | Ermöglicht Ihnen das Lesen, Aktivieren und Deaktivieren von Logik-Apps. Sie können diese aber nicht bearbeiten oder aktualisieren. | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | **Identität** |  |  |
> | [Mitwirkender für verwaltete Identität](#managed-identity-contributor) | Dem Benutzer zugewiesene Identität erstellen, lesen, aktualisieren und löschen. | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | [Operator für verwaltete Identität](#managed-identity-operator) | Dem Benutzer zugewiesene Identität lesen und zuweisen. | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Security** |  |  |
> | [Attestation-Mitwirkender](#attestation-contributor) | Lesen, Schreiben oder Löschen der Nachweisanbieterinstanz | bbf86eb8-f7b4-4cce-96e4-18cddf81d86e |
> | [Attestation-Leser](#attestation-reader) | Lesen der Eigenschaften des Nachweisanbieters | fd1bd22b-8476-40bc-a0bc-69b95687b9f3 |
> | [Key Vault-Administrator](#key-vault-administrator) | Ausführen beliebiger Vorgänge auf Datenebene für einen Schlüsseltresor und alle darin enthaltenen Objekte (einschließlich Zertifikate, Schlüssel und Geheimnisse). Kann keine Key Vault-Ressourcen oder Rollenzuweisungen verwalten. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. | 00482a5a-887f-4fb3-b363-3b7fe8e74483 |
> | [Key Vault-Zertifikatbeauftragter](#key-vault-certificates-officer) | Ausführen beliebiger Aktionen für die Zertifikate eines Schlüsseltresors mit Ausnahme der Verwaltung von Berechtigungen. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. | a4417e6f-fecd-4de8-b567-7b0420556985 |
> | [Key Vault-Mitwirkender](#key-vault-contributor) | Verwalten von Schlüsseltresoren, gestattet Ihnen jedoch nicht, Rollen in Azure RBAC zuzuweisen, und ermöglicht keinen Zugriff auf Geheimnisse, Schlüssel oder Zertifikate. | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | [Key Vault-Kryptografiebeauftragter](#key-vault-crypto-officer) | Ausführen beliebiger Aktionen für die Schlüssel eines Schlüsseltresors mit Ausnahme der Verwaltung von Berechtigungen. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. | 14b46e9e-c2b7-41b4-b07b-48a6ebf60603 |
> | [Key Vault Crypto Service Encryption-Benutzer](#key-vault-crypto-service-encryption-user) | Lesen von Metadaten von Schlüsseln und Ausführen von Vorgängen zum Packen/Entpacken. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. | e147488a-f6f5-4113-8e2d-b22465e65bf6 |
> | [Key Vault-Kryptografiebenutzer](#key-vault-crypto-user) | Ausführen kryptografischer Vorgänge mithilfe von Schlüsseln. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. | 12338af0-0e69-4776-bea7-57ae8d297424 |
> | [Key Vault-Leser](#key-vault-reader) | Lesen von Metadaten von Schlüsseltresoren und deren Zertifikaten, Schlüsseln und Geheimnissen. Sensible Werte, z. B. der Inhalt von Geheimnissen oder Schlüsselmaterial, können nicht gelesen werden. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. | 21090545-7ca7-4776-b22c-e363652d74d2 |
> | [Key Vault-Geheimnisbeauftragter](#key-vault-secrets-officer) | Ausführen beliebiger Aktionen für die Geheimnisse eines Schlüsseltresors mit Ausnahme der Verwaltung von Berechtigungen. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. | b86a8fe4-44ce-4948-aee5-eccb2c155cd7 |
> | [Key Vault-Geheimnisbenutzer](#key-vault-secrets-user) | Lesen der Inhalte von Geheimnissen. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. | 4633458b-17de-408a-b874-0445c86b69e6 |
> | [Mitwirkender für verwaltete HSMs](#managed-hsm-contributor) | Ermöglicht Ihnen das Verwalten von verwalteten HSM-Pools, aber nicht den Zugriff auf diese. | 18500a29-7fe2-46b2-a342-b16a415e101d |
> | [Microsoft Sentinel Automation-Mitarbeiter](#microsoft-sentinel-automation-contributor) | Microsoft Sentinel Automation-Mitarbeiter | f4c81013-99ee-4d62-a7ee-b3f1f648599a |
> | [Microsoft Sentinel-Mitwirkender](#microsoft-sentinel-contributor) | Microsoft Sentinel-Mitwirkender | ab8e14d6-4a74-4a29-9ba8-549422addade |
> | [Microsoft Sentinel Reader](#microsoft-sentinel-reader) | Microsoft Sentinel Reader | 8d289c81-5878-46d4-8554-54e1e3d8b5cb |
> | [Microsoft Sentinel Responder](#microsoft-sentinel-responder) | Microsoft Sentinel Responder | 3e150937-b8fe-4cfb-8069-0eaf05ecd056 |
> | [Sicherheitsadministrator](#security-admin) | Anzeigen und Aktualisieren von Berechtigungen für Security Center. Gleiche Rechte wie der Sicherheitsleseberechtigte und kann darüber hinaus die Sicherheitsrichtlinie aktualisieren sowie Warnungen und Empfehlungen verwerfen. | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | [Mitwirkender für Sicherheitsbewertungen](#security-assessment-contributor) | Ermöglicht das Pushen von Bewertungen an Security Center | 612c2aa1-cb24-443b-ac28-3ab7272de6f5 |
> | [Sicherheits-Manager (Legacy)](#security-manager-legacy) | Dies ist eine Legacyrolle. Verwenden Sie stattdessen „Sicherheitsadministrator“. | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | [Sicherheitsleseberechtigter](#security-reader) | Anzeigen von Berechtigungen für Security Center. Kann Empfehlungen, Warnungen, Sicherheitsrichtlinien und Sicherheitszustände anzeigen, jedoch keine Änderungen vornehmen. | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **DevOps** |  |  |
> | [DevTest Labs-Benutzer](#devtest-labs-user) | Ermöglicht Ihnen das Verbinden, Starten, Neustarten und Herunterfahren Ihrer virtuellen Computer in Ihren Azure DevTest Labs. | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | [Lab-Ersteller](#lab-creator) | Ermöglicht Ihnen das Erstellen neuer Labs unter ihren Azure Lab-Konten. | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **Überwachen** |  |  |
> | [Mitwirkender der Application Insights-Komponente](#application-insights-component-contributor) | Kann Application Insights-Komponenten verwalten | ae349356-3a1b-4a5e-921d-050484c6347e |
> | [Application Insights-Momentaufnahmedebugger](#application-insights-snapshot-debugger) | Gibt dem Benutzer die Berechtigung zum Anzeigen und Herunterladen von Debugmomentaufnahmen, die mit dem Application Insights-Momentaufnahmedebugger erfasst wurden. Beachten Sie, dass diese Berechtigungen in der Rolle [Besitzer](#owner) oder [Mitwirkender](#contributor) nicht enthalten sind. Die Application Insights-Rolle „Momentaufnahmedebugger“ muss Benutzern direkt zugewiesen werden. Die Rolle wird nicht erkannt, wenn sie einer benutzerdefinierten Rolle hinzugefügt wird. | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | [Überwachungsmitwirkender](#monitoring-contributor) | Kann sämtliche Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](../azure-monitor/roles-permissions-security.md#built-in-monitoring-roles). | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | [Herausgeber von Überwachungsmetriken](#monitoring-metrics-publisher) | Ermöglicht die Veröffentlichung von Metriken für Azure-Ressourcen. | 3913510d-42f4-4e42-8a64-420c390055eb |
> | [Überwachungsleser](#monitoring-reader) | Kann alle Überwachungsdaten (Metriken, Protokolle usw.) lesen. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](../azure-monitor/roles-permissions-security.md#built-in-monitoring-roles). | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | [Arbeitsmappenmitwirkender](#workbook-contributor) | Kann freigegebene Arbeitsmappen speichern. | e8ddcd69-c73f-4f9f-9844-4100522f16ad |
> | [Arbeitsmappenleser](#workbook-reader) | Kann Arbeitsmappen lesen. | b279062a-9be3-42a0-92ae-8b3cf002ec4d |
> | **Verwaltung und Governance** |  |  |
> | [Mitwirkender für Automatisierung](#automation-contributor) | Verwalten von Azure Automation-Ressourcen und anderen Ressourcen mit Azure Automation. | f353d9bd-d4a6-484e-a77a-8050b599b867 |
> | [Automation-Auftragsoperator](#automation-job-operator) | Hiermit werden Aufträge mithilfe von Automation-Runbooks erstellt und verwaltet. | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | [Operator für Automation](#automation-operator) | Automatisierungsoperatoren können Aufträge starten, beenden, anhalten und fortsetzen. | d3881f73-407a-4167-8283-e981cbba0404 |
> | [Automation-Runbookoperator](#automation-runbook-operator) | Runbookeigenschaften lesen: Ermöglicht das Erstellen von Runbookaufträgen. | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | [Benutzerrolle für Azure Arc-aktivierte Kubernetes-Cluster](#azure-arc-enabled-kubernetes-cluster-user-role) | Aktion zum Auflisten der Anmeldeinformationen eines Clusterbenutzers. | 00493d72-78f6-4148-b6c5-d3ce8e4799dd |
> | [Azure Arc Kubernetes-Administrator](#azure-arc-kubernetes-admin) | Ermöglicht Ihnen das Verwalten aller Ressourcen unter einem Cluster/Namespace, außer das Aktualisieren oder Löschen von Ressourcenkontingenten und Namespaces. | dffb1e0c-446f-4dde-a09f-99eb5cc68b96 |
> | [Azure Arc Kubernetes-Clusteradministrator](#azure-arc-kubernetes-cluster-admin) | Ermöglicht Ihnen das Verwalten aller Ressourcen im Cluster. | 8393591c-06b9-48a2-a542-1bd6b377f6a2 |
> | [Anzeigeberechtigter für Azure Arc Kubernetes](#azure-arc-kubernetes-viewer) | Ermöglicht Ihnen das Anzeigen aller Ressourcen im Cluster/Namespace mit Ausnahme von Geheimnissen. | 63f0a09d-1495-4db4-a681-037d84835eb4 |
> | [Schreibberechtigter für Azure Arc Kubernetes](#azure-arc-kubernetes-writer) | Mit dieser Rolle können Sie alle Elemente im Cluster oder Namespace aktualisieren, mit Ausnahme von Rollen (Clusterrollen) und Rollenbindungen (Clusterrollenbindungen). | 5b999177-9696-4545-85c7-50de3797e5a1 |
> | [Onboarding von Azure Connected Machine](#azure-connected-machine-onboarding) | Kann Azure Connected Machine integrieren. | b64e21ea-ac4e-4cdf-9dc9-5b892992bee7 |
> | [Ressourcenadministrator für Azure Connected Machine](#azure-connected-machine-resource-administrator) | Kann Azure Connected Machines lesen, schreiben, löschen und erneut integrieren. | cd570a14-e51a-42ad-bac8-bafd67325302 |
> | [Abrechnungsleser](#billing-reader) | Hiermit wird Lesezugriff auf Abrechnungsdaten ermöglicht. | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | [Blueprint-Mitwirkender](#blueprint-contributor) | Kann Blaupausendefinitionen verwalten, aber nicht zuweisen. | 41077137-e803-4205-871c-5a86e6a753b4 |
> | [Blueprint-Operator](#blueprint-operator) | Kann vorhandene veröffentlichte Blaupausen zuweisen, aber keine neuen Blaupausen erstellen. Beachten Sie, dass dies nur funktioniert, wenn die Zuweisung mit einer vom Benutzer zugewiesenen verwalteten Identität erfolgt. | 437d2ced-4a38-4302-8479-ed2bcb43d090 |
> | [Mitwirkender für Cost Management](#cost-management-contributor) | Ermöglicht Ihnen das Anzeigen der Kosten und das Verwalten der Kostenkonfiguration (z. B. Budgets, Exporte). | 434105ed-43f6-45c7-a02f-909b2ba83430 |
> | [Cost Management-Leser](#cost-management-reader) | Ermöglicht Ihnen das Anzeigen der Kostendaten und -konfiguration (z. B. Budgets, Exporte). | 72fafb9e-0641-4937-9268-a91bfd8191a3 |
> | [Hierarchieeinstellungsadministrator](#hierarchy-settings-administrator) | Ermöglicht Benutzern das Bearbeiten und Löschen von Hierarchieeinstellungen. | 350f8d15-c687-4448-8ae1-157740a3936d |
> | [Kubernetes-Cluster – Azure Arc-Onboarding](#kubernetes-cluster---azure-arc-onboarding) | Rollendefinition zum Autorisieren eines Benutzers/Diensts zum Erstellen einer connectedClusters-Ressource | 34e09817-6cbe-4d01-b1a2-e0eac5743d41 |
> | [Mitwirkender für Kubernetes-Erweiterungen](#kubernetes-extension-contributor) | Kann Kubernetes-Erweiterungen erstellen, aktualisieren, abrufen, auflisten und löschen und asynchrone Vorgänge für Kubernetes-Erweiterungen abrufen. | 85cb6faf-e071-4c9b-8136-154b5a04f717 |
> | [Rolle „Mitwirkender für verwaltete Anwendungen“](#managed-application-contributor-role) | Ermöglicht das Erstellen von Ressourcen für verwaltete Anwendungen. | 641177b8-a67a-45b9-a033-47bc880bb21e |
> | [Rolle „Bediener für verwaltete Anwendung“](#managed-application-operator-role) | Ermöglicht Ihnen das Lesen und Durchführen von Aktionen für Ressourcen der verwalteten Anwendung. | c7393b34-138c-406f-901b-d8cf2b17e6ae |
> | [Leser für verwaltete Anwendungen](#managed-applications-reader) | Ermöglicht Ihnen, Ressourcen in einer verwalteten App zu lesen und JIT-Zugriff anzufordern. | b9331d33-8a36-4f8c-b097-4f54124fdb44 |
> | [Rolle „Registrierungszuweisung für verwaltete Dienste löschen“](#managed-services-registration-assignment-delete-role) | Mit der Rolle „Registrierungszuweisung für verwaltete Dienste löschen“ können Benutzer des verwaltenden Mandanten die ihrem Mandanten zugewiesene Registrierungszuweisung löschen. | 91c1777a-f3dc-4fae-b103-61d183457e46 |
> | [Verwaltungsgruppenmitwirkender](#management-group-contributor) | Rolle „Verwaltungsgruppenmitwirkender“ | 5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c |
> | [Verwaltungsgruppenleser](#management-group-reader) | Rolle „Verwaltungsgruppenleser“ | ac63b705-F282-497d-ac71-919bf39d939d |
> | [Mitwirkender von New Relic APM-Konto](#new-relic-apm-account-contributor) | Ermöglicht Ihnen das Verwalten von New Relic Application Performance Management-Konten und -Anwendungen, nicht aber den Zugriff auf diese. | 5d28c62d-5b37-4476-8438-e587778df237 |
> | [Policy Insights-Datenschreiber (Vorschau)](#policy-insights-data-writer-preview) | Ermöglicht Lesezugriff auf Ressourcenrichtlinien und Schreibzugriff auf Richtlinienereignisse für Ressourcenkomponenten. | 66bb4e9e-b016-4a94-8249-4c0511c2be84 |
> | [Kontingentanforderungsoperator](#quota-request-operator) | Liest und erstellt Kontingentanforderungen, ruft den Status von Kontingentanforderungen ab und erstellt Supporttickets. | 0e5f05e5-9ab9-446b-b98d-1e2157c94125 |
> | [Reservierungseinkäufer](#reservation-purchaser) | Ermöglicht den Kauf von Reservierungen | f7b75c60-3036-4b75-91c3-6b41c27c1689 |
> | [Mitwirkender bei Ressourcenrichtlinien](#resource-policy-contributor) | Benutzer mit Rechten zum Erstellen/Ändern der Ressourcenrichtlinie, zum Erstellen eines Supporttickets und zum Lesen von Ressourcen/der Hierarchie. | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | [Site Recovery-Mitwirkender](#site-recovery-contributor) | Ermöglicht Ihnen die Verwaltung des Site Recovery-Diensts mit Ausnahme der Tresorerstellung und der Rollenzuweisung. | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | [Site Recovery-Operator](#site-recovery-operator) | Ermöglicht Ihnen ein Failover und ein Failback, aber nicht das Durchführen weiterer Site Recovery-Verwaltungsvorgänge. | 494ae006-db33-4328-bf46-533a6560a3ca |
> | [Site Recovery-Leser](#site-recovery-reader) | Ermöglicht Ihnen die Anzeige des Site Recovery-Status, aber nicht die Durchführung weiterer Verwaltungsvorgänge. | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | [Mitwirkender für Supportanfragen](#support-request-contributor) | Ermöglicht Ihnen die Erstellung und Verwaltung von Supportanfragen. | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | [Tagmitwirkender](#tag-contributor) | Hiermit können Tags für Entitäten verwaltet werden, ohne Zugriff auf die Entitäten selbst zu gewähren. | 4a9ae827-6dc8-4573-8ac7-8239d42aa03f |
> | **Andere** |  |  |
> | [Azure Digital Twins Data Owner (Azure Digital Twins-Datenbesitzer)](#azure-digital-twins-data-owner) | Vollzugriffsrolle für Digital Twins-Datenebene | bcd981a7-7f74-457b-83e1-cceb9e632ffe |
> | [Azure Digital Twins Data Reader (Azure Digital Twins-Datenleser)](#azure-digital-twins-data-reader) | Schreibgeschützte Rolle für Digital Twins-Datenebeneneigenschaften | d57506d4-4c8d-48b1-8587-93c323f6a5a3 |
> | [Mitwirkender von BizTalk](#biztalk-contributor) | Ermöglicht Ihnen das Verwalten von BizTalk-Diensten, nicht aber den Zugriff darauf. | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | [Anwendungsgruppenmitwirkender für die Desktopvirtualisierung](#desktop-virtualization-application-group-contributor) | Mitwirkender der Anwendungsgruppe für die Desktopvirtualisierung. | 86240b0e-9422-4c43-887b-b61143f32ba8 |
> | [Anwendungsgruppenleser für die Desktopvirtualisierung](#desktop-virtualization-application-group-reader) | Leser der Anwendungsgruppe für die Desktopvirtualisierung. | aebf23d0-b568-4e86-b8f9-fe83a2c6ab55 |
> | [Desktopvirtualisierungsmitwirkender](#desktop-virtualization-contributor) | Mitwirkender für die Desktopvirtualisierung. | 082f0a83-3be5-4ba1-904c-961cca79b387 |
> | [Hostpoolmitwirkender für die Desktopvirtualisierung](#desktop-virtualization-host-pool-contributor) | Mitwirkender des Hostpools für die Desktopvirtualisierung. | e307426c-f9b6-4e81-87de-d99efb3c32bc |
> | [Hostpoolleser für die Desktopvirtualisierung](#desktop-virtualization-host-pool-reader) | Leser des Hostpools für die Desktopvirtualisierung. | ceadfde2-b300-400a-ab7b-6143895aa822 |
> | [Desktopvirtualisierungsleser](#desktop-virtualization-reader) | Leser für die Desktopvirtualisierung. | 49a72310-ab8d-41df-bbb0-79b649203868 |
> | [Sitzungshostoperator für die Desktopvirtualisierung](#desktop-virtualization-session-host-operator) | Operator des Sitzungshosts für die Desktopvirtualisierung. | 2ad6aaab-ead9-4eaa-8ac5-da422f562408 |
> | [Desktopvirtualisierungsbenutzer](#desktop-virtualization-user) | Ermöglicht dem Benutzer die Verwendung der Anwendungen in einer Anwendungsgruppe. | 1d18fff3-a72a-46b5-b4a9-0b38a3cd7e63 |
> | [Benutzersitzungsoperator für die Desktopvirtualisierung](#desktop-virtualization-user-session-operator) | Operator der Benutzersitzung für die Desktopvirtualisierung. | ea4bfff8-7fb4-485a-aadd-d4129a0ffaa6 |
> | [Arbeitsbereichsmitwirkender für die Desktopvirtualisierung](#desktop-virtualization-workspace-contributor) | Mitwirkender des Arbeitsbereichs für die Desktopvirtualisierung. | 21efdde3-836f-432b-bf3d-3e8e734d4b2b |
> | [Arbeitsbereichsleser für die Desktopvirtualisierung](#desktop-virtualization-workspace-reader) | Leser des Arbeitsbereichs für die Desktopvirtualisierung. | 0fa44ee9-7a7d-466b-9bb2-2bf446b1204d |
> | [Leser für die Datenträgersicherung](#disk-backup-reader) | Berechtigung für den Sicherungstresor zum Ausführen einer Datenträgersicherung. | 3e5e47e6-65f7-47ef-90b5-e5dd4d455f24 |
> | [Datenträgerpooloperator](#disk-pool-operator) | Gewährt dem StoragePool-Ressourcenanbieter die Berechtigung zum Verwalten von Datenträgern, die einem Datenträgerpool hinzugefügt wurden | 60fc6e62-5479-42d4-8bf4-67625fcc2840 |
> | [Operator für die Datenträgerwiederherstellung](#disk-restore-operator) | Berechtigung für den Sicherungstresor zum Ausführen einer Datenträgerwiederherstellung. | b50d9833-a0cb-478e-945f-707fcc997c13 |
> | [Mitwirkender für Datenträgermomentaufnahmen](#disk-snapshot-contributor) | Berechtigung für den Sicherungstresor zum Verwalten von Datenträgermomentaufnahmen. | 7efff54f-a5b4-42b5-a1c5-5411624893ce |
> | [Mitwirkender von Zeitplanungsauftragssammlung](#scheduler-job-collections-contributor) | Ermöglicht Ihnen das Verwalten von Scheduler-Auftragssammlungen, nicht aber den Zugriff darauf. | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |
> | [Diensthuboperator](#services-hub-operator) | Berechtigungen für sämtliche Lese-, Schreib- und Löschvorgänge im Zusammenhang mit Dienstubconnectors. | 82200a5b-e217-47a5-b665-6d8765ee745b |


## <a name="general"></a>Allgemein


### <a name="contributor"></a>Mitwirkender

Hiermit wird Vollzugriff zum Verwalten aller Ressourcen gewährt, allerdings nicht zum Zuweisen von Rollen in Azure RBAC, zum Verwalten von Zuweisungen in Azure Blueprints oder zum Teilen von Imagekatalogen. [Weitere Informationen](rbac-and-directory-admin-roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | * | Erstellen und Verwalten von Ressourcen aller Typen |
> | **NotActions** |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/Delete | Löschen von Rollen, Richtlinienzuweisungen, Richtliniendefinitionen und Richtliniensatzdefinitionen |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/Write | Erstellen von Rollen, Rollenzuweisungen, Richtlinienzuweisungen, Richtliniendefinitionen und Richtliniensatzdefinitionen |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/elevateAccess/Action | Gewährt dem Aufrufer Zugriff vom Typ „Benutzerzugriffsadministrator“ auf der Mandantenebene. |
> | [Microsoft.Blueprint](resource-provider-operations.md#microsoftblueprint)/blueprintAssignments/write | Erstellt oder aktualisiert alle Blaupausenzuweisungen |
> | [Microsoft.Blueprint](resource-provider-operations.md#microsoftblueprint)/blueprintAssignments/delete | Löscht alle Blaupausenzuweisungen |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/galleries/share/action | Gibt einen Katalog in verschiedenen Bereichen frei. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants full access to manage all resources, but does not allow you to assign roles in Azure RBAC, manage assignments in Azure Blueprints, or share image galleries.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
  "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": [
        "Microsoft.Authorization/*/Delete",
        "Microsoft.Authorization/*/Write",
        "Microsoft.Authorization/elevateAccess/Action",
        "Microsoft.Blueprint/blueprintAssignments/write",
        "Microsoft.Blueprint/blueprintAssignments/delete",
        "Microsoft.Compute/galleries/share/action"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="owner"></a>Besitzer

Hiermit wird Vollzugriff zum Verwalten aller Ressourcen gewährt, einschließlich der Möglichkeit, Rollen in Azure RBAC zuzuweisen. [Weitere Informationen](rbac-and-directory-admin-roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | * | Erstellen und Verwalten von Ressourcen aller Typen |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants full access to manage all resources, including the ability to assign roles in Azure RBAC.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
  "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reader"></a>Leser

Hiermit können Sie alle Ressourcen anzeigen, aber keine Änderungen vornehmen. [Weitere Informationen](rbac-and-directory-admin-roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View all resources, but does not allow you to make any changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
  "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
  "permissions": [
    {
      "actions": [
        "*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="user-access-administrator"></a>Benutzerzugriffsadministrator

Ermöglicht Ihnen die Verwaltung von Benutzerzugriffen auf Azure-Ressourcen. [Weitere Informationen](rbac-and-directory-admin-roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/* | Verwalten der Autorisierung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage user access to Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
  "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Authorization/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "User Access Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="compute"></a>Compute


### <a name="classic-virtual-machine-contributor"></a>Mitwirkender von klassischen virtuellen Computern

Ermöglicht Ihnen das Verwalten klassischer virtueller Computer, aber weder den Zugriff darauf noch auf die mit ihnen verbundenen virtuellen Netzwerke oder Speicherkonten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.ClassicCompute](resource-provider-operations.md#microsoftclassiccompute)/domainNames/* | Erstellen und Verwalten von klassischen Compute-Domänennamen |
> | [Microsoft.ClassicCompute](resource-provider-operations.md#microsoftclassiccompute)/virtualMachines/* | Erstellen und Verwalten von virtuellen Computern |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/networkSecurityGroups/join/action |  |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/reservedIps/link/action | Dient zum Verknüpfen einer reservierten IP-Adresse. |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/reservedIps/read | Ruft die reservierten IP-Adressen ab. |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/virtualNetworks/join/action | Führt zum Beitritt zum virtuellen Netzwerk. |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/virtualNetworks/read | Dient zum Abrufen des virtuellen Netzwerks. |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/disks/read | Gibt den Speicherkontodatenträger zurück. |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/images/read | Gibt das Speicherkontoimage zurück. (Veraltet. Verwenden Sie „Microsoft.ClassicStorage/storageAccounts/vmImages“) |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/read | Dient zum Zurückgeben des Speicherkontos mit dem angegebenen Konto. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic virtual machines, but not access to them, and not the virtual network or storage account they're connected to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d73bb868-a0df-4d4d-bd69-98a00b01fccb",
  "name": "d73bb868-a0df-4d4d-bd69-98a00b01fccb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicCompute/domainNames/*",
        "Microsoft.ClassicCompute/virtualMachines/*",
        "Microsoft.ClassicNetwork/networkSecurityGroups/join/action",
        "Microsoft.ClassicNetwork/reservedIps/link/action",
        "Microsoft.ClassicNetwork/reservedIps/read",
        "Microsoft.ClassicNetwork/virtualNetworks/join/action",
        "Microsoft.ClassicNetwork/virtualNetworks/read",
        "Microsoft.ClassicStorage/storageAccounts/disks/read",
        "Microsoft.ClassicStorage/storageAccounts/images/read",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.ClassicStorage/storageAccounts/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Virtual Machine Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-administrator-login"></a>VM-Administratoranmeldung

Anzeigen von virtuellen Computern im Portal und Anmelden als Administrator [Weitere Informationen](../active-directory/devices/howto-vm-sign-in-azure-ad-windows.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/read | Ruft eine Lastenausgleichsdefinition ab. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/*/read |  |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/login/action | Hiermit melden Sie sich bei einem virtuellen Computer als normaler Benutzer an. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/loginAsAdmin/action | Hiermit melden Sie sich bei einem virtuellen Computer mit Windows-Administrator- oder Linux-Root-Benutzerrechten an. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/login/action | Hiermit melden Sie sich bei einem Azure Arc-Computer als normaler Benutzer an. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/loginAsAdmin/action | Hiermit melden Sie sich bei einem Azure Arc-Computer mit Windows-Administrator- oder Linux-Root-Benutzerrechten an. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View Virtual Machines in the portal and login as administrator",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/1c0163c0-47e6-4577-8991-ea5c82e286e4",
  "name": "1c0163c0-47e6-4577-8991-ea5c82e286e4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Compute/virtualMachines/*/read",
        "Microsoft.HybridCompute/machines/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Compute/virtualMachines/login/action",
        "Microsoft.Compute/virtualMachines/loginAsAdmin/action",
        "Microsoft.HybridCompute/machines/login/action",
        "Microsoft.HybridCompute/machines/loginAsAdmin/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine Administrator Login",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-contributor"></a>Mitwirkender von virtuellen Computern

Erstellen und Verwalten von VMs, Verwalten von Datenträgern und Datenträgermomentaufnahmen, Installieren und Ausführen von Software, Zurücksetzen des Kennworts des Stammbenutzers der VM mit VM-Erweiterungen und Verwalten lokaler Benutzerkonten mit VM-Erweiterungen. Diese Rolle gewährt Ihnen keinen Verwaltungszugriff auf das virtuelle Netzwerk oder das Speicherkonto, mit dem die VMs verbunden sind. Diese Rolle ermöglicht es Ihnen nicht, Rollen in Azure RBAC zuzuweisen. [Weitere Informationen](/azure/architecture/reference-architectures/n-tier/linux-vm)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/availabilitySets/* | Erstellen und Verwalten von Compute-Verfügbarkeitsgruppen |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/locations/* | Erstellen und Verwalten von Compute-Speicherorten |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/* | Ausführen beliebiger Aktionen für virtuelle Computer, einschließlich Erstellen, Aktualisieren, Löschen, Starten, Neustarten und Ausschalten virtueller Computer. Ausführen von Skripts auf VMs |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachineScaleSets/* | Erstellen und Verwalten von Skalierungsgruppen für virtuelle Computer |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/cloudServices/* |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/write | Erstellt einen neuen Datenträger oder aktualisiert einen bereits vorhandenen. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/read | Dient zum Abrufen der Eigenschaften eines Datenträgers. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/delete | Löscht den Datenträger. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/schedules/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/applicationGateways/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für ein Application Gateway. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für den Lastenausgleich. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/inboundNatPools/join/action | Verknüpft einen NAT-Pool für eingehenden Datenverkehr für den Lastenausgleich. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/inboundNatRules/join/action | Verknüpft eine NAT-Regel für eingehenden Datenverkehr für den Lastenausgleich. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/probes/join/action | Ermöglicht die Verwendung von Prüfpunkten eines Lastenausgleichs. Beispielsweise kann mit dieser Berechtigung die healthProbe-Eigenschaft einer VM-Skalierungsgruppe auf den Prüfpunkt verweisen. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/read | Ruft eine Lastenausgleichsdefinition ab. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/locations/* | Erstellen und Verwalten von Netzwerkspeicherorten |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/* | Erstellen und Verwalten von Netzwerkschnittstellen |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkSecurityGroups/join/action | Verknüpft eine Netzwerksicherheitsgruppe. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkSecurityGroups/read | Ruft eine Netzwerksicherheitsgruppen-Definition ab. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/join/action | Verknüpft eine öffentliche IP-Adresse. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. Nicht warnbar. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/backupProtectionIntent/write | Erstellt einen beabsichtigten Sicherungsschutz. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/write | Dient zum Erstellen eines geschützten Elements für die Sicherung. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/write | Erstellt Schutzrichtlinien. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/write | Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | Microsoft.SerialConsole/serialPorts/connect/action | Verbinden mit einem seriellen Anschluss |
> | [Microsoft.SqlVirtualMachine](resource-provider-operations.md#microsoftsqlvirtualmachine)/* |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they're connected to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
  "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/availabilitySets/*",
        "Microsoft.Compute/locations/*",
        "Microsoft.Compute/virtualMachines/*",
        "Microsoft.Compute/virtualMachineScaleSets/*",
        "Microsoft.Compute/cloudServices/*",
        "Microsoft.Compute/disks/write",
        "Microsoft.Compute/disks/read",
        "Microsoft.Compute/disks/delete",
        "Microsoft.DevTestLab/schedules/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
        "Microsoft.Network/loadBalancers/probes/join/action",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/locations/*",
        "Microsoft.Network/networkInterfaces/*",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Network/networkSecurityGroups/read",
        "Microsoft.Network/publicIPAddresses/join/action",
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.RecoveryServices/locations/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/write",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/write",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.SerialConsole/serialPorts/connect/action",
        "Microsoft.SqlVirtualMachine/*",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-user-login"></a>VM-Benutzeranmeldung

Anzeigen von virtuellen Computern im Portal und Anmelden als regulärer Benutzer. [Weitere Informationen](../active-directory/devices/howto-vm-sign-in-azure-ad-windows.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/read | Ruft eine Lastenausgleichsdefinition ab. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/*/read |  |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/login/action | Hiermit melden Sie sich bei einem virtuellen Computer als normaler Benutzer an. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/login/action | Hiermit melden Sie sich bei einem Azure Arc-Computer als normaler Benutzer an. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View Virtual Machines in the portal and login as a regular user.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fb879df8-f326-4884-b1cf-06f3ad86be52",
  "name": "fb879df8-f326-4884-b1cf-06f3ad86be52",
  "permissions": [
    {
      "actions": [
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Compute/virtualMachines/*/read",
        "Microsoft.HybridCompute/machines/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Compute/virtualMachines/login/action",
        "Microsoft.HybridCompute/machines/login/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine User Login",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="networking"></a>Netzwerk


### <a name="cdn-endpoint-contributor"></a>Mitwirkender für den CDN-Endpunkt

Diese Rolle kann CDN-Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/edgenodes/read |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/operationresults/* |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/profiles/endpoints/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage CDN endpoints, but can't grant access to other users.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/426e0c7f-0c7e-4658-b36f-ff54d6c29b45",
  "name": "426e0c7f-0c7e-4658-b36f-ff54d6c29b45",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/endpoints/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Endpoint Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-endpoint-reader"></a>CDN-Endpunktleser

Diese Rolle kann CDN-Endpunkte anzeigen, aber keine Änderungen vornehmen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/edgenodes/read |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/operationresults/* |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/profiles/endpoints/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view CDN endpoints, but can't make changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/871e35f6-b5c1-49cc-a043-bde969a0f2cd",
  "name": "871e35f6-b5c1-49cc-a043-bde969a0f2cd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/endpoints/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Endpoint Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-profile-contributor"></a>Mitwirkender für das CDN-Profil

Diese Rolle kann CDN-Profile und deren Endpunkte verwalten, aber anderen Benutzern keinen Zugriff erteilen. [Weitere Informationen](../cdn/cdn-app-dev-net.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/edgenodes/read |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/operationresults/* |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/profiles/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage CDN profiles and their endpoints, but can't grant access to other users.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ec156ff8-a8d1-4d15-830c-5b80698ca432",
  "name": "ec156ff8-a8d1-4d15-830c-5b80698ca432",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Profile Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-profile-reader"></a>CDN-Profilleser

Diese Rolle kann CDN-Profile und deren Endpunkte anzeigen, aber keine Änderungen vornehmen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/edgenodes/read |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/operationresults/* |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/profiles/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view CDN profiles and their endpoints, but can't make changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8f96442b-4075-438f-813d-ad51ab4019af",
  "name": "8f96442b-4075-438f-813d-ad51ab4019af",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Profile Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-network-contributor"></a>Mitwirkender von klassischem Netzwerk

Ermöglicht Ihnen das Verwalten von klassischen Netzwerken, nicht aber den Zugriff darauf. [Weitere Informationen](../virtual-network/virtual-network-manage-peering.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/* | Erstellen und Verwalten von klassischen Netzwerken |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic networks, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b34d265f-36f7-4a0d-a4d4-e158ca92e90f",
  "name": "b34d265f-36f7-4a0d-a4d4-e158ca92e90f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicNetwork/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Network Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="dns-zone-contributor"></a>DNS Zone Contributor

Ermöglicht Ihnen die Verwaltung von DNS-Zonen und Ressourceneintragssätzen in Azure DNS, aber nicht zu steuern, wer darauf Zugriff hat. [Weitere Informationen](../dns/dns-protect-zones-recordsets.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/dnsZones/* | Erstellen und Verwalten von DNS-Zonen und -Einträgen |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage DNS zones and record sets in Azure DNS, but does not let you control who has access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/befefa01-2a29-4197-83a8-272ff33ce314",
  "name": "befefa01-2a29-4197-83a8-272ff33ce314",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/dnsZones/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DNS Zone Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="network-contributor"></a>Mitwirkender von virtuellem Netzwerk

Ermöglicht Ihnen das Verwalten von Netzwerken, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/* | Erstellen und Verwalten von Netzwerken |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage networks, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4d97b98b-1d4f-4787-a291-c67834d212e7",
  "name": "4d97b98b-1d4f-4787-a291-c67834d212e7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Network Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="private-dns-zone-contributor"></a>Mitwirkender für private DNS-Zone

Ermöglicht Ihnen das Verwalten privater DNS-Zonenressourcen, aber nicht der virtuellen Netzwerke, mit denen sie verknüpft sind. [Weitere Informationen](../dns/dns-protect-private-zones-recordsets.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/privateDnsZones/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/privateDnsOperationResults/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/privateDnsOperationStatuses/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/join/action | Verknüpft ein virtuelles Netzwerk. Nicht warnbar. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage private DNS zone resources, but not the virtual networks they are linked to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b12aa53e-6015-4669-85d0-8515ebb3ae7f",
  "name": "b12aa53e-6015-4669-85d0-8515ebb3ae7f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/privateDnsZones/*",
        "Microsoft.Network/privateDnsOperationResults/*",
        "Microsoft.Network/privateDnsOperationStatuses/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/join/action",
        "Microsoft.Authorization/*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Private DNS Zone Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="traffic-manager-contributor"></a>Traffic Manager-Mitwirkender

Ermöglicht Ihnen die Verwaltung von Traffic Manager-Profilen, aber nicht die Steuerung des Zugriffs darauf.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/trafficManagerProfiles/* |  |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Traffic Manager profiles, but does not let you control who has access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a4b10055-b0c7-44c2-b00f-c7b5b3550cf7",
  "name": "a4b10055-b0c7-44c2-b00f-c7b5b3550cf7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/trafficManagerProfiles/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Traffic Manager Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="storage"></a>Storage


### <a name="avere-contributor"></a>Avere-Mitwirkender

Kann einen Avere vFXT-Cluster erstellen und verwalten. [Weitere Informationen](../avere-vfxt/avere-vfxt-deploy-plan.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/*/read |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/availabilitySets/* |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/proximityPlacementGroups/* |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/* |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/*/read |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/read | Ruft eine Subnetzdefinition für virtuelle Netzwerke ab. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkSecurityGroups/join/action | Verknüpft eine Netzwerksicherheitsgruppe. Nicht warnbar. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/*/read |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/resources/read | Ruft die Ressourcen für die Ressourcengruppe ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/delete | Gibt das Ergebnis beim Löschen eines Blobs zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/read | Gibt ein Blob oder eine Liste von Blobs zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/write | Gibt das Ergebnis beim Schreiben eines Blobs zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can create and manage an Avere vFXT cluster.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4f8fab4f-1852-4a58-a46a-8eaf358af14a",
  "name": "4f8fab4f-1852-4a58-a46a-8eaf358af14a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/*/read",
        "Microsoft.Compute/availabilitySets/*",
        "Microsoft.Compute/proximityPlacementGroups/*",
        "Microsoft.Compute/virtualMachines/*",
        "Microsoft.Compute/disks/*",
        "Microsoft.Network/*/read",
        "Microsoft.Network/networkInterfaces/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/*/read",
        "Microsoft.Storage/storageAccounts/*",
        "Microsoft.Support/*",
        "Microsoft.Resources/subscriptions/resourceGroups/resources/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Avere Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="avere-operator"></a>Avere-Bediener

Wird vom Avere vFXT-Cluster zum Verwalten des Clusters verwendet [Weitere Informationen](../avere-vfxt/avere-vfxt-manage-cluster.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/read | Dient zum Abrufen der Eigenschaften eines virtuellen Computers. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/write | Erstellt eine Netzwerkschnittstelle oder aktualisiert eine vorhandene Netzwerkschnittstelle.  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/read | Ruft eine Subnetzdefinition für virtuelle Netzwerke ab. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkSecurityGroups/join/action | Verknüpft eine Netzwerksicherheitsgruppe. Nicht warnbar. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/delete | Gibt das Ergebnis beim Löschen eines Containers zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/read | Hiermit wird eine Liste von Containern zurückgegeben. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/write | Gibt das Ergebnis des PUT-Vorgangs für den Blobcontainer zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/delete | Gibt das Ergebnis beim Löschen eines Blobs zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/read | Gibt ein Blob oder eine Liste von Blobs zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/write | Gibt das Ergebnis beim Schreiben eines Blobs zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Used by the Avere vFXT cluster to manage the cluster",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c025889f-8102-4ebf-b32c-fc0c6f0c6bd9",
  "name": "c025889f-8102-4ebf-b32c-fc0c6f0c6bd9",
  "permissions": [
    {
      "actions": [
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Avere Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-contributor"></a>Mitwirkender für Sicherungen

Ermöglicht Ihnen die Verwaltung des Sicherungsdiensts. Sie können jedoch keine Tresore erstellen oder anderen Benutzern Zugriff gewähren. [Weitere Informationen](../backup/backup-rbac-rs-vault.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/operationResults/* | Verwalten der Ergebnisse eines Vorgangs in der Sicherungsverwaltung |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/* | Erstellen und Verwalten von Sicherungscontainern in Sicherungsfabrics des Recovery Services-Tresors |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/refreshContainers/action | Aktualisiert die Containerliste. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobs/* | Erstellen und Verwalten von Sicherungsaufträgen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobsExport/action | Dient zum Exportieren von Aufträgen. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperationResults/* | Erstellen und Verwalten der Ergebnisse von Sicherungsverwaltungsvorgängen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/* | Erstellen und Verwalten von Sicherungsrichtlinien |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectableItems/* | Erstellen und Verwalten von Elementen, die gesichert werden können |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectedItems/* | Erstellen und Verwalten von gesicherten Elementen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionContainers/* | Erstellen und Verwalten von Containern mit Sicherungselementen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupSecurityPIN/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/certificates/* | Erstellen und Verwalten von Zertifikaten in Zusammenhang mit Sicherungen in einem Recovery Services-Tresor |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/* | Erstellen und Verwalten erweiterter Informationen in Zusammenhang mit einem Tresor |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/* | Erstellen und Verwalten von registrierten Identitäten |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/* | Erstellen und Verwalten der Nutzung des Recovery Services-Tresors |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupstorageconfig/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupconfig/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupValidateOperation/action | Überprüft Vorgang für geschütztes Element. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/write | Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperations/read | Gibt den Status eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupEngines/read | Gibt alle Sicherungsverwaltungsserver zurück, die beim Tresor registriert sind. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/backupProtectionIntent/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectableContainers/read | Ruft alle schützbaren Container ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupStatus/action | Überprüft den Sicherungsstatus für Recovery Services-Tresore. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupPreValidateProtection/action |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupValidateFeatures/action | Überprüft Features. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/write | Löst die Warnung auf. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/operations/read | Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/operationStatus/read | Ruft den Vorgangsstatus eines angegebenen Vorgangs ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionIntents/read | Listet den gesamten beabsichtigten Sicherungsschutz auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/getBackupStatus/action | Überprüft den Sicherungsstatus für Recovery Services-Tresore. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/write | Erstellt eine Sicherungsinstanz. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/delete | Löscht die Sicherungsinstanz. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Gibt alle Sicherungsinstanzen zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Gibt alle Sicherungsinstanzen zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/backup/action | Führt die Sicherung für die Sicherungsinstanz durch. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/validateRestore/action | Führt eine Überprüfung auf eine Wiederherstellung der Sicherungsinstanz durch. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/restore/action | Löst die Wiederherstellung auf der Sicherungsinstanz aus. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/write | Erstellt eine Sicherungsrichtlinie. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/delete | Löscht die Sicherungsrichtlinie. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Gibt alle Sicherungsrichtlinien zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Gibt alle Sicherungsrichtlinien zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Gibt alle Wiederherstellungspunkte zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Gibt alle Wiederherstellungspunkte zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/findRestorableTimeRanges/action | Sucht nach wiederherstellbaren Zeitbereichen. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/write | Mit dem Vorgang „Sicherungstresor erstellen“ wird eine Azure-Ressource vom Typ „Sicherungstresor“ erstellt. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Ruft die Liste mit den Sicherungstresoren in einem Abonnement ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/operationResults/read | Ruft das Vorgangsergebnis eines Patchvorgangs für einen Sicherungstresor ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/checkNameAvailability/action | Überprüft, ob der angeforderte BackupVault-Name verfügbar ist. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Ruft die Liste mit den Sicherungstresoren in einem Abonnement ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Ruft die Liste mit den Sicherungstresoren in einem Abonnement ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationStatus/read | Gibt den Status eines Sicherungsvorgangs für einen Sicherungstresor zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationResults/read | Gibt das Ergebnis eines Sicherungsvorgangs für einen Sicherungstresor zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/validateForBackup/action | Führt eine Überprüfung auf eine Sicherung der Sicherungsinstanz durch. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/providers/operations/read | Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage backup service,but can't create vaults and give access to others",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5e467623-bb1f-42f4-a55d-6e525e11384b",
  "name": "5e467623-bb1f-42f4-a55d-6e525e11384b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action",
        "Microsoft.RecoveryServices/Vaults/backupJobs/*",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectableItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/*",
        "Microsoft.RecoveryServices/Vaults/backupSecurityPIN/*",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/certificates/*",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/*",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/*",
        "Microsoft.RecoveryServices/Vaults/usages/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupValidateOperation/action",
        "Microsoft.RecoveryServices/Vaults/write",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/locations/backupPreValidateProtection/action",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.Support/*",
        "Microsoft.DataProtection/locations/getBackupStatus/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/write",
        "Microsoft.DataProtection/backupVaults/backupInstances/delete",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/backup/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/validateRestore/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/restore/action",
        "Microsoft.DataProtection/backupVaults/backupPolicies/write",
        "Microsoft.DataProtection/backupVaults/backupPolicies/delete",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/findRestorableTimeRanges/action",
        "Microsoft.DataProtection/backupVaults/write",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/operationResults/read",
        "Microsoft.DataProtection/locations/checkNameAvailability/action",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/locations/operationStatus/read",
        "Microsoft.DataProtection/locations/operationResults/read",
        "Microsoft.DataProtection/backupVaults/validateForBackup/action",
        "Microsoft.DataProtection/providers/operations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-operator"></a>Sicherungsoperator

Ermöglicht Ihnen das Verwalten von Sicherungsdiensten, jedoch nicht das Entfernen der Sicherung, die Tresorerstellung und das Erteilen von Zugriff an andere Benutzer. [Weitere Informationen](../backup/backup-rbac-rs-vault.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/operationResults/read | Gibt den Status des Vorgangs zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für den Schutzcontainer ausgeführt wurde. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Führt eine Sicherung für geschützte Elemente aus. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für geschützte Elemente ausgeführt wurde. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Dient zum Bereitstellen der sofortigen Elementwiederherstellung für geschützte Elemente. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/accessToken/action | Ruft das AccessToken für die regionsübergreifende Wiederherstellung ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Dient zum Abrufen von Wiederherstellungspunkten für geschützte Elemente. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Dient zum Wiederherstellen von Wiederherstellungspunkten für geschützte Elemente. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Dient zum Widerrufen der sofortigen Elementwiederherstellung für geschützte Elemente. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/write | Dient zum Erstellen eines geschützten Elements für die Sicherung. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/read | Gibt alle registrierten Container zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/refreshContainers/action | Aktualisiert die Containerliste. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobs/* | Erstellen und Verwalten von Sicherungsaufträgen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobsExport/action | Dient zum Exportieren von Aufträgen. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperationResults/* | Erstellen und Verwalten der Ergebnisse von Sicherungsverwaltungsvorgängen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/operationResults/read | Dient zum Abrufen der Ergebnisse von Richtlinienvorgängen. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectableItems/* | Erstellen und Verwalten von Elementen, die gesichert werden können |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectedItems/read | Gibt die Liste mit allen geschützten Elementen zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionContainers/read | Gibt alle zum Abonnement gehörenden Container zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/certificates/write | Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/write | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/write | Der Vorgang „Dienstcontainer registrieren“ kann zum Registrieren eines Containers beim Wiederherstellungsdienst verwendet werden. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupstorageconfig/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupValidateOperation/action | Überprüft Vorgang für geschütztes Element. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperations/read | Gibt den Status eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/operations/read | Dient zum Abrufen des Status von Richtlinienvorgängen. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/write | Erstellt einen registrierten Container. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/inquire/action | Führt die Abfrage für Workloads innerhalb eines Containers durch. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupEngines/read | Gibt alle Sicherungsverwaltungsserver zurück, die beim Tresor registriert sind. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/backupProtectionIntent/write | Erstellt einen beabsichtigten Sicherungsschutz. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/backupProtectionIntent/read | Ruft einen beabsichtigten Sicherungsschutz ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectableContainers/read | Ruft alle schützbaren Container ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/items/read | Ruft alle Elemente in einem Container ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupStatus/action | Überprüft den Sicherungsstatus für Recovery Services-Tresore. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupPreValidateProtection/action |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupValidateFeatures/action | Überprüft Features. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupAadProperties/read | Ruft AAD-Eigenschaften für die Authentifizierung in der dritten Region für die regionsübergreifende Wiederherstellung ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrJobs/action | Listet Aufträge der regionsübergreifenden Wiederherstellung in der sekundären Region für den Recovery Services-Tresor auf. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrJob/action | Ruft Auftragsdetails der regionsübergreifenden Wiederherstellung in der sekundären Region für den Recovery Services-Tresor ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrossRegionRestore/action | Löst die regionsübergreifende Wiederherstellung aus |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrOperationResults/read | Gibt das Ergebnis eines CRR-Vorgangs für einen Recovery Services-Tresor zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrOperationsStatus/read | Gibt den Status des CRR-Vorgangs für den Recovery Services-Tresor zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/write | Löst die Warnung auf. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/operations/read | Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/operationStatus/read | Ruft den Vorgangsstatus eines angegebenen Vorgangs ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionIntents/read | Listet den gesamten beabsichtigten Sicherungsschutz auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Gibt alle Sicherungsinstanzen zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Gibt alle Sicherungsinstanzen zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Gibt alle Sicherungsrichtlinien zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Gibt alle Sicherungsrichtlinien zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Gibt alle Wiederherstellungspunkte zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Gibt alle Wiederherstellungspunkte zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/findRestorableTimeRanges/action | Sucht nach wiederherstellbaren Zeitbereichen. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Ruft die Liste mit den Sicherungstresoren in einem Abonnement ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/operationResults/read | Ruft das Vorgangsergebnis eines Patchvorgangs für einen Sicherungstresor ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Ruft die Liste mit den Sicherungstresoren in einem Abonnement ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Ruft die Liste mit den Sicherungstresoren in einem Abonnement ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationStatus/read | Gibt den Status eines Sicherungsvorgangs für einen Sicherungstresor zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationResults/read | Gibt das Ergebnis eines Sicherungsvorgangs für einen Sicherungstresor zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/providers/operations/read | Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage backup services, except removal of backup, vault creation and giving access to others",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/00c29273-979b-4161-815c-10b084fb9324",
  "name": "00c29273-979b-4161-815c-10b084fb9324",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action",
        "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/accessToken/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action",
        "Microsoft.RecoveryServices/Vaults/backupJobs/*",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectableItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/certificates/write",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/write",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/write",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupValidateOperation/action",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/locations/backupPreValidateProtection/action",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/locations/backupAadProperties/read",
        "Microsoft.RecoveryServices/locations/backupCrrJobs/action",
        "Microsoft.RecoveryServices/locations/backupCrrJob/action",
        "Microsoft.RecoveryServices/locations/backupCrossRegionRestore/action",
        "Microsoft.RecoveryServices/locations/backupCrrOperationResults/read",
        "Microsoft.RecoveryServices/locations/backupCrrOperationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.Support/*",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/findRestorableTimeRanges/action",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/operationResults/read",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/locations/operationStatus/read",
        "Microsoft.DataProtection/locations/operationResults/read",
        "Microsoft.DataProtection/providers/operations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-reader"></a>Sicherungsleser

Kann Sicherungsdienste anzeigen, aber keine Änderungen vornehmen. [Weitere Informationen](../backup/backup-rbac-rs-vault.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/operationResults/read | Gibt den Status des Vorgangs zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für den Schutzcontainer ausgeführt wurde. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Ruft das Ergebnis eines Vorgangs ab, der für geschützte Elemente ausgeführt wurde. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/read | Gibt Objektdetails des geschützten Elements zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Dient zum Abrufen von Wiederherstellungspunkten für geschützte Elemente. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/read | Gibt alle registrierten Container zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobs/operationResults/read | Gibt das Ergebnis von Auftragsvorgängen zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobs/read | Gibt alle Auftragsobjekte zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobsExport/action | Dient zum Exportieren von Aufträgen. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperationResults/read | Gibt das Ergebnis eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/operationResults/read | Dient zum Abrufen der Ergebnisse von Richtlinienvorgängen. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/read | Gibt alle Schutzrichtlinien zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectedItems/read | Gibt die Liste mit allen geschützten Elementen zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionContainers/read | Gibt alle zum Abonnement gehörenden Container zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupUsageSummaries/read | Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupstorageconfig/read | Gibt die Speicherkonfiguration für Recovery Services-Tresore zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupconfig/read | Gibt die Konfiguration für Recovery Services-Tresore zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperations/read | Gibt den Status eines Sicherungsvorgangs für Recovery Services-Tresore zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/operations/read | Dient zum Abrufen des Status von Richtlinienvorgängen. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupEngines/read | Gibt alle Sicherungsverwaltungsserver zurück, die beim Tresor registriert sind. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/backupProtectionIntent/read | Ruft einen beabsichtigten Sicherungsschutz ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/items/read | Ruft alle Elemente in einem Container ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupStatus/action | Überprüft den Sicherungsstatus für Recovery Services-Tresore. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/write | Löst die Warnung auf. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/operations/read | Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/operationStatus/read | Ruft den Vorgangsstatus eines angegebenen Vorgangs ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionIntents/read | Listet den gesamten beabsichtigten Sicherungsschutz auf. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupValidateFeatures/action | Überprüft Features. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrJobs/action | Listet Aufträge der regionsübergreifenden Wiederherstellung in der sekundären Region für den Recovery Services-Tresor auf. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrJob/action | Ruft Auftragsdetails der regionsübergreifenden Wiederherstellung in der sekundären Region für den Recovery Services-Tresor ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrOperationResults/read | Gibt das Ergebnis eines CRR-Vorgangs für einen Recovery Services-Tresor zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrOperationsStatus/read | Gibt den Status des CRR-Vorgangs für den Recovery Services-Tresor zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/getBackupStatus/action | Überprüft den Sicherungsstatus für Recovery Services-Tresore. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/write | Erstellt eine Sicherungsinstanz. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Gibt alle Sicherungsinstanzen zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Gibt alle Sicherungsinstanzen zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/backup/action | Führt die Sicherung für die Sicherungsinstanz durch. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/validateRestore/action | Führt eine Überprüfung auf eine Wiederherstellung der Sicherungsinstanz durch. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/restore/action | Löst die Wiederherstellung auf der Sicherungsinstanz aus. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Gibt alle Sicherungsrichtlinien zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Gibt alle Sicherungsrichtlinien zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Gibt alle Wiederherstellungspunkte zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Gibt alle Wiederherstellungspunkte zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/findRestorableTimeRanges/action | Sucht nach wiederherstellbaren Zeitbereichen. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Ruft die Liste mit den Sicherungstresoren in einem Abonnement ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/operationResults/read | Ruft das Vorgangsergebnis eines Patchvorgangs für einen Sicherungstresor ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Ruft die Liste mit den Sicherungstresoren in einem Abonnement ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Ruft die Liste mit den Sicherungstresoren in einem Abonnement ab. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationStatus/read | Gibt den Status eines Sicherungsvorgangs für einen Sicherungstresor zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationResults/read | Gibt das Ergebnis eines Sicherungsvorgangs für einen Sicherungstresor zurück. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/validateForBackup/action | Führt eine Überprüfung auf eine Sicherung der Sicherungsinstanz durch. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/providers/operations/read | Der Vorgang gibt die Liste der Vorgänge für einen Ressourcenanbieter zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view backup services, but can't make changes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
  "name": "a795c7a0-d4a2-40c1-ae25-d81f01202912",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupJobs/read",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/read",
        "Microsoft.RecoveryServices/Vaults/backupconfig/read",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/locations/backupCrrJobs/action",
        "Microsoft.RecoveryServices/locations/backupCrrJob/action",
        "Microsoft.RecoveryServices/locations/backupCrrOperationResults/read",
        "Microsoft.RecoveryServices/locations/backupCrrOperationsStatus/read",
        "Microsoft.DataProtection/locations/getBackupStatus/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/write",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/backup/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/validateRestore/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/restore/action",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/findRestorableTimeRanges/action",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/operationResults/read",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/locations/operationStatus/read",
        "Microsoft.DataProtection/locations/operationResults/read",
        "Microsoft.DataProtection/backupVaults/validateForBackup/action",
        "Microsoft.DataProtection/providers/operations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-storage-account-contributor"></a>Mitwirkender von klassischem Speicherkonto

Ermöglicht Ihnen das Verwalten klassischer Speicherkonten, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic storage accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/86e8f5dc-a6e9-4c67-9d15-de283e8eac25",
  "name": "86e8f5dc-a6e9-4c67-9d15-de283e8eac25",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicStorage/storageAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Storage Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-storage-account-key-operator-service-role"></a>Klassische Dienstrolle „Speicherkonto-Schlüsseloperator“

Klassische Speicherkonto-Schlüsseloperatoren dürfen Schlüssel für klassische Speicherkonten auflisten und neu generieren. [Weitere Informationen](../key-vault/secrets/overview-storage-keys.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/listkeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/regeneratekey/action | Generiert die vorhandenen Zugriffsschlüssel für das Speicherkonto neu. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Classic Storage Account Key Operators are allowed to list and regenerate keys on Classic Storage Accounts",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/985d6b00-f706-48f5-a6fe-d0ca12fb668d",
  "name": "985d6b00-f706-48f5-a6fe-d0ca12fb668d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ClassicStorage/storageAccounts/listkeys/action",
        "Microsoft.ClassicStorage/storageAccounts/regeneratekey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Storage Account Key Operator Service Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-box-contributor"></a>Data Box-Mitwirkender

Ermöglicht Ihnen das Verwalten aller Komponenten unter dem Data Box-Dienst, mit Ausnahme der Gewährung des Zugriffs für andere Benutzer. [Weitere Informationen](../databox/data-box-logs.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage everything under Data Box Service except giving access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/add466c9-e687-43fc-8d98-dfcf8d720be5",
  "name": "add466c9-e687-43fc-8d98-dfcf8d720be5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Databox/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Box Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-box-reader"></a>Data Box-Leser

Ermöglicht Ihnen das Verwalten des Data Box-Diensts, mit Ausnahme der Erstellung von Aufträgen oder der Bearbeitung von Auftragsdetails und der Gewährung des Zugriffs für andere Benutzer. [Weitere Informationen](../databox/data-box-logs.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/*/read |  |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/jobs/listsecrets/action |  |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/jobs/listcredentials/action | Hiermit werden die unverschlüsselten Anmeldeinformationen für den Auftrag aufgelistet. |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/locations/availableSkus/action | Mit dieser Methode wird die Liste der verfügbaren SKUs zurückgegeben. |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/locations/validateInputs/action | Diese Methode führt alle Arten von Prüfungen aus. |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/locations/regionConfiguration/action | Diese Methode gibt die Konfigurationen für die Region zurück. |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/locations/validateAddress/action | Hiermit wird die Lieferadresse überprüft, und es werden – sofern vorhanden – alternative Adressen bereitgestellt. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Data Box Service except creating order or editing order details and giving access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027",
  "name": "028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Databox/*/read",
        "Microsoft.Databox/jobs/listsecrets/action",
        "Microsoft.Databox/jobs/listcredentials/action",
        "Microsoft.Databox/locations/availableSkus/action",
        "Microsoft.Databox/locations/validateInputs/action",
        "Microsoft.Databox/locations/regionConfiguration/action",
        "Microsoft.Databox/locations/validateAddress/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Box Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-lake-analytics-developer"></a>Data Lake Analytics-Entwickler

Ermöglicht Ihnen das Übermitteln, Überwachen und Verwalten Ihrer eigenen Aufträge, aber nicht das Erstellen oder Löschen von Data Lake Analytics-Konten. [Weitere Informationen](../data-lake-analytics/data-lake-analytics-manage-use-portal.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.BigAnalytics/accounts/* |  |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/Delete | Löscht ein DataLakeAnalytics-Konto. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/TakeOwnership/action | Erteilt Berechtigungen zum Abbrechen von Aufträgen, die von anderen Benutzern übermittelt wurden. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/Write | Erstellt oder aktualisiert ein DataLakeAnalytics-Konto. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/dataLakeStoreAccounts/Write | Erstellt oder aktualisiert ein verknüpftes DataLakeStore-Konto eines DataLakeAnalytics-Kontos. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/dataLakeStoreAccounts/Delete | Hebt die Verknüpfung eines DataLakeStore-Kontos mit einem DataLakeAnalytics-Konto auf. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/storageAccounts/Write | Erstellt oder aktualisiert ein verknüpftes Speicherkonto eines DataLakeAnalytics-Kontos. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/storageAccounts/Delete | Hebt die Verknüpfung eines Speicherkontos mit einem DataLakeAnalytics-Konto auf. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/firewallRules/Write | Dient zum Erstellen oder Aktualisieren einer Firewallregel. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/firewallRules/Delete | Dient zum Löschen einer Firewallregel. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/computePolicies/Write | Erstellt oder aktualisiert eine Computerichtlinie. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/computePolicies/Delete | Löscht eine Computerichtlinie. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you submit, monitor, and manage your own jobs but not create or delete Data Lake Analytics accounts.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/47b7735b-770e-4598-a7da-8b91488b4c88",
  "name": "47b7735b-770e-4598-a7da-8b91488b4c88",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.BigAnalytics/accounts/*",
        "Microsoft.DataLakeAnalytics/accounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.BigAnalytics/accounts/Delete",
        "Microsoft.BigAnalytics/accounts/TakeOwnership/action",
        "Microsoft.BigAnalytics/accounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action",
        "Microsoft.DataLakeAnalytics/accounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/firewallRules/Write",
        "Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete",
        "Microsoft.DataLakeAnalytics/accounts/computePolicies/Write",
        "Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Lake Analytics Developer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reader-and-data-access"></a>Lese- und Datenzugriff

Ermöglicht Ihnen das Anzeigen sämtlicher Aspekte, jedoch nicht das Löschen oder Erstellen eines Speicherkontos oder einer darin enthaltenen Ressource. Sie können auch Lese-/Schreibzugriff auf alle Daten in einem Speicherkonto durch Zugriff auf Speicherkontoschlüssel gewähren.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/ListAccountSas/action | Gibt das Konto-SAS-Token für das angegebene Speicherkonto zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view everything but will not let you delete or create a storage account or contained resource. It will also allow read/write access to all data contained in a storage account via access to storage account keys.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c12c1c16-33a1-487b-954d-41c89c60f349",
  "name": "c12c1c16-33a1-487b-954d-41c89c60f349",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageAccounts/ListAccountSas/action",
        "Microsoft.Storage/storageAccounts/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reader and Data Access",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-account-contributor"></a>Mitwirkender von Speicherkonto

Erlaubt die Verwaltung von Speicherkonten. Ermöglicht den Zugriff auf den Kontoschlüssel, der für den Datenzugriff über die Autorisierung mit einem gemeinsam verwendetem Schlüssel genutzt werden kann. [Weitere Informationen](../storage/common/storage-auth-aad.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. Nicht warnbar. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage storage accounts, including accessing storage account keys which provide full access to storage account data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/17d1049b-9a84-46fb-8f53-869881c3d3ab",
  "name": "17d1049b-9a84-46fb-8f53-869881c3d3ab",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-account-key-operator-service-role"></a>Dienstrolle „Speicherkonto-Schlüsseloperator“

Ermöglicht das Auflisten und erneute Generieren von Zugriffsschlüsseln für Speicherkonten. [Weitere Informationen](../storage/common/storage-account-keys-manage.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listkeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/regeneratekey/action | Generiert die Zugriffsschlüssel für das angegebene Speicherkonto neu. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Storage Account Key Operators are allowed to list and regenerate keys on Storage Accounts",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/81a9662b-bebf-436f-a333-f67b29880f12",
  "name": "81a9662b-bebf-436f-a333-f67b29880f12",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/regeneratekey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Account Key Operator Service Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-contributor"></a>Mitwirkender an Storage-Blobdaten

Lesen, Schreiben und Löschen von Azure Storage-Containern und -Blobs. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). [Weitere Informationen](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/delete | Löschen eines Containers. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/read | Zurückgeben eines Containers oder einer Liste von Containern. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/write | Ändern der Metadaten oder Eigenschaften eines Containers. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/generateUserDelegationKey/action | Gibt einen Benutzerdelegierungsschlüssel für den Blob-Dienst zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/delete | Löschen eines Blobs |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/read | Zurückgeben eines Blob oder einer Liste von Blobs. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/write | Schreiben in ein Blob. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/move/action | Verschiebt das Blob aus einem Pfad in einen anderen. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/add/action | Gibt das Ergebnis beim Hinzufügen von Blobinhalten zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write and delete access to Azure Storage blob containers and data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ba92f5b4-2d11-453d-a403-e96b0029c9fe",
  "name": "ba92f5b4-2d11-453d-a403-e96b0029c9fe",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/move/action",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/add/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-owner"></a>Besitzer von Speicherblobdaten

Bietet Vollzugriff auf Azure Storage-Blobcontainer und -daten, einschließlich POSIX-Zugriffssteuerung. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). [Weitere Informationen](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/* | Vollzugriffsberechtigungen für Container. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/generateUserDelegationKey/action | Gibt einen Benutzerdelegierungsschlüssel für den Blob-Dienst zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/* | Vollzugriffsberechtigungen für Blobs. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b7e6dc6d-f1e8-4753-8033-0f276bb0955b",
  "name": "b7e6dc6d-f1e8-4753-8033-0f276bb0955b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/*",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-reader"></a>Leser von Speicherblobdaten

Lesen und Auflisten von Azure Storage-Containern und -Blobs. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). [Weitere Informationen](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/read | Zurückgeben eines Containers oder einer Liste von Containern. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/generateUserDelegationKey/action | Gibt einen Benutzerdelegierungsschlüssel für den Blob-Dienst zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/read | Zurückgeben eines Blob oder einer Liste von Blobs. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage blob containers and data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
  "name": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-delegator"></a>Storage Blob-Delegator

Abrufen eines Benutzerdelegierungsschlüssels, mit dem dann eine SAS (Shared Access Signature) für einen Container oder Blob erstellt werden kann, die mit Azure AD-Anmeldeinformationen signiert ist. Weitere Informationen finden Sie unter [Erstellen einer SAS für die Benutzerdelegierung](/rest/api/storageservices/create-user-delegation-sas). [Weitere Informationen](/rest/api/storageservices/get-user-delegation-key)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/generateUserDelegationKey/action | Gibt einen Benutzerdelegierungsschlüssel für den Blob-Dienst zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for generation of a user delegation key which can be used to sign SAS tokens",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/db58b8e5-c6ad-4a2a-8342-4190687cbf4a",
  "name": "db58b8e5-c6ad-4a2a-8342-4190687cbf4a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Delegator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-contributor"></a>Speicherdateidaten-SMB-Freigabemitwirkender

Ermöglicht den Lese-, Schreib- und Löschzugriff auf Dateien/Verzeichnisse in Azure-Dateifreigaben. Für diese Rolle gibt es keine integrierte Entsprechung auf Windows-Dateiservern. [Weitere Informationen](../storage/files/storage-files-identity-auth-active-directory-enable.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/read | Gibt eine Datei oder einen Ordner oder eine Liste mit Dateien/Ordnern zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/write | Gibt das Ergebnis des Schreibens einer Datei oder des Erstellens eines Ordners zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/delete | Gibt das Ergebnis des Löschens einer Datei/eines Ordners zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, and delete access in Azure Storage file shares over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb",
  "name": "0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-elevated-contributor"></a>Speicherdateidaten-SMB-Freigabemitwirkender mit erhöhten Rechten

Ermöglicht das Lesen, Schreiben, Löschen und Bearbeiten von Zugriffssteuerungslisten für Dateien/Verzeichnisse in Azure-Dateifreigaben. Diese Rolle entspricht einer Dateifreigabe-ACL für das Bearbeiten auf Windows-Dateiservern. [Weitere Informationen](../storage/files/storage-files-identity-auth-active-directory-enable.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/read | Gibt eine Datei oder einen Ordner oder eine Liste mit Dateien/Ordnern zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/write | Gibt das Ergebnis des Schreibens einer Datei oder des Erstellens eines Ordners zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/delete | Gibt das Ergebnis des Löschens einer Datei/eines Ordners zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/modifypermissions/action | Gibt das Ergebnis der Berechtigungsänderung für eine Datei/einen Ordner zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, delete and modify NTFS permission access in Azure Storage file shares over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a7264617-510b-434b-a828-9731dc254ea7",
  "name": "a7264617-510b-434b-a828-9731dc254ea7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Elevated Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-reader"></a>Speicherdateidaten-SMB-Freigabeleser

Ermöglicht den Lesezugriff auf Dateien/Verzeichnisse in Azure-Dateifreigaben. Diese Rolle entspricht einer Dateifreigabe-ACL für das Lesen auf Windows-Dateiservern. [Weitere Informationen](../storage/files/storage-files-identity-auth-active-directory-enable.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/read | Gibt eine Datei oder einen Ordner oder eine Liste mit Dateien/Ordnern zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure File Share over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/aba4ae5f-2193-4029-9191-0cb91df5e314",
  "name": "aba4ae5f-2193-4029-9191-0cb91df5e314",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-contributor"></a>Mitwirkender an Storage-Warteschlangendaten

Lesen, Schreiben und Löschen von Azure Storage-Warteschlangen und -Warteschlangennachrichten. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). [Weitere Informationen](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Aktionen | Beschreibung |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/delete | Löschen einer Warteschlange. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/read | Zurückgeben einer Warteschlange oder Liste mit Warteschlangen. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/write | Ändern der Metadaten oder Eigenschaften einer Warteschlange. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/delete | Löschen einer oder mehrerer Nachrichten aus einer Warteschlange. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/read | Einsehen oder Abrufen einer oder mehrerer Nachrichten aus einer Warteschlange. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/write | Hinzufügen von Nachrichten zu einer Warteschlange. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/process/action | Gibt das Ergebnis beim Verarbeiten einer Nachricht zurück. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, and delete access to Azure Storage queues and queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/974c5e8b-45b9-4653-ba55-5f855dd0fb88",
  "name": "974c5e8b-45b9-4653-ba55-5f855dd0fb88",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/delete",
        "Microsoft.Storage/storageAccounts/queueServices/queues/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/write",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-message-processor"></a>Verarbeiter von Speicherwarteschlangen-Datennachrichten

Einsehen, Abrufen und Löschen einer Nachricht aus einer Azure Storage-Warteschlange. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). [Weitere Informationen](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/read | Einsehen einer Nachricht. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/process/action | Abrufen und Löschen einer Nachricht. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for peek, receive, and delete access to Azure Storage queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8a0f0c08-91a1-4084-bc3d-661d67233fed",
  "name": "8a0f0c08-91a1-4084-bc3d-661d67233fed",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Message Processor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-message-sender"></a>Absender der Speicherwarteschlangen-Datennachricht

Hinzufügen von Nachrichten zu einer Azure Storage-Warteschlange. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). [Weitere Informationen](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/add/action | Hinzufügen von Nachrichten zu einer Warteschlange. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for sending of Azure Storage queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c6a89b2d-59bc-44d0-9896-0f6e12d7b80a",
  "name": "c6a89b2d-59bc-44d0-9896-0f6e12d7b80a",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Message Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-reader"></a>Storage-Warteschlangendatenleser

Lesen und Auflisten von Azure Storage-Warteschlangen und -Warteschlangennachrichten. Um zu erfahren, welche Aktionen für einen bestimmten Datenvorgang erforderlich sind, siehe [Berechtigungen für den Aufruf von Datenvorgängen für Blobs und Warteschlangen](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). [Weitere Informationen](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/read | Gibt eine Warteschlange oder eine Liste von Warteschlangen zurück. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/read | Einsehen oder Abrufen einer oder mehrerer Nachrichten aus einer Warteschlange. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage queues and queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/19e7f393-937e-4f77-808e-94535e297925",
  "name": "19e7f393-937e-4f77-808e-94535e297925",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-table-data-contributor"></a>Mitwirkender an Storage-Tabellendaten

Ermöglicht den Lese-, Schreib- und Löschzugriff auf Azure Storage-Tabellen und -Entitäten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/read | Abfragen von Tabellen |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/write | Erstellen von Tabellen |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/delete | Zum Löschen von Tabellen |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/read | Zum Abfragen von Tabellenentitäten |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/write | Zum Einfügen, Zusammenführen oder Ersetzen von Tabellenentitäten |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/delete | Löschen von Tabellenentitäten |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/add/action | Zum Einfügen von Tabellenentitäten |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/update/action | Zum Zusammenführen oder Aktualisieren von Tabellenentitäten |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write and delete access to Azure Storage tables and entities",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0a9a7e1f-b9d0-4cc4-a60d-0319b160aaa3",
  "name": "0a9a7e1f-b9d0-4cc4-a60d-0319b160aaa3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/tableServices/tables/read",
        "Microsoft.Storage/storageAccounts/tableServices/tables/write",
        "Microsoft.Storage/storageAccounts/tableServices/tables/delete"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/read",
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/write",
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/delete",
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/add/action",
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/update/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Table Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-table-data-reader"></a>Storage-Tabellendatenleser

Ermöglicht den Lesezugriff auf Azure Storage-Tabellen und -Entitäten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/read | Abfragen von Tabellen |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/read | Zum Abfragen von Tabellenentitäten |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage tables and entities",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/76199698-9eea-4c19-bc75-cec21354c6b6",
  "name": "76199698-9eea-4c19-bc75-cec21354c6b6",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/tableServices/tables/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Table Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="web"></a>Web


### <a name="azure-maps-data-contributor"></a>Azure Maps-Datenmitwirkender

Gewährt Lese-, Schreib- und Löschzugriff auf kartenbezogene Daten von einem Azure Maps-Konto. [Weitere Informationen](../azure-maps/azure-maps-authentication.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Maps](resource-provider-operations.md#microsoftmaps)/accounts/*/read |  |
> | [Microsoft.Maps](resource-provider-operations.md#microsoftmaps)/accounts/*/write |  |
> | [Microsoft.Maps](resource-provider-operations.md#microsoftmaps)/accounts/*/delete |  |
> | [Microsoft.Maps](resource-provider-operations.md#microsoftmaps)/accounts/*/action |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants access to read, write, and delete access to map related data from an Azure maps account.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8f5e0ce6-4f7b-4dcf-bddf-e6f48634a204",
  "name": "8f5e0ce6-4f7b-4dcf-bddf-e6f48634a204",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Maps/accounts/*/read",
        "Microsoft.Maps/accounts/*/write",
        "Microsoft.Maps/accounts/*/delete",
        "Microsoft.Maps/accounts/*/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Maps Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-maps-data-reader"></a>Azure Maps-Datenleser

Gewährt Lesezugriff auf kartenbezogene Daten von einem Azure Maps-Konto. [Weitere Informationen](../azure-maps/azure-maps-authentication.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Maps](resource-provider-operations.md#microsoftmaps)/accounts/*/read |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants access to read map related data from an Azure maps account.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/423170ca-a8f6-4b0f-8487-9e4eb8f49bfa",
  "name": "423170ca-a8f6-4b0f-8487-9e4eb8f49bfa",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Maps/accounts/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Maps Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-spring-cloud-config-server-contributor"></a>Azure Spring Cloud Config Server-Mitwirkender

Gewährt Lese-, Schreib- und Löschzugriff auf Azure Spring Cloud Config Server ([weitere Informationen](../spring-cloud/how-to-access-data-plane-azure-ad-rbac.md))

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/configService/read | Liest den Konfigurationsinhalt (z. B. „application.yaml“) für eine bestimmte Azure Spring Cloud-Dienstinstanz |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/configService/write | Schreibt den Konfigurationsserverinhalt für eine bestimmte Azure Spring Cloud-Dienstinstanz |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/configService/delete | Löscht den Konfigurationsserverinhalt einer bestimmten Azure Spring Cloud-Dienstinstanz |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allow read, write and delete access to Azure Spring Cloud Config Server",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a06f5c24-21a7-4e1a-aa2b-f19eb6684f5b",
  "name": "a06f5c24-21a7-4e1a-aa2b-f19eb6684f5b",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppPlatform/Spring/configService/read",
        "Microsoft.AppPlatform/Spring/configService/write",
        "Microsoft.AppPlatform/Spring/configService/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Spring Cloud Config Server Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-spring-cloud-config-server-reader"></a>Azure Spring Cloud Config Server-Leser

Gewährt Lesezugriff auf Azure Spring Cloud Config Server ([weitere Informationen](../spring-cloud/how-to-access-data-plane-azure-ad-rbac.md))

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/configService/read | Liest den Konfigurationsinhalt (z. B. „application.yaml“) für eine bestimmte Azure Spring Cloud-Dienstinstanz |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allow read access to Azure Spring Cloud Config Server",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d04c6db6-4947-4782-9e91-30a88feb7be7",
  "name": "d04c6db6-4947-4782-9e91-30a88feb7be7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppPlatform/Spring/configService/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Spring Cloud Config Server Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-spring-cloud-data-reader"></a>Azure Spring Cloud-Datenleser

Gewährt Lesezugriff auf Azure Spring Cloud-Daten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/*/read |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allow read access to Azure Spring Cloud Data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b5537268-8956-4941-a8f0-646150406f0c",
  "name": "b5537268-8956-4941-a8f0-646150406f0c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppPlatform/Spring/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Spring Cloud Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-spring-cloud-service-registry-contributor"></a>Azure Spring Cloud Service Registry-Mitwirkender

Gewährt Lese-, Schreib- und Löschzugriff auf die Azure Spring Cloud Service Registry ([weitere Informationen](../spring-cloud/how-to-access-data-plane-azure-ad-rbac.md))

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/eurekaService/read | Liest die Benutzer-App-Registrierungsinformationen für eine bestimmte Azure Spring Cloud-Dienstinstanz |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/eurekaService/write | Schreibt die Benutzer-App-Registrierungsinformationen für eine bestimmte Azure Spring Cloud-Dienstinstanz |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/eurekaService/delete | Löscht die Benutzer-App-Registrierungsinformationen für eine bestimmte Azure Spring Cloud-Dienstinstanz |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allow read, write and delete access to Azure Spring Cloud Service Registry",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f5880b48-c26d-48be-b172-7927bfa1c8f1",
  "name": "f5880b48-c26d-48be-b172-7927bfa1c8f1",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppPlatform/Spring/eurekaService/read",
        "Microsoft.AppPlatform/Spring/eurekaService/write",
        "Microsoft.AppPlatform/Spring/eurekaService/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Spring Cloud Service Registry Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-spring-cloud-service-registry-reader"></a>Azure Spring Cloud Service Registry-Leser

Gewährt Lesezugriff auf die Azure Spring Cloud Service Registry ([weitere Informationen](../spring-cloud/how-to-access-data-plane-azure-ad-rbac.md))

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/eurekaService/read | Liest die Benutzer-App-Registrierungsinformationen für eine bestimmte Azure Spring Cloud-Dienstinstanz |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allow read access to Azure Spring Cloud Service Registry",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cff1b556-2399-4e7e-856d-a8f754be7b65",
  "name": "cff1b556-2399-4e7e-856d-a8f754be7b65",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppPlatform/Spring/eurekaService/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Spring Cloud Service Registry Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="media-services-account-administrator"></a>Media Services-Kontoadministrator

Erstellen, Lesen, Ändern und Löschen von Media Services Konten; schreibgeschützter Zugriff auf andere Media Services-Ressourcen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Dient zum Lesen von Metriken. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/*/read |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/listStreamingLocators/action | Listet Streaminglocators für ein Medienobjekt auf. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/listPaths/action | Führt Pfade auf. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/write | Erstellt oder aktualisiert ein Media Services-Konto. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/delete | Löscht ein Media Services-Konto. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/privateEndpointConnectionsApproval/action | Bestätigt Verbindungen mit privaten Endpunkten |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/privateEndpointConnections/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, read, modify, and delete Media Services accounts; read-only access to other Media Services resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/054126f8-9a2b-4f1c-a9ad-eca461f08466",
  "name": "054126f8-9a2b-4f1c-a9ad-eca461f08466",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Media/mediaservices/*/read",
        "Microsoft.Media/mediaservices/assets/listStreamingLocators/action",
        "Microsoft.Media/mediaservices/streamingLocators/listPaths/action",
        "Microsoft.Media/mediaservices/write",
        "Microsoft.Media/mediaservices/delete",
        "Microsoft.Media/mediaservices/privateEndpointConnectionsApproval/action",
        "Microsoft.Media/mediaservices/privateEndpointConnections/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Media Services Account Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="media-services-live-events-administrator"></a>Media Services-Administrator für Liveereignisse

Erstellen, Lesen, Ändern und Löschen von Liveereignissen, Medienobjekten, Medienobjektfiltern und Streaminglocators; schreibgeschützter Zugriff auf andere Media Services-Ressourcen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Dient zum Lesen von Metriken. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/*/read |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/assetfilters/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/liveEvents/* |  |
> | **NotActions** |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/getEncryptionKey/action | Ruft Verschlüsselungsschlüssel für ein Objekt ab. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/listContentKeys/action | Führt Inhaltsschlüssel auf. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, read, modify, and delete Live Events, Assets, Asset Filters, and Streaming Locators; read-only access to other Media Services resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/532bc159-b25e-42c0-969e-a1d439f60d77",
  "name": "532bc159-b25e-42c0-969e-a1d439f60d77",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Media/mediaservices/*/read",
        "Microsoft.Media/mediaservices/assets/*",
        "Microsoft.Media/mediaservices/assets/assetfilters/*",
        "Microsoft.Media/mediaservices/streamingLocators/*",
        "Microsoft.Media/mediaservices/liveEvents/*"
      ],
      "notActions": [
        "Microsoft.Media/mediaservices/assets/getEncryptionKey/action",
        "Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Media Services Live Events Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="media-services-media-operator"></a>Media Services-Medienoperator

Erstellen, Lesen, Ändern und Löschen von Medienobjekten, Medienobjektfiltern, Streaminglocators und Aufträgen; schreibgeschützter Zugriff auf andere Media Services-Ressourcen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Dient zum Lesen von Metriken. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/*/read |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/assetfilters/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/transforms/jobs/* |  |
> | **NotActions** |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/getEncryptionKey/action | Ruft Verschlüsselungsschlüssel für ein Objekt ab. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/listContentKeys/action | Führt Inhaltsschlüssel auf. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, read, modify, and delete Assets, Asset Filters, Streaming Locators, and Jobs; read-only access to other Media Services resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e4395492-1534-4db2-bedf-88c14621589c",
  "name": "e4395492-1534-4db2-bedf-88c14621589c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Media/mediaservices/*/read",
        "Microsoft.Media/mediaservices/assets/*",
        "Microsoft.Media/mediaservices/assets/assetfilters/*",
        "Microsoft.Media/mediaservices/streamingLocators/*",
        "Microsoft.Media/mediaservices/transforms/jobs/*"
      ],
      "notActions": [
        "Microsoft.Media/mediaservices/assets/getEncryptionKey/action",
        "Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Media Services Media Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="media-services-policy-administrator"></a>Media Services-Richtlinienadministrator

Erstellen, Lesen, Ändern und Löschen von Kontofiltern, Streamingrichtlinien, Richtlinien für symmetrische Schlüssel und Transformationen; schreibgeschützter Zugriff auf andere Media Services-Ressourcen. Aufträge, Medienobjekte oder Streamingressourcen können nicht erstellt werden.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Dient zum Lesen von Metriken. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/*/read |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/listStreamingLocators/action | Listet Streaminglocators für ein Medienobjekt auf. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/listPaths/action | Führt Pfade auf. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/accountFilters/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingPolicies/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/contentKeyPolicies/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/transforms/* |  |
> | **NotActions** |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Ruft Richtlinieneigenschaften mit geheimen Schlüsseln ab. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, read, modify, and delete Account Filters, Streaming Policies, Content Key Policies, and Transforms; read-only access to other Media Services resources. Cannot create Jobs, Assets or Streaming resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c4bba371-dacd-4a26-b320-7250bca963ae",
  "name": "c4bba371-dacd-4a26-b320-7250bca963ae",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Media/mediaservices/*/read",
        "Microsoft.Media/mediaservices/assets/listStreamingLocators/action",
        "Microsoft.Media/mediaservices/streamingLocators/listPaths/action",
        "Microsoft.Media/mediaservices/accountFilters/*",
        "Microsoft.Media/mediaservices/streamingPolicies/*",
        "Microsoft.Media/mediaservices/contentKeyPolicies/*",
        "Microsoft.Media/mediaservices/transforms/*"
      ],
      "notActions": [
        "Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Media Services Policy Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="media-services-streaming-endpoints-administrator"></a>Media Services-Administrator für Streamingendpunkte

Erstellen, Lesen, Ändern und Löschen von Streamingendpunkten; schreibgeschützter Zugriff auf andere Media Services-Ressourcen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Dient zum Lesen von Metriken. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/*/read |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/listStreamingLocators/action | Listet Streaminglocators für ein Medienobjekt auf. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/listPaths/action | Führt Pfade auf. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingEndpoints/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, read, modify, and delete Streaming Endpoints; read-only access to other Media Services resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/99dba123-b5fe-44d5-874c-ced7199a5804",
  "name": "99dba123-b5fe-44d5-874c-ced7199a5804",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Media/mediaservices/*/read",
        "Microsoft.Media/mediaservices/assets/listStreamingLocators/action",
        "Microsoft.Media/mediaservices/streamingLocators/listPaths/action",
        "Microsoft.Media/mediaservices/streamingEndpoints/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Media Services Streaming Endpoints Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="search-index-data-contributor"></a>Mitwirkender an Suchindexdaten

Gewährt Vollzugriff auf Azure Cognitive Search-Indexdaten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Search](resource-provider-operations.md#microsoftsearch)/searchServices/indexes/documents/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants full access to Azure Cognitive Search index data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8ebe5a00-799e-43f5-93ac-243d3dce84a7",
  "name": "8ebe5a00-799e-43f5-93ac-243d3dce84a7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Search/searchServices/indexes/documents/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Search Index Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="search-index-data-reader"></a>Suchindexdatenleser

Gewährt Lesezugriff auf Azure Cognitive Search-Indexdaten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Search](resource-provider-operations.md#microsoftsearch)/searchServices/indexes/documents/read | Liest Dokumente oder vorgeschlagene Abfragebegriffe aus einem Index. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants read access to Azure Cognitive Search index data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/1407120a-92aa-4202-b7e9-c0e197c71c8f",
  "name": "1407120a-92aa-4202-b7e9-c0e197c71c8f",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Search/searchServices/indexes/documents/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Search Index Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="search-service-contributor"></a>Mitwirkender von Suchdienst

Ermöglicht Ihnen das Verwalten von Search-Diensten, nicht aber den Zugriff darauf. [Weitere Informationen](../search/search-security-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Search](resource-provider-operations.md#microsoftsearch)/searchServices/* | Erstellen und Verwalten von Suchdiensten |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Search services, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7ca78c08-252a-4471-8644-bb5ff32d4ba0",
  "name": "7ca78c08-252a-4471-8644-bb5ff32d4ba0",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Search/searchServices/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Search Service Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalr-accesskey-reader"></a>SignalR-AccessKey-Leser

Ermöglicht das Lesen von SignalR Service-Zugriffsschlüsseln.

> [!div class="mx-tableFixed"]
> | Aktionen | Beschreibung |
> | --- | --- |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/*/read |  |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/listkeys/action | Zeigt den Wert von SignalR-Zugriffsschlüsseln im Verwaltungsportal oder über die API an. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read SignalR Service Access Keys",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/04165923-9d83-45d5-8227-78b77b0a687e",
  "name": "04165923-9d83-45d5-8227-78b77b0a687e",
  "permissions": [
    {
      "actions": [
        "Microsoft.SignalRService/*/read",
        "Microsoft.SignalRService/SignalR/listkeys/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR AccessKey Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalr-app-server-preview"></a>SignalR-App-Server (Vorschau)

Ermöglicht Ihrem App-Server den Zugriff auf SignalR Service mit AAD-Authentifizierungsoptionen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/auth/accessKey/action | Generiert einen Zugriffsschlüssel für das Signieren von Zugriffstoken. Der Schlüsse läuft standardmäßig nach 90 Minuten ab. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/serverConnection/write | Startet eine Serververbindung. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets your app server access SignalR Service with AAD auth options.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/420fcaa2-552c-430f-98ca-3264be4806c7",
  "name": "420fcaa2-552c-430f-98ca-3264be4806c7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.SignalRService/SignalR/auth/accessKey/action",
        "Microsoft.SignalRService/SignalR/serverConnection/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR App Server (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalr-rest-api-owner"></a>SignalR-REST-API-Besitzer

Bietet Vollzugriff auf Azure SignalR Service-REST-APIs.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/auth/clientToken/action | Generiert ein Zugriffstoken für den Client zur Verbindungsherstellung mit ASRS. Das Token läuft standardmäßig nach 5 Minuten ab. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/hub/send/action | Überträgt Nachrichten an alle Clientverbindungen im Hub. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/send/action | Überträgt eine Nachricht an eine Gruppe. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/read | Überprüft die Existenz einer Gruppe oder die Existenz eines Benutzers in einer Gruppe. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/write | Ermöglicht das Beitreten zu einer Gruppe bzw. das Verlassen einer Gruppe. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/send/action | Sendet Nachrichten direkt an eine Clientverbindung. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/read | Überprüft, ob eine Clientverbindung vorhanden ist. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/write | Schließt die Clientverbindung. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/send/action | Sendet Nachrichten an einen Benutzer (umfasst möglicherweise mehrere Clientverbindungen). |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/read | Überprüft, ob Benutzer vorhanden sind. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/write | Dient zum Ändern eines Benutzers. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Full access to Azure SignalR Service REST APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fd53cd77-2268-407a-8f46-7e7863d0f521",
  "name": "fd53cd77-2268-407a-8f46-7e7863d0f521",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.SignalRService/SignalR/auth/clientToken/action",
        "Microsoft.SignalRService/SignalR/hub/send/action",
        "Microsoft.SignalRService/SignalR/group/send/action",
        "Microsoft.SignalRService/SignalR/group/read",
        "Microsoft.SignalRService/SignalR/group/write",
        "Microsoft.SignalRService/SignalR/clientConnection/send/action",
        "Microsoft.SignalRService/SignalR/clientConnection/read",
        "Microsoft.SignalRService/SignalR/clientConnection/write",
        "Microsoft.SignalRService/SignalR/user/send/action",
        "Microsoft.SignalRService/SignalR/user/read",
        "Microsoft.SignalRService/SignalR/user/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR REST API Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalr-rest-api-reader"></a>SignalR-REST-API-Leser

Bietet schreibgeschützten Zugriff auf Azure SignalR Service-REST-APIs.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/read | Überprüft die Existenz einer Gruppe oder die Existenz eines Benutzers in einer Gruppe. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/read | Überprüft, ob eine Clientverbindung vorhanden ist. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/read | Überprüft, ob Benutzer vorhanden sind. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read-only access to Azure SignalR Service REST APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ddde6b66-c0df-4114-a159-3618637b3035",
  "name": "ddde6b66-c0df-4114-a159-3618637b3035",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.SignalRService/SignalR/group/read",
        "Microsoft.SignalRService/SignalR/clientConnection/read",
        "Microsoft.SignalRService/SignalR/user/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR REST API Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalr-service-owner"></a>SignalR Service-Besitzer

Bietet Vollzugriff auf Azure SignalR Service-REST-APIs.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/auth/accessKey/action | Generiert einen Zugriffsschlüssel für das Signieren von Zugriffstoken. Der Schlüsse läuft standardmäßig nach 90 Minuten ab. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/auth/clientToken/action | Generiert ein Zugriffstoken für den Client zur Verbindungsherstellung mit ASRS. Das Token läuft standardmäßig nach 5 Minuten ab. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/hub/send/action | Überträgt Nachrichten an alle Clientverbindungen im Hub. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/send/action | Überträgt eine Nachricht an eine Gruppe. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/read | Überprüft die Existenz einer Gruppe oder die Existenz eines Benutzers in einer Gruppe. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/write | Ermöglicht das Beitreten zu einer Gruppe bzw. das Verlassen einer Gruppe. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/send/action | Sendet Nachrichten direkt an eine Clientverbindung. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/read | Überprüft, ob eine Clientverbindung vorhanden ist. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/write | Schließt die Clientverbindung. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/serverConnection/write | Startet eine Serververbindung. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/send/action | Sendet Nachrichten an einen Benutzer (umfasst möglicherweise mehrere Clientverbindungen). |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/read | Überprüft, ob Benutzer vorhanden sind. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/write | Dient zum Ändern eines Benutzers. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Full access to Azure SignalR Service REST APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7e4f1700-ea5a-4f59-8f37-079cfe29dce3",
  "name": "7e4f1700-ea5a-4f59-8f37-079cfe29dce3",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.SignalRService/SignalR/auth/accessKey/action",
        "Microsoft.SignalRService/SignalR/auth/clientToken/action",
        "Microsoft.SignalRService/SignalR/hub/send/action",
        "Microsoft.SignalRService/SignalR/group/send/action",
        "Microsoft.SignalRService/SignalR/group/read",
        "Microsoft.SignalRService/SignalR/group/write",
        "Microsoft.SignalRService/SignalR/clientConnection/send/action",
        "Microsoft.SignalRService/SignalR/clientConnection/read",
        "Microsoft.SignalRService/SignalR/clientConnection/write",
        "Microsoft.SignalRService/SignalR/serverConnection/write",
        "Microsoft.SignalRService/SignalR/user/send/action",
        "Microsoft.SignalRService/SignalR/user/read",
        "Microsoft.SignalRService/SignalR/user/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR Service Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalrweb-pubsub-contributor"></a>SignalR-/Web PubSub-Mitwirkender

Ermöglicht das Erstellen, Lesen, Aktualisieren und Löschen von SignalR Service-Ressourcen.

> [!div class="mx-tableFixed"]
> | Aktionen | Beschreibung |
> | --- | --- |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/* |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, Read, Update, and Delete SignalR service resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8cf5e20a-e4b2-4e9d-b3a1-5ceb692c2761",
  "name": "8cf5e20a-e4b2-4e9d-b3a1-5ceb692c2761",
  "permissions": [
    {
      "actions": [
        "Microsoft.SignalRService/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR/Web PubSub Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="web-plan-contributor"></a>Mitwirkender von Webplan

Dieser verwaltet die Webpläne für Websites. Ermöglicht es Ihnen nicht, Rollen in Azure RBAC zuzuweisen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/* | Erstellen und Verwalten von Serverfarmen |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/hostingEnvironments/Join/Action | Tritt einer App Service-Umgebung bei. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage the web plans for websites, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b",
  "name": "2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/serverFarms/*",
        "Microsoft.Web/hostingEnvironments/Join/Action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Web Plan Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="website-contributor"></a>Mitwirkender von Website

Verwalten von Websites, aber nicht von Webplänen. Ermöglicht es Ihnen nicht, Rollen in Azure RBAC zuzuweisen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/* | Erstellen und Verwalten von Insights-Komponenten |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/certificates/* | Erstellen und Verwalten von Websitezertifikaten |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/listSitesAssignedToHostName/read | Dient zum Abrufen der Namen von Websites, die dem Hostnamen zugewiesen sind. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/join/action | Hiermit wird der Beitritt zu einem App Service-Plan ausgeführt. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/sites/* | Erstellen und Verwalten von Websites (die Erstellung von Websites erfordert auch Schreibberechtigungen für den zugehörigen App Service-Plan) |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage websites (not web plans), but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/de139f84-1756-47ae-9be6-808fbbe84772",
  "name": "de139f84-1756-47ae-9be6-808fbbe84772",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/components/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/certificates/*",
        "Microsoft.Web/listSitesAssignedToHostName/read",
        "Microsoft.Web/serverFarms/join/action",
        "Microsoft.Web/serverFarms/read",
        "Microsoft.Web/sites/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Website Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="containers"></a>Container


### <a name="acrdelete"></a>AcrDelete

Löschen von Repositorys, Tags oder Manifesten aus einer Containerregistrierung [Weitere Informationen](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/artifacts/delete | Löschen von Artefakten aus einer Containerregistrierung. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr delete",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c2f4ef07-c644-48eb-af81-4b1b4947fb11",
  "name": "c2f4ef07-c644-48eb-af81-4b1b4947fb11",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/artifacts/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrDelete",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrimagesigner"></a>AcrImageSigner

Pushen oder Pullen vertrauenswürdiger Images in einer Containerregistrierung, die für Inhaltsvertrauen aktiviert ist [Weitere Informationen](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/sign/write | Pushen/Pullen von Inhaltsvertrauen-Metadaten für eine Containerregistrierung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/trustedCollections/write | Ermöglicht das Pushen oder Veröffentlichen von vertrauenswürdigen Sammlungen mit Containerregistrierungsinhalten. Dies ähnelt der Aktion „Microsoft.ContainerRegistry/registries/sign/write“, aber es handelt sich um eine Datenaktion. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr image signer",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6cef56e8-d556-48e5-a04f-b8e64114680f",
  "name": "6cef56e8-d556-48e5-a04f-b8e64114680f",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/sign/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerRegistry/registries/trustedCollections/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "AcrImageSigner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrpull"></a>AcrPull

Pullen von Artefakten aus einer Containerregistrierung [Weitere Informationen](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/pull/read | Pullen oder Abrufen von Images aus einer Containerregistrierung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr pull",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7f951dda-4ed3-4680-a7ca-43fe172d538d",
  "name": "7f951dda-4ed3-4680-a7ca-43fe172d538d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/pull/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrPull",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrpush"></a>AcrPush

Pushen oder Pullen von Artefakten in einer Containerregistrierung [Weitere Informationen](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/pull/read | Pullen oder Abrufen von Images aus einer Containerregistrierung |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/push/write | Pushen oder Schreiben von Images in eine Containerregistrierung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr push",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8311e382-0749-4cb8-b61a-304f252e45ec",
  "name": "8311e382-0749-4cb8-b61a-304f252e45ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/pull/read",
        "Microsoft.ContainerRegistry/registries/push/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrPush",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrquarantinereader"></a>AcrQuarantineReader

Pullen von Images in Quarantäne aus einer Containerregistrierung [Weitere Informationen](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantine/read | Pullen oder Abrufen von Images in Quarantäne aus einer Containerregistrierung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantinedArtifacts/read | Ermöglicht das Pullen oder Abrufen der unter Quarantäne gestellten Artefakte aus der Containerregistrierung. Dies ähnelt „Microsoft.ContainerRegistry/registries/quarantine/read“, aber es handelt sich um eine Datenaktion. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr quarantine data reader",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cdda3590-29a3-44f6-95f2-9f980659eb04",
  "name": "cdda3590-29a3-44f6-95f2-9f980659eb04",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/quarantine/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerRegistry/registries/quarantinedArtifacts/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "AcrQuarantineReader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrquarantinewriter"></a>AcrQuarantineWriter

Pushen oder Pullen von Images in Quarantäne in einer Containerregistrierung [Weitere Informationen](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantine/read | Pullen oder Abrufen von Images in Quarantäne aus einer Containerregistrierung |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantine/write | Schreiben/Ändern des Quarantänezustands von unter Quarantäne gestellten Images |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantinedArtifacts/read | Ermöglicht das Pullen oder Abrufen der unter Quarantäne gestellten Artefakte aus der Containerregistrierung. Dies ähnelt „Microsoft.ContainerRegistry/registries/quarantine/read“, aber es handelt sich um eine Datenaktion. |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantinedArtifacts/write | Ermöglicht das Schreiben oder Aktualisieren des Quarantänezustands von unter Quarantäne gestellten Artefakten. Dies ähnelt der Aktion „Microsoft.ContainerRegistry/registries/quarantine/write“, aber es handelt sich um eine Datenaktion. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr quarantine data writer",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c8d4ff99-41c3-41a8-9f60-21dfdad59608",
  "name": "c8d4ff99-41c3-41a8-9f60-21dfdad59608",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/quarantine/read",
        "Microsoft.ContainerRegistry/registries/quarantine/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerRegistry/registries/quarantinedArtifacts/read",
        "Microsoft.ContainerRegistry/registries/quarantinedArtifacts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "AcrQuarantineWriter",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-cluster-admin-role"></a>Administratorrolle für Azure Kubernetes Service-Cluster

Listet die Aktion für Anmeldeinformationen des Clusteradministrators auf. [Weitere Informationen](../aks/control-kubeconfig-access.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/listClusterAdminCredential/action | Listet die clusterAdmin-Anmeldeinformationen eines verwalteten Clusters auf. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/accessProfiles/listCredential/action | Ruft ein Zugriffsprofil für verwaltete Cluster anhand des Rollennamens mithilfe der Liste der Anmeldeinformationen ab. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/read | Ruft einen verwalteten Cluster ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster admin credential action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8",
  "name": "0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action",
        "Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action",
        "Microsoft.ContainerService/managedClusters/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Cluster Admin Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-cluster-user-role"></a>Benutzerrolle für Azure Kubernetes Service-Cluster

Listet die Aktion für Anmeldeinformationen des Clusterbenutzer auf. [Weitere Informationen](../aks/control-kubeconfig-access.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/listClusterUserCredential/action | Listet die clusterUser-Anmeldeinformationen eines verwalteten Clusters auf. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/read | Ruft einen verwalteten Cluster ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster user credential action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4abbcc35-e782-43d8-92c5-2d3f1bd2253f",
  "name": "4abbcc35-e782-43d8-92c5-2d3f1bd2253f",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/listClusterUserCredential/action",
        "Microsoft.ContainerService/managedClusters/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Cluster User Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-contributor-role"></a>Rolle „Mitwirkender“ für Azure Kubernetes Service

Gewährt Lese- und Schreibzugriff auf Azure Kubernetes Service-Cluster [Weitere Informationen](../aks/concepts-identity.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/read | Ruft einen verwalteten Cluster ab. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/write | Erstellt einen neuen verwalteten Cluster oder aktualisiert einen vorhandenen. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants access to read and write Azure Kubernetes Service clusters",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ed7f3fbd-7b88-4dd4-9017-9adb7ce333f8",
  "name": "ed7f3fbd-7b88-4dd4-9017-9adb7ce333f8",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/read",
        "Microsoft.ContainerService/managedClusters/write",
        "Microsoft.Resources/deployments/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Contributor Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-rbac-admin"></a>RBAC-Administrator von Azure Kubernetes Service

Ermöglicht Ihnen das Verwalten aller Ressourcen unter einem Cluster/Namespace, außer das Aktualisieren oder Löschen von Ressourcenkontingenten und Namespaces. [Weitere Informationen](../aks/manage-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/listClusterUserCredential/action | Listet die clusterUser-Anmeldeinformationen eines verwalteten Clusters auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/* |  |
> | **NotDataActions** |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/resourcequotas/write | Schreibt resourcequotas. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/resourcequotas/delete | Löscht resourcequotas. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/namespaces/write | Schreibt namespaces. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/namespaces/delete | Löscht namespaces. |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage all resources under cluster/namespace, except update or delete resource quotas and namespaces.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3498e952-d568-435e-9b2c-8d77e338d7f7",
  "name": "3498e952-d568-435e-9b2c-8d77e338d7f7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.ContainerService/managedClusters/listClusterUserCredential/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerService/managedClusters/*"
      ],
      "notDataActions": [
        "Microsoft.ContainerService/managedClusters/resourcequotas/write",
        "Microsoft.ContainerService/managedClusters/resourcequotas/delete",
        "Microsoft.ContainerService/managedClusters/namespaces/write",
        "Microsoft.ContainerService/managedClusters/namespaces/delete"
      ]
    }
  ],
  "roleName": "Azure Kubernetes Service RBAC Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-rbac-cluster-admin"></a>RBAC-Clusteradministrator von Azure Kubernetes Service

Ermöglicht Ihnen das Verwalten aller Ressourcen im Cluster. [Weitere Informationen](../aks/manage-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/listClusterUserCredential/action | Listet die clusterUser-Anmeldeinformationen eines verwalteten Clusters auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage all resources in the cluster.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b1ff04bb-8a4e-4dc4-8eb5-8693973ce19b",
  "name": "b1ff04bb-8a4e-4dc4-8eb5-8693973ce19b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.ContainerService/managedClusters/listClusterUserCredential/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerService/managedClusters/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service RBAC Cluster Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-rbac-reader"></a>RBAC-Leser von Azure Kubernetes Service

Ermöglicht schreibgeschützten Zugriff, um die meisten Objekte in einem Namespace anzuzeigen. Es ist nicht möglich, Rollen oder Rollenbindungen anzuzeigen. Diese Rolle lässt das Anzeigen von Geheimnissen nicht zu, da das Lesen des Inhalts von Geheimnissen den Zugriff auf ServiceAccount-Anmeldeinformationen im Namespace ermöglicht, was den API-Zugriff als beliebiges Dienstkonto im Namespace ermöglichen würde (eine Form von Berechtigungsausweitung). Wenn Sie diese Rolle im Clusterumfang anwenden, wird der Zugriff auf alle Namespaces ermöglicht. [Weitere Informationen](../aks/manage-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/controllerrevisions/read | Liest controllerrevisions. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/daemonsets/read | Liest daemonsets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/deployments/read | Liest deployments. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/replicasets/read | Liest replicasets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/statefulsets/read | Liest statefulsets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/autoscaling/horizontalpodautoscalers/read | Liest horizontalpodautoscalers. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/batch/cronjobs/read | Liest cronjobs. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/batch/jobs/read | Liest jobs. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/configmaps/read | Liest configmaps. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/endpoints/read | Liest endpoints. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/events.k8s.io/events/read | Liest events. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/events/read | Liest events. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/daemonsets/read | Liest daemonsets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/deployments/read | Liest deployments. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/ingresses/read | Liest ingresses. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/networkpolicies/read | Liest networkpolicies. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/replicasets/read | Liest replicasets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/limitranges/read | Liest limitranges. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/namespaces/read | Liest namespaces. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/networking.k8s.io/ingresses/read | Liest ingresses. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/networking.k8s.io/networkpolicies/read | Liest networkpolicies. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/persistentvolumeclaims/read | Liest persistentvolumeclaims. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/pods/read | Liest pods. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/policy/poddisruptionbudgets/read | Liest poddisruptionbudgets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/replicationcontrollers/read | Liest replicationcontrollers. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/replicationcontrollers/read | Liest replicationcontrollers. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/resourcequotas/read | Liest resourcequotas. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/serviceaccounts/read | Liest serviceaccounts. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/services/read | Liest services. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read-only access to see most objects in a namespace. It does not allow viewing roles or role bindings. This role does not allow viewing Secrets, since reading the contents of Secrets enables access to ServiceAccount credentials in the namespace, which would allow API access as any ServiceAccount in the namespace (a form of privilege escalation). Applying this role at cluster scope will give access across all namespaces.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7f6c6a51-bcf8-42ba-9220-52d62157d7db",
  "name": "7f6c6a51-bcf8-42ba-9220-52d62157d7db",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerService/managedClusters/apps/controllerrevisions/read",
        "Microsoft.ContainerService/managedClusters/apps/daemonsets/read",
        "Microsoft.ContainerService/managedClusters/apps/deployments/read",
        "Microsoft.ContainerService/managedClusters/apps/replicasets/read",
        "Microsoft.ContainerService/managedClusters/apps/statefulsets/read",
        "Microsoft.ContainerService/managedClusters/autoscaling/horizontalpodautoscalers/read",
        "Microsoft.ContainerService/managedClusters/batch/cronjobs/read",
        "Microsoft.ContainerService/managedClusters/batch/jobs/read",
        "Microsoft.ContainerService/managedClusters/configmaps/read",
        "Microsoft.ContainerService/managedClusters/endpoints/read",
        "Microsoft.ContainerService/managedClusters/events.k8s.io/events/read",
        "Microsoft.ContainerService/managedClusters/events/read",
        "Microsoft.ContainerService/managedClusters/extensions/daemonsets/read",
        "Microsoft.ContainerService/managedClusters/extensions/deployments/read",
        "Microsoft.ContainerService/managedClusters/extensions/ingresses/read",
        "Microsoft.ContainerService/managedClusters/extensions/networkpolicies/read",
        "Microsoft.ContainerService/managedClusters/extensions/replicasets/read",
        "Microsoft.ContainerService/managedClusters/limitranges/read",
        "Microsoft.ContainerService/managedClusters/namespaces/read",
        "Microsoft.ContainerService/managedClusters/networking.k8s.io/ingresses/read",
        "Microsoft.ContainerService/managedClusters/networking.k8s.io/networkpolicies/read",
        "Microsoft.ContainerService/managedClusters/persistentvolumeclaims/read",
        "Microsoft.ContainerService/managedClusters/pods/read",
        "Microsoft.ContainerService/managedClusters/policy/poddisruptionbudgets/read",
        "Microsoft.ContainerService/managedClusters/replicationcontrollers/read",
        "Microsoft.ContainerService/managedClusters/replicationcontrollers/read",
        "Microsoft.ContainerService/managedClusters/resourcequotas/read",
        "Microsoft.ContainerService/managedClusters/serviceaccounts/read",
        "Microsoft.ContainerService/managedClusters/services/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service RBAC Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-rbac-writer"></a>RBAC-Writer von Azure Kubernetes Service

Ermöglicht den Lese-/Schreibzugriff auf die meisten Objekte in einem Namespace. Diese Rolle gestattet nicht das Anzeigen oder Ändern von Rollen oder Rollenbindungen. Diese Rolle ermöglicht jedoch den Zugriff auf Geheimnisse und das Ausführen von Pods als beliebiges Dienstkonto im Namespace, sodass sie verwendet werden kann, um die API-Zugriffsebenen eines beliebigen ServiceAccount im Namespace zu erhalten. Wenn Sie diese Rolle im Clusterumfang anwenden, wird der Zugriff auf alle Namespaces ermöglicht. [Weitere Informationen](../aks/manage-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/controllerrevisions/read | Liest controllerrevisions. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/daemonsets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/deployments/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/replicasets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/statefulsets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/autoscaling/horizontalpodautoscalers/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/batch/cronjobs/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/batch/jobs/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/configmaps/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/endpoints/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/events.k8s.io/events/read | Liest events. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/events/read | Liest events. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/daemonsets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/deployments/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/ingresses/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/networkpolicies/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/replicasets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/limitranges/read | Liest limitranges. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/namespaces/read | Liest namespaces. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/networking.k8s.io/ingresses/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/networking.k8s.io/networkpolicies/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/persistentvolumeclaims/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/pods/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/policy/poddisruptionbudgets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/replicationcontrollers/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/replicationcontrollers/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/resourcequotas/read | Liest resourcequotas. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/secrets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/serviceaccounts/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/services/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read/write access to most objects in a namespace.This role does not allow viewing or modifying roles or role bindings. However, this role allows accessing Secrets and running Pods as any ServiceAccount in the namespace, so it can be used to gain the API access levels of any ServiceAccount in the namespace. Applying this role at cluster scope will give access across all namespaces.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a7ffa36f-339b-4b5c-8bdf-e2c188b2c0eb",
  "name": "a7ffa36f-339b-4b5c-8bdf-e2c188b2c0eb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerService/managedClusters/apps/controllerrevisions/read",
        "Microsoft.ContainerService/managedClusters/apps/daemonsets/*",
        "Microsoft.ContainerService/managedClusters/apps/deployments/*",
        "Microsoft.ContainerService/managedClusters/apps/replicasets/*",
        "Microsoft.ContainerService/managedClusters/apps/statefulsets/*",
        "Microsoft.ContainerService/managedClusters/autoscaling/horizontalpodautoscalers/*",
        "Microsoft.ContainerService/managedClusters/batch/cronjobs/*",
        "Microsoft.ContainerService/managedClusters/batch/jobs/*",
        "Microsoft.ContainerService/managedClusters/configmaps/*",
        "Microsoft.ContainerService/managedClusters/endpoints/*",
        "Microsoft.ContainerService/managedClusters/events.k8s.io/events/read",
        "Microsoft.ContainerService/managedClusters/events/read",
        "Microsoft.ContainerService/managedClusters/extensions/daemonsets/*",
        "Microsoft.ContainerService/managedClusters/extensions/deployments/*",
        "Microsoft.ContainerService/managedClusters/extensions/ingresses/*",
        "Microsoft.ContainerService/managedClusters/extensions/networkpolicies/*",
        "Microsoft.ContainerService/managedClusters/extensions/replicasets/*",
        "Microsoft.ContainerService/managedClusters/limitranges/read",
        "Microsoft.ContainerService/managedClusters/namespaces/read",
        "Microsoft.ContainerService/managedClusters/networking.k8s.io/ingresses/*",
        "Microsoft.ContainerService/managedClusters/networking.k8s.io/networkpolicies/*",
        "Microsoft.ContainerService/managedClusters/persistentvolumeclaims/*",
        "Microsoft.ContainerService/managedClusters/pods/*",
        "Microsoft.ContainerService/managedClusters/policy/poddisruptionbudgets/*",
        "Microsoft.ContainerService/managedClusters/replicationcontrollers/*",
        "Microsoft.ContainerService/managedClusters/replicationcontrollers/*",
        "Microsoft.ContainerService/managedClusters/resourcequotas/read",
        "Microsoft.ContainerService/managedClusters/secrets/*",
        "Microsoft.ContainerService/managedClusters/serviceaccounts/*",
        "Microsoft.ContainerService/managedClusters/services/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service RBAC Writer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="databases"></a>Datenbanken


### <a name="azure-connected-sql-server-onboarding"></a>Azure Connected SQL Server-Onboarding

Ermöglicht Lese- und Schreibzugriff auf Azure-Ressourcen für SQL Server auf Arc-fähigen Servern. [Weitere Informationen](/sql/sql-server/azure-arc/connect)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | Microsoft.AzureArcData/sqlServerInstances/read | Ruft eine Ressource der SQL Server-Instanz ab |
> | Microsoft.AzureArcData/sqlServerInstances/write | Aktualisiert eine Ressource der SQL Server-Instanz |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Microsoft.AzureArcData service role to access the resources of Microsoft.AzureArcData stored with RPSAAS.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e8113dce-c529-4d33-91fa-e9b972617508",
  "name": "e8113dce-c529-4d33-91fa-e9b972617508",
  "permissions": [
    {
      "actions": [
        "Microsoft.AzureArcData/sqlServerInstances/read",
        "Microsoft.AzureArcData/sqlServerInstances/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected SQL Server Onboarding",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmos-db-account-reader-role"></a>Cosmos DB-Rolle „Kontoleser“

Kann Azure Cosmos DB-Kontodaten lesen. Informationen zum Verwalten von Azure Cosmos DB-Konten finden Sie unter [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor). [Weitere Informationen](../cosmos-db/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/*/read | Lesen einer beliebigen Sammlung |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/readonlykeys/action | Liest die schreibgeschützten Schlüssel für Datenbankkonten. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/MetricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/Metrics/read | Dient zum Lesen von Metriken. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read Azure Cosmos DB Accounts data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
  "name": "fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DocumentDB/*/read",
        "Microsoft.DocumentDB/databaseAccounts/readonlykeys/action",
        "Microsoft.Insights/MetricDefinitions/read",
        "Microsoft.Insights/Metrics/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cosmos DB Account Reader Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmos-db-operator"></a>Cosmos DB-Operator

Ermöglicht das Verwalten von Azure Cosmos DB-Konten, aber nicht das Zugreifen auf deren Daten. Verhindert den Zugriff auf Kontoschlüssel und Verbindungszeichenfolgen. [Weitere Informationen](../cosmos-db/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DocumentDb](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. Nicht warnbar. |
> | **NotActions** |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/readonlyKeys/* |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/regenerateKey/* |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/listKeys/* |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/listConnectionStrings/* |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/sqlRoleDefinitions/write | Erstellen oder Aktualisieren einer SQL-Rollendefinition |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/sqlRoleDefinitions/delete | Löschen einer SQL-Rollendefinition |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/sqlRoleAssignments/write | Erstellen oder Aktualisieren einer SQL-Rollenzuweisung |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/sqlRoleAssignments/delete | Löschen einer SQL-Rollenzuweisung |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Azure Cosmos DB accounts, but not access data in them. Prevents access to account keys and connection strings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/230815da-be43-4aae-9cb4-875f7bd000aa",
  "name": "230815da-be43-4aae-9cb4-875f7bd000aa",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDb/databaseAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action"
      ],
      "notActions": [
        "Microsoft.DocumentDB/databaseAccounts/readonlyKeys/*",
        "Microsoft.DocumentDB/databaseAccounts/regenerateKey/*",
        "Microsoft.DocumentDB/databaseAccounts/listKeys/*",
        "Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/*",
        "Microsoft.DocumentDB/databaseAccounts/sqlRoleDefinitions/write",
        "Microsoft.DocumentDB/databaseAccounts/sqlRoleDefinitions/delete",
        "Microsoft.DocumentDB/databaseAccounts/sqlRoleAssignments/write",
        "Microsoft.DocumentDB/databaseAccounts/sqlRoleAssignments/delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cosmos DB Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmosbackupoperator"></a>CosmosBackupOperator

Kann eine Wiederherstellungsanforderung für eine Cosmos DB-Datenbank oder einen Container für ein Konto übermitteln. [Weitere Informationen](../cosmos-db/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/backup/action | Sendet eine Anforderung zum Konfigurieren der Sicherung. |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/restore/action | Sendet eine Wiederherstellungsanforderung. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can submit restore request for a Cosmos DB database or a container for an account",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/db7b14f2-5adf-42da-9f96-f2ee17bab5cb",
  "name": "db7b14f2-5adf-42da-9f96-f2ee17bab5cb",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDB/databaseAccounts/backup/action",
        "Microsoft.DocumentDB/databaseAccounts/restore/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CosmosBackupOperator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmosrestoreoperator"></a>CosmosRestoreOperator

Kann eine Wiederherstellungsaktion für ein Cosmos DB-Datenbankkonto im fortlaufendem Sicherungsmodus ausführen

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/locations/restorableDatabaseAccounts/restore/action | Sendet eine Wiederherstellungsanforderung. |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/locations/restorableDatabaseAccounts/*/read |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/locations/restorableDatabaseAccounts/read | Liest ein wiederherstellbares Datenbankkonto oder listet alle wiederherstellbaren Datenbankkonten auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can perform restore action for Cosmos DB database account with continuous backup mode",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5432c526-bc82-444a-b7ba-57c5b0b5b34f",
  "name": "5432c526-bc82-444a-b7ba-57c5b0b5b34f",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restore/action",
        "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read",
        "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CosmosRestoreOperator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="documentdb-account-contributor"></a>Mitwirkender von DocumentDB-Konto

Kann Azure Cosmos DB-Konten verwalten. Azure Cosmos DB wurde früher als DocumentDB bezeichnet. [Weitere Informationen](../cosmos-db/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.DocumentDb](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/* | Kann Azure Cosmos DB-Konten erstellen und verwalten |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. Nicht warnbar. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage DocumentDB accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5bd9cd88-fe45-4216-938b-f97437e15450",
  "name": "5bd9cd88-fe45-4216-938b-f97437e15450",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DocumentDb/databaseAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DocumentDB Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="redis-cache-contributor"></a>Mitwirkender von Redis-Cache

Ermöglicht Ihnen das Verwalten von Redis Caches, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Cache](resource-provider-operations.md#microsoftcache)/register/action | Registriert den Ressourcenanbieter „Microsoft.Cache“ bei einem Abonnement. |
> | [Microsoft.Cache](resource-provider-operations.md#microsoftcache)/redis/* | Erstellen und Verwalten von Redis-Caches |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Redis caches, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e0f68234-74aa-48ed-b826-c38b57376e17",
  "name": "e0f68234-74aa-48ed-b826-c38b57376e17",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cache/register/action",
        "Microsoft.Cache/redis/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Redis Cache Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-db-contributor"></a>Mitwirkender von SQL DB

Ermöglicht Ihnen das Verwalten von SQL-Datenbanken, nicht aber den Zugriff darauf. Darüber hinaus können Sie deren sicherheitsbezogenen Richtlinien oder übergeordneten SQL-Server nicht verwalten. [Weitere Informationen](../data-share/concepts-roles-permissions.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/locations/*/read |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/* | Erstellen und Verwalten von SQL-Datenbanken |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Dient zum Lesen von Metriken. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | **NotActions** |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/ledgerDigestUploads/write | Zum Aktivieren des Hochladens von Ledgerdigests  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/ledgerDigestUploads/disable/action | Zum Deaktivieren des Hochladens von Ledgerdigests |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditingSettings/* | Überwachungseinstellungen bearbeiten |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditRecords/read | Dient zum Abrufen der Datensätze für die Datenbankblobüberwachung. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/dataMaskingPolicies/* | Datenmaskierungsrichtlinien bearbeiten |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/extendedAuditingSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen bearbeiten |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityMetrics/* | Sicherheitsmetriken bearbeiten |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentScans/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL databases, but not access to them. Also, you can't manage their security-related policies or their parent SQL servers.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9b7fa17d-e63e-47b0-bb0a-15c516ac86ec",
  "name": "9b7fa17d-e63e-47b0-bb0a-15c516ac86ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/servers/databases/*",
        "Microsoft.Sql/servers/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/servers/databases/ledgerDigestUploads/write",
        "Microsoft.Sql/servers/databases/ledgerDigestUploads/disable/action",
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL DB Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-managed-instance-contributor"></a>Verwaltete SQL-Instanz: Mitwirkender

Diese Rolle ermöglicht Ihnen das Verwalten verwalteter SQL-Instanzen und der erforderlichen Netzwerkkonfiguration, jedoch nicht das Erteilen des Zugriffs an andere.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkSecurityGroups/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/routeTables/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/locations/*/read |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/locations/instanceFailoverGroups/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/* |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/* |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Dient zum Lesen von Metriken. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | **NotActions** |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/azureADOnlyAuthentications/delete | Löscht ein spezifisches verwaltetes Serverobjekt für die ausschließliche Azure Active Directory-Authentifizierung. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/azureADOnlyAuthentications/write | Fügt ein spezifisches verwaltetes Serverobjekt für die ausschließliche Azure Active Directory-Authentifizierung hinzu oder aktualisiert es. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL Managed Instances and required network configuration, but can't give access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4939a1f6-9ae0-4e48-a1e0-f2cbe897382d",
  "name": "4939a1f6-9ae0-4e48-a1e0-f2cbe897382d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Network/networkSecurityGroups/*",
        "Microsoft.Network/routeTables/*",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/locations/instanceFailoverGroups/*",
        "Microsoft.Sql/managedInstances/*",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/*",
        "Microsoft.Network/virtualNetworks/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/managedInstances/azureADOnlyAuthentications/delete",
        "Microsoft.Sql/managedInstances/azureADOnlyAuthentications/write"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Managed Instance Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-security-manager"></a>SQL-Sicherheits-Manager

Ermöglicht Ihnen das Verwalten von sicherheitsbezogenen Richtlinien von SQL-Server und Datenbanken, jedoch nicht den Zugriff darauf. [Weitere Informationen](../azure-sql/database/azure-defender-for-sql.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/joinViaServiceEndpoint/action | Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Subnetz. Nicht warnbar. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/locations/administratorAzureAsyncOperation/read | Ruft das Ergebnis asynchroner Azure-Administratorvorgänge für die verwaltete Instanz ab. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/transparentDataEncryption/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/auditingSettings/* | Erstellen und Verwalten von SQL Server-Überwachungseinstellungen |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/extendedAuditingSettings/read | Dient zum Abrufen von Details zur Richtlinie für die erweiterte Serverblobüberwachung, die für einen bestimmten Server konfiguriert ist. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditingSettings/* | Erstellen und Verwalten von Überwachungseinstellungen von SQL Server-Datenbanken |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditRecords/read | Dient zum Abrufen der Datensätze für die Datenbankblobüberwachung. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/dataMaskingPolicies/* | Erstellen und Verwalten von Datenmaskierungsrichtlinien von SQL Server-Datenbanken |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/extendedAuditingSettings/read | Dient zum Abrufen von Details zur erweiterten Blobüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/read | Gibt die Liste der Datenbanken zurück oder ruft die Eigenschaften für die angegebene Datenbank ab. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/read | Ruft ein Datenbankschema ab. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/tables/columns/read | Ruft eine Datenbankspalte ab. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/tables/read | Abrufen einer Datentabelle. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityAlertPolicies/* | Erstellen und Verwalten von Richtlinien für Sicherheitswarnungen von SQL Server-Datenbanken |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityMetrics/* | Erstellen und Verwalten von Sicherheitsmetriken von SQL Server-Datenbanken |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/transparentDataEncryption/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentScans/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/devOpsAuditingSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/firewallRules/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/read | Gibt die Liste der Server zurück oder ruft die Eigenschaften für den angegebenen Server ab. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/securityAlertPolicies/* | Erstellen und Verwalten von Richtlinien für Sicherheitswarnungen von SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/vulnerabilityAssessments/* |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/azureADOnlyAuthentications/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/read | Gibt die Liste der verwalteten Instanzen zurück oder ruft die Eigenschaften für die angegebene verwaltete Instanz ab. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/azureADOnlyAuthentications/* |  |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/sqlVulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/administrators/read | Ruft eine Liste der Administratoren für verwaltete Instanzen ab. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/administrators/read | Ruft ein bestimmtes Azure Active Directory-Administratorobjekt ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage the security-related policies of SQL servers and databases, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/056cd41c-7e88-42e1-933e-88ba6a50c9c3",
  "name": "056cd41c-7e88-42e1-933e-88ba6a50c9c3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/administratorAzureAsyncOperation/read",
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/transparentDataEncryption/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/auditingSettings/*",
        "Microsoft.Sql/servers/extendedAuditingSettings/read",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/read",
        "Microsoft.Sql/servers/databases/read",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/read",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/read",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/read",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/transparentDataEncryption/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/devOpsAuditingSettings/*",
        "Microsoft.Sql/servers/firewallRules/*",
        "Microsoft.Sql/servers/read",
        "Microsoft.Sql/servers/securityAlertPolicies/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*",
        "Microsoft.Support/*",
        "Microsoft.Sql/servers/azureADOnlyAuthentications/*",
        "Microsoft.Sql/managedInstances/read",
        "Microsoft.Sql/managedInstances/azureADOnlyAuthentications/*",
        "Microsoft.Security/sqlVulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/administrators/read",
        "Microsoft.Sql/servers/administrators/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Security Manager",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-server-contributor"></a>Mitwirkender von SQL Server

Diese Rolle ermöglicht es Ihnen, SQL-Server und -Datenbanken zu verwalten, gewährt Ihnen jedoch keinen Zugriff darauf und auch nicht auf deren sicherheitsbezogenen Richtlinien. [Weitere Informationen](../azure-sql/database/authentication-aad-configure.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/locations/*/read |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/* | Erstellen und Verwalten von SQL-Servern |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Dient zum Lesen von Metriken. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | **NotActions** |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/auditingSettings/* | SQL Server-Überwachungseinstellungen bearbeiten |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditingSettings/* | Überwachungseinstellungen von SQL Server-Datenbanken bearbeiten |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditRecords/read | Dient zum Abrufen der Datensätze für die Datenbankblobüberwachung. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/dataMaskingPolicies/* | Datenmaskierungsrichtlinien von SQL Server-Datenbanken bearbeiten |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/extendedAuditingSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen von SQL Server-Datenbanken bearbeiten |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityMetrics/* | Sicherheitsmetriken von SQL Server-Datenbanken bearbeiten |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentScans/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/devOpsAuditingSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/extendedAuditingSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen von SQL Server bearbeiten |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/azureADOnlyAuthentications/delete | Löscht ein spezifisches Objekt für die ausschließliche Azure Active Directory-Authentifizierung. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/azureADOnlyAuthentications/write | Fügt ein spezifisches Objekt für die ausschließliche Azure Active Directory-Authentifizierung hinzu oder aktualisiert es. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL servers and databases, but not access to them, and not their security -related policies.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437",
  "name": "6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/servers/*",
        "Microsoft.Support/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/devOpsAuditingSettings/*",
        "Microsoft.Sql/servers/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/securityAlertPolicies/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/azureADOnlyAuthentications/delete",
        "Microsoft.Sql/servers/azureADOnlyAuthentications/write"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Server Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="analytics"></a>Analytics


### <a name="azure-event-hubs-data-owner"></a>Azure Event Hubs-Datenbesitzer

Ermöglicht den uneingeschränkten Zugriff auf die Azure Event Hubs-Ressourcen. [Weitere Informationen](../event-hubs/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f526a384-b230-433a-b45c-95f59c4a2dec",
  "name": "f526a384-b230-433a-b45c-95f59c4a2dec",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-event-hubs-data-receiver"></a>Azure Event Hubs-Datenempfänger

Ermöglicht Empfängern den Zugriff auf die Azure Event Hubs-Ressourcen. [Weitere Informationen](../event-hubs/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/*/eventhubs/consumergroups/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/*/receive/action |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows receive access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a638d3c7-ab3a-418d-83e6-5f17a39d4fde",
  "name": "a638d3c7-ab3a-418d-83e6-5f17a39d4fde",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*/eventhubs/consumergroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*/receive/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Receiver",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-event-hubs-data-sender"></a>Azure Event Hubs-Datensender

Ermöglicht Absendern den Zugriff auf die Azure Event Hubs-Ressourcen. [Weitere Informationen](../event-hubs/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/*/eventhubs/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/*/send/action |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows send access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2b629674-e913-4c01-ae53-ef4638d8f975",
  "name": "2b629674-e913-4c01-ae53-ef4638d8f975",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*/eventhubs/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-factory-contributor"></a>Mitwirkender von Data Factory

Erstellen und verwalten Sie Data Factorys sowie die darin enthaltenen untergeordneten Ressourcen. [Weitere Informationen](../data-factory/concepts-roles-permissions.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.DataFactory](resource-provider-operations.md#microsoftdatafactory)/dataFactories/* | Erstellt und verwaltet Data Factorys und darin enthaltene untergeordnete Ressourcen. |
> | [Microsoft.DataFactory](resource-provider-operations.md#microsoftdatafactory)/factories/* | Erstellt und verwaltet Data Factorys und darin enthaltene untergeordnete Ressourcen. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/write | Erstellt oder aktualisiert eventSubscription. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create and manage data factories, as well as child resources within them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/673868aa-7521-48a0-acc6-0f60742d39f5",
  "name": "673868aa-7521-48a0-acc6-0f60742d39f5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DataFactory/dataFactories/*",
        "Microsoft.DataFactory/factories/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.EventGrid/eventSubscriptions/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Factory Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-purger"></a>Datenpurger

Löschen privater Daten aus einem Log Analytics-Arbeitsbereich. [Weitere Informationen](../azure-monitor/logs/personal-data-mgmt.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/purge/action | Daten werden aus Application Insights gelöscht |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/purge/action | Löscht die angegebenen Daten aus dem Arbeitsbereich. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can purge analytics data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/150f5e0c-0603-4f03-8c7f-cf70034c4e90",
  "name": "150f5e0c-0603-4f03-8c7f-cf70034c4e90",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/components/*/read",
        "Microsoft.Insights/components/purge/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/purge/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Purger",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hdinsight-cluster-operator"></a>HDInsight-Clusteroperator

Ermöglicht Ihnen das Lesen und Ändern von HDInsight-Clusterkonfigurationen. [Weitere Informationen](../hdinsight/hdinsight-migrate-granular-access-cluster-configurations.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.HDInsight](resource-provider-operations.md#microsofthdinsight)/*/read |  |
> | [Microsoft.HDInsight](resource-provider-operations.md#microsofthdinsight)/clusters/getGatewaySettings/action | Ruft Gatewayeinstellungen für HDInsight-Cluster ab. |
> | [Microsoft.HDInsight](resource-provider-operations.md#microsofthdinsight)/clusters/updateGatewaySettings/action | Aktualisiert Gatewayeinstellungen für HDInsight-Cluster. |
> | [Microsoft.HDInsight](resource-provider-operations.md#microsofthdinsight)/clusters/configurations/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and modify HDInsight cluster configurations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/61ed4efc-fab3-44fd-b111-e24485cc132a",
  "name": "61ed4efc-fab3-44fd-b111-e24485cc132a",
  "permissions": [
    {
      "actions": [
        "Microsoft.HDInsight/*/read",
        "Microsoft.HDInsight/clusters/getGatewaySettings/action",
        "Microsoft.HDInsight/clusters/updateGatewaySettings/action",
        "Microsoft.HDInsight/clusters/configurations/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "HDInsight Cluster Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hdinsight-domain-services-contributor"></a>Mitwirkender für die HDInsight-Domänendienste

Ermöglicht Ihnen, Vorgänge im Zusammenhang mit Domänendiensten, die für das HDInsight Enterprise-Sicherheitspaket erforderlich sind, zu lesen, zu erstellen, zu ändern und zu löschen. [Weitere Informationen](../hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.AAD](resource-provider-operations.md#microsoftaad)/*/read |  |
> | [Microsoft.AAD](resource-provider-operations.md#microsoftaad)/domainServices/*/read |  |
> | [Microsoft.AAD](resource-provider-operations.md#microsoftaad)/domainServices/oucontainer/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can Read, Create, Modify and Delete Domain Services related operations needed for HDInsight Enterprise Security Package",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8d8d5a11-05d3-4bda-a417-a08778121c7c",
  "name": "8d8d5a11-05d3-4bda-a417-a08778121c7c",
  "permissions": [
    {
      "actions": [
        "Microsoft.AAD/*/read",
        "Microsoft.AAD/domainServices/*/read",
        "Microsoft.AAD/domainServices/oucontainer/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "HDInsight Domain Services Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="log-analytics-contributor"></a>Log Analytics-Mitwirkender

Ein Log Analytics-Mitwirkender kann alle Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Das Bearbeiten von Überwachungseinstellungen schließt folgende Aufgaben ein: Hinzufügen der VM-Erweiterung zu VMs, Lesen von Speicherkontoschlüsseln zum Konfigurieren von Protokollsammlungen aus Azure Storage, Hinzufügen von Lösungen, Konfigurieren der Azure-Diagnose für alle Azure-Ressourcen. [Weitere Informationen](../azure-monitor/logs/manage-access.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | [Microsoft.ClassicCompute](resource-provider-operations.md#microsoftclassiccompute)/virtualMachines/extensions/* |  |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/extensions/* |  |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/extensions/write | Installiert oder aktualisiert eine Azure Arc-Erweiterung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/* |  |
> | [Microsoft.OperationsManagement](resource-provider-operations.md#microsoftoperationsmanagement)/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourcegroups/deployments/* |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Log Analytics Contributor can read all monitoring data and edit monitoring settings. Editing monitoring settings includes adding the VM extension to VMs; reading storage account keys to be able to configure collection of logs from Azure Storage; adding solutions; and configuring Azure diagnostics on all Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/92aaf0da-9dab-42b6-94a3-d43ce8d16293",
  "name": "92aaf0da-9dab-42b6-94a3-d43ce8d16293",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.ClassicCompute/virtualMachines/extensions/*",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.Compute/virtualMachines/extensions/*",
        "Microsoft.HybridCompute/machines/extensions/write",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.OperationalInsights/*",
        "Microsoft.OperationsManagement/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourcegroups/deployments/*",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Log Analytics Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="log-analytics-reader"></a>Log Analytics-Leser

Ein Log Analytics-Leser kann alle Überwachungsdaten anzeigen und durchsuchen sowie Überwachungseinstellungen anzeigen. Hierzu zählt auch die Anzeige der Konfiguration von Azure-Diagnosen für alle Azure-Ressourcen. [Weitere Informationen](../azure-monitor/logs/manage-access.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/search/action | Führt eine Suchabfrage aus. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/sharedKeys/read | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Log Analytics Reader can view and search all monitoring data as well as and view monitoring settings, including viewing the configuration of Azure diagnostics on all Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/73c42c96-874c-492b-b04d-ab87d138a893",
  "name": "73c42c96-874c-492b-b04d-ab87d138a893",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.OperationalInsights/workspaces/sharedKeys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Log Analytics Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="purview-data-curator-legacy"></a>Purview-Datenkurator (Legacy)

Datenkurator für Microsoft Purview ist eine Rolle, die Katalogdatenobjekte erstellen, lesen, ändern und löschen und Beziehungen zwischen Objekten herstellen kann. Diese Rolle wurde vor Kurzem für den rollenbasierten Zugriff in Azure als veraltet gekennzeichnet und durch eine neue Datenkuratorrolle auf der Azure Purview-Datenebene ersetzt. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Purview: Rollen](../purview/catalog-permissions.md#roles).

> [!div class="mx-tableFixed"]
> | Aktionen | Beschreibung |
> | --- | --- |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/read | Liest die Kontoressource für den Microsoft Purview-Anbieter. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/data/read | Liest Datenobjekte. |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/data/write | Erstellt, aktualisiert und löscht Datenobjekte. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "The Microsoft.Purview data curator is a legacy role that can create, read, modify and delete catalog data objects and establish relationships between objects. We have recently deprecated this role from Azure role-based access and introduced a new data curator inside Azure Purview data plane. See https://docs.microsoft.com/azure/purview/catalog-permissions#roles",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8a3c2885-9b38-4fd2-9d99-91af537c1347",
  "name": "8a3c2885-9b38-4fd2-9d99-91af537c1347",
  "permissions": [
    {
      "actions": [
        "Microsoft.Purview/accounts/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Purview/accounts/data/read",
        "Microsoft.Purview/accounts/data/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Purview Data Curator (Legacy)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="purview-data-reader-legacy"></a>Purview-Datenleseberechtigter (Legacy)

Die Rolle Microsoft Purview-Datenleser ist eine Legacyrolle, die Katalogdatenobjekte lesen kann. Diese Rolle wurde vor Kurzem für den rollenbasierten Zugriff in Azure als veraltet gekennzeichnet und durch eine neue Datenleserrolle auf der Azure Purview-Datenebene ersetzt. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Purview: Rollen](../purview/catalog-permissions.md#roles).

> [!div class="mx-tableFixed"]
> | Aktionen | Beschreibung |
> | --- | --- |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/read | Liest die Kontoressource für den Microsoft Purview-Anbieter. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/data/read | Liest Datenobjekte. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "The Microsoft.Purview data reader is a legacy role that can read catalog data objects. We have recently deprecated this role from Azure role-based access and introduced a new data reader inside Azure Purview data plane. See https://docs.microsoft.com/azure/purview/catalog-permissions#roles",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ff100721-1b9d-43d8-af52-42b69c1272db",
  "name": "ff100721-1b9d-43d8-af52-42b69c1272db",
  "permissions": [
    {
      "actions": [
        "Microsoft.Purview/accounts/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Purview/accounts/data/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Purview Data Reader (Legacy)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="purview-data-source-administrator-legacy"></a>Purview-Datenquellenadministrator (Legacy)

Der Datenquellenadministrator für Microsoft Purview ist eine Legacyrolle, die Datenquellen und Datenüberprüfungen verwalten kann. Diese Rolle wurde vor Kurzem für den rollenbasierten Zugriff in Azure als veraltet gekennzeichnet und durch eine neue Datenquellenadministratorrolle auf der Azure Purview-Datenebene ersetzt. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Purview: Rollen](../purview/catalog-permissions.md#roles).

> [!div class="mx-tableFixed"]
> | Aktionen | Beschreibung |
> | --- | --- |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/read | Liest die Kontoressource für den Microsoft Purview-Anbieter. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/scan/read | Liest Datenquellen und -überprüfungen. |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/scan/write | Erstellt, aktualisiert und löscht Datenquellen und verwaltet Überprüfungen. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "The Microsoft.Purview data source administrator is a legacy role that can manage data sources and data scans. We have recently deprecated this role from Azure role-based access and introduced a new data source admin inside Azure Purview data plane. See https://docs.microsoft.com/azure/purview/catalog-permissions#roles",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/200bba9e-f0c8-430f-892b-6f0794863803",
  "name": "200bba9e-f0c8-430f-892b-6f0794863803",
  "permissions": [
    {
      "actions": [
        "Microsoft.Purview/accounts/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Purview/accounts/scan/read",
        "Microsoft.Purview/accounts/scan/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Purview Data Source Administrator (Legacy)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="schema-registry-contributor-preview"></a>Mitwirkender der Schemaregistrierung (Vorschau)

Lesen, Schreiben und Löschen von Schemaregistrierungsgruppen und Schemas.

> [!div class="mx-tableFixed"]
> | Aktionen | Beschreibung |
> | --- | --- |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/namespaces/schemagroups/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/namespaces/schemas/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read, write, and delete Schema Registry groups and schemas.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5dffeca3-4936-4216-b2bc-10343a5abb25",
  "name": "5dffeca3-4936-4216-b2bc-10343a5abb25",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/namespaces/schemagroups/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/namespaces/schemas/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Schema Registry Contributor (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="schema-registry-reader-preview"></a>Schemaregistrierungsleser (Vorschau)

Lesen und Auflisten von Schemaregistrierungsgruppen und Schemas.

> [!div class="mx-tableFixed"]
> | Aktionen | Beschreibung |
> | --- | --- |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/namespaces/schemagroups/read | Dient zum Abrufen einer Liste mit SchemaGroup-Ressourcenbeschreibungen. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/namespaces/schemas/read | Dient zum Abrufen von Schemas. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read and list Schema Registry groups and schemas.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2c56ea50-c6b3-40a6-83c0-9d98858bc7d2",
  "name": "2c56ea50-c6b3-40a6-83c0-9d98858bc7d2",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/namespaces/schemagroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/namespaces/schemas/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Schema Registry Reader (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="blockchain"></a>Blockchain


### <a name="blockchain-member-node-access-preview"></a>Zugriff auf Blockchainmitgliedsknoten (Vorschauversion)

Ermöglicht den Zugriff auf Blockchainmitgliedsknoten. [Weitere Informationen](../blockchain/service/configure-aad.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Blockchain](resource-provider-operations.md#microsoftblockchain)/blockchainMembers/transactionNodes/read | Ruft die vorhandenen Transaktionsknoten eines Blockchainmitglieds ab oder listet sie auf |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Blockchain](resource-provider-operations.md#microsoftblockchain)/blockchainMembers/transactionNodes/connect/action | Stellt eine Verbindung mit dem Transaktionsknoten eines Blockchainmitglieds her |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for access to Blockchain Member nodes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/31a002a1-acaf-453e-8a5b-297c9ca1ea24",
  "name": "31a002a1-acaf-453e-8a5b-297c9ca1ea24",
  "permissions": [
    {
      "actions": [
        "Microsoft.Blockchain/blockchainMembers/transactionNodes/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Blockchain Member Node Access (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="ai--machine-learning"></a>KI und Machine Learning


### <a name="azureml-data-scientist"></a>AzureML Data Scientist

Kann alle Aktionen innerhalb eines Azure Machine Learning-Arbeitsbereichs ausführen, mit Ausnahme des Erstellens oder Löschens von Computeressourcen und des Änderns des Arbeitsbereichs selbst.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/*/read |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/*/action |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/*/delete |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/*/write |  |
> | **NotActions** |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/delete | Löscht die Machine Learning Services-Arbeitsbereiche. |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/write | Erstellt oder aktualisiert die Machine Learning Services-Arbeitsbereiche. |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/computes/*/write |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/computes/*/delete |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/computes/listKeys/action | Führt geheime Schlüssel für die Computeressourcen in Machine Learning Services-Arbeitsbereichen auf. |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/listKeys/action | Führt die geheimen Schlüssel für einen Machine Learning Services-Arbeitsbereich auf. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can perform all actions within an Azure Machine Learning workspace, except for creating or deleting compute resources and modifying the workspace itself.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f6c7c914-8db3-469d-8ca1-694a8f32e121",
  "name": "f6c7c914-8db3-469d-8ca1-694a8f32e121",
  "permissions": [
    {
      "actions": [
        "Microsoft.MachineLearningServices/workspaces/*/read",
        "Microsoft.MachineLearningServices/workspaces/*/action",
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/*/write"
      ],
      "notActions": [
        "Microsoft.MachineLearningServices/workspaces/delete",
        "Microsoft.MachineLearningServices/workspaces/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/listKeys/action",
        "Microsoft.MachineLearningServices/workspaces/listKeys/action"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AzureML Data Scientist",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-contributor"></a>Mitwirkender für Cognitive Services

Ermöglicht Ihnen das Erstellen, Lesen, Aktualisieren, Löschen und Verwalten von Cognitive Services-Schlüsseln. [Weitere Informationen](../cognitive-services/cognitive-services-virtual-networks.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/* |  |
> | [Microsoft.Features](resource-provider-operations.md#microsoftfeatures)/features/read | Ruft die Features eines Abonnements ab. |
> | [Microsoft.Features](resource-provider-operations.md#microsoftfeatures)/providers/features/read | Ruft das Feature eines Abonnements in einem angegebenen Ressourcenanbieter ab. |
> | [Microsoft.Features](resource-provider-operations.md#microsoftfeatures)/providers/features/register/action | Registriert das Feature für ein Abonnement in einem angegebenen Ressourcenanbieter. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/logDefinitions/read | Dient zum Lesen von Protokolldefinitionen. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricdefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Dient zum Lesen von Metriken. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourcegroups/deployments/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create, read, update, delete and manage keys of Cognitive Services.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68",
  "name": "25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.CognitiveServices/*",
        "Microsoft.Features/features/read",
        "Microsoft.Features/providers/features/read",
        "Microsoft.Features/providers/features/register/action",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Insights/logDefinitions/read",
        "Microsoft.Insights/metricdefinitions/read",
        "Microsoft.Insights/metrics/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourcegroups/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-custom-vision-contributor"></a>Cognitive Services: Custom Vision-Mitwirkender

Vollzugriff auf Projekte, einschließlich der Möglichkeit zum Anzeigen, Erstellen, Bearbeiten oder Löschen von Projekten [Weitere Informationen](../cognitive-services/custom-vision-service/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Full access to the project, including the ability to view, create, edit, or delete projects.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c1ff6cc2-c111-46fe-8896-e0ef812ad9f3",
  "name": "c1ff6cc2-c111-46fe-8896-e0ef812ad9f3",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Custom Vision Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-custom-vision-deployment"></a>Cognitive Services: Custom Vision-Bereitstellung

Veröffentlichen, Aufheben der Veröffentlichung oder Exportieren von Modellen. Mit der Bereitstellungsrolle kann das Projekt angezeigt, aber nicht aktualisiert werden. [Weitere Informationen](../cognitive-services/custom-vision-service/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/*/read |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/predictions/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/iterations/publish/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/iterations/export/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/quicktest/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/classify/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/detect/* |  |
> | **NotDataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/export/read | Exportiert ein Projekt. |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Publish, unpublish or export models. Deployment can view the project but can't update.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5c4089e1-6d96-4d2f-b296-c1bc7137275f",
  "name": "5c4089e1-6d96-4d2f-b296-c1bc7137275f",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/*/read",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/predictions/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/iterations/publish/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/iterations/export/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/quicktest/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/classify/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/detect/*"
      ],
      "notDataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/export/read"
      ]
    }
  ],
  "roleName": "Cognitive Services Custom Vision Deployment",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-custom-vision-labeler"></a>Cognitive Services: Custom Vision-Beschriftungsersteller

Anzeigen, Bearbeiten von Trainingsbildern und Erstellen, Hinzufügen, Entfernen oder Löschen von Bildtags. Beschriftungsersteller können Projekte anzeigen, jedoch nichts außer Trainingsbildern und Tags aktualisieren. [Weitere Informationen](../cognitive-services/custom-vision-service/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/*/read |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/predictions/query/action | Ruft Bilder ab, die an Ihren Vorhersageendpunkt gesendet wurden. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/images/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/tags/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/images/suggested/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/tagsandregions/suggestions/action | Mit dieser API werden vorgeschlagene Tags und Regionen für ein Array bzw. einen Batch mit nicht getaggten Bildern sowie Zuverlässigkeitsbewertungen für die Tags abgerufen. Wenn keine Tags gefunden werden, wird ein leeres Array zurückgegeben. |
> | **NotDataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/export/read | Exportiert ein Projekt. |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View, edit training images and create, add, remove, or delete the image tags. Labelers can view the project but can't update anything other than training images and tags.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/88424f51-ebe7-446f-bc41-7fa16989e96c",
  "name": "88424f51-ebe7-446f-bc41-7fa16989e96c",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/*/read",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/predictions/query/action",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/images/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/tags/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/images/suggested/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/tagsandregions/suggestions/action"
      ],
      "notDataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/export/read"
      ]
    }
  ],
  "roleName": "Cognitive Services Custom Vision Labeler",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-custom-vision-reader"></a>Cognitive Services: Custom Vision-Leser

Schreibgeschützte Aktionen im Projekt. Leser können das Projekt weder erstellen noch aktualisieren. [Weitere Informationen](../cognitive-services/custom-vision-service/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/*/read |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/predictions/query/action | Ruft Bilder ab, die an Ihren Vorhersageendpunkt gesendet wurden. |
> | **NotDataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/export/read | Exportiert ein Projekt. |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read-only actions in the project. Readers can't create or update the project.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/93586559-c37d-4a6b-ba08-b9f0940c2d73",
  "name": "93586559-c37d-4a6b-ba08-b9f0940c2d73",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/*/read",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/predictions/query/action"
      ],
      "notDataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/export/read"
      ]
    }
  ],
  "roleName": "Cognitive Services Custom Vision Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-custom-vision-trainer"></a>Cognitive Services: Custom Vision-Trainer

Anzeigen, Bearbeiten von Projekten und Trainieren der Modelle, einschließlich der Möglichkeit zum Veröffentlichen, Aufheben der Veröffentlichung und Exportieren der Modelle. Trainer können das Projekt weder erstellen noch löschen. [Weitere Informationen](../cognitive-services/custom-vision-service/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/* |  |
> | **NotDataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/action | Erstellt ein Projekt. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/delete | Löscht ein bestimmtes Projekt. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/import/action | Importiert ein Projekt. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/export/read | Exportiert ein Projekt. |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View, edit projects and train the models, including the ability to publish, unpublish, export the models. Trainers can't create or delete the project.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0a5ae4ab-0d65-4eeb-be61-29fc9b54394b",
  "name": "0a5ae4ab-0d65-4eeb-be61-29fc9b54394b",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/*"
      ],
      "notDataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/action",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/delete",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/import/action",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/export/read"
      ]
    }
  ],
  "roleName": "Cognitive Services Custom Vision Trainer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-data-reader-preview"></a>Cognitive Services-Datenleser (Vorschau)

Ermöglicht das Lesen von Cognitive Services-Daten.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read Cognitive Services data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b59867f0-fa02-499b-be73-45a86b5b3e1c",
  "name": "b59867f0-fa02-499b-be73-45a86b5b3e1c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Data Reader (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-face-recognizer"></a>Cognitive Services: Gesichtserkennung

Ermöglicht Ihnen das Ausführen von Vorgängen zum Erkennen, Überprüfen, Identifizieren und Gruppieren sowie zum Auffinden ähnlicher Gesichter in der Gesichtserkennungs-API. Diese Rolle lässt keine Erstellungs- oder Löschvorgänge zu. Daher eignet sie sich gut für Endpunkte, die nur Rückschlussfunktionen basierend auf der Regel der geringsten Rechte benötigen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/Face/detect/action | Hiermit werden menschliche Gesichter in einem Bild erkannt und Rechtecke für die Gesichter sowie optional faceIds, Sehenswürdigkeiten und Attribute zurückgegeben. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/Face/verify/action | Hiermit überprüfen Sie, ob zwei Gesichter zu derselben Person gehören oder ob je ein Gesicht zu einer Person gehört. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/Face/identify/action | 1:n-Identifikationsvorgang zum Ermitteln der besten Ergebnisse für das Gesicht der abgefragten Person aus einer Personengruppe oder umfangreichen Personengruppe. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/Face/group/action | Hiermit unterteilen Sie die Gesichter der Kandidaten basierend auf der Gesichterähnlichkeit in Gruppen. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/Face/findsimilars/action | Hiermit können Sie anhand der faceId des abgefragten Gesichts ähnliche Gesichter aus einem faceId-Array, einer Gesichterliste oder einer umfangreichen Gesichterliste suchen. faceId |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you perform detect, verify, identify, group, and find similar operations on Face API. This role does not allow create or delete operations, which makes it well suited for endpoints that only need inferencing capabilities, following 'least privilege' best practices.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9894cab4-e18a-44aa-828b-cb588cd6f2d7",
  "name": "9894cab4-e18a-44aa-828b-cb588cd6f2d7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/Face/detect/action",
        "Microsoft.CognitiveServices/accounts/Face/verify/action",
        "Microsoft.CognitiveServices/accounts/Face/identify/action",
        "Microsoft.CognitiveServices/accounts/Face/group/action",
        "Microsoft.CognitiveServices/accounts/Face/findsimilars/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Face Recognizer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-metrics-advisor-administrator"></a>Cognitive Services Metrics Advisor-Administrator

Vollzugriff auf das Projekt, einschließlich der Konfiguration auf Systemebene. [Weitere Informationen](../applied-ai-services/metrics-advisor/how-tos/alerts.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/MetricsAdvisor/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Full access to the project, including the system level configuration.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cb43c632-a144-4ec5-977c-e80c4affc34a",
  "name": "cb43c632-a144-4ec5-977c-e80c4affc34a",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/MetricsAdvisor/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Metrics Advisor Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-qna-maker-editor"></a>Cognitive Services QnA Maker-Editor

Erstellen, Bearbeiten, Importieren und Exportieren einer Wissensdatenbank. Sie können keine Wissensdatenbank veröffentlichen oder löschen. [Weitere Informationen](../cognitive-services/qnamaker/reference-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/roleAssignments/read | Dient zum Abrufen von Informationen zu einer Rollenzuweisung. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/roleDefinitions/read | Dient zum Abrufen von Informationen zu einer Rollendefinition. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/read | Ruft die Liste der Wissensdatenbanken oder Details einer bestimmten Wissensdatenbank ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/download/read | Lädt die Wissensdatenbank herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/create/write | Asynchroner Vorgang zum Erstellen einer neuen Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/write | Asynchroner Vorgang zum Ändern einer Wissensdatenbank oder zum Ersetzen des Inhalts der Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/generateanswer/action | GenerateAnswer-Befehl zum Abfragen der Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/train/action | Trainingsaufruf zum Hinzuzufügen von Vorschläge zur Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/alterations/read | Lädt Änderungen aus der Runtime herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/alterations/write | Ersetzt Änderungsdaten. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointkeys/read | Ruft Endpunktschlüssel für einen Endpunkt ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointkeys/refreshkeys/action | Generiert einen Endpunktschlüssel neu. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointsettings/read | Ruft Endpunkteinstellungen für einen Endpunkt ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointsettings/write | Aktualisiert Endpunkteinstellungen für einen Endpunkt. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/operations/read | Ruft Details zu einem bestimmten zeitintensiven Vorgang ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/read | Ruft die Liste der Wissensdatenbanken oder Details einer bestimmten Wissensdatenbank ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/download/read | Lädt die Wissensdatenbank herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/create/write | Asynchroner Vorgang zum Erstellen einer neuen Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/write | Asynchroner Vorgang zum Ändern einer Wissensdatenbank oder zum Ersetzen des Inhalts der Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/generateanswer/action | GenerateAnswer-Befehl zum Abfragen der Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/train/action | Trainingsaufruf zum Hinzuzufügen von Vorschläge zur Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/alterations/read | Lädt Änderungen aus der Runtime herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/alterations/write | Ersetzt Änderungsdaten. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointkeys/read | Ruft Endpunktschlüssel für einen Endpunkt ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointkeys/refreshkeys/action | Generiert einen Endpunktschlüssel neu. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointsettings/read | Ruft Endpunkteinstellungen für einen Endpunkt ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointsettings/write | Aktualisiert Endpunkteinstellungen für einen Endpunkt. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/operations/read | Ruft Details zu einem bestimmten zeitintensiven Vorgang ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/read | Ruft die Liste der Wissensdatenbanken oder Details einer bestimmten Wissensdatenbank ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/download/read | Lädt die Wissensdatenbank herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/create/write | Asynchroner Vorgang zum Erstellen einer neuen Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/write | Asynchroner Vorgang zum Ändern einer Wissensdatenbank oder zum Ersetzen des Inhalts der Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/generateanswer/action | GenerateAnswer-Befehl zum Abfragen der Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/train/action | Trainingsaufruf zum Hinzuzufügen von Vorschläge zur Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/alterations/read | Lädt Änderungen aus der Runtime herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/alterations/write | Ersetzt Änderungsdaten. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointkeys/read | Ruft Endpunktschlüssel für einen Endpunkt ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointkeys/refreshkeys/action | Generiert einen Endpunktschlüssel neu. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointsettings/read | Ruft Endpunkteinstellungen für einen Endpunkt ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointsettings/write | Aktualisiert Endpunkteinstellungen für einen Endpunkt. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/operations/read | Ruft Details zu einem bestimmten zeitintensiven Vorgang ab. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Let's you create, edit, import and export a KB. You cannot publish or delete a KB.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f4cc2bf9-21be-47a1-bdf1-5c5804381025",
  "name": "f4cc2bf9-21be-47a1-bdf1-5c5804381025",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read",
        "Microsoft.Authorization/roleAssignments/read",
        "Microsoft.Authorization/roleDefinitions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/create/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/train/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker/alterations/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/alterations/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointkeys/refreshkeys/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointsettings/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointsettings/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker/operations/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/create/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/train/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/alterations/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/alterations/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointkeys/refreshkeys/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointsettings/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointsettings/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/operations/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/create/write",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/write",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/train/action",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/alterations/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/alterations/write",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointkeys/refreshkeys/action",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointsettings/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointsettings/write",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/operations/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services QnA Maker Editor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-qna-maker-reader"></a>Cognitive Services QnA Maker-Leseberechtigter

Sie können Wissensdatenbanken nur lesen und testen. [Weitere Informationen](../cognitive-services/qnamaker/reference-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/roleAssignments/read | Dient zum Abrufen von Informationen zu einer Rollenzuweisung. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/roleDefinitions/read | Dient zum Abrufen von Informationen zu einer Rollendefinition. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/read | Ruft die Liste der Wissensdatenbanken oder Details einer bestimmten Wissensdatenbank ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/download/read | Lädt die Wissensdatenbank herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/generateanswer/action | GenerateAnswer-Befehl zum Abfragen der Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/alterations/read | Lädt Änderungen aus der Runtime herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointkeys/read | Ruft Endpunktschlüssel für einen Endpunkt ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointsettings/read | Ruft Endpunkteinstellungen für einen Endpunkt ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/read | Ruft die Liste der Wissensdatenbanken oder Details einer bestimmten Wissensdatenbank ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/download/read | Lädt die Wissensdatenbank herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/generateanswer/action | GenerateAnswer-Befehl zum Abfragen der Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/alterations/read | Lädt Änderungen aus der Runtime herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointkeys/read | Ruft Endpunktschlüssel für einen Endpunkt ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointsettings/read | Ruft Endpunkteinstellungen für einen Endpunkt ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/read | Ruft die Liste der Wissensdatenbanken oder Details einer bestimmten Wissensdatenbank ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/download/read | Lädt die Wissensdatenbank herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/generateanswer/action | GenerateAnswer-Befehl zum Abfragen der Wissensdatenbank. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/alterations/read | Lädt Änderungen aus der Runtime herunter. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointkeys/read | Ruft Endpunktschlüssel für einen Endpunkt ab. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointsettings/read | Ruft Endpunkteinstellungen für einen Endpunkt ab. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Let's you read and test a KB only.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/466ccd10-b268-4a11-b098-b4849f024126",
  "name": "466ccd10-b268-4a11-b098-b4849f024126",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read",
        "Microsoft.Authorization/roleAssignments/read",
        "Microsoft.Authorization/roleDefinitions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker/alterations/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointsettings/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/alterations/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointsettings/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/alterations/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointsettings/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services QnA Maker Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-user"></a>Cognitive Services-Benutzer

Ermöglicht Ihnen das Lesen und Auflisten von Cognitive Services-Schlüsseln. [Weitere Informationen](../cognitive-services/authentication.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/listkeys/action | Auflisten von Schlüsseln |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Liest eine klassische Metrikwarnung. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/read | Liest eine Diagnoseeinstellung für eine Ressource. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/logDefinitions/read | Dient zum Lesen von Protokolldefinitionen. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricdefinitions/read | Dient zum Lesen von Metrikdefinitionen. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Dient zum Lesen von Metriken. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and list keys of Cognitive Services.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a97b65f3-24c7-4388-baec-2e87135dc908",
  "name": "a97b65f3-24c7-4388-baec-2e87135dc908",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read",
        "Microsoft.CognitiveServices/accounts/listkeys/action",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Insights/diagnosticSettings/read",
        "Microsoft.Insights/logDefinitions/read",
        "Microsoft.Insights/metricdefinitions/read",
        "Microsoft.Insights/metrics/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="internet-of-things"></a>Internet der Dinge (IoT)


### <a name="device-update-administrator"></a>Device Update-Administrator

Mit dieser Rolle erhalten Sie Vollzugriff auf Verwaltungs- und Inhaltsvorgänge. [Weitere Informationen](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/read | Führt einen Lesevorgang im Zusammenhang mit Updates aus. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/write | Führt einen Schreibvorgang im Zusammenhang mit Updates aus. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/delete | Führt einen Löschvorgang im Zusammenhang mit Updates aus. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/read | Führt einen Lesevorgang im Zusammenhang mit der Verwaltung aus. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/write | Führt einen Schreibvorgang im Zusammenhang mit der Verwaltung aus. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/delete | Führt einen Löschvorgang im Zusammenhang mit der Verwaltung aus. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you full access to management and content operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/02ca0879-e8e4-47a5-a61e-5c618b76e64a",
  "name": "02ca0879-e8e4-47a5-a61e-5c618b76e64a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/updates/read",
        "Microsoft.DeviceUpdate/accounts/instances/updates/write",
        "Microsoft.DeviceUpdate/accounts/instances/updates/delete",
        "Microsoft.DeviceUpdate/accounts/instances/management/read",
        "Microsoft.DeviceUpdate/accounts/instances/management/write",
        "Microsoft.DeviceUpdate/accounts/instances/management/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="device-update-content-administrator"></a>Device Update-Inhaltsadministrator

Mit dieser Rolle erhalten Sie Vollzugriff auf Inhaltsvorgänge. [Weitere Informationen](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/read | Führt einen Lesevorgang im Zusammenhang mit Updates aus. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/write | Führt einen Schreibvorgang im Zusammenhang mit Updates aus. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/delete | Führt einen Löschvorgang im Zusammenhang mit Updates aus. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you full access to content operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0378884a-3af5-44ab-8323-f5b22f9f3c98",
  "name": "0378884a-3af5-44ab-8323-f5b22f9f3c98",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/updates/read",
        "Microsoft.DeviceUpdate/accounts/instances/updates/write",
        "Microsoft.DeviceUpdate/accounts/instances/updates/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Content Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="device-update-content-reader"></a>Device Update-Inhaltsleser

Mit dieser Rolle erhalten Sie Lesezugriff auf Inhaltsvorgänge, können jedoch keine Änderungen vornehmen. [Weitere Informationen](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/read | Führt einen Lesevorgang im Zusammenhang mit Updates aus. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you read access to content operations, but does not allow making changes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d1ee9a80-8b14-47f0-bdc2-f4a351625a7b",
  "name": "d1ee9a80-8b14-47f0-bdc2-f4a351625a7b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/updates/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Content Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="device-update-deployments-administrator"></a>Device Update-Bereitstellungsadministrator

Mit dieser Rolle erhalten Sie Vollzugriff auf Verwaltungsvorgänge. [Weitere Informationen](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/read | Führt einen Lesevorgang im Zusammenhang mit der Verwaltung aus. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/write | Führt einen Schreibvorgang im Zusammenhang mit der Verwaltung aus. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/delete | Führt einen Löschvorgang im Zusammenhang mit der Verwaltung aus. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you full access to management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e4237640-0e3d-4a46-8fda-70bc94856432",
  "name": "e4237640-0e3d-4a46-8fda-70bc94856432",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/management/read",
        "Microsoft.DeviceUpdate/accounts/instances/management/write",
        "Microsoft.DeviceUpdate/accounts/instances/management/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Deployments Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="device-update-deployments-reader"></a>Device Update-Bereitstellungsleser

Mit dieser Rolle erhalten Sie Lesezugriff auf Verwaltungsvorgänge, können jedoch keine Änderungen vornehmen. [Weitere Informationen](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/read | Führt einen Lesevorgang im Zusammenhang mit der Verwaltung aus. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you read access to management operations, but does not allow making changes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/49e2f5d2-7741-4835-8efa-19e1fe35e47f",
  "name": "49e2f5d2-7741-4835-8efa-19e1fe35e47f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/management/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Deployments Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="device-update-reader"></a>Device Update-Leser

Mit dieser Rolle erhalten Sie Lesezugriff auf Verwaltungs- und Inhaltsvorgänge, können jedoch keine Änderungen vornehmen. [Weitere Informationen](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/read | Führt einen Lesevorgang im Zusammenhang mit Updates aus. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/read | Führt einen Lesevorgang im Zusammenhang mit der Verwaltung aus. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you read access to management and content operations, but does not allow making changes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e9dba6fb-3d52-4cf0-bce3-f06ce71b9e0f",
  "name": "e9dba6fb-3d52-4cf0-bce3-f06ce71b9e0f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/updates/read",
        "Microsoft.DeviceUpdate/accounts/instances/management/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="iot-hub-data-contributor"></a>Mitwirkender an IoT Hub-Daten

Ermöglicht Vollzugriff für Vorgänge auf der IoT Hub-Datenebene [Weitere Informationen](../iot-hub/iot-hub-dev-guide-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Devices](resource-provider-operations.md#microsoftdevices)/IotHubs/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to IoT Hub data plane operations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4fc6c259-987e-4a07-842e-c321cc9d413f",
  "name": "4fc6c259-987e-4a07-842e-c321cc9d413f",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Devices/IotHubs/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "IoT Hub Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="iot-hub-data-reader"></a>IoT Hub-Datenleser

Ermöglicht vollständigen Lesezugriff auf Eigenschaften der IoT Hub-Datenebene [Weitere Informationen](../iot-hub/iot-hub-dev-guide-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Devices](resource-provider-operations.md#microsoftdevices)/IotHubs/*/read |  |
> | [Microsoft.Devices](resource-provider-operations.md#microsoftdevices)/IotHubs/fileUpload/notifications/action | Empfangen, Abschließen oder Abbrechen von Dateiuploadbenachrichtigungen. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full read access to IoT Hub data-plane properties",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b447c946-2db7-41ec-983d-d8bf3b1c77e3",
  "name": "b447c946-2db7-41ec-983d-d8bf3b1c77e3",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Devices/IotHubs/*/read",
        "Microsoft.Devices/IotHubs/fileUpload/notifications/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "IoT Hub Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="iot-hub-registry-contributor"></a>Mitwirkender an IoT Hub-Registrierung

Ermöglicht Vollzugriff auf die IoT Hub-Geräteregistrierung [Weitere Informationen](../iot-hub/iot-hub-dev-guide-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Devices](resource-provider-operations.md#microsoftdevices)/IotHubs/devices/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to IoT Hub device registry.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4ea46cd5-c1b2-4a8e-910b-273211f9ce47",
  "name": "4ea46cd5-c1b2-4a8e-910b-273211f9ce47",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Devices/IotHubs/devices/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "IoT Hub Registry Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="iot-hub-twin-contributor"></a>Mitwirkender an IoT Hub-Zwillingen

Ermöglicht Lese- und Schreibzugriff auf alle IoT Hub-Geräte- und -Modulzwillinge. [Weitere Informationen](../iot-hub/iot-hub-dev-guide-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Devices](resource-provider-operations.md#microsoftdevices)/IotHubs/twins/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read and write access to all IoT Hub device and module twins.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/494bdba2-168f-4f31-a0a1-191d2f7c028c",
  "name": "494bdba2-168f-4f31-a0a1-191d2f7c028c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Devices/IotHubs/twins/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "IoT Hub Twin Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="mixed-reality"></a>Mixed Reality


### <a name="remote-rendering-administrator"></a>Remote Rendering-Administrator

Bietet dem Benutzer Konvertierungs-, Sitzungsverwaltungs-, Rendering- und Diagnosefunktionen für Azure Remote Rendering. [Weitere Informationen](../remote-rendering/how-tos/authentication.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/convert/action | Startet die Objektkonvertierung. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/convert/read | Ruft die Objektkonvertierungseigenschaften ab. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/convert/delete | Beendet die Objektkonvertierung. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/read | Ruft Sitzungseigenschaften ab. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/action | Startet Sitzungen. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/delete | Beendet Sitzungen. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/render/read | Stellt eine Verbindung mit einer Sitzung her. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/diagnostic/read | Stellt eine Verbindung mit der Remote Rendering-Prüfung her. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Provides user with conversion, manage session, rendering and diagnostics capabilities for Azure Remote Rendering",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3df8b902-2a6f-47c7-8cc5-360e9b272a7e",
  "name": "3df8b902-2a6f-47c7-8cc5-360e9b272a7e",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/RemoteRenderingAccounts/convert/action",
        "Microsoft.MixedReality/RemoteRenderingAccounts/convert/read",
        "Microsoft.MixedReality/RemoteRenderingAccounts/convert/delete",
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/read",
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/action",
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/delete",
        "Microsoft.MixedReality/RemoteRenderingAccounts/render/read",
        "Microsoft.MixedReality/RemoteRenderingAccounts/diagnostic/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Remote Rendering Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="remote-rendering-client"></a>Remote Rendering-Client

Bietet dem Benutzer Sitzungsverwaltungs-, Rendering- und Diagnosefunktionen für Azure Remote Rendering. [Weitere Informationen](../remote-rendering/how-tos/authentication.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/read | Ruft Sitzungseigenschaften ab. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/action | Startet Sitzungen. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/delete | Beendet Sitzungen. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/render/read | Stellt eine Verbindung mit einer Sitzung her. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/diagnostic/read | Stellt eine Verbindung mit der Remote Rendering-Prüfung her. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Provides user with manage session, rendering and diagnostics capabilities for Azure Remote Rendering.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d39065c4-c120-43c9-ab0a-63eed9795f0a",
  "name": "d39065c4-c120-43c9-ab0a-63eed9795f0a",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/read",
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/action",
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/delete",
        "Microsoft.MixedReality/RemoteRenderingAccounts/render/read",
        "Microsoft.MixedReality/RemoteRenderingAccounts/diagnostic/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Remote Rendering Client",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-contributor"></a>Spatial Anchors-Kontomitwirkender

Ermöglicht Ihnen das Verwalten von Raumankern in Ihrem Konto, nicht jedoch das Löschen von Ankern. [Weitere Informationen](../spatial-anchors/concepts/authentication.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/create/action | Hiermit werden Raumanker erstellt. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/discovery/read | Hiermit werden Raumanker in räumlicher Nähe ermittelt. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/properties/read | Hiermit werden die Eigenschaften von Raumankern abgerufen. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/query/read | Hiermit finden Sie Raumanker. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/submitdiag/read | Hiermit übermitteln Sie Diagnosedaten, um die Qualität des Azure Spatial Anchors-Diensts zu verbessern. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/write | Hiermit aktualisieren Sie die Eigenschaften von Raumankern. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage spatial anchors in your account, but not delete them",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827",
  "name": "8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/create/action",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-owner"></a>Spatial Anchors-Kontobesitzer

Ermöglicht Ihnen das Verwalten von Raumankern in Ihrem Konto, einschließlich der Löschung von Ankern. [Weitere Informationen](../spatial-anchors/concepts/authentication.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/create/action | Hiermit werden Raumanker erstellt. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/delete | Hiermit werden Raumanker gelöscht. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/discovery/read | Hiermit werden Raumanker in räumlicher Nähe ermittelt. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/properties/read | Hiermit werden die Eigenschaften von Raumankern abgerufen. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/query/read | Hiermit finden Sie Raumanker. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/submitdiag/read | Hiermit übermitteln Sie Diagnosedaten, um die Qualität des Azure Spatial Anchors-Diensts zu verbessern. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/write | Hiermit aktualisieren Sie die Eigenschaften von Raumankern. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage spatial anchors in your account, including deleting them",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/70bbe301-9835-447d-afdd-19eb3167307c",
  "name": "70bbe301-9835-447d-afdd-19eb3167307c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/create/action",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/delete",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-reader"></a>Spatial Anchors-Kontoleser

Ermöglicht Ihnen das Ermitteln und Lesen von Eigenschaften für Raumanker in Ihrem Dokument. [Weitere Informationen](../spatial-anchors/concepts/authentication.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/discovery/read | Hiermit werden Raumanker in räumlicher Nähe ermittelt. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/properties/read | Hiermit werden die Eigenschaften von Raumankern abgerufen. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/query/read | Hiermit finden Sie Raumanker. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/submitdiag/read | Hiermit übermitteln Sie Diagnosedaten, um die Qualität des Azure Spatial Anchors-Diensts zu verbessern. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you locate and read properties of spatial anchors in your account",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d51204f-eb77-4b1c-b86a-2ec626c49413",
  "name": "5d51204f-eb77-4b1c-b86a-2ec626c49413",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="integration"></a>Integration


### <a name="api-management-service-contributor"></a>Mitwirkender des API-Verwaltungsdienstes

Kann Dienst und APIs verwalten. [Weitere Informationen](../api-management/api-management-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/* | Erstellen und Verwalten des API Management-Diensts |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage service and the APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c",
  "name": "312a565d-c81f-4fd8-895a-4e21e48d571c",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="api-management-service-operator-role"></a>Operatorrolle des API Management-Diensts

Kann Dienst, aber nicht die APIs verwalten. [Weitere Informationen](../api-management/api-management-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/*/read | Dient zum Lesen von API Management-Dienstinstanzen. |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/backup/action | Dient zum Sichern des API Management-Diensts im angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/delete | Dient zum Löschen einer API Management-Dienstinstanz. |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/managedeployments/action | Dient zum Ändern der SKU/Einheiten sowie zum Hinzufügen/Entfernen regionaler Bereitstellungen des API Management-Diensts. |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/read | Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz. |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/restore/action | Dient zum Wiederherstellen des API Management-Diensts aus dem angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto. |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/updatecertificate/action | Dient zum Hochladen eines TLS/SSL-Zertifikats für einen API Management-Dienst |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/updatehostname/action | Dient zum Einrichten, Aktualisieren oder Entfernen benutzerdefinierter Domänennamen für einen API Management-Dienst. |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/write | Erstellen oder Aktualisieren einer API Management-Dienstinstanz |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/users/keys/read | Abrufen von Benutzern zugeordneten Schlüsseln |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage service but not the APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e022efe7-f5ba-4159-bbe4-b44f577e9b61",
  "name": "e022efe7-f5ba-4159-bbe4-b44f577e9b61",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*/read",
        "Microsoft.ApiManagement/service/backup/action",
        "Microsoft.ApiManagement/service/delete",
        "Microsoft.ApiManagement/service/managedeployments/action",
        "Microsoft.ApiManagement/service/read",
        "Microsoft.ApiManagement/service/restore/action",
        "Microsoft.ApiManagement/service/updatecertificate/action",
        "Microsoft.ApiManagement/service/updatehostname/action",
        "Microsoft.ApiManagement/service/write",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.ApiManagement/service/users/keys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Operator Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="api-management-service-reader-role"></a>Leserrolle des API Management-Diensts

Schreibgeschützter Zugriff auf Dienst und APIs [Weitere Informationen](../api-management/api-management-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/*/read | Dient zum Lesen von API Management-Dienstinstanzen. |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/read | Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/users/keys/read | Abrufen von Benutzern zugeordneten Schlüsseln |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read-only access to service and APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/71522526-b88f-4d52-b57f-d31fc3546d0d",
  "name": "71522526-b88f-4d52-b57f-d31fc3546d0d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*/read",
        "Microsoft.ApiManagement/service/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.ApiManagement/service/users/keys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Reader Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="app-configuration-data-owner"></a>App Configuration-Datenbesitzer

Ermöglicht den Vollzugriff auf App Configuration-Daten. [Weitere Informationen](../azure-app-configuration/concept-enable-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.AppConfiguration](resource-provider-operations.md#microsoftappconfiguration)/configurationStores/*/read |  |
> | [Microsoft.AppConfiguration](resource-provider-operations.md#microsoftappconfiguration)/configurationStores/*/write |  |
> | [Microsoft.AppConfiguration](resource-provider-operations.md#microsoftappconfiguration)/configurationStores/*/delete |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows full access to App Configuration data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b",
  "name": "5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppConfiguration/configurationStores/*/read",
        "Microsoft.AppConfiguration/configurationStores/*/write",
        "Microsoft.AppConfiguration/configurationStores/*/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "App Configuration Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="app-configuration-data-reader"></a>App Configuration-Datenleser

Ermöglicht den Lesezugriff auf App Configuration-Daten. [Weitere Informationen](../azure-app-configuration/concept-enable-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.AppConfiguration](resource-provider-operations.md#microsoftappconfiguration)/configurationStores/*/read |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to App Configuration data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/516239f1-63e1-4d78-a4de-a74fb236a071",
  "name": "516239f1-63e1-4d78-a4de-a74fb236a071",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppConfiguration/configurationStores/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "App Configuration Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-relay-listener"></a>Azure Relay-Listener

Ermöglicht Lauschzugriff auf Azure Relay-Ressourcen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/wcfRelays/read |  |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/hybridConnections/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/listen/action |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for listen access to Azure Relay resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/26e0b698-aa6d-4085-9386-aadae190014d",
  "name": "26e0b698-aa6d-4085-9386-aadae190014d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Relay/*/wcfRelays/read",
        "Microsoft.Relay/*/hybridConnections/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Relay/*/listen/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Relay Listener",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-relay-owner"></a>Azure Relay-Besitzer

Ermöglicht Vollzugriff auf Azure Relay-Ressourcen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Relay resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2787bf04-f1f5-4bfe-8383-c8a24483ee38",
  "name": "2787bf04-f1f5-4bfe-8383-c8a24483ee38",
  "permissions": [
    {
      "actions": [
        "Microsoft.Relay/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Relay/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Relay Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-relay-sender"></a>Azure Relay-Absender

Ermöglicht Sendezugriff auf Azure Relay-Ressourcen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/wcfRelays/read |  |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/hybridConnections/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/send/action |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for send access to Azure Relay resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/26baccc8-eea7-41f1-98f4-1762cc7f685d",
  "name": "26baccc8-eea7-41f1-98f4-1762cc7f685d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Relay/*/wcfRelays/read",
        "Microsoft.Relay/*/hybridConnections/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Relay/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Relay Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-owner"></a>Azure Service Bus-Datenbesitzer

Ermöglicht den uneingeschränkten Zugriff auf die Azure Service Bus-Ressourcen. [Weitere Informationen](../service-bus-messaging/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/090c5cfd-751d-490a-894a-3ce6f1109419",
  "name": "090c5cfd-751d-490a-894a-3ce6f1109419",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-receiver"></a>Azure Service Bus-Datenempfänger

Ermöglicht Empfängern den Zugriff auf die Azure Service Bus-Ressourcen. [Weitere Informationen](../service-bus-messaging/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/queues/read |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/topics/read |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/receive/action |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for receive access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0",
  "name": "4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*/queues/read",
        "Microsoft.ServiceBus/*/topics/read",
        "Microsoft.ServiceBus/*/topics/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*/receive/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Receiver",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-sender"></a>Azure Service Bus-Datensender

Ermöglicht Absendern den Zugriff auf die Azure Service Bus-Ressourcen. [Weitere Informationen](../service-bus-messaging/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/queues/read |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/topics/read |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/send/action |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for send access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/69a216fc-b8fb-44d8-bc22-1f3c2cd27a39",
  "name": "69a216fc-b8fb-44d8-bc22-1f3c2cd27a39",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*/queues/read",
        "Microsoft.ServiceBus/*/topics/read",
        "Microsoft.ServiceBus/*/topics/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-stack-registration-owner"></a>Besitzer der Azure Stack-Registrierung

Ermöglicht Ihnen die Verwaltung von Azure Stack-Registrierungen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.AzureStack](resource-provider-operations.md#microsoftazurestack)/edgeSubscriptions/read |  |
> | [Microsoft.AzureStack](resource-provider-operations.md#microsoftazurestack)/registrations/products/*/action |  |
> | [Microsoft.AzureStack](resource-provider-operations.md#microsoftazurestack)/registrations/products/read | Ruft die Eigenschaften eines Azure Stack-Marketplace-Produkts ab. |
> | [Microsoft.AzureStack](resource-provider-operations.md#microsoftazurestack)/registrations/read | Ruft die Eigenschaften einer Azure Stack-Registrierung ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Azure Stack registrations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6f12a6df-dd06-4f3e-bcb1-ce8be600526a",
  "name": "6f12a6df-dd06-4f3e-bcb1-ce8be600526a",
  "permissions": [
    {
      "actions": [
        "Microsoft.AzureStack/edgeSubscriptions/read",
        "Microsoft.AzureStack/registrations/products/*/action",
        "Microsoft.AzureStack/registrations/products/read",
        "Microsoft.AzureStack/registrations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Stack Registration Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-contributor"></a>EventGrid Contributor

Ermöglicht die Verwaltung von EventGrid-Vorgängen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/* | Erstellen und Verwalten von Event Grid-Ressourcen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage EventGrid operations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/1e241071-0855-49ea-94dc-649edcd759de",
  "name": "1e241071-0855-49ea-94dc-649edcd759de",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-data-sender"></a>EventGrid-Datenabsender

Ermöglicht Sendezugriff auf Event Grid-Ereignisse.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/topics/read | Liest ein Thema. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/domains/read | Liest eine Domäne. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/partnerNamespaces/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/events/send/action | Sendet Ereignisse an Themen. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows send access to event grid events.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d5a91429-5739-47e2-a06b-3470a27159e7",
  "name": "d5a91429-5739-47e2-a06b-3470a27159e7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/topics/read",
        "Microsoft.EventGrid/domains/read",
        "Microsoft.EventGrid/partnerNamespaces/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventGrid/events/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-eventsubscription-contributor"></a>EventGrid EventSubscription-Mitwirkender

Ermöglicht die Verwaltung von EventGrid-Ereignisabonnementvorgängen. [Weitere Informationen](../event-grid/security-authorization.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/* | Erstellt und verwaltet regionale Ereignisabonnements. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/topicTypes/eventSubscriptions/read | Listet globale Ereignisabonnements nach Thematyp auf. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/locations/eventSubscriptions/read | Listet regionale Ereignisabonnements auf. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/locations/topicTypes/eventSubscriptions/read | Listet regionale Ereignisabonnements nach Thematyp auf. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage EventGrid event subscription operations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/428e0ff0-5e57-4d9c-a221-2c70d0e0a443",
  "name": "428e0ff0-5e57-4d9c-a221-2c70d0e0a443",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/eventSubscriptions/*",
        "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid EventSubscription Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-eventsubscription-reader"></a>EventGrid EventSubscription-Leser

Ermöglicht das Lesen von EventGrid-Ereignisabonnements. [Weitere Informationen](../event-grid/security-authorization.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/read | Liest eventSubscription. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/topicTypes/eventSubscriptions/read | Listet globale Ereignisabonnements nach Thematyp auf. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/locations/eventSubscriptions/read | Listet regionale Ereignisabonnements auf. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/locations/topicTypes/eventSubscriptions/read | Listet regionale Ereignisabonnements nach Thematyp auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read EventGrid event subscriptions.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2414bbcf-6497-4faf-8c65-045460748405",
  "name": "2414bbcf-6497-4faf-8c65-045460748405",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/eventSubscriptions/read",
        "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid EventSubscription Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="fhir-data-contributor"></a>Mitwirkender an FHIR-Daten

Die Rolle ermöglicht dem Benutzer oder Prinzipal vollen Zugriff auf FHIR-Daten. [Weitere Informationen](../healthcare-apis/azure-api-for-fhir/configure-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.HealthcareApis/services/fhir/resources/* |  |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Role allows user or principal full access to FHIR Data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5a1fc7df-4bf1-4951-a576-89034ee01acd",
  "name": "5a1fc7df-4bf1-4951-a576-89034ee01acd",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.HealthcareApis/services/fhir/resources/*",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "FHIR Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="fhir-data-exporter"></a>FHIR-Datenexportierer

Die Rolle ermöglicht dem Benutzer oder Prinzipal das Lesen und Exportieren von FHIR-Daten. [Weitere Informationen](../healthcare-apis/azure-api-for-fhir/configure-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.HealthcareApis/services/fhir/resources/read | Liest FHIR-Ressourcen (einschließlich Suche und Verlauf mit Versionsangabe).  |
> | Microsoft.HealthcareApis/services/fhir/resources/export/action | Exportvorgang ($export). |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/read | Liest FHIR-Ressourcen (einschließlich Suche und Verlauf mit Versionsangabe).  |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/export/action | Exportvorgang ($export). |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Role allows user or principal to read and export FHIR Data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3db33094-8700-4567-8da5-1501d4e7e843",
  "name": "3db33094-8700-4567-8da5-1501d4e7e843",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.HealthcareApis/services/fhir/resources/read",
        "Microsoft.HealthcareApis/services/fhir/resources/export/action",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/read",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/export/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "FHIR Data Exporter",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="fhir-data-reader"></a>FHIR-Datenleser

Die Rolle ermöglicht dem Benutzer oder Prinzipal das Lesen von FHIR-Daten. [Weitere Informationen](../healthcare-apis/azure-api-for-fhir/configure-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.HealthcareApis/services/fhir/resources/read | Liest FHIR-Ressourcen (einschließlich Suche und Verlauf mit Versionsangabe).  |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/read | Liest FHIR-Ressourcen (einschließlich Suche und Verlauf mit Versionsangabe).  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Role allows user or principal to read FHIR Data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4c8d0bbc-75d3-4935-991f-5f3c56d81508",
  "name": "4c8d0bbc-75d3-4935-991f-5f3c56d81508",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.HealthcareApis/services/fhir/resources/read",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "FHIR Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="fhir-data-writer"></a>FHIR-Datenschreiber

Die Rolle ermöglicht dem Benutzer oder Prinzipal das Lesen und Schreiben von FHIR-Daten. [Weitere Informationen](../healthcare-apis/azure-api-for-fhir/configure-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | Microsoft.HealthcareApis/services/fhir/resources/* |  |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/* |  |
> | **NotDataActions** |  |
> | Microsoft.HealthcareApis/services/fhir/resources/hardDelete/action | Endgültiger Löschvorgang (einschließlich Versionsverlauf). |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/hardDelete/action | Endgültiger Löschvorgang (einschließlich Versionsverlauf). |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Role allows user or principal to read and write FHIR Data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3f88fce4-5892-4214-ae73-ba5294559913",
  "name": "3f88fce4-5892-4214-ae73-ba5294559913",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.HealthcareApis/services/fhir/resources/*",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/*"
      ],
      "notDataActions": [
        "Microsoft.HealthcareApis/services/fhir/resources/hardDelete/action",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/hardDelete/action"
      ]
    }
  ],
  "roleName": "FHIR Data Writer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="integration-service-environment-contributor"></a>Mitwirkender für Integrationsdienstumgebungen

Hiermit wird das Verwalten von Integrationsdienstumgebungen ermöglicht, nicht aber der Zugriff auf diese. [Weitere Informationen](../logic-apps/add-artifacts-integration-service-environment-ise.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/integrationServiceEnvironments/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage integration service environments, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a41e2c5b-bd99-4a07-88f4-9bf657a760b8",
  "name": "a41e2c5b-bd99-4a07-88f4-9bf657a760b8",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Support/*",
        "Microsoft.Logic/integrationServiceEnvironments/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Integration Service Environment Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="integration-service-environment-developer"></a>Entwickler für Integrationsdienstumgebungen

Hiermit wird Entwicklern das Erstellen und Aktualisieren von Workflows, Integrationskonten und API-Verbindungen in Integrationsdienstumgebungen ermöglicht. [Weitere Informationen](../logic-apps/add-artifacts-integration-service-environment-ise.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/integrationServiceEnvironments/read | Hiermit wird die Integrationsdienstumgebung gelesen. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/integrationServiceEnvironments/*/join/action |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows developers to create and update workflows, integration accounts and API connections in integration service environments.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c7aa55d3-1abb-444a-a5ca-5e51e485d6ec",
  "name": "c7aa55d3-1abb-444a-a5ca-5e51e485d6ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Support/*",
        "Microsoft.Logic/integrationServiceEnvironments/read",
        "Microsoft.Logic/integrationServiceEnvironments/*/join/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Integration Service Environment Developer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="intelligent-systems-account-contributor"></a>Mitwirkender von Intelligent Systems-Konto

Ermöglicht Ihnen das Verwalten von Intelligent Systems-Konten, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | Microsoft.IntelligentSystems/accounts/* | Erstellen und Verwalten von Intelligent Systems-Konten |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Intelligent Systems accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/03a6d094-3444-4b3d-88af-7477090a9e5e",
  "name": "03a6d094-3444-4b3d-88af-7477090a9e5e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.IntelligentSystems/accounts/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Intelligent Systems Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="logic-app-contributor"></a>Mitwirkender für Logik-Apps

Ermöglicht Ihnen die Verwaltung von Logik-Apps. Sie können aber nicht den App-Zugriff ändern. [Weitere Informationen](../logic-apps/logic-apps-securing-a-logic-app.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/listKeys/action | Listet die Zugriffsschlüssel für die Speicherkonten auf. |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/read | Dient zum Zurückgeben des Speicherkontos mit dem angegebenen Konto. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricAlerts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/logdefinitions/* | Diese Berechtigung ist für Benutzer notwendig, die über das Portal auf Aktivitätsprotokolle zugreifen müssen. Auflisten der Protokollkategorien im Aktivitätsprotokoll. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/* | Lesen von Metrikdefinitionen (Liste der verfügbaren Metriktypen für eine Ressource). |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/* | Verwaltet Logic Apps-Ressourcen. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listkeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/connectionGateways/* | Erstellt und verwaltet ein Verbindungsgateway. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/connections/* | Erstellt und verwaltet eine Verbindung. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/customApis/* | Erstellt und verwaltet eine benutzerdefinierte API. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/join/action | Hiermit wird der Beitritt zu einem App Service-Plan ausgeführt. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/sites/functions/listSecrets/action | Auflisten von Funktionsgeheimnissen. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage logic app, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/87a39d53-fc1b-424a-814c-f7e04687dc9e",
  "name": "87a39d53-fc1b-424a-814c-f7e04687dc9e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.ClassicStorage/storageAccounts/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metricAlerts/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Insights/logdefinitions/*",
        "Microsoft.Insights/metricDefinitions/*",
        "Microsoft.Logic/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*",
        "Microsoft.Web/connectionGateways/*",
        "Microsoft.Web/connections/*",
        "Microsoft.Web/customApis/*",
        "Microsoft.Web/serverFarms/join/action",
        "Microsoft.Web/serverFarms/read",
        "Microsoft.Web/sites/functions/listSecrets/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Logic App Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="logic-app-operator"></a>Logik-App-Operator

Ermöglicht Ihnen das Lesen, Aktivieren und Deaktivieren von Logik-Apps. Sie können diese aber nicht bearbeiten oder aktualisieren. [Weitere Informationen](../logic-apps/logic-apps-securing-a-logic-app.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/*/read | Lesen von Insights-Warnungsregeln |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricAlerts/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/*/read | Ruft die Diagnoseeinstellungen für Logic Apps ab. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/*/read | Ruft die verfügbaren Metriken für Logic Apps ab. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/*/read | Liest Logic Apps-Ressourcen. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/disable/action | Deaktiviert den Workflow. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/enable/action | Aktiviert den Workflow. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/validate/action | Überprüft den Workflow. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/connectionGateways/*/read | Liest Verbindungsgateways. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/connections/*/read | Liest Verbindungen. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/customApis/*/read | Liest benutzerdefinierte API. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/read | Dient zum Abrufen der Eigenschaften für einen App Service-Plan. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read, enable and disable logic app.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/515c2055-d9d4-4321-b1b9-bd0c9a0f79fe",
  "name": "515c2055-d9d4-4321-b1b9-bd0c9a0f79fe",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*/read",
        "Microsoft.Insights/metricAlerts/*/read",
        "Microsoft.Insights/diagnosticSettings/*/read",
        "Microsoft.Insights/metricDefinitions/*/read",
        "Microsoft.Logic/*/read",
        "Microsoft.Logic/workflows/disable/action",
        "Microsoft.Logic/workflows/enable/action",
        "Microsoft.Logic/workflows/validate/action",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/connectionGateways/*/read",
        "Microsoft.Web/connections/*/read",
        "Microsoft.Web/customApis/*/read",
        "Microsoft.Web/serverFarms/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Logic App Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="identity"></a>Identity


### <a name="managed-identity-contributor"></a>Mitwirkender für verwaltete Identität

Dem Benutzer zugewiesene Identität erstellen, lesen, aktualisieren und löschen. [Weitere Informationen](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ManagedIdentity](resource-provider-operations.md#microsoftmanagedidentity)/userAssignedIdentities/read | Ruft eine vorhandene zugewiesene Benutzeridentität ab. |
> | [Microsoft.ManagedIdentity](resource-provider-operations.md#microsoftmanagedidentity)/userAssignedIdentities/write | Erstellt eine neue zugewiesene Benutzeridentität oder aktualisiert die Markierungen, die einer vorhandenen zugewiesenen Benutzeridentität zugeordnet sind. |
> | [Microsoft.ManagedIdentity](resource-provider-operations.md#microsoftmanagedidentity)/userAssignedIdentities/delete | Löscht eine vorhandene zugewiesene Benutzeridentität. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, Read, Update, and Delete User Assigned Identity",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e40ec5ca-96e0-45a2-b4ff-59039f2c2b59",
  "name": "e40ec5ca-96e0-45a2-b4ff-59039f2c2b59",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedIdentity/userAssignedIdentities/read",
        "Microsoft.ManagedIdentity/userAssignedIdentities/write",
        "Microsoft.ManagedIdentity/userAssignedIdentities/delete",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Identity Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-identity-operator"></a>Operator für verwaltete Identität

Dem Benutzer zugewiesene Identität lesen und zuweisen. [Weitere Informationen](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ManagedIdentity](resource-provider-operations.md#microsoftmanagedidentity)/userAssignedIdentities/*/read |  |
> | [Microsoft.ManagedIdentity](resource-provider-operations.md#microsoftmanagedidentity)/userAssignedIdentities/*/assign/action |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read and Assign User Assigned Identity",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f1a07417-d97a-45cb-824c-7a7467783830",
  "name": "f1a07417-d97a-45cb-824c-7a7467783830",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedIdentity/userAssignedIdentities/*/read",
        "Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Identity Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="security"></a>Sicherheit


### <a name="attestation-contributor"></a>Attestation-Mitwirkender

Lesen, Schreiben oder Löschen der Nachweisanbieterinstanz. [Weitere Informationen](../attestation/quickstart-powershell.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | Microsoft.Attestation/attestationProviders/attestation/read |  |
> | Microsoft.Attestation/attestationProviders/attestation/write |  |
> | Microsoft.Attestation/attestationProviders/attestation/delete |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read write or delete the attestation provider instance",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/bbf86eb8-f7b4-4cce-96e4-18cddf81d86e",
  "name": "bbf86eb8-f7b4-4cce-96e4-18cddf81d86e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Attestation/attestationProviders/attestation/read",
        "Microsoft.Attestation/attestationProviders/attestation/write",
        "Microsoft.Attestation/attestationProviders/attestation/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Attestation Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="attestation-reader"></a>Attestation-Leser

Lesen der Eigenschaften des Nachweisanbieters. [Weitere Informationen](../attestation/troubleshoot-guide.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | Microsoft.Attestation/attestationProviders/attestation/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read the attestation provider properties",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fd1bd22b-8476-40bc-a0bc-69b95687b9f3",
  "name": "fd1bd22b-8476-40bc-a0bc-69b95687b9f3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Attestation/attestationProviders/attestation/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Attestation Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-administrator"></a>Key Vault-Administrator

Ausführen beliebiger Vorgänge auf Datenebene für einen Schlüsseltresor und alle darin enthaltenen Objekte (einschließlich Zertifikate, Schlüssel und Geheimnisse). Kann keine Key Vault-Ressourcen oder Rollenzuweisungen verwalten. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. [Weitere Informationen](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/checkNameAvailability/read | Prüft, ob ein Schlüsseltresorname gültig ist und noch nicht verwendet wird. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/deletedVaults/read | Dient zum Anzeigen der Eigenschaften vorläufig gelöschter Schlüsseltresore. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/operations/read | Listet die verfügbaren Vorgänge für den Microsoft.KeyVault-Ressourcenanbieter auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Perform all data plane operations on a key vault and all objects in it, including certificates, keys, and secrets. Cannot manage key vault resources or manage role assignments. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/00482a5a-887f-4fb3-b363-3b7fe8e74483",
  "name": "00482a5a-887f-4fb3-b363-3b7fe8e74483",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.KeyVault/checkNameAvailability/read",
        "Microsoft.KeyVault/deletedVaults/read",
        "Microsoft.KeyVault/locations/*/read",
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/operations/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-certificates-officer"></a>Key Vault-Zertifikatbeauftragter

Ausführen beliebiger Aktionen für die Zertifikate eines Schlüsseltresors mit Ausnahme der Verwaltung von Berechtigungen. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. [Weitere Informationen](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/checkNameAvailability/read | Prüft, ob ein Schlüsseltresorname gültig ist und noch nicht verwendet wird. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/deletedVaults/read | Dient zum Anzeigen der Eigenschaften vorläufig gelöschter Schlüsseltresore. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/operations/read | Listet die verfügbaren Vorgänge für den Microsoft.KeyVault-Ressourcenanbieter auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/certificatecas/* |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/certificates/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Perform any action on the certificates of a key vault, except manage permissions. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a4417e6f-fecd-4de8-b567-7b0420556985",
  "name": "a4417e6f-fecd-4de8-b567-7b0420556985",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.KeyVault/checkNameAvailability/read",
        "Microsoft.KeyVault/deletedVaults/read",
        "Microsoft.KeyVault/locations/*/read",
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/operations/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/certificatecas/*",
        "Microsoft.KeyVault/vaults/certificates/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Certificates Officer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-contributor"></a>Key Vault-Mitwirkender

Verwalten von Schlüsseltresoren, gestattet Ihnen jedoch nicht, Rollen in Azure RBAC zuzuweisen, und ermöglicht keinen Zugriff auf Geheimnisse, Schlüssel oder Zertifikate. [Weitere Informationen](../key-vault/general/security-features.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/deletedVaults/purge/action | Dient zum endgültigen Löschen eines vorläufig gelöschten Schlüsseltresors. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/hsmPools/* |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/managedHsms/* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage key vaults, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f25e0fa2-a7c8-4377-a976-54943a77a395",
  "name": "f25e0fa2-a7c8-4377-a976-54943a77a395",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.KeyVault/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.KeyVault/locations/deletedVaults/purge/action",
        "Microsoft.KeyVault/hsmPools/*",
        "Microsoft.KeyVault/managedHsms/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-crypto-officer"></a>Key Vault-Kryptografiebeauftragter

Ausführen beliebiger Aktionen für die Schlüssel eines Schlüsseltresors mit Ausnahme der Verwaltung von Berechtigungen. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. [Weitere Informationen](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/checkNameAvailability/read | Prüft, ob ein Schlüsseltresorname gültig ist und noch nicht verwendet wird. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/deletedVaults/read | Dient zum Anzeigen der Eigenschaften vorläufig gelöschter Schlüsseltresore. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/operations/read | Listet die verfügbaren Vorgänge für den Microsoft.KeyVault-Ressourcenanbieter auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Perform any action on the keys of a key vault, except manage permissions. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/14b46e9e-c2b7-41b4-b07b-48a6ebf60603",
  "name": "14b46e9e-c2b7-41b4-b07b-48a6ebf60603",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.KeyVault/checkNameAvailability/read",
        "Microsoft.KeyVault/deletedVaults/read",
        "Microsoft.KeyVault/locations/*/read",
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/operations/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/keys/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Crypto Officer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-crypto-service-encryption-user"></a>Key Vault Crypto Service Encryption-Benutzer

Lesen von Metadaten von Schlüsseln und Ausführen von Vorgängen zum Packen/Entpacken. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. [Weitere Informationen](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Aktionen | Beschreibung |
> | --- | --- |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/write | Erstellt oder aktualisiert eventSubscription. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/read | Liest eventSubscription. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/delete | Löscht eventSubscription. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/read | Listet Schlüssel im angegebenen Tresor auf oder liest Eigenschaften und öffentliche Informationen eines Schlüssels. Bei asymmetrischen Schlüsseln legt dieser Vorgang den öffentlichen Schlüssel offen und beinhaltet die Fähigkeit, Algorithmen für öffentliche Schlüssel durchzuführen, z. B. Verschlüsselung und Überprüfung der Signatur. Private Schlüssel und symmetrische Schlüssel werden nie offengelegt. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/wrap/action | Umschließt einen symmetrischen Schlüssel mit einem Key Vault-Schlüssel. Hinweis: Wenn es sich um einen asymmetrischen Key Vault-Schlüssel handelt, kann dieser Vorgang mit einem Prinzipal mit Lesezugriff durchgeführt werden. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/unwrap/action | Hebt die Umschließung eines symmetrischen Schlüssels mit einem Key Vault-Schlüssel auf. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read metadata of keys and perform wrap/unwrap operations. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e147488a-f6f5-4113-8e2d-b22465e65bf6",
  "name": "e147488a-f6f5-4113-8e2d-b22465e65bf6",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventGrid/eventSubscriptions/write",
        "Microsoft.EventGrid/eventSubscriptions/read",
        "Microsoft.EventGrid/eventSubscriptions/delete"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/keys/read",
        "Microsoft.KeyVault/vaults/keys/wrap/action",
        "Microsoft.KeyVault/vaults/keys/unwrap/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Crypto Service Encryption User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-crypto-user"></a>Key Vault-Kryptografiebenutzer

Ausführen kryptografischer Vorgänge mithilfe von Schlüsseln. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. [Weitere Informationen](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/read | Listet Schlüssel im angegebenen Tresor auf oder liest Eigenschaften und öffentliche Informationen eines Schlüssels. Bei asymmetrischen Schlüsseln legt dieser Vorgang den öffentlichen Schlüssel offen und beinhaltet die Fähigkeit, Algorithmen für öffentliche Schlüssel durchzuführen, z. B. Verschlüsselung und Überprüfung der Signatur. Private Schlüssel und symmetrische Schlüssel werden nie offengelegt. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/update/action | Aktualisiert die angegebenen Attribute, die dem jeweiligen Schlüssel zugeordnet sind. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/backup/action | Erstellt die Sicherungsdatei eines Schlüssels. Die Datei kann verwendet werden, um den Schlüssel in einem Key Vault desselben Abonnements wiederherzustellen. Es können Einschränkungen gelten. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/encrypt/action | Verschlüsselt Klartext mit einem Schlüssel. Hinweis: Wenn es sich um einen asymmetrischen Schlüssel handelt, kann dieser Vorgang mit einem Prinzipal mit Lesezugriff durchgeführt werden. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/decrypt/action | Entschlüsselt Chiffretext mit einem Schlüssel. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/wrap/action | Umschließt einen symmetrischen Schlüssel mit einem Key Vault-Schlüssel. Hinweis: Wenn es sich um einen asymmetrischen Key Vault-Schlüssel handelt, kann dieser Vorgang mit einem Prinzipal mit Lesezugriff durchgeführt werden. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/unwrap/action | Hebt die Umschließung eines symmetrischen Schlüssels mit einem Key Vault-Schlüssel auf. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/sign/action | Signiert einen Nachrichtenhash (Hash) mit einem Schlüssel. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/verify/action | Überprüft die Signatur eines Nachrichtenhashs (Hash) mit einem Schlüssel. Hinweis: Wenn es sich um einen asymmetrischen Schlüssel handelt, kann dieser Vorgang mit einem Prinzipal mit Lesezugriff durchgeführt werden. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Perform cryptographic operations using keys. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/12338af0-0e69-4776-bea7-57ae8d297424",
  "name": "12338af0-0e69-4776-bea7-57ae8d297424",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/keys/read",
        "Microsoft.KeyVault/vaults/keys/update/action",
        "Microsoft.KeyVault/vaults/keys/backup/action",
        "Microsoft.KeyVault/vaults/keys/encrypt/action",
        "Microsoft.KeyVault/vaults/keys/decrypt/action",
        "Microsoft.KeyVault/vaults/keys/wrap/action",
        "Microsoft.KeyVault/vaults/keys/unwrap/action",
        "Microsoft.KeyVault/vaults/keys/sign/action",
        "Microsoft.KeyVault/vaults/keys/verify/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Crypto User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-reader"></a>Key Vault-Leser

Lesen von Metadaten von Schlüsseltresoren und deren Zertifikaten, Schlüsseln und Geheimnissen. Sensible Werte, z. B. der Inhalt von Geheimnissen oder Schlüsselmaterial, können nicht gelesen werden. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. [Weitere Informationen](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/checkNameAvailability/read | Prüft, ob ein Schlüsseltresorname gültig ist und noch nicht verwendet wird. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/deletedVaults/read | Dient zum Anzeigen der Eigenschaften vorläufig gelöschter Schlüsseltresore. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/operations/read | Listet die verfügbaren Vorgänge für den Microsoft.KeyVault-Ressourcenanbieter auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/secrets/readMetadata/action | Listet die Eigenschaften eines Geheimnisses auf oder zeigt sie an, nicht aber dessen Wert. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read metadata of key vaults and its certificates, keys, and secrets. Cannot read sensitive values such as secret contents or key material. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/21090545-7ca7-4776-b22c-e363652d74d2",
  "name": "21090545-7ca7-4776-b22c-e363652d74d2",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.KeyVault/checkNameAvailability/read",
        "Microsoft.KeyVault/deletedVaults/read",
        "Microsoft.KeyVault/locations/*/read",
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/operations/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/vaults/secrets/readMetadata/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-secrets-officer"></a>Key Vault-Geheimnisbeauftragter

Ausführen beliebiger Aktionen für die Geheimnisse eines Schlüsseltresors mit Ausnahme der Verwaltung von Berechtigungen. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. [Weitere Informationen](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/checkNameAvailability/read | Prüft, ob ein Schlüsseltresorname gültig ist und noch nicht verwendet wird. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/deletedVaults/read | Dient zum Anzeigen der Eigenschaften vorläufig gelöschter Schlüsseltresore. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/operations/read | Listet die verfügbaren Vorgänge für den Microsoft.KeyVault-Ressourcenanbieter auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/secrets/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Perform any action on the secrets of a key vault, except manage permissions. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b86a8fe4-44ce-4948-aee5-eccb2c155cd7",
  "name": "b86a8fe4-44ce-4948-aee5-eccb2c155cd7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.KeyVault/checkNameAvailability/read",
        "Microsoft.KeyVault/deletedVaults/read",
        "Microsoft.KeyVault/locations/*/read",
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/operations/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/secrets/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Secrets Officer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-secrets-user"></a>Key Vault-Geheimnisbenutzer

Lesen der Inhalte von Geheimnissen. Funktioniert nur für Schlüsseltresore, die das Berechtigungsmodell „Rollenbasierte Azure-Zugriffssteuerung“ verwenden. [Weitere Informationen](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/secrets/getSecret/action | Ruft den Wert eines Geheimnisses ab. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/secrets/readMetadata/action | Listet die Eigenschaften eines Geheimnisses auf oder zeigt sie an, nicht aber dessen Wert. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read secret contents. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4633458b-17de-408a-b874-0445c86b69e6",
  "name": "4633458b-17de-408a-b874-0445c86b69e6",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/secrets/getSecret/action",
        "Microsoft.KeyVault/vaults/secrets/readMetadata/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Secrets User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-hsm-contributor"></a>Mitwirkender für verwaltete HSMs

Ermöglicht Ihnen das Verwalten von verwalteten HSM-Pools, aber nicht den Zugriff auf diese. [Weitere Informationen](../key-vault/managed-hsm/secure-your-managed-hsm.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/managedHSMs/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage managed HSM pools, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/18500a29-7fe2-46b2-a342-b16a415e101d",
  "name": "18500a29-7fe2-46b2-a342-b16a415e101d",
  "permissions": [
    {
      "actions": [
        "Microsoft.KeyVault/managedHSMs/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed HSM contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="microsoft-sentinel-automation-contributor"></a>Microsoft Sentinel Automation-Mitarbeiter

Microsoft Sentinel Automation-Mitarbeiter [Mehr erfahren](../sentinel/roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/triggers/read | Liest den Trigger. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/triggers/listCallbackUrl/action | Ruft die Rückruf-URL für Trigger ab. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/runs/read | Liest die Workflowausführung. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Microsoft Sentinel Automation Contributor",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f4c81013-99ee-4d62-a7ee-b3f1f648599a",
  "name": "f4c81013-99ee-4d62-a7ee-b3f1f648599a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Logic/workflows/triggers/read",
        "Microsoft.Logic/workflows/triggers/listCallbackUrl/action",
        "Microsoft.Logic/workflows/runs/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Microsoft Sentinel Automation Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="microsoft-sentinel-contributor"></a>Microsoft Sentinel-Mitwirkender

Microsoft Sentinel-Mitwirkender [Mehr erfahren](../sentinel/roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/* |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/savedSearches/* |  |
> | [Microsoft.OperationsManagement](resource-provider-operations.md#microsoftoperationsmanagement)/solutions/read | Dient zum Abrufen vorhandener OMS-Lösungen. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/read | Dient zum Ausführen von Abfragen für die Daten im Arbeitsbereich. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/*/read |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/dataSources/read | Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/querypacks/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/myworkbooks/read | Liest eine private Arbeitsmappe |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Microsoft Sentinel Contributor",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ab8e14d6-4a74-4a29-9ba8-549422addade",
  "name": "ab8e14d6-4a74-4a29-9ba8-549422addade",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/*",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.OperationalInsights/querypacks/*/read",
        "Microsoft.Insights/workbooks/*",
        "Microsoft.Insights/myworkbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Microsoft Sentinel Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="microsoft-sentinel-reader"></a>Microsoft Sentinel Reader

Microsoft Sentinel Reader [Mehr erfahren](../sentinel/roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/*/read |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/dataConnectorsCheckRequirements/action | Überprüft Benutzerautorisierung und -lizenz. |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/indicators/query/action | Abfragen von Threat Intelligence-Indikatoren |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/queryIndicators/action | Abfragen von Threat Intelligence-Indikatoren |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/LinkedServices/read | Ruft verknüpfte Dienste im angegebenen Arbeitsbereich ab. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/savedSearches/read | Ruft eine gespeicherte Suchabfrage ab. |
> | [Microsoft.OperationsManagement](resource-provider-operations.md#microsoftoperationsmanagement)/solutions/read | Dient zum Abrufen vorhandener OMS-Lösungen. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/read | Dient zum Ausführen von Abfragen für die Daten im Arbeitsbereich. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/*/read |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/querypacks/*/read |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/dataSources/read | Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/read | Lesen einer Arbeitsmappe |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/myworkbooks/read | Liest eine private Arbeitsmappe |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Microsoft Sentinel Reader",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8d289c81-5878-46d4-8554-54e1e3d8b5cb",
  "name": "8d289c81-5878-46d4-8554-54e1e3d8b5cb",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*/read",
        "Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action",
        "Microsoft.SecurityInsights/threatIntelligence/indicators/query/action",
        "Microsoft.SecurityInsights/threatIntelligence/queryIndicators/action",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/LinkedServices/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/read",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/querypacks/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.Insights/workbooks/read",
        "Microsoft.Insights/myworkbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Microsoft Sentinel Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="microsoft-sentinel-responder"></a>Microsoft Sentinel Responder

Microsoft Sentinel Responder [Mehr erfahren](../sentinel/roles.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/*/read |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/dataConnectorsCheckRequirements/action | Überprüft Benutzerautorisierung und -lizenz. |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/automationRules/* |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/cases/* |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/incidents/* |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/indicators/appendTags/action | Anfügen von Tags an Threat Intelligence-Indikator |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/indicators/query/action | Abfragen von Threat Intelligence-Indikatoren |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/bulkTag/action | Kennzeichnet Informationen zu Bedrohungen in einem Massenvorgang. |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/indicators/appendTags/action | Anfügen von Tags an Threat Intelligence-Indikator |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/indicators/replaceTags/action | Ersetzen von Tags eines Threat Intelligence-Indikators |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/queryIndicators/action | Abfragen von Threat Intelligence-Indikatoren |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/analytics/query/action | Führt eine Suche mit der neuen Engine aus. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/dataSources/read | Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/savedSearches/read | Ruft eine gespeicherte Suchabfrage ab. |
> | [Microsoft.OperationsManagement](resource-provider-operations.md#microsoftoperationsmanagement)/solutions/read | Dient zum Abrufen vorhandener OMS-Lösungen. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/read | Dient zum Ausführen von Abfragen für die Daten im Arbeitsbereich. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/*/read |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/dataSources/read | Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/querypacks/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/read | Lesen einer Arbeitsmappe |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/myworkbooks/read | Liest eine private Arbeitsmappe |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/cases/*/Delete |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/incidents/*/Delete |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Microsoft Sentinel Responder",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3e150937-b8fe-4cfb-8069-0eaf05ecd056",
  "name": "3e150937-b8fe-4cfb-8069-0eaf05ecd056",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*/read",
        "Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action",
        "Microsoft.SecurityInsights/automationRules/*",
        "Microsoft.SecurityInsights/cases/*",
        "Microsoft.SecurityInsights/incidents/*",
        "Microsoft.SecurityInsights/threatIntelligence/indicators/appendTags/action",
        "Microsoft.SecurityInsights/threatIntelligence/indicators/query/action",
        "Microsoft.SecurityInsights/threatIntelligence/bulkTag/action",
        "Microsoft.SecurityInsights/threatIntelligence/indicators/appendTags/action",
        "Microsoft.SecurityInsights/threatIntelligence/indicators/replaceTags/action",
        "Microsoft.SecurityInsights/threatIntelligence/queryIndicators/action",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/read",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.OperationalInsights/querypacks/*/read",
        "Microsoft.Insights/workbooks/read",
        "Microsoft.Insights/myworkbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.SecurityInsights/cases/*/Delete",
        "Microsoft.SecurityInsights/incidents/*/Delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Microsoft Sentinel Responder",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-admin"></a>Sicherheitsadministrator

Anzeigen und Aktualisieren von Berechtigungen für Security Center. Gleiche Rechte wie der Sicherheitsleseberechtigte und kann darüber hinaus die Sicherheitsrichtlinie aktualisieren sowie Warnungen und Empfehlungen verwerfen. [Weitere Informationen](../security-center/security-center-permissions.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyAssignments/* | Erstellen und Verwalten von Richtlinienzuweisungen |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyDefinitions/* | Erstellen und Verwalten von Richtliniendefinitionen |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyExemptions/* | Erstellen und Verwalten von Richtlinienausnahmen |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policySetDefinitions/* | Erstellen und Verwalten von Richtliniensätzen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | [Microsoft.operationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/* | Erstellen und Verwalten von Sicherheitskomponenten und -richtlinien |
> | [Microsoft.IoTSecurity](resource-provider-operations.md#microsoftiotsecurity)/* |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | [Microsoft.IoTSecurity](resource-provider-operations.md#microsoftiotsecurity)/defenderSettings/write | Erstellt oder aktualisiert IoT Defender-Einstellungen. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Security Admin Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fb1c8493-542b-48eb-b624-b4c8fea62acd",
  "name": "fb1c8493-542b-48eb-b624-b4c8fea62acd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Authorization/policyAssignments/*",
        "Microsoft.Authorization/policyDefinitions/*",
        "Microsoft.Authorization/policyExemptions/*",
        "Microsoft.Authorization/policySetDefinitions/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.operationalInsights/workspaces/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*",
        "Microsoft.IoTSecurity/*",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.IoTSecurity/defenderSettings/write"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-assessment-contributor"></a>Mitwirkender für Sicherheitsbewertungen

Ermöglicht das Pushen von Bewertungen an Security Center

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/assessments/write | Erstellen oder Aktualisieren von Sicherheitsbewertungen für Ihr Abonnement |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you push assessments to Security Center",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/612c2aa1-cb24-443b-ac28-3ab7272de6f5",
  "name": "612c2aa1-cb24-443b-ac28-3ab7272de6f5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Security/assessments/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Assessment Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-manager-legacy"></a>Sicherheits-Manager (Legacy)

Dies ist eine Legacyrolle. Verwenden Sie stattdessen „Sicherheitsadministrator“.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.ClassicCompute](resource-provider-operations.md#microsoftclassiccompute)/*/read | Lesen von Konfigurationsinformationen zu klassischen virtuellen Computern |
> | [Microsoft.ClassicCompute](resource-provider-operations.md#microsoftclassiccompute)/virtualMachines/*/write | Schreiben der Konfiguration für klassische virtuelle Computer |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/*/read | Lesen von Konfigurationsinformationen zu klassischem Netzwerk |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/* | Erstellen und Verwalten von Sicherheitskomponenten und -richtlinien |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "This is a legacy role. Please use Security Administrator instead",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e3d13bf0-dd5a-482e-ba6b-9b8433878d10",
  "name": "e3d13bf0-dd5a-482e-ba6b-9b8433878d10",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicCompute/*/read",
        "Microsoft.ClassicCompute/virtualMachines/*/write",
        "Microsoft.ClassicNetwork/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Manager (Legacy)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-reader"></a>Sicherheitsleseberechtigter

Anzeigen von Berechtigungen für Security Center. Kann Empfehlungen, Warnungen, Sicherheitsrichtlinien und Sicherheitszustände anzeigen, jedoch keine Änderungen vornehmen. [Weitere Informationen](../security-center/security-center-permissions.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Liest eine klassische Metrikwarnung. |
> | [Microsoft.operationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Anzeigen von Log Analytics-Daten |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/*/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/*/read | Lesen von Sicherheitskomponenten und -richtlinien |
> | [Microsoft.IoTSecurity](resource-provider-operations.md#microsoftiotsecurity)/*/read |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/*/read |  |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/iotDefenderSettings/packageDownloads/action | Ruft Informationen zu herunterladbaren IoT Defender-Paketen ab. |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/iotDefenderSettings/downloadManagerActivation/action | Lädt die Manager-Aktivierungsdatei mit Daten zum Abonnementkontingent herunter. |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/iotSensors/downloadResetPassword/action | Lädt die Dateien zum Zurücksetzen des Kennworts für IoT-Sensoren herunter. |
> | [Microsoft.IoTSecurity](resource-provider-operations.md#microsoftiotsecurity)/defenderSettings/packageDownloads/action | Ruft Informationen zu herunterladbaren IoT Defender-Paketen ab. |
> | [Microsoft.IoTSecurity](resource-provider-operations.md#microsoftiotsecurity)/defenderSettings/downloadManagerActivation/action | Lädt die Manager-Aktivierungsdatei herunter. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Security Reader Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/39bc4728-0917-49c7-9d2c-d95423bc2eb4",
  "name": "39bc4728-0917-49c7-9d2c-d95423bc2eb4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.operationalInsights/workspaces/*/read",
        "Microsoft.Resources/deployments/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*/read",
        "Microsoft.IoTSecurity/*/read",
        "Microsoft.Support/*/read",
        "Microsoft.Security/iotDefenderSettings/packageDownloads/action",
        "Microsoft.Security/iotDefenderSettings/downloadManagerActivation/action",
        "Microsoft.Security/iotSensors/downloadResetPassword/action",
        "Microsoft.IoTSecurity/defenderSettings/packageDownloads/action",
        "Microsoft.IoTSecurity/defenderSettings/downloadManagerActivation/action",
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="devops"></a>DevOps


### <a name="devtest-labs-user"></a>DevTest Labs-Benutzer

Ermöglicht Ihnen das Verbinden, Starten, Neustarten und Herunterfahren Ihrer virtuellen Computer in Ihren Azure DevTest Labs. [Weitere Informationen](../devtest-labs/devtest-lab-add-devtest-user.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/availabilitySets/read | Dient zum Abrufen der Eigenschaften einer Verfügbarkeitsgruppe. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/*/read | Lesen der Eigenschaften eines virtuellen Computers (VM-Größen, Laufzeitstatus, VM-Erweiterungen usw.) |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/deallocate/action | Schaltet den virtuellen Computer aus und gibt die Computeressourcen frei. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/read | Dient zum Abrufen der Eigenschaften eines virtuellen Computers. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/restart/action | Startet den virtuellen Computer neu. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/start/action | Startet den virtuellen Computer. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/*/read | Lesen der Eigenschaften eines Labs |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/claimAnyVm/action | Dient zum Anfordern eines beliebigen anforderbaren virtuellen Computers im Lab. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/createEnvironment/action | Dient zum Erstellen von virtuellen Computern in einem Lab. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/ensureCurrentUserProfile/action | Stellt sicher, dass der aktuelle Benutzer über ein gültiges Profil im Lab verfügt |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/formulas/delete | Dient zum Löschen von Formeln. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/formulas/read | Dient zum Lesen von Formeln. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/formulas/write | Dient zum Hinzufügen oder Ändern von Formeln. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/policySets/evaluatePolicies/action | Wertet Labrichtlinien aus. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/virtualMachines/claim/action | Dient dazu, einen vorhandenen virtuellen Computer in Besitz zu nehmen. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/virtualmachines/listApplicableSchedules/action | Listet die anwendbaren Zeitpläne zum Starten/Beenden auf, sofern vorhanden. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/virtualMachines/getRdpFileContents/action | Ruft eine Zeichenfolge ab, die den Inhalt der RDP-Datei für den virtuellen Computer darstellt. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/backendAddressPools/join/action | Verknüpft einen Back-End-Adresspool für den Lastenausgleich. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/inboundNatRules/join/action | Verknüpft eine NAT-Regel für eingehenden Datenverkehr für den Lastenausgleich. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/*/read | Lesen der Eigenschaften einer Netzwerkschnittstelle (z.B. alle Load Balancer, zu denen die Netzwerkschnittstelle gehört) |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/join/action | Verknüpft einen virtuellen Computer mit einer Netzwerkschnittstelle. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/read | Ruft eine Netzwerkschnittstellendefinition ab.  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/write | Erstellt eine Netzwerkschnittstelle oder aktualisiert eine vorhandene Netzwerkschnittstelle.  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/*/read | Lesen der Eigenschaften einer öffentlichen IP-Adresse |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/join/action | Verknüpft eine öffentliche IP-Adresse. Nicht warnbar. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/read | Ruft eine Definition für eine öffentliche IP-Adresse ab. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/join/action | Verknüpft ein virtuelles Netzwerk. Nicht warnbar. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/operations/read | Ruft Bereitstellungsvorgänge ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/read | Ruft Bereitstellungen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listKeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | **NotActions** |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/vmSizes/read | Listet die verfügbaren Größen auf, auf die der virtuelle Computer aktualisiert werden kann. |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you connect, start, restart, and shutdown your virtual machines in your Azure DevTest Labs.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/76283e04-6283-4c54-8f91-bcf1374a3c64",
  "name": "76283e04-6283-4c54-8f91-bcf1374a3c64",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/availabilitySets/read",
        "Microsoft.Compute/virtualMachines/*/read",
        "Microsoft.Compute/virtualMachines/deallocate/action",
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Compute/virtualMachines/restart/action",
        "Microsoft.Compute/virtualMachines/start/action",
        "Microsoft.DevTestLab/*/read",
        "Microsoft.DevTestLab/labs/claimAnyVm/action",
        "Microsoft.DevTestLab/labs/createEnvironment/action",
        "Microsoft.DevTestLab/labs/ensureCurrentUserProfile/action",
        "Microsoft.DevTestLab/labs/formulas/delete",
        "Microsoft.DevTestLab/labs/formulas/read",
        "Microsoft.DevTestLab/labs/formulas/write",
        "Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action",
        "Microsoft.DevTestLab/labs/virtualMachines/claim/action",
        "Microsoft.DevTestLab/labs/virtualmachines/listApplicableSchedules/action",
        "Microsoft.DevTestLab/labs/virtualMachines/getRdpFileContents/action",
        "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
        "Microsoft.Network/networkInterfaces/*/read",
        "Microsoft.Network/networkInterfaces/join/action",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/publicIPAddresses/*/read",
        "Microsoft.Network/publicIPAddresses/join/action",
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/listKeys/action"
      ],
      "notActions": [
        "Microsoft.Compute/virtualMachines/vmSizes/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DevTest Labs User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="lab-creator"></a>Lab-Ersteller

Ermöglicht Ihnen das Erstellen neuer Labs unter ihren Azure Lab-Konten. [Weitere Informationen](../lab-services/add-lab-creator.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labAccounts/*/read |  |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labAccounts/createLab/action | Hiermit wird ein Lab in einem Lab-Konto erstellt. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labAccounts/getPricingAndAvailability/action | Ruft die Preise und die Verfügbarkeit von Kombinationen aus Größen, Regionen und Betriebssystemen für das Labkonto ab. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labAccounts/getRestrictionsAndUsage/action | Ruft Kerneinschränkungen und Nutzungsdaten für dieses Abonnement ab |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labPlans/images/read | Ruft die Eigenschaften eines Images ab. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labPlans/read | Ruft die Eigenschaften eines Lab-Plans ab. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labPlans/saveImage/action | Erstellt ein Image aus einem virtuellen Computer in dem an den Lab-Plan angefügten Katalog. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labs/read | Ruft die Eigenschaften eines Labs ab. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labs/schedules/read | Ruft die Eigenschaften eines Zeitplans ab. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labs/users/read | Ruft die Eigenschaften eines Benutzers ab. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labs/virtualMachines/read | Ruft die Eigenschaften eines virtuellen Computers ab. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/locations/usages/read | Nutzung an einem Ort abrufen |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/skus/read | Ruft die Eigenschaften einer Lab-Dienst-SKU ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labPlans/createLab/action | Erstellt ein neues Lab aus einem Lab-Plan. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create new labs under your Azure Lab Accounts.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b97fb8bc-a8b2-4522-a38b-dd33c7e65ead",
  "name": "b97fb8bc-a8b2-4522-a38b-dd33c7e65ead",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.LabServices/labAccounts/*/read",
        "Microsoft.LabServices/labAccounts/createLab/action",
        "Microsoft.LabServices/labAccounts/getPricingAndAvailability/action",
        "Microsoft.LabServices/labAccounts/getRestrictionsAndUsage/action",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.LabServices/labPlans/images/read",
        "Microsoft.LabServices/labPlans/read",
        "Microsoft.LabServices/labPlans/saveImage/action",
        "Microsoft.LabServices/labs/read",
        "Microsoft.LabServices/labs/schedules/read",
        "Microsoft.LabServices/labs/users/read",
        "Microsoft.LabServices/labs/virtualMachines/read",
        "Microsoft.LabServices/locations/usages/read",
        "Microsoft.LabServices/skus/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.LabServices/labPlans/createLab/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Lab Creator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="monitor"></a>Überwachen


### <a name="application-insights-component-contributor"></a>Mitwirkender der Application Insights-Komponente

Kann Application Insights-Komponenten verwalten [Weitere Informationen](../azure-monitor/app/resources-roles-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten von klassischen Netzwerkregeln |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/generateLiveToken/read | Abrufen von Token für Livemetriken |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricAlerts/* | Erstellen und Verwalten von neuen Netzwerkregeln |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/* | Erstellen und Verwalten von Insights-Komponenten |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/scheduledqueryrules/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/topology/read | Lesen der Topologie |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/transactions/read | Lesen von Transaktionen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/webtests/* | Erstellen und Verwalten von Insights-Webtests |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage Application Insights components",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e",
  "name": "ae349356-3a1b-4a5e-921d-050484c6347e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/generateLiveToken/read",
        "Microsoft.Insights/metricAlerts/*",
        "Microsoft.Insights/components/*",
        "Microsoft.Insights/scheduledqueryrules/*",
        "Microsoft.Insights/topology/read",
        "Microsoft.Insights/transactions/read",
        "Microsoft.Insights/webtests/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Application Insights Component Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="application-insights-snapshot-debugger"></a>Application Insights-Momentaufnahmedebugger

Gibt dem Benutzer die Berechtigung zum Anzeigen und Herunterladen von Debugmomentaufnahmen, die mit dem Application Insights-Momentaufnahmedebugger erfasst wurden. Beachten Sie, dass diese Berechtigungen in der Rolle [Besitzer](#owner) oder [Mitwirkender](#contributor) nicht enthalten sind. Die Application Insights-Rolle „Momentaufnahmedebugger“ muss Benutzern direkt zugewiesen werden. Die Rolle wird nicht erkannt, wenn sie einer benutzerdefinierten Rolle hinzugefügt wird. [Weitere Informationen](../azure-monitor/app/snapshot-debugger.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/*/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives user permission to use Application Insights Snapshot Debugger features",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/08954f03-6346-4c2e-81c0-ec3a5cfae23b",
  "name": "08954f03-6346-4c2e-81c0-ec3a5cfae23b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/components/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Application Insights Snapshot Debugger",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-contributor"></a>Überwachungsmitwirkender

Kann sämtliche Überwachungsdaten lesen und Überwachungseinstellungen bearbeiten. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](../azure-monitor/roles-permissions-security.md#built-in-monitoring-roles). [Weitere Informationen](../azure-monitor/roles-permissions-security.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | [Microsoft.AlertsManagement](resource-provider-operations.md#microsoftalertsmanagement)/alerts/* |  |
> | [Microsoft.AlertsManagement](resource-provider-operations.md#microsoftalertsmanagement)/alertsSummary/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/actiongroups/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/activityLogAlerts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/AlertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/* | Erstellen und Verwalten von Insights-Komponenten |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/dataCollectionEndpoints/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/dataCollectionRules/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/dataCollectionRuleAssociations/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/DiagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/eventtypes/* | Auflisten von Aktivitätsprotokollereignissen (Verwaltungsereignissen) in einem Abonnement. Diese Berechtigung gilt sowohl für den programmgesteuerten als auch für den Portalzugriff auf das Aktivitätsprotokoll. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/LogDefinitions/* | Diese Berechtigung ist für Benutzer notwendig, die über das Portal auf Aktivitätsprotokolle zugreifen müssen. Auflisten der Protokollkategorien im Aktivitätsprotokoll. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricalerts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/MetricDefinitions/* | Lesen von Metrikdefinitionen (Liste der verfügbaren Metriktypen für eine Ressource). |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/Metrics/* | Lesen von Metriken für eine Ressource. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/Register/Action | Dient zum Registrieren des Microsoft Insights-Anbieters. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/scheduledqueryrules/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/webtests/* | Erstellen und Verwalten von Insights-Webtests |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/privateLinkScopes/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/privateLinkScopeOperationStatuses/* |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/write | Erstellt einen neuen Arbeitsbereich oder stellt eine Verknüpfung mit einem vorhandenen Arbeitsbereich her, indem die Kunden-ID für den vorhandenen Arbeitsbereich bereitgestellt wird. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/intelligencepacks/* | Lesen/Schreiben/Löschen von Log Analytics-Lösungspaketen. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/savedSearches/* | Lesen/Schreiben/Löschen von gespeicherten Log Analytics-Suchvorgängen. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/search/action | Führt eine Suchabfrage aus. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/sharedKeys/action | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/storageinsightconfigs/* | Lesen/Schreiben/Löschen von Log Analytics-Speicherdetailinformationen. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.WorkloadMonitor](resource-provider-operations.md#microsoftworkloadmonitor)/monitors/* | Erhalten Sie Informationen zu Integritätsmonitoren für Gast-VMs. |
> | [Microsoft.AlertsManagement](resource-provider-operations.md#microsoftalertsmanagement)/smartDetectorAlertRules/* |  |
> | [Microsoft.AlertsManagement](resource-provider-operations.md#microsoftalertsmanagement)/actionRules/* |  |
> | [Microsoft.AlertsManagement](resource-provider-operations.md#microsoftalertsmanagement)/smartGroups/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read all monitoring data and update monitoring settings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/749f88d5-cbae-40b8-bcfc-e573ddc772fa",
  "name": "749f88d5-cbae-40b8-bcfc-e573ddc772fa",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.AlertsManagement/alerts/*",
        "Microsoft.AlertsManagement/alertsSummary/*",
        "Microsoft.Insights/actiongroups/*",
        "Microsoft.Insights/activityLogAlerts/*",
        "Microsoft.Insights/AlertRules/*",
        "Microsoft.Insights/components/*",
        "Microsoft.Insights/dataCollectionEndpoints/*",
        "Microsoft.Insights/dataCollectionRules/*",
        "Microsoft.Insights/dataCollectionRuleAssociations/*",
        "Microsoft.Insights/DiagnosticSettings/*",
        "Microsoft.Insights/eventtypes/*",
        "Microsoft.Insights/LogDefinitions/*",
        "Microsoft.Insights/metricalerts/*",
        "Microsoft.Insights/MetricDefinitions/*",
        "Microsoft.Insights/Metrics/*",
        "Microsoft.Insights/Register/Action",
        "Microsoft.Insights/scheduledqueryrules/*",
        "Microsoft.Insights/webtests/*",
        "Microsoft.Insights/workbooks/*",
        "Microsoft.Insights/privateLinkScopes/*",
        "Microsoft.Insights/privateLinkScopeOperationStatuses/*",
        "Microsoft.OperationalInsights/workspaces/write",
        "Microsoft.OperationalInsights/workspaces/intelligencepacks/*",
        "Microsoft.OperationalInsights/workspaces/savedSearches/*",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.OperationalInsights/workspaces/sharedKeys/action",
        "Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*",
        "Microsoft.Support/*",
        "Microsoft.WorkloadMonitor/monitors/*",
        "Microsoft.AlertsManagement/smartDetectorAlertRules/*",
        "Microsoft.AlertsManagement/actionRules/*",
        "Microsoft.AlertsManagement/smartGroups/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-metrics-publisher"></a>Herausgeber von Überwachungsmetriken

Ermöglicht die Veröffentlichung von Metriken für Azure-Ressourcen. [Weitere Informationen](../azure-monitor/insights/container-insights-update-metrics.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/Register/Action | Dient zum Registrieren des Microsoft Insights-Anbieters. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/Metrics/Write | Hiermit werden Metriken geschrieben. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Enables publishing metrics against Azure resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c390055eb",
  "name": "3913510d-42f4-4e42-8a64-420c390055eb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/Register/Action",
        "Microsoft.Support/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Insights/Metrics/Write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Metrics Publisher",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-reader"></a>Überwachungsleser

Kann alle Überwachungsdaten (Metriken, Protokolle usw.) lesen. Siehe auch [Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor](../azure-monitor/roles-permissions-security.md#built-in-monitoring-roles). [Weitere Informationen](../azure-monitor/roles-permissions-security.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/search/action | Führt eine Suchabfrage aus. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read all monitoring data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/43d0d8ad-25c7-4714-9337-8ba259a9fe05",
  "name": "43d0d8ad-25c7-4714-9337-8ba259a9fe05",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="workbook-contributor"></a>Arbeitsmappenmitwirkender

Kann freigegebene Arbeitsmappen speichern. [Weitere Informationen](../sentinel/tutorial-monitor-your-data.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/write | Erstellen oder Aktualisieren einer Arbeitsmappe |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/delete | Löschen einer Arbeitsmappe |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/read | Lesen einer Arbeitsmappe |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can save shared workbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e8ddcd69-c73f-4f9f-9844-4100522f16ad",
  "name": "e8ddcd69-c73f-4f9f-9844-4100522f16ad",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/workbooks/write",
        "Microsoft.Insights/workbooks/delete",
        "Microsoft.Insights/workbooks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Workbook Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="workbook-reader"></a>Arbeitsmappenleser

Kann Arbeitsmappen lesen. [Weitere Informationen](../sentinel/tutorial-monitor-your-data.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [microsoft.insights](resource-provider-operations.md#microsoftinsights)/workbooks/read | Lesen einer Arbeitsmappe |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read workbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b279062a-9be3-42a0-92ae-8b3cf002ec4d",
  "name": "b279062a-9be3-42a0-92ae-8b3cf002ec4d",
  "permissions": [
    {
      "actions": [
        "microsoft.insights/workbooks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Workbook Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="management--governance"></a>Verwaltung + Governance


### <a name="automation-contributor"></a>Mitwirkender für Automatisierung

Verwalten von Azure Automation-Ressourcen und anderen Ressourcen mit Azure Automation. [Weitere Informationen](../automation/automation-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/* |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/ActionGroups/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/ActivityLogAlerts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/MetricAlerts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/ScheduledQueryRules/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/* | Erstellt, aktualisiert oder liest die Diagnoseeinstellung für den Analysis-Server. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/sharedKeys/action | Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Manage azure automation resources and other resources using azure automation.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f353d9bd-d4a6-484e-a77a-8050b599b867",
  "name": "f353d9bd-d4a6-484e-a77a-8050b599b867",
  "permissions": [
    {
      "actions": [
        "Microsoft.Automation/automationAccounts/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/ActionGroups/*",
        "Microsoft.Insights/ActivityLogAlerts/*",
        "Microsoft.Insights/MetricAlerts/*",
        "Microsoft.Insights/ScheduledQueryRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.OperationalInsights/workspaces/sharedKeys/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-job-operator"></a>Automation-Auftragsoperator

Hiermit werden Aufträge mithilfe von Automation-Runbooks erstellt und verwaltet. [Weitere Informationen](../automation/automation-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/hybridRunbookWorkerGroups/read | Liest Hybrid Runbook Worker-Ressourcen. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/read | Ruft einen Azure Automation-Auftrag ab. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/resume/action | Setzt einen Azure Automation-Auftrag fort. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/stop/action | Beendet einen Azure Automation-Auftrag. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/streams/read | Ruft einen Azure Automation-Auftragsdatenstrom ab. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/suspend/action | Hält einen Azure Automation-Auftrag an. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/write | Erstellt einen Azure Automation-Auftrag. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/output/read | Ruft die Ausgabe eines Auftrags ab. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create and Manage Jobs using Automation Runbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4fe576fe-1146-4730-92eb-48519fa6bf9f",
  "name": "4fe576fe-1146-4730-92eb-48519fa6bf9f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read",
        "Microsoft.Automation/automationAccounts/jobs/read",
        "Microsoft.Automation/automationAccounts/jobs/resume/action",
        "Microsoft.Automation/automationAccounts/jobs/stop/action",
        "Microsoft.Automation/automationAccounts/jobs/streams/read",
        "Microsoft.Automation/automationAccounts/jobs/suspend/action",
        "Microsoft.Automation/automationAccounts/jobs/write",
        "Microsoft.Automation/automationAccounts/jobs/output/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Job Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-operator"></a>Operator für Automation

Automatisierungsoperatoren können Aufträge starten, beenden, anhalten und fortsetzen. [Weitere Informationen](../automation/automation-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/hybridRunbookWorkerGroups/read | Liest Hybrid Runbook Worker-Ressourcen. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/read | Ruft einen Azure Automation-Auftrag ab. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/resume/action | Setzt einen Azure Automation-Auftrag fort. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/stop/action | Beendet einen Azure Automation-Auftrag. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/streams/read | Ruft einen Azure Automation-Auftragsdatenstrom ab. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/suspend/action | Hält einen Azure Automation-Auftrag an. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/write | Erstellt einen Azure Automation-Auftrag. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobSchedules/read | Ruft einen Azure Automation-Auftragszeitplan ab. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobSchedules/write | Erstellt einen Azure Automation-Auftragszeitplan. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/linkedWorkspace/read | Ruft den Arbeitsbereich ab, der mit dem Automation-Konto verknüpft ist. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/read | Ruft ein Azure Automation-Konto ab. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/runbooks/read | Ruft ein Azure Automation-Runbook ab. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/schedules/read | Ruft ein Azure Automation-Zeitplanasset ab. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/schedules/write | Erstellt oder aktualisiert ein Azure Automation-Zeitplanasset. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/output/read | Ruft die Ausgabe eines Auftrags ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Automation Operators are able to start, stop, suspend, and resume jobs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d3881f73-407a-4167-8283-e981cbba0404",
  "name": "d3881f73-407a-4167-8283-e981cbba0404",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read",
        "Microsoft.Automation/automationAccounts/jobs/read",
        "Microsoft.Automation/automationAccounts/jobs/resume/action",
        "Microsoft.Automation/automationAccounts/jobs/stop/action",
        "Microsoft.Automation/automationAccounts/jobs/streams/read",
        "Microsoft.Automation/automationAccounts/jobs/suspend/action",
        "Microsoft.Automation/automationAccounts/jobs/write",
        "Microsoft.Automation/automationAccounts/jobSchedules/read",
        "Microsoft.Automation/automationAccounts/jobSchedules/write",
        "Microsoft.Automation/automationAccounts/linkedWorkspace/read",
        "Microsoft.Automation/automationAccounts/read",
        "Microsoft.Automation/automationAccounts/runbooks/read",
        "Microsoft.Automation/automationAccounts/schedules/read",
        "Microsoft.Automation/automationAccounts/schedules/write",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Automation/automationAccounts/jobs/output/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-runbook-operator"></a>Automation-Runbookoperator

Runbookeigenschaften lesen: Ermöglicht das Erstellen von Runbookaufträgen. [Weitere Informationen](../automation/automation-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/runbooks/read | Ruft ein Azure Automation-Runbook ab. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read Runbook properties - to be able to create Jobs of the runbook.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5fb5aef8-1081-4b8e-bb16-9d5d0385bab5",
  "name": "5fb5aef8-1081-4b8e-bb16-9d5d0385bab5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/runbooks/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Runbook Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-arc-enabled-kubernetes-cluster-user-role"></a>Benutzerrolle für Azure Arc-aktivierte Kubernetes-Cluster

Aktion zum Auflisten der Anmeldeinformationen eines Clusterbenutzers.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/listClusterUserCredentials/action | Listet clusterUser-Anmeldeinformationen auf. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster user credentials action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/00493d72-78f6-4148-b6c5-d3ce8e4799dd",
  "name": "00493d72-78f6-4148-b6c5-d3ce8e4799dd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Kubernetes/connectedClusters/listClusterUserCredentials/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Arc Enabled Kubernetes Cluster User Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-arc-kubernetes-admin"></a>Azure Arc Kubernetes-Administrator

Ermöglicht Ihnen das Verwalten aller Ressourcen unter einem Cluster/Namespace, außer das Aktualisieren oder Löschen von Ressourcenkontingenten und Namespaces. [Weitere Informationen](../azure-arc/kubernetes/azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/controllerrevisions/read | Liest controllerrevisions. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/daemonsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/deployments/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/replicasets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/statefulsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/authorization.k8s.io/localsubjectaccessreviews/write | Schreibt localsubjectaccessreviews. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/autoscaling/horizontalpodautoscalers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/cronjobs/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/jobs/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/configmaps/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/endpoints/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events.k8s.io/events/read | Liest events. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events/read | Liest events. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/daemonsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/deployments/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/ingresses/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/networkpolicies/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/replicasets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/limitranges/read | Liest limitranges. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/namespaces/read | Liest namespaces. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/ingresses/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/networkpolicies/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/persistentvolumeclaims/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/pods/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/policy/poddisruptionbudgets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/rbac.authorization.k8s.io/rolebindings/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/rbac.authorization.k8s.io/roles/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/resourcequotas/read | Liest resourcequotas. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/secrets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/serviceaccounts/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/services/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage all resources under cluster/namespace, except update or delete resource quotas and namespaces.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/dffb1e0c-446f-4dde-a09f-99eb5cc68b96",
  "name": "dffb1e0c-446f-4dde-a09f-99eb5cc68b96",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Kubernetes/connectedClusters/apps/controllerrevisions/read",
        "Microsoft.Kubernetes/connectedClusters/apps/daemonsets/*",
        "Microsoft.Kubernetes/connectedClusters/apps/deployments/*",
        "Microsoft.Kubernetes/connectedClusters/apps/replicasets/*",
        "Microsoft.Kubernetes/connectedClusters/apps/statefulsets/*",
        "Microsoft.Kubernetes/connectedClusters/authorization.k8s.io/localsubjectaccessreviews/write",
        "Microsoft.Kubernetes/connectedClusters/autoscaling/horizontalpodautoscalers/*",
        "Microsoft.Kubernetes/connectedClusters/batch/cronjobs/*",
        "Microsoft.Kubernetes/connectedClusters/batch/jobs/*",
        "Microsoft.Kubernetes/connectedClusters/configmaps/*",
        "Microsoft.Kubernetes/connectedClusters/endpoints/*",
        "Microsoft.Kubernetes/connectedClusters/events.k8s.io/events/read",
        "Microsoft.Kubernetes/connectedClusters/events/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/daemonsets/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/deployments/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/ingresses/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/networkpolicies/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/replicasets/*",
        "Microsoft.Kubernetes/connectedClusters/limitranges/read",
        "Microsoft.Kubernetes/connectedClusters/namespaces/read",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/ingresses/*",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/networkpolicies/*",
        "Microsoft.Kubernetes/connectedClusters/persistentvolumeclaims/*",
        "Microsoft.Kubernetes/connectedClusters/pods/*",
        "Microsoft.Kubernetes/connectedClusters/policy/poddisruptionbudgets/*",
        "Microsoft.Kubernetes/connectedClusters/rbac.authorization.k8s.io/rolebindings/*",
        "Microsoft.Kubernetes/connectedClusters/rbac.authorization.k8s.io/roles/*",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/*",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/*",
        "Microsoft.Kubernetes/connectedClusters/resourcequotas/read",
        "Microsoft.Kubernetes/connectedClusters/secrets/*",
        "Microsoft.Kubernetes/connectedClusters/serviceaccounts/*",
        "Microsoft.Kubernetes/connectedClusters/services/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Arc Kubernetes Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-arc-kubernetes-cluster-admin"></a>Azure Arc Kubernetes-Clusteradministrator

Ermöglicht Ihnen das Verwalten aller Ressourcen im Cluster. [Weitere Informationen](../azure-arc/kubernetes/azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage all resources in the cluster.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8393591c-06b9-48a2-a542-1bd6b377f6a2",
  "name": "8393591c-06b9-48a2-a542-1bd6b377f6a2",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Kubernetes/connectedClusters/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Arc Kubernetes Cluster Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-arc-kubernetes-viewer"></a>Anzeigeberechtigter für Azure Arc Kubernetes

Ermöglicht Ihnen das Anzeigen aller Ressourcen im Cluster/Namespace mit Ausnahme von Geheimnissen. [Weitere Informationen](../azure-arc/kubernetes/azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/controllerrevisions/read | Liest controllerrevisions. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/daemonsets/read | Liest daemonsets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/deployments/read | Liest deployments. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/replicasets/read | Liest replicasets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/statefulsets/read | Liest statefulsets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/autoscaling/horizontalpodautoscalers/read | Liest horizontalpodautoscalers. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/cronjobs/read | Liest cronjobs. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/jobs/read | Liest jobs. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/configmaps/read | Liest configmaps. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/endpoints/read | Liest endpoints. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events.k8s.io/events/read | Liest events. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events/read | Liest events. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/daemonsets/read | Liest daemonsets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/deployments/read | Liest deployments. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/ingresses/read | Liest ingresses. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/networkpolicies/read | Liest networkpolicies. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/replicasets/read | Liest replicasets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/limitranges/read | Liest limitranges. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/namespaces/read | Liest namespaces. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/ingresses/read | Liest ingresses. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/networkpolicies/read | Liest networkpolicies. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/persistentvolumeclaims/read | Liest persistentvolumeclaims. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/pods/read | Liest pods. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/policy/poddisruptionbudgets/read | Liest poddisruptionbudgets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/read | Liest replicationcontrollers. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/read | Liest replicationcontrollers. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/resourcequotas/read | Liest resourcequotas. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/serviceaccounts/read | Liest serviceaccounts. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/services/read | Liest services. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view all resources in cluster/namespace, except secrets.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/63f0a09d-1495-4db4-a681-037d84835eb4",
  "name": "63f0a09d-1495-4db4-a681-037d84835eb4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Kubernetes/connectedClusters/apps/controllerrevisions/read",
        "Microsoft.Kubernetes/connectedClusters/apps/daemonsets/read",
        "Microsoft.Kubernetes/connectedClusters/apps/deployments/read",
        "Microsoft.Kubernetes/connectedClusters/apps/replicasets/read",
        "Microsoft.Kubernetes/connectedClusters/apps/statefulsets/read",
        "Microsoft.Kubernetes/connectedClusters/autoscaling/horizontalpodautoscalers/read",
        "Microsoft.Kubernetes/connectedClusters/batch/cronjobs/read",
        "Microsoft.Kubernetes/connectedClusters/batch/jobs/read",
        "Microsoft.Kubernetes/connectedClusters/configmaps/read",
        "Microsoft.Kubernetes/connectedClusters/endpoints/read",
        "Microsoft.Kubernetes/connectedClusters/events.k8s.io/events/read",
        "Microsoft.Kubernetes/connectedClusters/events/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/daemonsets/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/deployments/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/ingresses/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/networkpolicies/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/replicasets/read",
        "Microsoft.Kubernetes/connectedClusters/limitranges/read",
        "Microsoft.Kubernetes/connectedClusters/namespaces/read",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/ingresses/read",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/networkpolicies/read",
        "Microsoft.Kubernetes/connectedClusters/persistentvolumeclaims/read",
        "Microsoft.Kubernetes/connectedClusters/pods/read",
        "Microsoft.Kubernetes/connectedClusters/policy/poddisruptionbudgets/read",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/read",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/read",
        "Microsoft.Kubernetes/connectedClusters/resourcequotas/read",
        "Microsoft.Kubernetes/connectedClusters/serviceaccounts/read",
        "Microsoft.Kubernetes/connectedClusters/services/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Arc Kubernetes Viewer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-arc-kubernetes-writer"></a>Schreibberechtigter für Azure Arc Kubernetes

Mit dieser Rolle können Sie alle Elemente im Cluster oder Namespace aktualisieren, mit Ausnahme von Rollen (Clusterrollen) und Rollenbindungen (Clusterrollenbindungen). [Weitere Informationen](../azure-arc/kubernetes/azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/controllerrevisions/read | Liest controllerrevisions. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/daemonsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/deployments/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/replicasets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/statefulsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/autoscaling/horizontalpodautoscalers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/cronjobs/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/jobs/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/configmaps/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/endpoints/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events.k8s.io/events/read | Liest events. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events/read | Liest events. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/daemonsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/deployments/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/ingresses/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/networkpolicies/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/replicasets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/limitranges/read | Liest limitranges. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/namespaces/read | Liest namespaces. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/ingresses/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/networkpolicies/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/persistentvolumeclaims/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/pods/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/policy/poddisruptionbudgets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/resourcequotas/read | Liest resourcequotas. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/secrets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/serviceaccounts/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/services/* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you update everything in cluster/namespace, except (cluster)roles and (cluster)role bindings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5b999177-9696-4545-85c7-50de3797e5a1",
  "name": "5b999177-9696-4545-85c7-50de3797e5a1",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Kubernetes/connectedClusters/apps/controllerrevisions/read",
        "Microsoft.Kubernetes/connectedClusters/apps/daemonsets/*",
        "Microsoft.Kubernetes/connectedClusters/apps/deployments/*",
        "Microsoft.Kubernetes/connectedClusters/apps/replicasets/*",
        "Microsoft.Kubernetes/connectedClusters/apps/statefulsets/*",
        "Microsoft.Kubernetes/connectedClusters/autoscaling/horizontalpodautoscalers/*",
        "Microsoft.Kubernetes/connectedClusters/batch/cronjobs/*",
        "Microsoft.Kubernetes/connectedClusters/batch/jobs/*",
        "Microsoft.Kubernetes/connectedClusters/configmaps/*",
        "Microsoft.Kubernetes/connectedClusters/endpoints/*",
        "Microsoft.Kubernetes/connectedClusters/events.k8s.io/events/read",
        "Microsoft.Kubernetes/connectedClusters/events/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/daemonsets/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/deployments/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/ingresses/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/networkpolicies/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/replicasets/*",
        "Microsoft.Kubernetes/connectedClusters/limitranges/read",
        "Microsoft.Kubernetes/connectedClusters/namespaces/read",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/ingresses/*",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/networkpolicies/*",
        "Microsoft.Kubernetes/connectedClusters/persistentvolumeclaims/*",
        "Microsoft.Kubernetes/connectedClusters/pods/*",
        "Microsoft.Kubernetes/connectedClusters/policy/poddisruptionbudgets/*",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/*",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/*",
        "Microsoft.Kubernetes/connectedClusters/resourcequotas/read",
        "Microsoft.Kubernetes/connectedClusters/secrets/*",
        "Microsoft.Kubernetes/connectedClusters/serviceaccounts/*",
        "Microsoft.Kubernetes/connectedClusters/services/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Arc Kubernetes Writer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-connected-machine-onboarding"></a>Onboarding von Azure Connected Machine

Kann Azure Connected Machine integrieren. [Weitere Informationen](../azure-arc/servers/onboard-service-principal.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/read | Liest einen beliebigen Azure Arc-Computer. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/write | Schreibt auf einem Azure Arc-Computer |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/privateLinkScopes/read | Ermöglicht das Lesen beliebiger Private Link-Bereiche von Azure Arc. |
> | [Microsoft.GuestConfiguration](resource-provider-operations.md#microsoftguestconfiguration)/guestConfigurationAssignments/read | Abrufen der Zuweisung der Gastkonfiguration |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can onboard Azure Connected Machines.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b64e21ea-ac4e-4cdf-9dc9-5b892992bee7",
  "name": "b64e21ea-ac4e-4cdf-9dc9-5b892992bee7",
  "permissions": [
    {
      "actions": [
        "Microsoft.HybridCompute/machines/read",
        "Microsoft.HybridCompute/machines/write",
        "Microsoft.HybridCompute/privateLinkScopes/read",
        "Microsoft.GuestConfiguration/guestConfigurationAssignments/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected Machine Onboarding",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-connected-machine-resource-administrator"></a>Ressourcenadministrator für Azure Connected Machine

Kann Azure Connected Machines lesen, schreiben, löschen und erneut integrieren.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/read | Liest einen beliebigen Azure Arc-Computer. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/write | Schreibt auf einem Azure Arc-Computer |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/delete | Löscht einen Azure Arc-Computer |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/UpgradeExtensions/action | Aktualisiert Erweiterungen auf Azure Arc-Computern. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/extensions/read | Liest alle Azure Arc-Erweiterungen |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/extensions/write | Installiert oder aktualisiert eine Azure Arc-Erweiterung |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/extensions/delete | Löscht eine Azure Arc-Erweiterung |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/privateLinkScopes/* |  |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/*/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read, write, delete and re-onboard Azure Connected Machines.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cd570a14-e51a-42ad-bac8-bafd67325302",
  "name": "cd570a14-e51a-42ad-bac8-bafd67325302",
  "permissions": [
    {
      "actions": [
        "Microsoft.HybridCompute/machines/read",
        "Microsoft.HybridCompute/machines/write",
        "Microsoft.HybridCompute/machines/delete",
        "Microsoft.HybridCompute/machines/UpgradeExtensions/action",
        "Microsoft.HybridCompute/machines/extensions/read",
        "Microsoft.HybridCompute/machines/extensions/write",
        "Microsoft.HybridCompute/machines/extensions/delete",
        "Microsoft.HybridCompute/privateLinkScopes/*",
        "Microsoft.HybridCompute/*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected Machine Resource Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="billing-reader"></a>Abrechnungsleser

Hiermit wird Lesezugriff auf Abrechnungsdaten ermöglicht. [Weitere Informationen](../cost-management-billing/manage/manage-billing-access.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Billing](resource-provider-operations.md#microsoftbilling)/*/read | Lesen von Abrechnungsinformationen |
> | [Microsoft.Commerce](resource-provider-operations.md#microsoftcommerce)/*/read |  |
> | [Microsoft.Consumption](resource-provider-operations.md#microsoftconsumption)/*/read |  |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | [Microsoft.CostManagement](resource-provider-operations.md#microsoftcostmanagement)/*/read |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to billing data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64",
  "name": "fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Billing/*/read",
        "Microsoft.Commerce/*/read",
        "Microsoft.Consumption/*/read",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.CostManagement/*/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Billing Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="blueprint-contributor"></a>Blueprint-Mitwirkender

Kann Blaupausendefinitionen verwalten, aber nicht zuweisen. [Weitere Informationen](../governance/blueprints/overview.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Blueprint](resource-provider-operations.md#microsoftblueprint)/blueprints/* | Erstellen und Verwalten von Blaupausendefinitionen oder -artefakten |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage blueprint definitions, but not assign them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/41077137-e803-4205-871c-5a86e6a753b4",
  "name": "41077137-e803-4205-871c-5a86e6a753b4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Blueprint/blueprints/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Blueprint Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="blueprint-operator"></a>Blueprint-Operator

Kann vorhandene veröffentlichte Blaupausen zuweisen, aber keine neuen Blaupausen erstellen. Beachten Sie, dass dies nur funktioniert, wenn die Zuweisung mit einer vom Benutzer zugewiesenen verwalteten Identität erfolgt. [Weitere Informationen](../governance/blueprints/overview.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Blueprint](resource-provider-operations.md#microsoftblueprint)/blueprintAssignments/* | Erstellen und Verwalten von Blaupausenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can assign existing published blueprints, but cannot create new blueprints. NOTE: this only works if the assignment is done with a user-assigned managed identity.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/437d2ced-4a38-4302-8479-ed2bcb43d090",
  "name": "437d2ced-4a38-4302-8479-ed2bcb43d090",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Blueprint/blueprintAssignments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Blueprint Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cost-management-contributor"></a>Mitwirkender für Cost Management

Ermöglicht Ihnen das Anzeigen der Kosten und das Verwalten der Kostenkonfiguration (z. B. Budgets, Exporte). [Weitere Informationen](../cost-management-billing/costs/understand-work-scopes.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Consumption](resource-provider-operations.md#microsoftconsumption)/* |  |
> | [Microsoft.CostManagement](resource-provider-operations.md#microsoftcostmanagement)/* |  |
> | [Microsoft.Billing](resource-provider-operations.md#microsoftbilling)/billingPeriods/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Advisor](resource-provider-operations.md#microsoftadvisor)/configurations/read | Konfigurationen abrufen |
> | [Microsoft.Advisor](resource-provider-operations.md#microsoftadvisor)/recommendations/read | Liest Empfehlungen. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | [Microsoft.Billing](resource-provider-operations.md#microsoftbilling)/billingProperty/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view costs and manage cost configuration (e.g. budgets, exports)",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/434105ed-43f6-45c7-a02f-909b2ba83430",
  "name": "434105ed-43f6-45c7-a02f-909b2ba83430",
  "permissions": [
    {
      "actions": [
        "Microsoft.Consumption/*",
        "Microsoft.CostManagement/*",
        "Microsoft.Billing/billingPeriods/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Advisor/configurations/read",
        "Microsoft.Advisor/recommendations/read",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.Billing/billingProperty/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cost Management Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cost-management-reader"></a>Cost Management-Leser

Ermöglicht Ihnen das Anzeigen der Kostendaten und -konfiguration (z. B. Budgets, Exporte). [Weitere Informationen](../cost-management-billing/costs/understand-work-scopes.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Consumption](resource-provider-operations.md#microsoftconsumption)/*/read |  |
> | [Microsoft.CostManagement](resource-provider-operations.md#microsoftcostmanagement)/*/read |  |
> | [Microsoft.Billing](resource-provider-operations.md#microsoftbilling)/billingPeriods/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Advisor](resource-provider-operations.md#microsoftadvisor)/configurations/read | Konfigurationen abrufen |
> | [Microsoft.Advisor](resource-provider-operations.md#microsoftadvisor)/recommendations/read | Liest Empfehlungen. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | [Microsoft.Billing](resource-provider-operations.md#microsoftbilling)/billingProperty/read |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view cost data and configuration (e.g. budgets, exports)",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/72fafb9e-0641-4937-9268-a91bfd8191a3",
  "name": "72fafb9e-0641-4937-9268-a91bfd8191a3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Consumption/*/read",
        "Microsoft.CostManagement/*/read",
        "Microsoft.Billing/billingPeriods/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Advisor/configurations/read",
        "Microsoft.Advisor/recommendations/read",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.Billing/billingProperty/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cost Management Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hierarchy-settings-administrator"></a>Hierarchieeinstellungsadministrator

Ermöglicht Benutzern das Bearbeiten und Löschen von Hierarchieeinstellungen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/settings/write | Erstellt oder aktualisiert Hierarchieeinstellungen einer Verwaltungsgruppe. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/settings/delete | Löscht Hierarchieeinstellungen einer Verwaltungsgruppe. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows users to edit and delete Hierarchy Settings",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/350f8d15-c687-4448-8ae1-157740a3936d",
  "name": "350f8d15-c687-4448-8ae1-157740a3936d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/settings/write",
        "Microsoft.Management/managementGroups/settings/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Hierarchy Settings Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="kubernetes-cluster---azure-arc-onboarding"></a>Kubernetes-Cluster – Azure Arc-Onboarding

Rollendefinition zum Autorisieren eines Benutzers/Diensts zum Erstellen einer connectedClusters-Ressource [Weitere Informationen](../azure-arc/kubernetes/connect-cluster.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Erstellt oder aktualisiert eine Bereitstellung. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Dient zum Abrufen der Ergebnisse des Abonnementvorgangs. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/Write | Schreibt connectedClusters. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/read | Liest connectedClusters. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Role definition to authorize any user/service to create connectedClusters resource",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/34e09817-6cbe-4d01-b1a2-e0eac5743d41",
  "name": "34e09817-6cbe-4d01-b1a2-e0eac5743d41",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Kubernetes/connectedClusters/Write",
        "Microsoft.Kubernetes/connectedClusters/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Kubernetes Cluster - Azure Arc Onboarding",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="kubernetes-extension-contributor"></a>Mitwirkender für Kubernetes-Erweiterungen

Kann Kubernetes-Erweiterungen erstellen, aktualisieren, abrufen, auflisten und löschen und asynchrone Vorgänge für Kubernetes-Erweiterungen abrufen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.KubernetesConfiguration](resource-provider-operations.md#microsoftkubernetesconfiguration)/extensions/write | Hiermit wird eine Erweiterungsressource erstellt oder aktualisiert. |
> | [Microsoft.KubernetesConfiguration](resource-provider-operations.md#microsoftkubernetesconfiguration)/extensions/read | Hiermit wird die Erweiterungsinstanzressource abgerufen. |
> | [Microsoft.KubernetesConfiguration](resource-provider-operations.md#microsoftkubernetesconfiguration)/extensions/delete | Hiermit wird die Erweiterungsinstanzressource gelöscht. |
> | [Microsoft.KubernetesConfiguration](resource-provider-operations.md#microsoftkubernetesconfiguration)/extensions/operations/read | Hiermit wird der Status für einen asynchronen Vorgang abgerufen. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can create, update, get, list and delete Kubernetes Extensions, and get extension async operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/85cb6faf-e071-4c9b-8136-154b5a04f717",
  "name": "85cb6faf-e071-4c9b-8136-154b5a04f717",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.KubernetesConfiguration/extensions/write",
        "Microsoft.KubernetesConfiguration/extensions/read",
        "Microsoft.KubernetesConfiguration/extensions/delete",
        "Microsoft.KubernetesConfiguration/extensions/operations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Kubernetes Extension Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-application-contributor-role"></a>Rolle „Mitwirkender für verwaltete Anwendungen“

Ermöglicht das Erstellen von Ressourcen für verwaltete Anwendungen.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | [Microsoft.Solutions](resource-provider-operations.md#microsoftsolutions)/applications/* |  |
> | [Microsoft.Solutions](resource-provider-operations.md#microsoftsolutions)/register/action | Hiermit registrieren Sie sich bei Lösungen. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for creating managed application resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/641177b8-a67a-45b9-a033-47bc880bb21e",
  "name": "641177b8-a67a-45b9-a033-47bc880bb21e",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Solutions/applications/*",
        "Microsoft.Solutions/register/action",
        "Microsoft.Resources/subscriptions/resourceGroups/*",
        "Microsoft.Resources/deployments/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Application Contributor Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-application-operator-role"></a>Rolle „Bediener für verwaltete Anwendung“

Ermöglicht Ihnen das Lesen und Durchführen von Aktionen für Ressourcen der verwalteten Anwendung.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | [Microsoft.Solutions](resource-provider-operations.md#microsoftsolutions)/applications/read | Hiermit wird eine Liste mit Anwendungen abgerufen. |
> | [Microsoft.Solutions](resource-provider-operations.md#microsoftsolutions)/*/action |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and perform actions on Managed Application resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c7393b34-138c-406f-901b-d8cf2b17e6ae",
  "name": "c7393b34-138c-406f-901b-d8cf2b17e6ae",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Solutions/applications/read",
        "Microsoft.Solutions/*/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Application Operator Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-applications-reader"></a>Leser für verwaltete Anwendungen

Ermöglicht Ihnen, Ressourcen in einer verwalteten App zu lesen und JIT-Zugriff anzufordern.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Solutions](resource-provider-operations.md#microsoftsolutions)/jitRequests/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read resources in a managed app and request JIT access.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b9331d33-8a36-4f8c-b097-4f54124fdb44",
  "name": "b9331d33-8a36-4f8c-b097-4f54124fdb44",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Solutions/jitRequests/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Applications Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-services-registration-assignment-delete-role"></a>Rolle „Registrierungszuweisung für verwaltete Dienste löschen“

Mit der Rolle „Registrierungszuweisung für verwaltete Dienste löschen“ können Benutzer des verwaltenden Mandanten die ihrem Mandanten zugewiesene Registrierungszuweisung löschen. [Weitere Informationen](../lighthouse/how-to/remove-delegation.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.ManagedServices](resource-provider-operations.md#microsoftmanagedservices)/registrationAssignments/read | Ruft eine Liste der Registrierungszuweisungen für verwaltete Dienste ab. |
> | [Microsoft.ManagedServices](resource-provider-operations.md#microsoftmanagedservices)/registrationAssignments/delete | Entfernt die Registrierungszuweisung für verwaltete Dienste. |
> | [Microsoft.ManagedServices](resource-provider-operations.md#microsoftmanagedservices)/operationStatuses/read | Liest den Vorgangsstatus für die Ressource. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Managed Services Registration Assignment Delete Role allows the managing tenant users to delete the registration assignment assigned to their tenant.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/91c1777a-f3dc-4fae-b103-61d183457e46",
  "name": "91c1777a-f3dc-4fae-b103-61d183457e46",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedServices/registrationAssignments/read",
        "Microsoft.ManagedServices/registrationAssignments/delete",
        "Microsoft.ManagedServices/operationStatuses/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Services Registration assignment Delete Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="management-group-contributor"></a>Verwaltungsgruppenmitwirkender

Rolle „Verwaltungsgruppenmitwirkender“ [Weitere Informationen](../governance/management-groups/overview.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/delete | Löscht eine Verwaltungsgruppe. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/subscriptions/delete | Hebt die Zuordnung des Abonnements mit der Verwaltungsgruppe auf. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/subscriptions/write | Ordnet ein vorhandenes Abonnement der Verwaltungsgruppe zu. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/write | Erstellt oder aktualisiert eine Verwaltungsgruppe. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/subscriptions/read | Listet das Abonnement unter der angegebenen Verwaltungsgruppe auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Management Group Contributor Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c",
  "name": "5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/delete",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.Management/managementGroups/subscriptions/delete",
        "Microsoft.Management/managementGroups/subscriptions/write",
        "Microsoft.Management/managementGroups/write",
        "Microsoft.Management/managementGroups/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Management Group Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="management-group-reader"></a>Verwaltungsgruppenleser

Rolle „Verwaltungsgruppenleser“

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Listet die Verwaltungsgruppen für den authentifizierten Benutzer auf. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/subscriptions/read | Listet das Abonnement unter der angegebenen Verwaltungsgruppe auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Management Group Reader Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ac63b705-f282-497d-ac71-919bf39d939d",
  "name": "ac63b705-f282-497d-ac71-919bf39d939d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/read",
        "Microsoft.Management/managementGroups/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Management Group Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="new-relic-apm-account-contributor"></a>Mitwirkender von New Relic APM-Konto

Ermöglicht Ihnen das Verwalten von New Relic Application Performance Management-Konten und -Anwendungen, nicht aber den Zugriff auf diese.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | NewRelic.APM/accounts/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage New Relic Application Performance Management accounts and applications, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d28c62d-5b37-4476-8438-e587778df237",
  "name": "5d28c62d-5b37-4476-8438-e587778df237",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "NewRelic.APM/accounts/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "New Relic APM Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="policy-insights-data-writer-preview"></a>Policy Insights-Datenschreiber (Vorschau)

Ermöglicht Lesezugriff auf Ressourcenrichtlinien und Schreibzugriff auf Richtlinienereignisse für Ressourcenkomponenten. [Weitere Informationen](../governance/policy/concepts/policy-for-kubernetes.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyassignments/read | Dient zum Abrufen von Informationen zu einer Richtlinienzuweisung. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policydefinitions/read | Dient zum Abrufen von Informationen zu einer Richtliniendefinition. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyexemptions/read | Hiermit werden Informationen zu einer Richtlinienausnahme abgerufen. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policysetdefinitions/read | Ruft Informationen zu einer Richtliniengruppendefinition ab. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.PolicyInsights](resource-provider-operations.md#microsoftpolicyinsights)/checkDataPolicyCompliance/action | Überprüft den Compliancestatus einer angegebenen Komponente anhand von Datenrichtlinien. |
> | [Microsoft.PolicyInsights](resource-provider-operations.md#microsoftpolicyinsights)/policyEvents/logDataEvents/action | Protokollieren der Ressourcenkomponenten-Richtlinienereignisse. |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to resource policies and write access to resource component policy events.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/66bb4e9e-b016-4a94-8249-4c0511c2be84",
  "name": "66bb4e9e-b016-4a94-8249-4c0511c2be84",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/policyassignments/read",
        "Microsoft.Authorization/policydefinitions/read",
        "Microsoft.Authorization/policyexemptions/read",
        "Microsoft.Authorization/policysetdefinitions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.PolicyInsights/checkDataPolicyCompliance/action",
        "Microsoft.PolicyInsights/policyEvents/logDataEvents/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Policy Insights Data Writer (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="quota-request-operator"></a>Kontingentanforderungsoperator

Liest und erstellt Kontingentanforderungen, ruft den Status von Kontingentanforderungen ab und erstellt Supporttickets. [Weitere Informationen](/rest/api/reserved-vm-instances/quotaapi)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/resourceProviders/locations/serviceLimits/read | Ruft das aktuelle Dienstlimit oder -kontingent der angegebenen Ressource und des angegebenen Standorts ab |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/resourceProviders/locations/serviceLimits/write | Erstellt das Dienstlimit oder -kontingent für die angegebene Ressource und den angegebenen Standort |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/resourceProviders/locations/serviceLimitsRequests/read | Ruft jede Anforderung des Dienstlimits für die angegebene Ressource und den angegebenen Standort ab |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/register/action | Registriert den Kapazitätsressourcenanbieter und ermöglicht die Erstellung von Kapazitätsressourcen. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read and create quota requests, get quota request status, and create support tickets.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0e5f05e5-9ab9-446b-b98d-1e2157c94125",
  "name": "0e5f05e5-9ab9-446b-b98d-1e2157c94125",
  "permissions": [
    {
      "actions": [
        "Microsoft.Capacity/resourceProviders/locations/serviceLimits/read",
        "Microsoft.Capacity/resourceProviders/locations/serviceLimits/write",
        "Microsoft.Capacity/resourceProviders/locations/serviceLimitsRequests/read",
        "Microsoft.Capacity/register/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Quota Request Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reservation-purchaser"></a>Reservierungseinkäufer

Ermöglicht den Kauf von Reservierungen. [Weitere Informationen](../cost-management-billing/reservations/prepare-buy-reservation.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Ruft die Abonnementliste ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/register/action | Registriert den Kapazitätsressourcenanbieter und ermöglicht die Erstellung von Kapazitätsressourcen. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/register/action | Registriert ein Abonnement beim Microsoft.Compute-Ressourcenanbieter. |
> | [Microsoft.SQL](resource-provider-operations.md#microsoftsql)/register/action | Registriert das Abonnement für den Microsoft SQL-Datenbankressourcenanbieter und ermöglicht die Erstellung von Microsoft SQL-Datenbanken. |
> | [Microsoft.Consumption](resource-provider-operations.md#microsoftconsumption)/register/action | Führt die Registrierung für den Verbrauchsressourcenanbieter durch. |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/catalogs/read | Liest den Katalog mit den Reservierungen. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/roleAssignments/read | Dient zum Abrufen von Informationen zu einer Rollenzuweisung. |
> | [Microsoft.Consumption](resource-provider-operations.md#microsoftconsumption)/reservationRecommendations/read | Führt einzelne oder freigegebene Empfehlungen für reservierte Instanzen für ein Abonnement ab. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/supporttickets/write | Ermöglicht das Erstellen und Aktualisieren eines Supporttickets. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you purchase reservations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f7b75c60-3036-4b75-91c3-6b41c27c1689",
  "name": "f7b75c60-3036-4b75-91c3-6b41c27c1689",
  "permissions": [
    {
      "actions": [
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Capacity/register/action",
        "Microsoft.Compute/register/action",
        "Microsoft.SQL/register/action",
        "Microsoft.Consumption/register/action",
        "Microsoft.Capacity/catalogs/read",
        "Microsoft.Authorization/roleAssignments/read",
        "Microsoft.Consumption/reservationRecommendations/read",
        "Microsoft.Support/supporttickets/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reservation Purchaser",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="resource-policy-contributor"></a>Ressourcenrichtlinienmitwirkender

Benutzer mit Rechten zum Erstellen/Ändern der Ressourcenrichtlinie, zum Erstellen eines Supporttickets und zum Lesen von Ressourcen/der Hierarchie. [Weitere Informationen](../governance/policy/overview.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | */Lesen | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyassignments/* | Erstellen und Verwalten von Richtlinienzuweisungen |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policydefinitions/* | Erstellen und Verwalten von Richtliniendefinitionen |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyexemptions/* | Erstellen und Verwalten von Richtlinienausnahmen |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policysetdefinitions/* | Erstellen und Verwalten von Richtliniensätzen |
> | [Microsoft.PolicyInsights](resource-provider-operations.md#microsoftpolicyinsights)/* |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Users with rights to create/modify resource policy, create support ticket and read resources/hierarchy.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/36243c78-bf99-498c-9df9-86d9f8d28608",
  "name": "36243c78-bf99-498c-9df9-86d9f8d28608",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Authorization/policyassignments/*",
        "Microsoft.Authorization/policydefinitions/*",
        "Microsoft.Authorization/policyexemptions/*",
        "Microsoft.Authorization/policysetdefinitions/*",
        "Microsoft.PolicyInsights/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Resource Policy Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-contributor"></a>Site Recovery-Mitwirkender

Ermöglicht Ihnen die Verwaltung des Site Recovery-Diensts mit Ausnahme der Tresorerstellung und der Rollenzuweisung. [Weitere Informationen](../site-recovery/site-recovery-role-based-linked-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocateStamp/action | „AllocateStamp“ ist ein interner Vorgang des Diensts. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/certificates/write | Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/* | Erstellen und Verwalten erweiterter Informationen in Zusammenhang mit einem Tresor |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/refreshContainers/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/* | Erstellen und Verwalten von registrierten Identitäten |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationAlertSettings/* | Erstellen oder Aktualisieren von Einstellungen für Replikationswarnungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationEvents/read | Dient zum Lesen beliebiger Ereignisse |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/* | Erstellen und Verwalten von Replikationsfabrics |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationJobs/* | Erstellen und Verwalten von Replikationsaufträgen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationPolicies/* | Erstellen und Verwalten von Replikationsrichtlinien |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/* | Erstellen und Verwalten von Wiederherstellungsplänen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationVaultSettings/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/storageConfig/* | Erstellen und Verwalten der Speicherkonfiguration des Recovery Services-Tresors |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/tokenInfo/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/* | Lesen von Warnungen für den Recovery Services-Tresor |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationOperationStatus/read | Liest alle Status des Tresorreplikationsvorgangs |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Site Recovery service except vault creation and role assignment",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6670b86e-a3f7-4917-ac9b-5d6ab1be4567",
  "name": "6670b86e-a3f7-4917-ac9b-5d6ab1be4567",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/locations/allocateStamp/action",
        "Microsoft.RecoveryServices/Vaults/certificates/write",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/*",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/*",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/*",
        "Microsoft.RecoveryServices/vaults/replicationJobs/*",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/*",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/*",
        "Microsoft.RecoveryServices/vaults/replicationVaultSettings/*",
        "Microsoft.RecoveryServices/Vaults/storageConfig/*",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/*",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/vaults/replicationOperationStatus/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-operator"></a>Site Recovery-Operator

Ermöglicht Ihnen ein Failover und ein Failback, aber nicht das Durchführen weiterer Site Recovery-Verwaltungsvorgänge. [Weitere Informationen](../site-recovery/site-recovery-role-based-linked-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Dient zum Abrufen der Definition des virtuellen Netzwerks. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocateStamp/action | „AllocateStamp“ ist ein interner Vorgang des Diensts. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/refreshContainers/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationAlertSettings/read | Dient zum Lesen beliebiger Warnungseinstellungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationEvents/read | Dient zum Lesen beliebiger Ereignisse |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/checkConsistency/action | Prüft die Konsistenz des Fabrics. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/read | Dient zum Lesen beliebiger Fabrics |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/reassociateGateway/action | Dient zum Neuzuordnen des Gateways. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/renewcertificate/action | Verlängert das Zertifikat für Fabrics. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationNetworks/read | Dient zum Lesen beliebiger Netzwerke |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Dient zum Lesen beliebiger Netzwerkzuordnungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/read | Dient zum Lesen beliebiger Schutzcontainer |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Dient zum Lesen beliebiger schützbarer Elemente |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Dient zum Anwenden des Wiederherstellungspunkts. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failovercommit |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplantes Failover |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Dient zum Lesen beliebiger geschützter Elemente |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Dient zum Lesen beliebiger Wiederherstellungspunkte für die Replikation |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Dient zum Reparieren der Replikation. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Dient zum erneuten Schützen geschützter Elemente. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Dient zum Wechseln von Schutzcontainern. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Testfailover |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Testfailoverbereinigung |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Dient zum Aktualisieren von Mobility Service. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Dient zum Lesen beliebiger Schutzcontainerzuordnungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Dient zum Lesen beliebiger Recovery Services-Anbieter |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Dient zum Aktualisieren des Anbieters. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationStorageClassifications/read | Dient zum Lesen beliebiger Speicherklassifizierungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Dient zum Lesen beliebiger Speicherklassifizierungszuordnungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationvCenters/read | Dienst zum Lesen beliebiger vCenter |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationJobs/* | Erstellen und Verwalten von Replikationsaufträgen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationPolicies/read | Dient zum Lesen beliebiger Richtlinien |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/failoverCommit/action | Wiederherstellungsplan für Failovercommit |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/plannedFailover/action | Wiederherstellungsplan für geplantes Failover |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/read | Dient zum Lesen beliebiger Wiederherstellungspläne |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/reProtect/action | Wiederherstellungsplan für erneutes Schützen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/testFailover/action | Wiederherstellungsplan für Testfailover |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Wiederherstellungsplan für Testfailoverbereinigung |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/unplannedFailover/action | Wiederherstellungsplan für Failover |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationVaultSettings/read | Liest alles.  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/* | Lesen von Warnungen für den Recovery Services-Tresor |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/storageConfig/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/tokenInfo/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you failover and failback but not perform other Site Recovery management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/494ae006-db33-4328-bf46-533a6560a3ca",
  "name": "494ae006-db33-4328-bf46-533a6560a3ca",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/locations/allocateStamp/action",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/read",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read",
        "Microsoft.RecoveryServices/vaults/replicationJobs/*",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationVaultSettings/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/*",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.RecoveryServices/Vaults/storageConfig/read",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-reader"></a>Site Recovery-Leser

Ermöglicht Ihnen die Anzeige des Site Recovery-Status, aber nicht die Durchführung weiterer Verwaltungsvorgänge. [Weitere Informationen](../site-recovery/site-recovery-role-based-linked-access-control.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocatedStamp/read | „GetAllocatedStamp“ ist ein interner Vorgang des Diensts. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/read | Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/read | Ruft die Warnungen für den Recovery Services-Tresor ab. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/refreshContainers/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/operationResults/read | Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/read | Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationAlertSettings/read | Dient zum Lesen beliebiger Warnungseinstellungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationEvents/read | Dient zum Lesen beliebiger Ereignisse |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/read | Dient zum Lesen beliebiger Fabrics |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationNetworks/read | Dient zum Lesen beliebiger Netzwerke |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Dient zum Lesen beliebiger Netzwerkzuordnungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/read | Dient zum Lesen beliebiger Schutzcontainer |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Dient zum Lesen beliebiger schützbarer Elemente |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Dient zum Lesen beliebiger geschützter Elemente |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Dient zum Lesen beliebiger Wiederherstellungspunkte für die Replikation |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Dient zum Lesen beliebiger Schutzcontainerzuordnungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Dient zum Lesen beliebiger Recovery Services-Anbieter |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationStorageClassifications/read | Dient zum Lesen beliebiger Speicherklassifizierungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Dient zum Lesen beliebiger Speicherklassifizierungszuordnungen |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationvCenters/read | Dienst zum Lesen beliebiger vCenter |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationJobs/read | Dient zum Lesen beliebiger Aufträge |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationPolicies/read | Dient zum Lesen beliebiger Richtlinien |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/read | Dient zum Lesen beliebiger Wiederherstellungspläne |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationVaultSettings/read | Liest alles.  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/storageConfig/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/tokenInfo/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/vaultTokens/read | Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view Site Recovery status but not perform other management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/dbaa88c4-0c30-4179-9fb3-46319faa6149",
  "name": "dbaa88c4-0c30-4179-9fb3-46319faa6149",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/read",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read",
        "Microsoft.RecoveryServices/vaults/replicationJobs/read",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read",
        "Microsoft.RecoveryServices/vaults/replicationVaultSettings/read",
        "Microsoft.RecoveryServices/Vaults/storageConfig/read",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="support-request-contributor"></a>Mitwirkender für Supportanfragen

Ermöglicht Ihnen die Erstellung und Verwaltung von Supportanfragen. [Weitere Informationen](../azure-portal/supportability/how-to-create-azure-support-request.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create and manage Support requests",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e",
  "name": "cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Support Request Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="tag-contributor"></a>Tagmitwirkender

Hiermit können Tags für Entitäten verwaltet werden, ohne Zugriff auf die Entitäten selbst zu gewähren. [Weitere Informationen](../azure-resource-manager/management/tag-resources.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/resources/read | Ruft die Ressourcen für die Ressourcengruppe ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resources/read | Ruft die Ressourcen eines Abonnements ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/tags/* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage tags on entities, without providing access to the entities themselves.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4a9ae827-6dc8-4573-8ac7-8239d42aa03f",
  "name": "4a9ae827-6dc8-4573-8ac7-8239d42aa03f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
        "Microsoft.Resources/subscriptions/resources/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*",
        "Microsoft.Resources/tags/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Tag Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="other"></a>Andere


### <a name="azure-digital-twins-data-owner"></a>Azure Digital Twins Data Owner (Azure Digital Twins-Datenbesitzer)

Vollzugriffsrolle für Digital Twins-Datenebene [Weitere Informationen](../digital-twins/concepts-security.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/eventroutes/* | Lesen, Löschen, Erstellen oder Aktualisieren jeder Ereignisroute |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/digitaltwins/* | Lesen, Erstellen, Aktualisieren oder Löschen jedes digitalen Zwillings |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/digitaltwins/commands/* | Aufrufen jedes Befehls für einen digitalen Zwilling |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/digitaltwins/relationships/* | Lesen, Erstellen, Aktualisieren oder Löschen jeder Beziehung zwischen digitalen Zwillingen |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/models/* | Lesen, Erstellen, Aktualisieren oder Löschen jedes Modells |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/query/* | Abfragen jedes Digital Twins-Graphen |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Full access role for Digital Twins data-plane",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/bcd981a7-7f74-457b-83e1-cceb9e632ffe",
  "name": "bcd981a7-7f74-457b-83e1-cceb9e632ffe",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.DigitalTwins/eventroutes/*",
        "Microsoft.DigitalTwins/digitaltwins/*",
        "Microsoft.DigitalTwins/digitaltwins/commands/*",
        "Microsoft.DigitalTwins/digitaltwins/relationships/*",
        "Microsoft.DigitalTwins/models/*",
        "Microsoft.DigitalTwins/query/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Digital Twins Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-digital-twins-data-reader"></a>Azure Digital Twins Data Reader (Azure Digital Twins-Datenleser)

Schreibgeschützte Rolle für Digital Twins-Datenebeneneigenschaften [Weitere Informationen](../digital-twins/concepts-security.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/digitaltwins/read | Lesen jedes digitalen Zwillings |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/digitaltwins/relationships/read | Lesen jeder Beziehung zwischen digitalen Zwillingen |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/eventroutes/read | Lesen jeder Ereignisroute |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/models/read | Lesen jedes Modells |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/query/action | Abfragen jedes Digital Twins-Graphen |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read-only role for Digital Twins data-plane properties",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d57506d4-4c8d-48b1-8587-93c323f6a5a3",
  "name": "d57506d4-4c8d-48b1-8587-93c323f6a5a3",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.DigitalTwins/digitaltwins/read",
        "Microsoft.DigitalTwins/digitaltwins/relationships/read",
        "Microsoft.DigitalTwins/eventroutes/read",
        "Microsoft.DigitalTwins/models/read",
        "Microsoft.DigitalTwins/query/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Digital Twins Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="biztalk-contributor"></a>Mitwirkender von BizTalk

Ermöglicht Ihnen das Verwalten von BizTalk-Diensten, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | Microsoft.BizTalkServices/BizTalk/* | Erstellen und Verwalten von BizTalk-Diensten |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage BizTalk services, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5e3c6656-6cfa-4708-81fe-0de47ac73342",
  "name": "5e3c6656-6cfa-4708-81fe-0de47ac73342",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.BizTalkServices/BizTalk/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "BizTalk Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-application-group-contributor"></a>Anwendungsgruppenmitwirkender für die Desktopvirtualisierung

Mitwirkender der Anwendungsgruppe für die Desktopvirtualisierung. [Weitere Informationen](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationgroups/* |  |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/read | Lesen von Hostpools |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/sessionhosts/read | Lesen von Hostpools/Sessionhosts |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Contributor of the Desktop Virtualization Application Group.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/86240b0e-9422-4c43-887b-b61143f32ba8",
  "name": "86240b0e-9422-4c43-887b-b61143f32ba8",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/applicationgroups/*",
        "Microsoft.DesktopVirtualization/hostpools/read",
        "Microsoft.DesktopVirtualization/hostpools/sessionhosts/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Application Group Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-application-group-reader"></a>Anwendungsgruppenleser für die Desktopvirtualisierung

Leser der Anwendungsgruppe für die Desktopvirtualisierung. [Weitere Informationen](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationgroups/*/read |  |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationgroups/read | Lesen von Anwendungsgruppen |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/read | Lesen von Hostpools |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/sessionhosts/read | Lesen von Hostpools/Sessionhosts |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/read | Ruft Bereitstellungen ab oder listet sie auf. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Liest eine klassische Metrikwarnung. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Reader of the Desktop Virtualization Application Group.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/aebf23d0-b568-4e86-b8f9-fe83a2c6ab55",
  "name": "aebf23d0-b568-4e86-b8f9-fe83a2c6ab55",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/applicationgroups/*/read",
        "Microsoft.DesktopVirtualization/applicationgroups/read",
        "Microsoft.DesktopVirtualization/hostpools/read",
        "Microsoft.DesktopVirtualization/hostpools/sessionhosts/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Application Group Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-contributor"></a>Desktopvirtualisierungsmitwirkender

Mitwirkender für die Desktopvirtualisierung. [Weitere Informationen](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Contributor of Desktop Virtualization.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/082f0a83-3be5-4ba1-904c-961cca79b387",
  "name": "082f0a83-3be5-4ba1-904c-961cca79b387",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-host-pool-contributor"></a>Hostpoolmitwirkender für die Desktopvirtualisierung

Mitwirkender des Hostpools für die Desktopvirtualisierung. [Weitere Informationen](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Contributor of the Desktop Virtualization Host Pool.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e307426c-f9b6-4e81-87de-d99efb3c32bc",
  "name": "e307426c-f9b6-4e81-87de-d99efb3c32bc",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/hostpools/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Host Pool Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-host-pool-reader"></a>Hostpoolleser für die Desktopvirtualisierung

Leser des Hostpools für die Desktopvirtualisierung. [Weitere Informationen](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/*/read |  |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/read | Lesen von Hostpools |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/read | Ruft Bereitstellungen ab oder listet sie auf. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Liest eine klassische Metrikwarnung. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Reader of the Desktop Virtualization Host Pool.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ceadfde2-b300-400a-ab7b-6143895aa822",
  "name": "ceadfde2-b300-400a-ab7b-6143895aa822",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/hostpools/*/read",
        "Microsoft.DesktopVirtualization/hostpools/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Host Pool Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-reader"></a>Desktopvirtualisierungsleser

Leser für die Desktopvirtualisierung. [Weitere Informationen](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/*/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/read | Ruft Bereitstellungen ab oder listet sie auf. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Liest eine klassische Metrikwarnung. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Reader of Desktop Virtualization.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/49a72310-ab8d-41df-bbb0-79b649203868",
  "name": "49a72310-ab8d-41df-bbb0-79b649203868",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-session-host-operator"></a>Sitzungshostoperator für die Desktopvirtualisierung

Operator des Sitzungshosts für die Desktopvirtualisierung. [Weitere Informationen](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/read | Lesen von Hostpools |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/sessionhosts/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Operator of the Desktop Virtualization Session Host.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2ad6aaab-ead9-4eaa-8ac5-da422f562408",
  "name": "2ad6aaab-ead9-4eaa-8ac5-da422f562408",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/hostpools/read",
        "Microsoft.DesktopVirtualization/hostpools/sessionhosts/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Session Host Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-user"></a>Desktopvirtualisierungsbenutzer

Ermöglicht dem Benutzer die Verwendung der Anwendungen in einer Anwendungsgruppe. [Weitere Informationen](../virtual-desktop/delegated-access-virtual-desktop.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | *keine* |  |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationGroups/useApplications/action | Verwendet „ApplicationGroup“ |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows user to use the applications in an application group.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/1d18fff3-a72a-46b5-b4a9-0b38a3cd7e63",
  "name": "1d18fff3-a72a-46b5-b4a9-0b38a3cd7e63",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.DesktopVirtualization/applicationGroups/useApplications/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-user-session-operator"></a>Benutzersitzungsoperator für die Desktopvirtualisierung

Operator der Benutzersitzung für die Desktopvirtualisierung. [Weitere Informationen](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/read | Lesen von Hostpools |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/sessionhosts/read | Lesen von Hostpools/Sessionhosts |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/sessionhosts/usersessions/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Operator of the Desktop Virtualization Uesr Session.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ea4bfff8-7fb4-485a-aadd-d4129a0ffaa6",
  "name": "ea4bfff8-7fb4-485a-aadd-d4129a0ffaa6",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/hostpools/read",
        "Microsoft.DesktopVirtualization/hostpools/sessionhosts/read",
        "Microsoft.DesktopVirtualization/hostpools/sessionhosts/usersessions/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization User Session Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-workspace-contributor"></a>Arbeitsbereichsmitwirkender für die Desktopvirtualisierung

Mitwirkender des Arbeitsbereichs für die Desktopvirtualisierung. [Weitere Informationen](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/workspaces/* |  |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationgroups/read | Lesen von Anwendungsgruppen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Contributor of the Desktop Virtualization Workspace.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/21efdde3-836f-432b-bf3d-3e8e734d4b2b",
  "name": "21efdde3-836f-432b-bf3d-3e8e734d4b2b",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/workspaces/*",
        "Microsoft.DesktopVirtualization/applicationgroups/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Workspace Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-workspace-reader"></a>Arbeitsbereichsleser für die Desktopvirtualisierung

Leser des Arbeitsbereichs für die Desktopvirtualisierung. [Weitere Informationen](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/workspaces/read | Lesen von Arbeitsbereichen |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationgroups/read | Lesen von Anwendungsgruppen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/read | Ruft Bereitstellungen ab oder listet sie auf. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Liest eine klassische Metrikwarnung. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Reader of the Desktop Virtualization Workspace.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0fa44ee9-7a7d-466b-9bb2-2bf446b1204d",
  "name": "0fa44ee9-7a7d-466b-9bb2-2bf446b1204d",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/workspaces/read",
        "Microsoft.DesktopVirtualization/applicationgroups/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Workspace Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="disk-backup-reader"></a>Leser für die Datenträgersicherung

Berechtigung für den Sicherungstresor zum Ausführen einer Datenträgersicherung. [Weitere Informationen](../backup/disk-backup-faq.yml)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/read | Dient zum Abrufen der Eigenschaften eines Datenträgers. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/beginGetAccess/action | Dient zum Abrufen des SAS-URIs des Datenträgers für den Blobzugriff. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Provides permission to backup vault to perform disk backup.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3e5e47e6-65f7-47ef-90b5-e5dd4d455f24",
  "name": "3e5e47e6-65f7-47ef-90b5-e5dd4d455f24",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/disks/read",
        "Microsoft.Compute/disks/beginGetAccess/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Disk Backup Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="disk-pool-operator"></a>Datenträgerpooloperator

Gewährt dem StoragePool-Ressourcenanbieter die Berechtigung zum Verwalten von Datenträgern, die einem Datenträgerpool hinzugefügt wurden

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/write | Erstellt einen neuen Datenträger oder aktualisiert einen bereits vorhandenen. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/read | Dient zum Abrufen der Eigenschaften eines Datenträgers. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Used by the StoragePool Resource Provider to manage Disks added to a Disk Pool.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/60fc6e62-5479-42d4-8bf4-67625fcc2840",
  "name": "60fc6e62-5479-42d4-8bf4-67625fcc2840",
  "permissions": [
    {
      "actions": [
        "Microsoft.Compute/disks/write",
        "Microsoft.Compute/disks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Disk Pool Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="disk-restore-operator"></a>Operator für die Datenträgerwiederherstellung

Berechtigung für den Sicherungstresor zum Ausführen einer Datenträgerwiederherstellung. [Weitere Informationen](../backup/restore-managed-disks.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/write | Erstellt einen neuen Datenträger oder aktualisiert einen bereits vorhandenen. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/read | Dient zum Abrufen der Eigenschaften eines Datenträgers. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Provides permission to backup vault to perform disk restore.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b50d9833-a0cb-478e-945f-707fcc997c13",
  "name": "b50d9833-a0cb-478e-945f-707fcc997c13",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Compute/disks/write",
        "Microsoft.Compute/disks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Disk Restore Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="disk-snapshot-contributor"></a>Mitwirkender für Datenträgermomentaufnahmen

Berechtigung für den Sicherungstresor zum Verwalten von Datenträgermomentaufnahmen. [Weitere Informationen](../backup/backup-managed-disks.md)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/snapshots/delete | Dient zum Löschen einer Momentaufnahme. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/snapshots/write | Dient zum Erstellen einer neuen Momentaufnahme oder zum Aktualisieren einer bereits vorhandenen. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/snapshots/read | Dient zum Abrufen der Eigenschaften einer Momentaufnahme. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/snapshots/beginGetAccess/action | Dient zum Abrufen des SAS-URI der Momentaufnahme für den Blobzugriff. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/snapshots/endGetAccess/action | Dient zum Sperren des SAS-URI der Momentaufnahme. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/beginGetAccess/action | Dient zum Abrufen des SAS-URIs des Datenträgers für den Blobzugriff. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listkeys/action | Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/write | Erstellt ein Speicherkonto mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags oder fügt eine benutzerdefinierte Domäne zum angegebenen Speicherkonto hinzu. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/delete | Löscht ein vorhandenes Speicherkonto. |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Provides permission to backup vault to manage disk snapshots.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7efff54f-a5b4-42b5-a1c5-5411624893ce",
  "name": "7efff54f-a5b4-42b5-a1c5-5411624893ce",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Compute/snapshots/delete",
        "Microsoft.Compute/snapshots/write",
        "Microsoft.Compute/snapshots/read",
        "Microsoft.Compute/snapshots/beginGetAccess/action",
        "Microsoft.Compute/snapshots/endGetAccess/action",
        "Microsoft.Compute/disks/beginGetAccess/action",
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/write",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Storage/storageAccounts/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Disk Snapshot Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="scheduler-job-collections-contributor"></a>Mitwirkender von Zeitplanungsauftragssammlung

Ermöglicht Ihnen das Verwalten von Scheduler-Auftragssammlungen, nicht aber den Zugriff darauf.

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Erstellen und Verwalten einer klassischen Metrikwarnung |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Scheduler](resource-provider-operations.md#microsoftscheduler)/jobcollections/* | Erstellen und Verwalten von Auftragssammlungen |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Erstellen und Aktualisieren eines Supporttickets |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Scheduler job collections, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/188a0f2f-5c9e-469b-ae67-2aa5ce574b94",
  "name": "188a0f2f-5c9e-469b-ae67-2aa5ce574b94",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Scheduler/jobcollections/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Scheduler Job Collections Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="services-hub-operator"></a>Diensthuboperator

Berechtigungen für sämtliche Lese-, Schreib- und Löschvorgänge im Zusammenhang mit Dienstubconnectors. [Weitere Informationen](/services-hub/health/sh-connector-roles)

> [!div class="mx-tableFixed"]
> | Aktionen | BESCHREIBUNG |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Lesen von Rollen und Rollenzuweisungen |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Ruft Ressourcengruppen ab oder listet sie auf. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Erstellen und Verwalten einer Bereitstellung |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/connectors/write | Erstellen oder Aktualisieren eines Diensthubconnectors |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/connectors/read | Anzeigen oder Auflisten von Diensthubconnectors |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/connectors/delete | Löschen von Diensthubconnectors |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/connectors/checkAssessmentEntitlement/action | Auflisten der Bewertungsberechtigungen für einen bestimmten Diensthub-Arbeitsbereich |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/supportOfferingEntitlement/read | Anzeigen der Berechtigungen für Supportangebote für einen bestimmten Diensthub-Arbeitsbereich |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/workspaces/read | Auflisten der Diensthub-Arbeitsbereiche für einen bestimmten Benutzer |
> | **NotActions** |  |
> | *keine* |  |
> | **DataActions** |  |
> | *keine* |  |
> | **NotDataActions** |  |
> | *keine* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Services Hub Operator allows you to perform all read, write, and deletion operations related to Services Hub Connectors.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/82200a5b-e217-47a5-b665-6d8765ee745b",
  "name": "82200a5b-e217-47a5-b665-6d8765ee745b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.ServicesHub/connectors/write",
        "Microsoft.ServicesHub/connectors/read",
        "Microsoft.ServicesHub/connectors/delete",
        "Microsoft.ServicesHub/connectors/checkAssessmentEntitlement/action",
        "Microsoft.ServicesHub/supportOfferingEntitlement/read",
        "Microsoft.ServicesHub/workspaces/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Services Hub Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure-Rollen über das Azure-Portal](role-assignments-portal.md)
- [Benutzerdefinierte Azure-Rollen](custom-roles.md)
- [Berechtigungen in Azure Security Center](../security-center/security-center-permissions.md)
