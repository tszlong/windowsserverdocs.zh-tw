---
title: 了解和部署持續性記憶體
description: 哪些持續性記憶體都會，以及如何設定與儲存空間直接存取 Windows Server 2019 的詳細的資訊。
keywords: 儲存空間直接存取，持續性記憶體、 pmem、 儲存體，S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: ed4b2669ad35a2ce0f818c65f7024ce905d9e4d6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280040"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>了解和部署持續性記憶體

>適用於：Windows Server 2019

持續性記憶體 （或 PMem） 是一種全新的記憶體內部技術，可提供經濟實惠的大容量和持續性的唯一組合。 本主題提供 PMem 和步驟，將其部署與使用儲存空間直接存取的 Windows Server 2019 的背景。

## <a name="background"></a>背景

PMem 是 DRAM 的類型的靜態 DRAM (NVDIMM-N) 速度，但會保留其記憶體內容會流過電源循環 （記憶體內容維持系統電源中斷時，即使發生非預期的電源中斷、 使用者起始關機、 系統損壞時，等）。 因為這個緣故，從您先前離開的地方繼續獲得大幅提升，因為您的 RAM 的內容並不需要重新載入。 另一個唯一的特性是 PMem 位元組可定址的這表示您也可以使用它做為儲存體 （這就是為什麼您可能會聽到 PMem 稱為儲存類別記憶體）。


若要查看這些好處，讓我們看看這個來自 Microsoft Ignite 2018 的示範：

