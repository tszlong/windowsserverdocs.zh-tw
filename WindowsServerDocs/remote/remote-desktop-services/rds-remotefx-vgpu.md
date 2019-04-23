---
title: RDS-RemoteFX vGPU 安裝和設定
description: 若要設定 RemoteFX vGPU 圖形虛擬化的規劃資訊。
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876839"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>設定遠端桌面服務的 RemoteFX vGPU


RemoteFX vGPU 功能可讓多個共用實體的圖形介面卡的虛擬機器。 虛擬機器就能夠卸載從處理器到專用的圖形介面卡的圖形資訊的呈現。 這會降低 CPU 負載，並改善圖形密集的工作負載在 VDI 虛擬機器中執行的延展性。 

## <a name="remotefx-vgpu-requirements"></a>RemoteFX vGPU 需求

適用於主機系統需求： 

- Windows Server 2016 或 Windows 10
- DX 11.0 相容 GPU，搭配 WDDM 1.2 相容的驅動程式 
- Windows Server rd 工作階段主機角色啟用 （啟用 HYPER-V 角色） 
- 配備支援 SLAT （第二層位址轉譯） 之 CPU 的伺服器 

客體 VM 的需求：

- 執行 Windows 的企業用戶端 (Windows 7 搭配 Service Pack 1、 Windows 8.1 的 Windows 10) 或 Windows Server （Windows Server 2012 R2 或 Windows Server 2016） 的客體 VM。 其他作業系統的支援，請參閱[for Remote Desktop Services 的支援組態](rds-supported-config.md)。

客體 Vm 的額外考量：

- OpenGL 和 OpenCL 功能僅供以 Windows 10 或 Windows Server 2016。  
- DirectX 11.0 才可使用 Windows 8 或更新版本的客體 Vm。 
- 如果它以執行遠端桌面工作階段主機僅支援 RemoteFX vGPU[個人的工作階段桌面](rds-personal-session-desktops.md)。

客體 VM，請務必檢閱[VDI 部署-支援的客體 Os](rds-supported-config.md#vdi-deployment--supported-guest-oss)。

## <a name="install-remotefx-vgpu"></a>安裝 RemoteFX vGPU

安裝和設定在主機上的 RemoteFX，適用於 Windows Server 2016 與 Windows 10 中使用下列步驟：

1. 安裝作業系統。
2. 安裝最新 Windows 10 和 Windows Server 2016 GPU 驅動程式可從圖形卡廠商網站。
3. 安裝 Windows 10 和 Windows Server 2016 主機上的 RemoteFX vGPU:
   1. 在 Windows 10 主機上，啟用 （移至控制台的 / 程式和功能/關閉 Windows 功能開啟或關閉） 控制台中的 HYPER-V 功能：

      ![若要啟用 HYPER-V 功能的 Windows 功能 視窗](media/rds-hyperv-settings.png)

   2. 在 Windows Server 2016 主機上，安裝遠端桌面虛擬化主機 (RDVH) 角色。
   

4. 現在，建立並設定客體 VM:
   1. 使用 Windows 10 企業版或 Windows Server 2016 中建立 VM。
   2. 新增 RemoteFX 3D 圖形介面卡。 請參閱[設定 RemoteFX vGPU 3D 卡](#configure-the-remotefx-vgpu-3d-adapter)如需如何執行該作業使用 HYPER-V 管理員或 PowerShell cmdlet。 

RemoteFX vGPU 有一個以上的可用時，會使用所有的 Gpu。 不過，在某些情況下，您可能想要限制哪些 Gpu 會使用 RemoteFX。 在 HYPER-V 環境中，您選取來控制這特別的 Gpu 應該*不*供 RemoteFX。 請使用下列步驟： 

   1. 瀏覽至 HYPER-V 管理員中的 HYPER-V 設定。
   2. 按一下 **實體的 Gpu**在 HYPER-V 設定。
   3. 選取您不想要使用此項目，然後清除 GPU**搭配 RemoteFX 使用此 GPU**。


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>設定 RemoteFX vGPU 3D 配接器
您可以使用 HYPER-V 管理員 UI 或 PowerShell cmdlet 來設定 RemoteFX vGPU 3D 圖形卡。 

#### <a name="through-hyper-v-manager"></a>透過 HYPER-V 管理員：

1. 請確定 hyper-v 已設定系統，以及設定 VM。  
2. 如果它正在執行，請停止 VM。 
3. 在 HYPER-V 管理員中瀏覽至**VM 設定**，然後按一下**新增硬體**。
4. 選取  **RemoteFX 3D 圖形卡**，然後按一下**新增**。 
5. 設定監視、 最大監視器解析度和專用視訊記憶體的最大數目，或保留預設值。

   > [!NOTE]
   > - 設定較高的值，讓這些選項不會有影響，若要調整，因此您應該只設定什麼是絕對必要。
   >
   > - 當您需要使用 1 GB 的專用 VRAM 時，使用 64 位元的客體 VM 而不是 32 位元 (x86) 為了獲得最佳結果。
6. 按一下 **確定**以完成設定。

#### <a name="with-powershell-cmdlets"></a>使用 PowerShell cmdlet:

執行下列 cmdlet 來新增、 檢閱及設定配接器： 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

如需詳細資訊，請參閱[Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter)。

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

如需詳細資訊，請參閱[Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter)

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

如需詳細資訊，請參閱[Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter)。

執行下列 cmdlet 來檢閱實體 Gpu:

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

如需詳細資訊，請參閱[Get-vmremotefxphysicalvideoadapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter)。

## <a name="monitor-performance"></a>監視效能

效能和規模的 VDI 系統是由各種不同的因素，例如 GPU 的總記憶體，系統記憶體與記憶體速度、 CPU 核心數目及 CPU 時脈頻率、 儲存體的速度和 NUMA 實作數量決定的。

Rds 的遠端 vGPU 支援包含下列效能計數器，您可以檢視效能監視器 (perfmon.exe) 來收集畫面格速率輸送量的相關資訊。

- RemoteFX 圖形-遠端桌面通訊協定圖形壓縮的計數器。 例如，如果您想要查看壓縮呈現給 RDP 的畫面格數目，看看**輸入 Frames/Second**計數器。
- RemoteFX 網路-遠端桌面通訊協定的網路流量的計數器。 例如，**來回時間 (RTT)**。
- RemoteFX 根 GPU 管理-VRAM 可用和保留的量值。
- RemoteFX 軟體-提供擷取速率、 GPU 回應時間，以及其他的計數器。

更低層級效能監視，特別是針對進行疑難排解，您可以使用下列的其他效能計數器：

- RemoteFX Synth3D VSC VM 裝置 
- RemoteFX Synth3D VSC VM 傳輸通道 
- RemoteFX Synth3D VSP 
- RemoteFX Synth3D VSP VM 裝置 
- RemoteFX Synth3D VSP VM 通道
