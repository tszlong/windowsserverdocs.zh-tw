---
ms.assetid: 898d72f1-01e7-4b87-8eb3-a8e0e2e6e6da
title: 將伺服器或磁碟機新增至儲存空間直接存取
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2017
description: 如何將伺服器或磁碟機新增至儲存空間直接存取叢集
ms.localizationpriority: medium
ms.openlocfilehash: ae639b920788911dbc16952d7b61aab85b0a391b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833449"
---
# <a name="adding-servers-or-drives-to-storage-spaces-direct"></a>將伺服器或磁碟機新增至儲存空間直接存取

>適用於：Windows Server 2019，Windows Server 2016

本主題說明如何將伺服器或磁碟機新增至儲存空間直接存取。

## <a name="adding-servers"></a> 新增伺服器

新增伺服器 (通常稱為相應放大) 會增加儲存容量、改善儲存空間的效能，並解除鎖定更佳的儲存空間效率。 如果您的部署為超交集，新增伺服器也可為您的工作負載提供更多計算資源。

![將伺服器新增到四個節點叢集的動畫](media/add-nodes/Scaling-Out.gif)

一般部署可以藉由新增伺服器，簡單地相應放大： 只需要兩個步驟：

1. 執行[叢集驗證精靈](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx)，方法是使用容錯移轉叢集嵌入式管理單元或使用 PowerShell 中的 **Test-Cluster** Cmdlet (以系統管理員身分執行)。 包含您想新增的新伺服器*\<NewNode>*。

   ```PowerShell
   Test-Cluster -Node <Node>, <Node>, <Node>, <NewNode> -Include "Storage Spaces Direct", Inventory, Network, "System Configuration"
   ```

   這可確認新的伺服器正在執行 Windows Server 2016 Datacenter Edition、已加入與現有伺服器相同的 Active Directory Domain Services、具有所有必要的角色和功能，並且已正確設定網路。

   >[!IMPORTANT]
   > 若您重新使用的磁碟機內含您不再需要的舊資料或中繼資料，使用 [磁碟管理] 或 **Reset-PhysicalDisk** Cmdlet 加以清除。 若偵測到舊的資料或中繼資料，磁碟機便不會置於集區。

2. 在叢集上執行下列 Cmdlet 以完成新增伺服器：

```
Add-ClusterNode -Name NewNode 
```

   >[!NOTE]
   > 自動加入集區取決於您是否只有一個集區。 如果您已經規避標準設定來建立多個集區，您必須自行使用 **Add-PhysicalDisk**，將新的磁碟機新增至您慣用的集區。

### <a name="from-2-to-3-servers-unlocking-three-way-mirroring"></a>從 2 個伺服器變為 3 個：解除鎖定三向鏡像

![將第三個伺服器新增至兩個節點叢集](media/add-nodes/Scaling-2-to-3.png)

使用兩個伺服器，您只能建立雙向鏡像磁碟區 (相較於分散式 RAID-1)。 使用三個伺服器，您便能建立三向鏡像磁碟區並獲得更佳的容錯。 建議您盡可能使用三向鏡像。

雙向鏡像磁碟區無法就地升級至三向鏡像。 然而，您可建立新的磁碟區並將資料移轉 (複製，例如透過使用[儲存體複本](../storage-replica/server-to-server-storage-replication.md)) 至其中，然後移除舊的磁碟區。

若要開始建立三向鏡像磁碟區，您有數個好用的選項： 您可任意選擇想用的選項。 

#### <a name="option-1"></a>選項 1

在建立期間，於每個新磁碟區上指定 **PhysicalDiskRedundancy = 2**。

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2
```

#### <a name="option-2"></a>選項 2

或是在集區名為 **Mirror** 的 **ResiliencySetting** 物件上設定 **PhysicalDiskRedundancyDefault = 2**。 接著，任何新的鏡像磁碟區都將自動使用*三向*鏡像，即使您並未指定亦然。

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Mirror | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size>
```

#### <a name="option-3"></a>選項 3

在名為 *Capacity* 的 **StorageTier** 範本上設定 **PhysicalDiskRedundancy = 2**，然後藉由參考該層來建立磁碟區。

