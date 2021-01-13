---
description: 深入瞭解：推動儲存空間直接存取的對稱考慮
title: 推動儲存空間直接存取的對稱考慮
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
ms.localizationpriority: medium
ms.openlocfilehash: aad7c0feb36699bedf7c955cd0172669e56910dd
ms.sourcegitcommit: decb6c8caf4851b13af271d926c650d010a6b9e9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2021
ms.locfileid: "98177497"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>推動儲存空間直接存取的對稱考慮

> 適用於：Windows Server 2019、Windows Server 2016

當每個伺服器都有完全相同的磁片磁碟機時，[儲存空間直接存取](storage-spaces-direct-overview.md)的效果最佳。

實際上，我們認為這不一定是可行的：儲存空間直接存取是設計用來執行多年，並隨著組織的需要而調整。 目前，您可以購買 spacious 3 TB 硬碟;下一年，您可能不可能找到很小的專案。 因此，支援某種程度的混合和比對。

本主題說明條件約束，並提供支援和不支援設定的範例。

## <a name="constraints"></a>條件約束

### <a name="type"></a>類型

所有伺服器都應具有相同 [類型的磁片磁碟機](choosing-drives.md#drive-types)。

例如，如果一部 *伺服器有 nvme，它們應該都* 有 nvme。

### <a name="number"></a>Number

所有伺服器的每個類型都應該有相同數目的磁片磁碟機。

例如，如果一部伺服器有六個 SSD，它們應該 *都* 有六個 ssd。

   > [!NOTE]
   > 在失敗期間或新增或移除磁片磁碟機時，磁片磁碟機數目可能會暫時不同。

### <a name="model"></a>型號

建議您盡可能使用相同型號和固件版本的磁片磁碟機。 如果您無法這麼做，請小心選取越相似的磁片磁碟機。 我們不鼓勵相同類型的混合和相符磁片磁碟機具有顯著的效能或耐用性特性 (除非是快取，另一個則是容量) ，因為儲存空間直接存取平均分配 IO，且不會根據模型來區分。

   > [!NOTE]
   > 您可以混用和比對類似的 SATA 和 SAS 磁片磁碟機。

### <a name="size"></a>大小

建議盡可能使用相同大小的磁片磁碟機。 使用不同大小的容量磁片可能會導致某些無法使用的容量，而且使用不同大小的快取磁片磁碟機可能無法改善快取效能。 請參閱下一節以取得詳細資訊。

   > [!WARNING]
   > 跨伺服器的不同容量磁片磁碟機大小可能會導致閒置的容量。

## <a name="understand-capacity-imbalance"></a>瞭解：容量不平衡

儲存空間直接存取健全于跨磁片磁碟機與跨伺服器的容量不平衡。 即使不平衡很嚴重，所有專案仍會繼續運作。 不過，根據數個因素而定，每個伺服器中無法使用的容量可能無法使用。

若要瞭解為何會發生這種情況，請考慮以下簡化的圖解。 每個彩色方塊都代表一個鏡像資料複本。 例如，標示為 A、' 和 ' ' 的方塊為相同資料的三個複本。 若要接受伺服器容錯， *必須* 將這些複本儲存在不同的伺服器上。

### <a name="stranded-capacity"></a>擱置容量

伺服器 1 (10 TB) 和 Server 2 (10 TB) 已滿。 伺服器3擁有較大的磁片磁碟機，因此其總容量 (15 TB) 。 不過，若要將更多三向鏡像資料儲存在伺服器3上，則也需要伺服器1和伺服器2上的複本，這已經是已滿的複本。 伺服器3上剩餘的 5 TB 容量無法使用–它是「 *擱置* 」的容量。

![三向鏡像、三部伺服器、擱置的容量](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>最佳放置

相反地，如果有四部伺服器的 10 TB、10 TB、10 TB 和 15 TB 的容量，以及三向鏡像復原，則可以使用 *已* 繪製的所有可用容量，以有效的方式來有效放置複製。 如果有這種情況，儲存空間直接存取配置器會尋找並使用最佳位置，而不會留下任何閒置的容量。

![三向鏡像，四部伺服器，沒有擱置的容量](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

伺服器數目、復原能力、容量不平衡的嚴重性，以及其他因素會影響是否有擱置的容量。 **最審慎的一般規則是假設只有每個伺服器中可用的容量才可供使用。**

## <a name="understand-cache-imbalance"></a>瞭解：快取不平衡

儲存空間直接存取健全于快取跨磁片磁碟機和伺服器之間的不平衡。 即使不平衡很嚴重，所有專案仍會繼續運作。 此外，儲存空間直接存取一律會完全使用所有可用的快取。

不過，使用不同大小的快取磁片磁碟機，可能無法以一致或可預測的方式改善快取效能：只有 [IO 與較](understand-the-cache.md#server-side-architecture) 大的快取磁片磁碟機系結，可能會看到效能提升 儲存空間直接存取跨系結平均分配 IO，並且不會根據快取到容量的比率來區分。

![快取不平衡](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > 若要深入瞭解快取系結，請參閱 [瞭解](understand-the-cache.md) 快取。

## <a name="example-configurations"></a>範例設定

以下是一些支援和不支援的設定：

### <a name="image-typeicon-sourcemediadrive-symmetry-considerationssupportedpng-supported-different-models-between-servers"></a>:::image type="icon" source="media/drive-symmetry-considerations/supported.png"::: 支援：伺服器之間的不同模型

前兩部伺服器使用 NVMe 模型 "X"，但第三部伺服器使用 NVMe 模型 "Z"，這非常類似。

| 伺服器 1                    | 伺服器2                    | 伺服器3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x NVMe 模型 X (快取)     | 2 x NVMe 模型 X (快取)     | 2 x NVMe 模型 Z (cache)     |
| 10 x SSD 模型 Y (容量)  | 10 x SSD 模型 Y (容量)  | 10 x SSD 模型 Y (容量)  |

這有受到支援。

### <a name="image-typeicon-sourcemediadrive-symmetry-considerationssupportedpng-supported-different-models-within-server"></a>:::image type="icon" source="media/drive-symmetry-considerations/supported.png"::: 支援：伺服器內的不同模型

每部伺服器都會使用不同的 HDD 模型 "Y" 和 "Z" 混合，這兩者非常類似。 每部伺服器都有總共10個 HDD。

| 伺服器 1                   | 伺服器2                   | 伺服器3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x SSD 模型 X (快取)     | 2 x SSD 模型 X (快取)     | 2 x SSD 模型 X (快取)     |
| 7 x HDD 型號 Y (容量)  | 5 x HDD 型號 Y (容量)  | 1 x HDD 型號 Y (容量)  |
| 3 x HDD 型號 Z (容量)  | 5 x HDD 型號 Z (容量)  | 9 x HDD 型號 Z (容量)  |

這有受到支援。

### <a name="image-typeicon-sourcemediadrive-symmetry-considerationssupportedpng-supported-different-sizes-across-servers"></a>:::image type="icon" source="media/drive-symmetry-considerations/supported.png"::: 支援：跨伺服器的大小不同

前兩部伺服器使用 4 TB HDD，但第三部伺服器使用非常類似的 6 TB HDD。

| 伺服器 1                | 伺服器2                | 伺服器3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe (cache)  | 2 x 800 GB NVMe (cache)  | 2 x 800 GB NVMe (cache)  |
| 4 x 4 TB HDD (容量)  | 4 x 4 TB HDD (容量)  | 4 x 6 TB HDD (容量)  |

這是支援的，但這會導致閒置的容量。

### <a name="image-typeicon-sourcemediadrive-symmetry-considerationssupportedpng-supported-different-sizes-within-server"></a>:::image type="icon" source="media/drive-symmetry-considerations/supported.png"::: 支援：伺服器內的不同大小

每部伺服器都會使用不同的混合 1.2 TB 和非常類似的 1.6 TB SSD。 每部伺服器都有4個 SSD 總計。

| 伺服器 1                 | 伺服器2                 | 伺服器3                 |
|--------------------------|--------------------------|--------------------------|
| 3 x 1.2 TB 的 SSD (快取)    | 2 x 1.2 TB 的 SSD (快取)    | 4 x 1.2 TB 的 SSD (快取)    |
| 1 x 1.6 TB 的 SSD (快取)    | 2 x 1.6 TB 的 SSD (快取)    | -                        |
| 20 x 4 TB 的 HDD (容量)  | 20 x 4 TB 的 HDD (容量)  | 20 x 4 TB 的 HDD (容量)  |

這有受到支援。

### <a name="image-typeicon-sourcemediadrive-symmetry-considerationsunsupportedpng-not-supported-different-types-of-drives-across-servers"></a>:::image type="icon" source="media/drive-symmetry-considerations/unsupported.png"::: 不支援：跨伺服器的磁片磁碟機類型不同

伺服器1有 NVMe，但其他則否。

| 伺服器 1            | 伺服器2            | 伺服器3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (cache)     | -                   | -                   |
| -                   | 6 x SSD (快取)      | 6 x SSD (快取)      |
| 18 x HDD (容量)  | 18 x HDD (容量)  | 18 x HDD (容量)  |

不支援此做法。 每一部伺服器中的磁片磁碟機類型應該相同。

### <a name="image-typeicon-sourcemediadrive-symmetry-considerationsunsupportedpng-not-supported-different-number-of-each-type-across-servers"></a>:::image type="icon" source="media/drive-symmetry-considerations/unsupported.png"::: 不支援：跨伺服器的每個類型都有不同的數目

伺服器3的磁片磁碟機多於其他磁片磁碟機。

| 伺服器 1            | 伺服器2            | 伺服器3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (cache)     | 2 x NVMe (cache)     | 4 x NVMe (cache)     |
| 10 x HDD (容量)  | 10 x HDD (容量)  | 20 x HDD (容量)  |

不支援此做法。 每一部伺服器的每個類型磁片磁碟機數目應該相同。

### <a name="image-typeicon-sourcemediadrive-symmetry-considerationsunsupportedpng-not-supported-only-hdd-drives"></a>:::image type="icon" source="media/drive-symmetry-considerations/unsupported.png"::: 不支援：僅 HDD 磁片磁碟機

所有伺服器都只會連接 HDD 磁片磁碟機。

|伺服器 1|伺服器2|伺服器3|
|-|-|-|
|18 x HDD (容量)  |18 x HDD (容量) |18 x HDD (容量) |

不支援此做法。 您必須至少新增兩個快取磁片磁碟機 (NvME 或 SSD) 連接到每部伺服器。

## <a name="summary"></a>總結

為了回顧，叢集中的每一部伺服器都應該有相同類型的磁片磁碟機，而且每種類型都有相同的數目。 您可以視需要使用上述考慮，視需要混搭磁片磁碟機模型和磁片磁碟機大小。

| 條件約束 | 州 |
|--|--|
| 每一部伺服器中的磁片磁碟機類型相同 | **必要** |
| 每一部伺服器中的每個類型都有相同的數目 | **必要** |
| 每一部伺服器中的相同磁片磁碟機型號 | 建議 |
| 每一部伺服器都有相同的磁片磁碟機大小 | 建議 |

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取硬體需求](storage-spaces-direct-hardware-requirements.md)
- [儲存空間直接存取概觀](storage-spaces-direct-overview.md) \(部分機器翻譯\)
