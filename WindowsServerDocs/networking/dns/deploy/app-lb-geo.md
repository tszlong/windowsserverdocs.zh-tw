---
title: 使用 DNS 原則進行具有地理位置感知的應用程式負載平衡
description: 瞭解如何設定 DNS 原則，以對具有地理位置感知的應用程式進行負載平衡。
manager: brianlic
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: b573edf16fc347ff85d4b9d7690935dc9164ec68
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949444"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>使用 DNS 原則進行具有地理位置感知的應用程式負載平衡

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何設定 DNS 原則，以對具有地理位置感知的應用程式進行負載平衡。

本指南的上一個主題中， [使用適用于應用程式負載平衡的 DNS 原則](./app-lb.md)，並使用虛構公司 Contoso 禮物服務的範例，其中提供了線上贈與服務，而且具有名為 Contosogiftservices.com 的網站。 Contoso 禮物服務會在位於西雅圖、WA、芝加哥、IL 和達拉斯，德克薩斯州的北美洲資料中心伺服器之間，將其線上 Web 應用程式負載平衡。

>[!NOTE]
>建議您先熟悉 [應用程式負載平衡的 DNS 原則](./app-lb.md) ，再執行此案例中的指示。

本主題將使用相同的虛構公司和網路基礎結構作為新範例部署的基礎，包括地理位置感知。

在此範例中，Contoso 禮物服務已成功將其目前狀態擴展到全球。

類似于北美洲，公司現在已有在歐洲資料中心裝載的 web 伺服器。

Contoso 禮物服務 DNS 系統管理員想要以類似于美國的 DNS 原則實施方式來設定歐洲資料中心的應用程式負載平衡，並將應用程式流量分散到位於都柏林、愛爾蘭、阿姆斯特丹、Holland 和其他位置的 Web 服務器。

DNS 系統管理員也希望世界各地其他位置的所有查詢平均分散在所有資料中心之間。

在接下來的幾節中，您可以瞭解如何在您自己網路上的 Contoso DNS 系統管理員中達成類似目標。

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>如何使用 Geo-Location 認知設定應用程式負載平衡

下列各節說明如何使用地理位置感知來設定應用程式負載平衡的 DNS 原則。

>[!IMPORTANT]
>下列各節包含範例 Windows PowerShell 包含許多參數範例值的命令。 執行這些命令之前，請務必將這些命令中的範例值取代為適合您部署的值。

### <a name="create-the-dns-client-subnets"></a><a name="bkmk_clientsubnets"></a>建立 DNS 用戶端子網

您必須先識別北美洲和歐洲地區的子網或 IP 位址空間。

您可以從地理 IP 地圖取得此資訊。 根據這些地理 IP 散發套件，您必須建立 DNS 用戶端子網。

DNS 用戶端子網是 IPv4 或 IPv6 子網的邏輯群組，會將查詢傳送到 DNS 伺服器。

您可以使用下列 Windows PowerShell 命令建立 DNS 用戶端子網。

```powershell
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
```

如需詳細資訊，請參閱 [DnsServerClientSubnet](/powershell/module/dnsserver/add-dnsserverclientsubnet)。

### <a name="create-the-zone-scopes"></a><a name="bkmk_zscopes2"></a>建立區域範圍

用戶端子網就緒之後，您必須將區域 contosogiftservices.com 分割成不同的區域範圍，每個區域都適用于資料中心。

區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，每個區域範圍都包含一組專屬的 DNS 記錄。 相同記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。

>[!NOTE]
>依預設，DNS 區域上會有區域範圍。 此區域範圍與區域具有相同的名稱，且舊版 DNS 作業可在此範圍內運作。

先前的應用程式負載平衡案例會示範如何在北美洲中為資料中心設定三個區域範圍。

使用下列命令，您可以建立兩個以上的區域範圍，每個範圍分別適用于都柏林和阿姆斯特丹的資料中心。

您可以新增這些區域範圍，而不需要變更相同區域中的三個現有北美洲區域範圍。 此外，在建立這些區域範圍之後，您就不需要重新開機您的 DNS 伺服器。

您可以使用下列 Windows PowerShell 命令來建立區域範圍。

```powershell
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
```

如需詳細資訊，請參閱 [新增-DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope)

### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records2"></a>將記錄新增至區域範圍

現在您必須將代表 web 伺服器主機的記錄新增到區域範圍中。

先前的案例中已新增北美洲資料中心的記錄。 您可以使用下列 Windows PowerShell 命令，將記錄新增至歐洲資料中心的區域範圍。

```powershell
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
```

如需詳細資訊，請參閱 [DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord)。

### <a name="create-the-dns-policies"></a><a name="bkmk_policies2"></a>建立 DNS 原則

當您建立分割區 (區域範圍) 並新增記錄之後，您必須建立可將傳入查詢分散到這些範圍的 DNS 原則。

在此範例中，跨不同資料中心的應用程式伺服器之間的查詢散發符合下列準則。

1. 從北美洲用戶端子網中的來源接收 DNS 查詢時，50% 的 DNS 回應會指向西雅圖資料中心，25% 的回應會指向芝加哥的資料中心，而剩餘的25% 回應會指向達拉斯的資料中心。
2. 從歐洲用戶端子網中的來源接收 DNS 查詢時，50% 的 DNS 回應會指向都柏林資料中心，而50% 的 DNS 回應會指向阿姆斯特丹資料中心。
3. 當查詢來自世界各地時，會將 DNS 回應分散到所有五個資料中心。

您可以使用下列 Windows PowerShell 命令來執行這些 DNS 原則。

```powershell
Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
```

如需詳細資訊，請參閱 [DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy)。

您現在已成功建立 DNS 原則，可在多個洲的五個不同資料中心內的 Web 服務器上，提供應用程式負載平衡。

您可以根據您的流量管理需求建立數千個 DNS 原則，而且會動態套用所有新原則，而不需要重新開機 DNS 伺服器上的傳入查詢。
