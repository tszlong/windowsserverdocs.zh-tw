---
title: 叢集集合
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: 這篇文章說明叢集集案例
ms.localizationpriority: medium
ms.openlocfilehash: 349b69835ae68c626e886cad30f4d5a89d358372
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719712"
---
# <a name="cluster-sets"></a>叢集集合

> 適用於：Windows Server 2019

叢集設定為新的雲端向外延展技術，在 Windows Server 2019 版本中，會在單一的軟體定義資料中心 (SDDC) 雲端中的叢集節點計數增加的重要性順序。 叢集組是鬆散偶合的群組多個容錯移轉叢集： 計算、 儲存體或超交集。 叢集設定技術可讓虛擬機器倍速多個叢集組和整合的儲存體命名空間內的成員叢集以支援虛擬機器流動性在集合。

雖然叢集成員上，保留現有的容錯移轉叢集管理體驗，在叢集設定執行個體此外還提供在彙總的生命週期管理的主要使用案例。 此 Windows Server 2019 案例評估指南將提供必要的背景資訊，以及評估使用 PowerShell 的叢集設定技術的逐步指示。

## <a name="technology-introduction"></a>技術簡介

叢集設定技術開發來符合操作軟體定義資料中心 (SDDC) 雲端規模的特定客戶要求。 叢集的設定可能會彙總的價值主張，如下所示：  

- 藉由結合到單一大型網狀架構中，多個較小的叢集，即使保持在單一叢集軟體錯誤邊界執行高可用性虛擬機器支援的 SDDC 雲端規模會大幅增加
- 管理的整個叢集容錯移轉叢集生命週期包括上架及淘汰，而不會影響租用戶虛擬機器的可用性，透過流暢地移轉這個大型的網狀架構的虛擬機器
- 輕鬆地變更您超融合到儲存體的計算比率我
- 受益[類似 Azure 的容錯網域和可用性設定組](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)多個叢集在初始的虛擬機器放置和後續的虛擬機器移轉
- 混合與比對不同世代的 CPU 硬體到相同叢集中設定網狀架構中，即使保留個別的容錯網域的最高效率的同質性。  請注意，相同的硬體建議仍會出現在每個個別的叢集，以及在整個叢集組。

從高層次的檢視，這會是集看起來會像哪些叢集。

