---
title: "升級叢集作業系統"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2017
ms.openlocfilehash: 8463c163294a4d2223a74b7cfeaea6ac5ae4fcfe
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-operating-system-rolling-upgrade"></a>升級叢集作業系統

> 適用於：Windows Server（以每年次通道）、Windows Server 2016

作業系統循環升級叢集讓系統管理員可以而停止 HYPER-V 或延展檔案伺服器工作負載升級叢集節點作業系統。 使用這項功能，可避免當機損失針對服務層級合約 (SLA)。

叢集 OS 循環升級有下列優點：

- 執行 HYPER-V 一樣和延展檔案伺服器 (SOFS) 工作負載容錯可以升級（叢集中的所有節點都執行），Windows Server 2012 R2 的 Windows Server 2016（叢集所有叢集節點都都執行），而中斷。 其他叢集工作負載，例如 SQL Server 中，將不提供容錯移轉到 Windows Server 2016 所花費的時間（通常不會超過五分鐘）。  
- 它並不需要任何額外的硬體。 雖然您可以新增額外叢集節點暫時小群集叢集 OS 循環升級期間改善可用性叢集來處理。  
- 停止或重新啟動不需要叢集。  
- 不需要新的叢集。 升級現有叢集。 此外，也會使用儲存在 Active Directory 現有叢集物件。  
- 升級程序，而且可還原之前客戶 choses」點-的-無-報酬「，所有叢集節點時正在執行 Windows Server 2016 Update-ClusterFunctionalLevel PowerShell cmdlet 執行時。  
- 叢集可支援修補和維護作業時混合 OS 模式執行。  
- 透過 PowerShell 和 WMI 自動化支援。  
- 叢集公用屬性**ClusterFunctionalLevel**屬性表示 Windows Server 2016 叢集節點上叢集的狀態。 此屬性可以使用 Windows Server 2016 叢集節點屬於容錯移轉叢集從 PowerShell cmdlet 查詢：  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    為**8**代表叢集正在執行 Windows Server 2012 R2 功能層級。 為**9**代表叢集正在執行 Windows Server 2016 功能層級。  

本指南描述的各種階段叢集 OS 循環升級程序、安裝步驟、功能限制，以及常見問題的解答（常見問題集）與適用於在 Windows Server 2016 下列作業系統循環升級叢集案例：  
- HYPER-V 叢集  
- 延展檔案伺服器叢集  

Windows Server 2016 不支援下列案例：  
-  使用 virtual 硬碟升級的客體 OS 循環叢集叢集 (.vhdx file) 為共用存放裝置  

