---
title: DNS 原則概觀
description: 本主題是 Windows Server 2016 DNS 原則案例指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 613bb7f43b382389dc0db953a48668147cfaee88
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356050"
---
# <a name="dns-policies-overview"></a>DNS 原則概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解 DNS 原則，這是 Windows Server 2016 中的新功能。 您可以將 DNS 原則用於以地理位置為基礎的流量管理、以當天時間為基礎的智慧型 DNS 回應、管理設定為分割 @ no__t-0brain 部署的單一 DNS 伺服器、在 DNS 查詢上套用篩選等等。 下列專案提供有關這些功能的詳細資料。

-   **應用程式負載平衡。** 當您已在不同位置部署應用程式的多個實例時，可以使用 DNS 原則來平衡不同應用程式實例之間的流量負載，以動態方式配置應用程式的流量負載。

-   **異地 @ no__t-以1Location 為基礎的流量管理。** 您可以使用 DNS 原則，允許主要和次要 DNS 伺服器根據用戶端和用戶端嘗試連線之資源的地理位置來回應 DNS 用戶端查詢，為用戶端提供最接近的 IP 位址resource. 

-   **分割大腦 DNS。** 使用 split @ no__t-0brain DNS 時，DNS 記錄會分割成相同 DNS 伺服器上的不同區域範圍，而且 DNS 用戶端會根據用戶端是內部或外部用戶端來接收回應。 您可以針對 Active Directory 整合區域或獨立 DNS 伺服器上的區域，設定 split @ no__t-0brain DNS。

-   **濾除.** 您可以設定 DNS 原則，根據您提供的準則建立查詢篩選器。 DNS 原則中的查詢篩選器可讓您設定 DNS 伺服器，以自訂的方式，根據傳送 DNS 查詢的 DNS 查詢和 DNS 用戶端進行回應。 
-   **鑒識.** 您可以使用 DNS 原則，將惡意的 DNS 用戶端重新導向至非 @ no__t 0existent 的 IP 位址，而不是將它們引導至他們嘗試連線的電腦。

-   **以天為基礎的重新導向時間。** 您可以使用 DNS 原則，利用以當天時間為基礎的 DNS 原則，將應用程式流量分散到應用程式的不同地理位置上。

## <a name="new-concepts"></a>新概念  
為了建立原則以支援上列案例，必須能夠識別區域中的記錄群組、網路上的用戶端群組，以及其他元素。 這些元素會以下列新的 DNS 物件表示：  

- **用戶端子網：** 用戶端子網物件代表將查詢提交至 DNS 伺服器的 IPv4 或 IPv6 子網。 您可以建立子網，稍後根據要求的來源，定義要套用的原則。 比方說，在 split 大腦 DNS 案例中，可以使用內部子網的用戶端，將內部 IP 位址解析為<em>www.microsoft.com</em>名稱的解析要求，並將不同的 ip 位址回應給外部子網中的用戶端。

- **遞迴範圍：** 遞迴範圍是一組設定的唯一實例，可控制 DNS 伺服器上的遞迴。 遞迴範圍包含轉寄站的清單，並指定是否啟用遞迴。 DNS 伺服器可以有許多遞迴範圍。 DNS 伺服器遞迴原則可讓您為一組查詢選擇遞迴範圍。 如果 DNS 伺服器不是特定查詢的授權，則 DNS 伺服器遞迴原則可讓您控制如何解決這些查詢。 您可以指定要使用的轉寄站，以及是否要使用遞迴。

- **區域範圍：** DNS 區域可以有多個區域範圍，其中每個區域範圍都包含自己的一組 DNS 記錄。 相同的記錄可以出現在多個具有不同 IP 位址的範圍中。 此外，區域轉移也會在區域範圍層級進行。 這表示主要區域中的區域範圍內的記錄將會傳輸至次要區域中的相同區域範圍。

## <a name="types-of-policy"></a>原則類型

DNS 原則會依層級和類型來劃分。 您可以使用查詢解析原則來定義如何處理查詢，以及定義區域轉送原則，以定義區域轉移的發生方式。 您可以在伺服器層級或區域層級套用每個原則類型。

### <a name="query-resolution-policies"></a>查詢解決原則

您可以使用 DNS 查詢解析原則來指定 DNS 伺服器處理傳入解析查詢的方式。 每個 DNS 查詢解析原則都包含下列元素：  

|欄位|描述|可能值|  
|---------|---------------|-------------------|  
|**名稱**|原則名稱|-最多256個字元<br />-可以包含任何對檔案名有效的字元|  
|**狀態**|原則狀態|-Enable （預設值）<br />-已停用|  
|**二級**|原則層級|-伺服器<br />-區域|  
|**處理順序**|當查詢依層級分類並套用於之後，伺服器就會尋找查詢符合準則的第一個原則，並將其套用至查詢|-數值<br />-每個原則的唯一值，其中包含相同層級並適用于值|  
|**動作**|要由 DNS 伺服器執行的動作|-Allow （區域層級的預設值）<br />-Deny （伺服器層級上的預設值）<br />-忽略|  
|**指標**|原則條件（和/或），以及要套用原則所符合的準則清單|-條件運算子（和/或）<br />-準則清單（請參閱下方的準則表格）|  
|**範圍**|區域範圍的清單和每個領域的加權值。 加權值用於負載平衡散發。 比方說，如果這份清單包含權數為3的 datacenter1，而 datacenter2 的權數為5，則伺服器將會回應來自 datacentre1 的一筆記錄，而不是八個要求中的三倍|-區域範圍清單（依名稱）和加權|  

