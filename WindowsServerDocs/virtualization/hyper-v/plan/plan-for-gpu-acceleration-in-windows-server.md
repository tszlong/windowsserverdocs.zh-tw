---
title: 在 Windows Server 中規劃 GPU 加速
description: 瞭解 GPU 加速的不同 Hyper-v 技術，包括 DDA 和 RemoteFX vGPU
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 07/14/2020
ms.openlocfilehash: 8cba3ac4d2e4680f480ff76db12c10553c1857d3
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996962"
---
# <a name="plan-for-gpu-acceleration-in-windows-server"></a>在 Windows Server 中規劃 GPU 加速

> 適用于： Windows Server 2016、Microsoft Hyper-v Server 2016、Windows Server 2019、Microsoft Hyper-v Server 2019

本文介紹 Windows Server 提供的圖形虛擬化功能。

## <a name="when-to-use-gpu-acceleration"></a>使用 GPU 加速的時機

視您的工作負載而定，您可能會想要考慮 GPU 加速。 在選擇 GPU 加速之前，您應該考慮下列事項：

- **應用程式和桌面遠端 (VDI/DaaS) 工作負載**：如果您要使用 Windows Server 建立應用程式或桌面遠端服務，請考慮您希望使用者執行的應用程式目錄。 某些類型的應用程式（例如 CAD/CAM 應用程式、模擬應用程式、遊戲和轉譯/視覺效果應用程式）高度依賴3D 轉譯，以提供順暢且回應性的互動性。 大部分的客戶都會考慮使用 Gpu，以取得這類應用程式的合理使用者體驗。
- **遠端轉譯、編碼和視覺效果工作負載**：這些圖形導向的工作負載通常會依賴 GPU 的特定功能，例如有效率的3d 轉譯和框架編碼/解碼，以達到符合成本效益和輸送量目標的目的。 針對這種類型的工作負載，單一已啟用 GPU 的 VM 可能會比對許多僅限 CPU 的 Vm 的輸送量。
- **HPC 和 ML 工作負載**：對於高度資料平行計算工作負載（例如高效能計算和機器學習模型訓練或推斷），gpu 可以大幅縮短產生的時間、推斷時間，以及定型時間。 或者，它們可能比僅限 CPU 的架構在可比較的效能層級提供更佳的成本效益。 許多 HPC 和機器學習架構都有選項可啟用 GPU 加速;請考慮這是否可能會對您的特定工作負載有好處。

## <a name="gpu-virtualization-in-windows-server"></a>Windows Server 中的 GPU 虛擬化

GPU 虛擬化技術可在虛擬化環境中啟用 GPU 加速，通常是在虛擬機器內。 如果您的工作負載是以 Hyper-v 進行虛擬化，則您必須採用圖形虛擬化，才能從實體 GPU 將 GPU 加速提供給虛擬化的應用程式或服務。 不過，如果您的工作負載直接在實體 Windows Server 主機上執行，則不需要圖形虛擬化;您的應用程式和服務已可存取 Windows Server 原本就支援的 GPU 功能和 Api。

下列圖形虛擬化技術適用于 Windows Server 中的 Hyper-v Vm：

