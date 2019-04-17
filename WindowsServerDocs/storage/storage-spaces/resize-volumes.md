---
title: 延伸儲存空間直接存取中的磁碟區
ms.assetid: fa48f8f7-44e7-4a0b-b32d-d127eff470f0
ms.prod: windows-server-threshold
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 01/23/2017
ms.localizationpriority: medium
ms.openlocfilehash: 51f58ec23c55a6cb1664d800d6f4a75dae545899
ms.sourcegitcommit: dfd25348ea3587e09ea8c2224237a3e8078422ae
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "4678647"
---
# 延伸儲存空間直接存取中的磁碟區
> 適用於： Windows Server 2019、 Windows Server 2016

本主題將說明如何調整[直接儲存空間](storage-spaces-direct-overview.md)中的磁碟區大小。

## 必要條件

### 儲存集區的容量

您可以調整磁碟區大小之前，請確定儲存集區中有足夠的容量，以容納其新的、更大的使用量。 例如，將三向鏡像磁碟區從 1 TB 大小調整為 2 TB 時，使用量會從 3 TB 增加到 6 TB。 為了調整大小成功，儲存集區將需要至少 (6 - 3) = 3 TB 的可用容量。

### 熟悉儲存空間的磁碟區

在儲存空間直接存取，每個磁碟區包含幾個堆疊物件：叢集共用磁碟區 (CSV) (這是磁碟區)、磁碟分割、磁碟 (這是虛擬磁碟)，以及一或多個儲存層（如果有的話）。 若要調整磁碟區大小，您將需要調整幾個物件大小。

![volumes-in-smapi](media/resize-volumes/volumes-in-smapi.png)

若要熟悉它們，請嘗試執行**Get-** 搭配 PowerShell 的對應名詞使用。

例如：

```PowerShell
Get-VirtualDisk
```

若要追蹤堆疊中物件之間的關聯，使用管線將一個 **Get-** cmdlet 的結果傳送至下一個，當做輸入。

例如，以下是如何從虛擬磁碟取得，一直到其磁碟區︰

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-Disk | Get-Partition | Get-Volume 
```

## 步驟 1 – 調整虛擬磁碟大小

虛擬磁碟不一定使用儲存層，根據建立方式。

若要檢查，請執行下列 Cmdlet：

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier 
```

如果 cmdlet 沒有傳回任何項目，虛擬磁碟不使用儲存層。

### 無儲存層

如果虛擬磁碟沒有儲存層，您可以使用 **Resize-VirtualDisk** cmdlet 直接調整大小。

以 **-Size** 參數提供新的大小。

```PowerShell
Get-VirtualDisk <FriendlyName> | Resize-VirtualDisk -Size <Size>
```

當您調整 **VirtualDisk** 大小，**Disk** 會自動接續，也調整大小。

![Resize-VirtualDisk](media/resize-volumes/Resize-VirtualDisk.gif)

### 使用儲存層

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

## 步驟 2 – 調整磁碟分割大小

接下來，使用 **Resize-Partition**cmdlet 調整磁碟分割大小。 虛擬磁碟預期有兩個磁碟分割：第一個已保留，不應修改。第二個需要調整大小，有下列值 **PartitionNumber = 2** 和 **Type = Basic**。

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

## 請參閱

- [Windows Server 2016 中的儲存空間直接存取](storage-spaces-direct-overview.md)
- [規劃儲存空間直接存取中的磁碟區](plan-volumes.md)
- [建立儲存空間直接存取中的磁碟區](create-volumes.md)
