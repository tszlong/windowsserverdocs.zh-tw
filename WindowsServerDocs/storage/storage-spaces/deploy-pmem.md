---
title: 瞭解和部署持續性記憶體
description: 有關什麼是持續性記憶體的詳細資訊，以及如何在 Windows Server 2019 中使用儲存空間直接存取來設定它。
keywords: 儲存空間直接存取，持續性記憶體，pmem，儲存體，S2D
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 1/27/2020
ms.localizationpriority: medium
ms.openlocfilehash: a9070d2e2ab73c7882f4b2ef585ccb01986695bb
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822311"
---
# <a name="understand-and-deploy-persistent-memory"></a>瞭解和部署持續性記憶體

> 適用于： Windows Server 2019

持續性記憶體（或 PMem）是一種新型的記憶體技術，可提供經濟實惠的大型容量和持續性的獨特組合。 本文提供有關 PMem 的背景，以及使用儲存空間直接存取在 Windows Server 2019 中部署它的步驟。

## <a name="background"></a>背景

「PMem」是一種非動態 RAM （NVDIMM-N），可透過電源週期保留其內容。 即使在發生非預期的電源中斷、使用者起始關機、系統損毀等情況下，系統電源中斷，仍然會保留記憶體內容。 此唯一特性表示您也可以使用 PMem 作為儲存體。 這就是您可能會聽到人們將 PMem 稱為「儲存類別記憶體」的原因。

若要查看其中一些優點，讓我們看看 Microsoft Ignite 2018 的下列示範。

