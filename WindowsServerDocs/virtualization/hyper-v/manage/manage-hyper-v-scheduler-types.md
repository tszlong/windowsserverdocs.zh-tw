---
title: 瞭解和使用 Hyper-v 虛擬程式排程器類型
description: 提供 Hyper-v 主機系統管理員使用 Hyper-v 排程器模式的相關資訊
ms.author: benarm
author: BenjaminArmstrong
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: 8dc2fec785771db4ccefb08e2359506932e11620
ms.sourcegitcommit: 8c0a419ae5483159548eb0bc159f4b774d4c3d85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235865"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>管理 Hyper-v 虛擬程式排程器類型

>適用于： Windows 10、Windows Server 2016、Windows Server、1709版、Windows Server、1803版、Windows Server 2019

本文說明 Windows Server 2016 中首次引進的虛擬處理器排程邏輯新模式。 這些模式或排程器類型可決定 Hyper-v 管理程式如何跨來賓虛擬處理器配置和管理工作。 Hyper-v 主機系統管理員可以選取最適合虛擬機器 (Vm) 的虛擬程式管理器類型，並設定 Vm 以利用排程邏輯。

> [!NOTE]
> 需要更新才能使用本檔中所述的程式管理器排程器功能。 如需詳細資訊，請參閱 [必要的更新](#required-updates)。

## <a name="background"></a>背景

在討論 Hyper-v 虛擬處理器排程背後的邏輯和控制項之前，請先複習本文所涵蓋的基本概念。

### <a name="understanding-smt"></a>瞭解 SMT

並行多執行緒（或 SMT）是一種在新式處理器設計中使用的技術，可讓處理器的資源由獨立的獨立執行執行緒共用。 SMT 通常會在可能的情況下平行處理計算，以大幅提升大部分的工作負載，但提高指令輸送量，不過當共用處理器資源的執行緒之間發生競爭情況時，可能會導致效能稍微降低。
Intel 和 AMD 都提供支援 SMT 的處理器。 Intel 將其 SMT 供應專案稱為 Intel 超執行緒技術或 Intel HT。

基於本文的目的，SMT 的說明和 Hyper-v 的使用方式同樣適用于 Intel 和 AMD 系統。

* 如需 Intel HT 技術的詳細資訊，請參閱 [intel Hyper-Threading 技術](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* 如需有關 AMD SMT 的詳細資訊，請參閱「 [Zen」核心架構](https://www.amd.com/en/technologies/zen-core)

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>瞭解 Hyper-v 如何將處理器虛擬化

在考慮基礎程式排程器類型之前，瞭解 Hyper-v 架構也有説明。 您可以在 [Hyper-v 技術總覽](../hyper-v-technology-overview.md)中找到一般摘要。 以下是此文章的重要概念：

* Hyper-v 會建立和管理虛擬機器磁碟分割，在這些資料分割中，系統會在管理程式控制下配置並共用計算資源。 磁碟分割可在所有來賓虛擬機器之間，以及在來賓 Vm 和根磁碟分割之間提供強式隔離界限。

* 根磁碟分割本身是虛擬機器磁碟分割，雖然它具有唯一的屬性，而且許可權比來賓虛擬機器更高。 根磁碟分割提供管理服務，可控制所有來賓虛擬機器、提供來賓的虛擬裝置支援，以及管理來賓虛擬機器的所有裝置 i/o。 Microsoft 強烈建議不要在根磁碟分割中執行任何應用程式工作負載。

* 根磁碟分割的每個虛擬處理器 (VP) 都會將1:1 對應到基礎邏輯處理器 (LP) 。 主機 VP 一律會在相同的基礎 LP 上執行–不會遷移根磁碟分割的 VPs。

* 根據預設，主機 VPs 執行所在的 LPs 也可以執行來賓 VPs。

* 在任何可用的邏輯處理器上執行的程式管理者可能會排程來賓副總。 雖然「管理器」排程器會考慮暫時快取位置、NUMA 拓撲，以及排程來賓 VP 時的許多其他因素，但最終可能會在任何主機 LP 上排程 VP。

## <a name="hypervisor-scheduler-types"></a>虛擬程式排程器類型

從 Windows Server 2016 開始，Hyper-v 基礎程式支援多種排程器邏輯，以決定基礎程式如何排程基礎邏輯處理器上的虛擬處理器。 這些排程器類型為：

### <a name="the-classic-scheduler"></a>傳統排程器

傳統排程器是自其開始之後的所有 Windows Hyper-v 虛擬程式版本的預設值，包括 Windows Server 2016 Hyper-v。 傳統排程器會為來賓虛擬處理器提供公平共用、優先迴圈配置資源排程模型。

傳統的排程器類型最適合用於私用雲端、主機服務提供者等的大部分傳統 Hyper-v 用途。 效能特性很容易理解，且最適合用來支援各種虛擬化案例，例如 VPs 對 LPs 的過度訂用帳戶、執行許多並存的 Vm 和工作負載、執行較大規模的高效能 Vm、支援 Hyper-v 的完整功能集，且不受限制。

### <a name="the-core-scheduler"></a>核心排程器

虛擬程式核心排程器是傳統排程器邏輯的新替代方案，在 Windows Server 2016 和 Windows 10 版本1607中引進。 核心排程器提供強式安全性界限以進行來賓工作負載隔離，並針對在啟用 SMT 的虛擬化主機上執行的 Vm 內的工作負載降低效能變化。 核心排程器可讓您同時在啟用相同 SMT 的虛擬化主機上同時執行 SMT 和非 SMT 虛擬機器。

核心排程器會利用虛擬化主機的 SMT 拓撲，並選擇性地將 SMT 組公開給來賓虛擬機器，並將來賓虛擬處理器群組從相同的虛擬機器排程到 SMT 邏輯處理器群組。 這是以對稱方式完成，因此如果 LPs 是在兩個群組中，則 VPs 會排程在兩個群組中，而且 Vm 之間永遠不會共用一個核心。
當 VP 針對未啟用 SMT 的虛擬機器進行排程時，VP 會在執行時使用整個核心。

核心排程器的整體結果如下：

* 來賓 VPs 受限於在基礎實體核心組上執行、將 VM 隔離到處理器核心界限，進而降低來自惡意 Vm 的側聲道窺探攻擊的弱點。

* 輸送量的變化大幅減少。

* 效能可能會降低，因為如果只有一組 VPs 可以執行，則只有核心中的其中一個指令串流會在另一個閒置時執行。

* 在來賓虛擬機器中執行的 OS 和應用程式可以使用 SMT 行為和程式設計介面 (Api) 來控制和散發跨 SMT 執行緒的工作，就像執行非虛擬化一樣。

* 來賓工作負載隔離的增強式安全性界限-來賓 VPs 受限於在基礎實體核心配對上執行，減少對側通道窺探攻擊的弱點。

從 Windows Server 2019 開始，預設會使用核心排程器。 在 Windows Server 2016 上，核心排程器是選擇性的，且必須由 Hyper-v 主機系統管理員明確啟用，而且傳統排程器是預設值。

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>主機 SMT 已停用的核心排程器行為

如果虛擬程式已設定為使用核心排程器類型，但 SMT 功能已停用或不存在於虛擬化主機上，則無論是否使用程式管理器排程器的類型設定為何，程式管理器都會使用傳統排程器行為。

### <a name="the-root-scheduler"></a>根排程器

根排程器是 Windows 10 版本1803引進。 當根排程器類型已啟用時，虛擬程式管理人員 cedes 會對根分割區進行工作排程的控制。 根磁碟分割的 OS 實例中的 NT 排程器會管理排程工作至系統 LPs 的所有層面。

根排程器可解決支援公用程式磁碟分割所固有的獨特需求，以提供強式工作負載隔離，與 Windows Defender 應用程式防護 (WDAG) 搭配使用。 在此案例中，將排程責任保留給根 OS 可提供數個優點。 例如，適用于容器案例的 CPU 資源控制可搭配公用程式分割區使用，以簡化管理和部署。 此外，根 OS 排程器可以立即收集容器內工作負載 CPU 使用率的相關計量，並使用此資料作為與系統中所有其他工作負載適用的相同排程原則的輸入。 這些相同的計量也有助於清楚地將應用程式容器中完成的工作屬性（attribute）設定為主機系統。 使用傳統的虛擬機器工作負載來追蹤這些計量會比較困難，在此情況下，所有執行中 VM 的部分工作會在根磁碟分割中進行。

#### <a name="root-scheduler-use-on-client-systems"></a>用戶端系統上的根排程器使用

從 Windows 10 版本1803開始，預設會在用戶端系統上使用根排程器，在此情況下，可以啟用基礎程式來支援虛擬化型安全性與 WDAG 工作負載隔離，以及使用異類核心架構來適當地操作未來系統。 這是用戶端系統唯一支援的程式管理器排程器設定。 系統管理員不應該嘗試覆寫 Windows 10 用戶端系統上的預設的虛擬程式管理器類型。

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>虛擬機器 CPU 資源控制和根排程器

啟用基礎程式根排程器時，不支援 Hyper-v 提供的虛擬機器處理器資源控制，因為根目錄作業系統的排程器邏輯會以全域方式管理主機資源，且不知道 VM 的特定設定。 Hyper-v 的每一 VM 處理器資源控制（例如 cap、權數和保留）僅適用于程式管理者直接控制 VP 排程（例如，使用傳統和核心排程器類型）的情況。

#### <a name="root-scheduler-use-on-server-systems"></a>伺服器系統上的根排程器使用

目前不建議使用根排程器在伺服器上搭配 Hyper-v 使用，因為它的效能特性尚未具有完整的特性和微調，以容納許多伺服器虛擬化部署的一般工作負載。

## <a name="enabling-smt-in-guest-virtual-machines"></a>在來賓虛擬機器中啟用 SMT

當虛擬化主機的虛擬化主機的虛擬化主機的執行程式設定為使用核心排程器類型後，您就可以視需要將來賓虛擬機器設定為使用 SMT。 公開 VPs 超執行緒至來賓虛擬機器的事實，可讓客體作業系統中執行的排程器和在 VM 中執行的工作負載，在自己的工作排程中偵測並利用 SMT 拓朴。 在 Windows Server 2016 上，預設不會設定 guest SMT，而且必須由 Hyper-v 主機系統管理員明確啟用。 從 Windows Server 2019 開始，在主機上建立的新 Vm 預設會繼承主機的 SMT 拓撲。  也就是說，在主機上建立的9.0 版 VM （每個核心2個 SMT 執行緒）也會看到每個核心2個 SMT 執行緒。

PowerShell 必須用來在來賓虛擬機器中啟用 SMT;Hyper-v 管理員中未提供任何使用者介面。
若要在來賓虛擬機器中啟用 SMT，請以足夠的許可權開啟 PowerShell 視窗，然後輸入：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

其中 <n> 是來賓 VM 每個核心的 SMT 執行緒數目。
請注意， <n> = 0 會設定 HwThreadCountPerCore 值，使其符合每個核心值的主機 SMT 執行緒計數。

> [!NOTE]
> 從 Windows Server 2019 開始支援設定 HwThreadCountPerCore = 0。

以下是從虛擬機器中執行的客體作業系統（已啟用2個虛擬處理器和 SMT）取得的系統資訊範例。 客體作業系統偵測到屬於相同核心的2個邏輯處理器。

![螢幕擷取畫面，顯示已啟用 SMT 之來賓 VM 中的 msinfo32](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>設定 Windows Server 2016 Hyper-v 上的虛擬程式管理器類型

Windows Server 2016 Hyper-v 預設會使用傳統的程式管理器排程器模型。 您可以選擇性地將虛擬程式設定為使用核心排程器，藉由限制來賓 VPs 在對應的實體 SMT 組上執行，以及支援使用虛擬機器搭配 SMT 的來賓 VPs 排程來提高安全性。

> [!NOTE]
> Microsoft 建議所有執行 Windows Server 2016 Hyper-v 的客戶都選取核心排程器，以確保其虛擬化主機能以最佳方式受到保護，以防範潛在的惡意來賓 Vm。

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>Windows Server 2019 Hyper-v 預設為使用核心排程器

為了協助確保在最佳的安全性設定中部署 Hyper-v 主機，Windows Server 2019 Hyper-v 現在預設會使用核心虛擬程式排程器模型。 主機系統管理員可以選擇將主機設定為使用舊版傳統排程器。 在覆寫排程器類型的預設設定之前，系統管理員應該仔細閱讀、瞭解並考慮每個排程器類型對虛擬化主機安全性和效能的影響。 如需詳細資訊，請參閱 [hyper-v 虛擬程式排程器類型選取專案](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/about-hyper-v-scheduler-type-selection) 。

### <a name="required-updates"></a>必要的更新

> [!NOTE]
> 若要使用本檔中所述的程式管理器排程器功能，必須進行下列更新。 這些更新包括支援新 `hypervisorschedulertype` BCD 選項的變更，這是主機設定的必要選項。

| 版本 | 版本  | 需要更新 | 知識庫文章 |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | 無 | 無 |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>在 Windows Server 上選取虛擬程式排程器類型

虛擬程式管理器設定是透過 hypervisorschedulertype BCD 專案來控制。

若要選取排程器類型，請使用系統管理員許可權開啟命令提示字元：

```
bcdedit /set hypervisorschedulertype type
```

其中 `type` 是下列其中一個：

* 傳統
* 核心
* Root

系統必須重新開機，系統管理器排程器類型的任何變更才會生效。

> [!NOTE]
> 目前 Windows Server Hyper-v 不支援虛擬程式根排程器。 Hyper-v 系統管理員不應該嘗試設定根排程器以搭配伺服器虛擬化案例使用。

## <a name="determining-the-current-scheduler-type"></a>判斷目前的排程器類型

您可以藉由檢查系統記錄檔中的系統記錄檔事件檢視器中的 [最新的啟動程式啟動事件識別碼 2] 來判斷目前正在使用的目前程式管理器類型，其會報告在啟動程式啟動時所設定的虛擬程式 您可以從 Windows 事件檢視器或透過 PowerShell 取得管理程式啟動事件。

管理程式啟動事件識別碼2代表「管理器」排程器類型，其中：

- 1 = 傳統排程器，SMT 已停用

- 2 = 傳統排程器

- 3 = 核心排程器

- 4 = 根排程器

![顯示管理程式啟動事件識別碼2詳細資料的螢幕擷取畫面](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![顯示事件檢視器顯示管理程式啟動事件識別碼2的螢幕擷取畫面](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>使用 PowerShell 查詢 Hyper-v 虛擬程式排程器類型啟動事件

若要使用 PowerShell 查詢虛擬程式事件識別碼2，請從 PowerShell 提示字元輸入下列命令。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![顯示程式管理程式啟動事件識別碼2的 PowerShell 查詢和結果螢幕擷取畫面](media/Hyper-V-CoreScheduler-PowerShell.png)
