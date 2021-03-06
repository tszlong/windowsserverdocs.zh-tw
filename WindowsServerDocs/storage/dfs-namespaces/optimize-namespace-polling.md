---
title: 最佳化命名空間輪詢
description: 本文說明如何最佳化命名空間輪詢，以在命名空間伺服器間維持一致的網域型命名空間
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: cc14dd4f8d6cd833642b87caa32353d4f8940b05
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936133"
---
# <a name="optimize-namespace-polling"></a>最佳化命名空間輪詢

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

若要在命名空間伺服器間維護一致的網域型命名空間，命名空間伺服器必須定期輪詢 Active Directory 網域服務 (AD DS) 以取得最新的命名空間資料。

## <a name="to-optimize-namespace-polling"></a>最佳化命名空間輪詢

使用下列程序來最佳化命名空間輪詢的發生方式：

1.  按一下 [**開始**]，然後指向 [**系統管理工具**]，再按一下 [**DFS 管理**]。

2.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，於網域型命名空間上按一下滑鼠右鍵，再按一下 **\[內容\]**。

3.  在 **\[進階\]** 索引標籤上，選取您要最佳化命名空間的一致性或延展性。

    -   如果裝載命名空間的伺服器有 16 個或更少，請選擇 **\[最佳化一致性\]**。
    -   如果有超過 16 個命名空間伺服器，則選擇 **\[最佳化延展性\]**。 這樣雖然會減少網域主控站 (PDC) 模擬器的負載，不過卻會增加將命名空間的變更複寫到所有命名空間伺服器的時間。 將變更複寫到所有伺服器之前，使用者可能會看到不一致的命名空間。

> [!NOTE]
> 若要使用 Windows PowerShell 來設定命名空間輪詢模式，請使用 [Set-DfsnRoot EnableRootScalability](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771913(v=ws.11)) Cmdlet，此功能已在 Windows Server 2012 中推出。

## <a name="additional-references"></a>其他參考資料

-   [調整 DFS 命名空間](tuning-dfs-namespaces.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)
