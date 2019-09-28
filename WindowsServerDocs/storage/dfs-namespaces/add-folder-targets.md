---
title: 新增資料夾目標
description: 本文說明如何新增資料夾目標 (UNC 路徑)
ms.prod: windows-server
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms-date: 06/05/2017
ms.openlocfilehash: b0685ea795d53b36fad92d54f817f67de57e3a82
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403183"
---
# <a name="add-folder-targets"></a>新增資料夾目標

> 適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

資料夾目標是共用資料夾或其他與命名空間中資料夾相關聯之命名空間的通用命名慣例 (UNC) 路徑。 新增多個資料夾目標可提高命名空間中資料夾的可用性。

## <a name="to-add-a-folder-target"></a>若要新增伺服器目標

若要使用 DFS 管理來新增伺服器目標，請使用下列程序：

1.  按一下 [開始]，指向 [系統管理工具]，然後按一下 [DFS 管理]。

2.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，以滑鼠右鍵按一下資料夾，然後按一下 **\[新增伺服器目標\]** 。

3.  輸入路徑資料夾的目標，或按一下 **\[瀏覽\]** 來尋找資料夾目標。

4.  如果資料夾是使用 DFS 複寫來複寫的，您可以指定是否將新資料夾目標新增至複寫群組。

> [!TIP]
> 若要使用 Windows PowerShell 新增資料夾目標，請使用 [New-DfsnFolderTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfoldertarget) Cmdlet。 DFSN Windows PowerShell 模組於 Windows Server 2012 中引進。

> [!NOTE]
> 資料夾可以在同一個資料夾階層中包含資料夾目標或其他 DFS 資料夾，但不能同時包含兩者。

## <a name="see-also"></a>另請參閱

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)
-   [使用 DFS 複寫複寫資料夾目標](replicate-folder-targets-using-dfs-replication.md)