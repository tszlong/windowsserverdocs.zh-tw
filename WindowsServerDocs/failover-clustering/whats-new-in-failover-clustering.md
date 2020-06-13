---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: Windows Server 中容錯移轉叢集的新功能
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: cdc9280c4c7129d2e76341bd97b0ae3b8209888f
ms.sourcegitcommit: 7200143aa787c7ac05ae0e012263b1c9a95b87ed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721736"
---
# <a name="whats-new-in-failover-clustering"></a>容錯移轉叢集的新功能

> 適用於：Windows Server 2019、Windows Server 2016

本主題說明 Windows Server 2019 和 Windows Server 2016 容錯移轉叢集的新功能和已變更的功能。

## <a name="whats-new-in-windows-server-2019"></a>Windows Server 2019 的新功能

- **叢集集合**

    叢集集可讓您在單一軟體定義的資料中心（SDDC）解決方案中增加超過目前叢集限制的伺服器數目。 這是藉由將多個叢集分組成一個叢集集來完成，也就是多個容錯移轉叢集的鬆散結合群組：計算、儲存體和超交集。
    使用叢集集時，您可以在叢集集合內的叢集之間移動線上虛擬機器（即時移轉）。

    如需詳細資訊，請參閱叢集[集合](../storage/storage-spaces/cluster-sets.md)。

- **Azure 感知叢集**
  
    容錯移轉叢集現在會自動偵測在 Azure IaaS 虛擬機器中執行的時間，並將設定優化以提供 Azure 預定維護事件的主動式容錯移轉和記錄，以達到最高層級的可用性。 藉由移除為叢集名稱使用動態網路名稱設定負載平衡器的需求，也可簡化部署。

- **跨網域叢集移轉**

    容錯移轉叢集現在可以動態地從一個 Active Directory 網域移到另一個，以簡化網域合併，並允許硬體合作夥伴建立叢集，並于稍後加入客戶的網域。

- **USB 見證**

    您現在可以在判斷叢集的仲裁時，使用連接到網路交換器的簡單 USB 磁片磁碟機做為見證。 這會擴充檔案共用見證，以支援任何符合 SMB2 規範的裝置。

- **叢集基礎結構改進功能**

    CSV 快取現在預設為啟用，以提升虛擬機器的效能。 MSDTC 現在支援叢集共用磁碟區，以允許在儲存空間直接存取上部署 MSDTC 工作負載，例如使用 SQL Server。 強化邏輯以偵測具有自我修復的分割節點，讓節點回到叢集成員資格。 強化的叢集網路路由會偵測並自我修復。

- **叢集感知更新支援儲存空間直接存取**

    叢集感知更新 (CAU) 現在已整合並能察覺儲存空間直接存取，驗證和確保每個節點上的資料完成重新同步處理。 叢集感知更新會檢查更新，只有在必要時才會以智慧方式重新開機。 這可協調叢集中所有伺服器的重新開機，以進行預定的維護。

- **檔案共用見證增強功能**

    在下列案例中，我們已啟用檔案共用見證的使用：
  - 因遠端位置而不存在或極差的網際網路存取，因而導致無法使用雲端見證。
  - 缺少磁片見證的共用磁片磁碟機。 這可能是儲存空間直接存取的超大範圍設定、SQL Server Always On 可用性群組（AG）或 * Exchange 資料庫可用性群組（DAG），都不會使用共用磁片。
  - 因為叢集位於 DMZ 後方，所以缺少網域控制站連線。
  - 沒有 Active Directory 叢集名稱物件（CNO）的工作組或跨網域叢集。 如需這些增強功能的詳細資訊，請閱讀下列伺服器 & 管理的文章：容錯移轉叢集檔案共用見證和 DFS。

    我們現在也會明確地封鎖使用 DFS 命名空間共用作為位置。 將檔案共用見證新增至 DFS 共用可能會對您的叢集造成穩定性問題，而且從未支援此設定。 我們新增了邏輯來偵測共用是否使用 DFS 命名空間，如果偵測到 DFS 命名空間，容錯移轉叢集管理員會封鎖建立見證，並顯示不支援的錯誤訊息。

- **叢集強化**

    透過適用於叢集共用磁碟區和儲存空間直接存取的伺服器訊息區 (SMB) 的叢集間通訊，現在可運用憑證來提供最安全的平台。 這可讓容錯移轉叢集不再依賴 NTLM 運作，並啟用安全性基準。

