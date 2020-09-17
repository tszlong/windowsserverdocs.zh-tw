---
title: 關於 Hyper-v 管理程式排程器類型選取專案
description: 提供 Hyper-v 主機系統管理員使用 Hyper-v 排程器模式的相關資訊
ms.author: benarm
author: BenjaminArmstrong
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: 9c41dfb5bad28122f8c2a6b06ff6574acd89a9ec
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746623"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>關於 Hyper-v 管理程式排程器類型選取專案

適用於：

* Windows Server 2016
* Windows Server 1709 版
* Windows Server 1803 版
* Windows Server 2019

本檔說明 Hyper-v 預設和建議使用的程式管理器排程器類型的重要變更。 這些變更會影響系統安全性和虛擬化效能。 虛擬化主機系統管理員應複習並瞭解這份檔中所述的變更和含意，並仔細評估影響、建議的部署指引和風險因素，以充分瞭解如何在面對快速變化的安全性環境時部署及管理 Hyper-v 主機。

>[!IMPORTANT]
>Sighted 在多個處理器架構中的已知側邊通道安全性弱點，可能會透過舊版虛擬器傳統排程器類型在具有同時多執行緒處理 (SMT) 啟用的主機上執行時，受到惡意的來賓 VM 利用。  如果成功遭到惡意探索，惡意工作負載可能會觀察到資料分割界限之外的資料。 您可以藉由設定 Hyper-v 虛擬程式來利用虛擬程式核心排程器類型並重新設定來賓 Vm，來減輕這類攻擊。 使用核心排程器時，虛擬程式會限制來賓 VM 的 VPs 在相同的實體處理器核心上執行，因此會將資料存取其執行所在實體核心界限的能力緊密隔離。  這是對這些側邊通道攻擊的高度有效緩和措施，可防止 VM 觀察來自其他資料分割（不論是根或其他來賓磁碟分割）的任何成品。  因此，Microsoft 會變更虛擬化主機和來賓 Vm 的預設和建議的設定。

## <a name="background"></a>背景

從 Windows Server 2016 開始，Hyper-v 支援數種方式來排程和管理虛擬處理器，稱為基礎程式排程器類型。  如需所有程式管理器排程器類型的詳細說明 [，請參閱瞭解及使用 hyper-v 程式管理器類型](./manage-hyper-v-scheduler-types.md)。

>[!NOTE]
>新的程式管理器排程器類型首次在 Windows Server 2016 中引進，在舊版中無法使用。 Windows Server 2016 之前的所有 Hyper-v 版本僅支援傳統排程器。 最新發行的核心排程器支援。

## <a name="about-hypervisor-scheduler-types"></a>關於程式管理器排程器類型

本文特別著重于如何使用新的「虛擬程式核心」排程器類型與舊版「傳統」排程器，以及這些排程器型別如何與對稱式多執行緒或 SMT 的使用交集。  請務必瞭解核心和傳統排程器之間的差異，以及每個位置在基礎系統處理器上的來賓 Vm 之間的運作方式。

### <a name="the-classic-scheduler"></a>傳統排程器

