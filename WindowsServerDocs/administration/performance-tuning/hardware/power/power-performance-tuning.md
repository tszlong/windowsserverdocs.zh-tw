---
title: 能力和效能微調
description: 針對 Windows Server 平衡 」 電源計劃調整處理器電源管理 (PPM)
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 4ad58e9b477f61844dedd9f6638efb12f1a96500
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811572"
---
# <a name="power-and-performance-tuning"></a>電力和效能調整

能源效率就得越來越重要企業和資料中心環境中，並設定選項的混合新增另一組的權衡取捨。

Windows Server 2016 已針對最小效能影響的極佳的能源效率最佳化，跨各種不同的客戶工作負載。 [針對 Windows Server 平衡電源計劃的處理器電源管理 (PPM) 微調](processor-power-management-tuning.md)描述來進行微調 Windows Server 2016 中的預設參數的工作負載，並提供自訂 tunings 的建議。

此節則細述能源效率權衡取捨，可協助您做出明智的決策，如果您需要調整您的伺服器上的預設電源設定。 不過，大部分的伺服器硬體和工作負載應該不需要系統管理員 power 微調時執行 Windows Server 2016 中。

## <a name="calculating-server-energy-efficiency"></a>計算伺服器能源效率

當您調整您的伺服器可節約能源時，您還必須考量效能。 調整會影響效能和功能而論，有時在不相稱的數量。 針對每個可能的調整，請考慮以判斷是否可接受的取捨您 power 預算和效能目標。

您可以計算您的伺服器能源效率比例很有用的計量，其中包含能力和效能資訊。 能源效率是時間的，完成指定量時所需的平均乘冪的比例。

![能源效率公式](../../media/perftune-guide-power-formula.png)

若要設定採用能力和效能之間的權衡取捨的可行目標，您可以使用此計量。 相反地，目的 10%的能源節約跨資料中心無法擷取對應的效果對效能，反之亦然。

同樣地，如果您調整您的伺服器增加 5%，並導致較高的能源耗量 10%的效能，總的結果可能會或可能不會接受您的業務目標。 針對消息更靈通決策比單獨的電源或效能計量，可讓能源效率計量。

## <a name="measuring-system-energy-consumption"></a>測量系統能源消耗

微調您的能源效率的伺服器之前，您應該建立基準電量測量不太。

如果您的伺服器具有必要的支援，您可以使用電源計量和預算編列在 Windows Server 2016 的功能，若要使用效能監視器檢視系統層級的能源耗用量。

