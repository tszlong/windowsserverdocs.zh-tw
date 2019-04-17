---
title: 關於 HYPER-V hypervisor 排程器類型選取項目
description: 模式上的 HYPER-V 的排程器使用提供的 HYPER-V 主機系統管理員資訊
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: c5360d8e2fdc23f8b05c6be0f665407eebedeba2
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905125"
---
# 關於 HYPER-V hypervisor 排程器類型選取項目

適用於：

* Windows Server 2016
* Windows Server 版本 1709
* Windows Server 版本 1803
* Windows Server 2019

本文件說明 HYPER-V 的預設值的重要變更，並建議使用 hypervisor 的排程器類型。 這些變更會影響這兩個系統的安全性和虛擬化的效能。 虛擬化主機系統管理員應該檢閱和了解變更與本文件中所述的影響，以及審慎評估影響、 建議的部署指導方針和最佳了解如何部署及管理的風險因素快速變更安全性概況面對的 HYPER-V 主機。

>[!IMPORTANT]
>目前已知旁路安全性弱點，發現在多個處理器架構的套件中可能遭到惡意客體 VM 透過舊版 hypervisor 傳統排程器類型在具有 Simultaneous 主機上執行時的排程的行為啟用多執行緒 （smt） 即可。  如果成功利用，惡意的工作負載無法觀察其磁碟分割界限之外的資料。 藉由設定 HYPER-V hypervisor 來運用 hypervisor 核心排程器類型和重新設定客體虛擬機器可以減少這類攻擊。 與核心排程器 hypervisor 會限制在相同的實體處理器核心，因此強烈隔離 VM 的能力來存取資料，以在其執行所在之實體核心的界限上執行的客體 VM 的 Vp。  這是非常有效防護功能來抵禦這些旁路的攻擊，是否觀察任何其他磁碟分割，從成品會防止 VM 的根目錄或另一個客體磁碟分割。  因此，Microsoft 會變更預設值，並建議的組態設定適用於虛擬化主機與客體 Vm。

## 背景

從 Windows Server 2016 開始，HYPER-V 支援排程及管理虛擬處理器，稱為 hypervisor 排程器類型的幾個的方法。  [了解和使用 HYPER-V hypervisor 排程器類型](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)中可以找到所有 hypervisor 排程器類型的詳細的說明。

>[!NOTE]
>新的 hypervisor 排程器類型與 Windows Server 2016 中，引進第一次，而不是在先前版本中提供。 在 Windows Server 2016 之前的 HYPER-V 所有版本都支援傳統排程器。 支援核心排程器只最近已發佈。

## 關於 hypervisor 排程器類型

本文重點為特別相較於舊版的 「 傳統 」 排程器，新的 hypervisor 核心排程器類型使用，而且這些排程器類型與使用多執行緒，Symmetric 或 SMT 交集的方式。  請務必了解核心和傳統的排程器和每個如何基礎系統處理器上放置從客體 Vm 的工作之間的差異。

### 傳統的排程器 \]

傳統的排程器 \] 指的是以公平共用循環配置資源方法的排程整個系統-包括根 Vp，以及 Vp 屬於客體 Vm 虛擬處理器 (Vp) 上的工作。 傳統的排程器已經預設排程器類型 （直到 Windows Server 2019，此處所述) 適用於所有版本的 HYPER-V。  傳統的排程器的效能特性理解，和傳統排程器也會展示 ably 支援的工作負載-也就是過度的訂閱，在主機 VP:LP 比例合理的邊界過度訂閱 (取決於類型的虛擬工作負載、 整體資源使用率等等。）。

當虛擬化主機上執行時使用 SMT 啟用，傳統的排程器 \] 將會排程客體 Vp 從獨立屬於核心的每個 SMT 執行緒上的任何 VM。 因此，不同的虛擬機器可以相同的核心上執行，在此同時 (一個單一執行緒的核心上執行，而另一個 VM 另一個執行緒上執行的 VM)。

### 核心排程器

核心排程器會運用 SMT 提供隔離的客體工作負載，這會影響系統效能和安全性的屬性。 核心排程器可確保從 VM Vp 排程在同層級 SMT 執行緒上。 這是為了對稱讓如果 Lp 中的兩個，Vp 群組的排程的兩個群組中，以及系統 CPU 核心永遠不會共用虛擬機器之間。

