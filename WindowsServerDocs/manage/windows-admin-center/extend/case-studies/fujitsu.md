---
title: Windows Admin Center SDK 案例研究-Fujitsu
description: Windows Admin Center SDK 案例研究-Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814989"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Fujitsu ServerView 健全狀況和 RAID 延伸模組

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>從硬體、 作業系統中帶入 Windows Admin Center 的端對端可見性，

Fujitsu 是前置日文資訊和通訊技術公司和製造商[PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/)並[PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/)伺服器產品。 [Fujitsu ServerView management suite](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/)為伺服器提供完整的工具組包括 CIM 和 PowerShell 介面提供硬體管理的伺服器端代理程式的生命週期管理。

Fujitsu 看見了商機，它提供伺服器端代理程式進行通訊的 CIM 和 PowerShell 介面，輕鬆整合 Windows Admin Center 使用。 開發小組在 Fujitsu 是能夠輕鬆地實作 CIM 呼叫他們所熟悉的代理程式，並以視覺化方式檢視 使用可用的 UI 元件的 Windows Admin Center 內的資訊。

![Fujitsu 擴充功能-健全狀況樹狀檢視](../../media/extend-case-study-fujitsu/health-tree.png)

小組會慢慢熟悉 Windows Admin Center SDK，一旦新增 UI 以公開額外的硬體資訊通常只需幾行的 HTML 程式碼並他們能夠快速地從單一工具展開以顯示的硬體元件的摘要檢視健全狀況，系統事件記錄檔，驅動程式監視的詳細檢視，分隔檢視處理器、 記憶體、 風扇、 電源供應器、 溫度和電壓和甚至其他 RAID 管理工具。 格線和詳細資料窗格控制項使用的 SDK，例如樹狀結構中可用的 UI 控制項，啟用小組合作，快速建置 UI，並達到 視覺效果與互動設計，非常類似於 Windows Admin Center 的其餘部分。

![Fujitsu 擴充功能-RAID 樹狀檢視](../../media/extend-case-study-fujitsu/raid-tree.png)

![Fujitsu 擴充功能-RAID 磁碟區檢視](../../media/extend-case-study-fujitsu/raid-volumes.png)

Fujitsu 與 Windows Admin Center 小組之間的合作關係會清楚顯示整合的值內 Windows Admin Center，讓客戶以有端對端了解伺服器角色和作業系統，以及硬體管理的服務.