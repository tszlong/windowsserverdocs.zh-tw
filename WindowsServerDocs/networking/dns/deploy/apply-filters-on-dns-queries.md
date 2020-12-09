---
title: 使用 DNS 原則進行 DNS 查詢上的篩選套用
description: 本主題是 Windows Server 2016 的 DNS 原則案例指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4f309a304e4457b27eec0ae41d581c5a7bf9bd50
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865337"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>使用 DNS 原則進行 DNS 查詢上的篩選套用

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何設定 Windows Server 2016 中的 DNS 原則， &reg; 以根據您提供的準則建立查詢篩選準則。

DNS 原則中的查詢篩選器可讓您根據傳送 DNS 查詢的 DNS 查詢和 DNS 用戶端，將 DNS 伺服器設定為以自訂的方式回應。

例如，您可以使用查詢篩選封鎖清單來設定 DNS 原則，以封鎖來自已知惡意網域的 DNS 查詢，以防止 DNS 回應這些網域的查詢。 因為不會從 DNS 伺服器傳送回應，所以惡意網域成員的 DNS 查詢會超時。

另一個範例是建立查詢篩選器允許清單，只允許一組特定的用戶端解析特定的名稱。

## <a name="query-filter-criteria"></a><a name="bkmk_criteria"></a> 查詢篩選準則
您可以使用任何邏輯組合來建立查詢篩選， (及/或/不) 下列準則。

|名稱|描述|
|-----------------|---------------------|
|用戶端子網|預先定義之用戶端子網的名稱。 用來驗證傳送查詢的子網。|
|傳輸通訊協定|查詢中所使用的傳輸通訊協定。 可能的值為 UDP 和 TCP。|
|網際網路通訊協定|查詢中使用的網路通訊協定。 可能的值為 IPv4 和 IPv6。|
|伺服器介面 IP 位址|接收 DNS 要求的 DNS 伺服器之網路介面的 IP 位址。|
|FQDN|查詢中記錄的完整功能變數名稱，而且可能會使用萬用字元。|
|查詢類型|查詢 \( A、SRV、TXT 等的記錄類型 \) 。|
|一天中的時間|收到查詢的當日時間。|

下列範例示範如何建立 DNS 原則的篩選準則，以封鎖或允許 DNS 名稱解析查詢。

>[!NOTE]
>本主題中的範例命令使用 Windows PowerShell 命令 **DnsServerQueryResolutionPolicy**。 如需詳細資訊，請參閱 [DnsServerQueryResolutionPolicy](/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy)。

## <a name="block-queries-from-a-domain"></a><a name="bkmk_block1"></a>封鎖來自網域的查詢

在某些情況下，您可能會想要封鎖識別為惡意之網域的 DNS 名稱解析，或封鎖不符合組織使用指導方針的網域的 DNS 名稱解析。 您可以使用 DNS 原則來完成網域的封鎖查詢。

您在此範例中設定的原則不會在任何特定區域中建立，而是建立套用至 DNS 伺服器上所設定之所有區域的伺服器層級原則。 首先要評估伺服器層級原則，然後在 DNS 伺服器收到查詢時先進行比對。

下列範例命令會設定伺服器層級原則，以封鎖任何具有網域 **尾碼 contosomalicious.com** 的查詢。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru
`

>[!NOTE]
>當您使用 [**略** 過] 值來設定 **Action** 參數時，DNS 伺服器會設定為捨棄沒有回應的查詢。 這會導致惡意網域中的 DNS 用戶端超時。

## <a name="block-queries-from-a-subnet"></a><a name="bkmk_block2"></a>封鎖子網的查詢
在此範例中，您可以封鎖子網的查詢（如果發現它受到某些惡意程式碼的感染），並嘗試使用您的 DNS 伺服器來與惡意網站聯繫。

' Add-DnsServerClientSubnet-Name "MaliciousSubnet06"-IPv4Subnet 172.0.33.0/24-PassThru

Add-DnsServerQueryResolutionPolicy-Name "BlockListPolicyMalicious06"-Action IGNORE-ClientSubnet "EQ，MaliciousSubnet06"-PassThru '

下列範例示範如何搭配 FQDN 準則使用子網準則，以封鎖來自受感染子網的特定惡意網域的查詢。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

## <a name="block-a-type-of-query"></a><a name="bkmk_block3"></a>封鎖查詢類型
您可能需要在伺服器上封鎖特定查詢類型的名稱解析。 例如，您可以封鎖「任何」查詢，這可能會惡意地用來建立放大攻擊。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

## <a name="allow-queries-only-from-a-domain"></a><a name="bkmk_allow1"></a>只允許來自網域的查詢
您不僅可以使用 DNS 原則來封鎖查詢，也可以使用它們來自動核准來自特定網域或子網的查詢。 當您設定允許清單時，DNS 伺服器只會處理來自允許網域的查詢，同時封鎖其他網域的所有其他查詢。

下列範例命令只允許 contoso.com 和子域中的電腦和裝置查詢 DNS 伺服器。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru
`

## <a name="allow-queries-only-from-a-subnet"></a><a name="bkmk_allow2"></a>只允許來自子網的查詢
您也可以建立 IP 子網的允許清單，如此就不會忽略源自這些子網的所有查詢。

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

## <a name="allow-only-certain-qtypes"></a><a name="bkmk_allow3"></a>僅允許特定 QTypes
您可以將允許清單套用至 QTYPEs。

例如，如果您有外部客戶查詢 DNS 伺服器介面164.8.1.1，則只允許查詢特定的 QTYPEs，而其他 QTYPEs （例如 SRV 或 TXT 記錄）可供內部伺服器用來進行名稱解析或用於監視用途。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

您可以根據您的流量管理需求建立數千個 DNS 原則，而且會動態套用所有新原則，而不需要重新開機 DNS 伺服器上的傳入查詢。
