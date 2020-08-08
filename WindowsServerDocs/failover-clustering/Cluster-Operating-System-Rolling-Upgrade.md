---
title: 叢集作業系統輪流升級
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
manager: lizross
ms.date: 03/27/2018
ms.openlocfilehash: a1694bdb8af495073723a74f24b9d0e153d580b9
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991093"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>叢集作業系統輪流升級

> 適用於：Windows Server 2019、Windows Server 2016

叢集 OS 輪流升級可讓系統管理員升級叢集節點的作業系統，而不需要停止 Hyper-v 或擴充檔案伺服器工作負載。 此功能可以避免針對服務等級協定 (SLA) 的停機時間扣分。

叢集 OS 輪流升級提供下列優點：

- 執行 Hyper-v 虛擬機器和向外延展檔案伺服器 (SOFS) 工作負載的容錯移轉叢集，可以從 Windows Server 2012 R2 升級 (在叢集的所有節點上執行) 到 Windows Server 2016 (在叢集的所有叢集節點上執行) 而不需要停機。 其他叢集工作負載（例如 SQL Server）在 (通常不會在五分鐘內無法使用，) 容錯移轉至 Windows Server 2016。
- 它不需要任何額外的硬體。 不過，您可以暫時將額外的叢集節點新增至小型叢集，以改善叢集 OS 輪流升級程式期間的叢集可用性。
- 不需要停止或重新開機叢集。
- 不需要新的叢集。 現有的叢集已升級。 此外，也會使用儲存在 Active Directory 中的現有叢集物件。
- 升級程式會復原，直到客戶哪「不返回」狀態、當所有叢集節點都執行 Windows Server 2016，以及 ClusterFunctionalLevel PowerShell Cmdlet 執行時。
- 在混合 OS 模式中執行時，叢集可支援修補和維護作業。
- 它支援透過 PowerShell 和 WMI 進行自動化。
- Cluster public property **ClusterFunctionalLevel**屬性工作表示 Windows Server 2016 叢集節點上的叢集狀態。 您可以從屬於容錯移轉叢集的 Windows Server 2016 叢集節點使用 PowerShell Cmdlet 來查詢此屬性：
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel
    ```

    值為**8**表示叢集正在 Windows Server 2012 R2 功能等級執行。 值為**9**表示叢集正在 Windows Server 2016 功能等級執行。

本指南說明叢集 OS 輪流升級程式、安裝步驟、功能限制和常見問題 (常見問題) 的各個階段，並適用于 Windows Server 2016 中的下列叢集 OS 輪流升級案例：
- Hyper-v 叢集
- 向外延展檔案伺服器叢集

Windows Server 2016 不支援下列案例：
-  使用虛擬硬碟 ( .vhdx 檔案) 作為共用存放裝置之來賓叢集的叢集 OS 輪流升級

System Center Virtual Machine Manager (SCVMM) 2016 完全支援叢集 OS 輪流升級。 如果您使用的是 SCVMM 2016，請參閱[在 VMM 中執行 hyper-v 主機叢集的輪流升級至 Windows Server 2016](/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807) ，以取得升級叢集的指引，並將本檔中所述的步驟自動化。

## <a name="requirements"></a>需求
開始叢集 OS 輪流升級程式之前，請先完成下列需求：

- 從執行 Windows Server (半年通道) 、Windows Server 2016 或 Windows Server 2012 R2 的容錯移轉叢集開始。
- 不支援將儲存空間直接存取叢集升級到 Windows Server，版本1709。
- 如果叢集工作負載是 Hyper-v Vm 或向外延展檔案伺服器，您可以預期零停機升級。
- 使用下列其中一種方法，確認 Hyper-v 節點具有支援第二層位址資料表 (SLAT) 的 Cpu;      -請檢查[您是否相容 SLAT？WP8 SDK Tip 01](/archive/blogs/devfish/are-you-slat-compatible-wp8-sdk-tip-01)文章，描述兩種檢查 cpu 是否支援 SLATs 的方法-下載[Coreinfo v 3.31](/sysinternals/downloads/coreinfo)工具，以判斷 CPU 是否支援 SLAT。

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>叢集 OS 輪流升級期間的叢集轉換狀態

本節說明使用叢集 OS 輪流升級升級為 Windows Server 2016 之 Windows Server 2012 R2 叢集的各種轉換狀態。

為了讓叢集工作負載在叢集 OS 輪流升級程式期間執行，將叢集工作負載從 Windows Server 2012 R2 節點移至 Windows Server 2016 節點，就像這兩個節點都是執行 Windows Server 2012 R2 作業系統一樣。 當 Windows Server 2016 節點新增至叢集時，它們會在 Windows Server 2012 R2 相容性模式下運作。 新的概念叢集模式稱為「混合式 OS 模式」，可讓不同版本的節點存在於相同的叢集中 (見 [圖 1]) 。

![此圖顯示叢集 OS 輪流升級的三個階段： [所有節點]、[Windows Server 2012 R2]、[混合 OS 模式] 和 [所有節點]、[Windows Server 2016 ](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)
 **圖 1]：叢集作業系統狀態轉換**

當 Windows Server 2016 節點新增至叢集時，Windows Server 2012 R2 叢集會進入混合 OS 模式。 此程式可完全回復-Windows Server 2016 節點可以從叢集移除，而 Windows Server 2012 R2 節點可以在此模式下新增至叢集。 在叢集上執行 ClusterFunctionalLevel PowerShell Cmdlet 時，就會發生「不傳回的點」。 為了讓此 Cmdlet 成功，所有節點都必須是 Windows Server 2016，而且所有節點都必須在線上。

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>執行滾動 OS 升級時的四節點叢集轉換狀態

本節說明並描述叢集的四個不同階段，其中包含共用存放裝置，其節點會從 Windows Server 2012 R2 升級至 Windows Server 2016。

「第1階段」是初始狀態-我們從 Windows Server 2012 R2 叢集開始。

![顯示初始狀態的圖例：所有節點 Windows Server 2012 R2 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)
 **圖2：初始狀態： windows Server 2012 R2 容錯移轉叢集 (階段 1) **

在「階段2」中，兩個節點已暫停、已清空、已收回、重新格式化，以及與 Windows Server 2016 一起安裝。

![以混合式 OS 模式顯示叢集的圖例：在範例4節點叢集中，兩個節點正在執行 Windows Server 2016，而兩個節點正在執行 Windows Server 2012 R2 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)
 **圖3：中繼狀態：混合作業系統模式： Windows Server 2012 R2 和 Windows Server 2016 容錯移轉叢集 (階段 2) **

在「階段3」，叢集中的所有節點都已升級至 Windows Server 2016，而且叢集已準備好使用 ClusterFunctionalLevel PowerShell Cmdlet 進行升級。

> [!NOTE]
> 在這個階段，可以完全反轉程式，而且可以將 Windows Server 2012 R2 節點新增至此叢集。

![此圖顯示叢集已完全升級至 Windows Server 2016，並已準備好 ClusterFunctionalLevel Cmdlet 將叢集功能等級提升至 Windows Server 2016 [ ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)
 **圖 4]：中繼狀態：所有已升級至 windows server 2016 的節點，已準備好進行更新-ClusterFunctionalLevel (階段 3) **

在執行 ClusterFunctionalLevelCmdlet 之後，叢集會進入「第4階段」，其中可以使用新的 Windows Server 2016 叢集功能。

![此圖顯示已成功完成叢集滾動 OS 升級;所有節點都已升級至 Windows Server 2016，而叢集在 Windows Server 2016 叢集功能等級 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)
 **執行 [圖 5]：最終狀態： Windows Server 2016 容錯移轉叢集 (第4階段) **

## <a name="cluster-os-rolling-upgrade-process"></a>叢集 OS 輪流升級程式

本節說明執行叢集 OS 輪流升級的工作流程。

![此圖顯示升級叢集的工作流程 ](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)
 **圖6：叢集 OS 輪流升級程式工作流程**

叢集 OS 輪流升級包括下列步驟：

1. 為作業系統升級準備叢集，如下所示：
    1. 叢集 OS 輪流升級需要一次從叢集移除一個節點。 檢查叢集是否有足夠的容量，以在作業系統升級時從叢集移除其中一個叢集節點，以維護 HA Sla。 換句話說，當叢集 OS 輪流升級的過程中，從叢集移除某個節點時，您是否需要將工作負載容錯移轉到另一個節點的功能？ 當從叢集移除一個節點進行叢集 OS 輪流升級時，叢集是否具有執行必要工作負載的容量？
    2. 若為 Hyper-v 工作負載，請檢查所有 Windows Server 2016 Hyper-v 主機的 CPU 支援第二層位址表 (SLAT) 。 只有具備 SLAT 功能的電腦可以使用 Windows Server 2016 中的 Hyper-v 角色。
    3. 檢查是否有任何工作負載備份完成，並考慮備份叢集。 將節點新增至叢集時停止備份作業。
    4. 使用 Cmdlet 檢查所有叢集節點是否都在線上/running/up [`Get-ClusterNode`](/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) (參閱 [圖 7]) 。

        ![螢幕擷取畫面顯示執行 Start-clusternode Cmdlet 的結果 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png) **圖7：使用 Start-clusternode Cmdlet 判斷節點狀態**

    5. 如果您要 (CAU) 執行叢集感知更新，請使用叢集**感知更新**UI 或 (Cmdlet 來確認 cau 目前是否正在執行，請 [`Get-CauRun`](/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) 參閱 [圖 8]) 。 使用 Cmdlet 停止 CAU [`Disable-CauClusterRole`](/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) (參閱 [圖 9]) ，以防止在叢集 OS 輪流升級程式期間，cau 暫停和清空任何節點。

        ![螢幕擷取畫面顯示 Get-caurun 指令程式的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png) **圖8：使用 [`Get-CauRun`](/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) Cmdlet 判斷叢集感知更新是否正在叢集上執行**

        ![螢幕擷取畫面顯示 Add-cauclusterrole 指令程式的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png) **圖9：使用 [`Disable-CauClusterRole`](/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) Cmdlet 停用叢集感知更新角色**

2. 針對叢集中的每個節點，完成下列步驟：
    1. 使用 [叢集管理員] UI，選取節點並使用 [**暫停] |清空**節點 (請參閱 [圖 10]) 或使用 [`Suspend-ClusterNode`](/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) Cmdlet (請參閱 [圖 11]) 。

        ![螢幕擷取畫面顯示如何使用叢集管理員 UI 清空角色 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png) **圖10：使用容錯移轉叢集管理員清空節點中的角色**

        ![螢幕擷取畫面顯示暫止 Start-clusternode 指令的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png) **圖11：使用 [`Suspend-ClusterNode`](/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) Cmdlet 清空節點中的角色**

    2.  使用叢集管理員 UI，從叢集**收回**已暫停的節點，或使用 [`Remove-ClusterNode`](/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) Cmdlet。

        ![螢幕擷取畫面顯示 Start-clusternode 指令的輸出 [ ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)
         **圖12：使用 [`Remove-ClusterNode`](/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) Cmdlet 從叢集中移除節點**]

    3.  使用 [**自訂：僅安裝 Windows (advanced) **安裝]，重新格式化系統磁片磁碟機，並在節點上執行 windows Server 2016 的「全新作業系統安裝」 (參閱 setup.exe 中的 [圖 13]) 選項。 請避免選取 [**升級：安裝 Windows 並保留檔案、設定和應用程式**] 選項，因為叢集 OS 輪流升級不鼓勵就地升級。

        ![Windows Server 2016 安裝精靈螢幕擷取畫面，其中顯示已選取的自訂安裝選項 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)
         **圖13：適用于 Windows Server 2016 的可用安裝選項**

    4.  將節點新增至適當的 Active Directory 網域。
    5.  將適當的使用者新增至 [系統管理員] 群組。
    6.  使用伺服器管理員 UI 或 Install-的 PowerShell Cmdlet，安裝您需要的任何伺服器角色，例如 Hyper-v。

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V
        ```

    7.  使用伺服器管理員 UI 或 Install-的 PowerShell Cmdlet，安裝容錯移轉叢集功能。

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering
        ```

    8.  安裝叢集工作負載所需的任何其他功能。
    9. 使用容錯移轉叢集管理員 UI 檢查網路和儲存體連線設定。
    10. 如果使用 Windows 防火牆，請檢查叢集的防火牆設定是否正確。 例如，叢集感知更新 (CAU) 啟用的叢集可能需要防火牆設定。
    11. 若是 Hyper-v 工作負載，請使用 Hyper-v 管理員 UI 來啟動 [虛擬交換器管理員] 對話方塊 (參閱 [圖 14]) 。

        檢查叢集中所有 Hyper-v 主機節點的虛擬交換器 () 使用的名稱是否相同。

        ![顯示 [Hyper-v 虛擬交換器管理員] 對話方塊位置的螢幕擷取畫面 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)
         **圖14：虛擬交換器管理員**

    12. 在 Windows Server 2016 節點上 (不要使用 Windows Server 2012 R2 節點) ，請使用容錯移轉叢集管理員 (參閱 [圖 15) ] 來連線到叢集。

        ![螢幕擷取畫面顯示 [選取叢集] 對話方塊 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)
         **圖15：使用容錯移轉叢集管理員將節點新增至**叢集

    13. 使用容錯移轉叢集管理員 UI 或 [`Add-ClusterNode`](/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) Cmdlet (請參閱 [圖 16]) ，將節點新增至叢集。

        ![螢幕擷取畫面顯示 Start-clusternode Cmdlet 的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)
         **圖16：使用 [`Add-ClusterNode`](/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) Cmdlet 將節點新增至**叢集

        > [!NOTE]
        > 當第一個 Windows Server 2016 節點加入叢集時，叢集會進入「混合 OS」模式，而叢集核心資源則會移至 Windows Server 2016 節點。 「混合 OS」模式叢集是功能完整的叢集，其中新的節點會在與舊節點相容的模式下執行。 「混合作業系統」模式是叢集的暫時性模式。 它不是永久性的，而且客戶預期會在四周內更新其叢集的所有節點。

    14. 在 Windows Server 2016 節點成功新增至叢集之後，您可以 (選擇性地) 將一些叢集工作負載移到新加入的節點，以便在整個叢集中重新平衡工作負載，如下所示：

        ![螢幕擷取畫面顯示 Move-clustervirtualmachinerole 指令的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)
         **圖17：使用 [`Move-ClusterVirtualMachineRole`](/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) Cmdlet 將叢集工作負載 (叢集 VM 角色移動) **

        1. 使用**Live Migration**虛擬機器或 Cmdlet 之容錯移轉叢集管理員中的即時移轉 [`Move-ClusterVirtualMachineRole`](/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) (參閱圖 17) 以執行虛擬機器的即時移轉。

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3
            ```

        2. 針對其他叢集工作負載，請使用容錯移轉叢集管理員的**Move**或 [`Move-ClusterGroup`](/powershell/module/failoverclusters/Move-ClusterGroup?view=win10-ps) Cmdlet。

