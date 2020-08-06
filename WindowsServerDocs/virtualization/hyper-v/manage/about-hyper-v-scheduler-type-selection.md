---
title: 關於 Hyper-v 程式管理器排程器類型選取
description: 提供 Hyper-v 主機系統管理員使用 Hyper-v 排程器模式的相關資訊
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: 128f9d734311f8eaf0f06204e114171fa8b0f750
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/05/2020
ms.locfileid: "87768426"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>關於 Hyper-v 程式管理器排程器類型選取

適用於：

* Windows Server 2016
* Windows Server 1709 版
* Windows Server 1803 版
* Windows Server 2019

本檔說明 Hyper-v 的預設和建議使用的程式管理器排程器類型的重要變更。 這些變更會影響系統安全性和虛擬化效能。 虛擬化主機系統管理員應該檢查並瞭解本檔中所述的變更和含意，並仔細評估影響、建議的部署指引和風險因素，以充分瞭解如何在快速變更的安全性環境中部署和管理 Hyper-v 主機。

>[!IMPORTANT]
>目前已知的目擊北極熊在多個處理器架構中的側邊通道安全性弱點，可能會受到惡意來賓 VM 在具有同時執行多執行緒的主機上執行時的排程行為， (SMT) 啟用。  如果攻擊成功，惡意的工作負載可能會觀察到其分割區界限外的資料。 藉由設定 Hyper-v 虛擬機器來利用「虛擬程式核心排程器」類型並重新設定「來賓」 Vm，即可減輕此類攻擊。 使用核心排程器時，虛擬程式會將來賓 VM 的 VPs 限制在相同的實體處理器核心上執行，因此強烈將 VM 存取資料的能力，與其執行所在之實體核心的界限隔離。  這是對這些端通道攻擊具有高度效益的緩和措施，這可防止 VM 觀察其他分割區中的任何成品，不論是根或另一個來賓磁碟分割。  因此，Microsoft 會變更虛擬化主機和來賓 Vm 的預設和建議的設定。

## <a name="background"></a>背景

從 Windows Server 2016 開始，Hyper-v 支援數種排程和管理虛擬處理器的方法，稱為「虛擬程式管理者」。  如需所有「虛擬機器排程器」類型的詳細說明，請參閱[瞭解和使用 hyper-v 虛擬程式](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)排程器類型。

>[!NOTE]
>新的虛擬程式管理器排程器類型最初是在 Windows Server 2016 中引進，而且在舊版中無法使用。 Windows Server 2016 之前的所有 Hyper-v 版本僅支援傳統的排程器。 核心排程器的支援僅限最近發行。

## <a name="about-hypervisor-scheduler-types"></a>關於程式管理器排程器類型

本文特別著重于使用新的「虛擬機器核心排程器」類型與舊版「傳統」排程器，以及這些排程器類型與對稱多重執行緒或 SMT 的使用如何相交。  請務必瞭解核心和傳統排程器之間的差異，以及每個位置如何從基礎系統處理器上的來賓 Vm 工作。

### <a name="the-classic-scheduler"></a>傳統排程器

