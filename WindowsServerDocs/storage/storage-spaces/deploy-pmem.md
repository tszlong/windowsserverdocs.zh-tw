---
title: 瞭解和部署持續性記憶體
description: 有關什麼是持續性記憶體的詳細資訊，以及如何在 Windows Server 2019 中使用儲存空間直接存取來設定它。
keywords: 儲存空間直接存取，持續性記憶體，pmem，儲存體，S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 497fa201c500919fc857d25166d37ce87613d0f0
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872014"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>瞭解和部署持續性記憶體

>適用於：Windows Server Standard 2012 R2

持續性記憶體（或 PMem）是一種新型的記憶體技術，可提供經濟實惠的大型容量和持續性的獨特組合。 本主題提供有關 PMem 的背景，以及使用儲存空間直接存取將其部署到 Windows Server 2019 的步驟。

## <a name="background"></a>背景

「PMem」是一種非變動性 DRAM （NVDIMM-N），其具有 DRAM 的速度，但會透過電源週期保留其記憶體內容（即使系統電源在發生非預期的電源中斷、使用者起始關機、系統損毀、以此類推）。 因此，從停止的地方繼續會大幅加快，因為您的 RAM 內容不需要重載。 另一個唯一的特性是，PMem 是可定址的 byte，這表示您也可以使用它做為儲存體（這就是您可能會聽到將 PMem 稱為儲存類別記憶體的原因）。


為了查看其中一些優點，讓我們來看一下 Microsoft Ignite 2018 的示範：

