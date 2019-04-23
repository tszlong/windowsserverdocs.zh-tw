---
title: 儲存空間直接存取記憶體中讀取快取
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850549"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>儲存空間直接存取使用 CSV 在記憶體中讀取快取
> 適用於：Windows Server 2016、windows Server 2019

本主題描述如何使用系統記憶體，以提高的效能[儲存空間直接存取](storage-spaces-direct-overview.md)。

儲存空間直接存取適用於叢集共用磁碟區 (CSV) 在記憶體中讀取快取。 使用快取讀取的系統記憶體，可以改善像是 HYPER-V，它會使用無緩衝的 I/O 存取 VHD 或 VHDX 檔案的應用程式的效能。 （無緩衝的 IOs 是不會快取由 「 Windows 快取管理員 」 中的任何作業）。

因為記憶體中快取是伺服器本機，所以它可以改善超融合儲存空間直接存取部署的資料位置： 最新的讀取快取在記憶體中的虛擬機器執行所在的相同主機上，頻率減少讀取經過網路。 這會導致較低的延遲和更佳的儲存體效能。

## <a name="planning-considerations"></a>規劃考量

在記憶體中讀取快取是最有效率的大量讀取工作負載，例如虛擬桌面基礎結構 (VDI)。 相反地，如果工作負載非常密集寫入，快取可能會造成更多的額外負荷比值，並應停用。

您可以使用最多 80%的總實體記憶體，CSV 在記憶體中讀取快取。

  > [!TIP]
  > 超融合式部署中，其中計算和儲存在相同的伺服器上執行務必保留足夠的記憶體，虛擬機器。 交集的向外延展檔案伺服器 (SoFS) 部署，使用較少競爭記憶體，這不適用。

  > [!NOTE]
  > 某些 microbenchmarking 之類的工具 DISKSPD 並[VM Fleet](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet)可能會產生較差的結果與 CSV 記憶體中讀取快取啟用時比不使用它。 根據預設 VM 團隊會建立一個 10 GiB VHDX，每個虛擬機器-大約 1 TiB 的 100 個 Vm – 總，然後執行*統一隨機*讀取並寫入它們。 不同於實際工作負載，讀取未遵循任何可預測或重複的模式，因此不是有效的記憶體中快取，並只會產生額外負荷。

## <a name="configuring-the-in-memory-read-cache"></a>設定在記憶體中讀取快取

CSV 在記憶體中讀取快取已在 Windows Server 2016 和 Windows Server 2019 與相同的功能。 在 Windows Server 2016 中，它預設為關閉。 在 Windows Server 2019，它是位於預設配置的 1 gb。

| OS 版本          | 預設 CSV 快取大小 |
|---------------------|------------------------|
| Windows Server 2016 | 0 （停用）           |
| Windows Server 2019 | 1 GiB                   |

若要查看使用 PowerShell 來配置多少記憶體，請執行：

```PowerShell
(Get-Cluster).BlockCacheSize
```

傳回的值是在每一部伺服器的 mebibytes (MiB)。 比方說，`1024`代表 1 gib (GiB)。

若要變更配置多少記憶體，修改此值使用 PowerShell。 比方說，若要配置 2 GiB 每一部伺服器，請執行：

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

變更會立即生效，暫停然後繼續您的 CSV 磁碟區，或伺服器之間移動它們。 比方說，若要將每個 CSV 移至另一個伺服器節點，然後再次使用此 PowerShell 片段：

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
