---
title: 延伸儲存空間直接存取中的磁碟區
description: 如何使用 Windows Admin Center 和 PowerShell 調整儲存空間直接存取中的磁片區大小。
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.date: 03/10/2020
ms.topic: article
ms.openlocfilehash: 38ef41ac6cdb35efc72087ad73f08b04c1414c1a
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948574"
---
# <a name="extending-volumes-in-storage-spaces-direct"></a>延伸儲存空間直接存取中的磁碟區
> 適用於：Windows Server 2019、Windows Server 2016

本主題提供如何使用 Windows Admin Center 來調整 [儲存空間直接存取](storage-spaces-direct-overview.md) 叢集上的磁片區大小的指示。

> [!WARNING]
> **不支援：調整儲存空間直接存取所使用的基礎儲存體大小。** 如果您在虛擬化儲存體環境（包括 Azure）中執行儲存空間直接存取，則不支援調整或變更虛擬機器所使用之存放裝置的特性，而且會導致資料變成無法存取。 相反地，請依照 [ [新增伺服器或磁片磁碟機](add-nodes.md) ] 區段中的指示，在擴充磁片區之前新增額外容量。

觀賞如何調整磁片區大小的快速影片。

> [!VIDEO https://www.youtube-nocookie.com/embed/hqyBzipBoTI]

## <a name="extending-volumes-using-windows-admin-center"></a>使用 Windows Admin Center 擴充磁片區

1. 在 Windows Admin Center 中，連線到儲存空間直接存取叢集，然後從 [**工具**] 窗格中選取 [**磁片** 區]。
2. 在 [磁片區] 頁面上，選取 [ **清查** ] 索引標籤，然後選取要調整大小的磁片區。

    在 [磁片區詳細資料] 頁面上，會指出磁片區的儲存容量。 您也可以直接從儀表板開啟 [磁片區詳細資料] 頁面。 在 [儀表板] 的 [警示] 窗格中，選取警示，此警示會在磁片區的儲存容量不足時通知您，然後選取 [ **移至磁片** 區]。

4. 在 [磁片區詳細資料] 頁面的頂端，選取 [重 **設大小**]。
5. 輸入新的較大的大小，然後選取 [重 **設** 大小]。

    在 [磁片區詳細資料] 頁面上，會指出磁片區的較大儲存容量，並清除儀表板上的警示。

## <a name="extending-volumes-using-powershell"></a>使用 PowerShell 擴充磁片區

### <a name="capacity-in-the-storage-pool"></a>儲存集區的容量

您可以調整磁碟區大小之前，請確定儲存集區中有足夠的容量，以容納其新的、更大的使用量。 例如，將三向鏡像磁碟區從 1 TB 大小調整為 2 TB 時，使用量會從 3 TB 增加到 6 TB。 為了調整大小成功，儲存集區將需要至少 (6 - 3) = 3 TB 的可用容量。

### <a name="familiarity-with-volumes-in-storage-spaces"></a>熟悉儲存空間的磁碟區

在儲存空間直接存取，每個磁碟區包含幾個堆疊物件：叢集共用磁碟區 (CSV) (這是磁碟區)、磁碟分割、磁碟 (這是虛擬磁碟)，以及一或多個儲存層（如果有的話）。 若要調整磁碟區大小，您將需要調整幾個物件大小。

![volumes-in-smapi](media/resize-volumes/volumes-in-smapi.png)

若要熟悉它們，請嘗試執行 **Get-** 搭配 PowerShell 的對應名詞使用。

例如：

```PowerShell
Get-VirtualDisk
```

若要追蹤堆疊中物件之間的關聯，使用管線將一個 **Get-** cmdlet 的結果傳送至下一個，當做輸入。

例如，以下是如何從虛擬磁碟取得，一直到其磁碟區︰

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-Disk | Get-Partition | Get-Volume
```

### <a name="step-1--resize-the-virtual-disk"></a>步驟 1 – 調整虛擬磁碟大小

虛擬磁碟不一定使用儲存層，根據建立方式。

若要檢查，請執行下列 Cmdlet：

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier
```

如果 cmdlet 沒有傳回任何項目，虛擬磁碟不使用儲存層。

#### <a name="no-storage-tiers"></a>無儲存層

如果虛擬磁碟沒有儲存層，您可以使用 **Resize-VirtualDisk** cmdlet 直接調整大小。

以 **-Size** 參數提供新的大小。

```PowerShell
Get-VirtualDisk <FriendlyName> | Resize-VirtualDisk -Size <Size>
```

當您調整 **VirtualDisk** 大小，**Disk** 會自動接續，也調整大小。

![Resize-VirtualDisk](media/resize-volumes/Resize-VirtualDisk.gif)

#### <a name="with-storage-tiers"></a>使用儲存層

如果虛擬磁碟使用儲存層，您可以使用 **Resize-StorageTier** cmdlet 分別調整每個層大小。

從虛擬磁碟追蹤關聯，取得儲存層的名稱。

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier | Select FriendlyName
```

然後，針對每個層，以 **-Size** 參數提供新的大小。

```PowerShell
Get-StorageTier <FriendlyName> | Resize-StorageTier -Size <Size>
```

> [!TIP]
> 如果您的層級是不同的實體媒體類型 (例如 **MediaType = SSD** 和 **MediaType = HDD**)，您要確定儲存集區有足夠容量的媒體類型，以容納每一層新的、更大的使用量。

當您調整 **StorageTier** 大小，**VirtualDisk** 和 **Disk** 會自動接續，也調整大小。

![Resize-StorageTier](media/resize-volumes/Resize-StorageTier.gif)

### <a name="step-2--resize-the-partition"></a>步驟 2 – 調整磁碟分割大小

接下來，使用 **Resize-Partition** cmdlet 調整磁碟分割大小。 虛擬磁碟預期有兩個磁碟分割：第一個已保留，不應修改。第二個需要調整大小，有下列值 **PartitionNumber = 2** 和 **Type = Basic**。

以 **-Size** 參數提供新的大小。 我們建議使用支援的大小上限，如下所示。

```PowerShell
# Choose virtual disk
$VirtualDisk = Get-VirtualDisk <FriendlyName>

# Get its partition
$Partition = $VirtualDisk | Get-Disk | Get-Partition | Where PartitionNumber -Eq 2

# Resize to its maximum supported size
$Partition | Resize-Partition -Size ($Partition | Get-PartitionSupportedSize).SizeMax
```

當您調整 **Partition** 大小，**Volume** 和 **ClusterSharedVolume** 會自動接續，也調整大小。

![Resize-Partition](media/resize-volumes/Resize-Partition.gif)

這樣就完成了！

> [!TIP]
> 您可以執行 **Get-Volume** 檢查磁碟區是否有新的大小。

## <a name="additional-references"></a>其他參考資料

- [Windows Server 2016 中的儲存空間直接存取](storage-spaces-direct-overview.md)
- [規劃儲存空間直接存取中的磁碟區](plan-volumes.md)
- [建立儲存空間直接存取中的磁碟區](create-volumes.md)
- [在儲存空間直接存取中刪除磁片區](delete-volumes.md)
