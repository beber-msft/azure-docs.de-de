---
title: 'Tutorial: Zuordnen einer vorhandenen benutzerdefinierten Domäne zu Azure Spring Cloud'
description: Hier erfahren Sie, wie Sie Azure Spring Cloud einen bereits vorhandenen benutzerdefinierten DNS-Namen (Distributed Name Service) zuordnen.
author: karlerickson
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 03/19/2020
ms.author: karler
ms.custom: devx-track-java
ms.openlocfilehash: 73f6c8217b6d630a1e31b53d54da56e5d30582a6
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2021
ms.locfileid: "132488960"
---
# <a name="tutorial-map-an-existing-custom-domain-to-azure-spring-cloud"></a>Tutorial: Zuordnen einer vorhandenen benutzerdefinierten Domäne zu Azure Spring Cloud

**Dieser Artikel gilt für:** ✔️ Java ✔️ C#

DNS (Domain Name Service) wird verwendet, um Namen von Netzwerkknoten im gesamten Netzwerk zu speichern. In diesem Tutorial wird eine Domäne (beispielsweise www.contoso.com) mithilfe eines CNAME-Eintrags zugeordnet. Die benutzerdefinierte Domäne wird durch ein Zertifikat geschützt, und Sie erfahren, wie Sie Transport Layer Security (TLS) erzwingen, was auch als Secure Sockets Layer (SSL) bezeichnet wird.

Zertifikate dienen zur Verschlüsselung von Webdatenverkehr. Diese TLS-/SSL-Zertifikate können in Azure Key Vault gespeichert werden.

## <a name="prerequisites"></a>Voraussetzungen

* Eine in Azure Spring Cloud bereitgestellte Anwendung (siehe [Schnellstart: Bereitstellen Ihrer ersten Anwendung in Azure Spring Cloud](./quickstart.md), oder verwenden Sie eine vorhandene App)
* Ein Domänenname mit Zugriff auf die DNS-Registrierung für den Domänenanbieter (beispielsweise GoDaddy).
* Ein privates Zertifikat (d. h. ihr selbst signiertes Zertifikat) von einem Drittanbieter. Das Zertifikat muss der Domäne entsprechen.
* Eine bereitgestellte Instanz von [Azure Key Vault](../key-vault/general/overview.md).

## <a name="keyvault-private-link-considerations"></a>Überlegungen zu privaten Key Vault-Verbindungen

Die Azure Spring Cloud-Verwaltungs-IPs sind noch nicht Teil der vertrauenswürdigen Azure-Dienste von Microsoft. Damit Azure Spring Cloud Zertifikate aus einer Key Vault-Instanz laden kann, die durch private Endpunktverbindungen geschützt ist, müssen Sie daher der Azure Key Vault-Firewall die folgenden IP-Adressen hinzufügen: `20.53.123.160 52.143.241.210 40.65.234.114 52.142.20.14 20.54.40.121 40.80.210.49 52.253.84.152 20.49.137.168 40.74.8.134 51.143.48.243`

## <a name="import-certificate"></a>Importieren des Zertifikats

### <a name="prepare-your-certificate-file-in-pfx-optional"></a>Vorbereiten der Zertifikatsdatei als PFX (optional)

