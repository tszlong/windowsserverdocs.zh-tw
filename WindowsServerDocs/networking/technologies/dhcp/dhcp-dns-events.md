---
title: DNS 記錄註冊的 DHCP 記錄事件
description: 本主題提供有關 Windows Server 2016 中的 DHCP 伺服器記錄事件的資訊。
manager: brianlic
ms.topic: how-to
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 51ca6af9b060b754f75de63089464d8310263734
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948804"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>DNS 註冊的 DHCP 記錄事件

>適用於：Windows Server (半年度管道)、Windows Server 2016

DHCP 伺服器事件記錄檔現在提供有關 DNS 註冊失敗的詳細資訊。

>[!NOTE]
>在許多情況下，DHCP 伺服器的 DNS 記錄登錄失敗原因是 DNS 反向對應 \- 區域設定錯誤或根本未設定。

下列新的 DHCP 事件可協助您輕鬆識別 DNS 註冊失敗的原因，因為 DNS 反向對應區域設定錯誤或遺失 \- 。

|識別碼|事件|值|
|-----|--------------------|--------------------------------------------------------|
|20317|ForwardRecordDNSFailure|IPv4 位址 %1 和 FQDN %2 的轉送記錄註冊失敗，錯誤為 %3。 這可能是因為此記錄的正向對應區域不存在於 DNS 伺服器上。|
|20318|ForwardRecordDNSTimeout|IPv4 位址 %1 和 FQDN %2 的轉送記錄註冊失敗，錯誤為 %3。|
|20319|PTRRecordDNSFailure|IPv4 位址 %1 和 FQDN %2 的 PTR 記錄註冊失敗，錯誤為 %3。 這可能是因為此記錄的反向對應區域不存在於 DNS 伺服器上。|
|20320|PTRRecordDNSTimeout|IPv4 位址 %1 和 FQDN %2 的 PTR 記錄註冊失敗，錯誤為 %3。|
|20321|DHCPv6. ForwardRecordDNSFailure|IPv6 位址 %1 和 FQDN %2 的轉送記錄註冊失敗，錯誤為 %3。 這可能是因為此記錄的正向對應區域不存在於 DNS 伺服器上。|
|20322|DHCPv6. ForwardRecordDNSTimeout|IPv6 位址 %1 和 FQDN %2 的轉送記錄註冊失敗，錯誤為 %3。|
|20323|DHCPv6. PTRRecordDNSFailure|IPv6 位址 %1 和 FQDN %2 的 PTR 記錄註冊失敗，錯誤為 %3。 這可能是因為此記錄的反向對應區域不存在於 DNS 伺服器上。|
|20324|DHCPv6. PTRRecordDNSTimeout|IPv6 位址 %1 和 FQDN %2 的 PTR 記錄註冊失敗，錯誤為 %3。|
|20325|ForwardRecordDNSError|IPv4 位址 %1 和 FQDN %2 的 PTR 記錄註冊失敗，錯誤： %3 \( %4 \) 。|
|20326|DHCPv6. ForwardRecordDNSError|IPv6 位址 %1 和 FQDN %2 的轉送記錄註冊失敗，錯誤為 %3 \( %4\)|
|20327|DHCPv6. PTRRecordDNSError|IPv6 位址 %1 和 FQDN %2 的 PTR 記錄註冊失敗，錯誤： %3 \( %4 \) 。|

