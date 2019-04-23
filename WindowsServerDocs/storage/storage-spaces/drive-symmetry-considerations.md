---
title: 磁碟機儲存空間直接存取的對稱性的考量
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
Keywords: 儲存空間直接存取
ms.localizationpriority: medium
ms.openlocfilehash: 629e49a0c1919286d8e4f418b3e99d69e720f4fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866879"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>磁碟機儲存空間直接存取的對稱性的考量 

> 適用於：Windows Server 2019，Windows Server 2016

[儲存空間直接存取](storage-spaces-direct-overview.md)時每一部伺服器具有完全相同的磁碟機的效果最佳。

事實上，我們認為這不一定總是可行：儲存空間直接存取被設計來執行年和小數位數，隨著您組織的需要增長。 現在，您可能會購買 spacious 3TB 硬碟;明年，您可能無法找到的太小。 因此，支援一定程度的混合和-比對。

本主題說明的條件約束，並提供支援和不支援的組態範例。

## <a name="constraints"></a>限制式

### <a name="type"></a>類型

所有伺服器都應該都有相同[類型的磁碟機](choosing-drives.md#drive-types)。

例如，如果一部伺服器具有 NVMe，它們應該*所有*有 NVMe。

### <a name="number"></a>Number

所有伺服器都應該都有相同數目的每個類型的磁碟機。

例如，如果一部伺服器有六個 SSD，它們應該*所有*有六個 SSD。

   > [!NOTE]
   > 它是沒問題暫時發生故障時或在新增或移除磁碟機不同的磁碟機數目。

### <a name="model"></a>型號

我們建議使用磁碟機的韌體版本，請盡可能與相同的模型。 如果無法處理，請小心選取盡可能相似的磁碟機。 我們建議不要混合及符合具有相當不同的效能或耐力特性之相同類型的磁碟機 （除非其中一個是快取和另一個則是容量） 因為儲存空間直接存取的平均分散 IO，並不會區分根據模型.

   > [!NOTE]
   > 它是可以混合與比對類似 SATA 和 SAS 磁碟機。

### <a name="size"></a>大小

我們建議使用的相同大小的磁碟機。 使用不同大小的容量磁碟機可能會導致某些無法使用的容量，並使用不同大小的快取磁碟機可能無法改善快取效能。 請參閱下一節，如需詳細資訊。

   > [!WARNING]
   > 在伺服器之間的不同容量磁碟機大小可能會導致擱置的容量。

## <a name="understand-capacity-imbalance"></a>了解： 容量負載失衡的影響

儲存空間直接存取是強固的容量不平衡跨磁碟機和跨伺服器。 即使不平衡的狀態為嚴重，所有項目將會繼續運作。 不過，根據許多因素，在每一部伺服器中無法使用的容量可能會無法使用。

若要查看發生的原因，請考慮下面的的簡化的圖例。 每個彩色的方塊都代表一份鏡像資料。 比方說，方塊標示為 A、 A'，和 ' 是相同資料的三個複本。 若要接受伺服器的容錯功能，這些複本*必須*儲存在不同的伺服器。

### <a name="stranded-capacity"></a>擱置的容量

繪製，伺服器 1 (10 TB) 和伺服器 2 (10 TB) 已滿。 伺服器 3 具有較大的磁碟機，因此其總容量較大的 (15 TB)。 不過，伺服器 3 上儲存更多的三向鏡像資料需要在伺服器 1 」 和 「 伺服器 2 上的複本，這些區塊已滿。 無法使用伺服器 3 上的剩餘 5 TB 的容量一樣，它是 *「 受困 」* 容量。

![三向鏡像，三部伺服器，受困的容量](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>最佳的位置

相反地，使用四部伺服器的 10 TB，10 TB，10 TB，15 TB 容量和三向鏡像復原時，它*是*能夠有效地將複本放在使用所有可用的容量，以繪製方式。 這是可行的每當的儲存空間直接存取的配置器會尋找，並使用最佳的位置，留下任何擱置的容量。

![三向鏡像、 四部伺服器、 任何擱置的容量](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

伺服器、 復原功能、 容量不平衡的狀態，以及其他因素的嚴重性的數目會影響是否有擱置的容量。 **最審慎的一般規則是假設每一部伺服器中可用的唯一容量保證能夠使用。**

## <a name="understand-cache-imbalance"></a>了解： 快取不平衡

儲存空間直接存取是強固快取不平衡跨磁碟機和跨伺服器。 即使不平衡的狀態為嚴重，所有項目將會繼續運作。 此外，儲存空間直接存取一律會使用到最滿所有可用的快取。

不過，使用不同大小的快取磁碟機可能無法改善快取效能一致的方式或透過可預測方式： 只 IO[磁碟機繫結](understand-the-cache.md#server-side-architecture)具有較大的快取磁碟機可能會看見改善的效能。 儲存空間直接存取 IO 平均分散繫結，並不會區分根據快取-容量的比例。

![快取不平衡](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > 請參閱[了解快取](understand-the-cache.md)若要深入了解快取繫結。

## <a name="example-configurations"></a>範例組態

以下是一些支援和不支援的組態：

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-between-servers"></a>![支援](media/drive-symmetry-considerations/supported.png) 支援： 伺服器之間的不同模型

前兩個伺服器使用 NVMe 模型"X"，但第三個伺服器使用 NVMe 模型"Z"，這是非常類似。

| 伺服器 1                    | 伺服器 2                    | 伺服器 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| （快取） 的 2 倍 NVMe 模型 X    | （快取） 的 2 倍 NVMe 模型 X    | （快取） 的 2 倍 NVMe 模型 Z    |
| （容量） 的 10 倍的 SSD 模型 Y | （容量） 的 10 倍的 SSD 模型 Y | （容量） 的 10 倍的 SSD 模型 Y |

這項支援。

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-within-server"></a>![支援](media/drive-symmetry-considerations/supported.png) 支援： 伺服器內的不同模型

每一部伺服器會使用一些不同的"Y"和"Z"，非常類似的 HDD 模型組合。 每一部伺服器具有 10 個總 HDD。

| 伺服器 1                   | 伺服器 2                   | 伺服器 3                   |
|----------------------------|----------------------------|----------------------------|
| （快取） 的 2 倍 SSD 模型 X    | （快取） 的 2 倍 SSD 模型 X    | （快取） 的 2 倍 SSD 模型 X    |
| 7 倍 HDD 模型 Y （容量） | 5 x HDD 模型 Y （容量） | 1 倍的 HDD 模型 Y （容量） |
| （容量） 的 3 倍 HDD 模型 Z | （容量） 的 5 倍的 HDD 模型 Z | 9 倍的 HDD 模型 Z （容量） |

這項支援。

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-across-servers"></a>![支援](media/drive-symmetry-considerations/supported.png) 支援： 在伺服器之間的不同大小

前兩個伺服器使用 4 TB HDD，但第三個伺服器會使用非常類似 6 TB HDD。

| 伺服器 1                | 伺服器 2                | 伺服器 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 GB NVMe （快取） | 2 x 800 GB NVMe （快取） | 2 x 800 GB NVMe （快取） |
| 4 x 4 TB HDD （容量） | 4 x 4 TB HDD （容量） | 4 x 6 TB HDD （容量） |

這被支援，但它會導致擱置的容量。

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-within-server"></a>![支援](media/drive-symmetry-considerations/supported.png) 支援： 伺服器內的不同大小

每一部伺服器會使用一些不同的 1.2 TB，而非常類似 1.6 TB SSD 組合。 每一部伺服器有 4 個總和的 SSD。

| 伺服器 1                 | 伺服器 2                 | 伺服器 3                 |
|--------------------------|--------------------------|--------------------------|
| 3 x 1.2 TB SSD （快取）   | 2 x 1.2 TB SSD （快取）   | 4 x 1.2 TB SSD （快取）   |
| 1 x 1.6 TB SSD （快取）   | 2 x 1.6 TB SSD （快取）   | -                        |
| 20 x 4TB HDD （容量） | 20 x 4TB HDD （容量） | 20 x 4TB HDD （容量） |

這項支援。

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-types-of-drives-across-servers"></a>![不支援](media/drive-symmetry-considerations/unsupported.png) 不支援： 不同類型的伺服器上的磁碟機

伺服器 1 NVMe，但其他人。

| 伺服器 1            | 伺服器 2            | 伺服器 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe （快取）    | -                   | -                   |
| -                   | 6 x SSD （快取）     | 6 x SSD （快取）     |
| 18 x HDD （容量） | 18 x HDD （容量） | 18 x HDD （容量） |

不支援這個狀況。 類型的磁碟機應該是相同的每一部伺服器。

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-number-of-each-type-across-servers"></a>![不支援](media/drive-symmetry-considerations/unsupported.png) 不支援： 在伺服器之間的每種類型的不同數目

伺服器 3 具有比其他更多的磁碟機。

| 伺服器 1            | 伺服器 2            | 伺服器 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe （快取）    | 2 x NVMe （快取）    | 4 x NVMe （快取）    |
| 10 x HDD （容量） | 10 x HDD （容量） | 20 x HDD （容量） |

不支援這個狀況。 每個類型的磁碟機數目應該是相同的每一部伺服器。

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-only-hdd-drives"></a>![不支援](media/drive-symmetry-considerations/unsupported.png) 不支援： 僅 HDD 磁碟機

所有伺服器都具有僅連接的 HDD 磁碟機。

|伺服器 1|伺服器 2|伺服器 3|
|-|-|-| 
|18 x HDD （容量） |18 x HDD （容量）|18 x HDD （容量）|

不支援這個狀況。 您要新增的兩個快取磁碟機 （NvME 或 SSD） 附加至每個伺服器的最小值。

## <a name="summary"></a>總結

若要總而言之，在叢集中的每一部伺服器應該具有相同類型的磁碟機和相同數目的每個型別。 它支援混合和比對磁碟機模型和磁碟機大小如有需要但有上述考量事項。

| 條件約束                               |               |
|------------------------------------------|---------------|
| 相同類型的每一部伺服器中的磁碟機     | **所需**  |
| 相同數目的每種類型的每一部伺服器 | **所需**  |
| 每一部伺服器中的相同磁碟機模型        | 建議   |
| 每一部伺服器中的相同磁碟機大小         | 建議   |

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取硬體需求](storage-spaces-direct-hardware-requirements.md)
- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)
