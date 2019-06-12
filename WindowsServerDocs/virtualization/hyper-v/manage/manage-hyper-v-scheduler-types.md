---
title: 了解與使用 HYPER-V hypervisor 排程器類型
description: 提供 HYPER-V 主機系統管理員的資訊上使用 HYPER-V 的排程器模式
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: c0c2f85fbbeca9e8ac5d40bbcb71f286fabfb65c
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501666"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>管理 HYPER-V hypervisor 排程器類型

>適用於：Windows 10，Windows Server 2016，Windows Server 1709 版、 Windows Server，版本 1803，Windows Server 2019

此文章說明新的虛擬處理器排程第一次在 Windows Server 2016 中引進的邏輯模式。 這些模式或排程器型別，決定 HYPER-V hypervisor 如何配置和管理工作分散到客體虛擬處理器。 HYPER-V 主機系統管理員可以選取最適合用於客體虛擬機器 (Vm)，並設定的 Vm，以善用排程邏輯的 hypervisor 排程器類型。

>[!NOTE]
>需要更新才能使用這份文件中所述的 hypervisor 排程器功能。 如需詳細資訊，請參閱 <<c0> [ 必要更新](#required-updates)。

## <a name="background"></a>背景

之前討論的邏輯與 HYPER-V 的虛擬處理器排程後控制項，最好先檢閱本文章涵蓋的基本概念。

### <a name="understanding-smt"></a>了解 SMT

同時進行多執行緒處理或 SMT，是一種技術使用現代處理器設計中，讓共用的個別、 獨立執行執行緒的處理器資源。 SMT 通常提供適度的效能提升，大部分的工作負載的平行處理計算，可能的話，請增加指令輸送量，但沒有效能取得，或甚至是效能稍微降低可能就會發生時之間的競爭執行緒共用的處理器資源，就會發生。
提供從 Intel 和 AMD 處理器支援 SMT。 Intel 指的是以 Intel 超執行緒技術或 Intel HT 其 SMT 供應項目。

基於本文的目的的 SMT，以及如何利用 hyper-v 描述同樣適用於 Intel 和 AMD 系統。

* 如需有關 Intel HT 技術的詳細資訊，請參閱[Intel 超執行緒技術](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* 如需有關 AMD SMT 的詳細資訊，請參閱["Zen"核心架構](https://www.amd.com/en/technologies/zen-core)

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>了解如何為 HYPER-V 虛擬化的處理器

在考慮之前 hypervisor 排程器類型，它也很有用，了解 HYPER-V 架構。 您可以尋找在一般摘要[HYPER-V 技術概觀](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview)。 以下是這個發行項的重要概念：

* HYPER-V 會建立和管理虛擬機器資料分割，在哪些計算資源配置及共用，hypervisor 的控制之下。 資料分割提供強式的隔離界限，所有客體虛擬機器，以及客體 Vm 與根磁碟分割。

* 在根資料分割本身是虛擬機器的磁碟分割，不過它唯一的屬性和更大的權限，比客體虛擬機器。 在根磁碟分割提供控制所有客體虛擬機器管理服務、 虛擬裝置支援的來賓，以及管理客體虛擬機器的所有裝置 I/O。 Microsoft 強烈建議未在根磁碟分割中執行任何應用程式工作負載。

* 在根磁碟分割每個虛擬處理器 (VP) 是對應的 1:1 管理基礎的邏輯處理器 (LP)。 主應用程式副總裁一定會執行相同的基礎 LP 上 – 沒有移轉的根磁碟分割 VPs。

* 根據預設，在其執行主機 VPs LPs 也可以執行客體 VPs。

* Hypervisor 可能排定客體副總裁，任何可用的邏輯處理器上執行。 Hypervisor 排程器會負責排程客體副總裁時考慮暫時快取位置、 NUMA 拓撲及許多其他因素，而最終無法排定副總裁 LP 任何主機上。

## <a name="hypervisor-scheduler-types"></a>Hypervisor 排程器類型

從 Windows Server 2016 開始，HYPER-V hypervisor 支援數種模式的排程器的邏輯，以決定 hypervisor 排程基礎的邏輯處理器上的虛擬處理器的方式。 這些排程器類型如下：

- [傳統的公平共用的排程器](#the-classic-scheduler)
- [核心排程器](#the-core-scheduler)
- [根排程器](#the-root-scheduler)

### <a name="the-classic-scheduler"></a>傳統的排程器

傳統的排程器已自創立，包括 Windows Server 2016 HYPER-V 的 Windows HYPER-V hypervisor 的所有版本的預設值。 傳統的排程器提供合理，先佔式循環配置資源排程模型適用於客體的虛擬處理器。

傳統的排程器類型是最適合用於大部分的傳統的 HYPER-V 會使用 – 適用於私人雲端、 主機服務提供者，等等。 效能特性可充分了解及最佳化以支援各種不同的虛擬化案例，例如過度的訂用帳戶的 VPs 至 LPs，同時執行許多異質性的 Vm 和工作負載，執行較大的比例高支援的完整功能的 Vm 的效能設定 HYPER-V 而沒有任何限制，以及更多。

### <a name="the-core-scheduler"></a>核心排程器

Hypervisor 核心排程器是傳統的排程器的邏輯，在 Windows Server 2016 和 Windows 10 1607年版中引進的新替代方案。 核心排程器提供強式的安全性界限進行客體工作負載隔離，並在 SMT 啟用虛擬化主機執行的 Vm 內的工作負載的效能降低變化性。 核心排程器可讓您在相同的 SMT 啟用虛擬化主機上同時執行 SMT，以及非 SMT 的虛擬機器。

核心排程器會利用虛擬化主機的 SMT 拓撲中，並選擇性地公開到客體虛擬機器和客體的虛擬處理器的排程群組的 SMT 組，從相同的虛擬機器到 SMT 邏輯處理器的群組。 這是對稱，因此如果 LPs 中的兩個 VPs 群組都已排程的兩個群組中，且永遠不會共用核心的 Vm 之間。
當虛擬機器，而不需要 SMT 排定副總裁啟用，副總裁在執行時，會消耗整個核心。

核心排程器的整體結果是：

* 客體 VPs 都受限於基礎實體核心組，隔離的 VM 處理器核心界限，因此減少至側邊通道受到窺探攻擊的弱點可能會，從惡意虛擬機器上執行。

* 大幅減少輸送量的變化性。

* 可能會降低效能，因為如果只有一群 VPs 的其中一個可執行，只有其中一個核心中的指令資料流執行時另將處於閒置。

* OS 和客體虛擬機器中執行的應用程式可以利用 SMT 行為和程式設計介面 (Api) 來控制，並將工作分散到 SMT 執行緒，就像它們何時會執行非虛擬化。

* 之客體工作負載隔離客體 VPs 的強式的安全性範圍限制為基礎的實體核心配對，減少至側邊通道受到窺探攻擊的弱點可能會執行。

從 Windows Server 2019 的預設將會使用核心排程器。 在 Windows Server 2016 中，核心排程器是選擇性的您必須明確啟用 HYPER-V 主機系統管理員和傳統的排程器是預設值。

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>與主應用程式停用的 SMT 的核心排程器行為

如果 hypervisor 設定為使用核心排程器類型，但 SMT 功能已停用或不存在於虛擬化主機上，hypervisor 會使用傳統的排程器的行為，無論 hypervisor 排程器類型設定為何。

### <a name="the-root-scheduler"></a>根排程器

與 Windows 10 1803年版引進了根排程器。 啟用根排程器型別時，hypervisor 會 cedes 控制工作的排程，以在根磁碟分割。 在根磁碟分割作業系統執行個體的 NT 排程器管理排程工作至系統 LPs 的所有層面。

根排程器位址提供強式工作負載隔離，因為使用與 Windows Defender Application Guard (WDAG) 原本就有支援的公用程式磁碟分割的獨特需求。 在此案例中，保留排程根 OS 的責任，將可提供許多優點。 例如，CPU 資源控制適用於容器的案例可能搭配公用程式磁碟分割，以簡化管理和部署。 此外，根 OS 排程器可以輕易地收集有關工作負載在容器內的 CPU 使用率計量，並做為相同的排程原則適用於所有其他工作負載輸入中的這項資料，在系統中。 這些相同的度量也可協助清楚應用程式容器主機系統中完成的工作的屬性。 追蹤這些計量是更難用於傳統的虛擬機器工作負載，代表所有執行中 VM 的一些工作發生的地方在根磁碟分割。

#### <a name="root-scheduler-use-on-client-systems"></a>用戶端系統上的根排程器使用

從 Windows 10 1803年版開始，根排程器會依預設，用戶端系統上以支援虛擬化型安全性和隔離 WDAG 工作負載，和與未來的系統正常運作，啟用 hypervisor 可能位置異質的核心架構。 這是用戶端系統的唯一支援的 hypervisor 排程器組態。 系統管理員不應該嘗試覆寫預設的 hypervisor 排程器類型，在 Windows 10 用戶端系統上。

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>虛擬機器的 CPU 資源控制和根排程器

Hypervisor 根排程器已啟用，因為根作業系統的排程器邏輯正在管理以全域為基礎的主機資源，而不需要的虛擬機器的知識時，不支援提供 hyper-v 虛擬機器的處理器資源控制特定的組態設定。 在 hypervisor 直接控制的排程，例如與傳統和核心排程器型別一樣的副總裁，HYPER-V 每個 VM 處理器資源控制，例如大寫字、 加權值，以及保留，才適用。

#### <a name="root-scheduler-use-on-server-systems"></a>根伺服器系統上的排程器使用

根排程器不會建議搭配 HYPER-V 伺服器上目前，因為其效能特性有尚未完全特性及微調，以容納廣泛的許多伺服器虛擬化部署的一般工作負載。

## <a name="enabling-smt-in-guest-virtual-machines"></a>啟用客體虛擬機器中的 SMT

一旦虛擬化主機的 hypervisor 被設定為使用核心排程器型別，客體虛擬機器可能會設定為使用 SMT，如有需要。 公開 VPs 是客體虛擬機器的超執行緒，這可讓客體作業系統和執行 VM 以偵測並運用在自己的工作排程的 SMT 拓樸中的工作負載中的排程器。 Windows Server 2016，客體 SMT 並非預設設定，必須明確啟用 HYPER-V 的主機系統管理員。 從 Windows Server 2019 開始，在主機上建立新的 Vm 將主機的 SMT 拓撲預設繼承。  也就是 9.0 的 VM 建立每個核心 2 SMT 執行緒的主機的版本也會看到每個核心 2 SMT 執行緒。

必須使用 PowerShell 來 SMT 客體虛擬機器中啟用;提供在 HYPER-V 管理員中沒有使用者介面。
若要啟用 SMT 客體虛擬機器中，開啟 PowerShell 視窗具有足夠的權限，然後輸入：

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

其中<n>的客體 VM 將會看到為每個核心的 SMT 執行緒數目。  
請注意， <n> = 0 將會設定 HwThreadCountPerCore 值以符合主機的 SMT 執行緒計數，每個核心的值。

>[!NOTE] 
>設定 HwThreadCountPerCore = 0 具有 Windows Server 2019 開始支援。

以下是範例取自具有 2 個虛擬處理器的虛擬機器中執行的客體作業系統的系統資訊和 SMT 啟用。 客體作業系統偵測到 2 個邏輯處理器，屬於相同的核心。

![螢幕擷取畫面顯示 msinfo32 的客體 VM 與啟用的 SMT](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>設定 hypervisor 的排程器型別上 Windows Server 2016 HYPER-V

Windows Server 2016 HYPER-V 預設會使用傳統的 hypervisor 排程器模型。 Hypervisor 可以選擇性地設定為使用核心排程器，以加強安全性限制來賓 VPs 執行對應的實體 SMT 配對，並支援其客體 VPs 排程 SMT 的虛擬機器使用。

>[!NOTE]
>Microsoft 建議執行 Windows Server 2016 HYPER-V 的所有客戶都選取核心排程器，以確保其虛擬化主機以最佳方式不受潛在惡意的客體 Vm。

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>若要使用核心排程器的 Windows Server 2019 HYPER-V 預設值

為了協助確保獲得最佳的安全性組態中部署 HYPER-V 主機，Windows Server 2019 HYPER-V 現在預設會使用核心 hypervisor 排程器模型。 主機系統管理員可以選擇性地設定主應用程式使用舊版的傳統排程器。 系統管理員應該仔細閱讀、 了解並考慮每個排程器型別具有的安全性和效能之前覆寫的排程器類型的預設設定的虛擬化主機的影響。  請參閱[了解 HYPER-V 排程器類型選取項目](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection)如需詳細資訊。

### <a name="required-updates"></a>必要的更新

>[!NOTE]
>下列更新，才能使用這份文件中所述的 hypervisor 排程器功能。 這些更新包括變更以支援新的 'hypervisorschedulertype' BCD 選項，這是所需的主應用程式組態。

| Version | 發行  | 需要更新 | 知識庫文件 |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | None | None |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>選取 Windows Server 上的 hypervisor 排程器類型

Hypervisor 排程器組態是透過 hypervisorschedulertype BCD 項目加以控制。

若要選取的排程器型別，請使用系統管理員權限開啟命令提示字元：

``` command
     bcdedit /set hypervisorschedulertype type
```

其中`type`是其中一個：

* 傳統
* 核心
* 根目錄

系統必須重新開機的 hypervisor 排程器型別，才會生效的任何變更。

>[!NOTE]
>Hypervisor 根排程器目前不支援 Windows Server HYPER-V 上。 HYPER-V 系統管理員不應該嘗試使用伺服器虛擬化案例設定根排程器，以供使用。

## <a name="determining-the-current-scheduler-type"></a>判斷目前的排程器類型

您可以判斷目前的 hypervisor 排程器類型，藉由檢查系統記錄檔事件檢視器中的最新的 hypervisor 啟動事件識別碼為 2，它會報告在 hypervisor 啟動設定的 hypervisor 排程器類型的使用中。 從 Windows 事件檢視器中，或透過 PowerShell，則可以取得 Hypervisor 啟動事件。

Hypervisor 啟動事件識別碼為 2 表示 hypervisor 的排程器型別，其中：

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![螢幕擷取畫面顯示 hypervisor 啟動事件識別碼為 2 的詳細資料](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![顯示顯示 hypervisor 啟動事件識別碼為 2 的事件檢視器螢幕擷取畫面](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>查詢 HYPER-V hypervisor 排程器型別啟動事件使用 PowerShell

查詢 hypervisor 事件識別碼為 2 使用 PowerShell，從 PowerShell 提示字元輸入下列命令。

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![顯示 PowerShell 查詢和結果的 hypervisor 啟動 事件識別碼為 2 螢幕擷取畫面](media/Hyper-V-CoreScheduler-PowerShell.png)
