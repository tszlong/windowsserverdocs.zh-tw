---
title: 選擇儲存空間直接存取的磁碟機
description: 深入瞭解如何選擇儲存空間直接存取的磁片磁碟機。
ms.assetid: 1368bc83-9121-477a-af09-4ae73ac16789
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 07/01/2020
ms.localizationpriority: medium
ms.openlocfilehash: b84b177a6f73da1a74caa2c715cafc3c776aae59
ms.sourcegitcommit: 528bdff90a7c797cdfc6839e5586f2cd5f0506b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97977483"
---
# <a name="choosing-drives-for-storage-spaces-direct"></a>選擇儲存空間直接存取的磁碟機

> 適用於：Windows Server 2019、Windows Server 2016

本主題指引您如何選擇[儲存空間直接存取](storage-spaces-direct-overview.md)的磁碟機，以符合您的效能及容量需求。

## <a name="drive-types"></a>磁碟機類型

儲存空間直接存取目前適用于四種類型的磁片磁碟機：

| 磁片磁碟機類型 | 描述 |
| --- | --- |
| :::image type="content" source="media/understand-the-cache/pmem-100px.png" alt-text="PMem (持續性記憶體) 的影像 "::: | **持續性記憶體。** 一種新的低延遲、高效能儲存體。 |
| :::image type="content" source="media/understand-the-cache/NVMe-100px.png" alt-text="NVMe (非動態記憶體 Express) 的影像 "::: | **NVMe (非動態記憶體 Express) 。** 直接位於 PCIe 匯流排上的固態硬碟。 常見的板型規格為 2.5" U.2、PCIe Add-In-Card (AIC) 及 M.2。 NVMe 提供較高的 IOPS 和 IO 輸送量，其延遲低於目前支援的任何其他類型磁片磁碟機（持續性記憶體除外）。 |
| :::image type="content" source="media/understand-the-cache/SSD-100px.png" alt-text="SSD 磁片磁碟機) 的映射 "::: | **SSD.** 固態硬碟，可透過傳統 SATA 或 SAS 連接。 |
| :::image type="content" source="media/understand-the-cache/HDD-100px.png" alt-text="HDD) 的影像 "::: | **硬碟。** 磁片磁碟機的旋轉、磁片磁碟機，提供大量的儲存體容量。 |

## <a name="built-in-cache"></a>內建快取

儲存空間直接存取有內建的伺服器端快取。 它是大型持久的即時讀寫快取。 在使用多種磁碟機類型的部署，它會自動設定為使用「最快速」類型的所有磁碟機。 其餘磁碟機則作為容量磁碟機。

如需詳細資訊，請查看[了解儲存空間直接存取中的快取](understand-the-cache.md)。

## <a name="option-1--maximizing-performance"></a>選項 1 – 使效能最大化

