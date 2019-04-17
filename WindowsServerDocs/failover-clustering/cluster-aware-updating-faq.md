---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: "叢集更新的常見問題集"
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
description: "有關 Windows 伺服器叢集更新的常見問題的解答。"
ms.openlocfilehash: 8417ea8b6b76e16c3f4db3bac5062d90a8da3ff2
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>叢集更新：常見問題集

> 適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

[叢集更新](cluster-aware-updating.md)\(CAU\) 是座標容錯移轉叢集不會影響服務的可用性任何多叢集節點計劃容錯移轉的方式在所有伺服器上的軟體更新的功能。 某些應用程式以持續可用的功能 \ (Hyper\ HYPER-V 的即時移轉，) 或 SMB 透明 Failover\ SMB 3.x 檔案伺服器，例如 CAU 可以調整叢集自動更新的服務的可用性不會影響。

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>並 CAU 支援更新的儲存空間直接存取叢集嗎？  
[是]。 CAU 支援更新[儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md)叢集無論部署類型：超匯集或匯集。 具體而言，CAU 協調流程確保的暫停每個叢集節點等待基礎叢集的儲存空間的健康狀態。

## <a name="does-cau-work-with-windows-server-2008-r2-or-windows-7"></a>「與 Windows Server 2008 R2 或 Windows 7 CAU 運作？  
否。 CAU 協調叢集更新只從執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10、Windows 8.1 或 Windows 8 電腦操作。 正在更新容錯移轉叢集必須執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>是叢集的特定應用程式 CAU 有限嗎？  
否。 CAU 是以叢集應用程式類型。 CAU 是外部 cluster\ 更新方案，這因為叢集 Api 和 PowerShell cmdlet 上方。 因此，CAU 就可以調整設定 Windows Server 容錯移轉叢集任何叢集應用程式的更新。  
  
> [!NOTE]  
> 目前，在下列叢集工作負載測試及的 CAU: SMB Hyper\ HYPER-V、DFS 複寫、DFS 命名空間，iSCSI，以及 NFS。  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>CAU 支援更新從 Microsoft Update 和 Windows 更新？  
[是]。 根據預設，CAU 設定與 plug\ 入叢集節點上使用 Windows Update 代理程式 \(WUA\) 公用程式的 Api。 您可以設定 WUA 基礎結構指向 Microsoft Update 和 Windows 更新或 Windows Server Update Services \(WSUS\) 做為來源的更新。  
  
## <a name="does-cau-support-wsus-updates"></a>CAU 不支援 WSUS 更新嗎？  
[是]。 根據預設，CAU 設定與 plug\ 入叢集節點上使用 Windows Update 代理程式 \(WUA\) 公用程式的 Api。 您可以設定 WUA 基礎結構指向 Microsoft Update 和 Windows 更新或 Windows Server Update Services \(WSUS\) 本機伺服器做為來源的更新。  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>CAU 套用有限的 distribution 發行的更新？  
[是]。 讓他們無法透過 Windows Update 代理程式 \(WUA\) plug\ 下載有限的 distribution 版本 \(LDR\) 更新也稱為 hotfix，不會透過 Microsoft 更新或 Windows Update 發行-，CAU 使用預設。  
  
不過，CAU 包含第二個 plug\-，您可以選取套用 hotfix 更新。 您也可以自訂 plug\ 在此 hotfix 套用 non\ 的 Microsoft 驅動程式、韌體和 BIOS 更新。  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>可以使用 CAU 套用累積更新嗎？  
[是]。 如果的累積更新一般 distribution 發行的更新或 LDR 更新，可套用 CAU 它們。  
  
## <a name="can-i-schedule-updates"></a>排程更新？  
[是]。 CAU 支援下列更新模式，這兩種允許排定的更新：  
  
**Self\ 更新**更新本身根據定義的設定檔和定期，例如每月維護期間叢集可讓。 您也可以開始 Self\ 更新依需要執行任何時間。 若要讓 self\ 更新模式，您必須叢集新增 CAU 叢集的角色。 CAU self\ 更新功能執行任何其他叢集工作負載，例如，並可以順暢地進行工作計劃與計畫錯誤後移轉更新協調器的電腦使用。  
  
