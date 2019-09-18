---
title: RDS - 哪一種圖形虛擬化技術適合您？
description: 可協助您為 RDS 部署選擇正確圖形虛擬化選項的規劃資訊。
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
ms.openlocfilehash: ce10575d38bccc0b22dadf55bd89156c6ce5ea7b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871054"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>哪一種圖形虛擬化技術適合您？

在遠端桌面服務中啟用圖形轉譯時，您會有各式各樣的選項。 當您規劃虛擬環境時，下列考量可促使您選擇某種圖形轉譯技術：

![圖形轉譯考量：比較使用者規模和 GPU 需求，以便為您的環境判斷最佳的 GPU 技術](media/rds-gpu.png)

在 Windows Server 2016 中，您可以使用 Hyper-V 來提供兩種圖形虛擬化技術，讓您能夠利用 GPU 硬體：

- [離散裝置指派 (DDA)](#discrete-device-assignment)：使用一或多個專門用於 VM 的 GPU，在 VM 內提供原生的 GPU 驅動程式支援，以獲得最佳效能。 密度很低，因為它受限於伺服器中可用的實體 GPU 數目。 
- [遠端 FX vGPU](#remotefx-vgpu)：適用於知識工作者和高載 GPU 案例，其中有多個 VM 會透過半虛擬化來利用一或多個 GPU。 此解決方案會為每部伺服器提供較高的使用者密度。

下圖顯示 Windows Server 2016 中的圖形虛擬化選項。

![Windows Server 2016 中使用 RDS 的圖形虛擬化選項：顯示三種可用技術及其在規模和效能之間的差異](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>離散裝置指派
離散裝置指派 (DDA) 是可提供最佳效能的硬體傳遞解決方案，前提是 VM 可以使用原生的驅動程式完整存取 GPU。 您的 VM 使用者可以存取其裝置的完整功能，以及該裝置的原生驅動程式。 這表示在 VM 中執行裝置的特性與功能會鏡像處理在裸機上執行相同的裝置。

如需 DDA 的詳細資訊，請參閱[規劃部署離散裝置指派](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md)。

## <a name="remotefx-vgpu"></a>RemoteFX vGPU 
RemoteFX vGPU 是一種圖形虛擬化技術，可將 GPU 的處理能力分割到各種客體作業系統上，以啟用知識工作者案例 (請參閱上述第一個圖形)。 Windows Server 2016 中的進展能夠進一步增強GPU 高載案例，例如，設計工具應用程式和資料視覺效果。 其他功能改進包括：

- 支援第 2 代客體 VM、Windows Server 2016 客體 VM 和 Windows 用戶端 Hyper-V 主機。
  >[!NOTE] 
  > Windows Server 2016 客體 VM 上不支援遠端桌面工作階段主機；每個 Windows Server 2016 客體 VM 只能裝載 1 個工作階段。

- 改進的應用程式相容性和穩定性。
- VM 連線增強的工作階段模式，允許 USB 和剪貼簿透過 VM 連線重新導向至已針對 RemoteFX vGPU 啟用的 VM。

如需詳細資訊，請參閱[設定遠端桌面服務的 RemoteFX vGPU](rds-remotefx-vgpu.md)。

## <a name="which-should-you-use"></a>您應該使用哪一種？

關於哪種虛擬化技術的重要考量可能取決於您環境中的硬體規格或應用程式需求。 以下是有關 DDA 和 RemoteFX vGPU 功能的簡易表格：

| 功能               | RemoteFX vGPU                                                                       | 離散裝置指派                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| 裝置 GPU 指派 | 半虛擬化 (數個 VM 到一個或多個 GPU)                                     | 1 個或多個 GPU 到 1 個 VM                                                  |
| 縮放比例                 | 最佳規模 / 1 個 GPU 到數個 VM                                                      | 小規模 / 1 個或多個 GPU 到 1 個 VM                                     |
| 應用程式相容性     | DX 11.1、OpenGL 4.4、OpenCL 1.1                                                     | 廠商提供的所有 GPU 功能 (DX 12、OpenGL、CUDA)          |
| AVC444                | 預設已啟用 (Windows 10 和 Windows Server 2016)                             | 可透過群組原則使用 (Windows 10 和 Windows Server 2016)    |
| GPU VRAM              | 最多 1 GB 專用 VRAM                                                           | 最多 GPU 支援的 VRAM                                        |
| 畫面播放速率            | 最多 30 fps                                                                         | 最多 60 fps                                                            |
| 客體中的 GPU 驅動程式   | RemoteFX 3D 介面卡顯示器驅動程式 (Microsoft)                                      | GPU 廠商驅動程式 (Nvidia、AMD、Intel)                                 |
| 客體 OS 支援      |  Windows Server 2012 R2  Windows Server 2016  Windows 7 SP1  Windows 8.1 Windows 10 |  Windows Server 2012 R2  Windows Server 2016  Windows 10 Linux         |
| Hypervisor            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| 主機 OS 高可用性  |  Windows Server 2012 R2  Windows Server 2016 Windows 10                             | Windows Server 2016                                                    |
| GPU 硬體          | 企業 GPU (例如 Nvidia Quadro/GRID 或 AMD FirePro)                         | 企業 GPU (例如 Nvidia Quadro/GRID 或 AMD FirePro)            |
| 伺服器硬體       | 沒有特殊需求                                                             | 新式伺服器，會將 IOMMU 公開至 OS (通常為 SR-IOV 相容硬體) |

一般經驗法則是使用 DDA 來獲取最佳應用程式相容性，因為虛擬機器將可直接存取 GPU。 如果您的應用程式或工作負載沒有嚴格的 GPU 要求，而且您想要服務更廣泛的使用者群，則 RemoteFX vGPU 可能最適合您。