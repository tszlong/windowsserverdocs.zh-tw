---
title: 在 DFS 命名空間中建立資料夾
description: 本文說明如何在 DFS 命名空間中建立資料夾
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 47bb13aa404cdf4fef86b7250425a92cc208ba9d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856849"
---
# <a name="create-a-folder-in-a-dfs-namespace"></a>在 DFS 命名空間中建立資料夾

> 適用於：Windows Server 2019，Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

您可以使用資料夾，在命名空間中建立額外的階層層級。 您也可以建立具有資料夾目標的資料夾，以新增共用資料夾到命名空間。 具有資料夾目標的 DFS 資料夾不能包含其他 DFS 資料夾，所以如果您想要新增階層層級到命名空間，請勿新增資料夾目標至資料夾。

使用下列程序，在使用 DFS 管理的命名空間中建立資料夾：

## <a name="to-create-a-folder-in-a-dfs-namespace"></a>若要在 DFS 命名空間中建立資料夾

1.  按一下 [開始]，指向 [系統管理工具]，然後按一下 [DFS 管理]。

2.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，於命名空間或命名空間中的資料夾上按一下滑鼠右鍵，再按一下 **\[新增資料夾\]**。

3.  在 **\[名稱\]** 文字方塊中，輸入新資料夾的名稱。

4.  若要新增一或多個資料夾目標至資料夾，請按一下 **\[新增\]** 並指定資料夾目標的通用命名慣例 (UNC) 路徑，然後按一下 **\[確定\]**。


> [!TIP]
> 若要使用 Windows PowerShell 在命名空間中建立資料夾，請使用 [New-DfsnFolder](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfolder) Cmdlet。 DFSN Windows PowerShell 模組於 Windows Server 2012 中引進。


## <a name="see-also"></a>另請參閱

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [委派管理 DFS 命名空間的權限](delegate-management-permissions-for-dfs-namespaces.md)


