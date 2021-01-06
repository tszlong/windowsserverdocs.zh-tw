---
title: 透過主要伺服器使用地理位置流量管理的 DNS 原則
description: 瞭解如何設定 DNS 原則，以允許主要 DNS 伺服器根據用戶端嘗試連線的用戶端和資源的地理位置來回應 DNS 用戶端查詢，為用戶端提供最接近資源的 IP 位址。
manager: brianlic
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2aa9e1392f133c5f076eac3b508851ae88c41d03
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904193"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>透過主要伺服器使用地理位置流量管理的 DNS 原則

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何設定 DNS 原則，以允許主要 DNS 伺服器根據用戶端嘗試連線的用戶端和資源的地理位置來回應 DNS 用戶端查詢，為用戶端提供最接近資源的 IP 位址。

>[!IMPORTANT]
>此案例說明當您只使用主要 DNS 伺服器時，如何部署以地理位置為基礎的流量管理的 DNS 原則。 當您同時有主要和次要 DNS 伺服器時，您也可以完成以地理位置為基礎的流量管理。 如果您有主要次要部署，請先完成本主題中的步驟，然後完成本主題中所提供的步驟： [使用 DNS 原則進行以 Geo-Location 為基礎的流量管理與 Primary-Secondary 部署](primary-secondary-geo-location.md)。

使用新的 DNS 原則，您可以建立 DNS 原則，讓 DNS 伺服器回應用戶端查詢，要求提供 Web 服務器的 IP 位址。 Web 服務器的實例可能位於不同的實體位置不同的資料中心。 DNS 可以評估用戶端和網頁伺服器的位置，然後為用戶端要求提供 web 伺服器 IP 位址，該網頁伺服器實際上位於用戶端。

您可以使用下列 DNS 原則參數，來控制 DNS 用戶端對查詢的 DNS 伺服器回應。

- **用戶端子網**。 預先定義之用戶端子網的名稱。 用來驗證傳送查詢的子網。
- **傳輸通訊協定**。 查詢中所使用的傳輸通訊協定。 可能的專案為 **UDP** 和 **TCP**。
- **網際網路通訊協定**。 查詢中使用的網路通訊協定。 可能的專案為 **IPv4** 和 **IPv6**。
- **伺服器介面 IP 位址**。 接收 DNS 要求的 DNS 伺服器之網路介面的 IP 位址。
- **FQDN**。 查詢中記錄的完整功能變數名稱 (FQDN) ，而且可能會使用萬用字元。
- **查詢類型**。 正在查詢的記錄類型 (A、SRV、TXT 等 ) 。
- **一天中的時間**。 收到查詢的當日時間。

您可以使用邏輯運算子來結合下列準則 (和/或) 來制定原則運算式。 當這些運算式相符時，原則應該會執行下列其中一個動作。

- **Ignore**。 DNS 伺服器會以無訊息模式卸載查詢。
- **拒絕**。 DNS 伺服器會回應失敗回應的查詢。
- **允許**。 DNS 伺服器會回應流量管理的回應。

##  <a name="geo-location-based-traffic-management-example"></a><a name="bkmk_example"></a>以地理位置為基礎的流量管理範例

以下範例說明如何根據執行 DNS 查詢之用戶端的實體位置，使用 DNS 原則來達成流量重新導向。

此範例使用兩個虛構公司-Contoso 雲端服務，可提供 web 和網域裝載解決方案;和 Woodgrove 食物服務，在全球各地的多個城市提供食物傳遞服務，其中有一個名為 woodgrove.com 的網站。

Contoso 雲端服務有兩個資料中心，一個在美國，另一個在歐洲。 歐洲的資料中心裝載了 woodgrove.com 的食物訂購入口網站。

為了確保 woodgrove.com 客戶能夠從其網站獲得回應式體驗，Woodgrove 希望歐洲用戶端導向到歐洲資料中心，而將美國的客戶導向至美國的資料中心。 位於全球其他位置的客戶可以導向至其中一個資料中心。

下圖描述此案例。

![以 Geo-Location 為基礎的流量管理範例](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)

##  <a name="how-the-dns-name-resolution-process-works"></a><a name="bkmk_works"></a>DNS 名稱解析處理的運作方式

在名稱解析程式期間，使用者會嘗試連接至 www.woodgrove.com。 這會導致 DNS 名稱解析要求傳送至使用者電腦上網路連線屬性中所設定的 DNS 伺服器。 一般來說，這是作為快取解析程式的本機 ISP 所提供的 DNS 伺服器，也稱為 LDNS。

如果 DNS 名稱不存在於 LDNS 的本機快取中，LDNS 伺服器就會將查詢轉送至 woodgrove.com 的授權 DNS 伺服器。 授權 DNS 伺服器會以要求的記錄回應 (www.woodgrove.com) 傳送到 LDNS 伺服器，然後在本機快取記錄，然後再將記錄傳送至使用者的電腦。

因為 Contoso 雲端服務會使用 DNS 伺服器原則，所以裝載 contoso.com 的授權 DNS 伺服器會設定為傳回以地理位置為基礎的流量管理回應。 如此一來，歐洲的用戶端就會得到歐洲資料中心的方向，以及美國的資料中心向美國資料中心的方向，如圖所示。

在此案例中，授權 DNS 伺服器通常會看到來自 LDNS 伺服器的名稱解析要求，而且很少是來自使用者的電腦。 因此，授權 DNS 伺服器所見的名稱解析要求中的來源 IP 位址，是 LDNS 伺服器的來源 IP 位址，而不是使用者電腦的位址。 但是，當您設定以地理位置為基礎的查詢回應時，若使用 LDNS 伺服器的 IP 位址，就會提供使用者地理位置的公平估計，因為使用者會查詢本機 ISP 的 DNS 伺服器。

