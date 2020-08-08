---
title: Windows Admin Center SDK 案例研究-Fujitsu
description: Windows Admin Center SDK 案例研究-Fujitsu
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.openlocfilehash: 15f0120fdf1792ad127a9f5fa0e045ef7df18e85
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958746"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Fujitsu ServerView 健全狀況和 RAID 擴充功能

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>將端對端可見度（從作業系統到硬體）帶入 Windows 管理中心

Fujitsu 是領先的日文資訊和通訊技術公司，以及[PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/)和[PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/)伺服器產品的製造商。 [Fujitsu ServerView 管理套件](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/)提供了一套完整的伺服器生命週期管理工具組，包括提供 CIM 和 PowerShell 介面來進行硬體管理的伺服器端代理程式。

Fujitsu 看到一個機會，可以輕鬆地與 Windows 管理中心整合，因為它提供的 CIM 和 PowerShell 介面可以與伺服器端代理程式通訊。 Fujitsu 的開發小組能夠輕鬆地將熟悉的 CIM 呼叫提供給代理程式，並使用可用的 UI 元件將 Windows 系統管理中心內的資訊視覺化。

![Fujitsu 延伸模組-健全狀況樹狀檢視](../../media/extend-case-study-fujitsu/health-tree.png)

一旦小組熟悉 Windows Admin Center SDK，新增 UI 以公開額外的硬體資訊，通常只是幾行 HTML 程式碼，而且很快就能從單一工具擴充，以顯示硬體元件健全狀況的摘要、系統事件記錄檔的詳細資料、驅動程式監視器、處理器的個別視圖、記憶體、風扇、電源供應器、溫度和電壓，甚至還有額外的工具可進行 RAID 管理。 使用 SDK 中提供的 UI 控制項，例如 [樹狀結構]、[方格] 和 [詳細資料] 窗格控制項，可讓小組快速建立 UI，也能達成與 Windows 系統管理中心其他內容非常類似的視覺效果和互動設計。

![Fujitsu 擴充功能-RAID 樹狀檢視](../../media/extend-case-study-fujitsu/raid-tree.png)

![Fujitsu 擴充功能-RAID 磁片區視圖](../../media/extend-case-study-fujitsu/raid-volumes.png)

Fujitsu 與 Windows 管理中心團隊之間的合作，清楚地顯示 Windows 系統管理中心內的整合價值，讓客戶能夠對伺服器角色和服務、作業系統和硬體管理提供端對端的深入解析。