---
title: Windows Admin Center SDK 案例研究-平方向上
description: Windows Admin Center SDK 案例研究-平方向上
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 620d3d9f4b5c3638d49fe9141e83ebdcb9eb245c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406924"
---
# <a name="squared-up-extension"></a>方框向上擴充

## <a name="bringing-scom-based-monitoring-server-dependency-visibility-and-external-data-insights-into-windows-admin-center"></a>將以 SCOM 為基礎的監視、伺服器相依性可見度和外部資料見解帶入 Windows 管理中心

方形 Up 的目標是使用資料視覺效果來協助解決企業 IT 複雜度的挑戰。 方形 Up 的獨特、輕量、僅限 UI 的軟體建置於 Microsoft 強大的 System Center Operations Manager 平臺上，以及與其他資料來源整合-從 Microsoft 自己的 Azure Log Analytics、Application Insights 和 SystemService Manager 協力廠商產品（例如 ServiceNow、Splunk 等），以提供大規模企業基礎結構和應用程式資產的可見度，包括內部部署和跨混合式雲端環境。

> <cite>「我們一直在其 Technical Preview 中使用 Windows 系統管理中心，而且已經很大的衝擊，真正説明我們的工程師輕鬆存取我們的設定實驗室，而我們想要讓它成為我們的主要管理主控台（當它達到完整版本時）。我們喜歡與方形整合的可能性，以及在單一位置呈現所有資料的能力。」</cite>
>
> --David Acevedo，NuStar 能源 L.P. 的 I/S 專家

方形 Up 的用戶端管理數百部 Windows 伺服器和其所提供的多樣化應用程式組合，而這兩種方式都是讓 IT 團隊成為最快速、現代化的 web UI，以提供所需的見解。 因此，最新的小組會立即看到與 Windows 管理中心的令人興奮的一致，這會將這些相同的值和主體帶入下一代的 Windows Server 管理。 特別是，小組認為長期的效能資料、即時伺服器相依性深入解析，以及由方框所呈現的應用程式內容，都能完美地補充所提供的精緻、即時資料和伺服器管理功能。Windows 系統管理中心。

![方框向上擴充](../../media/extend-case-study-squared-up/squared-up-1.png)

> <cite>「身為管理大規模伺服器資產的組織，方形 Up/Windows 管理中心整合是我們的當地語系化和集中式工具的最佳結婚，也就是能夠從內部將伺服器直接進入維護模式的專案。Windows 系統管理中心對我們來說是一大小勝利，</cite>
>
> -–略過 k Granson，Purdue 大學的虛擬化系統管理員

有了清楚的願景，想要在 Windows 系統管理中心內順暢呈現該資料，接著與 Windows 系統管理中心 SDK 的初期私人預覽版本搭配使用，併發現它非常簡單，而且有彈性。

使用 Windows 系統管理中心 SDK，可以建立一個延伸模組，以動態方式在 Windows 管理中心體驗中內嵌相關的方形 Up 視圖。 例如，在特定伺服器或叢集的內容中，會自動內嵌方形 Up views 以提供延伸的可見度。 Views 包含關鍵效能和容量計量（例如 CPU、記憶體和磁片）、裝載堆疊（雲端平臺或資料中心虛擬化）、應用程式元件（例如 SQL 資料庫和服務，甚至是雲端式記錄分析）的歷程記錄趨勢和 ITSM 資料。

![方框向上擴充](../../media/extend-case-study-squared-up/squared-up-2.png)

方形 Up 和 Windows 管理中心共用現代化的 web 架構和設計 ethos，已同時啟用了簡單的技術整合和順暢的使用者體驗。 隨著以 web 為基礎的系統管理逐漸變得更好，我們相信在不同的系統之間整合這種方法，就是將現代化、統一的管理經驗解除鎖定的關鍵。

> <cite>「我們看到 Windows 系統管理中心是現代化 Windows Server 管理的最新功能，因此與小組密切合作，這是一項很棒的經驗，也就是使用這種速度、熱忱、彈性，並在根本上現代化的開發架構讓它們成為一種絕佳的方式，讓我們身為精簡、敏捷、快速的軟體發展公司、自己工作。」</cite>
>
> --Richard Benwell，以平方為基礎的產品架構設計人員

從這個自然的對齊方式，平方初的開發小組能夠快速地進行原型整合，在 Windows 管理中心體驗中以原生方式顯示方形，並將其交給自己的早期採納者技術預覽用戶端。 從客戶的反應來看，這是一種很明顯的案例。

> <cite>「在超過3500伺服器的環境中維護卓越服務的其中一項重要挑戰，就是將各種不同的管理和監視工具整合在一起，因此在方形 Up 與 Windows 管理中心之間進行整合-這會帶來將大量資料從這麼多不同的來源整合到單一主控台，對我們來說是非常龐大的。」</cite>
>
> --聖馬丁 Ehrnst，Azure 的技術組長 Intility A/S

由於這種熱忱是從方形 Up 用戶端開始，而在 Windows 系統管理中心仍然有大量的絕佳新功能，因此在這項整合的未來，會大幅興奮，而其用戶端和為 IT 營運管理的真正單一窗格提供旅程。

方形 Up/Windows 管理中心整合目前為搶鮮版（Beta）;如果您想要存取，請查看[方形的專用頁面](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-wac&utm_medium=public-relations&utm_campaign=honolulu)，以取得更多詳細資料。 如果您的組織使用 Microsoft System Center Operations Manager，但您還沒有啟動（這是延伸模組運作的必要功能），則您也可以從相同的位置取得全功能的30天免費試用版。 
