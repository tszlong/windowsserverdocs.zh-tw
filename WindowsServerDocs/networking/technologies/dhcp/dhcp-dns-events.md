---
title: DNS 使用碼表進行登錄 DHCP 登入事件
description: 本主題提供 DHCP 伺服器的相關資訊的登入 Windows Server 2016 中的活動。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 61a5099cd5e1ef1d4687baa8c20411c96ea8f519
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>DNS 登錄 DHCP 登入事件

>適用於：Windows Server（以每年次管道）、Windows Server 2016

DHCP 伺服器事件登現在提供 DNS 登記失敗的詳細的資訊。

>[!NOTE]
>很多時候，DHCP 伺服器 DNS 使用碼表進行登記失敗的原因是的 DNS Reverse\-對應區域是設定不正確或未設定。

下列新 DHCP 事件可協助您輕鬆地找出 DNS 登錄設定錯誤而無法或遺失 DNS Reverse\-對應的區域。

|來電顯示|事件|值。|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4.ForwardRecordDNSFailure|適用於 IPv4 位址 %1 和 FQDN %2 向前使用碼表進行登記失敗，錯誤 %3。 這可能是因為不存在正向對應區域這個記錄 DNS 伺服器上。|
|20318|DHCPv4.ForwardRecordDNSTimeout|適用於 IPv4 位址 %1 和 FQDN %2 向前使用碼表進行登記失敗，錯誤 %3。|
|20319|DHCPv4.PTRRecordDNSFailure|適用於 IPv4 位址 %1 和 FQDN %2 PTR 使用碼表進行登記失敗，錯誤 %3。 這可能是因為不存在反向對應區域這個記錄 DNS 伺服器上。|
|20320|DHCPv4.PTRRecordDNSTimeout|適用於 IPv4 位址 %1 和 FQDN %2 PTR 使用碼表進行登記失敗，錯誤 %3。|
|20321|DHCPv6.ForwardRecordDNSFailure|適用於 IPv6 位址 %1 和 FQDN %2 向前使用碼表進行登記失敗，錯誤 %3。 這可能是因為不存在正向對應區域這個記錄 DNS 伺服器上。|
|20322|DHCPv6.ForwardRecordDNSTimeout|適用於 IPv6 位址 %1 和 FQDN %2 向前使用碼表進行登記失敗，錯誤 %3。|
|20323|DHCPv6.PTRRecordDNSFailure|適用於 IPv6 位址 %1 和 FQDN %2 PTR 使用碼表進行登記失敗，錯誤 %3。 這可能是因為不存在反向對應區域這個記錄 DNS 伺服器上。|
|20324|DHCPv6.PTRRecordDNSTimeout|適用於 IPv6 位址 %1 和 FQDN %2 PTR 使用碼表進行登記失敗，錯誤 %3。|
|20325|DHCPv4.ForwardRecordDNSError|適用於 IPv4 位址 %1 和 FQDN %2 PTR 使用碼表進行登記失敗，錯誤 %3 \(%4\)。|
|20326|DHCPv6.ForwardRecordDNSError|適用於 IPv6 位址 %1 和 FQDN %2 向前使用碼表進行登記失敗，錯誤 %3 \(%4\)|
|20327|DHCPv6.PTRRecordDNSError|適用於 IPv6 位址 %1 和 FQDN %2 PTR 使用碼表進行登記失敗，錯誤 %3 \(%4\)。|