3. 當每個節點都已升級至 Windows Server 2016 並新增回叢集，或已收回任何剩餘的 Windows Server 2012 R2 節點時，請執行下列動作：

    > [!IMPORTANT]
    > -   更新叢集功能等級之後，您就無法回到 Windows Server 2012 R2 功能等級，而且 Windows Server 2012 R2 節點無法新增至叢集。
    > -   在 [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) 執行此 Cmdlet 之前，程式會完全回復，而 Windows server 2012 R2 節點可以新增到此叢集，而 Windows server 2016 節點則可以移除。
    > -   [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)執行 Cmdlet 之後，將會提供新功能。

    1.  使用容錯移轉叢集管理員 UI 或 [`Get-ClusterGroup`](/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) Cmdlet，檢查所有叢集角色是否如預期般在叢集中執行。 在下列範例中，未使用可用的存放裝置，而是使用 CSV，因此可用的存放裝置會顯示**離線**狀態 (請參閱 [圖 18]) 。

        ![螢幕擷取畫面顯示 Get-clustergroup 指令程式的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)
         **圖18：使用 [`Get-ClusterGroup`](/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) Cmdlet 確認所有叢集群組 (叢集角色) 正在執行**

    2.  檢查所有叢集節點都在線上，並使用 [`Get-ClusterNode`](/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) Cmdlet 執行。
    3.  執行 [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) Cmdlet-應該不會傳回任何錯誤 (見 [圖 19]) 。

        ![螢幕擷取畫面顯示 ClusterFunctionalLevel 指令的輸出-[ ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)
         **圖 19]：使用 PowerShell 更新叢集的功能等級**

    4.  [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)執行 Cmdlet 之後，即可使用新的功能。

