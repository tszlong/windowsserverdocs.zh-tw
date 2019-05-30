---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: Windows Server 中存放裝置的新功能
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 05/29/2019
ms.openlocfilehash: 5469d663f64fdb453e03863f409b675473d3f6aa
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308565"
---
# <a name="whats-new-in-storage-in-windows-server"></a>在 Windows Server 中的儲存體中最新消息

>適用於：Windows Server 2019，Windows Server 2016 中，Windows Server （半年通道）

本主題說明在 Windows Server 2019，Windows Server 2016 中的儲存體中新增和變更功能和 Windows Server 半年通道釋出。

## <a name="whats-new-in-storage-in-windows-server-2019-and-windows-server-version-1903"></a>在 Windows Server 2019 和 Windows Server 版 1903年的儲存體中最新消息

這一版的 Windows Server 加入了下列變更和技術。

### <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>本機帳戶、 叢集和 Linux 伺服器，現在會將移轉存放裝置移轉服務

儲存體移轉服務可讓您更輕鬆地移轉到新版的 Windows Server 的伺服器。 它提供圖形化工具清查伺服器上的資料，然後將資料和設定傳送至較新的伺服器 — 完全不需要應用程式或使用者不必變更任何項目。

當使用這個版本的 Windows Server 來協調移轉時，我們已新增下列功能：

- 將本機使用者和群組移轉至新的伺服器
- 從容錯移轉叢集移轉儲存體
- 使用 Samba 的 Linux 伺服器從移轉存放裝置
- 使用 Azure 檔案同步來更輕鬆地同步至 Azure 的已移轉的共用
- 移轉到新的網路，例如 Azure

如需儲存體移轉服務的詳細資訊，請參閱[儲存體移轉服務概觀](storage-migration-service/overview.md)。

### <a name="system-insights-disk-anomaly-detection"></a>系統 Insights 磁碟異常偵測

[系統 Insights](../manage/system-insights/overview.md)是預測性分析功能，在本機上分析 Windows Server 系統資料，並提供深入了解伺服器的正常運作。 它隨附許多內建功能，但我們已新增能夠在安裝額外的功能，透過 Windows Admin Center，從磁碟異常偵測。

磁碟異常偵測是一項新功能會反白顯示時磁碟運作*不同*比平常。 雖然不同不一定是一件壞事，看到這些異常時間可以是在系統上的問題進行疑難排解時很有幫助。

這項功能也適用於執行 Windows Server 2019 的伺服器。

### <a name="windows-admin-center-enhancements"></a>Windows Admin Center 增強功能

新版的 Windows Admin Center 已推出，Windows Server 中加入新的功能。 如需最新的功能資訊，請參閱[Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md)。

## <a name="whats-new-in-storage-in-windows-server-2019-and-windows-server-version-1809"></a>在 Windows Server 2019 和 Windows Server 版 1809年的儲存體中最新消息

這一版的 Windows Server 加入了下列變更和技術。

### <a name="manage-storage-with-windows-admin-center"></a>管理 Windows Admin Center 使用的儲存體

[Windows Admin Center](../manage/windows-admin-center/overview.md)是管理伺服器、 叢集、 與儲存空間直接存取 Windows 10 電腦的超交集基礎結構的新本機部署中，瀏覽器型應用程式。 它是 Windows 之外不需要額外收費，並且可供生產環境中使用。

若要老實說，Windows Admin Center 屬於不同下載，執行 Windows Server 2019 和其他版本的 Windows，但這是新，我們不想要錯過這項資訊...

### <a name="storage-migration-service"></a>存放裝置移轉服務

儲存空間移轉服務是新的技術，可讓您更輕鬆地將伺服器移轉至較新版本的 Windows Server。 它提供圖形化工具，可用於清查伺服器上的資料、將資料與設定傳輸到較新的伺服器，然後選擇性地將舊伺服器的身分識別移動至新伺服器，因此應用程式和使用者不需要變更任何項目。 如需詳細資訊，請參閱[存放裝置移轉服務](storage-migration-service/overview.md)。

### <a id="storage-spaces-direct"></a>儲存空間直接存取 （2019年僅限 Windows Server)

有幾個的儲存空間直接存取在 Windows Server 2019 的增強功能 （儲存空間直接存取未包含在 Windows Server 半年通道）：

