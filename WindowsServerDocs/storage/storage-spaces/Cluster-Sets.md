---
title: 叢集集合
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: 本文章說明叢集集合案例
ms.localizationpriority: medium
ms.openlocfilehash: 2deeb6968f910e80bacb2354ad2e575060a7797a
ms.sourcegitcommit: 07aefbdbb0eedb42aaed3d195c2429310c761da0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/31/2019
ms.locfileid: "9041004"
---
# 叢集集合

> 適用於： Windows Server Insider Preview 組建 17650 及更新版本

叢集集合是單一的軟體定義資料中心 (SDDC) 雲端中的叢集節點計數增加數量級此預覽版的新雲端向外延展技術。 叢集組是多個容錯移轉叢集的鬆散結合群組： 計算、 儲存或超融合式。 叢集設定技術可讓虛擬機器流暢度跨內叢集設定與統一的儲存空間的命名空間的成員叢集之間的虛擬機器流暢度支援。 

雖然保留現有的容錯移轉叢集管理體驗成員叢集上，叢集集合執行個體此外提供周圍彙總在生命週期管理金鑰的使用案例。 這個 Windows Server 預覽案例評估指南提供必要的背景資訊，以及評估叢集設定技術，使用 PowerShell 的逐步指示。 

## 技術簡介

叢集集合的技術是以符合作業系統的縮放比例的軟體定義資料中心 (SDDC) 雲朵的特定客戶要求開發。 叢集集合價值主張，在此預覽版本可能會摘要如下所示：  

- 大幅增加支援的 SDDC 雲端規模，用來結合多個較小的叢集成單一的大型網狀架構，即使在單一叢集中保持軟體容錯界限時執行高可用性的虛擬機器
- 管理容錯移轉叢集生命週期，包括上架，以及淘汰叢集，而不會影響租用戶虛擬機器的可用性，透過流暢地移轉的整個跨此大型網狀架構的虛擬機器
- 在您超交集的計算來儲存比例輕易的變更我
- 在初始虛擬機器的位置和後續的虛擬機器移轉叢集跨受益於[類似 Azure 的容錯網域和可用性設定](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)
- 混合和比對不同世代的 CPU 硬體到相同的叢集設定網狀架構，即使同時保持個別的容錯網域同質的最大效率。  請注意建議動作的相同硬體仍會出現在每個個別的叢集，以及在整個叢集集合內。

從高階檢視中，這是集可以看起來像哪些叢集。

![叢集設定方案檢視](media\Cluster-sets-Overview\Cluster-sets-solution-View.png)

以下提供每一個上述的映像中元素的快速的摘要：

**管理叢集**

在叢集中管理叢集是容錯移轉叢集裝載高可用性管理平面為方向的整個叢集設定與統一的儲存體命名空間 （叢集設定命名空間） 轉介向外延展檔案伺服器 (SOFS)。 管理叢集是從執行虛擬機器工作負載的成員叢集邏輯分離。 這會讓叢集中設定管理平面具彈性任何當地語系化的整個叢集的失敗，例如遺失的成員叢集的強大功能。   

**成員叢集**

在叢集中成員叢集通常是執行虛擬機器和儲存空間直接存取的工作負載的傳統超融合式叢集。 多個成員叢集參與單一叢集設定部署，以形成較大的 SDDC 雲端網狀架構。 成員叢集不同之處管理叢集在兩個主要層面： 參與容錯網域的成員叢集和可用性設定建構，以及叢集成員也會調整大小對主機的虛擬機器及儲存空間直接存取的工作負載。 在叢集中的叢集界限上移動的叢集設定虛擬機器不必須裝載管理叢集上基於這個原因。

**叢集設定命名空間轉介 SOFS**

叢集設定命名空間轉介 （叢集設定命名空間中） SOFS 是移位，其中每個 SMB 共用上的叢集設定命名空間 SOFS 是轉介的共用資料夾 – 類型 'SimpleReferral' 新導入此預覽版本中的向外延展檔案伺服器。  這個轉介允許伺服器訊息區 (SMB) 的用戶端存取成員叢集 SOFS 上裝載 SMB 共用的目標。 叢集設定中的 I/O 路徑的命名空間轉介 SOFS 是輕量轉介機制，因此，不會不參與。 在每個用戶端節點上的 「 SMB 推薦會快取狀態而叢集設定命名空間動態自動更新這些轉介視。

**設定主要的叢集**

