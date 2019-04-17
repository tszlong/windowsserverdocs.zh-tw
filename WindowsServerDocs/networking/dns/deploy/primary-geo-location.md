---
title: 地理位置的使用 DNS 原則的資料傳輸主要伺服器管理
description: 本主題是 DNS 原則案例指南適用於 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd72e13cd3d2d7f3ca886afcdcf97e824ef227f5
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>地理位置的使用 DNS 原則的資料傳輸主要伺服器管理

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何設定 DNS 原則，以允許主要回應 DNS client 查詢 client 和資源嘗試 client 連接的地理位置為基礎的 DNS 伺服器 client 提供接近資源的 IP 位址。  
  
>[!IMPORTANT]  
>本案例示範如何部署的地理位置資料傳輸管理 DNS 原則，當您正在使用只主要 DNS 伺服器。 當您有主要及次要 DNS 伺服器，您也可以完成地理位置資料傳輸管理。 如果您的主要次要部署，先完成此主題中的步驟，然後完成此主題中所提供的步驟[使用 DNS 原則主要次要部署的地理位置型流量管理的](primary-secondary-geo-location.md)。

有了新的 DNS 原則，您可以建立 DNS 原則的允許要求的網頁伺服器的 IP 位址 client 查詢回應 DNS 伺服器。 執行個體的網頁伺服器可能位於在不同的所在位置的不同資料中心。 DNS 可以評估 client 與 Web 伺服器位置，然後 client 要求提供 client 與 Web 伺服器的 IP 位址位於實際靠近 client 的網頁伺服器的回應。  

您可以使用下列的 DNS 原則參數控制從 DNS 用查詢的 DNS 伺服器回應。 

- **Client 子網路**。 預先定義的 client 子網路的名稱。 用來確認寄查詢子網路。
- **傳輸通訊協定**。 傳輸通訊協定查詢中使用。 可能的項目是**UDP**和**TCP**。
- **網際網路通訊協定**。 用於查詢網路通訊協定。 可能的項目是**IPv4**和**IPv6**。
- **伺服器介面 IP 位址**。 網路介面收到 DNS 要求的 DNS 伺服器的 IP 位址。
- **FQDN**。 完全完整網域名稱 (FQDN) 查詢中的資料的使用萬用字元可使用。
- **查詢輸入**。 記錄查詢（A、SRV、TXT 等）的類型。
- **一天的時間**。 查詢收到的時間。 
  
您可以在邏輯電信業者（和/或）制訂原則運算式結合下列條件。 這些運算式比對，當原則應該可以執行下列任何動作。
 
- **忽略**。 DNS 伺服器以無訊息方式卸除查詢。          
- **拒絕**。 DNS 伺服器看查詢做出的回應失敗。          
- **讓**。 DNS 伺服器會再與管理流量回應回應。          
  
##  <a name="bkmk_example"></a>地理位置型流量管理範例

以下是使用 DNS 原則達到流量重新導向以執行 DNS 查詢 client 的所在位置為基礎的範例。   
  
此範例中使用兩個虛構公司-Contoso 雲端服務，提供網頁和網域裝載方案。及 Woodgrove 食物服務提供多個城市的食物傳送服務全球有名 woodgrove.com 的網站。  
  
Contoso 雲端服務將有兩個資料中心，其中在美國和歐洲另一個。 歐洲 datacenter 主控訂購 woodgrove.com 入口網站食物。   
  
為了確保 woodgrove.com 針對回應式體驗從他們的網站，Woodgrove 想要歐洲戶端導向歐洲 datacenter 美國戶端導向美國資料中心。 針對其他地方找到世界可以導向的資料中心。   
  
下圖描述此案例。  
  
