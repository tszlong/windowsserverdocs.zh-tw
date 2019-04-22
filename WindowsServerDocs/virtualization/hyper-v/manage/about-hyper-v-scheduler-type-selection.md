---
title: 關於 HYPER-V hypervisor 排程器類型選取項目
description: 提供 HYPER-V 主機系統管理員的資訊上使用 HYPER-V 的排程器模式
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: c5360d8e2fdc23f8b05c6be0f665407eebedeba2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823879"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>關於 HYPER-V hypervisor 排程器類型選取項目

適用於：

* Windows Server 2016
* Windows Server 版本 1709
* Windows Server 版本 1803
* Windows Server 2019

本文件說明 HYPER-V 的預設值的重要變更，並建議使用 hypervisor 的排程器的類型。 這些變更會影響這兩個系統的安全性和虛擬化的效能。 虛擬化主機系統管理員應該檢閱並了解的變更和此文件中所述的含意並仔細評估影響建議的部署指引，以充分了解如何部署和管理所涉及的風險因素面臨瞬息萬變的安全性架構的 HYPER-V 主機。

>[!IMPORTANT]
>目前已知在多重處理器架構中發現的弱點可能會遭惡意客體 VM 透過並行的主機上執行時，舊版的 hypervisor 傳統的排程器型別的排程行為的側邊通道安全性啟用多執行緒 （smt） 即可。  如果成功惡意探索，惡意的工作負載會觀察到其資料分割界限外的資料。 藉由設定 HYPER-V hypervisor 利用 hypervisor 核心排程器型別與重新設定客體 Vm，可以減輕此類的攻擊。 使用核心排程器中，hypervisor 會限制在相同的實體處理器核心，因此強烈隔離的 VM 能夠存取資料，其執行所在之實體核心的界限上執行的客體 VM 的 VPs。  這是非常有效防護側邊通道攻擊，這是為了避免 VM 是否觀察從其他分割區中，任何成品根或另一個客體資料分割。  因此，Microsoft 正在變更預設值，並建議使用虛擬化主機和客體 Vm 的組態設定。

## <a name="background"></a>背景

從 Windows Server 2016 開始，HYPER-V 支援排程和管理虛擬處理器，稱為 hypervisor 排程器類型的數種的方法。  Hypervisor 排程器的所有類型的詳細的描述可在[了解與使用 HYPER-V hypervisor 排程器類型](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)。

>[!NOTE]
>新的 hypervisor 排程器類型首次引進使用 Windows Server 2016，，和舊版本中未提供。 所有版本的 Windows Server 2016 之前的 HYPER-V 都支援傳統排程器。 核心排程器的支援是最近才發行。

## <a name="about-hypervisor-scheduler-types"></a>關於 hypervisor 排程器型別

這篇文章特別著重於使用舊版 「 傳統 」 的排程器中，與新的 hypervisor 核心排程器類型和這些排程器型別與使用多執行緒處理，對稱或 SMT 交集的方式。  請務必了解核心和傳統的排程器，並每個將來自客體 Vm 的工作放在基礎的系統處理器上的差異。

### <a name="the-classic-scheduler"></a>傳統的排程器

