---
title: 在 DFS 命名空間中建立資料夾
description: 本文說明如何在 DFS 命名空間中建立資料夾
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d854cfcb02288f6262ee380edc0614baf9e29878
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957696"
---
# <a name="create-a-folder-in-a-dfs-namespace"></a>在 DFS 命名空間中建立資料夾

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

您可以使用資料夾，在命名空間中建立額外的階層層級。 您也可以建立具有資料夾目標的資料夾，以新增共用資料夾到命名空間。 具有資料夾目標的 DFS 資料夾不能包含其他 DFS 資料夾，所以如果您想要新增階層層級到命名空間，請勿新增資料夾目標至資料夾。

使用下列程序，在使用 DFS 管理的命名空間中建立資料夾：

## <a name="to-create-a-folder-in-a-dfs-namespace"></a>若要在 DFS 命名空間中建立資料夾

1.  按一下 [**開始**]，然後指向 [**系統管理工具**]，再按一下 [**DFS 管理**]。

2.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，於命名空間或命名空間中的資料夾上按一下滑鼠右鍵，再按一下 **\[新增資料夾\]**。

3.  在 **\[名稱\]** 文字方塊中，輸入新資料夾的名稱。

4.  若要新增一或多個資料夾目標至資料夾，請按一下 **\[新增\]** 並指定資料夾目標的通用命名慣例 (UNC) 路徑，然後按一下 **\[確定\]**。


> [!TIP]
> 若要使用 Windows PowerShell 在命名空間中建立資料夾，請使用 [New-DfsnFolder](/powershell/module/dfsn/new-dfsnfolder) Cmdlet。 DFSN Windows PowerShell 模組於 Windows Server 2012 中引進。


## <a name="additional-references"></a>其他參考資料

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)
