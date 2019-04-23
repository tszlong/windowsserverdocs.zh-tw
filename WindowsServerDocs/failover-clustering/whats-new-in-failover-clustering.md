---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: 容錯移轉叢集的 Windows Server 最新消息
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: b4fa59aa62acba5c89f20c191da2c3c1b776b1ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884749"
---
# <a name="whats-new-in-failover-clustering"></a>容錯移轉叢集的新功能

> 適用於：Windows Server 2019，Windows Server 2016 中，Windows Server （半年通道）

本主題說明容錯移轉叢集的 Windows Server 2019，Windows Server 2016 中新的和變更功能和 Windows Server 半年通道釋出。

## <a name="whats-new-in-windows-server-2019"></a>Windows Server 2019 的新功能

- **叢集設定**

    叢集設定可讓您增加在單一軟體定義資料中心 (SDDC) 解決方案之外的叢集中目前的限制中的伺服器數目。 這可以藉由將分組至叢集 set--鬆散偶合的多個容錯移轉叢集群組的多個叢集： 計算、 儲存體和超融合式。
    與叢集設定中，您可以移動線上虛擬機器 （即時移轉） 在叢集中的叢集之間設定。

    如需詳細資訊，請參閱 <<c0> [ 叢集組](../storage/storage-spaces/cluster-sets.md)。

- **Azure 可感知叢集**

    容錯移轉叢集現在會自動偵測它們在 Azure IaaS 虛擬機器中執行並最佳化來提供主動式容錯移轉和記錄 Azure 計劃性的維護事件，以達到最高層級的可用性設定。 藉由移除需要設定負載平衡器與動態叢集名稱的網路名稱也簡化部署。

- **跨網域的叢集移轉**

    容錯移轉叢集可以立即以動態方式從一個 Active Directory 網域間移動，簡化網域合併，並讓叢集由硬體合作夥伴建立和更新版本加入至客戶的網域。
- **USB 見證**

    您現在可以使用簡單的 USB 磁碟機，連接至網路交換器以決定叢集的仲裁見證。 這會擴充以支援任何 SMB2 符合規範的裝置檔案共用見證。

- **叢集基礎結構改進**

    預設值現在會啟用 CSV 快取，大幅提升虛擬機器效能。 MSDTC 現在支援叢集共用磁碟區，以允許在儲存空間直接存取上部署 MSDTC 工作負載，例如使用 SQL Server。 強化邏輯以偵測具有自我修復的分割節點，讓節點回到叢集成員資格。 強化的叢集網路路由會偵測並自我修復。

- **叢集感知更新的支援儲存空間直接存取**

    叢集感知更新 (CAU) 現在已整合並能察覺儲存空間直接存取，驗證和確保每個節點上的資料完成重新同步處理。 叢集感知更新會檢查只有在必要時以智慧方式重新啟動的更新。 這可讓協調維護計劃叢集中的所有伺服器重新啟動。

- **檔案共用見證的增強功能**我們啟用下列案例中的檔案共用見證的使用： 
    - 不存在或極差網際網路存取因為遠端位置，避免使用的雲端見證。 
    - 缺少的共用磁碟機的磁碟見證。 這可能是儲存空間直接存取的超融合組態，SQL Server Alwayson 可用性群組 (AG)，或 * Exchange 資料庫可用性群組 (DAG)，都不會使用共用的磁碟。 
    - 缺乏因位在 DMZ 正在叢集的網域控制站連線。 
    - 工作群組或跨網域叢集，有是任何 Active Directory 叢集名稱物件 (CNO)。 深入了解下列在伺服器和管理部落格文章中的這些增強功能：容錯移轉叢集檔案共用見證，DFS。
    
    我們現在也會明確封鎖使用 DFS 命名空間共用，做為位置。 新增檔案共用見證至 DFS 共用可能會造成穩定性問題為您的叢集，並永遠不會支援此設定。 我們已新增邏輯，可偵測如果共用使用 DFS 命名空間，而且如果偵測到 DFS 命名空間時，容錯移轉叢集管理員封鎖見證的建立，並顯示有關不支援的錯誤訊息。
- **叢集強化**

    透過適用於叢集共用磁碟區和儲存空間直接存取的伺服器訊息區 (SMB) 的叢集間通訊，現在可運用憑證來提供最安全的平台。 這可讓容錯移轉叢集不再依賴 NTLM 運作，並啟用安全性基準。