其中一種方式來判斷是否您的伺服器有支援的計量和預算是檢閱[Windows Server Catalog](http://www.windowsservercatalog.com)。 如果您的伺服器模型符合新 Enhanced Power Management 認證 Windows 硬體認證計劃中，一定會支援的計量和預算的功能。

檢查有計量支援的另一種方式是以手動方式尋找效能監視器中的計數器。 開啟效能監視器中，選取**新增計數器**，然後找出**鼣糪耵涫**計數器群組。

如果功率計的具名執行個體出現在方塊中標示**的選取物件的執行個體**、 您的平台支援計量。 **電源**計數器，顯示瓦的電力會出現在選取的計數器群組。 未指定確切的衍生的電源資料值。 比方說，它可能是即時的 power 繪製或部分的時間間隔內平均功率繪製。

如果您的伺服器平台不支援計量，您可以使用實體計量裝置連接到電源供應器輸入來測量系統電源繪製或能源消耗。

若要建立基準，您應該衡量在不同的系統負載點，從閒置，到 100%（最大輸送量） 來產生負載列所需的平均電力。 下圖顯示三個範例組態的負載行：

![載入列範例](../../media/perftune-guide-sample-loadlines.png)

您可以使用負載線來評估和比較的效能和能源消耗的組態完全載入點。 在此範例中，很容易了解的最佳組態。 不過，可以輕鬆地有一個組態最適合用於繁重的工作負載，而且其中一個最適合用於輕量工作負載。

您要徹底了解您工作負載的需求選擇最佳的設定。 請勿假設，當您找到正確的設定時，它會永遠維持最佳狀態。 您應該測量系統使用量和能源消耗定期變更前後的工作負載、 工作負載層級或伺服器硬體的變更。

## <a name="diagnosing-energy-efficiency-issues"></a>診斷能源效率的問題

**PowerCfg.exe**支援可用來分析您的伺服器的閒置的能源效率的命令列選項。 當您執行使用 PowerCfg.exe **/energy**選項時，此工具會執行 60 秒測試來偵測潛在節能效率的問題。 此工具會產生簡單的 HTML 報表中目前的目錄。

> [!Important]
> 若要確保精確分析，確定所有的本機應用程式都已關閉，然後再執行**PowerCfg.exe**。 

縮短計時器刻度率，缺少電源管理支援，以及 CPU 使用率過高是幾個行為所偵測到問題的驅動程式**powercfg /energy**命令。 這項工具提供簡單的方式來識別並修正電源管理問題，而且可能會導致在大型的資料中心的大幅降低成本。

如需 PowerCfg.exe 的詳細資訊，請參閱[使用 PowerCfg 評估系統能源效率](https://msdn.microsoft.com/windows/hardware/gg463250.aspx)。

## <a name="using-power-plans-in-windows-server"></a>使用 Windows Server 中的電源計劃

Windows Server 2016 已設計成符合業務需求的不同組的三個內建的電源計劃。 這些方案提供簡單的方法讓您自訂伺服器以符合能力與效能目標。 下表描述計劃，會列出常見的案例中要使用每個方案，並針對每個方案會提供一些實作詳細資料。

| **規劃** | **描述** | **常見的適用案例** | **實作重點** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 平衡 （建議） | 預設的設定。 目標的最小效能影響的良好的能源效率。 | 一般計算 | 符合需求的容量。 能源節約功能平衡能力和效能。 |
| 高效能 | 可以提升效能，但代價是高的能源耗用量。 適用於 power 和熱限制，操作 expenses 和可靠性的考量。 | 低延遲的應用程式和處理器效能變更相當敏感的應用程式程式碼 | 處理器一律會鎖定在最高的效能狀態 （包括 「 渦輪 」 頻率）。 所有的核心就是 unparked。 熱的輸出可能會很大。 |
| 省電 | 會限制效能，以節省能源和降低營運成本。 不建議未徹底測試，將確定效能就已足夠。 | 部署限制的功率預算和熱的條件約束 | 上限，限制的最大值 （如果支援），以百分比的處理器頻率，並讓其他的能源節約功能。 |


這些電源計劃存在於 Windows alternating 目前 (AC) 和直流電 (DC) 電源系統，但我們會假設伺服器一律使用 AC 電源來源。

如需電源計劃和電源原則設定的詳細資訊，請參閱[電源原則組態和部署 Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx)。

> [!Note]
> 某些伺服器製造商有自己的 BIOS 設定，透過提供的電源管理選項。 如果作業系統沒有電源管理的控制，變更在 Windows 中的電源計劃不會影響系統的能力和效能。

## <a name="tuning-processor-power-management-parameters"></a>調整處理器電源管理參數

每個電源計劃代表多個基礎的電源管理參數的組合。 內建的計劃是三個建議的設定，其中涵蓋各種不同的工作負載和案例的集合。 不過，我們了解這些計劃將不會符合每位客戶的需求。

下列各節會說明用來微調以符合目標不會由三個內建計劃解決一些特定處理器電源管理參數的方式。 如果您需要了解更多 power 參數陣列，請參閱[電源原則組態和部署 Windows](https://msdn.microsoft.com/windows/hardware/gg463243.aspx)。

## <a name="processor-performance-boost-mode"></a>處理器效能提升模式

Intel Turbo Boost 和 AMD 渦輪核心技術是允許處理器來達到額外的效能時最有用的功能 （也就是在高系統載入中）。 不過，這項功能會增加 CPU 核心的能源耗量，讓 Windows Server 2016 設定正在使用的特定處理器實作中的電源原則為基礎的渦輪技術。

渦輪已啟用所有的 Intel 和 AMD 處理器上的高效能電源計劃，而且它會停用電電源計劃。 依賴傳統 P 狀態為基礎的頻率管理的系統上的平衡 電源計劃，渦輪才會啟用預設的平台支援 EPB 暫存器。

> [!Note]
> EPB 暫存器只支援 Intel Westmere 和更新版本的處理器。

Intel Nehalem 和 AMD 處理器渦輪 P 狀態為基礎的平台上預設會停用。 不過，如果系統支援共同作業處理器效能控制項 (CPPC)，也就是新的替代模式的效能作業系統和硬體 （定義於 ACPI 5.0） 之間的通訊，渦輪可能會進行如果 Windows 作業系統系統動態要求要提供最高的效能層級的硬體。

若要啟用或停用 Turbo Boost 功能，處理器效能提升 Mode 參數必須設定系統管理員，或所選擇的電源計劃的預設參數設定。 處理器效能提升模式有五個允許的值，表 5 中所示。

P 狀態為基礎的控制項，這些選項會停用，已啟用 （渦輪可用硬體每當名義上的效能要求時），和高效率 （渦輪是 EPB 暫存器實作時，才提供使用）。

CPPC 型控制項，這些選項會停用，有效率的啟用 （Windows 指定渦輪提供確切的數量），而 「 積極 (Windows 會詢問 「 最大效能 」 啟用渦輪)。

Windows Server 2016 中提升模式的預設值為 3。

| **名稱** | **P 狀態為基礎的行為** | **CPPC 行為** |
|--------------------------|------------------------|-------------------|
| 0 （停用） | 已停用 | 已停用 |
| 1 （已啟用） | Enabled | 有效率的啟用 |
| 2 （危險） | Enabled | 積極 |
| 3 （有效率啟用） | 有效率 | 有效率的啟用 |
| 4 (有效率積極) | 有效率 | 積極 |

 
下列命令在目前的電源計劃上啟用處理器效能提升模式 （透過使用 GUID 別名中指定的原則）：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

> [!Important]
> 您必須執行**powercfg setactive**命令以啟用新的設定。 您不需要重新啟動伺服器。

若要設定此值以外的目前選取的計劃的電源計劃，您可以使用別名，例如配置\_MAX （省電）、 配置\_最小值 （高效能），以及配置\_平衡 （平衡） 來配置取代\_目前。 取代 「 目前 」 中配置的 powercfg-setactive 命令先前啟用的電源計劃，以顯示與所需的別名。

比方說，若要省電計劃中調整 Boost 模式，然後讓該電是目前的方案，請執行下列命令：

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>最小值和最大處理器效能狀態

處理器效能狀態 （P 狀態） 之間非常快速變更要求的比對供應、 在必要時提供的效能並節省能源，可能的話。 如果您的伺服器有高效能或最小功率耗用量的特定需求，您可以考慮設定**最少的處理器效能狀態**參數或**最大處理器效能狀態**參數。

值**最少的處理器效能狀態**並**最多的處理器效能狀態**參數會以百分比表示的最大處理器的頻率，範圍從 0 – 中的值100。

如果您的伺服器需要超低延遲，而異的 CPU 頻率 （例如，針對可重複的測試） 或最高的效能層級，您可能不想在切換至較低的效能狀態的處理器。 這類的伺服器，您可以使用下列命令來限制 100%的最小處理器效能狀態：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

如果您的伺服器需要較低的能源耗用量，您可能想要限制的最大百分比的處理器效能狀態。 例如，您也可以使用下列命令，以限制處理器 %到 75%其最大頻率：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

> [!Note]
> 達到上限的最大百分比的處理器效能需要處理器支援。 請檢查處理器文件，以判斷是否這類支援存在，或檢視的效能監視器計數器 **%的最大頻率**中**處理器**群組以查看是否任何頻率上限套用。

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>處理器效能增加和減少的臨界值和原則

處理器效能狀態來增加或減少的速度會受到多個參數。 下列四個參數都具有最明顯的影響：

-   **處理器效能增加閾值**定義使用率值以上的處理器效能狀態將會增加。 較大的值變慢的速率增加以回應增加的活動的效能狀態。

-   **處理器效能降低臨界值**定義使用率低於值以減少在處理器效能狀態。 較大的值會增加在閒置期間的效能狀態減少的速率。

-   **處理器效能增加原則和處理器效能會降低**原則可讓您判斷變更發生時，就應該設定哪些效能狀態。 「 單一 」 原則，表示它會選擇下一個狀態。 "Rocket"表示的最大或最少的電源效能狀態。 「 理想 」 會嘗試尋找能力和效能之間取得平衡。

比方說，如果您的伺服器需要超低延遲，一方面仍可受益於低電源閒置期間，您無法 quicken 增加任何負載增加的效能 」 狀態，然後緩慢減少的情形，當負載減少時。 下列命令會設定為"Rocket"增加原則更快速的狀態增加，並設定減少原則，來 「 單一 」。 增加與減少的臨界值會分別設定為 10 和 8。

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>暫止的最大和最小核心的處理器效能核心

Core 停車位是 Windows Server 2008 R2 中引進的功能。 處理器電源管理 (PPM) 引擎和排程器一起運作，以動態調整的可執行執行緒的核心數目。 PPM 引擎會選擇將排程的執行緒的核心數目下限。

通常已暫止的核心並沒有任何執行緒排程，並不會處理插斷、 Dpc 或其他完全是相似的工作時，它們會放入非常低電源狀態。 剩餘的核心就是負責的工作負載的其餘部分。 Core 停車可能可以提高能源效率較低使用量期間。

大多數伺服器的預設核心停車行為提供合理的平衡的輸送量和能源效率。 其中 core 停車可能不會顯示為很多優點泛用的工作負載的處理器，它可以是預設停用。

如果您的伺服器有暫止需求的特定核心，您可以控制 park 使用可用的核心數目**處理器效能核心暫止上限核心**參數或**處理器效能核心暫止核心下限**Windows Server 2016 中的參數。

Core 停車不一定最適合的其中一個案例是有一或多個作用中執行緒相似化為 NUMA 節點中的 Cpu 重要子集時 (也就是 1 個以上的 CPU，但大於或等於節點上的 Cpu 整組)。 當核心停車演算法挑選 unpark 的核心 （假設工作負載強度增加，就會發生），它可能不會永遠挑選內作用中的相似化的子集 （或子集），到 unpark，核心，並因此可能最後會 unparking 核心，不會實際是利用。

這些參數的值是介於 0 – 100 的百分比。 **處理器效能核心暫止上限核心**參數會控制可以 unparked （可執行執行緒） 的核心的最大百分比在任何時間，而**處理器效能核心暫止最小核心數**參數可控制的核心可 unparked 的最小百分比。 若要關閉核心停車位，請設定**處理器效能核心暫止最少核心**參數為 100%，使用下列命令：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

若要減少 50%的最大計數可排程的核心數目，設定**處理器效能核心暫止上限核心**參數設為 50，如下所示：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>處理器效能核心暫止公用程式發佈

公用程式的分佈是設計來改善某些工作負載的電源效率的 Windows Server 2016 中的演算法最佳化。 它會追蹤無法移動的 CPU 活動 （也就是 Dpc、 插斷或完全相似的執行緒），以及預測未來的工作，根據任何可移動工作可以平均分配給所有 unparked 核心的假設每個處理器上。

啟用公用程式的散發時，預設會針對某些處理器的平衡 電源計劃。 它可以降低要求的 CPU 頻率相當穩定狀態的工作負載，以減少處理器的功率耗用量。 不過，公用程式分佈並不一定是個好的演算法選擇，為受限於大量活動暴增的工作負載或其中的工作負載快速及隨意會轉移到處理器的程式。

針對這類工作負載，建議您停用公用程式的散發，使用下列命令：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="see-also"></a>另請參閱
- [伺服器硬體的效能考量](../index.md)
- [伺服器硬體電源的考量](../power.md)
- [處理器電源管理](processor-power-management-tuning.md)
- [建議的平衡方案參數](recommended-balanced-plan-parameters.md)
