---
title: 了解與使用 HYPER-V hypervisor 排程器類型
description: 模式上的 HYPER-V 的排程器使用提供的 HYPER-V 主機系統管理員資訊
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: 7af6d68b02367d349580eacb27405c6f37e97ff8
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783690"
---
# 管理 HYPER-V hypervisor 排程器類型

>適用於： Windows 10、 Windows Server 2016、 Windows Server 版本 1709年、 Windows Server 版本 1803 起，Windows Server 2019

本文章說明新模式的虛擬處理器排程首次引進 Windows Server 2016 中的邏輯。 這些模式中或排程器類型，判斷如何配置 HYPER-V hypervisor 和跨客體虛擬處理器管理工作。 HYPER-V 主機系統管理員可以選取最適合客體虛擬機器 (Vm) 與設定 Vm 以充分利用排程邏輯的 hypervisor 排程器類型。

>[!NOTE]
>更新，才能使用這份文件中所述的 hypervisor 排程器功能。 如需詳細資訊，請參閱[必要更新](#required-updates)。

## 背景

之前討論的邏輯和 HYPER-V 虛擬處理器排程背後的控制項，最好先檢閱本文章說明的基本概念。

### 了解 SMT

同時多執行緒或 SMT，是一種技術的現代處理器的設計中各種，可讓是獨立的獨立執行緒供共用處理器的資源。 SMT 通常增加指令輸送量方式平行處理計算，盡可能提供大部分的工作負載中等的效能提升，但沒有效能獲得或甚至是稍微損失的效能時可能會發生爭用之間的執行緒共用的處理器資源就會發生。
提供來自 Intel 和 AMD 支援 SMT 處理器。 Intel 指的是做為 Intel 超執行緒技術或 Intel HT 其 SMT 供應項目的。

基於本文章的目的，SMT 和如何利用 hyper-v 的說明適用於 Intel 和 AMD 同時系統。

* 如需 Intel HT 技術的詳細資訊，請參閱[Intel 超執行緒技術](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* 如需 AMD SMT 的詳細資訊，請參閱[「 Zen 」 核心架構](https://www.amd.com/en/technologies/zen-core)

## 了解如何 HYPER-V 把處理器

之前先考慮 hypervisor 排程器類型，也很有幫助，若要了解 HYPER-V 架構。 您可以在[HYPER-V 技術概觀](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview)中找到一般的摘要。 以下是本文的重要概念：

* HYPER-V 會建立和管理虛擬機器磁碟分割，跨哪些運算資源是配置和共用，受控制的 hypervisor。 磁碟分割提供強式隔離界限之間所有客體虛擬機器，以及在客體 Vm 和根磁碟分割。

* 根磁碟分割本身是虛擬機器的磁碟分割，雖然它具有唯一的屬性和客體虛擬機器比更大的權限。 根磁碟分割提供控制所有客體虛擬機器管理服務，提供虛擬裝置支援的客體，及管理客體虛擬機器的所有裝置 I/O。 Microsoft 強烈建議不執行任何應用程式工作負載的根磁碟分割中。

* 根磁碟分割的每個虛擬處理器 (VP) 是對應的 1:1 到基礎的邏輯處理器 (LP)。 主機副總裁一律會執行相同的基礎 LP 上 – 的根磁碟分割 Vp 沒有移轉。

* 根據預設，執行主機 Vp 的 Lp 也可以執行客體 Vp。

* Hypervisor 可能會排程客體副總裁，任何可用的邏輯處理器上執行。 Hypervisor 排程器負責排程客體副總裁時考慮時態性快取位置、 NUMA 拓撲，以及許多其他因素，而最後可能排程副總裁 LP 任何主機上。

## Hypervisor 排程器類型

從 Windows Server 2016 開始，HYPER-V hypervisor 支援數個模式的排程器邏輯，來決定如何 hypervisor 排程基礎的邏輯處理器上的虛擬處理器。 這些排程器類型包括：

- [傳統、 以公平共用為排程器](#the-classic-scheduler)
- [核心排程器](#the-core-scheduler)
- [根排程器](#the-root-scheduler)

### 傳統的排程器

傳統的排程器已經自一，包括 Windows Server 2016 HYPER-V 所有版本的 Windows HYPER-V hypervisor 的預設值。 傳統的排程器提供公平共用，優先循環配置資源排程模型客體虛擬處理器。

傳統的排程器類型是最適合大部分的傳統 HYPER-V 使用 – 適用於私人雲端、 主機服務提供者，以及等等。 效能特性了解和最適合最佳化，以支援各種虛擬化案例，例如過度 Vp 到 LPs，同時執行許多異質性 Vm 和工作負載、 執行高的規模更大的訂閱HYPER-V 無限制，以及更多的設定虛擬機器，支援的完整功能的效能。

### 核心排程器

Hypervisor 核心排程器是傳統排程器邏輯，Windows Server 2016 和 Windows 10 版本 1607年中導入新的替代。 核心排程器提供客體工作負載隔離的強式安全性界限和適用於 SMT 啟用虛擬化主機執行的虛擬機器內的工作負載的效能降低的變化。 核心排程器可讓您同時在相同的 SMT 啟用虛擬化主機上執行同時 SMT 和非 SMT 虛擬機器。

核心排程器利用虛擬化主機 SMT 拓撲，並選擇性地公開 SMT 成對出現客體虛擬機器，並排程群組的客體虛擬處理器從相同虛擬機器的 SMT 邏輯處理器群組到。 這是為了對稱讓如果 Lp 中的兩個，Vp 群組排程的兩個群組中，並在 Vm 之間永遠不會共用核心。
針對虛擬機器不需要 SMT 排定副總裁啟用，副總裁在執行時，會消耗整個核心。

整體的核心排程器結果是：

* 客體 Vp 會被限制為基礎實體核心組，隔離 VM 以處理器核心界限，從而降低弱點旁路窺探功能攻擊從惡意的虛擬機器上執行。

* 大幅減少在輸送量的變化。

* 可能會降低效能，因為如果只有一個 Vp 一組可以執行，只是其中一個指令串流核心中執行其他剩下閒置時。

* 在作業系統和客體虛擬機器中執行的應用程式可以利用 SMT 行為和程式設計介面 (Api) 來控制，並將工作分散在 SMT 執行緒，就像它們何時會執行非虛擬化。

* 客體工作負載隔離客體 Vp 的強式安全性界限會被限制為基礎的實體核心組，減少弱點旁路窺探功能的攻擊上執行。

核心排程器將會使用預設在 Windows Server 2019 中開始。 在 Windows Server 2016 中，核心排程器為選擇性，必須明確啟用 HYPER-V 主機系統管理員，傳統的排程器是預設值。

#### 與主機的核心排程器行為 SMT 停用

如果 hypervisor 會設定為使用核心排程器類型，但 SMT 功能已停用或不存在於虛擬化主機，hypervisor 會使用傳統的排程器行為，無論 hypervisor 排程器類型設定。

### 根排程器

根排程器是隨 Windows 10 版本 1803年中引進。 啟用根排程器類型時，hypervisor cedes 工作排程到根磁碟分割的控制項。 根磁碟分割的作業系統執行個體中 NT 排程器管理排程工作 Lp 系統的各個層面。

根排程器位址為使用與 Windows Defender 應用程式防護 (WDAG)，提供強式的工作負載的隔離性，以支援的公用程式磁碟分割固有的獨特需求。 在這個案例中，離開排程到根 OS 責任提供幾個優點。 例如，適用於容器案例的 CPU 資源控制項可能搭配公用程式磁碟分割，以簡化管理及部署。 此外，根 OS 排程器可以輕易地收集計量相關工作負載在容器內的 CPU 使用率，並為相同的排程原則適用於所有其他工作負載的輸入使用此資料在系統中。 這些相同的計量也可以協助清楚地運作完成到主機系統應用程式容器中的屬性。 追蹤下列計量會更難使用傳統的虛擬機器的工作負載，需要代表所有正在執行的 VM 的一些工作根磁碟分割中的位置。

#### 在用戶端系統上的根排程器使用

從 Windows 10 版本 1803年開始，根排程器依預設使用僅，用戶端系統上支援虛擬化型安全性和 WDAG 工作負載的隔離性，以及適當的未來系統的操作，啟用 hypervisor 可能其中異質核心架構。 這是用戶端系統的唯一支援的 hypervisor 排程器設定。 系統管理員不應嘗試覆寫預設 hypervisor 排程器類型，在 Windows 10 用戶端系統上。

#### 虛擬機器的 CPU 資源控制項和根排程器

Hypervisor 根排程器為根作業系統的排程器邏輯管理主機全域為基礎的資源，且沒有的 VM 的知識啟用時不支援 HYPER-V 所提供的虛擬機器處理器資源控制項特定組態設定。 Hypervisor 直接控制副總裁排程，例如如同傳統和核心排程器類型的位置的 HYPER-V VM 每個處理器資源控制項，例如 caps、 重量和會保留，資料才適用。

#### 伺服器的系統上的根排程器使用

根排程器是建議您不要使用與 HYPER-V 伺服器上在這個時候，因為其效能特性有尚未完全加以區分及調整以容納各種工作負載典型的許多伺服器虛擬化部署。

## 在客體虛擬機器中啟用 SMT

一旦了虛擬主機的 hypervisor 設定要使用核心排程器類型，可能會利用 SMT，如果需要的話，設定客體虛擬機器。 公開 Vp 是客體虛擬機器的超執行緒的事實讓中的客體作業系統和偵測，並利用 SMT 拓撲中他們自己的工作排程在 VM 中執行的工作負載的排程器。 Windows Server 2016 上，客體 SMT 尚未設定的預設值，並由 HYPER-V 主機系統管理員必須明確啟用。 開始使用 Windows Server 2019，在主機上建立新的虛擬機器將預設會繼承的主機 SMT 拓撲。  也就是每個核心的 2 個 SMT 執行緒主機建立 9.0 VM 版本也會看到每個核心的 2 個 SMT 執行緒。

必須使用 PowerShell 來啟用 SMT 在客體虛擬機器;提供 HYPER-V 管理員中沒有使用者介面。
若要啟用 SMT 客體虛擬機器中，開啟 PowerShell 視窗與充足的權限，然後輸入：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

其中<n>是的客體 VM 將會看到每個核心 SMT 執行緒的數目。  
請注意， <n> = 0 將設定 HwThreadCountPerCore 值以符合每個核心值的主機 SMT 執行緒的計數。

>[!NOTE] 
>設定 HwThreadCountPerCore = 0 從 Windows Server 2019 支援。

以下是取自 2 部虛擬處理器與虛擬機器中執行，客體作業系統的系統資訊的範例，並且 SMT 啟用。 客體作業系統會偵測屬於相同的核心的 2 個邏輯處理器。

![螢幕擷取畫面顯示 msinfo32 中以客體 VM 與 SMT 啟用](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## 設定在 Windows Server 2016 HYPER-V hypervisor 排程器類型

Windows Server 2016 HYPER-V 預設會使用傳統的 hypervisor 排程器模型。 Hypervisor 可以選擇性地設定為使用核心排程，來提升安全性藉由限制客體 Vp 對應實體 SMT 組上執行，並支援具有 SMT 為其客體 Vp 排程的虛擬機器的使用。

>[!NOTE]
>Microsoft 建議所有的客戶執行 Windows Server 2016 HYPER-V 選取核心排程器，以確保其虛擬化主機的最佳方式能防止潛在惡意客體 Vm。

## Windows Server 2019 HYPER-V 預設為使用核心排程器

為了協助確保最佳的安全性組態中部署 HYPER-V 主機，Windows Server 2019 HYPER-V 現在預設會使用核心 hypervisor 排程器模型。 主機系統管理員可能會選擇性設定要使用舊版傳統排程器主機。 系統管理員應仔細閱讀、 了解並考慮的安全性和效能的虛擬化主機之前覆寫排程器類型的預設設定對每個排程器類型的影響。  如需詳細資訊，請參閱[了解 HYPER-V 排程器類型選取項目](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection)。

### 必要的更新

>[!NOTE]
>下列的更新，才能使用這份文件中所述的 hypervisor 排程器功能。 這些更新會包含支援新的 'hypervisorschedulertype' BCD 選項，這是必要的主機設定的變更。

| 版本 | 版本  | 需要更新 | KB 文章 |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | 無 | 無 |

## 選取在 Windows Server 上的 hypervisor 排程器類型

Hypervisor 排程器組態是透過 hypervisorschedulertype BCD 項目所控制。

若要選取排程器類型，開啟系統管理員權限的命令提示字元：

``` command
     bcdedit /set hypervisorschedulertype type
```

其中`type`是下列其中一個：

* 傳統
* 核心版

系統必須重新開機的任何變更為 hypervisor 的排程器類型，才會生效。

>[!NOTE]
>Hypervisor 根排程器此時不支援在 Windows Server HYPER-V。 HYPER-V 系統管理員不應嘗試使用伺服器虛擬化案例設定根排程器，以供使用。

## 判斷目前的排程器類型

您可以判斷目前的 hypervisor 排程器類型，藉由檢查事件檢視器中的最新的 hypervisor 啟動事件識別碼 2 和系統記錄檔，就會報告設定在 hypervisor 啟動 hypervisor 排程器類型的使用中。 從 Windows 事件檢視器，或透過 PowerShell 可以取得 Hypervisor 啟動事件。

Hypervisor 啟動事件識別碼 2 代表 hypervisor 排程器類型，其中：

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![螢幕擷取畫面顯示 hypervisor 啟動事件識別碼 2 詳細資料](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![顯示事件檢視器顯示 hypervisor 啟動事件識別碼 2 螢幕擷取畫面](media/Hyper-V-CoreScheduler-EventViewer.png)

### 查詢的 HYPER-V hypervisor 排程器類型啟動事件使用 PowerShell

查詢 hypervisor 事件識別碼 2 使用 PowerShell，從 PowerShell 提示字元中輸入下列命令。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![顯示 PowerShell 查詢和結果 hypervisor 啟動事件識別碼 2 螢幕擷取畫面](media/Hyper-V-CoreScheduler-PowerShell.png)
