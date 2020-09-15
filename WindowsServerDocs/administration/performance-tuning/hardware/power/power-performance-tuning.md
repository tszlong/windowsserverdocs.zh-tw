---
title: 介紹 Windows Server 的電源和效能調整
description: 針對 Windows Server 進行處理器電源管理 (PPM) 微調的總覽。
ms.topic: conceptual
ms.author: qizha
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 1e1f409e5c84a603c628e1ceebe97317864c8785
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077610"
---
# <a name="power-and-performance-tuning"></a>電源和效能調整

在企業和資料中心環境中，能源效率日益重要，並增加了混合設定選項的另一組取捨。

Windows Server 2016 已針對絕佳的能源效率進行優化，且對各種客戶工作負載的效能影響降到最低。 [處理器電源管理 (PPM) 微調 Windows Server 平衡的電源計劃](processor-power-management-tuning.md) 說明用來微調 windows server 2016 中預設參數的工作負載，並提供自訂並的建議。

本節將擴充能源效率的取捨，以協助您在需要調整伺服器的預設電源設定時，做出明智的決策。 不過，在執行 Windows Server 2016 時，大部分的伺服器硬體和工作負載都不應要求系統管理員電源調整。

## <a name="calculating-server-energy-efficiency"></a>計算伺服器能源效率

當您調整伺服器的節省能源時，也必須考慮到效能。 調整會影響效能和能力，有時會有不相稱的數量。 針對每個可能的調整，請考慮您的功率預算和效能目標，以判斷是否可接受取捨。

您可以針對包含電源和效能資訊的實用計量，計算伺服器的能源效率比率。 能源效率是指在指定的一段時間內所需的平均功率所執行的工作量比率。

![能源效率公式](../../media/perftune-guide-power-formula.png)

您可以使用此計量來設定實際的目標，以達成電源與效能之間的取捨。 相反地，在資料中心內節省10% 能源的目標，將無法抓住相對於效能的影響，反之亦然。

同樣地，如果您將伺服器調整為提高效能5%，而這會導致高達10% 的能源耗用量，則您的業務目標可能或可能無法接受總結果。 能源效率計量可讓您以更明智的決策進行，而不是單獨的電源或效能度量。

## <a name="measuring-system-energy-consumption"></a>測量系統能源耗用量

在調整伺服器的能源效率之前，您應該先建立基準功率度量。

如果您的伺服器有必要的支援，您可以使用 Windows Server 2016 中的電源計量和預算功能，利用效能監視器來查看系統層級的能源耗用量。