```PowerShell
Set-StorageTier -FriendlyName Capacity -PhysicalDiskRedundancy 2 

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Capacity -StorageTierSizes <Size>
```

### <a name="from-3-to-4-servers-unlocking-dual-parity"></a>從 3 個伺服器變為 4 個：解除鎖定雙同位

![將第四個伺服器新增至三個節點叢集](media/add-nodes/Scaling-3-to-4.png)

使用四個伺服器，您便可以使用通常也稱為清除編碼的雙同位 (相較於分散式 RAID-6)。 這會提供與三向鏡像相同的容錯功能，但具有更佳的儲存空間效率。 如需深入了解，請參閱[容錯與儲存空間效率](storage-spaces-fault-tolerance.md)。

若您的部署規模較小，有多個好用選項可供您開始建立雙同位磁碟區。 您可任意選擇想用的選項。

#### <a name="option-1"></a>選項 1

在建立期間，於每個新磁碟區上指定 **PhysicalDiskRedundancy = 2** 及 **ResiliencySettingName = Parity**。

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity
```

#### <a name="option-2"></a>選項 2

在集區名為 **Parity** 的 **ResiliencySetting** 物件上設定 **PhysicalDiskRedundancy = 2**。 接著，任何新的同位磁碟區都將自動使用*雙*同位，即使您並未指定亦然。

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Parity | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -ResiliencySettingName Parity
```

利用四個伺服器，您也可以開始使用鏡像加速的同位，其中的個別磁碟區為部分鏡像及部分同位。

為此，您必須更新您的 **StorageTier** 範本，以同時擁有 *[效能]* 和 [容量]*[容量]* 層，因為如果您先在四個伺服器上執行 **Enable-ClusterS2D** 就會加以建立。 具體來說，這兩層都應該具有您容量裝置 (例如 SSD 或 HDD) 的 **MediaType** 且 **PhysicalDiskRedundancy = 2**。 *[效能]* 層應該是 **ResiliencySettingName = Mirror**，而 *[容量]* 層應該是 **ResiliencySettingName = Parity**。

#### <a name="option-3"></a>選項 3

您可能會發現最簡單的方式是只移除現有的階層範本，並建立兩個新的階層範本即可。 這並不會影響任何透過參考階層範本所建立的預先存在磁碟區：其僅是範本。

```PowerShell
Remove-StorageTier -FriendlyName Capacity

New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Mirror -FriendlyName Performance
New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity -FriendlyName Capacity
```

就這麼容易！ 您現在已可透過參考這些階層範本建立鏡像加速的同位磁碟區。

#### <a name="example"></a>範例

```PowerShell
New-Volume -FriendlyName "Sir-Mix-A-Lot" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes <Size, Size> 
```

### <a name="beyond-4-servers-greater-parity-efficiency"></a>超越 4 個伺服器：絕佳同位效率

當您擴充超過四個伺服器時，新的磁碟區就能受惠於前所未有的絕佳同位編碼效率。 例如，介於六到七個伺服器之間，效率會從 50.0% 提升到 66.7%，因為它變為可能可使用 Reed-Solomon 4+2 (而不是 2+2)。 您不需採取任何步驟，就能開始享受這種新的效率；最佳可行的編碼會在您每次建立磁碟區時自動決定。

不過，任何預先存在的磁碟區將不會「轉換」為範圍更廣泛的新編碼。 其中一個合理的原因是，這樣做需要大量計算，而此計算確實會影響整個部署中的 *「每個單一位元」*。 如果您想要讓預先存在的資料以更佳的效率進行編碼，您可以將它移轉至新的磁碟區。

如需詳細資料，請參閱[容錯與儲存空間效率](storage-spaces-fault-tolerance.md)。

### <a name="adding-servers-when-using-chassis-or-rack-fault-tolerance"></a>使用底座或機架容錯時新增伺服器

如果您的部署會使用底座或機架容錯，您必須先指定新伺服器的底座或機架，然後再將它們新增至叢集。 這會告訴儲存空間直接存取，利用最佳方式來分散資料，以充分利用容錯功能。

