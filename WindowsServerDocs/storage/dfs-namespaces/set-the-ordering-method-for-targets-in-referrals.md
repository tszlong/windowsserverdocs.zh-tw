---
title: 設定轉介中目標的排序方法
description: 本文說明如何設定轉介中目標的排序方法。
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9b420e311c98477d369c81f10eca274e665dae3a
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475155"
---
# <a name="set-the-ordering-method-for-targets-in-referrals"></a>設定轉介中目標的排序方法

> 適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

轉介是排序的目標清單，當使用者存取命名空間根目錄或包含目標的資料夾時，用戶端電腦會從網域控制站或命名空間伺服器收到轉介。 當用戶端接收轉介之後，用戶端會嘗試存取清單中的第一個目標。 如果目標無法使用，用戶端會嘗試存取下一個目標。
用戶端站台上的目標一定會在轉介中優先列出。 用戶端站台外的目標則根據排序方法列出。

請使用下列章節來指定將目標轉介給用戶端的順序，以及了解排序目標轉介的不同方法：

## <a name="to-set-the-ordering-method-for-targets-in-namespace-root-referrals"></a>若要設定命名空間根目錄轉介中目標的排序方法

使用下列程序來設定命名空間根目錄的排序方法：

1.  按一下 [**開始**]，然後指向 [**系統管理工具**]，再按一下 [**DFS 管理**]。

2.  在主控台樹狀目錄的 [命名空間]**** 節點下，以滑鼠右鍵按一下命名空間，然後按一下 [內容]****。

3.  在 **\[轉介\]** 索引標籤上，選取排序方法。

> [!NOTE]
> 若要使用 Windows PowerShell 來設定命名空間根目錄轉介中的目標排序方法，請使用 [Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx) Cmdlet 並搭配下列其中一個參數：
>    -   **EnableSiteCosting** 指定**最低成本排序**方法
>    -   **EnableInsiteReferrals** 指定**排除用戶端站台外的目標**排序方法。
>    -   省略任一參數則指定**隨機順序**轉介排序方法。

DFSN Windows PowerShell 模組於 Windows Server 2012 中引進。

## <a name="to-set-the-ordering-method-for-targets-in-folder-referrals"></a>若要設定資料夾轉介中目標的排序方法

含目標的資料夾會繼承命名空間根目錄的排序方法。 您可以使用下列程序來覆寫排序方法：

1.  按一下 [**開始**]，然後指向 [**系統管理工具**]，再按一下 [**DFS 管理**]。

2.  在主控台樹狀目錄的 [命名空間]**** 節點下，以滑鼠右鍵按一下含目標的資料夾，然後按一下 [內容]****。

3.  在 **\[轉介\]** 索引標籤上，選取 **\[排除用戶端站台外的目標\]** 核取方塊。

> [!NOTE]
> 若要使用 Windows PowerShell 來排除用戶端站台外的資料夾目標，請使用 [Set-DfsnFolder –EnableInsiteReferrals](https://technet.microsoft.com/library/jj884283.aspx) Cmdlet。

## <a name="target-referral-ordering-methods"></a>目標轉介排序方法

三個排序方法如下：

-   隨機
-   最低成本
-   排除用戶端站台外的目標

## <a name="random-order"></a>隨機

在這種方法中，目標的排序如下所示：

1.  與用戶端相同 Active Directory Directory Services (AD DS) 站台中的目標，依隨機順序列在轉介頂端。
2.  用戶端站台外的目標隨機列出。

如果沒有相同站台的目標伺服器可供使用，用戶端電腦會被轉介至隨機的目標伺服器，不考慮連線多昂貴或目標的距離多遠。

## <a name="lowest-cost"></a>最低成本

在這種方法中，目標的排序如下所示：

1.  位於相同站台的目標，依隨機順序列在轉介頂端。
2.  用戶端站台外的目標則依最低成本到最高成本列出。 具有相同成本的轉介分在同一組，每個群組中隨機列出目標。

> [!NOTE]
> DFS 管理嵌入式管理單元中不會顯示站台連結成本。 若要檢視站台連結成本，請使用 Active Directory 站台及服務嵌入式管理單。

## <a name="exclude-targets-outside-of-the-clients-site"></a>排除用戶端站台外的目標

在這種方法中，轉介僅包含位於與用戶端相同站台中的目標。 這些相同站台目標隨機列出。 如果不存在相同站台目標，用戶端不會收到轉介，也無法存取該部分的命名空間。

> [!NOTE]
> 即使排序方法為 **\[排除用戶端站台外的目標\]**，目標優先順序設定為 \[所有目標中的第一個目標\] 或 \[所有目標中的最後一個目標\] 的目標仍會列在轉介中。

## <a name="additional-references"></a>其他參考

-   [調整 DFS 命名空間](tuning-dfs-namespaces.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)