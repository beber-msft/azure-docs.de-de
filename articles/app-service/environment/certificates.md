---
title: Zertifikatbindungen
description: Hier werden zahlreiche Themen rund um Zertifikate in einer App Service-Umgebung v2 erläutert. Informieren Sie sich über die Funktionsweise von Zertifikatbindungen auf Apps mit einem einzelnen Mandanten in einer App Service-Umgebung.
author: madsd
ms.topic: overview
ms.date: 11/15/2021
ms.author: madsd
ms.openlocfilehash: 0ecc9f29ae469cea01c29a23c5491320c37e7cc5
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132520995"
---
# <a name="certificates-and-the-app-service-environment-v2"></a>Zertifikate und die App Service-Umgebung v2
> [!NOTE]
> In diesem Artikel wird die App Service-Umgebung v2 beschrieben, die mit isolierten App Service-Plänen verwendet wird
> 
Die App Service-Umgebung (ASE) ist eine Bereitstellung des Azure App Service, die in Ihrem Azure Virtual Network (VNet) ausgeführt wird. Sie kann mit einem über das Internet zugänglichen Anwendungsendpunkt oder einem Anwendungsendpunkt in Ihrem VNet bereitgestellt werden. Wenn Sie die ASE mit einem über das Internet zugänglichen Endpunkt bereitstellen, wird diese Bereitstellung als externe ASE bezeichnet. Wenn Sie die ASE mit einem Endpunkt in Ihrem VNet bereitstellen, wird diese Bereitstellung als IBL-ASE bezeichnet. Weitere Informationen zur ILB-ASE finden Sie im Dokument [Erstellen und Verwenden einer ILB-ASE](./create-ilb-ase.md).

Die ASE ist ein einzelnes Mandantensystem. Da es sich um einen einzelnen Mandanten handelt, gibt es einige Features, die nur mit einer ASE verfügbar sind und die im mehrinstanzenfähigen App Service nicht verfügbar sind. 

## <a name="ilb-ase-certificates"></a>ILB-ASE-Zertifikate 

Bei Verwendung einer externen ASE sind Ihre Apps unter „&lt;Name der App&gt;.&lt;Name der ASE&gt;.p.azurewebsites.net“ erreichbar. Standardmäßig werden alle ASEs, auch ILB-ASEs, mit Zertifikaten erstellt, die diesem Format folgen. Wenn Sie über eine ILB ASE verfügen, werden die Apps basierend auf dem Domänennamen erreicht, den Sie beim Erstellen der ILB-ASE angeben. Damit die Apps TLS unterstützen, müssen Sie Zertifikate hochladen. Ein gültiges TLS/SSL-Zertifikat können Sie erhalten, indem Sie interne Zertifizierungsstellen verwenden, ein Zertifikat von einem externen Aussteller erwerben oder ein selbstsigniertes Zertifikat verwenden. 

Es gibt zwei Möglichkeiten, Zertifikate mit Ihrer ILB-ASE zu konfigurieren.  Sie können ein Platzhalterzertifikat für die ILB-ASE festlegen oder Zertifikate für die einzelnen Web-Apps in der ASE festlegen.  Unabhängig davon, welche Auswahl Sie treffen, müssen die folgenden Zertifikatsattribute ordnungsgemäß konfiguriert sein:

- **:** Dieses Attribut muss für ein ILB-ASE-Platzhalterzertifikat auf *.[ihre-stammdomäne-hier] festgelegt werden. Wenn Sie das Zertifikat für Ihre App erstellen, dann sollte es [app-name].[ihre-stammdomäne-hier] lauten.
- **Alternativer Antragstellername**: Dieses Attribut muss sowohl *.[ihre-stammdomäne-hier] als auch *.scm.[ihre-stammdomäne-hier] für das ILB-ASE-Platzhalterzertifikat enthalten. Wenn Sie das Zertifikat für Ihre App erstellen, dann sollte es [app-name].[ihre-stammdomäne-hier] und [app-name].scm.[ihre-stammdomäne-hier] lauten.

Als dritte Variante können Sie ein ILB-ASE-Zertifikat erstellen, das alle Ihre individuellen App-Namen im SAN des Zertifikats anstelle einer Platzhalterreferenz enthält. Das Problem mit dieser Methode ist, dass Sie im Voraus die Namen der Apps wissen müssen, die Sie in die ASE einfügen, oder dass Sie das ILB-ASE-Zertifikat ständig aktualisieren müssen.

### <a name="upload-certificate-to-ilb-ase"></a>Hochladen des Zertifikats in die ILB-ASE 

Nachdem eine ILB-ASE im Portal erstellt wurde, muss das Zertifikat für die ILB-ASE festgelegt werden. Bis das Zertifikat festgelegt ist, zeigt die ASE ein Banner an, dass das Zertifikat nicht festgelegt wurde.  

Das Zertifikat, das Sie hochladen, muss eine PFX-Datei sein. Nach dem Hochladen des Zertifikats dauert es etwa 20 Minuten, bis das Zertifikat verwendet wird. 

