---
title: 效能歷程記錄的儲存空間直接存取
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: 儲存空間直接存取
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870859"
---
# <a name="performance-history-for-storage-spaces-direct"></a>效能歷程記錄的儲存空間直接存取

> 適用於：Windows Server 2019

效能歷程記錄是新的功能，可讓[儲存空間直接存取](storage-spaces-direct-overview.md)跨主應用程式伺服器、 磁碟機、 磁碟區、 虛擬機器，以及更多歷程記錄的計算、 記憶體、 網路和儲存體度量 」 的系統管理員輕鬆存取。 效能歷程記錄會自動收集並儲存在叢集上達一年。

   > [!IMPORTANT]
   > 這項功能的新 Windows Server 2019。 您不可以使用 Windows Server 2016 中。

## <a name="get-started"></a>立即開始

效能歷程記錄會收集與儲存空間直接存取在 Windows Server 2019 的預設值。 您不必安裝、 設定或啟動的任何項目。 不需要網際網路連線、 並非必要，System Center 和外部資料庫則不需要。

若要以圖形方式查看您叢集的效能記錄，請使用[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![在 Windows Admin Center 中的效能歷程記錄](media/performance-history/perf-history-in-wac.png)

若要查詢，並以程式設計方式處理它，使用 新`Get-ClusterPerf`cmdlet。 請參閱[在 PowerShell 中的使用方式](#usage-in-powershell)。

## <a name="whats-collected"></a>收集項目的

效能歷程記錄會收集 7 類型的物件：

![類型的物件](media/performance-history/types-of-object.png)

每個物件類型有許多系列： 例如，`ClusterNode.Cpu.Usage`會收集每個伺服器。

收集項目的每個物件類型，以及如何解譯這些的詳細資訊，請參閱下列子主題：

| 物件             | 系列                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| 磁碟機             | [收集項目的磁碟機](performance-history-for-drives.md)                     |
| 網路介面卡   | [什麼被收集網路介面卡](performance-history-for-network-adapters.md) |
| 伺服器            | [什麼被收集的伺服器](performance-history-for-servers.md)                   |
| 虛擬硬碟 | [什麼被收集的虛擬硬碟](performance-history-for-vhds.md)           |
| 虛擬機器   | [什麼被收集的虛擬機器](performance-history-for-vms.md)              |
| 磁碟區            | [收集項目的磁碟區](performance-history-for-volumes.md)                   |
| 叢集           | [收集項目的叢集](performance-history-for-clusters.md)                 |

許多系列會彙總至其父代的對等物件:，例如`NetAdapter.Bandwidth.Inbound`是分別針對每個網路介面卡收集和彙總成整體的伺服器; 同樣地`ClusterNode.Cpu.Usage`整體叢集使用; 彙總，依此類推。

## <a name="timeframes"></a>時間範圍

效能歷程記錄會儲存最多一年的資料，以減少資料粒度。 最新的小時度量可供使用每隔 10 秒。 之後，它們會以聰明的方式合併 （藉由平均或加總，適當地） 成較不精細的系列跨越更多的時間。 最新的一天，度量可每隔五分鐘;最新的當週，每隔十五分鐘;等等。

在 Windows Admin Center，您可以選取在右上方圖表上方的時間範圍。

![在 Windows Admin Center 中的時間範圍](media/performance-history/timeframes-in-honolulu.png)

在 PowerShell 中，使用`-TimeFrame`參數。

以下是可用的時間範圍：

| 時間範圍   | 度量的頻率 | 保留 |
|-------------|-----------------------|--------------|
| `LastHour`  | 每隔 10 秒         | 1 小時       |
| `LastDay`   | 每隔 5 分鐘       | 25 個小時     |
| `LastWeek`  | 每隔 15 分鐘      | 8 天       |
| `LastMonth` | 每 1 小時          | 35 天前      |
| `LastYear`  | 每隔 1 天           | 400 天     |

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

使用`Get-ClusterPerformanceHistory`cmdlet，在 PowerShell 中的查詢和處理序效能歷程記錄。

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > 使用**Get ClusterPerf**儲存某些按鍵輸入的別名。

### <a name="example"></a>範例

取得虛擬機器的 CPU 使用量*MyVM*四個：

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

如需更進階的範例，請參閱已發行[範例指令碼](performance-history-scripting.md)提供起始程式碼，以找出尖峰值、 計算平均值、 繪製趨勢線、 執行極端值偵測和更多功能。

### <a name="specify-the-object"></a>指定的物件

您可以指定想要在管線的物件。 這適用於 7 種物件：

| 從管線物件 | 範例     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

如果您未指定，則會傳回整體叢集的效能歷程記錄。

### <a name="specify-the-series"></a>指定此系列

您可以使用這些參數來指定所需的序列：


| 參數                 | 範例                       | 清單                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [收集項目的磁碟機](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [什麼被收集網路介面卡](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [什麼被收集的伺服器](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [什麼被收集的虛擬硬碟](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [什麼被收集的虛擬機器](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [收集項目的磁碟區](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [收集項目的叢集](performance-history-for-clusters.md)                 |


   > [!TIP]
   > 您可以使用 tab 鍵自動完成來探索可用的序列。

如果您未指定，則會傳回指定的物件可使用每個數列。

### <a name="specify-the-timeframe"></a>指定的時間範圍

您可以指定您想要使用的歷程記錄的時間範圍`-TimeFrame`參數。

   > [!TIP]
   > 您可以使用 tab 鍵自動完成來探索可用的時間範圍。

如果您未指定，`MostRecent`度量會傳回。

## <a name="how-it-works"></a>運作方式

### <a name="performance-history-storage"></a>效能歷程記錄儲存體

短時間內儲存空間直接存取啟用之後，大約 10 GB 的磁碟區命名為`ClusterPerformanceHistory`建立，而且那里佈建的可延伸儲存引擎 (也稱為 Microsoft JET) 執行個體。 此輕量級資料庫存放區效能歷程記錄，而不需要任何系統管理員介入或管理。

![效能歷程記錄儲存體的磁碟區](media/performance-history/perf-history-volume.png)

磁碟區是由儲存空間，會使用簡單的雙向鏡像或三向鏡像復原時，叢集中的節點數目而定。 修復磁碟機或伺服器故障，就像任何其他磁碟區中儲存空間直接存取之後。

磁碟區會使用 ReFS，但不是叢集共用磁碟區 (CSV)，所以才會出現在 叢集群組擁有者節點上。 除了自動建立，沒有特別關於此磁碟區： 您可以看到它，瀏覽它、 調整其大小，或刪除它 （不建議）。 如果發生錯誤，請參閱[疑難排解](#troubleshooting)。 

### <a name="object-discovery-and-data-collection"></a>物件探索和資料集合

效能歷程記錄會自動探索相關的物件，例如虛擬機器，叢集中的任何位置，並開始串流處理及其效能計數器。 計數器會彙總、 同步處理，以及插入至資料庫。 串流處理會持續執行，而且已最佳化的最基本的系統的影響。

集合由健全狀況服務，也就是高可用性： 如果執行所在的節點故障時，它會繼續時間稍後在另一個叢集中的節點。 效能歷程記錄簡單地說，可能會失敗，但它會自動繼續。 您可以看到健全狀況服務和其擁有者節點執行`Get-ClusterResource Health`在 PowerShell 中。

### <a name="handling-measurement-gaps"></a>處理度量的間距

當度量會合併成較不精細的系列跨越更多的時間，如中所述[時間表](#Timeframes)，遺漏資料的期間會排除。 比方說，如果伺服器已關閉 30 分鐘，然後執行 50%的 CPU 在接下來的 30 分鐘內，`ClusterNode.Cpu.Usage`平均時數會正確地記錄為 50%（不是 25%)。

### <a name="extensibility-and-customization"></a>擴充和自訂功能

效能歷程記錄是指令碼友善。 使用 PowerShell 來提取任何可用的歷程記錄，直接從建置自動化報告或警示，匯出以利妥善保存的歷程記錄資料庫，請回復您自己的視覺效果，依此類推。請參閱已發行[範例指令碼](performance-history-scripting.md)很有幫助的起始程式碼。

您不可以收集其他物件、 時間範圍內，或系列的歷程記錄。

測量頻率和保留期間不會是目前無法設定項目。

## <a name="start-or-stop-performance-history"></a>啟動或停止效能歷程記錄

### <a name="how-do-i-enable-this-feature"></a>如何啟用這項功能？

除非您`Stop-ClusterPerformanceHistory`，預設會啟用效能記錄。

若要重新啟用它，請以系統管理員身分執行這個 PowerShell cmdlet:

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>如何停用這項功能？

若要停止收集效能歷程記錄，請以系統管理員身分執行這個 PowerShell cmdlet:

```PowerShell
Stop-ClusterPerformanceHistory
```

若要刪除現有的度量，請使用`-DeleteHistory`旗標：

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > 初始部署期間，您可以防止效能歷程記錄設定啟動`-CollectPerformanceHistory`的參數`Enable-ClusterStorageSpacesDirect`至`$False`。

## <a name="troubleshooting"></a>疑難排解

### <a name="the-cmdlet-doesnt-work"></a>此 cmdlet 無法運作

之類的錯誤訊息 「*' 取得 ClusterPerf' 詞彙無法辨識為 cmdlet 名稱*」 表示此功能不是沒有或未安裝。 請確認您有 17692 或更新版本的 Windows Server Insider Preview 組建，您已安裝容錯移轉叢集，以及您執行儲存空間直接存取。

   > [!NOTE]
   > 這項功能不提供 Windows Server 2016 或更早版本。

### <a name="no-data-available"></a>沒有可用的資料 

如果有圖表可顯示 」*沒有可用的資料*"，如圖所示，以下是如何進行疑難排解：

![沒有可用的資料](media/performance-history/no-data-available.png)

1. 如果新加入或建立的物件，等候它才能探索到的 （最多 15 分鐘）。

2. 重新整理頁面，或等到下一步 的背景重新整理 （最多 30 秒為單位）。

3. 效能歷程記錄 – 比方說，不叢集的虛擬機器而不使用叢集共用磁碟區 (CSV) 檔案系統磁碟區不包含某些特殊的物件。 檢查子主題的物件類型，例如[磁碟區的效能歷程](performance-history-for-volumes.md)，fine 列印。

4. 如果問題持續發生，請開啟 PowerShell，以系統管理員身分執行`Get-ClusterPerf`cmdlet。 此 cmdlet 可讓您包含移難排解邏輯，以識別常見的問題，例如，如果 ClusterPerformanceHistory 磁碟區遺失，並提供補救指示。

5. 如果上一個步驟中的命令會傳回任何項目的，您可以嘗試重新啟動健全狀況服務 （以收集效能歷程記錄），藉由執行`Stop-ClusterResource Health ; Start-ClusterResource Health`在 PowerShell 中。

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
