---
title: DNS 原則概觀
description: 本主題是 DNS 原則案例指南適用於 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06086bbd7edc2fa489805eb5075062332e002ab4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="dns-policies-overview"></a>DNS 原則概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要深入了解 DNS 原則，這是在 Windows Server 2016 中的新功能，您可以使用此主題。 您可以使用地理位置資料傳輸管理依據時間、 智慧型 DNS 回應 DNS 原則管理設定為 split\ 蛋部署，單一 DNS 伺服器上 DNS 查詢套用篩選。 下列項目提供更多有關這些功能的詳細資料。

-   **應用程式負載平衡。** 當您完成部署多次應用程式的不同位置時，您可以使用 DNS 原則，以平衡流量載入之間動態應用程式的配置流量載入的不同的應用程式執行個體。

-   **Geo\ 位置型流量管理。** 您可以使用 DNS 原則，以允許回應 DNS client 查詢 client 和資源嘗試 client 連接的地理位置為基礎的主要和次要 DNS 伺服器 client 提供接近資源的 IP 位址。 

-   **請分割大腦 DNS。** 、 Split\ 蛋 DNS 記錄分為不同區域範圍上相同的 DNS 伺服器，並 DNS 用收到依據戶端是否內部或外部用的回應。 您可以設定 split\ 蛋 DNS 整合 Active Directory 區域或區域獨立 DNS 伺服器上。

-   **篩選。** 您可以設定來建立查詢篩選器根據您所提供的準則 DNS 原則。 查詢篩選 DNS 原則中的，讓您設定根據 DNS 查詢和傳送 DNS 查詢 DNS client 的方式自訂回應 DNS 伺服器。 
-   **Forensics。** 您可以將 non\ 存在 IP 位址，而不將它們想瀏覽電腦惡意 DNS 用使用 DNS 原則。

-   **一天的時間型重新導向。** 您可以將應用程式流量跨不同的分散執行個體的應用程式，使用 DNS 原則根據一天的時間，使用 DNS 原則。

## <a name="new-concepts"></a>新的概念  
為了建立上面所列的案例的支援原則，則必須無法辨識的區域的網路、其他項目之間上用群組中記錄群組。 這些項目會以下列新 DNS 物件：  

- **Client 子網路：** client 子網路物件代表 IPv4 或 IPv6 子網路，查詢提交 DNS 伺服器。 您可以建立子網路，若要稍後定義根據要求來自何種子網路上套用原則。 例如，在分割大腦 DNS 案例中，要求解析度的名稱，例如*www.microsoft.com*可以回答從內部子網路，以戶端內部 IP 位址和戶端外部子網路中不同的 IP 位址。

- **遞迴範圍：**遞迴領域的唯一的執行個體群組的控制遞迴 DNS 伺服器上的設定。 遞迴範圍包含轉送程式的清單，並指定遞迴是否已支援。 DNS 伺服器可以有許多遞迴範圍。 DNS 伺服器遞迴原則可讓您選擇的一組查詢遞迴範圍。 如果不是授權 DNS 伺服器某些查詢、DNS 伺服器遞迴原則可讓您控制如何解析那些查詢。 您可以指定哪些轉寄使用，以及是否要使用遞迴器。

- **區域領域：** DNS 區域可以有多個區域領域，與每個包含他們自己的 DNS 記錄設定的區域範圍。 相同記錄可能會出現在多個領域，以不同的 IP 位址。 此外，區域轉送完成區域範圍層級。 這表示記錄區域中的範圍主要區域會將它們傳輸到相同的時區範圍在次要的區域。

## <a name="types-of-policy"></a>原則的類型

DNS 原則的狀態層級的類型。 您可以使用查詢解析度原則，以定義查詢處理和區域傳輸原則，以定義區域轉送發生的方式。 您可以將套用原則各種伺服器層級或區域層級。
  
### <a name="query-resolution-policies"></a>查詢解析度原則

您可以使用 DNS 查詢解析度原則，來指定如何傳入的解析度查詢由 DNS 伺服器。 每個 DNS 查詢解析度原則包含下列項目：  
  
|欄位|描述|可能的值|  
|---------|---------------|-------------------|  
|**名稱**|原則的名稱|-最多 256 個字元<br />-可以包含任何字元有效的檔案名稱|  
|**狀態**|原則狀態|讓（預設值）<br />-停用|  
|**層級**|原則層級|伺服器<br />區域|  
|**處理訂單**|一旦查詢歸類層級，適用於伺服器找到的查詢符合的條件，並套用查詢的第一個原則|-數值<br />每個原則包含相同層級，以及適用於值唯一值|  
|**控制項目**|DNS 伺服器所執行的動作|-允許（預設值區域層級）<br />-拒絕伺服器層級（預設值）<br />-忽略|  
|**條件**|原則條件（和（或)，以及原則套用符合的條件的清單|條件電信業者（和/或）<br />-（請條件如下表所示）標準清單|  
|**範圍**|時區範圍和每個範圍加權的值的清單。 用於負載平衡 distribution 加權的值。 例如，如果此清單會包含 datacenter1 的 3 輕量的以及 datacenter2 與減重 5 個伺服器將回應記錄從 datacentre1 退出 8 要求三次|式（依名稱）區域範圍和重量清單|  