傳統的排程器是指系統-包括根 VPs 以及 VPs 屬於客體 Vm 排程在虛擬處理器 (VPs) 上的工作的公平共用循環配置資源方法。 傳統的排程器已使用所有的 HYPER-V 版本 （之前 Windows Server 2019，此處所述) 的預設排程器類型。  傳統的排程器的效能特性可充分了解，並示範 ably 支援過度的訂用帳戶的工作負載-也就是合理的邊界主機的 VP:LP 比例的過度訂用帳戶的傳統的排程器 (取決於類型的工作負載虛擬化、 整體的資源使用率等。）。

當執行虛擬化主機上啟用的 SMT，傳統的排程器將排程從每個獨立屬於核心的 SMT 執行緒上的任何 VM 的客體 VPs。 因此，不同的 Vm 可以在相同核心上執行，在相同的時間 (一個在另一個執行緒上執行另一個 VM 時，核心的一個執行緒上執行的 VM)。

### <a name="the-core-scheduler"></a>核心排程器

核心排程器會利用提供的客體工作負載，而這會影響系統效能和安全性隔離的 SMT 的屬性。 核心排程器可確保，從 VM VPs 已排定在同層級 SMT 執行緒上。 這是對稱，因此如果 LPs 中的兩個 VPs 群組都已排程的兩個群組中，且永遠不會共用系統 CPU 核心的 Vm 之間。

排程客體 VPs 基礎 SMT 配對，核心排程器提供強式的安全性界限的工作負載隔離，並也可用來減少受效能變異影響延遲敏感的工作負載。

請注意，當虛擬機器已排定副總裁沒有啟用，SMT 副總裁會取用整個核心時執行，而且核心的同層級 SMT 執行緒將會保持閒置。  這是為了提供正確的工作負載隔離，但會影響整體系統效能，尤其是隨著系統 LPs 過度訂閱-也就是當總 VP:LP 比例超過 1:1。 因此，執行而不需要每個核心的多個執行緒所設定的客體 Vm 是次佳的組態。

### <a name="benefits-of-the-using-the-core-scheduler"></a>使用核心排程器的優點

核心排程器會提供下列優點：

* 之客體工作負載隔離客體 VPs 的強式的安全性範圍限制為基礎的實體核心配對，減少至側邊通道受到窺探攻擊的弱點可能會執行。

* 大幅降低降低工作負載的變化性-客體工作負載輸送量變化性，這會提供更好的工作負載的一致性。

* 在客體 Vm 的 OS 和客體虛擬機器中執行的應用程式中使用 SMT 可以利用 SMT 行為以及程式設計介面 (Api) 來控制，並將工作分散到 SMT 執行緒，就像它們何時會執行非虛擬化。

Azure 的虛擬化主機上為利用強大的安全性界限和低的工作負載 variabilty 目前使用核心排程器。 Microsoft 認為核心排程器類型應該是的而且會持續以預設 hypervisor 排程對於大部分的虛擬化案例的型別。  因此，為了確保我們的客戶是安全的預設值，Microsoft 會進行這項變更的 Windows Server 2019 現在。

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>客體工作負載核心排程器的效能影響

雖然才能有效地降低特定類別的弱點，核心排程器可能會也可能會降低效能。 客戶可能會看到整體的工作負載容量，其虛擬化主機的 Vm 與影響的效能特性的差異。 在核心排程器必須執行非 SMT 的 VP 的情況下，只有其中一個基礎的邏輯核心指令資料流時執行其他必須處於閒置。 這會限制客體工作負載的總主應用程式容量。

這些效能的影響可以降到最低依照本文件中的部署指引。 主機系統管理員必須仔細考慮其特定的虛擬化部署案例，並平衡其容許的最大的工作負載密度、 過度彙總等虛擬化主機的需求的安全性風險。

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>變更預設的 Windows Server 2016 和 Windows Server 2019 的建議的設定

部署使用的最大的安全性狀態的 HYPER-V 主機需要使用 hypervisor 核心排程器型別。 為了確保我們的客戶是安全的預設值，Microsoft 正在變更下列預設和建議的設定。

>[!NOTE]
>而 hypervisor 的排程器類型的內部支援已包含在 Windows Server 2016、 Windows Server 1709,hyper 和 Windows Server 1803 的初始版本中，更新所需為了存取可讓您選取的組態控制項hypervisor 排程器的型別。  請參閱[了解與使用 HYPER-V hypervisor 排程器類型](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)如需有關這些更新。

### <a name="virtualization-host-changes"></a>虛擬化主機變更

* Hypervisor 會使用核心排程器預設 Windows Server 2019 的開頭。

* 設定 Windows Server 2016 上的核心排程器的 Microsoft reccommends。 在 Windows Server 2016 中，支援 hypervisor 核心排程器類型，不過，預設值是傳統的排程器。 核心排程器是選擇性的且必須明確啟用 HYPER-V 主機系統管理員。

### <a name="virtual-machine-configuration-changes"></a>變更虛擬機器組態

* 於 Windows Server 2019，使用預設的 VM 版本 9.0 建立的新虛擬機器會自動繼承的 SMT 屬性 （啟用或停用） 的虛擬化主機。 也就是如果 SMT 上已啟用實體的主應用程式，新建立的 Vm 也會啟用，SMT，而且根據預設，會繼承的 SMT 拓撲的主機與 VM 具有相同數目的每個核心的硬體執行緒作為基礎的系統。 這將會反映在 VM 的設定與 HwThreadCountPerCore = 的 0，0 表示 VM 應該繼承主機的 SMT 設定。

* 現有的虛擬機器的 8.2 或更早版本將 VM 版本保留 HwThreadCountPerCore，其原始 VM 處理器設定和 8.2 VM 版本來賓的預設值是 HwThreadCountPerCore = 1。 當這些客體執行 Windows Server 2019 主機上時，它們會處理，如下所示：

    1. 如果 VM 有副總裁計數小於或等於 LP 核心的計數，VM 會被處理為非 SMT VM 核心排程器。 當客體副總裁在單一的 SMT 執行緒上執行時，就會閒置核心的同層級 SMT 執行緒。 這是未最佳化，而且會導致整體效能損失。

    2. 如果 VM 有多個 VPs 比 LP 核心，VM 會當作 SMT VM 核心排程器所處理。 不過，VM 不會發現它是 SMT VM 的其他指示。 例如，使用 CPUID 指令或 Windows Api 來查詢由作業系統或應用程式的 CPU 拓撲不會指出 SMT，會啟用。

* 當現有的 VM 明確更新從更早版本的 VM 版本透過更新 VM 作業的 9.0 版時，VM 將 HwThreadCountPerCore 保留其目前的值。  VM 不會強制啟用的 SMT。

* Windows Server 2016，Microsoft 會建議客體 Vm 啟用 SMT。  根據預設，Vm 建立在 Windows Server 2016 上會有 SMT 停用，這是 HwThreadCountPerCore 設為 1，除非明確地變更。

>[!NOTE]
>Windows Server 2016 不支援設定 HwThreadCountPerCore 設為 0。

#### <a name="managing-virtual-machine-smt-configuration"></a>管理虛擬機器 SMT 組態

客體虛擬機器 SMT 組態會設定每個 VM 為基礎。 主機系統管理員可以檢查，並設定 VM 的 SMT 組態，以選取 從下列選項：

    1. 設定 Vm 來執行為 SMT 功能，選擇性地自動繼承主機 SMT 拓樸

    2. 設定為非 SMT 執行的 Vm

在 HYPER-V Manager 主控台的 [摘要] 窗格中，會顯示 VM 的 SMT lt;{0}>。  設定 VM 的 SMT 可能使用的 VM 設定或 PowerShell 來完成。

#### <a name="configuring-vm-smt-settings-using-powershell"></a>使用 PowerShell 設定 VM 的 SMT

若要設定客體虛擬機器的 SMT 設定，請使用足夠的權限和型別中開啟 PowerShell 視窗：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

其中：

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

若要閱讀客體虛擬機器的 SMT 設定，請使用足夠的權限和型別開啟 PowerShell 視窗：

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

請注意，客體 Vm 設有 HwThreadCountPerCore = 0 表示 SMT 將能讓來賓，而會公開至客體的 SMT 執行緒數目相同，因為它們是基礎虛擬化主機上，通常是 2。

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>客體 Vm 可能會注意到 CPU 拓樸對跨 VM 行動性的案例

OS 和應用程式在 VM 中的可能會看到變更主機和 VM 設定之前和之後 VM 生命週期事件這類的即時移轉或儲存和還原作業。 中的 VM 狀態儲存和還原作業期間會移轉 VM 的 HwThreadCountPerCore 設定並具現化的值 （也就是計算的結合虛擬機器的設定與來源主機的設定）。 VM 會繼續使用這些設定在目的主機上的執行。 在時間點關閉 VM 之後，重新啟動，可能實現的值所觀察的 vm 將會變更。 這應該是良性，為 OS 和應用程式層級軟體看起來應該如 CPU 拓樸資訊做為其正常的啟動及初始化程式碼流程的一部分。 不過，因為即時移轉或儲存/還原作業期間，進行這些狀態轉換的虛擬機器的序列會略過這些開機階段初始化會觀察到原先計算實現值，直到關閉並重新啟動。  

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>關於 VM 的非最佳化設定的警示

以多個 VPs 比實體核心的主應用程式結果中的非最佳化的組態設定的虛擬機器。 Hypervisor 排程器會將這些 Vm，如同它們是 SMT 感知。 不過，作業系統和應用程式在 VM 中的軟體將會看到顯示已停用 SMT CPU 拓樸。 HYPER-V 的背景工作處理序偵測到這種情況時，會記錄警告的主機系統管理員，VM 的設定是未最佳化，而且建議 SMT 是虛擬化主機上的事件已針對 VM 啟用。

#### <a name="how-to-identify-non-optimally-configured-vms"></a>如何找出非以最佳方式設定 Vm

您可以識別非 SMT Vm 藉由檢查系統記錄檔事件檢視器中針對 HYPER-V 背景工作處理序事件識別碼 3498 會觸發 vm 中，每當 VPs 中 VM 數目大於實體核心計數。 從事件檢視器，或透過 PowerShell，您可以取得背景工作處理序事件。

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>查詢 HYPER-V 背景工作處理序 VM 事件使用 PowerShell

查詢 HYPER-V 背景工作處理序事件識別碼 3498 使用 PowerShell，從 PowerShell 提示字元輸入下列命令。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>使用客體作業系統的 hypervisor enlightenment 客體 SMT lt;{0}> 的影響

Microsoft hypervisor 提供多個 enlightenment 或提示，客體 VM 中執行的作業系統可能會查詢並使用觸發程序最佳化，例如可能有益於效能，或執行時，否則為改善的各種狀況的處理虛擬化。 虛擬處理器排程的處理和使用 OS 緩和措施，利用 SMT 的側邊通道攻擊有關，一個近期推出的啟蒙。

>[!NOTE]
>Microsoft 建議主機系統管理員啟用 SMT 的最佳化工作負載效能的客體 Vm。

此客體啟蒙的詳細資料會提供以下，不過重點的虛擬化主機系統管理員就是虛擬機器應有 HwThreadCountPerCore 設定為符合主機的實體的 SMT 組態。 這可讓報告沒有任何非架構的核心 hypervisor 共用。 因此，您可能會啟用任何客體 OS 支援最佳化需要啟蒙的。 在 Windows Server 2019，建立新的 Vm，保留預設值 HwThreadCountPerCore (0)。 從 Windows Server 2016 的較舊 Vm 移轉至 Windows Server 2019 組態版本，可以更新主機。 這樣做之後，Microsoft 建議您設定 HwThreadCountPerCore = 0。  Windows Server 2016，Microsoft 會建議設定 HwThreadCountPerCore 來比對主應用程式組態 (通常是 2)。

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>NoNonArchitecturalCoreSharing 啟蒙詳細資料

從 Windows Server 2016 開始，hypervisor 會定義新的啟蒙教學，來描述其處理副總裁排程和客體作業系統的位置。 中所定義的這個啟蒙[Hypervisor Top 層級的功能性規格 v5.0c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs)。

綜合 CPUID 分葉 Hypervisor CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] 指出虛擬處理器，將永遠不會共用實體核心使用另一個虛擬處理器的詳細資訊，除了會回報為同層級 SMT 的虛擬處理器執行緒。 例如，客體副總裁將永遠不會執行緒上執行 SMT 與根副總裁同層級上相同的處理器核心的 SMT 執行緒上同時執行。 這種情況時，才可執行虛擬化時，並因此代表一種非架構的 SMT 行為，也有嚴重的安全性含意。 客體作業系統可以使用 NoNonArchitecturalCoreSharing = 1，以指出它是安全地啟用最佳化，可能有助於避免設定 STIBP 的效能負荷。