叢集 OS 循環升級完全支援的系統中心一樣管理員 (SCVMM) 2016 年。 如果您使用 SCVMM 2016，請查看[升級 Windows Server 2012 R2 叢集 Windows Server 2016 中 VMM 以](https://technet.microsoft.com/library/mt445417.aspx)指導方針升級叢集和自動化本文件中所述的步驟。  

## <a name="requirements"></a>需求  
叢集 OS 循環升級程序開始之前，請完成下列需求：

- 開始執行 Windows Server（以每年次通道）、Windows Server 2016 或 Windows Server 2012 R2 容錯移轉叢集。
- 升級到 Windows Server 的儲存空間直接存取叢集，不支援 1709 年版本。
- 如果 HYPER-V Vm 中或延展檔案伺服器叢集工作負載，您可以預期的中斷零升級。
- 請確認 HYPER-V 節點有 Cpu 支援第二層級位址表格（造板條）使用其中一項下列方法。  
        -檢視[您會造板條相容？WP8 SDK 秘訣 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx)描述兩種方法來檢查是否 CPU 支援 SLATs 文件  
        -下載[Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722)若要判斷是否 CPU 支援造板條工具。

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>叢集狀態轉換期間叢集 OS 循環升級

本節升級到 Windows Server 2016 使用叢集 OS 循環升級的 Windows Server 2012 R2 叢集各種轉換狀態。  

為了持續在叢集 OS 循環升級過程中，將叢集工作負載的 Windows Server 2012 R2 節點從移到 Windows Server 2016 上執行的叢集工作負載節點適用於為兩個節點執行 Windows Server 2012 R2 的作業系統。 Windows Server 2016 節點新增至叢集時, 他們在 Windows Server 2012 R2 相容模式運作。 新的概念叢集模式，稱為「混合 OS 模式」可讓在於相同不同版本的節點叢集（查看圖 1）。  

![顯示叢集 OS 循環升級的三個階段的圖例：所有節點 Windows Server 2012 R2、混合 OS 模式和 Windows Server 2016 的所有節點](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**圖 1：叢集作業系統狀態轉換**  

Windows Server 2012 R2 叢集進入混合 OS 模式，當叢集加入 Windows Server 2016 節點。 程序完全復原-叢集可以會從 Windows Server 2016 節點，而且可在此模式下叢集加入 Windows Server 2012 R2 節點。 「點的無報酬「發生於 Update-ClusterFunctionalLevel PowerShell cmdlet 已叢集上執行。 為了讓這個 cmdlet 成功，所有節點都必須 Windows Server 2016 和都必須 online 所有節點。  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>在執行作業系統循環升級時的四個節點叢集的轉換狀態

本章節示範，並描述叢集共用其節點從 Windows Server 2012 R2 升級至 Windows Server 2016 的儲存空間不同的四個階段。  

「階段 1」的初始狀態-我們開始與 Windows Server 2012 R2 叢集。  

![圖的顯示的初始狀態：Windows Server 2012 R2 的所有節點](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**圖 2 所示：起始狀態：Windows Server 2012 R2 容錯移轉叢集（1 階段）**  

在「步驟 2」，有兩個節點已暫停、耗盡、收回、格式化，並安裝 Windows Server 2016。  

![顯示叢集混合 OS 模式中的圖例：退出範例 4 節點叢集、兩個節點執行 Windows Server 2016 和兩個節點執行 Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**圖 3 所示：中繼狀態：模式混合作業系統：Windows Server 2012 R2 和 Windows Server 2016 容錯移轉叢集 (第 2 階段)**  

在「步驟 3」、節點叢集中的所有已升級至 Windows Server 2016 和叢集已準備好升級的 Update-ClusterFunctionalLevel PowerShell cmdlet。  

> [!NOTE]  
> 這個階段程序可以完全還原，且 Windows Server 2012 R2 節點可以新增至此叢集。  

![顯示叢集已完全升級到 Windows Server 2016，並將操之在 Windows Server 2016 的叢集功能層級可供 Update-ClusterFunctionalLevel cmdlet 的圖例](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**圖 4：中繼狀態：所有節點升級到 Windows Server 2016 供 Update-ClusterFunctionalLevel (階段 3)**  

Update-ClusterFunctionalLevelcmdlet 執行之後，叢集進入「步驟 4」，可以使用 Windows Server 2016 叢集的新功能的地方。  

![顯示已成功完成升級作業系統叢集; 的圖例所有節點已都升級至 Windows Server 2016 和叢集正在執行的 Windows Server 2016 叢集功能層級。](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**圖 5 所示：最終狀態：Windows Server 2016 容錯移轉叢集（階段 4）**  

## <a name="cluster-os-rolling-upgrade-process"></a>叢集 OS 循環升級程序

本節適用於執行叢集 OS 循環升級流程。  

![顯示適用於升級叢集工作流程的圖例](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**圖 6：叢集 OS 循環升級程序工作流程**  

叢集 OS 循環升級包含下列步驟：  

1. 準備叢集作業系統升級，如下所示：  
    1. 作業系統循環升級叢集需要一次移除叢集一個節點。 檢查是否有容量不足時其中一個節點叢集移除作業系統升級叢集維持哈 Sla 叢集上。 亦即，您需要到另一個節點容錯移轉工作負載的功能的叢集 OS 循環升級程序期間移除一個節點時嗎？ 叢集有一個節點中移除的叢集 OS 循環升級時，請執行所需的工作負載的容量嗎？  
    2. HYPER-V 工作負載，查看所有的 Windows Server 2016 HYPER-V 主機有 CPU 支援第二層級地址表格（造板條）。 僅限造板條能力的電腦可以使用 Windows Server 2016 HYPER-V 角色。  
    3. 檢查任何工作負載的備份完成後，請考慮將備份叢集。 將節點新增至叢集時停止備份作業。  
    4. 查看所有叢集節點都是 online/執行/上使用[`Get-ClusterNode`](https://technet.microsoft.com/library/ee460990.aspx) cmdlet（看到圖 7 所示）。  

        ![顯示的執行 Get-ClusterNode cmdlet 結果 Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **圖 7 所示：判斷使用 Get-ClusterNode cmdlet 節點狀態**  

    5. 如果您執行叢集注意更新 (CAU)，請確認如果 CAU 目前執行的是，使用**更新叢集-Aware**的 UI，或[`Get-CauRun`](https://technet.microsoft.com/library/hh847230.aspx) cmdlet（看到圖 8）。 停止 CAU 使用[`Disable-CauClusterRole`](https://technet.microsoft.com/library/hh847219.aspx) cmdlet 為防止任何節點暫停和耗盡，CAU 叢集 OS 循環升級程序期間的（查看圖 9）。  

        ![Screencap 顯示 Get-CauRun cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **使用圖 8: [`Get-CauRun`](https://technet.microsoft.com/library/hh847230.aspx)cmdlet 來判斷是否叢集上執行叢集注意更新**  

        ![Screencap 顯示 Disable-CauClusterRole cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **圖 9：停用叢集注意更新角色使用[`Disable-CauClusterRole`](https://technet.microsoft.com/library/hh847219.aspx) cmdlet**  

2. 用於叢集中每個節點，完成下列動作：  
    1. 使用叢集管理員 UI，請選取 [節點，使用**暫停 |耗盡**功能表選項，來耗盡節點（看到圖 10）或使用[`Suspend-ClusterNode`](https://technet.microsoft.com/library/ee461051.aspx) cmdlet（看到圖 11）。  

        ![Screencap 顯示如何耗盡 UI 叢集管理員角色](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **圖 10：耗盡使用容錯移轉叢集管理員節點角色**  

        ![Screencap 顯示 Suspend-ClusterNode cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **圖 11：耗盡角色節點，使用[`Suspend-ClusterNode`](https://technet.microsoft.com/library/ee461051.aspx) cmdlet**  

    2.  使用叢集管理員 UI，**收回**叢集或使用來自 [暫停] 節點[`Remove-ClusterNode`](https://technet.microsoft.com/library/ee461001.aspx) cmdlet。  

        ![Screencap 顯示 Remove-ClusterNode cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **圖 12：移除節點叢集使用[`Remove-ClusterNode`](https://technet.microsoft.com/library/ee461001.aspx) cmdlet**  

    3.  格式化系統磁碟機，並節點使用執行 Windows Server 2016 的「全新作業系統安裝」**自訂：只安裝 Windows（進階），**中 setup.exe 安裝 (看到圖 13) 選項。 避免選取**升級：安裝 Windows 並保留檔案、設定和應用程式**選項，因為 OS 循環升級叢集無法鼓勵就地升級。  

        ![Windows Server 2016 安裝精靈中顯示自訂的 Screencap 安裝選取選項](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Windows Server 2016 圖 13：安裝可用的選項**  

    4.  將節點新增至適當的 Active Directory domain。  
    5.  系統管理員群組新增適當的使用者。  
    6.  使用伺服器管理員 UI 或 Install-WindowsFeature PowerShell cmdlet、安裝您需要例如 HYPER-V 的任何伺服器角色。  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  使用伺服器管理員 UI 或 Install-WindowsFeature PowerShell cmdlet、安裝容錯功能。  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  安裝任何其他您叢集工作負載所需的功能。  
    9. 檢查網路與儲存空間使用容錯移轉叢集管理員 UI 連接設定。  
    10. 如果您使用 Windows 防火牆，檢查防火牆設定是否正確叢集。 例如，群集叢集注意更新 (CAU) 支援可能需要防火牆設定。  
    11. 對於 HYPER-V 工作負載，使用 HYPER-V 管理員 UI 上市 Virtual 切換管理員對話方塊（看到圖 14）。  

        檢查 Virtual 達名稱所使用的相同叢集中的所有 HYPER-V 主機節點。  

        ![Screencap 顯示 \ [HYPER-V Virtual 切換管理員] 中的位置](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **圖 14︰ Virtual 切換管理員**  

    12. Windows Server 2016 節點上（請使用 Windows Server 2012 R2 節點）、使用容錯移轉叢集管理員叢集連接的（查看圖 15）。  

        ![Screencap 顯示 \ 選取叢集對話方塊](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **圖 15：將節點新增至使用容錯移轉叢集管理員叢集**  

    13. 使用容錯移轉叢集管理員 UI 或[`Add-ClusterNode`](https://technet.microsoft.com/library/ee461047.aspx) cmdlet 將節點新增至叢集的（查看圖 16）。  

        ![Screencap 顯示 Add-ClusterNode cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **圖 16：將節點新增至叢集使用[`Add-ClusterNode`](https://technet.microsoft.com/library/ee461047.aspx) cmdlet**  

        > [!NOTE]  
        > 當的第一個 Windows Server 2016 節點加入叢集時，叢集進入「混合 OS」模式，並叢集核心資源的移到 Windows Server 2016 節點。 「混合 OS」模式叢集是完整功能叢集新節點執行舊節點與相容性模式中的位置。 「混合 OS」模式是叢集暫時性模式。 這不是會永久與他們叢集節點所有更新中四個星期會如預期般針對。  

    14. Windows Server 2016 節點成功新增至後叢集，您可以（選擇性）移叢集工作負載的部分新加入節點以重新工作負載平衡叢集，如下所示：

        ![Screencap 顯示 Move-ClusterVirtualMachineRole cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **圖 17：移動叢集工作負載（叢集 VM 角色）使用[`Move-ClusterVirtualMachineRole`](https://technet.microsoft.com/library/ee461041.aspx) cmdlet**  

        1. 使用**動態移轉**來自容錯移轉叢集管理員虛擬機器或[`Move-ClusterVirtualMachineRole`](https://technet.microsoft.com/library/ee461041.aspx) cmdlet 執行虛擬機器動態移轉的（查看圖 17）。  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. 使用**移動**來自容錯移轉叢集管理員或[`Move-ClusterGroup`](https://technet.microsoft.com/library/ee461002.aspx)的其他叢集工作負載 cmdlet。  

3. 當您已升級至 Windows Server 2016 和加回叢集，每個節點或任何其他 Windows Server 2012 R2 節點上有移除，執行下列動作：  

    > [!IMPORTANT]  
    > -   叢集功能層級更新之後，您無法回復到 Windows Server 2012 R2 功能層級，叢集無法加入 Windows Server 2012 R2 節點。
    > -   直到[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) cmdlet 執行、程序會完全復原 Windows Server 2012 R2 節點新增到此叢集及 Windows Server 2016 節點可以移除。  
    > -   在[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) cmdlet 執行時，將提供新功能。  

    1.  使用「容錯移轉叢集管理員 UI 或[`Get-ClusterGroup`](https://technet.microsoft.com/library/ee461017.aspx) cmdlet，檢查所有叢集角色執行叢集上如預期般運作。 以下的範例，可用儲存空間未使用，改為使用 CSV，因此，會顯示可用的儲存**離線**狀態（看到圖 18）。  

        ![Screencap 顯示 Get-ClusterGroup cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **使用驗證所有叢集群組（叢集角色）執行圖 18: [`Get-ClusterGroup`](https://technet.microsoft.com/library/ee461017.aspx)cmdlet**  

    2.  查看所有叢集節點都是 online 並執行使用[`Get-ClusterNode`](https://technet.microsoft.com/library/ee460990.aspx) cmdlet。  
    3.  執行[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) cmdlet-不應該傳回錯誤（看到圖 19）。  

        ![Screencap 顯示 Update-ClusterFunctionalLevel cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **圖 19：更新功能的使用 PowerShell 叢集層級**  

    4.  在[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) cmdlet 執行、的新功能可供使用。  

4. Windows Server 2016-恢復正常叢集更新和備份：  

    1. 如果您之前已執行 CAU，使用 CAU UI 重新開機，或使用[`Enable-CauClusterRole`](https://technet.microsoft.com/library/hh847229.aspx) cmdlet（看到圖 20）。  

        ![Screencap 顯示 Enable-CauClusterRole 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **圖 20：讓叢集注意更新角色使用[`Enable-CauClusterRole`](https://technet.microsoft.com/library/hh847229.aspx) cmdlet**  

    2. 繼續備份作業。  

5. 讓和的 HYPER-V 虛擬電腦上使用 Windows Server 2016 的功能。  

    1. 已叢集升級到 Windows Server 2016 功能等級之後，類似於 HYPER-V Vm 許多工作負載會有新功能。 於 HYPER-V 的新功能的清單。 查看[移轉和升級虛擬電腦](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. 在每個 HYPER-V 主機] 節點中叢集上使用[`Get-VMHostSupportedVersion`](https://technet.microsoft.com/library/mt653838.aspx) cmdlet 所支援的主機的 HYPER-V VM 設定版本的檢視。  

        ![Screencap 顯示 Get-VMHostSupportedVersion cmdlet 的輸出](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **圖 21：檢視支援的主機的 HYPER-V VM 設定版本**  

   3.  每個 HYPER-V 主機] 節點中叢集上可以 HYPER-V VM 設定版本升級排程簡短維護視窗的使用者，備份、關閉虛擬電腦，然後執行[`Update-VMVersion`](https://technet.microsoft.com/library/mt484146.aspx) cmdlet（看到圖 22）。 這將會更新一樣版本，以及新 HYPER-V 功能，不需要 HYPER-V 整合元件 (IC) 未來的更新。 可以從 HYPER-V 節點裝載 VM 中，執行下列 cmdlet 或`-ComputerName`參數可從遠端更新 VM 版本。 在此範例中，以下我們 VM1 設定版本從升級 5.0 7.0 善用 Production 檢查點（應用程式一致備份），和二進位 VM 設定檔此 VM 設定版本與相關的許多新 HYPER-V 功能。  

        ![控制項目] 中顯示 Update-VMVersion cmdlet Screencap](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
        **圖 22：升級使用 Update-VMVersion PowerShell cmdlet VM 版本**  

4.  可以使用升級儲存集區[更新-StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/update-storagepool) PowerShell cmdlet-這是 online 作業。  

雖然我們的目標私人雲端案例中，尤其是 HYPER-V 和延展檔案伺服器叢集，而當機，升級作業系統循環叢集程序可以升級可用於任何叢集角色。  

## <a name="restrictions--limitations"></a>限制日限制  
- 此功能僅適只有在 Windows Server 2016 版本的 Windows Server 2012 R2。 此功能無法升級至 Windows Server 2016 的較舊版本的 Windows Server，例如 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012。  
- Windows Server 2016 的每個節點應該只格式化日全新安裝。 「就地」或「升級「建議您安裝類型。  
- Windows Server 2016 節點新增至叢集必須使用 Windows Server 2016 節點。  
- 管理混合 OS 模式叢集、時一律從在執行 Windows Server 2016 新版節點執行管理工作。 舊版 Windows Server 2012 R2 節點無法使用針對 Windows Server 2016 的 UI 或管理工具。  
- 我們建議針對移動叢集升級程序快速因為部分叢集功能不最佳化混合 OS 模式。  
- 避免建立或調整節點 Windows Server 2016 上的儲存空間大小叢集執行時混合 OS 模式因為可能不相容容錯移轉上從 Windows Server 2016 節點舊版 Windows Server 2012 R2 節點。  

## <a name="frequently-asked-questions"></a>常見問題集  
**多久容錯移轉叢集在執行作業系統混合模式？**  
    我們建議針對完成的四個星期中升級。 有許多最佳化 Windows Server 2016 中。 我們已經順利升級 HYPER-V 和延展檔案伺服器叢集零中斷的四個小時總中。  

**您將會連接埠回到 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 這項功能？**  
    我們不需要任何計劃連接埠回復到舊版這項功能。 作業系統循環升級叢集是我們的 Windows Server 2016 和其他升級 Windows Server 2012 R2 叢集的願景。  

**Windows Server 2012 R2 叢集需要先升級作業系統循環叢集程序安裝所有的軟體更新嗎？**  
    是的在開始之前叢集 OS 循環升級程序，確認所有叢集節點都更新的最新的軟體更新。  

**我可以執行[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) cmdlet 時節點已關閉或暫停嗎？**  
    否。 在和的使用資格，必須為所有叢集節點[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) cmdlet 運作。  

**作業系統循環升級叢集運作的任何叢集工作負載嗎？ 它 SQL server 運作？**  
    是的作業系統循環升級叢集適用於任何叢集工作負載。 不過，它是 HYPER-V 和延展檔案伺服器叢集的唯一零中斷。 其他大部分的工作負載收取某些當機（通常幾分鐘）時他們錯誤移轉及容錯移轉是需要叢集 OS 循環升級程序期間至少一次。  

**可以自動執行此程序使用 PowerShell 嗎？**  
    是的我們已設計叢集 OS 循環無法使用 PowerShell 自動升級。  

**大叢集的額外工作負載與錯誤後移轉的容量，我是否可以升級多個節點同時？**  
    [是]。 當升級作業系統叢集從移除一個節點時，叢集將會有一個較節點容錯移轉的因此必須容錯減少的移轉的容量。 大容錯移轉容量不足，無法工作負載與叢集，可以同時升級多個節點。 您可以暫時將提供工作負載改進和錯誤後移轉容量叢集 OS 循環升級程序期間叢集叢集節點加入。  

**如果我之後我叢集探索問題[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)順利執行嗎？**  
    如果您有備份叢集資料庫系統狀態備份之前，請先執行的[`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)，您應該可以執行置於還原到 Windows Server 2012 R2 叢集節點上並還原原始叢集資料庫和設定。  

**可以使用就地升級為每個節點，而不是使用清潔作業系統安裝重新系統磁碟機格式化嗎？**  
    我們不會鼓勵就地升級的 Windows Server、使用，但我們已經知道運作有時候可用預設驅動程式。 請仔細朗讀叢集節點就地升級過程中顯示所有警告訊息。  

**如果我正在使用的 HYPER-V VM HYPER-V 複寫，我 HYPER-V 叢集上，將會複寫維持關聯性，叢集 OS 循環升級程序期間後嗎？**  
    是的 HYPER-V 複本 app 會維持關聯性叢集 OS 循環升級程序期間後。  

**可以使用 System Center 2016 一樣管理員 (SCVMM) 自動叢集 OS 循環升級程序嗎？**  
    是的您可以將使用 System Center 2016 VMM 叢集 OS 循環升級程序。  

## <a name="see-also"></a>也了  
-   [版本注意事項：在 Windows Server 2016 中的重要問題](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Windows Server 2016 中的新功能](../get-started/What-s-New-in-windows-server-2016.md)  
-   [Windows Server 容錯中的新功能](whats-new-in-failover-clustering.md)  