判斷您的伺服器是否支援計量和預算的其中一種方式是檢查 [Windows Server Catalog](https://www.windowsservercatalog.com)。 如果您的伺服器模型符合 Windows 硬體認證計畫中新的增強電源管理資格，則保證支援計量和預算功能。

另一種檢查計量支援的方法，是在效能監視器中手動尋找計數器。 開啟效能監視器，選取 [ **新增計數器**]，然後找出 [ **電源計量** ] 計數器群組。

如果已命名的電源計量實例出現在標示為 [ **所選取物件的實例**] 方塊中，則您的平臺支援計量。 顯示瓦功率的 **電源** 計數器會顯示在選取的計數器群組中。 未指定 power data 值的精確衍生。 例如，它可能是即時的電源繪製，或一段時間間隔內的平均電源繪製。

如果您的伺服器平臺不支援計量，您可以使用連線到電源供應器輸入的實體計量裝置來測量系統電源繪製或能源耗用量。

若要建立基準，您應該測量各種系統負載點所需的平均電源，從閒置到 100% (最大輸送量) 以產生載入行。 下圖顯示三個範例設定的載入行：

![載入行範例](../../media/perftune-guide-sample-loadlines.png)

您可以使用載入行來評估和比較所有載入點上的設定效能和能源耗用量。 在此特定範例中，很容易就能看到最適合的設定。 不過，很容易就會有一個設定最適合用於繁重工作負載，另一個則最適合輕量工作負載。

您必須徹底瞭解您的工作負載需求，才能選擇最佳設定。 不要假設當您找到良好的設定時，它一定會保持最佳狀態。 您應該定期測量系統使用率和能源耗用量，以及變更工作負載、工作負載層級或伺服器硬體之後的耗用量。

## <a name="diagnosing-energy-efficiency-issues"></a>診斷能源效率問題

**PowerCfg.exe** 支援命令列選項，可讓您用來分析伺服器的閒置能源效率。 當您使用 **/energy** 選項執行 PowerCfg.exe 時，此工具會執行60秒的測試，以偵測潛在的能源效率問題。 此工具會在目前的目錄中產生簡單的 HTML 報表。

> [!Important]
> 若要確保正確的分析，請先確定所有的本機應用程式都已關閉，然後再執行 **PowerCfg.exe**。 

縮短計時器滴答率、缺乏電源管理支援的驅動程式，以及 CPU 使用率過高，是由 **powercfg/energy** 命令偵測到的一些行為問題。 這項工具提供簡單的方式來識別及修正電源管理問題，可能會大幅節省大型資料中心的成本。

如需 PowerCfg.exe 的詳細資訊，請參閱 [使用 PowerCfg 評估系統能源效率](/previous-versions/windows/hardware/download/dn550976(v=vs.85))。

## <a name="using-power-plans-in-windows-server"></a>在 Windows Server 中使用電源計劃

Windows Server 2016 有三個內建的電源計劃，其設計目的是為了滿足不同的商務需求組合。 這些方案提供簡單的方式，讓您自訂伺服器以符合電源或效能目標。 下表描述方案、列出使用每個計畫的常見案例，並提供每個計畫的一些實作為詳細資料。

| **規劃** | **說明** | **常見的適用案例** | **執行重點摘要** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 建議) 對稱 ( | 預設值， 以效能影響降至低的良好能源效率為目標。 | 一般計算 | 符合需求的容量。 省電功能會平衡電力和效能。 |
| 高效能 | 以高能源耗用量的成本來提高效能。 適用電源和散熱限制、營運費用和可靠性考慮。 | 對處理器效能變更很敏感的低延遲應用程式和應用程式程式碼 | 處理器一律會以最高效能狀態鎖定 (包括) 的「turbo」頻率。 所有核心皆已離開。 熱輸出可能很重要。 |
| 節能程式 | 限制效能以節省能源並降低營運成本。 不建議進行完整測試，以確保效能足夠。 | 具有有限功率預算和溫度條件約束的部署 | 如果支援) ，則為最大 (的最高處理器頻率，並啟用其他能源節省功能。 |


這些電源計劃存在於 Windows 中，可用來替代目前的 (AC) 和直接目前 (DC) 驅動的系統，但我們會假設伺服器一律使用 AC 電源來源。

如需電源計劃和電源原則設定的詳細資訊，請參閱 [Windows 中的電源原則設定和部署](/previous-versions/windows/hardware/design/dn642106(v=vs.85))。

> [!Note]
> 有些伺服器製造商有自己的電源管理選項可透過 BIOS 設定來使用。 如果作業系統沒有電源管理的控制權，變更 Windows 的電源計劃將不會影響系統電源和效能。

## <a name="tuning-processor-power-management-parameters"></a>微調處理器電源管理參數

每個電源計劃都代表許多基礎電源管理參數的組合。 內建方案是三個建議設定的集合，其中涵蓋各種不同的工作負載和案例。 不過，我們認為這些方案將不符合每個客戶的需求。

下列各節說明調整一些特定處理器電源管理參數的方式，以符合三個內建方案未解決的目標。 如果您需要瞭解更大的電源參數陣列，請參閱 [Windows 中的電源原則設定和部署](/previous-versions/windows/hardware/design/dn642106(v=vs.85))。

## <a name="processor-performance-boost-mode"></a>處理器效能提升模式

Intel Turbo 加速和 AMD Turbo 核心技術是一種功能，可讓處理器在最有用的 (（也就是高系統負載) ）達到更高的效能。 不過，這項功能會增加 CPU 核心能源耗用量，因此 Windows Server 2016 會根據使用中的電源原則和特定處理器實行來設定 Turbo 技術。

在所有 Intel 和 AMD 處理器上，已啟用 Turbo 以進行高效能電源計劃，並已停用 Power 節電電源計劃。 針對依賴傳統 P 狀態式頻率管理的系統平衡電源計劃，只有在平臺支援 EPB 註冊時，才會預設啟用 Turbo。

> [!Note]
> 只有 Intel Westmere 和更新版本的處理器支援 EPB 註冊。

針對 Intel Nehalem 和 AMD 處理器，在 P 狀態平臺上預設會停用 Turbo。 但是，如果系統支援共同處理處理器效能控制 (CPPC) ，也就是作業系統和 ACPI 5.0) 中定義之硬體 (之間的新替代模式，它會在 Windows 作業系統動態要求硬體以提供可能的最高效能層級時參與。

若要啟用或停用 Turbo 增強功能，處理器效能提升模式參數必須由系統管理員或選擇的電源計劃的預設參數設定來設定。 處理器效能提升模式有五個允許的值，如 [表 5] 所示。

若為 P 狀態的控制項，則會停用此選項，並在每次要求名義效能時，使用 (Turbo 可供硬體使用) ，而且只有在 EPB 暫存器執行) 時，才可以使用有效率的 (Turbo。

針對以 CPPC 為基礎的控制項，已停用、有效率地啟用 (Windows 指定要提供) 的適當 Turbo 數量，以及積極的 (Windows 要求「最大效能」以啟用 Turbo) 。

在 Windows Server 2016 中，提升模式的預設值為3。

| **Name** | **P-以狀態為基礎的行為** | **CPPC 行為** |
|--------------------------|------------------------|-------------------|
| 0 (停用)  | 已停用 | 已停用 |
| 1 (已啟用)  | 啟用 | 有效率地啟用 |
| 2 (積極的)  | 啟用 | 主動 |
| 3 (有效率的啟用)  | 有效率 | 有效率地啟用 |
| 4 (有效率的主動式)  | 有效率 | 主動 |


下列命令會在目前的電源計劃上啟用處理器效能提升模式 (使用 GUID 別名) 來指定原則：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

> [!Important]
> 您必須執行 **powercfg-advanced.field.setactive** 命令，才能啟用新的設定。 您不需要重新開機伺服器。

若要為目前選取的方案以外的電源計劃設定此值，您可以使用別名，例如 [配置 \_ 最大值] (節能程式) 、[配置 \_ 最小值] ([高效能]) 和 [配置] \_ (平衡) 以取代目前的配置 \_ 。 將先前顯示的 powercfg advanced.field.setactive 命令中的「目前配置」取代為所需的別名，以啟用該電源計劃。

例如，若要調整「省電」計畫中的「提升」模式，並讓「電源保護」成為目前的方案，請執行下列命令：

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>最小和最大處理器效能狀態

處理器在效能狀態之間的變更 (P-狀態) 非常快速，以符合供應的需求，並盡可能在必要時提供效能並節省能源。 如果您的伺服器具有特定的高效能或最少電源耗用量需求，您可以考慮設定 **最低處理器效能狀態** 參數或 **最大處理器效能狀態** 參數。

**最小處理器效能狀態**和**處理器效能狀態**參數的值，會以最大處理器頻率的百分比表示，值的範圍為 0-100。

如果您的伺服器需要超低延遲、非變異的 CPU 頻率 (例如，針對可重複的測試) 或最高效能層級，您可能不想讓處理器切換至較低效能的狀態。 針對這類伺服器，您可以使用下列命令，將最低處理器效能狀態上限為100%：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

如果您的伺服器需要較低的能源耗用量，您可能會想要將處理器效能狀態的上限設為最大百分比。 例如，您可以使用下列命令，將處理器限制為其最大頻率的75%：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

> [!Note]
> 最大百分比的處理器效能需要處理器支援。 請檢查處理器檔以判斷是否存在這類支援，或查看**處理器**群組中**最大頻率**的效能監視器計數器%，查看是否已套用任何頻率上限。

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>處理器效能提升和降低閾值和原則

處理器效能狀態增加或減少的速度是由多個參數所控制。 下列四個參數具有最明顯的影響：

-   **處理器效能增加閾值** 會定義高於處理器效能狀態將增加的使用率值。 較大的值會減緩效能狀態增加的速率，以回應增加的活動。

-   **處理器效能降低閾值** 會定義低於此值的使用率值，處理器效能狀態將會降低。 較大的值會在閒置期間增加效能狀態的減少率。

-   **處理器效能提升原則和處理器效能降低** 原則會決定發生變更時應設定的效能狀態。 「單一」原則表示它會選擇下一個狀態。 "Rocket" 表示最大或最基本的電源效能狀態。 「理想」會嘗試找出電源與效能之間的平衡。

例如，如果您的伺服器需要超低延遲，但仍想要在閒置期間內獲得低耗電量，您可以 quicken 增加負載的效能狀態，並在負載下降時降低降低的速度。 下列命令會將增加原則設定為 "Rocket"，以加快狀態增加，並將降低原則設定為 [單一]。 增加和減少閾值會分別設定為10和8。

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>處理器效能核心停車最大和最小核心

核心停車是 Windows Server 2008 R2 中引進的功能。 處理器電源管理 (PPM) 引擎和排程器會一起運作，以動態調整可執行執行緒的核心數目。 PPM 引擎會為要排程的執行緒選擇最少的核心數目。

通常會暫停的核心不會有任何已排程的執行緒，而且當它們不處理中斷、Dpc 或其他嚴格相似化為的工作時，它們將會降到非常低的電源狀態。 其餘的核心則負責工作負載的其餘部分。 核心停車可能會在使用量較低時提高能源效率。

對於大部分的伺服器而言，預設的核心停車行為可提供合理的輸送量和能源效率平衡。 在核心停車可能不會對一般工作負載顯示最大效益的處理器上，預設可以停用它。

如果您的伺服器有特定的核心停車需求，您可以使用 Windows Server 2016 中的 **處理器效能核心暫止最大核心** 參數或 **處理器效能核心暫止最小** 核心參數，來控制公園可用的核心數目。

當有一或多個作用中線程相似化為至 NUMA (節點中非一般的 cpu 子集時（也就是超過1個 CPU，但小於節點) 上的整個 Cpu 集），其中一個核心停車不一定是最佳的情況。 當核心停車演算法挑選核心來進行 unpark (假設) 的工作負載強度增加時，可能不會一律挑選主動相似化為子集內的核心 (或 unpark 的子集) ，因此可能會導致無法實際使用的 unparking 核心。

這些參數的值是範圍0–100的百分比。 [ **處理器效能] 核心暫止 [最大核心** 數] 參數可讓您隨時 (可用來執行) 執行緒的最大核心百分比，而 [ **處理器效能核心暫止最小核心** 數] 參數則會控制可以離開的核心最小百分比。 若要關閉核心停車，請使用下列命令將 **處理器效能核心暫止最低核心** 參數設定為100%：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

若要將可排程的核心數目減少為50% 的最大計數，請將 **處理器效能核心暫止最大核心** 參數設定為50，如下所示：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>處理器效能核心停車公用程式分佈

公用程式散發是 Windows Server 2016 中的演算法優化，其設計目的是為了改善某些工作負載的電源效率。 它會追蹤可移動的 CPU 活動 (（即 Dpc、中斷或嚴格相似化為的執行緒) ），並根據假設任何可移動工作可平均分散到所有已離開的核心，來預測每個處理器上的未來工作。

針對某些處理器的平衡電源計劃，預設會啟用公用程式散發。 它可以減少處理器的耗電量，方法是降低處於穩定狀態的工作負載要求 CPU 頻率。 不過，對於需要高活動高載的工作負載，或工作負載在不同處理器之間快速且隨機移動的程式，公用程式散發不一定是理想的演算法選擇。

針對這類工作負載，建議使用下列命令停用公用程式散發：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="additional-references"></a>其他參考資料

- [伺服器硬體效能考量](../index.md)
- [伺服器硬體電源的考量](../power.md)
- [處理器電源管理](processor-power-management-tuning.md)
- [建議的平衡方案參數](recommended-balanced-plan-parameters.md)