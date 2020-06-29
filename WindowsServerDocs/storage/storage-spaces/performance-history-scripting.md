---
title: 使用儲存空間直接存取效能歷程記錄的腳本
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
ms.localizationpriority: medium
ms.openlocfilehash: 53a5f2aa403c83d24acde1fc57e793141175d9b6
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474715"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>使用 PowerShell 編寫腳本，並儲存空間直接存取效能歷程記錄

> 適用於：Windows Server 2019

在 Windows Server 2019 中，[儲存空間直接存取](storage-spaces-direct-overview.md)記錄並儲存虛擬機器、伺服器、磁片磁碟機、磁片區、網路介面卡等等的大量[效能歷程記錄](performance-history.md)。 效能歷程記錄很容易在 PowerShell 中進行查詢和處理，因此您可以快速地從*原始資料*移至*實際答案*，例如：

1. 上周是否有任何 CPU 尖峰？
2. 是否有任何實體磁片發生異常延遲？
3. 哪些 Vm 目前耗用最多的儲存體 IOPS？
4. 我的網路頻寬是否飽和？
5. 此磁片區的可用空間何時會用盡？
6. 在過去一個月中，哪些 Vm 使用了最多的記憶體？

`Get-ClusterPerf`Cmdlet 是針對腳本而建立的。 它會接受來自或的 Cmdlet 的輸入 `Get-VM` `Get-PhysicalDisk` 來處理關聯，而且您可以使用管線將其輸出傳送至公用程式 Cmdlet `Sort-Object` ，例如、 `Where-Object` 和， `Measure-Object` 以快速撰寫功能強大的查詢。

**本主題提供並說明6個可回答上述6個問題的範例腳本。** 它們呈現的模式可供您在各種不同的資料和時間範圍內，用來尋找尖峰、尋找平均值、繪製趨勢線、執行極端情況偵測等等。 它們會以免費的入門程式碼的形式提供，供您複製、擴充和重複使用。

   > [!NOTE]
   > 為求簡潔，範例腳本會省略您可能會預期高品質 PowerShell 程式碼的錯誤處理。 其主要用於靈感和教育，而不是生產用途。

## <a name="sample-1-cpu-i-see-you"></a>範例1： CPU，我看到您！

這個範例會使用 `ClusterNode.Cpu.Usage` `LastWeek` 時間範圍內的數列來顯示叢集中每部伺服器的最大值（「上限標準」）、最小和平均 CPU 使用量。 它也會執行簡單的四次分析，以顯示過去8天內 CPU 使用量超過25%、50% 和75% 的小時數。

### <a name="screenshot"></a>螢幕擷取畫面

在下面的螢幕擷取畫面中，我們看到*Server-02*的過去一周內有無法解釋的尖峰：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>運作方式

管道的輸出會適當地 `Get-ClusterPerf` 放入內建的 `Measure-Object` Cmdlet 中，我們只會指定 `Value` 屬性。 使用其 `-Maximum` 、 `-Minimum` 和 `-Average` 旗標，可 `Measure-Object` 讓我們幾乎免費提供前三個數據行。 若要進行四向分析，我們可以透過管道傳送至， `Where-Object` 並計算有多少值 `-Gt` （大於）25、50或75。 最後一個步驟是美化 with `Format-Hours` 和 helper 函式 `Format-Percent` –當然是選擇性的。

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

這個範例會使用 `PhysicalDisk.Latency.Average` 時間範圍內的數列 `LastHour` 來尋找統計極端值，其定義為磁片磁碟機，其每小時平均延遲超過 +3 σ（三個標準差）高於人口平均。

   > [!IMPORTANT]
   > 為求簡潔，此腳本不會針對低變異數執行保護措施，不會處理部分遺失的資料，也不會因模型或固件而區別等等。請執行良好的 judgement，不要單獨依賴此腳本來判斷是否要更換硬碟。 在此只是為了教育目的而呈現。

