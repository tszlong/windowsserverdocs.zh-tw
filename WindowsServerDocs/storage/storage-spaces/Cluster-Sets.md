---
title: 叢集集合
ms.prod: windows-server
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: 本文說明叢集集合案例
ms.localizationpriority: medium
ms.openlocfilehash: db427e8fa4e5574c6eb7837cf0ab4a9fcc180410
ms.sourcegitcommit: 3c3dfee8ada0083f97a58997d22d218a5d73b9c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639956"
---
# <a name="cluster-sets"></a>叢集集合

> 適用于： Windows Server 2019

叢集集是 Windows Server 2019 版本中的新雲端向外延展技術，可在單一軟體定義資料中心（SDDC）雲端中依等級的順序增加叢集節點計數。 叢集集是多個容錯移轉叢集的鬆散結合群組：計算、儲存體或超融合。 叢集集合技術可讓叢集集內的成員叢集與虛擬機器流動性間的統一儲存命名空間中的每個成員叢集進行虛擬機器流動性。

在成員叢集上保留現有的容錯移轉叢集管理體驗時，叢集集合實例還會在匯總時提供生命週期管理的重要使用案例。 此 Windows Server 2019 案例評估指南提供您所需的背景資訊，以及使用 PowerShell 評估叢集集合技術的逐步指示。

## <a name="technology-introduction"></a>技術簡介

叢集集技術的開發目的，是為了滿足特定客戶對大規模作業軟體定義資料中心（SDDC）雲端的要求。 叢集集合價值主張可能摘要如下：  

- 將多個較小的叢集結合成單一大型網狀架構，以大幅增加支援的 SDDC 雲端規模以執行高可用性虛擬機器，即使將軟體錯誤界限保留在單一叢集
- 透過流暢地跨此大型網狀架構遷移虛擬機器，來管理整個容錯移轉叢集生命週期，包括上線和淘汰叢集，而不會影響租使用者虛擬機器的可用性
- 在您的超融合 I 中輕鬆變更計算與儲存體的比率
- 在初始虛擬機器放置和後續的虛擬機器遷移中，從[類似 Azure 的容錯網域和可用性設定組](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)中獲益
- 混用不同的 CPU 硬體層代到相同的叢集集網狀架構中，即使是讓個別的容錯網域保持在最高效率也一樣。  請注意，相同硬體的建議仍然存在於每個個別的叢集，以及整個叢集集內。

從高階觀點來看，這是叢集集的樣子。

