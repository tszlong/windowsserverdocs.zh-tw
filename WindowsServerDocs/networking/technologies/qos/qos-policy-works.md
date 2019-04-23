---
title: QoS 原則的運作方式
description: 本主題概述的服務品質 (QoS) 原則，可讓您使用群組原則來設定特定的應用程式和服務 Windows Server 2016 中的網路流量頻寬優先順序。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 272272c833bb38924f1daa5561037901f6ff4e25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864279"
---
# <a name="how-qos-policy-works"></a>QoS 原則的運作方式

>適用於：Windows Server （半年通道），Windows Server 2016

當啟動或取得更新的使用者或電腦設定群組原則設定 qos，就會發生下列程序。

1. 群組原則引擎會從 Active Directory 擷取使用者或電腦設定群組原則設定。

2. 群組原則引擎通知 QoS 用戶端延伸 QoS 原則中沒有變更。

3. QoS 用戶端延伸傳送 QoS 原則事件通知 QoS 偵測模組來。

4. QoS 偵測模組擷取的使用者或電腦 QoS 原則，並將它們儲存。

當新的傳輸層端點\(TCP 連接或 UDP 流量\)建立時，會發生下列處理序。

1. TCP/IP 堆疊的傳輸層元件通知 QoS 偵測模組。

2. QoS 偵測模組會比較儲存的 QoS 原則的傳輸層端點的參數。

3. 如果找到相符項目，QoS 偵測模組會連絡 Pacer.sys 建立流程時，資料結構，包含 DSCP 值和節流設定比對的 QoS 原則的流量。 如果有多個 QoS 原則對應的傳輸層端點的參數，則會使用最特定的 QoS 原則。

4. Pacer.sys 儲存流量，並傳回對應至 QoS 偵測模組的整個流程的流程數字。

5. QoS 偵測模組會將流量號碼傳回傳輸層。

6. 傳輸層會儲存與傳輸層端點的流量號碼。

傳輸層端點相對應的封包有流量號碼的標記時傳送，會發生下列處理序。

1. 傳輸層在內部標示封包以流量號碼。

2. 網路層查詢 Pacer.sys 封包的流量號碼相對應的 DSCP 值。

3. Pacer.sys 將 DSCP 值回到網路層級。

4. 網路層級變更為 Pacer.sys 所指定的 DSCP 值 IPv4 TOS 欄位或 IPv6 Traffic Class 欄位，並針對 IPv4 封包計算最終的 IPv4 標頭總和檢查碼。

5. 網路層級會將封包交給框架層。

6. 因為已有流量號碼標示封包，所以框架層將封包交給 Pacer.sys 透過 NDIS 6.x。

7. Pacer.sys 使用封包的流量號碼，以判斷封包必須進行節流處理，且如果是的話，則排定封包以便傳送。

8. Pacer.sys 將封包交給是立即\(如果沒有流量節流\)或依排程\(如果流量節流\)NDIS 來 6.x 透過適當的網路介面卡的傳輸。

這些程序的原則為依據的 QoS 提供下列優點。

- -傳輸層端點，而不是每個封包，會完成的流量，以判斷是否要套用 QoS 原則檢查。

- 沒有任何效能影響不符合 QoS 原則的流量。

- 應用程式不需要修改，以利用 DSCP 架構的差異服務或流量節流。

- QoS 原則可以套用到以 IPsec 保護的流量。

如本指南中的下一個主題，請參閱 < [QoS 原則架構](qos-policy-architecture.md)。

如本指南中的第一個主題，請參閱 <<c0> [ 服務品質 (QoS) 原則](qos-policy-top.md)。
