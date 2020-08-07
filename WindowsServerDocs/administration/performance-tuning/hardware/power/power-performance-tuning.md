---
title: Windows Server 的電源與效能微調總覽
description: Windows Server 的處理器電源管理 (PPM) 微調的總覽。
ms.topic: conceptual
ms.author: qizha;tristanb
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5e758e2335d8a5b536b0f0db9626dc88337de631
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896733"
---
# <a name="power-and-performance-tuning"></a>電源與效能調整

在企業和資料中心環境中，能源效率日益重要，並且在混合設定選項方面增加了另一組取捨。

Windows Server 2016 已針對各種客戶工作負載的最小效能影響，優化了絕佳的能源效率。 [處理器電源管理 (PPM) 微調 Windows Server 平衡電源計劃](processor-power-management-tuning.md)說明在 windows server 2016 中用來調整預設參數的工作負載，並提供自訂 tunings 的建議。

本節擴展了能源效率取捨，以協助您在需要調整伺服器上的預設電源設定時做出明智的決策。 不過，執行 Windows Server 2016 時，大部分的伺服器硬體和工作負載都不需要系統管理員電源調整。

## <a name="calculating-server-energy-efficiency"></a>計算伺服器的能源效率

當您微調伺服器以節省能源時，您也必須考慮效能。 微調會影響效能和能力，有時會有不相稱的數量。 針對每個可能的調整，請考慮您的功率預算和效能目標，以判斷是否可接受取捨。

您可以針對包含電源和效能資訊的有用計量，計算伺服器的能源效率比率。 「能源效率」是指在指定時間內所需的平均耗電量，所做的工作比例。

![能源效益公式](../../media/perftune-guide-power-formula.png)

您可以使用此計量來設定符合電源與效能之間取捨的實際目標。 相較之下，跨資料中心節省10% 能源的目標，無法捕獲相對於效能的影響，反之亦然。

同樣地，如果您將伺服器調整為增加5% 的效能，並導致10% 的能源消耗較高，則總結果可能或可能無法接受您的商務目標。 「能源效率」度量可讓您以更明智的決策為依據，而不是僅限電源或效能計量。

## <a name="measuring-system-energy-consumption"></a>測量系統能源耗用量

在調整伺服器的能源效率之前，您應該先建立基準功率測量。

如果您的伺服器有必要的支援，您可以使用 Windows Server 2016 中的電源計量和預算功能，透過「效能監視器」來查看系統層級的能源耗用量。

