---
title: QoS 原則的運作方式
description: 本主題提供服務品質 (QoS) 原則的總覽，可讓您使用群組原則來排定 Windows Server 2016 中特定應用程式和服務的網路流量頻寬。
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 41bbf2245143281cd80a46c23c2c6bf704ea3ccc
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97943164"
---
# <a name="how-qos-policy-works"></a>QoS 原則的運作方式

>適用於：Windows Server (半年度管道)、Windows Server 2016

當您啟動或取得更新的使用者或電腦設定群組原則 QoS 的設定時，會發生下列進程。

1. 群組原則引擎會從 Active Directory 抓取使用者或電腦設定群組原則設定。

2. 群組原則引擎會通知 QoS Client-Side 擴充功能在 QoS 原則中有變更。

3. QoS Client-Side 擴充功能會將 QoS 原則事件通知傳送到 QoS 檢查模組。

4. QoS 偵測模組會抓取使用者或電腦 QoS 原則並加以儲存。

建立新的傳輸層端點 \( TCP 連線或 UDP 流量時 \) ，會發生下列進程。

1. TCP/IP 堆疊的傳輸層元件會通知 QoS 檢查模組。

2. QoS 偵測模組會將傳輸層端點的參數與儲存的 QoS 原則進行比較。

3. 如果找到相符的結果，QoS 檢查模組會聯絡 Pacer.sys 建立流程，此資料結構包含 DSCP 值和相符 QoS 原則的流量節流設定。 如果有多個 QoS 原則符合傳輸層端點的參數，則會使用最特定的 QoS 原則。

4. Pacer.sys 儲存流程，並傳回對應至 QoS 檢查模組流程的流程號碼。

5. QoS 偵測模組會將流量號碼傳回傳輸層。

6. 傳輸層會將流量號碼與傳輸層端點儲存在一起。

當傳送的封包對應到以流量號碼標示的傳輸層端點時，會發生下列進程。

1. 傳輸層會在內部以流量號碼標示封包。

2. 網路層查詢會 Pacer.sys 與封包的流量號碼相對應的 DSCP 值。

3. Pacer.sys 會將 DSCP 值傳回網路層。

4. 網路層會將 IPv4 TOS 欄位或 IPv6 流量類別欄位變更為 Pacer.sys 所指定的 DSCP 值，而針對 IPv4 封包，則會計算最終的 IPv4 標頭總和檢查碼。

5. 網路層將封包交給框架層。

6. 因為封包已標示流程號碼，所以框架層會透過 NDIS 6.x 將封包交給 Pacer.sys。

7. Pacer.sys 使用封包的流量號碼來判斷封包是否需要進行節流處理，如果是，則會排程要傳送的封包。

8. Pacer.sys 如果沒有流量節流，請立即將封包交給， \( \) \( 如果有對 NDIS 6.x 的流量節流，則會在 \) 適當的網路介面卡進行傳輸。

這些以原則為基礎的 QoS 進程提供下列優點。

- 檢查流量以判斷 QoS 原則是否適用于每個傳輸層端點，而不是每個封包。

- 不符合 QoS 原則的流量不會影響效能。

- 應用程式不需要修改，就能利用以 DSCP 為基礎的差異服務或流量節流。

- QoS 原則可套用至以 IPsec 保護的流量。

如需本指南的下一個主題，請參閱 [QoS 原則架構](qos-policy-architecture.md)。

本指南的第一個主題中，請參閱 [服務品質 (QoS) 原則](qos-policy-top.md)。
