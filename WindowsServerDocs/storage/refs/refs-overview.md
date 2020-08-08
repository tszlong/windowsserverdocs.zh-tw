---
title: 復原檔案系統 (ReFS) 概觀
ms.author: gawatu
manager: mchad
ms.topic: article
author: gawatu
ms.date: 06/29/2019
ms.openlocfilehash: 668ee7a0c9e948c12140d3e25309a68ad3b2148b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957265"
---
# <a name="resilient-file-system-refs-overview"></a>復原檔案系統 (ReFS) 概觀

>適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server (半年通道) 

復原檔案系統 (ReFS) 是 Microsoft 最新的檔案系統，設計用來將資料可用性最大化、在各種工作負載間有效率地調整為極大型的資料集，以及透過復原損毀來提供資料完整性。 其會設法因應一組擴充的儲存體案例，並建立基礎供未來創新所用。

## <a name="key-benefits"></a>主要權益

### <a name="resiliency"></a>災害復原

ReFS 所引入的新功能可精準偵測損毀，並在維持上線狀態時修正該類損毀，進而協助提升您資料的完整性及可用性：

- **完整性資料流** - ReFS 對中繼資料使用總和檢查碼，對檔案資料則為予以選用，使 ReFS 可穩固偵測損毀。
- **儲存空間整合** - 與鏡像或同位空間共用時，ReFS 可使用儲存空間提供的資料其他備份檔，自動修復偵測到的損毀。 修復程序可以將範圍局部縮小為毀損的區域並線上執行，而不需要讓任何磁碟區停機。
- **殘餘資料** - 若磁碟區變為損毀且該損毀資料的其他備份檔不存在，ReFS 會從命名空間移除損毀資料。 ReFS 會在處理多數無法修正的損毀時使磁碟區維持上線狀態，但極少情況會需要 ReFS 使磁碟區離線。
- **主動式錯誤修正** - ReFS 除了在讀取與寫入前驗證資料之外，還引入稱作<i>清除程式</i>的資料完整性掃描器。 此清除程式會定期掃描磁碟區、識別隱藏的損毀，以及主動觸發該毀損資料的修復。

### <a name="performance"></a>效能

ReFS 除了提供復原改善外，還引入了供重視效能的虛擬工作負載所用的新功能。 即時階層最佳化、區塊複製及疏鬆 VDL 為 ReFS 功能演化的最佳範例，設計用來支援動態及多種工作負載：

- **[鏡像加速的同位](./mirror-accelerated-parity.md)** - 鏡像加速的同位為您的資料提供不僅效能高，而且容量也很有效率的儲存體。

    - 為了提供效能高且容量有效率的儲存體，ReFS 將磁碟區分為兩個邏輯儲存群組，稱為階層。 這些階層可具備自己專屬的磁碟機及復原類型，讓各個階層針對效能或容量進行最佳化。 部分範例設定包括：

      | 效能層級 | 容量層 |
      | ---------------- | ----------------- |
      | 鏡像 SSD | 鏡像 HDD |
      | 鏡像 SSD | 同位 SSD |
      | 鏡像 SSD | 同位 HDD |

    - 這些階層設定後，ReFS 會加以使用為經常存取的資料提供快速的儲存體，並為非經常存取的資料提供容量有效率的儲存體。
        - 所有寫入都會在效能層發生，而維持在效能層中的大型資料區塊將有效率地即時移至容量層。
        - 如果使用混合式部署 (混用快閃記憶體和 HDD 磁片磁碟機) ，[儲存空間直接存取中的](../storage-spaces/understand-the-cache.md)快取有助於加速讀取，進而降低虛擬化工作負載的資料片段特性影響。 否則，如果使用全閃部署，則也會在效能層級中進行讀取。

> [!NOTE]
> 對於伺服器部署，鏡像加速的同位僅支援[儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md)。 我們建議您只將鏡像加速同位檢查與封存和備份工作負載搭配使用。 針對虛擬化和其他高效能的隨機工作負載，我們建議使用三向鏡像，以獲得更好的效能。

- **高速 VM 作業** - ReFS 引入新功能，特別將目標擺在改善虛擬工作負載的效能：
    - [區塊複製](./block-cloning.md) - 區塊複製會加快複製作業的速度，達成快速且低影響的 VM 檢查點合併作業。
    - 疏鬆 VDL - 疏鬆 VDL 可讓 ReFS 將檔案快速歸零，將建立固定 VHD 所需的時間從 10 幾分鐘降至只需幾秒。

- **變數叢集大小** - ReFS 支援 4K 與 64K 叢集大小。 4K 為多數部署的建議叢集大小，但 64K 叢集適用於大型循序 IO 工作負載。

### <a name="scalability"></a>延展性

ReFS 設計用來支援超大型資料集 (百萬個 TB) 而不會對效能有負面影響，比先前的檔案系統達到更大的規模。

## <a name="supported-deployments"></a>支援的部署

Microsoft 已特別針對一般用途搭配各種設定和工作負載開發 NTFS，不過對於特別需要 ReFS 提供之可用性、復原和/或規模的客戶，Microsoft 支援 ReFS 以在下列設定和案例下使用。

