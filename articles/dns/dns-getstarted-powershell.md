---
title: 'Schnellstart: Erstellen einer Azure DNS-Zone und eines entsprechenden Eintrags – Azure PowerShell'
titleSuffix: Azure DNS
description: Erfahren Sie, wie Sie eine DNS-Zone und einen DNS-Eintrag in Azure DNS erstellen. Dies ist ein schrittweiser Schnellstart zum Erstellen und Verwalten Ihrer ersten DNS-Zone und Ihres ersten DNS-Eintrags mithilfe von Azure PowerShell.
services: dns
author: rohinkoul
ms.author: rohink
ms.date: 04/23/2021
ms.topic: quickstart
ms.service: dns
ms.custom: devx-track-azurepowershell, mode-api
ms.openlocfilehash: f30fd154f3998166381f4cf3a0341c023480bd5e
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2021
ms.locfileid: "131080040"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-powershell"></a>Schnellstart: Erstellen einer Azure DNS-Zone und eines entsprechenden Eintrags mithilfe von Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

In diesem Schnellstart erstellen Sie Ihre erste Azure DNS-Zone und einen -Eintrag mithilfe von Azure PowerShell. Sie können diese Schritte auch im [Azure-Portal](dns-getstarted-portal.md) oder mit der [Azure CLI](dns-getstarted-cli.md) ausführen. 

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt. Und schließlich müssen Sie die Namenserver für die Domäne konfigurieren, um Ihre DNS-Zone im Internet zu veröffentlichen. Jeder dieser Schritte wird im Folgenden beschrieben.

:::image type="content" source="media/dns-getstarted-portal/environment-diagram.png" alt-text="Diagramm: DNS-Bereitstellungsumgebung mit Azure PowerShell" border="false":::

Azure DNS unterstützt auch das Erstellen privater Domänen. Schritt-für-Schritt-Anweisungen zum Erstellen Ihrer ersten privaten DNS-Zone und des ersten Datensatzes finden Sie unter [Erste Schritte mit privaten Azure DNS-Zonen mithilfe von PowerShell](private-dns-getstarted-powershell.md).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Azure PowerShell (lokal installiert) oder Azure Cloud Shell

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Bevor Sie die DNS-Zone erstellen, erstellen Sie eine Ressourcengruppe für die DNS-Zone:

```powershell
New-AzResourceGroup -name MyResourceGroup -location "eastus"
```

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `New-AzDnsZone` -Cmdlet erstellt. Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.xyz* erstellt. Verwenden Sie das Beispiel, um eine DNS-Zone zu erstellen, und ersetzen Sie dabei die Werte durch Ihre eigenen.

```powershell
New-AzDnsZone -Name contoso.xyz -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Sie erstellen Ressourceneintragssätze mit dem Cmdlet `New-AzDnsRecordSet`. Im folgenden Beispiel wird ein Eintrag mit dem relativen Namen „www“ in der DNS-Zone „contoso.xyz“ in der Ressourcengruppe „MyResourceGroup“ erstellt. Der vollqualifizierte Name des Eintragssatzes ist „www.contoso.xyz“. Der Eintragstyp ist „A“, die IP-Adresse lautet „10.10.10.10“, und die Gültigkeitsdauer beträgt 3600 Sekunden.

```powershell
New-AzDnsRecordSet -Name www -RecordType A -ZoneName contoso.xyz -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "10.10.10.10")
```

## <a name="view-records"></a>Anzeigen von Datensätzen

Listen Sie die DNS-Einträge in Ihrer Zone mit diesem Befehl auf:

```powershell
Get-AzDnsRecordSet -ZoneName contoso.xyz -ResourceGroupName MyResourceGroup
```

## <a name="test-the-name-resolution"></a>Testen der Namensauflösung

Sie besitzen nun eine DNS-Testzone mit einem A-Testeintrag und können die Namensauflösung mit dem Tool *nslookup* testen. 

**Testen Sie die DNS-Namensauflösung wie folgt:**

1. Führen Sie das folgende Cmdlet aus, um die Liste mit den Namenservern für Ihre Zone abzurufen:

   ```azurepowershell
   Get-AzDnsRecordSet -ZoneName contoso.xyz -ResourceGroupName MyResourceGroup -RecordType ns
   ```

1. Kopieren Sie einen der Namen der Namenserver aus der Ausgabe des vorherigen Schritts.

1. Öffnen Sie eine Eingabeaufforderung, und führen Sie den folgenden Befehl aus:

   ```
   nslookup www.contoso.xyz <name server name>
   ```

   Beispiel:

   ```
   nslookup www.contoso.xyz ns1-08.azure-dns.com.
   ```

   Es sollte ein Bildschirm angezeigt werden, der in etwa wie folgt aussieht:

   ![Screenshot, der ein Eingabeaufforderungsfenster mit einem Namenserver-Suchbefehl und Werten für Server, Adresse, Name und Adresse zeigt.](media/dns-getstarted-portal/nslookup.PNG)

Der Hostname **www\.contoso.xyz** wird gemäß Ihrer Konfiguration zu **10.10.10.10** aufgelöst. Mit diesem Ergebnis wird bestätigt, dass die Namensauflösung richtig funktioniert.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn sie nicht mehr benötigt werden, können Sie alle in diesem Schnellstart erstellten Ressourcen löschen, indem Sie die Ressourcengruppe löschen:

```powershell
Remove-AzResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun Ihre erste DNS-Zone und Ihren ersten -Eintrag mit Azure PowerShell erstellt haben, können Sie Einträge für eine Web-App in einer benutzerdefinierten Domäne erstellen.

> [!div class="nextstepaction"]
> [Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne](./dns-web-sites-custom-domain.md)
