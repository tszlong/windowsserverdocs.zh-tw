---
title: 虛擬機器大小
description: 每個工作負載類型的大小建議。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/02/2019
ms.topic: article
author: Heidilohr
manager: lizross
ms.openlocfilehash: 1d6a7daa3966488c951117b083411587d13be56b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857101"
---
# <a name="virtual-machine-sizing-guidance"></a>虛擬機器大小指引

無論您是在遠端桌面服務或 Windows 虛擬桌面上執行虛擬機器，不同類型的工作負載需要不同的工作階段主機虛擬機器 (VM) 設定。 為了獲得最佳體驗，請根據您的使用者需求來調整部署。

## <a name="multi-session-recommendations"></a>多工作階段建議

下表列出每個虛擬中央處理器 (vCPU) 的建議使用者人數上限，以及每個工作負載的 VM 設定下限。 這些建議是以[遠端桌面工作負載](remote-desktop-workloads.md)為基礎。

| 工作負載類型 | 每個 vCPU 的使用者人數上限 | vCPU/RAM/OS 儲存空間下限 | Azure 執行個體範例 | 設定檔容器儲存空間下限 |
| --- | --- | --- | --- | --- |
| 輕量型 | 6 | 2 個 vCPU、8 GB RAM、16 GB 儲存空間 | D2s_v3、F2s_v2 | 30 GB |
| 中 | 4 | 4 個 vCPU、16 GB RAM、32 GB 儲存空間 | D4s_v3、F4s_v2 | 30 GB |
| 大量 | 2 | 4 個 vCPU、16 GB RAM、32 GB 儲存空間 | D4s_v3、F4s_v2 | 30 GB |
| 電源 | 1 | 6 個 vCPU、56 GB RAM、340 GB 儲存空間 | D4s_v3、F4s_v2、NV6 | 30 GB |

## <a name="single-session-recommendations"></a>單一工作階段建議

針對單一工作階段案例的 VM 大小調整建議，我們建議每個 VM 至少有兩個實體 CPU 核心 (通常是四個具有超執行緒的 vCPU)。 針對單一工作階段案例，如果您需要更具體的 VM 大小建議，請詢問您工作負載專屬的軟體廠商。 單一工作階段 VM 的 VM 大小調整可能會與實體裝置指導方針一致。

## <a name="general-virtual-machine-recommendations"></a>一般虛擬機器建議事項

如需執行作業系統的 VM 需求，請參閱 [Windows 10 電腦規格和系統需求](https://www.microsoft.com/windows/windows-10-specifications)。

針對需要服務等級協定 (SLA) 的生產環境工作負載，建議您在 OS 磁碟中使用進階 SSD 儲存體。 如需詳細資訊，請參閱[虛擬機器 SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/)。

對於經常使用圖形密集型程式進行影片轉譯、3D 設計和模擬的使用者而言，圖形處理器 (GPU) 通常是不錯的選擇。 如需深入了解圖形加速，請參閱[選擇您的圖形轉譯技術](rds-graphics-virtualization.md)。 Azure 具有數個圖形加速部署選項和多個可用的 GPU VM 大小。 深入了解 [GPU 最佳化虛擬機器大小](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu)。

對於不一定需要最大 CPU 效能的使用者而言，[B 系列高載 VM](https://docs.microsoft.com/azure/virtual-machines/windows/b-series-burstable) 是不錯的選擇。 如需 VM 類型和大小的詳細資訊，請參閱 [Azure 中的 Windows 虛擬機器大小](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)和[虛擬機器系列頁面](https://azure.microsoft.com/pricing/details/virtual-machines/series/)的定價資訊。

## <a name="test-your-workload"></a>測試您的工作負載

最後，我們建議您使用模擬工具進行壓力測試和實際使用方式的模擬來測試部署。 請確保您系統的回應能力和彈性足以滿足使用者需求，並記得改變負載大小以避免發生意外。