- **容錯移轉叢集不會再使用 NTLM 驗證**

    容錯移轉叢集無法再使用 NTLM 驗證。 Kerberos 和憑證型驗證會改用以獨佔方式。 沒有所需的使用者或部署工具，利用這項安全性增強功能的變更。 您也可以在其中已停用 NTLM 的環境中部署的容錯移轉叢集。 


## <a name="whats-new-in-windows-server-2016"></a>什麼是 Windows Server 2016 的新功能

### <a name="BKMK_RollingUpgrade"></a>叢集作業系統輪流升級

叢集作業系統輪流升級可讓系統管理員而不需停止 HYPER-V 或向外延展檔案伺服器工作負載較新的版本升級叢集節點的作業系統從 Windows Server 2012 R2。 此功能可以避免針對服務等級協定 (SLA) 的停機時間扣分。 

**這個變更增加了什麼價值？**  

從 Windows Server 2012 R2 中將 HYPER-V 或向外延展檔案伺服器叢集升級到 Windows Server 2016 時，不再需要停機時間。 叢集會繼續在 Windows Server 2012 R2 層級函式，直到所有叢集中的節點都執行 Windows Server 2016。 叢集功能等級時，會升級到 Windows Server 2016 上，使用 Windows PowerShell cmdlt `Update-ClusterFunctionalLevel`。 

> [!WARNING]  
> -   更新叢集功能等級之後，您不能移回至 Windows Server 2012 R2 叢集功能等級。 
> -   直到`Update-ClusterFunctionalLevel`cmdlet 執行時，程序可逆轉的和 Windows Server 2012 R2 節點加入，而且可以移除 Windows Server 2016 節點。 

**有哪些不同？**  

HYPER-V 或向外延展檔案伺服器容錯移轉叢集可以立即輕鬆升級而不需要停機，或需要建立新的叢集節點執行 Windows Server 2016 作業系統。 移轉到 Windows Server 2012 R2 的叢集牽涉到讓現有的叢集離線並重新安裝新的作業系統，對於每個節點，然後讓叢集回到線上。 舊的處理序已經很麻煩且需要停機時間。 不過，在 Windows Server 2016 中，叢集不會不需要在任何時間點離線。 

分階段升級叢集的作業系統會對於叢集中的每個節點，如下所示：  
-   節點會暫停並清空所有其上執行的虛擬機器。 
-   虛擬機器 （或其他叢集工作負載） 移轉至叢集中的另一個節點。 
-   在移除現有的作業系統，並在執行 Windows Server 2016 上的作業系統 節點的全新安裝。 
-   執行 Windows Server 2016 作業系統的節點會加回至叢集。 
-   到目前為止，叢集即稱為混合模式中，在執行中，因為在叢集節點都執行 Windows Server 2012 R2 或 Windows Server 2016。 
-   將叢集功能等級仍然保持為 Windows Server 2012 R2。 此功能的層級，在 Windows Server 2016 中影響舊版作業系統的相容性的新功能將無法使用。 
-   最後，所有節點會都升級到 Windows Server 2016。 
-   叢集功能等級然後變更為使用 Windows PowerShell cmdlet 的 Windows Server 2016 `Update-ClusterFunctionalLevel`。 此時，您也可以利用 Windows Server 2016 功能。 

如需詳細資訊，請參閱 <<c0> [ 叢集作業系統輪流升級](cluster-operating-system-rolling-upgrade.md)。 

### <a name="BKMK_SR"></a>儲存體複本  
儲存體複本是新的功能，可讓存放裝置無關、 區塊層級、 同步複寫伺服器或叢集的災害復原，以及延伸容錯移轉叢集站台間之間。 同步複寫能以當機時保持一致的磁碟區啟用對實體站台中資料的鏡像，來確保檔案系統層級零資料遺失。 非同步複寫允許都會範圍外的站台擴充功能，但有資料遺失的可能性。 

**這個變更增加了什麼價值？**  

儲存體複本可讓您執行下列作業：  

-   針對任務關鍵性工作負載的計畫與非計畫中斷，提供單一廠商災害復原解決方案。 

