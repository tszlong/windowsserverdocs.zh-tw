---
title: 指令碼搭配儲存空間直接存取的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 78ecb64cac789751abf9fd3f55b4a1fcbbe4dad2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2018
ms.locfileid: "8843106"
---
# 指令碼搭配 PowerShell 和儲存空間直接存取的效能歷程記錄

> 適用於： Windows Server Insider Preview 組建 17692 及更新版本

在 Windows Server 2019，[儲存空間直接存取](storage-spaces-direct-overview.md)會記錄並儲存虛擬機器、 伺服器、 磁碟機、 磁碟區、 網路介面卡，及更廣泛的[效能歷程記錄](performance-history.md)。 效能歷程記錄是輕鬆地查詢和 PowerShell 中的程序，因此您可以快速瀏覽從*原始資料*的*實際解答*像：

1. 是否有任何 CPU 尖峰最後一週？
2. 任何實體磁碟一直發生異常延遲？
3. 現在耗用大量那些 Vm 最多儲存 IOPS？
4. 我的網路頻寬飽和？
5. 何時這個磁碟區將可用空間退出執行？
6. 在過去一個月，那些 Vm 使用最多的記憶體？

`Get-ClusterPerf` Cmdlet 內建的指令碼。 它接受來自像 cmdlet 的輸入`Get-VM`或`Get-PhysicalDisk`管線處理關聯的關係，而且您可以使用管線傳送其輸出到公用程式 cmdlet，例如`Sort-Object`， `Where-Object`，以及`Measure-Object`快速編寫強大的查詢。

**本主題提供，並說明 6 回答 6 上述問題的範例指令碼。** 他們呈現的模式，您可以套用了山峰、 尋找平均值、 繪製趨勢行，請執行 outlier，偵測，以及更多各種不同的資料和時間範圍。 他們提供免費的起始程式碼，為您要複製、 擴充和重複使用。

   > [!NOTE]
   > 為求簡明起見，範例指令碼會省略像是您所預期的高品質的 PowerShell 程式碼的錯誤處理。 它們是主要用於靈感和教育版而不是實際執行使用。

## 範例 1: CPU，查看您 ！

這個範例使用`ClusterNode.Cpu.Usage`從系列`LastWeek`來顯示叢集中的最大值 （「 高水域標記 」），為每個伺服器的最小和平均 CPU 使用量的時間範圍。 它也會顯示多少小時 CPU 使用量超過已 25%的簡單四分位分析 50%，並在最後一個 8 天內的 75%。

### Screenshot

我們在下方的螢幕擷取畫面，請參閱*伺服器 02*擁有不明的增量最後一週：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-CpuMinMaxAvg.png)

### 運作方式

從輸出`Get-ClusterPerf`恰當到內建的管道`Measure-Object`cmdlet，我們只需指定`Value`屬性。 使用其`-Maximum`， `-Minimum`，以及`-Average`旗標， `Measure-Object` ，讓我們的前三個資料行幾乎免費。 若要執行四分位分析，我們可以使用管線傳送到`Where-Object`並計算了多少值`-Gt`（大於） 25、 50 或 75。 最後一個步驟是使用美化`Format-Hours`和`Format-Percent`協助程式函式 – 當然是選用。

### 指令碼

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

## 範例 2： 射擊，射擊、 延遲 outlier

這個範例使用`PhysicalDisk.Latency.Average`從系列`LastHour`統計範數，尋找的時間範圍定義為每小時平均延遲超過 + 3σ 的磁碟機 （三個標準差） 擴展平均上方。

   > [!IMPORTANT]
   > 為求簡潔，這個指令碼不會實作針對低差異的防護功能，不會處理遺失的部分資料，不會無法區別的模型或韌體等等。請練習良好 judgement 並不依賴此指令碼來判斷是否要取代的硬碟。 僅供教育此處提供它。

### Screenshot

我們在下方的螢幕擷取畫面，請參閱有沒有範數：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-LatencyOutlierHDD.png)

### 運作方式

首先，我們排除閒置或幾乎閒置的磁碟機，檢查`PhysicalDisk.Iops.Total`以一致的方式是`-Gt 1`。 針對每個作用中的 HDD，我們使用管線傳送其`LastHour`時期，組成 360 度量 10 第二個間隔，為`Measure-Object -Average`以取得其平均延遲在過去一小時。 這會設定我們的人員。

我們實作[廣泛已知公式](http://www.mathsisfun.com/data/standard-deviation.html)來尋找平均數`μ`和標準差`σ`的擴展。 針對每個作用中的 HDD，我們會比較擴展平均值其平均延遲並除以標準差。 我們保留原始值，因此我們可以`Sort-Object`我們的結果，但使用`Format-Latency`和`Format-StandardDeviation`來美化什麼我們將說明 – 當然是選用的協助程式函式。

如果任何磁碟機是多個 + 3σ，我們`Write-Host`以紅色;如果不是，為綠色。

### 指令碼

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

## 範例 3： 芳鄰的雜訊？ 這就是寫入 ！

效能歷程記錄可以太回答*立即*的相關問題。 新的測量值是提供即時、 每隔 10 秒。 這個範例使用`VHD.Iops.Total`從系列`MostRecent`來識別忙碌的 （一些可能說出 「 noisiest 」） 的時間範圍取用最儲存體 IOPS，整個叢集，並顯示其活動的讀/寫分解過程中的每一部主機的虛擬機器。

### Screenshot

在下方的螢幕擷取畫面，我們會儲存活動所看到前 10 虛擬機器：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-TopIopsVMs.png)

### 運作方式

