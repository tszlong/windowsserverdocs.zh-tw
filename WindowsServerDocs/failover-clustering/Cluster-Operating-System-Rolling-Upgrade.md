---
title: 叢集作業系統輪流升級
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 03/27/2018
ms.openlocfilehash: f56c036768de7c1afcf3327135a7ff7d7a690a8b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440139"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>叢集作業系統輪流升級

> 適用於：Windows Server 2019，Windows Server 2016

叢集 OS 輪流升級可讓系統管理員可以將一個叢集節點的作業系統升級而不需停止 HYPER-V 或向外延展檔案伺服器工作負載。 此功能可以避免針對服務等級協定 (SLA) 的停機時間扣分。

叢集 OS 輪流升級提供下列優點：

- 執行 HYPER-V 虛擬機器和向外延展檔案伺服器 (SOFS) 工作負載容錯移轉叢集可以升級從 （叢集中的所有節點上執行） 的 Windows Server 2012 R2 到 Windows Server 2016 （叢集中的所有叢集節點上執行） 而不需要停機。 其他叢集工作負載，例如 SQL Server 容錯移轉至 Windows Server 2016 所花費的時間 （通常少於 5 分鐘） 將無法使用。  
- 它不需要任何額外的硬體。 雖然您可以新增其他叢集節點暫時以改善可用性叢集的叢集作業系統輪流升級期間的小型叢集處理。  
- 叢集不需要停止或重新啟動。  
- 不需要新的叢集。 升級現有的叢集。 此外，會使用儲存在 Active Directory 中的現有叢集物件。  
- 客戶選擇 「 點-的-否-return 」，當所有叢集節點執行 Windows Server 2016 中，直到和 Update-clusterfunctionallevel PowerShell cmdlet 執行時可回復升級的程序。  
- 叢集可以在 OS 混合模式中執行時支援修補和維護作業。  
- 它支援透過 PowerShell 和 WMI 的自動化。  
- 叢集的公用屬性**ClusterFunctionalLevel**屬性會指出 Windows Server 2016 叢集節點上叢集的狀態。 使用 PowerShell cmdlet 從 Windows Server 2016 叢集節點屬於容錯移轉叢集可以查詢這個屬性：  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    值為**8**表示叢集正在執行 Windows Server 2012 R2 的功能層級。 值為**9**表示叢集正在執行 Windows Server 2016 功能層級。  

本指南描述叢集作業系統輪流升級程序、 安裝步驟、 功能限制和常見問題 (Faq)，各個階段，而且也適用於 Windows Server 2016 中的下列叢集作業系統輪流升級案例：  
- HYPER-V 叢集  
- 向外延展檔案伺服器叢集  

Windows Server 2016 中不支援下列案例：  
-  叢集作業系統輪流升級的使用做為共用的儲存體的虛擬硬碟 （.vhdx 檔案） 的客體叢集  

叢集 OS 輪流升級，則完全支援由 System Center Virtual Machine Manager (SCVMM) 2016年。 如果您使用 SCVMM 2016，請參閱[在 VMM 中執行輪流升級到 Windows Server 2016 的 HYPER-V 主機叢集的](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807)如升級叢集，以及自動執行這份文件中所述的步驟的指引。  

## <a name="requirements"></a>需求  
在您開始叢集作業系統輪流升級程序前，請完成下列需求：

- 開始執行 Windows Server （半年通道）、 Windows Server 2016 或 Windows Server 2012 R2 的容錯移轉叢集。
- 儲存空間直接存取叢集升級到 Windows Server 1709 版不支援。
- 如果 HYPER-V Vm 或向外延展檔案伺服器叢集工作負載，您可以預期零停機時間升級。
- 請確認 HYPER-V 節點有支援第二層位址資料表 (SLAT) 使用下列方法，其中的 Cpu  
        -檢閱[嗎 SLAT 相容嗎？WP8 SDK 提示 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx)描述兩種方法可以檢查 CPU 是否支援 SLATs 文章  
        -下載[Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722)工具來判斷是否 CPU 支援 SLAT。

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>叢集 OS 輪流升級期間的叢集移轉狀態

