---
title: 網路效能子系統調整
description: 本主題是部分的 Windows Server 2016 的網路效能子系統調整節目表。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7ef50335a6dcc7dc5187cc30ff1b2dc2c5cdfed
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-subsystem-performance-tuning"></a>網路效能子系統調整

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題的網路子系統概觀和其他本指南主題的連結。

>[!NOTE]
>本主題中，除了本指南下列章節提供的網路的裝置效能調整建議和網路堆疊。
> - [選擇 [網路介面卡](net-sub-choose-nic.md)
> - [設定網路介面的訂單](net-sub-interface-metric.md)
> - [效能調整網路介面卡](net-sub-performance-tuning-nics.md)
> - [網路相關的效能計數器](net-sub-performance-counters.md)
> - [效能網路工作負載的工具](net-sub-performance-tools.md)

調整網路子系統，尤其是網路大量工作負載的效能可能需要的網路架構，也稱為網路堆疊的每個層級。 這些層級廣泛分為下列各節。

1. **網路介面**。 這是在網路堆疊的最低層級，並包含直接進行通訊的網路介面卡的網路驅動程式。

2. **網路驅動程式介面規格 (NDIS)**。 NDIS 公開進行下方的驅動程式，以及適用於上述，例如通訊協定堆疊層級。
  
3. **通訊協定堆疊**。 通訊協定堆疊實作 TCP/IP 和 UDP 日 IP 通訊協定。 這些層級公開它們上層傳輸層介面。
  
4. **系統驅動程式**。 這些都是通常戶端公開介面使用者模式應用程式使用的資料延伸模組傳輸 (TDX) 或介面 Winsock 核心 (WSK)。 在 Windows Server 2008 和 Windows 推出 WSK 介面&reg;AFD.sys 公開 Vista，而且它。 Interface 改善效能排除核心模式使用者模式之間切換。
  
5. **使用者模式應用程式**。 這通常是由 Microsoft 或自訂應用程式。

下表會提供垂直圖網路堆疊，包括的項目執行每個層級範例層級。  

![網路堆疊層](../../media/Network-Subsystem/network-layers.jpg)

