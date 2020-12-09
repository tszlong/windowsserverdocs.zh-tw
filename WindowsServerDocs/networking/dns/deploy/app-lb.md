---
title: 使用 DNS 原則進行應用程式負載平衡
description: 本主題是 Windows Server 2016 的 DNS 原則案例指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fa415f6c1b7065b0e5da6e83999ed425d9f6b28e
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865354"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>使用 DNS 原則進行應用程式負載平衡

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何設定 DNS 原則，以執行應用程式負載平衡。

舊版 Windows Server DNS 只提供使用迴圈配置資源回應的負載平衡;但是在 Windows Server 2016 中使用 DNS 時，您可以設定應用程式負載平衡的 DNS 原則。

當您已部署應用程式的多個實例時，您可以使用 DNS 原則來平衡不同應用程式實例之間的流量負載，進而動態配置應用程式的流量負載。

## <a name="example-of-application-load-balancing"></a>應用程式負載平衡範例

以下是您可以如何使用 DNS 原則進行應用程式負載平衡的範例。

這個範例會使用一個虛構的公司 Contoso 禮物服務，其提供線上 gifing 服務，以及一個名為 **contosogiftservices.com** 的網站。

Contosogiftservices.com 網站裝載在多個資料中心，每個資料中心都有不同的 IP 位址。

在北美洲（Contoso 禮物服務的主要市場）中，此網站裝載于三個資料中心：芝加哥、IL、達拉斯、德克薩斯州和西雅圖、WA。

西雅圖的網頁伺服器具有最佳的硬體設定，而且可以處理兩倍的負載，如同其他兩個網站一樣多。 Contoso 禮物服務希望應用程式流量以下列方式導向。

- 因為西雅圖網頁伺服器包含更多資源，所以會將一半的應用程式用戶端導向此伺服器
- 應用程式用戶端的四分之一會導向至達拉斯、德克薩斯州的資料中心
- 應用程式用戶端的四分之一會導向至芝加哥、IL、datacenter

下圖描述此案例。

![使用 DNS 原則的 DNS 應用程式負載平衡](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>應用程式負載平衡的運作方式

在使用此範例案例針對應用程式負載平衡設定 dns 伺服器的 dns 原則之後，DNS 伺服器會回應50% 的時間與西雅圖 Web 服務器位址，有25% 的時間使用達拉斯 web 伺服器位址，以及25% 的時間與芝加哥 Web 服務器位址。

因此，在 DNS 伺服器收到的四個查詢中，它會以兩個西雅圖的回應回應，每個都有兩個針對達拉斯和芝加哥的回應。

Dns 用戶端和解析程式/LDNS 會快取 DNS 記錄的其中一個可能的問題，因為用戶端或解析程式不會將查詢傳送到 DNS 伺服器，因此可能會干擾負載平衡。

您可以使用 \- \- 適用于 \( \) 應負載平衡之 DNS 記錄的低存留時間 TTL 值，來減輕此行為的影響。

### <a name="how-to-configure-application-load-balancing"></a>如何設定應用程式負載平衡

下列各節說明如何設定應用程式負載平衡的 DNS 原則。

#### <a name="create-the-zone-scopes"></a>建立區域範圍

您必須先為裝載的資料中心建立區域 contosogiftservices.com 的範圍。

區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，每個區域範圍都包含一組專屬的 DNS 記錄。 相同記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。

>[!NOTE]
>依預設，DNS 區域上會有區域範圍。 此區域範圍與區域具有相同的名稱，且舊版 DNS 作業可在此範圍內運作。

您可以使用下列 Windows PowerShell 命令來建立區域範圍。

```powershell
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"
```

如需詳細資訊，請參閱 [新增-DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope)

#### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records"></a>將記錄新增至區域範圍

現在您必須將代表 web 伺服器主機的記錄新增到區域範圍中。

在 **SeattleZoneScope** 中，您可以新增 [記錄 www.contosogiftservices.com] 與 [IP 位址 192.0.0.1] （位於西雅圖資料中心）。

在 **ChicagoZoneScope** 中，您可以在 \( \) 芝加哥資料中心新增具有 IP 位址182.0.0.1 的相同記錄 www.contosogiftservices.com。

同樣地，在 **DallasZoneScope** 中，您可以 \( \) 在芝加哥資料中心新增具有 IP 位址162.0.0.1 的記錄 www.contosogiftservices.com。

您可以使用下列 Windows PowerShell 命令將記錄新增至區域範圍。

```powershell
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope"
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
```

如需詳細資訊，請參閱 [DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord)。

#### <a name="create-the-dns-policies"></a><a name="bkmk_policies"></a>建立 DNS 原則

當您建立分割區 (區域範圍) 並已新增記錄之後，您必須建立可將傳入查詢分散到這些範圍的 DNS 原則，如此一來，contosogiftservices.com 的50% 查詢就會以西雅圖資料中心內 Web 服務器的 IP 位址回應，其餘的資料則會平均分散在芝加哥和達拉斯的資料中心之間。

您可以使用下列 Windows PowerShell 命令建立 DNS 原則，以平衡這三個資料中心之間的應用程式流量。

>[!NOTE]
>在下列範例命令中，expression – ZoneScope "SeattleZoneScope，2;ChicagoZoneScope，1;DallasZoneScope，1 "會使用包含參數組合的陣列來設定 DNS 伺服器 `<ZoneScope>` `<weight>` 。

```powershell
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
```

如需詳細資訊，請參閱 [DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy)。

您現在已成功建立 DNS 原則，可在三個不同的資料中心內跨 Web 服務器提供應用程式負載平衡。

您可以根據您的流量管理需求建立數千個 DNS 原則，而且會動態套用所有新原則，而不需要重新開機 DNS 伺服器上的傳入查詢。