傳統排程器指的是公平共用、迴圈配置資源方法，用於排程虛擬處理器上的工作 (VPs) 整個系統，包括根 VPs 以及屬於來賓 Vm 的 VPs。 傳統排程器是在所有 Hyper-v 版本上使用的預設排程器類型 (在 Windows Server 2019 之前，如本文所述) 。  傳統排程器的效能特性很容易理解，而傳統排程器會示範 ably 支援過度訂用工作負載，也就是根據虛擬化工作負載的類型、整體的資源使用率等 ) ，將主機 VP： LP 比例的訂 (用帳戶。

在啟用 SMT 的虛擬化主機上執行時，傳統排程器會從屬於核心的每個 SMT 執行緒上的任何 VM 排程來賓 VPs。 因此，不同的 Vm 可以同時在相同的核心上執行 (一個在核心的某個執行緒上執行的 VM，另一個則是在另一個執行緒) 上執行的 vm。

### <a name="the-core-scheduler"></a>核心排程器

核心排程器會利用 SMT 的屬性來提供來賓工作負載的隔離，而這會影響安全性和系統效能。 核心排程器可確保來自 VM 的 VPs 已排程在同級 SMT 執行緒上。 這是以對稱方式完成，因此如果 LPs 是在兩個群組中，則 VPs 會以兩組的群組進行排程，而系統 CPU 核心絕對不會在 Vm 間共用。

藉由排程基礎 SMT 配對上的來賓 VPs，核心排程器可為工作負載隔離提供強式安全性界限，也可以用來降低延遲敏感工作負載的效能變化性。

請注意，當 VP 已針對未啟用 SMT 的虛擬機器進行排程時，該 VP 將會在執行時使用整個核心，而核心的同級 SMT 執行緒將會閒置。  這是提供正確工作負載隔離的必要動作，但會影響整體系統效能，特別是當系統 LPs 變成過度訂閱時，也就是總 VP： LP 比率超過1:1。 因此，在每個核心上執行設定為不含多個執行緒的來賓 Vm 是次佳的設定。

### <a name="benefits-of-the-using-the-core-scheduler"></a>使用核心排程器的優點

核心排程器提供下列優點：

* 來賓工作負載隔離的增強式安全性界限-來賓 VPs 受限於在基礎實體核心配對上執行，減少對側通道窺探攻擊的弱點。

* 降低工作負載的變異-來賓工作負載輸送量變化大幅降低，並提供更高的工作負載一致性。

* 在來賓 Vm 中使用 SMT-在來賓虛擬機器中執行的 OS 和應用程式可以使用 SMT 行為和程式設計介面 (Api) 來控制和散發跨 SMT 執行緒的工作，就像執行非虛擬化一樣。

核心排程器目前用於 Azure 虛擬化主機，特別是利用強式安全性界限和低工作量 variabilty。 Microsoft 認為核心排程器類型應該是，而且會繼續成為大部分虛擬化案例的預設程式管理器排程類型。  因此，為了確保我們的客戶預設是安全的，Microsoft 現在會對 Windows Server 2019 進行這項變更。

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>對來賓工作負載的核心排程器效能影響

為了有效地緩和某些弱點類別，核心排程器也可能會降低效能。 客戶可能會看到其 Vm 的效能特性有所差異，並影響其虛擬化主機的整體工作負載容量。 在核心排程器必須執行非 SMT VP 的情況下，只有基礎邏輯核心中的其中一個指令串流會執行，另一個則必須保持閒置。 這會限制來賓工作負載的主機容量總計。

您可以遵循本檔中的部署指引，將這些效能影響降到最低。 主機系統管理員必須仔細考慮他們的特定虛擬化部署案例，並根據最大的工作負載密度、過度匯總的虛擬化主機等需求，平衡其對安全性風險的承受度。

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>Windows Server 2016 和 Windows Server 2019 的預設和建議設定變更

若要部署具有最大安全性狀態的 Hyper-v 主機，則必須使用「虛擬程式核心」排程器類型。 為了確保我們的客戶預設是安全的，Microsoft 會變更下列預設和建議的設定。

>[!NOTE]
>雖然 Windows Server 2016、Windows Server 1709 和 Windows Server 1803 的初始發行版本中包含了程式管理人員的內部支援，但需要更新才能存取設定控制項，以允許選取虛擬程式排程器的類型。  如需這些更新的詳細資訊，請參閱 [瞭解及使用 hyper-v 程式管理](./manage-hyper-v-scheduler-types.md) 器排程器類型。

### <a name="virtualization-host-changes"></a>虛擬化主機變更

* 根據預設，從 Windows Server 2019 開始，此虛擬程式會使用核心排程器。

* Microsoft reccommends 在 Windows Server 2016 上設定核心排程器。 Windows Server 2016 支援「虛擬程式核心」排程器類型，但預設值為「傳統排程器」。 核心排程器是選擇性的，且必須由 Hyper-v 主機系統管理員明確啟用。

### <a name="virtual-machine-configuration-changes"></a>虛擬機器設定變更

* 在 Windows Server 2019 上，使用預設 VM 9.0 版建立的新虛擬機器將會自動繼承虛擬化主機)  (啟用或停用的 SMT 屬性。 也就是說，如果實體主機上已啟用 SMT，則新建立的 Vm 也會啟用 SMT，並且預設會繼承主機的 SMT 拓撲，而且 VM 的每個核心都有相同數目的硬體執行緒作為基礎系統。 這會反映在 VM 的設定中，HwThreadCountPerCore = 0，其中0表示 VM 應繼承主機的 SMT 設定。

* VM 版本為8.2 或更早版本的現有虛擬機器將會保留其原始的 VM 處理器設定以進行 HwThreadCountPerCore，而 8.2 VM 版本來賓的預設值為 HwThreadCountPerCore = 1。 當這些來賓在 Windows Server 2019 主機上執行時，系統會將其視為如下所示：

    1. 如果 VM 的 VP 計數小於或等於 LP 核心的計數，核心排程器會將 VM 視為非 SMT 的 VM。 當來賓 VP 在單一 SMT 執行緒上執行時，核心的同級 SMT 執行緒將會閒置。 這是不理想的做法，會導致整體效能損失。

    2. 如果 VM 的 VPs 比 LP 核心更多，核心排程器會將 VM 視為 SMT VM。 不過，VM 不會看到它是 SMT VM 的其他指示。 例如，使用 CPUID 指令或 Windows Api 來查詢 OS 或應用程式所拓撲的 CPU，將不會指出 SMT 已啟用。

* 從 dsc-pullserver VM 版本明確地將現有 VM 更新為9.0 版時，VM 會保留其目前的 HwThreadCountPerCore 值。  VM 將不會啟用 SMT 強制啟用。

* 在 Windows Server 2016 上，Microsoft 建議啟用來賓 Vm 的 SMT。  根據預設，在 Windows Server 2016 上建立的 Vm 會 SMT 停用，除非明確變更，否則 HwThreadCountPerCore 會設定為1。

>[!NOTE]
>Windows Server 2016 不支援將 HwThreadCountPerCore 設定為0。

#### <a name="managing-virtual-machine-smt-configuration"></a>管理虛擬機器 SMT 設定

來賓虛擬機器 SMT 設定是以每個 VM 為基礎。 主機系統管理員可以檢查並設定 VM 的 SMT 設定，以從下列選項中選取：

1. 將 Vm 設定為以啟用 SMT 的方式執行，選擇性地自動繼承主機 SMT 拓撲

2. 將 Vm 設定為以非 SMT 的方式執行

VM 的 SMT configuaration 會顯示在 [Hyper-v 管理員] 主控台的 [摘要] 窗格中。  您可以使用 VM 設定或 PowerShell 來設定 VM 的 SMT 設定。

#### <a name="configuring-vm-smt-settings-using-powershell"></a>使用 PowerShell 設定 VM SMT 設定

若要設定來賓虛擬機器的 SMT 設定，請以足夠的許可權開啟 PowerShell 視窗，然後輸入：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

其中：

- 0 = 從主機繼承 SMT 拓撲 (在 Windows Server 2016 上不支援 HwThreadCountPerCore = 0 的這個設定) 

- 1 = 非 SMT

- 值 > 1 = 每個核心所需的 SMT 執行緒數目。 不可超過每個核心的實體 SMT 執行緒數目。

若要讀取來賓虛擬機器的 SMT 設定，請以足夠的許可權開啟 PowerShell 視窗，然後輸入：

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

請注意，使用 HwThreadCountPerCore = 0 設定的來賓 Vm 表示將會為來賓啟用 SMT，並且會將相同數目的 SMT 執行緒公開給來賓，因為它們在基礎虛擬化主機上，通常是2。

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>來賓 Vm 可能會觀察到跨 VM 行動案例的 CPU 拓撲變更

VM 中的 OS 和應用程式可能會看到 VM 生命週期事件（例如，即時移轉或儲存和還原作業）前後的主機和 VM 設定變更。 在儲存和還原 VM 狀態的作業期間，VM 的 HwThreadCountPerCore 設定和實現值 (也就是，VM 設定和來源主機設定) 的計算組合會遷移。 VM 將繼續以目的地主機上的這些設定執行。 當 VM 關機並重新啟動時，VM 觀察到的實現值可能會變更。 這應該是良性的，因為作業系統和應用層軟體應該在其正常啟動和初始化程式碼流程中尋找 CPU 拓撲資訊。 不過，由於這些開機時間初始化順序會在即時移轉或儲存/還原作業期間略過，因此採用這些狀態轉換的 Vm 可能會觀察到原本計算的實現值，直到它們關閉並重新啟動為止。

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>有關非最佳 VM 設定的警示

設定的虛擬機器比主機上的實體核心更 VPs，會導致非最佳設定。 虛擬程式管理器排程器會將這些 Vm 視為 SMT 感知。 不過，VM 中的 OS 和應用程式軟體將會顯示一個 CPU 拓撲，顯示 SMT 已停用。 偵測到這種情況時，Hyper-v 工作者進程會在虛擬化主機上記錄事件，警告主機系統管理員 VM 的設定不是最佳的，並建議為 VM 啟用 SMT。

#### <a name="how-to-identify-non-optimally-configured-vms"></a>如何識別非優化設定的 Vm

您可以檢查 SMT 的系統記錄檔中的系統記錄檔，以找出非的 Vm，這會事件檢視器在 VM 中的 VPs 數目大於實體核心計數時，針對 VM 觸發此事件識別碼3498。 背景工作進程事件可以從事件檢視器或透過 PowerShell 取得。

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>使用 PowerShell 查詢 Hyper-v 背景工作進程 VM 事件

若要使用 PowerShell 查詢 Hyper-v 背景工作進程事件識別碼3498，請從 PowerShell 提示字元中輸入下列命令。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>來賓 SMT configuaration 對客體作業系統使用虛擬程式 enlightenments 的影響

Microsoft 虛擬程式提供多個 enlightenments 或提示，在來賓 VM 中執行的作業系統可能會查詢並使用它來觸發優化，例如可能會受益于效能，或在執行虛擬化時改善各種狀況的處理方式。 最近引進的啟蒙會考慮處理虛擬處理器排程，以及利用 SMT 的側邊通道攻擊的 OS 緩和措施。

>[!NOTE]
>Microsoft 建議主機系統管理員為來賓 Vm 啟用 SMT，以將工作負載效能優化。

此來賓啟蒙的詳細資料如下所示，但虛擬化主機系統管理員的主要重點是虛擬機器應該設定 HwThreadCountPerCore，以符合主機的實體 SMT 設定。 這可讓程式管理人員報告沒有非架構核心共用。 因此，任何支援需要啟蒙之優化的客體作業系統都可以啟用。 在 Windows Server 2019 上，建立新的 Vm，並保留預設值 HwThreadCountPerCore (0) 。 從 Windows Server 2016 主機遷移的較舊 Vm 可以更新至 Windows Server 2019 設定版本。 這麼做之後，Microsoft 建議設定 HwThreadCountPerCore = 0。  在 Windows Server 2016 上，Microsoft 建議設定 HwThreadCountPerCore，使其符合主機配置 (通常是 2) 。

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>NoNonArchitecturalCoreSharing 啟蒙詳細資料

從 Windows Server 2016 開始，基礎程式會定義新的啟蒙，以描述其對虛擬作業系統的副總排程和位置處理。 此啟蒙是在程式 [管理程式最上層功能規格 v v c](/virtualization/hyper-v-on-windows/reference/tlfs)中定義。

虛擬程式綜合 CPUID 的0x40000004： 18 [NoNonArchitecturalCoreSharing = 1] 表示虛擬處理器永遠不會與另一個虛擬處理器共用實體核心，除了回報為同輩 SMT 執行緒的虛擬處理器之外。 例如，來賓 VP 永遠不會在 SMT 執行緒上執行，並在相同處理器核心的同級 SMT 執行緒上同時執行的根副總。 只有在執行虛擬化時，才可能發生這種情況，因此表示也有嚴重安全性含意的非架構 SMT 行為。 來賓 OS 可以使用 NoNonArchitecturalCoreSharing = 1，表示可以安全地啟用優化，這有助於避免設定 STIBP 的效能額外負荷。

在某些設定中，管理程式不會指出 NoNonArchitecturalCoreSharing = 1。 例如，如果主機已啟用 SMT，且已設定為使用「虛擬程式管理器傳統排程器」，則 NoNonArchitecturalCoreSharing 將會是0。 這可能會導致啟用 rms 的來賓無法啟用某些優化。 因此，Microsoft 建議使用 SMT 的主機系統管理員依賴虛擬程式核心排程器，並確定虛擬機器已設定為從主機繼承其 SMT 設定，以確保工作負載效能最佳。

## <a name="summary"></a>摘要

安全性威脅的環境持續演進。 為了確保我們的客戶預設是安全的，Microsoft 會變更從 Windows Server 2019 Hyper-v 開始之虛擬機器和虛擬機器的預設設定，並為執行 Windows Server 2016 Hyper-v 的客戶提供更新的指引和建議。 虛擬化主機管理員應該：

* 閱讀並瞭解本檔中提供的指導方針

* 仔細評估並調整其虛擬化部署，以確保它們符合其獨特需求的安全性、效能、虛擬化密度和工作負載回應目標

* 請考慮重新設定現有的 Windows Server 2016 Hyper-v 主機，以利用由虛擬程式核心排程器所提供的強大安全性優勢

* 更新現有的非 SMT Vm，以降低解決硬體安全性漏洞之 VP 隔離所加諸的排程條件約束所造成的效能影響