![叢集設定的解決方案檢視](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

以下提供每個圖中元素的快速摘要：

**管理叢集**

管理叢集，叢集中會裝載整個叢集中設定和整合的儲存體 （叢集設定的命名空間） 的命名空間轉介向外延展檔案伺服器 (SOFS) 的高可用性的管理平面的容錯移轉叢集。 管理叢集是以邏輯方式分開執行的虛擬機器工作負載的成員叢集。 這可讓設定管理平面的彈性，為任何當地語系化的全叢集失敗，例如失去電源成員叢集中的叢集。   

**叢集成員**

在叢集中成員叢集通常是執行虛擬機器和儲存空間直接存取的工作負載的傳統超交集叢集。 在單一叢集中設定部署中，以形成更大的 SDDC 雲端網狀架構，參與多個成員的叢集。 成員叢集不同於在兩個重要層面的管理叢集： 叢集成員會參與容錯網域和可用性設定組的建構，以及成員叢集也會調整成主機虛擬機器和儲存空間直接存取的工作負載。 跨叢集界限，在叢集中移動的叢集設定虛擬機器不必須裝載在管理叢集上有基於此。

**叢集中設定 SOFS 的命名空間轉介**

叢集設定命名空間轉介 （叢集設定命名空間） SOFS 是向外延展檔案伺服器，其中每個 SMB 共用上的叢集設定命名空間 sofs 是轉介共用-型別 'SimpleReferral' Windows Server 2019 中新引入。 這種轉介會允許伺服器訊息區塊 (SMB) 用戶端存取 SMB 共用上的目標成員叢集 SOFS 上裝載。 叢集中設定 SOFS 是輕量級轉介機制，因此，不會參與的命名空間轉介 I/O 路徑中。 SMB 快取轉介的永久上每個用戶端節點和叢集設定命名空間以動態方式自動這些轉介會視需要更新。

**設定主機叢集**

在叢集中，成員叢集之間的通訊鬆散偶合，並協調稱為 「 叢集設定主要 」 （CS 主機） 的新叢集資源。 任何其他叢集的資源，例如 CS 主要是高度可用且能夠接受個別成員叢集失敗及/或管理叢集節點失敗。 透過新的叢集設定 WMI 提供者，CS 主要會提供所有的叢集設定管理性互動的管理端點。

**叢集設定背景工作**

在叢集設定的部署中，CS 主要互動都是新的叢集資源，稱為 「 叢集設定背景工作 」 （CS-背景工作角色） 的叢集成員上。 CS-背景工作角色做為協調的本機叢集互動，要求的 CS 主要叢集上的唯一連絡窗口。 此類互動的範例包括虛擬機器放置與叢集本機資源清查。 只會有一個叢集中，叢集的每個成員的 CS-背景工作角色執行個體。 

**容錯網域**

容錯網域是群組的軟體和硬體成品，系統管理員決定可能一起失敗，發生失敗時。  雖然系統管理員無法將指定一個或多個叢集在一起為容錯網域，每個節點可以參與可用性設定組中的容錯網域中。 叢集設定所設計的分葉的容錯網域界限判斷決策的系統管理員是很熟悉的資料中心拓撲考量 – 例如 PDU，網路功能-叢集成員共用。 

**可用性設定組**

可用性設定組可協助系統管理員來設定所需的備援能力的叢集工作負載跨容錯網域組織這些應用程式的可用性設定組和工作負載部署到該可用性設定組。 讓我們假設 是否您要部署兩層式應用程式，我們建議您在可用性設定組的每一層，如此可確保，當該可用性設定組中的一個故障網域停止運作，您的應用程式至少必須設定至少兩部虛擬機器裝載於不同的容錯網域，該相同的可用性設定組的每個層中的一部虛擬機器。

## <a name="why-use-cluster-sets"></a>為何要使用 叢集設定

叢集設定會提供擴充的功效，而不會犧牲恢復功能。  

叢集設定可讓叢集多個叢集一起建立大型的網狀架構中，而每個叢集會維持獨立提供恢復功能。  例如，您有數個的 4 節點 HCI 執行虛擬機器的叢集。  每個叢集提供本身所需的彈性。  如果儲存體或記憶體不夠時，相應增加是下一個步驟。  使用相應增加時，有一些選項和考量。

1. 新增更多的儲存體為目前的叢集。  使用儲存空間直接存取，這可能難處理完全相同的模型/韌體磁碟機可能無法使用。  重建時間的考量也需要列入考量。
2. 擴充更多的記憶體。  如果您被超量機器可以處理的記憶體上呢？  如果所有可用的記憶體位置是完整的？
3. 加入額外的計算節點與磁碟機分組成目前的叢集。  這帶我們回到選項 1 需要考量。
4. 購買全新的叢集

這是其中的叢集設定提供調整的優點。  如果我新增到叢集組叢集時，我可以利用的儲存體或另一個叢集，而不需要任何其他購買項目使用的記憶體。  復原的觀點而言，將其他節點新增至儲存空間直接存取不會提供其他仲裁的投票。  如所述[此處](drive-symmetry-considerations.md)，儲存空間直接存取叢集可以損壞的 2 個節點再往下繼續。  如果您有 4 個節點 HCI 叢集，3 個節點停機會使整個叢集。  如果您有 8 個節點的叢集，3 個節點停機會使整個叢集。  集合中有兩個的 4 節點 HCI 叢集的叢集設定，在一個 HCI 的 2 個節點當機和 1 個節點中其他 HCI 移往下，這兩個叢集保持啟用。  最好是建立一個大型的 16 個節點儲存空間直接存取叢集或細分為四個的 4 節點叢集並使用叢集設定嗎？  有四個的 4 節點叢集與叢集設定可讓相同的標尺，但更好的恢復功能 （意外或進行維護），將可以當機的多個計算節點，並且實際執行的持續狀態。

## <a name="considerations-for-deploying-cluster-sets"></a>部署叢集設定的考量

在考慮叢集設定是否要使用的項目時，請考慮下列問題：

- 您需要超過目前 HCI 計算和儲存體規模限制嗎？
- 所有的計算和儲存體不是完全相同嗎？
- 即時移轉的叢集之間的虛擬機器嗎？
- 您希望類似 Azure 的電腦可用性設定組 」 和 「 容錯網域跨多個叢集？
- 您需要花點時間查看您所有以判斷任何新的虛擬機器要放置的叢集嗎？

如果您的答案為是，叢集設定是會以您的需要。

有幾個考量，較大的 SDDC 可能會變更您的整體資料中心策略的項目。  SQL Server 是一個好例子。  需要授權的其他節點上執行的 SQL 叢集之間移動 SQL Server 虛擬機器嗎？  

## <a name="scale-out-file-server-and-cluster-sets"></a>向外延展檔案伺服器和叢集設定

在 Windows Server 2019，沒有新的向外延展檔案伺服器角色，稱為 「 基礎結構向外延展檔案伺服器 (SOFS)。 

下列考量適用於基礎結構 SOFS 角色：

1.  容錯移轉叢集上可以有最多只能有一個基礎結構 SOFS 叢集角色。 基礎結構 SOFS 角色由指定 「 **-基礎結構**」 切換參數**新增 ClusterScaleOutFileServerRole** cmdlet。  例如:

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  在容錯移轉中自動建立每個 CSV 磁碟區觸發 SMB 共用的建立使用自動產生的名稱為基礎的 CSV 磁碟區名稱。 系統管理員無法直接建立或修改 SMB 共用，將 SOFS 角色，而非透過 CSV 磁碟區建立/修改作業。

3.  在超融合組態中，基礎結構 SOFS 允許 SMB 用戶端 （HYPER-V 主機） 進行通訊與保證持續可用性 (CA) 與基礎結構 SOFS SMB 伺服器。 此超融合 SMB 回送 CA 是透過存取其擁有的虛擬機器識別碼在此轉寄用戶端與伺服器之間的虛擬磁碟 (VHDx) 檔案的虛擬機器來達成。 此身分識別轉送可讓 acl VHDx 檔案，就像標準的超交集叢集設定與之前一樣。

叢集組建立之後，叢集設定命名空間依賴基礎結構 SOFS 上每個成員的叢集，和此外基礎結構 SOFS 管理叢集中。

在成員叢集會新增至叢集組，系統管理員會指定該叢集上的基礎結構 SOFS 名稱是否已經存在。 如果基礎結構 SOFS 不存在，這項作業會建立新的基礎結構 SOFS 角色，在新的成員叢集上。 如果成員在叢集上已經存在的基礎結構 SOFS 角色，新增作業會隱含地重新命名它為指定的名稱視。 任何現有單一 SMB 伺服器或非基礎結構 SOFS 角色成員會保留叢集未用的叢集組上。 

建立叢集集時，系統管理員有做為命名空間管理叢集上使用現有的 AD 電腦物件的選項。 叢集設定建立作業在管理叢集上建立基礎結構 SOFS 叢集角色，或重新命名現有的基礎結構 SOFS 角色只是如先前所述的叢集成員。 在管理叢集上的基礎結構 SOFS 是在叢集中設定命名空間轉介 （叢集設定命名空間） SOFS 做。 它只是表示，每個 SMB 共用，在叢集上設定 SOFS 是轉介共用-Windows Server 2019 中新引入的類型 'SimpleReferral-' 的命名空間。  這種轉介可讓成員叢集 SOFS 上裝載的 SMB 用戶端存取 SMB 共用上的目標。 叢集中設定 SOFS 是輕量級轉介機制，因此，不會參與的命名空間轉介 I/O 路徑中。 SMB 快取轉介的永久上每個用戶端節點和叢集設定命名空間以動態方式自動這些轉介會視需要更新

## <a name="creating-a-cluster-set"></a>建立叢集組

### <a name="prerequisites"></a>必要條件

當建立叢集設定時，您符合下列必要條件，建議使用：

1. 設定執行 Windows Server 2019 的管理用戶端。
2. 此管理伺服器上安裝容錯移轉叢集工具。
3. 建立叢集的成員 （至少兩個叢集至少兩個叢集共用磁碟區上每個叢集）
4. 建立跨越成員叢集管理叢集 （實體或來賓）。  這種方法可確保管理平面會繼續保持為可供使用可能的成員叢集失敗的叢集設定。

### <a name="steps"></a>步驟

1. 建立新的叢集，如必要條件中所定義，從三個叢集設定。  圖表下方提供 建立叢集的範例。  在此範例中設定叢集的名稱會是**CSMASTER**。

   | 叢集名稱               | 基礎結構 SOFS 名稱，以供稍後使用 | 
   |----------------------------|-------------------------------------------|
   | 設定叢集                | SOFS CLUSTERSET                           |
   | CLUSTER1                   | SOFS CLUSTER1                             |
   | CLUSTER2                   | SOFS CLUSTER2                             |

2. 一旦已建立所有的叢集，請使用下列命令來建立叢集集主複本。

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. 若要將叢集伺服器新增至叢集設定中，以下會使用。

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > 如果您使用的靜態的 IP 位址配置，您必須包含 *-StaticAddress x.x.x.x*上**新增 ClusterSet**命令。

4. 當您建立叢集成員超出設定的叢集之後時，您可以列出的節點集和其屬性。  若要列舉在叢集中的所有成員叢集：

        Get-ClusterSetMember -CimSession CSMASTER

5. 若要列舉設定包括管理叢集節點的叢集中的所有成員叢集：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. 若要列出成員叢集的所有節點：

        Get-ClusterSetNode -CimSession CSMASTER

7. 若要列出所有在叢集中的資源群組設定：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. 若要確認叢集設定建立程序建立一個 （識別為 Volume1 或基礎結構的檔案伺服器並為路徑名稱 ScopeName 會標示任何的 CSV 資料夾） 的 SMB 共用基礎結構 SOFS 上每個叢集成員的CSV 磁碟區：

        Get-SmbShare -CimSession CSMASTER

8. 叢集設定已可供檢閱收集偵錯記錄檔。  兩個叢集設定，而叢集偵錯記錄檔可以收集的所有成員和管理叢集。

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. 設定 Kerberos[限制委派](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/)之間所有的叢集設定的成員。

10. 在叢集中的每個節點上設定 Kerberos 的跨叢集虛擬機器即時移轉驗證類型。

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. 將管理叢集新增至叢集中的每個節點上的本機 administrators 群組中。

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>建立新的虛擬機器，並將新增至叢集設定

建立叢集設定後下, 一個步驟是建立新的虛擬機器。  通常，當建立虛擬機器，並將其新增至叢集時，您需要執行某些檢查，以查看可能是最佳上執行叢集上。  這些檢查包括：

- 記憶體數量是在叢集節點上使用？
- 磁碟空間是在叢集節點上使用？
- 虛擬機器需要特定的儲存需求 （也就是我想我的 SQL Server 虛擬機器移至執行更快速的磁碟機; 的叢集，或是我的基礎結構虛擬機器不是那麼重要，而且可以執行速度較慢的磁碟機上）。

之後會回答這個問題，您會建立虛擬機器，您需要在叢集上。  叢集設定的好處之一是叢集設定為您執行這些檢查，以及最適合的節點上放置虛擬機器。

下列命令會同時識別最佳的叢集並將虛擬機器部署到它。  在下列範例中，新的虛擬機器會建立指定至少 4 gb 的記憶體可供虛擬機器，且將需要在利用 1 虛擬處理器。

- 請確定 4gb，僅適用於虛擬機器
- 設定在 1 使用的虛擬處理器
- 請檢查以確定沒有至少 10%的 CPU 使用適用於虛擬機器

   ```PowerShell
   # Identify the optimal node to create a new virtual machine
   $memoryinMB=4096
   $vpcount = 1
   $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10
   $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
   $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

   # Deploy the virtual machine on the optimal node
   Invoke-Command -ComputerName $targetnode.name -scriptblock { param([String]$storagepath); New-VM CSVM1 -MemoryStartupBytes 3072MB -path $storagepath -NewVHDPath CSVM.vhdx -NewVHDSizeBytes 4194304 } -ArgumentList @("\\SOFS-CLUSTER1\VOLUME1") -Credential $cred | Out-Null
   
   Start-VM CSVM1 -ComputerName $targetnode.name | Out-Null
   Get-VM CSVM1 -ComputerName $targetnode.name | fl State, ComputerName
   ```

完成之後，您將會提供有關虛擬機器，以及放置位置的資訊。  在上述範例中，它會顯示為：

        State         : Running
        ComputerName  : 1-S2D2

如果您沒有將虛擬機器足夠的記憶體、 cpu 或磁碟空間，您會收到錯誤：

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

一旦建立虛擬機器之後，它就會顯示在指定的特定節點上的 HYPER-V 管理員 」 中。  因為叢集中設定虛擬機器，並在叢集中，請新增它，則命令如下。  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

完成之後，輸出會是：

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

如果您已新增具有現有的虛擬機器的叢集，虛擬機器也必須向集合註冊所有虛擬機器，要使用的命令是的叢集：

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

不過，程序會完成虛擬機器的路徑需求新增至叢集設定命名空間。

比方說，會加入現有的叢集，而且它具有預先設定的虛擬機器 reside 本機叢集共用磁碟區 (CSV) 上，將 VHDX 的路徑看起來應該類似於 「 C:\ClusterStorage\Volume1\MYVM\Virtual 硬 Disks\MYVM.vhdx。  將存放裝置移轉必須如 CSV 路徑在設計區域的單一成員叢集才能完成。 因此，將無法存取虛擬機器一旦即時成員叢集之間移轉。 

在此範例中，CLUSTER3 已新增至設定新增 ClusterSetMember 使用 SOFS CLUSTER3 作為基礎結構向外延展檔案伺服器叢集。  若要移動的虛擬機器組態和儲存體，命令是：

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

完成時，您會收到一則警告：

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

可以忽略此警告，因為這項警告是 「 已偵測到的虛擬機器角色的儲存體設定的任何變更 」。  做為實際的實體位置警告的原因並不會變更;只有組態路徑。 

如需有關 Move-vmstorage 的詳細資訊，請參閱這[連結](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps)。 

即時移轉虛擬機器不同的叢集設定叢集之間是過去不相同。 在非叢集組的情況下，會使用步驟：

1. 從叢集移除虛擬機器角色。
2. 即時移轉至不同的叢集的成員節點的虛擬機器。
3. 新增到叢集中的虛擬機器，做為新的虛擬機器角色。

叢集將使用下列步驟並非必要，並需要只能有一個指令。  首先，您應該設定所有的網路，可供移轉使用命令：

    Set-VMHost -UseAnyNetworkMigration $true

比方說，我想要將叢集設定的虛擬機器從 CLUSTER1 移至 NODE2 CL3，CLUSTER3 上。  會使用單一命令：

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

請注意這不會移動虛擬機器儲存體或組態檔。  這不是必要，因為虛擬機器的路徑會保持為\\SOFS CLUSTER1\VOLUME1。  一旦註冊虛擬機器具有 叢集設定的 基礎結構的檔案伺服器共用路徑，磁碟機和虛擬機器不需要在虛擬機器相同的電腦上。

## <a name="creating-availability-sets-fault-domains"></a>建立可用性設定組的容錯網域

如簡介中所述，在叢集中可以設定類似 Azure 的容錯網域和可用性設定組。  這是有幫助初始虛擬機器放置與叢集之間的移轉。  

在下列範例中，有四個參與叢集中的叢集。  集中邏輯容錯網域將會建立具有兩個叢集和建立與其他兩個叢集的容錯網域。  這些兩個容錯網域會組成可用性集。 

在下列範例中，cluster1，然後 CLUSTER2 會稱為 「 容錯網域**FD1** CLUSTER3 和 CLUSTER4 會稱為 「 容錯網域中雖然**FD2**。  可用性設定組將會呼叫**CSMASTER-AS** ，且包含兩個容錯網域。

若要建立的容錯網域，命令如下：

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

若要確保他們已成功建立，您可以使用它所顯示的輸出，執行 Get ClusterSetFaultDomain。

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

既然已建立的容錯網域的可用性設定組建立的需求。

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

若要驗證它已建立，然後使用：

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

當建立新的虛擬機器，然後您需要使用-AvailabilitySet 參數做為一部分判斷最佳的節點。  因此它會接著看起來像這樣：

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

從叢集移除叢集中設定因為各種不同的生命週期。 有些的時候，當叢集必須從叢集中移除。 最佳做法，所有叢集設定的虛擬機器應該都移出叢集。 這可以使用來完成**移動 ClusterSetVM**並**Move-vmstorage**命令。

但是，如果也不會移動虛擬機器，叢集設定就會執行一系列的動作，以系統管理員提供直覺式的結果。  從集合移除的叢集時，所有剩餘叢集組上裝載虛擬機器移除叢集只會繫結至該叢集，假設他們能夠存取其存放裝置的高可用性虛擬機器。  叢集設定也會自動將更新其清查：

- 不會再追蹤現在移除叢集，並在其上執行的虛擬機器的健全狀況
- 從叢集設定命名空間和現在移除叢集上裝載共用的所有參考移除

比方說，是要從叢集設定中移除 CLUSTER1 叢集的命令：

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>常見問題集 (FAQ)

**問題：** 在叢集中，只能使用只使用超交集叢集嗎？ <br>
**回答：** 資料分割  您可以混合儲存空間直接存取使用傳統的叢集。

**問題：** 我可以管理我叢集設定透過 System Center Virtual Machine Manager 嗎？ <br>
**回答：** System Center Virtual Machine Manager 目前不支援叢集設定 <br><br> **問題：** 可以 Windows Server 2012 R2 或 2016年叢集共存於相同的叢集組嗎？ <br>
**問題：** 我可以在移轉工作負載關閉 Windows Server 2012 R2 或 2016年的叢集，只要讓這些叢集加入相同的叢集設定嗎？ <br>
**回答：** 叢集設定將推出的 Windows Server 2019，因此這類新技術，在舊版本中不存在。 舊版作業系統為基礎的叢集無法加入叢集組。 不過，叢集作業系統輪流升級技術應該提供您要尋找這些叢集升級到 Windows Server 2019 的移轉功能。

**問題：** 可以 叢集設定會讓我來調整儲存體，或計算 （獨立）？ <br>
**回答：** 是，只要加上的儲存空間直接存取或傳統的 HYPER-V 叢集。 使用叢集設定，就直接變更至儲存體的計算比率，甚至在超交集叢集上的一。

**問題：** 叢集設定的管理工具是什麼 <br>
**回答：** PowerShell 或 WMI，在此版本中。

**問題：** 如何跨叢集即時移轉的不同世代處理器搭配運作？  <br>
**回答：** 叢集設定不會解決處理器的差異和取代 HYPER-V 目前支援。  因此，必須使用的處理器相容性模式與快速移轉。  叢集設定的建議是使用相同處理器的硬體中每個個別的叢集，以及完整的叢集設定進行叢集之間的即時移轉。

**問題：** 可以我的叢集組虛擬機器會自動容錯移轉叢集失敗？  <br>
**回答：** 在此版本中，叢集設定的虛擬機器只能手動即時移轉到叢集中;但不能自動容錯移轉。 

**問題：** 我們如何確保儲存體是從叢集失敗中復原？ <br>
**回答：** 使用多個叢集成員的跨叢集儲存體複本 (SR) 解決方案，以了解叢集失敗的儲存體恢復功能。

**問題：** 我可以使用儲存體複本 (SR) 來複寫多個叢集成員。 SR 容錯移轉叢集設定命名空間儲存體 UNC 路徑變更至複本的目標儲存空間直接存取叢集嗎？ <br>
**回答：** 在此版本中，這類叢集設定命名空間轉介變更不會發生與 SR 容錯移轉。 請讓 Microsoft 知道此案例中是重要，而且您打算使用它的方式。

**問題：** 是否能夠容錯移轉虛擬機器的災害復原狀況中的容錯網域 （比方說，整個容錯網域已關閉）？ <br>
**回答：** 否，請注意，跨叢集容錯移轉中邏輯容錯網域尚不支援。 

**問題：** 我的叢集可以在多個站台 （或 DNS 網域） 中設定範圍的叢集嗎？ <br> 
**回答：** 這是未經測試的案例，而且不會立即針對生產環境支援計畫。 請讓 Microsoft 知道此案例中是重要，而且您打算使用它的方式。

**問題：** 叢集是否設定使用 IPv6？ <br>
**回答：** 在使用容錯移轉叢集，會將 IPv4 和 IPv6 支援與叢集設定。

**問題：** 什麼是叢集的 Active Directory 樹系需求集 <br>
**回答：** 成員的所有叢集必須都位於相同的 AD 樹系。

**問題：** 多少個叢集或節點可以設定為單一叢集的一部分？ <br>
**回答：** 在 Windows Server 2019，叢集組已經過測試並支援多達 64 總叢集節點。 不過，叢集設定架構調整為更大的限制，而且不是硬式編碼為限制的項目。 請讓 Microsoft 知道是否更大的規模很重要而且您打算使用它的方式。

**問題：** 在叢集中的所有儲存空間直接存取叢集將會都形成單一儲存體集區？ <br>
**回答：** 資料分割 單一叢集內，而非橫跨成員在叢集中的叢集，仍可運作儲存空間直接存取技術。

**問題：** 叢集設定高度可用的命名空間？ <br>
**回答：** 是，叢集設定命名空間會提供透過管理叢集上執行的持續可用 (CA) 轉介 SOFS 命名空間伺服器。 Microsoft 建議有足夠數目的虛擬機器從成員的叢集，以進行當地語系化的整個叢集的失敗具有恢復功能。 不過，來處理無法預見的災難性失敗 – 例如中的所有虛擬機器管理叢集停止運作，在同一時間 – 轉介資訊是此外持續快取中每個叢集組節點，即使是在重新開機之間。
 
**問題：** 叢集沒有在叢集中設定命名空間為基礎的儲存體存取儲存體效能變慢？ <br>
**回答：** 資料分割 叢集設定命名空間可以讓叢集集 – 在概念上類似分散式檔案系統命名空間 (DFSN) 內的重疊轉介命名空間。 而且不同於 DFSN，所有叢集設定命名空間轉介中繼資料自動填入，而且自動更新不需要系統管理員介入，所有節點上讓儲存體的存取路徑中不沒有幾乎任何效能負擔。 

**問題：** 如何備份叢集設定中繼資料？ <br>
**回答：** 本指南是容錯移轉叢集的相同。 系統狀態備份會備份叢集狀態。  透過 Windows Server Backup，您可以執行的只是節點的叢集資料庫 （永遠不應該需要因為自我修復邏輯，我們有一堆） 還原或執行權威復原來復原整個叢集資料庫的所有節點。 在叢集設定的情況下，Microsoft 建議進行這類系統授權還原第一次成員的叢集，然後在管理叢集上如有需要。
