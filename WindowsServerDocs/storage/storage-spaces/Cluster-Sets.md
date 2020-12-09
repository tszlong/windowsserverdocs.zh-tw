---
title: 叢集集合
manager: eldenc
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 01/30/2019
description: 本文說明叢集集合案例
ms.localizationpriority: medium
ms.openlocfilehash: edef2a8585a773069eb4b7f36ee607926e78b60c
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865937"
---
# <a name="cluster-sets"></a>叢集集合

> 適用於：Windows Server 2019

叢集集是 Windows Server 2019 版中的新雲端相應放大技術，可將單一軟體定義資料中心內的叢集節點計數增加 (SDDC) 雲端依量級的順序排列。 叢集集是多個容錯移轉叢集的鬆散結合群組：計算、儲存體或超交集。 叢集集技術可跨叢集集內的成員叢集，以及跨虛擬機器流動性支援的集中整合儲存體命名空間，來流動性虛擬機器。

雖然在成員叢集上保留現有的容錯移轉叢集管理經驗，但叢集集合實例還會在匯總的生命週期管理周圍提供主要的使用案例。 此 Windows Server 2019 案例評估指南提供您所需的背景資訊，以及使用 PowerShell 評估叢集集合技術的逐步指示。

## <a name="technology-introduction"></a>技術簡介

叢集集技術的開發目的，是為了滿足特定客戶要求，以大規模的 (SDDC) 雲端來操作軟體定義的資料中心。 群集集價值主張可能摘要如下：

- 藉由將多個較小的叢集合併成單一大型網狀架構，以大幅增加支援的 SDDC 雲端規模來執行高可用性虛擬機器，即使將軟體錯誤界限保留在單一叢集
- 透過流暢地跨此大型網狀架構遷移虛擬機器，管理整個容錯移轉叢集生命週期，包括上線和淘汰叢集，而不會影響租使用者虛擬機器的可用性
- 在您的超融合式 I 中輕鬆變更計算和儲存體的比率
- 在初始虛擬機器放置和後續的虛擬機器遷移中，從叢集的 [容錯網域和可用性設定組](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) 獲益
- 將不同的 CPU 硬體世代混合和比對到相同的叢集組網狀架構中，即使是讓個別的容錯網域維持在最高效率的情況下也是一樣。 請注意，相同硬體的建議仍存在於每個個別叢集和整個叢集集內。

從高階觀點來看，叢集集看起來會像這樣。

