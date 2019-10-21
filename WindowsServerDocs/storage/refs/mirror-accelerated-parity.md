---
title: 鏡像加速的同位
ms.prod: windows-server
ms.author: gawatu
ms.manager: masriniv
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 10/17/2018
ms.assetid: ''
ms.openlocfilehash: 2721f1c744c5c03d8e4bce0508fd23fa5237f95f
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591090"
---
# <a name="mirror-accelerated-parity"></a>鏡像加速的同位

>適用于： Windows Server 2019、Windows Server 2016

儲存空間可以使用兩個基本技術提供資料容錯：鏡像和同位。 在[儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md)中，ReFS 引入鏡像加速同位，可讓您建立同時使用鏡像和同位復原的磁碟區。 鏡像加速的同位提供便宜、節省空間的儲存空間，同時不犧牲效能。 

![鏡像加速的同位磁碟區](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume.png)

## <a name="background"></a>背景

鏡像和同位復原配置有徹底不同的儲存空間及效能特性：
- 鏡像復原功能可讓使用者達到快速的寫入效能，但是複寫每個複本的資料並不會有空間效率。 
- 另一方面，同位必須重新計算每個寫入的同位，造成隨機寫入效能不佳。 不過，同位確實可讓使用者有更多空間來儲存資料。 如需詳細資訊，請參閱[儲存空間容錯](../storage-spaces/Storage-Spaces-Fault-Tolerance.md)。

因此，鏡像傾向於提供要求效能的儲存空間，而同位可改善儲存空間容量的使用率。 在鏡像加速的同位中，ReFS 運用兩個復原類型的優勢，透過將兩個復原配置同時結合至單一磁碟區中，來同時提供容量有效率以及重視效能的儲存空間。

## <a name="data-rotation-on-mirror-accelerated-parity"></a>鏡像加速同位上的資料循環

ReFS 會即時在鏡像和同位之間主動循環資料。 這可讓連入寫入快速寫入至鏡像，接著循環至同位以有效率地儲存。 如此一來，連入 IO 可在鏡像中獲得快速服務，而非經常存取的資料可有效率地儲存在同位中，在相同的磁碟區同時提供最佳效能和低成本的儲存空間。 

為了在鏡像和同位之間循環資料，ReFS 在邏輯上將磁碟區分為 64 MiB 的區域 (這是循環的單位)。 下列影像描述區分為區域的鏡像加速同位磁碟區。 

![鏡像加速同位磁碟區與儲存體容器](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume-with-Storage-Containers.png)

當鏡像層已達指定的容量層級，ReFS 會開始從鏡像到同位循環完整區域。 ReFS 不會立即將資料從鏡像移動到同位，而是盡可能等待並將資料保留在鏡像，讓 ReFS 可以繼續提供資料的最佳效能 (請參閱下方的＜IO 效能＞)。 

當資料從鏡像移至同位時，系統會讀取資料、計算同位編碼，接著將資料寫入同位。 下方動畫使用三向鏡像區域來說明這個程序，在循環期間會轉換為清除編碼區域：

![鏡像加速同位循環](media/mirror-accelerated-parity/Container-Rotation.gif)

## <a name="io-on-mirror-accelerated-parity"></a>鏡像加速同位上的 IO
### <a name="io-behavior"></a>IO 行為
**寫入：** ReFS 以三種不同方式服務連入寫入：

1.  **寫入鏡像：**

    - **1a.** 如果連入寫入修改鏡像上的現有資料，ReFS 會就地修改資料。
    - **1a-1b.** 如果連入寫入是新的寫入，且 ReFS 可以在鏡像中成功找到服務這個寫入的足夠空間，則 ReFS 將會寫入鏡像。
    ![Write 對鏡像 ](media/mirror-accelerated-parity/Write-to-Mirror.png)

2. **寫入鏡像，從同位重新配置：**

    如果傳入的寫入會修改同位檢查中的資料，且 ReFS 可以成功地在鏡像中找到足夠的可用空間，以服務傳入的寫入，則 ReFS 會先使先前的資料在同位檢查中失效，然後寫入鏡像。 這個失效動作是快速又便宜的中繼資料操作，可協助有意義地改善對同位的寫入效能。
    ![Reallocated-寫入 ](media/mirror-accelerated-parity/Reallocated-Write.png)