[![Microsoft Ignite 2018 Pmem 示範](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

任何儲存系統，它一定會提供容錯移轉可讓分散式的寫入，它必須周遊網路，並會造成後端寫入放大複本。 基於這個理由，絕對最大 IOPS 基準測試數字是通常透過讀取，特別是在儲存體系統具有一般常識最佳化，以讀取本機複本，可能的話，哪一個儲存空間直接存取會。

**100%的讀取，叢集中提供 13,798,674 IOPS。**

![13.7 m IOPS 記錄的螢幕擷取畫面](media/deploy-pmem/iops-record.png)

如果您仔細觀看影片，您會發現 thatwhat 的甚至更瞠目結舌時的延遲： 即使在超過 13.7 M IOPS，Windows 中的檔案系統報告延遲是一致的方式少於 40 個 µs ！ （如毫秒、 百萬分之一秒的是符號）。這會比什麼一般的全快閃廠商非常驕傲地通告立即快重要性排序。

在一起，儲存空間直接存取 Windows Server 2019 和 Intel® Optane™ DC 的持續性記憶體中時，提供突破性的效能。 這個領先業界 HCI 基準測試超過 13.7 M IOPS 的可預測且極低的延遲，是多個雙精確度我們先前領先業界的效能評定，6.7 M IOPS。 更甚者，這次我們需要只 12 的伺服器節點，少於兩年前的 25%。

![IOPS 提升](media/deploy-pmem/iops-gains.png)

已使用三向鏡像的 12 伺服器叢集所使用的硬體，並分隔 ReFS 磁碟區， **12** x Intel® S2600WFT， **384 GiB**記憶體、 核心 2 x 28"CascadeLake" **1.5 TB**Intel® Optane™ DC 的持續性記憶體快取，作為**32 TB** NVMe (4 x 8 TB Intel® DC P4510) 容量，為**2** x Mellanox ConnectX 4 25 Gbps

下表提供完整的效能數字： 

| 基準測試                   | 效能         |
|-----------------------------|---------------------|
| 4k 100%隨機讀取         | 13.8 百萬 IOPS   |
| 4 K 90 / 10%隨機讀取/寫入 | 9.45 百萬 IOPS   |
| 2 MB 的循序讀取         | 549 GB/秒的輸送量 |

### <a name="supported-hardware"></a>支援的硬體

下表顯示支援的持續性記憶體硬體的 Windows Server 2019 和 Windows Server 2016。 請注意 Intel Optane 特別支援記憶體內部模式和應用程式直接模式。 Windows Server 2019 支援混合模式作業。

| 持續性記憶體內部技術                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N**應用程式直接模式                                       | 支援                | 支援                |
| **Intel Optane™ DC 持續性記憶體**應用程式直接模式             | 不支援            | 支援                |
| **Intel Optane™ DC 持續性記憶體**兩個層級記憶體模式 (2LM) | 不支援            | 支援                |

現在，讓我們深入了解您如何設定持續性記憶體。

## <a name="interleave-sets"></a>交錯集

### <a name="understanding-interleave-sets"></a>了解交錯集

您應該記得，NVDIMM-N 位於標準的 DIMM （記憶體） 位置，將資料更接近處理器 （因此，減少延遲和擷取更佳的效能）。 若要建置在此，間隔設定時，兩個或多個 NVDIMMs 建立集合，以提供更高的輸送量的等量分散讀取/寫入作業 N 方間隔。 最常見的安裝程式會向 2 或 4 向交錯。

交錯式的集合通常可以讓多個持續性記憶體裝置顯示為單一邏輯磁碟到 Windows Server 平台的 BIOS 中建立。 每個持續性記憶體邏輯磁碟包含實體裝置的交錯式的集合執行：

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

我們可以看到邏輯 pmem 磁碟 #2 有沒有 Id20 和 Id120 的實體裝置，邏輯 pmem 磁碟 #3 已 Id1020 和 Id1120 的實體裝置。 我們也可以取得所有其實體 NVDIMMs 設定，如下所示的間隔中的 Get PmemPhysicalDevice 摘要特定 pmem 磁碟。


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

### <a name="configuring-interleave-sets"></a>設定交錯集

若要設定的間隔設定，執行下列 PowerShell cmdlet:

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

這會顯示所有未指派至系統上的邏輯的持續性記憶體磁碟的持續性記憶體區域。

若要查看所有持續性記憶體裝置資訊在系統中，包括裝置類型、 位置、 健康情況和操作狀態等您可以執行下列 cmdlet 在本機伺服器上：

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

由於我們有可用的未使用的 pmem 區域時，我們可以建立新的持續性記憶體磁碟。 我們可以建立多個持續性記憶體磁碟使用的未使用的區域：

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

這麼做之後，我們可以看到結果，藉由執行：

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

值得注意的是我們可能已執行**Get-physicaldisk |其中 MediaType-Eq SCM**而非**Get PmemDisk**來取得相同的結果。 新建立的持續性記憶體磁碟對應至 PowerShell 和 Windows Admin Center 中顯示的磁碟機的 1:1。

### <a name="using-persistent-memory-for-cache-or-capacity"></a>使用持續性記憶體快取或容量

儲存空間直接存取於 Windows Server 2019 支援使用持續性記憶體為快取或容量磁碟機。 請參閱此[文件](understand-the-cache.md)如需有關設定快取和容量的磁碟機的詳細資訊。

## <a name="creating-a-dax-volume"></a>建立 DAX 磁碟區

### <a name="understanding-dax"></a>了解 DAX

有兩種方法來存取持續性記憶體。 其中包括：

1. **直接存取 (DAX)** ，這在像是記憶體內部來達到最低的延遲。 應用程式直接修改持續性記憶體，並略過的堆疊。 請注意，這僅適用於使用 NTFS。
2. **封鎖存取**，這在像是應用程式相容性的儲存體。 使資料流過這項設定中的堆疊，這可與 NTFS 和 ReFS。

這個範例可以如下所示：

![DAX 堆疊](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>設定 DAX

我們必須使用 PowerShell cmdlet 來建立持續性記憶體 DAX 磁碟區。 藉由使用 **-IsDax**交換器中，我們可以格式化要啟用的 DAX 磁碟區。

```PowerShell
Format-Volume -IsDax:$true
```

下列程式碼將協助您建立 DAX 磁碟區在持續性記憶體磁碟上。

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

當您使用持續性記憶體時，有幾項差異的監視體驗：

1. 持續性記憶體不會建立效能計數器，因此如果您不會看到 Windows Admin Center 中的圖表中顯示的實體磁碟。
2. 持續性記憶體並不會建立 Storport 505 資料，因此您無法取得主動式的極端值偵測。

除了，體驗監視功能相當於任何其他實體磁碟。 您可以藉由執行查詢的持續性記憶體磁碟的健全狀況：

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

**HealthStatus**顯示持續性記憶體磁碟是否狀況良好。 **UnsafeshutdownCount**追蹤可能會導致資料遺失，此邏輯磁碟的關機的數目。 它是所有此磁碟的基礎持續性記憶體裝置的不安全的關機計數的總和。 我們也可以使用下列命令來查詢健全狀況資訊。 **OperationalStatus**並**OperationalDetails**提供健全狀況狀態的詳細資訊。

若要查詢的持續性記憶體裝置的健全狀況：

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

這會顯示哪一個持續性記憶體裝置狀況不良。 狀況不良的裝置 (**DeviceId**) 20 符合上述範例中的案例。 **PhysicalLocation**從 BIOS 可協助您識別哪一個持續性記憶體裝置處於錯誤狀態。

## <a name="replacing-persistent-memory"></a>取代持續性記憶體

以上所述，我們會說明如何檢視您的持續性記憶體的健全狀況狀態。 如果您需要取代故障的模組，您將需要重新佈建的持續性記憶體磁碟 （請參閱我們上面所述的步驟）。

進行疑難排解時，您可能需要使用**移除 PmemDisk**，以移除特定的持續性記憶體磁碟。 我們可以移除所有目前的永續性磁碟，藉由：

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

請務必請注意，移除持續性記憶體磁碟將會導致資料遺失，在該磁碟上。

您可能需要的另一個 cmdlet **Initialize PmemPhysicalDevice**，這將會初始化實體的持續性記憶體裝置上的標籤存放區。 這可用來清除損毀的標籤在持續性記憶體裝置上的儲存體資訊。

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

請務必注意此命令應該用做最後的手段，來修正持續性記憶體相關問題。 它會導致資料遺失，到持續性記憶體。

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
- [在 Windows 中的存放裝置類別記憶體 (NVDIMM-N) 健全狀況管理](storage-class-memory-health.md)
- [了解快取](understand-the-cache.md)