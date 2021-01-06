---
title: DNS 原則概觀
description: 瞭解如何使用 DNS 原則進行以 Geo-Location 為基礎的流量管理、以每天時間為基礎的智慧型 DNS 回應、管理針對分割腦部署設定的單一 DNS 伺服器 \- 、對 DNS 查詢套用篩選等。
manager: brianlic
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d913905fe5b40e4e3e24828a0c6b392c1691043d
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904663"
---
# <a name="dns-policies-overview"></a>DNS 原則概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解 DNS 原則，這是 Windows Server 2016 中的新功能。 您可以使用 DNS 原則進行以 Geo-Location 為基礎的流量管理、以每天時間為基礎的智慧型 DNS 回應、管理針對分割腦部署設定的單一 DNS 伺服器 \- 、對 DNS 查詢套用篩選等。 下列專案提供這些功能的更多詳細資料。

-   **應用程式負載平衡。** 當您在不同位置部署應用程式的多個實例時，您可以使用 DNS 原則來平衡不同應用程式實例之間的流量負載，並動態配置應用程式的流量負載。

-   **以地理 \- 位置為基礎的流量管理。** 您可以使用 DNS 原則，根據用戶端嘗試連線的用戶端和資源的地理位置，讓主要和次要 DNS 伺服器回應 DNS 用戶端查詢，為用戶端提供最接近資源的 IP 位址。

-   **分割大腦 DNS。** 使用分割 \- 大腦 dns 時，dns 記錄會分割成相同 DNS 伺服器上的不同區域範圍，而 dns 用戶端會根據用戶端是內部或外部用戶端來接收回應。 您可以 \- 針對 Active Directory 整合式區域或獨立 DNS 伺服器上的區域，設定分割的大腦 DNS。

-   **濾波。** 您可以設定 DNS 原則，以根據您提供的準則建立查詢篩選準則。 DNS 原則中的查詢篩選器可讓您根據傳送 DNS 查詢的 DNS 查詢和 DNS 用戶端，將 DNS 伺服器設定為以自訂的方式回應。
-   **取證。** 您可以使用 DNS 原則，將惡意的 DNS 用戶端重新導向至不 \- 存在的 IP 位址，而不是將它們導向至它們嘗試連接的電腦。

-   **以天為基礎的重新導向時間。** 您可以使用 DNS 原則，藉由使用以一天時間為基礎的 DNS 原則，將應用程式流量分散到應用程式的不同地理位置分散的實例上。

## <a name="new-concepts"></a>新概念
若要建立原則以支援以上所列的案例，必須能夠識別區域中的記錄群組、網路上的用戶端群組，以及其他元素。 這些元素會以下列新的 DNS 物件表示：

- **用戶端子網：** 用戶端子網物件代表將查詢提交到 DNS 伺服器的 IPv4 或 IPv6 子網。 您可以建立子網，以便稍後根據要求來源的子網來定義要套用的原則。 比方說，在分裂分裂 DNS 案例中，您可以使用內部子網的用戶端 IP 位址，將解析的解析要求 <em>解析為內部</em> 子網的用戶端，並使用不同的 ip 位址來回應外部子網中的用戶端。

- **遞迴範圍：** 遞迴範圍是一組設定的唯一實例，可控制 DNS 伺服器上的遞迴。 遞迴範圍包含轉寄站清單，並指定是否啟用遞迴。 DNS 伺服器可以有許多遞迴範圍。 DNS 伺服器遞迴原則可讓您選擇一組查詢的遞迴範圍。 如果 DNS 伺服器不是特定查詢的授權，DNS 伺服器遞迴原則可讓您控制如何解決這些查詢。 您可以指定要使用的轉寄站，以及是否要使用遞迴。

- **區域範圍：** DNS 區域可以有多個區域範圍，每個區域範圍都包含自己的一組 DNS 記錄。 相同記錄可以出現在多個具有不同 IP 位址的範圍中。 此外，區域傳輸是在區域範圍層級進行。 這表示主要區域中的區域範圍記錄會傳輸到次要區域中的相同區域範圍。