1. 建立節點的暫存容錯網域，方法是開啟已提升權限的 PowerShell 工作階段，然後使用下列命令，其中 *\<NewNode>* 是新叢集節點的名稱：

   ```PowerShell
   New-ClusterFaultDomain -Type Node -Name <NewNode> 
   ```

2. 將此暫存的容錯網域移至真實世界中新伺服器所在的底座或機架，如 *\<ParentName>* 所指定：

   ```PowerShell
   Set-ClusterFaultDomain -Name <NewNode> -Parent <ParentName> 
   ```

   如需詳細資訊，請參閱 [Windows Server 2016 中的容錯網域感知](../../failover-clustering/fault-domains.md)。

3. 將伺服器新增到叢集，如[新增伺服器](#adding-servers)中所述。 當新伺服器加入叢集時，它會自動與預留位置容錯網域產生關聯 (使用它的名稱)。

## <a name="adding-drives"></a> 新增磁碟機

新增磁碟機 (也稱為相應增加) 會增加儲存容量，並可改善效能。 如果您有可用的插槽，可將磁碟機新增至每個伺服器以擴充儲存容量，而不需新增伺服器。 您可以隨時個別新增快取磁碟機或容量磁碟機。

   >[!IMPORTANT]
   > 強烈建議您所有伺服器都應有相同的儲存體設定。

![動畫顯示 新增磁碟機，以系統](media/add-nodes/Scale-Up.gif)

若要相應增加，請連接磁碟機並驗證 Windows 會加以探索。 其應顯示於 PowerShell 中 **Get-PhysicalDisk** Cmdlet 的輸出中，且其 **CanPool** 屬性設為 **True**。 若其顯示為 **CanPool = False**，您可透過檢查其 **CannotPoolReason** 屬性了解原因。

```PowerShell
Get-PhysicalDisk | Select SerialNumber, CanPool, CannotPoolReason
```

在短時間內，儲存空間直接存取將會自動宣告符合資格的磁碟機、將之新增至儲存集區，而磁碟區將自動[平均重新分散到所有磁碟機上](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/)。 此時，您已完成並準備好[延伸磁碟區](resize-volumes.md)或[建立新磁碟區](create-volumes.md)。

如果磁碟機沒有出現，請手動掃描硬體變更。 這可以使用 [動作] 功能表下方的 [裝置管理員] 來完成。 如果它們包含舊的資料或中繼資料，請考慮加以重新格式化。 可透過使用 **[磁碟管理]** 或 **Reset-PhysicalDisk** Cmdlet 進行此操作。

   >[!NOTE]
   > 自動加入集區取決於您是否只有一個集區。 如果您已經規避標準設定來建立多個集區，您必須自行使用 **Add-PhysicalDisk**，將新的磁碟機新增至您慣用的集區。

## <a name="optimizing-drive-usage-after-adding-drives-or-servers"></a>新增磁碟機或伺服器之後，最佳化磁碟機使用量

經過一段時間，因為磁碟機新增或移除，集區中的磁碟機之間的資料分佈可能會變得不平均。 在某些情況下，這可能會造成在變滿，而集區中的其他磁碟機具有更低的耗用量特定磁碟機中。

為了協助保持甚至跨集區的磁碟機配置，儲存空間直接存取會自動最佳化磁碟機使用量之後您將磁碟機或伺服器加入至集區 （這是手動程序適用於使用共用 SAS 機箱的儲存空間系統）。 最佳化會啟動 15 分鐘之後您將新的磁碟機新增至集區。 集區最佳化會以執行低優先順序背景作業，因此可能需要小時或天，才能完成，特別是當您使用大型的硬碟機。

最佳化會使用兩個工作的另一個名*最佳化*而另一個名*重新平衡*-您可以監視其進度，使用下列命令：

```powershell
Get-StorageJob
```

您可以手動將最佳化的儲存體集區[Optimize-storagepool](https://docs.microsoft.com/powershell/module/storage/optimize-storagepool?view=win10-ps) cmdlet。 以下為範例：

```powershell
Get-StoragePool <PoolName> | Optimize-StoragePool
```