判斷您的伺服器是否支援計量和預算的其中一種方式，就是查看[Windows Server Catalog](https://www.windowsservercatalog.com)。 如果您的伺服器模型符合 Windows 硬體認證計畫中新的增強電源管理合格規定，保證會支援計量和預算功能。

另一個檢查計量支援的方法，是在效能監視器中手動尋找計數器。 開啟 [效能監視器]，選取 [**新增計數器**]，然後找出 [**電源計量**] 計數器群組。

如果 power 計量的命名實例出現在標示為 [**選取的物件的實例**] 方塊中，則您的平臺支援計量。 顯示功率為瓦的**電源**計數器會出現在所選的計數器群組中。 未指定正確的 power data 值衍生。 例如，它可能是瞬間的電源繪製，或是一段時間間隔的平均電源繪製。

如果您的伺服器平臺不支援計量，您可以使用連接到電源供應器輸入的實體計量裝置來測量系統電源繪製或能源消耗。

若要建立基準，您應該測量各種系統負載點所需的平均耗電量，從閒置到 100% (最大輸送量) 以產生負載線。 下圖顯示三個範例設定的載入行：

![載入行範例](../../media/perftune-guide-sample-loadlines.png)

您可以使用負載線來評估和比較所有載入點上設定的效能和能源耗用量。 在此特定範例中，很容易就能看出最佳設定。 不過，很容易就會有一個設定最適合用於繁重的工作負載，而一個最適合用於輕量工作負載的情況。

您必須徹底瞭解您的工作負載需求，才能選擇最佳設定。 請不要假設當您找到良好的設定時，它一定會保持最佳狀態。 您應該定期測量系統使用率和能源耗用量，以及變更工作負載、工作負載層級或伺服器硬體之後。

## <a name="diagnosing-energy-efficiency-issues"></a>診斷能源效率問題

**PowerCfg.exe**支援命令列選項，可讓您用來分析伺服器的閒置能源效率。 當您使用 **/energy**選項執行 PowerCfg.exe 時，此工具會執行60秒的測試，以偵測潛在的能源效率問題。 此工具會在目前的目錄中產生簡單的 HTML 報表。

> [!Important]
> 若要確保正確的分析，請確定所有本機應用程式都已關閉，然後再執行**PowerCfg.exe**。 

縮短計時器滴答率、缺少電源管理支援的驅動程式，以及過多的 CPU 使用率，是**powercfg/energy**命令所偵測到的一些行為問題。 這項工具提供簡單的方式來識別並修正電源管理問題，可能會導致大型資料中心的大量成本節約。

如需 PowerCfg.exe 的詳細資訊，請參閱[使用 PowerCfg 評估系統能源效率](https://msdn.microsoft.com/windows/hardware/gg463250.aspx)。

## <a name="using-power-plans-in-windows-server"></a>在 Windows Server 中使用電源計劃

Windows Server 2016 有三個內建的電源計劃，是為了符合不同的商務需求而設計的。 這些方案提供簡單的方式，讓您自訂伺服器以符合電源或效能目標。 下表描述這些計畫、列出使用每個計畫的常見案例，並提供每個計畫的一些執行詳細資料。

| **規劃** | **說明** | **常見的適用案例** | **實現重點** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 建議的平衡 ()  | 預設值， 以最小效能影響的目標為良好的能源效率。 | 一般計算 | 符合需求的容量。 省電功能會平衡電源和效能。 |
| 高效能 | 以高能源耗用量的成本來增加效能。 適用電源和冷卻限制、營運費用和可靠性考慮。 | 對處理器效能變更敏感的低延遲應用程式和應用程式代碼 | 處理器一律會處於效能最高的狀態， (包括) 的「turbo」頻率。 已將所有核心都已離開。 熱輸出可能很重要。 |
| 省電 | 限制效能以節省能源並降低營運成本。 不建議在不進行徹底測試的情況下，確定效能是否足夠。 | 具有有限功率預算和熱條件約束的部署 | 如果支援) ，則會以最大 (的百分比表示 cap 處理器頻率，並啟用其他省電功能。 |


這些電源計劃存在於 Windows 中，可用於替代目前的 (AC) ，並將目前的 (DC) 電源系統，但我們會假設伺服器一律使用 AC 電源。

如需電源計劃和電源原則設定的詳細資訊，請參閱[Windows 中的電源原則設定和部署](https://msdn.microsoft.com/windows/hardware/gg463243.aspx)。

> [!Note]
> 有些伺服器製造商有自己的電源管理選項可透過 BIOS 設定來取得。 如果作業系統無法控制電源管理，變更 Windows 中的電源計劃將不會影響系統電源和效能。

## <a name="tuning-processor-power-management-parameters"></a>調整處理器電源管理參數

每個電源計劃都代表許多基礎電源管理參數的組合。 內建方案是三個建議設定的集合，其中涵蓋各種不同的工作負載和案例。 不過，我們認為這些計畫不會符合每個客戶的需求。

下列各節將說明如何調整某些特定處理器電源管理參數，以符合三個內建計畫未解決的目標。 如果您需要瞭解更多的電源參數陣列，請參閱[Windows 中的電源原則設定和部署](https://msdn.microsoft.com/windows/hardware/gg463243.aspx)。

## <a name="processor-performance-boost-mode"></a>處理器效能提升模式

Intel Turbo 加速和 AMD Turbo CORE 技術是一種功能，可讓處理器在最有用的 (（也就是在系統載入) 時）達到更高的效能。 不過，這項功能會增加 CPU 核心能源耗用量，因此 Windows Server 2016 會根據使用中的電源原則和特定的處理器實現來設定 Turbo 技術。

已在所有 Intel 和 AMD 處理器上啟用 Turbo 以取得高效能電源計劃，並已針對省電電源計劃予以停用。 針對依賴傳統 P 狀態頻率管理之系統的平衡電源計劃，只有在平臺支援 EPB 暫存器的情況下，才會啟用 Turbo。

> [!Note]
> 只有 Intel Westmere 和更新版本的處理器才支援 EPB 暫存器。

針對 Intel Nehalem 和 AMD 處理器，在 P 狀態平臺上預設會停用 Turbo。 不過，如果系統支援協同作業的處理器效能控制 (CPPC) ，這是作業系統與 ACPI 5.0) 中所定義硬體 (之間的新替代模式，如果 Windows 作業系統會動態要求硬體以提供最高的效能層級，則可能會參與。

若要啟用或停用 [Turbo 加速功能]，必須由系統管理員或所選電源計劃的預設參數設定來設定 [處理器效能提升模式] 參數。 處理器效能提升模式具有五個允許的值，如 [表 5] 所示。

若為 P 狀態控制項，則會停用選項，啟用 (Turbo 會在) 要求名義效能時供硬體使用，而且只有在 EPB 暫存器執行) 時，才可以使用有效率的 (Turbo。

針對以 CPPC 為基礎的控制項，選項會停用、有效率地啟用 (Windows 會指定提供) 的正確 Turbo 量，而積極的 (Windows 會要求「最大效能」以啟用 Turbo) 。

在 Windows Server 2016 中，[提升模式] 的預設值為3。

| **名稱** | **P-以狀態為基礎的行為** | **CPPC 行為** |
|--------------------------|------------------------|-------------------|
| 0 (停用)  | 已停用 | 已停用 |
| 已啟用1個 ()  | 啟用 | 有效率的啟用 |
| 2 (積極的)  | 啟用 | 主動 |
| 3 (有效率的啟用)  | 有效率 | 有效率的啟用 |
| 4 (有效率的積極)  | 有效率 | 主動 |


下列命令會在目前的電源計劃上啟用處理器效能提升模式 (使用 GUID 別名) 來指定原則：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

> [!Important]
> 您必須執行**powercfg-setactive**命令，以啟用新的設定。 您不需要重新開機伺服器。

若要針對目前所選方案以外的電源計劃設定此值，您可以使用別名，例如「配置 \_ 上限」 (省電) 、「配置 \_ 最小 (高效能) ，以及配置 \_ 平衡 (平衡) 取代配置 \_ 目前。 以所需的別名取代前面所示的 setactive 命令中的「配置目前」，以啟用該電源計劃。

例如，若要調整「省電」計畫中的「提升」模式，並將「省電」功能設為目前的方案，請執行下列命令：

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>最小和最大處理器效能狀態

處理器會在效能狀態之間變更 (P 狀態) 非常快速地符合提供給需求，盡可能在必要時提供效能並節省能源。 如果您的伺服器具有特定的高效能或最小耗電量需求，您可以考慮設定**最低處理器效能狀態**參數或**最大處理器效能狀態**參數。

**最低處理器效能狀態**和**最大處理器效能狀態**參數的值會以最大處理器頻率的百分比表示，其值為0–100。

如果您的伺服器需要超低延遲、不穩定的 CPU 頻率 (例如，針對可重複的測試) 或最高的效能層級，您可能不希望處理器切換到較低的效能狀態。 對於這類伺服器，您可以使用下列命令，將最低處理器效能狀態上限為100%：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

如果您的伺服器需要較低的能源消耗，您可能會想要將處理器效能狀態的上限設為最大值的百分比。 例如，您可以使用下列命令，將處理器的最大頻率限制為75%：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

> [!Note]
> 以最大值的百分比來上限處理器效能，需要處理器支援。 請檢查處理器檔以判斷這類支援是否存在，或在**處理器**群組中查看效能監視器計數器 **% 的最大頻率**，以查看是否已套用任何頻率上限。

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>處理器效能增加和降低閾值和原則

處理器效能狀態增加或減少的速度是由多個參數所控制。 下列四個參數具有最明顯的影響：

-   [**處理器效能增加] 閾值**會定義高於處理器效能狀態會增加的使用率值。 較大的值會使效能狀態增加的速度變慢，以回應增加的活動。

-   [**處理器效能降低閾值**] 會定義低於處理器效能狀態的使用率值。 較大的值會增加閒置期間的效能狀態減少的速率。

-   **處理器效能增加原則和處理器效能降低**原則會決定當發生變更時應設定的效能狀態。 「單一」原則表示它會選擇下一個狀態。 「Rocket」是指最大或最小的電源效能狀態。 「理想」會嘗試找出電源與效能之間的平衡。

例如，如果您的伺服器需要超低延遲，同時又想要在閒置期間從低電源獲益，您可以 quicken 負載增加的效能狀態增加，並在負載下降時降低降低的速度。 下列命令會將 [增加原則] 設定為 [Rocket]，以加快狀態增加的速度，並將 [降低原則] 設定為 [單一]。 增加和減少閾值會分別設定為10和8。

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>處理器效能核心停車最大和最小核心數

核心停車是 Windows Server 2008 R2 中引進的功能。 處理器電源管理 (PPM) 引擎，而排程器會共同運作，以動態調整可執行執行緒的核心數目。 PPM 引擎會為要排程的執行緒選擇最小的核心數目。

通常會暫停的核心並不會排程任何執行緒，而且當它們不會處理中斷、Dpc 或其他嚴格相似化為的工作時，就會進入非常低的電源狀態。 其餘的核心會負責工作負載的其餘部分。 核心停車可能會在較低使用量期間增加能源效率。

對於大部分的伺服器而言，預設的核心停車行為會提供合理的輸送量和能源效率平衡。 在核心停車可能不會顯示對一般工作負載有太大好處的處理器上，預設可以停用。

如果您的伺服器有特定的核心停車需求，您可以使用**處理器效能核心停車最大核心**參數或 Windows server 2016 中的**處理器效能核心停車最小**核心參數，來控制可供公園使用的核心數目。

當有一或多個作用中線程相似化為至 NUMA 節點中的非一般 cpu 子集時，其中一種情況是核心停車不一定是最佳的情況 (也就是，超過1個 CPU，但小於節點) 上的整組 Cpu。 當核心停車演算法挑選核心來 unpark (假設) 的工作負載強度增加時，可能不一定會在使用中的相似化為子集內挑選核心 (或 unpark 的子集) ，因此可能會導致無法實際使用的 unparking 核心。

這些參數的值是範圍0–100中的百分比。 **處理器效能核心 [停車最大核心**數] 參數可控制可以隨時離開 (可供執行執行緒) 的最大核心百分比，而 [**處理器效能核心] [停車最小核心**] 參數則控制可被停用的核心百分比下限。 若要關閉核心停車，請使用下列命令，將**處理器效能核心停車最小核心**參數設定為100%：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

若要將可排程核心的數目減少到最大計數的50%，請將**處理器效能核心停車最大核心**參數設定為50，如下所示：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>處理器效能核心停車公用程式散發

公用程式散發是 Windows Server 2016 中的演算法優化，其設計目的是為了改善某些工作負載的電源效率。 它會追蹤) 的無法移動的 CPU 活動 (，即 Dpc、插斷或嚴格相似化為的執行緒，並根據假設，所有可移動的工作都可以在所有已離開的核心上平均散發，來預測每個處理器的未來工作。

針對某些處理器的平衡電源計劃，預設會啟用公用程式散發功能。 它可以減少處於合理穩定狀態的工作負載所要求的 CPU 頻率，以降低處理器耗電量。 不過，公用程式散發不一定是適合大量活動高載的工作負載的理想演算法選擇，或是適用于工作負載快速且隨機轉移到處理器的程式。

針對這類工作負載，建議使用下列命令來停用公用程式散發：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="additional-references"></a>其他參考資料

- [伺服器硬體效能考量](../index.md)
- [伺服器硬體電源的考量](../power.md)
- [處理器電源管理](processor-power-management-tuning.md)
- [建議的平衡方案參數](recommended-balanced-plan-parameters.md)
