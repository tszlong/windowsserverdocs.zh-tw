---
description: 深入瞭解：使用 PowerShell 編寫腳本和儲存空間直接存取效能歷程記錄
title: 使用儲存空間直接存取效能歷程記錄的腳本
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
ms.localizationpriority: medium
ms.openlocfilehash: 76820414e98487e3cf046f53d914f090ba037e48
ms.sourcegitcommit: 6a62d736e4d9989515c6df85e2577662deb042b6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2021
ms.locfileid: "98103760"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>使用 PowerShell 編寫腳本和儲存空間直接存取效能歷程記錄

> 適用於：Windows Server 2019

在 Windows Server 2019 中， [儲存空間直接存取](storage-spaces-direct-overview.md) 記錄並儲存虛擬機器、伺服器、磁片區、磁片區、網路介面卡等的廣泛 [效能歷程記錄](performance-history.md) 。 您可以輕鬆地在 PowerShell 中查詢和處理效能歷程記錄，讓您可以快速地從 *原始資料* 移至 *實際的答案* ，如下所示的問題：

1. 上周是否有任何 CPU 尖峰？
2. 任何實體磁片是否有異常的延遲？
3. 哪些 Vm 目前耗用最多儲存體 IOPS？
4. 我的網路頻寬是否飽和？
5. 此磁片區何時會用盡可用空間？
6. 在過去幾個月中，哪些 Vm 使用最多的記憶體？

`Get-ClusterPerf`Cmdlet 是針對腳本所建立的。 它接受來自管線等 Cmdlet 的輸入 `Get-VM` `Get-PhysicalDisk` 來處理關聯，而您可以使用管線將其輸出傳送至公用程式 Cmdlet，例如 `Sort-Object` 、 `Where-Object` 和， `Measure-Object` 以快速撰寫強大的查詢。

**本主題提供並說明6個範例腳本，以回答上述6個問題。** 它們呈現您可以套用的模式，以在各種資料和時間範圍內尋找尖峰、尋找平均、繪製趨勢線、執行極端偵測等等。 它們是以免費的起始程式碼提供，供您複製、擴充和重複使用。

   > [!NOTE]
   > 為了簡潔起見，範例腳本會省略您可能預期會有高品質 PowerShell 程式碼的一些內容，例如錯誤處理。 它們主要是用於靈感和教育版，而不是生產環境的用途。

## <a name="sample-1-cpu-i-see-you"></a>範例1： CPU，我看到您！

此範例會使用 `ClusterNode.Cpu.Usage` 時間範圍中的數列來顯示叢集中每部 `LastWeek` 伺服器的最大 ( 「上限」 ) 、最小值和平均 CPU 使用率。 它也會執行簡單的四向分析，以顯示過去8天內 CPU 使用量超過25%、50% 和75% 的時數。

### <a name="screenshot"></a>螢幕擷取畫面

在以下的螢幕擷取畫面中，我們發現 *Server-02* 有上周的尖峰尖峰：

