---
title: 調整 DFS 命名空間
description: 本文說明如何調整或最佳化 DFS 命名空間
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c11bbf65c3baebebe1e5143a5e694ca752500aca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844629"
---
# <a name="tuning-dfs-namespaces"></a>調整 DFS 命名空間

> 適用於：Windows Server 2019，Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

建立命名空間，並新增資料夾與目標之後，請參閱下列各節來微調或最佳化 DFS 命名空間處理轉介與輪詢方式更新命名空間資料的 Active Directory 網域服務 (AD DS):

-   [啟用存取型列舉型別命名空間](enable-access-based-enumeration-on-a-namespace.md)
-   [啟用或停用轉介和用戶端容錯回復](enable-or-disable-referrals-and-client-failback.md)
-   [變更用戶端快取轉介的時間長度](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [設定轉介中的目標的排序方法](set-the-ordering-method-for-targets-in-referrals.md)
-   [設定目標優先順序以覆寫轉介順序](set-target-priority-to-override-referral-ordering.md)
-   [最佳化命名空間輪詢](optimize-namespace-polling.md)
-   [使用存取型列舉中的繼承的權限](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> 若要搜尋資料夾或資料夾目標，請選取命名空間、按一下 **\[搜尋\]** 索引標籤，在文字方塊中輸入您的搜尋字串，再按 **\[搜尋\]**。