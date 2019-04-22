---
title: 指令碼與儲存空間直接存取的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: 儲存空間直接存取
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816319"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>指令碼與 PowerShell 和儲存空間直接存取的效能歷程記錄

> 適用於：Windows Server Insider Preview 組建 17692 和更新版本

在 Windows Server 2019，[儲存空間直接存取](storage-spaces-direct-overview.md)記錄和廣泛的存放區[效能歷程記錄](performance-history.md)的虛擬機器、 伺服器、 磁碟機、 磁碟區、 網路介面卡，以及更多。 效能歷程記錄是查詢和在 PowerShell 中的程序變得更加容易，因此您可以從快速移*未經處理資料*要*實際答案*這類問題：

1. 已有任何 CPU 尖峰的原因上週？
2. 任何實體磁碟發生異常的延遲？
3. 哪些 Vm 會立即耗用最多儲存體 IOPS？
4. 我的網路頻寬飽和嗎？
5. 何時將此磁碟區用盡可用空間？
6. 在過去一個月中，哪些 Vm 會使用最多的記憶體？

`Get-ClusterPerf` Cmdlet 針對指令碼所建置。 它會接受來自等 cmdlet 的輸入`Get-VM`或`Get-PhysicalDisk`管線來處理，以及您可以將其輸出輸送到公用程式 cmdlet，例如`Sort-Object`， `Where-Object`，和`Measure-Object`快速撰寫功能強大的查詢。

**本主題提供，並說明 6 回答上述問題，6 的範例指令碼。** 它們呈現的模式，您可以套用以尋找尖峰、 尋找平均值、 繪製趨勢線、 執行極端值偵測，以及更多，跨各種不同的資料和時間範圍。 它們提供免費的入門即可複製、 擴充和重複使用的程式碼。

   > [!NOTE]
   > 為求簡潔，範例指令碼會省略之類的高品質的 PowerShell 程式碼，您可能預期的錯誤處理。 它們主要供靈感和教育版而不是生產環境使用。

## <a name="sample-1-cpu-i-see-you"></a>範例 1:CPU，我見 ！

這個範例會使用`ClusterNode.Cpu.Usage`序列從`LastWeek`顯示叢集中的最大值 （「 上限標準 」），而每一部伺服器的最小和平均 CPU 使用量的時間範圍內。 它也會執行簡單的四分位數的分析，以顯示多少小時 CPU 使用量已超過 25%、 50%和過去 8 天內的 75%。

### <a name="screenshot"></a>Screenshot

在以下的螢幕擷取畫面，我們看到*Server 02*有無法解釋的高峰過去一週：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>運作方式

輸出`Get-ClusterPerf`妥善至內建的管道`Measure-Object`cmdlet，我們只需要指定`Value`屬性。 使用其`-Maximum`， `-Minimum`，並`-Average`旗標，`Measure-Object`提供我們前三個資料行幾乎免費。 若要執行的四分位數分析，我們可以透過管道傳送至`Where-Object`與計算多少值已`-Gt`（大於） 25、 50、 或 75。 最後一個步驟是使用美化`Format-Hours`和`Format-Percent`helper 函式-當然選擇性。

### <a name="script"></a>指令碼

以下是指令碼：

```
Function Format-Hours {
    Param (
        $RawValue
    )
    # Weekly timeframe has frequency 15 minutes = 4 points per hour
    [Math]::Round($RawValue/4)
}

Function Format-Percent {
    Param (
        $RawValue
    )
    [String][Math]::Round($RawValue) + " " + "%"
}

$Output = Get-ClusterNode | ForEach-Object {
    $Data = $_ | Get-ClusterPerf -ClusterNodeSeriesName "ClusterNode.Cpu.Usage" -TimeFrame "LastWeek"

    $Measure = $Data | Measure-Object -Property Value -Minimum -Maximum -Average
    $Min = $Measure.Minimum
    $Max = $Measure.Maximum
    $Avg = $Measure.Average

    [PsCustomObject]@{
        "ClusterNode"    = $_.Name
        "MinCpuObserved" = Format-Percent $Min
        "MaxCpuObserved" = Format-Percent $Max
        "AvgCpuObserved" = Format-Percent $Avg
        "HrsOver25%"     = Format-Hours ($Data | Where-Object Value -Gt 25).Length
        "HrsOver50%"     = Format-Hours ($Data | Where-Object Value -Gt 50).Length
        "HrsOver75%"     = Format-Hours ($Data | Where-Object Value -Gt 75).Length
    }
}

$Output | Sort-Object ClusterNode | Format-Table
```

