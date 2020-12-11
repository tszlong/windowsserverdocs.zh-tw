---
description: 深入瞭解：儲存空間直接存取的效能歷程記錄
title: 儲存空間直接存取的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: b90f010d45dc9e9013c2bc661232fb444247b661
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048916"
---
# <a name="performance-history-for-storage-spaces-direct"></a>儲存空間直接存取的效能歷程記錄

> 適用於：Windows Server 2019

效能歷程是一項新功能，可讓 [儲存空間直接存取](storage-spaces-direct-overview.md) 系統管理員在主機伺服器、磁片磁碟機、磁片區、虛擬機器等之間，輕鬆存取歷程記錄計算、記憶體、網路和存放裝置度量。 系統會自動收集效能歷程記錄，並將其儲存在叢集中，最多一年。

   > [!IMPORTANT]
   > 這項功能是 Windows Server 2019 中的新功能。 它無法在 Windows Server 2016 中使用。

## <a name="get-started"></a>開始使用

預設會收集 Windows Server 2019 中的儲存空間直接存取的效能歷程記錄。 您不需要安裝、設定或啟動任何動作。 不需要網際網路連線、不需要 System Center，也不需要外部資料庫。

若要以圖形方式查看叢集的效能歷程記錄，請使用 [Windows Admin Center](../../manage/windows-admin-center/overview.md)：

![Windows Admin Center 中的效能歷程記錄](media/performance-history/perf-history-in-wac.png)