### <a name="screenshot"></a>螢幕擷取畫面

在下面的螢幕擷取畫面中，我們看到沒有極端值：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>運作方式

首先，我們會藉由檢查是否一致地排除閒置或近乎閒置的磁片磁碟機 `PhysicalDisk.Iops.Total` `-Gt 1` 。 針對每個使用中 HDD，我們會 `LastHour` 將其時間範圍（以10秒間隔的360度量組成）輸送至， `Measure-Object -Average` 以取得其過去一小時的平均延遲。 這會設定我們的人口。

我們會實行[廣為](http://www.mathsisfun.com/data/standard-deviation.html)辨識的公式來尋找擴展的平均值 `μ` 和標準差 `σ` 。 針對每個使用中 HDD，我們會比較其平均延遲與人口平均值，並除以標準差。 我們會保留原始值，讓我們可以 `Sort-Object` 得到結果，但使用 `Format-Latency` 和 `Format-StandardDeviation` helper 函式來美化我們所要顯示的內容–當然是選擇性的。

如果有任何磁片磁碟機大於 +3 σ，我們會 `Write-Host` 以紅色表示，如果沒有，則以綠色顯示。

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

## <a name="sample-3-noisy-neighbor-thats-write"></a>範例3：有雜訊的鄰居？ 那就寫了！

效能歷程記錄*現在*也可以回答問題。 新的度量會即時提供，每10秒一次。 這個範例會使用 `VHD.Iops.Total` `MostRecent` 時間範圍內的數列，找出耗用最多儲存 IOPS 的虛擬機器（可能是 "noisiest"），在叢集中的每個主機上，並顯示其活動的讀取/寫入細目。

### <a name="screenshot"></a>螢幕擷取畫面

在下面的螢幕擷取畫面中，我們會看到依儲存體活動列出的前10名虛擬機器：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>運作方式

不同 `Get-PhysicalDisk` 于，此 `Get-VM` Cmdlet 不是叢集感知的，它只會傳回本機伺服器上的 vm。 若要以平行方式從每一部伺服器查詢，我們會將呼叫包裝在中 `Invoke-Command (Get-ClusterNode).Name { ... }` 。 針對每個 VM，我們會取得 `VHD.Iops.Total` 、 `VHD.Iops.Read` 和 `VHD.Iops.Write` 度量。 藉由不指定 `-TimeFrame` 參數，我們會取得 `MostRecent` 每個的單一資料點。

   > [!TIP]
   > 這些系列會將此 VM 活動的總和反映到其所有 VHD/VHDX 檔案。 這是為我們自動匯總效能歷程記錄的範例。 若要取得每個 VHD/VHDX 細目，您可以使用管線將個人移 `Get-VHD` 入， `Get-ClusterPerf` 而不是 VM。

每一部伺服器的結果都是 `$Output` ，我們可以接著再進行 `Sort-Object` `Select-Object -First 10` 。 請注意，會 `Invoke-Command` 以指出其來源的屬性裝飾結果 `PsComputerName` ，我們可以加以列印以知道 VM 的執行位置。

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

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>範例4：如其所說，"25-gb 是新的 10 gb"

這個範例會使用 `NetAdapter.Bandwidth.Total` `LastDay` 時間範圍內的數列來尋找網路飽和的徵兆，其定義為 >90% 的理論最大頻寬。 針對叢集中的每個網路介面卡，它會比較過去一天中最高觀察到的頻寬使用量和其指定的連結速度。

### <a name="screenshot"></a>螢幕擷取畫面

在下面的螢幕擷取畫面中，我們看到過去一天內有一個*FABRIKAM NX-4 Pro #2*尖峰：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>運作方式

我們會 `Invoke-Command` `Get-NetAdapter` 在每一部伺服器上重複我們的技巧，並將其傳送至 `Get-ClusterPerf` 。 在此過程中，我們會抓取兩個相關的屬性：其 `LinkSpeed` 字串（例如 "10 Gbps"）及其原始 `Speed` 整數（例如10000000000）。 我們使用 `Measure-Object` 來取得最後一天的平均值和尖峰（提醒：時間範圍內的每個度量 `LastDay` 代表5分鐘），並乘以每個位元組8位，以取得蘋果對蘋果的比較。

   > [!NOTE]
   > 某些廠商（例如，Chelsio）包含其*網路介面卡*效能計數器中的遠端直接記憶體存取（RDMA）活動，因此包含在系列中 `NetAdapter.Bandwidth.Total` 。 有些人（如 Mellanox）可能不會。 如果您的廠商不這麼做，只要 `NetAdapter.Bandwidth.RDMA.Total` 在您的此腳本版本中新增數列即可。

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

## <a name="sample-5-make-storage-trendy-again"></a>範例5：再次使儲存體 trendy！

為了查看宏趨勢，會保留最多1年的效能歷程記錄。 這個範例會使用 `Volume.Size.Available` 時間範圍內的數列 `LastYear` 來判斷儲存體已填滿的速率，並預估何時會完成。

### <a name="screenshot"></a>螢幕擷取畫面

在下面的螢幕擷取畫面中，我們看到*備份*磁片區每天新增大約 15 GB：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-StorageTrend.png)

