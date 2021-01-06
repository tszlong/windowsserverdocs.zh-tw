---
title: 叢集作業系統輪流升級
description: 深入瞭解：叢集作業系統輪流升級
ms.topic: how-to
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
manager: lizross
ms.date: 03/27/2018
ms.openlocfilehash: 1900e85dced6219c1711906e04fb7948f1dd3981
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946874"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>叢集作業系統輪流升級

> 適用於：Windows Server 2019、Windows Server 2016

叢集 OS 輪流升級可讓系統管理員升級叢集節點的作業系統，而不需要停止 Hyper-v 或 Scale-Out 檔案伺服器工作負載。 此功能可以避免針對服務等級協定 (SLA) 的停機時間扣分。

叢集 OS 輪流升級提供下列優點：

- 執行 Hyper-v 虛擬機器和向外延展檔案伺服器 (SOFS) 工作負載的容錯移轉叢集，可以從叢集中所有節點上執行的 Windows Server 2012 R2 (升級) 至叢集的所有叢集節點上執行的 Windows Server 2016 (，而不需要停機。 其他叢集工作負載（例如 SQL Server）在一段時間內將無法使用， (通常不到五分鐘的時間，) 需要容錯移轉至 Windows Server 2016。
- 它不需要任何額外的硬體。 不過，您可以將其他叢集節點暫時新增至小型叢集，以在叢集 OS 輪流升級程式期間改善叢集的可用性。
- 叢集不需要停止或重新開機。
- 不需要新的叢集。 已升級現有的叢集。 此外，也會使用儲存在 Active Directory 中的現有叢集物件。
- 升級程式可復原，直到客戶哪「不回傳」、所有叢集節點都執行 Windows Server 2016，以及執行 Update-ClusterFunctionalLevel PowerShell Cmdlet 的時候。
- 在混合作業系統模式中執行時，叢集可以支援修補和維護作業。
- 它支援透過 PowerShell 和 WMI 進行自動化。
- Cluster public property **ClusterFunctionalLevel** 屬性會指出 Windows Server 2016 叢集節點上的叢集狀態。 您可以使用 PowerShell Cmdlet，從屬於容錯移轉叢集的 Windows Server 2016 叢集節點查詢此屬性：
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel
    ```

    值為 **8** 表示叢集正在 Windows Server 2012 R2 功能等級執行。 值為 **9** 表示叢集正在 Windows Server 2016 功能等級執行。

本指南說明叢集 OS 輪流升級程式的各種階段、安裝步驟、功能限制和常見問題)  (常見問題，並適用于 Windows Server 2016 中的下列叢集 OS 輪流升級案例：
- Hyper-v 叢集
- Scale-Out 檔案伺服器叢集

Windows Server 2016 不支援下列案例：
-  使用虛擬硬碟 ( .vhdx 檔案將來賓叢集的叢集 OS 輪流升級為共用存放裝置) 

System Center Virtual Machine Manager (SCVMM) 2016 完全支援叢集 OS 輪流升級。 如果您使用的是 SCVMM 2016，請參閱在 [VMM 中執行 hyper-v 主機叢集到 Windows Server 2016 的輪流升級](/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807&preserve-view=true) ，以取得升級叢集的指引，並將本檔中所述的步驟自動化。

## <a name="requirements"></a>規格需求
開始進行叢集 OS 輪流升級程式之前，請先完成下列需求：

- 從執行 Windows Server (半年通道) 、Windows Server 2016 或 Windows Server 2012 R2 的容錯移轉叢集開始著手。
- 將儲存空間直接存取叢集升級至 Windows Server 時，不支援1709版。
- 如果叢集工作負載是 Hyper-v Vm 或 Scale-Out 檔案伺服器，則您可以預期零停機時間的升級。
- 使用下列其中一種方法，確認 Hyper-v 節點具有支援 Second-Level 將資料表定址 (SLAT) 的 Cpu。      -請檢查 [您的 SLAT 是否相容？WP8 SDK 提示 01](/archive/blogs/devfish/are-you-slat-compatible-wp8-sdk-tip-01) 文章，描述兩種檢查 cpu 是否支援 SLATs 的方法：下載 [Coreinfo v 3.31](/sysinternals/downloads/coreinfo) 工具，以判斷 CPU 是否支援 SLAT。

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>叢集 OS 輪流升級期間的叢集轉換狀態

本節說明使用叢集 OS 輪流升級來升級為 Windows Server 2016 的 Windows Server 2012 R2 叢集的各種轉換狀態。

為了讓叢集工作負載在叢集 OS 輪流升級程式期間保持執行，將叢集工作負載從 Windows Server 2012 R2 節點移至 Windows Server 2016 節點，會像這兩個節點都執行 Windows Server 2012 R2 作業系統一樣運作。 當 Windows Server 2016 節點新增至叢集時，它們會以 Windows Server 2012 R2 相容性模式運作。 新的概念叢集模式稱為「混合作業系統模式」，可讓不同版本的節點存在於相同的叢集中 (請參閱 [圖 1]) 。

![圖例顯示叢集 OS 輪流升級的三個階段：所有節點的 Windows Server 2012 R2、混合作業系統模式和所有節點 Windows Server 2016 ](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)
 **圖1：叢集作業系統狀態轉換**

當 Windows Server 2016 節點新增至叢集時，Windows Server 2012 R2 叢集會進入混合作業系統模式。 此程式可完全還原-Windows Server 2016 節點可以從叢集移除，而 Windows Server 2012 R2 節點則可以在此模式中新增至叢集。 在叢集上執行 Update-ClusterFunctionalLevel PowerShell Cmdlet 時，會發生「不會傳回」的點。 為了讓此 Cmdlet 成功，所有節點都必須是 Windows Server 2016，而且所有節點都必須在線上。

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>執行滾動 OS 升級時，四節點叢集的轉換狀態

本節說明和描述叢集的四個不同階段，其中有共用儲存體，其節點會從 Windows Server 2012 R2 升級至 Windows Server 2016。

「第1階段」是初始狀態-我們從 Windows Server 2012 R2 叢集開始。

![顯示初始狀態：所有節點的 Windows Server 2012 R2 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)
 **圖2：初始狀態： windows Server 2012 R2 容錯移轉叢集 (階段 1)**

在「階段2」中，已將兩個節點暫停、清空、收回、重新格式化，並隨 Windows Server 2016 安裝。

![以混合作業系統模式顯示叢集的圖解：範例4節點叢集、兩個節點正在執行 Windows Server 2016，以及兩個節點正在執行 Windows Server 2012 R2 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)
 **圖3：中繼狀態：混合作業系統模式： windows Server 2012 R2 和 windows Server 2016 容錯移轉叢集 (階段 2)**

在「第3階段」中，叢集中的所有節點都已升級為 Windows Server 2016，並已準備好使用 Update-ClusterFunctionalLevel PowerShell Cmdlet 進行升級。

> [!NOTE]
> 在這個階段中，可以完全反轉進程，而且可以將 Windows Server 2012 R2 節點新增到此叢集。

![圖例顯示叢集已完全升級至 Windows Server 2016，並已準備好可供 Update-ClusterFunctionalLevel Cmdlet 將叢集功能等級提升到 Windows Server 2016 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)
 **圖4：中繼狀態：已升級為 windows server 2016 的所有節點，可供 Update-ClusterFunctionalLevel (階段 3)**

執行 Update-ClusterFunctionalLevelcmdlet 之後，叢集會進入「第4階段」，可使用新的 Windows Server 2016 叢集功能。

![圖例顯示已順利完成叢集滾動 OS 升級;所有節點都已升級為 Windows Server 2016，且叢集正在 Windows Server 2016 叢集功能等級 [ ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)
 **圖5：最終狀態： Windows Server 2016 容錯移轉叢集 (第4階段)**

## <a name="cluster-os-rolling-upgrade-process"></a>叢集 OS 輪流升級程式

本節說明執行叢集 OS 輪流升級的工作流程。

![圖例顯示升級叢集的工作流程 ](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)
 **圖6：叢集 OS 輪流升級程式工作** 流程

叢集 OS 輪流升級包含下列步驟：

1. 準備叢集以進行作業系統升級，如下所示：
    1. 叢集 OS 輪流升級需要一次從叢集中移除一個節點。 檢查當其中一個叢集節點從叢集移除以進行作業系統升級時，叢集上是否有足夠的容量來維護 HA Sla。 換句話說，當叢集 OS 輪流升級程式期間，如果有一個節點從叢集移除，您是否需要將工作負載容錯移轉到另一個節點的功能？ 從叢集移除一個節點以進行叢集 OS 輪流升級時，叢集是否擁有執行必要工作負載的容量？
    2. 針對 Hyper-v 工作負載，請檢查所有 Windows Server 2016 Hyper-v 主機是否有 Second-Level 位址資料表 (SLAT) 的 CPU 支援。 只有支援 SLAT 的電腦可以使用 Windows Server 2016 中的 Hyper-v 角色。
    3. 檢查是否有任何工作負載備份已完成，並考慮備份叢集。 將節點新增至叢集時，停止備份作業。
    4. 使用 Cmdlet 確認所有叢集節點都在線上/running/up [`Get-ClusterNode`](/powershell/module/failoverclusters/Get-ClusterNode) (參閱 [圖 7]) 。

        ![螢幕擷取畫面顯示執行 Get-ClusterNode Cmdlet 的結果 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png) **圖7：使用 Get-ClusterNode Cmdlet 判斷節點狀態**

    5. 如果您正在執行 (CAU) 的叢集感知更新，請使用叢集 **感知更新** UI 來確認 cau 目前是否正在執行，或 [`Get-CauRun`](/powershell/module/clusterawareupdating/Get-CauRun) Cmdlet (請參閱圖 8) 。 使用指令程式停止 CAU [`Disable-CauClusterRole`](/powershell/module/clusterawareupdating/Disable-CauClusterRole) (請參閱 [圖 9]) ，以防止 cau 在叢集 OS 輪流升級程式期間暫停和耗盡任何節點。

        ![螢幕擷取畫面顯示 Get-CauRun Cmdlet 的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png) **圖8：使用指令 [`Get-CauRun`](/powershell/module/clusterawareupdating/Get-CauRun) 程式判斷叢集感知更新是否正在叢集上執行**

        ![螢幕擷取畫面顯示 Disable-CauClusterRole Cmdlet 的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png) **圖9：使用 [`Disable-CauClusterRole`](/powershell/module/clusterawareupdating/Disable-CauClusterRole) Cmdlet 停用叢集感知更新角色**

2. 針對叢集中的每個節點，完成下列步驟：
    1. 使用叢集管理員 UI，選取節點並使用 **暫停 |清空功能表選項** 以清空節點 (請參閱 [圖 10]) 或使用 [`Suspend-ClusterNode`](/powershell/module/failoverclusters/Suspend-ClusterNode) Cmdlet (請參閱 [圖 11]) 。

        ![螢幕擷取畫面顯示如何使用叢集管理員 UI 清空角色 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png) **圖10：使用容錯移轉叢集管理員清空節點中的角色**

        ![螢幕擷取畫面顯示 Suspend-ClusterNode Cmdlet 的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png) **圖11：使用 [`Suspend-ClusterNode`](/powershell/module/failoverclusters/Suspend-ClusterNode) Cmdlet 清空節點中的角色**

    2.  使用叢集管理員 UI，從叢集 **收回** 暫停的節點，或使用 [`Remove-ClusterNode`](/powershell/module/failoverclusters/Remove-ClusterNode) Cmdlet。

        ![螢幕擷取畫面顯示 Remove-ClusterNode Cmdlet 的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)
         **圖12：使用 [`Remove-ClusterNode`](/powershell/module/failoverclusters/Remove-ClusterNode) Cmdlet 從叢集中移除節點**

    3.  使用 **自訂：只安裝 windows (advanced)** 安裝，重新格式化系統磁片磁碟機，並在節點上執行 Windows Server 2016 的「全新作業系統安裝」 (請參閱 setup.exe 中的圖 13) 選項。 避免選取 [ **升級：安裝 Windows 並保留檔案、設定和應用程式** ] 選項，因為叢集 OS 輪流升級並不建議就地升級。

        ![Windows Server 2016 安裝程式的螢幕擷取畫面，其中顯示選取的自訂安裝選項 [ ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)
         **圖13：適用于 Windows Server 2016 的可用安裝選項**]

    4.  將節點新增至適當的 Active Directory 網域。
    5.  將適當的使用者新增至 Administrators 群組。
    6.  使用伺服器管理員 UI 或 Install-WindowsFeature PowerShell Cmdlet，安裝您需要的任何伺服器角色，例如 Hyper-v。

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V
        ```

    7.  使用伺服器管理員 UI 或 Install-WindowsFeature PowerShell Cmdlet 安裝容錯移轉叢集功能。

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering
        ```

    8.  安裝叢集工作負載所需的任何其他功能。
    9. 使用容錯移轉叢集管理員 UI 檢查網路和存放裝置連線設定。
    10. 如果使用 Windows 防火牆，請檢查叢集的防火牆設定是否正確。 例如，叢集感知更新 (CAU) 啟用的叢集可能需要防火牆設定。
    11. 針對 Hyper-v 工作負載，請使用 Hyper-v 管理員 UI 來啟動 [虛擬交換器管理員] 對話方塊 (參閱圖 14) 。

        檢查叢集中所有 Hyper-v 主機節點的虛擬交換器 () 所使用的名稱是否相同。

        ![螢幕擷取畫面顯示 Hyper-v 虛擬交換器管理員對話方塊的位置 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)
         **圖14：虛擬交換器管理員**

    12. 在 Windows Server 2016 節點上 (不要使用 Windows Server 2012 R2 節點) ，請使用容錯移轉叢集管理員 (查看 [圖 15]) 以連接到叢集。

        ![螢幕擷取畫面顯示 [選取叢集] 對話方塊 [ ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)
         **圖15：使用容錯移轉叢集管理員將節點新增至** 叢集

    13. 使用容錯移轉叢集管理員 UI 或 [`Add-ClusterNode`](/powershell/module/failoverclusters/Add-ClusterNode) Cmdlet (請參閱 [圖 16]) 將節點新增至叢集。

        ![螢幕擷取畫面顯示 Add-ClusterNode Cmdlet 的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)
         **圖16：使用 [`Add-ClusterNode`](/powershell/module/failoverclusters/Add-ClusterNode) Cmdlet 將節點新增至** 叢集

        > [!NOTE]
        > 當第一個 Windows Server 2016 節點加入叢集時，叢集會進入「混合作業系統」模式，並將叢集核心資源移至 Windows Server 2016 節點。 「混合 OS」模式叢集是功能完整的叢集，其中新的節點會在具有舊節點的相容性模式下執行。 「混合作業系統」模式是叢集的暫時性模式。 它不是永久性的，而且客戶預期會在四周內更新其叢集的所有節點。

    14. 在 Windows Server 2016 節點成功新增至叢集之後，您可以 (選擇性地) 將某些叢集工作負載移至新加入的節點，以便在整個叢集中重新平衡工作負載，如下所示：

        ![螢幕擷取畫面顯示 Move-ClusterVirtualMachineRole Cmdlet 的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)
         **圖17：使用 [`Move-ClusterVirtualMachineRole`](/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole) Cmdlet)  (叢集 VM 角色移動叢集工作負載**

        1. 使用虛擬機器或 Cmdlet 的容錯移轉叢集管理員 **即時移轉** [`Move-ClusterVirtualMachineRole`](/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole) (請參閱圖 17) 以執行虛擬機器的即時移轉。

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3
            ```

        2. 使用從容錯移轉叢集管理員或 Cmdlet **移至** [`Move-ClusterGroup`](/powershell/module/failoverclusters/Move-ClusterGroup) 其他叢集工作負載。

3. 當每個節點都已升級為 Windows Server 2016 並新增回叢集，或已收回任何剩餘的 Windows Server 2012 R2 節點時，請執行下列動作：

    > [!IMPORTANT]
    > -   更新叢集功能等級之後，您就無法回到 Windows Server 2012 R2 功能等級，也無法將 Windows Server 2012 R2 節點新增至叢集。
    > -   在 [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel) 執行指令程式之前，程式是可完全還原的，而且可以將 Windows server 2012 R2 節點新增到此叢集，而且可以移除 Windows server 2016 節點。
    > -   [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel)執行 Cmdlet 之後，將可使用新功能。

    1.  使用容錯移轉叢集管理員 UI 或 [`Get-ClusterGroup`](/powershell/module/failoverclusters/Get-ClusterGroup) Cmdlet，檢查叢集中的所有叢集角色是否如預期般執行。 在下列範例中，未使用可用的存放裝置，而是使用 CSV，因此可用的存放裝置會顯示 **離線** 狀態 (請參閱圖 18) 。

        ![螢幕擷取畫面顯示 Get-ClusterGroup Cmdlet 的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)
         **圖18：確認 (叢集角色) 的所有叢集群組都是使用 [`Get-ClusterGroup`](/powershell/module/failoverclusters/Get-ClusterGroup) Cmdlet 執行**

    2.  使用 Cmdlet 檢查所有叢集節點是否已上線且正在執行 [`Get-ClusterNode`](/powershell/module/failoverclusters/Get-ClusterNode) 。
    3.  執行 [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) Cmdlet-不會傳回任何錯誤 (請參閱圖 19) 。

        ![螢幕擷取畫面顯示 Update-ClusterFunctionalLevel Cmdlet 的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)
         **圖19：使用 PowerShell 更新叢集的功能等級**

    4.  [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel)執行 Cmdlet 之後，就可以使用新功能。

4. Windows Server 2016-繼續一般叢集更新和備份：

    1. 如果您先前執行 CAU，請使用 CAU UI 重新開機它，或使用 [`Enable-CauClusterRole`](/powershell/module/clusterawareupdating/Enable-CauClusterRole) Cmdlet (參閱 [圖 20]) 。

        ![螢幕擷取畫面顯示 Add-cauclusterrole ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png) **圖20：使用 [`Enable-CauClusterRole`](/powershell/module/clusterawareupdating/Enable-CauClusterRole) Cmdlet 啟用叢集感知更新角色** 的輸出

    2. 繼續執行備份作業。

5. 在 Hyper-v 虛擬機器上啟用及使用 Windows Server 2016 功能。

    1. 將叢集升級至 Windows Server 2016 功能等級之後，許多工作負載（例如 Hyper-v Vm）將會有新的功能。 以取得新 Hyper-v 功能的清單。 請參閱 [遷移及升級虛擬機器](../virtualization/hyper-v/deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)

    2. 在叢集中的每個 Hyper-v 主機節點上，使用 [`Get-VMHostSupportedVersion`](/powershell/module/hyper-v/Get-VMHostSupportedVersion) Cmdlet 來查看主機支援的 HYPER-V VM 設定版本。

        ![螢幕擷取畫面顯示 Get-VMHostSupportedVersion Cmdlet 的輸出 ](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png) **圖21：觀看主機支援的 hyper-v VM 設定版本**

   3. 在叢集中的每個 Hyper-v 主機節點上，您可以藉由排程具有使用者、備份、關閉虛擬機器和執行 Cmdlet (的短暫維護時段來升級 Hyper-v VM 設定版本 [`Update-VMVersion`](/powershell/module/hyper-v/Update-VMVersion) 參閱圖 22) 。 這將會更新虛擬機器版本，並啟用新的 Hyper-v 功能，而不需要 (IC) 更新的未來 Hyper-v 整合元件。 您可以從裝載 VM 的 Hyper-v 節點執行此 Cmdlet，也可以 `-ComputerName` 使用參數從遠端更新 VM 版本。 在此範例中，我們會將 VM1 的設定版本從5.0 升級為7.0，以利用與此 VM 設定版本相關聯的許多新 Hyper-v 功能，例如生產檢查點 (應用程式一致備份) 和二進位 VM 設定檔案。

       ![螢幕擷取畫面顯示操作圖22中的 Update-VMVersion Cmdlet ](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png) **：使用 Update-VMVersion PowerShell CMDLET 升級 VM 版本**

6. 您可以使用 [StoragePool](/powershell/module/storage/Update-StoragePool) PowerShell Cmdlet 來升級儲存集區，這是一項線上作業。

雖然我們的目標是私用雲端案例，特別是 Hyper-v 和向外延展檔案伺服器叢集（可以在不停機的情況下升級），但叢集 OS 輪流升級程式可以用於任何叢集角色。

## <a name="restrictions--limitations"></a>限制/限制
- 這項功能僅適用于 Windows Server 2012 R2 至 Windows Server 2016 版本。 這項功能無法將較早版本的 Windows Server （例如 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012）升級為 Windows Server 2016。
- 每個 Windows Server 2016 節點都應該只重新格式化/全新安裝。 不建議「就地」或「升級」安裝類型。
- Windows Server 2016 節點必須用來將 Windows Server 2016 節點新增至叢集。
- 管理混合作業系統模式叢集時，請一律從執行 Windows Server 2016 的上層節點執行管理工作。 舊版 Windows Server 2012 R2 節點無法針對 Windows Server 2016 使用 UI 或管理工具。
- 我們鼓勵客戶迅速進行叢集升級程式，因為某些叢集功能並未針對混合作業系統模式進行優化。
- 在叢集以混合作業系統模式執行時，請避免在 Windows Server 2016 節點上建立或調整儲存體大小，因為從 Windows Server 2016 節點容錯移轉至舊版 Windows Server 2012 R2 節點可能不相容。

## <a name="frequently-asked-questions"></a>常見問題集
**容錯移轉叢集可在混合作業系統模式中執行多久？**
我們鼓勵客戶在四周內完成升級。 Windows Server 2016 中有許多優化。 我們已成功升級 Hyper-v 和向外延展檔案伺服器叢集，且在四小時內都不會停機。

**您會將這項功能移植回 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 嗎？**
我們沒有任何計畫將這項功能移植回先前的版本。 叢集 OS 輪流升級是我們將 Windows Server 2012 R2 叢集升級至 Windows Server 2016 的願景。

**Windows Server 2012 R2 叢集是否需要在啟動叢集 OS 輪流升級程式之前安裝所有的軟體更新？**
是，在開始叢集 OS 輪流升級程式之前，請先確認所有的叢集節點都已更新為最新的軟體更新。

**[`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel)當節點關閉或暫停時，我可以執行此 Cmdlet 嗎？**
不會。 所有叢集節點都必須在作用中，且必須在作用中的成員資格中， [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel) Cmdlet 才能運作。

**叢集 OS 輪流升級是否適用于任何叢集工作負載？它是否適用于 SQL Server？**
是，叢集 OS 輪流升級適用于任何叢集工作負載。 不過，Hyper-v 和向外延展檔案伺服器叢集只有零停機時間。 大部分的其他工作負載會產生一些停機時間 (通常會在容錯移轉時) 幾分鐘，而在叢集作業系統輪流升級程式期間，至少需要進行一次容錯移轉。

**我可以使用 PowerShell 將此程式自動化嗎？**
是，我們設計了叢集 OS 輪流升級，以使用 PowerShell 進行自動化。

**對於具有額外工作負載和容錯移轉容量的大型叢集，是否可以同時升級多個節點？**
是。 當某個節點從叢集移除以升級 OS 時，叢集將會有一個較少的節點進行容錯移轉，因此將會有降低的容錯移轉容量。 對於具有足夠工作負載和容錯移轉容量的大型叢集，可以同時升級多個節點。 您可以暫時將叢集節點新增至叢集，以在叢集 OS 輪流升級程式期間提供改進的工作負載和容錯移轉容量。

**如果我在我的叢集中發現問題之後發現問題，該怎麼辦 [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel) ？**
如果您在執行之前已備份叢集資料庫和系統狀態備份 [`Update-ClusterFunctionalLevel`](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel) ，您應該能夠在 Windows Server 2012 R2 叢集節點上執行授權還原，並還原原始叢集資料庫和設定。

**我可以針對每個節點使用就地升級，而不是透過重新格式化系統磁片磁碟機來使用乾淨 OS 安裝？**
我們不鼓勵使用 Windows Server 的就地升級，但我們知道在使用預設驅動程式的某些情況下，它可以運作。 請仔細閱讀在叢集節點的就地升級期間顯示的所有警告訊息。

**如果我在 Hyper-v 叢集上使用 hyper-v VM 的 Hyper-v 複寫，在叢集 OS 輪流升級程式期間和之後，複寫是否會維持不變？**
是，在叢集 OS 輪流升級程式期間和之後，Hyper-v 複本會維持不變。

**我可以使用 System Center 2016 Virtual Machine Manager (SCVMM) 來自動化叢集 OS 輪流升級程式嗎？**
是的，您可以在 System Center 2016 中使用 VMM 將叢集 OS 輪流升級程式自動化。

## <a name="additional-references"></a>其他參考資料
-   [版本資訊：Windows Server 2016 中的重要問題](../get-started/windows-server-2016-ga-release-notes.md)
-   [Windows Server 2016 中的新功能](../get-started/whats-new-in-windows-server-2016.md)
-   [Windows Server 容錯移轉叢集的新功能](whats-new-in-failover-clustering.md)