- **容錯移轉叢集不再使用 NTLM 驗證**

    容錯移轉叢集不再使用 NTLM 驗證。 相反地，會以獨佔方式使用 Kerberos 和以憑證為基礎的驗證。 使用者或部署工具不需要進行任何變更，就能利用這項安全性增強功能。 它也允許在已停用 NTLM 的環境中部署容錯移轉叢集。

## <a name="whats-new-in-windows-server-2016"></a>Windows Server 2016 中的新功能

### <a name="cluster-operating-system-rolling-upgrade"></a><a name="BKMK_RollingUpgrade"></a>叢集作業系統輪流升級

叢集作業系統輪流升級可讓系統管理員將叢集節點的作業系統從 Windows Server 2012 R2 升級至較新的版本，而不需停止 Hyper-v 或擴充檔案伺服器工作負載。 此功能可以避免針對服務等級協定 (SLA) 的停機時間扣分。

**這個變更增加了什麼價值？**  

將 Hyper-v 或擴充檔案伺服器叢集從 Windows Server 2012 R2 升級到 Windows Server 2016 不再需要停機。 叢集將繼續在 Windows Server 2012 R2 層級上運作，直到叢集中的所有節點都執行 Windows Server 2016 為止。 叢集功能等級會使用 Windows PowerShell cmdlt 升級至 Windows Server 2016 `Update-ClusterFunctionalLevel` 。

> [!WARNING]  
> - 更新叢集功能等級之後，您就無法回到 Windows Server 2012 R2 叢集功能等級。
>
> - 在 `Update-ClusterFunctionalLevel` 執行指令程式之前，進程是可復原的，而且可以新增 Windows server 2012 R2 節點，而且可以移除 Windows server 2016 節點。

**有哪些不同？**  

Hyper-v 或向外延展檔案伺服器容錯移轉叢集現在可以輕鬆地升級，而不需要停機或建立新叢集，並執行 Windows Server 2016 作業系統的節點。 將叢集遷移至 Windows Server 2012 R2 牽涉到將現有的叢集離線，然後重新安裝每個節點的新作業系統，然後讓叢集重新上線。 舊的程式很麻煩，而且需要停機時間。 不過，在 Windows Server 2016 中，叢集不需要在任何時間點離線。

針對叢集中的每個節點，升級階段的叢集作業系統如下所示：  
-   節點已暫停，並已清空其上執行的所有虛擬機器。 
-   虛擬機器（或其他叢集工作負載）會遷移到叢集中的另一個節點。 
-   系統會移除現有的作業系統，並在節點上執行 Windows Server 2016 作業系統的全新安裝。 
-   執行 Windows Server 2016 作業系統的節點會新增回叢集。 
-   此時，叢集會被視為在混合模式中執行，因為叢集節點正在執行 Windows Server 2012 R2 或 Windows Server 2016。 
-   叢集功能等級會保留在 Windows Server 2012 R2。 在此功能層級，Windows Server 2016 中會影響與舊版作業系統相容性的新功能將無法使用。 
-   最後，所有節點都會升級至 Windows Server 2016。 
-   然後，使用 Windows PowerShell Cmdlet，將叢集功能等級變更為 Windows Server 2016 `Update-ClusterFunctionalLevel` 。 此時，您可以利用 Windows Server 2016 的功能。 

如需詳細資訊，請參閱叢集[作業系統輪流升級](cluster-operating-system-rolling-upgrade.md)。 

### <a name="storage-replica"></a><a name="BKMK_SR"></a>儲存體複本  
「儲存體複本」是一項新功能，可在伺服器或叢集之間啟用與存放裝置無關、區塊層級、同步複寫，以進行嚴重損壞修復，以及在網站間延展容錯移轉叢集。 同步複寫能以當機時保持一致的磁碟區啟用對實體站台中資料的鏡像，來確保檔案系統層級零資料遺失。 非同步複寫允許都會範圍外的站台擴充功能，但有資料遺失的可能性。 

**這個變更增加了什麼價值？**  

儲存體複本可讓您執行下列動作：  

-   針對任務關鍵性工作負載的計畫與非計畫中斷，提供單一廠商災害復原解決方案。 

-   使用經證明具可靠性、可調整性和效能的 SMB3 傳輸。 

-   將 Windows 容錯移轉叢集延伸至都會距離。 

-   針對儲存體和叢集使用 Microsoft 軟體端對端，例如 Hyper-v、儲存體複本、儲存空間、叢集、向外延展檔案伺服器、SMB3、重復資料刪除和 ReFS/NTFS。 