本章節描述各種轉換狀態正在升級至 Windows Server 2016 使用叢集作業系統輪流升級的 Windows Server 2012 R2 叢集。  

為了避免執行在叢集作業系統輪流升級過程中，從 Windows Server 2012 R2 節點的叢集工作負載移至 Windows Server 2016 的叢集工作負載節點運作方式就像這兩個節點執行 Windows Server 2012 R2 作業系統。 當 Windows Server 2016 節點新增至叢集時，它們會在 Windows Server 2012 R2 相容性模式運作。 新的概念的叢集模式，稱為 「 OS 混合模式 」，可讓節點存在於相同的不同版本的叢集 （請參閱 圖 1）。  

![圖解顯示三個叢集作業系統輪流升級階段： Windows Server 2012 R2 的所有節點、 OS 混合模式，以及 Windows Server 2016 的所有節點](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**圖 1:叢集作業系統狀態轉換**  

Windows Server 2016 節點新增至叢集時，將 Windows Server 2012 R2 叢集會進入混合 OS 模式。 處理程序是完全復原-可以從叢集移除節點 Windows Server 2016 和 Windows Server 2012 R2 節點加入到此模式中的叢集。 在叢集上執行 Update-clusterfunctionallevel PowerShell cmdlet 時，就會發生 「 點的 no return 」。 為了讓這個指令程式可成功，所有節點必須都是 Windows Server 2016 中，而且所有節點都必須都都在線上。  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>在執行輪流 OS 升級時為四個節點叢集的移轉狀態

本節說明，並描述具有其節點會從 Windows Server 2012 R2 升級到 Windows Server 2016 的共用存放裝置叢集的四個不同的階段。  

「 第 1 階段 」 的初始狀態-我們的 Windows Server 2012 R2 叢集開始。  

![圖例中顯示的初始狀態： Windows Server 2012 R2 的所有節點](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**圖 2:初始狀態：Windows Server 2012 R2 容錯移轉叢集 （第 1 階段）**  

在 「 階段 2 」 中，兩個節點已暫停、 清空、 收回、 重新格式化，並使用 Windows Server 2016 安裝。  

![圖解顯示叢集中混合 OS 模式： 自範例 4 節點叢集，兩個節點都在執行 Windows Server 2016 和兩個節點都在執行 Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**圖 3:中繼狀態：OS 混合模式：Windows Server 2012 R2 和 Windows Server 2016 容錯移轉叢集 (第 2 階段)**  

在 「 階段 3 」、 在叢集中節點的所有已升級至 Windows Server 2016 中，和叢集已經準備好使用 Update-clusterfunctionallevel PowerShell cmdlet 來升級。  

> [!NOTE]  
> 在這個階段，處理程序可以完全回復，且 Windows Server 2012 R2 節點加入到此叢集。  

![顯示已完全升級為 Windows Server 2016 中，該叢集，並將叢集到 Windows Server 2016 功能等級是供 Update-clusterfunctionallevel cmdlet 的圖例](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**圖 4:中繼狀態：升級至 Windows Server 2016，供 Update-clusterfunctionallevel (階段 3) 的所有節點**  

在執行更新 ClusterFunctionalLevelcmdlet 之後，叢集就會進入 「 階段 4 」，可以使用新的 Windows Server 2016 叢集功能的地方。  

![顯示已成功完成叢集輪流升級 OS; 的圖例所有節點都已都升級為 Windows Server 2016 中，和叢集正在以 Windows Server 2016 叢集功能等級](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**圖 5:最終狀態：Windows Server 2016 容錯移轉叢集 （階段 4）**  

## <a name="cluster-os-rolling-upgrade-process"></a>叢集 OS 輪流升級程序

本章節描述執行叢集作業系統輪流升級的工作流程。  

![顯示升級叢集的工作流程的圖例](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**圖 6:叢集 OS 輪流升級程序工作流程**  

叢集 OS 輪流升級包含下列步驟：  

1. 準備叢集的作業系統升級，如下所示：  
    1. 叢集 OS 輪流升級時，需要從叢集移除一次的一個節點。 檢查您是否擁有足夠的容量，叢集維護 HA Sla 時從作業系統升級叢集中移除的其中一個叢集節點上。 換句話說，您需要將工作負載容錯移轉至另一個節點的功能時有一個節點的叢集作業系統輪流升級程序期間，會移除從叢集？ 叢集中有一個節點從叢集移除的叢集作業系統輪流升級時執行必要的工作負載的容量嗎？  
    2. 對於 HYPER-V 工作負載，請檢查所有的 Windows Server 2016 HYPER-V 主機必須支援第二層位址資料表 (SLAT) 的 CPU。 只支援 SLAT 的機器可以使用 Windows Server 2016 中的 HYPER-V 角色。  
    3. 請檢查任何工作負載備份已完成，並考慮備份叢集。 停止備份作業時將節點新增至叢集。  
    4. 檢查所有叢集節點都都在線上/使用的執行/相應增加[ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) cmdlet （請參閱 [圖 7）。  

        ![螢幕擷取畫面顯示執行 Get-clusternode cmdlet 的結果](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **圖 7:判斷使用 Get-clusternode cmdlet 的節點狀態**  

    5. 如果您執行叢集感知更新 (CAU)，請確認 CAU 目前使用中執行**更新叢集 Aware** UI，或有[ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) cmdlet （請參閱 [圖 8）。 停止使用 CAU [ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) cmdlet （請參閱 [圖 9） 以避免任何不會暫停及清空 cau 叢集作業系統輪流升級程序期間的節點。  

        ![螢幕擷取畫面顯示 Get-caurun cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **圖 8:使用[ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps)來判斷是否在叢集上執行叢集感知更新 cmdlet**  

        ![螢幕擷取畫面顯示 Disable-cauclusterrole cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **圖 9:叢集感知更新的角色使用停用[ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) cmdlet**  

2. 每個節點叢集中，完成下列工作︰  
    1. 使用叢集管理員 UI，請選取節點，並使用**暫停 |清空**清空節點的功能表選項 （請參閱 圖 10），或使用[ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) cmdlet （請參閱 [圖 11）。  

        ![示範如何清空角色與 「 叢集管理員 」 UI 的螢幕擷取畫面](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **圖 10:清空節點，使用容錯移轉叢集管理員的角色**  

        ![螢幕擷取畫面顯示 Suspend-clusternode cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **圖 11:清空節點使用的角色[ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) cmdlet**  

    2.  使用叢集管理員 UI**收回**叢集，或使用已暫停的節點[ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) cmdlet。  

        ![螢幕擷取畫面顯示移除節點 cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **圖 12:從叢集中使用 odebrat uzel [ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) cmdlet**  

    3.  重新格式化系統磁碟機，並在節點上執行 Windows Server 2016 的 「 全新的作業系統安裝"**自訂：只安裝 Windows （進階）** setup.exe 安裝 (請參閱 圖 13) 選項。 避免選取**升級：安裝 Windows，並保留檔案、 設定和應用程式**選項，因為叢集作業系統輪流升級不鼓勵在就地升級。  

        ![Windows Server 2016 安裝精靈顯示自訂安裝選項的螢幕擷取畫面](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **圖 13:適用於 Windows Server 2016 的可用安裝選項**  

    4.  將節點新增到適當的 Active Directory 網域。  
    5.  將適當的使用者新增至 Administrators 群組。  
    6.  使用伺服器管理員 UI 或 Install-windowsfeature PowerShell cmdlet，安裝任何您需要例如 HYPER-V 的伺服器角色。  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  使用伺服器管理員 UI 或 Install-windowsfeature PowerShell cmdlet，安裝容錯移轉叢集功能。  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  安裝您的叢集工作負載所需的任何其他功能。  
    9. 請檢查使用容錯移轉叢集管理員 UI 的網路和儲存體的連線設定。  
    10. 如果使用 Windows 防火牆，請檢查防火牆設定是正確的叢集。 例如，叢集感知更新 (CAU) 啟用叢集可能需要防火牆設定。  
    11. 對於 HYPER-V 工作負載，使用 HYPER-V 管理員 UI 來啟動 虛擬交換器管理員 對話方塊 （請參閱 圖 14）。  

        請檢查用於虛擬 Switch(s) 名稱完全相同的叢集中的所有 HYPER-V 主機節點。  

        ![螢幕擷取畫面顯示 [HYPER-V 虛擬交換器管理員] 對話方塊的位置](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **圖 14:虛擬交換器管理員**  

    12. Windows Server 2016 節點上 （不使用 Windows Server 2012 R2 節點），使用容錯移轉叢集管理員] （請參閱 [圖 15） 來連線到叢集。  

        ![顯示選取的叢集 對話方塊的螢幕擷取畫面](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **圖 15:將節點加入叢集使用容錯移轉叢集管理員**  

    13. 使用容錯移轉叢集管理員 UI 或[ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet （請參閱 [圖 16） 將節點新增至叢集。  

        ![螢幕擷取畫面顯示 Add-clusternode cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **圖 16:將節點新增到叢集時使用[ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) cmdlet**  

        > [!NOTE]  
        > 當第一個 Windows Server 2016 節點加入叢集時，叢集會進入 「 混合模式-OS"模式中，和叢集核心資源移到 Windows Server 2016 節點。 「 混合模式-OS"模式叢集是新的節點就是與舊節點相容性模式中執行功能完整叢集。 叢集中的暫時性模式為"混合 OS 」 模式。 它不是在為永久性而客戶應四週內更新其叢集的所有節點中。  

    14. 在 Windows Server 2016 之後節點已成功新增至叢集，您可以 （選擇性） 移動叢集工作負載的一些新加入的節點為了，如下所示，在叢集中重新平衡工作負載：

        ![螢幕擷取畫面顯示 Move-clustervirtualmachinerole cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **圖 17:移動叢集工作負載 （叢集 VM 角色），使用[ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) cmdlet**  

        1. 使用**即時移轉**容錯移轉叢集管理員的虛擬機器或[ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) cmdlet （請參閱 [圖 17） 來執行即時移轉的虛擬機器。  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. 使用**移動**容錯移轉叢集管理員或[ `Move-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterGroup?view=win10-ps) cmdlet 適用於其他叢集工作負載。  

3. 當每個節點已升級至 Windows Server 2016 並新增回叢集，或移出任何其餘的 Windows Server 2012 R2 節點時，執行下列作業：  

    > [!IMPORTANT]  
    > -   更新叢集功能等級之後，您不能移回至 Windows Server 2012 R2 的功能層級和 Windows Server 2012 R2 節點無法新增至叢集。
    > -   直到[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet 執行時，程序是完全復原 Windows Server 2012 R2 節點加入到此叢集和可以移除 Windows Server 2016 節點。  
    > -   在後[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet 執行，將可使用新的功能。  

    1.  使用容錯移轉叢集管理員 UI 或[ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) cmdlet，如預期般運作，所有叢集角色會在叢集都執行的檢查。 在下列範例中，可用的儲存體未使用，改為使用 CSV，因此，會顯示可用的儲存體**離線**狀態 （請參閱 圖 18）。  

        ![螢幕擷取畫面顯示 Get-clustergroup cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **圖 18:驗證所有叢集群組 （叢集角色） 正在使用[ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) cmdlet**  

    2.  檢查所有叢集節點都都在線上並使用執行[ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) cmdlet。  
    3.  執行[ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) cmdlet-不應該會傳回錯誤 （請參閱 [圖 19）。  

        ![螢幕擷取畫面顯示 Update-clusterfunctionallevel cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **圖 19:更新叢集，使用 PowerShell 的功能等級**  

    4.  在後[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet 執行時，有新的功能。  

4. Windows Server 2016-恢復正常的叢集更新與備份：  

    1. 如果您先前已執行 CAU，請重新啟動使用 CAU UI 或使用[ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) cmdlet （請參閱圖 20）。  

        ![顯示的 Enable-cauclusterrole 輸出的螢幕擷取畫面](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **圖 20:啟用叢集感知更新角色使用[ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) cmdlet**  

    2. 繼續執行備份作業。  

5. 啟用並為 HYPER-V 虛擬機器上使用的 Windows Server 2016 功能。  

    1. 叢集升級為 Windows Server 2016 功能等級之後，許多工作負載，例如 HYPER-V Vm 會有新的功能。 如需 HYPER-V 的新功能。 請參閱[移轉和升級虛擬機器](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. 每個 HYPER-V 主機叢集中節點上，使用[ `Get-VMHostSupportedVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Get-VMHostSupportedVersion?view=win10-ps) cmdlet，以檢視主應用程式所支援的 HYPER-V VM 設定版本。  

        ![螢幕擷取畫面顯示 Get VMHostSupportedVersion cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **圖 21:檢視主機所支援的 HYPER-V VM 設定版本**  

   3. 每個 HYPER-V 主機叢集中節點上，HYPER-V VM 組態版本只能升級排定簡短的維護期間與使用者、 備份、 關閉虛擬機器，並執行[ `Update-VMVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Update-VMVersion?view=win10-ps) cmdlet （請參閱圖 22）。 這會更新虛擬機器版本，並啟用新的 HYPER-V 功能，不必將未來的 HYPER-V 整合元件 (IC) 更新。 這個指令程式可以執行從裝載 VM 的 HYPER-V 節點或`-ComputerName`參數可用來從遠端更新 VM 版本。 在此範例中，這裡我們升級設定版本的 VM1 從 5.0 到 7.0 利用許多與此 VM 設定版本，例如生產檢查點 （應用程式一致備份） 和二進位的 VM 相關聯的新 HYPER-V 功能組態檔。  

       ![顯示作用中的更新 VMVersion 指令程式的螢幕擷取畫面](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
       **圖 22:使用更新 VMVersion PowerShell cmdlet 的 VM 版本升級**  

6. 存放集區可以使用升級[Update-storagepool](https://docs.microsoft.com/powershell/module/storage/Update-StoragePool?view=win10-ps) PowerShell cmdlet-這是一項線上作業。  

雖然我們的目標私用雲端案例，特別是 HYPER-V 與向外延展檔案伺服器叢集，可以升級而不需要停機，叢集作業系統輪流升級程序可用於任何叢集角色。  

## <a name="restrictions--limitations"></a>限制 / 限制  
- 這項功能僅適用於 Windows Server 2012 R2，以僅限 Windows Server 2016 的版本。 這項功能無法升級舊版的 Windows Server，例如 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 到 Windows Server 2016。  
- Windows Server 2016 中的每個節點應該重新格式化/新的安裝。 「 位置 」 或 「 升級 」 安裝類型，否則不建議。  
- Windows Server 2016 節點必須用來將 Windows Server 2016 節點新增至叢集。  
- 在管理作業系統混合模式叢集時，一律執行管理工作，從執行 Windows Server 2016 的上層節點。 舊版 Windows Server 2012 R2 節點無法使用 UI 或管理工具，對 Windows Server 2016。  
- 我們鼓勵客戶透過叢集的升級程序快速移動，因為某些叢集功能不適合混合 OS 模式。  
- 避免建立，或在 Windows Server 2016 節點上的儲存體執行時調整大小叢集以 OS 混合模式因為可能的不相容性在容錯移轉從 Windows Server 2016 節點到舊版 Windows Server 2012 R2 節點。  

## <a name="frequently-asked-questions"></a>常見問題集  
**多久可以容錯移轉叢集執行混合 OS 模式？**  
    我們鼓勵客戶完成四週內升級。 有 Windows Server 2016 中的許多最佳化。 我們已經成功升級 HYPER-V 與向外延展檔案伺服器叢集零停機時間少於四個小時的總計。  

**您將連接埠回到 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 的這項功能嗎？**  
    我們並沒有任何連接埠回舊版的這項功能的計劃。 叢集 OS 輪流升級是我們的願景，升級至 Windows Server 2016 和更新版本的 Windows Server 2012 R2 叢集。  

**Windows Server 2012 R2 叢集需要已安裝開始叢集作業系統輪流升級程序之前的所有軟體更新嗎？**  
    是，開始叢集作業系統輪流升級程序之前，請確認所有叢集節點會都更新與最新的軟體更新。  

**我可以執行[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet 時的節點已關閉，或已暫停嗎？**  
    資料分割 所有叢集節點必須都是在和中作用中的成員資格對象[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) cmdlet 才能運作。  

**叢集 OS 輪流升級適用於任何叢集工作負載？它適用於 SQL Server 運作？**  
    是，叢集作業系統輪流升級適用於任何叢集工作負載。 不過，它是 HYPER-V 與向外延展檔案伺服器叢集的唯一零停機。 大部分其他工作負載會產生一些停機時間 （通常幾分鐘的時間） 時它們容錯移轉及容錯移轉時必須有至少一次叢集作業系統輪流升級程序。  

**我可以自動化此程序，使用 PowerShell 嗎？**  
    是，我們有設計叢集作業系統輪流升級來利用 PowerShell 變成自動化。  

**具有額外的工作負載和容錯移轉容量為大型叢集，可以升級多個節點同時嗎？**  
    是的。 從 OS 升級叢集中移除節點時，叢集將會有較少節點容錯移轉，因此會減少容錯移轉容量。 對於具有足夠的工作負載和容錯移轉容量的大型叢集，可以同時升級多個節點。 您可以暫時將叢集節點新增至叢集，以在叢集作業系統輪流升級程序期間提供改善的工作負載和容錯移轉容量。  

**如果我在我的叢集之後，會在發現問題時[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)已順利執行？**  
    如果您已備份的叢集資料庫的系統狀態備份來執行前[ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)，您應該能夠執行授權還原 Windows Server 2012 R2 的叢集節點上，將還原的原始叢集資料庫和組態。  

**可以使用就地升級的每一個節點，而不是藉由重新格式化的系統磁碟機使用清除 OS 安裝嗎？**  
    不建議讓使用就地升級的 Windows Server，但是我們知道，它適用於在某些情況下，使用預設的驅動程式的位置。 叢集節點的就地升級期間，請仔細閱讀所有的警告訊息顯示。  

**如果我使用 HYPER-V 複寫 HYPER-V VM 的 HYPER-V 叢集上，將會複寫仍維持不變期間和之後的叢集作業系統輪流升級程序？**  
    是，HYPER-V 複本會保持不變，期間和之後的叢集作業系統輪流升級程序。  

**可以使用 System Center 2016 Virtual Machine Manager (SCVMM) 將叢集作業系統輪流升級程序自動化？**  
    是，您可以自動在 System Center 2016 中使用 VMM 的叢集作業系統輪流升級程序。  

## <a name="see-also"></a>另請參閱  
-   [版本資訊：Windows Server 2016 中的重要問題](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Windows Server 2016 中的新功能](../get-started/What-s-New-in-windows-server-2016.md)  
-   [容錯移轉叢集的 Windows Server 最新消息](whats-new-in-failover-clustering.md)  
