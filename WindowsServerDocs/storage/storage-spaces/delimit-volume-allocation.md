---
description: 深入瞭解：分隔儲存空間直接存取中的磁片區配置
title: 在儲存空間直接存取中分隔磁片區的配置
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.date: 03/29/2018
ms.openlocfilehash: b0d036b08ab3c538a134cdbc294c2208d8d547aa
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041406"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>在儲存空間直接存取中分隔磁片區的配置
> 適用於：Windows Server 2019

Windows Server 2019 引進了在儲存空間直接存取中手動分隔磁片區配置的選項。 這樣做可能會在特定情況下大幅提高容錯，但會強加一些新增的管理考慮和複雜度。 本主題說明其運作方式，並在 PowerShell 中提供範例。

   > [!IMPORTANT]
   > 這項功能是 Windows Server 2019 中的新功能。 它無法在 Windows Server 2016 中使用。

## <a name="prerequisites"></a>必要條件

### <a name="green-checkmark-icon-consider-using-this-option-if"></a>![綠色核取記號圖示。](media/delimit-volume-allocation/supported.png) 如果有下列情況，請考慮使用此選項：

- 您的叢集有六部以上的伺服器;和
- 您的叢集只使用 [三向鏡像](storage-spaces-fault-tolerance.md#mirroring) 復原

### <a name="red-x-icon-do-not-use-this-option-if"></a>![紅色 X 圖示。](media/delimit-volume-allocation/unsupported.png) 如果有下列情況，請勿使用此選項：

- 您的叢集有少於六部伺服器;或
- 您 [的叢集使用](storage-spaces-fault-tolerance.md#parity) 同 [位或鏡像加速同位](storage-spaces-fault-tolerance.md#mirror-accelerated-parity) 復原

## <a name="understand"></a>了解

### <a name="review-regular-allocation"></a>審核：一般配置

使用一般的三向鏡像時，磁片區會分割成許多較小的「slab」，並複製三次，並平均分散到叢集中每部伺服器的每個磁片磁碟機。 如需詳細資訊，請閱讀 [這篇深入探討的 blog](https://techcommunity.microsoft.com/t5/storage-at-microsoft/deep-dive-the-storage-pool-in-storage-spaces-direct/ba-p/425959)。

![此圖顯示將磁片區分割成三個 slab 堆疊，並平均分散在每部伺服器上。](media/delimit-volume-allocation/regular-allocation.png)

此預設配置可將平行讀取和寫入最大化，進而提升效能，並在簡化時吸引人：每部伺服器都同樣忙碌、每個磁片磁碟機都同樣地滿，且所有磁片區都保持上線或離線。 每個磁片區保證最多可承受兩個並行失敗，如 [下列範例](storage-spaces-fault-tolerance.md#examples) 所示。

不過，使用此配置時，磁片區無法存活三個並行失敗。 如果三部伺服器一次失敗，或三部伺服器中的磁片磁碟機一次失敗，磁片區就會變成無法存取，因為至少有一些 (slab 的機率很高，) 配置給第三個磁片磁碟機或失敗的伺服器。

在下列範例中，伺服器1、3和5會同時失敗。 雖然許多 slab 都有存活的複本，但有些則不會：

![顯示六部伺服器中以紅色醒目提示的三部伺服器的圖表，且整體磁片區為紅色。](media/delimit-volume-allocation/regular-does-not-survive.png)

磁片區會離線且無法存取，直到伺服器復原為止。

### <a name="new-delimited-allocation"></a>新增：分隔配置

使用分隔配置，您可以指定要使用 (最少四個) 的伺服器子集。 磁片區會分割成複製三次的 slab （如先前所示），但不會在每一部伺服器上配置，而是 **只會將 slab 配置給您指定的伺服器子集**。

例如，如果您有8個節點叢集 (節點1到 8) ，您可以將磁片區指定為只能在節點1、2、3、4中的磁片上找到。
#### <a name="advantages"></a>優點

使用範例配置時，磁片區可能會存活三個並行失敗。 如果節點1、2和6停止運作，則只有2個節點會保留磁片區的3個數據複本，而磁片區會保持在線上。

生存機率取決於伺服器數目和其他因素-請參閱 [分析](#analysis) 以取得詳細資料。

#### <a name="disadvantages"></a>缺點

分隔配置會有一些新增的管理考慮和複雜性：

1. 系統管理員必須負責分隔每個磁片區的配置，以平衡伺服器之間的儲存使用率，並保持存活的高機率，如 [最佳作法](#best-practices) 一節中所述。

2. 使用分隔配置時，請為 **每個伺服器 (保留同一個容量磁片磁碟機，而沒有最大)**。 這不是一般配置的 [發佈建議](plan-volumes.md#choosing-the-size-of-volumes) ，而是到超出在四個容量磁片磁碟機總計。

3. 如果伺服器失敗且需要被取代（如 [移除伺服器及其磁片磁碟機](remove-servers.md#remove-a-server-and-its-drives)中所述），系統管理員必須藉由新增新的伺服器並移除失敗的單一範例（以下範例），負責更新受影響磁片區的 delimitation。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

您可以使用 `New-Volume` Cmdlet，在儲存空間直接存取中建立磁片區。

例如，若要建立一般的三向鏡像磁片區：

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>建立磁片區並將其配置加以分隔

若要建立三向鏡像磁片區並分隔其配置：

1. 首先，將叢集中的伺服器指派給變數 `$Servers` ：

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > 在儲存空間直接存取中，「存放裝置縮放單位」一詞是指連接到一部伺服器的所有原始儲存體，包括直接連接的磁片磁碟機，以及具有磁片磁碟機的直接連結的外部主機殼。 在此內容中，它與「伺服器」相同。

2. 指定要搭配新參數使用的伺服器 `-StorageFaultDomainsToUse` ，以及將加入的 `$Servers` 索引。 例如，若要將配置分割為第一個、第二、第三和第四部伺服器， (索引0、1、2和 3) ：

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2,3]
    ```

### <a name="see-a-delimited-allocation"></a>查看分隔的配置

若要查看 *MyVolume* 的配置方式，請使用 `Get-VirtualDiskFootprintBySSU.ps1` [附錄](#appendix)中的腳本：

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  100 GB  0       0
```

請注意，只有 Server1、Server2、Server3 和 Server4 包含 *MyVolume* 的 slab。

### <a name="change-a-delimited-allocation"></a>變更分隔配置

使用新的 `Add-StorageFaultDomain` 和 `Remove-StorageFaultDomain` Cmdlet 來變更配置的分隔方式。

例如，若要將 *MyVolume* 移到一部伺服器上：

1. 指定第五部伺服器 **可以** 儲存 *MyVolume* 的 slab：

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[4]
    ```

2. 指定第一部伺服器 **無法** 儲存 *MyVolume* 的 slab：

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. 重新平衡儲存集區，變更才會生效：

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

您可以使用監視重新平衡的進度 `Get-StorageJob` 。

完成之後，請再次執行，以確認 *MyVolume* 已移動 `Get-VirtualDiskFootprintBySSU.ps1` 。

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  100 GB  0
```

請注意，Server1 不再包含 *MyVolume* 的 slab，而是 Server5。

## <a name="best-practices"></a>最佳作法

以下是使用分隔磁片區配置時要遵循的最佳作法：

### <a name="choose-four-servers"></a>選擇四部伺服器

將每個三向鏡像磁片區分隔成四部伺服器，而不是其他。

### <a name="balance-storage"></a>平衡儲存體

平衡配置給每部伺服器的儲存體量、磁片區大小的帳戶處理量。

### <a name="stagger-delimited-allocation-volumes"></a>錯開分隔的配置磁片區

為了達到最大容錯，請讓每個磁片區的配置變成唯一的，這表示它不會與另一個磁片區共用其 *所有* 伺服器 (部分重迭) 。

例如，在八個節點的系統上：磁片區1：伺服器1、2、3、4磁片區2：伺服器5、6、7、8磁片區3：伺服器3、4、5、6磁片區4：伺服器1、2、7、8

## <a name="analysis"></a>分析

本節會衍生出磁片區維持在線上且可存取 (或相同的數學機率，也就是在線上且可存取的整體儲存空間的預期比例，) 為失敗數目和叢集大小的功能。

   > [!NOTE]
   > 此區段是選擇性讀取。 如果您想要查看數學，請繼續閱讀！ 但如果沒有，請別擔心： [PowerShell 中的使用方式](#usage-in-powershell) 和 [最佳作法](#best-practices) ，都只需要成功執行分隔配置。

### <a name="up-to-two-failures-is-always-okay"></a>最多有兩個失敗永遠都沒問題

無論配置為何，每個三向鏡像磁片區最多可存留兩次失敗。 如果有兩個磁片磁碟機故障，或兩部伺服器故障，或其中一個伺服器失敗，則每個三向鏡像磁片區都會保持連線且可供存取，即使是一般配置。

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>叢集故障的一半不會有一半

相反地，如果叢集中有一半以上的伺服器或磁片磁碟機故障， [仲裁就會遺失](understand-quorum.md) ，而且每個三向鏡像磁片區都會離線且無法存取，不論其配置為何。

### <a name="what-about-in-between"></a>兩者之間有何差異？

如果發生三個或多個失敗，但至少有一半的伺服器和磁片磁碟機仍在運作中，則具有分隔配置的磁片區可能會保持連線並可供存取，這取決於哪些伺服器故障。

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="can-i-delimit-some-volumes-but-not-others"></a>我可以分隔某些磁片區，而不是其他磁片區嗎？

是。 您可以選擇是否要分隔配置的每個磁片區。

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>分隔配置變更磁片磁碟機更換的運作方式為何？

否，與一般配置相同。

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
- [儲存空間直接存取中的容錯](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>附錄

此腳本可協助您查看磁片區的配置方式。

如上面所述使用它，複製/貼上並另存新檔 `Get-VirtualDiskFootprintBySSU.ps1` 。

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
