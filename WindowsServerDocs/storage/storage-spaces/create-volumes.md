---
title: 建立儲存空間直接存取中的磁碟區
description: 如何使用 Windows Admin Center 和 PowerShell 在儲存空間直接存取中建立磁片區。
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.date: 02/25/2020
ms.topic: article
ms.openlocfilehash: e781afdef06c7cd965e53f0103cfc02f4e20aedc
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946934"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>建立儲存空間直接存取中的磁碟區

> 適用於：Windows Server 2019、Windows Server 2016

本主題說明如何使用 Windows Admin Center 和 PowerShell 在儲存空間直接存取叢集上建立磁片區。

> [!TIP]
> 如果您尚未開始，請先查看[規劃儲存空間直接存取中的磁碟區](plan-volumes.md)。

## <a name="create-a-three-way-mirror-volume"></a>建立三向鏡像磁碟區

若要在 Windows Admin Center 中建立三向鏡像磁片區：

1. 在 Windows Admin Center 中，連線到儲存空間直接存取叢集，然後從 [**工具**] 窗格中選取 [**磁片** 區]。
2. 在 [磁片區] 頁面上，選取 [ **清查** ] 索引 **標籤**，然後選取 [建立磁片區]。
3. 在 [ **建立磁片** 區] 窗格中，輸入磁片區的名稱，並將 **恢復** 功能保留為 **三向鏡像**。
4. 在 **HDD 上**，指定磁片區的大小。 例如，5 TB (tb) 。
5. 選取 [建立]。

視大小而定，建立磁片區可能需要幾分鐘的時間。 右上方的通知可讓您知道磁片區的建立時間。 新的磁片區會出現在清查清單中。

觀賞如何建立三向鏡像磁碟區的快速影片。