傳統排程器指的是公平共用的迴圈配置資源方法，可在虛擬處理器上排定工作 (VPs) 跨系統，包括根 VPs，以及屬於來賓 Vm 的 VPs。 傳統排程器已是所有 Hyper-v (的預設排程器類型，直到 Windows Server 2019 為止，如這裡所述) 。  傳統排程器的效能特性已充分瞭解，並示範傳統排程器，以 ably 支援工作負載的過度訂用帳戶，也就是根據虛擬化的工作負載類型、整體資源使用率等 ) ，以合理的邊界來過度訂用主機 VP： LP 比率 (。

在已啟用 SMT 的虛擬化主機上執行時，傳統排程器會從屬於核心的每個 SMT 執行緒上的任何 VM，分別排定來賓 VPs。 因此，不同的 Vm 可以同時在相同的核心上執行 (一個在核心的一個執行緒上執行的 VM，而另一個 VM 則) 。

### <a name="the-core-scheduler"></a>核心排程器

核心排程器會利用 SMT 的屬性來隔離來賓工作負載，這會影響安全性和系統效能。 核心排程器可確保來自 VM 的 VPs 會排定在兄弟 SMT 執行緒上。 這是以對稱方式完成，因此，如果 LPs 位於兩個群組中，VPs 會以兩個群組排程，而 Vm 之間永遠不會共用系統 CPU 核心。

藉由在基礎 SMT 配對上排定來賓 VPs，核心排程器會為工作負載隔離提供強大的安全性界限，而且也可以用來降低延遲敏感工作負載的效能變化。

請注意，如果為虛擬機器排程 VP，而未啟用 SMT，則 VP 會在執行時使用整個核心，而核心的同輩 SMT 執行緒將會閒置。  這是為了提供正確的工作負載隔離，但會影響整體系統效能，尤其是當系統 LPs 變成過度訂閱時，亦即，總 VP： LP 比率超過1:1。 因此，針對每個核心而未使用多個執行緒設定的執行中虛擬機器，是一項最理想的設定。

### <a name="benefits-of-the-using-the-core-scheduler"></a>使用核心排程器的優點

核心排程器提供下列優點：

* 來賓工作負載隔離的強式安全性界限-來賓 VPs 受限於在基礎實體核心配對上執行，以降低對側通道窺探攻擊的弱點。

* 降低工作負載變化-來賓工作負載輸送量的變化大幅降低，提供更高的工作負載一致性。

* 在來賓 Vm 中使用 SMT-來賓虛擬機器中執行的 OS 和應用程式可以利用 SMT 行為和程式設計介面 (Api) 來控制和散佈 SMT 執行緒之間的工作，就像執行非虛擬化一樣。

核心排程器目前用於 Azure 虛擬化主機，特別是利用強式安全性界限和低工作負載 variabilty。 Microsoft 認為核心排程器類型應該是，而且會繼續成為大多數虛擬化案例的預設「虛擬程式管理類型」。  因此，為了確保我們的客戶預設是安全的，Microsoft 現在會對 Windows Server 2019 進行這項變更。

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>對來賓工作負載的核心排程器效能影響

雖然必須有效地降低特定的弱點類別，但核心排程器可能也會降低效能。 客戶可能會看到其 Vm 的效能特性有差異，並會影響其虛擬化主機的整體工作負載容量。 在核心排程器必須執行非 SMT 副總裁的情況下，只有基礎邏輯核心中的其中一個指令串流會執行，而另一個則必須保持閒置。 這會限制來賓工作負載的主機容量總計。

遵循本檔中的部署指引，可以將這些效能影響降到最低。 主機系統管理員必須仔細考慮其特定的虛擬化部署案例，並根據最大的工作負載密度、虛擬化主機的匯總等需求，平衡其對安全性風險的承受度。

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>Windows Server 2016 和 Windows Server 2019 的預設和建議設定的變更

部署具有最大安全性狀態的 Hyper-v 主機時，需要使用「虛擬程式核心排程器」類型。 為確保我們的客戶預設是安全的，Microsoft 會變更下列預設和建議的設定。

>[!NOTE]
>雖然在 Windows Server 2016、Windows Server 1709 和 Windows Server 1803 的初始版本中已包含管理器的內部支援，但需要更新才能存取設定控制項，這可讓您選取「虛擬程式」排程器類型。  如需這些更新的詳細資訊，請參閱[瞭解和使用 hyper-v 虛擬機器管理器類型](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)。

### <a name="virtualization-host-changes"></a>虛擬化主機變更

* 根據預設，在 Windows Server 2019 開始時，虛擬程式會使用核心排程器。

* Microsoft reccommends 在 Windows Server 2016 上設定核心排程器。 Windows Server 2016 支援「虛擬程式核心排程器」類型，但預設為傳統排程器。 核心排程器是選擇性的，且必須由 Hyper-v 主機管理員明確啟用。

### <a name="virtual-machine-configuration-changes"></a>虛擬機器設定變更

* 在 Windows Server 2019 上，使用預設 VM 9.0 版建立的新虛擬機器將會自動繼承 SMT 內容 (啟用或停用虛擬化主機) 。 也就是說，如果實體主機上已啟用 SMT，新建立的 Vm 也會啟用 SMT，而且預設會繼承主機的 SMT 拓撲，而 VM 的硬體執行緒數目與基礎系統相同。 這會反映在 VM 的設定中，其 HwThreadCountPerCore = 0，其中0表示 VM 應繼承主機的 SMT 設定。

* VM 版本為8.2 或更早版本的現有虛擬機器將會保留其原始 VM 處理器設定以進行 HwThreadCountPerCore，而 8.2 VM 版本來賓的預設值為 HwThreadCountPerCore = 1。 當這些來賓在 Windows Server 2019 主機上執行時，系統會將其視為如下：

    1. 如果 VM 的 VP 計數小於或等於 LP 核心的計數，則核心排程器會將 VM 視為非 SMT VM。 當來賓 VP 在單一 SMT 執行緒上執行時，將會閒置核心的兄弟 SMT 執行緒。 這不是最佳做法，而且會導致整體效能損失。

    2. 如果 VM 具有比 LP 核心更多的 VPs，則核心排程器會將 VM 視為 SMT VM。 不過，VM 不會觀察到它是 SMT VM 的其他指示。 例如，使用 CPUID 指示或 Windows Api 來查詢 OS 或應用程式的 CPU 拓撲，將不會指出 SMT 已啟用。

* 當現有 VM 從更早版本 VM 版本明確更新至9.0 版時，會透過更新 VM 作業，讓 VM 保留其目前的 HwThreadCountPerCore 值。  VM 將不會啟用 SMT 強制。

* 在 Windows Server 2016 上，Microsoft 建議您啟用來賓 Vm 的 SMT。  根據預設，在 Windows Server 2016 上建立的 Vm 會停用 SMT，而 HwThreadCountPerCore 會設定為1，除非明確變更。

>[!NOTE]
>Windows Server 2016 不支援將 HwThreadCountPerCore 設定為0。

#### <a name="managing-virtual-machine-smt-configuration"></a>管理虛擬機器 SMT 設定

[來賓虛擬機器 SMT] 設定是以每個 VM 為基礎。 主機管理員可以檢查並設定 VM 的 SMT 設定，以從下列選項中選取：

1. 將 Vm 設定為以啟用 SMT 的方式執行，並選擇性地自動繼承主機 SMT 拓撲

2. 將 Vm 設定為以非 SMT 的身分執行

VM 的 [SMT] configuaration 會顯示在 [Hyper-v 管理員] 主控台的 [摘要] 窗格中。  您可以使用 VM 設定或 PowerShell 來設定 VM 的 SMT 設定。

#### <a name="configuring-vm-smt-settings-using-powershell"></a>使用 PowerShell 進行 VM SMT 設定

若要設定來賓虛擬機器的 SMT 設定，請以足夠的許可權開啟 PowerShell 視窗，然後輸入：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

其中：

- 0 = 從主機繼承 SMT 拓撲 (此設定的 HwThreadCountPerCore = 0 在 Windows Server 2016 上不受支援) 

- 1 = 非 SMT

- 值 > 1 = 每個核心所需的 SMT 執行緒數目。 不可超過每個核心的實體 SMT 執行緒數目。

若要讀取來賓虛擬機器的 SMT 設定，請以足夠的許可權開啟 PowerShell 視窗，然後輸入：

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

請注意，使用 HwThreadCountPerCore = 0 設定的來賓 Vm，表示將為來賓啟用 SMT，而且會公開相同數目的 SMT 執行緒給來賓，如同在基礎虛擬化主機上（通常是2）。

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>來賓 Vm 可能會觀察到跨 VM 行動性案例的 CPU 拓撲變更

VM 中的 OS 和應用程式可能會看到 VM 生命週期事件前後的主機和 VM 設定變更，例如即時移轉或儲存和還原作業。 在儲存和還原 VM 狀態的作業期間，VM 的 HwThreadCountPerCore 設定和實現的值 (也就是，會遷移 VM 設定和來源主機之 configuration) 的計算組合。 VM 將會繼續在目的地主機上使用這些設定來執行。 VM 在關閉並重新啟動時，可能會變更 VM 觀察到的實現值。 這應該是良性的，因為作業系統和應用層軟體應該在其正常啟動和初始化程式碼流程中尋找 CPU 拓撲資訊。 不過，因為這些開機時間初始化順序會在即時移轉或儲存/還原作業期間略過，所以通過這些狀態轉換的 Vm 可能會觀察到原始計算的已實現值，直到它們關閉並重新啟動為止。

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>有關非最佳 VM 設定的警示

設定的虛擬機器所使用的 VPs 超過主機上的實體核心時，會導致不理想的設定。 虛擬程式管理者會將這些 Vm 視為 SMT 感知。 不過，VM 中的 OS 和應用程式軟體會顯示 CPU 拓撲，顯示 SMT 已停用。 當偵測到此狀況時，Hyper-v 工作者進程將會在虛擬化主機上記錄事件，警告主機管理員 VM 的設定不是最佳，而且建議您為 VM 啟用 SMT。

#### <a name="how-to-identify-non-optimally-configured-vms"></a>如何識別未優化的已設定 Vm

您可以藉由檢查 Hyper-v 工作者進程事件識別碼3498的事件檢視器中的系統記錄檔來識別非 SMT 的 Vm，每當 VM 中的 VPs 數目大於實體核心計數時，就會觸發該 VM。 工作者進程事件可以從事件檢視器或透過 PowerShell 取得。

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>使用 PowerShell 查詢 Hyper-v 工作者進程 VM 事件

若要使用 PowerShell 查詢 Hyper-v 工作者進程事件識別碼3498，請從 PowerShell 提示字元輸入下列命令。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>來賓 SMT configuaration 對客體作業系統使用虛擬機器 enlightenment 的影響

Microsoft 虛擬程式提供多個 enlightenment 或提示，在來賓 VM 中執行的 OS 可能會進行查詢和使用以觸發優化，例如可能受益于效能，或在執行虛擬化時改善各種狀況的處理方式。 其中一個最近引進的啟蒙教學，就是處理虛擬處理器排程，以及使用作業系統緩和措施來進行攻擊的 SMT。

>[!NOTE]
>Microsoft 建議主機系統管理員啟用來賓 Vm 的 SMT，以將工作負載效能優化。

以下提供此來賓啟蒙教學的詳細資料，不過，虛擬化主機系統管理員的主要重點是虛擬機器應該將 HwThreadCountPerCore 設定為符合主機的實體 SMT 設定。 這可讓管理人員報告沒有非架構核心共用。 因此，可能會啟用任何需要啟蒙教學之支援優化的虛擬作業系統。 在 Windows Server 2019 上，建立新的 Vm，並保留預設值 HwThreadCountPerCore (0) 。 從 Windows Server 2016 主機遷移的較舊 Vm 可以更新為 Windows Server 2019 設定版本。 這麼做之後，Microsoft 建議您設定 HwThreadCountPerCore = 0。  在 Windows Server 2016 上，Microsoft 建議您將 HwThreadCountPerCore 設定為符合主機配置 (通常是 2) 。

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>NoNonArchitecturalCoreSharing 啟蒙教學詳細資料

從 Windows Server 2016 開始，基礎程式會定義新的啟蒙教學，以描述它對虛擬作業系統的副總排程和位置的處理。 此啟蒙教學定義于[虛擬機器的頂層功能規格 v 5.0 c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs)中。

「虛擬程式」綜合 CPUID 的 cpuid 0x40000004。 EAX： 18 [NoNonArchitecturalCoreSharing = 1] 表示虛擬處理器絕對不會與另一個虛擬處理器共用實體核心，但回報為「同輩 SMT」執行緒的虛擬處理器除外。 例如，來賓副總不會在 SMT 執行緒上執行，而根副總會同時在相同處理器核心上的兄弟 SMT 執行緒上執行。 只有在執行虛擬化時才可能發生這種情況，因此代表也會有嚴重安全性影響的非架構 SMT 行為。 虛擬作業系統可以使用 NoNonArchitecturalCoreSharing = 1 來表示啟用優化，這可能有助於避免設定 STIBP 的效能負擔。

在某些設定中，管理程式不會指出 NoNonArchitecturalCoreSharing = 1。 例如，如果主機已啟用 SMT，且設定為使用「虛擬程式」（傳統）排程器，NoNonArchitecturalCoreSharing 將會是0。 這可能會讓啟用 rms 來賓無法啟用特定的優化。 因此，Microsoft 建議使用 SMT 的主機管理員依賴「虛擬程式核心排程器」，並確保虛擬機器已設定為從主機繼承 SMT 設定，以確保工作負載效能達到最佳。

## <a name="summary"></a>總結

安全性威脅環境會持續進化。 為確保我們的客戶預設是安全的，Microsoft 會變更從 Windows Server 2019 Hyper-v 開始的基礎程式和虛擬機器的預設設定，並為執行 Windows Server 2016 Hyper-v 的客戶提供更新的指引和建議。 虛擬化主機系統管理員應該：

* 閱讀並瞭解本檔中提供的指導方針

* 仔細評估並調整其虛擬化部署，以確保它們符合其獨特需求的安全性、效能、虛擬化密度和工作負載回應目標

* 請考慮重新設定現有的 Windows Server 2016 Hyper-v 主機，以利用由虛擬程式核心排程器所提供的強大安全性優勢

* 更新現有的非 SMT Vm，以減少因應硬體安全性弱點之 VP 隔離所強加的排程條件約束所造成的效能影響