## <a name="types-of-policy"></a>原則類型

DNS 原則會依層級和類型劃分。 您可以使用查詢解析原則來定義查詢的處理方式，以及用來定義區域傳輸發生方式的區域傳輸原則。 您可以在伺服器層級或區域層級套用每個原則類型。

### <a name="query-resolution-policies"></a>查詢解析原則

您可以使用 DNS 查詢解析原則來指定 DNS 伺服器處理傳入解析查詢的方式。 每個 DNS 查詢解析原則都包含下列元素：

|欄位|描述|可能值|
|---------|---------------|-------------------|
|**名稱**|原則名稱|-最多256個字元<br />-可包含任何適用于檔案名的字元|
|**State**|原則狀態|-啟用 (預設) <br />-已停用|
|**Level**|原則層級|-伺服器<br />-區域|
|**處理順序**|當查詢依層級分類並套用於之後，伺服器會尋找查詢符合準則的第一個原則，並將其套用至查詢|-數值<br />-每個原則的唯一值，包含相同層級並套用於值|
|**動作**|DNS 伺服器要執行的動作|-允許區域層級的 (預設值) <br />-拒絕伺服器層級的 (預設值) <br />-忽略|
|**準則**|原則條件 (及/或) ，以及要套用原則的準則清單|-Condition 運算子 (和/或) <br />-準則清單 (請參閱下方的準則表格) |
|**範圍**|每個範圍的區域範圍和加權值的清單。 加權值用於負載平衡散發。 比方說，如果這份清單包含權數為3的 datacenter1，而 datacenter2 的加權為5，則伺服器將會從 datacentre1 三次的記錄回應八個要求中的記錄|-依名稱) 和加權 (的區域範圍清單|

> [!NOTE]
> 伺服器層級原則的值只能是 [ **拒絕** ] 或 [ **略** 過]。

DNS 原則準則欄位由兩個元素組成：


|              名稱               |                                         描述                                          |                                                                                                                               範例值                                                                                                                               |
|---------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **用戶端子網**        | 預先定義之用戶端子網的名稱。 用來驗證傳送查詢的子網。 |                             -   **EQ、西班牙、法國** -如果子網識別為西班牙或法國，則會解析為 true<br />-   **NE、加拿大、墨西哥** -如果用戶端子網是加拿大和墨西哥以外的任何子網，則會解析為 true                             |
|     **傳輸通訊協定**      |        查詢中所使用的傳輸通訊協定。 可能的專案為 **UDP** 和 **TCP**        |                                                                                                                    -   **EQ、TCP**<br />-   **EQ、UDP**                                                                                                                     |
|      **網際網路通訊協定**      |        查詢中使用的網路通訊協定。 可能的專案為 **IPv4** 和 **IPv6**        |                                                                                                                   -   **EQ、IPv4**<br />-   **EQ，IPv6**                                                                                                                    |
| **伺服器介面 IP 位址** |                   傳入 DNS 伺服器網路介面的 IP 位址                   |                                                                                                              -   **EQ、10.0.0。1**<br />-   **EQ、192.168.1。1**                                                                                                              |
|            **FQDN**             |            查詢中記錄的 FQDN，有可能使用萬用字元            | -   **EQ、www .com** -只有在查詢嘗試解析 <em>www.contoso.com</em> FQDN 時，才會解析為 true<br />-   **EQ、 \* . contoso.com、 \* . woodgrove.com** -如果查詢是針對以 * contoso.com ***或**_woodgrove.com_ 結尾的任何記錄，則會解析為 true |
|         **查詢類型**          |                          查詢的記錄類型 (A、SRV、TXT)                           |                                                  -   **EQ、TXT、SRV** -如果查詢要求 TXT **或** SRV 記錄，則會解析為 true<br />-   **EQ，mx** -如果查詢要求 MX 記錄，則會解析為 true                                                   |
|         **當日時間**         |                              收到查詢的當日時間                               |                                                                    -   **EQ、10： 00-12：00、22： 00-23： 00** -如果在上午10點和中午之間， **或** 下午10點和晚上11點之間收到查詢，則解析為 true。                                                                    |

使用上表做為起點，下表可以用來定義用來比對任何記錄類型之查詢的準則，但 contoso.com 網域中的 SRV 記錄是由 10.0.0.0/24 子網中的用戶端透過 TCP 從 10.0.0.0/24 子網中的用戶端，透過介面10.0.0.3：

|名稱|值|
|--------|---------|
|用戶端子網|EQ、10.0.0.0/24|
|傳輸通訊協定|EQ、TCP|
|伺服器介面 IP 位址|EQ、10.0.0。3|
|FQDN|EQ、*. .com|
|查詢類型|NE、SRV|
|一天中的時間|EQ、20： 00-22：00|

您可以建立多個相同層級的查詢解析原則，只要它們具有不同的處理順序值即可。 當有多個可用的原則時，DNS 伺服器會以下列方式處理傳入的查詢：

![DNS 原則處理](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)

### <a name="recursion-policies"></a>遞迴原則
遞迴原則是一 **種特殊類型** 的伺服器層級原則。 遞迴原則可控制 DNS 伺服器對查詢執行遞迴的方式。 只有當查詢處理到達遞迴路徑時，才會套用遞迴原則。 您可以針對一組查詢選擇 [拒絕] 或 [忽略遞迴] 值。 或者，您可以為一組查詢選擇一組轉寄站。

您可以使用遞迴原則來實行分裂式 DNS 設定。 在此設定中，DNS 伺服器會針對一組用戶端執行遞迴，而 DNS 伺服器不會針對該查詢執行其他用戶端的遞迴。

遞迴原則包含一般 DNS 查詢解析原則所包含的相同元素，以及下表中的元素：

|名稱|描述|
|--------|---------------|
|**適用于遞迴**|指定此原則只應用於遞迴。|
|**遞迴範圍**|遞迴範圍的名稱。|

> [!NOTE]
> 您只能在伺服器層級建立遞迴原則。

### <a name="zone-transfer-policies"></a>區域轉送原則
區域轉送原則可控制您的 DNS 伺服器是否允許或不允許進列區域轉送。 您可以在伺服器層級或區域層級建立區域轉送的原則。 伺服器層級原則適用于 DNS 伺服器上發生的每個區域傳輸查詢。 區域層級原則只適用于 DNS 伺服器上裝載之區域的查詢。 區域層級原則最常見的用途是執行封鎖或安全的清單。

> [!NOTE]
> 區域轉移原則只能使用 [拒絕] 或 [略過] 動作。

您可以使用下方的伺服器層級區域轉移原則，拒絕指定子網中 contoso.com 網域的區域轉送：

```
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"
```

您可以建立相同層級的多個區域轉移原則，只要它們具有不同的處理順序值即可。 當有多個可用的原則時，DNS 伺服器會以下列方式處理傳入的查詢：

![多個區域轉移原則的 DNS 進程](../../media/DNS-Policies-Overview/DNSPolicyZone.png)

## <a name="managing-dns-policies"></a>管理 DNS 原則
您可以使用 PowerShell 來建立和管理 DNS 原則。 下列範例會逐步解說可透過 DNS 原則設定的不同範例案例：

### <a name="traffic-management"></a>流量管理
您可以根據 DNS 用戶端的位置，將根據 FQDN 的流量導向至不同的伺服器。 下列範例顯示如何建立流量管理原則，以將來自特定子網的客戶導向至北美洲資料中心，並將另一個子網導向至歐洲資料中心。

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

腳本的前兩行會建立北美洲和歐洲的用戶端子網物件。 在 contoso.com 網域內建立區域範圍的兩行之後，每個區域會有一個。 這兩行之後，會在每個區域中建立一筆記錄，使 ww.contoso.com 與不同的 IP 位址產生關聯，一個用於歐洲，另一個用於北美洲。 最後，腳本的最後一行會建立兩個 DNS 查詢解析原則，一個是要套用到北美洲子網，另一個套用至歐洲子網。

### <a name="block-queries-for-a-domain"></a>封鎖定義域的查詢
您可以使用 DNS 查詢解析原則來封鎖對網域的查詢。 下列範例會封鎖所有對 treyresearch.net 的查詢：

```
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"
```

### <a name="block-queries-from-a-subnet"></a>封鎖子網的查詢
您也可以封鎖來自特定子網的查詢。 下列腳本會建立 172.0.33.0/24 的子網，然後建立原則來忽略來自該子網的所有查詢：

```
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"
```

### <a name="allow-recursion-for-internal-clients"></a>允許內部用戶端的遞迴
您可以使用 DNS 查詢解析原則來控制遞迴。 下列範例可用來啟用內部用戶端的遞迴，並在分裂腦案例中針對外部用戶端停用它。

```
Set-DnsServerRecursionScope -Name . -EnableRecursion $False
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"
```

腳本中的第一行會變更預設的遞迴範圍，簡單的命名為 "." (點) 停用遞迴。 第二行會建立名為 *InternalClients* 的遞迴範圍，並啟用遞迴。 第三行建立一個原則，將新建立的遞迴範圍套用至透過以 IP 位址10.0.0.34 的伺服器介面進行的任何查詢。

### <a name="create-a-server-level-zone-transfer-policy"></a>建立伺服器層級區域傳輸原則
您可以使用 DNS 區域轉移原則，以更細微的形式控制區域轉送。 下列範例腳本可用來允許指定子網上的任何伺服器進列區域轉送：

```
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"
```

腳本中的第一行會建立名為 *AllowedSubnet* 的子網物件，其 IP 區塊為 172.21.33.0/24。 第二行建立區域轉移原則，以允許在先前建立的子網上的任何 DNS 伺服器上進列區域轉送。

### <a name="create-a-zone-level-zone-transfer-policy"></a>建立區域層級區域傳輸原則
您也可以建立區域層級區域傳輸原則。 下列範例會針對來自 IP 位址為 >10.0.0.33 之伺服器介面的 contoso.com，略過任何區域轉送的要求：

```
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "eq,10.0.0.33" -PassThru -ZoneName "contoso.com"
```

## <a name="dns-policy-scenarios"></a>DNS 原則案例

如需有關如何在特定案例中使用 DNS 原則的詳細資訊，請參閱本指南中的下列主題。

- [透過主要伺服器使用地理位置流量管理的 DNS 原則](primary-geo-location.md)
- [透過主要-次要部署使用地理位置流量管理的 DNS 原則](primary-secondary-geo-location.md)
- [使用 DNS 原則以時間為基礎進行智慧型 DNS 回應](dns-tod-intelligent.md)
- [以 Azure Cloud App Server 的當日時間為基礎的 DNS 回應](dns-tod-azure-cloud-app-server.md)
- [使用 DNS 原則進行 Split-Brain DNS 部署](split-brain-DNS-deployment.md)
- [在 Active Directory 中使用 Split-Brain DNS 的 DNS 原則](dns-sb-with-ad.md)
- [使用 DNS 原則在 DNS 查詢上套用篩選](apply-filters-on-dns-queries.md)
- [使用 DNS 原則進行應用程式負載平衡](app-lb.md)
- [使用 DNS 原則進行具有地理位置感知的應用程式負載平衡](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>使用 Read-Only 網域控制站上的 DNS 原則

DNS 原則與 Read-Only 網域控制站相容。 請注意，需要重新開機 DNS 伺服器服務，才能將新的 DNS 原則載入 Read-Only 網域控制站。 這在可寫入的網域控制站上並不是必要的。