> [!VIDEO https://www.youtube-nocookie.com/embed/o66etKq70N8]

## <a name="create-a-mirror-accelerated-parity-volume"></a>建立鏡像加速同位磁片區

鏡像加速同位可減少 HDD 上的磁片區使用量。 例如，三向鏡像磁片區表示每 10 tb 的大小都需要 30 tb 的空間。 若要降低使用量的額外負荷，請建立具有鏡像加速同位的磁片區。 如此一來，即使只有4部伺服器，也可以將最活躍的20% 資料鏡像，並使用同位（較節省空間）來儲存其餘部分，以將使用量從 30 tb 減少到 22 tb。 您可以調整同位和鏡像的比例，以使效能與容量的平衡更適合您的工作負載。 例如，90% 的同位和10% 的鏡像會產生較少的效能，但更能簡化使用量。

若要在 Windows Admin Center 中建立具有鏡像加速同位的磁片區：

1. 在 Windows Admin Center 中，連線到儲存空間直接存取叢集，然後從 [**工具**] 窗格中選取 [**磁片** 區]。
2. 在 [磁片區] 頁面上，選取 [ **清查** ] 索引 **標籤**，然後選取 [建立磁片區]。
3. 在 [ **建立磁片** 區] 窗格中，輸入磁片區的名稱。
4. 在 [ **復原**] 中，選取 [ **鏡像加速同位**]。
5. 在 [同位 **百分比**] 中，選取同位的百分比。
6. 選取 [建立]。

觀看有關如何建立鏡像加速同位磁片區的快速影片。

> [!VIDEO https://www.youtube-nocookie.com/embed/R72QHudqWpE]

## <a name="open-volume-and-add-files"></a>開啟磁片區並新增檔案

若要在 Windows Admin Center 中開啟磁片區並將檔案新增至磁片區：

1. 在 Windows Admin Center 中，連線到儲存空間直接存取叢集，然後從 [**工具**] 窗格中選取 [**磁片** 區]。
2. 在 [磁片區] 頁面上，選取 [ **清查** ] 索引標籤。
2. 在磁片區清單中，選取您要開啟之磁片區的名稱。

    在 [磁片區詳細資料] 頁面上，您可以看到磁片區的路徑。

4. 在頁面頂端，選取 [ **開啟**]。 這會啟動 Windows Admin Center 中的 [檔案] 工具。
5. 流覽至磁片區的路徑。 您可以在這裡流覽磁片區中的檔案。
6. 選取 **[上傳**]，然後選取要上傳的檔案。
7. 使用瀏覽器的 [ **上一頁** ] 按鈕，返回 Windows Admin Center 中的 [工具] 窗格。

觀賞如何開啟磁片區和新增檔案的快速影片。

> [!VIDEO https://www.youtube-nocookie.com/embed/j59z7ulohs4]

## <a name="turn-on-deduplication-and-compression"></a>開啟重復資料刪除和壓縮

每個磁片區會管理重復資料刪除和壓縮。 重復資料刪除和壓縮會使用後置處理模型，這表示在執行之前，您不會看到任何節省量。 當它執行時，它會處理所有檔案，甚至是之前的檔案。

1. 在 Windows Admin Center 中，連線到儲存空間直接存取叢集，然後從 [**工具**] 窗格中選取 [**磁片** 區]。
2. 在 [磁片區] 頁面上，選取 [ **清查** ] 索引標籤。
3. 在磁片區清單中，選取想要管理之磁片區的名稱。
4. 在 [磁片區詳細資料] 頁面上，按一下標示為 **重復資料刪除和壓縮** 的參數。
5. 在 [啟用重復資料刪除] 窗格中，選取重復資料刪除模式。

    Windows Admin Center 可讓您在適用于不同工作負載的現成設定檔之間做選擇，而不是複雜的設定。 如果您不確定，請使用預設設定。

6. 選取 [啟用]。

觀賞如何開啟重復資料刪除和壓縮的快速影片。

> [!VIDEO https://www.youtube-nocookie.com/embed/PRibTacyKko]

## <a name="create-volumes-using-powershell"></a>使用 PowerShell 建立磁碟區

我們建議使用 **New-Volume** cmdlet 建立儲存空間直接存取的磁碟區。 其提供最快速且最直接的體驗。 此單一 Cmdlet 會自動建立虛擬磁碟、分割區 (並將其格式化)，以及建立具有相符名稱的磁碟區，並將其新增至叢集共用磁碟區 – 全部動作皆可在一個簡單步驟中完成。

**New-Volume** Cmdlet 有四個一定要提供的參數：

- **FriendlyName：** 您想要的任何字串，例如 "Volume1" 
- **FileSystem：** **CSVFS_ReFS** (建議) 或 **CSVFS_NTFS**
- **StoragePoolFriendlyName：** 存放集區的名稱，例如 "S2D on ClusterName" 
- **Size：** 磁碟區的大小，例如 "10TB" 

   > [!NOTE]
   > Windows (包括 PowerShell) 會使用二進位 (以 2 為基底) 數字來計算，而磁碟機通常是使用十進位 (以 10 為基底) 數字來標示。 這說明為什麼 "1 TB" 磁碟機 (定義為 1,000,000,000,000 個位元組) 在 Windows 中會顯示為大約 "909 GB"。 這是預期行為。 使用 **New-Volume** 來建立磁碟區時，您應該以二進位 (以 2 為基底) 數字指定 **Size** 參數。 例如，指定 "909GB" 或 "0.909495TB" 將會建立大約 1,000,000,000,000 個位元組的磁碟區。

### <a name="example-with-2-or-3-servers"></a>範例：2 或 3 部伺服器

為了簡便起見，如果您的部署只有兩部伺服器，儲存空間直接存取會自動使用雙向鏡像來提供復原功能。 如果您的部署只有三部伺服器，則會自動使用三向鏡像。

```PowerShell
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>範例：4 部以上的伺服器

如果您有四部或四部以上的伺服器，您可以使用選擇性的 **ResiliencySettingName** 參數來選擇您的復原類型。

-   **ResiliencySettingName：** **鏡像** 或 **同位**。

在下列範例中，"Volume2"  會使用三向鏡像，而 "Volume3"  會使用雙同位 (通常稱為「抹除碼」)。

```PowerShell
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>範例：使用儲存層

在具有三種磁碟機的部署中，一個磁碟區可以分成幾部分放在 SSD 和 HDD 層上。 同樣地，在具有四部或四部以上伺服器的部署中，一個磁碟區可以混合使用鏡像和雙同位，將其各部分放在這些伺服器上。

為了協助您建立這類磁碟區，儲存空間直接存取會提供稱為「效能」  和「容量」  的預設層範本。 其會在較快的容量磁碟機 (如果有的話) 上封裝三向鏡像的定義，並在較慢的容量磁碟機 (如果有的話) 上使用雙同位。

您可以藉由執行 **StorageTier** Cmdlet 來查看這些磁碟機。

```PowerShell
Get-StorageTier | Select FriendlyName, ResiliencySettingName, PhysicalDiskRedundancy
```

![儲存層 PowerShell 螢幕擷取畫面](media/creating-volumes/storage-tiers-screenshot.png)

若要建立階層式磁碟區，請使用 **New-Volume** Cmdlet 的 **StorageTierFriendlyNames** 和 **StorageTierSizes** 參數來參考這些分層範本。 例如，下列 Cmdlet 會建立一個以 30:70 比例混合三向鏡像和雙同位的磁碟區。

```PowerShell
New-Volume -FriendlyName "Volume4" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 300GB, 700GB
```

大功告成！ 如有需要，可重複步驟來建立一個以上的磁碟區。

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md) \(部分機器翻譯\)
- [規劃儲存空間直接存取中的磁碟區](plan-volumes.md)
- [延伸儲存空間直接存取中的磁碟區](resize-volumes.md)
- [在儲存空間直接存取中刪除磁片區](delete-volumes.md)
