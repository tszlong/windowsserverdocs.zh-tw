---
title: 透過主要伺服器使用地理位置流量管理的 DNS 原則
description: 本主題是 Windows Server 2016 DNS 原則案例指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5c74ca9fe60374d1bc1396d95c2e34cc5cd1fdd6
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317760"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>透過主要伺服器使用地理位置流量管理的 DNS 原則

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解如何設定 DNS 原則，以允許主要 DNS 伺服器根據用戶端和用戶端嘗試連線之資源的地理位置來回應 DNS 用戶端查詢，提供用戶端 IP最接近資源的位址。  
  
>[!IMPORTANT]  
>此案例說明當您只使用主要 DNS 伺服器時，如何針對以地理位置為基礎的流量管理部署 DNS 原則。 當您同時有主要和次要 DNS 伺服器時，您也可以完成地理位置型流量管理。 如果您有主要-次要部署，請先完成本主題中的步驟，然後完成將[DNS 原則用於地理位置型流量管理與主要-次要部署](primary-secondary-geo-location.md)主題中所提供的步驟。

使用新的 DNS 原則時，您可以建立 DNS 原則，讓 DNS 伺服器回應用戶端查詢，要求提供 Web 服務器的 IP 位址。 網頁伺服器的實例可能位於不同實體位置的不同資料中心。 DNS 可以評估用戶端和網頁伺服器的位置，然後藉由提供用戶端 web 伺服器 IP 位址給實體位於靠近用戶端的 Web 服務器，來回應用戶端要求。  

您可以使用下列 DNS 原則參數來控制 DNS 伺服器對 DNS 用戶端之查詢的回應。 

- **用戶端子網**。 預先定義的用戶端子網名稱。 用來驗證傳送查詢的子網。
- **傳輸通訊協定**。 查詢中使用的傳輸通訊協定。 可能的專案為**UDP**和**TCP**。
- **網際網路通訊協定**。 查詢中使用的網路通訊協定。 可能的專案為**IPv4**和**IPv6**。
- **伺服器介面 IP 位址**。 接收 DNS 要求的 DNS 伺服器網路介面的 IP 位址。
- **FQDN**。 查詢中記錄的完整功能變數名稱（FQDN），而且可能會使用萬用字元。
- **查詢類型**。 查詢的記錄類型（A、SRV、TXT 等）。
- **一天中的時間**。 接收查詢的當日時間。 
  
您可以使用邏輯運算子（和/或）來結合下列準則，以制訂原則運算式。 當這些運算式相符時，原則應該會執行下列其中一項動作。
 
- **忽略**。 DNS 伺服器會以無訊息方式卸載查詢。          
- **拒絕**。 DNS 伺服器會以失敗回應來回應該查詢。          
- **允許**。 DNS 伺服器會以流量管理的回應回應。          
  
##  <a name="geo-location-based-traffic-management-example"></a><a name="bkmk_example"></a>以地理位置為基礎的流量管理範例

以下範例示範如何使用 DNS 原則，以執行 DNS 查詢的用戶端實體位置為基礎，達到流量重新導向。   
  
這個範例使用兩個虛構公司-Contoso 雲端服務，提供 web 和網域裝載解決方案;和 Woodgrove 食物服務，可提供全球多個城市中的食物傳遞服務，以及具有名為 woodgrove.com 的網站。  
  
Contoso 雲端服務有兩個資料中心，一個位於美國，另一個在歐洲。 歐洲資料中心裝載 woodgrove.com 的食物訂購入口網站。   
  
為了確保 woodgrove.com 客戶從其網站獲得回應式體驗，Woodgrove 想要將歐洲的用戶端導向歐洲資料中心，並將美國客戶導向至美國資料中心。 位於世界各地的客戶可以導向到其中一個資料中心。   
  
下圖描述此案例。  
  
