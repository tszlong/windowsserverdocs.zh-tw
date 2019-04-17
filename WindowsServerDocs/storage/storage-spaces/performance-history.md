---
title: 儲存空間直接存取的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239255"
---
# 儲存空間直接存取的效能歷程記錄

> 適用於： Windows Server 2019

效能歷程記錄是新的功能，可讓[儲存空間直接存取](storage-spaces-direct-overview.md)的系統管理員輕鬆存取歷史運算、 記憶體、 網路及儲存度量跨主機伺服器、 磁碟機、 磁碟區、 虛擬機器，以及更多。 效能歷程記錄是會自動收集並儲存在叢集上最多一年。

   > [!IMPORTANT]
   > 這項功能是 Windows Server 2019 中的新功能。 不在 Windows Server 2016 中提供。

## 入門

根據預設，使用儲存空間直接存取 Windows Server 2019 中，會收集效能歷程記錄。 您不需要安裝、 設定，或啟動任何項目。 不需要網際網路連線的、 System Center 並非必要，和外部資料庫並非必要。

若要查看您的叢集效能歷程記錄而言，使用[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Windows Admin Center 中的效能歷程記錄](media/performance-history/perf-history-in-wac.png)

若要查詢，並以程式設計方式進行處理，請使用新的`Get-ClusterPerf`cmdlet。 請參閱[在 PowerShell 中的使用方式](#usage-in-powershell)。

## 什麼被收集

效能歷程記錄會收集 7 類型的物件：

![類型的物件](media/performance-history/types-of-object.png)

每個物件類型有許多系列： 例如，`ClusterNode.Cpu.Usage`會針對每個伺服器收集。

如需針對每個物件類型所收集的項目，以及如何解譯他們的詳細資訊，請參閱這些子主題：

| 物件             | 系列                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| 磁碟機             | [什麼是收集到的磁碟機](performance-history-for-drives.md)                     |
| 網路介面卡   | [什麼被收集的網路介面卡](performance-history-for-network-adapters.md) |
| 伺服器            | [什麼被收集的伺服器](performance-history-for-servers.md)                   |
| 虛擬硬碟 | [什麼被收集的虛擬硬碟](performance-history-for-vhds.md)           |
| 虛擬機器   | [什麼被收集的虛擬機器](performance-history-for-vms.md)              |
| 磁碟區            | [什麼被收集的磁碟區](performance-history-for-volumes.md)                   |
| 叢集           | [什麼被收集的叢集](performance-history-for-clusters.md)                 |

許多系列彙總至其父系的對等物件： 例如，`NetAdapter.Bandwidth.Inbound`是針對每個網路介面卡分別收集和彙總到整體的伺服器;同樣地`ClusterNode.Cpu.Usage`彙總，到整體的叢集。等等。

## 時間範圍

最多一年，會儲存效能歷程記錄，以減少資料粒度。 最新的小時，測量值是可用每隔 10 秒。 此後，它們會明智地合併 （藉由將平均，或加總，做為適用） 成較不細微系列跨越更多的時間。 最近的一天的測量值是可用每 5 分鐘。最新的週，每隔 15 分鐘。等等。

在 Windows Admin Center，您可以選取右上方上述圖表中的時間範圍。

![Windows Admin Center 中的時間範圍](media/performance-history/timeframes-in-honolulu.png)

在 PowerShell 中，使用`-TimeFrame`參數。

以下是可用的時間範圍：

| 時間範圍   | 度量頻率 | 保留的 |
|-------------|-----------------------|--------------|
| `LastHour`  | 每隔 10 秒         | 1 小時       |
| `LastDay`   | 每 5 分鐘       | 25 個小時     |
| `LastWeek`  | 每隔 15 分鐘      | 8 天       |
| `LastMonth` | 每個 1 小時          | 過了 35 天      |
| `LastYear`  | 每月 1 日           | 400 天     |

## 在 PowerShell 中的使用方式

使用`Get-ClusterPerformanceHistory`cmdlet 來查詢和處理程序的效能歷程記錄在 PowerShell 中。

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > 使用**Get ClusterPerf**別名以節省一些按鍵動作。

### 範例

取得一個小時的虛擬機器*MyVM* CPU 使用量：

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

如需更多進階的範例，請參閱提供了尖峰值、 計算平均值、 繪製趨勢行，請執行 outlier，偵測，以及更多的起始程式碼已發佈之的[範例指令碼](performance-history-scripting.md)。

### 指定的物件

您可以指定您想在管線的物件。 這適用於 7 的物件類型：

| 從管線的物件 | 範例     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

如果您沒有指定，則會傳回適用於整個叢集的效能歷程記錄。

### 指定一系列

您可以使用這些參數來指定您想要的一系列：


| 參數                 | 範例                       | 清單                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [什麼是收集到的磁碟機](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [什麼被收集的網路介面卡](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [什麼被收集的伺服器](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [什麼被收集的虛擬硬碟](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [什麼被收集的虛擬機器](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [什麼被收集的磁碟區](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [什麼被收集的叢集](performance-history-for-clusters.md)                 |


   > [!TIP]
   > 使用 tab 鍵自動完成來探索可用的一系列。

如果您沒有指定，則會傳回每個系列適用於指定的物件。

### 指定時間範圍

您可以指定您想要使用的歷程記錄的時間範圍`-TimeFrame`參數。

   > [!TIP]
   > 使用 tab 鍵自動完成來探索可用的時間範圍。

如果您沒有指定，`MostRecent`會傳回度量單位。

## 運作方式

### 效能歷程記錄存放裝置

以約 10 GB 的磁碟區不久儲存空間直接存取啟用之後，名為`ClusterPerformanceHistory`會建立那里佈建的可延伸的儲存體引擎 (也稱為 Microsoft JET) 執行個體。 這個輕量資料庫儲存效能歷程記錄，不需要任何系統管理員身分參與或管理。

![做為效能歷程記錄儲存空間的的磁碟區](media/performance-history/perf-history-volume.png)

磁碟區都由儲存空間支援，並使用簡單、 雙向鏡像或三向鏡像復原類型，根據叢集中的節點的數量。 它被修復磁碟機或伺服器故障，就像任何其他磁碟區中儲存空間直接存取之後。

使用 ReFS 磁碟區，但不是叢集共用磁碟區 (CSV)，所以只會出現在叢集群組擁有者節點上。 除了自動建立，並沒有特別有關這個磁碟區： 您可以看見它、 瀏覽、 調整大小，或刪除它 （不建議）。 如果發生錯誤，請參閱[疑難排解](#troubleshooting)。 

### 物件探索和資料收集

效能歷程記錄自動探索相關的物件，例如叢集中的任何位置的虛擬機器，並開始串流及其效能計數器。 計數器會彙總、 同步處理，以及插入資料庫。 串流處理會持續執行，並最適合最基本的系統的影響。

集合健全狀況服務，也就是高度可用來處理： 如果它執行所在的節點會向下，它將會繼續時刻叢集中的稍後的另一個節點。 簡言之，lapse 效能歷程記錄，可能會但它會自動繼續執行。 您可以看到健全狀況服務和其擁有者 」 節點執行`Get-ClusterResource Health`在 PowerShell 中。

### 處理測量間隔

當度量合併成較不細微橫跨更多的時間，[時間範圍](#Timeframes)中所述的系列時，會被排除期間的資料遺失。 例如，如果伺服器是向下 30 分鐘的時間，然後執行 50 %cpu 接下來 30 分鐘的時間，`ClusterNode.Cpu.Usage`平均的該小時會正確地記錄為 50%（不 25%)。

### 擴充性和自訂項目

效能歷程記錄是指令碼方便。 使用 PowerShell 來提取任何可用的歷程記錄，直接從資料庫來建置自動化報告或警示，匯出妥善保管的歷程記錄，讓您自己的視覺效果等等。請參閱適用於很有幫助的入門程式碼已發佈的[範例指令碼](performance-history-scripting.md)。

您不可能收集其他物件、 時間範圍或一系列的歷程記錄。

度量頻率和保留期間不是目前可設定的。

## 開始或停止效能歷程記錄

### 如何啟用這項功能？

除非您`Stop-ClusterPerformanceHistory`，依預設會啟用效能歷程記錄。

若要重新啟用它，請以系統管理員身分執行此 PowerShell cmdlet:

```PowerShell
Start-ClusterPerformanceHistory
```

### 如何停用這項功能？

若要停止收集效能歷程記錄，請以系統管理員身分執行此 PowerShell cmdlet:

```PowerShell
Stop-ClusterPerformanceHistory
```

若要刪除現有的度量，使用`-DeleteHistory`旗標：

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > 初始部署期間，您可以防止效能歷程記錄啟動藉由設定`-CollectPerformanceHistory`參數`Enable-ClusterStorageSpacesDirect`到`$False`。

## 疑難排解

### 此 cmdlett 無法運作

錯誤訊息像 」*一詞 ' Get-ClusterPerf' 無法辨識名稱*」 表示該功能不是 cmdlet 的可用，或已安裝。 確認您有 Windows Server Insider Preview 組建 17692 或更新版本，您已安裝容錯移轉叢集，且您正在執行儲存空間直接存取。

   > [!NOTE]
   > 此功能並非 Windows Server 2016 上使用或更舊版本。

### 沒有可用的資料 

如果因為示，圖表會顯示 「*沒有可用的資料*」，以下是如何疑難排解：

![沒有可用的資料](media/performance-history/no-data-available.png)

1. 如果新新增或建立物件，才可加以等候發現 （最多 15 分鐘）。

2. 重新整理頁面，或等待下一個背景重新整理 （最多 30 秒）。

3. 某些特殊的物件會排除效能歷程記錄 – 例如，虛擬機器，不叢集，而不要使用叢集共用磁碟區 (CSV) 檔案系統的磁碟區。 檢查的物件類型，像[磁碟區的效能歷程記錄](performance-history-for-volumes.md)、 補充的子主題。

4. 如果問題持續發生，開啟 PowerShell 做為系統管理員，然後執行`Get-ClusterPerf`cmdlet。 此 cmdlett 內含疑難排解邏輯來識別常見的問題，例如，如果 ClusterPerformanceHistory 磁碟區遺失，並提供補救措施的指示。

5. 如果先前步驟中的命令會傳回任何項目，您可以嘗試重新啟動健全狀況服務 （這會收集效能歷程記錄），執行`Stop-ClusterResource Health ; Start-ClusterResource Health`在 PowerShell 中。

## 請參閱

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
