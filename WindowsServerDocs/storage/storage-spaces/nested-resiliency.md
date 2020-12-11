---
description: 深入瞭解：儲存空間直接存取的嵌套復原
title: 儲存空間直接存取的嵌套復原
ms.author: jgerend
manager: dansimpspaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/15/2019
ms.openlocfilehash: e433539eced1a9f7a52bbaf9bc45e8a6586586ff
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039396"
---
# <a name="nested-resiliency-for-storage-spaces-direct"></a>儲存空間直接存取的嵌套復原

> 適用於：Windows Server 2019

嵌套復原是 Windows Server 2019 中 [儲存空間直接存取](storage-spaces-direct-overview.md) 的新功能，可讓兩部伺服器的叢集同時承受多個硬體故障，而不會遺失存放裝置可用性，讓使用者、應用程式和虛擬機器繼續執行而不會中斷。 本主題將說明其運作方式，並提供逐步指示，讓您開始使用，並回答最常遇到的問題。

## <a name="prerequisites"></a>必要條件

### <a name="green-checkmark-icon-consider-nested-resiliency-if"></a>![綠色核取記號圖示。](media/nested-resiliency/supported.png) 如果有下列情況，請考慮嵌套復原：

- 您的叢集執行的是 Windows Server 2019;和
- 您的叢集剛好有2個伺服器節點

### <a name="red-x-icon-you-cant-use-nested-resiliency-if"></a>![紅色 X 圖示。](media/nested-resiliency/unsupported.png) 如果有下列情況，您就無法使用嵌套復原：

- 您的叢集執行的是 Windows Server 2016;或
- 您的叢集有3個或更多個伺服器節點

## <a name="why-nested-resiliency"></a>為何要將嵌套復原

使用嵌套復原的磁片區可以 **保持連線且可供存取，即使同時發生多個硬體失敗**，與傳統 [的雙向鏡像](storage-spaces-fault-tolerance.md) 復原不同。 例如，如果兩個磁片磁碟機同時失敗，或如果伺服器停止運作且磁片磁碟機失敗，使用嵌套復原功能的磁片區會保持連線並可供存取。 針對超融合式基礎結構，這會增加應用程式和虛擬機器的執行時間;針對檔案伺服器工作負載，這表示使用者可不間斷地存取其檔案。

![儲存體可用性](media/nested-resiliency/storage-availability.png)