若要在隨機讀取和寫入至任何資料之間達成可預測且一致的快速延遲，或達到高 IOPS (我們已在 [6000000](https://www.youtube.com/watch?v=0LviCzsudGY&t=28m)！ ) 或 IO 輸送量 (我們已完成 [1 Tb/s](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=16m50s)！ ) ，您應該移至「全快閃」。

目前有三種方式可做到：

![全快閃部署選項，以將效能最大化](media/choosing-drives-and-resiliency-types/All-Flash-Deployment-Possibilities.png)

1. **全部 NVMe。** 使用全 NVMe 可提供無與倫比的效能，包括最能夠預測的低延遲。 若您的磁碟機全為相同的模型，則沒有快取。 您也可混合較高耐力及較低耐力的 NVMe 模型，並將前者設定為替後者快取寫入 ([需要設定](understand-the-cache.md#manual-configuration))。

2. **NVMe + SSD。** 搭配 SSD 使用 NVMe，NVMe 會自動快取 SSD 的寫入。 這可讓寫入只在需要時於快取中進行聯合並取消暫存，以減少耗用 SSD。 這可提供類似 NVMe 的特性，而讀取會直接由同樣快速的 SSD 提供。

3. **所有 SSD。** 如同全 NVMe，若您的磁碟機全為相同的模型，則沒有快取。 若您混合較高耐力及較低耐力的模型，可將前者設定為替後者快取寫入 ([需要設定](understand-the-cache.md#manual-configuration))。

    > [!NOTE]
    > 使用全 NVMe 或全 SSD 而不使用快取有一項好處，即您可從所有裝置取得可使用的儲存容量。 容量無須用於快取，對規模較小者頗具吸引力。

## <a name="option-2--balancing-performance-and-capacity"></a>選項 2 – 平衡效能及容量

針對具有各種應用程式和工作負載的環境，有些部分具有嚴格的效能需求，而有些則需要大量的儲存體容量，您應該針對較大的 Hdd 使用 NVMe 或 Ssd 快取來進行「混合式」。

![用於平衡效能和容量的混合式部署選項](media/choosing-drives-and-resiliency-types/Hybrid-Deployment-Possibilities.png)

1. **NVMe + HDD**。 NVMe 磁碟機會透過快取讀取與寫入來加快兩者的速度。 快取讀取可讓 HDD 聚焦於寫入。 快取寫入會吸收高載，並允許寫入只在需要時以人工序列化的方式進行聯合並取消暫存，進而將 HDD IOPS 及 IO 輸送量最大化。 這會提供類似 NVMe 的寫入特性，而對於經常或最近讀取的資料也會提供類似 NVMe 的讀取特性。

2. **SSD + HDD**。 如同上述項目，SSD 會透過快取讀取與寫入來加快兩者的速度。 這會提供類似 SSD 的寫入特性，而對於經常或最近讀取的資料會提供類似 SSD 的讀取特性。

    另外還有一個外來選項：使用 *這三種* 類型的磁片磁碟機。

3. **NVMe + SSD + HDD。** 有了所有三種類型的磁碟機，NVMe 磁碟機會對 SSD 和 HDD 進行快取。 最吸引人的是，您可以在 Ssd 上建立磁片區，並在相同的叢集中並存硬碟的磁片區，並以 NVMe 加速。 前者完全形同「全快閃」部署，而後者完全形同「混合式」部署，如上所述。 在概念上形同擁有兩個集區，且具備極度的獨立容量管理及錯誤及修復循環等。

    > [!IMPORTANT]
    > 我們建議使用 SSD 層，將最重視效能的工作負載放在全快閃裝置上。

## <a name="option-3--maximizing-capacity"></a>選項 3 – 將容量最大化

對於需要大量容量和不常寫入的工作負載（例如封存、備份目標、資料倉儲或「非經常性存取」儲存體），您應該結合一些 Ssd 來快取與許多較大型 Hdd 的容量。

![用於將容量最大化的部署選項](media/choosing-drives-and-resiliency-types/maximizing-capacity.png)

1. **SSD + HDD**。 SSD 會快取讀取與寫入，以吸收高載並提供類似 SSD 的寫入效能，並於稍後對 HDD 進行最佳化的取消暫存。

>[!IMPORTANT]
>不支援只使用 Hdd 進行設定。 不建議將高耐用性 Ssd 快取至低耐用性 Ssd。

## <a name="sizing-considerations"></a>大小調整考量

### <a name="cache"></a>快取

每個伺服器都必須具備至少兩個快取磁碟機 (備援所需的下限)。 建議您讓容量磁碟機數為快取磁碟機數的倍數。 例如，如果您有四個快取磁片磁碟機，您將會遇到具有8個容量磁片磁碟機的一致效能 (1:2 比率) 超過7或9。

快取應該調整大小以容納您的應用程式和工作負載的工作集，例如在任何指定時間主動讀取和寫入的所有資料。 此外即無其他快取大小需求。 針對具有 Hdd 的部署，合理的起始位置是容量的10%，例如，如果每部伺服器都有 4 x 4 TB 的 HDD = 16 TB 的容量，則每一部伺服器的 2 x 800 GB SSD = 1.6 TB 的快取。 針對所有的快閃部署（特別是高耐用性） (https://techcommunity.microsoft.com/t5/storage-at-microsoft/understanding-ssd-endurance-drive-writes-per-day-dwpd-terabytes/ba-p/426024) ssd，您可以合理地開始更接近5% 的容量，例如，如果每部伺服器都有 24 x 1.2 TB 的 SSD = 28.8 TB 的容量，則每一伺服器 2 x 750 GB NVMe = 1.5 TB 的快取。 您稍後隨時都可新增或移除快取磁碟機進行調整。

### <a name="general"></a>一般

建議您將每部伺服器的總儲存容量限制為大約 400 tb (TB) 。 每個伺服器的儲存容量愈多，停機或重新開機後 (如套用軟體更新) 重新同步資料所需花費的時間愈長。 每個存放集區目前的大小上限為 4 pb (PB)  (4000 TB) 適用于 Windows Server 2019，或 1 pb （適用于 Windows Server 2016）。

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取概觀](storage-spaces-direct-overview.md) \(部分機器翻譯\)
- [了解儲存空間直接存取中的快取](understand-the-cache.md)
- [儲存空間直接存取硬體需求](storage-spaces-direct-hardware-requirements.md)
- [規劃儲存空間直接存取中的磁碟區](plan-volumes.md)
- [容錯與儲存空間效率](storage-spaces-fault-tolerance.md)