## <a name="sample-2-fire-fire-latency-outlier"></a>範例 2:火災、 火災、 延遲極端值

這個範例會使用`PhysicalDisk.Latency.Average`序列從`LastHour`時間範圍來尋找統計的極端值，定義為磁碟機，以每小時的平均延遲超出 + 3σ （三個標準差） 高於母體擴展平均。

   > [!IMPORTANT]
   > 為求簡潔，此指令碼不會實作低變異數的保護措施，不會處理部分遺失的資料，不會區分模型或韌體等。請執行良好的判斷，請勿依賴此指令碼來判斷是否要更換的硬碟。 它會在此僅供教育。

### <a name="screenshot"></a>Screenshot

在以下的螢幕擷取畫面，我們可以看到有任何極端值：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>運作方式

首先，我們藉由檢查排除閒置或幾乎閒置的磁碟機`PhysicalDisk.Iops.Total`一直`-Gt 1`。 每個作用中的 HDD，我們透過管道傳送其`LastHour`時間範圍內，組成 360 的度量值 10 秒間隔，為`Measure-Object -Average`過去一小時中取得其平均的延遲。 這會設定我們的母體擴展。

我們會實作[廣泛已知公式](http://www.mathsisfun.com/data/standard-deviation.html)來尋找平均`μ`和標準差`σ`母體擴展。 針對每個作用中的 HDD，我們會比較其母體擴展平均的平均延遲，並除以標準差。 我們保留原始值，我們可以`Sort-Object`我們的結果，但使用`Format-Latency`和`Format-StandardDeviation`美化什麼我們將示範 – 當然是選擇性的 helper 函式。

如果任何磁碟機超過 + 3σ，我們`Write-Host`紅色; 否則為綠色。

### <a name="script"></a>指令碼

以下是指令碼：

```
Function Format-Latency {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("s", "ms", "μs", "ns") # Petabits, just in case!
    Do { $RawValue *= 1000 ; $i++ } While ( $RawValue -Lt 1 )
    # Return
    [String][Math]::Round($RawValue, 2) + " " + $Labels[$i]
}

Function Format-StandardDeviation {
    Param (
        $RawValue
    )
    If ($RawValue -Gt 0) {
        $Sign = "+"
    }
    Else {
        $Sign = "-"
    }
    # Return
    $Sign + [String][Math]::Round([Math]::Abs($RawValue), 2) + "σ"
}

$HDD = Get-StorageSubSystem Cluster* | Get-PhysicalDisk | Where-Object MediaType -Eq HDD

$Output = $HDD | ForEach-Object {

    $Iops = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Iops.Total" -TimeFrame "LastHour"
    $AvgIops = ($Iops | Measure-Object -Property Value -Average).Average

    If ($AvgIops -Gt 1) { # Exclude idle or nearly idle drives

        $Latency = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Latency.Average" -TimeFrame "LastHour"
        $AvgLatency = ($Latency | Measure-Object -Property Value -Average).Average

        [PsCustomObject]@{
            "FriendlyName"  = $_.FriendlyName
            "SerialNumber"  = $_.SerialNumber
            "MediaType"     = $_.MediaType
            "AvgLatencyPopulation" = $null # Set below
            "AvgLatencyThisHDD"    = Format-Latency $AvgLatency
            "RawAvgLatencyThisHDD" = $AvgLatency
            "Deviation"            = $null # Set below
            "RawDeviation"         = $null # Set below
        }
    }
}

If ($Output.Length -Ge 3) { # Minimum population requirement

    # Find mean μ and standard deviation σ
    $μ = ($Output | Measure-Object -Property RawAvgLatencyThisHDD -Average).Average
    $d = $Output | ForEach-Object { ($_.RawAvgLatencyThisHDD - $μ) * ($_.RawAvgLatencyThisHDD - $μ) }
    $σ = [Math]::Sqrt(($d | Measure-Object -Sum).Sum / $Output.Length)

    $FoundOutlier = $False

    $Output | ForEach-Object {
        $Deviation = ($_.RawAvgLatencyThisHDD - $μ) / $σ
        $_.AvgLatencyPopulation = Format-Latency $μ
        $_.Deviation = Format-StandardDeviation $Deviation
        $_.RawDeviation = $Deviation
        # If distribution is Normal, expect >99% within 3σ
        If ($Deviation -Gt 3) {
            $FoundOutlier = $True
        }
    }

    If ($FoundOutlier) {
        Write-Host -BackgroundColor Black -ForegroundColor Red "Oh no! There's an HDD significantly slower than the others."
    }
    Else {
        Write-Host -BackgroundColor Black -ForegroundColor Green "Good news! No outlier found."
    }

    $Output | Sort-Object RawDeviation -Descending | Format-Table FriendlyName, SerialNumber, MediaType, AvgLatencyPopulation, AvgLatencyThisHDD, Deviation

}
Else {
    Write-Warning "There aren't enough active drives to look for outliers right now."
}
```

## <a name="sample-3-noisy-neighbor-thats-write"></a>範例 3:吵雜的芳鄰嗎？ 這是寫入 ！

效能歷程記錄可以回答問題的相關*立即*也。 新的度量是即時提供，每隔 10 秒。 這個範例會使用`VHD.Iops.Total`系列從`MostRecent`來識別最忙碌的 （有些可能會說 「 繁忙"） 的時間範圍內耗用最多儲存體 IOPS，跨叢集中的每個主機的虛擬機器，並顯示的讀/寫解析其活動。

### <a name="screenshot"></a>Screenshot

在以下的螢幕擷取畫面，我們可以看到前 10 個虛擬機器的儲存體活動：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>運作方式

不同於`Get-PhysicalDisk`，則`Get-VM`cmdlet 無法感知叢集 – 它只會在本機伺服器上傳回的 Vm。 若要查詢從每一部伺服器，以平行方式，將在我們呼叫`Invoke-Command (Get-ClusterNode).Name { ... }`。 為每個 VM 中，會得到`VHD.Iops.Total`， `VHD.Iops.Read`，和`VHD.Iops.Write`度量。 不指定`-TimeFrame`參數，會得到`MostRecent`每個單一資料點。

   > [!TIP]
   > 這些數列會反映這個 VM 的活動，其所有的 VHD/VHDX 檔案的總和。 這是其中效能歷程記錄會自動彙總為我們的範例。 若要取得的每個 VHD/VHDX 細分，您可以透過管道傳送個人`Get-VHD`成`Get-ClusterPerf`而不是 VM。

每一部伺服器的結果為聚集在一起`$Output`，我們可以`Sort-Object`，然後`Select-Object -First 10`。 請注意，`Invoke-Command`裝飾具有結果`PsComputerName`指出它們來自何處，我們可以列印到知道 VM 執行所在的內容。

### <a name="script"></a>指令碼

以下是指令碼：

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Iops {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = (" ", "K", "M", "B", "T") # Thousands, millions, billions, trillions...
        Do { if($RawValue -Gt 1000){$RawValue /= 1000 ; $i++ } } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-VM | ForEach-Object {
        $IopsTotal = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Total"
        $IopsRead  = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Read"
        $IopsWrite = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Write"
        [PsCustomObject]@{
            "VM" = $_.Name
            "IopsTotal" = Format-Iops $IopsTotal.Value
            "IopsRead"  = Format-Iops $IopsRead.Value
            "IopsWrite" = Format-Iops $IopsWrite.Value
            "RawIopsTotal" = $IopsTotal.Value # For sorting...
        }
    }
}

$Output | Sort-Object RawIopsTotal -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, IopsTotal, IopsRead, IopsWrite
```

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>範例 4:因為他們會說 「 25 gb 是新的 10 gb 」

這個範例會使用`NetAdapter.Bandwidth.Total`序列從`LastDay`時間範圍內尋找符號的網路已飽和，定義為 > 90%的理論的最大頻寬。 在叢集中每個網路介面卡，它會比較最高的觀察的頻寬使用量中所述的連結速度的最後一天。

### <a name="screenshot"></a>Screenshot

在以下的螢幕擷取畫面，我們會看到一*Fabrikam NX 4 Pro #2*尖峰最後一天：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>運作方式

我們會重複我們`Invoke-Command`一輪牌上述來`Get-NetAdapter`上每個伺服器和管道到`Get-ClusterPerf`。 過程中，我們抓取兩個相關的屬性： 其`LinkSpeed`字串，例如"10 Gbps 」，和其原始`Speed`10000000000 等的整數。 我們會使用`Measure-Object`以取得從最後一天的平均與尖峰 (提醒： 中的每個度量單位`LastDay`時間範圍表示 5 分鐘) 乘以每個位元組，若要取得的蘋果對蘋果比較的 8 位元。

   > [!NOTE]
   > 有些廠商，例如 Chelsio，包括遠端直接記憶體存取 (RDMA) 活動，在其*網路介面卡*效能計數器，讓它包含在`NetAdapter.Bandwidth.Total`系列。 例如 Mellanox，卻不然。 如果沒有，請只新增您的供應商`NetAdapter.Bandwidth.RDMA.Total`此指令碼的版本中的數列。

### <a name="script"></a>指令碼

以下是指令碼：

```
$Output = Invoke-Command (Get-ClusterNode).Name {

    Function Format-BitsPerSec {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("bps", "kbps", "Mbps", "Gbps", "Tbps", "Pbps") # Petabits, just in case!
        Do { $RawValue /= 1000 ; $i++ } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-NetAdapter | ForEach-Object {

        $Inbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Inbound" -TimeFrame "LastDay"
        $Outbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Outbound" -TimeFrame "LastDay"

        If ($Inbound -Or $Outbound) {

            $InterfaceDescription = $_.InterfaceDescription
            $LinkSpeed = $_.LinkSpeed
    
            $MeasureInbound = $Inbound | Measure-Object -Property Value -Maximum
            $MaxInbound = $MeasureInbound.Maximum * 8 # Multiply to bits/sec
    
            $MeasureOutbound = $Outbound | Measure-Object -Property Value -Maximum
            $MaxOutbound = $MeasureOutbound.Maximum * 8 # Multiply to bits/sec
    
            $Saturated = $False
    
            # Speed property is Int, e.g. 10000000000
            If (($MaxInbound -Gt (0.90 * $_.Speed)) -Or ($MaxOutbound -Gt (0.90 * $_.Speed))) {
                $Saturated = $True
                Write-Warning "In the last day, adapter '$InterfaceDescription' on server '$Env:ComputerName' exceeded 90% of its '$LinkSpeed' theoretical maximum bandwidth. In general, network saturation leads to higher latency and diminished reliability. Not good!"
            }
    
            [PsCustomObject]@{
                "NetAdapter"  = $InterfaceDescription
                "LinkSpeed"   = $LinkSpeed
                "MaxInbound"  = Format-BitsPerSec $MaxInbound
                "MaxOutbound" = Format-BitsPerSec $MaxOutbound
                "Saturated"   = $Saturated
            }
        }
    }
}

$Output | Sort-Object PsComputerName, InterfaceDescription | Format-Table PsComputerName, NetAdapter, LinkSpeed, MaxInbound, MaxOutbound, Saturated
```

## <a name="sample-5-make-storage-trendy-again"></a>範例 5:請儲存體 trendy 一次 ！

若要查看巨集的趨勢，效能歷程記錄會保留最多 1 年。 這個範例會使用`Volume.Size.Available`序列從`LastYear`時就會填滿，判斷儲存體填滿的速率及估計的時間範圍。

### <a name="screenshot"></a>Screenshot

在以下的螢幕擷取畫面，我們會看到*備份*新增每天大約 15 GB 的磁碟區：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-StorageTrend.png)

依此速率，它會在另一個 42 天達到其容量。

### <a name="how-it-works"></a>運作方式

`LastYear`時間範圍內有每日的一個資料點。 雖然您只會嚴格需要兩個點，以符合的趨勢線，實際上它是比較好的作法需要的詳細資訊，例如 14 天。 我們會使用`Select-Object -Last 14`若要設定的陣列 *（x，y）* 點，如*x*範圍 [1，14] 中。 這些點中，我們實作簡單[線性的最小平方演算法](http://mathworld.wolfram.com/LeastSquaresFitting.html)尋找`$A`並`$B`，會參數化為最適合的一行*y = ax + b<*。 歡迎來到高中所有超過一次。

將分割的磁碟區`SizeRemaining`趨勢屬性 (斜率`$A`) 可讓我們初步估計多少天後，在目前的儲存體成長速率，到磁碟區已滿為止。 `Format-Bytes`， `Format-Trend`，和`Format-Days`helper 函式美化輸出。

   > [!IMPORTANT]
   > 這項估計是線性和只根據最近 14 的每日測量。 有更複雜且正確的技術。 請執行良好的判斷，請勿依賴此指令碼來判斷是否要將心力灌注在擴充您的儲存體。 它會在此僅供教育。

### <a name="script"></a>指令碼

以下是指令碼：

```

Function Format-Bytes {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
    Do { $RawValue /= 1024 ; $i++ } While ( $RawValue -Gt 1024 )
    # Return
    [String][Math]::Round($RawValue) + " " + $Labels[$i]
}

Function Format-Trend {
    Param (
        $RawValue
    )
    If ($RawValue -Eq 0) {
        "0"
    }
    Else {
        If ($RawValue -Gt 0) {
            $Sign = "+"
        }
        Else {
            $Sign = "-"
        }
        # Return
        $Sign + $(Format-Bytes [Math]::Abs($RawValue)) + "/day"
    }
}

Function Format-Days {
    Param (
        $RawValue
    )
    [Math]::Round($RawValue)
}

$CSV = Get-Volume | Where-Object FileSystem -Like "*CSV*"

$Output = $CSV | ForEach-Object {

    $N = 14 # Require 14 days of history

    $Data = $_ | Get-ClusterPerf -VolumeSeriesName "Volume.Size.Available" -TimeFrame "LastYear" | Sort-Object Time | Select-Object -Last $N

    If ($Data.Length -Ge $N) {

        # Last N days as (x, y) points
        $PointsXY = @()
        1..$N | ForEach-Object {
            $PointsXY += [PsCustomObject]@{ "X" = $_ ; "Y" = $Data[$_-1].Value }
        }

        # Linear (y = ax + b) least squares algorithm
        $MeanX = ($PointsXY | Measure-Object -Property X -Average).Average
        $MeanY = ($PointsXY | Measure-Object -Property Y -Average).Average
        $XX = $PointsXY | ForEach-Object { $_.X * $_.X }
        $XY = $PointsXY | ForEach-Object { $_.X * $_.Y }
        $SSXX = ($XX | Measure-Object -Sum).Sum - $N * $MeanX * $MeanX
        $SSXY = ($XY | Measure-Object -Sum).Sum - $N * $MeanX * $MeanY
        $A = ($SSXY / $SSXX)
        $B = ($MeanY - $A * $MeanX)
        $RawTrend = -$A # Flip to get daily increase in Used (vs decrease in Remaining)
        $Trend = Format-Trend $RawTrend

        If ($RawTrend -Gt 0) {
            $DaysToFull = Format-Days ($_.SizeRemaining / $RawTrend)
        }
        Else {
            $DaysToFull = "-"
        }
    }
    Else {
        $Trend = "InsufficientHistory"
        $DaysToFull = "-"
    }

    [PsCustomObject]@{
        "Volume"     = $_.FileSystemLabel
        "Size"       = Format-Bytes ($_.Size)
        "Used"       = Format-Bytes ($_.Size - $_.SizeRemaining)
        "Trend"      = $Trend
        "DaysToFull" = $DaysToFull
    }
}

$Output | Format-Table
```

## <a name="sample-6-memory-hog-you-can-run-but-you-cant-hide"></a>範例 6:您可以執行記憶體的怪物，但您無法隱藏

因為效能歷程記錄會收集及整個叢集，您永遠不會需要將拼接在一起資料來自不同的電腦，不論多次會集中儲存 Vm 主機之間移動。 這個範例會使用`VM.Memory.Assigned`序列從`LastMonth`識別耗用最多記憶體，過去 35 天的虛擬機器的時間範圍。

### <a name="screenshot"></a>Screenshot

以下的螢幕擷取畫面，在中，我們可以看到前 10 個虛擬機器的記憶體使用量上個月：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>運作方式

我們會重複我們`Invoke-Command`技巧，到上面，介紹`Get-VM`每部伺服器上。 我們會使用`Measure-Object -Average`然後針對每個 VM，取得每月的平均`Sort-Object`後面`Select-Object -First 10`取得我們排行榜。 (或它可能是我們*最令人期待*清單？)

### <a name="script"></a>指令碼

以下是指令碼：

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Bytes {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        Do { if( $RawValue -Gt 1024 ){ $RawValue /= 1024 ; $i++ } } While ( $RawValue -Gt 1024 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }
    
    Get-VM | ForEach-Object {
        $Data = $_ | Get-ClusterPerf -VMSeriesName "VM.Memory.Assigned" -TimeFrame "LastMonth"
        If ($Data) {
            $AvgMemoryUsage = ($Data | Measure-Object -Property Value -Average).Average
            [PsCustomObject]@{
                "VM" = $_.Name
                "AvgMemoryUsage" = Format-Bytes $AvgMemoryUsage.Value
                "RawAvgMemoryUsage" = $AvgMemoryUsage.Value # For sorting...
            }
        }
    }
}

$Output | Sort-Object RawAvgMemoryUsage -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, AvgMemoryUsage
```

就這麼容易！ 希望這些範例可激發您，並幫助您開始。 與儲存空間直接存取的效能歷程記錄功能強大，指令碼友善`Get-ClusterPerf`cmdlet，您能夠的問答園地 – ！ – 當您管理及監視您的 Windows Server 2019 基礎結構的複雜問題。

## <a name="see-also"></a>另請參閱

- [開始使用 Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
- [效能歷程記錄](performance-history.md)
