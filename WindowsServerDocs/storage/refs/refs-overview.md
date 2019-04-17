---
title: "復原檔案系統 (ReFS) 概觀"
ms.prod: windows-server-threshold
ms.author: gawatu
ms.manager: dmoss
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 1/3/2016
ms.assetid: 
ms.openlocfilehash: 4460df978f2a3d5e30e43b82cf952ed37f3d91a9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="resilient-file-system-refs-overview"></a>復原檔案系統 (ReFS) 概觀
>適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

復原檔案系統 (ReFS) 是 Microsoft 最新的檔案系統，設計用來將資料可用性最大化、在各種工作負載間有效率地調整為極大型的資料集，以及透過復原損毀來提供資料完整性。 其會設法因應一組擴充的儲存體案例，並建立基礎供未來創新所用。 

## <a name="key-benefits"></a>主要優點

### <a name="resiliency"></a>復原 
ReFS 所引入的新功能可精準偵測損毀，並在維持上線狀態時修正該類損毀，進而協助提升您資料的完整性及可用性： 

- **完整性資料流** - ReFS 對中繼資料使用總和檢查碼，對檔案資料則為予以選用，使 ReFS 可穩固偵測損毀。 
- **儲存空間整合** - 與鏡像或同位空間共用時，ReFS 可使用儲存空間提供的資料其他備份檔，自動修復偵測到的損毀。 修復程序可以將範圍局部縮小為毀損的區域並線上執行，而不需要讓任何磁碟區停機。
- **殘餘資料** - 若磁碟區變為損毀且該損毀資料的其他備份檔不存在，ReFS 會從命名空間移除損毀資料。 ReFS 會在處理多數無法修正的損毀時使磁碟區維持上線狀態，但極少情況會需要 ReFS 使磁碟區離線。
- **主動式錯誤修正** - ReFS 除了在讀取與寫入前驗證資料之外，還引入稱作<i>清除程式</i>的資料完整性掃描器。 此清除程式會定期掃描磁碟區、識別隱藏的損毀，以及主動觸發該毀損資料的修復。 


### <a name="performance"></a>效能
ReFS 除了提供復原改善外，還引入了供重視效能的虛擬工作負載所用的新功能。 即時階層最佳化、區塊複製及疏鬆 VDL 為 ReFS 功能演化的最佳範例，設計用來支援動態及多種工作負載：

- **[鏡像加速的同位](./mirror-accelerated-parity.md)** - 鏡像加速的同位為您的資料提供不僅效能高，而且容量也很有效率的儲存體。 

    - 為了提供效能高且容量有效率的儲存體，ReFS 將磁碟區分為兩個邏輯儲存群組，稱為階層。 這些階層可具備自己專屬的磁碟機及復原類型，讓各個階層針對效能或容量進行最佳化。 部分範例設定包括： 
    
    
    | 效能層 | 容量層 |
    |----------------|-----------------|
     鏡像 SSD | 鏡像 HDD |
     鏡像 SSD | 同位 SSD |
     鏡像 SSD | 同位 HDD |   
            
    - 這些階層設定後，ReFS 會加以使用為經常存取的資料提供快速的儲存體，並為非經常存取的資料提供容量有效率的儲存體。
        - 所有寫入都會在效能層發生，而維持在效能層中的大型資料區塊將有效率地即時移至容量層。
        - 若使用[混合部署](../storage-spaces/choosing-drives.md) (混合快閃與 HDD 磁碟機)，[儲存空間直接存取中的快取](../storage-spaces/understand-the-cache.md)將協助加快讀取速度，及降低虛擬工作負載資料分散特性的影響。 否則，若使用全快閃部署，讀取也將於效能層發生。 

>[!NOTE]
>對於伺服器部署，鏡像加速的同位僅支援[儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md)。

- **高速 VM 作業** - ReFS 引入新功能，特別將目標擺在改善虛擬工作負載的效能：
    - [區塊複製](./block-cloning.md) - 區塊複製會加快複製作業的速度，達成快速且低影響的 VM 檢查點合併作業。 
    - 疏鬆 VDL - 疏鬆 VDL 可讓 ReFS 將檔案快速歸零，將建立固定 VHD 所需的時間從 10 幾分鐘降至只需幾秒。


