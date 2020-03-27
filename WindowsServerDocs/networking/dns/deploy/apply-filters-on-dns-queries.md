---
title: 使用 DNS 原則進行 DNS 查詢上的篩選套用
description: 本主題是 Windows Server 2016 DNS 原則案例指南的一部分
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 45dad1eb40caba7ac304fc640e3d56044254f08c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317836"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>使用 DNS 原則進行 DNS 查詢上的篩選套用

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解如何在 Windows Server&reg; 2016 中設定 DNS 原則，以根據您所提供的準則建立查詢篩選器。 

DNS 原則中的查詢篩選器可讓您設定 DNS 伺服器，以自訂的方式，根據傳送 DNS 查詢的 DNS 查詢和 DNS 用戶端進行回應。

例如，您可以使用查詢篩選封鎖清單來設定 DNS 原則，以封鎖來自已知惡意網域的 DNS 查詢，這可防止 DNS 回應這些網域中的查詢。 因為不會從 DNS 伺服器傳送回應，所以惡意網域成員的 DNS 查詢會超時。

另一個範例是建立查詢篩選允許清單，只允許一組特定的用戶端解析特定的名稱。

## <a name="query-filter-criteria"></a><a name="bkmk_criteria"></a>查詢篩選準則
您可以使用下列準則的任何邏輯組合（和/或）來建立查詢篩選。

|名稱|描述|
|-----------------|---------------------|
|用戶端子網|預先定義的用戶端子網名稱。 用來驗證傳送查詢的子網。|
|傳輸通訊協定|查詢中使用的傳輸通訊協定。 可能的值為 UDP 和 TCP。|
|網際網路通訊協定|查詢中使用的網路通訊協定。 可能的值為 IPv4 和 IPv6。|
|伺服器介面 IP 位址|接收 DNS 要求的 DNS 伺服器網路介面的 IP 位址。|
|FQDN|查詢中記錄的完整功能變數名稱，有可能會使用萬用字元。|
|查詢類型|所查詢的記錄類型 \(A、SRV、TXT 等等\)。|
|一天中的時間|接收查詢的當日時間。|

下列範例示範如何針對封鎖或允許 DNS 名稱解析查詢的 DNS 原則建立篩選器。

>[!NOTE]
>本主題中的範例命令會使用 Windows PowerShell 命令**DnsServerQueryResolutionPolicy**。 如需詳細資訊，請參閱[DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。 

## <a name="block-queries-from-a-domain"></a><a name="bkmk_block1"></a>封鎖來自網域的查詢

在某些情況下，您可能會想要針對已識別為惡意的網域，或不符合貴組織使用指導方針的網域封鎖 DNS 名稱解析。 您可以使用 DNS 原則來完成網域的封鎖查詢。

您在此範例中設定的原則不會在任何特定區域上建立，而是會建立套用到 DNS 伺服器上設定之所有區域的伺服器層級原則。 伺服器層級原則是第一個要評估的，因此會在 DNS 伺服器收到查詢時先進行比對。

下列範例命令會設定伺服器層級原則，以封鎖具有網域**尾碼 contosomalicious.com**的任何查詢。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>當您使用 [**略**過] 值設定**動作**參數時，會將 DNS 伺服器設定為卸載沒有回應的查詢。 這會導致惡意網域中的 DNS 用戶端超時。

## <a name="block-queries-from-a-subnet"></a><a name="bkmk_block2"></a>封鎖子網的查詢
在此範例中，您可以封鎖子網的查詢（如果它被發現受到部分惡意程式碼感染，並嘗試使用您的 DNS 伺服器來連線到惡意網站）。 

' DnsServerClientSubnet-Name "MaliciousSubnet06"-IPv4Subnet 172.0.33.0/24-PassThru

DnsServerQueryResolutionPolicy-Name "BlockListPolicyMalicious06"-Action IGNORE-ClientSubnet "EQ，MaliciousSubnet06"-PassThru '

下列範例示範如何搭配 FQDN 準則使用子網準則，以封鎖來自受感染子網的特定惡意網域查詢。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

## <a name="block-a-type-of-query"></a><a name="bkmk_block3"></a>封鎖查詢類型
您可能需要在伺服器上封鎖特定類型查詢的名稱解析。 例如，您可以封鎖「任何」查詢，這可以用來惡意建立放大攻擊。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

## <a name="allow-queries-only-from-a-domain"></a><a name="bkmk_allow1"></a>只允許來自網域的查詢
您不能只使用 DNS 原則來封鎖查詢，您可以使用它們來自動核准來自特定網域或子網的查詢。 當您設定允許清單時，DNS 伺服器只會處理來自允許網域的查詢，同時封鎖其他網域的所有其他查詢。

下列範例命令只允許 contoso.com 和子域中的電腦和裝置查詢 DNS 伺服器。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

## <a name="allow-queries-only-from-a-subnet"></a><a name="bkmk_allow2"></a>只允許來自子網的查詢
您也可以建立 IP 子網的允許清單，以忽略所有不是源自這些子網的查詢。

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

## <a name="allow-only-certain-qtypes"></a><a name="bkmk_allow3"></a>僅允許特定 QTypes
您可以將允許清單套用至 QTYPEs。 

例如，如果您有外部客戶查詢 DNS 伺服器介面164.8.1.1，則只允許查詢特定 QTYPEs，而有其他 QTYPEs （例如 SRV 或 TXT 記錄）供內部伺服器用來進行名稱解析或用於監視用途。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

您可以根據您的流量管理需求來建立數以千計的 DNS 原則，而所有的新原則都會以動態方式套用，而不需要重新開機 DNS 伺服器上的傳入查詢。 