![叢集設定解決方案視圖](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

以下提供上述影像中每個元素的快速摘要：

**管理叢集**

叢集集中的管理叢集是一個容錯移轉叢集，可裝載整個叢集集的高可用性管理平面，以及 (叢集組命名空間) 參考 Scale-Out 檔案伺服器 (SOFS) 的整合儲存體命名空間。 管理叢集會以邏輯方式與執行虛擬機器工作負載的成員叢集分離。 這可讓叢集集管理平面復原到任何當地語系化的全叢集失敗，例如遺失成員叢集的電源。

**成員叢集**

群集集中的成員叢集通常是執行虛擬機器和儲存空間直接存取工作負載的傳統超交集叢集。 多個成員叢集會參與單一叢集集部署，形成較大型的 SDDC 雲端網狀架構。 成員叢集與管理叢集有兩個不同之處：成員叢集參與容錯網域和可用性設定組結構，而成員叢集也會調整大小以裝載虛擬機器和儲存空間直接存取工作負載。 因為這個原因，所以在叢集集內移動叢集界限的叢集設定虛擬機器，不能裝載于管理叢集上。

**叢集組命名空間參考 SOFS**

叢集組命名空間的參考 (叢集集合命名空間) SOFS 是一種 Scale-Out 檔案伺服器，其中叢集組命名空間 SOFS 上的每個 SMB 共用都是在 Windows Server 2019 中新引進的參考共用類型 ' SimpleReferral '。 此參照可讓伺服器訊息區 (SMB) 用戶端存取成員叢集 SOFS 上裝載的目標 SMB 共用。 Cluster set namespace 參考 SOFS 是輕量的參考機制，因此不會參與 i/o 路徑。 系統會在每個用戶端節點上快取永久的 SMB 參考，而叢集集合命名空間則會視需要自動自動更新這些參考。

**叢集設定主機**

在叢集集中，成員叢集之間的通訊會鬆散結合，並由稱為「叢集設定主機」的新叢集資源 (CS-主要) 進行協調。 如同任何其他叢集資源，CS-Master 對個別成員叢集失敗和/或管理叢集節點失敗的高度可用和復原。 透過新的叢集設定 WMI 提供者，CS-Master 提供管理端點，讓所有叢集設定的管理能力互動。

**叢集設定背景工作**

在叢集集部署中，CS-Master 會與名為「叢集集背景工作」之成員叢集上的新叢集資源進行互動 (CS 背景工作) 。 CS-Worker 可作為叢集上的唯一聯絡人，以協調 CS 主機所要求的本機叢集互動。 這類互動的範例包括虛擬機器放置和叢集本機資源清查。 叢集中的每個成員叢集只有一個 CS-Worker 實例。

**容錯網域**

容錯網域是在發生失敗時，系統管理員所決定的軟體和硬體構件群組。 雖然系統管理員可以將一或多個叢集指定為容錯網域，但每個節點都可以參與可用性設定組中的容錯網域。 依設計而定，叢集集會決定容錯網域界限的決策，而系統管理員則是使用資料中心拓撲考慮（例如 PDU、網路功能）進行精通共用的系統管理員。

**可用性設定組**

可用性設定組可協助系統管理員跨容錯網域設定所需的叢集工作負載冗余，方法是將它們組織成可用性設定組，並將工作負載部署到該可用性設定組。 假設您要部署兩層式應用程式，我們建議您在每個階層的可用性設定組中至少設定兩部虛擬機器，如此一來，當該可用性設定組中的一個容錯網域中斷時，您的應用程式在每一層中至少會有一部虛擬機器裝載在同一個可用性設定組的不同容錯網域上。

## <a name="why-use-cluster-sets"></a>為何要使用叢集集合

叢集集會提供調整的優點，而不會犧牲復原能力。

叢集集可讓您將多個叢集叢集在一起，以建立大型網狀架構，而每個叢集都維持獨立以提供復原功能。 例如，您有幾個執行虛擬機器的四節點 HCI 叢集。 每個叢集都提供自己所需的復原能力。 如果儲存體或記憶體開始填滿，則相應增加是下一個步驟。 相應增加時，有一些選項和考慮。

1. 將更多儲存體新增至目前的叢集。 使用儲存空間直接存取時，這可能會很難處理，因為完全相同的模型/固件磁片磁碟機可能無法使用。 重建時間的考慮也需要納入考慮。
2. 擴充更多的記憶體。 如果您要超量機器可以處理的記憶體，該怎麼辦？  如果所有可用的記憶體插槽都已滿，該怎麼辦？
3. 將具有磁片磁碟機的其他計算節點新增至目前的叢集中。 這會讓我們回到需要考慮的選項1。
4. 購買全新的叢集

這是叢集集合提供調整的優點。 如果我將叢集新增至叢集集合，則可以利用另一個叢集上可用的儲存體或記憶體，而不需要任何額外的購買專案。 從復原的角度來看，將其他節點新增至儲存空間直接存取不會提供其他的仲裁投票。 如 [這裡](drive-symmetry-considerations.md)所述，儲存空間直接存取叢集可在中斷前中斷2個節點。 如果您有4個節點的 HCI 叢集，3個節點將會讓整個叢集停止運作。 如果您有8個節點的叢集，3個節點將會讓整個叢集停止運作。 在集合中具有 2 4 節點 HCI 叢集的叢集集之後，一個 HCI 中的2個節點會停止運作，而另一個 HCI 中的1個節點則會維持在最新狀態。 最好是建立一個大型的16節點儲存空間直接存取叢集，還是將它分成 4 4 節點的叢集，並使用叢集集？  擁有具有叢集集的 4 4 節點叢集可提供相同的規模，但更好的復原方式是讓多個計算節點 (非預期地關閉，或維持) 和生產環境的維護。

## <a name="considerations-for-deploying-cluster-sets"></a>部署叢集集的考慮

當您考慮到叢集集是否為您需要使用的任何內容時，請考慮下列問題：

- 您是否需要超越目前的 HCI 計算和儲存體調整限制？
- 所有計算和儲存體的名稱都不相同嗎？
- 您是否要在叢集之間即時移轉虛擬機器？
- 您是否希望在多個叢集之間有類似 Azure 的電腦可用性設定組和容錯網域？
- 您需要花時間查看所有叢集，以判斷是否需要放置任何新的虛擬機器？

如果您的答案是肯定的，則叢集集合就是您需要的。

有幾個其他專案需要考慮，因為較大的 SDDC 可能會變更您的整體資料中心策略。 SQL Server 是很好的範例。 在叢集之間移動 SQL Server 虛擬機器是否需要授權 SQL 才能在其他節點上執行？

## <a name="scale-out-file-server-and-cluster-sets"></a>擴充檔案伺服器和叢集集

在 Windows Server 2019 中，有一個新的擴充檔案伺服器角色，稱為基礎結構 Scale-Out 檔案伺服器 (SOFS) 。

下列考慮適用于基礎結構 SOFS 角色：

1. 在容錯移轉叢集上最多隻能有一個基礎結構 SOFS 叢集角色。 基礎結構 SOFS 角色是藉由指定 **ClusterScaleOutFileServerRole** 指令程式的 "**-基礎**" 切換參數來建立。 例如：

    ```PowerShell
    Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure
    ```

2. 在容錯移轉中建立的每個 CSV 磁片區都會根據 CSV 磁片區名稱，自動觸發以自動產生的名稱建立 SMB 共用。 系統管理員無法直接在 SOFS 角色下建立或修改 SMB 共用，而不是透過 CSV 磁片區建立/修改作業。

3. 在超交集設定中，基礎結構 SOFS 可讓 (Hyper-v 主機) 的 SMB 用戶端與基礎結構 SOFS SMB 伺服器的保證持續可用性 () CA 進行通訊。 這項超交集 SMB 回送 CA 可透過存取其虛擬磁片的虛擬機器，在用戶端與伺服器之間轉送的擁有虛擬機器 (VHDx) 檔案中達成。 此身分識別轉送允許 ACL 的 VHDx 檔案，如同之前的標準超交集叢集設定一樣。

一旦建立叢集集，叢集集合命名空間會依賴每個成員叢集上 SOFS 的基礎結構，以及管理叢集中的基礎結構 SOFS。

當成員叢集新增至叢集集時，系統管理員會在該叢集上指定基礎結構 SOFS 的名稱（如果已經存在）。 如果基礎結構 SOFS 不存在，此作業會建立新成員叢集上的新基礎結構 SOFS 角色。 如果成員叢集上已有基礎結構 SOFS 角色，則「新增」作業會視需要隱含地將它重新命名為指定的名稱。 叢集集會未運用成員叢集上任何現有的單一 SMB 伺服器或非基礎結構 SOFS 角色。

建立叢集集時，系統管理員可以選擇使用現有的 AD 電腦物件做為管理叢集上的命名空間根。 叢集集建立作業會在管理叢集上建立基礎結構 SOFS 叢集角色，或重新命名現有的基礎結構 SOFS 角色，如同先前針對成員叢集所述。 管理叢集上的基礎結構 SOFS 會用來作為叢集組命名空間的參考 (Cluster Set Namespace) SOFS。 這只是表示叢集組命名空間 SOFS 上的每個 SMB 共用都是一種參考共用，其類型為 ' SimpleReferral '-剛在 Windows Server 2019 中引進。 此參照可讓 SMB 用戶端存取成員叢集 SOFS 上裝載的目標 SMB 共用。 Cluster set namespace 參考 SOFS 是輕量的參考機制，因此不會參與 i/o 路徑。 系統會在每個用戶端節點上快取永久的 SMB 參考，而叢集集合命名空間則會視需要動態地自動更新這些參考

## <a name="creating-a-cluster-set"></a>建立叢集集

### <a name="prerequisites"></a>必要條件

建立叢集集時，建議您遵循下列必要條件：

1. 設定執行 Windows Server 2019 的管理用戶端。
2. 在此管理伺服器上安裝容錯移轉叢集工具。
3. 在每個叢集上建立至少有兩個叢集共用磁片區的叢集成員 () 
4. 建立跨越成員叢集 (實體或來賓) 的管理叢集。 這種方法可確保即使有可能的成員叢集失敗，仍可繼續使用叢集集管理平面。

### <a name="steps"></a>步驟

1. 從三個叢集建立新的叢集集，如必要條件中所定義。 下圖提供要建立的叢集範例。 此範例中的叢集集名稱將會是 **CSMASTER**。

   | 叢集名稱 | 稍後要使用的基礎結構 SOFS 名稱 |
   |--------------|-------------------------------------------|
   | 設定叢集  | SOFS-CLUSTERSET                           |
   | CLUSTER1     | SOFS-CLUSTER1                             |
   | CLUSTER2     | SOFS-CLUSTER2                             |

2. 建立所有叢集之後，請使用下列命令來建立叢集設定主機：

    ```PowerShell
    New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER
    ```

3. 使用下面的命令集將叢集伺服器新增至叢集集合：

    ```PowerShell
    Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
    Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2
    ```

   > [!NOTE]
   > 如果您使用的是靜態 IP 位址配置，則必須在 **ClusterSet** 命令上包含 *-StaticAddress x* . x. x. x. x. x. x. x. x. x. x。

4. 當您建立叢集成員的叢集集合之後，您可以列出已設定的節點和其屬性。 列舉叢集中的所有成員叢集：

    ```PowerShell
    Get-ClusterSetMember -CimSession CSMASTER
    ```

5. 列舉叢集集中的所有成員叢集，包括管理叢集節點：

    ```PowerShell
    Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode
    ```

6. 若要列出成員叢集中的所有節點：

    ```PowerShell
    Get-ClusterSetNode -CimSession CSMASTER
    ```

7. 列出整個叢集集的所有資源群組：

    ```PowerShell
    Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup
    ```

8. 若要確認叢集集建立程式建立了一個 SMB 共用 (識別為 Volume1，或有任何標記為的 CSV 資料夾，ScopeName 是基礎結構檔案伺服器名稱，以及在基礎結構 SOFS 上為每個叢集成員 CSV 磁片區) 的路徑：

    ```PowerShell
    Get-SmbShare -CimSession CSMASTER
    ```

9. 您可以收集叢集設定的偵錯工具記錄以供審查。 您可以針對所有成員和管理叢集收集叢集設定和叢集的偵錯工具記錄：

    ```PowerShell
    Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>
    ```

10. 設定所有叢集集合成員之間的 Kerberos [限制委派](https://techcommunity.microsoft.com/t5/virtualization/live-migration-via-constrained-delegation-with-kerberos-in/ba-p/382334) 。

11. 在叢集集合中的每個節點上，將 [跨叢集虛擬機器的即時移轉驗證類型] 設定為 [Kerberos]。

    ```PowerShell
    foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }
    ```

12. 將管理叢集新增至叢集集中每個節點上的本機系統管理員群組。

    ```PowerShell
    foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }
    ```

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>建立新的虛擬機器並新增至叢集集

建立叢集集之後，下一步是建立新的虛擬機器。 一般來說，當您建立虛擬機器並將它們新增至叢集時，您必須在叢集上執行一些檢查，以查看可能最好執行的動作。 這些檢查可能包括：

- 叢集節點上有多少記憶體可供使用？
- 叢集節點上有多少可用的磁碟空間？
- 虛擬機器需要特定的存放裝置需求 (也就是我想要讓 SQL Server 的虛擬機器移至執行速度更快的叢集;或者，我的基礎結構虛擬機器並不重要，而且可以在) 的較慢磁片磁碟機上執行。

一旦此問題獲得解答，您就可以在您需要的叢集上建立虛擬機器。 叢集集的優點之一，就是叢集集會為您進行這些檢查，然後將虛擬機器放在最理想的節點上。

下列命令會識別最佳的叢集，並將虛擬機器部署到其中。 在下列範例中，會建立新的虛擬機器，指定虛擬機器至少有 4 gb 的記憶體，而且需要利用1個虛擬處理器。

- 確認虛擬機器可使用4gb
- 設定用於1的虛擬處理器
- 檢查以確定至少有10% 的 CPU 可供虛擬機器使用

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

完成時，系統將會提供虛擬機器的相關資訊，以及放置該虛擬機器的位置。 在上述範例中，它會顯示為：

```
State         : Running
ComputerName  : 1-S2D2
```

如果您的記憶體、CPU 容量或磁碟空間不足，無法新增虛擬機器，您將會收到下列錯誤：

```
Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.
```

建立虛擬機器之後，它會顯示在指定的特定節點上的 [Hyper-v 管理員] 中。 若要將它新增為叢集集虛擬機器，並將它新增至叢集，請使用下列命令：

```PowerShell
Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1
```

完成時，輸出將會是：

```
Id  VMName  State  MemberName  PSComputerName
--  ------  -----  ----------  --------------
 1  CSVM1     On   CLUSTER1    CSMASTER
```

如果您已將叢集新增至現有的虛擬機器，則虛擬機器也必須向叢集集合註冊。 若要一次註冊所有這些虛擬機器，請使用下列命令：

```PowerShell
Get-ClusterSetMember -Name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER
```

不過，此程式尚未完成，因為必須將虛擬機器的路徑新增至叢集組命名空間。

例如：新增現有的叢集，而且它已預先設定的虛擬機器位於本機叢集共用磁碟區 (CSV) 。 VHDX 的路徑會類似于「C:\ClusterStorage\Volume1\MYVM\Virtual Hard Disks\MYVM.vhdx」。 儲存體遷移需要完成，因為 CSV 路徑是由單一成員叢集的本機設計所提供，因此當虛擬機器在成員叢集之間進行即時移轉時，將無法存取該虛擬機器。

在此範例中，CLUSTER3 已新增至使用 Add-ClusterSetMember 搭配基礎結構 Scale-Out 檔案伺服器的叢集設定為 SOFS CLUSTER3。 若要移動虛擬機器設定和存放裝置，命令為：

```PowerShell
Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM
```

完成之後，您會收到一則警告：

```
WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running. For more information view the report file below.
WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.
```

您可以忽略此警告，因為警告為「偵測到虛擬機器角色存放裝置設定中沒有任何變更」。 因為實際的實體位置不會變更，所以警告的原因為：僅限設定路徑。

如需有關移動-Move-vmstorage 的詳細資訊，請參閱此 [連結](/powershell/module/hyper-v/move-vmstorage)。

在不同叢集集叢集之間即時移轉虛擬機器與過去不同。 在非叢集設定案例中，步驟如下：

1. 從叢集中移除虛擬機器角色。
2. 即時將虛擬機器遷移至不同叢集的成員節點。
3. 將虛擬機器新增至叢集中作為新的虛擬機器角色。

使用叢集設定時，不需要這些步驟，而且只需要一個命令。 首先，您應該使用下列命令，將所有網路設定為可供遷移：

```PowerShell
Set-VMHost -UseAnyNetworkForMigration $true
```

例如，我想要將叢集集虛擬機器從 CLUSTER1 移至 CLUSTER3 上的節點 2-CL3。 單一命令會是：

```PowerShell
Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3
```

請注意，這不會移動虛擬機器存放裝置或設定檔。 這並不是必要的，因為虛擬機器的路徑維持為 \\ \\ SOFS-CLUSTER1\VOLUME1。 一旦虛擬機器已向叢集集合註冊之後，就會有基礎結構檔案伺服器共用路徑，磁片磁碟機和虛擬機器不需要與虛擬機器位於相同的電腦上。

## <a name="creating-availability-sets-fault-domains"></a>建立可用性設定組容錯網域

如同簡介中所述，Azure 的容錯網域和可用性設定組可以在叢集集合中設定。 這對於初始虛擬機器放置和叢集之間的遷移很有説明。

在下列範例中，有四個叢集參與叢集集。 在此集合中，將會建立具有兩個叢集的邏輯容錯網域，以及使用其他兩個叢集建立的容錯網域。 這兩個容錯網域將組成可用性集合。

在下列範例中，CLUSTER1 和 CLUSTER2 將會位於名為 **FD1** 的容錯網域中，而 CLUSTER3 和 CLUSTER4 將位於稱為 **FD2** 的容錯網域中。 可用性設定組會被稱為 **CSMASTER** ，並由兩個容錯網域所組成。

若要建立容錯網域，命令如下：

```PowerShell
New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"
```

為了確保已成功建立，Get-ClusterSetFaultDomain 可以執行，並顯示其輸出。

```PowerShell
PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

PSShowComputerName    : True
FaultDomainType       : Logical
ClusterName           : {CLUSTER1, CLUSTER2}
Description           : This is my first fault domain
FDName                : FD1
Id                    : 1
PSComputerName        : CSMASTER
```

現在已建立容錯網域，必須建立可用性設定組。

```PowerShell
New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2
```

若要驗證是否已建立，請使用：

```PowerShell
Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER
```

建立新的虛擬機器時，您必須使用-AvailabilitySet 參數做為判斷最佳節點的一部分。 這樣就會看起來像這樣：

```PowerShell
# Identify the optimal node to create a new virtual machine
$memoryinMB=4096
$vpcount = 1
$av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
$targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
$secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
$cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)
```

因為不同的生命週期，從叢集集中移除叢集。 有時候需要從叢集集合中移除叢集。 最佳做法是將所有叢集設定的虛擬機器移出叢集。 您可以使用 **移動 ClusterSetVM** 和 **move-vmstorage** 命令來完成這項作業。

但是，如果也不會移動虛擬機器，叢集集會執行一系列的動作，以提供直覺的結果給系統管理員。 從集合中移除叢集時，在要移除的叢集上所裝載的所有剩餘叢集組虛擬機器，將會變成與該叢集系結的高可用性虛擬機器，前提是它們可以存取其儲存體。 叢集集也會自動更新其清查方式：

- 不再追蹤立即移除的叢集和其上執行的虛擬機器的健全狀況
- 從叢集集合命名空間中移除，並從現在已移除的叢集上裝載的所有共用參考

例如，從叢集集移除 CLUSTER1 叢集的命令將會是：

```PowerShell
Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER
```

## <a name="frequently-asked-questions-faq"></a>常見問題集 (FAQ)

**問題：** 在我的叢集設定中，我僅限使用超交集叢集嗎？ <br>
**答：** 不會。 您可以使用傳統叢集來混合儲存空間直接存取。

**問題：** 我可以透過 System Center Virtual Machine Manager 管理叢集設定嗎？ <br>
**答：** System Center Virtual Machine Manager 目前不支援叢集集 <br><br> **問題：** Windows Server 2012 R2 或2016叢集是否可並存存在於相同的叢集集中？ <br>
**問題：** 我可以將工作負載從 Windows Server 2012 R2 或2016叢集遷移，只要讓這些叢集加入相同的叢集集就可以了嗎？ <br>
**答：** 叢集集是 Windows Server 2019 中引進的新技術，因此在舊版中並不存在。 下層以 OS 為基礎的叢集無法加入叢集集。 不過，叢集作業系統輪流升級技術應提供您所需的遷移功能，方法是將這些叢集升級至 Windows Server 2019。

**問題：** 叢集集是否可讓我調整儲存體或計算 (單獨) ？ <br>
**答：** 是，只要新增儲存空間直接存取或傳統的 Hyper-v 叢集即可。 有了叢集集，即使是在超交集的叢集集中，也是計算到儲存體比例的直接變更。

**問題：** 叢集集的管理工具是什麼 <br>
**答：** 此版本中的 PowerShell 或 WMI。

**問題：** 跨叢集的即時移轉如何與不同層代的處理器搭配運作？  <br>
**答：** 叢集集無法解決處理器差異，並取代 Hyper-v 目前支援的功能。 因此，處理器相容性模式必須與快速遷移一起使用。 叢集集的建議是在每個個別叢集內使用相同的處理器硬體，以及整個叢集集，以便在叢集之間進行即時移轉。

**問題：** 我的叢集設定虛擬機器是否可在叢集失敗時自動容錯移轉？  <br>
**答：** 在此版本中，叢集設定的虛擬機器只能在叢集間手動進行即時移轉;但是無法自動容錯移轉。

**問題：** 我們要如何確保儲存體對叢集失敗有復原能力？ <br>
**答：** 跨成員叢集使用跨叢集儲存體複本 (SR) 解決方案，以實現叢集失敗的儲存體復原。

**問題：** 我使用儲存體複本 (SR) 在成員叢集之間進行複寫。 在 SR 容錯移轉到複本目標儲存空間直接存取叢集時，叢集集合命名空間儲存 UNC 路徑是否有變更？ <br>
**答：** 在此版本中，SR-IOV 容錯移轉不會發生這類叢集組命名空間的參考變更。 請讓 Microsoft 知道此案例對您有重要性，以及您打算使用它的方式。

**問題：** 是否可以在嚴重損壞修復情況下容錯移轉容錯網域中的虛擬機器 (說整個容錯網域) ？ <br>
**答：** 否，請注意，尚未支援邏輯容錯網域內的跨叢集容錯移轉。

**問題：** 我的叢集設定可以跨多個網站中的叢集 (還是 DNS 網域) ？ <br>
**答：** 這是未經過測試的案例，並不會立即規劃生產環境的支援。 請讓 Microsoft 知道此案例對您有重要性，以及您打算使用它的方式。

**問題：** 叢集設定是否適用于 IPv6？ <br>
**答：** 使用容錯移轉叢集時，叢集集會支援 IPv4 和 IPv6。

**問題：** 叢集集的 Active Directory 樹系需求為何 <br>
**答：** 所有成員叢集都必須位於相同的 AD 樹系中。

**問題：** 有多少個叢集或節點可以成為單一叢集集的一部分？ <br>
**答：** 在 Windows Server 2019 中，已測試並支援最多64個叢集節點的叢集設定。 不過，叢集集架構會調整為較大的限制，而不是硬式編碼的某些限制。 請讓 Microsoft 知道如果規模較大，您的重要性，以及您打算使用它的方式。

**問題：** 叢集集中的所有儲存空間直接存取叢集都會形成單一儲存集區嗎？ <br>
**答：** 不會。 儲存空間直接存取技術仍在單一叢集內運作，而不是在叢集集中的成員叢集上運作。

**問題：** 叢集組命名空間是否具高可用性？ <br>
**答：** 是的，透過在管理叢集上執行的 (CA) 參考 SOFS 命名空間伺服器，可提供叢集集命名空間。 Microsoft 建議從成員叢集擁有足夠數量的虛擬機器，讓它能夠在當地語系化的全叢集失敗時復原。 不過，若要考慮未預期的重大失敗（例如，管理叢集中的所有虛擬機器都同時停止運作），則會在每個叢集集節點中另外持續快取參考資訊，甚至是在重新開機時。

**問題：** 叢集設定以命名空間為基礎的儲存體存取是否會使叢集集中的儲存體效能變慢？ <br>
**答：** 不會。 叢集集合命名空間會在叢集集合內提供重迭的參考命名空間，在概念上類似分散式檔案系統命名空間 (DFSN) 。 與 DFSN 不同的是，所有叢集集合命名空間的參考中繼資料會在所有節點上自動填入和自動更新，而不需要系統管理員介入，因此儲存體存取路徑幾乎不會有任何效能負擔。

**問題：** 如何備份叢集設定中繼資料？ <br>
**答：** 本指南與容錯移轉叢集相同。 系統狀態備份也會備份叢集狀態。 透過 Windows Server Backup，您只可以針對節點的叢集 (資料庫進行還原，因為我們已) 或執行授權還原，以在所有節點之間復原整個叢集資料庫，所以永遠不需要這項作業。 在叢集集合的情況下，Microsoft 建議先在成員叢集上進行這類授權還原，然後視需要進行管理叢集。