![以地理位置為基礎的流量管理範例](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="how-the-dns-name-resolution-process-works"></a><a name="bkmk_works"></a>DNS 名稱解析程式的運作方式  
  
在名稱解析程式期間，使用者會嘗試連接到 www.woodgrove.com。 這會導致 DNS 名稱解析要求傳送至使用者電腦上網路線上內容中所設定的 DNS 伺服器。 一般來說，這是由本機 ISP 提供作為快取解析程式的 DNS 伺服器，稱為「LDNS」（caching）。   
  
如果 DNS 名稱不存在於 LDNS 的本機快取中，則 LDNS 伺服器會將查詢轉送到具有 woodgrove.com 授權的 DNS 伺服器。 授權 DNS 伺服器會以要求的記錄（ www.woodgrove.com）回應 LDNS 伺服器，然後在將記錄傳送到使用者的電腦之前，先在本機上快取記錄。  
  
因為 Contoso 雲端服務使用 DNS 伺服器原則，所以裝載 contoso.com 的授權 DNS 伺服器會設定為傳回以地理位置為基礎的流量管理回應。 這會導致歐洲的用戶端到歐洲資料中心的方向，以及美國資料中心的美國用戶端方向，如下圖所示。  
  
在此案例中，授權 DNS 伺服器通常會看到來自 LDNS 伺服器的名稱解析要求，而且很少會從使用者的電腦。 因此，授權 DNS 伺服器所看到的名稱解析要求中的來源 IP 位址，就是 LDNS 伺服器的，而不是使用者的電腦。 不過，當您設定以地理位置為基礎的查詢回應時，使用 LDNS 伺服器的 IP 位址會提供使用者地理位置的公平估計，因為使用者會查詢其本機 ISP 的 DNS 伺服器。  
  
>[!NOTE]  
>DNS 原則會利用包含 DNS 查詢的 UDP/TCP 封包中的寄件者 IP。 如果查詢透過多個解析程式/LDNS 躍點到達主伺服器，原則將只會考慮 DNS 伺服器接收查詢之最後一個解析程式的 IP。  
  
##  <a name="how-to-configure-dns-policy-for-geo-location-based-query-responses"></a><a name="bkmk_config"></a>如何設定以地理位置為基礎的查詢回應的 DNS 原則  
若要針對以地理位置為基礎的查詢回應設定 DNS 原則，您必須執行下列步驟。  
  
1. [建立 DNS 用戶端子網](#bkmk_subnets)  
2. [建立區域的範圍](#bkmk_scopes)  
3. [將記錄新增至區域範圍](#bkmk_records)  
4. [建立原則](#bkmk_policies)  
  
>[!NOTE]  
>您必須在您想要設定之區域的授權 DNS 伺服器上執行這些步驟。 若要執行下列程式，必須要有**DnsAdmins**的成員資格或同等許可權。  
  
下列各節提供詳細的設定指示。  
  
>[!IMPORTANT]  
>下列各節包含範例 Windows PowerShell 命令，其中包含許多參數的範例值。 執行這些命令之前，請務必將這些命令中的範例值取代為適用于您的部署的值。  
  
### <a name="create-the-dns-client-subnets"></a><a name="bkmk_subnets"></a>建立 DNS 用戶端子網  
  
第一個步驟是識別您想要將流量重新導向之區域的子網或 IP 位址空間。 例如，如果您想要重新導向「美國」和「歐洲」的流量，則需要識別這些區域的子網或 IP 位址空間。  
  
您可以從地理 IP 對應取得此資訊。 根據這些地理 IP 散發套件，您必須建立「DNS 用戶端子網」。 DNS 用戶端子網是 IPv4 或 IPv6 子網的邏輯群組，會將查詢傳送到 DNS 伺服器。  
  
您可以使用下列 Windows PowerShell 命令來建立 DNS 用戶端子網。  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
如需詳細資訊，請參閱[DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="create-zone-scopes"></a><a name="bkmk_scopes"></a>建立區域範圍  
設定用戶端子網之後，您必須將想要重新導向其流量的區域分割成兩個不同的區域範圍，也就是您已設定的每個 DNS 用戶端子網都有一個範圍。   
  
例如，如果您想要重新導向 DNS 名稱 www.woodgrove.com 的流量，您必須在 woodgrove.com 區域中建立兩個不同的區域範圍，一個用於美國，一個用於歐洲。  
  
區域範圍是區域的唯一實例。 DNS 區域可以有多個區域範圍，其中每個區域範圍都包含自己的一組 DNS 記錄。 相同的記錄可以出現在多個範圍中，具有不同的 IP 位址或相同的 IP 位址。  
  
>[!NOTE]  
>根據預設，區域範圍會存在於 DNS 區域中。 此區域範圍的名稱與區域相同，且舊版 DNS 作業會在此範圍內運作。   
  
您可以使用下列 Windows PowerShell 命令來建立區域範圍。  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

如需詳細資訊，請參閱[DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="add-records-to-the-zone-scopes"></a><a name="bkmk_records"></a>將記錄新增至區域範圍  
現在您必須將代表 web 伺服器主機的記錄新增到這兩個區域範圍中。   
  
例如， **USZoneScope**和**EuropeZoneScope**。 在 USZoneScope 中，您可以使用位於美國資料中心的 IP 位址192.0.0.1 來新增記錄 www.woodgrove.com;而在 EuropeZoneScope 中，您可以使用歐洲資料中心內的 IP 位址141.1.0.1 來新增相同的記錄（ www.woodgrove.com）。   
  
您可以使用下列 Windows PowerShell 命令，將記錄新增至區域範圍。  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
在此範例中，您也必須使用下列 Windows PowerShell 命令將記錄新增至預設區域範圍，以確保世界的其他人仍然可以從這兩個資料中心的其中一個來存取 woodgrove.com web 伺服器。  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
當您在預設範圍中新增記錄時，不會包含**ZoneScope**參數。 這與將記錄新增至標準 DNS 區域相同。  
  
如需詳細資訊，請參閱[DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
### <a name="create-the-policies"></a><a name="bkmk_policies"></a>建立原則  
建立子網、分割區（區域範圍）並新增記錄之後，您必須建立用來連接子網和分割區的原則，如此一來，當查詢來自其中一個 DNS 用戶端子網中的來源時，就會從傳回查詢回應區域的正確範圍。 對應預設區域範圍不需要任何原則。   
  
您可以使用下列 Windows PowerShell 命令來建立連結 DNS 用戶端子網和區域範圍的 DNS 原則。   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
如需詳細資訊，請參閱[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
現在，DNS 伺服器已設定必要的 DNS 原則，以根據地理位置來重新導向流量。  
  
當 DNS 伺服器收到名稱解析查詢時，DNS 伺服器會根據所設定的 DNS 原則來評估 DNS 要求中的欄位。 如果名稱解析要求中的來源 IP 位址符合任何原則，則會使用相關聯的區域範圍來回應查詢，而使用者會被導向到地理位置最接近的資源。   
  
您可以根據您的流量管理需求來建立數以千計的 DNS 原則，而所有的新原則都會以動態方式套用，而不需要重新開機 DNS 伺服器上的傳入查詢。  
  
  