在特定組態中，hypervisor 並不會指出該 NoNonArchitecturalCoreSharing = 1。 例如，如果主機已啟用的 SMT，而且已設定為使用 hypervisor 傳統排程器，則 NoNonArchitecturalCoreSharing 會是 0。 這可能會導致無法啟用某些最佳化功能已啟用的來賓。 因此，Microsoft 建議使用 SMT 的主機系統管理員依賴 hypervisor 核心排程器，並確保虛擬機器會設定為其 SMT 組態繼承主應用程式，以確保最佳的工作負載效能。

## <a name="summary"></a>總結

安全性威脅型態持續發展。 為了確保我們的客戶是安全的預設值，Microsoft 會變更預設組態的 hypervisor 和虛擬機器從 Windows Server 2019 HYPER-V 中，並提供更新的指引和建議執行 Windows 的客戶Server 2016 Hyper-v。 虛擬化主機系統管理員應該：

* 閱讀並了解本文件中提供的指導方針

* 仔細評估及調整其虛擬化部署，以確保其符合安全性、 效能、 虛擬化密度和其獨特需求的工作負載的回應性目標

* 請考慮重新設定現有的 Windows Server 2016 HYPER-V 主機都運用 hypervisor 核心排程器所提供的強大的安全性優點

* 更新現有非 SMT Vm 若要從排程解決硬體安全性弱點的副總裁隔離所加諸的條件約束降低效能影響
