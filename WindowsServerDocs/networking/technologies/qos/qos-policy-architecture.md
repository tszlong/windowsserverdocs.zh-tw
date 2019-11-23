---
title: QoS 原則架構
description: 本主題提供「服務品質」（QoS）原則的總覽，讓您可以使用群組原則來設定 Windows Server 2016 中特定應用程式和服務的網路流量頻寬優先順序。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 609d3f28465380b7d15648cfeb73070a39b9362f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395978"
---
# <a name="qos-policy-architecture"></a>QoS 原則架構

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解 QoS 原則的架構。

下圖顯示以原則為依據的 QoS 的架構。

![QoS 原則的架構](../../media/QoS/QoS-Policy-Architecture.jpg)

以原則為依據的 QoS 的架構包含下列元件：

- **群組原則用戶端服務**。 一種 Windows 服務，負責管理使用者和電腦設定群組原則設定。

- **群組原則引擎**。 群組原則用戶端服務的元件，它會在啟動時從 Active Directory 抓取使用者和電腦設定群組原則設定，並定期檢查預設 \(的變更（每90分鐘\)）。 如果偵測到變更，群組原則引擎會抓取新的群組原則設定。 群組原則引擎會處理傳入的 Gpo，並在 QoS 原則更新時通知 QoS 用戶端延伸。

- **QoS 用戶端延伸**。 群組原則用戶端服務的元件，會等待 QoS 原則已變更的群組原則引擎指示，並通知 QoS 偵測模組。

- **Tcp/ip 堆疊**。 TCP/IP 堆疊，其中包含對 IPv4 和 IPv6 的整合支援，並且支援 Windows 篩選平台。 

- **QoS 檢查**。 模組一個 TCP/IP 堆疊中的元件，會等待 qos 用戶端延伸中的 QoS 原則變更，並抓取 QoS 原則設定，並與傳輸層和 Pacer 互動，以內部標示符合 QoS 原則的流量。

- **NDIS**6.x. x。 核心模式網路驅動程式與 Windows Server 和用戶端作業系統中的作業系統之間的標準介面。 NDIS 6.x 支援輕量篩選，這是可提供較佳效能的 NDIS 中繼驅動程式和微型埠驅動程式的簡化驅動程式模型。

- **QoS 網路提供者介面 \(NPI\)** 。 用於與 Pacer 互動之核心模式驅動程式的介面。

- **Pacer. sys**。 NDIS 6. x 輕量篩選器驅動程式，可控制以原則為依據的 QoS 的封包排程，以及使用一般 QoS \(GQoS\) 和流量控制 \(TC\) Api 的應用程式流量。 Pacer. sys 已取代 Windows Server 2003 和 Windows XP 中的 Psched.sys。 Pacer 會隨著 QoS 封包排程器元件從網路連線或介面卡的內容安裝。

如需本指南中的下一個主題，請參閱[QoS 原則案例](qos-policy-scenarios.md)。

如需本指南的第一個主題，請參閱[服務品質（QoS）原則](qos-policy-top.md)。