> [!NOTE]
> 伺服器層級原則只能將值設為 [**拒絕**] 或 [**略**過] 做為動作。

[DNS 原則準則] 欄位是由兩個元素所組成：


|              Name               |                                         描述                                          |                                                                                                                               範例值                                                                                                                               |
|---------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **用戶端子網**        | 預先定義的用戶端子網名稱。 用來驗證傳送查詢的子網。 |                             -   **EQ、西班牙、法國**-如果子網識別為西班牙或法國，則會解析為 true<br />-   **NE、加拿大、墨西哥**-若用戶端子網為加拿大和墨西哥以外的任何子網，則會解析為 true                             |
|     **傳輸通訊協定**      |        查詢中使用的傳輸通訊協定。 可能的專案為**UDP**和**TCP**        |                                                                                                                    -   **EQ、TCP**<br />-   **EQ、UDP**                                                                                                                     |
|      **網際網路通訊協定**      |        查詢中使用的網路通訊協定。 可能的專案為**IPv4**和**IPv6**        |                                                                                                                   -   **EQ、IPv4**<br />-   **EQ、IPv6**                                                                                                                    |
| **伺服器介面 IP 位址** |                   傳入 DNS 伺服器網路介面的 IP 位址                   |                                                                                                              -   **EQ、10.0.0.1**<br />-   **EQ、192.168.1.1**                                                                                                              |
|            **稱**             |            查詢中記錄的 FQDN，以及使用萬用字元的可能性            | -   **EQ，www. contoso .com** -只有在查詢嘗試解析<em>www.contoso.com</em> FQDN 時，才會解析為 true<br />-   **EQ，\*.contoso.com，\*.woodgrove.com** -如果查詢是針對以*contoso.com***或***woodgrove.com*結尾的任何記錄，則會解析為 true |
|         **查詢類型**          |                          查詢的記錄類型（A、SRV、TXT）                          |                                                  -   **EQ、TXT、SRV** -解析為 true （如果查詢要求 TXT**或**srv 記錄）<br />-   **EQ，MX** -如果查詢要求 MX 記錄，則會解析為 true                                                   |
|         **當日時間**         |                              接收查詢的當日時間                               |                                                                    -   **EQ、10： 00-12：00、22： 00-23： 00** -如果在上午10點和中午之間**或**10pm-12am 與晚上11點之間收到查詢，則會解析為 true                                                                    |

使用上表做為起點，下表可用來定義用來比對任何記錄類型之查詢的準則，但在 contoso.com 網域中，來自 10.0.0.0/24 子網中用戶端的 SRV 記錄，是透過 TCP 介於1到下午10點到 i 之間nterface 10.0.0.3：  

|Name|值|  
|--------|---------|  
|用戶端子網|EQ、10.0.0.0/24|  
|傳輸通訊協定|EQ、TCP|  
|伺服器介面 IP 位址|EQ、10.0.0。3|  
|FQDN|EQ、*. contoso .com|  
|查詢類型|NE、SRV|  
|當日時間|EQ、20： 00-22：00|  

您可以建立相同層級的多個查詢解決原則，只要它們的處理順序有不同的值即可。 當有多個原則可供使用時，DNS 伺服器會以下列方式處理傳入的查詢：  

