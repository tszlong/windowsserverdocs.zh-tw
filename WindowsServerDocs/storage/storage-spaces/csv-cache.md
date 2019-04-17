---
title: 儲存空間直接存取記憶體內部讀取快取
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122051"
---
# 儲存空間直接存取使用 CSV 記憶體內部讀取快取
> 適用於： Windows Server 2016、 Windows Server 2019

本主題說明如何使用提高效能的[儲存空間直接存取](storage-spaces-direct-overview.md)的系統記憶體。

儲存空間直接存取是與叢集共用磁碟區 (CSV) 記憶體內部讀取快取相容。 使用系統記憶體快取讀取可增加應用程式，例如 HYPER-V，它會使用未緩衝處理的 I/O 存取 VHD 或 VHDX 檔案的效能。 （未緩衝處理的 IOs 都不會快取由 Windows 快取管理員中的任何作業）。

因為記憶體中快取伺服器本機，它可改善資料區域性超交集的儲存空間直接存取部署： 最近讀取快取在何處執行虛擬機器在相同主機上的記憶體，減少頻率，以及讀取通過網路。 這會導致較低延遲和更好的存放裝置效能。

## 規劃考量

在記憶體內部讀取快取是最有效的讀取大量工作負載，例如虛擬桌面基礎結構 (VDI)。 相反地，如果工作負載是非常大量的寫入，快取可能會超過值的多個額外負荷，並應該停用。

您可以使用 CSV 記憶體內部讀取快取高達 80%的總實體記憶體。

  > [!TIP]
  > 對於超交集的部署，其中運算和儲存在相同的伺服器上執行，請小心離開您的虛擬機器足夠的記憶體。 適用於具有較少的記憶體，爭用融合式向外延展檔案伺服器 (SoFS) 部署，這不適用。

  > [!NOTE]
  > 某些 microbenchmarking 工具，像是 DISKSPD 和[VM 艦隊](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet)可能會產生更糟的是結果使用 CSV 記憶體內部讀取快取啟用比沒有它。 根據預設 VM 艦隊會建立一個 10 鉤每個虛擬機器 – 約 1 TiB VHDX 總計為 100 Vm – 以及再執行*統一隨機*的讀取與寫入它們。 不同於實際的工作負載，讀取未依照任何可預測或重複模式，因此不是有效的記憶體中快取，並只會產生額外負荷。

## 設定記憶體內部讀取快取

CSV 記憶體內部讀取快取中有提供 Windows Server 2016 和 Windows Server 2019 都具有相同功能。 在 Windows Server 2016 中，它預設為關閉。 在 Windows Server 2019 中，它預設為開啟具有 1 GB 的配置。

| 作業系統版本          | 預設 CSV 快取大小 |
|---------------------|------------------------|
| WindowsServer 2016 | 0 （停用）           |
| Windows Server 2019 | 1 鉤                   |

若要查看使用 PowerShell 來配置多少記憶體，請執行：

```PowerShell
(Get-Cluster).BlockCacheSize
```

傳回的值是在每個伺服器 mebibytes (MiB)。 例如，`1024`代表 1 gibibyte （鉤）。

若要變更多少記憶體配置，修改此值使用 PowerShell。 例如，若要配置 2 鉤每個伺服器，請執行：

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

變更就會立即生效、 暫停，然後繼續您的 CSV 磁碟區，或伺服器之間移動。 例如，使用此 PowerShell 片段將每個 CSV 移到另一個伺服器節點，並再次備份：

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## 請參閱

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
