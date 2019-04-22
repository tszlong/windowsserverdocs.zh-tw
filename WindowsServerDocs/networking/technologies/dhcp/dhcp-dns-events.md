---
title: DNS 記錄登錄的 DHCP 記錄事件
description: 本主題提供 Windows Server 2016 中的記錄事件的 DHCP 伺服器的相關資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f4ce217b19cfd8a63bff1ae504362d4fc24fcd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816899"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>DNS 登錄的 DHCP 記錄事件

>適用於：Windows Server （半年通道），Windows Server 2016

DHCP 伺服器事件記錄檔現在會提供 DNS 註冊失敗的詳細的資訊。

>[!NOTE]
>在許多情況下，由 DHCP 伺服器的 DNS 記錄登錄失敗的原因是 DNS 反向\-對應區域設定不正確或未完全設定。

下列新的 DHCP 事件可協助您更容易找出失敗的原因設定不正確或遺失的 DNS 反向 DNS 登錄時\-對應區域。

|識別碼|Event - 事件|值|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4.ForwardRecordDNSFailure|%1 的 IPv4 位址和 FQDN %2 的轉送記錄登錄失敗，錯誤為 %3。 這可能是因為這筆記錄的正向對應區域沒有 DNS 伺服器上。|
|20318|DHCPv4.ForwardRecordDNSTimeout|%1 的 IPv4 位址和 FQDN %2 的轉送記錄登錄失敗，錯誤為 %3。|
|20319|DHCPv4.PTRRecordDNSFailure|%1 的 IPv4 位址和 FQDN %2 的 PTR 記錄登錄失敗，錯誤為 %3。 這可能是因為這筆記錄的反向對應區域沒有 DNS 伺服器上。|
|20320|DHCPv4.PTRRecordDNSTimeout|%1 的 IPv4 位址和 FQDN %2 的 PTR 記錄登錄失敗，錯誤為 %3。|
|20321|DHCPv6.ForwardRecordDNSFailure|%1 的 IPv6 位址和 FQDN %2 的轉送記錄登錄失敗，錯誤為 %3。 這可能是因為這筆記錄的正向對應區域沒有 DNS 伺服器上。|
|20322|DHCPv6.ForwardRecordDNSTimeout|%1 的 IPv6 位址和 FQDN %2 的轉送記錄登錄失敗，錯誤為 %3。|
|20323|DHCPv6.PTRRecordDNSFailure|%1 的 IPv6 位址和 FQDN %2 的 PTR 記錄登錄失敗，錯誤為 %3。 這可能是因為這筆記錄的反向對應區域沒有 DNS 伺服器上。|
|20324|DHCPv6.PTRRecordDNSTimeout|%1 的 IPv6 位址和 FQDN %2 的 PTR 記錄登錄失敗，錯誤為 %3。|
|20325|DHCPv4.ForwardRecordDNSError|%1 的 IPv4 位址和 FQDN %2 的 PTR 記錄登錄失敗，錯誤 %3 \(%4\)。|
|20326|DHCPv6.ForwardRecordDNSError|%1 的 IPv6 位址和 FQDN %2 的轉送記錄登錄失敗，錯誤 %3 \(%4\)|
|20327|DHCPv6.PTRRecordDNSError|%1 的 IPv6 位址和 FQDN %2 的 PTR 記錄登錄失敗，錯誤 %3 \(%4\)。|

