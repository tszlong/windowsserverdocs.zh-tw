---
title: 儲存空間直接存取中的容錯和儲存效率
ms.prod: windows-server
ms.author: cosmosdarwin
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/11/2017
ms.assetid: 5e1d7ecc-e22e-467f-8142-bad6d82fc5d0
description: 討論儲存空間直接存取中的復原選項，包括鏡像和同位。
ms.localizationpriority: medium
ms.openlocfilehash: b64592bf3cf5659410dcbbeb4c190d2d6a85485a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859011"
---
# <a name="fault-tolerance-and-storage-efficiency-in-storage-spaces-direct"></a>儲存空間直接存取中的容錯和儲存效率

>適用於︰Windows Server 2016

本主題介紹[儲存空間直接存取](storage-spaces-direct-overview.md)中可用的復原選項，並概述各選項的規模需求、儲存效率，以及一般的優點和取捨。 文中也會提供一些使用上的指示供您入門，並且會參考一些可讓您進行深入了解的優質文章、部落格和其他內容。

如果您已熟悉儲存空間，您可以跳到[摘要](#summary)一節。

## <a name="overview"></a>概觀

事實上，儲存空間是要為您的資料提供容錯 (通常稱為「復原」)。 其實作類似 RAID，但差在會分散於伺服器，並實作於軟體中。

和 RAID 一樣，儲存空間有幾個不同方式可執行此操作，這些方式會在容錯、儲存效率與計算複雜度之間做出不同取捨。 其大致分為兩種類別：「鏡像」和「同位」，後者有時也稱為「清除編碼」。

## <a name="mirroring"></a>鏡像

鏡像會藉由為所有資料保留多個複本來提供容錯。 其行為最像 RAID-1。 資料如何等量分割和放置是相當複雜的 (請參閱[這篇部落格文章](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/)以深入了解)，但絕對正確的說法是：使用鏡像儲存的任何資料都會完整地撰寫多次。 每個複本都會寫入到應該只會個別發生故障的不同實體硬體 (不同伺服器中的不同磁碟機)。

在 Windows Server 2016 中，儲存空間會提供兩種鏡像：「雙向」和「三向」。

### <a name="two-way-mirror"></a>雙向鏡像

雙向鏡像會為所有內容寫入兩個複本。 其儲存效率是 50%，因此若要撰寫 1 TB 的資料，您會需要至少 2 TB 的實體儲存容量。 同樣地，您也需要至少兩個[硬體「容錯網域」](../../failover-clustering/fault-domains.md)，在使用儲存空間直接存取時，這表示需要兩部伺服器。

![two-way-mirror](media/Storage-Spaces-Fault-Tolerance/two-way-mirror-180px.png)

   >[!WARNING]
   > 如果您有兩個以上伺服器，建議您改為使用三向鏡像。

### <a name="three-way-mirror"></a>三向鏡像

三向鏡像會為所有內容寫入三個複本。 其儲存效率是 33.3%，因此若要撰寫 1 TB 的資料，您會需要至少 3 TB 的實體儲存容量。 同樣地，您也需要至少三個硬體「容錯網域」，在使用儲存空間直接存取時，這表示需要三部伺服器。

三向鏡像可安全地[一次容許至少兩個硬體問題（磁碟機或伺服器）](#examples)。 例如，如果您重新開機一部伺服器，同時突然另一部磁碟機或伺服器故障，所有資料會保持安全，持續可供存取。

![three-way-mirror](media/Storage-Spaces-Fault-Tolerance/three-way-mirror-180px.png)

## <a name="parity"></a>Parity

同位編碼 (通常稱為「清除編碼」) 會使用位元算術提供容錯，而這會變得[非常複雜](https://www.microsoft.com/research/wp-content/uploads/2016/02/LRC12-cheng20webpage.pdf)。 相較於鏡像，同位的運作方式較不容易理解，但有許多絕佳的線上資源 (例如，這個由第三方提供的[清除編碼傻瓜指南](http://smahesh.com/blog/2012/07/01/dummies-guide-to-erasure-coding/)) 可協助您了解。 可以這麼說，它提供更好的儲存效率，卻又不會犧牲容錯能力。

在 Windows Server 2016 中，儲存空間會提供兩種同位：「單同位」和「雙同位」，後者採用稱為「本機重建程式碼」的較大規模進階技術。

> [!IMPORTANT]
> 我們建議使用鏡像處理大部分易受效能影響的工作負載。 若要深入了解如何根據您的工作負載來平衡效能與產能，請參閱[規劃磁碟區](plan-volumes.md#choosing-the-resiliency-type)。

### <a name="single-parity"></a>單同位
單同位只會保留一個位元同位符號，因此一次只能對一個故障提供容錯。 其行為最像 RAID-5。 若要使用單同位，您需要至少三個硬體「容錯網域」，在使用儲存空間直接存取時，這表示需要三部伺服器。 因為三向鏡像可在相同規模提供更多容錯能力，我們不建議您使用單同位。 但如果您堅持要使用也沒問題，而且會受到完全支援。

   >[!WARNING]
   > 我們不建議使用單同位，因為它一次只能安全地容許一個硬體故障：如果您重新開機一部伺服器，同時突然另一部磁碟機或伺服器故障，就會發生停機。 如果您只有三部伺服器，建議您使用三向鏡像。 如果您有四部以上，請參閱下一節。

### <a name="dual-parity"></a>雙同位

雙同位會實作 Reed-Solomon 錯誤修正程式碼以保留兩個位元同位符號，進而提供和三向鏡像相同的容錯能力 (也就是一次最多兩個故障)，但此方式擁有更好的儲存效率。 其行為最像 RAID-6。 若要使用雙同位，您需要至少四個硬體「容錯網域」，在使用儲存空間直接存取時，這表示需要四部伺服器。 在此規模下，其儲存效率是 50%，因此若要儲存 2 TB 的資料，您需要至少 4 TB 的實體儲存容量。

![dual-parity](media/Storage-Spaces-Fault-Tolerance/dual-parity-180px.png)

雙同位的儲存效率會讓您增加更多硬體容錯網域 (從 50% 增加為 80%)。 例如，在七個時 (在使用儲存空間直接存取時，這表示需要七部伺服器)，效率會躍升至 66.7%，若要儲存 4 TB 資料，您只需要 6 TB 的實體儲存容量。

![dual-parity-wide](media/Storage-Spaces-Fault-Tolerance/dual-parity-wide-180px.png)

如需各種規模的雙同位效率和本機重建程式碼，請參閱[摘要](#summary)一節。

### <a name="local-reconstruction-codes"></a>本機重建程式碼

Windows Server 2016 的儲存空間引進了 Microsoft Research 所開發的進階技術，其名稱為「本機重建程式碼」，簡稱 LRC。 在大規模的環境中，雙同位會使用 LRC 將其編碼/解碼分割為幾個較小的群組，以降低進行寫入或從故障中復原所需的負荷。

使用硬碟 (HDD) 時，群組大小是四個符號；使用固態硬碟 (SSD) 時，群組大小是六個符號。 例如，以下是具有硬碟和 12 個硬體容錯網域 (亦即 12 部伺服器) 之配置的可能樣貌，其中有兩組四個一群的資料符號。 此方式可達到 72.7% 儲存效率。

![local-reconstruction-codes](media/Storage-Spaces-Fault-Tolerance/local-reconstruction-codes-180px.png)

建議您閱讀這篇深入淺出的逐步解說：[本機重建程式碼如何處理各種故障案例，以及它們吸引人的原因](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/)，作者是我們公司的 [Claus Joergensen](https://twitter.com/clausjor)。

## <a name="mirror-accelerated-parity"></a>鏡像加速的同位

從 Windows Server 2016 起，一個儲存空間直接存取磁碟區可以部分鏡像、部分同位。 寫入首先登陸鏡像部分，稍後逐漸移動至同位部分。 實際上，這是[使用鏡像加速清除編碼](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/)。

若要混合使用三向鏡像與雙同位，您需要至少四個容錯網域，亦即四部伺服器。

鏡像加速同位的儲存效率介於全部使用鏡像與全部使用同位之間，並取決於您選擇的混合比例。 例如，本簡報 37 分鐘處的示範顯示具有 12 部伺服器、[達到 46%、54% 和 65% 效率的各種混合](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s)。

> [!IMPORTANT]
> 我們建議使用鏡像處理大部分易受效能影響的工作負載。 若要深入了解如何根據您的工作負載來平衡效能與產能，請參閱[規劃磁碟區](plan-volumes.md#choosing-the-resiliency-type)。

## <a name="summary"></a><a name="summary"></a>總計

本節摘要說明儲存空間直接存取中可用的復原類型、使用各種類型的最小規模需求、每種類型可容許多少故障，以及對應的儲存效率。

### <a name="resiliency-types"></a>復原類型

|    復原          |    容錯       |    儲存效率      |
|------------------------|----------------------------|----------------------------|
|    雙向鏡像      |    1                       |    50.0%                   |
|    三向鏡像    |    2                       |    33.3%                   |
|    雙同位         |    2                       |    50.0% - 80.0%           |
|    Mixed               |    2                       |    33.3% - 80.0%           |

### <a name="minimum-scale-requirements"></a>最小規模需求

|    復原          |    最小必要容錯網域   |
|------------------------|-------------------------------------|
|    雙向鏡像      |    2                                |
|    三向鏡像    |    3                                |
|    雙同位         |    4                                |
|    Mixed               |    4                                |

   >[!TIP]
   > 除非您使用[底座或機架容錯](../../failover-clustering/fault-domains.md)，否則容錯網域的數目就是指伺服器數目。 只要您符合儲存空間直接存取的最小需求，每部伺服器中的磁碟機數目就不會影響您所能使用的復原類型。 

### <a name="dual-parity-efficiency-for-hybrid-deployments"></a>混合式部署的雙同位效率

下表顯示同時包含硬碟 (HDD) 和固態硬碟 (SSD) 的混合式部署中，雙同位和本機重建程式碼在各種規模的儲存效率。

|    容錯網域      |    配置           |    效率   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50.0%        |
|    5                  |    RS 2+2           |    50.0%        |
|    6                  |    RS 2+2           |    50.0%        |
|    7                  |    RS 4+2           |    66.7%        |
|    8                  |    RS 4+2           |    66.7%        |
|    9                  |    RS 4+2           |    66.7%        |
|    10                 |    RS 4+2           |    66.7%        |
|    11                 |    RS 4+2           |    66.7%        |
|    12                 |    LRC (8, 2, 1)    |    72.7%        |
|    13                 |    LRC (8, 2, 1)    |    72.7%        |
|    14                 |    LRC (8, 2, 1)    |    72.7%        |
|    15                 |    LRC (8, 2, 1)    |    72.7%        |
|    16                 |    LRC (8, 2, 1)    |    72.7%        |

### <a name="dual-parity-efficiency-for-all-flash-deployments"></a>全快閃部署的雙同位效率

下表顯示只包含固態硬碟 (SSD) 的全快閃部署中，雙同位和本機重建程式碼在各種規模的儲存效率。 在全快閃組態中，同位配置可使用較大的群組大小，並達到更好的儲存效率。

|    容錯網域      |    配置           |    效率   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50.0%        |
|    5                  |    RS 2+2           |    50.0%        |
|    6                  |    RS 2+2           |    50.0%        |
|    7                  |    RS 4+2           |    66.7%        |
|    8                  |    RS 4+2           |    66.7%        |
|    9                  |    RS 6+2           |    75.0%        |
|    10                 |    RS 6+2           |    75.0%        |
|    11                 |    RS 6+2           |    75.0%        |
|    12                 |    RS 6+2           |    75.0%        |
|    13                 |    RS 6+2           |    75.0%        |
|    14                 |    RS 6+2           |    75.0%        |
|    15                 |    RS 6+2           |    75.0%        |
|    16                 |    LRC (12, 2, 1)   |    80.0%        |

## <a name="examples"></a><a name="examples"></a>典型

除非您只有兩部伺服器，否則建議您使用三向鏡像和/或雙同位，因為它們提供較好的容錯能力。 具體而言，它們可確保即使兩個錯誤網域 (在使用儲存空間直接存取時，這表示兩部伺服器) 都受到同時故障的影響，所有資料仍可保持安全並持續可供存取。

### <a name="examples-where-everything-stays-online"></a>全都保持上線的範例

這六個範例示範三向鏡像和/或雙同位**可以**容許的情形。

- **1.**    失去一個磁碟機 (包括快取磁碟機)
- **2.**    失去一部伺服器

![fault-tolerance-examples-1-and-2](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-12.png)

- **3.**    失去一部伺服器和一個磁碟機
- **4.**    失去不同伺服器中的兩個磁碟機

![fault-tolerance-examples-3-and-4](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-34.png)

- **5.**    只要最多兩部伺服器受到影響，就會失去兩個以上的磁碟機
- **6.**    失去兩部伺服器

![fault-tolerance-examples-5-and-6](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-56.png)

...在每個案例中，所有磁碟區都會保持上線。 (請確定您的叢集會保持仲裁)。

### <a name="examples-where-everything-goes-offline"></a>全都離線的範例

儲存空間在其存留期內可容許任何數目的故障，因為只要時間足夠，它就會在每次故障後還原成完整復原能力。 不過，任何特定時刻最多只會有兩個錯誤網域可安全地受到故障影響。 因此，下列是三向鏡像和/或雙同位**無法**容許之情形的範例。

- **7.** 一次失去三部以上伺服器中的磁碟機
- **8.** 一次失去三部以上的伺服器

![fault-tolerance-examples-7-and-8](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-78.png)

## <a name="usage"></a>使用方式

請查看[建立儲存空間直接存取中的磁碟區](create-volumes.md)。

## <a name="see-also"></a>另請參閱

下列每個連結都內嵌在本主題的內文中。

- [Windows Server 2016 中的儲存空間直接存取](storage-spaces-direct-overview.md)
- [Windows Server 2016 中的容錯網域感知](../../failover-clustering/fault-domains.md)
- [在 Azure 中由 Microsoft Research 進行抹除編碼](https://www.microsoft.com/research/publication/erasure-coding-in-windows-azure-storage/)
- [本機重建代碼和加速同位磁片區](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/)
- [存放裝置管理 API 中的磁片區](https://blogs.technet.microsoft.com/filecab/2016/08/29/deep-dive-volumes-in-spaces-direct/)
- [Microsoft Ignite 2016 的儲存體效率示範](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s)
- [儲存空間直接存取的容量計算機預覽](https://aka.ms/s2dcalc)