不同於`Get-PhysicalDisk`、 `Get-VM` cmdlet 不是叢集感知 – 它只會傳回 Vm 在本機伺服器上。 若要查詢從每個伺服器以並行方式，我們我們將呼叫包裝在`Invoke-Command (Get-ClusterNode).Name { ... }`。 對於每個 VM 中，我們取得`VHD.Iops.Total`， `VHD.Iops.Read`，以及`VHD.Iops.Write`度量。 藉由不指定`-TimeFrame`參數，我們取得`MostRecent`的每個單一資料點。

   > [!TIP]
   > 這些系列反映此 VM 活動，其所有的 VHD/VHDX 檔案的總和。 這是其中效能歷程記錄自動彙總我們的範例。 若要取得每個 VHD/VHDX 細項，您可以使用管線傳送個別`Get-VHD`到`Get-ClusterPerf`而不是 VM。

每個伺服器的結果成形了做為`$Output`，其中我們可以`Sort-Object`，然後`Select-Object -First 10`。 請注意，`Invoke-Command`裝飾結果與`PsComputerName`屬性，指出他們來自何處，我們可以知道 VM 在何處執行列印。

### 指令碼

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

## 範例 4： 因為他們說，「 25 gb 是新的 10 gb 」

這個範例使用`NetAdapter.Bandwidth.Total`從系列`LastDay`以尋找網路飽和度的時間範圍定義為 > 90%的理論上的最大頻寬。 叢集中每個網路介面卡，它會為其指定的連結速度的最後一天中比較最高的觀察到的頻寬使用量。

### Screenshot

在下方的螢幕擷取畫面，我們會看到一個*Fabrikam NX 4 專業版 #2*尖峰最後一天中：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-NetworkSaturation.png)

### 運作方式

我們重複我們`Invoke-Command`以上述策略`Get-NetAdapter`上每個伺服器與管道到`Get-ClusterPerf`。 我們可以沿著一來，抓取兩個相關的屬性： 其`LinkSpeed`字串，如 「 10 Gbps 」，以及其原始`Speed`像 10000000000 整數。 我們使用`Measure-Object`以取得從最後一天的平均和尖峰 (提醒： 每個度量單位`LastDay`的時間範圍代表 5 分鐘)，並將乘以每個位元組，以取得頻果-頻果比較 8 位元。

   > [!NOTE]
   > 一些廠商，Chelsio，例如遠端直接記憶體存取 (RDMA) 活動中包含其*網路介面卡*的效能計數器，因此會包含在`NetAdapter.Bandwidth.Total`系列。 其他使用者，例如 Mellanox，可能不會。 如果您的廠商不會只需將新增`NetAdapter.Bandwidth.RDMA.Total`這個指令碼的版本中的一系列。

### 指令碼

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

## 範例 5： 儲存體 trendy 再讓 ！

若要查看巨集趨勢，效能歷程記錄保留的高達 1 年。 這個範例使用`Volume.Size.Available`從系列`LastYear`來判斷的儲存體填滿的速率和估計值時將會完整的時間範圍。

### Screenshot

我們在下方的螢幕擷取畫面，請參閱*備份*磁碟區新增每日約 15 GB:

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-StorageTrend.png)

在此速率，它將到達其容量另一個 42 天內。

### 運作方式

`LastYear`時間範圍中有一個的資料點，每日。 雖然您嚴格只需要兩個點，以符合趨勢線，實際上它是較好的需要更多，例如 14 天。 我們使用`Select-Object -Last 14`若要設定的 *（x，y）* 點， *x* [1，14] 的範圍中的陣列。 使用這些點，我們實作簡單[線性最少方形演算法](http://mathworld.wolfram.com/LeastSquaresFitting.html)來尋找`$A`和`$B`的參數化最適營運*y = ax + b*。 歡迎使用高學校所有超過一次。

分割的磁碟區`SizeRemaining`由趨勢的屬性 (斜率`$A`) 可讓我們初步估計多少天，以目前的存放裝置成長速率，直到磁碟區已滿。 `Format-Bytes`， `Format-Trend`，以及`Format-Days`協助程式函式美化輸出。

   > [!IMPORTANT]
   > 這個估計值是線性和僅根據最新的 14 每日度量。 更複雜且準確的技術存在。 請練習良好 judgement 並不依賴單獨以判斷是否要願意投資於展開您儲存此指令碼。 僅供教育此處提供它。

### 指令碼

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

## 範例 6： 您可以執行記憶體的怪物，但您無法隱藏

因為效能歷程記錄是收集並儲存在集中，針對整個叢集，您永遠不需要拼接在一起資料從不同的電腦，無論如何多次 Vm 之間移動的主機。 這個範例使用`VM.Memory.Assigned`從系列`LastMonth`來找出最多記憶體消耗過去 35 天的虛擬機器的時間範圍。

### Screenshot

在以下的螢幕擷取畫面，我們查看記憶體使用量上個月前 10 虛擬機器：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-TopMemoryVMs.png)

### 運作方式

我們重複我們`Invoke-Command`技巧，上方，為`Get-VM`每個伺服器上。 我們使用`Measure-Object -Average`針對每個 VM，然後取得每月平均`Sort-Object`後面加上`Select-Object -First 10`以取得我們排行榜。 （或有可能是我們*最想*list?）

### 指令碼

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

這樣就完成了！ 希望這些樣本啟發您，並協助您開始。 使用儲存空間直接存取的效能歷程記錄和強大，指令碼方便的`Get-ClusterPerf`cmdlet，您會獲得授權，以要求 – 和回答 ！ – 複雜的問題，當您管理和監視您的 Windows Server 2019 基礎結構。

## 請參閱

- [開始使用 Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
- [效能歷程](performance-history.md)