-   使用經證明具可靠性、可調整性和效能的 SMB3 傳輸。 

-   將 Windows 容錯移轉叢集延伸至都會距離。 

-   使用端對端的 Microsoft 軟體的儲存體和叢集，例如 HYPER-V、 儲存體複本，儲存空間、 叢集、 向外延展檔案伺服器、 SMB3、 重複資料刪除，以及 ReFS/NTFS。 

-   以下列方式協助降低成本和複雜性：  

    -   與硬體無關，不具有像 DAS 或 SAN 的特定儲存體組態需求。 

    -   允許平價儲存體和網路技術。 

    -   透過容錯移轉叢集管理員，針對個別的節點和叢集提供易於使用的圖形化管理。 

    -   透過 Windows PowerShell，提供完整、大規模的指令碼選項。 

-   協助降低停機時間，並提升 Windows 本身的可靠性和生產力。 

-   提供支援能力、效能標準及診斷功能。 

如需詳細資訊，請參閱 [Windows Server 2016 中的儲存體複本](../storage/storage-replica/storage-replica-overview.md)。 


### <a name="BKMK_CloudWitness"></a>雲端見證  
雲端見證是 Windows Server 2016 中的新類型「容錯移轉叢集」仲裁見證，其利用 Microsoft Azure 作為仲裁點。 雲端見證 (例如任何其他仲裁見證) 會獲得票數，並可以參與仲裁計算。 您可以使用 [設定叢集仲裁精靈] 將雲端見證設定為仲裁見證。 

**這個變更增加了什麼價值？**  

使用雲端見證，做為容錯移轉叢集仲裁見證可提供下列優點：  

-   利用 Microsoft Azure，並不需要第三個不同的資料中心。 

-   使用標準公開提供使用 Microsoft Azure Blob 儲存體的 Vm 裝載於公用雲端中的額外維護額外負荷。 

-   相同的 Microsoft Azure 儲存體帳戶可以用於多個群集 （一個 blob 檔案，每個叢集，叢集的唯一識別碼做為 blob 檔案名稱）。 

-   提供極低的持續成本的儲存體帳戶 （非常小的資料寫入每個 blob 檔案，當叢集節點的狀態變更時，只要更新 blob 檔案）。 

如需詳細資訊，請參閱 <<c0> [ 部署雲端見證的容錯移轉叢集的](deploy-cloud-witness.md)。 

**有哪些不同？**  

這是 Windows Server 2016 中的新功能。 

### <a name="BKMK_VMs"></a>虛擬機器復原功能  
**計算復原**Windows Server 2016 包含增加的虛擬機器計算復原，有助於減少您電腦叢集中的叢集內通訊問題，如下所示： 

-   **適用於虛擬機器的復原選項：** 您現在可以設定虛擬機器定義的虛擬機器的行為，在暫時性失敗的恢復功能選項：  

    -   **恢復功能層級：** 可協助您定義如何處理暫時性失敗。 

    -   **復原期間：** 可協助您定義的所有虛擬機器都允許執行隔離的時間長度。 

-   **健康情況不良節點的隔離區：** 狀況不良的節點遭到隔離，並不會再允許加入叢集。 這可防止造成負面影響的其他節點和整體叢集 flapping 的節點。 