在叢集集合中，成員叢集之間的通訊鬆散結合，並透過稱為 「 叢集設定主機 」 （主要 CS） 的新叢集資源協調。 如同任何其他叢集資源 CS 主要是高可用且更具彈性的個別成員叢集失敗和/或管理叢集節點失敗。 透過新的叢集設定 WMI 提供者，CS 主要會提供所有叢集設定的管理性互動的管理端點。

**叢集設定背景工作**

在叢集中設定部署中，CS 主要互動新成員稱為 「 叢集設定背景工作 」 （CS 背景工作） 的叢集上叢集資源。 CS 背景工作會做為要求的 CS 主要協調的本機叢集互動叢集上唯一的橋樑。 這類互動的範例包括虛擬機器放置和清查叢集本機資源。 沒有只有一個 CS 背景工作執行個體成員的每個叢集中的叢集。 

**容錯網域**

容錯網域是最群組的軟體和硬體成品，系統管理員決定可能會失敗一起發生失敗時。  系統管理員可以一起指定一或多個叢集，做為容錯網域，雖然每個節點可以參與可用性集合中的容錯網域。 叢集設定樹葉設計決策的容錯網域界限判斷來是與資料中心拓撲考量例如 PDU 良好部分的系統管理員網路功能 – 成員叢集，共用。 

**可用性設定**

可用性組可協助系統管理員設定的叢集工作負載所需的備援整個容錯網域，並部署到該可用性集的工作負載，藉以那些成可用性。 比方說是否您正在部署兩層式應用程式，我們建議您針對每個層，這將能確保可用性集合中的一個容錯網域移往下，您的應用程式至少必須設定可用性設定至少兩個虛擬機器設定一部虛擬機器中裝載於該相同可用性的不同的容錯網域上的每一層。

## 為什麼要使用叢集集合

叢集集合提供縮放比例的優勢，同時不犧牲復原。  

叢集集合可讓多個叢集的叢集一起來建立大型的網狀架構，而每個叢集會保持獨立的復原類型。  例如，您有數個的 4 節點 HCI 叢集執行虛擬機器。  每個叢集提供復原功能所需的本身。  儲存空間或記憶體啟動時填滿，相應增加時，您的下一個步驟。  相應增加，有一些選項和考量。

1. 將更多的存放裝置新增至目前的叢集。  使用儲存空間直接存取，這可能會需要一些技巧時完全相同的模型/韌體磁碟機可能無法使用。  重新建置次數的考量也需要納入考量。
2. 新增更多的記憶體。  如果您被超量上的電腦可以處理的記憶體呢？  如果所有的可用記憶體插槽為完整嗎？
3. 新增額外的計算節點到目前的叢集磁碟機。  這會帶我們回到選項 1 會被視為需要。
4. 購買全新的叢集

這是叢集集合其中提供縮放比例的優勢。  如果我新增到叢集組我叢集，我可以充分利用存放區或可能不需要任何其他購買的另一個叢集上可用的記憶體。  復原的觀點而言，將其他節點新增至儲存空間直接存取不會提供額外票數仲裁。  做為所述在[這裡](drive-symmetry-considerations.md)，儲存空間直接存取叢集可以繼續之前支撐 2 個節點的遺失。  如果您有 4 雙節點 HCI 叢集，3 個節點移往下將會關閉整個叢集。  如果您有 8 節點的叢集，3 個節點移往下將會關閉整個叢集。  有兩個 4 雙節點 HCI 叢集集合中的叢集集合，向下一個 HCI 中的 2 個節點和 1 \] 節點中其他 HCI 移往下，這兩個叢集維持向上。  更清楚地建立一個大型的 16 雙節點儲存空間直接存取叢集或向下分成四個 4 個節點的叢集和使用叢集集合是？  有四個 4 個節點的叢集，叢集集合可讓使用相同的縮放比例，但更好的復原能力，因為多個計算節點可以向下 （意外或以進行維護） 和生產保持。

## 叢集集合的部署考量

當考慮叢集集合您需要使用的項目，請考量下列問題：

- 您需要超越目前的 HCI 運算和儲存縮放比例限制嗎？
- 並非所有的運算和儲存相同相同？
- 您即時移轉叢集之間的虛擬機器嗎？
- 您想類似 Azure 的電腦可用性組 」 和 「 容錯網域跨多個叢集嗎？
- 您需要花點時間看看您的所有叢集來判斷其中任何新的虛擬機器需要放嗎？

如果您的答案是，叢集集合是會以您所需的項目。

