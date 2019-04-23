---
title: 網路子系統效能調整
description: 本主題是 Windows Server 2016 的網路子系統效能調整指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c0706c6ddbb678eacd3e609cfad3ccdda943fbd3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857409"
---
# <a name="network-subsystem-performance-tuning"></a>網路子系統效能調整

>適用於：Windows Server （半年通道），Windows Server 2016

如需網路子系統的概觀，並如本指南中的其他主題的連結，您可以使用本主題。

>[!NOTE]
>除了本主題中，本指南的下列各節會提供效能微調建議，為網路裝置和網路堆疊。
> - [選擇網路介面卡](net-sub-choose-nic.md)
> - [設定網路介面的順序](net-sub-interface-metric.md)
> - [效能調整網路介面卡](net-sub-performance-tuning-nics.md)
> - [網路相關效能計數器](net-sub-performance-counters.md)
> - [網路工作負載的效能工具](net-sub-performance-tools.md)

效能調整網路子系統，特別是針對網路密集型工作負載，可能是網路架構，也稱為網路堆疊的每個圖層。 這些層級大致可分成下列各節。

1. **網路介面**。 這是在網路堆疊中，最低層級，而且包含會直接與網路介面卡通訊的網路驅動程式。

2. **網路驅動程式介面規格 (NDIS)**。 NDIS 會公開其下的驅動程式和它的上方，圖層，例如通訊協定堆疊的介面。
  
3. **通訊協定堆疊**。 通訊協定堆疊實作通訊協定 TCP/IP 和 UDP/IP。 這些層級公開其上方的圖層的傳輸層介面。
  
4. **系統驅動程式**。 這些通常是使用傳輸資料延伸模組 (TDX) 或 Winsock Kernel (WSK) 介面來公開介面，以使用者模式應用程式的用戶端。 Windows Server 2008 和 Windows 中引進了 WSK 介面&reg;Vista，而且由 AFD.sys。 介面會排除使用者模式和核心模式之間切換，以提高效能。
  
5. **使用者模式應用程式**。 這些通常是 Microsoft 解決方案或自訂應用程式。

下表提供的網路堆疊，包括範例，在每個圖層中執行的項目層級的垂直解說。  

![網路堆疊層](../../media/Network-Subsystem/network-layers.jpg)

