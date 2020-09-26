---
title: 儲存空間直接存取記憶體中讀取快取
ms.author: eldenc
manager: siroy
ms.topic: article
author: eldenchristensen
ms.date: 09/21/2020
ms.localizationpriority: medium
ms.openlocfilehash: 94f541b38e0084ae3284dc0e56b2643f23b15bfe
ms.sourcegitcommit: 8a826e992f28a70e75137f876a5d5e61238a24e4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365331"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>使用儲存空間直接存取搭配 CSV 記憶體內部讀取快取

> 適用於：Windows Server 2019、Windows Server 2016

本主題說明如何使用系統記憶體來提升 [儲存空間直接存取](storage-spaces-direct-overview.md)的效能。

儲存空間直接存取與叢集共用磁碟區 (CSV) 記憶體中讀取快取相容。 使用系統記憶體快取讀取可以改善 Hyper-v 應用程式的效能，這會使用未緩衝的 i/o 來存取 VHD 或 VHDX 檔案。  (未緩衝的 IOs 是 Windows 快取管理員未快取的任何作業。 ) 

因為記憶體中的快取是伺服器本機的，所以它會改善超交集儲存空間直接存取部署的資料位置：最近的讀取會在虛擬機器執行所在的相同主機上的記憶體中快取，減少網路讀取的頻率。 這會導致較低的延遲和較佳的儲存體效能。

## <a name="planning-considerations"></a>規劃考量

記憶體中的讀取快取最適用于需要大量讀取的工作負載，例如虛擬桌面基礎結構 (VDI) 。 相反地，如果工作負載非常密集寫入，快取可能會導致比值更多的額外負荷，而且應該停用。

您最多可以針對 CSV 記憶體中讀取快取使用80% 的總實體記憶體。

  > [!TIP]
  > 針對在相同伺服器上執行計算和儲存體的超交集部署，請小心將足夠的記憶體保留給您的虛擬機器。 針對交集的向外延展檔案伺服器 (SoFS) 部署，記憶體的爭用較少，這並不適用。

  > [!NOTE]
  > 某些 microbenchmarking 工具（例如 DISKSPD 和 [VM 車隊](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) ）可能會產生較差的結果，並啟用 CSV 記憶體中的讀取快取，而不需要它。 依預設，VM 車隊會為每部虛擬機器建立 1 10 gib (GiB) VHDX –大約 1 TiB 的 Vm 總數，然後對它們執行一致100的 *隨機* 讀取和寫入。 不同于實際的工作負載，讀取不會遵循任何可預測或重複的模式，因此記憶體內部快取不會有效，而只會產生額外負荷。

## <a name="configuring-the-in-memory-read-cache"></a>設定記憶體中讀取快取

CSV 記憶體內部讀取快取可在具有相同功能的 Windows Server 2019 和 Windows Server 2016 中使用。 在 Windows Server 2019 中，預設會開啟並配置1個 GiB。 在 Windows Server 2016 中，它預設是關閉的。

| OS 版本             | 預設 CSV 快取大小           |
|------------------------|----------------------------------|
| Windows Server 2019    | 1 GiB                            |
| Windows Server 2016    | 0 (停用)                     |
| Windows Server 2012 R2 | enabled-使用者必須指定大小 |
| Windows Server 2012    | 0 (停用)                     |

若要查看使用 PowerShell 配置的記憶體數量，請執行：

```PowerShell
(Get-Cluster).BlockCacheSize
```

傳回的值是每一部伺服器的數量 (MiB) 。 例如， `1024` 代表 1 gib (GiB) 。

若要變更配置的記憶體數量，請使用 PowerShell 來修改此值。 例如，若要配置每部伺服器的2個 GiB，請執行：

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

若要讓變更立即生效，請暫停然後繼續您的 CSV 磁片區，或在伺服器之間移動它們。 例如，使用此 PowerShell 片段，將每個 CSV 移至另一個伺服器節點，然後再次移回：

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="additional-references"></a>其他參考

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
