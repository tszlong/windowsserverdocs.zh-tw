---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: Windows Server 中存放裝置的新功能
ms.prod: windows-server
ms.author: jgerend
manager: dongill
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 05/29/2019
ms.openlocfilehash: cae164de90738996a6c4663dfcbde794b6c2a18e
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473775"
---
# <a name="whats-new-in-storage-in-windows-server"></a>Windows Server 中存放裝置的新功能

>適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

本主題說明 Windows Server 2019、Windows Server 2016 和 Windows Server 半年通道版本中存放裝置的新功能和已變更的功能。

## <a name="whats-new-in-storage-in-windows-server-version-1903"></a>Windows Server （版本1903）中存放裝置的新功能

這一版的 Windows Server 加入了下列變更和技術。

### <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>儲存空間移轉服務現在可遷移本機帳戶、叢集和 Linux 伺服器

儲存空間移轉服務可讓您更輕鬆地將伺服器遷移至較新版本的 Windows Server。 其提供的圖形工具可清查伺服器上的資料，然後將資料和設定轉送至較新的伺服器 — 完全不需要應用程式或使用者變更任何項目。

使用此版本的 Windows Server 來協調移轉時，我們已新增下列功能：

- 將本機使用者和群組遷移到新伺服器
- 從容錯移轉叢集遷移儲存空間
- 從使用 Samba 的 Linux 伺服器遷移儲存空間
- 使用 Azure 檔案同步，更輕鬆地同步已遷移至 Azure 的共用
- 遷移新的網路，例如 Azure

如需有關儲存空間移轉服務的詳細資訊，請參閱[儲存空間移轉服務概觀](storage-migration-service/overview.md)。

### <a name="system-insights-disk-anomaly-detection"></a>系統深入解析的磁碟異常偵測

[系統深入解析](../manage/system-insights/overview.md)是預測性分析功能，可在本機上分析 Windows Server 系統資料，並提供伺服器的功能深入解析。 其隨附許多內建功能，但從磁碟異常偵測開始，我們已新增能夠透過 Windows Admin Center 安裝額外功能的能力。

磁碟異常偵測是一項新功能，可在磁碟出現與平常「不同」** 的行為時醒目提示。 雖然不同不一定是件壞事，但查看這些異常狀態有助於針對系統問題進行疑難排解。

這項功能也適用於執行 Windows Server 2019 的伺服器。

### <a name="windows-admin-center-enhancements"></a>Windows Admin Center 功能強化

新版 Windows Admin Center 已推出，並在 Windows Server 中加入新功能。 如需最新的功能資訊，請參閱 [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md)。

## <a name="whats-new-in-storage-in-windows-server-2019-and-windows-server-version-1809"></a>Windows Server 2019 和 Windows Server （版本1809）中存放裝置的新功能

這一版的 Windows Server 加入了下列變更和技術。

### <a name="manage-storage-with-windows-admin-center"></a>使用 Windows 管理中心管理存放裝置

[Windows 管理中心](../manage/windows-admin-center/overview.md)是新的本機部署、以瀏覽器為基礎的應用程式，可用於管理伺服器、叢集、具有儲存空間直接存取的超融合式基礎結構，以及 Windows 10 電腦。 除了 Windows 之外，它還沒有額外的成本，並且可供生產環境使用。

就公平而言，Windows 管理中心是一種獨立的下載，它會在 Windows Server 2019 和其他版本的 Windows 上執行，但這是新的，我們不希望您錯過它 .。。

### <a name="storage-migration-service"></a>存放裝置移轉服務

儲存空間移轉服務是新的技術，可讓您更輕鬆地將伺服器移轉至較新版本的 Windows Server。 它提供圖形化工具，可用於清查伺服器上的資料、將資料與設定傳輸到較新的伺服器，然後選擇性地將舊伺服器的身分識別移動至新伺服器，因此應用程式和使用者不需要變更任何項目。 如需詳細資訊，請參閱[存放裝置移轉服務](storage-migration-service/overview.md)。

### <a name="storage-spaces-direct-windows-server-2019-only"></a><a id="storage-spaces-direct"></a>儲存空間直接存取（僅限 Windows Server 2019）

Windows Server 2019 中的儲存空間直接存取有一些改良功能（儲存空間直接存取不包含在 Windows Server、半年通道）：

