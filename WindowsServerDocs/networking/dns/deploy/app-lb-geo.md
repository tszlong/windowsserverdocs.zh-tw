---
title: 使用 DNS 原則進行具有地理位置感知的應用程式負載平衡
description: 本主題是 Windows Server 2016 DNS 原則案例指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 00195c4993f3e5bef9688adbfd09f62f908b6276
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996933"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>使用 DNS 原則進行具有地理位置感知的應用程式負載平衡

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何設定 DNS 原則，以對具有地理位置感知的應用程式進行負載平衡。

本指南中的上一個主題[使用適用于應用程式負載平衡的 DNS 原則](./app-lb.md)，使用虛構公司的範例-Contoso 禮物服務-提供線上贈與服務，以及具有名為 Contosogiftservices.com 的網站。 Contoso 禮品服務會在位於美國西雅圖、WA、芝加哥、IL 和達拉斯、德克薩斯州的北美資料中心內的伺服器之間，進行其線上 Web 應用程式的負載平衡。

>[!NOTE]
>在執行此案例中的指示之前，建議您先熟悉[使用適用于應用程式負載平衡的 DNS 原則](./app-lb.md)主題。

本主題使用相同的虛構公司和網路基礎結構，作為包含地理位置感知的新範例部署基礎。

在此範例中，Contoso 禮物服務已成功將其目前狀態擴展到全球各地。

與北美洲類似，公司現在已有在歐洲資料中心內裝載的 web 伺服器。

Contoso 禮品服務 DNS 系統管理員想要以類似美國的 DNS 原則執行方式，設定歐洲資料中心的應用程式負載平衡，並將應用程式流量分散到位於都柏林、愛爾蘭、阿姆斯特丹、Holland 和其他位置的 Web 服務器。

DNS 系統管理員也希望世界各地其他位置的所有查詢在其資料中心之間均等分散。

在接下來的章節中，您可以瞭解如何達到您自己網路上 Contoso DNS 系統管理員的類似目標。

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>如何設定具有地理位置感知的應用程式負載平衡

下列各節說明如何使用地理位置感知設定應用程式負載平衡的 DNS 原則。

>[!IMPORTANT]
>下列各節包含範例 Windows PowerShell 命令，其中包含許多參數的範例值。 執行這些命令之前，請務必將這些命令中的範例值取代為適用于您的部署的值。

### <a name="create-the-dns-client-subnets"></a><a name="bkmk_clientsubnets"></a>建立 DNS 用戶端子網

您必須先識別北美洲和歐洲地區的子網或 IP 位址空間。

您可以從地理 IP 對應取得此資訊。 根據這些地理 IP 散發套件，您必須建立 DNS 用戶端子網。

DNS 用戶端子網是 IPv4 或 IPv6 子網的邏輯群組，會將查詢傳送到 DNS 伺服器。

您可以使用下列 Windows PowerShell 命令來建立 DNS 用戶端子網。

```powershell
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
```

如需詳細資訊，請參閱[DnsServerClientSubnet](/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。

### <a name="create-the-zone-scopes"></a><a name="bkmk_zscopes2"></a>建立區域範圍

在用戶端子網備妥之後，您必須將區域 contosogiftservices.com 分割成不同的區域範圍，每個都適用于資料中心。

區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，其中每個區域範圍都包含自己的一組 DNS 記錄。 相同的記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。

>[!NOTE]
>根據預設，區域範圍會存在於 DNS 區域中。 此區域範圍與區域具有相同的名稱，且舊版 DNS 作業會在此範圍內運作。

先前的應用程式負載平衡案例示範如何在北美洲中設定資料中心的三個區域範圍。

使用下列命令，您可以建立兩個更多區域範圍，分別用於都柏林和阿姆斯特丹資料中心。

您可以新增這些區域範圍，而不需要變更相同區域中的三個現有北美洲區域範圍。 此外，在建立這些區域範圍之後，您不需要重新開機您的 DNS 伺服器。

您可以使用下列 Windows PowerShell 命令來建立區域範圍。

```powershell
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
```

如需詳細資訊，請參閱[DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records2"></a>將記錄新增至區域範圍

現在您必須將代表 web 伺服器主機的記錄新增到區域範圍中。

先前的案例中已加入北美洲資料中心的記錄。 您可以使用下列 Windows PowerShell 命令，將記錄新增至歐洲資料中心的區域範圍。

```powershell
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
```

如需詳細資訊，請參閱[DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。

### <a name="create-the-dns-policies"></a><a name="bkmk_policies2"></a>建立 DNS 原則

建立分割區 (區域範圍之後) 並新增記錄之後，您必須建立可將傳入查詢分散到這些範圍的 DNS 原則。

在此範例中，跨不同資料中心的應用程式伺服器的查詢散發符合下列準則。

1. 從北美洲用戶端子網中的來源接收 DNS 查詢時，50% 的 DNS 回應會指向西雅圖資料中心，而25% 的回應會指向芝加哥資料中心，而其餘25% 的回應會指向達拉斯資料中心。
2. 從歐洲用戶端子網上的來源接收 DNS 查詢時，50% 的 DNS 回應會指向都柏林資料中心，而50% 的 DNS 回應會指向阿姆斯特丹資料中心。
3. 當查詢來自世界各地的任何地方時，DNS 回應會分散在所有五個資料中心。

您可以使用下列 Windows PowerShell 命令來執行這些 DNS 原則。

```powershell
Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
```

如需詳細資訊，請參閱[DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。

您現在已成功建立 DNS 原則，以在多洲的五個不同資料中心內的網頁伺服器上提供應用程式負載平衡。

您可以根據您的流量管理需求來建立數以千計的 DNS 原則，而所有的新原則都會以動態方式套用，而不需要重新開機 DNS 伺服器上的傳入查詢。