> [!NOTE]
> 所有 ReFS 支援的設定都必須使用[Windows Server Catalog](https://www.WindowsServerCatalog.com)認證的硬體，並符合應用程式需求。

### <a name="storage-spaces-direct"></a>儲存空間 Direct

建議針對虛擬工作負載或網路連接儲存裝置，將 ReFS 部署在儲存空間直接存取上：
- 鏡像加速的同位以及[儲存空間直接存取中的快取](../storage-spaces/understand-the-cache.md)會提供效能高且容量有效率的儲存體。
- 引入區塊複製與疏鬆 VDL 大幅提升了 .vhdx 檔案作業的速度，例如建立、合併及擴充。
- 完整性-串流、線上修復和替代資料複本可讓 ReFS 和儲存空間直接存取共同偵測並修正中繼資料和資料中的儲存控制器和儲存媒體損毀。
- ReFS 提供可擴充和支援大型資料集的功能。

### <a name="storage-spaces"></a>儲存空間

- 完整性-串流、線上修復和替代資料複本可讓 ReFS 和[儲存空間](../storage-spaces/overview.md)共同偵測並修正中繼資料和資料中的儲存控制器和儲存媒體損毀。
- 儲存空間部署也可以使用 ReFS 中的提供的區塊複製和延展性。
- 在具有共用 SAS 主機殼的儲存空間上部署 ReFS，適合用來裝載封存資料和儲存使用者檔。

> [!NOTE]
> 儲存空間支援透過 BusTypes SATA、SAS、NVME，或透過 HBA 連接的本機非卸載式直接連接， (也就是傳遞模式) 的 RAID 控制器。

### <a name="basic-disks"></a>基本磁碟

在基本磁碟上部署 ReFS 最適合用來執行自己的軟體復原和可用性解決方案的應用程式。
- 自行導入復原及可用性軟體解決方案的應用程式可以善加利用完整性資料流、區塊複製，以及擴充和支援大型資料集的功能。

> [!NOTE]
> 基本磁碟包含透過 BusTypes SATA、SAS、NVME 或 RAID 的本機非卸載式直接連接。 基本磁碟不包含儲存空間。

### <a name="backup-target"></a>備份目標

將 ReFS 部署為備份目標，最適合用來執行自己的復原和可用性解決方案的應用程式和硬體。
- 自行導入復原及可用性軟體解決方案的應用程式可以善加利用完整性資料流、區塊複製，以及擴充和支援大型資料集的功能。

> [!NOTE]
> 備份目標包括上述支援的設定。 如需光纖通道和 iSCSI San 的支援詳細資料，請洽詢應用程式和存放裝置陣列廠商。 針對 San，如果需要精簡布建、修剪/取消對應或卸載資料傳輸等功能 (ODX) 是必要的，則必須使用 NTFS。

## <a name="feature-comparison"></a>功能比較

### <a name="limits"></a>限制

| 功能       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| 檔案名稱長度上限 | 255 個 Unicode 字元  | 255 個 Unicode 字元               |
| 路徑名稱長度上限 |32K Unicode 字元 | 32K Unicode 字元                |
| 檔案大小上限 | 35 PB (pb)   | 256 TB               |
| 磁碟區大小上限 | 35 PB                           | 256 TB                |

### <a name="functionality"></a>功能

#### <a name="the-following-features-are-available-on-refs-and-ntfs"></a>您可以在 ReFS 和 NTFS 中使用下列功能：

| 功能       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| BitLocker 加密 | 是 | 是 |
| 重複資料刪除 | 是<sup>1</sup> | 是 |
| 叢集共用磁碟區 (CSV) 支援 | 是<sup>2</sup> | 是 |
| 軟式連結 | 是 | 是 |
| 容錯移轉叢集支援 | 是 | 是 |
| 存取控制清單 | 是 | 是 |
| USN 日誌 | 是 | 是 |
| 變更通知 | 是 | 是 |
| 連接點 | 是 | 是 |
| 掛接點 | 是 | 是 |
| 重新分析點 | 是 | 是 |
| 磁碟區快照 | 是 | 是 |
| 檔案識別碼 | 是 | 是 |
| Oplock | 是 | 是 |
| 疏鬆檔案 | 是 | 是 |
| 已命名的資料流 | 是 | 是 |
| 精簡佈建 | 是<sup>3</sup> | 是 |
| 修剪/取消對應 | 是<sup>3</sup> | 是 |
1. 適用于 Windows Server，版本1709及更新版本。
2. 適用于 Windows Server 2012 R2 和更新版本。
3. 僅限儲存空間

#### <a name="the-following-features-are-only-available-on-refs"></a>只有在 ReFS 中才可以使用下列功能：

| 功能       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| 區塊複製 | 是 | 否 |
| 疏鬆 VDL | 是 | 否 |
| 鏡像加速的同位| 是 (在儲存空間直接存取上) | 否 |

#### <a name="the-following-features-are-unavailable-on-refs-at-this-time"></a>目前下列功能無法在 ReFS 上使用：

| 功能       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| 檔案系統壓縮 | 否 | 是 |
| 檔案系統加密 | 否 | 是 |
| 異動 | 否 | 是 |
| 硬式連結 | 否 | 是 |
| 物件識別碼 | 否 | 是 |
| 卸載的資料傳輸 (ODX)  | 否 | 是 |
| 簡短名稱 | 否 | 是 |
| 擴充屬性 | 否 | 是 |
| 磁碟配額 | 否 | 是 |
| 可開機 | 否 | 是 |
| 分頁檔案支援 | 否 | 是 |
| 在抽取式媒體上受支援 | 否 | 是 |

## <a name="additional-references"></a>其他參考資料

- [ReFS 和 NTFS 的叢集大小建議](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [儲存空間直接存取總覽](../storage-spaces/storage-spaces-direct-overview.md)
- [ReFS 區塊複製](block-cloning.md)
- [ReFS 完整性資料流](integrity-streams.md)
- [使用 ReFSUtil 針對 ReFS 進行疑難排解](../../administration/windows-commands/refsutil.md)
