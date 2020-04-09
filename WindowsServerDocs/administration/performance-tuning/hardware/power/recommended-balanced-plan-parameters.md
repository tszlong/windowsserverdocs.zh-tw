---
title: 建議的平衡電源計劃參數快速回應時間
description: 建議的平衡電源計劃參數以快速回應時間
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: qizha;tristanb
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 288746b5361c550e167f64886a929c96c81ff8d0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851961"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>針對需要快速回應時間的工作負載，建議的平衡電源計劃參數

預設的**平衡**電源計劃會使用**輸送量**做為微調的效能度量。 在穩定狀態期間，**輸送量**不會隨著系統完全超載（~ 100% 使用率）而改變使用率。  如此一來，**平衡**的電源計劃就能以最小化的處理器頻率和最大化使用率，來提供強大的電源。

不過，**回應時間**會隨著使用率增加呈指數增加。 現今，快速回應時間的需求大幅增加。 雖然 Microsoft 建議使用者在需要快速回應時間時切換到**高效**能電源計劃，但有些使用者並不想要在光線到中型負載層級時喪失電力效益。 因此，Microsoft 會針對需要快速回應時間的工作負載，提供下列建議的參數變更集。


| 參數 | 描述 | 預設值 | 提議的值 |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 處理器效能增加閾值 | 使用率閾值高於此頻率會增加 | 90 | 60 |
| 處理器效能降低閾值 | 頻率要減少的使用率閾值 | 80 | 40 |
| 處理器效能增加時間 | 頻率要增加之前的 PPM 檢查視窗數目 | 3 | 1 |
| 處理器效能增加原則 | 頻率增加的速度 | Single | 理想 |

若要設定建議的值，使用者可以在具有系統管理員的視窗中執行下列命令：

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

這項變更是以使用下列工作負載的效能和電源取捨分析為基礎。 對於想要以特定 SLA 需求進一步微調電源效率的使用者，請參閱[伺服器硬體效能考慮](../power.md)。

>[!Note]
> 如需運用電源計劃微調虛擬化工作負載的其他建議和深入解析，請參閱[hyper-v](../../role/hyper-v-server/configuration.md)設定

## <a name="specpower--java-workload"></a>SPECpower – JAVA 工作負載

[SPECpower\_ssj2008](http://spec.org/power_ssj2008/)，這是最受歡迎的業界標準規格，適用于伺服器電源和效能特性，可用來檢查電源影響。 因為它只會使用**輸送量**做為效能計量，所以預設的**平衡**電源計劃可提供最佳的電源效率。

提議的參數變更在光線上耗用稍微高一點的電源（也就是 < = 20%）載入層級。 但是在負載層級較高的情況下，差異會增加，而且開始使用與**高效**能電源計劃在60% 負載層級之後的相同電源。 若要使用提議的變更參數，使用者應該知道在其機架容量規劃期間，以中到高負載層級的電力成本。

## <a name="geekbench-3"></a>GeekBench 3

[GeekBench 3](http://www.geekbench.com/geekbench3/)是跨平臺處理器基準測試，可區隔單一核心和多核心效能的分數。 它會模擬一組工作負載，包括整數工作負載（加密、壓縮、影像處理等）、浮點工作負載（模型化、碎形、影像銳化、影像模糊等）和記憶體工作負載（串流）。

**回應時間**是其分數計算中的主要量值。 在我們測試過的系統中，相**較于高效**能的電源計劃，預設的**平衡**電源計劃在單一核心測試中有 ~ 18% 的回歸，而在多核心測試中有 ~ 40% 的回歸。 提議的變更會移除這些回歸。

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd)是 Microsoft 所開發之存放裝置效能評定的命令列工具。 它廣泛用來針對儲存體效能分析的儲存系統產生各種要求。

我們會設定 [容錯移轉叢集]，並使用 Diskspd 來產生隨機和連續，並在具有不同 IO 大小的本機和遠端存放系統上讀取和寫入 Io。 我們的測試顯示 IO 回應時間對不同電源計劃下的處理器頻率非常敏感。 **平衡**的電源計劃可以從特定工作負載下的**高效**能電源計劃中，將其回應時間加倍。 提議的變更會移除大部分的回歸。

>[!Important]
>從執行 Windows Server 2016 的 Intel [Broadwell] 處理器開始，大部分的處理器電源管理決策都是在處理器中進行，而不是 OS 層級，以達到更快的工作負載變更音。 OS 使用的舊版 PPM 參數會對實際的頻率決策造成最小的影響，但會告訴處理器其是否應具備電源或效能，或將最小和最大頻率降到最低。 因此，建議的 PPM 參數變更僅以預先 Broadwell 的系統為目標。

## <a name="see-also"></a>另請參閱
- [伺服器硬體效能考慮](../index.md)
- [伺服器硬體電源的考量](../power.md)
- [電源與效能調整](power-performance-tuning.md)
- [處理器電源管理](processor-power-management-tuning.md)
- [容錯移轉叢集](https://technet.microsoft.com/library/cc725923.aspx)
