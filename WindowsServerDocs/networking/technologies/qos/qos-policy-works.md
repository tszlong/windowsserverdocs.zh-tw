---
title: QoS 原則的運作方式
description: 本主題提供概觀品質服務 (QoS) 原則，讓您可以使用群組原則優先順序網路流量頻寬特定應用程式和 Windows Server 2016 中的服務。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1073308b5939e648fdcc2006acdce76ecf0331c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="how-qos-policy-works"></a>QoS 原則的運作方式

>適用於：Windows Server（以每年次管道）、Windows Server 2016

當開始或向可能受到更新的使用者或電腦群組原則設定的 QoS，下列程序。

1. 群組原則引擎 Active directory 擷取電腦的使用者或群組原則設定。

2. 群組原則引擎會通知 QoS Client 端延伸 QoS 原則中已變更。

3. QoS Client 端延伸模組 QoS 檢查傳送 QoS 原則事件通知。

4. 擷取的使用者或電腦 QoS 原則 QoS 偵測模組，並將它們儲存。

當新傳輸層端點 \（TCP 連接或 UDP traffic\）建立，下列程序。

1. TCP/IP 堆疊傳輸層元件通知 QoS 偵測模組。

2. QoS 偵測模組比較傳輸層端點儲存 QoS 原則的參數。

3. 如果您找到符合 QoS 偵測模組連絡人 Pacer.sys 建立包含 DSCP 值，資料傳輸節流對應 QoS 原則設定的資料結構流程。 如果有多個 QoS 原則符合傳輸層的端點的參數，則使用最特定 QoS 原則。

4. Pacer.sys 儲存流程，並傳回對應 QoS 偵測模組 flow 流程數字。

5. QoS 偵測模組傳回傳輸層流量號碼。

6. 傳輸層儲存傳輸層端點流程電話號碼。

對應至傳輸層端點封包標示流程數字會傳送，下列程序。

1. 傳輸層內部標記流量號碼封的包。

2. 網路層級查詢 Pacer.sys 對應的封包流程數目 DSCP 值。

3. Pacer.sys 網路層級會 DSCP 值。

4. 網路層級變更 DSCP 值 Pacer.sys 指定 IPv4 TO 欄位或 IPv6 流量課程欄位，並 IPv4 封包，計算最終 IPv4 標頭檢查值。

5. 網路層級框架層傳遞封包。

6. 因為封包已標示的流程數字，框架層將封包交給 Pacer.sys NDIS 透過 6.x。

7. Pacer.sys 來判斷封包需要節流，如果是，使用封包流量號碼、排定的傳送封包。

8. Pacer.sys 傳遞封包可以立即 \（如果未流量 throttling\）或已排定為 \（如果流量 throttling\）來 NDIS 6.x 傳輸到適當的網路介面卡。

這些處理程序的原則為主 QoS 提供下列優點。

- 檢查以判斷是否 QoS 原則套用流量是每個傳輸層端點，而不是每一封包。

- 也不會效能影響流量不符合 QoS 原則。

- 利用 DSCP 型已區分服務或流量節流修改不需要應用程式。

- 交通受 IPsec 可套用 QoS 原則。

本指南下一步主題，請查看[QoS 原則架構](qos-policy-architecture.md)。

本指南中第一次主題，請查看[品質服務 (QoS) 原則](qos-policy-top.md)。
