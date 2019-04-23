---
title: 巢狀中復原的儲存空間直接存取
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dansimp
ms.technology: storagespaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2018
ms.openlocfilehash: 206d5d19ec55774f9055e7265d4c87d276c60590
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879569"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>巢狀中復原的儲存空間直接存取

> 適用於：Windows Server 2019

巢狀的恢復功能是新功能[儲存空間直接存取](storage-spaces-direct-overview.md)在 Windows Server 2019 可承受多個硬體故障，在相同的時間，而不會遺失儲存體可用性，所以使用者、 應用程式的兩部伺服器叢集和虛擬機器仍會繼續執行，而不中斷。 本主題說明其運作方式，提供逐步指示開始著手及回答的最常見問題集。

## <a name="prerequisites"></a>先決條件

### <a name="green-checkmark-iconmedianested-resiliencysupportedpng-consider-nested-resiliency-if"></a>![綠色核取記號圖示。](media/nested-resiliency/supported.png) 如果，請考慮巢狀的恢復功能：

- 您的叢集執行 Windows Server 2019;和
- 您的叢集有 2 個伺服器節點

### <a name="red-x-iconmedianested-resiliencyunsupportedpng-you-cant-use-nested-resiliency-if"></a>![紅色 X 圖示。](media/nested-resiliency/unsupported.png) 如果，則無法使用巢狀的恢復功能：

- 您的叢集執行 Windows Server 2016;或
- 您的叢集有 3 個或多個伺服器節點

## <a name="why-nested-resiliency"></a>為什麼巢狀的恢復功能

使用巢狀的恢復功能的磁碟區可以**保持線上且可存取，即使在同一時間發生的多個硬體故障**，不同於傳統[雙向鏡像](storage-spaces-fault-tolerance.md)恢復功能。 例如，如果兩個磁碟機無法在此同時，或如果伺服器當機時，磁碟機失敗時，使用巢狀的恢復功能的磁碟區會保持線上且可存取。 超交集基礎結構，這會增加應用程式和虛擬機器; 的執行時間針對檔案伺服器工作負載，這表示使用者享有不中斷的存取權，他們的檔案。

![儲存體可用性](media/nested-resiliency/storage-availability.png)