[![Microsoft Ignite 2018 Pmem 示範](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

提供容錯的任何儲存系統都必須建立分散式複本的寫入。 這類作業必須跨越網路並擴展後端寫入流量。 基於這個理由，絕對最大的 IOPS 基準測試數位通常是藉由測量讀取來達成，特別是當儲存系統具有可盡可能從本機複本讀取的一般感知優化。 儲存空間直接存取已優化來執行此動作。

**以僅使用讀取作業來測量時，叢集會提供 13798674 IOPS。**

![13.7 m IOPS 記錄螢幕擷取畫面](media/deploy-pmem/iops-record.png)

如果您仔細觀賞影片，您會發現更多的 jaw 捨棄是延遲。 即使在超過 13.7 M IOPS，Windows 中的檔案系統仍會回報持續低於40μs 的延遲！ （這是毫秒的符號，一秒的百萬分之一秒）。這種速度比平常極光榮公告的一般全 flash 廠商快很多。

儲存空間直接存取在 Windows Server 2019 和 Intel® Optane 中，™ DC 持續性記憶體提供突破性的效能。 這項領先業界的 13.7 M IOPS 基準測試，加上可預測且非常低的延遲，而不是我們先前領先的 6.7 M IOPS 基準測試的兩倍。 另外，這次我們只需要12個伺服器節點，&mdash;25% 以上。

![IOPS 增益](media/deploy-pmem/iops-gains.png)

測試硬體是12部伺服器的叢集，已設定為使用三向鏡像和分隔的 ReFS 磁片區、 **12** x INTEL® S2600WFT、 **384 GiB**記憶體、2 x 28 核心 "CASCADELAKE"、 **1.5 TB** Intel® Optane™ DC 持續性記憶體作為快取、 **32 TB** NVME （4 x 8 TB Intel® DC P4510）作為容量， **2** x Mellanox ConnectX-4 25 Gbps。

下表顯示完整的效能數位。  

| 測                   | 效能         |
|-----------------------------|---------------------|
| 4K 100% 隨機讀取         | 13800000 IOPS   |
| 4K 90/10% 隨機讀取/寫入 | 9450000 IOPS   |
| 2 MB 連續讀取         | 549 GB/秒的輸送量 |

### <a name="supported-hardware"></a>支援的硬體

下表顯示 Windows Server 2019 和 Windows Server 2016 的支援持續性記憶體硬體。  

| 持續性記憶體技術                                      | WIN ENT LTSB 2016 Finnish 64 Bits | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| 持續模式中**的 nvdimm-n**                                  | 支援                | 支援                |
| Intel Optane 在應用程式直接模式中**™ DC 持續性記憶體**             | 不支援            | 支援                |
| **Intel Optane™ DC**在記憶體模式中的持續性記憶體 | 支援            | 支援                |

> [!NOTE]  
> Intel Optane 支援*記憶體*（volatile）和*應用程式直接*（持續）模式。
   
> [!NOTE]  
> 當您重新開機具有多個 Intel® Optane 的系統時，™應用程式直接模式中的 PMem 模組劃分成多個命名空間時，您可能會失去部分或所有相關邏輯儲存體磁片的存取權。 此問題發生在版本1903之前的 Windows Server 2019 版本上。
>   
> 因為 PMem 模組未定型，或在系統啟動時失敗，所以會發生這種存取中斷的情況。 在這種情況下，系統上任何 PMem 模組上的所有儲存命名空間都會失敗，包括未實際對應至失敗模組的命名空間。
>   
> 若要還原所有命名空間的存取權，請更換失敗的模組。
>   
> 如果 Windows Server 2019 1903 版或更新版本上的模組失敗，您就會失去實體對應至受影響模組的命名空間存取權。 其他命名空間則不會受到影響。

現在，讓我們深入瞭解如何設定持續性記憶體。

## <a name="interleaved-sets"></a>交錯集合

### <a name="understanding-interleaved-sets"></a>瞭解交錯集合

回想一下，NVDIMM-N 位於標準 DIMM （記憶體）插槽中，將資料放在較接近處理器的位置。 此設定可減少延遲並改善提取效能。 若要進一步增加輸送量，有兩個或多個 NVDIMMs 建立 n 向交錯式集合以進行等量讀取/寫入作業。 最常見的設定是雙向或四向交錯。 交錯式集合也會讓多個持續性記憶體裝置顯示為 Windows Server 的單一邏輯磁片。 您可以使用 Windows PowerShell **PmemDisk** Cmdlet 來檢查這類邏輯磁片的設定，如下所示：

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

我們可以看到邏輯 PMem 磁片 #2 使用實體裝置 Id20 和 Id120，而邏輯 PMem 磁片 #3 使用實體裝置 Id1020 和 Id1120。  

若要取出邏輯磁碟機所使用之交錯集合的進一步資訊，請執行**PmemPhysicalDevice** Cmdlet：

```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleaved-sets"></a>設定交錯集合

若要設定交錯集，請從查看系統上未指派給邏輯 PMem 磁片的所有持續性記憶體區域開始。 若要這麼做，請執行下列 PowerShell Cmdlet：

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

若要查看系統中的所有 PMem 裝置資訊，包括裝置類型、位置、健康情況和操作狀態等等，請在本機伺服器上執行下列 Cmdlet：

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

因為我們有可用的未使用 PMem 區域，所以我們可以建立新的持續性記憶體磁片。 我們可以藉由執行下列 Cmdlet 來使用未使用的區域建立多個持續性記憶體磁片：

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

完成之後，我們可以執行下列程式來查看結果：

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

值得注意的是，我們可以執行**PhysicalDisk |其中的媒體型-Eq SCM**而不是**PmemDisk** ，以取得相同的結果。 新建立的 PMem 磁片會與出現在 PowerShell 和 Windows 系統管理中心中的磁片磁碟機相對應一對一。

### <a name="using-persistent-memory-for-cache-or-capacity"></a>針對快取或容量使用持續性記憶體

Windows Server 2019 上的儲存空間直接存取支援使用持續性記憶體作為快取或容量磁片磁碟機。 如需有關如何設定快取和容量磁片磁碟機的詳細資訊，請參閱[瞭解儲存空間直接存取中的](understand-the-cache.md)快取。

## <a name="creating-a-dax-volume"></a>建立 DAX 磁片區

### <a name="understanding-dax"></a>瞭解 DAX

有兩種方法可存取持續性記憶體。 這些類型包括：

1. **直接存取（DAX）** ，其運作方式就像記憶體，以取得最低延遲。 應用程式會直接修改持續性記憶體，略過堆疊。 請注意，您只能搭配使用 DAX 與 NTFS。
1. **封鎖存取**，其運作方式類似于應用程式相容性的儲存體。 在此組態中，資料會流經堆疊。 您可以搭配使用此設定與 NTFS 和 ReFS。

下圖顯示 DAX 設定的範例：

![DAX 堆疊](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>設定 DAX

我們必須使用 PowerShell Cmdlet，在持續性記憶體磁片上建立 DAX 量。 藉由使用 **-IsDax**參數，我們可以將磁片區格式化為啟用 DAX。

```PowerShell
Format-Volume -IsDax:$true
```

下列程式碼片段可協助您在持續性記憶體磁片上建立 DAX 卷。

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

- 持續性記憶體不會建立實體磁片效能計數器，因此您不會看到它出現在 Windows 系統管理中心的圖表上。
- 持續性記憶體不會建立 Storport 505 資料，因此您將不會收到主動式的極端情況偵測。

除此之外，監視體驗與其他任何實體磁片一樣。 您可以執行下列 Cmdlet 來查詢持續性記憶體磁片的健全狀況：

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

**HealthStatus**顯示 PMem 磁片是否狀況良好。  

**UnsafeshutdownCount**值會追蹤可能造成此邏輯磁片遺失資料的關機次數。 這是此磁片的所有基礎 PMem 裝置之不安全關閉計數的總和。 如需健全狀況狀態的詳細資訊，請使用**PmemPhysicalDevice** Cmdlet 來尋找資訊，例如**OperationalStatus**。

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

此 Cmdlet 會顯示哪一個持續性記憶體裝置狀況不良。 狀況不良的裝置（**DeviceId** 20）符合上一個範例中的案例。 BIOS 中的**PhysicalLocation**有助於識別哪一個持續性記憶體裝置處於錯誤狀態。

## <a name="replacing-persistent-memory"></a>取代持續性記憶體

本文說明如何查看持續性記憶體的健全狀況狀態。 如果您必須更換失敗的模組，您必須重新布建 PMem 磁片（請參閱先前所述的步驟）。

當您進行疑難排解時，您可能必須使用**PmemDisk**。 此 Cmdlet 會移除特定的持續性記憶體磁片。 我們可以藉由執行下列 Cmdlet 來移除所有目前的 PMem 磁片：

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

> [!IMPORTANT]  
> 移除持續性記憶體磁片會導致該磁片上的資料遺失。

您可能需要的另一個 Cmdlet 是**PmemPhysicalDevice**。 此 Cmdlet 會初始化實體持續性記憶體裝置上的標籤存放區，並可清除在 PMem 裝置上損毀的標籤儲存資訊。

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

> [!IMPORTANT]  
> **PmemPhysicalDevice**會導致持續性記憶體中的資料遺失。 使用它做為修正持續性記憶體相關問題的最後手段。

## <a name="see-also"></a>請參閱

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
- [Windows 中的存放裝置類別記憶體（NVDIMM-N）健全狀況管理](storage-class-memory-health.md)
- [了解快取](understand-the-cache.md)
