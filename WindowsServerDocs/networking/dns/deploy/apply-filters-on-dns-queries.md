---
title: 使用 DNS 原則進行 DNS 查詢上的篩選套用
description: 本主題是 DNS 原則案例指南的 Windows Server 2016 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e9322da3142c584c7b9d0a28396a1d1fd62ce6ee
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446398"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>使用 DNS 原則進行 DNS 查詢上的篩選套用

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來了解如何設定 Windows Server 中的 DNS 原則&reg;2016，以便建立查詢篩選器會根據您提供的準則。 

DNS 原則中的查詢篩選器可讓您設定 DNS 伺服器回應 DNS 查詢和傳送 DNS 查詢的 DNS 用戶端為基礎的方式自訂。

比方說，您也可以使用查詢篩選器封鎖清單來封鎖已知的惡意網域的 DNS 查詢回應這些網域中的查詢時，防止 DNS 設定 DNS 原則。 從 DNS 伺服器沒有回應，因為惡意網域成員的 DNS 查詢逾時。

另一個範例是建立查詢篩選器允許只有一組特定的用戶端解析特定名稱的允許清單。

## <a name="bkmk_criteria"></a> 查詢篩選準則
您可以建立任何邏輯的組合的查詢篩選器 (和/或/NOT)，下列準則。

|名稱|描述|
|-----------------|---------------------|
|用戶端子網路|預先定義的用戶端的子網路名稱。 用來驗證傳送查詢的子網路。|
|傳輸通訊協定|傳輸通訊協定查詢中使用。 可能的值為 UDP 和 TCP。|
|網際網路通訊協定|在查詢中使用的網路通訊協定。 可能的值為 IPv4 和 IPv6。|
|伺服器介面的 IP 位址|DNS 伺服器收到 DNS 要求的網路介面的 IP 位址。|
|FQDN|完整網域名稱的記錄，在查詢中，可能會使用萬用字元。|
|查詢類型|正在查詢的記錄類型\(A、 SRV、 TXT 等\)。|
|當日時間|在收到查詢的日期時間。|

下列範例會示範如何建立其中一個區塊 DNS 原則的篩選器，或讓 DNS 名稱解析查詢。

>[!NOTE]
>本主題中的範例命令會使用 Windows PowerShell 命令**新增 DnsServerQueryResolutionPolicy**。 如需詳細資訊，請參閱 <<c0> [ 新增 DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)。 

## <a name="bkmk_block1"></a>從網域的區塊查詢

在某些情況下，您可能想要封鎖的網域，您已識別為惡意的或不符合您組織的使用方式指導方針的網域的 DNS 名稱解析。 您可以使用 DNS 原則，以完成封鎖查詢的網域。

您在此範例中所設定的原則不會建立在任何特定的區域，改為您建立伺服器層級原則套用至所有 DNS 伺服器上設定的區域。 伺服器層級原則是要評估的第一個和因此第一次在查詢時要比對會收到由 DNS 伺服器。

下列範例命令會設定伺服器層級原則，以阻止與網域的任何查詢**後置詞 contosomalicious.com**。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>當您設定**動作**具有值的參數**忽略**，DNS 伺服器設定為完全捨棄查詢但未收到回覆。 這會造成惡意網域的逾時時間，DNS 用戶端。

## <a name="bkmk_block2"></a>從子網路的區塊查詢
此範例中，使用中，您可以封鎖來自子網路的查詢如果它找到的惡意程式碼受到感染，並嘗試連絡惡意網站使用您的 DNS 伺服器。 

` Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru

Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" -PassThru `

下列範例會示範如何使用子網路條件結合 FQDN 準則來封鎖從受感染的子網路特定惡意網域的要求。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

## <a name="bkmk_block3"></a>封鎖查詢的類型
您可能需要封鎖您的伺服器上的查詢特定類型的名稱解析。 例如，您可以封鎖 'ANY' 查詢，因為它可以用來建立 amplification 攻擊的惡意行為。

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

## <a name="bkmk_allow1"></a>允許查詢只從網域
您不能只使用 DNS 原則封鎖的查詢，您可以使用它們來自動核准來自特定網域或子網路的查詢。 當您設定允許清單時，DNS 伺服器只會處理查詢，從允許的網域，同時封鎖來自其他網域的所有其他查詢。

下列範例命令可讓只在電腦和裝置來查詢 DNS 伺服器的最大網域 contoso.com 和子網域。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

## <a name="bkmk_allow2"></a>允許查詢只能從子網路
您也可以建立允許清單 IP 子網路，以便不來自這些子網路的所有查詢都會被都忽略。

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

## <a name="bkmk_allow3"></a>允許只有特定 QTypes
您可以套用允許列出 QTYPEs。 

比方說，如果您有外部查詢 DNS 伺服器介面 164.8.1.1 的客戶，只有特定 QTYPEs 可進行查詢，例如 SRV 或 TXT 記錄進行名稱解析，或進行監視的內部伺服器所使用的其他 QTYPEs 時。

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

您可以建立數以千計的 DNS 原則根據您的流量管理需求，而且所有新的原則都會套用動態-不需要重新啟動 DNS 伺服器-在傳入的查詢。 