有幾個其他項目，請考慮此處較大的 SDDC 可能會變更您的整體資料中心策略。  SQL Server 是一個很好的範例。  需要授權額外的節點上執行 SQL 移動叢集之間的 SQL Server 虛擬機器？  

## 向外延展檔案伺服器和叢集集合

在 Windows Server 2019，沒有新的向外延展檔案伺服器角色，呼叫基礎結構向外延展檔案伺服器 (SOFS)。 

下列考量將套用到了基礎結構 SOFS 的角色：

1.  容錯移轉叢集上，可能會有最多只能有一個基礎結構 SOFS 叢集角色。 藉由指定也會建立基礎結構 SOFS 角色 」**-基礎結構**「**新增 ClusterScaleOutFileServerRole** cmdlet 來切換參數。  例如：

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  自動建立容錯移轉的每個 CSV 磁碟區會觸發建立 SMB 共用 CSV 磁碟區名稱為基礎的自動產生的名稱。 系統管理員無法直接建立或修改了 SOFS 的角色，其他比透過 CSV 磁碟區建立/修改作業底下的 SMB 共用。

3.  在超融合組態中，基礎結構 SOFS 可讓 SMB 用戶端 （HYPER-V 主機） 來傳達與保證持續可用性 (CA) 基礎結構 SOFS SMB 伺服器。 此超融合式 SMB 回送 CA 是透過虛擬機器存取其位置的擁有者的虛擬機器身分識別轉送用戶端與伺服器之間的虛擬磁碟 (VHDx) 檔案來達成。 此身分識別轉送可讓您將設定做為標準的超融合式叢集之前一樣的 ACL 來執行此 VHDx 檔案。

一旦建立叢集集合，叢集設定命名空間依賴上每個成員叢集基礎結構 SOFS 和此外管理叢集中的基礎結構 SOFS。

在時間成員叢集會新增到叢集集合中，系統管理員該叢集上指定的基礎結構 SOFS 名稱已經存在。 如果基礎結構 SOFS 不存在，這項作業會建立新的基礎結構 SOFS 角色，新成員叢集上。 如果成員叢集上已經有了基礎結構 SOFS 的角色，新增作業以隱含方式重新命名它指定名稱視。 任何現有的單一執行個體 SMB 伺服器或非基礎結構 SOFS unutilized 叢集設定所設定的成員會保留叢集上的角色。 

在建立叢集集合時，系統管理員已選擇使用現有的 AD 電腦物件做為命名空間根目錄上管理叢集。 叢集設定建立作業管理叢集上建立基礎結構 SOFS 叢集角色，或重新命名現有的基礎結構 SOFS 角色只是先前為叢集成員中所述。 管理叢集上基礎結構 SOFS 會使用如叢集設定命名空間 （叢集設定命名空間中） SOFS 的轉介。 它只是表示每個 SMB 共用上的叢集設定 SOFS 是新導入此預覽版本中的轉介共用 – 的類型 'SimpleReferral'-命名空間。  這個轉介可讓到目標 SMB 共用的 SMB 用戶端存取裝載成員叢集 SOFS 上。 叢集設定中的 I/O 路徑的命名空間轉介 SOFS 是輕量轉介機制，因此，不會不參與。 每個用戶端節點上的 「 SMB 推薦會快取狀態和叢集設定命名空間會動態更新自動這些轉介視

## 建立叢集集合

### 必要條件

建立叢集設定，當您依照必要條件建議：

1. 設定執行最新的 Windows Server 測試人員版本管理用戶端。
2. 此管理伺服器上安裝的容錯移轉叢集的工具。
3. 建立叢集成員 （至少兩個叢集至少兩個叢集共用磁碟區與每個叢集上）
4. 建立跨立成員叢集管理叢集 （實體或來賓）。  這種方式可確保叢集集合，可供使用，儘管可能成員叢集失敗的管理平面會繼續。

### 步驟

1. 建立新的叢集設定從三個叢集必要條件中所定義。  下面圖表提供建立叢集的範例。  在此範例中設定的叢集名稱將會是**CSMASTER**。

   | 叢集名稱               | 以供稍後基礎結構 SOFS 名稱 | 
   |----------------------------|-------------------------------------------|
   | 設定叢集                | SOFS CLUSTERSET                           |
   | CLUSTER1                   | SOFS CLUSTER1                             |
   | CLUSTER2                   | SOFS CLUSTER2                             |

