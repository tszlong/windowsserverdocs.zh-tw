---
title: 了解儲存空間直接存取中的快取
ms.assetid: 69b1adc0-ee64-4eed-9732-0fb216777992
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: 0050a8931162e37408895ef664293be2349d1bde
ms.sourcegitcommit: 1bc3c229e9688ac741838005ec4b88e8f9533e8a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/18/2019
ms.locfileid: "68315002"
---
# <a name="understanding-the-cache-in-storage-spaces-direct"></a>了解儲存空間直接存取中的快取

>適用於：Windows Server 2019、Windows Server 2016

[儲存空間直接存取](storage-spaces-direct-overview.md)包含內建的伺服器端快取，可將儲存效能發揮極致。 它是大型持久的即時*讀寫*快取。 啟用儲存空間直接存取時，就會自動設定快取。 大多數的情況下，不需要任何的手動管理。
快取的運作方式取決於磁碟機呈現的類型。

下列影片深入探討儲存空間直接存取的快取運作方式，以及其他設計考量。

<strong>儲存空間直接存取設計考慮</strong><br>(20 分鐘)<br>
<iframe src="https://channel9.msdn.com/Blogs/windowsserver/Design-Considerations-for-Storage-Spaces-Direct/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="drive-types-and-deployment-options"></a>磁碟機類型及部署選項

儲存空間直接存取目前適用於三種類型的存放裝置：

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/NVMe-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            NVMe (非揮發性記憶體高速規格)
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/SSD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            SATA/SAS SSD (固態硬碟)
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/HDD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            HDD (硬碟)
        </td>
    </tr>
</table>

有六種組合，且分為「全快閃」及「混合式」兩類。

### <a name="all-flash-deployment-possibilities"></a>全快閃部署可能性

全快閃部署的目標是將儲存效能發揮極致，且不包括旋轉硬碟 (HDD)。

![All-Flash-Deployment-Possibilities](media/understand-the-cache/All-Flash-Deployment-Possibilities.png)

### <a name="hybrid-deployment-possibilities"></a>混合式部署可能性

混合式部署的目標是平衡效能與容量或將容量發揮極致，且不包括旋轉硬碟 (HDD)。

![混合式部署可能性](media/understand-the-cache/Hybrid-Deployment-Possibilities.png)

## <a name="cache-drives-are-selected-automatically"></a>快取磁碟機為自動選取

在多類型磁碟機的部署中，儲存空間直接存取會自動使用所有「最快」類型的磁碟機進行快取。 其餘磁碟機則用於容量。

哪種類型「最快」則交由下列階層決定。

![Drive-Type-Hierarchy](media/understand-the-cache/Drive-Type-Hierarchy.png)

例如，若有 NVMe 和 SSD，則 NVMe 會快取 SSD。

如果您有 SSD 和 HDD，則 SSD 會快取 HDD。

   >[!NOTE]
   > 快取磁碟機不提供可用的儲存容量。 儲存在快取中的所有資料也會儲存在其他位置，或一旦取消暫存即另存他處。 意即部署的原始儲存容量只是容量磁碟機的總和。