![地理位置型流量管理範例](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>如何 DNS 名稱解析程序運作方式  
  
在名稱解析過程中，使用者會嘗試 www.woodgrove.com 連接。這會導致 DNS 名稱解析要求傳送給中網路使用者的電腦上屬性設定 DNS 伺服器。 一般而言，這是本機 ISP 做為 [快取器，提供的 DNS 伺服器，並且指 LDNS。   
  
如果不 LDNS 本機快取中顯示的 DNS 名稱，LDNS 伺服器會將 DNS 伺服器的 woodgrove.com 授權查詢。授權 DNS 伺服器的 LDNS 伺服器，依序傳送到電腦的使用者之前本機快取記錄回應要求記錄 (www.woodgrove.com)。  
  
因為 Contoso 雲端服務會使用 DNS 伺服器原則，該主機 contoso.com 返回地理位置設定授權 DNS 伺服器根據流量受管理的回應。 這會導致方向的歐洲用歐洲 datacenter 和的美國用的方向來美國 datacenter，如下圖所示。  
  
在本案例中的授權 DNS 伺服器，通常會看到提供從 LDNS 伺服器，很少，使用者電腦的名稱解析要求。 因此中所見授權 DNS 伺服器的名稱解析要求, 來源 IP 位址是電腦的 LDNS 伺服器的而非使用者。 不過，當您設定的地理位置時使用 LDNS 伺服器的 IP 位址根據查詢回應提供公平估計的地理位置的使用者，因為使用者查詢他本機 ISP 的 DNS 伺服器。  
  
>[!NOTE]  
>DNS 原則利用寄件者 IP 中包含了 DNS 查詢 UDP 與 TCP 封包。 如果查詢達到透過多個解析日 LDNS 躍點主要伺服器，原則會將只的 IP 的 DNS 伺服器接收查詢的最後一個解析。  
  
##  <a name="bkmk_config"></a>如何 DNS 原則設定的地理位置型查詢回應  
若要設定的地理位置查詢回應 DNS 原則，您必須執行下列步驟。  
  
1. [建立 DNS Client 子網路](#bkmk_subnets)  
2. [建立的區域的領域](#bkmk_scopes)  
3. [若要的區域領域加入資料](#bkmk_records)  
4. [[建立原則](#bkmk_policies)  
  
>[!NOTE]  
>您必須是針對您想要設定的區域授權的 DNS 伺服器上執行這些步驟。 資格在**DnsAdmins**，或等，才能執行下列程序。  
  
下列章節提供詳細的設定指示操作。  
  
>[!IMPORTANT]  
>以下的各節包含包含許多參數值範例範例 Windows PowerShell 命令。 請確認值是適用於您的部署，執行下列命令之前，先取代範例值這些命令列中。  
  
### <a name="bkmk_subnets"></a>建立 DNS Client 子網路  
  
找出子網路的 IP 位址，您想要重新導向流量地區空間是第一個步驟。 例如，如果您想要將流量美國和歐洲重新導向，您需要找出子網路的 IP 位址空間這些地區。  
  
您可以從地理 IP 「 地圖 」 來取得此資訊。 依據這些地理 IP 散發，您必須建立」DNS Client 子」。 DNS Client 子網路是 IPv4 或 IPv6 子網路，查詢會傳送至 DNS 伺服器的邏輯群組。  
  
若要建立 DNS Client 子網路，您可以使用下列的 Windows PowerShell 命令。  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
如需詳細資訊，請查看[新增-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps)。  
  
### <a name="bkmk_scopes"></a>建立區域範圍  
Client 子網路設定之後，您必須磁碟分割的流量您想要重新導向至兩種不同的區域範圍，領域 DNS Client 子網路，您所設定的區域。   
  
例如，如果您想要重新導向之 DNS 名稱 www.woodgrove.com 流量，您必須建立兩種不同的區域範圍 woodgrove.com 區域，另一個用於美國和歐洲的其中一個。  
  
時區領域是區域的唯一執行個體。 DNS 區域可以有多個區域領域，與每個包含 DNS 記錄它自己設定的區域範圍。 相同記錄可能會出現在多個領域，以不同的 IP 位址或相同的 IP 位址。  
  
>[!NOTE]  
>根據預設，區域領域存在於 DNS 區域。 這個區域領域區域具有相同的名稱，並在這個領域中工作舊版 DNS 作業。   
  
您可以使用下列的 Windows PowerShell 命令來建立區域範圍。  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

如需詳細資訊，請查看[新增-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)。  
  
### <a name="bkmk_records"></a>若要的區域領域加入資料  
現在，您必須將記錄代表網頁伺服器主機成兩個區域範圍。   
  
例如，**USZoneScope**和**EuropeZoneScope**。 您可以在 USZoneScope、加入記錄 www.woodgrove.com IP 位址 192.0.0.1 位於美國資料中心。並在 EuropeZoneScope 您可以使用的 IP 位址 141.1.0.1 歐洲 datacenter 中新增相同記錄 (www.woodgrove.com)。   
  
您可以使用下列 Windows PowerShell 命令若要的區域領域加入資料。  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
在此範例中，您必須以確保世界的其餘部分仍然可以存取 woodgrove.com 網頁伺服器的其中兩個 datacenter 預設區域領域中新增記錄也使用下列 Windows PowerShell 命令。  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
**ZoneScope**當您新增記錄中的預設範圍時，不包含參數。 這是標準 DNS 時區新增記錄相同。  
  
如需詳細資訊，請查看[新增-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps)。  
  
### <a name="bkmk_policies"></a>[建立原則  
子網路建立後的磁碟分割（區域領域），而且您已新增記錄、查詢回應 DNS client 子網路的來源查詢時，會傳回正確的範圍的區域的您必須建立連接子網路和的磁碟分割的原則。 不原則所需的對應區域預設範圍。   
  
您可以使用下列的 Windows PowerShell 命令來建立 DNS 原則連結 DNS Client 子網路，以及區域範圍。   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
如需詳細資訊，請查看[新增-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。  
  
立即所需的 DNS 原則，將根據地理位置資料傳輸與設定的 DNS 伺服器。  
  
當 DNS 伺服器接收名稱解析查詢時、DNS 伺服器評估 DNS 要求針對 DNS 原則設定中的欄位。 如果名稱解析要求來源 IP 位址比對任何原則，相關的區域範圍用來回應查詢，和使用者導向它們地理位置最接近的資源。   
  
您可以建立數千 DNS 原則根據您的資料傳輸管理的需求，且所有的新原則已經套用動態-不需要重新 DNS 伺服器-連入查詢。  
  
  