2. 只要所有叢集都建立之後，使用下列命令來建立叢集設定主機。

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. 將叢集伺服器新增至叢集集合，以下可能會用到。

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > 如果您使用靜態 IP 位址配置，您必須包含 *-StaticAddress x.x.x.x*上**新增 ClusterSet**命令。

4. 一旦您已建立叢集設定叢集成員退出，您可以列出節點組和其屬性。  若要列舉的叢集中的所有成員叢集：

        Get-ClusterSetMember -CimSession CSMASTER

5. 若要列舉設定包括管理叢集節點叢集中的所有成員叢集：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. 若要列出從成員叢集的所有節點：

        Get-ClusterSetNode -CimSession CSMASTER

7. 若要列出所有整個叢集的資源群組設定：

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. 若要確認叢集建立程序，建立一個 （識別為 Volume1 或正在的基礎結構的檔案伺服器和同時作為路徑名稱 ScopeName 標示任何 CSV 資料夾） 的 SMB 共用上設定基礎結構 SOFS 針對每個叢集成員的CSV 磁碟區：

        Get-SmbShare -CimSession CSMASTER

8. 叢集集合具有可收集供檢閱的偵錯記錄檔。  設定叢集和叢集偵錯記錄檔可以收集的所有成員及管理叢集。

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. 設定 Kerberos 之間所有的叢集設定成員 [[限制委派](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/)。

10. 在叢集中設定每個節點上設定 Kerberos 跨叢集虛擬機器即時移轉驗證類型。

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. 加入叢集集合中每個節點上的本機系統管理員群組管理叢集。

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## 建立新的虛擬機器並加入至叢集集合

建立叢集設定之後下, 一個步驟是建立新的虛擬機器。  一般而言，當建立虛擬機器，並將它們新增至叢集時，您需要執行某些檢查，以查看它可能是最佳上執行叢集上。  這些檢查可能包含：

- 多少記憶體是在叢集節點上可用？
- 在叢集節點上有多少磁碟空間？
- 虛擬機器不需要特定的儲存空間需求 （也就是我想要以移至叢集中執行較快的磁碟機; 我 SQL Server 的虛擬機器，或是我基礎結構的虛擬機器不是那麼重要，而且可以較慢的磁碟機上執行）。

一旦會回答此問題，您會需要它能夠在叢集上建立虛擬機器。  其中一個叢集集合的優點是這些檢查會為您做到的叢集集合，並將虛擬機器放在最佳的節點上。

以下命令會同時找出最佳的叢集並部署虛擬機器到它。  在以下範例中，新的虛擬機器會建立指定至少 4 gb 的記憶體適用於虛擬機器，而它將會需要利用 1 虛擬處理器。

- 確保 4 gb 是適用於虛擬機器
- 設定虛擬處理器相連 1
- 檢查以確保至少 10%適用於虛擬機器的 CPU

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

當它完成時，您將會提供關於虛擬機器和放置位置的資訊。  在上述範例中，它會顯示為：

        State         : Running
        ComputerName  : 1-S2D2

如果您沒有足夠的記憶體、 cpu 或磁碟空間新增虛擬機器，您會收到錯誤：

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

一旦建立虛擬機器，它將會顯示在指定的特定節點上的 HYPER-V 管理員 」 中。  若要新增它，因為在叢集中設定虛擬機器和至叢集，以下是命令。  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

當它完成時，將會在輸出：

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

如果您已新增與現有的虛擬機器叢集，虛擬機器也需要使用叢集集合因此註冊所有虛擬機器一次，若要使用命令是來註冊：

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

不過，在程序不完整像需要新增到叢集設定命名空間的虛擬機器的路徑。

因此，舉例來說，現有的叢集已新增，且它有預先設定的虛擬機器 reside 上本機叢集共用磁碟區 (CSV)，為 VHDX 的路徑看起來應該類似於 「 C:\ClusterStorage\Volume1\MYVM\Virtual 硬碟 Disks\MYVM.vhdx。  因為 CSV 路徑是由本機至單一成員叢集的設計必須完成需要儲存空間移轉。 因此，將無法存取到虛擬機器後即時移轉到叢集成員。 

在此範例中，CLUSTER3 已新增至叢集設定與基礎結構向外延展檔案伺服器使用新增 ClusterSetMember，做為 SOFS CLUSTER3。  若要移動的虛擬機器設定及存放裝置，則命令為：

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

在它完成後，您會收到一則警告：

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

因為警告 」 不偵測到任何在虛擬機器角色存放裝置設定中的變更 」 時，會忽略這個警告。  不會變更做為實際的實體位置警告的原因;只設定路徑。 

如需移動 VMStorage 的詳細資訊，請檢閱此[連結](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps)。 

即時移轉虛擬機器在不同的叢集設定叢集之間是過去不相同。 在非叢集組案例就是步驟：

1. 從叢集移除虛擬機器角色。
2. 即時移轉至不同的叢集的成員節點的虛擬機器。
3. 新增至叢集的虛擬機器，做為新的虛擬機器角色。

使用叢集集這些不是必要步驟，且只有一個命令。  例如，我想要將叢集設定虛擬機器從 CLUSTER1 NODE2 CL3 CLUSTER3 上。  就是單一命令：

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

請注意這不會移動虛擬機器的存放區或組態檔案。  這不是因為虛擬機器的路徑均維持 \\SOFS-CLUSTER1\VOLUME1 為必要。  一旦已經註冊虛擬機器叢集集合與基礎結構的檔案伺服器共用路徑，磁碟機和虛擬機器不需要虛擬機器的相同電腦上。

## 建立可用性設定容錯網域

中所述的簡介，類似 Azure 的容錯網域和可用性集可以設定一個叢集集合。  這是初始虛擬機器的位置和叢集之間移轉非常實用。  

在下列範例中，有四個參與 「 叢集設定的叢集。  在組，邏輯的容錯網域將會建立與兩個叢集和使用其他兩個叢集建立容錯網域。  這些兩個容錯網域會構成 Availabiilty 集。 

在下列範例中，CLUSTER1 和 CLUSTER2 會呼叫**FD1** ，雖然 CLUSTER3 和 CLUSTER4 將可呼叫**FD2**容錯網域中的容錯網域中。  「 可用性 」 設定，就會呼叫**CSMASTER-AS**與包含兩個容錯網域。

若要建立容錯網域，命令是：

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

若要確保他們已成功建立，可以使用其輸出顯示執行 Get ClusterSetFaultDomain。

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

現在已建立 「 容錯網域，可用性設定要建立的需求。

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

若要驗證它已經建立，然後使用：

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

在建立新的虛擬機器，然後您必須使用-AvailabilitySet 參數做為判斷最佳的節點的一部分。  讓它然後看起來像這樣：

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

從叢集移除叢集中設定因各種不同的生命週期。 有的時候必須從叢集中移除叢集中。 最佳做法是，所有的叢集設定虛擬機器應該移出叢集。 這可以使用 [**移動 ClusterSetVM**和**移動 VMStorage** ] 命令來完成。

不過，如果也不會移動虛擬機器，叢集集合將會執行一系列的動作來為系統管理員提供直覺的結果。  從集合移除叢集時，所有剩餘叢集設定虛擬機器裝載於叢集移除將只會變成高可用性繫結至該叢集，假設，他們可以存取其儲存空間的虛擬機器。  叢集集合也會自動將會更新其詳細目錄所：

- 不再追蹤目前已移除的叢集並在其上執行的虛擬機器的健康情況
- 移除叢集中設定命名空間和共用裝載於現在移除叢集中的所有參考

例如，就是要從叢集集合移除 CLUSTER1 叢集的命令：

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## 常見問題 (FAQ)

**問題：** 我叢集中 gender 我受限於只能使用超融合式叢集？ <br>
**回應：**[否]。  您可以混合儲存空間直接存取使用傳統的叢集。

**問題：** 我是否可以管理我叢集透過 System Center 虛擬機器 Manager 設定？ <br>
**回應：** System Center 虛擬機器管理員目前不支援叢集集合 <br><br> **問題：** 可以 Windows Server 2012 R2 或 2016年叢集共置存在於相同叢集中嗎？ <br>
**問題：** 可以將移轉關閉 Windows Server 2012 R2 的工作負載，或是只讓那些叢集 2016年叢集加入相同的叢集設定？ <br>
**回應：** 叢集集合是新的技術導入 Windows Server preview 組建，因此因如此，不存在在先前的版本。 下層 OS 型叢集就無法加入叢集設定。 不過，叢集作業系統輪流升級技術應該提供您正在尋找藉由將這些叢集升級到 Windows Server 2019 的移轉功能。

**問題：** 可以叢集集合，允許我縮放存放裝置，或 （單獨） 計算？ <br>
**回應：** 是的只需新增儲存空間直接存取或傳統 HYPER-V 叢集。 叢集集合，會直接運算-存放裝置比例，即使在超融合式叢集設定中的變更。

**問題：** 什麼是叢集集合的管理工具 <br>
**回應：** PowerShell 或 WMI 在此版本中。

**問題：** 如何跨叢集即時移轉將會有不同的世代的處理器運作？  <br>
**回應：** 叢集集合不會因應處理器差異和取代 HYPER-V 目前支援。  因此，處理器相容性模式必須搭配快速移轉。  叢集集合的建議是使用相同的處理器硬體內每個個別的叢集，以及整個叢集設定適用於發生的叢集之間的即時移轉。

**問題：** 可以我的叢集中設定虛擬電腦自動容錯移轉叢集失敗？  <br>
**回應：** 在此版本中，叢集設定虛擬機器僅能手動即時移轉到叢集;但無法自動容錯移轉。 

**問題：** 我們如何確保存放裝置是彈性叢集失敗？ <br>
**回應：** 了解儲存體復原叢集失敗在成員叢集上使用跨叢集儲存體複本 (SR) 解決方案。

**問題：** 我可以使用儲存體複本 (SR) 來複寫整個叢集成員。 上 SR 容錯移轉叢集設定命名空間儲存 UNC 路徑變更至複本目標儲存空間直接存取叢集？ <br>
**回應：** 在此版本中，這類叢集設定轉介需要變更命名空間不會發生 SR 容錯移轉。 請讓 Microsoft 知道這個案例中是否為您和您打算如何使用它的關鍵。

**問題：** 它可能會容錯移轉虛擬機器跨嚴重損壞修復狀況中的容錯網域 （說出整個容錯網域發生向下）？ <br>
**回應：** 否，請注意該跨叢集容錯移轉內邏輯的容錯網域尚不支援的。 

**問題：** 我的叢可以在多個網站 （或 DNS 網域） 中設定 span 叢集嗎？ <br> 
**回應：** 這是一個未經測試的案例，並不會立即計劃生產環境支援。 請讓 Microsoft 知道這個案例中是否為您和您打算如何使用它的關鍵。

**問題：** 並不會叢集設定與 IPv6 搭配運作嗎？ <br>
**回應：** 如同容錯移轉叢集，會將 IPv4 和 IPv6 支援使用叢集集。

**問題：** 適用於叢集的 Active Directory 樹系需求是什麼設定 <br>
**回應：** 所有成員的叢集必須都是相同的 AD 樹系。

**問題：** 有多個叢集節點可以在單一叢集中的一部分設定或？ <br>
**回應：** 在預覽中，叢集集合經過測試，支援的最高 64 總叢集節點。 不過，叢集將架構縮放比例設定為更大的限制，而且沒有限制的硬式編碼的東西。 請讓 Microsoft 知道是否為較大的縮放比例您和您打算如何使用它的關鍵。

**問題：** 將會在叢集中所有的儲存空間直接存取叢集形成單一儲存集區？ <br>
**回應：**[否]。 儲存空間直接存取技術仍可運作的單一叢集內，並非所有成員在叢集中的叢集。

**問題：** 叢集設定高可用性的命名空間？ <br>
**回應：** 是的叢集設定命名空間會提供持續可用 (CA) 轉介 SOFS 命名空間伺服器上管理叢集執行透過。 Microsoft 建議有足夠數量的虛擬機器進行當地語系化的整個叢集的失敗具彈性的成員叢集。 不過，針對預見嚴重失敗 – 例如所有的虛擬機器移往下同時管理叢集中 – 轉介資訊是此外持續快取中每個叢集設定節點，甚至是在重新開機之間。
 
**問題：** 叢集並不會在叢集中設定命名空間為基礎的儲存存取慢的存放裝置效能嗎？ <br>
**回應：**[否]。 叢集設定命名空間提供叢集集合 – 在概念上像是分散式檔案系統的命名空間 (DFSN) 內的重疊轉介命名空間。 和不同於 DFSN、 所有叢集設定命名空間轉介中繼資料都是自動填入和自動更新的所有節點，而不需要任何系統管理員介入，因此不幾乎沒有效能負擔存放裝置存取路徑中。 

**問題：** 我要如何備份叢集設定中繼資料？ <br>
**回應：** 本指南是容錯移轉叢集相同。 系統狀態備份會備份叢集狀態。  透過 Windows Server Backup，您可以還原的只是節點的叢集資料庫 （這應該永遠不需要自我修復的邏輯，我們已經有許多因為） 也不會回復所有節點跨整個叢集資料庫中的授權還原。 對於叢集集合，Microsoft 建議這類授權還原第一次成員叢集，然後管理叢集上如有需要。 