- **ReFS 磁碟區的重複資料刪除和壓縮**

    使用 ReFS 檔案系統的重復資料刪除和壓縮，在相同的磁片區上儲存多達10倍的資料。 （[只需按一下](https://www.youtube.com/watch?v=PRibTacyKko&feature=youtu.be)即可開啟 Windows 系統管理中心。）具有選擇性壓縮的可變大社區塊存放區會最大化節省率，而多執行緒的後置處理架構則會使效能影響降到最低。 支援高達 64 TB 的磁片區，並將刪除重複每個檔案的前 4 TB。

- **原生支援持續性記憶體**

    運用對持續性記憶體模組的原生儲存空間直接存取來發揮前所未有的效能，包括 Intel® Optane™ DC PM 和 NVDIMM-N。 使用持續性記憶體做為快取來加速使用中工作集，或做為容量來保證一致的低延遲，低至以毫秒為單位。 管理持續性記憶體的方式，就如同在 PowerShell 或 Windows Admin Center 中管理任何其他磁碟機一樣。

- **在邊緣的雙節點超融合式基礎結構的巢狀復原力**

    受 RAID 5+1 啟發的全新軟體復原選項，即使同時有兩部硬體失敗也不受影響。 運用巢狀復原功能，即使同時有一個伺服器節點關閉而另一個伺服器節點中的磁碟機故障，雙節點的儲存空間直接存取叢集也能為應用程式和虛擬機器提供持續可存取的儲存空間。

- **使用 USB 快閃磁碟做為見證的兩個伺服器叢集**

    使用插入路由器的低成本 USB 快閃磁片磁碟機，做為兩個伺服器叢集中的見證。 如果伺服器關閉後再備份，則 USB 磁片磁碟機叢集知道哪一台伺服器擁有最新的資料。 如需詳細資訊，請參閱[Microsoft blog 的儲存體](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/)。

- **Windows Admin Center**

    運用 Windows Admin Center 中的全新[特殊用途儀表板](../manage/windows-admin-center/use/manage-hyper-converged.md)和體驗來管理及監控儲存空間直接存取。 只需按幾下滑鼠即可建立、開啟、擴充或刪除磁碟區。 可監控大至整體叢集小至個別 SSD 或 HDD 的效能，例如 IOPS 和 IO 延遲。 對於 Windows Server 2016 和 Windows Server 2019 為免費提供。

- **效能歷程記錄**

    使用[內建歷程](storage-spaces/performance-history.md)，輕鬆掌握資源利用率和效能。 跨越運算、記憶體、網路和儲存空間的超過 50 個基本計數器，會自動收集並在叢集上存放高達一年。 最棒的是，沒有辦法可以安裝、設定或啟動–它只是運作。 可在 Windows Admin Center 中視覺化呈現以及在 PowerShell 中查詢並處理。

- **每個叢集可擴充到最多 4 PB**

    達到 PB 等級規模 – 適合用於媒體、備份和保存使用案例。 在 Windows Server 2019 中，儲存空間直接存取支援每個儲存集區高達 4 PB = 4,000 TB 的原始容量。 相關容量指導方針也增加了：例如，您可以建立兩倍數量的磁碟區 (64 個，而不是 32 個)，每個磁碟區也比以前大兩倍 (64 TB，而不是 32 TB)。 將多個叢集結合成一個叢集[集](storage-spaces/cluster-sets.md)，以便在一個儲存體命名空間內進行更大規模的調整。 如需詳細資訊，請參閱[Microsoft blog 的儲存體](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/)。

- **鏡像加速的同位加快 2 倍**

    使用鏡像加速的同位，您可以建立部分鏡像、部分同位的儲存空間直接存取磁碟區，例如混合 RAID-1 和 RAID-5/6 來充分發揮兩者的優點。 （這比您在 Windows 系統管理中心的[想法更簡單](https://www.youtube.com/watch?v=R72QHudqWpE)）。在 Windows Server 2019 中，由於優化的關係，鏡像加速同位的效能比 Windows Server 2016 多兩倍。

- **磁碟機延遲極端值偵測**

    透過主動式監控和內建極端值偵測，輕鬆地找出具有異常延遲的磁片磁碟機，並 Microsoft Azure 長期且成功的方法。 不論是第99百分位延遲的平均延遲或更細微的東西，緩慢的磁片磁碟機會在 PowerShell 和 Windows 系統管理中心自動標示為「異常延遲」狀態。

- **以手動方式分隔分隔磁碟區配置來增加容錯能力**

    這可讓系統管理員以手動方式分隔儲存空間直接存取中的磁片區配置。 這麼做可以在某些情況下大幅增加容錯能力，但會強加一些額外的管理考慮和複雜度。 如需詳細資訊，請參閱[分隔磁片區配置](storage-spaces/delimit-volume-allocation.md)。

### <a name="storage-replica"></a><a name="storage-replica2019"></a>儲存體複本

在此版本中，[儲存體複本](storage-replica/storage-replica-overview.md)有一些改良功能：

#### <a name="storage-replica-in-windows-server-standard-edition"></a>Windows Server Standard Edition 中的儲存體複本

除了 Datacenter Edition 以外，您現在可以搭配 Windows Server Standard Edition 使用儲存體複本。 在 Windows Server Standard Edition 上執行的儲存體複本具有下列限制：

- 儲存體複本會複寫單一磁片區，而不是不限數目的磁片區。
- 磁片區的大小最高可達 2 TB，而不是無限制大小。

#### <a name="storage-replica-log-performance-improvements"></a>儲存體複本記錄檔效能改進

我們也改進了儲存體複本記錄檔追蹤複寫的方式、改善複寫輸送量和延遲，特別是在所有的快閃儲存體以及彼此之間複寫儲存空間直接存取叢集。

若要獲得更高的效能，複寫群組的所有成員都必須執行 Windows Server 2019。

#### <a name="test-failover"></a>測試容錯移轉

您現在可以暫時將複寫儲存體的快照集掛接在目的地伺服器上，以供測試或備份之用。 如需詳細資訊，請參閱[儲存體複本的常見問題集](https://aka.ms/srfaq)。

#### <a name="windows-admin-center-support"></a>Windows Admin Center 支援

透過 [伺服器管理員] 工具，Windows 管理中心現已提供複寫的圖形化管理支援。 這包括伺服器對伺服器複寫、叢集對叢集，以及延展叢集複寫。

#### <a name="miscellaneous-improvements"></a>其他改良功能

儲存體複本也包含下列增強功能：

-   改變異步延展叢集行為，以便立即進行自動容錯移轉
-   多個 bug 修正

### <a name="smb"></a>SMB

- **Smb1 和來賓驗證移除**： Windows Server 預設不會再安裝 smb1 用戶端和伺服器。 此外，在 SMB2 和更新版本中以客體身分驗證的功能也預設為關閉。 如需詳細資訊，請檢閱[在 Windows 10 版本 1709 及 Windows Server 版本 1709 中，預設不安裝 SMBv1](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server)。

- **SMB2/SMB3 安全性與相容性**：已新增安全性及應用程式相容性的額外選項，包括可在 SMB2+ 中停用舊版應用程式 Oplocks 的功能，以及向用戶端要求對每一連線的簽署或加密。 如需詳細資訊，請檢閱 SMBShare PowerShell 模組說明。

### <a name="data-deduplication"></a>重複資料刪除

- **重複資料刪除現在支援 ReFS**：再也不必權衡新式檔案系統在 ReFS 和重複資料刪除方面的優勢，從兩者之間做出選擇：您現在只要可以啟用 ReFS，也就可以啟用重複資料刪除。 透過 ReFS 提升儲存效率，增加 95% 以上。
- **適用於重複資料刪除磁碟區最佳化輸入/輸出的 DataPort API**：開發人員現在可以利用重複資料刪除功能關於有效率儲存資料方面的優勢，在磁碟區、伺服器和叢集之間有效率地移動資料。

### <a name="file-server-resource-manager"></a>File Server Resource Manager

Windows Server 2019 包括在服務啟動時，防止檔案伺服器 Resource Manager 服務在所有磁片區上建立變更日誌（也稱為 USN 日誌）的功能。 這可以節省每個磁碟區的空間，但會停用即時檔案分類。 如需詳細資訊，請參閱[檔案伺服器資源管理員概觀](fsrm/fsrm-overview.md)。

## <a name="whats-new-in-storage-in-windows-server-version-1803"></a>Windows Server （版本1803）中存放裝置的新功能

### <a name="file-server-resource-manager"></a>File Server Resource Manager

Windows Server 1803 版包含在服務啟動時，防止檔案伺服器 Resource Manager 服務在所有磁片區上建立變更日誌（也稱為 USN 日誌）的功能。 這可以節省每個磁碟區的空間，但會停用即時檔案分類。 如需詳細資訊，請參閱[檔案伺服器資源管理員概觀](fsrm/fsrm-overview.md)。

## <a name="whats-new-in-storage-in-windows-server-version-1709"></a>Windows Server （版本1709）中存放裝置的新功能

Windows Server （版本1709）是半年通道中的第一個 Windows Server 版本。 「半年通道」是一項軟體保證權益，在生產環境中完全支援18個月，每六個月有一個新版本。

如需詳細資訊，請參閱 [Windows Server 半年通道概觀](../get-started/semi-annual-channel-overview.md)。

### <a name="storage-replica"></a>儲存體複本

儲存體複本所新增的嚴重損壞修復保護現在已擴充為包含：

- **測試容錯移轉**：掛接目的地存放裝置的選項現在可以透過測試容錯移轉功能來使用。 您可以在目的地節點上暫時掛接已複寫存放裝置的快照集以作測試或備份之用。 如需詳細資訊，請參閱[儲存體複本的常見問題集](https://aka.ms/srfaq)。
- **Windows 系統管理中心支援**： Windows 系統管理中心已透過伺服器管理員工具提供複寫的圖形化管理支援。 這包括伺服器對伺服器複寫、叢集對叢集，以及延展叢集複寫。

儲存體複本也包含下列增強功能：

-   改變異步延展叢集行為，以便立即進行自動容錯移轉
-   多個 bug 修正

### <a name="smb"></a>SMB

- **SMB1 與客體驗證移除**：Windows Server 版本 1709 不再預設安裝 SMB1 用戶端及伺服器。 此外，在 SMB2 和更新版本中以客體身分驗證的功能也預設為關閉。 如需詳細資訊，請檢閱[在 Windows 10 版本 1709 及 Windows Server 版本 1709 中，預設不安裝 SMBv1](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server)。

- **SMB2/SMB3 安全性與相容性**：已新增安全性及應用程式相容性的額外選項，包括可在 SMB2+ 中停用舊版應用程式 Oplocks 的功能，以及向用戶端要求對每一連線的簽署或加密。 如需詳細資訊，請檢閱 SMBShare PowerShell 模組說明。

### <a name="data-deduplication"></a>重複資料刪除

- **重複資料刪除現在支援 ReFS**：再也不必權衡新式檔案系統在 ReFS 和重複資料刪除方面的優勢，從兩者之間做出選擇：您現在只要可以啟用 ReFS，也就可以啟用重複資料刪除。 透過 ReFS 提升儲存效率，增加 95% 以上。
- **適用於重複資料刪除磁碟區最佳化輸入/輸出的 DataPort API**：開發人員現在可以利用重複資料刪除功能關於有效率儲存資料方面的優勢，在磁碟區、伺服器和叢集之間有效率地移動資料。

## <a name="whats-new-in-storage-in-windows-server-2016"></a>Windows Server 2016 中存放裝置的新功能

### <a name="storage-spaces-direct"></a><a name="s2d"></a>儲存空間 Direct
「儲存空間直接存取」能讓您使用具本機磁碟的伺服器，建置高可用且可調整的儲存空間。 它簡化了軟體定義儲存體系統的部署和管理，也解除了先前使用共用磁碟的叢集儲存空間限制，而能夠使用新的磁碟裝置類型 (例如 SATA SSD 和 NVMe 磁碟裝置)。

**這個變更增加了什麼價值？**
「儲存空間直接存取」能讓服務提供者和企業使用具本機存放區的業界標準伺服器，建立高可用且可調整的軟體定義儲存空間。 使用具本機存放區的伺服器可降低複雜性、提升延展性，並能夠使用之前不支援的存放裝置 (例如 SATA 固態硬碟) 以降低快閃儲存體成本，或是使用 NVMe 固態硬碟以取得較佳的效能。

「儲存空間直接存取」免除了共用 SAS 結構的需求，以針對部署及設定做出簡化。 它改為以網路做為存放結構，利用 SMB3 和 SMB 直接傳輸 (RDMA) 來取得高速、低延遲，並能有效運用 CPU 的儲存空間。 若要相應放大，只需新增更多伺服器來增加儲存容量和 i/o 效能以取得詳細資訊，請參閱[Windows Server 2016 中的儲存空間直接存取](storage-spaces/storage-spaces-direct-overview.md)。

**有哪些不同？**
這是 Windows Server 2016 中的新功能。

### <a name="storage-replica"></a><a name="storage-replica"></a>儲存體複本

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

### <a name="storage-quality-of-service"></a><a name="storage-qos"></a>存放裝置服務品質
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

### <a name="data-deduplication"></a><a name="dedup"></a>重複資料刪除
| 功能 | 新功能或更新功能 | 描述 |
|---------------|----------------|-------------|
| [支援大型磁片區](data-deduplication/whats-new.md#large-volume-support) | 已更新 | 在 Windows Server 2016 之前，使用者必須特別針對預期的變動設定磁碟區大小，而大於 10 TB 的磁碟區並不是重複資料刪除的良好候選項目。 在 Windows Server 2016 中，重復資料刪除支援**最多 64 TB**的磁片區大小。 |
| [大型檔案的支援](data-deduplication/whats-new.md#large-file-support) | 已更新 | 在 Windows Server 2016 之前，大小接近 1 TB 的檔案並不是重複資料刪除的良好候選項目。 在 Windows Server 2016 中，完全支援**高達 1 TB**的檔案。 |
| [支援 Nano 伺服器](data-deduplication/whats-new.md#nano-server-support) | 新增 | Windows Server 2016 的新 Nano 伺服器部署選項可以使用並且完全支援重複資料刪除。 |
| [簡化的備份支援](data-deduplication/whats-new.md#simple-backup-support) | 新增 | 在 Windows Server 2012 R2 中，虛擬備份應用程式 (例如 Microsoft 的 [Data Protection Manager](https://technet.microsoft.com/library/hh758173.aspx)) 必須透過一系列的手動設定步驟來取得支援。 在 Windows Server 2016 中，已加入新的預設使用類型 "Backup"，以無接縫地針對虛擬備份應用程式部署重複資料刪除。 |
| [支援叢集 OS 輪流升級](data-deduplication/whats-new.md#cluster-upgrade-support) | 新增 | 重複資料刪除完整支援 Windows Server 2016 新的[叢集 OS 輪流升級](..//failover-clustering/cluster-operating-system-rolling-upgrade.md)功能。 |

### <a name="smb-hardening-improvements-for-sysvol-and-netlogon-connections"></a><a name="smb-hardening-improvements"></a>針對 SYSVOL 和 NETLOGON 連線的 SMB 強化功能改進
在 Windows 10 和 Windows Server 2016 中，用戶端針對網域控制站上 Active Directory 網域服務之預設 SYSVOL 和 NETLOGON 共用的連線，現在需要 SMB 簽署及相互驗證 (例如 Kerberos)。

**這個變更增加了什麼價值？**
這項變更可降低攔截式攻擊的可能性。

**有哪些不同？**
如果無法使用 SMB 簽署和相互驗證，Windows 10 或 Windows Server 2016 電腦將不會處理網域型的群組原則和指令碼。

> [!NOTE]
> 這些設定的登錄值預設將不會顯示，但仍會套用強化原則，直到由群組原則或其他登錄值覆寫為止。

如需這些安全性改進 (也稱為 UNC 強化) 的詳細資訊，請參閱 Microsoft 知識庫文章 [3000483](https://support.microsoft.com/kb/3000483) 和 [MS15-011 與 MS15-014：強化群組原則](https://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy) (英文)。

### <a name="work-folders"></a>工作資料夾
已改善當工作資料夾伺服器執行 Windows Server 2016 和工作資料夾用戶端為 Windows 10 時的變更通知。

**這個變更增加了什麼價值？**<br>
在 Windows Server 2012 R2 中，將檔案變更同步處理到工作資料夾伺服器時，用戶端不會收到變更的通知，而且最多等待 10 分鐘就會收到更新。  使用 Windows Server 2016 時，工作資料夾伺服器會立即通知 Windows 10 用戶端，而且檔案變更會立即同步處理。

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

## <a name="additional-references"></a>其他參考
* [Windows Server 2016 中的新功能](../get-started/what-s-new-in-windows-server-2016.md)
