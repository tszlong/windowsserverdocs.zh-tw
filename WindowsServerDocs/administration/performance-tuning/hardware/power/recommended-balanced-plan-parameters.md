---
title: 快速回應時間的建議平衡電源計劃參數
description: 快速回應時間的建議平衡電源計劃參數
ms.topic: conceptual
ms.author: qizha
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 77c33c80598645c4823748663d52fa3aa265e1dd
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077555"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>適用于需要快速回應時間之工作負載的建議平衡電源計劃參數

預設 **平衡** 的電源計劃使用 **輸送量** 作為微調的效能度量。 在穩定狀態下， **輸送量** 不會因為不同的使用率而改變，直到系統完全超載 (~ 100% 使用率) 。  如此一來， **平衡** 的電源計劃就能將處理器頻率降到最低，並將使用率最大化。

不過， **回應時間** 可能會以指數方式增加，並增加使用率。 現今，快速回應時間的需求大幅增加。 雖然 Microsoft 建議使用者在需要快速回應時間時切換至 **高效** 能電源計劃，但某些使用者不會想要在光線到中型負載層級中失去電源優勢。 因此，Microsoft 會針對需要快速回應時間的工作負載，提供下列一組建議的參數變更。


| 參數 | 描述 | 預設值 | 建議值 |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 處理器效能提高閾值 | 頻率增加的使用量閾值 | 90 | 60 |
| 處理器效能降低閾值 | 頻率低於閾值的使用率閾值 | 80 | 40 |
| 處理器效能增加時間 | 在頻率增加之前的檢查時段數目 | 3 | 1 |
| 處理器效能增加原則 | 頻率增加的速度 | Single | 理想 |

若要設定建議的值，使用者可以在具有系統管理員身分的視窗中執行下列命令：

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

這項變更是以使用下列工作負載的效能與電源取捨分析為基礎。 如果使用者想要使用特定 SLA 需求進一步微調電源效率，請參閱 [伺服器硬體效能考慮](../power.md)。

>[!Note]
> 如需運用電源計劃微調虛擬化工作負載的其他建議和深入資訊，請參閱[hyper-v](../../role/hyper-v-server/configuration.md)設定

## <a name="specpower--java-workload"></a>SPECpower – JAVA 工作負載

[SPECpower \_ ssj2008](http://spec.org/power_ssj2008/)是適用于伺服器電源和效能特性的最受歡迎產業標準規格基準測試，可用來檢查電源衝擊。 因為它只會使用 **輸送量** 作為效能度量，所以預設的 **平衡** 電源計劃會提供最佳的電源效率。

提議的參數變更會以稍微較高的光線 (，亦即 <= 20% ) 負載層級。 但在負載等級較高的情況下，差異會增加，且會開始使用與 **高效** 能電源計劃相同的電源，在60% 的負載層級之後。 若要使用建議的變更參數，使用者應該要知道在其機架容量規劃期間，「中」到「高」負載層級的電力成本。

## <a name="geekbench-3"></a>GeekBench 3

[GeekBench 3](http://www.geekbench.com/geekbench3/) 是跨平臺處理器效能評定，可分隔單一核心和多核心效能的分數。 它會模擬一組工作負載，包括整數工作負載 (加密、壓縮、影像處理等 ) 、浮點數工作負載 (模型化、碎形、影像銳化、影像模糊等 ) 和記憶體工作負載 (串流) 。

**回應時間** 是其分數計算中的主要量值。 在經過測試的系統中，相**較于高效**能電源計劃，預設**平衡**的電源計劃在單一核心測試中具有 ~ 18% 的回歸，而多核心測試的回歸為 ~ 40%。 建議的變更會移除這些回歸。

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd) 是一種命令列工具，適用于 Microsoft 所開發的儲存體基準測試。 它廣泛用來針對儲存效能分析的儲存體系統產生各種要求。

我們會設定 [容錯移轉叢集]，並使用 Diskspd 來產生隨機和連續，以及使用不同 IO 大小的本機和遠端存放系統來讀取和寫入 IOs。 我們的測試顯示，IO 回應時間在不同電源計劃下的處理器頻率非常敏感。 **平衡**的電源計劃可從特定工作負載下的**高效**能電源計劃中，將其回應時間加倍。 建議的變更會移除大部分的回歸。

>[!Important]
>從執行 Windows Server 2016 的 Intel [Broadwell] 處理器開始，大部分的處理器電源管理決策都是在處理器中進行，而不是作業系統層級，以達到更快的工作負載變更音。 OS 所使用的舊版 PPM 參數對實際的頻率決策會有最大的影響，但如果處理器應該偏好電源或效能，或達到最短和最大頻率，則會造成影響。 因此，建議的 PPM 參數變更只會以預先 Broadwell 的系統為目標。

## <a name="see-also"></a>另請參閱
- [伺服器硬體效能考量](../index.md)
- [伺服器硬體電源的考量](../power.md)
- [電源與效能調整](power-performance-tuning.md)
- [處理器電源管理](processor-power-management-tuning.md)
- [容錯移轉叢集](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725923(v=ws.10))