-   以下列方式協助降低成本和複雜性：  

    -   與硬體無關，不具有像 DAS 或 SAN 的特定儲存體組態需求。 

    -   允許平價儲存體和網路技術。 

    -   透過容錯移轉叢集管理員，針對個別的節點和叢集提供易於使用的圖形化管理。 

    -   透過 Windows PowerShell，提供完整、大規模的指令碼選項。 

-   協助降低停機時間，並提升 Windows 本身的可靠性和生產力。 

-   提供支援能力、效能標準及診斷功能。 

如需詳細資訊，請參閱 [Windows Server 2016 中的儲存體複本](../storage/storage-replica/storage-replica-overview.md)。 

### <a name="cloud-witness"></a><a name="BKMK_CloudWitness"></a>雲端見證

雲端見證是 Windows Server 2016 中的新類型「容錯移轉叢集」仲裁見證，其利用 Microsoft Azure 作為仲裁點。 雲端見證 (例如任何其他仲裁見證) 會獲得票數，並可以參與仲裁計算。 您可以使用 [設定叢集仲裁精靈] 將雲端見證設定為仲裁見證。 

**這個變更增加了什麼價值？**  

使用雲端見證做為容錯移轉叢集仲裁見證提供下列優點：  

-   利用 Microsoft Azure，不需要第三個個別的資料中心。 

-   使用標準公開提供的 Microsoft Azure Blob 儲存體，可消除裝載在公用雲端中 Vm 的額外維護負擔。 

-   相同的 Microsoft Azure 儲存體帳戶可以用於多個叢集（每個叢集一個 blob 檔案; 做為 blob 檔案名使用的叢集唯一識別碼）。 

-   為儲存體帳戶提供非常低的持續成本（每個 blob 檔案寫入的資料非常少，blob 檔案只會在叢集節點的狀態變更時更新一次）。 

如需詳細資訊，請參閱為[容錯移轉叢集部署雲端見證](deploy-cloud-witness.md)。 

**有哪些不同？**  

這是 Windows Server 2016 中的新功能。 

### <a name="virtual-machine-resiliency"></a><a name="BKMK_VMs"></a>虛擬機器復原

**計算復原**Windows Server 2016 包括增加的虛擬機器計算復原功能，可協助減少計算叢集中的叢集內通訊問題，如下所示： 

-   **虛擬機器可用的復原選項：** 您現在可以設定虛擬機器復原選項，在暫時性失敗期間定義虛擬機器的行為：  

    -   **復原層級：** 協助您定義如何處理暫時性失敗。 

    -   **復原期間：** 協助您定義允許所有虛擬機器執行隔離的時間長度。 

-   **隔離狀況不良的節點：** 已隔離狀況不良的節點，且不再允許加入叢集。 這可防止 flapping 節點對其他節點和整體叢集造成負面影響。 