在這種情況下，它會在另一個42天內達到其容量。

### <a name="how-it-works"></a>運作方式

`LastYear`時間範圍每天有一個資料點。 雖然您只需要兩個點以符合趨勢線，但實際上最好是需要更多的時間，例如14天。 我們使用 `Select-Object -Last 14` 設定範圍 [1，14] 中*x*的 *（x，y）* 點陣列。 有了這些點之後，我們會執行簡單的[線性最小平方演算法](http://mathworld.wolfram.com/LeastSquaresFitting.html)，以尋找 `$A` 並 `$B` 將最符合*y = ax + b*的線條參數化。 歡迎使用高中。

將磁片區的 `SizeRemaining` 屬性除以趨勢（斜率）， `$A` 可讓我們初步估計目前的儲存體成長率，直到量已滿為止。 `Format-Bytes`、和 helper 函式會 `Format-Trend` `Format-Days` 美化輸出。

   > [!IMPORTANT]
   > 此預估是線性的，而且僅根據最新的14個每日測量。 有更複雜且更精確的技術存在。 請執行良好的 judgement，並不要單獨依賴此腳本來判斷是否要投資擴充您的儲存體。 在此只是為了教育目的而呈現。

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

由於會針對整個叢集收集並集中儲存效能歷程記錄，因此不論 Vm 在主機之間移動多少次，您都不需要將不同電腦的資料結合在一起。 這個範例會使用 `VM.Memory.Assigned` `LastMonth` 時間範圍內的數列，來識別在過去35天耗用最多記憶體的虛擬機器。

### <a name="screenshot"></a>螢幕擷取畫面

在下面的螢幕擷取畫面中，我們會看到上個月的前10部虛擬機器（依記憶體使用量）：

![PowerShell 的螢幕擷取畫面](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>運作方式

我們在每部 `Invoke-Command` 伺服器上重複上述的訣竅 `Get-VM` 。 我們會使用 `Measure-Object -Average` 來取得每個 VM 的每月平均，然後再 `Sort-Object` 按 `Select-Object -First 10` 以取得我們的排行榜。 （或者，這是我們*最希望*的清單？）

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

大功告成！ 希望這些範例可以讓您激發，並協助您開始使用。 有了儲存空間直接存取的效能歷程記錄，以及功能強大、腳本易懂的 `Get-ClusterPerf` Cmdlet，您都可以問與答！ –管理和監視 Windows Server 2019 基礎結構時的複雜問題。

## <a name="additional-references"></a>其他參考

- [Windows PowerShell 使用者入門](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
- [效能歷程記錄](performance-history.md)
