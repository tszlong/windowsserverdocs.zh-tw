---
title: Windows Server 硬體電源考慮
description: Windows Server 硬體電源的考慮。
ms.topic: conceptual
ms.author: qizha
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fe316dd1f21d3f5e151cef60f63c3644ad1af06d
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077630"
---
# <a name="server-hardware-power-considerations"></a>伺服器硬體電源的考量

在企業和資料中心環境中辨識能源效率的重要性，是很重要的。 高效能和低能源使用量通常是衝突的目標，但藉由仔細選取伺服器元件，您可以在兩者之間達成正確的平衡。 下列各節列出伺服器硬體元件的電源特性和功能的指導方針。

## <a name="processor-recommendations"></a>處理器建議事項

頻率、營運電壓、快取大小和處理技術會影響處理器的能源消耗。 處理器具有熱設計點 (TDP) 評等，以提供與其他模型相關之能源耗用量的基本指示。

一般而言，選擇符合您效能目標的最低 TDP 處理器。 此外，較新的處理器世代通常會更有效率，而且它們可能會公開更多 Windows 電源管理演算法的電源狀態，以在所有效能層級上啟用更佳的電源管理。 也可以使用 Microsoft 與硬體製造商合作開發的一些新「合作」電源管理技術。

如需有關合作式電源管理技術的詳細資訊，請參閱 [進階組態與電源介面規格](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf)中的「共同處理處理器效能控制項」一節。

## <a name="memory-recommendations"></a>記憶體建議

記憶體帳戶，以增加整體系統電源的一部分。 許多因素會影響記憶體 DIMM 的能源耗用量，例如記憶體技術、錯誤修正程式碼 (ECC) 、匯流排頻率、容量、密度，以及排名數目。 因此，最好在購買海量儲存體之前，先比較預期的功率評等。

低電源記憶體現已可供使用，但您必須考慮效能和成本取捨。 如果您的伺服器將分頁，您也應該將分頁磁片的能源成本納入考慮。

## <a name="disks-recommendations"></a>磁片建議

較高的 RPM 表示增加能源耗用量。 SSD 磁片磁碟機比旋轉磁片磁碟機的電源效率更高。 此外，2.5-英寸的磁片磁碟機通常需要比 3.5-英寸的磁片磁碟機更少的電源。

## <a name="network-and-storage-adapter-recommendations"></a>網路和儲存裝置介面卡建議

有些介面卡會在閒置期間減少能源耗用量。 這是 10 Gb 網路介面卡和高頻寬 (4-8 Gb) 儲存體連結的重要考慮。 這類裝置可能耗用大量能源。

## <a name="power-supply-recommendations"></a>電源供應器建議

提高電源供應的效率是減少能源耗用量的絕佳方法，而不會影響效能。 高效率的電源供應器每一部伺服器可以節省每年的千瓦時。

## <a name="fan-recommendations"></a>風扇建議

像是電源供應器的風扇是一個區域，您可以在不影響系統效能的情況下，減少能源耗用量。 當系統負載減少時，可變速度的風扇可減少 RPM，消除不必要的能源消耗。

## <a name="usb-devices-recommendations"></a>USB 裝置建議

Windows Server 2016 預設會啟用 USB 裝置的選擇性暫止。 不過，撰寫不良的設備磁碟機仍可透過相當大邊界來中斷系統能源效率。 若要避免潛在的問題，請中斷 USB 裝置連線、在 BIOS 中停用它們，或選擇不需要 USB 裝置的伺服器。

## <a name="remotely-managed-power-strip-recommendations"></a>遠端系統管理的電源帶建議

電源線不是伺服器硬體不可或缺的一部分，但可以在資料中心內產生很大的差異。 度量會顯示插入的磁片區伺服器，但 root\wmi 關閉電源，可能仍需要最多30瓦的電力。

為了避免浪費電力，您可以為每部伺服器機架部署遠端系統管理的電源線，以程式設計方式中斷特定伺服器的電源。

## <a name="processor-terminology"></a>處理器術語

本主題中使用的處理器術語反映下圖中可用的元件階層。 從最大到最小的元件細微性使用的詞彙如下：

- 處理器通訊端
- NUMA 節點
- 核心版
- 邏輯處理器

![處理器術語](../media/perftune-guide-figure-1.png)

## <a name="additional-references"></a>其他參考資料

- [伺服器硬體效能考量](index.md)

- [電源與效能調整](power/power-performance-tuning.md)

- [處理器電源管理](power/processor-power-management-tuning.md)

- [建議的平衡方案參數](power/recommended-balanced-plan-parameters.md)