**Remote\ 更新**可讓您的電腦執行的是 Windows 或 Windows Server 隨時開始更新執行。 更新執行透過叢集更新視窗或使用 [開始] **Invoke\-CauRun** PowerShell cmdlet。 Remote\ 更新是針對 CAU 更新模式預設值。 您可以使用工作排程器來執行[叫用-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) cmdlet 上您想要從遠端電腦不是其中一個節點叢集排程。  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>排程更新適用於在 [備份期間？  
[是]。 在這方面 CAU 不加上任何限制。 但是，在伺服器上執行的軟體更新 \（相關潛在 restarts\) 伺服器備份時進行不 IT 最好的作法。 請注意，CAU 只依賴叢集判斷資源錯誤後的移轉和 failbacks; Api 因此，CAU 是不知道的伺服器備份狀態。  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>CAU 能使用 System Center Configuration Manager 嗎？  
CAU 是座標叢集節點上的軟體更新的工具與 Configuration Manager 也會執行伺服器的軟體更新。 請務必設定這些工具，以便他們未重疊的涵蓋範圍的任何資料中心部署，包括使用不同的 Windows Server Update Services 伺服器在相同的伺服器。 這樣可確保使用 CAU 背後的目標不小心失敗，因為設定 Manager\ 導向更新不會納入叢集感知。  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>我需要執行 CAU 管理的認證嗎？  
[是]。 適用於執行這些 CAU 工具，CAU 需要管理認證本機伺服器，或需要**後驗證模擬 Client**上本機伺服器或 client 電腦所執行的使用者。 不過，協調叢集節點上的軟體更新，CAU 需要在每個節點叢集管理認證。 雖然不認證可以開始 CAU UI，它會提示用於叢集管理認證連接之後叢集執行個體預覽或套用更新。  
  
## <a name="can-i-script-cau"></a>可以指令碼 CAU 嗎？  
[是]。 CAU 隨附 PowerShell cmdlet 提供一組豐富的指令碼選項。 這些是 CAU UI 呼叫 CAU 執行相同 cmdlet。  

## <a name="what-happens-to-active-clustered-roles"></a>事情叢集作用中的角色？

叢集角色 \（先前稱為應用程式和 services\）節點上使用，無法透過其他節點之前，才能開始軟體更新。 CAU 使用維護模式，會暫停，以及清空叢集作用中的角色所有的節點協調這些錯誤後的移轉。 軟體更新完成時，CAU 繼續節點和叢集的角色失敗回更新節點。 這樣可確保的叢集角色節點和目的地的相對的散發保持不變 CAU 更新回合的叢集。  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>如何 CAU 選取目標節點叢集角色？

CAU 依賴叢集協調錯誤後的移轉 Api。 叢集 API 實作依賴內部計量和智慧位置 heuristics 選取目標節點 \（例如工作負載 levels\) 上的目標節點。  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>會 CAU 負載平衡叢集的角色嗎？

CAU 不負載的平衡叢集的節點，但它嘗試保留散發叢集角色。 當 CAU 完成更新叢集節點時，它會嘗試失敗返回先前裝載到該節點叢集的角色。 CAU 依賴叢集 Api 回復資源暫停程序的開頭。 因此在缺少意外的錯誤後移轉和慣用的擁有者的設定的叢集角色散發應維持不變。  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>如何 CAU 選取節點順序更新？  
根據預設，CAU 選取更新節點順序型活動的層級。 節點裝載少叢集的角色是第一次更新。 不過，系統管理員可以指定特定讓參數指定對更新執行 CAU UI 或使用 PowerShell cmdlet 更新節點。  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>萬一叢集節點是離線？

開始更新執行的系統管理員可以指定數目離線節點接受臨界值。 因此，更新執行可以繼續在叢集上即使叢集節點不 online。  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>可以使用 CAU 更新只有一個節點嗎？  
否。 CAU 是 cluster\ 範圍更新的工具，讓它只可讓您選取叢集更新。 如果您想要更新的單一節點，您可以使用現有更新 CAU 獨立工具的伺服器。  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>可以 CAU 報告的車載機起始外 CAU 的更新？  
否。 CAU 可以僅限報告更新執行的從在 CAU 車載機起始。 不過時後續 CAU 更新執行，, 透過 CAU non\ 的方法已安裝的更新的適當地被視為判斷的額外更新，可能適用的每個節點叢集。  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>可以我唯一 IT 程序會需要 CAU 支援？  
[是]。 CAU 提供下列尺寸彈性符合的企業針對的唯一 IT 程序會需要：  
  
**指令碼**更新執行可以指定 PowerShell 指令碼 pre\ 更新和更新 post\ PowerShell 指令碼。 Pre\ 更新指令碼執行每個叢集節點上之前暫停節點。 Post\ 更新指令碼執行每個叢集節點上之後安裝的更新，節點。  
  
> [!NOTE]  
> 必須在每個您想要執行 pre\ 更新及更新 post\ 指令碼的叢集節點安裝.NET framework 4.6 或 4.5 和 PowerShell。 您必須也可讓遠端 PowerShell 服務叢集節點。 對於詳細的系統需求，請查看[需求與最佳做法叢集更新](cluster-aware-updating-requirements.md)。  
  
**進階更新執行選項]**系統管理員可以此外指定的一組大型進階更新執行的選項，例如更新程序會在每個節點重試次數最大。 可使用 CAU UI 或 CAU PowerShell cmdlet 來指定這些選項。 儲存更新執行設定檔中，針對較新的更新執行其他這些自訂的設定。  
  
