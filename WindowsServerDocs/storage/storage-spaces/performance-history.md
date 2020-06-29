---
title: 儲存空間直接存取的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: 0c8adf5f5586bd9f86ed3c4cd42b6172ff3f91e7
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474695"
---
# <a name="performance-history-for-storage-spaces-direct"></a>儲存空間直接存取的效能歷程記錄

> 適用於：Windows Server 2019

效能歷程記錄是一項新功能，可讓[儲存空間直接存取](storage-spaces-direct-overview.md)系統管理員能夠輕鬆存取跨主機伺服器、磁片磁碟機、磁片區、虛擬機器等的歷程記錄計算、記憶體、網路和存放裝置測量。 系統會自動收集效能歷程記錄，並將其儲存在叢集中，最多一年。

   > [!IMPORTANT]
   > 這是 Windows Server 2019 中的新功能。 它在 Windows Server 2016 中無法使用。

## <a name="get-started"></a>開始使用

預設會使用 Windows Server 2019 中的儲存空間直接存取來收集效能歷程記錄。 您不需要安裝、設定或啟動任何專案。 不需要網際網路連線，系統中心不是必要的，而且不需要外部資料庫。

若要以圖形方式查看叢集的效能歷程記錄，請使用[Windows 系統管理中心](../../manage/windows-admin-center/understand/windows-admin-center.md)：

![Windows 系統管理中心的效能歷程記錄](media/performance-history/perf-history-in-wac.png)

