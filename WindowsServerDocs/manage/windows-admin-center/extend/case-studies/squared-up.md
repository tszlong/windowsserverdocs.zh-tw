---
title: Windows Admin Center SDK 案例研究-平方
description: Windows Admin Center SDK 案例研究-平方
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ab0a7bdcf2388ffc867763c04e183b7388fd13e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863939"
---
# <a name="squared-up-extension"></a>總平方的延伸模組

## <a name="bringing-scom-based-monitoring-server-dependency-visibility-and-external-data-insights-into-windows-admin-center"></a>將 SCOM 監視、 伺服器相依性的可見性和外部的資料深入解析到 Windows Admin Center

平方向上創立的使用資料視覺效果來協助您解決這些挑戰的企業 IT 複雜性的願景。 平方向上的唯一的輕量型、 僅限 UI 的軟體建置在 Microsoft 的強大 System Center Operations Manager 的平台，以及與其他資料來源-從 Microsoft 自己的 Azure Log Analytics、 Application Insights 和系統整合第三方產品，例如 ServiceNow、 Splunk 及更多-掌握大型的企業基礎結構和應用程式的組織，這兩個內部部署和混合式雲端環境之間 center Service Manager。

> <cite>」 我們已被大量使用整個其 Technical Preview 的 Windows Admin Center 已經過大點擊，真的幫助解決挑戰，例如取得我們設定的實驗室，讓您輕鬆存取我們的工程師和我們想要讓我們的主要管理一旦達到完整版的主控台。我們很樂意與平方增加的整合與能夠呈現我們在單一位置的所有資料的潛力。 」</cite>
>
> -David Acevedo 我 / NuStar 能源 L.P.S 專家

平方的用戶端管理數百個通常數以千計，Windows 伺服器和不同，而且平方向上及 Microsoft 所提供的組合會使 IT 小組的任務的應用程式的最佳的快速、 現代化、 web UI，以提供深入解析所需。 如此一來，平方組成的小組立即看到其帶入新一代的 Windows Server 管理 中的這些相同的值和主體的 Windows Admin Center 中令人興奮的配合。 特別是，小組認為，長期的效能資料、 即時的伺服器相依性的深入解析和呈現平方組成的應用程式內容會完美搭配的簡潔、 即時的資料和所提供的伺服器管理功能Windows Admin Center。

![總平方的延伸模組](../../media/extend-case-study-squared-up/squared-up-1.png)

> <cite>「 為組織管理大規模的伺服器資產，平方向上 / Windows Admin Center 整合是完美的結婚的當地語系化和集中式工具與像是要能夠從 throw 直接進入維護模式的伺服器Windows Admin Center 會為我們的絕佳小 wins 」</cite>
>
> --略 Granson、 Purdue 大學的虛擬化系統管理員

有了清楚的想要在 Windows Admin Center 內順暢地呈現該資料，平方從事早期私人預覽版本的 Windows Admin Center sdk 和簡單、 有詳細記載且彈性找到它。

使用 Windows Admin Center SDK，總平方是能夠建置相關平方增加 Windows Admin Center 內的檢視體驗會以動態方式內嵌的擴充功能。 比方說，在特定伺服器或叢集的內容中，總平方檢視會自動內嵌至提供擴充的可見性。 檢視包含歷史趨勢的關鍵效能和容量計量 （例如 CPU、 記憶體和磁碟)，裝載 stack （雲端平台或資料中心虛擬化），應用程式元件，例如 SQL 資料庫和服務，甚至是雲端架構的 log analytics與 ITSM 資料。

![總平方的延伸模組](../../media/extend-case-study-squared-up/squared-up-2.png)

平方] 和 [Windows Admin Center 共用現代的 web 架構和設計風氣，已啟用順暢的使用者體驗和簡單的技術整合。 使用以 web 為基礎的管理，越來越容易成為範，我們相信這個方法的不同系統之間的整合是解除鎖定提供現代、 統一的系統管理體驗的關鍵。

> <cite>「 我們看到 Windows Admin Center 為最新的現代化的 Windows Server 管理，因此已經過我們密切合作，因此小組與他們正在使用這類的速度、 熱忱、 彈性和內這類基本上事實的絕佳體驗新式開發架構使得它們非常適合的方式，精簡、 敏捷式軟體開發快速軟體開發公司，工作自行。 」</cite>
>
> -平方的 Richard Benwell，產品架構設計人員

從這個自然對齊，總平方的開發團隊無法快速進展到原生顯示平方設定內的 Windows Admin Center 體驗的原型整合，並把自己早期採納者，技術的手預覽用戶端。 從客戶的反應，顯然立即劇本是成功者。

> <cite>「 維護服務的超過 3,500 伺服器環境統一我們多樣化的重要挑戰之一改變的管理和監視工具，因此平方] 和 [Windows Admin Center-帶來之間的整合一起這麼多資料，從許多不同來源的詳細資訊，為單一主控台 – 會很可觀的。 」</cite>
>
> -Martin Ehrnst，適用於 Azure 在 Intility A/S 技術主管

這種方用戶端已經從熱忱與大量的絕佳的新功能仍與進入 Windows Admin Center，平方向上之所以已大幅期待未來的這項整合與絕佳的可能性，它會開啟其用戶端及其true 單一-窗格-的-玻璃杯 IT 作業管理其之旅。

平方向上 / Beta; 目前正在 Windows Admin Center 整合如果您想要存取，請造訪[平方 」 的專用的頁面](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-wac&utm_medium=public-relations&utm_campaign=honolulu)如需詳細資訊。 如果您的組織使用 Microsoft System Center Operations Manager，您還沒有平方向上 （這是不可或缺的擴充功能才能運作），然後您可以也享受功能完整、 30 天免費試用期間從相同的位置。 
