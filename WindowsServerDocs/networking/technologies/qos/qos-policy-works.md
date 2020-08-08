---
title: QoS 原則的運作方式
description: 本主題概述服務品質 (QoS) 原則，這可讓您使用群組原則來設定 Windows Server 2016 中特定應用程式和服務的網路流量頻寬優先順序。
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fe91bba99000be307ed011cb5636dc49d65c389a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942528"
---
# <a name="how-qos-policy-works"></a>QoS 原則的運作方式

>適用於：Windows Server (半年度管道)、Windows Server 2016

當啟動或取得更新的使用者或電腦設定群組原則的 QoS 時，將會發生下列進程。

1. 群組原則引擎會從 Active Directory 抓取使用者或電腦設定群組原則設定。

2. 群組原則引擎會通知 QoS 用戶端延伸，其中有 QoS 原則的變更。

3. QoS 用戶端延伸會將 QoS 原則事件通知傳送至 QoS 偵測模組。

4. QoS 偵測模組會抓取使用者或電腦的 QoS 原則，並加以儲存。

當新的傳輸層端點 \( TCP 連線或 UDP 流量 \) 建立時，會發生下列程式。

1. TCP/IP 堆疊的傳輸層元件會通知 QoS 偵測模組。

2. QoS 檢查模組會將傳輸層端點的參數與儲存的 QoS 原則做比較。

3. 如果找到相符的資料，QoS 偵測模組會聯絡 Pacer.sys 以建立流程，這是一個包含 DSCP 值和符合 QoS 原則之流量節流設定的資料結構。 如果有多個 QoS 原則符合傳輸層端點的參數，則會使用最特定的 QoS 原則。

4. Pacer.sys 儲存流程，並傳回對應至 QoS 偵測模組流程的流程號碼。

5. QoS 偵測模組會將流量號碼傳回傳輸層。

6. 傳輸層會將流量號碼與傳輸層端點儲存在一起。

當傳送對應至以流量號碼標示之傳輸層端點的封包時，會發生下列程式。

1. 傳輸層在內部以流量號碼標示封包。

2. 網路層查詢 Pacer.sys，以取得對應至封包之流量號碼的 DSCP 值。

3. Pacer.sys 會將 DSCP 值傳回網路層。

4. 網路層會將 [IPv4 TOS] 欄位或 [IPv6 流量類別] 欄位變更為 Pacer.sys 所指定的 DSCP 值，而針對 IPv4 封包，則會計算最終的 IPv4 標頭總和檢查碼。

5. 網路層將封包交給框架層。

6. 因為封包已標記為流量號碼，所以框架層會將封包交給透過 NDIS 6.x Pacer.sys。

7. Pacer.sys 使用封包的流量號碼來判斷封包是否需要進行節流處理，如果是，則會排程封包以進行傳送。

8. 如果沒有流量節流，請 Pacer.sys 直接將封包， \( \) 或在流量節流處理至 NDIS 6.x 以透過 \( \) 適當的網路介面卡進行傳輸時，立即取得封包。

以原則為依據的 QoS 的這些程式會提供下列優點。

- 檢查流量以判斷 QoS 原則是否適用于每個傳輸層端點，而不是針對每個封包進行。

- 不符合 QoS 原則的流量不會有任何效能影響。

- 不需要修改應用程式，即可利用以 DSCP 為基礎的差異服務或流量節流。

- QoS 原則可套用至以 IPsec 保護的流量。

如需本指南中的下一個主題，請參閱[QoS 原則架構](qos-policy-architecture.md)。

如需本指南的第一個主題，請參閱[服務品質 (QoS) 原則](qos-policy-top.md)。