- **重複資料刪除和壓縮 ReFS 磁碟區**

    具有重複資料刪除和壓縮 ReFS 檔案系統的相同磁碟區上儲存最多十倍更多資料。 (它有[只需要按一下](https://www.youtube.com/watch?v=PRibTacyKko&feature=youtu.be)開啟 Windows Admin Center 使用。)可變大小區塊存放區，以使用選用的壓縮最大化節省率，而多執行緒的後置處理架構，會保留效能影響最小。 支援的磁碟區最多 64 TB 及將重複資料刪除第一次的 4 TB 的每個檔案。

- **持續性記憶體的原生支援**

    運用對持續性記憶體模組的原生儲存空間直接存取來發揮前所未有的效能，包括 Intel® Optane™ DC PM 和 NVDIMM-N。 使用持續性記憶體做為快取來加速使用中工作集，或做為容量來保證一致的低延遲，低至以毫秒為單位。 管理持續性記憶體的方式，就如同在 PowerShell 或 Windows Admin Center 中管理任何其他磁碟機一樣。

- **在邊緣的雙節點超交集基礎結構的巢狀恢復功能**

    受 RAID 5+1 啟發的全新軟體復原選項，即使同時有兩部硬體失敗也不受影響。 運用巢狀復原功能，即使同時有一個伺服器節點關閉而另一個伺服器節點中的磁碟機故障，雙節點的儲存空間直接存取叢集也能為應用程式和虛擬機器提供持續可存取的儲存空間。

- **兩部伺服器叢集使用 USB 快閃磁碟機為見證**

    做為見證，以在兩部伺服器叢集中使用低成本 USB 快閃磁碟機插到您的路由器。 如果伺服器當機時，然後回復連線，USB 磁碟機叢集就會知道哪些伺服器有最新的資料。 如需詳細資訊，請參閱 < [Microsoft 部落格的儲存體](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/)。

- **Windows Admin Center**

    運用 Windows Admin Center 中的全新[特殊用途儀表板](../manage/windows-admin-center/use/manage-hyper-converged.md)和體驗來管理及監控儲存空間直接存取。 只需按幾下滑鼠即可建立、開啟、擴充或刪除磁碟區。 可監控大至整體叢集小至個別 SSD 或 HDD 的效能，例如 IOPS 和 IO 延遲。 對於 Windows Server 2016 和 Windows Server 2019 為免費提供。

- **效能歷程記錄**

    使用[內建歷程](storage-spaces/performance-history.md)，輕鬆掌握資源利用率和效能。 跨越運算、記憶體、網路和儲存空間的超過 50 個基本計數器，會自動收集並在叢集上存放高達一年。 最棒的是，完全無須執行任何安裝、設定或啟動動作，就會開始運作。 可在 Windows Admin Center 中視覺化呈現以及在 PowerShell 中查詢並處理。

- **調整最多 4 PB，每個叢集**

    達到 PB 等級規模 – 適合用於媒體、備份和保存使用案例。 在 Windows Server 2019 中，儲存空間直接存取支援每個儲存集區高達 4 PB = 4,000 TB 的原始容量。 相關容量指導方針也增加了：例如，您可以建立兩倍數量的磁碟區 (64 個，而不是 32 個)，每個磁碟區也比以前大兩倍 (64 TB，而不是 32 TB)。 將多個叢集拼接到一起[設定叢集](storage-spaces/cluster-sets.md)的更大的小數位數，一個儲存體命名空間內。 如需詳細資訊，請參閱 < [Microsoft 部落格的儲存體](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/)。

- **鏡像加速同位檢查的速度會 2 X**

    使用鏡像加速的同位，您可以建立部分鏡像、部分同位的儲存空間直接存取磁碟區，例如混合 RAID-1 和 RAID-5/6 來充分發揮兩者的優點。 (它有[比您想像的更容易](https://www.youtube.com/watch?v=R72QHudqWpE)Windows Admin Center 中。)在 Windows Server 2019，鏡像加速同位檢查的效能是一倍以上，相對於 Windows Server 2016 由於最佳化。

- **磁碟機延遲極端值偵測**

    運用主動監控和內建極端值偵測 (受 Microsoft Azure 長期的成功方法所啟發) 輕鬆識別異常延遲的磁碟機。 無論是平均延遲或是更細微的項目 (例如突出的第 99 百分位數延遲)，都會在 PowerShell 和 Windows Admin Center 中將緩慢的磁碟機自動標示為「異常延遲」狀態。

- **以手動方式分隔的磁碟區來提高容錯的配置**

    這可讓系統管理員以手動方式分隔儲存空間直接存取磁碟區的配置。 這麼做讓可能會大幅增加容錯功能，某些狀況下，但會施加一些新增的管理考量和複雜度。 如需詳細資訊，請參閱 <<c0> [ 分隔的磁碟區配置](storage-spaces/delimit-volume-allocation.md)。

### <a name="storage-replica2019"></a>儲存體複本

有數個改善功能[儲存體複本](storage-replica/storage-replica-overview.md)在此版本中：

#### <a name="storage-replica-in-windows-server-standard-edition"></a>在 Windows Server Standard Edition 中的儲存體複本

您現在可以使用 Windows Server，除了 Datacenter Edition 的 Standard Edition 中使用儲存體複本。 在 Windows Server Standard Edition 上執行的儲存體複本具有下列限制：

- 儲存體複本複寫的單一磁碟區，而不是無限數量的磁碟區。
- 磁碟區可以有最多 2 TB，而不是大小無限制的大小。

#### <a name="storage-replica-log-performance-improvements"></a>儲存體複本記錄檔效能改進

我們也改善了儲存體複本記錄檔可如何追蹤複寫，改善複寫輸送量和延遲，特別是在全快閃存放裝置，以及彼此之間進行複寫的儲存空間直接存取叢集上。

若要獲得更高的效能，複寫群組的所有成員都必須都執行 Windows Server 2019。

#### <a name="test-failover"></a>測試容錯移轉

您現在可以暫時用於測試到目的地伺服器上裝載的複寫的儲存體快照集或備份之用。 如需詳細資訊，請參閱[儲存體複本的常見問題集](https://aka.ms/srfaq)。

#### <a name="windows-admin-center-support"></a>Windows Admin Center 支援

複寫的圖形化管理現已支援在 Windows Admin Center 透過伺服器管理員工具。 這包括伺服器對伺服器複寫、 叢集對叢集，以及延展式叢集複寫。

#### <a name="miscellaneous-improvements"></a>其他改進

儲存體複本也會包含下列增強功能：

-   改變非同步延展叢集行為，以便立即發生自動容錯移轉
-   多個錯誤修正

### <a name="smb"></a>SMB

- **SMB1 和來賓驗證移除**:Windows Server 不會再預設會安裝在 SMB1 用戶端和伺服器。 此外，在 SMB2 和更新版本中以客體身分驗證的功能也預設為關閉。 如需詳細資訊，請檢閱 [在 Windows 10 版本 1709 及 Windows Server 版本 1709 中，預設不安裝 SMBv1](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server)。 

- **SMB2/SMB3 安全性與相容性**:已新增安全性及應用程式相容性的其他選項，包括停用 oplocks SMB2 + 中的舊版應用程式，以及需要簽章或加密從用戶端的每個連線為基礎的功能。 如需詳細資訊，請檢閱 SMBShare PowerShell 模組說明。

### <a name="data-deduplication"></a>重複資料刪除

- **現在重複資料刪除支援 ReFS**:您不再必須選擇使用 ReFS 現代的檔案系統的優點，以及重複資料刪除： 現在，啟用重複資料刪除，只要您可以啟用 ReFS。 透過 ReFS 提升儲存效率，增加 95% 以上。
- **重複資料刪除磁碟區最佳化入口流量/出口流量的資料連接埠 API**:開發人員現在可以利用重複資料刪除包含有關如何儲存資料，有效率地以磁碟區，伺服器之間移動資料，並有效率地叢集的知識。

### <a name="file-server-resource-manager"></a>檔案伺服器資源管理員

在服務啟動時，Windows Server 2019 會包括能夠防止檔案伺服器資源管理員服務建立變更日誌 （也稱為 USN 日誌） 上所有磁碟區。 這可以節省每個磁碟區的空間，但會停用即時檔案分類。 如需詳細資訊，請參閱[檔案伺服器資源管理員概觀](fsrm/fsrm-overview.md)。

## <a name="whats-new-in-storage-in-windows-server-version-1803"></a>在 Windows Server，版本 1803年中的儲存體中最新消息

### <a name="file-server-resource-manager"></a>檔案伺服器資源管理員

Windows Server 1803 版包括能夠防止檔案伺服器資源管理員服務建立變更日誌 （也稱為 USN 日誌） 上的所有磁碟區在服務啟動時。 這可以節省每個磁碟區的空間，但會停用即時檔案分類。 如需詳細資訊，請參閱[檔案伺服器資源管理員概觀](fsrm/fsrm-overview.md)。

## <a name="whats-new-in-storage-in-windows-server-version-1709"></a>在 Windows Server 1709 版中的儲存體中最新消息

Windows Server 1709 版是半年通道中的第一個 Windows Server 版本。 半年通道是軟體保證權益，並完全支援 18 個月，新的版本每六個月的生產環境中。

如需詳細資訊，請參閱 [Windows Server 半年通道概觀](../get-started/semi-annual-channel-overview.md)。

### <a name="storage-replica"></a>儲存體複本

新增儲存體複本的災害復原保護現在已擴充成包括：

- **測試容錯移轉**：掛接目的地存放裝置的選項現在可以透過測試容錯移轉功能來使用。 您可以在目的地節點上暫時掛接已複寫存放裝置的快照集以作測試或備份之用。 如需詳細資訊，請參閱[儲存體複本的常見問題集](https://aka.ms/srfaq)。
- **Windows Admin Center 支援**:複寫的圖形化管理現已支援在 Windows Admin Center 透過伺服器管理員工具。 這包括伺服器對伺服器複寫、 叢集對叢集，以及延展式叢集複寫。

儲存體複本也會包含下列增強功能：

-   改變非同步延展叢集行為，以便立即發生自動容錯移轉
-   多個錯誤修正

### <a name="smb"></a>SMB

- **SMB1 和來賓驗證移除**:Windows Server，版本 1709年不再預設會安裝在 SMB1 用戶端和伺服器。 此外，在 SMB2 和更新版本中以客體身分驗證的功能也預設為關閉。 如需詳細資訊，請檢閱 [在 Windows 10 版本 1709 及 Windows Server 版本 1709 中，預設不安裝 SMBv1](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server)。 

- **SMB2/SMB3 安全性與相容性**:已新增安全性及應用程式相容性的其他選項，包括停用 oplocks SMB2 + 中的舊版應用程式，以及需要簽章或加密從用戶端的每個連線為基礎的功能。 如需詳細資訊，請檢閱 SMBShare PowerShell 模組說明。

### <a name="data-deduplication"></a>重複資料刪除

- **現在重複資料刪除支援 ReFS**:您不再必須選擇使用 ReFS 現代的檔案系統的優點，以及重複資料刪除： 現在，啟用重複資料刪除，只要您可以啟用 ReFS。 透過 ReFS 提升儲存效率，增加 95% 以上。
- **重複資料刪除磁碟區最佳化入口流量/出口流量的資料連接埠 API**:開發人員現在可以利用重複資料刪除包含有關如何儲存資料，有效率地以磁碟區，伺服器之間移動資料，並有效率地叢集的知識。

## <a name="whats-new-in-storage-in-windows-server-2016"></a>Windows Server 2016 中存放裝置的新功能

### <a name="s2d"></a>儲存空間直接存取  
「儲存空間直接存取」能讓您使用具本機磁碟的伺服器，建置高可用且可調整的儲存空間。 它簡化了軟體定義儲存體系統的部署和管理，也解除了先前使用共用磁碟的叢集儲存空間限制，而能夠使用新的磁碟裝置類型 (例如 SATA SSD 和 NVMe 磁碟裝置)。  

**這個變更增加了什麼價值？**  
「儲存空間直接存取」能讓服務提供者和企業使用具本機存放區的業界標準伺服器，建立高可用且可調整的軟體定義儲存空間。 使用具本機存放區的伺服器可降低複雜性、提升延展性，並能夠使用之前不支援的存放裝置 (例如 SATA 固態硬碟) 以降低快閃儲存體成本，或是使用 NVMe 固態硬碟以取得較佳的效能。  

「儲存空間直接存取」免除了共用 SAS 結構的需求，以針對部署及設定做出簡化。 它改為以網路做為存放結構，利用 SMB3 和 SMB 直接傳輸 (RDMA) 來取得高速、低延遲，並能有效運用 CPU 的儲存空間。 如需進行擴充，只需要新增更多伺服器即可增加儲存空間和 I/O 效能  
如需詳細資訊，請參閱 [Windows Server 2016 中的儲存空間直接存取](storage-spaces/storage-spaces-direct-overview.md)。  

**有哪些不同？**  
這是 Windows Server 2016 中的新功能。  

### <a name="storage-replica"></a>儲存體複本

儲存體複本可在伺服器或叢集之間啟用與存放裝置無關、區塊層級及同步的複寫來進行災害復原，以及在站台之間延伸容錯移轉叢集。 同步複寫能以當機時保持一致的磁碟區啟用對實體站台中資料的鏡像，來確保檔案系統層級零資料遺失。 非同步複寫允許都會範圍外的站台擴充功能，但有資料遺失的可能性。  

**這個變更增加了什麼價值？**  
儲存體複寫可讓您執行下列操作︰  

* 針對任務關鍵性工作負載的計畫與非計畫中斷，提供單一廠商災害復原解決方案。
* 使用經證明具可靠性、可調整性和效能的 SMB3 傳輸。
* 將 Windows 容錯移轉叢集延伸至都會距離。
* 針對儲存體和叢集端對端地使用 Microsoft 軟體，例如 Hyper-V、儲存體複本、儲存空間、叢集、向外延展檔案伺服器、SMB3、重複資料刪除，以及 ReFS/NTFS。
* 以下列方式協助降低成本和複雜性： 
    * 與硬體無關，不具有像 DAS 或 SAN 的特定儲存體組態需求。
    * 允許平價儲存體和網路技術。
    * 透過容錯移轉叢集管理員，針對個別的節點和叢集提供易於使用的圖形化管理。
    * 透過 Windows PowerShell，提供完整、大規模的指令碼選項。 
* 協助降低停機時間，並提升 Windows 本身的可靠性和生產力。  
* 提供支援能力、效能標準及診斷功能。  

如需詳細資訊，請參閱 [Windows Server 2016 中的儲存體複本](storage-replica/storage-replica-overview.md)。  

**有哪些不同？**  
這是 Windows Server 2016 中的新功能。  

### <a name="storage-qos"></a>存放裝置服務品質  
您現在可以在 Windows Server 2016 中，使用存放裝置服務品質 (QoS) 集中監視端對端儲存體效能，以及使用 Hyper-V 和 CSV 叢集建立管理原則。  

**這個變更增加了什麼價值？**  
您現在可在 CSV 叢集上建立存放裝置 QoS 原則，並將它們指派給 Hyper-V 虛擬機器上的一或多個虛擬磁碟。 存放裝置效能會隨工作負載和存放裝置負載的變動自動進行調整以符合原則。  

* 每個原則都可以指定要套用到資料流集合 (例如虛擬硬碟、單一或群組的虛擬機器、服務，或租用戶) 的保留 (最小值) 和/或上限 (最大值)。  
* 透過 Windows PowerShell 或 WMI，您可以執行下列工作︰  
    * 在 CSV 叢集上建立原則。
    * 列舉 CSV 叢集上可用的原則。
    * 將原則指派至 Hyper-V 虛擬機器的虛擬硬碟。 
    * 監視原則內每個流程和狀態的效能。  
* 如果有多個虛擬硬碟共用同一個原則，則效能會均勻分散以符合原則之最小值和最大值設定內的要求。 因此，原則可用來管理一個虛擬硬碟、一部虛擬機器、組成服務的多部虛擬機器，或是由租用戶擁有的所有虛擬機器。  

**有哪些不同？**  
這是 Windows Server 2016 中的新功能。 在舊版 Windows Server 中，無法管理最小保留值、透過單一命令來監視叢集中所有虛擬磁碟的流程，以及進行集中式原則管理。  

如需詳細資訊，請參閱[存放裝置服務品質](storage-qos/storage-qos-overview.md)

### <a name="dedup"></a>重複資料刪除  
| 功能 | 新功能或更新功能 | 描述 |
|---------------|----------------|-------------|
| [支援大型磁碟區](data-deduplication/whats-new.md#large-volume-support) | 已更新 | 在 Windows Server 2016 之前，使用者必須特別針對預期的變動設定磁碟區大小，而大於 10 TB 的磁碟區並不是重複資料刪除的良好候選項目。 在 Windows Server 2016 中，重複資料刪除支援**最高 64 TB** 的磁碟區大小。 |
| [支援大型檔案](data-deduplication/whats-new.md#large-file-support) | 已更新 | 在 Windows Server 2016 之前，大小接近 1 TB 的檔案並不是重複資料刪除的良好候選項目。 在 Windows Server 2016 中，已完全支援**最高 1 TB** 大小的檔案。 |
| [適用於 Nano Server 的支援](data-deduplication/whats-new.md#nano-server-support) | 新的 | Windows Server 2016 的新 Nano 伺服器部署選項可以使用並且完全支援重複資料刪除。 |
| [簡化的備份支援](data-deduplication/whats-new.md#simple-backup-support) | 新的 | 在 Windows Server 2012 R2 中，虛擬備份應用程式 (例如 Microsoft 的 [Data Protection Manager](https://technet.microsoft.com/library/hh758173.aspx)) 必須透過一系列的手動設定步驟來取得支援。 在 Windows Server 2016 中，已加入新的預設使用類型 "Backup"，以無接縫地針對虛擬備份應用程式部署重複資料刪除。 |
| [支援叢集 OS 輪流升級](data-deduplication/whats-new.md#cluster-upgrade-support) | 新的 | 重複資料刪除完整支援 Windows Server 2016 新的[叢集 OS 輪流升級](..//failover-clustering/cluster-operating-system-rolling-upgrade.md)功能。 |

### <a name="smb-hardening-improvements"></a>SMB 強化功能改進的 SYSVOL 和 NETLOGON 連線  
在 Windows 10 和 Windows Server 2016 中，用戶端針對網域控制站上 Active Directory 網域服務之預設 SYSVOL 和 NETLOGON 共用的連線，現在需要 SMB 簽署及相互驗證 (例如 Kerberos)。   

**這個變更增加了什麼價值？**  
這項變更降低了攔截式攻擊的可能性。   

**有哪些不同？**  
如果無法使用 SMB 簽署和相互驗證，Windows 10 或 Windows Server 2016 電腦將不會處理網域型的群組原則和指令碼。  

> [!NOTE]  
> 這些設定的登錄值預設將不會顯示，但仍會套用強化原則，直到由群組原則或其他登錄值覆寫為止。  

如需有關這些安全性改進-也稱為 UNC 強化，請參閱 Microsoft 知識庫文件[3000483](https://support.microsoft.com/kb/3000483)和[與 MS15-011 & 與 MS15-014:強化群組原則](https://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy)。  

### <a name="work-folders"></a>工作資料夾
工作資料夾伺服器執行 Windows Server 2016 和工作資料夾用戶端時的改良的變更通知為 Windows 10。

**這個變更增加了什麼價值？**<br>
在 Windows Server 2012 R2 中，將檔案變更同步處理到工作資料夾伺服器時，用戶端不會收到變更的通知，而且最多等待 10 分鐘就會收到更新。  當使用 Windows Server 2016 時，工作資料夾伺服器就會立即通知 Windows 10 用戶端和立即同步處理檔案變更。

**有哪些不同？**<br>
這是 Windows Server 2016 中的新功能。 這需要 Windows Server 2016 工作資料夾伺服器，而且用戶端必須是 Windows 10。

如果您使用的是舊版用戶端，或工作資料夾伺服器是 Windows Server 2012 R2，則用戶端會每隔 10 分鐘繼續輪詢變更。

### <a name="refs"></a>ReFS 
ReFS 的下一個反覆運算支援大規模儲存部署並具備多種工作負載，為您的資料提供可靠性、復原及延展性。     

**這個變更增加了什麼價值？**<br>
ReFS 引入下列改善：

* ReFS 實作新儲存層功能，協助提供更快的效能並提升儲存容量。 此項新功能可以達成：
    * 相同虛擬磁碟上的多種復原類型 (例如，在效能層使用鏡像並在容量層使用同位)
    * 對漂移工作集的回應性提升。  
* 區塊複製的引入大幅改善了 VM 作業的效能，例如 .vhdx 檢查點合併作業。
* 新的 ReFS 掃描工具可讓流失的儲存體復原，並協助回收嚴重損毀的資料。 

**有哪些不同？**<br>
這些為 Windows Server 2016 的新功能。 

## <a name="see-also"></a>另請參閱  
* [Windows Server 2016 中的新功能](../get-started/what-s-new-in-windows-server-2016.md)  
