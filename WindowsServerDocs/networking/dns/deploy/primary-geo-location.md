---
title: 透過主要伺服器使用地理位置流量管理的 DNS 原則
description: 本主題是 DNS 原則案例指南的 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 110014adf1e23be574f23efc01e8a4d69397e547
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831979"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>透過主要伺服器使用地理位置流量管理的 DNS 原則

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解如何設定 DNS 原則以允許主要 DNS 伺服器回應 DNS 用戶端查詢，根據地理位置的用戶端和資源的用戶端嘗試連線，用戶端提供 IP ad包裝法之最接近的資源。  
  
>[!IMPORTANT]  
>此案例說明如何部署地理位置流量管理的 DNS 原則，當您使用只有主要 DNS 伺服器。 當您有主要和次要 DNS 伺服器時，您也可以完成地理位置流量管理。 如果您有主要-次要部署，先完成本主題中的步驟，然後完成 本主題中所提供的步驟[使用與主要-次要部署地理位置型流量管理的DNS原則](primary-secondary-geo-location.md).

使用新的 DNS 原則，您可以建立可讓 DNS 伺服器回應要求的網頁伺服器的 IP 位址的用戶端查詢的 DNS 原則。 Web 伺服器執行個體可能位於不同實體位置的不同資料中心。 DNS 可以評估用戶端和 Web 伺服器位置，然後藉由提供用戶端與 Web 伺服器的 IP 位址位於實體上更接近用戶端的 Web 伺服器回應對用戶端要求。  

您可以使用下列的 DNS 原則參數來控制來自 DNS 用戶端查詢 DNS 伺服器回應。 

- **用戶端子網路**。 預先定義的用戶端的子網路名稱。 用來驗證傳送查詢的子網路。
- **傳輸通訊協定**。 傳輸通訊協定查詢中使用。 可能的項目都**UDP**並**TCP**。
- **網際網路通訊協定**。 在查詢中使用的網路通訊協定。 可能的項目都**IPv4**並**IPv6**。
- **伺服器介面的 IP 位址**。 DNS 伺服器收到 DNS 要求的網路介面的 IP 位址。
- **FQDN**。 完整格式網域名稱 (FQDN) 的記錄，在查詢中，可能會使用萬用字元。
- **查詢類型**。 （A、 SRV、 TXT 等） 進行查詢的記錄的類型。
- **當日時間**。 在收到查詢的日期時間。 
  
使用邏輯運算子 （和/或） 以制定原則運算式，您可以結合下列的準則。 當這些運算式相符時，原則應該執行下列動作之一。
 
- **忽略**。 DNS 伺服器以無訊息方式卸除的查詢。          
- **拒絕**。 DNS 伺服器回應失敗回應與該查詢。          
- **允許**。 DNS 伺服器上一步以管理流量的回應。          
  
##  <a name="bkmk_example"></a>地理位置型流量管理範例

以下是如何使用 DNS 原則以達到根據用戶端，執行 DNS 查詢的實體位置的流量重新導向的範例。   
  
這個範例會使用兩個虛構公司服務-Contoso 的雲端服務提供 web 與網域託管解決方案，為 Woodgrove 餐飲業，它會提供在多個城市的食物傳遞服務全球各地且具有網站 woodgrove.com。  
  
Contoso 的雲端服務有兩個資料中心，一個在美國和歐洲的另一個。 在歐洲資料中心裝載食物，排序 woodgrove.com 的入口網站。   
  
若要確保 woodgrove.com 客戶，從其網站取得回應的體驗，Woodgrove 想歐洲的用戶端導向到歐洲資料中心和美國的用戶端導向至在美國資料中心。 在其他地方位於世界各地的客戶可以導向至其中一個資料中心。   
  
下圖說明此案例。  
  