- [ (DDA) 的離散裝置指派](#discrete-device-assignment-dda)
- [RemoteFX vGPU](#remotefx-vgpu)

除了 VM 工作負載，Windows Server 也支援 Windows 容器中容器化工作負載的 GPU 加速。 如需詳細資訊，請參閱[Windows 容器中的 GPU 加速](/virtualization/windowscontainers/deploy-containers/gpu-acceleration)。

## <a name="discrete-device-assignment-dda"></a> (DDA) 的離散裝置指派

離散裝置指派 (DDA) （也稱為 GPU 傳遞）可讓您將一或多個實體 Gpu 專用於虛擬機器。 在 DDA 部署中，虛擬化工作負載會在原生驅動程式上執行，而且通常會擁有 GPU 功能的完整存取權。 DDA 提供最高層級的應用程式相容性和潛在的效能。 DDA 也可以提供 GPU 加速給 Linux Vm，受限於支援。

因為每個實體 GPU 最多可以為一部 VM 提供加速，所以一次部署的虛擬機器只可加速數量有限。 如果您正在開發架構支援共用虛擬機器的服務，請考慮為每個 VM 裝載多個加速的工作負載。 例如，如果您要使用 RDS 建立桌面遠端服務，您可以利用 Windows Server 的多會話功能，在每部 VM 上裝載多個使用者桌面，來改善使用者規模。 這些使用者會分享 GPU 加速的優點。

如需詳細資訊，請參閱下列主題：

- [規劃部署離散裝置指派](plan-for-deploying-devices-using-discrete-device-assignment.md)
- [使用離散裝置指派來部署圖形裝置](../deploy/Deploying-graphics-devices-using-dda.md)

## <a name="remotefx-vgpu"></a>RemoteFX vGPU

> [!NOTE]
> 基於安全性考量，自 2020 年 7 月 14 日的安全性更新開始，預設會停用所有 Windows 版本上的 RemoteFX vGPU。 若要深入了解，請參閱 [KB 4570006](https://support.microsoft.com/help/4570006)。

RemoteFX vGPU 是一種圖形虛擬化技術，可讓單一實體 GPU 在多部虛擬機器之間共用。 在 RemoteFX vGPU 部署中，虛擬化工作負載會在 Microsoft 的 RemoteFX 3D 介面卡上執行，這會協調主機和來賓之間的 GPU 處理要求。 RemoteFX vGPU 最適合知識工作者和高載的工作負載，而不需要專用的 GPU 資源。 RemoteFX vGPU 只能提供 GPU 加速給 Windows Vm。

如需詳細資訊，請參閱下列主題：

- [使用 RemoteFX vGPU 部署圖形裝置](../deploy/deploy-graphics-devices-using-remotefx-vgpu.md)
- [RemoteFX 3D 視訊卡 (vGPU) 支援](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support)

## <a name="comparing-dda-and-remotefx-vgpu"></a>比較 DDA 和 RemoteFX vGPU

規劃您的部署時，請考慮下列功能並支援圖形虛擬化技術之間的差異：

| 描述 | RemoteFX vGPU | 離散裝置指派 |
|--|--|--|
| GPU 資源模型 | 專用或共用 | 僅限專用 |
| VM 密度 | 高 (一或多個 Gpu 到多個 Vm)  | 低 (一或多個 Gpu 到一個 VM)  |
| 應用程式相容性 | DX 11.1、OpenGL 4.4、OpenCL 1.1 | 廠商提供的所有 GPU 功能 (DX 12、OpenGL、CUDA) |
| AVC444 | 預設已啟用 | 可透過群組原則 |
| GPU VRAM | 最多 1 GB 專用 VRAM | 最多 GPU 支援的 VRAM |
| 畫面播放速率 | 最多 30 fps | 最多 60 fps |
| 客體中的 GPU 驅動程式 | RemoteFX 3D 介面卡顯示器驅動程式 (Microsoft) |  (NVIDIA、AMD、Intel) 的 GPU 廠商驅動程式 |
| 主機 OS 支援 | Windows Server 2016 | Windows Server 2016;Windows Server 2019 |
| 客體 OS 支援 | Windows Server 2012 R2;Windows Server 2016;Windows 7 SP1;Windows 8.1;Windows 10 | Windows Server 2012 R2;Windows Server 2016;Windows Server 2019;Windows 10;廠商 |
| Hypervisor | Microsoft Hyper-V | Microsoft Hyper-V |
| GPU 硬體 | 企業 GPU (例如 Nvidia Quadro/GRID 或 AMD FirePro) | 企業 GPU (例如 Nvidia Quadro/GRID 或 AMD FirePro) |
| 伺服器硬體 | 沒有特殊需求 | 新式伺服器，會將 IOMMU 公開至 OS (通常為 SR-IOV 相容硬體) |