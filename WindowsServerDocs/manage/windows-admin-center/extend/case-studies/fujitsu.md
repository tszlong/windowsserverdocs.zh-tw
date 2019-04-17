---
title: Windows 系統管理中心 sdk （英文） 案例研究-Fujitsu
description: Windows 系統管理中心 sdk （英文） 案例研究-Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "2052372"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Fujitsu ServerView 狀況與 RAID 延伸模組

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>將端對端可見性、 硬體、 作業系統從導入 Windows 系統管理中心

Fujitsu 是前置日文資訊及通訊技術公司和製造商[PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/)和[PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/)伺服器產品。 [Fujitsu ServerView 管理套件](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/)提供全方位工具組伺服器的生命週期管理包括硬體管理提供 CIM 和 PowerShell 介面的伺服器端代理程式。

Fujitsu 正如 CIM 和 PowerShell 無法進行通訊的介面隨附的伺服器端代理程式輕鬆地整合 Windows 系統管理中心有機會。 開發小組在 Fujitsu 已能夠輕易地實作交易熟悉專員 CIM 通話和視覺化使用可用的使用者介面元件的 Windows 系統管理中心內的資訊。

![Fujitsu 副檔名-狀況樹狀檢視](../../media/extend-case-study-fujitsu/health-tree.png)

一旦小組變成熟悉 Windows Admin Center SDK、 新增公開額外的硬體資訊的使用者介面已經通常只需幾個 HTML 程式碼的更多行並要求過快速能夠從單一工具擴充到顯示硬體元件的摘要檢視健康情況、 系統事件記錄檔，驅動程式監視詳細檢視分隔處理器、 記憶體、 扇子、 電源供應器、 溫度和伏特數、 檢視和即使 RAID 管理的其他工具。 使用 SDK 例如樹狀目錄中的使用者介面控制項，啟用快速建立 UI 及也達到非常類似於在其餘的 Windows 系統管理中心視覺和互動設計小組格線和詳細資料窗格的控制項。

![Fujitsu 副檔名-RAID 樹狀檢視](../../media/extend-case-study-fujitsu/raid-tree.png)

![Fujitsu 副檔名-RAID 磁碟區檢視](../../media/extend-case-study-fujitsu/raid-volumes.png)

Fujitsu 與 Windows 系統管理中心小組合作關係清楚顯示值整合的 Windows 系統管理中心，內啟用客戶有端對端深入了解伺服器角色和服務、 作業系統，以及硬體管理.