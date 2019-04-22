---
title: DNS 原則概觀
description: 本主題是 DNS 原則案例指南的 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ad8fa904f43bb51871e5063eaddedd0762d54d95
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814299"
---
# <a name="dns-policies-overview"></a>DNS 原則概觀

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解 DNS 原則，這是 Windows Server 2016 中新功能。 對於地理位置為基礎的流量管理，根據的時間，以管理單一的 DNS 伺服器設定為分割的智慧型 DNS 回應，您可以使用 DNS 原則\-大腦部署，DNS 查詢和多個套用的篩選。 下列項目會提供有關這些功能的更多詳細資料。

-   **應用程式負載平衡。** 當您部署的不同位置的應用程式的多個執行個體時，您可以使用 DNS 原則來平衡不同的應用程式執行個體，以動態方式配置應用程式的流量負載之間的流量負載。

-   **異地\-依據流量管理的位置。** 您可以使用 DNS 原則以允許主要和次要 DNS 伺服器回應 DNS 用戶端查詢，根據地理位置的用戶端和資源的用戶端嘗試連線，用戶端提供的最接近的 IP 位址資源。 

-   **分割 DNS 的大腦。** 分割\-大腦 DNS，DNS 記錄會被分割成不同的區域範圍，在相同的 DNS 伺服器和 DNS 用戶端收到回應，根據用戶端是否為內部或外部的用戶端。 您可以設定分割\-大腦 DNS 的 Active Directory 整合區域，或在獨立 DNS 伺服器上的區域。

-   **篩選。** 您可以設定 DNS 原則來建立查詢篩選器會根據您提供的準則。 DNS 原則中的查詢篩選器可讓您設定 DNS 伺服器回應 DNS 查詢和傳送 DNS 查詢的 DNS 用戶端為基礎的方式自訂。 
-   **鑑識調查。** 您可以使用 DNS 原則以惡意的 DNS 用戶端重新導向到非\-現存的 IP 位址，而不是將他們引導至他們嘗試連線到電腦。

-   **一天的時間基礎重新導向。** 您可以使用 DNS 原則以分散不同地理位置分散的應用程式執行個體中的應用程式流量，藉由使用 DNS 原則為基礎的當日時間。

## <a name="new-concepts"></a>新的概念  
若要建立原則，以支援以上所列的案例，就必須能夠識別群組的區域，而其他項目之間的網路上的用戶端群組中的記錄。 這些項目是由下列的新 DNS 物件表示：  

- **用戶端的子網路：** 用戶端的子網路物件所表示的查詢會提交到 DNS 伺服器的 IPv4 或 IPv6 子網路。 您可以建立子網路，以稍後定義會根據所要求所來自的子網路套用的原則。 例如，在核心分裂 DNS 情況，例如名稱解析的要求*www.microsoft.com*可以回答內部 IP 位址給用戶端從內部的子網路，與不同的 IP 位址，在外部的用戶端子網路。

- **遞迴範圍：** 遞迴領域都是唯一的執行個體的一組控制遞迴 DNS 伺服器上的設定。 遞迴範圍包含的轉寄站清單，並指定是否會啟用遞迴功能。 DNS 伺服器可以有許多的遞迴範圍。 DNS 伺服器遞迴原則可讓您選擇一組查詢的遞迴範圍。 如果 DNS 伺服器不是授權特定查詢，DNS 伺服器遞迴原則可讓您控制如何解決這些查詢。 您可以指定使用，以及是否使用遞迴的轉寄站。

- **區域範圍：** DNS 區域可以有多個區域範圍，與包含自己的 DNS 記錄集的每個區域範圍。 同一筆記錄中可以存在多個領域，具有不同 IP 位址。 此外，在區域範圍層級完成區域轉送。 這表示，從主要區域中的區域範圍的記錄會轉移到次要區域中相同的區域範圍。

## <a name="types-of-policy"></a>類型的原則

DNS 原則會區分依層級和類型。 您可以使用查詢解析原則，以定義如何處理查詢，以及區域傳輸原則，以定義如何發生區域轉送。 您可以套用在伺服器層級或區域層級的每種原則類型。
  
### <a name="query-resolution-policies"></a>查詢解析原則

您可以使用 DNS 查詢解析原則來指定如何傳入查詢由 DNS 伺服器所處理的解決方法。 每個 DNS 查詢解析原則包含下列元素：  
  
|欄位|描述|可能值|  
|---------|---------------|-------------------|  
|**名稱**|原則名稱|-最多 256 個字元<br />-可以包含有效檔案名稱的任何字元|  
|**狀態**|原則狀態|-啟用 （預設值）<br />-已停用|  
|**Level**|原則層級|-   Server<br />區域|  
|**處理順序**|一旦查詢根據層級來分類，然後套用，伺服器就會發現，查詢符合準則且它套用至查詢的第一個原則|-數字值<br />每個原則包含相同層級，並套用值的唯一值|  
|**動作**|DNS 伺服器所執行的動作|-允許 （預設值區域層級）<br />拒絕 （伺服器層級上的預設值）<br />-   Ignore|  
|**準則**|原則條件 （和/或） 和原則套用至符合準則的清單|條件運算子 （/或）<br />-（請參閱下面的準則表格） 的準則的清單|  
|**範圍**|區域範圍的每個範圍的加權的值的清單。 負載平衡散發，會使用加權的值。 比方說，如果這份清單包括 datacenter1 權數為 3 和 5 的權數 datacenter2 伺服器會回應一筆資料錄從 datacentre1 三次超出八個要求|-（依名稱） 的區域範圍的加權清單|  