依排程上基礎 SMT 組客體 Vp，核心排程器提供的工作負載隔離的強式安全性界限，並且也可以用來減少延遲敏感的工作負載的效能變化。

請注意，當副總裁排定的虛擬機器不需要 SMT 啟用，它執行，且核心的同層級 SMT 執行緒將會保留閒置時副總裁會消耗整個核心。  這是為了提供正確的工作負載的隔離性，但對整體系統效能的影響，特別是系統 Lp 成為過度訂閱-也就是當總 VP:LP 比例超過 1:1。 因此，執行客體虛擬機器設定而每個核心的多個執行緒不是一個未最佳化的設定。

### 使用核心排程器的優點

核心排程器提供下列優點：

* 客體工作負載隔離客體 Vp 的強式安全性界限會被限制為基礎的實體核心組，減少弱點旁路窺探功能的攻擊上執行。

* 減少工作負載變化-客體工作負載輸送量變化會大幅降低，提供更大的工作負載一致性。

* 在客體虛擬機器內的 OS 和客體虛擬機器中執行的應用程式中使用 SMT 可以利用 SMT 行為，與程式設計介面 (Api) 來控制，並將工作分散在 SMT 執行緒，就像它們時就執行非虛擬化。

核心排程器目前使用 Azure 的虛擬化主機上特別以充分利用的強式安全性界限和低的工作負載 variabilty。 Microsoft 相信應該核心排程器類型，而且將會繼續被預設 hypervisor 排程對大多數的虛擬化案例的類型。  因此，若要確保我們的客戶是安全的預設值，Microsoft 進行這項變更的 Windows Server 2019 現在。

### 在客體工作負載上核心排程器效能的影響

雖然必要有效地降低某些類別的弱點，核心排程器可能也可能會降低效能。 客戶可能會看到其虛擬化主機的整體的工作負載容量的差異在於使用他們的 Vm 和影響的效能特性。 在 core 排程器必須在其中執行非 SMT 的 VP 的情況下，只是其中一個指令串流基礎的邏輯核心中執行其他，必須保留閒置時。 這將會限制客體工作負載的主機總容量。

這些效能影響可以依照本文件中的部署指導方針最小化。 主機系統管理員必須謹慎考慮其特定的虛擬化的部署案例和平衡安全性風險的最大的工作負載密度、 虛擬化主機等等的頂彙總需要對其容錯度。

## 預設及 Windows Server 2016 和 Windows Server 2019 的建議的設定的變更

部署具有最高的安全性狀態的 HYPER-V 主機需要 hypervisor 核心排程器類型使用。 若要確保我們的客戶是安全的預設值，Microsoft 會變更下列預設和建議的設定。

>[!NOTE]
>雖然 hypervisor 的內部類型支援，排程器已包含在初始版本的 Windows Server 2016、 Windows Server 1709，以及 Windows Server 1803 中，更新所需才能存取組態控制項可讓您選取hypervisor 排程器類型。  請參閱[了解與使用 HYPER-V hypervisor 排程器類型](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types)的這些更新的詳細資訊。

### 虛擬化主機變更

* Hypervisor 會使用核心排程器以 Windows Server 2019 的預設開頭。

* 在 Windows Server 2016 上設定核心排程器 Microsoft reccommends。 在 Windows Server 2016 中，支援 hypervisor 核心排程器類型，但預設值是傳統的排程器 \]。 核心排程器為選擇性，必須明確啟用 HYPER-V 主機系統管理員。

### 虛擬機器設定變更

* 在 Windows Server 2019，使用預設的 VM 版本 iron︰9.0 建立新的虛擬機器將會自動繼承 SMT 屬性 （啟用或停用） 的虛擬化主機。 也就是，如果 SMT 上已啟用實體主機，新建立的 Vm 也會有 SMT 啟用，並根據預設，會繼承的主機的 SMT 拓撲與 VM 具有相同數量的每個核心硬體執行緒做為基礎的系統。 這將會反映在 VM 的設定與 HwThreadCountPerCore = 的 0，其中 0 表示 VM 應該繼承的主機 SMT 設定。