代價是，嵌套的復原功能 **比傳統雙向鏡像的容量效率低**，這表示您的可用空間會稍微減少。 如需詳細資訊，請參閱下方的 [容量效率](#capacity-efficiency) 一節。

## <a name="how-it-works"></a>運作方式

### <a name="inspiration-raid-51"></a>靈感： RAID 5 + 1

RAID 5 + 1 是已建立的分散式儲存體復原形式，可提供實用的背景來瞭解嵌套的復原能力。 在 RAID 5 + 1 中，每部伺服器內的本機復原都是由 RAID-5 或 *單一同* 位提供，以防止任何單一磁片磁碟機遺失。 然後，在兩部伺服器之間，RAID 1 或 *雙向鏡像* 會提供進一步的復原功能，以防止任一伺服器遺失。

![RAID 5 + 1](media/nested-resiliency/raid-51.png)

### <a name="two-new-resiliency-options"></a>兩個新的復原選項

Windows Server 2019 中的儲存空間直接存取提供在軟體中執行的兩個新的復原選項，而不需要特製化 RAID 硬體：

- **嵌套的雙向鏡像。** 在每一部伺服器中，本機復原是由雙向鏡像提供，接著會透過兩個伺服器之間的雙向鏡像來提供進一步的復原功能。 這基本上是四向鏡像，每部伺服器都有兩個複本。 嵌套的雙向鏡像可提供高效能：寫入至所有複本，而讀取則來自任何複本。

  ![嵌套的雙向鏡像](media/nested-resiliency/nested-two-way-mirror.png)

- **嵌套的鏡像加速同位。** 結合上述的嵌套雙向鏡像與嵌套的同位。 在每一部伺服器內，大部分資料的本機復原都是由單一 [位同位演算法](storage-spaces-fault-tolerance.md#parity)所提供，但新的最近寫入則是使用雙向鏡像。 然後，所有資料的進一步復原都是由伺服器之間的雙向鏡像所提供。 如需鏡像加速同位運作方式的詳細資訊，請參閱 [鏡像加速同位](../refs/mirror-accelerated-parity.md)。

  ![嵌套的鏡像加速同位](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="capacity-efficiency"></a>容量效率

容量效率是可用空間與 [磁片](plan-volumes.md#choosing-the-size-of-volumes)區使用量的比率。 它會描述因復原而異的容量額外負荷，並取決於您選擇的復原選項。 簡單的範例是，在不具復原性的情況下儲存資料是100% 的容量效率 (1 TB 的資料會佔用 1 TB 的實體儲存體容量) ，而雙向鏡像則是50% 的高效率 (1 TB 的資料會佔用 2 TB 的實體儲存體容量) 。

- **嵌套的雙向鏡像** 會寫入四個專案的複本，亦即儲存 1 TB 的資料，您需要 4 TB 的實體儲存體容量。 雖然其簡化功能很吸引人，但是在儲存空間直接存取的任何復原選項中，以嵌套的雙向鏡像的容量效率為25%。

- **嵌套的鏡像加速同位** 可達到更高的容量效率，大約是 35%-40%，這取決於兩個因素：每部伺服器的容量磁片磁碟機數目，以及您為磁片區指定的鏡像和同位檢查混合。 下表提供一般設定的查閱：

  | 每一伺服器的容量磁片磁碟機 | 10% 鏡像 | 20% 鏡像 | 30% 鏡像 |
  |----------------------------|------------|------------|------------|
  | 4                          | 35.7%      | 34.1%      | 32.6%      |
  | 5                          | 37.7%      | 35.7%      | 33.9%      |
  | 6                          | 39.1%      | 36.8%      | 34.7%      |
  | 7+                         | 40.0%      | 37.5%      | 35.3%      |

  > [!NOTE]
  > **如果您很好奇，以下是完整數學的範例。** 假設在兩部伺服器中有六個容量磁片磁碟機，而我們想要建立 1 100 GB 的磁片區，其中包含 10 GB 的鏡像和 90 GB 的同位。 伺服器本機雙向鏡像的50.0% 有效率，這表示 10 GB 的鏡像資料在每部伺服器上都需要 20 GB 的儲存空間。 鏡像至兩部伺服器，其總使用量為 40 GB。 在此情況下，伺服器本機單一同位（在此案例中為 5/6 = 83.3%），這表示 90 GB 的同位資料在每部伺服器上都需要 108 GB 的儲存空間。 鏡像至兩部伺服器，其總使用量為 216 GB。 因此，總使用量如下： [ (10 GB/50.0% ) + (90 GB/83.3% ) ] × 2 = 256 GB，適用于39.1% 的整體容量效率。

請注意，傳統雙向鏡像的容量效率 (大約 50% ) ，而嵌套的鏡像加速同位 (高達 40% ) 並不太不同。 視您的需求而定，較低的容量效率可能很值得大幅提高儲存體可用性。 您可以選擇每個磁片區的復原，因此您可以在相同的叢集中混用嵌套的復原磁片區和傳統的雙向鏡像磁片區。

![權衡](media/nested-resiliency/tradeoff.png)

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

您可以在 PowerShell 中使用熟悉的儲存體 Cmdlet 來建立具有嵌套復原功能的磁片區。

### <a name="step-1-create-storage-tier-templates"></a>步驟1：建立儲存層範本

首先，使用 Cmdlet 建立新的儲存層範本 `New-StorageTier` 。 您只需要做一次，然後您所建立的每個新磁片區都可以參考這些範本。 指定 `-MediaType` 您的容量磁片磁碟機，並選擇性地指定 `-FriendlyName` 您選擇的。 請勿修改其他參數。

如果您的容量磁片磁碟機是硬碟 (HDD) ，請以系統管理員身分啟動 PowerShell 並執行：

```PowerShell
# For mirror
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedMirror -ResiliencySettingName Mirror -MediaType HDD -NumberOfDataCopies 4

# For parity
New-StorageTier -StoragePoolFriendlyName S2D* -FriendlyName NestedParity -ResiliencySettingName Parity -MediaType HDD -NumberOfDataCopies 2 -PhysicalDiskRedundancy 1 -NumberOfGroups 1 -FaultDomainAwareness StorageScaleUnit -ColumnIsolation PhysicalDisk
```

如果您的容量磁片磁碟機是固態硬碟 (SSD) ，請 `-MediaType` 改為將設定為 `SSD` 。 請勿修改其他參數。

> [!TIP]
> 確認已成功建立使用的層 `Get-StorageTier` 。

### <a name="step-2-create-volumes"></a>步驟2：建立磁片區

然後，使用 Cmdlet 建立新的磁片區 `New-Volume` 。

#### <a name="nested-two-way-mirror"></a>嵌套的雙向鏡像

若要使用嵌套的雙向鏡像，請參考階層 `NestedMirror` 範本並指定大小。 例如：

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume01 -StorageTierFriendlyNames NestedMirror -StorageTierSizes 500GB
```

#### <a name="nested-mirror-accelerated-parity"></a>嵌套的鏡像加速同位

若要使用嵌套的鏡像加速同位，請參考 `NestedMirror` 和階層 `NestedParity` 範本，並指定兩個大小，每個磁片區的每個部分都有一個大小 (鏡像第一個、同位檢查第二個)  例如，若要建立20% 的嵌套雙向鏡像和80% 的嵌套同位檢查的 1 500 GB 磁片區，請執行：

```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName Volume02 -StorageTierFriendlyNames NestedMirror, NestedParity -StorageTierSizes 100GB, 400GB
```

### <a name="step-3-continue-in-windows-admin-center"></a>步驟3：在 Windows Admin Center 中繼續

使用嵌套復原的磁片區會以明確標籤顯示在 [Windows Admin Center](../../manage/windows-admin-center/overview.md) 中，如下列螢幕擷取畫面所示。 一旦建立之後，您就可以使用 Windows Admin Center 來管理和監視它們，就像儲存空間直接存取中的任何其他磁片區一樣。

![Windows Admin Center 中的磁片區管理](media/nested-resiliency/windows-admin-center.png)

### <a name="optional-extend-to-cache-drives"></a>選用：延伸至快取磁片磁碟機

使用其預設設定時，嵌套復原可以防止同時遺失多個容量磁片磁碟機，或是同時使用一部伺服器和一個容量磁片磁碟機。 若要將這項保護延伸至快取 [磁片磁碟機](understand-the-cache.md)，請考慮其他事項：由於快取磁片磁碟機通常會提供 *多個* 容量磁片磁碟機的讀取 *和寫入* 快取，因此當另一部伺服器關閉時，確保您可以容忍快取磁片磁碟機遺失的唯一方法就是只是不快取寫入，但會影響效能。

為了解決這種情況，儲存空間直接存取提供選項，讓您在兩部伺服器的叢集中的一部伺服器關閉時，自動停用寫入快取，然後在伺服器備份之後重新啟用寫入快取。 若要在不影響效能的情況下允許例行重新開機，則在伺服器已關閉30分鐘之後，就不會停用寫入快取。 一旦停用寫入快取，寫入快取的內容就會寫入容量裝置。 如此一來，伺服器就能容忍線上伺服器中失敗的快取裝置，不過如果快取裝置失敗，快取的讀取可能會延遲或失敗。

若要將此行為設定 (選用) ，請以系統管理員身分啟動 PowerShell 並執行：

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.NestedResiliency.DisableWriteCacheOnNodeDown.Enabled" -Value "True"
```

一旦設為 **True**，快取行為為：

| 情況                       | 快取行為                           | 是否可容忍快取磁片磁碟機遺失？ |
|---------------------------------|------------------------------------------|--------------------------------|
| 這兩部伺服器都已啟動                 | 快取讀取和寫入，完整效能 | 是                            |
| 伺服器關閉，前30分鐘   | 快取讀取和寫入，完整效能 | 沒有暫時) 的 (               |
| 前30分鐘後          | 僅快取讀取，受影響的效能   | 是 (將快取寫入容量磁片磁碟機之後)                            |

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="can-i-convert-an-existing-volume-between-two-way-mirror-and-nested-resiliency"></a>是否可以在雙向鏡像與嵌套復原之間轉換現有的磁片區？

否，磁片區無法在復原類型之間轉換。 針對 Windows Server 2019 上的新部署，請事先決定最適合您需求的復原類型。 如果您要從 Windows Server 2016 進行升級，您可以建立具有嵌套復原功能的新磁片區、遷移您的資料，然後刪除較舊的磁片區。

### <a name="can-i-use-nested-resiliency-with-multiple-types-of-capacity-drives"></a>我可以使用具有多個容量磁片磁碟機類型的嵌套復原嗎？

是，您只需 `-MediaType` 在上述 [步驟 1](#step-1-create-storage-tier-templates) 中指定每個層級的。 例如，在相同叢集中使用 NVMe、SSD 和 HDD，NVMe 會提供快取，而後者則提供容量：將層級設定為，以及將階層設定 `NestedMirror` `-MediaType SSD` `NestedParity` 為 `-MediaType HDD` 。 在此情況下，請注意，同位檢查容量的效率取決於 HDD 磁片磁碟機的數量，而且每個伺服器至少需要4個磁片磁碟機。

### <a name="can-i-use-nested-resiliency-with-3-or-more-servers"></a>我可以搭配3部或多部伺服器使用 nested 復原嗎？

否，只有在您的叢集只有2部伺服器時，才使用嵌套的復原。

### <a name="how-many-drives-do-i-need-to-use-nested-resiliency"></a>我需要多少個磁片磁碟機才能使用嵌套復原？

儲存空間直接存取所需的磁片磁碟機數目下限為每個伺服器節點4個容量磁片磁碟機，加上2個每個伺服器節點的快取磁片磁碟機 (如果有任何) 。 這與 Windows Server 2016 的變更不一樣。 不需要額外的嵌套復原，而且保留容量的建議也不會變更。

### <a name="does-nested-resiliency-change-how-drive-replacement-works"></a>嵌套復原是否會變更磁片磁碟機取代的運作方式？

否。

### <a name="does-nested-resiliency-change-how-server-node-replacement-works"></a>嵌套復原是否會變更伺服器節點取代的運作方式？

否。 若要取代伺服器節點及其磁片磁碟機，請遵循下列順序：

1. 淘汰傳出伺服器中的磁片磁碟機
2. 將新的伺服器及其磁片磁碟機新增至叢集
3. 儲存集區會重新平衡
4. 移除傳出的伺服器及其磁片磁碟機

如需詳細資訊，請參閱 [移除伺服器](remove-servers.md) 主題。

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
- [瞭解儲存空間直接存取中的容錯能力](storage-spaces-fault-tolerance.md)
- [規劃儲存空間直接存取中的磁片區](plan-volumes.md)
- [建立儲存空間直接存取中的磁碟區](create-volumes.md)