![地理位置型流量管理範例](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>DNS 名稱解析程序會如何運作  
  
在名稱解析過程中，使用者會嘗試連線到 www.woodgrove.com。 這會導致 DNS 名稱解析要求傳送到已在使用者電腦上的網路連接屬性中設定的 DNS 伺服器。 一般而言，這是做為快取的解析程式中，本機 ISP 所提供的 DNS 伺服器，而且會被視為 LDNS。   
  
如果 DNS 名稱不存在於 LDNS 的本機快取，LDNS 伺服器將查詢轉寄到 woodgrove.com 的授權 DNS 伺服器。 授權 DNS 伺服器回應與要求記錄 (www.woodgrove.com) LDNS 伺服器接著會記錄快取在本機，再將它傳送給使用者的電腦。  
  
Contoso 的雲端服務會使用 DNS 伺服器的原則，因為授權 DNS 伺服器裝載 contoso.com 已傳回地理位置流量管理回應。 這會導致歐洲的用戶端的方向在歐洲資料中心和美國的用戶端的方向，美國資料中心，在圖中所述。  
  
在此案例中，授權 DNS 伺服器通常會看到來自 LDNS 伺服器而，很少，來自使用者的電腦名稱解析要求。 因為這個緣故，在授權 DNS 伺服器所見，才會進行名稱解析要求的來源 IP 位址會是電腦的 LDNS 伺服器而非使用者。 不過，當您設定地理位置時，使用 LDNS 伺服器的 IP 位址基礎回應提供使用者地理位置的合理估計的查詢因為使用者查詢他的當地 ISP 的 DNS 伺服器。  
  
>[!NOTE]  
>DNS 原則會使用 UDP/TCP 封包，其中包含將 DNS 查詢中的寄件者 IP。 如果查詢達到透過多個的解析程式/LDNS 躍點的主要伺服器，該原則會考慮的 DNS 伺服器收到查詢的最後一個解析程式 IP。  
  
##  <a name="bkmk_config"></a>如何設定基礎查詢回應的地理位置的 DNS 原則  
若要設定的地理位置查詢回應的 DNS 原則，您必須執行下列步驟。  
  
1. [建立 DNS 用戶端的子網路](#bkmk_subnets)  
2. [建立區域的範圍](#bkmk_scopes)  
3. [將記錄新增至區域範圍](#bkmk_records)  
4. [建立原則](#bkmk_policies)  
  
>[!NOTE]  
>您必須有權管理您想要設定區域的 DNS 伺服器上執行這些步驟。 中的成員資格**DnsAdmins**，或同等權限，才能執行下列程序。  
  
下列各節提供詳細的設定指示。  
  
>[!IMPORTANT]  
>下列各節包含 Windows PowerShell 命令範例包含許多參數的範例值。 請確定這些命令列中的範例值取代是適用於您的部署，然後再執行這些命令的值。  
  
### <a name="bkmk_subnets"></a>建立 DNS 用戶端的子網路  
  
第一個步驟是識別的子網路或 IP 位址空間，您要將流量重新導向的區域。 例如，如果您想要將流量重新導向適用於美國和歐洲，您需要識別的子網路或 IP 位址空間，這些區域。  
  
您可以從異地 IP 對應來取得這項資訊。 根據這些地理 IP 散發套件，您必須建立 「 DNS 用戶端的子網路。 」 DNS 用戶端的子網路是從中查詢傳送至 DNS 伺服器的 IPv4 或 IPv6 子網路的邏輯群組。  
  
您可以使用下列 Windows PowerShell 命令來建立 DNS 用戶端的子網路。  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="bkmk_scopes"></a>建立區域範圍  
用戶端子網路設定之後，您必須分割您想要重新導向至兩個不同的區域範圍，其流量一個範圍，您已設定 DNS 用戶端子網路的每個區域。   
  
比方說，如果您想要將 DNS 名稱 www.woodgrove.com 流量重新導向，您必須建立兩個不同的區域範圍 woodgrove.com 區域、 一個用於美國和歐洲的其中一個。  
  
區域範圍內是區域的唯一的執行個體。 DNS 區域可以有多個區域範圍，與包含它自己的 DNS 記錄集的每個區域範圍。 同一筆記錄中可以存在多個領域，具有不同 IP 位址或相同的 IP 位址。  
  
>[!NOTE]  
>根據預設，在區域範圍存在於 DNS 區域。 此區域範圍與區域有相同的名稱，此範圍上運作的舊版 DNS 作業。   
  
您可以使用下列 Windows PowerShell 命令來建立區域範圍。  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="bkmk_records"></a>將記錄新增至區域範圍  
現在，您必須新增代表 web 伺服器主機的兩個區域範圍的記錄。   
  
例如， **USZoneScope**並**EuropeZoneScope**。 USZoneScope，在中，您可以新增位於美國資料中心; 的 IP 位址 192.0.0.1，記錄 www.woodgrove.com而且 EuropeZoneScope 中您可以將相同的記錄 (www.woodgrove.com) 141.1.0.1 歐洲資料中心內的 IP 位址。   
  
您可以使用下列 Windows PowerShell 命令，將記錄新增至區域範圍。  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
在此範例中，您也必須使用下列 Windows PowerShell 命令將記錄新增至預設的區域範圍，以確保，在世界的其餘部分仍然可以存取 woodgrove.com 網頁伺服器從兩個資料中心。  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
**ZoneScope**預設範圍中的記錄時，未包含參數。 這是將記錄新增至標準 DNS 區域相同。  
  
如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
### <a name="bkmk_policies"></a>建立原則  
建立子網路之後，資料分割 （區域範圍），而且您已新增記錄，您必須建立連接的子網路和資料分割的原則，以便中 DNS 用戶端子網路的其中一個來源的查詢時，會傳回查詢回應正確的範圍內的區域。 沒有任何原則所需的對應預設區域範圍。   
  
您可以使用下列 Windows PowerShell 命令來建立 DNS 原則，DNS 用戶端的子網路連結和區域範圍。   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
現在已設定 DNS 伺服器，以必要的 DNS 原則，將根據地理位置的流量重新導向。  
  
當 DNS 伺服器收到名稱解析查詢時，DNS 伺服器會評估 DNS 要求，根據設定的 DNS 原則中的欄位。 如果在 名稱解析要求的來源 IP 位址會符合任何原則，相關聯的區域範圍來回應查詢，並將使用者導向的地理位置最接近它們的資源。   
  
您可以建立數以千計的 DNS 原則根據您的流量管理需求，而且所有新的原則都會套用動態-不需要重新啟動 DNS 伺服器-在傳入的查詢。  
  
  