如需可控制如何隔離或隔離節點的虛擬機器計算復原工作流程和節點隔離設定的詳細資訊，請參閱[Windows Server 2016 中的虛擬機器計算復原](https://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx)功能。 

**儲存體復原**在 Windows Server 2016 中，虛擬機器在暫時性儲存體失敗方面更有彈性。 改善的虛擬機器復原功能有助於在發生儲存體中斷時保留租使用者虛擬機器會話狀態。 這是透過智慧和快速虛擬機器對儲存體基礎結構問題的回應所達成。 

當虛擬機器與其基礎存放裝置中斷連線時，它會暫停並等候存放裝置復原。 暫停時，虛擬機器會保留其中正在執行之應用程式的內容。 當虛擬機器與其存放裝置的線上還原時，虛擬機器會回到其執行狀態。 因此，租使用者電腦的會話狀態會在復原時保留。 

在 Windows Server 2016 中，虛擬機器儲存體復原功能也會感知並針對來賓叢集進行優化。 

### <a name="diagnostic-improvements-in-failover-clustering"></a><a name="BKMK_Diagnostics"></a>容錯移轉叢集的診斷改善

為了協助診斷容錯移轉叢集的問題，Windows Server 2016 包括下列各項：  

- 叢集記錄檔的幾項增強功能（例如時區資訊和 DiagnosticVerbose 記錄），可讓您更輕鬆地針對容錯移轉叢集問題進行疑難排解。 如需詳細資訊，請參閱[Windows Server 2016 容錯移轉叢集疑難排解增強功能-叢集記錄](https://techcommunity.microsoft.com/t5/failover-clustering/windows-server-2016-failover-cluster-troubleshooting/ba-p/372005)檔。

- 一種新的「使用中**記憶體傾印**」類型，可篩選出配置給虛擬機器的大部分記憶體頁面，因而使記憶體的 dmp 變得更小且更容易儲存或複製。 如需詳細資訊，請參閱[Windows Server 2016 容錯移轉叢集疑難排解增強功能-Active Dump](https://techcommunity.microsoft.com/t5/failover-clustering/windows-server-2016-failover-cluster-troubleshooting/ba-p/372008)。

### <a name="site-aware-failover-clusters"></a><a name="BKMK_SiteAware"></a>網站感知容錯移轉叢集

Windows Server 2016 包含網站感知的容錯移轉叢集，可根據實體位置（網站）在延展的叢集中啟用群組節點。 叢集網站感知會增強叢集生命週期期間的關鍵作業，例如容錯移轉行為、放置原則、節點之間的信號，以及仲裁行為。 如需詳細資訊，請參閱[Windows Server 2016 中的網站感知容錯移轉](https://techcommunity.microsoft.com/t5/failover-clustering/site-aware-failover-clusters-in-windows-server-2016/ba-p/372060)叢集。

### <a name="workgroup-and-multi-domain-clusters"></a><a name="BKMK_multidomainclusters"></a>工作群組和多網域叢集

在 Windows Server 2012 R2 和先前版本中，只能在加入相同網域的成員節點之間建立叢集。 Windows Server 2016 已解除了這些障礙，並引進不需 Active Directory 相依性就能建立容錯移轉叢集的能力。 您現在可以在下列設定中建立容錯移轉叢集：  

-   **單一網域叢集。** 已加入相同網域之所有節點的叢集。 

-   **多網域叢集。** 叢集，其中節點是不同網域的成員。 

-   **工作組叢集。** 節點為成員伺服器/工作組（未加入網域）的叢集。 

如需詳細資訊，請參閱[Windows Server 2016 中的工作組和多網域](https://techcommunity.microsoft.com/t5/failover-clustering/workgroup-and-multi-domain-clusters-in-windows-server-2016/ba-p/372059)叢集

### <a name="virtual-machine-load-balancing"></a><a name="BKMK_VMLoadBalancing"></a>虛擬機器負載平衡  

虛擬機器負載平衡是容錯移轉叢集的一項新功能，可讓您在叢集中跨節點順暢地對虛擬機器進行負載平衡。 系統會根據節點上的虛擬機器記憶體和 CPU 使用率來識別過度認可的節點。 然後，會將虛擬機器從過度認可的節點移動（即時移轉）到具有可用頻寬的節點（如果適用）。 您可以調整平衡的能力，以確保最佳的叢集效能和使用率。 預設會在 Windows Server 2016 Technical Preview 中啟用負載平衡。 不過，啟用 SCVMM 動態優化時，會停用負載平衡。 

### <a name="virtual-machine-start-order"></a><a name="BKMK_VMStartOrder"></a>虛擬機器啟動順序

虛擬機器啟動順序是容錯移轉叢集的新功能，引進叢集中虛擬機器（和所有群組）的開始順序協調流程。 虛擬機器現在可以分組成各層，而且可以在不同層之間建立開始順序相依性。 這可確保先啟動最重要的虛擬機器（例如網域控制站或公用程式虛擬機器）。 虛擬機器在其相依性的虛擬機器也已啟動之前，都不會啟動。 

### <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a><a name="BKMK_SMBMultiChannel"></a>簡化的 SMB 多重通道和多個 NIC 叢集網路

容錯移轉叢集網路不再受限於每個子網/網路的單一 NIC。 使用簡化的 SMB 多重通道和多個 NIC 叢集網路，網路設定會自動進行，且子網上的每個 NIC 都可以用於叢集和工作負載流量。 這項增強功能可讓客戶將 Hyper-v、SQL Server 容錯移轉叢集實例和其他 SMB 工作負載的網路輸送量最大化。 

如需詳細資訊，請參閱[簡化的 SMB 多重通道和多個 NIC 叢集網路](smb-multichannel.md)。

## <a name="see-also"></a>另請參閱

* [Storage](../storage/storage.md)  
* [Windows Server 2016 中存放裝置的新功能](../storage/whats-new-in-storage.md)  
