---
title: 建立儲存空間直接存取中的磁碟區
description: 如何使用 Windows 管理中心和 PowerShell 在儲存空間直接存取中建立磁片區。
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 06/06/2019
ms.openlocfilehash: 8c17671f2f15d1373973dcf2fbafc753f0a163a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402885"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>建立儲存空間直接存取中的磁碟區

> 適用於：Windows Server 2019、Windows Server 2016

本主題描述如何使用 Windows 系統管理中心、PowerShell 或容錯移轉叢集管理員，在儲存空間直接存取叢集上建立磁片區。

> [!TIP]
> 如果您尚未開始，請先查看[規劃儲存空間直接存取中的磁碟區](plan-volumes.md)。

## <a name="create-a-three-way-mirror-volume"></a>建立三向鏡像磁片區

若要在 Windows 系統管理中心建立三向鏡像磁片區： 

1. 在 Windows 系統管理中心，連接到儲存空間直接存取叢集，然後從 [**工具**] 窗格中選取 [**磁片**區]。
2. 在 [磁片區] 頁面上，選取 [**清查**] 索引**標籤**，然後選取 [建立磁片區]。
3. 在 [**建立磁片**區] 窗格中，輸入磁片區的名稱，並將 [**復原**] 保留為 [**三向鏡像**]。
4. 在 [ **HDD 大小**] 中，指定磁片區的大小。 例如，5 TB （tb）。
5. 選取 [建立]。

視大小而定，建立磁片區可能需要幾分鐘的時間。 右上方的通知可讓您知道何時建立磁片區。 新的磁片區會出現在清查清單中。

觀看如何建立三向鏡像磁碟區的快速影片。

