---
title: 變更用戶端快取轉介的時間量
description: 本文說明如何變更用戶端快取轉介的時間量
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d421a14c2a6021d45cd16f30c526ff1670ae62e3
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475195"
---
# <a name="change-the-amount-of-time-that-clients-cache-referrals"></a>變更用戶端快取轉介的時間量

> 適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

轉介是排序的目標清單，當使用者存取命名空間根目錄或命名空間中包含目標的資料夾時，用戶端電腦會從網域控制站或命名空間伺服器收到轉介。 您可以調整用戶端快取轉介多久之後可要求新的轉介。

## <a name="to-change-the-amount-of-time-that-clients-cache-namespace-root-referrals"></a>變更用戶端快取命名空間根目錄轉介的時間量

1.  按一下 [**開始**]，然後指向 [**系統管理工具**]，再按一下 [**DFS 管理**]。

2.  在主控台樹狀目錄的 [命名空間]**** 節點下，以滑鼠右鍵按一下命名空間，然後按一下 [內容]****。

3.  在 [轉介]**** 索引標籤的 [快取期間 (以秒為單位)]**** 文字方塊上，輸入用戶端快取命名空間根目錄轉介的時間量 (以秒為單位)。 預設設定是 300 秒 (5 分鐘)。

> [!TIP]
> 若要使用 Windows PowerShell 變更用戶端快取命名空間根目錄轉介的時間量，請使用 [Set-DfsnRoot TimeToLiveSec](https://technet.microsoft.com/library/jj884281.aspx) Cmdlet。 這些 Cmdlet 於 Windows Server 2012 中引進。

## <a name="to-change-the-amount-of-time-that-clients-cache-folder-referrals"></a>變更用戶端快取資料夾轉介的時間量

1.  按一下 **\[開始\]**，指向 **\[系統管理工具\]**，然後按一下 **\[DFS 管理\]**。

2.  在主控台樹狀目錄的 [命名空間]**** 節點下，以滑鼠右鍵按一下具有目標的資料夾，然後按一下 [內容]****。

3.  在 [轉介]**** 索引標籤的 [快取期間 (以秒為單位)]**** 文字方塊上，輸入用戶端快取資料夾轉介的時間量 (以秒為單位)。 預設設定為 1800 秒 (30 分鐘)。

## <a name="additional-references"></a>其他參考

-   [調整 DFS 命名空間](tuning-dfs-namespaces.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)


