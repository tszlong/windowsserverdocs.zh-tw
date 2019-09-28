---
title: 儲存空間直接存取的嵌套復原
ms.prod: windows-server
ms.author: jgerend
ms.manager: dansimp
ms.technology: storagespaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/15/2019
ms.openlocfilehash: ea1c4b2c249759634e00f6a1ac2caa34f8085ae1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402865"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>儲存空間直接存取的嵌套復原

> 適用於：Windows Server Standard 2012 R2

Nested 復原是 Windows Server 2019 中[儲存空間直接存取](storage-spaces-direct-overview.md)的新功能，可讓兩部伺服器的叢集在同一時間承受多個硬體失敗，而不會遺失存放裝置可用性，讓使用者、應用程式和虛擬機器繼續執行而不中斷。 本主題說明其運作方式、提供開始使用的逐步指示，以及回答最常見問題。

## <a name="prerequisites"></a>必要條件

### <a name="green-checkmark-iconmedianested-resiliencysupportedpng-consider-nested-resiliency-if"></a>![綠色核取記號圖示。](media/nested-resiliency/supported.png) 假設有下列情況，請考慮 nested 復原：

- 您的叢集執行 Windows Server 2019;和
- 您的叢集只有2個伺服器節點

### <a name="red-x-iconmedianested-resiliencyunsupportedpng-you-cant-use-nested-resiliency-if"></a>![紅色 X 圖示。](media/nested-resiliency/unsupported.png) 如果是下列情況，則無法使用 nested 復原：

- 您的叢集執行 Windows Server 2016;或
- 您的叢集有3個或多個伺服器節點

## <a name="why-nested-resiliency"></a>為什麼要有嵌套的復原

使用嵌套恢復功能的磁片區**即使同時發生多個硬體失敗**（與傳統的[雙向鏡像](storage-spaces-fault-tolerance.md)復原不同），仍可保持連線並可供存取。 例如，如果兩個磁片磁碟機同時失敗，或是伺服器關機且磁片磁碟機失敗，則使用嵌套復原的磁片區會保持連線並可供存取。 針對超融合式基礎結構，這會增加應用程式和虛擬機器的執行時間;針對檔案伺服器工作負載，這表示使用者可以不間斷地存取其檔案。

![儲存體可用性](media/nested-resiliency/storage-availability.png)

