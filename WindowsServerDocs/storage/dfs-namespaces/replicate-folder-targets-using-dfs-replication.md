---
title: 使用 DFS 複寫來複寫資料夾目標
description: 本文說明如何用 DFS 複寫來複寫資料夾目標
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ad26b8685539869e9302eafac69df19d11778a1b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386129"
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>使用 DFS 複寫來複寫資料夾目標

> 適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

您可以使用 DFS 複寫來保持資料夾目標的內容同步，以便無論用戶端電腦轉介至哪個資料夾目標，使用者都能看到相同的檔案。

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>若要使用 DFS 複寫來複寫資料夾目標

1.  按一下 [開始]，指向 [系統管理工具]，然後按一下 [DFS 管理]。

2.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，以滑鼠右鍵按一下具有兩個以上資料夾目標的資料夾，然後按一下 **\[複寫資料夾\]** 。

3.  依照 \[複寫資料夾精靈\] 中的指示執行。

> [!NOTE]
> 除非使用 [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) 和 [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup) Cmdlet，否則變更設定不會立即套用所有成員。 新的組態必須複寫到所有網域控制站，而且複寫群組中的每個成員必須輪詢其最接近的網域控制站以取得變更。 所需的時間量取決於 Active Directory 目錄服務（AD DS）複寫延遲和每個成員的長輪詢間隔（60分鐘）。 若要針對組態變更立即輪詢，請開啟命令提示字元視窗，然後針對每個複寫群組成員輸入一次下列命令： <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
若要從 Windows PowerShell 會話執行這項操作，請使用 Windows Server 2012 R2 中引進的[DfsrConfigurationFromAD 指令程式](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad)。

## <a name="see-also"></a>另請參閱

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)
-   [DFS 複寫](../dfs-replication/dfsr-overview.md)