[![Microsoft Ignite 2018 Pmem 示範](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

任何可提供容錯功能的儲存系統，都必須進行寫入的分散式複本，而這項功能必須跨越網路並產生後端寫入放大。 基於這個理由，絕對最大的 IOPS 基準測試數位通常只會以讀取來達成，特別是當儲存系統有一般的優化可在可能的情況下讀取本機複本時，儲存空間直接存取執行此工作。

**使用 100% 的讀取時，叢集會提供 13798674 IOPS。**

![13.7 m IOPS 記錄螢幕擷取畫面](media/deploy-pmem/iops-record.png)

如果您密切觀賞影片，就會發現 thatwhat 的 jaw 下降就是延遲：即使超過 13.7 M IOPS，Windows 中的檔案系統也會回報持續低於40μs 的延遲！ （這是毫秒的符號，一秒的百萬分之一秒）。這種程度的順序比一般的全部 flash 廠商立即極光榮公告的速度更快。

儲存空間直接存取在 Windows Server 2019 和 Intel® Optane 中，™ DC 持續性記憶體提供突破性的效能。 這項領先業界的 13.7 M IOPS 基準測試（具有可預測且非常低的延遲），比我們先前領先業界的 6.7 M IOPS 基準的倍。 還有一點，這次我們只需要12個伺服器節點，25% 不到兩年前。

![IOPS 增益](media/deploy-pmem/iops-gains.png)

使用的硬體是使用三向鏡像和分隔 ReFS 磁片區的12部伺服器叢集、 **12** x INTEL® S2600WFT、 **384 GiB**記憶體、2 x 28-核心 "CASCADELAKE"、 **1.5 TB** Intel® Optane™ DC 持續性記憶體作為快取、 **32 TB** NVMe （4 x 8TB Intel® DC P4510）作為容量， **2** x Mellanox ConnectX-4 25 Gbps

下表具有完整的效能數位： 

| 測                   | 效能         |
|-----------------------------|---------------------|
| 4K 100% 隨機讀取         | 13800000 IOPS   |
| 4K 90/10% 隨機讀取/寫入 | 9450000 IOPS   |
| 2 MB 順序讀取         | 549 GB/秒的輸送量 |

### <a name="supported-hardware"></a>支援的硬體

下表顯示 Windows Server 2019 和 Windows Server 2016 的支援持續性記憶體硬體。 請注意，Intel Optane 特別支援記憶體模式和應用程式直接模式。 Windows Server 2019 支援混合模式作業。

| 持續性記憶體技術                                      | Windows Server 2016 | Windows Server Standard 2012 R2 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| 應用程式直接模式中的**nvdimm-n**                                       | 支援                | 支援                |
| Intel Optane 在應用程式直接模式中**™ DC 持續性記憶體**             | 不支援            | 支援                |
| Intel Optane 在兩層記憶體模式中**™ DC 持續性記憶體**（2LM） | 不支援            | 支援                |

現在，讓我們深入瞭解如何設定持續性記憶體。

## <a name="interleave-sets"></a>交錯集合

### <a name="understanding-interleave-sets"></a>瞭解交錯集

回想一下，NVDIMM-N 位於標準 DIMM （記憶體）插槽中，將資料放在較接近處理器的位置（因此可減少延遲並取得較佳的效能）。 為此，您可以在兩個或多個 NVDIMMs 建立 N 向交錯，以提供更高輸送量的條紋讀取/寫入作業時，進行交錯設定。 最常見的方式是雙向或4向交錯。

交錯式集合通常可以在平臺的 BIOS 中建立，讓多個持續性記憶體裝置顯示為 Windows Server 的單一邏輯磁片。 每個持續性記憶體邏輯磁片都包含一組交錯的實體裝置，方法是執行：

```PowerShell
Get-PmemDisk
```

範例輸出如下所示：

```
DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

我們可以看到邏輯 pmem 磁片 #2 具有 Id20 和 Id120 的實體裝置，而邏輯 pmem 磁片 #3 具有 Id1020 和 Id1120 的實體裝置。 我們也可以將特定的 pmem 磁片饋送至 PmemPhysicalDevice，以取得其在交錯設定中的所有實體 NVDIMMs，如下所示。


```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice
```

範例輸出如下所示：

```
DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleave-sets"></a>設定交錯集合

若要設定交錯集，請執行下列 PowerShell Cmdlet：

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

這會顯示未指派給系統上邏輯持續性記憶體磁片的所有持續性記憶體區域。

若要查看系統中所有持續性記憶體裝置的資訊，包括裝置類型、位置、健康情況和操作狀態等。您可以在本機伺服器上執行下列 Cmdlet：

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile
                                                                                                                      memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

因為我們有可用的 pmem 區域，所以我們可以建立新的持續性記憶體磁片。 我們可以使用未使用的區域建立多個持續性記憶體磁片，方法是：

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

啟動完成後，我們可以執行下列程式來查看結果：

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

值得注意的是，我們可以執行**PhysicalDisk |其中的媒體型-Eq SCM**而不是**PmemDisk** ，以取得相同的結果。 新建立的持續性記憶體磁片會將1:1 對應到出現在 PowerShell 和 Windows 系統管理中心的磁片磁碟機。

### <a name="using-persistent-memory-for-cache-or-capacity"></a>針對快取或容量使用持續性記憶體

Windows Server 2019 上的儲存空間直接存取支援使用持續性記憶體作為快取或容量磁片磁碟機。 如需設定快取和容量磁片磁碟機的詳細資訊，請參閱此[檔](understand-the-cache.md)。

## <a name="creating-a-dax-volume"></a>建立 DAX 磁片區

### <a name="understanding-dax"></a>瞭解 DAX

有兩種方法可存取持續性記憶體。 其中包括：

1. **直接存取（DAX）** ，其運作方式就像記憶體，以取得最低延遲。 應用程式會直接修改持續性記憶體，略過堆疊。 請注意，這只能與 NTFS 搭配使用。
2. **封鎖存取**，其運作方式類似于應用程式相容性的儲存體。 這項設定中的資料流程會流經堆疊，而這可與 NTFS 和 ReFS 搭配使用。

這種情況的範例如下所示：

![DAX 堆疊](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>設定 DAX

我們必須使用 PowerShell Cmdlet，在持續性記憶體上建立 DAX 磁片區。 藉由使用 **-IsDax**參數，我們可以將磁片區格式化為啟用 DAX。

```PowerShell
Format-Volume -IsDax:$true
```

下列程式碼片段將協助您在持續性記憶體磁片上建立 DAX 卷。

```PowerShell
# Here we use the first pmem disk to create the volume as an example
$disk = (Get-PmemDisk)[0] | Get-PhysicalDisk | Get-Disk
# Initialize the disk to GPT if it is not initialized
If ($disk.partitionstyle -eq "RAW") {$disk | Initialize-Disk -PartitionStyle GPT}
# Create a partition with drive letter 'S' (can use any available drive letter)
$disk | New-Partition -DriveLetter S -UseMaximumSize


   DiskPath: \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{53f56307-b6
bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                               Size Type
---------------  ----------- ------                                               ---- ----
2                S           16777216                                        251.98 GB Basic

# Format the volume with drive letter 'S' to DAX Volume
Format-Volume -FileSystem NTFS -IsDax:$true -DriveLetter S

DriveLetter FriendlyName FileSystemType DriveType HealthStatus OperationalStatus SizeRemaining      Size
----------- ------------ -------------- --------- ------------ ----------------- -------------      ----
S                        NTFS           Fixed     Healthy      OK                    251.91 GB 251.98 GB

# Verify the volume is DAX enabled
Get-Partition -DriveLetter S | fl


UniqueId             : {00000000-0000-0000-0000-000100000000}SCMLD\VEN_8980&DEV_097A&SUBSYS_89804151&REV_0018\3&1B1819F6&0&03018089F
                       B63494DB728D8418B3CBBF549997891:WIN-8KGI228ULGA
AccessPaths          : {S:\, \\?\Volume{cf468ffa-ae17-4139-a575-717547d4df09}\}
DiskNumber           : 2
DiskPath             : \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{5
                       3f56307-b6bf-11d0-94f2-00a0c91efb8b}
DriveLetter          : S
Guid                 : {cf468ffa-ae17-4139-a575-717547d4df09}
IsActive             : False
IsBoot               : False
IsHidden             : False
IsOffline            : False
IsReadOnly           : False
IsShadowCopy         : False
IsDAX                : True                   # <- True: DAX enabled
IsSystem             : False
NoDefaultDriveLetter : False
Offset               : 16777216
OperationalStatus    : Online
PartitionNumber      : 2
Size                 : 251.98 GB
Type                 : Basic
```

## <a name="monitoring-health"></a>監視健全狀況

當您使用持續性記憶體時，監視體驗會有一些差異：

1. 持續性記憶體不會建立實體磁片效能計數器，因此您不會看到是否出現在 Windows 系統管理中心的圖表上。
2. 持續性記憶體不會建立 Storport 505 資料，因此您將不會收到主動式的極端情況偵測。

除此之外，監視體驗與其他任何實體磁片一樣。 您可以執行下列動作來查詢持續性記憶體磁片的健全狀況：

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Unhealthy    None          True         {20, 120}         2
3          252 GB Healthy      None          True         {1020, 1120}      0

Get-PmemDisk | Get-PhysicalDisk | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails

SerialNumber               HealthStatus OperationalStatus  OperationalDetails
------------               ------------ ------------------ ------------------
802c-01-1602-117cb5fc      Healthy      OK
802c-01-1602-117cb64f      Warning      Predictive Failure {Threshold Exceeded,NVDIMM_N Error}
```

**HealthStatus**會顯示持續性記憶體磁片是否狀況良好。 **UnsafeshutdownCount**會追蹤可能造成此邏輯磁片遺失資料的關機次數。 這是此磁片的所有基礎持續性記憶體裝置之不安全關閉計數的總和。 我們也可以使用下列命令來查詢健康狀態資訊。 **OperationalStatus**和**OperationalDetails**會提供有關健全狀況狀態的詳細資訊。

若要查詢持續性記憶體裝置的健全狀況：

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

這會顯示哪一個持續性記憶體裝置狀況不良。 狀況不良的裝置（**DeviceId**）20符合上述範例中的案例。 從 BIOS **PhysicalLocation**有助於識別哪一個持續性記憶體裝置處於錯誤狀態。

## <a name="replacing-persistent-memory"></a>取代持續性記憶體

在上方，我們已說明如何查看持續性記憶體的健全狀況狀態。 如果您需要更換失敗的模組，您將需要重新布建持續性記憶體磁片（請參閱上面概述的步驟）。

進行疑難排解時，您可能需要使用**移除-PmemDisk**，這會移除特定的持續性記憶體磁片。 我們可以藉由下列方式移除所有目前的持續性磁片：

```PowerShell
Get-PmemDisk | Remove-PmemDisk

cmdlet Remove-PmemDisk at command pipeline position 1
Supply values for the following parameters:
DiskNumber: 2

This will remove the persistent memory disk(s) from the system and will result in data loss.
Remove the persistent memory disk(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
Removing the persistent memory disk. This may take a few moments.
```

請務必注意，移除持續性記憶體磁片會導致該磁片上的資料遺失。

另一個您可能需要的 Cmdlet 是**PmemPhysicalDevice**，它會初始化實體持續性記憶體裝置上的標籤儲存區域。 這可以用來清除持續性記憶體裝置上損毀的標籤儲存資訊。

```PowerShell
Get-PmemPhysicalDevice | Initialize-PmemPhysicalDevice

This will initialize the label storage area on the physical persistent memory device(s) and will result in data loss.
Initializes the physical persistent memory device(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): A
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
```

請務必注意，此命令應該用來做為修正持續性記憶體相關問題的最後手段。 這會導致持續性記憶體的資料遺失。

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
- [Windows 中的存放裝置類別記憶體（NVDIMM-N）健全狀況管理](storage-class-memory-health.md)
- [了解快取](understand-the-cache.md)