* 現有的虛擬機器都是 VM 版本的 8.2 或更早版本將會保留 HwThreadCountPerCore，其原始 VM 處理器設定和 8.2 VM 版本來賓的預設值是 HwThreadCountPerCore = 1。 當這些客體執行 Windows Server 2019 主機上時，它們會被視為是，如下所示：

    1. 如果 VM 是小於或等於 LP 核心的計數副總裁計數，VM 將會被視為是做為非 SMT VM 的核心排程器 \]。 當客體副總裁單一 SMT 執行緒上執行時，將會 idled 核心的同層級 SMT 執行緒。 這是未最佳化，而且將會導致效能的整體遺失。

    2. 如果 VM 有多個 Vp 比 LP 核心，VM 將會被視為 SMT VM 核心排程器。 不過，VM 將會不需觀察它是 SMT VM 其他指示。 例如，使用 CPUID 指令或 Windows Api 的查詢 CPU 拓撲由作業系統或應用程式不會指出 SMT 已啟用。

* 從 eariler VM 版本明確地更新現有的 VM 版本 iron︰9.0 透過更新 VM 作業時, VM 將會針對 HwThreadCountPerCore 保留其目前的值。  VM 不會有 SMT 強制啟用。

* 在 Windows Server 2016 中，Microsoft 建議啟用 SMT 適用於客體 Vm。  根據預設，會有 SMT 停用，Windows Server 2016 上建立虛擬機器，是 HwThreadCountPerCore 設為 1，除非明確變更。

>[!NOTE]
>Windows Server 2016 不支援設定 HwThreadCountPerCore 為 0。

#### 管理虛擬機器 SMT 設定

客體虛擬機器 SMT 設定會設定每個 VM 為基礎。 主機系統管理員可以檢查，並設定 VM 的 SMT 設定，以從下列選項選取：

    1. 設定 Vm 執行為 SMT 已啟用，也可以自動繼承主機 SMT 拓撲

    2. 設定為以非 SMT 執行的 Vm

Vm SMT 組態會顯示在 HYPER-V 管理員主控台中的摘要窗格。  設定 VM 的 SMT 設定可能會使用 VM 設定 \] 或 PowerShell 來完成。

#### 使用 PowerShell 設定 VM SMT

若要設定客體虛擬機器的 SMT 設定，請使用充足的權限，然後輸入開啟 PowerShell 視窗：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

其中：

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

若要讀取客體虛擬機器的 SMT 設定，請使用充足的權限，然後輸入開啟 PowerShell 視窗：

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

請注意該客體虛擬機器使用 HwThreadCountPerCore 設定 = 0 表示 SMT 客體，就會啟用，將會公開相同數量的來賓 SMT 執行緒，因為它們是基礎虛擬化主機上，通常 2。

### 客體虛擬機器可能會跨 VM mobility 案例觀察 CPU 拓撲的變更

OS 及 VM 中的應用程式可能會看到變更到主機和 VM 設定之前和之後 VM 週期事件如即時移轉或儲存和還原作業。 作業的 VM 中儲存和還原狀態期間會移轉 VM 的 HwThreadCountPerCore 設定和具現化的值 （也就是 VM 的設定和組態來源的主機的計算組合）。 VM 將會繼續執行利用這些設定目的地主機上。 某個點，還是 VM 關機和重新啟動，有可能觀察到的具現化的值由 VM 將會變更。 這應該是良性、 做為作業系統及應用程式層級軟體應該看起來如 CPU 拓撲資訊做為其一般的新創公司與初始化程式碼流程的一部分。 不過，因為這些開機階段初始化順序會略過即時移轉或儲存/還原作業期間，經歷這些狀態轉換的 Vm 可能觀察原先計算值具現化，直到其關閉並重新啟動。  

### 非最佳的 VM 組態相關的警示

虛擬機器使用更多 Vp 比實體核心上的主機結果中有非最佳的組態設定。 如果因為它們是 SMT 感知 hypervisor 排程器會處理這些 Vm。 不過，OS 及 VM 中的應用程式軟體將會出現顯示已停用 SMT CPU 拓撲。 偵測到這個條件時，HYPER-V 工作者處理序會記錄事件，VM 的設定會非最佳建議 SMT 會警告主機系統管理員的虛擬化主機上啟用之 vm。

#### 如何找出非最佳方式設定 Vm