當所有的磁碟機類型相同時，不會自動設定任何快取。 您可以選擇手動設定較高耐力磁碟機快取同類型的較低耐力磁碟機，詳情請參閱[手動設定](#manual-configuration)一節。

   >[!TIP]
   > 在全 NVMe 或全 SSD 部署中，特別是極小規模時，不「配置」任何磁碟機執行快取，可大幅提升儲存效能。

## <a name="cache-behavior-is-set-automatically"></a>快取行為會自動設定

快取行為會根據要進行快取的磁碟機類型自動決定。 快取固態硬碟時 (例如 NVMe 快取 SSD)，只快取寫入。 快取硬碟時 (例如 SSD 快取 HDD)，則會快取讀取和寫入。

![Cache-Read-Write-Behavior](media/understand-the-cache/Cache-Read-Write-Behavior.png)

### <a name="write-only-caching-for-all-flash-deployments"></a>全快閃部署的僅寫入快取

快取固態硬碟時 (NVMe 或 SSD)，只快取寫入。 這會降低容量磁碟機損耗，因為快取會合併多次寫入和重寫，然後只在有需要時取消暫存，可降低容量磁碟機的累計流量並延長其壽命。 因此，建議您選取[較高耐力、最佳化寫入的](http://whatis.techtarget.com/definition/DWPD-device-drive-writes-per-day)磁碟機執行快取。 容量磁碟機的寫入耐力本就較低。

因為讀取不會顯著影響快閃的壽命，且因為固態硬碟普遍難覺察讀取延遲，所以不快取讀取：它們直接從容量磁碟機提供 (除非資料最近才寫入且尚未取消暫存)。 這可讓快取全部投入寫入，發揮最大效率。

這就造成快取磁碟機記錄寫入特性，如寫入延遲，而容量磁碟機記錄讀取特性。 兩者為一致、可預測且統一。

### <a name="readwrite-caching-for-hybrid-deployments"></a>混合式部署的讀/寫快取

快取硬碟 (HDD) 時，會快取讀取*和*寫入，為兩者提供快閃式延遲 (通常提升約 10 倍速度)。 讀取快取儲存最近和經常讀取的資料供快速存取，並可將 HDD 的隨機流量減到最低。 (因為搜尋和旋轉延遲, 隨機存取 HDD 所產生的延遲和遺失時間很重要)。寫入會進行快取以吸收高載, 並如同之前一樣合併寫入和重新寫入, 並將累計流量降到最低容量磁片磁碟機。

儲存空間直接存取實作的演算法，可以先取消隨機化再取消暫存寫入，模擬似乎循序的磁碟 IO 模式，即使實際的工作負載 IO (如虛擬機器) 為隨機。 這可最大化 IOPS 及 HDD 輸送量。

### <a name="caching-in-deployments-with-drives-of-all-three-types"></a>三種類型磁碟機都齊全之部署的快取

當三種類型的磁碟機都齊全時，NVMe 磁碟機可以快取 SSD 和 HDD。 此行為如前文所述：SSD 只快取寫入，HDD 則讀取和寫入都快取。 快取 HDD 的負荷會平均分散到快取磁碟。 

## <a name="summary"></a>總結

本表摘要說明哪些磁碟機用於快取、哪些用於容量，以及各種部署可能出現的快取行為。

| 部署     | 快取磁碟機                        | 容量磁碟機 | 快取行為 (預設)  |
| -------------- | ----------------------------------- | --------------- | ------------------------- |
| 全 NVMe         | 無 (選擇性：手動設定) | NVMe            | 僅寫入 (如有設定)  |
| 全 SSD          | 無 (選擇性：手動設定) | SSD             | 僅寫入 (如有設定)  |
| NVMe + SSD       | NVMe                                | SSD             | 唯寫                  |
| NVMe + HDD       | NVMe                                | HDD             | 讀取 + 寫入                |
| SSD + HDD        | SSD                                 | HDD             | 讀取 + 寫入                |
| NVMe + SSD + HDD | NVMe                                | SSD + HDD       | HDD 讀取 + 寫入、SSD 僅寫入  |

## <a name="server-side-architecture"></a>伺服器端架構

快取在磁碟機層級實作：一部伺服器內的個別快取磁碟機可繫結至同伺服器內的一或多部容量磁碟機。

因為快取在其餘 Windows 軟體定義的儲存堆疊之下，它沒有也不需要感知任何儲存空間或容錯等概念。 您可以把這當成建立對 Windows 而言的「混合式」(部分快閃、部分磁碟) 磁碟機。 至於真正的混合式磁碟機，外部幾乎看不到經常存取的資料和原始資料在實體媒體的較快和較慢部分之間的即時移動。

假設儲存空間直接存取中的復原至少要在伺服器上 (表示資料複本一律寫入不同的伺服器，每部伺服器最多一份)，資料在不在快取中，從同一復原得到的協助皆為相同。

![Cache-Server-Side-Architecture](media/understand-the-cache/Cache-Server-Side-Architecture.png)

例如，使用三向鏡像時，任何資料都會有三份複本寫入不同的伺服器，其在快取中置於該處。 無論它們待會是否會取消暫存，這三份複本都會一直存在。

## <a name="drive-bindings-are-dynamic"></a>磁碟機繫結為動態

快取和容量磁碟機之間的繫結可為任何比例，從 1:1 到 1:12 甚至更多。 只要新增或移除磁碟機就會動態調整，例如相應增加時或失敗後。 這表示您可以隨時獨立新增快取磁碟機或容量磁碟機。

![Dynamic-Binding](media/understand-the-cache/Dynamic-Binding.gif)

為對稱起見，建議容量磁碟機數為快取磁碟機數的倍數。 例如，若有 4 部快取磁碟機，則 8 部容量磁碟機 (比例為 1:2) 展現的效能會比 7 或 9 部更好。

## <a name="handling-cache-drive-failures"></a>處理快取磁碟機失敗錯誤

當快取磁碟機失敗時，任何尚未取消暫存的*本機伺服器*寫入都會遺失，表示它們只存在於其他複本 (在其他伺服器中)。 如同任何其他磁碟機失敗後，儲存空間可以並會諮詢留存的複本以自動復原。

短時間內，繫結到遺失快取磁碟機的容量磁碟機會顯示狀況不良。 一旦快取重新繫結 (自動) 並完成資料修復 (自動)，它們會繼續顯示狀況良好。

這就是為什麼每部伺服器至少要有兩部快取磁碟機才能保持效能的原因。

![Handling-Failure](media/understand-the-cache/Handling-Failure.gif)

您便可替換快取磁碟機，就像替換任何其他磁碟機一樣。

   >[!NOTE]
   > 您可能需要關閉電源以安全替換附加介面卡 (AIC) 或 M.2 尺寸的 NVMe。

## <a name="relationship-to-other-caches"></a>與其他快取的關係

Windows 軟體定義的儲存堆疊中還有幾種無關聯的快取。 例如，儲存空間回寫式快取，以及叢集共用磁碟區 (CSV) 記憶體內部讀取快取。

使用儲存空間直接存取，不必修改儲存空間回寫式快取的預設行為。 例如，**New-Volume** Cmdlet 不該使用 **-WriteCacheSize** 參數。

您可以選擇是否要使用 CSV 快取，由您作主。 儲存空間直接存取預設予以關閉，但與本主題中所有提及的新快取都不衝突。 在某些情況下，它可提供極佳的效能表現。 如需詳細資訊，請參閱[如何啟用 CSV 快取](../../failover-clustering/failover-cluster-csvs.md#enable-the-csv-cache-for-read-intensive-workloads-optional) (英文)。

## <a name="manual-configuration"></a>手動設定

大部分的部署都不需要手動設定。 如果您需要它, 請參閱下列各節。 

如果您需要在安裝之後對快取裝置模型進行變更, 請編輯健全狀況服務的支援元件檔, 如[健全狀況服務總覽](../../failover-clustering/health-service-overview.md#supported-components-document)中所述。

### <a name="specify-cache-drive-model"></a>指定快取磁碟機模型

在所有磁碟機都是同樣類型的部署中 (例如全 NVMe 或 SSD 部署) 不會設定任何快取，因為 Windows 無法自動分辨同類型磁碟機的寫入耐力等特性。

您可以指定哪些磁碟機模型使用 **Enable-ClusterS2D** Cmdlet 的 **-CacheDeviceModel** 參數，進而使用較高耐力磁碟機快取同類型的較低耐力磁碟機。 一旦啟用儲存空間直接存取，該模型的所有磁碟機都會用於快取。

   >[!TIP]
   > 模型字串與 **Get-PhysicalDisk** 輸出中的內容要完全一致。

####  <a name="example"></a>範例

首先, 取得實體磁片的清單:

```PowerShell
Get-PhysicalDisk | Group Model -NoElement
```

以下是一些輸出範例︰

```
Count Name
----- ----
    8 FABRIKAM NVME-1710
   16 CONTOSO NVME-1520
```

然後輸入下列命令, 並指定快取裝置型號:

```PowerShell
Enable-ClusterS2D -CacheDeviceModel "FABRIKAM NVME-1710"
```

您可以在 PowerShell 中執行 **Get-PhysicalDisk** 驗證其 **Usage** 屬性是否顯示 **"Journal"** ，檢查您屬意的磁碟機是否用於快取。

### <a name="manual-deployment-possibilities"></a>手動部署可能性

手動設定有可能啟用下列部署：

![Exotic-Deployment-Possibilities](media/understand-the-cache/Exotic-Deployment-Possibilities.png)

### <a name="set-cache-behavior"></a>設定快取行為

有可能覆寫快取的預設行為。 例如，即使在全快間部署中仍可將它設為快取讀取。 除非您確定預設不適合您的工作負載，否則不鼓勵修改行為。

若要覆寫此行為, 請使用**ClusterStorageSpacesDirect** Cmdlet 和其 **-CacheModeSSD**和 **-CacheModeHDD**參數。 **CacheModeSSD** 參數設定快取固態硬碟時的快取行為。 **CacheModeHDD** 參數設定快取硬碟時的快取行為。 啟用儲存空間直接存取後隨時可執行此作業。

您可以使用**ClusterStorageSpacesDirect**來確認是否已設定此行為。

#### <a name="example"></a>範例

首先, 取得儲存空間直接存取設定:

```PowerShell
Get-ClusterStorageSpacesDirect
```

以下是一些輸出範例︰

```
CacheModeHDD : ReadWrite
CacheModeSSD : WriteOnly
```

然後, 執行下列動作:

```PowerShell
Set-ClusterStorageSpacesDirect -CacheModeSSD ReadWrite

Get-ClusterS2D
```

以下是一些輸出範例︰

```
CacheModeHDD : ReadWrite
CacheModeSSD : ReadWrite
```

## <a name="sizing-the-cache"></a>調整快取大小

快取的大小應設為能容納您應用程式及工作負載的工作組 (任何時間正在讀取及寫入的資料)。

這在硬碟混合式部署中尤其重要。 如果使用中的工作組超過快取大小，或者使用中的工作組漂移太快，讀取快取遺漏會增加，而寫入需要更頻繁地取消暫存，破壞整體效能。

您可以使用 Windows 的內建效能監視器 (PerfMon.exe) 公用程式檢查快取遺漏的比例。 特別是您可以比較 **[Cache Miss Reads/sec]&nbsp;(叢集儲存混合式磁碟)** 計數器集合的 **[Cluster Storage Hybrid Disk]&nbsp;(快取遺漏讀取數/秒)** 和您部署的整體讀取 IOPS。 每個「混合式磁碟」都對應一部容量磁碟機。

例如，2 部快取磁碟機繫結至 4 部容量磁碟機，造成每部伺服器有 4 個「混合式磁碟」物件執行個體。

![Performance-Monitor](media/understand-the-cache/PerfMon.png)

規則並無標準，但若讀取有太多快取遺漏，就需要縮減，而您應該考慮增加快取磁碟機擴張快取。 您可以隨時獨立新增快取磁碟機或容量磁碟機。

## <a name="see-also"></a>另請參閱

- [選擇磁片磁碟機和復原類型](choosing-drives.md)
- [容錯與儲存空間效率](storage-spaces-fault-tolerance.md)
- [儲存空間直接存取硬體需求](storage-spaces-direct-hardware-requirements.md)