3. **寫入同位檢查：**
    
    如果 ReFS 無法在鏡像中成功找到足夠空間，則 ReFS 會將新資料寫入同位，或直接在同位中修改現有資料。 下方的＜效能最佳化＞章節提供協助將寫入同位最小化的指導方針。
    ![Write 對同位檢查 ](media/mirror-accelerated-parity/Write-to-Parity.png)

**讀取：** ReFS 會直接從包含相關資料的層讀取。 如果同位是以 HDD 建構，則「儲存空間直接存取」中的快取會快取此資料來加快未來的讀取。 

> [!NOTE]
> 讀取不會導致 ReFS 將資料循環回鏡像層。 

### <a name="io-performance"></a>IO 效能

**寫入：** 上述每一種寫入都有它自己的效能特性。 大致而言，寫入鏡像層會比重新配置寫入更快，而重新配置寫入又會比直接寫入同位層快得多。 我們以下方的不等式來說明這種關係： 


- **鏡像層 > 重新配置寫入 > > 同位層**


**讀取：** 從同位讀取時，不會產生有意義的負面效能影響：
- 如果鏡像和同位以相同的媒體類型建構，則讀取效能相等。 
- 如果鏡像和同位以不同的媒體類型建構 — 例如鏡像 SSD、同位 HDD — 則[儲存空間直接存取中的快取](../storage-spaces/understand-the-cache.md)可協助快取經常存取的資料，來加速從同位的任何讀取。

## <a name="refs-compaction"></a>ReFS 壓縮

在此秋季的半年度版本中，ReFS 引進了壓縮功能，大幅提升了 90 +% 已滿的鏡像加速同位磁片區的效能。 

**背景：** 以前，在鏡像加速同位磁碟區快滿時，這些磁碟區的效能會降低。 效能降低是因為經常存取和非經常存取的資料隨著時間而在整個磁碟區中混在一起。 這表示由於非經常存取的資料佔用原本可供經常存取的資料使用的鏡像空間，因此較不經常存取的資料儲存在鏡像中。 將經常存取的資料儲存在鏡像對於維護高效能來說很重要，因為直接寫入鏡像會比重新配置寫入來得快，而數量級又比直接寫入同位更快。 因此，鏡像中有非經常存取的資料對效能來說是不好的，因為它會降低 ReFS 可以直接寫入鏡像的可能性。 

ReFS 壓縮透過釋放鏡像中的空間來供經常存取的資料使用，解決這些效能問題。 壓縮會先將鏡像和同位中的所有資料合併到同位中。 這樣可以減少磁碟區中的分散程度，提高鏡像中的可定位空間量。 更重要的是，此程序可讓 ReFS 將經常存取的資料合併回鏡像：
-   有新寫入傳入時，它們會在鏡像中服務。 因此，新寫入、經常存取的資料會保留在鏡像中。 
-   對同位中的資料有修改寫入時，ReFS 會進行重新配置寫入，因此也可以在鏡像中服務這個寫入。 如此一來，在壓縮期間移至同位的經常存取的資料將會重新配置回到鏡像。 

## <a name="performance-optimizations"></a>效能最佳化

>[!IMPORTANT]
> 建議您將大量寫入的 Vhd 放在不同的子目錄中。 這是因為 ReFS 會在目錄及其檔案的層級寫入中繼資料變更。 因此，如果您在目錄之間散發寫入繁重的檔案，中繼資料作業會變得較小並平行執行，以減少應用程式的延遲。

### <a name="performance-counters"></a>效能計數器