取捨的是，相**較于傳統的雙向鏡像**，嵌套的復原能力具有更低的容量效率，這表示您會得到稍微少的可用空間。 如需詳細資訊，請參閱下方的[容量效率](#capacity-efficiency)一節。

## <a name="how-it-works"></a>運作方式

### <a name="inspiration-raid-51"></a>靈感RAID 5 + 1

RAID 5 + 1 是一種已建立的分散式儲存體復原形式，可提供實用的背景來瞭解嵌套的復原。 在 RAID 5 + 1 中，每部伺服器內，本機復原是由 RAID-5 提供，或*單一同*位檢查，以防止任何單一磁片磁碟機遺失。 然後，在兩部伺服器之間，由 RAID-5 或*雙向鏡像*提供進一步的復原功能，以防止任一伺服器遺失。

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>兩個新的復原選項

Windows Server 2019 中的儲存空間直接存取提供在軟體中執行的兩個新的復原選項，而不需要特製化的 RAID 硬體：

- **嵌套的雙向鏡像。** 在每部伺服器中，本機復原是由雙向鏡像所提供，然後在兩部伺服器之間由雙向鏡像提供進一步的復原方式。 這基本上是一個四向鏡像，每部伺服器都有兩個複本。 嵌套的雙向鏡像提供了不折不扣的效能：寫入會移至所有複本，而讀取則來自任何複本。

  ![嵌套的雙向鏡像](media/nested-resiliency/nested-two-way-mirror.png)

- **嵌套的鏡像加速同位。** 結合上述的嵌套雙向鏡像與嵌套的同位。 在每個伺服器內，大部分資料的本機復原是由單一[位同位](storage-spaces-fault-tolerance.md#parity)檢查所提供，但新的最近寫入會使用雙向鏡像。 然後，伺服器之間的雙向鏡像會提供所有資料的進一步復原。 如需有關鏡像加速同位檢查如何運作的詳細資訊，請參閱[鏡像加速同位](https://docs.microsoft.com/windows-server/storage/refs/mirror-accelerated-parity)。

  ![嵌套鏡像加速同位](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>容量效率

容量效率是可用空間與[磁片](plan-volumes.md#choosing-the-size-of-volumes)區使用量的比率。 它描述復原所需的容量負荷，並取決於您選擇的復原選項。 簡單的範例是，在沒有復原的情況下儲存資料是 100% 的容量有效率（1 TB 的資料佔用 1 TB 的實體儲存體容量），而雙向鏡像則是 50% 的效率（1 TB 的資料佔用 2 TB 的實體儲存體容量）。

- **嵌套的雙向鏡像**會寫入四個複本，這表示若要儲存 1 tb 的資料，您需要 4 TB 的實體儲存體容量。 雖然它的簡單性很吸引人，但嵌套的雙向鏡像的容量效率 25% 是儲存空間直接存取中任何復原選項的最低。

- 根據兩個因素而定，**嵌套鏡像加速同位**會達到較高的容量效率（大約 35%-40%）：每部伺服器中的容量磁片磁碟機數目，以及您為磁片區指定的鏡像和同位檢查混合。 下表提供一般設定的查閱：

  | 每一伺服器的容量磁片磁碟機 | 10% 鏡像 | 20% 鏡像 | 30% 鏡像 |
  |----------------------------|------------|------------|------------|
  | 4                          | 35.7%      | 34.1%      | 32.6%      |
  | 5                          | 37.7%      | 35.7%      | 33.9%      |
  | 6                          | 39.1%      | 36.8%      | 34.7%      |
  | 7 +                         | 40.0%      | 37.5%      | 35.3%      |

  > [!NOTE]
  > **如果您想知道，以下是完整數學運算的範例。** 假設我們在兩部伺服器中都有六個容量磁片磁碟機，而我們想要建立 1 100 GB 的磁片區，其中包含 10 GB 的鏡像和 90 GB 的同位。 伺服器本機雙向鏡像是 50.0% 有效率，這表示 10 GB 的鏡像資料會在每部伺服器上儲存 20 GB。 鏡像到這兩部伺服器，其總使用量為 40 GB。 在此情況下，伺服器本機單一同位檢查是 5/6 = 83.3% 有效率，這表示 90 GB 的同位資料會在每部伺服器上儲存 108 GB。 鏡像到這兩部伺服器，其總使用量為 216 GB。 總使用量會因此 [（10 GB/50.0% + （90 GB/83.3%）] × 2 = 256 GB，以達 39.1% 的整體容量效率。

請注意傳統雙向鏡像的容量效率（大約 50%）和嵌套的鏡像加速同位檢查（最多 40%）並不會有太大的差異。 視您的需求而定，較低的容量效率可能值得大幅增加儲存體可用性。 您可以選擇每個磁片區的復原功能，以便在相同的叢集中混合使用嵌套的復原磁片區和傳統雙向鏡像磁片區。

![折衷](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

您可以在 PowerShell 中使用熟悉的儲存體 Cmdlet 來建立具有嵌套復原功能的磁片區。

### <a name="step-1-create-storage-tier-templates"></a>步驟 1:建立儲存層範本

首先，使用 `New-StorageTier` Cmdlet 來建立新的儲存層範本。 您只需要執行這項動作一次，然後您所建立的每個新磁片區都可以參考這些範本。 指定容量磁片的 @no__t 0，並選擇性地指定您選擇的 `-FriendlyName`。 請勿修改其他參數。

如果您的容量磁片磁碟機是硬碟機（HDD），請以系統管理員身分啟動 PowerShell 並執行：

```PowerShell 
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk 
``` 

如果您的容量磁片磁碟機是固態硬碟（SSD），請改為將 `-MediaType` 設定為 `SSD`。 請勿修改其他參數。

> [!TIP]
> 使用 `Get-StorageTier` 確認已成功建立層。

### <a name="step-2-create-volumes"></a>步驟 2:建立磁碟區

然後，使用 `New-Volume` Cmdlet 來建立新的磁片區。

#### <a name="nested-two-way-mirror"></a>嵌套的雙向鏡像

若要使用「嵌套的雙向鏡像」，請參考 `NestedMirror` 層範本，並指定大小。 例如:

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>嵌套鏡像加速同位

若要使用嵌套鏡像加速同位，請參考 `NestedMirror` 和 @no__t 1 層範本，並指定兩個大小，分別用於磁片區的每個部分（鏡像優先、同位檢查秒）。 例如，若要建立 1 500 GB 的磁片區，其為 20% 的嵌套雙向鏡像和 80% 的嵌套同位，請執行：

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>步驟 3：在 Windows 系統管理中心繼續

使用嵌套復原的磁片區會以明確標籤出現在[Windows 系統管理中心](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)，如下列螢幕擷取畫面所示。 建立之後，您就可以使用 Windows 管理中心來管理和監視它們，就像儲存空間直接存取中的任何其他磁片區一樣。

![](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>選擇性：擴充至快取磁片磁碟機

透過其預設設定，嵌套的復原可防止同時遺失多個容量磁片磁碟機，或同時保護一部伺服器和一個容量磁片磁碟機。 若要將這項保護擴充到快取[磁片磁碟機](understand-the-cache.md)，有其他考慮：由於快取磁片磁碟機通常會提供*多個*容量磁片磁碟機的讀取*和寫入*快取，因此，確保您可以容忍在其他伺服器已關閉，只是不會快取寫入，而是會影響效能。

為了解決此案例，儲存空間直接存取提供選項，讓您在兩部伺服器的叢集中的一部伺服器關閉時自動停用寫入快取，然後在伺服器備份之後重新啟用寫入快取。 若要在不影響效能的情況下允許常式重新開機，則在伺服器關閉30分鐘之前，不會停用寫入快取。 一旦停用寫入快取，寫入快取的內容就會寫入容量裝置。 在此之後，伺服器可以容忍線上伺服器中失敗的快取裝置，但如果快取裝置失敗，則快取的讀取可能會延遲或失敗。

若要設定此行為（選擇性），請以系統管理員身分啟動 PowerShell 並執行：

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

一旦設定為**True**，快取行為會是：

| 實際                       | 快取行為                           | 可以容許快取磁片磁碟機遺失嗎？ |
|---------------------------------|------------------------------------------|--------------------------------|
| 兩部伺服器都啟動                 | 快取讀取和寫入，完整效能 | 是                            |
| 伺服器關閉，前30分鐘   | 快取讀取和寫入，完整效能 | 否（暫時）               |
| 前30分鐘後          | 僅快取讀取，受影響的效能   | 是（在快取寫入容量磁片磁碟機之後）                           |

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>我可以在雙向鏡像和嵌套復原之間轉換現有的磁片區嗎？

否，無法在復原類型之間轉換磁片區。 針對 Windows Server 2019 上的新部署，請在一段時間後決定最符合您需求的復原類型。 如果您要從 Windows Server 2016 升級，可以建立具有嵌套復原功能的新磁片區、遷移您的資料，然後刪除較舊的磁片區。

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>我可以搭配多種類型的容量磁片使用嵌套的復原嗎？

是，您只需在上述[步驟 1](#step-1-create-storage-tier-templates)中指定每一層的 `-MediaType`。 例如，在相同叢集中使用 NVMe、SSD 和 HDD，NVMe 會提供快取，而後面兩個則提供容量：將 @no__t 0 層設定為 `-MediaType SSD`，將 @no__t 2 層設為 `-MediaType HDD`。 在此情況下，請注意同位檢查容量效率取決於 HDD 磁片磁碟機的數目，而且每個伺服器至少需要4個。

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>我可以搭配3部或多部伺服器使用嵌套復原嗎？

否，只有在您的叢集只有2部伺服器時，才使用嵌套的復原功能。

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>我需要多少個磁片磁碟機才能使用嵌套的復原功能？

儲存空間直接存取所需的最小磁片磁碟機數目是每個伺服器節點4個容量磁片磁碟機，加上每個伺服器節點2個快取磁片磁碟機（如果有的話）。 這在 Windows Server 2016 中是不變的。 不需要任何額外的嵌套復原，而且保留容量的建議也不會變更。

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>Nested 復原是否會變更磁片磁碟機更換的運作方式？

資料分割

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>Nested 復原是否會變更伺服器節點取代的運作方式？

資料分割 若要取代伺服器節點和其磁片磁碟機，請遵循下列順序：

1. 淘汰傳出伺服器中的磁片磁碟機
2. 將含有其磁片磁碟機的新伺服器新增至叢集
3. 存放集區會重新平衡
4. 移除傳出伺服器及其磁片磁碟機

如需詳細資訊，請參閱[移除伺服器](remove-servers.md)主題。

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
- [瞭解儲存空間直接存取中的容錯](storage-spaces-fault-tolerance.md)
- [規劃儲存空間直接存取中的磁片區](plan-volumes.md)
- [在儲存空間直接存取中建立磁片區](create-volumes.md)