![DNS 原則處理](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  

### <a name="recursion-policies"></a>遞迴原則  
遞迴原則是一**種特殊類型**的伺服器層級原則。 遞迴原則可控制 DNS 伺服器執行查詢遞迴的方式。 只有在查詢處理到達遞迴路徑時，遞迴原則才適用。 針對一組查詢，您可以選擇 [拒絕] 或 [忽略遞迴] 的值。 或者，您也可以為一組查詢選擇一組轉寄站。  

您可以使用遞迴原則來執行分割大腦 DNS 設定。 在此設定中，DNS 伺服器會針對查詢的一組用戶端執行遞迴，而 DNS 伺服器不會針對該查詢的其他用戶端執行遞迴。  

遞迴原則包含一般 DNS 查詢解析原則所包含的相同元素，以及下表中的元素：  

|Name|描述|  
|--------|---------------|  
|**套用於遞迴**|指定此原則應該僅用於遞迴。|  
|**遞迴範圍**|遞迴範圍的名稱。|  

> [!NOTE]  
> 遞迴原則只能在伺服器層級建立。  

### <a name="zone-transfer-policies"></a>區域轉送原則  
區域轉送原則可控制是否允許您的 DNS 伺服器進列區域轉送。 您可以在伺服器層級或區域層級建立區域轉送的原則。 伺服器層級原則適用于在 DNS 伺服器上發生的每個區域轉送查詢。 區域層級原則只適用于 DNS 伺服器上裝載之區域上的查詢。 區域層級原則最常見的用途是執行封鎖或安全清單。  

> [!NOTE]  
> 區域轉送原則只能使用 [拒絕] 或 [略過] 做為動作。  

您可以使用下方的伺服器層級區域轉送原則，拒絕來自指定子網之 contoso.com 網域的區域轉送：  

```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  

您可以建立相同層級的多個區域轉移原則，只要它們的處理順序有不同的值即可。 當有多個原則可供使用時，DNS 伺服器會以下列方式處理傳入的查詢：  

![多個區域轉送原則的 DNS 進程](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  

## <a name="managing-dns-policies"></a>管理 DNS 原則  
您可以使用 PowerShell 來建立和管理 DNS 原則。 下列範例會逐步解說您可以透過 DNS 原則設定的不同範例案例：  

### <a name="traffic-management"></a>流量管理  
您可以根據 DNS 用戶端的位置，將流量導向至不同的伺服器。 下列範例示範如何建立流量管理原則，以將來自特定子網的客戶導向至北美資料中心，並將另一個子網導向至歐洲資料中心。  

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

腳本的前兩行會建立北美洲和歐洲的用戶端子網物件。 之後的兩行會在 contoso.com 網域內建立區域範圍，每個區域各一個。 之後的兩行會在每個區域中建立一筆記錄，將 ww.contoso.com 關聯至不同的 IP 位址，一個用於歐洲，另一個用於北美洲。 最後，腳本的最後幾行會建立兩個 DNS 查詢解析原則，一個套用至北美洲子網，另一個則套用至歐洲子網。  

### <a name="block-queries-for-a-domain"></a>封鎖網域的查詢  
您可以使用 DNS 查詢解析原則來封鎖對網域的查詢。 下列範例會封鎖所有要 treyresearch.net 的查詢：  

```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  

### <a name="block-queries-from-a-subnet"></a>封鎖子網的查詢  
您也可以封鎖來自特定子網的查詢。 下列腳本會建立 172.0.33.0/24 的子網，然後建立一個原則來忽略來自該子網的所有查詢：  

```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  

### <a name="allow-recursion-for-internal-clients"></a>允許內部用戶端的遞迴  
您可以使用 DNS 查詢解析原則來控制遞迴。 下列範例可用來啟用內部用戶端的遞迴，同時在分裂的大腦案例中停用外部用戶端的。  

```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  

腳本中的第一行會變更預設遞迴範圍，簡單地命名為 "."。（點）停用遞迴。 第二行會建立名為*InternalClients*的遞迴範圍，並啟用遞迴。 而第三行會建立一個原則，將新建立的遞迴範圍套用到透過10.0.0.34 為 IP 位址的伺服器介面傳入的任何查詢。  

### <a name="create-a-server-level-zone-transfer-policy"></a>建立伺服器層級區域轉送原則  
您可以使用 DNS 區域轉送原則，以更細微的形式控制區域轉送。 下列範例腳本可用來允許指定子網上任何伺服器的區域轉送：  

```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  

腳本中的第一行會建立名為*AllowedSubnet*的子網物件，其中包含 IP 區塊 172.21.33.0/24。 第二行會建立區域轉送原則，以允許在先前建立的子網上，將區域傳輸到任何 DNS 伺服器。  

### <a name="create-a-zone-level-zone-transfer-policy"></a>建立區域層級區域轉送原則  
您也可以建立區域層級的區域傳輸原則。 下列範例會忽略來自 IP 位址為為10.0.0.33 之伺服器介面的 contoso.com 區域轉送要求：  

```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "eq,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  

## <a name="dns-policy-scenarios"></a>DNS 原則案例

如需有關如何在特定案例中使用 DNS 原則的詳細資訊，請參閱本指南中的下列主題。

- [將 DNS 原則用於以地理位置為基礎的流量管理與主伺服器](primary-geo-location.md)  
- [針對以地理位置為基礎的流量管理，使用主要-次要部署的 DNS 原則](primary-secondary-geo-location.md)  
- [根據當日時間使用 DNS 原則來進行智慧型 DNS 回應](dns-tod-intelligent.md)
- [使用 Azure 雲端應用程式伺服器以一天的時間為基礎的 DNS 回應](dns-tod-azure-cloud-app-server.md)
- [使用 DNS 原則進行分割的 DNS 部署](split-brain-DNS-deployment.md)
- [在 Active Directory 中使用適用于分裂式 DNS 的 DNS 原則](dns-sb-with-ad.md)
- [使用 DNS 原則在 DNS 查詢上套用篩選](apply-filters-on-dns-queries.md)
- [使用 DNS 原則進行應用程式負載平衡](app-lb.md)
- [使用 DNS 原則進行應用程式負載平衡與地理位置感知](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>在唯讀網域控制站上使用 DNS 原則

DNS 原則與唯讀網域控制站相容。 請注意，需要重新開機 DNS 伺服器服務，才能在唯讀網域控制站上載入新的 DNS 原則。 這在可寫入的網域控制站上不是必要的。