ReFS 維護效能計數器，協助評估鏡像加速同位的效能。 
- 如上所述，在「寫入至同位」一節中，ReFS 在鏡像中找不到可用空間時，會直接寫入同位檢查。 一般而言，當鏡像層的填滿速度比 ReFS 將資料循環至同位更快時，就會發生這種情形。 換句話說，ReFS 循環跟不上擷取率。 下方的效能計數器可識別 ReFS 直接寫入同位的時機：

  ```
  # Windows Server 2016
  ReFS\Data allocations slow tier/sec
  ReFS\Metadata allocations slow tier/sec

  # Windows Server 2019
  ReFS\Allocation of Data Clusters on Slow Tier/sec
  ReFS\Allocation of Metadata Clusters on Slow Tier/sec
  ```

- 如果這些計數器不是零，表示 ReFS 將資料循環出鏡像的速度不夠快。 若要協助改善此點，您可以變更循環積極程度或提高鏡像層的大小。

### <a name="rotation-aggressiveness"></a>旋轉增強

ReFS 在鏡像到達指定的容量閾值時開始循環資料。
-   提高此循環閾值，會使得 ReFS 將資料留在鏡像層中更久。 將經常存取的資料保留在鏡像層中對效能來說最理想，但 ReFS 就無法有效地服務大量連入 IO。 
-   降低此值可讓 ReFS 主動移出資料並改善擷取連入 IO。 這適用於大量擷取的工作負載，例如封存儲存空間。 不過，降低值可能會降低一般用途工作負載的效能。 非必要的將資料循環出鏡像層會降低效能。 

ReFS 引進了可微調參數來調整這個閾值，可使用登錄機碼來設定此參數。 必須在**儲存空間直接存取部署的每個節點**上設定此登錄機碼，並需要重新開機，讓變更生效。 
-   **機碼：** HKEY_LOCAL_MACHINE\System\CurrentControlSet\Policies
-   **ValueName (DWORD)：** DataDestageSsdFillRatioThreshold
-   **ValueType：** Percentage

若未設定此登錄機碼，ReFS 會使用預設值 85%。  針對大部分部署，建議使用此預設值，並不建議使用低於 50% 的值。 以下的 PowerShell 指令示範如何以 75% 的值來設定此登錄機碼： 
```PowerShell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75
 ```
 若要在儲存空間直接存取部署中的每個節點上設定此登錄機碼，您可以使用下面的 PowerShell 命令：
 ```PowerShell
 $Nodes = 'S2D-01', 'S2D-02', 'S2D-03', 'S2D-04'
 Invoke-Command $Nodes {Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75}
 ```

### <a name="increasing-the-size-of-the-mirrored-tier"></a>增加鏡像層的大小

增加鏡像層的大小，可讓 ReFS 在鏡像中保留工作集的較大部分。 這可改善 ReFS 直接寫入鏡像的可能性，協助獲得更好的效能。 以下的 PowerShell Cmdlet 示範如何增加鏡像層的大小：
```PowerShell
Resize-StorageTier -FriendlyName “Performance” -Size 20GB
Resize-StorageTier -InputObject (Get-StorageTier -FriendlyName “Performance”) -Size 20GB
```
>[!TIP]
>務必在調整 **StorageTier** 的大小之後調整 **Partition** 和 **Volume** 的大小。 如需詳細資訊及範例，請參閱 [Resize-Volumes](../storage-spaces/resize-volumes.md)。

## <a name="creating-a-mirror-accelerated-parity-volume"></a>建立鏡像加速的同位磁碟區

以下的 PowerShell Cmdlet 使用鏡像:同位比 20:80 來建立鏡像加速的同位磁碟區，這是適用於大部分工作負載的建議設定。 如需詳細資訊與範例，請參閱[建立儲存空間直接存取中的磁碟區](../storage-spaces/Create-volumes.md)。

```PowerShell
New-Volume – FriendlyName “TestVolume” -FileSystem CSVFS_ReFS -StoragePoolFriendlyName “StoragePoolName” -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 200GB, 800GB
```

## <a name="see-also"></a>請參閱

-   [ReFS 總覽](refs-overview.md)
-   [ReFS 區塊複製](block-cloning.md)
-   [ReFS 完整性資料流程](integrity-streams.md)
-   [儲存空間直接存取總覽](../storage-spaces/storage-spaces-direct-overview.md)