若要以程式設計方式查詢並處理它，請使用新的 `Get-ClusterPerf` Cmdlet。 請參閱 [PowerShell 中的使用方式](#usage-in-powershell)。

## <a name="whats-collected"></a>收集的內容

針對7種類型的物件收集效能歷程記錄：

![物件類型](media/performance-history/types-of-object.png)

每個物件類型都有許多數列：例如，為 `ClusterNode.Cpu.Usage` 每個伺服器收集。

如需針對每個物件類型所收集之專案的詳細資訊，以及如何解讀它們的詳細資訊，請參閱下列子主題：

| 物件             | 數列                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| 磁碟機             | [磁片磁碟機的收集內容](performance-history-for-drives.md)                     |
| 網路介面卡   | [針對網路介面卡所收集的內容](performance-history-for-network-adapters.md) |
| 伺服器            | [針對伺服器所收集的內容](performance-history-for-servers.md)                   |
| 虛擬硬碟 | [針對虛擬硬碟所收集的內容](performance-history-for-vhds.md)           |
| 虛擬機器   | [針對虛擬機器收集的內容](performance-history-for-vms.md)              |
| 磁碟區            | [為磁片區收集的內容](performance-history-for-volumes.md)                   |
| 叢集           | [針對叢集所收集的內容](performance-history-for-clusters.md)                 |

許多數列會在對等物件之間匯總至其父系：例如， `NetAdapter.Bandwidth.Inbound` 針對每個網路介面卡分開收集並匯總至整體伺服器; 同樣地， `ClusterNode.Cpu.Usage` 也會匯總至整體叢集; 依此類推。

## <a name="timeframes"></a>框架

效能歷程記錄最多儲存一年，且資料細微性較低。 在最近的一小時內，測量可每隔10秒使用一次。 之後，系統會將平均或加總 (，以智慧的方式合併，以適當的) 到更多時間的較細微數列。 在最近一天，測量值每隔五分鐘可供使用;最近一周，每十五分鐘;依此類推。

在 Windows Admin Center 中，您可以在圖表上方的右上方選取時間範圍。

![Windows Admin Center 中的時程表](media/performance-history/timeframes-in-honolulu.png)

在 PowerShell 中，使用 `-TimeFrame` 參數。

以下是可用的時程表：

| 時間範圍   | 測量頻率 | 保留給 |
|-------------|-----------------------|--------------|
| `LastHour`  | 每10秒         | 1 小時       |
| `LastDay`   | 每 5 分鐘       | 25小時     |
| `LastWeek`  | 每 15 分鐘      | 8 天       |
| `LastMonth` | 每1小時          | 35 天      |
| `LastYear`  | 每隔1天           | 400天     |

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用 `Get-ClusterPerformanceHistory` Cmdlet 來查詢和處理 PowerShell 中的效能歷程記錄。

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > 使用 **ClusterPerf** 別名來儲存某些按鍵。

### <a name="example"></a>範例

取得過去一小時的虛擬機器 *MYVM* CPU 使用量：

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

如需更高階的範例，請參閱已發佈的 [範例腳本](performance-history-scripting.md) ，以提供起始程式碼來尋找尖峰值、計算平均值、繪製趨勢線、執行極端值偵測等等。

### <a name="specify-the-object"></a>指定物件

您可以指定管線所要的物件。 這適用于7種類型的物件：

| 管線中的物件 | 範例     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

如果您未指定，則會傳回整個叢集的效能歷程記錄。

### <a name="specify-the-series"></a>指定數列

您可以使用下列參數指定您想要的數列：


| 參數                 | 範例                       | 清單                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [磁片磁碟機的收集內容](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [針對網路介面卡所收集的內容](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [針對伺服器所收集的內容](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [針對虛擬硬碟所收集的內容](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [針對虛擬機器收集的內容](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [為磁片區收集的內容](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [針對叢集所收集的內容](performance-history-for-clusters.md)                 |


   > [!TIP]
   > 使用 tab 鍵自動完成功能來探索可用的數列。

如果您未指定，則會傳回指定之物件的每個可用數列。

### <a name="specify-the-timeframe"></a>指定時間範圍

您可以使用參數指定您想要的歷程記錄時間範圍 `-TimeFrame` 。

   > [!TIP]
   > 使用 tab 鍵自動完成功能來探索可用的時程表。

如果您未指定，則 `MostRecent` 會傳回度量。

## <a name="how-it-works"></a>運作方式

### <a name="performance-history-storage"></a>效能歷程記錄儲存體

啟用儲存空間直接存取之後，會建立大約 10 GB 的磁片 `ClusterPerformanceHistory` 區，並在該處布建可延伸儲存引擎的實例 (也稱為 MICROSOFT JET) 。 這個輕量資料庫會儲存效能歷程記錄，而不需要任何系統管理員介入或管理。

![效能記錄儲存體的磁片區](media/performance-history/perf-history-volume.png)

磁片區是由儲存空間所支援，並且使用簡單、雙向鏡像或三向鏡像復原，視叢集中的節點數目而定。 它會在磁片磁碟機或伺服器故障之後修復，就像儲存空間直接存取中的任何其他磁片區一樣。

磁片區會使用 ReFS，但不叢集共用磁碟區 (CSV) ，所以它只會出現在叢集群組擁有者節點上。 除了自動建立外，這個磁片區沒有任何特殊的專案：您可以看到、流覽、調整其大小，或將它刪除 (不建議) 。 如果發生錯誤，請參閱 [疑難排解](#troubleshooting)。

### <a name="object-discovery-and-data-collection"></a>物件探索和資料收集

效能歷程記錄會自動在叢集中的任何位置探索相關的物件（例如虛擬機器），並開始串流處理其效能計數器。 計數器會匯總、同步處理，然後插入資料庫中。 串流會持續執行，並針對最少量的系統影響進行優化。

集合是由健全狀況服務（高度可用）所處理：如果執行的節點停止運作，則會在叢集中的另一個節點之後繼續進行。 效能歷程記錄可能會短暫消失，但是會自動繼續。 您可以在 PowerShell 中執行，以查看健全狀況服務及其擁有者節點 `Get-ClusterResource Health` 。

### <a name="handling-measurement-gaps"></a>處理量測間距

當量測會合並成較不多時間的較細微序列時（如時間範圍所 [述），](#timeframes)就會排除遺失資料的期間。 例如，如果伺服器已關閉30分鐘，然後在 50% CPU 上的30分鐘內執行，則 `ClusterNode.Cpu.Usage` 該小時的平均將會正確地記錄為 50% (不是 25% ) 。

### <a name="extensibility-and-customization"></a>擴充性和自訂

效能歷程記錄易於編寫腳本。 使用 PowerShell 直接從資料庫提取任何可用的記錄，以建立自動化的報告或警示、可保護的匯出歷程記錄、變換您自己的視覺效果等等。如需實用的入門程式碼，請參閱已發佈的 [範例腳本](performance-history-scripting.md) 。

您無法收集其他物件、時間範圍或數列的歷程記錄。

度量頻率和保留期限目前無法設定。

## <a name="start-or-stop-performance-history"></a>啟動或停止效能歷程記錄

### <a name="how-do-i-enable-this-feature"></a>如何? 啟用這項功能？

除非您 `Stop-ClusterPerformanceHistory` ，否則預設會啟用效能歷程記錄。

若要重新啟用，請以系統管理員身分執行此 PowerShell Cmdlet：

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>如何? 停用此功能？

若要停止收集效能歷程記錄，請以系統管理員身分執行此 PowerShell Cmdlet：

```PowerShell
Stop-ClusterPerformanceHistory
```

若要刪除現有的度量，請使用 `-DeleteHistory` 旗標：

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > 在初始部署期間，您可以藉由將的參數設定為，以防止效能歷程記錄開始 `-CollectPerformanceHistory` `Enable-ClusterStorageSpacesDirect` `$False` 。

## <a name="troubleshooting"></a>疑難排解

### <a name="the-cmdlet-doesnt-work"></a>Cmdlet 無法運作

「ClusterPerf」一 *詞「無法辨識為 Cmdlet 的名稱*」這類錯誤訊息表示該功能無法使用或未安裝。 確認您有 Windows Server Insider Preview 組建17692或更新版本，您已安裝容錯移轉叢集，而且您正在執行儲存空間直接存取。

   > [!NOTE]
   > 這項功能無法在 Windows Server 2016 或更早版本上使用。

### <a name="no-data-available"></a>沒有可用資料

如圖所示，圖表顯示「*沒有可用的資料*」，以下是疑難排解的方法：

![沒有可用資料](media/performance-history/no-data-available.png)

1. 如果物件是新加入或建立的，請等候它被探索 (最多15分鐘的) 。

2. 重新整理頁面，或等候下一次背景重新整理 (最多30秒) 。

3. 某些特殊物件會排除在效能歷程記錄中（例如，未叢集化的虛擬機器），以及未使用叢集共用磁碟區 (CSV) 檔案系統的磁片區。 針對 [列印]，檢查物件類型（例如磁片區的 [效能歷程記錄](performance-history-for-volumes.md)）的子主題。

4. 如果問題持續發生，請以系統管理員身分開啟 PowerShell，然後執行 `Get-ClusterPerf` Cmdlet。 此 Cmdlet 包含疑難排解邏輯以識別常見問題，例如，如果 ClusterPerformanceHistory 磁片區遺失，並提供補救指示。

5. 如果上一個步驟中的命令不會傳回任何內容，您可以嘗試重新開機健全狀況服務 (，它會透過在 PowerShell 中執行來收集效能歷程記錄) `Stop-ClusterResource Health ; Start-ClusterResource Health` 。

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