- **變數叢集大小** - ReFS 支援 4K 與 64K 叢集大小。 4K 為多數部署的建議叢集大小，但 64K 叢集適用於大型循序 IO 工作負載。
    
    
### **<a name="scalability"></a>延展性**
ReFS 設計用來支援超大型資料集 (百萬個 TB) 而不會對效能有負面影響，比先前的檔案系統達到更大的規模。 

## <a name="supported-deployments"></a>支援的部署

### <a name="storage-spaces-direct"></a>儲存空間直接存取 ###

建議針對虛擬工作負載或網路連接儲存裝置，將 ReFS 部署在儲存空間直接存取上： 
- 鏡像加速的同位以及[儲存空間直接存取中的快取](../storage-spaces/understand-the-cache.md)會提供效能高且容量有效率的儲存體。 
- 引入區塊複製與疏鬆 VDL 大幅提升了 .vhdx 檔案作業的速度，例如建立、合併及擴充。
- 完整性資料流、線上修復及替代資料複本可讓 ReFS 及儲存空間直接存取共同偵測並修正中繼資料及資料內的損毀。 
- ReFS 提供可擴充和支援大型資料集的功能。 

### <a name="storage-spaces-with-sas-drive-enclosures"></a>儲存空間內含 SAS 磁碟機機箱 ###
在儲存空間 (含共用 SAS 機箱) 上部署 ReFS，很適合用來裝載封存資料和存放使用者文件：
- 完整性資料流、線上修復及替代資料複本可讓 ReFS 與儲存空間共同偵測並修正中繼資料及資料內的損毀。 
- 儲存空間部署也可以使用 ReFS 中的提供的區塊複製和延展性。

### <a name="basic-disks"></a>基本磁碟 ###
在基本磁碟上部署 ReFS，最適合自行實作軟體復原及可用性解決方案的應用程式。 
- 自行導入復原及可用性軟體解決方案的應用程式可以善加利用完整性資料流、區塊複製，以及擴充和支援大型資料集的功能。 

>[!NOTE]
>連接 SAN 的儲存體不支援 ReFS。


## <a name="feature-comparison"></a>功能比較

### <a name="limits"></a>限制

| 功能       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| 檔案名稱長度上限 | 255 個 Unicode 字元  | 255 個 Unicode 字元               |
| 路徑名稱長度上限 |32K Unicode 字元 | 32K Unicode 字元                |
| 檔案大小上限 | 18 EB (艾位元組)  | 18 EB (艾位元組)                |
| 磁碟區大小上限 | 4.7 ZB (澤位元組)                           | 256 TB                |


### <a name="functionality"></a>功能

#### <a name="the-following-features-are-available-on-refs-and-ntfs"></a>您可以在 ReFS 和 NTFS 中使用下列功能：

| 功能       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| BitLocker 加密 | 是 | 是 |
| 叢集共用磁碟區 (CSV) 支援 | 是 | 是 |
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
| Oplocks | 是 | 是 |
| 疏鬆檔案 | 是 | 是 |
| 已命名的資料流 | 是 | 是 |

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
| 重複資料刪除 | 否 | 是 |
| 交易 | 否 | 是 |
| 永久連結 | 否 | 是 |
| 物件識別碼 | 否 | 是 |
| 簡短名稱 | 否 | 是 |
| 擴充屬性 | 否 | 是 |
| 磁碟配額 | 否 | 是 |
| 可開機 | 否 | 是 |
| 在抽取式媒體上受支援 | 否 | 是 |
| NTFS 儲存層 | 否 | 是 |


## <a name="see-also"></a>另請參閱

-   [ReFS 區塊複製](block-cloning.md)
-   [ReFS 完整性資料流](integrity-streams.md)
-   [儲存空間直接存取概觀](../storage-spaces/storage-spaces-direct-overview.md)