> [!NOTE]
> 伺服器層級原則僅能值**拒絕**或**略過**為動作。

DNS 原則條件欄位是由兩個項目所組成：

|名稱|描述|範例值|
|--------|---------------|-----------------|
|**Client 子網路**|傳輸通訊協定查詢中使用。 可能的項目是**UDP**和**TCP**|-   **法國 EQ、西班牙、** -解析為 true 如果子網路被視為西班牙或法國<br />-   **墨西哥 NE、加拿大、** -解析為 true client 子網路是否加拿大和墨西哥以外的任何子網路|  
|**傳輸通訊協定**|傳輸通訊協定查詢中使用。 可能的項目是**UDP**和**TCP**|-   **EQ TCP**<br />-   **EQ UDP**|  
|**網際網路通訊協定**|用於查詢網路通訊協定。 可能的項目是**IPv4**和**IPv6**|-   **EQ IPv4**<br />-   **EQ IPv6**|  
|**伺服器介面 IP 位址**|IP 位址，連入 DNS 伺服器網路介面|-   **EQ 10.0.0.1**<br />-   **EQ 192.168.1.1**|  
|**FQDN**|Server 的 FQDN 記錄在查詢時，可能會使用萬用字元與|-   **EQ，www.contoso.com** -解析 tot rue 只 if 查詢嘗試解析*www.contoso.com* FQDN<br />-   **EQ,\*.contoso.com,\*.woodgrove.com** -解析為 true 如果查詢任何記錄結尾的*contoso.com***或者***woodgrove.com*|  
|**查詢類型**|使用碼表進行正在類型查詢 (A，SVR，TXT)|-   **EQ，TXT，SRV** -解析 tot rue，如果查詢要求 TXT**或者**SRV 記錄<br />-   **EQ、MX** -解析 tot rue，如果查詢 MX 記錄要求|  
|**一天的時間**|查詢收到一天的時間|-   **22:00 23:00，10:00-12:00，EQ** -解析 tot rue，如果查詢收到 10 上午之間正午，**或者**之間下午 10 及 11 PM|  
  
使用上的表做為起點，如下表所示無法用來定義的條件，用來與查詢記錄但 SRV 記錄來自 10.0.0.0 24 子網路中 client TCP 透過 8 之間 10 PM 透過介面 10.0.0.3 contoso.com 網域中的任何類型：  
  
|名稱|值。|  
|--------|---------|  
|Client 子網路|EQ，10.0.0.0 24|  
|傳輸通訊協定|EQ TCP|  
|伺服器介面 IP 位址|EQ 10.0.0.3|  
|FQDN|EQ，*。contoso.com|  
|查詢類型|NE SRV|  
|一天的時間|20:00 22:00，EQ|  
  
可以建立多個查詢解析度原則相同的層級，只要有不同的值為處理訂單。 多個原則可用時，DNS 伺服器處理輸入查詢以下列方式：  
  
