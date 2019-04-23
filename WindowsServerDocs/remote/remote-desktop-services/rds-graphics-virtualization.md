---
title: RDS-哪一種圖形虛擬化技術適合您嗎？
description: 規劃可協助您選擇正確的圖形為您的 RDS 部署虛擬化選項的詳細資訊。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/16/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d6ff5b22-7695-4fee-b1bd-6c9dce5bd0e8
author: lizap
manager: scottman
ms.openlocfilehash: 7cf7fdf3510fcaaa955bd0031fb3564fe4372472
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875799"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>哪一種圖形虛擬化技術是最適合您？

啟用遠端桌面服務中的圖形轉譯時，您會有各種選項。 當您計劃您的虛擬環境時，下列考量磁碟機的圖形轉譯您選擇的技術：

![圖形轉譯考量-比較使用者規模和 GPU 的需求，以判斷最佳的 GPU 技術，為您的環境](media/rds-gpu.png)

在 Windows Server 2016 中，您會有兩個圖形的虛擬化技術可讓您利用 GPU 硬體的 HYPER-V 提供：

- [離散裝置指派 (DDA)](#discrete-device-assignment) -針對最高的效能，使用一或多個 Gpu 專門用來提供原生的 GPU 驅動程式支援，在 VM 內的 VM。 密度是低，因為它會受到實體伺服器中可用的 Gpu 的數目。 
- [遠端 FX vGPU](#remotefx-vgpu) -適用於知識工作者和多個 Vm，利用透過並行虛擬化的一或多個 Gpu 的高高載 GPU 案例。 此解決方案會提供每一部伺服器的較高使用者密度。

下圖顯示 Windows Server 2016 中的圖形虛擬化選項。

![圖形與 RDS-Windows Server 2016 中的虛擬化選項會顯示三個可用的技術和它們之間的差異在規模和效能](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>不同的裝置指派
離散裝置指派 (DDA) 會為硬體的傳遞解決方案，提供最佳效能，假設 VM 可完整存取 GPU 使用原生的驅動程式。 您 VM 的使用者可以存取其裝置的完整功能，也是裝置的原生的驅動程式。 這表示 VM 鏡像裸機上執行相同的裝置中執行裝置的功能。

如需有關 DDA 的詳細資訊，請參閱[規劃的部署不連續的裝置指派](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。

## <a name="remotefx-vgpu"></a>RemoteFX vGPU 
RemoteFX vGPU 是一種圖形虛擬化技術，可讓分散於客體作業系統以啟用 （請參閱上述的第一個圖形） 的知識工作者案例 GPU 的處理能力。 Windows Server 2016 中的已大幅提升，允許進一步的增強功能的 GPU 高載的情況下，例如設計工具的應用程式和資料視覺效果。 其他功能改良，包括：

-   支援第 2 代客體 Vm、 Windows Server 2016 客體 Vm 和 Windows 用戶端 HYPER-V 主機。
   >[!NOTE] 
   > Windows Server 2016 客體 VM; 不支援遠端桌面工作階段主機1 個工作階段可以裝載每個 Windows Server 2016 客體 VM。

-   改進的應用程式相容性和穩定性。
-   VM 連線增強的工作階段模式，允許透過 VM 連線至 VM 已啟用 RemoteFX vGPU USB 和剪貼簿的重新導向。

如需詳細資訊，請參閱[設定安裝及設定遠端桌面服務的 RemoteFX vGPU](rds-remotefx-vgpu.md)。

## <a name="which-should-you-use"></a>您應該使用？

哪些虛擬化技術可能取決於您的環境中的應用程式需求的硬體規格的重要考量。 以下是簡短資料表有關 DDA 和 RemoteFX vGPU 功能：

| 功能               | RemoteFX vGPU                                                                       | 不同的裝置指派                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| 裝置 GPU 指派 | 並行虛擬化 (許多 Vm 至一或多個 Gpu)                                     | 1 個以上的 GPU，以 1 個 VM                                                  |
| 縮放比例                 | 最佳規模 / 1 個 GPU 許多 Vm                                                      | 低比例 / 1 個 VM 1 個或多個 Gpu                                     |
| 應用程式相容性     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | (第 12 DX OpenGL，CUDA) 廠商所提供的所有 GPU 功能          |
| AVC444                | 啟用預設 （Windows 10 和 Windows Server 2016）                             | 可透過群組原則 （Windows 10 和 Windows Server 2016）    |
| GPU VRAM              | 最多 1 GB 專用 VRAM                                                           | 最多支援 GPU 的 VRAM                                        |
| 畫面播放速率            | 最多 30 fps                                                                         | 最多 60 fps                                                            |
| 客體中的 GPU 驅動程式   | RemoteFX 3D 介面卡顯示驅動程式 (Microsoft)                                      | GPU 廠商驅動程式 (Nvidia，AMD、 Intel)                                 |
| 客體 OS 支援      |  Windows Server 2012 R2  Windows Server 2016  Windows 7 SP1  Windows 8.1 Windows 10 |  Windows Server 2012 R2  Windows Server 2016  Windows 10 Linux         |
| hypervisor            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| 主機作業系統的可用性  |  Windows Server 2012 R2  Windows Server 2016 Windows 10                             | Windows Server 2016                                                    |
| GPU 硬體          | 企業 Gpu （例如 Nvidia Quadro/方格或 AMD FirePro）                         | 企業 Gpu （例如 Nvidia Quadro/方格或 AMD FirePro）            |
| 伺服器硬體       | 沒有特殊需求                                                             | 最新的伺服器，會公開 IOMMU 作業系統 （通常 SR-IOV 相容硬體） |

一般經驗法則是使用 DDA 最佳的應用程式相容性，因為虛擬機器都具有 GPU 的直接存取。 如果您的應用程式或工作負載不需要為 GPU 的嚴格要求，而且您想要伺服器的使用者更多基底，RemoteFX vGPU 可能最適合您。