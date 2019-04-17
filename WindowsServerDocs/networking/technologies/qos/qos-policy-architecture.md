---
title: QoS 原則架構
description: 本主題提供概觀品質服務 (QoS) 原則，讓您可以使用群組原則優先順序網路流量頻寬特定應用程式和 Windows Server 2016 中的服務。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00d36604c57add6bf9f45b0166b08c1fb15be467
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-architecture"></a>QoS 原則架構

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以深入了解 QoS 原則的架構。

下圖顯示原則為主 QoS 的架構。

![QoS 原則的架構](../../media/QoS/QoS-Policy-Architecture.jpg)

原則為主 QoS 的架構所組成下列元件：

- **群組原則 Client 服務**。 Windows 的服務管理使用者和電腦群組原則設定。

- **群組原則引擎**。 使用者和電腦群組原則設定擷取 Active Directory 開機時和定期群組原則 Client 服務的元件檢查對 \（預設每 90 minutes\）。 如果偵測到的變更，群組原則引擎擷取的新群組原則設定。 群組原則引擎處理傳入 Gpo 和 QoS 原則更新時通知 QoS Client 側邊擴充功能。

- **QoS 用側邊延伸**。 等待 QoS 原則已變更群組原則引擎的指示，會通知 QoS 偵測模組群組原則 Client 服務的元件。

- **TCP/IP 堆疊**。 TCP/IP 堆疊包含 IPv4 和 IPv6 整合的支援與支援的 Windows 篩選平台。 

- **檢查 QoS**。 中的指示 QoS Client 側邊擴充功能，從 QoS 原則變更等待 TCP/IP 堆疊模組元件擷取 QoS 原則設定，並互動傳輸層並 Pacer.sys 內部標記的流量符合 QoS 原則。

- **NDIS 6.x**。 標準介面之間核心模式網路驅動程式和 Windows Server 和 Client 的作業系統中的作業系統。 NDIS 6.x 支援輕量的篩選器簡化驅動程式模型 NDIS 中繼驅動程式和迷你連接埠驅動程式，可提供更好的效能。

- **QoS 網路提供者介面 \(NPI\)**。 Interface 互動 Pacer.sys 核心模式驅動程式。

- **Pacer.sys**。 NDIS 6.x 輕量 filter 驅動程式的控制項封包排程原則為主 QoS 和流量的應用程式使用一般 QoS \(GQoS\) 和控制資料傳輸 \(TC\) Api。 Pacer.sys 取代 Psched.sys Windows Server 2003 及 Windows XP 中。 安裝 Pacer.sys 與 QoS 封包排程元件從網路或介面卡的屬性。

本指南下一步主題，請查看[QoS 原則案例](qos-policy-scenarios.md)。

本指南中第一次主題，請查看[品質服務 (QoS) 原則](qos-policy-top.md)。