![顯示 Server-02 有上周未解釋尖峰尖峰的螢幕擷取畫面。](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>運作方式

管道的輸出會很 `Get-ClusterPerf` 美觀地進入內建的 `Measure-Object` Cmdlet 中，我們只會指定 `Value` 屬性。 使用它的 `-Maximum` 、 `-Minimum` 和 `-Average` 旗標， `Measure-Object` 讓我們幾乎免費提供前三個數據行。 若要進行四分四個分析，我們可以傳送管線， `Where-Object` 並計算 `-Gt` (大於) 25、50或75的值數目。 最後一個步驟是美化 with `Format-Hours` 和 helper 函式 `Format-Percent` -當然是選擇性的。

### <a name="script"></a>指令碼

腳本如下：

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

## <a name="sample-2-fire-fire-latency-outlier"></a>範例2：引發、引發、延遲極端

此範例會使用 `PhysicalDisk.Latency.Average` 時間範圍中的數列 `LastHour` 來尋找統計極端值，並將其定義為每小時平均延遲超過 +3 σ (三個標準差) 在人口平均之上。

   > [!IMPORTANT]
   > 為了簡潔起見，此腳本不會針對低變異數執行保護，不會處理部分遺漏的資料，也不會區分模型或固件等等。請執行良好的判斷來，不要單獨依賴此腳本來判斷是否要更換硬碟。 基於教育目的，此處只提供此功能。

### <a name="screenshot"></a>螢幕擷取畫面

在以下螢幕擷取畫面中，我們會看到沒有極端值：

![顯示沒有極端值的螢幕擷取畫面。](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>運作方式

首先，藉由檢查是否一致地排除閒置或幾乎閒置的磁片磁碟機 `PhysicalDisk.Iops.Total` `-Gt 1` 。 針對每個作用中的 HDD，我們會 `LastHour` 將其時間範圍（由10秒間隔的360度量組成）輸送至，以 `Measure-Object -Average` 取得其過去一小時內的平均延遲。 這會設定我們的人口。

我們會執行 [廣泛的公式](http://www.mathsisfun.com/data/standard-deviation.html) ，以找出人口的平均數 `μ` 和標準差 `σ` 。 針對每個使用中的 HDD，我們會比較其平均延遲與人口平均，並除以標準差。 我們會保留未經處理的值，因此我們可以 `Sort-Object` 得到結果，但使用 `Format-Latency` 和 helper 函式 `Format-StandardDeviation` 來美化我們所顯示的內容-當然是選擇性的。

如果有任何磁片磁碟機超過 +3 σ，我們會 `Write-Host` 以紅色表示; 如果不是，則以綠色表示。

### <a name="script"></a>指令碼

腳本如下：

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

## <a name="sample-3-noisy-neighbor-thats-write"></a>範例3：有雜訊的鄰居？ 這是寫的！

效能歷程記錄也可以回答有關 *目前* 的問題。 每隔10秒可即時使用新的度量。 此範例會使用 `VHD.Iops.Total` 時間範圍中的數列 `MostRecent` 找出最忙碌的 (其中一些可能會說「noisiest」 ) 虛擬機器在叢集中的每一部主機上耗用最多儲存體 IOPS，並顯示其活動的讀取/寫入明細。

### <a name="screenshot"></a>螢幕擷取畫面

在以下螢幕擷取畫面中，我們會依儲存體活動看到前10部虛擬機器：

![依儲存體活動顯示前10部虛擬機器的螢幕擷取畫面。](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>運作方式

與不同 `Get-PhysicalDisk` 的是， `Get-VM` Cmdlet 不是叢集感知的-它只會傳回本機伺服器上的 vm。 若要以平行方式查詢每一部伺服器，我們會將呼叫包裝在中 `Invoke-Command (Get-ClusterNode).Name { ... }` 。 針對每個 VM，我們會取得 `VHD.Iops.Total` 、 `VHD.Iops.Read` 和 `VHD.Iops.Write` 測量。 藉由未指定 `-TimeFrame` 參數，我們會取得 `MostRecent` 每個參數的單一資料點。

   > [!TIP]
   > 這些數列會反映此 VM 的所有 VHD/VHDX 檔案的活動總和。 這是為我們自動匯總效能歷程記錄的範例。 若要取得每個 VHD/VHDX 細目，您可以使用管線將個別的管線傳送 `Get-VHD` 到， `Get-ClusterPerf` 而不是使用 VM。

每一部伺服器的結果會以 `$Output` 我們的方式合併 `Sort-Object` `Select-Object -First 10` 。 請注意， `Invoke-Command` 裝飾的結果具有 `PsComputerName` 指出其來源的屬性，我們可以列印這些結果來瞭解 VM 的執行位置。

### <a name="script"></a>指令碼

腳本如下：

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

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>範例4：說，「25-gb 是新的 10 gb」

此範例會使用 `NetAdapter.Bandwidth.Total` 時間範圍中的數列 `LastDay` 來尋找網路飽和符號，定義為 >90% 的理論最大頻寬。 針對叢集中的每個網路介面卡，它會比較最後一天觀察到的最高頻寬使用量，以及其所指定的連結速度。

### <a name="screenshot"></a>螢幕擷取畫面

在以下螢幕擷取畫面中，我們會看到前一天有一個 *FABRIKAM NX-4 Pro #2* 尖峰：

![顯示前一天 Fabrikam NX-4 Pro #2 尖峰的螢幕擷取畫面。](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>運作方式

我們 `Invoke-Command` `Get-NetAdapter` 在每個伺服器和管道上重複我們的訣竅 `Get-ClusterPerf` 。 在過程中，我們會抓取兩個相關的屬性：其 `LinkSpeed` 字串（例如 "10 Gbps"）及其原始 `Speed` 整數（如10000000000）。 我們使用 `Measure-Object` 來取得最後一天的平均和尖峰 (提醒：時間範圍中的每個測量值都 `LastDay` 代表5分鐘) 並且乘以每個位元組8個位以取得蘋果至蘋果的比較。

   > [!NOTE]
   > 某些廠商（例如，Chelsio）包含遠端直接記憶體存取 (RDMA) 在其 *網路介面卡* 效能計數器中的活動，因此包含在 `NetAdapter.Bandwidth.Total` 數列中。 其他（例如 Mellanox）則不可能。 如果您的廠商未這樣做，只要 `NetAdapter.Bandwidth.RDMA.Total` 在您的版本中加入此系列腳本即可。

### <a name="script"></a>指令碼

腳本如下：

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

## <a name="sample-5-make-storage-trendy-again"></a>範例5：再次讓儲存體 trendy！

若要查看宏趨勢，可保留最多1年的效能歷程記錄。 此範例會使用 `Volume.Size.Available` 時間範圍中的數列 `LastYear` 來判斷儲存體填滿的速率，並在其填滿時進行預估。

### <a name="screenshot"></a>螢幕擷取畫面

在以下螢幕擷取畫面中，我們會看到 *備份* 磁片區每日新增大約 15 GB：

![顯示備份磁片區每天大約增加 15 GB 的螢幕擷取畫面。](media/performance-history/Show-StorageTrend.png)

如此一來，它就會在另一個42天內達到其容量。

### <a name="how-it-works"></a>運作方式

`LastYear`時間範圍每天有一個資料點。 雖然您只需要兩個點來容納趨勢線，但其實最好還是需要更多，例如14天。 我們使用 `Select-Object -Last 14` 來設定範圍 [1，14] 中 *x* 的 *(x，y)* 點的陣列。 使用這些點時，我們會採用簡單的 [線性最小平方演算法](http://mathworld.wolfram.com/LeastSquaresFitting.html) 來尋找 `$A` ，並將 `$B` 最適合 *y = ax + b* 的行參數化。 歡迎使用高中。

將磁片區的 `SizeRemaining` 屬性除以 (斜率的趨勢 `$A`) 可讓我們初步估計目前的儲存體成長率，直到磁片區已滿的天數。 `Format-Bytes`、 `Format-Trend` 和 `Format-Days` helper 函數會美化輸出。

   > [!IMPORTANT]
   > 這項估計值是線性的，且僅根據最新的14個每日度量。 有更精密且更精確的技巧。 請執行良好的判斷來，而且不要單獨依賴此腳本來判斷是否投資擴充您的儲存體。 基於教育目的，此處只提供此功能。

### <a name="script"></a>指令碼

腳本如下：

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

## <a name="sample-6-memory-hog-you-can-run-but-you-cant-hide"></a>範例6：記憶體怪物，您可以執行但無法隱藏

由於系統會針對整個叢集集中收集和儲存效能歷程記錄，因此無論 Vm 在主機之間移動多少次，您都不需要將來自不同機器的資料拼接在一起。 此範例會使用 `VM.Memory.Assigned` `LastMonth` 時間範圍內的數列，來識別在過去35天內耗用最多記憶體的虛擬機器。

### <a name="screenshot"></a>螢幕擷取畫面

在以下螢幕擷取畫面中，我們會看到上個月的前10部虛擬機器（依記憶體使用量）：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>運作方式

我們會 `Invoke-Command` `Get-VM` 在每一部伺服器上重複上述介紹的秘訣。 我們會使用 `Measure-Object -Average` 來取得每個 VM 的每月平均，然後 `Sort-Object` 接著取得我們的 `Select-Object -First 10` 排行榜。  (或也許是 *最需要* 的清單呢？ ) 

### <a name="script"></a>指令碼

腳本如下：

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

這樣就完成了！ 希望這些範例能為您提供協助，並説明您開始著手。 有了儲存空間直接存取的效能歷程記錄和功能強大、腳本方便的 `Get-ClusterPerf` Cmdlet，您就有權詢問–和解答！ -管理和監視 Windows Server 2019 基礎結構時的複雜問題。

## <a name="additional-references"></a>其他參考資料

- [開始使用 Windows PowerShell](/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [儲存空間直接存取概觀](storage-spaces-direct-overview.md) \(部分機器翻譯\)
- [效能歷程記錄](performance-history.md)