> [!VIDEO https://www.youtube-nocookie.com/embed/o66etKq70N8]

## <a name="create-a-mirror-accelerated-parity-volume"></a>建立鏡像加速同位磁片區

鏡像加速的同位會減少 HDD 上磁片區的使用量。 例如，三向鏡像磁片區表示每隔 10 tb 的大小，您的使用量將需要 30 tb。 若要降低使用量的負荷，請建立具有鏡像加速同位的磁片區。 如此一來，即使只有4部伺服器，也可以將最多的 20% 資料鏡像，並使用同位檢查來儲存其餘部分，以減少 30 tb 到 22 tb 的使用量。 您可以調整同位和鏡像的比例，讓效能與容量的取捨最適合您的工作負載。 例如，90% 同位檢查和 10% 鏡像會產生較低的效能，但更進一步簡化使用量。

若要在 Windows 系統管理中心建立具有鏡像加速同位的磁片區：

1. 在 Windows 系統管理中心，連接到儲存空間直接存取叢集，然後從 [**工具**] 窗格中選取 [**磁片**區]。
2. 在 [磁片區] 頁面上，選取 [**清查**] 索引**標籤**，然後選取 [建立磁片區]。
3. 在 [**建立磁片**區] 窗格中，輸入磁片區的名稱。
4. 在 [**復原**] 中，選取 [**鏡像-加速同位**]。
5. 在 [同位檢查**百分比**] 中，選取同位的百分比。
6. 選取 [建立]。

觀看如何建立鏡像加速同位磁片區的快速影片。

> [!VIDEO https://www.youtube-nocookie.com/embed/R72QHudqWpE]

## <a name="open-volume-and-add-files"></a>開啟磁片區並新增檔案

若要開啟磁片區，並在 Windows 系統管理中心將檔案新增至磁片區：

1. 在 Windows 系統管理中心，連接到儲存空間直接存取叢集，然後從 [**工具**] 窗格中選取 [**磁片**區]。
2. 在 [磁片區] 頁面上，選取 [**清查**] 索引標籤。
2. 在磁片區清單中，選取您要開啟的磁片區名稱。

    在 [磁片區詳細資料] 頁面上，您可以看到磁片區的路徑。

4. 在頁面頂端，選取 [**開啟**]。 這會在 Windows 系統管理中心啟動 [檔案] 工具。
5. 流覽至磁片區的路徑。 您可以在這裡流覽磁片區中的檔案。
6. 選取 **[上傳**]，然後選取要上傳的檔案。
7. 使用瀏覽器的 [**上一頁**] 按鈕，返回 Windows 系統管理中心的 [工具] 窗格。

觀賞如何開啟磁片區及新增檔案的快速影片。

> [!VIDEO https://www.youtube-nocookie.com/embed/j59z7ulohs4]

## <a name="turn-on-deduplication-and-compression"></a>開啟重復資料刪除和壓縮

重復資料刪除和壓縮是依每個磁片區來管理。 重復資料刪除和壓縮會使用後置處理模型，這表示在執行之前，您將不會看到節省的費用。 當它這麼做時，它就會處理所有檔案，甚至是先前從之前的檔案。

1. 在 Windows 系統管理中心，連接到儲存空間直接存取叢集，然後從 [**工具**] 窗格中選取 [**磁片**區]。
2. 在 [磁片區] 頁面上，選取 [**清查**] 索引標籤。
3. 在磁片區清單中，選取想要管理的磁片區名稱。
4. 在 [大量詳細資料] 頁面上，按一下標示為 [**重復資料刪除和壓縮**] 的交換器。
5. 在 [啟用重復資料刪除] 窗格中，選取重復資料刪除模式。

    Windows 系統管理中心可讓您針對不同的工作負載選擇現成的設定檔，而不是複雜的設定。 如果您不確定，請使用預設設定。

6. 選取 [**啟用**]。

觀賞如何開啟重復資料刪除和壓縮的快速影片。

> [!VIDEO https://www.youtube-nocookie.com/embed/PRibTacyKko]

## <a name="create-volumes-using-powershell"></a>使用 PowerShell 建立磁碟區

我們建議使用 **New-Volume** cmdlet 建立儲存空間直接存取的磁碟區。 它提供最快速與最簡單的體驗。 這個單一 cmdlet 會自動建立虛擬磁碟、磁碟分割以及格式化，以相符名稱建立磁碟區，並將其加入至叢集共用磁碟區 – 全在一個簡易步驟中。

**New-Volume** cmdlet 有四個您永遠需要提供的參數：

- **FriendlyName**您想要的任何字串，例如 *"Volume1"*
- **內**可能是**CSVFS_ReFS** （建議）或**CSVFS_NTFS**
- **StoragePoolFriendlyName**儲存集區的名稱，例如 *"S2D On ClusterName"*
- **大小:** 磁片區的大小，例如「 *10tb* 」

   > [!NOTE]
   > Windows，包括 PowerShell，使用二進位 (以 2 為底數) 數字計算，而磁碟機通常使用十進位 (以 10 為底數) 數字標示。 這解釋 "1 TB" 磁碟機，定義為 1,000,000,000,000 位元組，為何在 Windows 中顯示為約 "909 GB"。 這是預期行為。 在使用 **New-Volume** 建立磁碟區時，您應該以二進位 (以 2 為底數) 數字指定 **Size** 參數。 例如，指定 "909GB" 或 "0.909495TB" 會建立大約 1,000,000,000,000 位元組的磁碟區。

### <a name="example-with-2-or-3-servers"></a>範例：含2或3部伺服器

為了簡化，如果您的部署只有兩部伺服器，儲存空間直接存取會自動使用雙向鏡像復原類型。 如果您的部署只有三部伺服器，它就會自動使用三向鏡像。

```PowerShell
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>範例：含4部以上的伺服器

如果您有四部以上的伺服器，您可以使用選擇性的 **ResiliencySettingName** 參數選擇您的復原類型。

-   **ResiliencySettingName**可能是鏡像**或同**位。

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

### <a name="step-1-create-virtual-disk"></a>步驟 1:建立虛擬磁片

![新增虛擬硬碟](media/creating-volumes/GUI-Step-1.png)

1. 在 \[容錯移轉叢集管理員\] 中，瀏覽至 **\[儲存\]**  ->  **\[集區\]** 。
2. 從右側 \[執行\] 窗格選取 **\[新增虛擬硬碟\]** ，或以滑鼠右鍵按一下集區，然後選取 **\[新增虛擬硬碟\]** 。
3. 選取儲存集區然後按一下 **\[確定\]** 。 *\[新增虛擬硬碟精靈 (儲存空間直接存取)\]* 隨即開啟。
4. 使用精靈為虛擬硬碟命名，並指定其大小。
5. 檢視您的選項，然後按一下 **\[建立\]** 。
6. 關閉前，確定標記 **\[當此精靈關閉時建立磁碟區\]** 核取方塊已選取。

### <a name="step-2-create-volume"></a>步驟 2:建立磁片區

此時會開啟 *\[新增磁碟區精靈\]* 。

7. 選擇您剛建立的虛擬磁碟，然後按一下 **\[下一步\]** 。
8. 指定磁碟區大小 (預設：與虛擬磁碟大小相同)，按一下 **\[下一步\]** 。 
9. 指定磁碟區的磁碟機代號，或選擇 **\[不指派成磁碟機代號或資料夾\]** ，按一下 **\[下一步\]** 。
10. 指定要使用的檔案系統，將配置單位大小保留為 *\[預設\]* ，命名磁碟區，並按一下 **\[下一步\]** 。
11. 檢視您的選項，然後按一下 **\[建立\]** ，然後按一下 **\[關閉\]** 。

### <a name="step-3-add-to-cluster-shared-volumes"></a>步驟 3：新增至叢集共用磁片區

![[新增至叢集共用磁碟區]](media/creating-volumes/GUI-Step-2.png)

12. 在 \[容錯移轉叢集管理員\] 中，瀏覽至 **\[儲存\]**  ->  **\[磁碟\]** 。
13. 選擇您剛建立的虛擬磁碟，然後從右側 \[執行\] 窗格選取 **\[新增至叢集共用磁碟區\]** 或以滑鼠右鍵按一下虛擬磁碟，然後選取 **\[新增至叢集共用磁碟區\]** 。

大功告成！ 視需要重複以建立一個以上的磁碟區。

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
- [規劃儲存空間直接存取中的磁片區](plan-volumes.md)
- [擴充儲存空間直接存取中的磁片區](resize-volumes.md)
- [刪除儲存空間直接存取中的磁片區](delete-volumes.md)
