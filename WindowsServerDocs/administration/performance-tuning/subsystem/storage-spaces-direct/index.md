---
title: 儲存空間直接存取的效能調整
description: 「儲存空間直接存取」會自動根據您使用之硬體的快取設定自動調整其效能，如此主題所描述。
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.assetid: 15a519fa-37cc-4d84-a9fe-097d33bb71ea
author: phstee
ms.author: Vshankar; DanLo; clausjor; StevenEk
ms.date: 4/14/2017
ms.openlocfilehash: dabfadb30666ec93aa36985e2bc55a3f496e6d34
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383468"
---
# <a name="performance-tuning-for-storage-spaces-direct"></a>儲存空間直接存取的效能調整

「儲存空間直接存取」是一個 Windows Server 型軟體解決方案，它會自動調整其效能，讓您不需要手動指定資料行計數、您使用之硬體的快取設定，以及必須搭配共用 SAS 存放裝置解決方案手動設定的其他因素。 如需背景資訊，請參閱 [Windows Server 2016 中的儲存空間直接存取](../../../../storage/storage-spaces/storage-spaces-direct-overview.md)。

「儲存空間直接存取軟體存放裝置匯流排快取」會自動根據系統中存在的存放裝置類型來設定。 辨識的三個類型：**HDD**、**SSD** 與 **NVMe**。 快取會視需要為讀取和/或寫入快取要求最快的存放裝置，並為資料的永續性存放使用較慢的存放裝置。

下表摘要說明預設值：

| 存放裝置類型 | 快取設定 |
| --- | --- |
| 任何單一類型 | 若只有一種類型的存放裝置存在，則不會設定「軟體存放裝置匯流排快取」。 |
| SSD+HDD 或 NVMe+HDD | 系統會將最快的存放裝置設定為快取層，並同時快取讀取與寫入。 |
| SSD+SSD 或 NVMe+NVMe | 系統會將這些快速+快速選項設定為以較高與較低耐力存放裝置的組合為目標，例如使用每天 (DWPD) NAND 快閃記憶體 SSD 10 個磁碟機寫入用於快取，並將 1.5 DWPD NAND 快閃記憶體 SSD 用於容量。 它們是透過為「儲存空間直接存取」賦予一組用於識別快取裝置的模型字串所啟用。 如需詳細資訊，請參閱 [Enable-StorageSpacesDirect](https://technet.microsoft.com/library/mt589697.aspx) Cmdlet 參考 (`CacheDeviceModel`)。 <br><br>在快速+快速系統中，只會快取寫入。 不會快取讀取。 |

請注意，SSD 或 NVMe 裝置上的快取預設只會寫入快取。 目的是既然容量裝置夠快，將讀取內容移至快取裝置只能帶來有限的價值。 在有些情況中則不是這樣，雖然您必須注意一些事項，因未啟用讀取快取可能會意外取用快取裝置耐力，但效能並未提高。 範例可能包括：

* **NVme+SSD** 啟用讀取快取將會允許讀取 IO 利用 PCIe 連線的優點和/或 NVMe 裝置的較高 IOPS 效能 (相較於彙總的 SSD)。 <br>由於 NVMe 裝置相較於連線到 SSD 之 HBA 的相對頻寬容量，這對頻寬導向案例而言可能  也成立。 這對在實現提高的效能之前 IOPS 的 CPU 成本可能限制系統的 IOPS 導向案例而言可能  不成立。
* **NVMe+NVMe** 同樣地，若快取 NVMe 的讀取容量大於結合容量 NVMe，啟用讀取快取或許可以帶來價值。 <br>這些設定中讀取快取的良好案例不常見。

若要檢視並修改快取設定，請使用 [Get-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt634616.aspx) 與 [Set-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt763265.aspx) Cmdlet。 `CacheModeHDD` 與 `CacheModeSSD` 屬性定義快取如何在指定類型的容量媒體上作業。

## <a name="see-also"></a>請參閱

- [了解儲存空間直接存取](../../../../storage/storage-spaces/understand-storage-spaces-direct.md)
- [規劃儲存空間直接存取](../../../../storage/storage-spaces/plan-storage-spaces-direct.md)
- [檔案伺服器的效能調整](../../role/file-server/index.md)
- [軟體定義存放裝置設計考量指南](https://technet.microsoft.com/library/mt243829.aspx) (適用於 Windows Server 2012 R2 與共用 SAS 存放裝置)