您可以找出非-SMT Vm 藉由檢查系統記錄檔，事件檢視器中的 HYPER-V 工作者處理序事件識別碼 3498 哪一個 VM 中 Vp 數目時大於實體核心計數，就會觸發的 VM。 從 [事件檢視器，或透過 PowerShell，可以取得工作者處理序事件。

#### 查詢使用 PowerShell 的 HYPER-V 工作者處理序 VM 事件

查詢適用於 HYPER-V 背景工作處理程序事件識別碼 3498 使用 PowerShell，從 PowerShell 提示字元中輸入下列命令。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### 客體 SMT 組態上的 hypervisor enlightenment 客體作業系統使用的影響

Microsoft 的 hypervisor 提供多個 enlightenment 或在客體 VM 中執行的作業系統可能會查詢，並使用觸發最佳化，例如，可能改善效能，或執行時，否則改善處理各種不同的條件的提示虛擬化。 一個最近導入的啟蒙涉及的虛擬處理器排程處理和 SMT 旁路攻擊的 OS 防護功能的用途。

>[!NOTE]
>Microsoft 建議主機系統管理員啟用 SMT 客體 vm 來最佳化工作負載的效能。

這個客體啟蒙的詳細資料會提供下方，不過主要來說最適用於虛擬化主機系統管理員的是虛擬機器應該會有 HwThreadCountPerCore 以符合主機的實體 SMT 組態設定。 這可讓報告沒有非架構核心 hypervisor 共用。 因此，任何客體 OS 支援，需要經過啟蒙，可能會啟用最佳化。 在 Windows Server 2019，建立新的 Vm，保留預設值的 HwThreadCountPerCore (0)。 從 Windows Server 2016 的較舊的虛擬機器移轉的主機可以更新至 Windows Server 2019 的設定版本。 這麼做之後，Microsoft 建議設定 HwThreadCountPerCore = 0。  在 Windows Server 2016 中，Microsoft 建議設定 HwThreadCountPerCore 以符合主機設定 (通常是 2)。

### NoNonArchitecturalCoreSharing 啟蒙詳細資料

Hypervisor 會從 Windows Server 2016 中，定義來描述其副總裁排程及放置到客體作業系統的處理新啟蒙。 [Hypervisor Top 層級 Functional Specification v5.0c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs)中定義這個啟蒙。

Hypervisor 綜合 CPUID 分葉 CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] 表示虛擬處理器，將永遠不會共用實體核心具有另一部虛擬處理器，除了回報為同層級 SMT 的虛擬處理器執行緒。 例如，客體副總裁將永遠不會執行一起根副總裁 SMT 執行緒上同層級 SMT 執行緒上相同的處理器核心上同時執行。 這種情況是僅能執行虛擬化時，因此代表也有嚴重的安全性影響的非架構 SMT 行為。 客體作業系統可以使用 NoNonArchitecturalCoreSharing 做為安全啟用最佳化，可協助避免不必要的設定 STIBP 效能它表示 = 1。

在某些設定，hypervisor 不會指出該 NoNonArchitecturalCoreSharing = 1。 例如，如果主機有 SMT 啟用，而且會設定為使用 hypervisor 傳統排程器 NoNonArchitecturalCoreSharing 會是 0。 這可能會防止啟用某些最佳化啟發式的客體。 因此，Microsoft 建議主機系統管理員使用 SMT 依賴 hypervisor 核心排程器 \]，並確保虛擬機器的主機，以確保最佳的工作負載的效能會繼承其 SMT 設定來設定。

## 摘要

安全性威脅形勢持續進化。 若要確保我們的客戶是安全的預設值，Microsoft 已變更的預設設定為 hypervisor 和虛擬機器開始在 Windows Server 2019 HYPER-V，以及指導方針和建議執行 Windows 的客戶提供更新Server 2016 Hyper-v。 虛擬化主機系統管理員應該：

* 閱讀並了解這份文件中所提供的指導方針

* 仔細評估及調整其虛擬化部署，以確定它們符合安全性、 效能、 虛擬化密度和工作負載的回應性目標其獨特的需求

* 請考慮重新設定運用 hypervisor 核心排程器 \] 所提供的強式安全性優勢現有的 Windows Server 2016 HYPER-V 主機

* 更新現有非-SMT Vm 以從排程副總裁隔離，解決硬體安全性弱點受限制減少的效能影響