![叢集集解決方案視圖](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

以下提供上述影像中每個元素的快速摘要：

**管理叢集**

叢集集內的管理叢集是一個容錯移轉叢集，它會裝載整個叢集集的高可用性管理平面，以及整合存放裝置命名空間（叢集集命名空間）參考向外延展檔案伺服器（SOFS）。 管理叢集在邏輯上會與執行虛擬機器工作負載的成員叢集分離。 這可讓叢集集管理平面彈性地復原任何當地語系化的全叢集失敗，例如成員叢集的斷電。   

**成員叢集**

叢集集合中的成員叢集通常是執行虛擬機器和儲存空間直接存取工作負載的傳統超融合叢集。 多個成員叢集參與單一叢集集部署，形成較大型的 SDDC 雲端網狀架構。 成員叢集與管理叢集在兩個重要層面的差異：成員叢集參與容錯網域和可用性設定組結構，而成員叢集也會調整大小以裝載虛擬機器和儲存空間直接存取工作負載。 在叢集集合中跨叢集界限移動的叢集集虛擬機器，不得裝載于管理叢集上，原因如下。

**叢集集命名空間參考 SOFS**

叢集集命名空間參考（叢集集命名空間） SOFS 是向外延展檔案伺服器，叢集集命名空間 SOFS 上的每個 SMB 共用都是一種參考共用–在 Windows Server 2019 中新引進的 ' SimpleReferral ' 類型。 此參照可讓伺服器訊息區（SMB）用戶端存取成員叢集 SOFS 上主控的目標 SMB 共用。 叢集集命名空間的參考 SOFS 是輕量的參考機制，因此不會參與 i/o 路徑。 SMB 參考會在每個用戶端節點上進行快取永久，而叢集集合命名空間會視需要自動更新這些參考。

**叢集設定主機**

在叢集集合中，成員叢集之間的通訊會鬆散結合，並由稱為「叢集集主機」（CS-主機）的新叢集資源進行協調。 就像任何其他叢集資源，CS-主機具有高可用性，且可復原個別成員叢集失敗和/或管理叢集節點失敗。 透過新的叢集集 WMI 提供者，CS 主機提供所有叢集集管理能力互動的管理端點。

**叢集設定背景工作**

在叢集集合部署中，CS 主機會與名為「叢集集工作者」（CS-Worker）的成員叢集上的新叢集資源互動。 CS-Worker 作為叢集上唯一的聯絡者，以協調 CS 主機要求的本機叢集互動。 這類互動的範例包括虛擬機器放置和叢集本機資源清查。 叢集集合中的每個成員叢集都只有一個 CS 背景工作實例。 

**容錯網域**

容錯網域是一組軟體和硬體成品的群組，當發生失敗時，系統管理員判斷可能會一起失敗。  雖然系統管理員可以將一或多個叢集同時指定為容錯網域，但每個節點都可以參與可用性設定組中的容錯網域。 依設計的叢集集會將容錯網域界限判斷的決策留給具有資料中心拓撲考慮（例如，PDU、網路–該成員叢集共用）精通的系統管理員。 

**可用性設定組**

可用性設定組可協助系統管理員在容錯網域中設定所需的叢集工作負載複本，方法是將它們組織成可用性設定組，並將工作負載部署到該可用性設定組。 假設您要部署兩層式應用程式，建議您在可用性設定組中為每個階層設定至少兩部虛擬機器，以確保當該可用性設定組中的一個容錯網域停止運作時，您的應用程式至少會在相同可用性設定組的不同容錯網域上主控一部虛擬機器。

## <a name="why-use-cluster-sets"></a>為何要使用叢集集合

叢集集提供了調整的優點，而不犧牲復原功能。  

叢集集可讓您將多個叢集叢集在一起，以建立大型網狀架構，而每個叢集會保持獨立以提供復原功能。  例如，您有多個執行虛擬機器的4節點 HCI 叢集。  每個叢集都會提供本身所需的復原。  如果儲存體或記憶體開始填滿，則相應增加是您的下一個步驟。  在相應增加的情況下，有一些選項和考慮。

1. 將更多儲存體新增至目前的叢集。  有了儲存空間直接存取，這可能有點棘手，因為可能無法使用完全相同的模型/固件磁片磁碟機。  重建時間的考慮也必須納入考慮。
2. 擴充更多的記憶體。  如果您要超量而電腦可以處理的記憶體，該怎麼辦？  如果所有可用的記憶體位置都已滿，會發生什麼情況？
3. 將具有磁片磁碟機的其他計算節點新增至目前的叢集中。  這會讓我們回到需要考慮的選項1。
4. 購買全新的叢集

這是叢集集提供調整優勢的地方。  如果我將叢集新增到叢集集，我可以利用另一個叢集上可能提供的儲存體或記憶體，而不需要任何額外的購買。  從復原的觀點來看，將額外的節點新增至儲存空間直接存取並不會為仲裁提供額外的投票。  如[這裡](drive-symmetry-considerations.md)所述，儲存空間直接存取叢集在關閉前，可能會在遺失2個節點後存活。  如果您有4個節點的 HCI 叢集，則3個節點會停止執行整個叢集。  如果您有8個節點的叢集，則3個節點會停止執行整個叢集。  在集合中有兩個4節點 HCI 叢集的叢集集合中，一個 HCI 中的2個節點會關閉，而另一個 HCI 中的1個節點會停止運作，這兩個叢集都會保持運作。  最好是建立一個大型的16節點儲存空間直接存取叢集，或將它分成四個4節點的叢集並使用叢集集合？  有四個具有叢集集的4節點叢集會提供相同的規模，但更好的復原方式，因為多個計算節點可能會停止運作（非預期或進行維護），而生產環境仍然存在。

## <a name="considerations-for-deploying-cluster-sets"></a>部署叢集集的考慮

考慮到叢集集合是否為您需要使用的專案時，請考慮下列問題：

- 您是否需要超越目前的 HCI 計算和儲存體規模限制？
- 所有計算和儲存體的名稱都不相同嗎？
- 您是否在叢集之間即時移轉虛擬機器？
- 您是否想要在多個叢集之間進行類似 Azure 的電腦可用性設定組和容錯網域？
- 您需要花時間查看所有叢集，以判斷需要放置任何新虛擬機器的位置嗎？

如果您的答案是肯定的，則叢集集就是您所需的。

還有一些其他專案需要考慮，較大型的 SDDC 可能會變更您的整體資料中心策略。  SQL Server 是一個很好的例子。  在叢集之間移動 SQL Server 虛擬機器是否需要授權 SQL 才能在其他節點上執行？  

## <a name="scale-out-file-server-and-cluster-sets"></a>向外延展檔案伺服器和叢集集

在 Windows Server 2019 中，有一個新的向外延展檔案伺服器角色，稱為基礎結構向外延展檔案伺服器（SOFS）。 

下列考慮適用于基礎結構 SOFS 角色：

1.  容錯移轉叢集上最多隻能有一個基礎結構 SOFS 叢集角色。 基礎結構 SOFS 角色是藉由指定**ClusterScaleOutFileServerRole** Cmdlet 的「**基礎結構**」切換參數來建立。  例如，

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  在容錯移轉中建立的每個 CSV 磁片區都會根據 CSV 磁片區名稱，自動觸發建立具有自動產生之名稱的 SMB 共用。 除了透過 CSV 磁片區建立/修改作業以外，系統管理員無法直接在 SOFS 角色下建立或修改 SMB 共用。

3.  在超融合式設定中，基礎結構 SOFS 可讓 SMB 用戶端（Hyper-v 主機）與基礎結構 SOFS SMB 伺服器的保證持續可用性（CA）進行通訊。 這個超交集 SMB 回送 CA 是透過存取其虛擬磁片（VHDx）檔案的虛擬機器來達成，其中擁有的虛擬機器身分識別會在用戶端與伺服器之間轉送。 此身分識別轉送允許 ACL 的 VHDx 檔案，如同之前的標準超融合叢集設定。

建立叢集集之後，叢集集命名空間會依賴每個成員叢集上的基礎結構 SOFS，以及管理叢集中的基礎結構 SOFS。

當成員叢集新增到叢集集時，系統管理員會指定該叢集上 SOFS 的基礎結構名稱（如果已經存在的話）。 如果基礎結構 SOFS 不存在，此操作會建立新成員叢集上的新基礎結構 SOFS 角色。 如果成員叢集上已經有基礎結構 SOFS 角色，則「新增」作業會視需要隱含地將它重新命名為指定的名稱。 叢集集會保留成員叢集上任何現有的單一 SMB 伺服器或非基礎結構 SOFS 角色的未運用。 

建立叢集集時，系統管理員可以選擇使用已存在的 AD 電腦物件，做為管理叢集上的命名空間根。 叢集設定建立作業會在管理叢集上建立基礎結構 SOFS 叢集角色，或重新命名現有的基礎結構 SOFS 角色，如同先前針對成員叢集所述。 管理叢集上的基礎結構 SOFS 會用來做為叢集集命名空間參考（叢集集命名空間） SOFS。 這只是表示叢集集命名空間 SOFS 上的每個 SMB 共用都是「SimpleReferral」類型的參考共用，在 Windows Server 2019 中是新引進的。  此參照可讓 SMB 用戶端存取成員叢集 SOFS 上主控的目標 SMB 共用。 叢集集命名空間的參考 SOFS 是輕量的參考機制，因此不會參與 i/o 路徑。 SMB 參考會在每個用戶端節點上快取永久，而叢集集命名空間會視需要自動更新這些參照

## <a name="creating-a-cluster-set"></a>建立叢集集

### <a name="prerequisites"></a>必要條件

建立叢集集時，建議您遵循下列必要條件：

1. 設定執行 Windows Server 2019 的管理用戶端。
2. 在此管理伺服器上安裝容錯移轉叢集工具。
3. 建立叢集成員（至少兩個叢集中至少有兩個叢集共用磁片區）
4. 建立跨越成員叢集的管理叢集（實體或來賓）。  這種方法可確保即使有可能的成員叢集失敗，叢集集管理平面仍可繼續使用。

### <a name="steps"></a>步驟

1. 依照必要條件中的定義，從三個叢集建立新的叢集集。  下圖提供要建立的叢集範例。  在此範例中，叢集集的名稱將會是**CSMASTER**。

   | 叢集名稱               | 稍後要使用的基礎結構 SOFS 名稱 | 
   |----------------------------|-------------------------------------------|
   | 設定-叢集                | SOFS-CLUSTERSET                           |
   | CLUSTER1                   | SOFS-CLUSTER1                             |
   | CLUSTER2                   | SOFS-CLUSTER2                             |

2. 建立所有叢集之後，請使用下列命令來建立叢集集主機。

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. 若要將叢集伺服器新增到叢集集，將會使用以下的。

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > 如果您使用靜態 IP 位址配置，您必須在**ClusterSet**命令上包含 *-StaticAddress x* . x. x。

4. 建立叢集成員的叢集集之後，您可以列出節點集及其屬性。  若要列舉叢集集合中的所有成員叢集：

        Get-ClusterSetMember -CimSession CSMASTER

5. 列舉叢集集合中的所有成員叢集，包括管理叢集節點：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. 若要列出成員叢集中的所有節點：

        Get-ClusterSetNode -CimSession CSMASTER

7. 若要列出整個叢集集的所有資源群組：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. 若要確認叢集集建立程式是否已建立一個 SMB 共用（識別為 Volume1，或在每個叢集成員的 CSV 磁片區的基礎結構 SOFS 上，將 ScopeName 標示為基礎結構檔案伺服器的名稱，以及兩者的路徑）：

        Get-SmbShare -CimSession CSMASTER

8. 叢集集具有可收集來進行審查的偵錯工具記錄。  您可以為所有成員和管理叢集收集叢集集和叢集偵錯工具記錄。

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. 設定所有叢集集合成員之間的 Kerberos[限制委派](https://techcommunity.microsoft.com/t5/virtualization/live-migration-via-constrained-delegation-with-kerberos-in/ba-p/382334)。

10. 在叢集集合中的每個節點上，將跨叢集虛擬機器即時移轉驗證類型設定為 Kerberos。

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. 將管理叢集新增至叢集集合中每個節點上的本機 administrators 群組。

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>建立新的虛擬機器並新增至叢集集合

建立叢集集之後，下一個步驟是建立新的虛擬機器。  一般來說，當您建立虛擬機器並將它們新增到叢集時，您必須在叢集上執行一些檢查，以查看可能在上執行的最佳方式。  這些檢查可能包括：

- 叢集節點上有多少記憶體可供使用？
- 叢集節點上有多少可用的磁碟空間？
- 虛擬機器需要特定的儲存需求（也就是我想要讓 SQL Server 的虛擬機器移至執行速度較快的叢集; 或者，我的基礎結構虛擬機器並不重要，而且可以在較慢的磁片磁碟機上執行）。

當此問題獲得解答之後，您就可以在需要的叢集上建立虛擬機器。  叢集集的其中一個優點是，叢集集會為您執行這些檢查，並將虛擬機器放置在最適合的節點上。

下列命令會識別最佳的叢集，並將虛擬機器部署至該叢集。  在下列範例中，會建立新的虛擬機器，指定至少有 4 gb 的記憶體可供虛擬機器使用，而且它將需要使用1個虛擬處理器。

- 確定虛擬機器可使用4gb
- 將使用的虛擬處理器設定為1
- 檢查以確定虛擬機器至少有10% 的 CPU 可用

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

當它完成時，將會提供虛擬機器的相關資訊及其放置位置。  在上述範例中，它會顯示為：

        State         : Running
        ComputerName  : 1-S2D2

如果您的記憶體、cpu 或磁碟空間不足，無法新增虛擬機器，您將會收到下列錯誤：

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

建立虛擬機器之後，它就會顯示在指定的特定節點上的 [Hyper-v 管理員] 中。  若要將它新增為叢集集虛擬機器和叢集，請使用下列命令。  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

當它完成時，輸出將會是：

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

如果您已新增具有現有虛擬機器的叢集，則虛擬機器也必須向叢集集合註冊，因此請一次註冊所有虛擬機器，使用的命令為：

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

不過，此程式並未完成，因為虛擬機器的路徑必須新增至叢集集命名空間。

例如，已新增現有的叢集，且其預先設定的虛擬機器位於本機叢集共用磁碟區（CSV）上，VHDX 的路徑會類似于 "C:\ClusterStorage\Volume1\MYVM\Virtual Hard Disks\MYVM.vhdx。  儲存體遷移需要完成，因為 CSV 路徑是由單一成員叢集的本機設計所組成。 如此一來，當虛擬機器在成員叢集上進行即時移轉之後，就無法存取它們。 

在此範例中，CLUSTER3 已使用 ClusterSetMember 作為基礎結構向外延展檔案伺服器新增至叢集集，做為 SOFS-CLUSTER3。  若要移動虛擬機器設定和存放裝置，命令為：

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

完成後，您會收到警告：

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

您可以忽略此警告，因為警告是「偵測不到虛擬機器角色存放裝置設定中的變更」。  警告的原因是實際實體位置不會變更;僅限設定路徑。 

如需有關移動 VMStorage 的詳細資訊，請參閱此[連結](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps)。 

在不同叢集集叢集之間即時移轉虛擬機器的方式與過去的相同。 在非叢集集案例中，步驟如下：

1. 從叢集移除虛擬機器角色。
2. 將虛擬機器即時移轉至不同叢集的成員節點。
3. 將虛擬機器新增至叢集中，做為新的虛擬機器角色。

使用叢集設定時，不需要執行這些步驟，而且只需要一個命令。  首先，您應該使用命令將所有網路設定為可供遷移：

    Set-VMHost -UseAnyNetworkForMigration $true

例如，我想要將叢集集虛擬機器從 CLUSTER1 移至 CLUSTER3 上的節點 2-CL3。  單一命令會是：

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

請注意，這不會移動虛擬機器存放裝置或設定檔。  這不是必要的，因為虛擬機器的路徑會維持 \\SOFS-CLUSTER1\VOLUME1。  當虛擬機器已向叢集集合註冊基礎結構檔案伺服器共用路徑後，磁片磁碟機和虛擬機器就不需要與虛擬機器位於相同的電腦上。

## <a name="creating-availability-sets-fault-domains"></a>建立可用性設定組容錯網域

如簡介中所述，您可以在叢集集合中設定類似 Azure 的容錯網域和可用性設定組。  這對於初始虛擬機器放置和叢集之間的遷移非常有用。  

在下列範例中，有四個叢集參與叢集集。  在此集合中，將會建立具有兩個叢集的邏輯容錯網域，以及使用其他兩個叢集所建立的容錯網域。  這兩個容錯網域會組成可用性集。 

在下列範例中，CLUSTER1 和 CLUSTER2 會位於名為**FD1**的容錯網域中，而 CLUSTER3 和 CLUSTER4 將位於稱為**FD2**的容錯網域中。  可用性設定組將稱為**CSMASTER** ，並由兩個容錯網域所組成。

若要建立容錯網域，命令為：

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

為確保已成功建立它們，ClusterSetFaultDomain 可以執行，並顯示其輸出。

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

既然已建立容錯網域，就必須建立可用性設定組。

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

若要驗證它是否已建立，請使用：

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

建立新的虛擬機器時，您必須使用-AvailabilitySet 參數做為判斷最佳節點的一部分。  因此，它會看起來像這樣：

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

因為各種生命週期而從叢集集移除叢集。 有時候，叢集必須從叢集集合中移除。 最佳做法是將所有叢集集虛擬機器移出叢集。 您可以使用**ClusterSetVM**和**VMStorage**命令來完成這項作業。

不過，如果虛擬機器也不會移動，叢集集會執行一系列的動作，為系統管理員提供直覺的結果。  從集合中移除叢集時，所移除的所有剩餘叢集集虛擬機器都會變成系結至該叢集的高可用性虛擬機器，前提是它們可以存取其儲存體。  叢集集合也會自動更新其清查，方法如下：

- 不再追蹤現在已移除之叢集的健全狀況，以及在其上執行的虛擬機器
- 從叢集集命名空間移除，並從現在已移除的叢集上裝載的所有共用參考

例如，從叢集集移除 CLUSTER1 叢集的命令會是：

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>常見問題集 (FAQ)

**問題：** 在我的叢集集合中，我是否僅限於使用超融合叢集？ <br>
**答：** 不。  您可以使用傳統叢集來混合儲存空間直接存取。

**問題：** 我可以透過 System Center Virtual Machine Manager 來管理叢集集嗎？ <br>
**答：** System Center Virtual Machine Manager 目前不支援叢集集 <br><br> **問題：** Windows Server 2012 R2 或2016叢集是否可以並存存在於相同的叢集集合中？ <br>
**問題：** 我是否可以將工作負載從 Windows Server 2012 R2 或2016叢集遷移，只要讓這些叢集加入相同的叢集集即可？ <br>
**答：** 叢集集是 Windows Server 2019 引進的新技術，因此在舊版中並不存在。 下層 OS 型叢集無法聯結叢集集。 不過，叢集作業系統輪流升級技術應該提供您所需的遷移功能，方法是將這些叢集升級至 Windows Server 2019。

**問題：** 叢集集可以讓我調整儲存體或計算（單獨）嗎？ <br>
**答：** 可以，只是新增儲存空間直接存取或傳統 Hyper-v 叢集。 使用叢集集時，即使是在超交集叢集集合中，也會直接變更計算對儲存體的比率。

**問題：** 叢集集的管理工具為何 <br>
**答：** PowerShell 或 WMI 在此版本中。

**問題：** 跨叢集即時移轉如何處理不同世代的處理器？  <br>
**答：** 叢集集無法解決處理器差異，並取代 Hyper-v 目前支援的功能。  因此，處理器相容性模式必須與快速遷移搭配使用。  叢集集的建議是在每個個別叢集內使用相同的處理器硬體，以及在要進行叢集間即時移轉的整個叢集集。

**問題：** 我的叢集設定虛擬機器是否可以在叢集失敗時自動容錯移轉？  <br>
**答：** 在此版本中，叢集設定虛擬機器只能以手動方式跨叢集進行即時移轉;但無法自動容錯移轉。 

**問題：** 我們要如何確保存放裝置對於叢集失敗有彈性？ <br>
**答：** 跨成員叢集使用跨叢集儲存體複本（SR）解決方案，以實現叢集失敗的儲存體恢復功能。

**問題：** 我使用儲存體複本（SR）在成員叢集之間進行複寫。 在 SR 容錯移轉到複本目標儲存空間直接存取叢集上，叢集集命名空間儲存體 UNC 路徑是否變更？ <br>
**答：** 在此版本中，SR 容錯移轉不會發生這樣的叢集集命名空間參考變更。 請讓 Microsoft 知道此案例對您以及您計畫如何使用這種情況十分重要。

**問題：** 是否可以在嚴重損壞修復的情況下容錯移轉跨容錯網域的虛擬機器（假設整個容錯網域已關閉）？ <br>
**答：** 否，請注意，尚未支援邏輯容錯網域內的跨叢集容錯移轉。 

**問題：** 我的叢集集是否可以跨越多個網站（或 DNS 網域）中的叢集？ <br> 
**解答：** 這是未經測試的案例，不會立即針對生產環境支援進行規劃。 請讓 Microsoft 知道此案例對您以及您計畫如何使用這種情況十分重要。

**問題：** 叢集集是否與 IPv6 搭配使用？ <br>
**答：** 叢集集和容錯移轉叢集都支援 IPv4 和 IPv6。

**問題：** 叢集集的 Active Directory 樹系需求為何 <br>
**答：** 所有成員叢集都必須位於相同的 AD 樹系中。

**問題：** 有多少叢集或節點可以是單一叢集集的一部分？ <br>
**答：** 在 Windows Server 2019 中，已測試並支援最多64個叢集節點的叢集集。 不過，叢集集架構會調整為更大的限制，而不是硬式編碼的限制。 請讓 Microsoft 知道，如果規模較大，您和計畫使用的方式都很重要。

**問題：** 叢集集合中的所有儲存空間直接存取叢集都會形成單一存放集區嗎？ <br>
**答：** 不。 儲存空間直接存取技術仍然會在單一叢集內運作，而不是在叢集集合中的成員叢集。

**問題：** 叢集集合命名空間是否具有高可用性？ <br>
**答：** 是，叢集集命名空間是透過在管理叢集上執行的持續可用（CA）參照 SOFS 命名空間伺服器提供。 Microsoft 建議從成員叢集擁有足夠數量的虛擬機器，讓它能夠從當地語系化的全叢集失敗中復原。 不過，為了說明未預期的重大失敗（例如，管理叢集中的所有虛擬機器同時關閉），會在每個叢集集節點中額外持續快取參照資訊，甚至是在重新開機時。
 
**問題：** 叢集設定以命名空間為基礎的存放裝置存取會使叢集集合中的存放裝置效能變慢嗎？ <br>
**答：** 不。 叢集集命名空間提供叢集集合內的重迭參考命名空間–概念上就像分散式檔案系統命名空間（DFSN）。 與 DFSN 不同的是，所有叢集集命名空間的參考中繼資料都會自動填入並自動更新所有節點，而不需要系統管理員介入，因此在儲存體存取路徑中幾乎不會產生任何效能負擔。 

**問題：** 如何備份叢集設定中繼資料？ <br>
**答：** 此指導方針與容錯移轉叢集相同。 系統狀態備份也會備份叢集狀態。  透過 Windows Server Backup，您可以只還原節點的叢集資料庫（因為我們有一堆自我修復邏輯，所以絕對不需要這麼做），或執行授權還原，在所有節點上回複整個叢集資料庫。 就叢集集而言，Microsoft 建議先在成員叢集上進行這類的授權還原，然後再進行管理叢集（如有需要）。