缺點是該巢狀的恢復功能已**較低容量效率比傳統的雙向鏡像**，這表示您會收到稍微小一點的可用空間。 如需詳細資訊，請參閱 <<c0> [ 容量效率](#capacity-efficiency)下一節。

## <a name="how-it-works"></a>運作方式

### <a name="inspiration-raid-51"></a>靈感：RAID 5 + 1

RAID 5 + 1 是已建立的一種有用的背景中提供了解巢狀的復原能力的分散式儲存體恢復功能。 在 RAID 5 + 1，每部伺服器，復原區域內提供 RAID-5，或*單一同位檢查*，以防止任何單一磁碟機的遺失。 然後，RAID 1 時，提供恢復功能的進一步或*雙向鏡像*，兩部伺服器，以防止遺失的任一部伺服器之間。

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>兩個新的復原選項

儲存空間直接存取 Windows Server 2019 中提供兩個新的復原選項在軟體，而不需要特殊的 RAID 硬體中實作：

- **巢狀的雙向鏡像。** 內每一部伺服器，本機的恢復功能提供的雙向鏡像，則會藉由將兩部伺服器之間的雙向鏡像提供進一步的恢復功能。 它是基本上是四向鏡像，與每一部伺服器中的兩個複本。 巢狀的雙向鏡像提供募效能： 寫入都會送到所有複本，並讀取來自任何複本。

  ![巢狀的雙向鏡像](media/nested-resiliency/nested-two-way-mirror.png)

- **巢狀的鏡像加速同位檢查。** 合併巢狀雙向鏡像，從以上版本，以巢狀的同位檢查。 單一內每一部伺服器，提供大部分資料的本機復原[位元同位檢查算術](storage-spaces-fault-tolerance.md#parity)，除非新的最新寫入使用雙向鏡像。 然後，進一步的所有資料的恢復功能是由提供在伺服器之間的雙向鏡像。 如需有關如何鏡像加速同位檢查的運作方式的詳細資訊，請參閱[鏡像加速同位檢查](https://docs.microsoft.com/windows-server/storage/refs/mirror-accelerated-parity)。

  ![巢狀的鏡像加速的同位檢查](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>容量效率

容量效率是可用空間的比例[磁碟區使用量](plan-volumes.md#choosing-the-size-of-volumes)。 它說明能歸因於復原，容量的額外負荷，並取決於您選擇的復原選項。 簡單的範例中，沒有復原是 100%容量有效率 (1 TB 的資料會佔據的實體儲存體容量的 1 TB)，儲存資料時雙向鏡像為 50%有效率 (1 TB 的資料會採用到 2 TB 的實體儲存容量)。

- **巢狀的雙向鏡像**寫入四個複本的所有項目，來儲存 1 TB 的資料，這表示您需要 4 TB 的實體儲存體容量。 雖然其單純性是吸引人，但為 25%的巢狀的雙向鏡像的容量效率是最小的儲存空間直接存取中的任何復原選項。

- **巢狀鏡像加速同位檢查**可達到更高的容量效率，約 35%-40%，取決於兩個因素： 數目容量磁碟機中每個伺服器和鏡像和同位檢查您指定的磁碟區的混合。 下表提供查閱的常見設定：

  | 每一部伺服器的容量磁碟機 | 10%鏡像 | 20%鏡像 | 30%鏡像 |
  |----------------------------|------------|------------|------------|
  | 4                          | 35.7%      | 34.1%      | 32.6%      |
  | 5                          | 37.7%      | 35.7%      | 33.9%      |
  | 6                          | 39.1%      | 36.8%      | 34.7%      |
  | 7+                         | 40.0%      | 37.5%      | 35.3%      |

  > [!NOTE]
  > **如果您想知道，以下是完整的數學運算的範例。** 假設我們有六個的容量磁碟機中每個兩部伺服器，而我們想要建立一個 100 GB 的磁碟區包含 10 GB 的鏡像和同位檢查的 90 GB。 伺服器本機雙向鏡像是有效率的 50.0%，這表示 10GB 鏡像資料會在 20 GB 來儲存每個伺服器上。 鏡像處理這兩部伺服器，其總耗用量為 40 GB。 伺服器本機單一同位檢查，在此情況下，會是 5/6 = 83.3%效率，這表示同位檢查資料 90 GB 會在每一部伺服器上儲存的 108 GB。 鏡像處理這兩部伺服器，其總耗用量是 216 GB。 總使用量因此不會是 [(10 GB / 50.0%) + (90 GB / 83.3%)] × 2 = 256 GB，39.1%整體容量效率。

請注意，傳統的雙向鏡像 （大約 50%) 的容量效率巢狀鏡像加速同位檢查 （最多 40%)差異不大。 根據您的需求，稍微較低的容量效率可能值得大幅增加儲存體可用性。 您可以選擇復原個別磁碟區，讓您可以混合使用巢狀的復原磁碟區和相同的叢集內的傳統的雙向鏡像磁碟區。

![權衡取捨](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

您可以在 PowerShell 中使用熟悉的儲存體 cmdlet，來建立磁碟區與巢狀的恢復功能。

### <a name="step-1-create-storage-tier-templates"></a>步驟 1：建立儲存體層範本

首先，建立新的儲存體層範本，使用`New-StorageTier`cmdlet。 您只需要一次，執行這項操作，然後每個您所建立的新磁碟區可以參考這些範本。 指定`-MediaType`容量磁碟機，並選擇性地`-FriendlyName`您選擇。 請勿修改其他參數。

如果您的容量磁碟機是硬碟機 (HDD)，啟動以系統管理員的 PowerShell，然後執行：

```PowerShell 
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk 
``` 

如果您的容量磁碟機固態硬碟 (SSD)，設定`-MediaType`至`SSD`改。 請勿修改其他參數。

> [!TIP]
> 確認已成功建立包含的層`Get-StorageTier`。

### <a name="step-2-create-volumes"></a>步驟 2：建立磁碟區

接著，建立新的磁碟區使用`New-Volume`cmdlet。

#### <a name="nested-two-way-mirror"></a>巢狀的雙向鏡像

若要使用巢狀的雙向鏡像，參考`NestedMirror`層範本，並指定大小。 例如: 

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>巢狀的鏡像加速的同位檢查

若要使用巢狀的鏡像加速的同位檢查，請參考這兩`NestedMirror`和`NestedParity`層範本，並指定兩個大小，其中每個部分的磁碟區 （鏡像第一，同位檢查第二個）。 例如，若要建立一個 500 GB 的磁碟區是 20%巢狀的雙向鏡像和 80%的巢狀同位檢查，執行：

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>步驟 3：繼續在 Windows Admin Center

使用巢狀的恢復功能的磁碟區會出現在[Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)與清除的標記，如以下螢幕擷取畫面所示。 一旦建立，您可以管理和監視使用 Windows Admin Center，就像任何其他磁碟區中儲存空間直接存取它們。

![](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>選擇性：將延伸至快取磁碟機

使用其預設值，巢狀的恢復功能會防止遺失多個容量的磁碟，在此同時，或一部伺服器以及在同一時間一個容量磁碟機。 若要將此防護擴充到[快取磁碟機](understand-the-cache.md)有額外的考量： 因為快取磁碟機通常會提供讀取*和寫入*快取*多個*容量磁碟機請確定已關閉的另一部伺服器時，您可以容忍遺失的快取磁碟機的唯一方式是只是不要快取寫入，但，對效能的影響。

若要解決這種情況下，儲存空間直接存取，請提供自動停用寫入快取，當兩部伺服器叢集中的一個伺服器已關機，然後再重新啟用寫入快取伺服器執行備份後的選項。 若要讓例行的重新啟動，而造成效能的影響，寫入快取未停用，直到伺服器已關閉 30 分鐘。

若要設定此行為 （選擇性），請啟動 PowerShell，以系統管理員身分，並執行：

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

一次設`True`，快取行為是：

| 情況                       | 快取行為                           | 可以容許快取磁碟機遺失嗎？ |
|---------------------------------|------------------------------------------|--------------------------------|
| 這兩個伺服器執行中                 | 快取讀取和寫入，就會有完整的效能 | 是                            |
| 伺服器關機前 30 分鐘內   | 快取讀取和寫入，就會有完整的效能 | 否 （暫時）               |
| 在前 30 分鐘後          | 快取讀取，效能會受到影響   | 是                            |

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>可以將轉換的雙向鏡像和巢狀的恢復功能之間的現有磁碟區？

否，磁碟區無法復原類型之間轉換。 Windows Server 2019 的新部署，決定最佳的復原類型符合您需求的事先。 如果您要升級 Windows Server 2016 中，您可以巢狀的恢復功能以建立新的磁碟區、 移轉您的資料，然後再刪除 較舊的磁碟區。

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>可以使用巢狀的恢復功能有許多種容量磁碟機嗎？

是，只要指定`-MediaType`據以每個層期間[步驟 1](#step-1-create-storage-tier-templates)上方。 比方說，NVMe、 SSD 和 HDD 的相同叢集中，NVMe 提供快取而後面兩個提供的容量： 設定`NestedMirror`層，以`-MediaType SSD`並`NestedParity`層，以`-MediaType HDD`。 在此情況下，請注意，同位檢查的容量效率而定，HDD 磁碟機數目，而且您需要至少其中每一部伺服器的 4 個。

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>可以使用巢狀的恢復功能的 3 個以上的伺服器嗎？

否，如果只使用巢狀的復原您的叢集有 2 個伺服器。

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>若要使用巢狀的恢復功能是否需要多少個磁碟機？

所需的儲存空間直接存取的磁碟機的最小數目是 4 個容量的磁碟，每個伺服器節點，再加上 2 的快取磁碟機，每個伺服器節點 （如果有的話）。 這是從 Windows Server 2016 不變。 沒有其他需求的巢狀的恢復功能，並保留容量的建議也是不變。

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>不會巢狀的恢復功能就會變更磁碟機更換的運作方式？

資料分割

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>不會巢狀的恢復功能，變更伺服器的節點取代的運作方式？

資料分割 若要取代的伺服器節點和其磁碟機，請遵循此順序：

1. 淘汰外寄郵件伺服器中的磁碟機
2. 將新的伺服器，其磁碟機中，新增至叢集
3. 將會重新平衡儲存集區
4. 移除 外寄郵件伺服器和其磁碟機

如需詳細資訊，請參閱[移除伺服器](remove-servers.md)主題。

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
- [了解容錯功能，在儲存空間直接存取](storage-spaces-fault-tolerance.md)
- [計劃中儲存空間直接存取磁碟區](plan-volumes.md)
- [建立磁碟區中儲存空間直接存取](create-volumes.md)
