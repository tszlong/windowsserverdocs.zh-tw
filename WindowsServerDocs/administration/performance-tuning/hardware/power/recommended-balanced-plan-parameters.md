---
title: 建議快速回應時間平衡 」 電源計劃參數
description: 建議快速回應時間的平衡 電源計劃參數
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 134e868e1400729f754039fc8120cea0c73945bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878789"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>建議用於工作負載平衡 電源計劃參數，需要快速回應時間

預設值**平衡**電源計劃使用**輸送量**做為用來微調效能計量。 在穩定狀態下，期間**輸送量**不會變更與不同的使用率，直到系統完全多載 （~ 100%使用率）。  如此一來，**平衡**電源計劃就很有透過處理器頻率降至最低，並將使用量最大化。

不過**回應時間**無法以指數方式增加使用率增加。 時至今日，快速回應時間的需求已大幅增加。 雖然 Microsoft 建議使用者切換至**高效能**電源計劃在需要快速回應時間時，某些使用者不要光期間會失去電源優點，中型負載層級。 因此，Microsoft 會提供下列一組建議的參數變更為需要快速回應時間的工作負載。


| 參數 | 描述 | 預設值 | 建議的值 |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 處理器效能增加臨界值 | 使用率閾值上述的頻率是以增加 | 90 | 60 |
| 處理器效能降低臨界值 | 使用率閾值以下這是要減少的頻率 | 80 | 40 |
| 處理器效能增加時間 | PPM 核取 windows 之前的頻率是以增加數目 | 3 | 1 |
| 處理器效能增加原則 | 若要增加的速度的頻率是 | 單一 | 理想 |

若要設定的建議的值，使用者可以執行下列命令在視窗中使用系統管理員：

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

這項變更根據電源取捨分析，使用下列工作負載與效能。 如想要進一步精確的使用者會微調特定 SLA 需求的電源效率，請參閱[伺服器硬體的效能考量](../power.md)。

>[!Note]
> 如需其他建議和深入了解利用電源計劃，來微調虛擬化工作負載，讀取[hyper-v 設定](../../role/hyper-v-server/configuration.md)

## <a name="specpower--java-workload"></a>SPECpower – JAVA 工作負載

[SPECpower\_ssj2008](http://spec.org/power_ssj2008/)、 最熱門的業界標準規格基準測試，伺服器的能力和效能特性，用來檢查 power 影響。 因為它只會使用**輸送量**做為效能計量，預設值**平衡**電源計劃提供最佳的電源效率。

建議的參數變更消耗較高的電力，燈光在 (也就是 < = 20%)負載層級。 使用較高負載層級，此差異會增加，但它開始為同一個 power**高效能**後的 60%負載層級的電源計劃。 若要使用的建議的變更參數，使用者應該注意的電費支出，在高負載層級的媒體在其機架容量計劃期間。

## <a name="geekbench-3"></a>GeekBench 3

[GeekBench 3](http://www.geekbench.com/geekbench3/)是跨平台處理器基準測試，用來分隔單一核心和多重核心效能分數。 它會模擬一組包括整數工作負載 （加密、 壓縮、 映像處理等） 的工作負載、 浮動點工作負載 （模型、 碎、 銳利化映像、 映像變得模糊不清等） 和 (streaming) 的記憶體內部工作負載。

**回應時間**是在分數計算中的主要量值。 我們已測試的系統預設值**平衡**電源計劃在多核心測試相較於在單一核心測試和大約 40%迴歸具有 ~ 18%迴歸**高效能**電源計劃。 提議的變更會移除這些迴歸。

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd)是命令列工具，若是由 Microsoft 所開發的儲存體效能評定。 它是廣泛用來產生各種不同的儲存體效能分析的儲存體系統的要求。

我們會設定 [容錯移轉叢集，]，並用以產生隨機和循序、 讀取和寫入至不同的 IO 大小的本機和遠端存放裝置系統 Io 的 Diskspd。 我們的測試會顯示在不同的電源計劃的 IO 回應時間是非常敏感處理器頻率。 **平衡**電源計劃可能兩倍的回應時間，從**高效能**在特定工作量下的電源計劃。 提議的變更會移除大部分的迴歸。

>[!Important]
>從執行 Windows Server 2016 的 [Broadwell] Intel 處理器，處理器電源管理決策大多會對處理器，而不是作業系統層級中達成更快速的音時的工作負載變更。 OS 所使用的舊版 PPM 參數對實際頻率決策，但不包括告訴處理器是否應該偏好能力與效能，或達到上限的最小和最大頻率的影響降到最低。 因此前, Broadwell 系統所只針對提議的 PPM 參數變更。

## <a name="see-also"></a>另請參閱
- [伺服器硬體的效能考量](../index.md)
- [伺服器硬體能力的考量](../power.md)
- [電力和效能調整](power-performance-tuning.md)
- [處理器電源管理調整](processor-power-management-tuning.md)
- [容錯移轉叢集](https://technet.microsoft.com/library/cc725923.aspx)
