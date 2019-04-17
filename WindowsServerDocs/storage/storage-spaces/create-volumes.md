---
ms.assetid: a9f229eb-bef4-4231-97d0-0899e17cef32
title: 建立儲存空間直接存取中的磁碟區
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 01/11/2017
ms.localizationpriority: medium
ms.openlocfilehash: 277a676d8e53a7847d54039aab6607be8e5a78c5
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1833430"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>建立儲存空間直接存取中的磁碟區

>適用於：Windows Server 2016

本主題說明如何使用 PowerShell 或容錯移轉叢管理員，建立儲存空間直接存取的磁碟區。

   >[!TIP]
   >  如果您尚未開始，請先查看[規劃儲存空間直接存取中的磁碟區](plan-volumes.md)。

## <a name="create-volumes-using-powershell"></a>使用 PowerShell 建立磁碟區

我們建議使用 **New-Volume** cmdlet 建立儲存空間直接存取的磁碟區。 它提供最快速與最簡單的體驗。 這個單一 cmdlet 會自動建立虛擬磁碟、磁碟分割以及格式化，以相符名稱建立磁碟區，並將其加入至叢集共用磁碟區 – 全在一個簡易步驟中。

**New-Volume** cmdlet 有四個您永遠需要提供的參數：

- **FriendlyName：** 任何您想要的字串，例如 *"Volume1"*
- **FileSystem**：**CSVFS_ReFS** (建議選項) 或 **CSVFS_NTFS**
- **StoragePoolFriendlyName：** 儲存集區的名稱，例如 *"S2D on ClusterName"*
- **Size：** 磁碟區大小，例如 *"10TB"*

   >[!NOTE]
   >  Windows，包括 PowerShell，使用二進位 (以 2 為底數) 數字計算，而磁碟機通常使用十進位 (以 10 為底數) 數字標示。 這解釋 "1 TB" 磁碟機，定義為 1,000,000,000,000 位元組，為何在 Windows 中顯示為約 "909 GB"。 此為預期性行為。 在使用 **New-Volume** 建立磁碟區時，您應該以二進位 (以 2 為底數) 數字指定 **Size** 參數。 例如，指定 "909GB" 或 "0.909495TB" 會建立大約 1,000,000,000,000 位元組的磁碟區。

### <a name="example-with-2-or-3-servers"></a>範例：使用 2 或 3 部伺服器

為了簡化，如果您的部署只有兩部伺服器，儲存空間直接存取會自動使用雙向鏡像復原類型。 如果您的部署只有三部伺服器，它就會自動使用三向鏡像。

```
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>範例：使用 4 部以上伺服器

如果您有四部以上的伺服器，您可以使用選擇性的 **ResiliencySettingName** 參數選擇您的復原類型。

-   **ResiliencySettingName**：**Mirror** 或 **Parity**。

在下列範例，*"Volume2"* 使用三向鏡像，而 *"Volume3"* 使用雙同位（通常稱為「清除編碼」）。

```
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>範例：使用儲存層

在具有全部三種磁碟機類型的部署，一個磁碟區可以跨 SSD 和 HDD 層，部分存放在每個層。 同樣地，在具有四個以上伺服器的部署，一個磁碟區可以混合鏡像和雙同位，分別部分存放。

為了協助您建立這類磁碟區，儲存空間直接存取提供稱為 *Performance* 和 *Capacity* 的預設分層範本。 它們在較快的容量磁碟機（如果有的話）封裝三向鏡像的定義，在較慢的容量磁碟機（如果有的話）封裝雙同位的定義。

您可以執行 **Get-StorageTier** cmdlet 看到這些選項。

```
Get-StorageTier | Select FriendlyName, ResiliencySettingName, PhysicalDiskRedundancy
```

![儲存層 PowerShell 螢幕擷取畫面](media/creating-volumes/storage-tiers-screenshot.png)

若要建立分層磁碟區，請使用 **New-Volume** cmdlet 的 **StorageTierFriendlyNames** 和 **StorageTierSizes** 參數參考這些分層範本。 例如，下列 cmdlet 建立一個依 30:70 比例混合三向鏡像和雙同位的磁碟區。

```
New-Volume -FriendlyName "Volume4" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 300GB, 700GB
```

## <a name="create-volumes-using-failover-cluster-manager"></a>使用容錯移轉叢集管理員建立磁碟區

您也可以使用*新增虛擬硬碟精靈 (儲存空間直接存取)*，後面接著容錯移轉叢集管理員的*新增磁碟區精靈*，來建立磁碟區，不過這個工作流程有更多手動步驟而且不建議。

有三個主要步驟：

### <a name="step-1-create-virtual-disk"></a>步驟 1：建立虛擬磁碟

![新增虛擬硬碟](media/creating-volumes/GUI-Step-1.png)

1. 在 \[容錯移轉叢集管理員\] 中，瀏覽至 **\[儲存\]** -> **\[集區\]**。
2. 從右側 \[執行\] 窗格選取 **\[新增虛擬硬碟\]**，或以滑鼠右鍵按一下集區，然後選取 **\[新增虛擬硬碟\]**。
3. 選取儲存集區然後按一下 **\[確定\]**。 *\[新增虛擬硬碟精靈 (儲存空間直接存取)\]* 隨即開啟。
4. 使用精靈為虛擬硬碟命名，並指定其大小。
5. 檢視您的選項，然後按一下 **\[建立\]**。
6. 關閉前，確定標記 **\[當此精靈關閉時建立磁碟區\]** 核取方塊已選取。

### <a name="step-2-create-volume"></a>步驟 2：建立磁碟區

此時會開啟 *\[新增磁碟區精靈\]*。

7. 選擇您剛建立的虛擬磁碟，然後按一下 **\[下一步\]**。
8. 指定磁碟區大小 (預設：與虛擬磁碟大小相同)，按一下 **\[下一步\]**。 
9. 指定磁碟區的磁碟機代號，或選擇 **\[不指派成磁碟機代號或資料夾\]**，按一下 **\[下一步\]**。
10. 指定要使用的檔案系統，將配置單位大小保留為 *\[預設\]*，命名磁碟區，並按一下 **\[下一步\]**。
11. 檢視您的選項，然後按一下 **\[建立\]**，然後按一下 **\[關閉\]**。

### <a name="step-3-add-to-cluster-shared-volumes"></a>步驟 3：新增至叢集共用磁碟區

![新增至叢集共用磁碟區](media/creating-volumes/GUI-Step-2.png)

12. 在 \[容錯移轉叢集管理員\] 中，瀏覽至 **\[儲存\]** -> **\[磁碟\]**。
13. 選擇您剛建立的虛擬磁碟，然後從右側 \[執行\] 窗格選取 **\[新增至叢集共用磁碟區\]** 或以滑鼠右鍵按一下虛擬磁碟，然後選取 **\[新增至叢集共用磁碟區\]**。

大功告成！ 視需要重複以建立一個以上的磁碟區。

## <a name="see-also"></a>請參閱

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
- [規劃儲存空間直接存取中的磁碟區](plan-volumes.md)