Sie können die ASE nicht erstellen und das Zertifikat als eine Aktion im Portal oder sogar in einer Vorlage hochladen. Als separate Aktion können Sie das Zertifikat mithilfe einer Vorlage hochladen, wie im Dokument [Erstellen einer ASE aus einer Vorlage](./create-from-template.md) beschrieben.  

Wenn Sie schnell ein selbstsigniertes Zertifikat zum Testen erstellen möchten, können Sie das folgende Element von PowerShell verwenden:

```azurepowershell-interactive
$certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

$certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
$password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

$fileName = "exportedcert.pfx"
Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password
```

Wenn Sie ein selbstsigniertes Zertifikat erstellen, müssen Sie sicherstellen, dass der Betreffname das Format CN={ASE_NAME_HERE}_InternalLoadBalancingASE hat.

## <a name="application-certificates"></a>Anwendungszertifikate 

In einer ASE gehostete Apps können die anwendungsorientierten Zertifikatsfeatures nutzen, die im mehrinstanzenfähigen App Service verfügbar sind. Zu den Features zählen:  

- SNI-Zertifikate 
- IP-basiertes SSL, das nur mit einer externen ASE unterstützt wird.  Eine ILB-ASE unterstützt kein IP-basiertes SSL.
- KeyVault-gehostete Zertifikate 

Anweisungen zum Hochladen und Verwalten dieser Zertifikate finden Sie unter [Hinzufügen eines TLS/SSL-Zertifikats in Azure App Service](../configure-ssl-certificate.md).  Wenn Sie lediglich Zertifikate so konfigurieren, dass sie mit einem benutzerdefinierten Domänennamen übereinstimmen, den Sie Ihrer Web-App zugewiesen haben, dann reichen diese Anweisungen aus. Wenn Sie das Zertifikat für eine ILB-ASE-Web-App mit dem Standarddomänennamen hochladen, dann geben Sie die SCM-Website im SAN des Zertifikats an, wie bereits erwähnt. 

## <a name="tls-settings"></a>TLS-Einstellungen 

Sie können die TLS-Einstellung auf App-Ebene konfigurieren.  

## <a name="private-client-certificate"></a>Privates Clientzertifikat 

Ein häufiger Anwendungsfall ist die Konfiguration Ihrer App als Client in einem Client/Server-Modell. Wenn Sie Ihren Server mit einem privaten CA-Zertifikat schützen, müssen Sie das Clientzertifikat in Ihre App hochladen.  Die folgenden Anweisungen laden Zertifikate in den Truststore der Worker, in dem Ihre App ausgeführt wird. Wenn Sie das Zertifikat in eine App laden, können Sie es mit Ihren anderen Apps im gleichen App Service-Plan verwenden, ohne das Zertifikat erneut hochzuladen.

So laden Sie das Zertifikat in Ihre App in der ASE

1. Generieren Sie eine *CER*-Datei für Ihr Zertifikat. 
2. Wechseln Sie zu der App im Azure-Portal, die das Zertifikat benötigt.
3. Wechseln Sie zu den SSL-Einstellungen in der App. Klicken Sie auf „Zertifikat hochladen“. Wählen Sie „Öffentlich“ aus. Wählen Sie „Lokaler Computer“ aus. Geben Sie einen Namen ein. Navigieren Sie zu Ihrer *CER*-Datei und wählen Sie sie aus. Wählen Sie „Hochladen“ aus. 
4. Kopieren Sie den Fingerabdruck.
5. Wechseln Sie zu den Anwendungseinstellungen. Erstellen Sie die Anwendungseinstellung WEBSITE_LOAD_ROOT_CERTIFICATES mit dem Fingerabdruck als Wert. Wenn Sie über mehrere Zertifikate verfügen, können Sie diese in derselben Einstellung einfügen, getrennt durch Kommas und ohne Leerzeichen wie 

    84EC242A4EC7957817B8E48913E50953552DAFA6,6A5C65DC9247F762FE17BF8D4906E04FE6B31819

Das Zertifikat steht allen Apps im gleichen App Service-Plan zur Verfügung wie die App, die diese Einstellung konfiguriert hat. Wenn es für Apps in einem anderen App Service-Plan verfügbar sein soll, müssen Sie den Vorgang der Anwendungseinstellung in einer App in diesem App Service-Plan wiederholen. Um zu überprüfen, ob das Zertifikat festgelegt ist, wechseln Sie zur Kudu-Konsole, und geben Sie den folgenden Befehl in die PowerShell-Debugkonsole ein:

```azurepowershell-interactive
dir cert:\localmachine\root
```

Sie können zum Testen ein selbstsigniertes Zertifikat erstellen und eine *CER*-Datei mit der folgenden PowerShell generieren: 

```azurepowershell-interactive
$certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

$certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
$password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

$fileName = "exportedcert.cer"
export-certificate -Cert $certThumbprint -FilePath $fileName -Type CERT
```
