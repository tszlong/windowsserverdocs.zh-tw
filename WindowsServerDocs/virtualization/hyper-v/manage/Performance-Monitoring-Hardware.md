---
title: 啟用 Hyper-v 虛擬機器中的 Intel 效能監視硬體
description: 如何在 Hyper-v 機器中啟用 Intel 的效能監視硬體。 另請參閱如何讓效能監視硬體影響即時移轉。
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 73ab88d6ee5d50ae8b433a064a7cea7c249094a2
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996743"
---
# <a name="enable-intel-performance-monitoring-hardware-in-a-hyper-v-virtual-machine"></a>啟用 Hyper-v 虛擬機器中的 Intel 效能監視硬體

Intel 處理器包含統稱為效能監視硬體 (的功能，例如 PMU、PEBS、LBR) 。 這些功能可供效能調整軟體（例如 Intel VTune 放大器）用來分析軟體效能。  在 Windows Server 2019 和 Windows 10 版本1809之前，主機作業系統或 Hyper-v 來賓虛擬機器都無法在啟用 Hyper-v 時使用效能監視硬體。  從 Windows Server 2019 和 Windows 10 版本1809開始，主機作業系統預設具有效能監視硬體的存取權。  Hyper-v 來賓虛擬機器預設沒有存取權，但 Hyper-v 系統管理員可以選擇授與一或多部來賓虛擬機器的存取權。  本檔說明將效能監視硬體公開給來賓虛擬機器所需的步驟。

## <a name="requirements"></a>需求

若要在虛擬機器中啟用效能監視硬體，您需要：

- 具有效能監控硬體的 Intel 處理器， (PMU、PEBS、LBR) 。  請參閱 Intel 的[這份檔]( https://software.intel.com/en-us/vtune-amplifier-cookbook-configuring-a-hyper-v-virtual-machine-for-hardware-based-hotspots-analysis)，以判斷您的系統支援哪些效能監視硬體。
- Windows Server 2019 或 Windows 10 1809 版 (10 月 2018 Update) 或更新版本
- _沒有_[嵌套虛擬化](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)也處於已停止狀態的 hyper-v 虛擬機器

若要啟用近期的 Intel 處理器追蹤 (IPT 虛擬機器中) 效能監視硬體，您需要：

- 支援 IPT 和 PT2GPA 功能的 Intel 處理器。  請參閱 Intel 的[這份檔]( https://software.intel.com/en-us/vtune-amplifier-cookbook-configuring-a-hyper-v-virtual-machine-for-hardware-based-hotspots-analysis)，以判斷您的系統支援哪些效能監視硬體。
- Windows Server 1903 版 (SAC) 或 Windows 10 版本 1903 (可能會2019更新) 或更新版本
- _沒有_[嵌套虛擬化](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)也處於已停止狀態的 hyper-v 虛擬機器

## <a name="enabling-performance-monitoring-components-in-a-virtual-machine"></a>在虛擬機器中啟用效能監視元件

若要為特定來賓虛擬機器啟用不同的效能監視元件，請在 `Set-VMProcessor` 以系統管理員身分執行時使用 PowerShell Cmdlet：

``` Powershell
# Enable all components except IPT
Set-VMProcessor MyVMName -Perfmon @("pmu", "lbr", "pebs")
```

``` Powershell
# Enable a specific component
Set-VMProcessor MyVMName -Perfmon @("pmu")
```

``` Powershell
# Enable IPT
Set-VMProcessor MyVMName -Perfmon @("ipt")
```

``` Powershell
# Disable all components
Set-VMProcessor MyVMName -Perfmon @()
```
> [!NOTE]
> 啟用效能監視元件時，如果 `"pebs"` 指定，則 `"pmu"` 也必須指定。
> 只有 PMU 版本 >= 4 的硬體才支援 PEBS。
> 啟用不受主機實體處理器支援的元件會導致虛擬機器啟動失敗。

## <a name="effects-of-enabling-performance-monitoring-hardware-on-saverestore-export-and-live-migration"></a>在儲存/還原、匯出和即時移轉時啟用效能監視硬體的效果

Microsoft 不建議在具有不同 Intel 硬體的系統之間，即時移轉或儲存/還原具有效能監控硬體的虛擬機器。 效能監控硬體的特定行為通常不會在 Intel 硬體系統之間進行架構和變更。  在不同的系統之間移動執行中的虛擬機器，可能會導致非架構計數器的無法預期行為。