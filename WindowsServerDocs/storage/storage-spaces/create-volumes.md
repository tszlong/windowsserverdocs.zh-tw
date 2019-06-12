---
title: 建立儲存空間直接存取中的磁碟區
description: 如何在儲存空間直接存取使用 Windows Admin Center 和 PowerShell 建立磁碟區。
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 06/06/2019
ms.openlocfilehash: 85eca06a5d8c103851596055099876cb53a902ad
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810553"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>建立儲存空間直接存取中的磁碟區

> 適用於：Windows Server 2019，Windows Server 2016

本主題描述如何使用 Windows Admin Center、 PowerShell 或容錯移轉叢集管理員建立的儲存空間直接存取叢集上的磁碟區。

> [!TIP]
> 如果您尚未開始，請先查看[規劃儲存空間直接存取中的磁碟區](plan-volumes.md)。

## <a name="create-a-three-way-mirror-volume"></a>建立三向鏡像磁碟區

若要建立三向鏡像磁碟區的 Windows Admin Center: 

1. 在 Windows Admin Center，連接到儲存空間直接存取叢集中，然後再選取**磁碟區**從**工具**窗格。
2. 在 [磁碟區] 頁面中，選取**清查**索引標籤，然後按**建立磁碟區**。
3. 在**建立磁碟區**窗格中，輸入磁碟區的名稱，同時保持**復原**做為**三向鏡像**。
4. 在  **HDD 上的大小**，指定磁碟區的大小。 例如，5 TB (tb)。
5. 選取 [建立]  。

根據大小，建立磁碟區可能需要幾分鐘的時間。 在右上方的通知會讓您知道當建立磁碟區。 新的磁碟區會出現在清查清單中。

快速觀看如何建立三向鏡像磁碟區。

