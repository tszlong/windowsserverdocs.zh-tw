---
title: 設定目標優先順序以覆寫轉介順序
description: 本文說明如何設定目標優先順序以覆寫轉介順序
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 77fe5b82b73a0f37ba81dda210f15d6017788822
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966820"
---
# <a name="set-target-priority-to-override-referral-ordering"></a>設定目標優先順序以覆寫轉介順序

> 適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

轉介是排序的目標清單，當使用者存取命名空間根目錄或命名空間中包含目標的資料夾時，用戶端電腦會從網域控制站或命名空間伺服器收到轉介。 轉介中的每一個目標皆根據命名空間根目錄或資料夾的排序方法來排序。

若要修訂目標的排序方式，可在個別目標上設定優先順序。 例如，可將目標指定為所有目標中的第一個目標、所有目標中的最後一個目標，或所有相同成本之目標中的第一個目標 (或最後一個目標)。

## <a name="to-set-target-priority-on-a-root-target-for-a-domain-based-namespace"></a>針對網域型命名空間設定根目標的目標優先順序

若要針對網域型命名空間，設定根目標的目標優先順序，請使用下列程序：

1.  按一下 [**開始**]，然後指向 [**系統管理工具**]，再按一下 [**DFS 管理**]。

2.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，按一下要設定根目標優先順序的網域型命名空間。

3.  在 **\[詳細資料\]** 窗格的 **\[命名空間伺服器\]** 索引標籤上，以滑鼠右鍵按一下要變更優先順序的根目標，然後按一下 **\[內容\]**。

4.  在 **\[進階\]** 索引標籤上，按一下 **\[覆寫轉介順序\]**，然後按一下所需的優先順序。

    -   **\[所有目標中的第一個目標\]** 指定如果目標為可用時，永遠將使用者轉介到此目標。
    -   **\[所有目標中的最後一個目標\]** 指定只有在其他所有目標都無法使用時，才可將使用者轉介到此目標。
    -   **\[相同成本之目標中的第一個目標\]** 指定在目標成本相同時，優先將使用者轉介到此目標 (通常表示相同站台中的其他目標)。
    -   **\[相同成本之目標中的最後一個目標\]** 指定如果目標成本相同且其他目標可用時，永遠不要將使用者轉介到此目標 (通常表示相同站台中的其他目標)。

## <a name="to-set-target-priority-on-a-folder-target"></a>設定資料夾目標的目標優先順序

若要設定資料夾目標的目標優先順序，請使用下列程序：

1.  按一下 [**開始**]，然後指向 [**系統管理工具**]，再按一下 [**DFS 管理**]。

2.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，按一下要設定優先順序之目標的資料夾。

3.  在 **\[詳細資料\]** 窗格的 **\[資料夾目標\]** 索引標籤上，以滑鼠右鍵按一下要變更優先順序的資料夾目標，然後按一下 **\[內容\]**。

4.  在 **\[進階\]** 索引標籤上，按一下 **\[覆寫轉介順序\]**，然後按一下所需的優先順序。

> [!NOTE]
> 若要使用 Windows PowerShell 來設定目標優先順序，請使用 [Set-DfsnRootTarget](/powershell/module/dfsr/update-dfsrconfigurationfromad?view=win10-ps) 和 [Set-DfsnFolderTarget](/powershell/module/dfsr/update-dfsrconfigurationfromad?view=win10-ps) Cmdlet 與 **ReferralPriorityClass** 和 **ReferralPriorityRank** 參數。 這些 Cmdlet 於 Windows Server 2012 中引進。

## <a name="additional-references"></a>其他參考

-   [調整 DFS 命名空間](tuning-dfs-namespaces.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)
