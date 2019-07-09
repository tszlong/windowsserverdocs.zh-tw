---
title: RDS - RemoteFX vGPU 安裝和設定
description: 設定 RemoteFX vGPU 圖形虛擬化的規劃資訊。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/23/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0263fa6b-2185-4cc3-99ef-3588e2f4ada5
author: lizap
manager: scottman
ms.openlocfilehash: 3e7da1a70826dc720a96ceb3fe5d04868943f163
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712121"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>安裝和設定遠端桌面服務的 RemoteFX vGPU


RemoteFX 的 vGPU 功能可讓多部虛擬機器共用一張實體圖形介面卡。 虛擬機器可以將圖形資訊的呈現從處理器卸載到專用的圖形介面卡。 這會降低 CPU 負載，並提高在 VDI 虛擬機器中執行之圖形密集工作負載的延展性。 

## <a name="remotefx-vgpu-requirements"></a>RemoteFX vGPU 需求

主機系統的需求： 

- Windows Sever 2016 或 Windows 10
- DX 11.0 相容的 GPU，搭配 WDDM 1.2 相容的驅動程式 
- 已啟用 Windows Server RD 虛擬化主機角色 (啟用 HYPER-V 角色) 
- 含支援 SLAT (第二層位址轉譯) 之 CPU 的伺服器 

客體 VM 需求：

- 執行 Windows 企業版用戶端 (Windows 7 Service Pack 1、Windows 8.1、Windows 10) 或 Windows Server (Windows Server 2012 R2 或 Windows Server 2016) 的客體 VM。 如需了解其他作業系統支援，請參閱[遠端桌面服務支援的設定](rds-supported-config.md)。

客體 VM 的其他考量事項：

- OpenGL 和 OpenCL 功能僅能適用於 Windows 10 或 Windows Server 2016。  
- DirectX 11.0 僅適用於 Windows 8 或更新版本的客體 VM。 
- 只有在 RemoteFX vGPU 當作[個人的工作階段桌面](rds-personal-session-desktops.md)執行時，才支援遠端桌面工作階段主機。

若是客體 VM，請務必檢閱 [VDI 部署 - 支援的客體作業系統](rds-supported-config.md#vdi-deployment--supported-guest-oss)。

## <a name="install-remotefx-vgpu"></a>安裝 RemoteFX vGPU

使用下列步驟，在 Windows Server 2016 與 Windows 10 的主機上安裝和設定 RemoteFX：

1. 安裝作業系統。
2. 安裝可從圖形卡廠商網站取得的最新 Windows 10/Windows Server 2016 GPU 驅動程式。
3. 在 Windows 10/Windows Server 2016 主機上安裝 RemoteFX vGPU：
   1. 在 Windows 10 主機的 [控制台] 中，啟用 HYPER-V 功能 (移至 [控制台]/[程式和功能]/[開啟或關閉 Windows 功能])：

      ![啟用 Hyper-V 功能的 [Windows 功能] 視窗](media/rds-hyperv-settings.png)

   2. 在 Windows Server 2016 主機上，安裝遠端桌面虛擬化主機 (RDVH) 角色。
   

4. 現在，建立並設定客體 VM：
   1. 使用 Windows 10 企業版或 Windows Server 2016 建立 VM。
   2. 新增 RemoteFX 3D 圖形介面卡。 如需有關如何使用 Hyper-V 管理員或 PowerShell Cmdlet 執行該作業的資訊，請參閱[設定 RemoteFX vGPU 3D 介面卡](#configure-the-remotefx-vgpu-3d-adapter)。 

有多個 GPU 可用時，RemoteFX vGPU 將會使用所有 GPU。 不過，在某些情況下，您可以限制 RemoteFX 所使用的 GPU。 在 Hyper-V 環境中，您可以專門選取 RemoteFX *不*應該使用的 GPU 來控制這個情況。 請使用下列步驟： 

   1. 瀏覽至 Hyper-V 管理員中的 Hyper-V 設定。
   2. 按一下 [Hyper-V 設定] 中的 [實體 GPU]  。
   3. 選取您不想要使用的 GPU，然後清除 [使用含有 RemoteFX 的這個 GPU]  。


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>設定 RemoteFX vGPU 3D 介面卡
您可以使用 Hyper-V 管理員 UI 或 PowerShell Cmdlet 來設定 RemoteFX vGPU 3D 圖形介面卡。 

#### <a name="through-hyper-v-manager"></a>透過 Hyper-V 管理員：

1. 請確定已使用 Hyper-V 設定系統，而且已經設定 VM。  
2. 如果 VM 正在執行，請停止它。 
3. 在 Hyper-V 管理員中，瀏覽至 [VM 設定]  ，然後按一下 [新增硬體]  。
4. 選取 [RemoteFX 3D 圖形介面卡]  ，然後按一下 [新增]  。 
5. 設定監視器數目上限、最大監視器解析度，以及專用視訊記憶體，或保留預設值。

   > [!NOTE]
   > - 為這些選項中的任何一個選項設定更高的值都會對比例造成影響，因此您應該只設定絕對必要的值。
   >
   > - 當您需要使用 1GB 的專用 VRAM 時，請使用 64 位元的客體 VM 而不是 32 位元 (x86) 的客體 VM 以獲得最佳效果。
6. 按一下 [確定]  以完成設定。

#### <a name="with-powershell-cmdlets"></a>利用 PowerShell Cmdlet：

執行下列 Cmdlet 來新增、檢閱及設定介面卡： 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

如需詳細資訊，請參閱 [Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter)。

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

如需詳細資訊，請參閱 [Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter)。

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

如需詳細資訊，請參閱 [Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter)。

執行下列 Cmdlet 來檢閱實體 GPU：

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

如需詳細資訊，請參閱 [Get-VMRemoteFXPhysicalVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter)。

## <a name="monitor-performance"></a>監視器效能

VDI 系統的效能和規模取決於各種不同的因素，例如 GPU 的記憶體總數、系統記憶體數量與記憶體速度、CPU 核心數目及 CPU 時脈頻率、儲存速度，以及 NUMA 實作。

RDS 中的遠端 vGPU 支援包含下列效能計數器，您可以在效能監視器 (perfmon.exe) 中檢視這些效能計數器來收集畫面播放速率輸送量的相關資訊。

- RemoteFX 圖形 - 遠端桌面通訊協定圖形壓縮的計數器。 例如，如果您想要查看呈現給 RDP 的畫面數目以進行壓縮，請查看 [每秒輸入畫面]  計數器。
- RemoteFX 網路 - 用於遠端桌面通訊協定網路流量的計數器。 例如，**來回時間 (RTT)** 。
- RemoteFX 根 GPU 管理 - 測量可用的 VRAM 和保留的 VRAM。
- RemoteFX 軟體 - 提供擷取速率、GPU 回應時間等等的計數器。

針對更低層級效能監視，特別是針對疑難排解，您可以使用下列其他效能計數器：

- RemoteFX Synth3D VSC VM 裝置 
- RemoteFX Synth3D VSC VM 傳輸通道 
- RemoteFX Synth3D VSP 
- RemoteFX Synth3D VSP VM 裝置 
- RemoteFX Synth3D VSP VM 通道
