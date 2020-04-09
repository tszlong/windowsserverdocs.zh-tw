---
title: 驅動儲存空間直接存取的對稱考慮
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
ms.localizationpriority: medium
ms.openlocfilehash: b06d69c020ea38a2fb9f23df2cfd9cd4191ae315
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857551"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>驅動儲存空間直接存取的對稱考慮 

> 適用于： Windows Server 2019、Windows Server 2016

當每一部伺服器都有完全相同的磁片磁碟機時，[儲存空間直接存取](storage-spaces-direct-overview.md)的效果最佳。

實際上，我們認為這不一定可行：儲存空間直接存取的設計是要多年來執行，並隨著您組織的需求而調整。 今天，您可以購買 spacious 3 TB 硬碟;下一年，可能會無法找到小型的。 因此，支援某種程度的混合和比對。

本主題說明條件約束，並提供支援和不支援設定的範例。

## <a name="constraints"></a>限制式

### <a name="type"></a>類型

所有伺服器都應該具有相同[類型的磁片磁碟機](choosing-drives.md#drive-types)。

例如，如果一部*伺服器有 nvme，它們應該都*有 nvme。

### <a name="number"></a>Number

所有伺服器的每個類型都應該要有相同數目的磁片磁碟機。

例如，如果一部伺服器有六個 SSD，則它們*都應該有*六個 ssd。

   > [!NOTE]
   > 在失敗期間或在新增或移除磁片磁碟機時，磁片磁碟機數目會暫時不同。

### <a name="model"></a>模型

我們建議您盡可能使用相同型號和固件版本的磁片磁碟機。 如果您無法這麼做，請小心選取越類似的磁片磁碟機。 我們不鼓勵混合和比對相同類型的磁片磁碟機，其具有顯著不同的效能或耐用特性（除非其中一個是快取，另一個則是容量），因為儲存空間直接存取會平均分配 IO，而不會根據模型來區分。

   > [!NOTE]
   > 可以混搭和比對類似的 SATA 和 SAS 磁片磁碟機。

### <a name="size"></a>大小

我們建議盡可能使用相同大小的磁片磁碟機。 使用不同大小的容量磁片可能會導致一些無法使用的容量，而且使用不同大小的快取磁片磁碟機可能無法改善快取效能。 如需詳細資訊，請參閱下一節。

   > [!WARNING]
   > 跨伺服器的容量磁片磁碟機大小不同可能會導致閒置的容量。

## <a name="understand-capacity-imbalance"></a>瞭解：容量不平衡

儲存空間直接存取在磁片磁碟機和跨伺服器之間的容量不平衡功能上穩定。 即使不平衡是嚴重的，所有專案仍會繼續作用。 不過，視數個因素而定，每個伺服器中無法使用的容量可能無法使用。

若要查看為何會發生這種情況，請考慮以下簡化的圖解。 每個彩色方塊都代表一個鏡像資料複本。 例如，標示為、' 和 ' ' 的方塊是相同資料的三個複本。 若要接受伺服器容錯，這些複本*必須*儲存在不同的伺服器中。

### <a name="stranded-capacity"></a>孤立的容量

如已繪製，伺服器1（10 TB）和伺服器2（10 TB）已滿。 伺服器3具有較大的磁片磁碟機，因此它的總容量較大（15 TB）。 不過，若要在伺服器3上儲存更多的三向鏡像資料，也需要伺服器1和伺服器2上的複本（已滿）。 伺服器3上剩餘的 5 TB 容量無法使用–它是「*閒置*」的容量。

![三向鏡像，三部伺服器，孤立容量](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>最佳放置

相反地，有四部 10 TB 的伺服器、10 TB、10 TB 和 15 TB 的容量，以及三向鏡像復原*功能*，您可以使用所繪製的所有可用容量，以有效的方式來進行複製。 當這種情況發生時，儲存空間直接存取配置器會尋找並使用最佳的放置，而不留下任何孤立的容量。

![三向鏡像，四部伺服器，沒有閒置的容量](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

伺服器的數目、復原、容量不平衡的嚴重性，以及其他因素都會影響是否有擱置的容量。 **最審慎的一般規則是假設只有每個伺服器中可用的容量才可供使用。**

## <a name="understand-cache-imbalance"></a>瞭解：快取不平衡

儲存空間直接存取在磁片磁碟機和跨伺服器之間快取不平衡的情況穩定。 即使不平衡是嚴重的，所有專案仍會繼續作用。 此外，儲存空間直接存取一律會完全使用所有可用的快取。

不過，使用不同大小的快取磁片磁碟機可能無法以一致或可預測的方式改善快取效能：只有 IO 才能以較大的快取磁片[磁碟機](understand-the-cache.md#server-side-architecture)進行系結，可能會發現效能 儲存空間直接存取會在系結之間平均分配 IO，而不會根據快取到容量的比率來區分。

![快取不平衡](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > 若要深入瞭解快取系結，請參閱[瞭解](understand-the-cache.md)快取。

## <a name="example-configurations"></a>範例設定

以下是一些支援和不支援的設定：

### <a name="supported-supported-different-models-between-servers"></a>![支援](media/drive-symmetry-considerations/supported.png) 支援：伺服器之間的不同模型

前兩部伺服器使用 NVMe 模型 "X"，但第三部伺服器使用 NVMe 模型 "Z"，這非常類似。

| 伺服器 1                    | 伺服器2                    | 伺服器3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x NVMe 模型 X （快取）    | 2 x NVMe 模型 X （快取）    | 2 x NVMe 模型 Z （快取）    |
| 10 x SSD 模型 Y （容量） | 10 x SSD 模型 Y （容量） | 10 x SSD 模型 Y （容量） |

這有受到支援。

### <a name="supported-supported-different-models-within-server"></a>![支援](media/drive-symmetry-considerations/supported.png) 支援：伺服器內的不同模型

每部伺服器都使用一些不同的 HDD 型號「Y」和「Z」，非常類似。 每部伺服器的 HDD 總計為10個。

| 伺服器 1                   | 伺服器2                   | 伺服器3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x SSD 模型 X （快取）    | 2 x SSD 模型 X （快取）    | 2 x SSD 模型 X （快取）    |
| 7 x HDD 型號 Y （容量） | 5 x HDD 型號 Y （容量） | 1 x HDD 型號 Y （容量） |
| 3 x HDD 型號 Z （容量） | 5 x HDD 型號 Z （容量） | 9 x HDD 型號 Z （容量） |

這有受到支援。

### <a name="supported-supported-different-sizes-across-servers"></a>![支援](media/drive-symmetry-considerations/supported.png) 支援：跨伺服器的不同大小

前兩部伺服器使用 4 TB HDD，但第三部伺服器使用非常類似的 6 TB HDD。

| 伺服器 1                | 伺服器2                | 伺服器3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe （快取） | 2 x 800 GB NVMe （快取） | 2 x 800 GB NVMe （快取） |
| 4 x 4 TB HDD （容量） | 4 x 4 TB HDD （容量） | 4 x 6 TB HDD （容量） |

這是支援的功能，不過它會導致閒置的容量。

### <a name="supported-supported-different-sizes-within-server"></a>![支援](media/drive-symmetry-considerations/supported.png) 支援：伺服器內的不同大小

每部伺服器都使用不同的 1.2 TB 混合和非常類似的 1.6 TB SSD。 每部伺服器都有4個 SSD。

| 伺服器 1                 | 伺服器2                 | 伺服器3                 |
|--------------------------|--------------------------|--------------------------|
| 3 x 1.2 TB SSD （快取）   | 2 x 1.2 TB SSD （快取）   | 4 x 1.2 TB SSD （快取）   |
| 1 x 1.6 TB SSD （快取）   | 2 x 1.6 TB SSD （快取）   | -                        |
| 20 x 4 TB HDD （容量） | 20 x 4 TB HDD （容量） | 20 x 4 TB HDD （容量） |

這有受到支援。

### <a name="unsupported-not-supported-different-types-of-drives-across-servers"></a>![不支援](media/drive-symmetry-considerations/unsupported.png) 不支援：跨伺服器的不同磁片磁碟機類型

伺服器1有 NVMe，但其他則沒有。

| 伺服器 1            | 伺服器2            | 伺服器3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe （快取）    | -                   | -                   |
| -                   | 6 x SSD （快取）     | 6 x SSD （快取）     |
| 18 x HDD （容量） | 18 x HDD （容量） | 18 x HDD （容量） |

這不受支援。 每一部伺服器中的磁片磁碟機類型應該相同。

### <a name="unsupported-not-supported-different-number-of-each-type-across-servers"></a>![不支援](media/drive-symmetry-considerations/unsupported.png) 不支援：跨伺服器的每個類型都有不同的數目

伺服器3所擁有的磁片磁碟機比其他數目還多。

| 伺服器 1            | 伺服器2            | 伺服器3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe （快取）    | 2 x NVMe （快取）    | 4 x NVMe （快取）    |
| 10 x HDD （容量） | 10 x HDD （容量） | 20 x HDD （容量） |

這不受支援。 每一部伺服器中的每個類型磁片磁碟機數目應該都相同。

### <a name="unsupported-not-supported-only-hdd-drives"></a>![不支援](media/drive-symmetry-considerations/unsupported.png) 不支援：僅 HDD 磁片磁碟機

所有伺服器都只有 HDD 磁片磁碟機連線。

|伺服器 1|伺服器2|伺服器3|
|-|-|-| 
|18 x HDD （容量） |18 x HDD （容量）|18 x HDD （容量）|

這不受支援。 您必須至少新增兩個連接到每部伺服器的快取磁片磁碟機（NvME 或 SSD）。

## <a name="summary"></a>摘要

回顧一下，叢集中的每部伺服器都應該有相同類型的磁片磁碟機，而且每種類型都有相同的數目。 根據上述考慮，支援混合和比對磁片磁碟機型號和磁片磁碟機大小。

| constraint                               |               |
|------------------------------------------|---------------|
| 每一部伺服器中的磁片磁碟機類型     | **必填**  |
| 每一部伺服器中的每個類型都有相同的數目 | **必填**  |
| 每一部伺服器中的相同磁片磁碟機型號        | 推薦項目   |
| 每一部伺服器中的相同磁片磁碟機大小         | 推薦項目   |

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取硬體需求](storage-spaces-direct-hardware-requirements.md)
- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)
