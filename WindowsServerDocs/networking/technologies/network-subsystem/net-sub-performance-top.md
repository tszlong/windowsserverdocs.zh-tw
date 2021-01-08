---
title: 網路子系統效能調整
description: 深入瞭解網路子系統，以及本指南中其他主題的連結。
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 08/07/2020
ms.openlocfilehash: 8684630634dcb1d1a5998f69f98ce16dd32b7f38
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040438"
---
# <a name="network-subsystem-performance-tuning"></a>網路子系統效能調整

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解網路子系統的總覽，以及本指南中其他主題的連結。

>[!NOTE]
>除了本主題之外，本指南的下列各節還提供網路裝置和網路堆疊的效能微調建議。
> - [選擇網路介面卡](net-sub-choose-nic.md)
> - [設定網路介面的順序](net-sub-interface-metric.md)
> - [效能調整網路介面卡](net-sub-performance-tuning-nics.md)
> - [網路相關效能計數器](net-sub-performance-counters.md)
> - [網路工作負載的效能工具](net-sub-performance-tools.md)

調整網路子系統的效能，特別是針對需要大量網路的工作負載，可牽涉到網路架構的每一層，也稱為網路堆疊。 這些圖層廣泛分成下列各節。

1. **網路介面**。 這是網路堆疊中的最低層，包含直接與網路介面卡通訊的網路驅動程式。

2. **(NDIS) 的網路驅動程式介面規格**。 NDIS 會公開其下的驅動程式介面，以及其上方的層級，例如通訊協定堆疊。

3. **通訊協定堆疊**。 通訊協定堆疊會實行 TCP/IP 和 UDP/IP 等通訊協定。 這些圖層會公開其上方各層的傳輸層介面。

4. **系統驅動程式**。 這些通常是使用傳輸資料延伸模組 (TDX) 或 Winsock 核心 (WSK) 介面，以公開介面給使用者模式應用程式的用戶端。 WSK 介面是在 Windows Server 2008 和 Windows Vista 中引進的 &reg; ，而且是由 AFD.sys 所公開。 介面藉由消除在使用者模式和核心模式之間切換，來改善效能。

5. **使用者模式應用程式**。 這些通常是 Microsoft 解決方案或自訂應用程式。

下表提供網路堆疊層級的垂直圖例，包括在每個圖層中執行之專案的範例。

![網路堆疊層](../../media/Network-Subsystem/network-layers.jpg)

