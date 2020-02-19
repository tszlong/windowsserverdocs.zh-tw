---
title: 瞭解和使用 Hyper-v 虛擬程式排程器類型
description: 提供 Hyper-v 主機系統管理員使用 Hyper-v 排程器模式的相關資訊
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: 8ba413b831c7b11780113ee2ffd3cce598781a44
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465572"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>管理 Hyper-v 虛擬程式排程器類型

>適用于： Windows 10、Windows Server 2016、Windows Server、版本1709、Windows Server、版本1803、Windows Server 2019

本文說明 Windows Server 2016 中首次引進的虛擬處理器排程邏輯新模式。 這些模式或排程器類型會決定 Hyper-v 虛擬機器如何配置及管理跨來賓虛擬處理器的工作。 Hyper-v 主機系統管理員可以選取最適用于來賓虛擬機器（Vm）的虛擬程式排程器類型，並設定 Vm 以利用排程邏輯。

>[!NOTE]
>需要進行更新，才能使用本檔中所述的程式管理器排程器功能。 如需詳細資訊，請參閱[必要的更新](#required-updates)。

## <a name="background"></a>背景

在討論 Hyper-v 虛擬處理器排程背後的邏輯和控制項之前，請先參閱這篇文章中涵蓋的基本概念，將有所説明。

### <a name="understanding-smt"></a>瞭解 SMT

同時執行多執行緒或 SMT，是在新式處理器設計中使用的技術，可讓處理器的資源由個別獨立的執行緒共用。 SMT 通常會在可能的情況下平行處理計算，藉此為大部分的工作負載提供適度的效能提升，而增加指令輸送量，但效能可能會線上程之間競爭發生共用的處理器資源。
Intel 和 AMD 皆提供支援 SMT 的處理器。 Intel 是以 Intel 超執行緒技術或 Intel HT 的形式，將其 SMT 供應專案。

基於本文的目的，Hyper-v 的 SMT 和其使用方式的描述同樣適用于 Intel 和 AMD 系統。

* 如需 Intel HT 技術的詳細資訊，請參閱[Intel 超執行緒技術](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* 如需有關 AMD SMT 的詳細資訊，請參閱「 [Zen」核心架構](https://www.amd.com/en/technologies/zen-core)

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>瞭解 Hyper-v 如何虛擬化處理器

在考慮「基礎程式排程器」類型之前，瞭解 Hyper-v 架構也很有説明。 您可以在[Hyper-v 技術總覽](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview)中找到一般摘要。 以下是這篇文章的重要概念：

* Hyper-v 會建立及管理虛擬機器磁碟分割，在這些資料分割中，會在管理者控制下配置和共用計算資源。 分割區可在所有來賓虛擬機器之間，以及在來賓 Vm 與根磁碟分割之間提供強式隔離界限。

* 根磁碟分割本身是虛擬機器磁碟分割，雖然它具有與來賓虛擬機器不同的唯一屬性和更高的許可權。 根磁碟分割提供管理服務，可控制所有來賓虛擬機器、提供來賓的虛擬裝置支援，以及管理來賓虛擬機器的所有裝置 i/o。 Microsoft 強烈建議您不要在根磁碟分割中執行任何應用程式工作負載。

* 根磁碟分割的每個虛擬處理器（VP）都會將1:1 對應到基礎邏輯處理器（LP）。 主機副總一律會在相同的基礎 LP 上執行–不會遷移根磁碟分割的 VPs。

* 根據預設，主機 VPs 執行所在的 LPs 也可以執行來賓 VPs。

* 在任何可用的邏輯處理器上執行的虛擬程式都可能會排程來賓副總裁。 當「執行程式管理者」在排程來賓副總時，會負責考慮時態性快取位置、NUMA 拓朴和許多其他因素，最後就是在任何主機 LP 上排定副總裁。

## <a name="hypervisor-scheduler-types"></a>程式管理器排程器類型

從 Windows Server 2016 開始，Hyper-v 虛擬機器支援數種排程器邏輯模式，可決定程式管理者如何排定基礎邏輯處理器上的虛擬處理器。 這些排程器類型如下：

- [傳統的公平共用排程器](#the-classic-scheduler)
- [核心排程器](#the-core-scheduler)
- [根排程器](#the-root-scheduler)

### <a name="the-classic-scheduler"></a>傳統排程器

傳統排程器是自其開始後所有版本的 Windows Hyper-v 虛擬機器的預設值，包括 Windows Server 2016 Hyper-v。 傳統排程器會為來賓虛擬處理器提供公平共用、搶先式迴圈配置資源排程模型。

傳統的排程器類型最適合大部分傳統的 Hyper-v 用途–適用于私人雲端、主機服務提供者等等。 效能特性已充分瞭解，且最適合用來支援各種不同的虛擬化案例，例如過度訂閱 VPs 至 LPs、同時執行許多異類 Vm 和工作負載，以及執行較大的規模高效能 Vm，支援不受限制的 Hyper-v 完整功能集等等。

### <a name="the-core-scheduler"></a>核心排程器

「虛擬程式核心排程器」是傳統排程器邏輯的新替代方案，在 Windows Server 2016 和 Windows 10 1607 版中引進。 核心排程器為來賓工作負載隔離提供強大的安全性界限，並降低在 SMT 啟用的虛擬化主機上執行之 Vm 內工作負載的效能變化。 核心排程器允許同時在相同的已啟用 SMT 虛擬化主機上同時執行 SMT 和非 SMT 虛擬機器。

核心排程器會利用虛擬化主機的 SMT 拓撲，並選擇性地將 SMT 配對公開給來賓虛擬機器，並將來自相同虛擬機器的來賓虛擬處理器群組排程到 SMT 邏輯處理器群組。 這是以對稱方式完成，因此，如果 LPs 位於兩個群組中，VPs 會以兩個群組排程，而在 Vm 之間不會共用核心。
若已為虛擬機器排程 VP，而未啟用 SMT，則 VP 會在執行時使用整個核心。

核心排程器的整體結果如下：

* 來賓 VPs 受限於在基礎實體核心配對上執行，將 VM 隔離到處理器核心界限，進而減少對來自惡意 Vm 的側通路窺探攻擊的弱點。

* 輸送量的變化大幅降低。

* 效能可能會降低，因為如果只有一個 VPs 群組可以執行，則核心中只有一個指令資料流程會執行，另一個則是閒置。

* 在來賓虛擬機器中執行的作業系統和應用程式，可以利用 SMT 行為和程式設計介面（Api）來控制和散佈跨 SMT 執行緒的工作，就像執行非虛擬化一樣。

* 來賓工作負載隔離的強式安全性界限-來賓 VPs 受限於在基礎實體核心配對上執行，以降低對側通道窺探攻擊的弱點。

從 Windows Server 2019 開始，預設會使用核心排程器。 在 Windows Server 2016 上，核心排程器是選擇性的，而且必須由 Hyper-v 主機管理員明確啟用，而且傳統排程器是預設值。

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>已停用主機 SMT 的核心排程器行為

如果虛擬機器已設定為使用核心排程器類型，但 SMT 功能已停用或不存在於虛擬化主機上，則不論是否使用「虛擬程式排程器」類型設定，此管理器將會使用傳統排程器行為。

### <a name="the-root-scheduler"></a>根排程器

根排程器是在 Windows 10 1803 版中引進。 當根排程器類型為 [已啟用] 時，[處理常式 cedes] 會對根磁碟分割進行工作排程的控制。 根磁碟分割的 OS 實例中的 NT 排程器會管理將工作排程到系統 LPs 的所有層面。

根排程器會解決支援公用程式磁碟分割所固有的獨特需求，以提供與 Windows Defender 應用程式防護（WDAG）搭配使用的強式工作負載隔離。 在此案例中，將排程責任留給根 OS 會提供幾個優點。 例如，適用于容器案例的 CPU 資源控制可以搭配公用程式分割使用，以簡化管理和部署。 此外，根 OS 排程器可以隨時收集容器內工作負載 CPU 使用率的相關計量，並使用此資料做為與系統中所有其他工作負載適用的相同排程原則的輸入。 這些相同的計量也有助於清楚地將應用程式容器中所執行的屬性，設定為主機系統。 使用傳統虛擬機器工作負載來追蹤這些計量會比較棘手，因為所有執行中 VM 的工作都是在根磁碟分割中進行。

#### <a name="root-scheduler-use-on-client-systems"></a>根排程器在用戶端系統上使用

從 Windows 10 1803 版開始，預設會在用戶端系統上使用根排程器，而在此情況下，可以支援以虛擬化為基礎的安全性和 WDAG 工作負載隔離，以及未來系統的適當操作異類核心架構。 這是用戶端系統唯一支援的程式管理器排程器設定。 管理員不應嘗試覆寫 Windows 10 用戶端系統上的預設「虛擬程式管理者」類型。

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>虛擬機器 CPU 資源控制和根排程器

當虛擬機器根排程器因為根作業系統的排程器邏輯以全域方式管理主機資源，且沒有 VM 的知識時，不支援 Hyper-v 所提供的虛擬機器處理器資源控制特定的設定。 Hyper-v 每個 VM 的處理器資源控制，例如 cap、權數和保留，僅適用于管理者直接控制 VP 排程的情況，例如使用傳統和核心排程器類型。

#### <a name="root-scheduler-use-on-server-systems"></a>根排程器在伺服器系統上使用

目前不建議將根排程器與伺服器上的 Hyper-v 搭配使用，因為它的效能特性尚未具備完整的特性和微調，以容納許多伺服器虛擬化部署的一般工作負載。

## <a name="enabling-smt-in-guest-virtual-machines"></a>在來賓虛擬機器中啟用 SMT

當虛擬化主機的虛擬機器設定為使用核心排程器類型時，可以視需要將來賓虛擬機器設定為利用 SMT。 公開 VPs 超執行緒至來賓虛擬機器的事實，可讓客體作業系統中的排程器和 VM 中執行的工作負載，在自己的工作排程中偵測並使用 SMT 拓撲。 在 Windows Server 2016 上，預設不會設定來賓 SMT，而且必須由 Hyper-v 主機管理員明確啟用。 從 Windows Server 2019 開始，在主機上建立的新 Vm 預設會繼承主機的 SMT 拓撲。  也就是，在每個核心有2個 SMT 執行緒的主機上建立的9.0 版 VM，也會看到每個核心2個 SMT 執行緒。

PowerShell 必須用來在來賓虛擬機器中啟用 SMT;Hyper-v 管理員中沒有提供使用者介面。
若要在來賓虛擬機器中啟用 SMT，請以足夠的許可權開啟 PowerShell 視窗，然後輸入：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

其中 <n> 是來賓 VM 將會看到的每個核心 SMT 執行緒數目。  
請注意，<n> = 0 會將 HwThreadCountPerCore 值設定為符合主機的每個核心值的 SMT 執行緒計數。

>[!NOTE] 
>從 Windows Server 2019 開始支援設定 HwThreadCountPerCore = 0。

以下是從在虛擬機器中執行且已啟用2個虛擬處理器和 SMT 的客體作業系統所取得的系統資訊範例。 客體作業系統偵測到屬於相同核心的2個邏輯處理器。

![顯示已啟用 SMT 之來賓 VM 中的 msinfo32 的螢幕擷取畫面](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>在 Windows Server 2016 Hyper-v 上設定虛擬程式排程器類型

Windows Server 2016 Hyper-v 預設會使用傳統的虛擬程式管理器排程器模型。 您可以選擇性地將管理程式設定為使用核心排程器，藉由限制來賓 VPs 在對應的實體 SMT 組上執行，以及支援使用虛擬機器與其來賓 VPs 的 SMT 排程，來提高安全性。

>[!NOTE]
>Microsoft 建議所有執行 Windows Server 2016 Hyper-v 的客戶選取核心排程器，以確保其虛擬化主機能以最佳的方式受到保護，以防止潛在的惡意來賓 Vm。

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>Windows Server 2019 Hyper-v 預設為使用核心排程器

為了協助確保 Hyper-v 主機已部署在最佳的安全性設定中，Windows Server 2019 Hyper-v 現在預設會使用核心程式管理者排程器模型。 主機管理員可以選擇性地將主機設定為使用舊版傳統排程器。 在覆寫排程器類型的預設設定之前，系統管理員應該仔細閱讀、瞭解和考慮每個排程器類型對虛擬化主機的安全性和效能的影響。  如需詳細資訊，請參閱[瞭解 hyper-v 排程器類型選取專案](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection)。

### <a name="required-updates"></a>必要的更新

>[!NOTE]
>若要使用本檔中所述的程式管理器排程器功能，必須進行下列更新。 這些更新包含變更，以支援主機設定所需的新 ' hypervisorschedulertype ' BCD 選項。

| 版本 | 發行  | 需要更新 | 知識庫文章 |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | 無 | 無 |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>選取 Windows Server 上的虛擬程式排程器類型

虛擬程式管理器排程器設定是透過 hypervisorschedulertype BCD 專案來控制。

若要選取排程器類型，請使用系統管理員許可權開啟命令提示字元：

``` command
     bcdedit /set hypervisorschedulertype type
```

其中 `type` 是下列其中一個：

* 傳統
* Core
* 根目錄

系統必須重新開機，程式管理器排程器類型的任何變更才會生效。

>[!NOTE]
>Windows Server Hyper-v 目前不支援虛擬程式根排程器。 Hyper-v 系統管理員不應嘗試設定根排程器來與伺服器虛擬化案例搭配使用。

## <a name="determining-the-current-scheduler-type"></a>判斷目前的排程器類型

您可以藉由檢查最新的程式管理器啟動事件識別碼2的事件檢視器中的系統記錄，來判斷目前使用中的管理器排程器類型，這會報告在啟動程式管理程式時所設定的管理器排程器類型。 您可以從 Windows 事件檢視器或透過 PowerShell 來取得「虛擬程式啟動」事件。

「虛擬程式啟動事件識別碼2」代表「管理器排程器」類型，其中：

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![顯示管理程式啟動事件識別碼2詳細資料的螢幕擷取畫面](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![顯示事件檢視器顯示虛擬程式啟動事件識別碼2的螢幕擷取畫面](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>使用 PowerShell 查詢 Hyper-v 虛擬機器排程器類型的啟動事件

若要使用 PowerShell 查詢虛擬程式事件識別碼2，請從 PowerShell 提示字元輸入下列命令。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![顯示虛擬機器啟動事件識別碼2之 PowerShell 查詢和結果的螢幕擷取畫面](media/Hyper-V-CoreScheduler-PowerShell.png)
