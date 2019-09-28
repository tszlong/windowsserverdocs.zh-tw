---
title: 調整 DFS 命名空間
description: 本文說明如何調整或最佳化 DFS 命名空間
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 011512deaeb99ded7d0bfc32a48f19ab3b622475
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386147"
---
# <a name="tuning-dfs-namespaces"></a>調整 DFS 命名空間

> 適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

建立命名空間並新增資料夾和目標之後，請參閱下列各節，以微調或優化 DFS 命名空間處理參考和輪詢的方式 Active Directory Domain Services （AD DS），以取得更新的命名空間資料：

-   [在命名空間上啟用以存取為基礎的列舉](enable-access-based-enumeration-on-a-namespace.md)
-   [啟用或停用轉介和用戶端容錯回復](enable-or-disable-referrals-and-client-failback.md)
-   [變更用戶端快取參考的時間長度](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [設定轉介中目標的排序方法](set-the-ordering-method-for-targets-in-referrals.md)
-   [設定目標優先順序以覆寫轉介順序](set-target-priority-to-override-referral-ordering.md)
-   [最佳化命名空間輪詢](optimize-namespace-polling.md)
-   [使用繼承的許可權搭配以存取為基礎的列舉](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> 若要搜尋資料夾或資料夾目標，請選取命名空間、按一下 **\[搜尋\]** 索引標籤，在文字方塊中輸入您的搜尋字串，再按 **\[搜尋\]** 。