4. Windows Server 2016-恢復正常的叢集更新和備份：

    1. 如果您先前執行 CAU，請使用 CAU UI 重新開機，或使用 [`Enable-CauClusterRole`](/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) Cmdlet (參閱圖 20) 。

        ![螢幕擷取畫面顯示 Add-cauclusterrole 的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png) **：使用 [`Enable-CauClusterRole`](/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) Cmdlet 啟用叢集感知更新角色**

    2. 繼續備份作業。

5. 啟用並使用 Hyper-v 虛擬機器上的 Windows Server 2016 功能。

    1. 將叢集升級至 Windows Server 2016 功能等級之後，許多工作負載（如 Hyper-v Vm）將會有新的功能。 以取得新 Hyper-v 功能的清單。 請參閱[遷移和升級虛擬機器](../virtualization/hyper-v/deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)

    2. 在叢集中的每個 Hyper-v 主機節點上，使用 [`Get-VMHostSupportedVersion`](/powershell/module/hyper-v/Get-VMHostSupportedVersion?view=win10-ps) Cmdlet 來查看主機支援的 HYPER-V VM 設定版本。

        ![螢幕擷取畫面顯示 VMHostSupportedVersion 指令程式的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png) **圖21：查看主機支援的 hyper-v VM 設定版本**

   3. 在叢集中的每個 Hyper-v 主機節點上，可以透過排程使用者的簡短維護時間、備份、關閉虛擬機器，以及執行 Cmdlet 來升級 Hyper-v VM 設定版本 [`Update-VMVersion`](/powershell/module/hyper-v/Update-VMVersion?view=win10-ps) (見 [圖 22]) 。 這會更新虛擬機器版本，並啟用新的 Hyper-v 功能，而不需要 (IC) 更新的未來 Hyper-v 整合元件。 您可以從裝載 VM 的 Hyper-v 節點執行此 Cmdlet，或 `-ComputerName` 使用參數從遠端更新 VM 版本。 在此範例中，我們會將 VM1 的設定版本從5.0 升級至7.0，以利用與此 VM 設定版本相關聯的許多新 Hyper-v 功能，例如生產檢查點 (應用程式一致備份) ，以及二進位 VM 設定檔。

       ![螢幕擷取畫面顯示 VMVersion Cmdlet 的動作 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png) **圖22：使用 VMVersion PowerShell CMDLET 升級 VM 版本**

