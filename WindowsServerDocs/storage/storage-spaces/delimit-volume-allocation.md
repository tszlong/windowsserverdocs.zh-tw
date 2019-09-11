---
title: 分隔儲存空間直接存取中的磁片區配置
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/29/2018
ms.openlocfilehash: faf9547833764e9075e86515d1f486a5a3f61ff8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872085"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>分隔儲存空間直接存取中的磁片區配置
> 適用於：Windows Server Standard 2012 R2

Windows Server 2019 引進了一個選項，可手動將儲存空間直接存取中的磁片區配置加以分隔。 這麼做可以在某些情況下大幅增加容錯能力，但會強加一些額外的管理考慮和複雜度。 本主題說明其運作方式，並提供 PowerShell 中的範例。

   > [!IMPORTANT]
   > 這是 Windows Server 2019 中的新功能。 它在 Windows Server 2016 中無法使用。 

## <a name="prerequisites"></a>必要條件

### <a name="green-checkmark-iconmediadelimit-volume-allocationsupportedpng-consider-using-this-option-if"></a>![綠色核取記號圖示。](media/delimit-volume-allocation/supported.png) 如果是下列情況，請考慮使用此選項：

- 您的叢集有六部以上的伺服器;和
- 您的叢集僅使用[三向鏡像](storage-spaces-fault-tolerance.md#mirroring)復原

### <a name="red-x-iconmediadelimit-volume-allocationunsupportedpng-do-not-use-this-option-if"></a>![紅色 X 圖示。](media/delimit-volume-allocation/unsupported.png) 如果是下列情況，請勿使用此選項：

- 您的叢集少於六部伺服器;或
- 您[的叢集使用同](storage-spaces-fault-tolerance.md#parity)位檢查或[鏡像加速同位](storage-spaces-fault-tolerance.md#mirror-accelerated-parity)復原

## <a name="understand"></a>了解

### <a name="review-regular-allocation"></a>審查：一般配置

使用一般的三向鏡像時，磁片區會分割成多個小型的「slab」，並複製三次，並平均分散到叢集中每部伺服器上的每個磁片磁碟機。 如需詳細資訊，請閱讀[這篇深入探討的 blog](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/)。

![此圖顯示分割成三個 slab 堆疊，並在每部伺服器上平均散發的磁片區。](media/delimit-volume-allocation/regular-allocation.png)

此預設配置可將平行讀取和寫入最大化，進而提升效能，而且非常方便：每部伺服器都同樣忙碌，每個磁片磁碟機都有相同的空間，而且所有磁片區都保持上線或離線。 每個磁片區保證最多可承受兩個並行失敗，如[下列範例](storage-spaces-fault-tolerance.md#examples)所示。

不過，使用此配置時，磁片區不能在三個並行失敗的情況下存活。 如果有三部伺服器一次失敗，或三部伺服器中的磁片磁碟機一次失敗，磁片區就會變得無法存取，因為至少有一些 slab 是配置給每個失敗的磁片磁碟機或伺服器。

在下列範例中，伺服器1、3和5會同時失敗。 雖然許多 slab 都有存活的複本，但有些則不會：

![顯示六部伺服器以紅色醒目提示的圖表，而整體磁片區則為紅色。](media/delimit-volume-allocation/regular-does-not-survive.png)

磁片區會離線，並在伺服器復原之前變成無法存取。

### <a name="new-delimited-allocation"></a>新增：分隔配置

使用分隔配置時，您可以指定要使用的伺服器子集（三向鏡像最少三個）。 磁片區會分割成複製三次（如先前）的 slab，而不是在每一部伺服器上配置，而是**只會將 slab 配置給您指定的伺服器子集**。

![此圖顯示分割成三個 slab 堆疊，而且只散發到六部伺服器的三個。](media/delimit-volume-allocation/delimited-allocation.png)

#### <a name="advantages"></a>優點

使用此配置時，磁片區可能會存活三個並行失敗：事實上，在此情況下，其生存機率會從 0% （具有一般配置）增加到 95% （使用分隔的配置）！ 直覺來說，這是因為它不會相依于伺服器4、5或6，因此不會受到其失敗的影響。

在上述範例中，伺服器1、3和5會同時失敗。 因為分隔的配置可確保伺服器2包含每個樓板的複本，所以每個樓板都會有一個存活的複本，而且磁片區會保持連線並可供存取：

![顯示六部伺服器以紅色醒目提示的圖表，但整體磁片區為綠色。](media/delimit-volume-allocation/delimited-does-survive.png)

生存機率取決於伺服器的數目和其他因素–如需詳細資訊，請參閱[分析](#analysis)。

#### <a name="disadvantages"></a>缺點

分隔配置會強加一些新增的管理考慮和複雜度：

1. 系統管理員會負責分隔每個磁片區的配置，以平衡伺服器間的儲存使用量，並以[最佳做法](#best-practices)一節中所述的方式來維持高機率的生存率。

2. 使用分隔配置時，**每個伺服器保留一個容量磁片磁碟機的對應項（無最大值）** 。 這不是一般配置的[已發佈建議](plan-volumes.md#choosing-the-size-of-volumes)，會到超出出四個容量磁片磁碟機總計。

3. 如果伺服器失敗且需要取代（如[移除伺服器和其磁片磁碟機](remove-servers.md#remove-a-server-and-its-drives)中所述），系統管理員會負責新增新的伺服器並移除失敗的其中一個範例，以更新受影響的磁片區 delimitation。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

您可以使用`New-Volume` Cmdlet 在儲存空間直接存取中建立磁片區。

例如，若要建立一般的三向鏡像磁片區：

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>建立磁片區並界定其配置

建立三向鏡像磁片區，並將其配置分隔：

1. 首先，將您叢集中的伺服器指派給`$Servers`變數：

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > 在儲存空間直接存取中，「存放裝置縮放單位」一詞指的是連接到一部伺服器的所有原始存放裝置，包括直接連接的磁片磁碟機，以及具有磁片磁碟機的直接連接外部主機殼。 在此內容中，它與「伺服器」相同。

2. 指定要搭配新`-StorageFaultDomainsToUse`參數使用的伺服器，並藉由`$Servers`編制索引。 例如，將配置分隔到第一個、第二個和第三個伺服器（索引0、1和2）：

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2]
    ```

### <a name="see-a-delimited-allocation"></a>查看分隔的配置

若要查看*MyVolume*的配置方式，請`Get-VirtualDiskFootprintBySSU.ps1`使用[附錄](#appendix)中的腳本：

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  0       0       0      
```

請注意，只有 Server1、Server2 和 Server3 包含*MyVolume*的 slab。

### <a name="change-a-delimited-allocation"></a>變更分隔的配置

使用新`Add-StorageFaultDomain`的和`Remove-StorageFaultDomain` Cmdlet 來變更配置的分隔方式。

例如，若要將*MyVolume*移到一部伺服器上：

1. 指定第四部伺服器**可以**儲存*MyVolume*的 slab：

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[3]
    ```

2. 指定第一部伺服器**無法**儲存*MyVolume*的 slab：

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. 重新平衡存放集區，讓變更生效：

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

![此圖顯示從伺服器1、2和3到伺服器2、3和4的 slab 遷移移。](media/delimit-volume-allocation/move.gif)

您可以使用`Get-StorageJob`監視重新平衡的進度。

完成後，請`Get-VirtualDiskFootprintBySSU.ps1`再次執行以確認*MyVolume*已移動。

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  0       0      
```

請注意，Server1 不再包含*MyVolume*的 slab，而是 Server04。

## <a name="best-practices"></a>最佳做法

以下是使用分隔磁片區配置時要遵循的最佳作法：

### <a name="choose-three-servers"></a>選擇三部伺服器

將每個三向鏡像磁片區分隔成三部伺服器，而不是更多。

### <a name="balance-storage"></a>平衡儲存體

平衡配置給每部伺服器的儲存空間量，會計入磁片區大小。

### <a name="every-delimited-allocation-unique"></a>每個分隔的唯一配置

若要最大化容錯，請將每個磁片區的配置設為唯一，這表示它不會與另一個磁片區共用其*所有*伺服器（有一些重迭功能）。 有了 N 部伺服器，有「N 選擇3」獨特的組合–以下是對一些常見叢集大小的意義：

| 伺服器數目（N） | 唯一分隔配置的數目（N 選擇3） |
|-----------------------|-----------------------------------------------------|
| 6                     | 20                                                  |
| 8                     | 56                                                  |
| 12                    | 220                                                 |
| 16                    | 560                                                 |

   > [!TIP]
   > 請考慮這項 combinatorics 的實用審查[，並選擇標記法](https://betterexplained.com/articles/easy-permutations-and-combinations/)。

以下是最大化容錯的範例-每個磁片區都有唯一的分隔配置：

![唯一-配置](media/delimit-volume-allocation/unique-allocation.png)

相反地，在下一個範例中，前三個磁片區會使用相同的分隔配置（對伺服器1、2和3），而最後三個磁片區會使用相同的分隔配置（到伺服器4、5和6）。 這不會將容錯最大化：如果有三部伺服器失敗，多個磁片區可能會離線，並同時變成無法存取。

![非唯一的配置](media/delimit-volume-allocation/non-unique-allocation.png)

## <a name="analysis"></a>Analysis

這一節衍生的數學機率，是磁片區保持在線上且可存取的（或同等的整體儲存空間，可維持在線上且可供存取）作為失敗次數和叢集大小的功能。

   > [!NOTE]
   > 這是選擇性的閱讀區段。 如果您想要查看數學，請繼續閱讀！ 但如果沒有，別擔心：[PowerShell 中的使用方式](#usage-in-powershell)和[最佳作法](#best-practices)，只是您必須成功執行已分隔的配置。

### <a name="up-to-two-failures-is-always-okay"></a>最多有兩個失敗永遠都沒問題

每個三向鏡像磁片區在同一時間最多可承受兩個失敗，因為[這些範例](storage-spaces-fault-tolerance.md#examples)會說明，而不論其配置為何。 如果兩個磁片磁碟機失敗，或兩部伺服器故障，或其中一個失敗，則每個三向鏡像磁片區都保持連線並可供使用，即使是一般配置也一樣。

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>超過一半的叢集失敗，永遠都沒問題

相反地，在叢集中一半以上的伺服器或磁片磁碟機一次失敗時，[仲裁就會遺失](understand-quorum.md)，而且每個三向鏡像磁片區都會離線並變成無法存取，不論其配置為何。

### <a name="what-about-in-between"></a>在之間有何意義？

如果一次發生三個以上的失敗，但至少有一半的伺服器和磁片磁碟機仍在運作中，則具有分隔配置的磁片區可能會保持連線並可供存取，視哪些伺服器失敗而定。 讓我們執行數位來判斷確切的機率。

為了簡單起見，根據上述最佳做法，假設磁片區是獨立且完全分散的（IID），而且每個磁片區的配置都有足夠的唯一組合可供使用。 任何指定的磁片區不受的機率，也是由預期的線性不受的整體儲存體所預期的分數。 

假設有**N**個伺服器**的 f**發生失敗，配置給其中**3**的磁片區會離線（如果是），而只有在**F** **中發生**失敗。 有 **（N 選擇 f）** 發生**F**失敗的方法，其中 **（F 選擇3）** 導致磁片區離線並變成無法存取。 機率可以表示為：

![P_offline = Fc3/NcF](media/delimit-volume-allocation/probability-volume-offline.png)

在所有其他情況下，磁片區會保持連線並可供存取：

![P_online = 1 –（Fc3/NcF）](media/delimit-volume-allocation/probability-volume-online.png)

下表會評估一些常見叢集大小的機率，以及最多5次失敗，並顯示在每個案例中，相較于一般配置，已分隔的配置會增加容錯能力。

### <a name="with-6-servers"></a>含6部伺服器

| 配置                           | 存活1失敗的機率 | 存活2次失敗的機率 | 存活3次失敗的機率 | 存活4次失敗的機率 | 存活5次失敗的機率 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 一般，散佈在所有6部伺服器上 | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| 僅分隔為3部伺服器          | 100%                               | 100%                                | 95.0%                               | 0                                  | 0                                  |

   > [!NOTE]
   > 在6部以上的伺服器中超過3個失敗之後，叢集就會失去仲裁。

### <a name="with-8-servers"></a>含8部伺服器

| 配置                           | 存活1失敗的機率 | 存活2次失敗的機率 | 存活3次失敗的機率 | 存活4次失敗的機率 | 存活5次失敗的機率 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 一般，分散在所有8部伺服器上 | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| 僅分隔為3部伺服器          | 100%                               | 100%                                | 98.2%                               | 94.3%                               | 0                                  |

   > [!NOTE]
   > 在總共8部伺服器中超過4個失敗之後，叢集就會失去仲裁。

### <a name="with-12-servers"></a>含12部伺服器

| 配置                            | 存活1失敗的機率 | 存活2次失敗的機率 | 存活3次失敗的機率 | 存活4次失敗的機率 | 存活5次失敗的機率 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 一般，散佈在所有12部伺服器上 | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| 僅分隔為3部伺服器           | 100%                               | 100%                                | 99.5%                               | 99.2%                               | 98.7%                               |

### <a name="with-16-servers"></a>含16部伺服器

| 配置                            | 存活1失敗的機率 | 存活2次失敗的機率 | 存活3次失敗的機率 | 存活4次失敗的機率 | 存活5次失敗的機率 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 一般，散佈在所有16部伺服器上 | 100%                               | 100%                                | 0                                  | 0                                  | 0                                  |
| 僅分隔為3部伺服器           | 100%                               | 100%                                | 99.8%                               | 99.8%                               | 99.8%                               |

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="can-i-delimit-some-volumes-but-not-others"></a>我可以分隔某些磁片區，而不是其他磁片區嗎？

是的。 您可以選擇每個磁片區是否要分隔配置。

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>分隔配置是否會變更磁片磁碟機更換的運作方式？

否，與一般配置相同。

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
- [儲存空間直接存取中的容錯](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>附錄

此腳本可協助您查看磁片區的配置方式。

如以上所述使用它，請複製/貼上並`Get-VirtualDiskFootprintBySSU.ps1`另存新檔。

```PowerShell
Function ConvertTo-PrettyCapacity {
    Param (
        [Parameter(
            Mandatory = $True,
            ValueFromPipeline = $True
            )
        ]
    [Int64]$Bytes,
    [Int64]$RoundTo = 0
    )
    If ($Bytes -Gt 0) {
        $Base = 1024
        $Labels = ("bytes", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        $Order = [Math]::Floor( [Math]::Log($Bytes, $Base) )
        $Rounded = [Math]::Round($Bytes/( [Math]::Pow($Base, $Order) ), $RoundTo)
        [String]($Rounded) + " " + $Labels[$Order]
    }
    Else {
        "0"
    }
    Return
}

Function Get-VirtualDiskFootprintByStorageFaultDomain {

    ################################################
    ### Step 1: Gather Configuration Information ###
    ################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Gathering configuration information..." -Status "Step 1/4" -PercentComplete 00

    $ErrorCannotGetCluster = "Cannot proceed because 'Get-Cluster' failed."
    $ErrorNotS2DEnabled = "Cannot proceed because the cluster is not running Storage Spaces Direct."
    $ErrorCannotGetClusterNode = "Cannot proceed because 'Get-ClusterNode' failed."
    $ErrorClusterNodeDown = "Cannot proceed because one or more cluster nodes is not Up."
    $ErrorCannotGetStoragePool = "Cannot proceed because 'Get-StoragePool' failed."
    $ErrorPhysicalDiskFaultDomainAwareness = "Cannot proceed because the storage pool is set to 'PhysicalDisk' fault domain awareness. This cmdlet only supports 'StorageScaleUnit', 'StorageChassis', or 'StorageRack' fault domain awareness."

    Try  {
        $GetCluster = Get-Cluster -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetCluster
    }

    If ($GetCluster.S2DEnabled -Ne 1) {
        throw $ErrorNotS2DEnabled
    }

    Try  {
        $GetClusterNode = Get-ClusterNode -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetClusterNode
    }

    If ($GetClusterNode | Where State -Ne Up) {
        throw $ErrorClusterNodeDown
    }

    Try {
        $GetStoragePool = Get-StoragePool -IsPrimordial $False -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetStoragePool
    }

    If ($GetStoragePool.FaultDomainAwarenessDefault -Eq "PhysicalDisk") {
        throw $ErrorPhysicalDiskFaultDomainAwareness
    }

    ###########################################################
    ### Step 2: Create SfdList[] and PhysicalDiskToSfdMap{} ###
    ###########################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing physical disk information..." -Status "Step 2/4" -PercentComplete 25

    $SfdList = Get-StorageFaultDomain -Type ($GetStoragePool.FaultDomainAwarenessDefault) | Sort FriendlyName # StorageScaleUnit, StorageChassis, or StorageRack

    $PhysicalDiskToSfdMap = @{} # Map of PhysicalDisk.UniqueId -> StorageFaultDomain.FriendlyName
    $SfdList | ForEach {
        $StorageFaultDomain = $_
        $_ | Get-StorageFaultDomain -Type PhysicalDisk | ForEach {
            $PhysicalDiskToSfdMap[$_.UniqueId] = $StorageFaultDomain.FriendlyName
        }
    }

    ##################################################################################################
    ### Step 3: Create VirtualDisk.FriendlyName -> { StorageFaultDomain.FriendlyName -> Size } Map ###
    ##################################################################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing virtual disk information..." -Status "Step 3/4" -PercentComplete 50

    $GetVirtualDisk = Get-VirtualDisk | Sort FriendlyName

    $VirtualDiskMap = @{}

    $GetVirtualDisk | ForEach {
        # Map of PhysicalDisk.UniqueId -> Size for THIS virtual disk
        $PhysicalDiskToSizeMap = @{}
        $_ | Get-PhysicalExtent | ForEach {
            $PhysicalDiskToSizeMap[$_.PhysicalDiskUniqueId] += $_.Size
        }
        # Map of StorageFaultDomain.FriendlyName -> Size for THIS virtual disk
        $SfdToSizeMap = @{}
        $PhysicalDiskToSizeMap.keys | ForEach {
            $SfdToSizeMap[$PhysicalDiskToSfdMap[$_]] += $PhysicalDiskToSizeMap[$_]
        }
        # Store
        $VirtualDiskMap[$_.FriendlyName] = $SfdToSizeMap
    }

    #########################
    ### Step 4: Write-Out ###
    #########################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Formatting output..." -Status "Step 4/4" -PercentComplete 75

    $Output = $GetVirtualDisk | ForEach {
        $Row = [PsCustomObject]@{}

        $VirtualDiskFriendlyName = $_.FriendlyName
        $Row | Add-Member -MemberType NoteProperty "VirtualDiskFriendlyName" $VirtualDiskFriendlyName

        $TotalFootprint = $_.FootprintOnPool | ConvertTo-PrettyCapacity
        $Row | Add-Member -MemberType NoteProperty "TotalFootprint" $TotalFootprint

        $SfdList | ForEach {
            $Size = $VirtualDiskMap[$VirtualDiskFriendlyName][$_.FriendlyName] | ConvertTo-PrettyCapacity
            $Row | Add-Member -MemberType NoteProperty $_.FriendlyName $Size
        }

        $Row
    }

    # Calculate width, in characters, required to Format-Table
    $RequiredWindowWidth = ("TotalFootprint").length + 1 + ("VirtualDiskFriendlyName").length + 1
    $SfdList | ForEach {
        $RequiredWindowWidth += $_.FriendlyName.Length + 1
    }

    $ActualWindowWidth = (Get-Host).UI.RawUI.WindowSize.Width

    If (!($ActualWindowWidth)) {
        # Cannot get window width, probably ISE, Format-List
        Write-Warning "Could not determine window width. For the best experience, use a Powershell window instead of ISE"
        $Output | Format-Table
    }
    ElseIf ($ActualWindowWidth -Lt $RequiredWindowWidth) {
        # Narrower window, Format-List
        Write-Warning "For the best experience, try making your PowerShell window at least $RequiredWindowWidth characters wide. Current width is $ActualWindowWidth characters."
        $Output | Format-List
    }
    Else {
        # Wider window, Format-Table
        $Output | Format-Table
    }
}

Get-VirtualDiskFootprintByStorageFaultDomain
```
