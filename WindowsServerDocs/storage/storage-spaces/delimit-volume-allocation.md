---
title: 分隔儲存空間直接存取磁碟區的配置
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/29/2018
ms.openlocfilehash: c93cbf4ba418f6702cf8747508605952d993508d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889049"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>分隔儲存空間直接存取磁碟區的配置
> 適用於：Windows Server Insider Preview 組建 17093 和更新版本

Windows Server Insider Preview 導入了以手動方式分隔儲存空間直接存取磁碟區的配置選項。 這麼做讓可能會大幅增加容錯功能，某些狀況下，但會施加一些新增的管理考量和複雜度。 本主題說明如何運作，並提供在 PowerShell 中的範例。

   > [!IMPORTANT]
   > 這項功能是在 Windows Server Insider Preview 組建 17093 和更新版本中新功能。 您不可以使用 Windows Server 2016 中。 我們誠邀參加的 IT 專業人員[Windows Server Insider 計劃](https://aka.ms/serverinsider)提供意見反應，我們可以如何讓更適合您組織的 Windows Server 上。

## <a name="prerequisites"></a>必要條件

### <a name="green-checkmark-iconmediadelimit-volume-allocationsupportedpng-consider-using-this-option-if"></a>![綠色核取記號圖示。](media/delimit-volume-allocation/supported.png) 請考慮使用此選項，如果：

- 您的叢集有六個或多部伺服器;和
- 只會使用您的叢集[三向鏡像](storage-spaces-fault-tolerance.md#mirroring)恢復功能

### <a name="red-x-iconmediadelimit-volume-allocationunsupportedpng-do-not-use-this-option-if"></a>![紅色 X 圖示。](media/delimit-volume-allocation/unsupported.png) 請勿使用此選項，如果：

- 您的叢集有少於六部的伺服器;或
- 您的叢集使用[同位檢查](storage-spaces-fault-tolerance.md#parity)或是[鏡像加速同位檢查](storage-spaces-fault-tolerance.md#mirror-accelerated-parity)恢復功能

## <a name="understand"></a>了解

### <a name="review-regular-allocation"></a>檢閱： 一般配置

搭配規則的三向鏡像，磁碟區分成許多小型"slab 」 會複製三次，且平均分散至叢集中的每一部伺服器中的每個磁碟機。 如需詳細資訊，請參閱[這個深入探討的部落格](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/)。

![此圖表顯示磁碟區被分成三個牌堆的 slab 和平均分散至每一部伺服器。](media/delimit-volume-allocation/regular-allocation.png)

此預設配置最大化平行讀取及寫入，導致更好的效能，且吸引人的簡單： 每一部伺服器忙線中一樣，每個磁碟機已同樣滿，且所有磁碟區維持在線上或離線一起。 保證每個磁碟區為存留最多兩個的並行失敗[這些範例](storage-spaces-fault-tolerance.md#examples)說明。

不過，此配置中，磁碟區無法存活下來三個的並行失敗。 如果三部伺服器失敗，或在三部伺服器的磁碟機失敗一次，磁碟區會變成無法存取，因為至少某種 slab （具有極高的可能） 配置給確切的三個磁碟機或失敗的伺服器。

在下列範例中，1、 3 和 5 的伺服器無法在相同的時間。 雖然許多 slab 有仍正常運作的複本，但某些不：

![此圖顯示三個紅色及整體的磁碟區中反白顯示的六部伺服器是紅色。](media/delimit-volume-allocation/regular-does-not-survive.png)

磁碟區離線，且會變成無法存取，直到復原伺服器。

### <a name="new-delimited-allocation"></a>新增： 分隔配置

分隔的配置，您可以指定伺服器使用 （最小三個用於三向鏡像） 的子集。 磁碟區分為三次，像以前，而不是配置在每一部伺服器中，複製的 slab **slab 配置給您指定的伺服器子集只**。

![此圖表顯示磁碟區正在分成三個 slab 堆疊，並僅散發到三個六部伺服器。](media/delimit-volume-allocation/delimited-allocation.png)

#### <a name="advantages"></a>優點

此配置中，磁碟區是存活下來三個的並行失敗： 事實上，它的可能性求生會從 0%（與一般配置） （含分隔配置） 的 95%在此情況下 ！ 直接易懂的方式，這是因為它不需依賴伺服器 4、 5 或 6 讓它不會受到其失敗。

在上述範例中，1、 3 和 5 的伺服器無法在相同的時間。 分隔的配置，確保該伺服器 2 包含一份每個平台，每個平台有仍正常運作的複本，磁碟區保持線上且可存取：

![此圖表顯示三個以紅色反白顯示的六部伺服器的整體的磁碟區綠色。](media/delimit-volume-allocation/delimited-does-survive.png)

求生機率取決於伺服器和其他因素的數目，請參閱[分析](#analysis)如需詳細資訊。

#### <a name="disadvantages"></a>缺點

分隔的配置施加一些新增的管理考量和複雜性：

1. 用來分隔在伺服器之間平衡儲存體使用量和支持機率很高的存活，每個磁碟區的配置中所述的系統管理員負責[最佳做法](#best-practices)一節。

2. 分隔的配置，保留的對等**每一部伺服器 （有無最大值） 的一個容量磁碟機**。 這是高於[發行建議](plan-volumes.md#choosing-the-size-of-volumes)哪些超出最大值在四個容量磁碟機總計的一般配置。

3. 如果伺服器失敗，並需要更換，如中所述[移除的伺服器和其磁碟機](remove-servers.md#remove-a-server-and-its-drives)，系統管理員會負責更新受影響的磁碟區的 delimitation 藉由新增新的伺服器移除失敗的其中一個-範例下面。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

您可以使用`New-Volume`cmdlet 來建立磁碟區中儲存空間直接存取。

例如，若要建立規則的三向鏡像磁碟區：

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>建立磁碟區，並分隔其配置

若要建立三向鏡像磁碟區，並分隔其配置：

1. 先將您叢集中的伺服器指派給變數`$Servers`:

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > 在儲存空間直接存取儲存體縮放單位 ' 一詞是指附加至一部伺服器，包括直接連結的磁碟機和直接連結外部磁碟機的所有原始儲存體。 在此情況下，它是 「 伺服器 」 相同。

2. 指定要使用與新的伺服器`-StorageFaultDomainsToUse`參數並編製索引為`$Servers`。 例如，若要分隔的第一，第二，配置和第三個伺服器 （索引 0、 1 和 2）：

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2]
    ```

### <a name="see-a-delimited-allocation"></a>請參閱分隔的配置

若要查看如何*MyVolume*是配置，使用`Get-VirtualDiskFootprintBySSU.ps1`指令碼[附錄](#appendix):

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  0       0       0      
```

請注意，Server1、 Server2，以及 Server3 包含的 slab *MyVolume*。

### <a name="change-a-delimited-allocation"></a>變更分隔的配置

使用新`Add-StorageFaultDomain`和`Remove-StorageFaultDomain`cmdlet 來變更配置分隔的方式。

例如，若要移動*MyVolume*依一部伺服器：

1. 指定第四個伺服器**可以**存放區的 slab *MyVolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[3]
    ```

2. 指定第一部伺服器**無法**存放區的 slab *MyVolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. 重新平衡儲存集區，讓變更生效：

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

![此圖表顯示 slab en masse 從 1、 2 和 3 的伺服器移轉到伺服器 2、 3 和 4。](media/delimit-volume-allocation/move.gif)

您可以監視與重新平衡的進度`Get-StorageJob`。

一旦完成後，請確認*MyVolume*藉由執行已經移動`Get-VirtualDiskFootprintBySSU.ps1`一次。

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  0       0      
```

請注意，不包含 Server1 的 slab *MyVolume*不再-相反地，Server04 沒有。

## <a name="best-practices"></a>最佳作法

以下是使用分隔的磁碟區配置時要遵循的最佳作法：

### <a name="choose-three-servers"></a>選擇三部伺服器

不是更分隔到三部伺服器，每個三向鏡像磁碟區。

### <a name="balance-storage"></a>平衡儲存體

平衡每個伺服器，負責磁碟區大小配置多少儲存體。

### <a name="every-delimited-allocation-unique"></a>每個唯一的分隔的配置

若要充分利用容錯功能，請唯一的這表示它不會共用的每個磁碟區的配置*所有*與另一個磁碟區 （部分重疊也沒關係） 及其伺服器。 有 n 個伺服器，有 「 N 選擇 3 」 的唯一組合-以下是一些常見的叢集大小的意義：

| (N) 的伺服器數目 | 唯一數字分隔的 （N 選擇 3） 的配置 |
|-----------------------|-----------------------------------------------------|
| 6                     | 20                                                  |
| 8                     | 56                                                  |
| 12                    | 220                                                 |
| 16                    | 560                                                 |

   > [!TIP]
   > 請考慮這個很有幫助檢閱[combinatorics，然後選擇 標記法](https://betterexplained.com/articles/easy-permutations-and-combinations/)。

以下是最大化容錯功能的範例，因為每個磁碟區包含唯一的分隔的配置：

![unique-allocation](media/delimit-volume-allocation/unique-allocation.png)

相反地，在下一個範例中前, 三個磁碟區使用的相同分隔的配置 （1、 2 和 3 的伺服器） 和最後三個磁碟區使用的相同分隔的配置 （伺服器 4、 5 和 6）。 這不會發揮最大容錯性： 如果三部伺服器失敗時，多個磁碟區離線並變得無法存取一次。

![non-unique-allocation](media/delimit-volume-allocation/non-unique-allocation.png)

## <a name="analysis"></a>分析

本節中衍生該磁碟區會維持線上且可存取的數學機率 （或者同樣地，預期會保持線上且可存取的整體儲存體的分數） 做為失敗和叢集大小的數目。

   > [!NOTE]
   > 本節為選擇性讀取。 如果您是很希望能夠查看數學計算，請繼續閱讀 ！ 不過，如果沒有，請別擔心：[在 PowerShell 中的使用方式](#usage-in-powershell)並[最佳做法](#best-practices)是您只需要成功地實作分隔的配置。

### <a name="up-to-two-failures-is-always-okay"></a>最多兩次失敗一定是好的

每隔三向鏡像磁碟區可以存留在此同時，最多兩次失敗做[這些範例](storage-spaces-fault-tolerance.md#examples)說明這一點，不論其配置。 如果兩個磁碟機失敗，或兩部伺服器失敗，或其中一個每、 每三向鏡像磁碟區保持線上且可供存取，即使使用一般配置。

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>一半以上叢集失敗絕不會是沒問題

相反地，在極端的情況下，一半以上的伺服器或在叢集中的磁碟機，失敗[仲裁已遺失](understand-quorum.md)和每個三向鏡像磁碟區離線，而且變成無法存取，不論其配置。

### <a name="what-about-in-between"></a>呢之間？

如果三個或多個失敗就會發生一次，但至少一半的伺服器和磁碟機仍正常運作，線上且可存取的取決於哪一部伺服器的電腦失敗，就可能會保持與分隔配置的磁碟區。 讓我們執行的數字，以判斷精確的機率。

為了簡單起見，假設磁碟區是獨立，並且具有相同分散式 (IID) 根據上述的最佳作法和該足夠的唯一組合可供每個磁碟區配置必須是唯一。 任何給定的磁碟區不受影響的機率也是預期的線性的預期的情況下不受影響的整體儲存體的分數。 

指定**N**伺服器，其中**F**發生失敗，磁碟區配置給**3**其中會離線 if-和-僅限-如果所有**3**皆**F**但發生錯誤。 有 **（N 選擇 F）** 方式**F**失敗發生時，其中 **（F 選擇 3）** 導致磁碟區離線，並且變得無法存取。 機率可以表示為：

![P_offline = Fc3 / NcF](media/delimit-volume-allocation/probability-volume-offline.png)

在其他情況下，磁碟區會保持線上且可存取：

![P_online = 1 – (Fc3 / NcF)](media/delimit-volume-allocation/probability-volume-online.png)

下表會評估一些常見的叢集大小和最多 5 失敗，揭露，分隔的配置會增加容錯功能相較於一般配置中被視為每個案例的機率。

### <a name="with-6-servers"></a>與 6 的伺服器

| 配置                           | 仍正常運作的 1 個故障的機率 | 仍正常運作的 2 個故障的機率 | 仍正常運作的 3 個失敗的機率 | 仍正常運作的 4 個故障的機率 | 存留 5 失敗的機率 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 一般，分散到所有的 6 部伺服器 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| 分隔，以 3 部伺服器          | 100%                               | 100%                                | 95.0%                               | 0%                                  | 0%                                  |

   > [!NOTE]
   > 3 個以上失敗之後從 6 的伺服器總數、 叢集失去仲裁。

### <a name="with-8-servers"></a>8 的伺服器

| 配置                           | 仍正常運作的 1 個故障的機率 | 仍正常運作的 2 個故障的機率 | 仍正常運作的 3 個失敗的機率 | 仍正常運作的 4 個故障的機率 | 存留 5 失敗的機率 |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 一般，分散到 8 的所有伺服器 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| 分隔，以 3 部伺服器          | 100%                               | 100%                                | 98.2%                               | 94.3%                               | 0%                                  |

   > [!NOTE]
   > 4 個以上失敗之後超出 8 的伺服器總數、 叢集失去仲裁。

### <a name="with-12-servers"></a>使用 12 部伺服器

| 配置                            | 仍正常運作的 1 個故障的機率 | 仍正常運作的 2 個故障的機率 | 仍正常運作的 3 個失敗的機率 | 仍正常運作的 4 個故障的機率 | 存留 5 失敗的機率 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 一般，分散到所有的 12 部伺服器 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| 分隔，以 3 部伺服器           | 100%                               | 100%                                | 99.5%                               | 99.2%                               | 98.7%                               |

### <a name="with-16-servers"></a>使用 16 部伺服器

| 配置                            | 仍正常運作的 1 個故障的機率 | 仍正常運作的 2 個故障的機率 | 仍正常運作的 3 個失敗的機率 | 仍正常運作的 4 個故障的機率 | 存留 5 失敗的機率 |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| 一般，分散到所有 16 部伺服器 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| 分隔，以 3 部伺服器           | 100%                               | 100%                                | 99.8%                               | 99.8%                               | 99.8%                               |

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="can-i-delimit-some-volumes-but-not-others"></a>我可以分隔一些磁碟區，但不是會針對其他嗎？

是的。 您可以選擇每個磁碟區要分隔配置。

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>不會分隔的配置會變更磁碟機更換的運作方式？

否，它是一般的配置相同。

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
- [儲存空間直接存取中的容錯功能](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>附錄

此指令碼可協助您瞭解如何配置磁碟區。

若要使用它，如上面所述，複製/貼上及另存`Get-VirtualDiskFootprintBySSU.ps1`。

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