6. 您可以使用[StoragePool](/powershell/module/storage/Update-StoragePool?view=win10-ps) PowerShell Cmdlet 來升級儲存集區-這是一種線上作業。

雖然我們的目標是私人雲端案例，特別是 Hyper-v 和向外延展檔案伺服器叢集（可以在不停機的情況下升級），叢集 OS 輪流升級程式可以用於任何叢集角色。

## <a name="restrictions--limitations"></a>限制/限制
- 這項功能僅適用于 Windows Server 2012 R2 到 Windows Server 2016 版本。 這項功能無法將舊版的 Windows Server （例如 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012）升級至 Windows Server 2016。
- 每個 Windows Server 2016 節點都應該只重新格式化/全新安裝。 不建議採用「就地」或「升級」安裝類型。
- Windows Server 2016 節點必須用來將 Windows Server 2016 節點新增至叢集。
- 管理混合 OS 模式叢集時，請一律從執行 Windows Server 2016 的上級節點執行管理工作。 舊版 Windows Server 2012 R2 節點無法針對 Windows Server 2016 使用 UI 或管理工具。
- 我們鼓勵客戶快速地進行叢集升級程式，因為某些叢集功能並未針對混合 OS 模式優化。
- 當叢集在混合 OS 模式下執行時，請避免在 Windows Server 2016 節點上建立或調整存放裝置大小，因為從 Windows Server 2016 節點容錯移轉到舊版 Windows Server 2012 R2 節點可能不相容。

