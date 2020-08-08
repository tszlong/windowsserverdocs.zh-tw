---
title: 儲存空間直接存取記憶體內部讀取快取
ms.author: eldenc
manager: siroy
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: ce4546a3c3933700b7aec812027e2abc91f718f9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960911"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>使用儲存空間直接存取搭配 CSV 記憶體內部讀取快取
> 適用於：Windows Server 2016、Windows Server 2019

本主題描述如何使用系統記憶體來提升[儲存空間直接存取](storage-spaces-direct-overview.md)的效能。

儲存空間直接存取與叢集共用磁碟區 (CSV) 記憶體內部讀取快取相容。 使用系統記憶體來快取讀取可以改善 Hyper-v 之類應用程式的效能，這會使用未緩衝的 i/o 來存取 VHD 或 VHDX 檔案。  (未緩衝的 Io 是 Windows 快取管理員不會快取的任何作業。 ) 

因為記憶體內部快取是伺服器本機，所以可改善超融合儲存空間直接存取部署的資料位置：最近的讀取會在虛擬機器執行所在的相同主機上，于記憶體中快取，以減少讀取網路的頻率。 這會導致延遲較低且儲存體效能更佳。

## <a name="planning-considerations"></a>規劃考量

記憶體內部讀取快取最適用于大量讀取的工作負載，例如 (VDI) 的虛擬桌面基礎結構。 相反地，如果工作負載非常密集寫入，則快取可能會造成額外的負擔，而不是值，應該予以停用。

您最多可以為 CSV 記憶體內部讀取快取使用80% 的總實體記憶體。

  > [!TIP]
  > 針對在相同伺服器上執行計算和儲存體的超交集部署，請小心為您的虛擬機器保留足夠的記憶體。 針對交集的向外延展檔案伺服器 (SoFS) 部署，對記憶體的爭用較少，這並不適用。

  > [!NOTE]
  > 某些 microbenchmarking 工具（例如 DISKSPD 和[VM 車隊](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet)）可能會產生較差的結果，並啟用 CSV 記憶體中的讀取快取，而不需要它。 根據預設，VM 車隊會針對每個虛擬機器建立 1 10 GiB VHDX – 100 Vm 約有 1 TiB 總計–然後執行*一致的隨機*讀取和寫入。 不同于實際的工作負載，讀取不會遵循任何可預測或重複的模式，因此記憶體內部快取不會生效，而且只會產生額外負荷。

## <a name="configuring-the-in-memory-read-cache"></a>設定記憶體內部讀取快取

CSV 記憶體內部讀取快取可在 Windows Server 2016 和 Windows Server 2019 中使用相同的功能。 在 Windows Server 2016 中，預設為關閉。 在 Windows Server 2019 中，它預設為開啟，並配置 1 GB。

| OS 版本          | 預設 CSV 快取大小 |
|---------------------|------------------------|
| Windows Server 2016 | 0 (停用)           |
| Windows Server 2019 | 1 GiB                   |

若要查看使用 PowerShell 配置多少記憶體，請執行：

```PowerShell
(Get-Cluster).BlockCacheSize
```

傳回的值是每一伺服器的數量 (MiB) 。 例如， `1024` 代表1個 gib (GiB) 。

若要變更配置的記憶體數量，請使用 PowerShell 來修改此值。 例如，若要為每部伺服器配置2個 GiB，請執行：

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

若要讓變更立即生效，請暫停再繼續您的 CSV 磁片區，或在伺服器之間移動它們。 例如，使用此 PowerShell 片段將每個 CSV 移至另一個伺服器節點，然後再返回一次：

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