設定的詳細資訊的虛擬機器計算復原工作流程和節點隔離控制您的節點放置在 [隔離] 或 [隔離的方式，請參閱[Windows Server 2016 中的虛擬機器計算復原功能](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx)。 

**儲存復原**在 Windows Server 2016 中，虛擬機器是暫時性儲存體失敗更有彈性。 改進的虛擬機器復原功能可協助保存租用戶虛擬機器的工作階段狀態發生儲存體中斷的情況。 做法是聰明且快速的虛擬機器儲存體基礎結構問題的回應。 

虛擬機器中斷連接其基礎儲存體時，它會暫停，並等候要復原的儲存體。 當暫停時，虛擬機器會保留在其中執行的應用程式的內容。 還原至其儲存體的虛擬機器的連線時，虛擬機器就會傳回其執行狀態。 如此一來，租用戶電腦的工作階段狀態會保留的復原。 

在 Windows Server 2016 中，虛擬機器儲存體復原功能太注意和最佳化以供客體叢集。 

### <a name="BKMK_Diagnostics"></a>在 容錯移轉叢集的診斷改善  
若要協助診斷問題使用容錯移轉叢集，Windows Server 2016 包含下列內容：  

-   叢集記錄檔 （例如時區資訊和 DiagnosticVerbose 記錄），可讓數個增強功能是您更輕鬆地針對容錯移轉叢集的問題進行疑難排解。 如需詳細資訊，請參閱 < [Windows Server 2016 容錯移轉叢集疑難排解增強功能-叢集記錄檔](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx)。 

-   新的傾印類型**作用中的記憶體傾印**，這會篩選掉大部分的記憶體頁面配置給虛擬機器，因此 memory.dmp 更小且更輕鬆地儲存或複製。 如需詳細資訊，請參閱 < [Windows Server 2016 容錯移轉叢集疑難排解增強功能為作用中的傾印](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx)。 

### <a name="BKMK_SiteAware"></a>網站感知容錯移轉叢集  
Windows Server 2016 包含站台-了解容錯移轉叢集，讓群組根據其實體位置 （網站） 的延伸叢集中的節點。 叢集網站感知會增強叢集生命週期，例如容錯移轉行為、 放置原則、 節點，以及仲裁行為之間的活動訊號期間的索引鍵作業。 如需詳細資訊，請參閱 < [Windows Server 2016 中的網站感知容錯移轉叢集](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx)。 

### <a name="BKMK_multidomainclusters"></a>工作群組和多網域叢集  
在 Windows Server 2012 R2 和先前版本中，叢集只能加入相同網域的成員節點之間建立。 Windows Server 2016 已解除了這些障礙，並引進不需 Active Directory 相依性就能建立容錯移轉叢集的能力。 您現在可以建立下列組態的容錯移轉叢集：  

-   **單一網域叢集。** 含有已加入相同網域的所有節點的叢集。 

-   **多網域叢集。** 具有節點屬於不同網域之成員的叢集。 

-   **Workgroup 叢集。** 也就是成員伺服器節點的叢集 / 工作群組 （非網域聯結）。 

如需詳細資訊，請參閱[Windows Server 2016 中的工作群組和多網域叢集](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
### <a name="BKMK_VMLoadBalancing"></a>虛擬機器負載平衡  
負載平衡虛擬機器是容錯移轉叢集，可簡化無縫式的負載平衡虛擬機器在叢集中節點之間的新功能。 過度認可的節點是根據虛擬機器的記憶體和 CPU 使用率在節點上的進行識別。 虛擬機器移 （即時移轉） 從過度認可的節點，節點中可用的頻寬 （如果適用）。 平衡此加強值可進行微調，以確保最佳的叢集效能和使用率。 Windows Server 2016 Technical Preview 中的預設會啟用負載平衡。 不過，負載平衡會停用時啟用 SCVMM 動態最佳化。 

### <a name="BKMK_VMStartOrder"></a>虛擬機器啟動順序  
虛擬機器啟動順序是在叢集中的容錯移轉叢集中引進虛擬機器的啟動順序協調流程 （和所有群組） 的新功能。 虛擬機器現在可以分成層，並啟動順序的相依性可以建立不同層之間。 這可確保第一個啟動，最重要的虛擬機器 （例如網域控制站或公用程式的虛擬機器）。 它們具有相依性的虛擬機器也會啟動之前將無法啟動虛擬機器。 

### <a name="BKMK_SMBMultiChannel"></a> 簡化的 SMB 多重通道和多個 NIC 的叢集網路  
容錯移轉叢集網路已不再限制為每個子網路的單一 NIC / 網路。 簡化 SMB 多重通道和多個 NIC 的叢集網路，網路組態會自動與子網路上的每個 NIC 可以用於叢集和工作負載的流量。 這項增強功能可讓客戶 HYPER-V、 SQL Server 容錯移轉叢集執行個體，與其他 SMB 工作負載的網路輸送量最大化。 

如需詳細資訊，請參閱 <<c0> [ 簡化 SMB 多重通道和多個 NIC 的叢集網路](smb-multichannel.md)。

## <a name="see-also"></a>另請參閱  
* [儲存空間](../storage/storage.md)  
* [在 Windows Server 2016 中的儲存體中最新消息](../storage/whats-new-in-storage.md)  
