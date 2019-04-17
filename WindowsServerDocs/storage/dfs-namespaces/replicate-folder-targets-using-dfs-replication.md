---
title: "使用 DFS 複寫來複寫資料夾目標"
description: "本文說明如何用 DFS 複寫來複寫資料夾目標"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ef13c338d8b13c24a02efb0468f06d5fef803cea
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>使用 DFS 複寫來複寫資料夾目標

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

您可以使用 DFS 複寫來保持資料夾目標的內容同步，以便無論用戶端電腦轉介至哪個資料夾目標，使用者都能看到相同的檔案。

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>若要使用 DFS 複寫來複寫資料夾目標

1.  按一下 **\[開始\]**，指向 **\[系統管理工具\]**，然後按一下 **\[DFS 管理\]**。

2.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，以滑鼠右鍵按一下具有兩個以上資料夾目標的資料夾，然後按一下 **\[複寫資料夾\]**。

3.  依照 \[複寫資料夾精靈\] 中的指示執行。

> [!NOTE]
> 除非使用 [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) 和 [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup) Cmdlet，否則變更設定不會立即套用所有成員。 新的組態必須複寫到所有網域控制站，而且複寫群組中的每個成員必須輪詢其最接近的網域控制站以取得變更。 這個動作所花的時間取決於 Active Directory Directory Services (AD DS) 的複寫延遲和每個成員的長輪詢間隔 (60 分鐘)。 若要針對組態變更立即輪詢，請開啟命令提示字元視窗，然後針對每個複寫群組成員輸入一次下列命令： <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
若要從 Windows PowerShell 工作階段這樣做，請使用 [Update-DfsrConfigurationFromAD](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad) Cmdlet，此功能已在 Windows Server 2012 R2 推出。

## <a name="see-also"></a>請參閱

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)
-   [複寫](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)