**公開 plug\ 中架構**CAU 包含功能登記、取消註冊，並選取 plug\ 寫 CAU 隨附兩個預設 plug\ 增益集：其中每個叢集節點; 座標 Windows Update 代理程式 \(WUA\) Api 第二個適用於 hotfix 手動複製到叢集節點可以存取檔案共用。 如果企業具有獨特的需求，無法使用這些兩個 plug\ 單元符合，企業可以建置新 CAU plug\ 入公用 API 規格依據。 如需詳細資訊，請查看[Cluster\ 感知更新參考 Plug\ 在](http://msdn.microsoft.com/library/hh418084(VS.85).aspx)。  
  
設定及自訂 CAU 相關資訊的 plug\ 單元支援不同更新案例中，查看[Plug\ 集的運作方式](assetId:///847b571b-12b3-473c-953f-75a5a1f51333)。  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>如何匯出 CAU 預覽和更新結果？  
CAU 提供 command\ 列介面，直到 UI 匯出選項。  
  
**Command\ 列介面選項：**  
  
-   透過 PowerShell cmdlet 預覽結果**Invoke\-CauScan |ConvertTo\ Xml**。 輸出：XML  
  
-   透過 PowerShell cmdlet 報告結果**Invoke\-CauRun |ConvertTo\ Xml**。 輸出：XML  
  
-   透過 PowerShell cmdlet 報告結果**Get\-CauReport |Export\-CauReport**。 輸出：HTML CSV  
  
**UI 選項：**  
  
-   複製報告結果從**更新的預覽**畫面。 輸出：CSV  
  
-   複製報告結果從**產生報告**畫面。 輸出：CSV  
  
-   匯出報告結果從**產生報告**畫面。 輸出：HTML  
  
## <a name="how-do-i-install-cau"></a>我該如何安裝 CAU？  
CAU 安裝是順暢整合的容錯功能。 安裝 CAU，如下所示：  
  
-   當叢集節點上安裝容錯時，自動安裝 CAU Windows 管理檢測 \(WMI\) 提供者。  
  
-   當伺服器或 client 的電腦上安裝的容錯移轉叢集工具功能時，會自動安裝更新的叢集 UI 和 PowerShell cmdlet。
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>CAU 需要更新叢集節點上執行元件嗎？  
CAU 不需要叢集節點上執行的服務。 不過，CAU 需要軟體元件 \ 叢集節點安裝 (WMI provider\)。 安裝這個元件容錯功能。  
  
要 self\ 更新模式，請也必須叢集加入 CAU 叢集的角色。  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>有何不同之間 CAU 和 VMM？  
  
-   System Center 一樣管理員 \(VMM\) 的焦點是在更新只 Hyper\ HYPER-V 叢集，而 CAU 可以更新任何類型的支援容錯移轉叢集，包括 Hyper\ HYPER-V 叢集。  
  
-   VMM 需要額外的授權，但 CAU 授權適用於所有 Windows Server。 安裝 CAU 功能、工具及 UI 容錯元件。  
  
-   如果您已經擁有的 System Center 授權，您可以繼續使用 VMM 更新 Hyper\ HYPER-V 叢集，因為它提供的整合式的管理和軟體更新的體驗。  
  
-   CAU 只有群集執行 Windows Server 2016、Windows Server 2012 R2，以及 Windows Server 2012 的支援。 VMM 也支援 Hyper\ HYPER-V 群集上執行 Windows Server 2008 R2 或 Windows Server 2008。  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>我可以使用 remote\ 更新設定叢集上 self\ 更新嗎？  
[是]。 可以透過 remote\ 更新 on\ 需求，更新容錯移轉叢集 self\ 更新設定，就像即使 Windows Update 設定為自動安裝更新，您可以對您的電腦，實施在任何時候掃描 Windows Update。 不過，您必須先確認的更新執行不已在進行中。  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>我是否可以在群集上重複使用我叢集更新設定？  
[是]。 CAU 支援許多判斷更新執行時的處理方式它更新叢集更新執行選項。 這些選項可被儲存成更新執行設定檔，並在任何叢集重複使用。 我們建議您儲存，並在容錯有類似的更新需求重複使用您的設定。 例如，您可能會建立的」Business\ 重大 SQL 伺服器叢集更新執行設定檔」所有的 Microsoft SQL Server 叢集支援 business\ 重要的服務。  
  
## <a name="where-is-the-cau-plug-in-specification"></a>何處是規格 plug\ 中 CAU？  
  
-   [Cluster\ 感知更新 Plug\ 中參考資料](http://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [叢集注意更新 plug\ 中範例](http://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>也了  
  
-   [Cluster\ 感知更新概觀](cluster-aware-updating.md)  
  