## <a name="frequently-asked-questions"></a>常見問題集
**容錯移轉叢集可以在混合作業系統模式中執行多久？**
我們鼓勵客戶在四周內完成升級。 Windows Server 2016 中有許多優化。 我們已成功升級 Hyper-v 和向外延展檔案伺服器叢集，但不超過四小時的停機時間。

**您要將此功能移植回 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 嗎？**
我們沒有任何計畫可將此功能移植回先前的版本。 「叢集作業系統輪流升級」是我們將 Windows Server 2012 R2 叢集升級至 Windows Server 2016 及更新的願景。

**Windows Server 2012 R2 叢集在開始叢集 OS 輪流升級程式之前，是否需要安裝所有的軟體更新？**
是，在開始叢集 OS 輪流升級程式之前，請先確認所有叢集節點都已更新最新的軟體更新。

**[`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)當節點關閉或暫停時，我可以執行此 Cmdlet 嗎？**
否。 所有叢集節點都必須是 on 和 active 成員資格， [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) Cmdlet 才能正常執行。

**叢集 OS 輪流升級是否適用于任何叢集工作負載？它是否適用于 SQL Server？**
是，叢集 OS 輪流升級適用于任何叢集工作負載。 不過，Hyper-v 和向外延展檔案伺服器叢集只會有零停機時間。 大部分其他工作負載會產生一些停機時間 (通常會在容錯移轉時) 幾分鐘，而且在叢集 OS 輪流升級程式期間至少需要容錯移轉一次。

**我可以使用 PowerShell 自動執行此程式嗎？**
是，我們已設計要使用 PowerShell 自動化叢集 OS 輪流升級。

**針對具有額外工作負載和容錯移轉容量的大型叢集，是否可以同時升級多個節點？**
是。 從叢集移除一個節點以升級 OS 時，叢集將會有一個較少的節點來進行容錯移轉，因此會降低容錯移轉的容量。 對於具有足夠工作負載和容錯移轉容量的大型叢集，可以同時升級多個節點。 您可以暫時將叢集節點新增至叢集，以在叢集 OS 輪流升級程式期間提供改良的工作負載和容錯移轉功能。

**成功執行之後，如果我在叢集中發現問題，該怎麼辦 [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) ？**
如果您已使用系統狀態備份來備份叢集資料庫，然後再 [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) 執行，您應該能夠在 Windows Server 2012 R2 叢集節點上執行授權還原，並還原原始叢集資料庫和設定。

**我可以為每個節點使用就地升級，而不是透過重新格式化系統磁片磁碟機來使用清理 OS 安裝嗎？**
我們不鼓勵使用 Windows Server 的就地升級，但我們知道在使用預設驅動程式的某些情況下，它會運作。 請仔細閱讀叢集節點的就地升級期間顯示的所有警告訊息。

**如果我在 Hyper-v 叢集上針對 Hyper-v VM 使用 Hyper-v 複寫，在叢集 OS 輪流升級程式期間和之後，複寫是否會維持不變？**
是，在叢集 OS 輪流升級程式期間和之後，Hyper-v 複本會保持不變。

**我可以使用 System Center 2016 Virtual Machine Manager (SCVMM) 來自動化叢集 OS 輪流升級程式嗎？**
是，您可以使用 System Center 2016 中的 VMM，將叢集 OS 輪流升級程式自動化。

## <a name="additional-references"></a>其他參考資料
-   [版本資訊：Windows Server 2016 中的重要問題](../get-started/windows-server-2016-ga-release-notes.md)
-   [Windows Server 2016 中的新功能](../get-started/whats-new-in-windows-server-2016.md)
-   [Windows Server 容錯移轉叢集的新功能](whats-new-in-failover-clustering.md)