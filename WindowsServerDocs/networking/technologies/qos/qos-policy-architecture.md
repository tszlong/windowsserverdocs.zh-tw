---
title: QoS 原則架構
description: 深入瞭解 QoS 原則的架構。
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 68b61610a28c81141e336826229793f681276ade
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98113284"
---
# <a name="qos-policy-architecture"></a>QoS 原則架構

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解 QoS 原則的架構。

下圖顯示以原則為基礎的 QoS 架構。

![QoS 原則的架構](../../media/QoS/QoS-Policy-Architecture.jpg)

以原則為基礎的 QoS 架構是由下列元件所組成：

- **群組原則用戶端服務**。 管理使用者和電腦設定群組原則設定的 Windows 服務。

- **群組原則引擎**。 群組原則用戶端服務的元件，會在啟動時從 Active Directory 中取出使用者和電腦設定群組原則設定，並定期 \( 每隔90分鐘檢查變更 \) 。 如果偵測到變更，群組原則引擎會捕獲新的群組原則設定。 群組原則引擎會處理傳入的 Gpo，並在更新 QoS 原則時通知 QoS 用戶端延伸。

- **QoS 用戶端延伸**。 群組原則用戶端服務的元件，等候 QoS 原則變更的群組原則引擎指出，並通知 QoS 檢查模組。

- **Tcp/ip 堆疊**。 TCP/IP 堆疊，其中包含 IPv4 和 IPv6 的整合支援，並支援 Windows 篩選平台。

- **QoS 檢查**。 模組 A TCP/IP 堆疊內的元件，可等候 qos 用戶端延伸的 QoS 原則變更、抓取 QoS 原則設定，以及與傳輸層和 Pacer.sys 進行互動，以在內部標示符合 QoS 原則的流量。

- **NDIS** 6.x。 核心模式網路驅動程式與 Windows Server 和用戶端作業系統中作業系統之間的標準介面。 NDIS 6.x 支援輕量篩選器，這是針對 NDIS 中繼驅動程式和微型埠驅動程式簡化的驅動程式模型，可提供更好的效能。

- **QoS 網路提供者介面 \(NPI \)**。 核心模式驅動程式的介面，可與 Pacer.sys 互動。

- **Pacer.sys**。 NDIS 6.x 輕型濾波器驅動程式，可控制以原則為基礎的 QoS 的封包排程，以及使用一般 QoS \( GQoS \) 和流量控制 \( TC api 的應用程式流量 \) 。 Pacer.sys 取代了 Windows Server 2003 和 Windows XP 中的 Psched.sys。 Pacer.sys 會與 QoS 封包排程器元件安裝在網路連線或介面卡的內容中。

如需本指南的下一個主題，請參閱 [QoS 原則案例](qos-policy-scenarios.md)。

本指南的第一個主題中，請參閱 [服務品質 (QoS) 原則](qos-policy-top.md)。