> [!VIDEO https://www.youtube-nocookie.com/embed/o66etKq70N8]

## <a name="create-a-mirror-accelerated-parity-volume"></a>建立鏡像加速同位檢查的磁碟區

鏡像加速同位檢查會減少的 HDD 上的磁碟區的使用量。 比方說，三向鏡像磁碟區會表示針對每一個 10 tb 的大小，您會需要 30 tb 為使用量。 若要減少使用量的額外負荷，建立具有鏡像加速同位檢查的磁碟區。 這會減少使用量從 30 tb 只是 22 兆位元，即使使用只有 4 的伺服器，以鏡像最常使用 20%的資料，並使用同位檢查，也就是更多的空間效率，來儲存其餘部分。 您可以調整這個比率同位檢查的鏡像來讓效能與容量最適合您的工作負載的權衡取捨。 比方說，90%的同位和百分之 10 的鏡像會產生較低的效能，但可簡化更進一步的使用量。

若要建立磁碟區鏡像加速同位檢查的 Windows Admin Center:

1. 在 Windows Admin Center，連接到儲存空間直接存取叢集中，然後再選取**磁碟區**從**工具**窗格。
2. 在 [磁碟區] 頁面中，選取**清查**索引標籤，然後按**建立磁碟區**。
3. 在 [**建立磁碟區**] 窗格中，輸入磁碟區的名稱。
4. 在 **恢復**，選取**鏡像加速同位檢查**。
5. 在 **同位檢查百分比**，選取 同位檢查的百分比。
6. 選取 [建立]  。

快速觀看如何建立鏡像加速同位檢查的磁碟區。

> [!VIDEO https://www.youtube-nocookie.com/embed/R72QHudqWpE]

## <a name="open-volume-and-add-files"></a>開啟磁碟區，並將檔案新增

若要開啟 磁碟區，並將檔案新增至 Windows Admin Center 中的磁碟區：

1. 在 Windows Admin Center，連接到儲存空間直接存取叢集中，然後再選取**磁碟區**從**工具**窗格。
2. 在 磁碟區 頁面中，選取**清查** 索引標籤。
2. 在磁碟區清單中，選取您想要開啟的磁碟區的名稱。

    在磁碟區的詳細資料頁面上，您可以看到磁碟區的路徑。

4. 在頁面頂端，選取**開啟**。 這會啟動 Windows Admin Center 中的檔案工具。
5. 瀏覽至 磁碟區的路徑。 這裡您可以瀏覽磁碟區中的檔案。
6. 選取 **上傳**，然後選取要上傳的檔案。
7. 使用瀏覽器**回**回到 Windows Admin Center 中的 [工具] 窗格的按鈕。

快速觀看如何開啟磁碟區，並將檔案加入。

> [!VIDEO https://www.youtube-nocookie.com/embed/j59z7ulohs4]

## <a name="turn-on-deduplication-and-compression"></a>開啟 重複資料刪除和壓縮

每個磁碟區管理重複資料刪除和壓縮。 重複資料刪除和壓縮會使用後置處理模型中，這表示，您就不會看到節約，直到它執行。 載入時，它將會使用所有檔案，即使之前有從。

1. 在 Windows Admin Center，連接到儲存空間直接存取叢集中，然後再選取**磁碟區**從**工具**窗格。
2. 在 磁碟區 頁面中，選取**清查** 索引標籤。
3. 在磁碟區清單中，選取要管理的磁碟區的名稱。
4. 在磁碟區的詳細資料頁面上，按一下 標示為 交換器**重複資料刪除和壓縮**。
5. 在 啟用重複資料刪除 窗格中，選取 重複資料刪除模式。

    而不是複雜的設定，Windows Admin Center 可讓您選擇不同的工作負載的現成的設定檔。 如果您不確定，請使用預設設定。

6. 選取 [**啟用**]。

快速觀看如何開啟重複資料刪除和壓縮。

> [!VIDEO https://www.youtube-nocookie.com/embed/PRibTacyKko]

## <a name="create-volumes-using-powershell"></a>使用 PowerShell 建立磁碟區

我們建議使用 **New-Volume** cmdlet 建立儲存空間直接存取的磁碟區。 它提供最快速與最簡單的體驗。 這個單一 cmdlet 會自動建立虛擬磁碟、磁碟分割以及格式化，以相符名稱建立磁碟區，並將其加入至叢集共用磁碟區 – 全在一個簡易步驟中。

**New-Volume** cmdlet 有四個您永遠需要提供的參數：

- **FriendlyName:** 任何您想的字串，例如 *"Volume1 」*
- **檔案系統：** 任一**CSVFS_ReFS** （建議選項） 或**CSVFS_NTFS**
- **StoragePoolFriendlyName:** 名稱，您的儲存體集區，例如 *"S2D 上 ClusterName"*
- **大小:** 磁碟區，例如大小 *"10 TB 」*

   > [!NOTE]
   > Windows，包括 PowerShell，使用二進位 (以 2 為底數) 數字計算，而磁碟機通常使用十進位 (以 10 為底數) 數字標示。 這解釋 "1 TB" 磁碟機，定義為 1,000,000,000,000 位元組，為何在 Windows 中顯示為約 "909 GB"。 此為預期性行為。 在使用 **New-Volume** 建立磁碟區時，您應該以二進位 (以 2 為底數) 數字指定 **Size** 參數。 例如，指定 "909GB" 或 "0.909495TB" 會建立大約 1,000,000,000,000 位元組的磁碟區。

### <a name="example-with-2-or-3-servers"></a>範例：具有 2 或 3 個伺服器

為了簡化，如果您的部署只有兩部伺服器，儲存空間直接存取會自動使用雙向鏡像復原類型。 如果您的部署只有三部伺服器，它就會自動使用三向鏡像。

```PowerShell
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>範例：4 + 伺服器

如果您有四部以上的伺服器，您可以使用選擇性的 **ResiliencySettingName** 參數選擇您的復原類型。

-   **ResiliencySettingName:** 任一**鏡像**或是**同位檢查**。

在下列範例， *"Volume2"* 使用三向鏡像，而 *"Volume3"* 使用雙同位（通常稱為「清除編碼」）。

```PowerShell
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>範例：使用儲存層

在具有全部三種磁碟機類型的部署，一個磁碟區可以跨 SSD 和 HDD 層，部分存放在每個層。 同樣地，在具有四個以上伺服器的部署，一個磁碟區可以混合鏡像和雙同位，分別部分存放。

為了協助您建立這類磁碟區，儲存空間直接存取提供稱為 *Performance* 和 *Capacity* 的預設分層範本。 它們在較快的容量磁碟機（如果有的話）封裝三向鏡像的定義，在較慢的容量磁碟機（如果有的話）封裝雙同位的定義。

您可以執行 **Get-StorageTier** cmdlet 看到這些選項。

```PowerShell
Get-StorageTier | Select FriendlyName, ResiliencySettingName, PhysicalDiskRedundancy
```

![儲存層 PowerShell 螢幕擷取畫面](media/creating-volumes/storage-tiers-screenshot.png)

若要建立分層磁碟區，請使用 **New-Volume** cmdlet 的 **StorageTierFriendlyNames** 和 **StorageTierSizes** 參數參考這些分層範本。 例如，下列 cmdlet 建立一個依 30:70 比例混合三向鏡像和雙同位的磁碟區。

```PowerShell
New-Volume -FriendlyName "Volume4" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 300GB, 700GB
```

## <a name="create-volumes-using-failover-cluster-manager"></a>使用容錯移轉叢集管理員建立磁碟區

您也可以使用*新增虛擬硬碟精靈 (儲存空間直接存取)* ，後面接著容錯移轉叢集管理員的*新增磁碟區精靈*，來建立磁碟區，不過這個工作流程有更多手動步驟而且不建議。

有三個主要步驟：

### <a name="step-1-create-virtual-disk"></a>步驟 1：建立虛擬磁碟

![新增虛擬硬碟](media/creating-volumes/GUI-Step-1.png)

1. 在 \[容錯移轉叢集管理員\] 中，瀏覽至 **\[儲存\]**  ->  **\[集區\]** 。
2. 從右側 \[執行\] 窗格選取 **\[新增虛擬硬碟\]** ，或以滑鼠右鍵按一下集區，然後選取 **\[新增虛擬硬碟\]** 。
3. 選取儲存集區然後按一下 **\[確定\]** 。 *\[新增虛擬硬碟精靈 (儲存空間直接存取)\]* 隨即開啟。
4. 使用精靈為虛擬硬碟命名，並指定其大小。
5. 檢視您的選項，然後按一下 **\[建立\]** 。
6. 關閉前，確定標記 **\[當此精靈關閉時建立磁碟區\]** 核取方塊已選取。

### <a name="step-2-create-volume"></a>步驟 2：建立磁碟區

此時會開啟 *\[新增磁碟區精靈\]* 。

7. 選擇您剛建立的虛擬磁碟，然後按一下 **\[下一步\]** 。
8. 指定磁碟區大小 (預設：與虛擬磁碟大小相同)，按一下 **\[下一步\]** 。 
9. 指定磁碟區的磁碟機代號，或選擇 **\[不指派成磁碟機代號或資料夾\]** ，按一下 **\[下一步\]** 。
10. 指定要使用的檔案系統，將配置單位大小保留為 *\[預設\]* ，命名磁碟區，並按一下 **\[下一步\]** 。
11. 檢視您的選項，然後按一下 **\[建立\]** ，然後按一下 **\[關閉\]** 。

### <a name="step-3-add-to-cluster-shared-volumes"></a>步驟 3：新增至叢集共用磁碟區

![[新增至叢集共用磁碟區]](media/creating-volumes/GUI-Step-2.png)

12. 在 \[容錯移轉叢集管理員\] 中，瀏覽至 **\[儲存\]**  ->  **\[磁碟\]** 。
13. 選擇您剛建立的虛擬磁碟，然後從右側 \[執行\] 窗格選取 **\[新增至叢集共用磁碟區\]** 或以滑鼠右鍵按一下虛擬磁碟，然後選取 **\[新增至叢集共用磁碟區\]** 。

大功告成！ 視需要重複以建立一個以上的磁碟區。

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
- [規劃中儲存空間直接存取磁碟區](plan-volumes.md)
- [擴充儲存空間直接存取中的磁碟區](resize-volumes.md)
- [刪除磁碟區中儲存空間直接存取](delete-volumes.md)
