---
title: 調整 DFS 命名空間
description: 本文說明如何調整或最佳化 DFS 命名空間
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 348a34e24cf7d22dc376df37607f21f1dceea74a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939394"
---
# <a name="tuning-dfs-namespaces"></a>調整 DFS 命名空間

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

建立命名空間及新增資料夾與目標之後，請參閱以下幾節，對已更新命名空間資料的 DFS 命名空間處理轉介與輪詢 Active Directory 網域服務 (AD DS) 方式進行調整或最佳化：

-   [在命名空間上啟用以存取為基礎的列舉](enable-access-based-enumeration-on-a-namespace.md)
-   [啟用或停用轉介和用戶端容錯回復](enable-or-disable-referrals-and-client-failback.md)
-   [變更用戶端快取參考的時間長度](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [設定轉介中目標的排序方法](set-the-ordering-method-for-targets-in-referrals.md)
-   [設定目標優先順序以覆寫轉介順序](set-target-priority-to-override-referral-ordering.md)
-   [最佳化命名空間輪詢](optimize-namespace-polling.md)
-   [使用繼承的許可權搭配以存取為基礎的列舉](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> 若要搜尋資料夾或資料夾目標，請選取命名空間、按一下 **\[搜尋\]** 索引標籤，在文字方塊中輸入您的搜尋字串，再按 **\[搜尋\]**。