>[!NOTE]
>DNS 原則會利用 UDP/TCP 封包中包含 DNS 查詢的寄件者 IP。 如果查詢透過多個解析程式/LDNS 躍點到達主伺服器，原則就只會考慮到 DNS 伺服器接收查詢的最後一個解析程式 IP。

##  <a name="how-to-configure-dns-policy-for-geo-location-based-query-responses"></a><a name="bkmk_config"></a>如何針對以 Geo-Location 為基礎的查詢回應設定 DNS 原則
若要針對以地理位置為基礎的查詢回應設定 DNS 原則，您必須執行下列步驟。

1. [建立 DNS 用戶端子網](#bkmk_subnets)
2. [建立區域的範圍](#bkmk_scopes)
3. [將記錄新增至區域範圍](#bkmk_records)
4. [建立原則](#bkmk_policies)

>[!NOTE]
>您必須在您想要設定之區域的授權 DNS 伺服器上執行這些步驟。 若要執行下列程式，必須要有 **DnsAdmins** 的成員資格或同等許可權。

下列各節提供詳細的設定指示。

>[!IMPORTANT]
>下列各節包含範例 Windows PowerShell 包含許多參數範例值的命令。 執行這些命令之前，請務必將這些命令中的範例值取代為適合您部署的值。

### <a name="create-the-dns-client-subnets"></a><a name="bkmk_subnets"></a>建立 DNS 用戶端子網

第一個步驟是識別您要將流量重新導向之區域的子網或 IP 位址空間。 例如，如果您想要重新導向美國和歐洲的流量，您需要識別這些區域的子網或 IP 位址空間。

您可以從地理 IP 地圖取得此資訊。 根據這些地理 IP 散發套件，您必須建立「DNS 用戶端子網」。 DNS 用戶端子網是 IPv4 或 IPv6 子網的邏輯群組，會將查詢傳送到 DNS 伺服器。

您可以使用下列 Windows PowerShell 命令建立 DNS 用戶端子網。

```powershell
Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"
```

如需詳細資訊，請參閱 [DnsServerClientSubnet](/powershell/module/dnsserver/add-dnsserverclientsubnet)。

### <a name="create-zone-scopes"></a><a name="bkmk_scopes"></a>建立區域範圍
設定用戶端子網之後，您必須將想要重新導向至兩個不同區域範圍的流量分割成一個範圍，也就是您已設定的每個 DNS 用戶端子網都有一個範圍。

例如，如果您想要重新導向 DNS 名稱 www.woodgrove.com 的流量，您必須在 woodgrove.com 區域中建立兩個不同的區域範圍，一個用於美國，另一個用於歐洲。

區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，每個區域範圍都包含一組專屬的 DNS 記錄。 相同記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。

>[!NOTE]
>依預設，DNS 區域上會有區域範圍。 此區域範圍與區域具有相同的名稱，且舊版 DNS 作業適用于此範圍。

您可以使用下列 Windows PowerShell 命令來建立區域範圍。

```powershell
Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"
Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"
```

如需詳細資訊，請參閱 [DnsServerZoneScope](/powershell/module/dnsserver/add-dnsserverzonescope)。

### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records"></a>將記錄新增至區域範圍
現在您必須將代表 web 伺服器主機的記錄新增至兩個區域範圍。

例如， **USZoneScope** 和 **EuropeZoneScope**。 在 USZoneScope 中，您可以新增具有 IP 位址192.0.0.1 的記錄 www.woodgrove.com，其位於美國資料中心。此外，在 EuropeZoneScope 中，您可以將相同的記錄新增 (www.woodgrove.com) ，以及在歐洲資料中心內141.1.0.1 的 IP 位址。

您可以使用下列 Windows PowerShell 命令將記錄新增至區域範圍。

```powershell
Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope
Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"
```

在此範例中，您也必須使用下列 Windows PowerShell 命令將記錄新增至預設區域範圍，以確保世界的其餘部分仍然可以從這兩個資料中心的任一個存取 woodgrove.com web 伺服器。

```powershell
Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"
Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
```

當您在預設範圍中新增記錄時，不會包含 **ZoneScope** 參數。 這與將記錄新增至標準 DNS 區域相同。

如需詳細資訊，請參閱 [DnsServerResourceRecord](/powershell/module/dnsserver/add-dnsserverresourcerecord)。

### <a name="create-the-policies"></a><a name="bkmk_policies"></a>建立原則
當您建立子網、磁碟分割 (區域範圍) ，而且您已新增記錄之後，您必須建立可連接子網和磁碟分割的原則，如此一來，當查詢來自其中一個 DNS 用戶端子網中的來源時，就會從正確的區域範圍傳回查詢回應。 對應預設區域範圍不需要任何原則。

您可以使用下列 Windows PowerShell 命令建立 DNS 原則，以連結 DNS 用戶端子網和區域範圍。

```powershell
Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"
```

如需詳細資訊，請參閱 [DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy)。

現在 DNS 伺服器已設定必要的 DNS 原則，以根據地理位置來重新導向流量。

當 DNS 伺服器收到名稱解析查詢時，DNS 伺服器會根據設定的 DNS 原則來評估 DNS 要求中的欄位。 如果名稱解析要求中的來源 IP 位址符合任何原則，則會使用相關聯的區域範圍來回應查詢，並將使用者導向至地理位置最接近的資源。

您可以根據您的流量管理需求建立數千個 DNS 原則，而且會動態套用所有新原則，而不需要重新開機 DNS 伺服器上的傳入查詢。
