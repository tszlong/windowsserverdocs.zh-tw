---
title: "調整 DFS 命名空間"
description: "本文說明如何調整或最佳化 DFS 命名空間"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4614441fc54913ba5a8b547bbf1ad3e8ce7ee69b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="tuning-dfs-namespaces"></a>調整 DFS 命名空間

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

建立命名空間以及新增資料夾和目標後，請參考下列章節來調整或最佳化 DFS 命名空間為更新的命名空間資料處理轉介和輪詢 Active Directory Domain Services (AD DS) 的方式：

-   [在命名空間上啟用存取型列舉](enable-access-based-enumeration-on-a-namespace.md)
-   [啟用或停用轉介和用戶端容錯回復](enable-or-disable-referrals-and-client-failback.md)
-   [變更用戶端快取轉介的時間量](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [設定轉介中目標的排序方法](set-the-ordering-method-for-targets-in-referrals.md)
-   [設定目標優先順序以覆寫轉介順序](set-target-priority-to-override-referral-ordering.md)
-   [最佳化命名空間輪詢](optimize-namespace-polling.md)
-   [使用繼承的權限搭配存取型列舉](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> 若要搜尋資料夾或資料夾目標，請選取命名空間、按一下 **\[搜尋\]** 索引標籤，在文字方塊中輸入您的搜尋字串，再按 **\[搜尋\]**。