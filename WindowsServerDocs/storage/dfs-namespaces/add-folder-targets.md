---
title: Add Folder Targets
description: 本文說明如何新增資料夾目標 (UNC 路徑)
ms.prod: windows-server
ms.author: jgerend
manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms-date: 06/05/2017
ms.openlocfilehash: d2f3845a612556a51692aaf51d256bbedd518e7a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854101"
---
# <a name="add-folder-targets"></a>Add folder targets

> 適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

資料夾目標為共用資料夾或與命名空間資料夾相關之命名空間的通用命名慣例 (UNC) 路徑。 新增多個資料夾目標可增加命名空間資料夾的可用性。

## <a name="to-add-a-folder-target"></a>新增資料夾目標

若要使用 DFS 管理來新增伺服器目標，請使用下列程序：

1.  按一下 [**開始**]，然後指向 [**系統管理工具**]，再按一下 [**DFS 管理**]。

2.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，以滑鼠右鍵按一下資料夾，然後按一下 **\[新增伺服器目標\]** 。

3.  輸入路徑資料夾的目標，或按一下 **\[瀏覽\]** 來尋找資料夾目標。

4.  如果資料夾是使用 DFS 複寫來複寫的，您可以指定是否將新資料夾目標新增至複寫群組。

> [!TIP]
> 若要使用 Windows PowerShell 新增資料夾目標，請使用 [New-DfsnFolderTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfoldertarget) Cmdlet。 DFSN Windows PowerShell 模組於 Windows Server 2012 中引進。

> [!NOTE]
> 資料夾可以包含資料夾目標或其他 DFS 資料夾，但在資料夾階層的相同層級中不得同時包含兩者。

## <a name="see-also"></a>另請參閱

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)
-   [使用 DFS 複寫複寫資料夾目標](replicate-folder-targets-using-dfs-replication.md)