Azure Key Vault unterstützt das Importieren eines privaten Zertifikats im PEM- und PFX-Format. Wenn die PEM-Datei, die Sie von Ihrem Zertifikatanbieter erhalten haben, im folgenden Abschnitt nicht funktioniert, gehen Sie wie folgt vor: [Speichern Sie das Zertifikat in Key Vault](#save-certificate-in-key-vault), und führen Sie die hier beschriebenen Schritte aus, um eine PFX-Datei für Azure Key Vault zu generieren.

#### <a name="merge-intermediate-certificates"></a>Zusammenführen von Zwischenzertifikaten

Wenn Ihre Zertifizierungsstelle Ihnen mehrere Zertifikate in der Zertifikatskette bereitstellt, müssen Sie die Zertifikate nacheinander zusammenführen.

Öffnen Sie hierzu alle Zertifikate, die Sie erhalten haben, in einem Text-Editor.

Erstellen Sie eine Datei für das zusammengeführte Zertifikat mit dem Namen _mergedcertificate.crt_. Kopieren Sie den Inhalt der einzelnen Zertifikate in einem Text-Editor in diese Datei. Die Reihenfolge Ihrer Zertifikate sollte der Reihenfolge in der Zertifikatkette folgen, beginnend mit Ihrem Zertifikat und endend mit dem Stammzertifikat. Dies sieht in etwa wie im folgenden Beispiel aus:

```crt
-----BEGIN CERTIFICATE-----
<your entire Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded root certificate>
-----END CERTIFICATE-----
```

#### <a name="export-certificate-to-pfx"></a>Exportieren des Zertifikats als PFX-Datei

Exportieren Sie Ihr zusammengeführtes TLS-/SSL-Zertifikat mit dem privaten Schlüssel, mit dem Ihre Zertifikatanforderung generiert wurde.

Wenn Sie die Zertifikatanforderung mittels OpenSSL generiert haben, haben Sie eine Datei des privaten Schlüssels erstellt. Führen Sie folgenden Befehl aus, um Ihr Zertifikat nach PFX zu exportieren. Ersetzen Sie die Platzhalter _&lt;private-key-file>_ und _&lt;merged-certificate-file>_ mit den Pfaden zu Ihrem privaten Schlüssel und Ihrer zusammengeführten Zertifikatdatei.

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>
```

Definieren Sie ein Kennwort für den Export, wenn Sie dazu aufgefordert werden. Dieses Kennwort verwenden Sie, wenn Sie Ihr TLS-/SSL-Zertifikat zu einem späteren Zeitpunkt in Azure Key Vault hochladen.

Wenn Sie IIS oder _Certreq.exe_ zum Generieren Ihrer Zertifikatanforderung verwendet haben, installieren Sie das Zertifikat auf dem lokalen Computer, und klicken Sie anschließend auf [Zertifikat nach PFX exportieren](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754329(v=ws.11)).

### <a name="save-certificate-in-key-vault"></a>Speichern eines Zertifikats in Key Vault

Zum Importieren eines Zertifikats muss sich die PEM- oder PFX-codierte Datei auf dem Datenträger befinden, und Sie müssen über den privaten Schlüssel verfügen.

#### <a name="portal"></a>[Portal](#tab/Azure-portal)
So laden Sie Ihr Zertifikat in den Schlüsseltresor hoch:
1. Navigieren Sie zu Ihrer Schlüsseltresorinstanz.
1. Wählen Sie im linken Navigationsbereich die Option **Zertifikate** aus.
1. Wählen Sie im oberen Menü **Generieren/importieren** aus.
1. Wählen Sie im Dialogfeld **Zertifikat erstellen** unter **Methode der Zertifikaterstellung** die Option `Import` aus.
1. Navigieren Sie unter **Zertifikatdatei hochladen** zum Speicherort des Zertifikats, und wählen Sie die Datei aus.
1. **Kennwort**: Wenn Sie eine kennwortgeschützte Zertifikatsdatei hochladen, geben Sie das Kennwort hier an. Lassen Sie dieses Feld andernfalls leer. Nachdem die Zertifikatsdatei erfolgreich importiert wurde, entfernt Key Vault das Kennwort.
1. Klicken Sie auf **Erstellen**.

    ![Importieren des Zertifikats 1](./media/custom-dns-tutorial/import-certificate-a.png)

#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/Azure-CLI)

```azurecli
az keyvault certificate import --file <path to .pfx file> --name <certificate name> --vault-name <key vault name> --password <export password>
```

---

### <a name="grant-azure-spring-cloud-access-to-your-key-vault"></a>Gewähren von Zugriff auf Ihren Schlüsseltresor für Azure Spring Cloud

Sie müssen Azure Spring Cloud Zugriff auf Ihren Schlüsseltresor gewähren, bevor Sie das Zertifikat importieren:

#### <a name="portal"></a>[Portal](#tab/Azure-portal)
1. Navigieren Sie zu Ihrer Schlüsseltresorinstanz.
1. Wählen Sie im linken Navigationsbereich **Zugriffsrichtlinie** aus.
1. Wählen Sie im oberen Menü **Zugriffsrichtlinie hinzufügen** aus.
1. Geben Sie die Informationen ein, und wählen Sie die Schaltfläche **Hinzufügen** aus. Wählen Sie anschließend **Speichern** aus, um die Zugriffsrichtlinie zu speichern.

| Geheimnisberechtigung | Zertifikatsberechtigung | Prinzipal auswählen |
|--|--|--|
| Get, List | Get, List | Azure Spring Cloud-Domänenverwaltung |

![Importieren des Zertifikats 2](./media/custom-dns-tutorial/import-certificate-b.png)

#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/Azure-CLI)

Gewähren Sie Azure Spring Cloud Lesezugriff auf den Schlüsseltresor. Ersetzen Sie dabei *\<key vault resource group>* und *\<key vault name>* im folgenden Befehl.

```azurecli
az keyvault set-policy -g <key vault resource group> -n <key vault name>  --object-id 938df8e2-2b9d-40b1-940c-c75c33494239 --certificate-permissions get list --secret-permissions get list
```

---

### <a name="import-certificate-to-azure-spring-cloud"></a>Importieren eines Zertifikats in Azure Spring Cloud

#### <a name="portal"></a>[Portal](#tab/Azure-portal)
1. Navigieren Sie zu Ihrer Dienstinstanz.
1. Wählen Sie im linken Navigationsbereich Ihrer App die Option **TLS-/SSL-Einstellungen** aus.
1. Wählen Sie anschließend **Key Vault-Zertifikat importieren** aus.

    ![Importieren des Zertifikats](./media/custom-dns-tutorial/import-certificate.png)

1. Wenn das Zertifikat erfolgreich importiert wurde, wird es in der Liste **Private Schlüsselzertifikate** angezeigt.

    ![Privates Schlüsselzertifikat](./media/custom-dns-tutorial/key-certificates.png)

#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/Azure-CLI)

```azurecli
az spring-cloud certificate add --name <cert name> --vault-uri <key vault uri> --vault-certificate-name <key vault cert name>
```

So zeigen Sie eine Liste der importierten Zertifikate an:

```azurecli
az spring-cloud certificate list --resource-group <resource group name> --service <service name>
```

---

> [!IMPORTANT]
> Das Zertifikat muss noch an eine benutzerdefinierte Domäne gebunden werden, um sie mit diesem Zertifikat zu schützen. Führen Sie die Schritte im folgenden Abschnitt aus: [Hinzufügen von SSL-Bindung](#add-ssl-binding).

## <a name="add-custom-domain"></a>Hinzufügen einer benutzerdefinierten Domäne
Sie können einen CNAME-Eintrag verwenden, um Azure Spring Cloud einen benutzerdefinierten DNS-Namen zuzuordnen.

> [!NOTE]
> Der A-Eintrag wird nicht unterstützt.

### <a name="create-the-cname-record"></a>Erstellen des CNAME-Eintrags

Navigieren Sie zu Ihrem DNS-Anbieter, und fügen Sie einen CNAME-Eintrag hinzu, um Ihre Domäne Folgendem zuzuordnen: <Dienstname>.azuremicroservices.io. „<Dienstname>“ ist hierbei der Name Ihrer Azure Spring Cloud-Instanz. Für Domäne und Unterdomäne werden Platzhalter unterstützt.
Nach dem Hinzufügen des CNAME-Eintrags sieht die Seite mit den DNS-Einträgen in etwa wie folgt aus:

![Seite der DNS-Einträge](./media/custom-dns-tutorial/dns-records.png)

## <a name="map-your-custom-domain-to-azure-spring-cloud-app"></a>Zuordnen Ihrer benutzerdefinierten Domäne zur Azure Spring Cloud-App
Wenn Sie keine Anwendung in Azure Spring Cloud haben, befolgen Sie die Anweisungen unter [Schnellstart: Bereitstellen Ihrer ersten Anwendung in Azure Spring Cloud](./quickstart.md).

#### <a name="portal"></a>[Portal](#tab/Azure-portal)
Navigieren Sie zur Anwendungsseite.

1. Wählen Sie **Benutzerdefinierte Domäne** aus.
2. Wählen Sie **Benutzerdefinierte Domäne hinzufügen** aus.

    ![Benutzerdefinierte Domäne](./media/custom-dns-tutorial/custom-domain.png)

3. Geben Sie den vollqualifizierten Domänennamen ein, für den Sie einen CNAME-Eintrag hinzugefügt haben (beispielsweise www.contoso.com). Vergewissern Sie sich, dass der Typ des Hostnamenseintrags auf „CNAME“ (<Dienstname>.azuremicroservices.io) festgelegt ist.
4. Wählen Sie **Überprüfen** aus, um die Schaltfläche **Hinzufügen** zu aktivieren.
5. Wählen Sie **Hinzufügen**.

    ![Hinzufügen einer benutzerdefinierten Domäne](./media/custom-dns-tutorial/add-custom-domain.png)

Eine App kann über mehrere Domänen verfügen, eine Domäne kann jedoch nur einer einzelnen App zugeordnet werden. Wenn Sie Ihre benutzerdefinierte Domäne erfolgreich der App zugeordnet haben, wird sie in der Tabelle mit den benutzerdefinierten Domänen angezeigt.

![Tabelle mit benutzerdefinierten Domänen](./media/custom-dns-tutorial/custom-domain-table.png)

#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/Azure-CLI)
```azurecli
az spring-cloud app custom-domain bind --domain-name <domain name> --app <app name> --resource-group <resource group name> --service <service name>
```

So zeigen Sie die Liste der benutzerdefinierten Domänen an:

```azurecli
az spring-cloud app custom-domain list --app <app name> --resource-group <resource group name> --service <service name>
```

---

> [!NOTE]
> Ist Ihre benutzerdefinierte Domäne mit **Nicht sicher** gekennzeichnet, wurde sie noch nicht an ein SSL-Zertifikat gebunden. Bei HTTPS-Anforderungen, die von einem Browser an Ihre benutzerdefinierte Domäne gerichtet werden, wird ein Fehler oder eine Warnung zurückgegeben.

## <a name="add-ssl-binding"></a>Hinzufügen von SSL-Bindung

#### <a name="portal"></a>[Portal](#tab/Azure-portal)
Wählen Sie in der Tabelle mit den benutzerdefinierten Domänen die Option **Add ssl binding** (SSL-Bindung hinzufügen) aus, wie in der vorherigen Abbildung zu sehen.
1. Wählen Sie unter **Zertifikat** Ihr Zertifikat aus, oder importieren Sie es.
1. Wählen Sie **Speichern** aus.

    ![Hinzufügen von SSL-Bindung 1](./media/custom-dns-tutorial/add-ssl-binding.png)

#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/Azure-CLI)
```azurecli
az spring-cloud app custom-domain update --domain-name <domain name> --certificate <cert name> --app <app name> --resource-group <resource group name> --service <service name>
```

---

Nach erfolgreichem Hinzufügen der SSL-Bindung ist die Domäne sicher und wird mit folgendem Zustand gekennzeichnet: **Fehlerfrei**.

![Hinzufügen von SSL-Bindung 2](./media/custom-dns-tutorial/secured-domain-state.png)

## <a name="enforce-https"></a>Erzwingen von HTTPS

Standardmäßig können Benutzer weiterhin über HTTP auf Ihre App zugreifen, alle HTTP-Anforderungen können jedoch an den HTTPS-Port umgeleitet werden.
#### <a name="portal"></a>[Portal](#tab/Azure-portal)
Wählen Sie im linken Navigationsbereich Ihrer App-Seite die Option **Benutzerdefinierte Domäne** aus. Legen Sie **Nur HTTPS** auf *True* fest.

![Hinzufügen von SSL-Bindung 3](./media/custom-dns-tutorial/enforce-http.png)

#### <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/Azure-CLI)
```azurecli
az spring-cloud app update -n <app name> --resource-group <resource group name> --service <service name> --https-only
```

---

Navigieren Sie nach Abschluss des Vorgangs zu einer beliebigen HTTPS-URL, die auf Ihre App verweist. Beachten Sie, dass HTTP-URLs nicht funktionieren.

## <a name="see-also"></a>Weitere Informationen

* [Was ist der Azure-Schlüsseltresor?](../key-vault/general/overview.md)
* [Importieren eines Zertifikats](../key-vault/certificates/certificate-scenarios.md#import-a-certificate)
* [Starten einer Spring Cloud-App mit der Azure CLI](./quickstart.md)
