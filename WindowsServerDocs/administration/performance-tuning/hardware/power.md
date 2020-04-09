---
title: 伺服器硬體電源考慮
description: 伺服器硬體電源考慮
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: qizha;tristanb
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 865899e5f33bde97dff97efaff6010b95aafd3e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851981"
---
# <a name="server-hardware-power-considerations"></a>伺服器硬體電源考慮

請務必辨識企業和資料中心環境中，能源效率增加的重要性。 高效能和低能源使用量通常是衝突的目標，但藉由謹慎選取伺服器元件，您可以在兩者之間達到正確的平衡。 下列各節列出伺服器硬體元件的電源特性和功能的指導方針。

## <a name="processor-recommendations"></a>處理器建議事項

頻率、操作電壓、快取大小和處理技術會影響處理器的能源消耗。 處理器具有散熱設計點（TDP）分級，可提供相對於其他型號之能源耗用量的基本指示。

一般而言，選擇符合您的效能目標的最低 TDP 處理器。 此外，較新的處理器層級通常會比能源效率更高，而且它們可能會公開更多 Windows 電源管理演算法的電源狀態，以提供更佳的電源管理。 或者，他們也可以使用 Microsoft 與硬體製造商合作開發的一些新的「合作」電源管理技術。

如需有關合作電源管理技術的詳細資訊，請參閱[先進設定和電源介面規格](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf)中的「共同處理處理器效能控制」一節。


## <a name="memory-recommendations"></a>記憶體建議
記憶體帳戶會增加整體系統能力的分數。 許多因素會影響記憶體 DIMM 的能源耗用量，例如記憶體技術、錯誤修正程式碼（ECC）、匯流排頻率、容量、密度和等級數。 因此，最好在購買大量記憶體之前，先比較預期的電源評等。

現在提供低功率記憶體，但您必須考慮效能和成本取捨。 如果您的伺服器將會分頁，您也應該考慮分頁磁片的能源成本。


## <a name="disks-recommendations"></a>磁片建議
RPM 越高表示增加能源耗用量。 SSD 磁片磁碟機比旋轉磁片磁碟機的電源效率更高。 此外，2.5 英寸的磁片磁碟機通常需要的電源比3.5 寸的磁片磁碟機少。

## <a name="network-and-storage-adapter-recommendations"></a>網路和儲存裝置介面卡建議
某些介面卡會減少閒置期間的能源耗用量。 這是 10 Gb 網路介面卡和高頻寬（4-8 Gb）儲存體連結的重要考慮。 這類裝置可能會耗用大量的能源。


## <a name="power-supply-recommendations"></a>電源供應器建議
改善電源供應器的效率是降低能源耗用量的絕佳方式，而不會影響效能。 高效率電源供應器每年可以省下許多千瓦時的費用。


## <a name="fan-recommendations"></a>風扇建議
風扇（例如電源供應器）是您可以降低能源耗用量的區域，而不會影響系統效能。 可變速度的風扇可以在系統負載降低時減少 RPM，以排除其他不必要的能源耗用量。


## <a name="usb-devices-recommendations"></a>USB 裝置建議
Windows Server 2016 預設會啟用 USB 裝置的選擇性暫止。 不過，撰寫不良的設備磁碟機仍然可以透過相當大邊界來中斷系統能源效率。 為避免潛在的問題，請中斷 USB 裝置的連線、在 BIOS 中停用它們，或選擇不需要 USB 裝置的伺服器。


## <a name="remotely-managed-power-strip-recommendations"></a>遠端系統管理的 Power 帶狀建議
Power 塊不是伺服器硬體不可或缺的一部分，但在資料中心內可能會有很大的差異。 [測量] 顯示已插入但已很明顯是電源的磁片區伺服器，可能仍需要最多30瓦的電源。

為避免浪費電力，您可以為每部伺服器機架部署遠端系統管理的電源線，以程式設計方式中斷特定伺服器的電源。

## <a name="processor-terminology"></a>處理器術語
本主題中使用的處理器術語會反映下圖中可用元件的階層。 從最大到最小的元件資料細微性使用的詞彙如下：

-   處理器通訊端
-   NUMA 節點
-   核心版
-   邏輯處理器

![處理器術語](../media/perftune-guide-figure-1.png)

## <a name="see-also"></a>另請參閱
- [伺服器硬體效能考慮](index.md)
- [電源與效能調整](power/power-performance-tuning.md)
- [處理器電源管理](power/processor-power-management-tuning.md)
- [建議的平衡方案參數](power/recommended-balanced-plan-parameters.md)
