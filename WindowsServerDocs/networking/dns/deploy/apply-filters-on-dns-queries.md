---
title: 使用 DNS 原則 DNS 查詢上套用篩選
description: 本主題是 DNS 原則案例指南適用於 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ff10af5f88a03f806a5e5b5fa698c7c3637816bd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>使用 DNS 原則 DNS 查詢上套用篩選

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何在 Windows Server 設定 DNS 原則&reg;2016 建立查詢篩選器根據您所提供的條件。 

查詢篩選 DNS 原則中的，讓您設定根據 DNS 查詢和傳送 DNS 查詢 DNS client 的方式自訂回應 DNS 伺服器。

例如，您可以設定 DNS 原則的查詢篩選封鎖清單封鎖來自已知的惡意網域，DNS 查詢防止 DNS 網域這些回應查詢。 從 DNS 伺服器傳送無回應，因為惡意網域成員的 DNS 查詢逾時。

另一個範例是建立查詢篩選允許的特定設定的將特定的名稱解析用的允許清單。

## <a name="bkmk_criteria"></a>查詢準則
您可以下列條件建立任何邏輯組合查詢篩選器 （和/或日不）。

|名稱|描述|
|-----------------|---------------------|
|Client 子網路|預先定義的 client 子網路的名稱。 用來確認寄查詢子網路。|
|傳輸通訊協定|傳輸通訊協定查詢中使用。 可能的值為 UDP 與 TCP。|
|網際網路通訊協定|用於查詢網路通訊協定。 可能的值為 IPv4 和 IPv6。|
|伺服器介面 IP 位址|網路介面收到 DNS 要求的 DNS 伺服器的 IP 位址|
|FQDN|完整網域名稱中查詢記錄使用萬用字元可使用。|
|查詢類型|查詢記錄類型 \ (A、 SRV、 TXT 等。 \)|
|一天的時間|查詢收到的時間。|

下列範例顯示如何建立篩選 DNS 原則的任一封鎖或允許 DNS 名稱解析查詢。

>[!NOTE]
>此主題中的範例命令使用 Windows PowerShell 命令**新增-DnsServerQueryResolutionPolicy**。 如需詳細資訊，請查看[新增-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx)。 

##<a name="bkmk_block1"></a>封鎖查詢的網域

有時您可能要封鎖 DNS 名稱解析為惡意的您找出的網域或不符合您的組織使用方針的網域。 您可以使用 DNS 原則完成網域封鎖查詢。

您在此範例中設定原則不會建立任何特定區域 – 改為您建立伺服器層級原則套用到所有 DNS 伺服器上設定的區域。 伺服器層級原則是第一個評估，因此第一次在查詢時符合接收 DNS 伺服器。

下列範例命令設定伺服器層級原則封鎖任何查詢網域中的**尾碼 contosomalicious.com**。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>當您設定**動作**參數值**略過**、 DNS 伺服器設定為完全拖放以無回應查詢。 如此 DNS client 逾惡意網域中。

##<a name="bkmk_block2"></a>子網路從封鎖查詢
此範例中，您可以封鎖查詢子網路中的找到一些惡意程式碼感染病毒並嘗試連絡惡意網站使用您的 DNS 伺服器。 

[新增 DnsServerClientSubnet-命名為 「 MaliciousSubnet06 「-IPv4Subnet 172.0.33.0/24-過渡

新增 DnsServerQueryResolutionPolicy-命名為 「 BlockListPolicyMalicious06 「-控制項略過 ClientSubnet 」 EQ MaliciousSubnet06 「-過渡 '

下例示範如何使用子網路準則 FQDN 條件搭配封鎖子網路受感染的特定惡意網域查詢。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

##<a name="bkmk_block3"></a>封鎖一種查詢
您可能需要封鎖特定類型的查詢伺服器上的名稱解析。 例如，您可以封鎖 '任何' 查詢，用來建立放大攻擊惡意。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

##<a name="bkmk_allow1"></a>允許查詢，僅的網域
您可以只使用 DNS 原則封鎖查詢，您可以使用它們來自動核准查詢特定網域或子網路。 當您設定允許清單中時，DNS 伺服器僅封鎖來自其他網域所有其他查詢時處理查詢允許網域中，從。

下列範例命令可讓僅電腦和裝置 contoso.com 和子女網域中查詢 DNS 伺服器。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

##<a name="bkmk_allow2"></a>允許查詢，僅限從子網路
您也可以建立允許列出的 IP 子網路，使會忽略所有查詢不來自這些子網路。

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

##<a name="bkmk_allow3"></a>[允許只有特定 QTypes
您可以到 QTYPEs 套用允許清單。 

例如，如果您有查詢 DNS 伺服器介面 164.8.1.1 外部針對，只有特定 QTYPEs 允許查詢，如 SRV 或 TXT 記錄內部伺服器的名稱解析或監控所使用的其他 QTYPEs 時。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

您可以建立數千 DNS 原則根據您的資料傳輸管理的需求，且所有的新原則已經套用動態-不需要重新 DNS 伺服器-連入查詢。 