若要以程式設計方式查詢和處理它，請使用新的 `Get-ClusterPerf` Cmdlet。 請參閱[PowerShell 中的使用方式](#usage-in-powershell)。

## <a name="whats-collected"></a>收集的內容

會針對7種物件類型收集效能歷程記錄：

![物件類型](media/performance-history/types-of-object.png)

每個物件類型都有許多數列：例如， `ClusterNode.Cpu.Usage` 會針對每個伺服器收集。

如需針對每個物件類型所收集的內容，以及如何解讀這些專案的詳細資訊，請參閱下列子主題：

| Object             | 數列                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| 磁碟機             | [磁片磁碟機的收集內容](performance-history-for-drives.md)                     |
| 網路介面卡   | [為網路介面卡收集的內容](performance-history-for-network-adapters.md) |
| 伺服器            | [為伺服器收集的內容](performance-history-for-servers.md)                   |
| 虛擬硬碟 | [為虛擬硬碟收集的內容](performance-history-for-vhds.md)           |
| 虛擬機器   | [為虛擬機器收集的內容](performance-history-for-vms.md)              |
| 磁碟區            | [磁片區的收集內容](performance-history-for-volumes.md)                   |
| 叢集           | [為叢集收集的內容](performance-history-for-clusters.md)                 |

許多數列會在對等物件之間匯總至其父系：例如， `NetAdapter.Bandwidth.Inbound` 會分別針對每個網路介面卡收集並匯總至整體伺服器; 同樣 `ClusterNode.Cpu.Usage` 會匯總至整體叢集; 依此類推。

## <a name="timeframes"></a>段

效能歷程記錄最多儲存一年，而且資料細微性會降低。 在最近的一小時內，每十秒就會提供測量。 之後，它們會以智慧方式合併（依適當情況平均或總和）至較不細微的數列，而更多時間。 在最近的一天，每五分鐘可取得度量;最近一周，每十五分鐘;以此類推。

在 Windows 系統管理中心內，您可以選取圖表右上方的時間範圍。

![Windows 系統管理中心的時程表](media/performance-history/timeframes-in-honolulu.png)

在 PowerShell 中，使用 `-TimeFrame` 參數。

以下是可用的時間範圍：

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
   > 使用**ClusterPerf**別名來儲存一些按鍵。

### <a name="example"></a>範例

取得過去一小時內虛擬機器*MyVM*的 CPU 使用量：

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

如需更多的範例，請參閱已發佈的[範例腳本](performance-history-scripting.md)，其提供起始程式碼來尋找尖峰值、計算平均值、繪製趨勢線、執行極端值偵測等等。

### <a name="specify-the-object"></a>指定物件

您可以指定管線所需的物件。 這適用于7種類型的物件：

| 管線中的物件 | 範例     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

如果您未指定，則會傳回整體叢集的效能歷程記錄。

### <a name="specify-the-series"></a>指定數列

您可以使用下列參數來指定您想要的數列：


| 參數                 | 範例                       | 清單                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [磁片磁碟機的收集內容](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [為網路介面卡收集的內容](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [為伺服器收集的內容](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [為虛擬硬碟收集的內容](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [為虛擬機器收集的內容](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [磁片區的收集內容](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [為叢集收集的內容](performance-history-for-clusters.md)                 |


   > [!TIP]
   > 使用 tab 鍵自動完成來探索可用的系列。

如果您未指定，則會傳回指定物件可用的每個數列。

### <a name="specify-the-timeframe"></a>指定時間範圍

您可以使用參數來指定您想要的歷程記錄時間範圍 `-TimeFrame` 。

   > [!TIP]
   > 使用 tab 鍵自動完成來探索可用的時間範圍。

如果您未指定，則 `MostRecent` 會傳回測量。

## <a name="how-it-works"></a>運作方式

### <a name="performance-history-storage"></a>效能歷程記錄儲存體

一旦啟用儲存空間直接存取之後，就會建立大約 10 GB 的磁片區， `ClusterPerformanceHistory` 並在該處布建可延伸儲存引擎（也稱為 MICROSOFT JET）的實例。 這個輕量資料庫會儲存效能歷程記錄，而不需要任何系統管理員介入或管理。

![效能記錄儲存的磁片區](media/performance-history/perf-history-volume.png)

磁片區是由儲存空間支援，而且會根據叢集中的節點數目，使用簡單、雙向鏡像或三向鏡像復原功能。 它會在磁片磁碟機或伺服器失敗之後修復，就像儲存空間直接存取中的任何其他磁片區一樣。

磁片區使用 ReFS，但不叢集共用磁碟區（CSV），因此它只會出現在叢集群組擁有者節點上。 除了自動建立以外，此磁片區沒有任何特別的內容：您可以查看、流覽、調整其大小，或將它刪除（不建議）。 如果發生錯誤，請參閱[疑難排解](#troubleshooting)。

### <a name="object-discovery-and-data-collection"></a>物件探索和資料收集

效能歷程會在叢集中的任何位置自動探索相關物件（例如虛擬機器），並開始串流處理其效能計數器。 計數器會進行匯總、同步處理，並插入到資料庫中。 串流會持續執行，並已針對最低系統影響進行優化。

集合是由健全狀況服務（高度可用）所處理：如果執行的節點停止運作，它稍後會在叢集中的另一個節點上恢復時間。 效能歷程記錄可能很快就會繼續，但會自動復原。 您可以在 PowerShell 中執行，查看健全狀況服務和其擁有者節點 `Get-ClusterResource Health` 。

### <a name="handling-measurement-gaps"></a>處理測量間隙

當量值合併到較不精確的數列時（如時間[範圍中所述），會](#timeframes)排除遺失資料的期間。 例如，如果伺服器已關閉30分鐘，然後在接下來的30分鐘內于 50% CPU 執行， `ClusterNode.Cpu.Usage` 則該小時的平均將會正確地記錄為50% （不是25%）。

### <a name="extensibility-and-customization"></a>擴充性和自訂

效能歷程記錄是腳本易懂的。 使用 PowerShell 直接從資料庫提取任何可用的歷程記錄，以建立自動化的報告或警示、用於妥善保管的匯出歷程記錄、製作您自己的視覺效果等等。如需實用的入門程式碼，請參閱已發佈的[範例腳本](performance-history-scripting.md)。

不可能收集其他物件、時間範圍或數列的記錄。

目前無法設定測量頻率和保留期限。

## <a name="start-or-stop-performance-history"></a>啟動或停止效能歷程記錄

### <a name="how-do-i-enable-this-feature"></a>如何? 啟用此功能嗎？

除非您這樣 `Stop-ClusterPerformanceHistory` 做，否則預設會啟用效能歷程記錄。

若要重新啟用它，請以系統管理員身分執行此 PowerShell Cmdlet：

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>如何? 停用此功能嗎？

若要停止收集效能歷程記錄，請以系統管理員身分執行此 PowerShell Cmdlet：

```PowerShell
Stop-ClusterPerformanceHistory
```

若要刪除現有的度量，請使用 `-DeleteHistory` 旗標：

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > 在初始部署期間，您可以將的參數設定為，以避免啟動效能歷程記錄 `-CollectPerformanceHistory` `Enable-ClusterStorageSpacesDirect` `$False` 。

## <a name="troubleshooting"></a>疑難排解

### <a name="the-cmdlet-doesnt-work"></a>Cmdlet 無法使用

「ClusterPerf」一*詞「無法辨識為 Cmdlet 的名稱*」之類的錯誤訊息表示此功能無法使用或已安裝。 確認您有 Windows Server Insider Preview 組建17692或更新版本，且您已安裝容錯移轉叢集，而且您正在執行儲存空間直接存取。

   > [!NOTE]
   > Windows Server 2016 或更早版本無法使用這項功能。

### <a name="no-data-available"></a>沒有可用資料

如圖所示，如果圖表顯示「*沒有可用的資料*」，以下是疑難排解的方法：

![沒有可用資料](media/performance-history/no-data-available.png)

1. 如果是新加入或建立的物件，請等候它被探索（最多15分鐘）。

2. 重新整理頁面，或等候下一個背景重新整理（最多30秒）。

3. 某些特殊物件會從效能歷程中排除（例如，未叢集化的虛擬機器），以及未使用叢集共用磁碟區（CSV）檔案系統的磁片區。 檢查物件類型的子主題（例如磁片區的[效能歷程記錄](performance-history-for-volumes.md)），以進行微調列印。

4. 如果問題持續發生，請以系統管理員身分開啟 PowerShell 並執行 `Get-ClusterPerf` Cmdlet。 此 Cmdlet 包含可識別常見問題的疑難排解邏輯，例如，如果 ClusterPerformanceHistory 磁片區遺失，則會提供補救指示。

5. 如果上一個步驟中的命令未傳回任何內容，您可以嘗試重新開機健全狀況服務（這會收集效能歷程記錄），方法是 `Stop-ClusterResource Health ; Start-ClusterResource Health` 在 PowerShell 中執行。

## <a name="additional-references"></a>其他參考

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
