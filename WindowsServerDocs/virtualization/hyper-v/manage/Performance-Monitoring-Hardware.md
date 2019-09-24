---
title: 將 Intel 效能監視硬體公開給 Hyper-v 虛擬機器
description: 描述如何向 Hyper-v 機器公開 Intel 的效能 Monitorning 硬體。 同時也要瞭解啟用如何影響即時移轉。
ms.prod: windows-server-threshold
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 1bc821cc46a3a402778e1c76f4a2d8feb862fd75
ms.sourcegitcommit: 45415ba58907d650cfda45f4c57f6ddf1255dcbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213614"
---
# <a name="exposing-intel-performance-monitoring-hardware-to-a-hyper-v-virtual-machine"></a>將 Intel 效能監視硬體公開給 Hyper-v 虛擬機器
 
## <a name="background"></a>背景
Intel 處理器包含統稱為效能監視硬體的功能（例如 PMU、PEBS、LBR）。 這些功能可供效能調整軟體（例如 Intel VTune 放大器）用來分析軟體效能。  在 Windows Server 2019 和 Windows 10 版本1809之前，主機作業系統或 Hyper-v 來賓虛擬機器都無法在啟用 Hyper-v 時使用效能監視硬體。  從 Windows Server 2019 和 Windows 10 版本1809開始，主機作業系統預設具有效能監視硬體的存取權。  Hyper-v 來賓虛擬機器預設沒有存取權，但 Hyper-v 系統管理員可以選擇授與一或多部來賓虛擬機器的存取權。  本檔說明將效能監視硬體公開給來賓虛擬機器所需的步驟。
 
## <a name="requirements"></a>需求 
若要將效能監視硬體公開到虛擬機器，您需要：
- 具有效能監控硬體的 Intel 處理器（亦即 PMU、PEBS、IPT）
- Windows Server 2019 或 Windows 10 版本1809（10月2018更新）或更新版本
- _沒有_已停止狀態之[嵌套虛擬化](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)的 hyper-v 虛擬機器
 
## <a name="exposing-the-pmu-capabilities-pmu-lbr-pebs-to-virtual-machines-via-powershells-set-vmprocessor-cmdlet"></a>透過 Powershell 的 Set-VMProcessor Cmdlet，將 PMU 功能（PMU、LBR、PEBS）公開至虛擬機器
若要為特定來賓虛擬機器啟用不同的效能監視元件，請`Set-VMProcessor`使用 PowerShell Cmdlet：
 
``` Powershell
# Enable all components
Set-VMProcessor MyVMName -Perfmon @("pmu", "lbr", "pebs")
```
 
``` Powershell
# Enable a specific component
Set-VMProcessor MyVMName -Perfmon @("pmu")
```
 
``` Powershell
# Disable all components
Set-VMProcessor MyVMName -Perfmon @()
```
 
>注意：啟用 perfmon 元件時，如果`"pebs"`指定，則`"pmu"`必須指定。  此外，若要啟用不受主機實體處理器支援的 perfmon 元件，將導致虛擬機器啟動失敗。
 
## <a name="effect-of-pmu-enabelement-on-saverestore-export-and-live-migration"></a>PMU enabelement 對儲存/還原、匯出和即時移轉的影響
 
Microsoft 不建議在具有不同 Intel 硬體的系統之間，即時移轉或儲存/還原具有效能監控硬體的虛擬機器。 效能監控硬體的特定行為通常不會在 Intel 硬體系統之間進行架構和變更。  在不同的系統之間移動執行中的虛擬機器，可能會導致非架構計數器的無法預期行為。