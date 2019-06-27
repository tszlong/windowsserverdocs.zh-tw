---
title: 儲存空間直接存取概觀
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 06/26/2019
ms.assetid: 8bd0d09a-0421-40a4-b752-40ecb5350ffd
description: 儲存空間直接存取，可讓您使用內部儲存體的叢集伺服器成軟體定義存放裝置解決方案的 Windows Server 的一項功能的概觀。
ms.localizationpriority: medium
ms.openlocfilehash: 98801af7f753e071e27f100f20ed149110c90f66
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407689"
---
# <a name="storage-spaces-direct-overview"></a>儲存空間直接存取概觀

>適用於：Windows Server 2019，Windows Server 2016

儲存空間直接存取會使用業界標準的伺服器搭配本機連接的磁碟機，以比傳統 SAN 或 NAS 陣列更少的成本來建立高可用性、高延展性的軟體定義存放裝置。 其交集或超交集架構會徹底簡化採購及部署，同時功能例如快取、 儲存層及清除編碼，例如 RDMA 網路和 NVMe 磁碟機的最新的硬體創新，以及傳遞無可比擬的效率與效能。

儲存空間直接存取包含在 Windows Server 2019 Datacenter、 Windows Server 2016 Datacenter 及[Windows Server Insider Preview 組建](https://insider.windows.com/for-business-getting-started-server/)。 

儲存空間的其他應用程式，例如共用 SAS 叢集和兩部獨立伺服器，請參閱 <<c0> [ 儲存空間概觀](overview.md)。 如果您要尋找的資訊的 Windows 10 電腦上使用儲存空間，請參閱[Windows 10 中的儲存空間](https://support.microsoft.com/help/12438/windows-10-storage-spaces)。

|       |       |
|   -   |   -   |
| **了解**<br><ul><li>概觀 (您在此處)</li><li>[了解快取](understand-the-cache.md)</li><li>[容錯與儲存空間效率](storage-spaces-fault-tolerance.md)<li>[磁碟機對稱考量](drive-symmetry-considerations.md)</li><li>[了解和監視存放裝置重新同步](understand-storage-resync.md)</li><li>[了解叢集和集區仲裁](understand-quorum.md)</li><li>[叢集集合](cluster-sets.md)</li> | **規劃**<br><ul><li>[硬體需求](storage-spaces-direct-hardware-requirements.md)</li><li>[使用 CSV 記憶體內部讀取快取](csv-cache.md)</li><li>[選擇磁碟機](choosing-drives.md)</li><li>[規劃磁碟區](plan-volumes.md)</li><li>[使用客體 VM 叢集](storage-spaces-direct-in-vm.md)</li><li>[嚴重損壞修復](storage-spaces-direct-disaster-recovery.md)</li> |
| **部署**<br><ul><li>[部署儲存空間直接存取](deploy-storage-spaces-direct.md)</li><li>[建立磁碟區](create-volumes.md)</li><li>[巢狀復原](nested-resiliency.md)</li><li>[設定仲裁](../../failover-clustering/manage-cluster-quorum.md)</li><li>[將儲存空間直接存取叢集升級到 Windows Server 2019](upgrade-storage-spaces-direct-to-windows-server-2019.md)</li><li>[了解和部署持續性記憶體](deploy-pmem.md)</li> | **管理**<br><ul><li>[使用 Windows Admin Center 管理](../../manage/windows-admin-center/use/manage-hyper-converged.md)</li><li>[新增伺服器或磁碟機](add-nodes.md)</li><li>[使伺服器離線以進行維護](maintain-servers.md)</li><li>[移除伺服器](remove-servers.md)</li><li>[延伸磁碟區](resize-volumes.md)</li><li>[刪除磁碟區](delete-volumes.md)</li><li>[更新磁碟機韌體](../update-firmware.md)</li><li>[效能歷程記錄](performance-history.md)</li><li>[限定磁碟區配置](delimit-volume-allocation.md)</li><li>[超交集叢集上使用 Azure 監視器](configure-azure-monitor.md)</li> |
| **疑難排解**<br><ul><li>[疑難排解案例](troubleshooting-storage-spaces.md)</li><li>[健全狀況和操作狀態的疑難排解](storage-spaces-states.md)</li><li>[收集診斷資料與儲存空間直接存取](data-collection.md)</li><li>[存放裝置類別記憶體健康情況管理](Storage-class-memory-health.md)</li> | **近期部落格文章**<br><ul><li>[使用儲存空間直接存取的 13.7 百萬 IOPS： 超交集基礎結構的新業界記錄](https://blogs.technet.microsoft.com/filecab/2018/10/30/windows-server-2019-and-intel-optane-dc-persistent-memory/)</li><li>[超交集基礎結構，在 Windows Server 2019-倒數時鐘會立即啟動 ！](https://blogs.technet.microsoft.com/filecab/2018/10/02/hci-the-countdown-clock-starts-now/)</li><li>[從 Windows Server 高峰會的五個大公告](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap)</li><li>[10,000 的儲存空間直接存取叢集和計算...](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/)</li> |

## <a name="videos"></a>影片

**快速的影片概觀 （5 分鐘）**

> [!Video https://www.youtube-nocookie.com/embed/raeUiNtMk0E]

**儲存空間直接存取在 Microsoft Ignite 2018 （1 小時）**

> [!Video https://www.youtube-nocookie.com/embed/5kaUiW3qo30]

**儲存空間直接存取在 Microsoft Ignite 2017 （1 小時）**

> [!Video https://www.youtube-nocookie.com/embed/YDr2sqNB-3c]

**啟動事件，在 Microsoft Ignite 2016 （1 小時）**

> [!Video https://www.youtube-nocookie.com/embed/LK2ViRGbWs]

## <a name="key-benefits"></a>主要優點

|       |       |
|   -   |   -   |
| ![簡潔](media/storage-spaces-direct-in-windows-server-2016/simplicity-icon.png)   | **為了簡單起見。** 15 分鐘內搞定從執行 Windows Server 2016 的業界標準伺服器到您的第一個儲存空間直接存取叢集。 對於 System Center 使用者而言，部署只需一個核取方塊。       |
| ![無可比擬的效能](media/storage-spaces-direct-in-windows-server-2016/performance-icon.png)   | **無可比擬的效能。** 不論是全快閃式或混合式，由於儲存空間直接存取的 Hypervisor 內嵌架構、其內建的讀取/寫入快取，以及對於在 PCIe 匯流排上直接裝載尖端 NVMe 磁碟機的支援，讓它具有一致性且低延遲的特性，輕鬆超越[每部伺服器 150,000 個混合式 4k 隨機 IOPS](https://blogs.technet.microsoft.com/filecab/2016/07/26/storage-iops-update-with-storage-spaces-direct/)。      |
| ![容錯移轉](media/storage-spaces-direct-in-windows-server-2016/fault-tolerance-icon.png)   | **容錯功能。** 內建的復原功能會使用持續可用性，來處理磁碟機、伺服器或元件失敗。 也可針對較大型的部署設定[底座和機架容錯](../../failover-clustering/fault-domains.md)。 當硬體故障時，只需進行交換；軟體會自行修復，不需複雜的管理步驟。       |
| ![資源效率](media/storage-spaces-direct-in-windows-server-2016/efficiency-icon.png)   | **資源效率。** 清除編碼可提供多達 2.4 倍的儲存空間效率，透過獨特的創新 (例如本機重建程式碼和 ReFS 即時分層) 來將這些獲益延展至硬碟機和混合式熱/冷工作負載，同時將 CPU 耗用量降到最低，以便讓資源回到最需要它們的地方 (VM)。       |
| ![管理性](media/storage-spaces-direct-in-windows-server-2016/manageability-icon.png)   | **管理性**。 使用[存放裝置 QoS 控制項](../storage-qos/storage-qos-overview.md)，利用每個 VM 之 IOPS 的最小值和最大值限制來抑制過度忙碌的 VM。 [健全狀況服務](../../failover-clustering/health-service-overview.md)提供連續的內建監視和警示，而新的 API 可讓您輕鬆收集豐富、整個叢集的效能和容量計量。      |
| ![延展性](media/storage-spaces-direct-in-windows-server-2016/scalability-icon.png)   | **延展性**。 如果每個叢集的儲存空間最大可達 1 拍位元組 (1,000 TB)，則最多可擴展到 16 部伺服器和超過 400 個磁碟機。 若要向外延展，只需新增磁碟機或新增更多伺服器；儲存空間直接存取就會自動將新的磁碟機上架，並開始使用它們。 存放裝置效率與效能在規模上可如預期般提升。       |

## <a name="deployment-options"></a>部署選項

儲存空間直接存取是為兩個不同的部署選項而設計：

### <a name="converged"></a>交集

**儲存體和計算在不同的叢集。** 交集的部署選項 (也就是「分離式」) 會在儲存空間直接存取上方設定向外延展檔案伺服器 (SoFS) 層，以便透過 SMB3 檔案共用提供網路連接的儲存空間。 這樣就能為從存放裝置叢集獨立出來的電腦/工作負載調整範圍，對於 Hyper-V IaaS (基礎結構即服務) 等適用於服務提供者和企業的大規模部署非常重要。

![儲存空間直接存取使用「向外延展檔案伺服器」功能向其他伺服器或叢集的 Hyper-V VM 提供儲存空間](media/storage-spaces-direct-in-windows-server-2016/converged-minimal.png)

### <a name="hyper-converged"></a>超交集

**計算和儲存體的一個叢集。** 超交集的部署選項會在提供儲存空間以便在本機磁碟區上儲存其檔案的伺服器上，直接執行 Hyper-V 虛擬機器或 SQL Server 資料庫。 這樣就不需要設定檔案伺服器存取權和權限，並可降低小至中型企業或遠端辦公室/分公司部署的硬體成本。 請參閱[部署儲存空間直接](deploy-storage-spaces-direct.md)。

![儲存空間直接存取向同一個叢集中的 Hyper-V VM 提供儲存空間](media/storage-spaces-direct-in-windows-server-2016/hyper-converged-minimal.png)

## <a name="how-it-works"></a>運作方式

儲存空間直接存取是在 Windows Server 2012 中首次引進的儲存空間演進。 它會利用 Windows Server 中您現在熟知的許多功能，例如，容錯移轉叢集、叢集共用磁碟區 (CSV) 檔案系統、伺服器訊息區 (SMB) 3，當然還包括儲存空間。 它也引進了新技術，最值得注意的是軟體存放匯流排。

以下是儲存空間直接存取堆疊的概觀：

![儲存空間直接存取堆疊](media/storage-spaces-direct-in-windows-server-2016/converged-full-stack.png)

**網路硬體。** 儲存空間直接存取會使用 SMB3 (包括 SMB Direct 與 SMB 多重通道)，透過乙太網路在伺服器之間進行通訊。 我們強烈建議 10+ GbE 搭配遠端直接記憶體存取 (RDMA)，iWARP 或 RoCE。

**儲存硬體。** 從 2 到 16 部伺服器，以及本機連接的 SATA、SAS 或 NVMe 磁碟機。 每部伺服器必須至少有 2 個固態磁碟機，以及至少 4 個額外的磁碟機。 SATA 和 SAS 裝置應該位於主機匯流排介面卡 (HBA) 和 SAS 擴展器後面。 我們強烈建議使用由我們的合作夥伴所推出的精心工程設計且廣泛驗證的平台 (敬請期待)。

**容錯移轉叢集。** Windows Server 的內建叢集功能可用來連接伺服器。

**軟體存放匯流排。** 軟體存放匯流排是儲存空間直接存取的新功能。 它橫跨叢集並建立軟體定義的存放裝置網狀架構，因此，所有伺服器都能看見彼此的所有本機磁碟機。 您可以將它視為取代既昂貴又嚴格的光纖通道或共用 SAS 纜線。

**存放匯流排層快取。** 軟體存放匯流排動態繫結到速度較慢的磁碟機 (例如 Hdd) 提供伺服器端的讀取/寫入快取，加速 IO 並提高輸送量的最快的磁碟機存在 (例如 SSD)。

**儲存體集區。** 形成儲存空間基礎的磁碟機集合稱為儲存集區。 它會自動建立，而且會自動探索所有適合的磁碟機並加以新增。 強烈建議您在每個叢集中使用一個叢集搭配預設設定。 如需深入了解，請閱讀我們的[深入探討儲存集區](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/) (英文)。

**儲存空間。** 儲存空間會使用[鏡像、清除編碼或兩者](storage-spaces-fault-tolerance.md)，來為虛擬「磁碟」提供容錯功能。 您可以將它視為集區中使用磁碟機之由分散式軟體定義的 RAID。 在儲存空間直接存取中，儘管也會提供底座和機架容錯功能，但這些虛擬磁碟通常能夠復原兩個同時發生的磁碟或伺服器失敗 (例如 3 向鏡像，每個資料複本都位於不同的伺服器上)。

**彈性檔案系統 (ReFS)。** ReFS 是基於虛擬化目的而建置的首要檔案系統。 其中包括大幅加速 .vhdx 檔案操作 (例如建立、擴充，以及合併檢查點)，以及內建的總和檢查碼來偵測並修正位元錯誤。 它還引進了即時層，可根據使用量，在所謂的「熱」和「冷」儲存層之間即時輪換資料。

**叢集共用磁碟區。** CSV 檔案系統會將所有 ReFS 磁碟區統一為可透過任何伺服器存取的單一命名空間，如此一來，對每部伺服器來說，每個磁碟區的外觀與行為就像是其本機掛接的。

**向外延展檔案伺服器。** 這最後一層僅適用於交集的部署。 它會透過網路使用 SMB3 存取通訊協定為使用者提供遠端檔案存取 (例如執行 Hyper-V 的其他叢集)，有效地使儲存空間直接存取轉換為網路連接儲存裝置 (NAS)。

## <a name="customer-stories"></a>客戶案例

有[10,000 叢集](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/)全球各地執行 儲存空間直接存取。 各種規模的組織，從小型企業到大型企業和政府部署數百個節點，部署只是兩個節點，取決於儲存空間直接存取其關鍵應用程式和基礎結構。

請瀏覽[Microsoft.com/HCI](https://www.microsoft.com/hci)讀取他們的故事：

[![方格中的客戶標誌](media/storage-spaces-direct-in-windows-server-2016/customer-stories.png)](https://www.microsoft.com/hci)

## <a name="management-tools"></a>管理工具

下列工具可用來管理和/或監視儲存空間直接存取使用中：

| 名稱 | 圖形化或命令列嗎？ | 付費服務，或包含嗎？ |
|-----------------|----------------------------|-------------------|
| [Windows Admin Center](../../manage/windows-admin-center/overview.md)     | 圖形    | 內含 |
| 伺服器管理員和容錯移轉叢集管理員                                 | 圖形    | 內含 |
| Windows PowerShell                                                        | 命令列 | 內含 |
| [System Center Virtual Machine Manager (SCVMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-storage-spaces-direct-vmm) <br>& [Operations Manager (SCOM)](https://www.microsoft.com/download/details.aspx?id=54700) | 圖形    | 付費     |

## <a name="get-started"></a>立即開始

[在 Microsoft Azure 中](https://blogs.technet.microsoft.com/filecab/2016/05/05/s2dazuretp5/)，立即試用儲存空間直接存取，或從 [Windows Server 評估版](https://go.microsoft.com/fwlink/?linkid=842602)下載 Windows Server 的 180 天授權評估版。

## <a name="see-also"></a>另請參閱

- [容錯與儲存空間效率](storage-spaces-fault-tolerance.md)
- [儲存體複本](../storage-replica/storage-replica-overview.md)
- [儲存體，且 Microsoft 部落格](https://blogs.technet.microsoft.com/filecab/)
- [搭配 iWARP 的儲存空間直接存取輸送量](https://blogs.technet.microsoft.com/filecab/2017/03/13/storage-spaces-direct-throughput-with-iwarp) (TechNet 部落格)
- [容錯移轉叢集的 Windows Server 最新消息](../../failover-clustering/whats-new-in-failover-clustering.md)  
- [存放裝置服務品質](../storage-qos/storage-qos-overview.md)
- [Windows IT 專業人員支援](https://www.microsoft.com/itpro/windows/support)