> [!NOTE]
> 伺服器層級原則只能有值**Deny**或是**忽略**做為動作。

DNS 原則準則欄位是由兩個項目所組成：

|名稱|描述|範例值|
|--------|---------------|-----------------|
|**用戶端子網路**|預先定義的用戶端的子網路名稱。 用來驗證傳送查詢的子網路。|-   **EQ、 西班牙，法國**-解析為 true，如果子網路識別為西班牙或法國<br />-   **NE、 加拿大、 墨西哥**-解析為 true，如果用戶端的子網路是加拿大和墨西哥以外的任何子網路|  
|**傳輸通訊協定**|傳輸通訊協定查詢中使用。 可能的項目都**UDP**和**TCP**|-   **EQ,TCP**<br />-   **EQ,UDP**|  
|**網際網路通訊協定**|在查詢中使用的網路通訊協定。 可能的項目都**IPv4**和**IPv6**|-   **EQ,IPv4**<br />-   **EQ,IPv6**|  
|**伺服器介面的 IP 位址**|內送的 DNS 伺服器網路介面的 IP 位址|-   **EQ,10.0.0.1**<br />-   **EQ,192.168.1.1**|  
|**FQDN**|可能會使用萬用字元的 FQDN，在查詢中，記錄|-   **EQ、 www.contoso.com** -解析加到 rue 只当查詢正在嘗試解決*www.contoso.com* FQDN<br />-   **EQ、\*。 contoso.com\*。 woodgrove.com** -解析為 true，如果查詢是在結束任何記錄*contoso.com***或***woodgrove.com*|  
|**查詢類型**|（A、 SVR、 TXT） 進行查詢的記錄類型|-   **EQ、 TXT、 SRV** -會解析為 true，如果查詢要求 TXT**或**SRV 記錄<br />-   **EQ、 MX** -解析為 true，如果查詢要求 MX 記錄|  
|**當日時間**|在收到查詢的日期時間|-   **22:00-23:00、 10:00-12:00，EQ** -會解析為 true，如果查詢收到之間上午 10 點及中午**或**下午 10 點與下午 11 點|  
  
使用上的表做為起點下, 表可用來定義用來比對任何類型的記錄，但來自 10.0.0.0/24 子網路中的用戶端透過 TCP 8 與透過下午 10 點之間的 contoso.com 網域中的 SRV 記錄的查詢準則我介面 10.0.0.3:  
  
|名稱|值|  
|--------|---------|  
|用戶端子網路|EQ,10.0.0.0/24|  
|傳輸通訊協定|EQ,TCP|  
|伺服器介面的 IP 位址|EQ,10.0.0.3|  
|FQDN|EQ,*.contoso.com|  
|查詢類型|NE,SRV|  
|當日時間|EQ,20:00-22:00|  
  
只要他們擁有的處理順序的不同值您可以建立指定多個查詢的相同層級，解決原則。 當多個原則可供使用時，DNS 伺服器會以下列方式處理傳入的查詢：  
  
