---
title: QoS 原則架構
description: 本主題概述的服務品質 (QoS) 原則，可讓您使用群組原則來設定特定的應用程式和服務 Windows Server 2016 中的網路流量頻寬優先順序。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bad37ba3558137b02ae495fe8dd9be2c903cdd97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843139"
---
# <a name="qos-policy-architecture"></a>QoS 原則架構

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來深入了解 QoS 原則的架構。

下圖顯示以原則為基礎的 QoS 的架構。

![QoS 原則的架構](../../media/QoS/QoS-Policy-Architecture.jpg)

原則式 QoS 的架構是由下列元件所組成：

- **群組原則用戶端服務**。 管理使用者和電腦設定群組原則設定 Windows 服務。

- **群組原則引擎**。 在啟動時，並定期從 Active Directory 擷取使用者和電腦設定群組原則設定群組原則用戶端服務的元件會檢查變更\(根據預設，每隔 90 分鐘\)。 如果偵測到變更，群組原則引擎會擷取新的群組原則設定。 群組原則引擎會處理傳入的 Gpo，並通知 QoS 用戶端延伸 QoS 原則更新時。

- **QoS 用戶端延伸**。 群組原則用戶端服務會等候 QoS 原則已變更的群組原則引擎的指示，並通知 QoS 偵測模組元件。

- **TCP/IP 堆疊**。 TCP/IP 堆疊包含 IPv4 和 IPv6 的整合式的支援和支援 Windows 篩選平台。 

- **QoS 檢查**。 模組內的 TCP/IP 堆疊可等候 QoS 用戶端延伸 QoS 原則變更的指示，擷取 QoS 原則設定，並與在內部標示符合 QoS 流量的傳輸層和 Pacer.sys 互動的元件原則。

- **NDIS 6.x**。 核心模式網路驅動程式和 Windows Server 和用戶端作業系統中的作業系統之間的標準介面。 NDIS 6.x 支援輕量型篩選器，這是針對 NDIS 中繼驅動程式和 miniport 驅動程式簡化的驅動程式模型提供更佳的效能。

- **QoS 網路提供者介面\(NPI\)**。 核心模式驅動程式，用以與 Pacer.sys 互動的介面。

- **Pacer.sys**。 NDIS 6.x 輕量型篩選器驅動程式，控制封包排程原則為依據的 QoS 和採用 Generic QoS 的應用程式的流量\(GQoS\)和流量控制\(TC\) Api。 Pacer.sys 取代在 Windows Server 2003 和 Windows XP 中的 Psched.sys。 Pacer.sys 被隨 QoS 封包排程器元件從 網路連線或配接器的屬性。

如本指南中的下一個主題，請參閱 < [QoS 原則案例](qos-policy-scenarios.md)。

如本指南中的第一個主題，請參閱 <<c0> [ 服務品質 (QoS) 原則](qos-policy-top.md)。

