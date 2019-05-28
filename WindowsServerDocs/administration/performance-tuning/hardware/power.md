---
title: 伺服器硬體能力的考量
description: 伺服器硬體能力的考量
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5fe91888188796c96d5da80e8f9bd3ed627b9d43
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564735"
---
# <a name="server-hardware-power-considerations"></a>伺服器硬體能力的考量

請務必辨識增加企業和資料中心環境中的能源效率的重要性。 高效能和低能源使用情形通常是衝突的目標，但仔細選取伺服器元件，您可以得到它們之間的正確平衡點。 下列各節會列出 power 特性和功能的伺服器硬體元件的指導方針。

## <a name="processor-recommendations"></a>處理器建議事項

作業系統電壓、 快取大小，以及處理程序技術的頻率會影響處理器的能源耗用量。 處理器具有熱設計，點 (TDP) 提供的能源耗量，相對於其他模型的基本指示的評等。

一般情況下，選擇符合您的效能目標的最低 tdp，才能在處理器。 此外，較新的層代的處理器通常較多的能源效率，而且它們可能會公開更多的電源狀態，適用於 Windows 的電源管理演算法，可讓所有層級的效能更好的電源管理。 或者，可能會使用一些新的 「 合作 」 的電源管理技術，Microsoft 已開發出與硬體製造商協力合作。

如需合作式電源管理技術的詳細資訊，請參閱 > 裡面的共同作業中的處理器效能控制項[進階組態與電源介面規格](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf)。


## <a name="memory-recommendations"></a>記憶體建議
記憶體的總系統電源增加分數的帳戶。 許多因素會影響記憶體 DIMM，例如記憶體內部技術、 錯誤修正碼 (ECC)、 匯流排頻率、 容量、 密度和陣序規範數目的能源耗用量。 因此，最好是記憶體的比較預期的電源評等，再決定購買大量。

低電源的記憶體現已推出，但您必須考慮效能和成本的取捨。 如果您的伺服器將分頁，您應該也會納入分頁磁碟的能源成本。


## <a name="disks-recommendations"></a>磁碟的建議
較高的 RPM 表示增加的能源耗用量。 SSD 磁碟機是更強大的高效率比旋轉式磁碟機。 此外，2.5 英吋磁碟機通常需要較少的電量比 3.5 英吋磁碟機。

## <a name="network-and-storage-adapter-recommendations"></a>網路和儲存裝置介面卡建議
某些配接器會降低在閒置期間的能源耗用量。 這是很重要的考量，10 Gb 網路介面卡和高頻寬 (4-8 Gb) 儲存體的連結。 這類裝置會消耗大量的能源。


## <a name="power-supply-recommendations"></a>電源供應器建議
改善電源供應器的效率是減少能源耗用量，而不會影響效能的好方法。 高效率的電源供應器可以儲存許多 kilowatt-hours 每年每一部伺服器。


## <a name="fan-recommendations"></a>風扇建議
風扇、 電源供應器，如是，您可以減少能源消耗，而不會影響系統效能的區域。 變數速度的愛用者可以降低系統的負載減少，消除否則不必要的能源耗用量 RPM。


## <a name="usb-devices-recommendations"></a>建議的 USB 裝置
Windows Server 2016 啟用選擇性暫停 USB 裝置的預設值。 不過，撰寫不夠周全的裝置驅動程式可能仍然會中斷系統能源效率由相當的邊界。 若要避免潛在的問題，請中斷連接 USB 裝置、 停用它們在 BIOS 中，或選擇不需要 USB 裝置的伺服器。


## <a name="remotely-managed-power-strip-recommendations"></a>從遠端管理電源區域建議
電源不是伺服器硬體中不可或缺的一部分，但可以讓資料中心很大的差異。 度量顯示磁碟區的伺服器已插入，但有明顯的電源已關閉，可能仍然需要最多 30 瓦的電力。

若要避免浪費電力，您可以部署遠端受管理的電源線的每個伺服器，以程式設計方式將電源中斷特定的伺服器機架。

## <a name="processor-terminology"></a>處理器術語
在本主題中所使用的處理器術語，會反映在下圖中，您可以使用元件的階層。 所用的元件的最小資料粒度從最大詞彙如下所示：

-   處理器通訊端
-   NUMA 節點
-   核心
-   邏輯處理器

![處理器術語](../media/perftune-guide-figure-1.png)

## <a name="see-also"></a>另請參閱
- [伺服器硬體的效能考量](index.md)
- [電源與效能調整](power/power-performance-tuning.md)
- [處理器電源管理](power/processor-power-management-tuning.md)
- [建議的平衡方案參數](power/recommended-balanced-plan-parameters.md)