![DNS 原則處理](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  
  
### <a name="recursion-policies"></a>遞迴原則  
遞迴原則的特殊**輸入**的伺服器層級的原則。 遞迴原則控制 DNS 伺服器執行遞迴查詢的方式。 遞迴原則套用只有當查詢處理到達遞迴路徑。 您可以選擇遞迴的一組查詢 DENY 或略過的值。 或者，您可以選擇的一組查詢轉送程式的設定。  
  
您可以使用原則遞迴實作 Split-brain DNS 設定。 此設定，DNS 伺服器執行遞迴的用查詢，如一組時 DNS 伺服器不會執行適用於其他戶端該查詢遞迴。  
  
遞迴原則包含相同的項目包含一般的 DNS 查詢解析度原則，加上的元素，如下表所示：  
  
|名稱|描述|  
|--------|---------------|  
|**適用於在遞迴**|指定這項原則僅適用於遞迴使用。|  
|**遞迴範圍**|遞迴領域的名稱。|  
  
> [!NOTE]  
> 遞迴原則只能建立伺服器層級。  
  
### <a name="zone-transfer-policies"></a>時區傳輸原則  
時區傳輸原則控制是否允許區域或不是您的 DNS 伺服器。 您可以建立區域轉送原則伺服器層級或區域層級。 伺服器層級原則套用發生 DNS 伺服器上的每個區域傳輸查詢。 時區層級原則套用只會在查詢 DNS 伺服器上的區域。 最常見的時區層級原則使用是實作封鎖或安全的清單。  
  
> [!NOTE]  
> 時區傳輸原則只能使用 DENY 或略過為動作。  
  
您可以使用伺服器層級區域傳輸原則下列拒絕區域轉送 contoso.com 網域從給定的子網路：  
  
```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfCOnsotostoFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  
  
可以建立多個區域轉送原則相同的層級，只要有不同的值為處理訂單。 多個原則可用時，DNS 伺服器處理輸入查詢以下列方式：  
  
![多個區域傳輸原則 DNS 程序](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  
  
## <a name="managing-dns-policies"></a>管理 DNS 原則  
您可以建立和透過 PowerShell 管理 DNS 原則。 以下範例瀏覽不同樣本，您可以透過 DNS 原則設定：  
  
### <a name="traffic-management"></a>交通管理  
您可以直接流量根據 FQDN 到不同的伺服器根據 DNS client 的位置。 下列範例顯示如何建立流量管理原則，以直接從至北美 datacenter 特定子網路和歐洲 datacenter 到另一個子網路針對。  
  
```  
Add-DnsServerClientSubnet -Name "NorthAmericaSubnet" -IPv4Subnet "172.21.33.0/24"  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "172.17.44.0/24"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "NorthAmericaZoneScope"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.17.97.97" -ZoneScope "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.21.21.21" -ZoneScope "NorthAmericaZoneScope"  
Add-DnsServerQueryResolutionPolicy -Name "NorthAmericaPolicy" -Action ALLOW -ClientSubnet "eq,NorthAmericaSubnet" -ZoneScope "NorthAmericaZoneScope,1" -ZoneName "Contoso.com"  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName contoso.com  
```  
  
第一次並列的指令碼建立 client 適用於北美地區和歐洲子網路物件。 並列之後，建立一個每個地區 contoso.com 網域中的時區範圍。 並列之後，在不同的 IP 位址、歐洲的另一個適用於北美地區 ww.contoso.com 將相關聯的每個區域建立記錄。 最後，指令碼的最後一個行建立兩個 DNS 查詢解析度原則，其中套用到北美子網路，另一個歐洲子網路。  
  
### <a name="block-queries-for-a-domain"></a>封鎖查詢的網域  
您可以使用 DNS 查詢解析度原則封鎖查詢網域。 以下範例封鎖所有查詢 treyresearch.net:  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  
  
### <a name="block-queries-from-a-subnet"></a>子網路從封鎖查詢  
您也可以封鎖來自特定子網路查詢。 下方的指令碼建立子網路的 172.0.33.0 月 24，，然後建立的原則，忽略來自子網路所有查詢：  
  
```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  
  
### <a name="allow-recursion-for-internal-clients"></a>允許內部戶端遞迴  
您可以控制遞迴，使用 DNS 查詢解析度原則。 以下的範例可用於賦予遞迴內部用戶端的外部戶端分割大腦案例中停用。  
  
```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  
  
第一次列指令碼中的變更預設遞迴範圍，只要名為「。」（若要停用遞迴點。） 在第二列建立遞迴範圍名為*InternalClients*以遞迴支援。 第三行建立套用原則和新建立透過伺服器的 IP 位址 10.0.0.34 介面提供任何查詢遞迴範圍。  
  
### <a name="create-a-server-level-zone-transfer-policy"></a>建立伺服器區層級傳輸原則  
您可以控制區域轉送更精細的形式透過區域轉送原則。 以下範例指令碼用於允許指定子網路上的任何伺服器區傳輸：  
  
```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  
  
第一次列指令碼中的建立子網路物件名為*AllowedSubnet*封鎖 IP 172.21.33.0 24。 在第二列建立允許先前建立的子網路上的任何 DNS 伺服器區域轉送區域傳輸原則。  
  
### <a name="create-a-zone-level-zone-transfer-policy"></a>建立區域層級區域傳輸原則  
您也可以建立區域層級區域傳輸原則。 以下範例忽略 contoso.com 來自伺服器的介面，可的 10.0.0.33 的 IP 位址的區域轉移任何要求：  
  
```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  
  
## <a name="dns-policy-scenarios"></a>DNS 原則案例

如有關如何使用 DNS 原則的特定案例，請查看本指南下列主題。

- [地理位置的使用 DNS 原則的資料傳輸主要伺服器管理](primary-geo-location.md)  
- [地理位置型主要次要部署的流量管理，使用 DNS 原則](primary-secondary-geo-location.md)  
- [使用 DNS 原則智慧 DNS 回應根據一天的時間](dns-tod-intelligent.md)
- [根據一天中 Azure 時間 DNS 回應雲端伺服器的應用程式](dns-tod-azure-cloud-app-server.md)
- [使用 DNS 原則 Split-Brain DNS 部署](split-brain-DNS-deployment.md)
- [使用 DNS 原則 Split-Brain DNS Active Directory 中](dns-sb-with-ad.md)
- [使用 DNS 原則 DNS 查詢上套用篩選](apply-filters-on-dns-queries.md)
- [使用應用程式負載平衡 DNS 原則](app-lb.md)
- [使用應用程式負載平衡的地理位置感知 DNS 原則](app-lb-geo.md)