![DNS 原則處理](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  
  
### <a name="recursion-policies"></a>遞迴原則  
遞迴原則是一種特殊**型別**伺服器層級原則。 遞迴原則來控制 DNS 伺服器執行遞迴查詢的方式。 遞迴原則適用於僅當查詢處理到達遞迴路徑。 您可以選擇一組查詢的遞迴的 DENY 或略過的值。 或者，您可以選擇一組的一組查詢的轉寄站。  
  
您可以使用的遞迴原則實作 Split-brain DNS 組態。 在此組態中，DNS 伺服器執行遞迴查詢，用戶端的一組當 DNS 伺服器不會執行該查詢的其他用戶端的遞迴。  
  
遞迴原則包含相同的項目包含一般的 DNS 查詢解析原則，以及下表中的項目：  
  
|名稱|描述|  
|--------|---------------|  
|**套用於遞迴**|指定此原則應該只使用遞迴。|  
|**遞迴範圍**|遞迴領域的名稱。|  
  
> [!NOTE]  
> 只能在伺服器層級建立遞迴原則。  
  
### <a name="zone-transfer-policies"></a>區域傳輸原則  
區域傳輸原則會控制是否允許區域轉送，或不是由您的 DNS 伺服器。 您可以在伺服器層級或區域層級建立原則的區域轉送。 伺服器層級原則會套用在 DNS 伺服器發生的每個區域轉送查詢。 區域層級原則僅套用於 DNS 伺服器上裝載的區域上的查詢。 區域層級原則的最常見用途是實作已封鎖 」 或 「 安全 」 清單。  
  
> [!NOTE]  
> 區域傳輸原則只能使用拒絕或忽略做為動作。  
  
您可以使用伺服器層級的區域傳輸原則下，拒絕指定之子網路為 contoso.com 網域的區域轉送：  
  
```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  
  
只要他們擁有的處理順序的不同值您可以建立指定多個區域轉送的相同層級的原則。 當多個原則可供使用時，DNS 伺服器會以下列方式處理傳入的查詢：  
  
![DNS 區域傳輸的多個原則的程序](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  
  
## <a name="managing-dns-policies"></a>DNS 的管理原則  
您可以建立和使用 PowerShell 管理 DNS 原則。 下列範例會瀏覽不同的範例案例，您可以透過 DNS 原則設定：  
  
### <a name="traffic-management"></a>流量管理  
您可以將 FQDN，以根據 DNS 用戶端的位置不同的伺服器為基礎的流量。 下列範例示範如何建立管理原則來將客戶引導至北美的資料中心的特定子網路從和到歐洲資料中心的另一個子網路的流量。  
  
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
  
前兩行指令碼會建立用戶端北美洲與歐洲的子網路物件。 在這之後的兩行建立 contoso.com 網域，其中每個區域內的區域範圍。 這兩行之後，將不同的 IP 位址、 一個用於歐洲、 北美洲的另一個 ww.contoso.com 每個區域中建立記錄。 最後，指令碼的最後一行會建立兩個 DNS 查詢解析原則，一個套用至北美地區的子網路，另一個則是歐洲子網路。  
  
### <a name="block-queries-for-a-domain"></a>區塊查詢的網域  
您可以使用封鎖查詢的 DNS 查詢解析原則至網域。 下列範例會封鎖所有 treyresearch.net 的查詢：  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  
  
### <a name="block-queries-from-a-subnet"></a>從子網路的區塊查詢  
您也可以封鎖來自特定子網路的查詢。 下列指令碼會建立 172.0.33.0/24 的子網路，並接著會建立原則，以忽略來自子網路的所有查詢：  
  
```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  
  
### <a name="allow-recursion-for-internal-clients"></a>允許遞迴內部的用戶端  
您可以使用 DNS 查詢解析原則來控制遞迴。 下列範例可用來針對內部的用戶端，啟用遞迴功能，同時停用核心分裂情況中的外部用戶端。  
  
```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  
  
指令碼中的第一行變更預設遞迴範圍，只是命名為"。"（點） 停用遞迴功能。 第二行則會建立名為的遞迴領域*InternalClients*使用遞迴已啟用。 第三行會建立要套用的原則和新建立的遞迴透過 server 介面具有 10.0.0.34 為 IP 位址傳入任何查詢的範圍。  
  
### <a name="create-a-server-level-zone-transfer-policy"></a>建立伺服器層級的區域傳輸原則  
您可以使用 DNS 區域傳輸原則來控制更細微的表單中的區域轉送。 下列範例指令碼可用來指定子網路上的任何伺服器允許區域轉送：  
  
```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  
  
指令碼中的第一行會建立名為的子網路物件*AllowedSubnet* ip 封鎖 172.21.33.0/24。 第二行建立區域傳輸原則以允許對任何 DNS 伺服器上先前建立的子網路的區域轉送。  
  
### <a name="create-a-zone-level-zone-transfer-policy"></a>建立區域層級的區域傳輸原則  
您也可以建立區域層級的區域傳輸原則。 下列範例會略過任何 contoso.com 來自 10.0.0.33 IP 位址的伺服器介面進行區域轉送要求：  
  
```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  
  
## <a name="dns-policy-scenarios"></a>DNS 原則案例

如需如何使用 DNS 原則進行特定案例的資訊，請參閱本指南中的下列主題。

- [使用地理位置的 DNS 原則以透過主要伺服器的流量管理](primary-geo-location.md)  
- [對於地理位置為基礎的流量管理與主要-次要部署，使用 DNS 原則](primary-secondary-geo-location.md)  
- [使用 DNS 原則進行智慧型 DNS 回應的時間為基礎](dns-tod-intelligent.md)
- [DNS 回應為基礎的當日時間和 Azure 雲端應用程式伺服器](dns-tod-azure-cloud-app-server.md)
- [使用 DNS 原則進行拆分式 DNS 部署](split-brain-DNS-deployment.md)
- [使用 DNS 原則進行 Active Directory 中的拆分式 DNS](dns-sb-with-ad.md)
- [使用 DNS 原則進行 DNS 查詢上套用篩選器](apply-filters-on-dns-queries.md)
- [使用 DNS 原則進行應用程式負載平衡](app-lb.md)
- [使用 DNS 原則進行應用程式負載平衡與地理位置感知](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>唯讀網域控制站上使用 DNS 原則

DNS 原則適用於唯讀網域控制站。 請注意，重新啟動 DNS 伺服器服務做是為了要在唯讀網域控制站上載入新的 DNS 原則。 這並不需要可寫入網域控制站上。
