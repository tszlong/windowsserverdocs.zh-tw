---
title: 使用 RemoteFX vGPU 部署圖形裝置
description: 瞭解如何在 Windows Server 中部署和設定 RemoteFX vGPU
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 07/14/2020
ms.openlocfilehash: b92e1fa6906298b3f78476d105f963d6f51f6b82
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866147"
---
# <a name="deploy-graphics-devices-using-remotefx-vgpu"></a>使用 RemoteFX vGPU 部署圖形裝置

> 適用于： Windows Server 2016、Microsoft Hyper-V Server 2016

> [!NOTE]
> 基於安全性考量，自 2020 年 7 月 14 日的安全性更新開始，預設會停用所有 Windows 版本上的 RemoteFX vGPU。 若要深入了解，請參閱 [KB 4570006](https://support.microsoft.com/help/4570006)。

適用于 RemoteFX 的 vGPU 功能讓多部虛擬機器可以共用實體 GPU。 轉譯和計算資源會在虛擬機器之間動態共用，使 RemoteFX vGPU 適用于不需要專用 GPU 資源的高高載工作負載。 例如，在 VDI 服務中，RemoteFX vGPU 可用來將應用程式轉譯成本卸載至 GPU，其效果是減少 CPU 負載，並改善服務的調整能力。

## <a name="remotefx-vgpu-requirements"></a>RemoteFX vGPU 需求

主機系統需求：

- Windows Server 2016
- 具有 WDDM 1.2 相容驅動程式的 DirectX 11.0 相容 GPU
- 具有第二層位址轉譯 (SLAT) 支援的 CPU

客體 VM 需求：

- 支援的客體作業系統。 如需詳細資訊，請參閱 [RemoteFX 3D 視訊卡 (vGPU) 支援](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support)。

客體 VM 的其他考量事項：

- OpenGL 和 OpenCL 功能僅適用于執行 Windows 10 或 Windows Server 2016 的來賓。
- DirectX 11.0 僅適用于執行 Windows 8 或更新版本的來賓。

## <a name="enable-remotefx-vgpu"></a>啟用 RemoteFX vGPU

若要在您的 Windows Server 2016 主機上設定 RemoteFX vGPU：

1. 安裝適用于 Windows Server 2016 的 GPU 廠商所建議的圖形驅動程式。
2. 建立執行 RemoteFX vGPU 所支援之來賓 OS 的 VM。 若要深入瞭解，請參閱 [RemoteFX 3D 視訊卡 (vGPU) 支援](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support)。
3. 將 RemoteFX 3D 圖形介面卡新增至 VM。 若要深入瞭解，請參閱 [設定 RemoteFX VGPU 3d 介面卡](#configure-the-remotefx-vgpu-3d-adapter)。

根據預設，RemoteFX vGPU 將會使用所有可用和支援的 Gpu。 若要限制 RemoteFX vGPU 使用的 Gpu，請遵循下列步驟：

1. 瀏覽至 Hyper-V 管理員中的 Hyper-V 設定。
2. 在 [Hyper-v 設定] 中選取 **實體 gpu** 。
3. 選取您不想要使用的 GPU，然後清除 [使用含有 RemoteFX 的這個 GPU]。

### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>設定 RemoteFX vGPU 3D 介面卡

您可以使用 Hyper-V 管理員 UI 或 PowerShell Cmdlet 來設定 RemoteFX vGPU 3D 圖形介面卡。

#### <a name="configure-remotefx-vgpu-with-hyper-v-manager"></a>使用 Hyper-v 管理員設定 RemoteFX vGPU

1. 如果 VM 正在執行，請將它停止。
2. 開啟 [Hyper-v 管理員]，流覽至 [ **VM 設定**]，然後選取 [ **新增硬體**]。
3. 選取 [ **RemoteFX 3D 圖形介面卡**]，然後選取 [ **新增**]。
4. 設定監視器數目上限、最大監視器解析度，以及專用視訊記憶體，或保留預設值。

   > [!NOTE]
   > - 為這些選項設定較高的值將會影響您的服務規模，因此您應該只設定必要的設定。
   > - 當您需要使用 1 GB 的專用 VRAM 時，請使用64位的來賓 VM，而不是32位 (x86) 以獲得最佳結果。

5. 選取 **[確定** ] 以完成設定。

#### <a name="configure-remotefx-vgpu-with-powershell-cmdlets"></a>使用 PowerShell Cmdlet 設定 RemoteFX vGPU

使用下列 PowerShell Cmdlet 來新增、檢查和設定介面卡：

- [新增-Get-vmremotefx3dvideoadapter](/powershell/module/hyper-v/add-vmremotefx3dvideoadapter)
- [Get-vmremotefx3dvideoadapter](/powershell/module/hyper-v/get-vmremotefx3dvideoadapter)
- [設定-Get-vmremotefx3dvideoadapter](/powershell/module/hyper-v/set-vmremotefx3dvideoadapter)
- [Microsoft.hyperv.powershell.vmremotefxphysicalvideoadapter](/powershell/module/hyper-v/get-vmremotefxphysicalvideoadapter)

## <a name="monitor-performance"></a>監視效能

啟用 RemoteFX 的服務的效能和規模取決於各種不同的因素，例如系統上的 Gpu 數目、GPU 記憶體總計、系統記憶體的數量和記憶體速度、CPU 核心數目和 CPU 頻率頻率、儲存速度和 NUMA 實行。

### <a name="host-system-memory"></a>主機系統記憶體

針對使用 vGPU 啟用的每個 VM，RemoteFX 都會在客體作業系統和主機伺服器中使用系統記憶體。 虛擬程式可保證客體作業系統的系統記憶體可用性。 在主機上，每個啟用 vGPU 的虛擬桌面都必須通告其系統記憶體需求給虛擬程式。 當啟用 vGPU 的虛擬桌面啟動時，管理程式會在主機中保留額外的系統記憶體。

啟用 RemoteFX 之伺服器的記憶體需求是動態的，因為已啟用 RemoteFX 之伺服器上所耗用的記憶體數量，取決於與啟用 vGPU 的虛擬桌面相關聯的監視器數目，以及這些監視器的最大解析度。

### <a name="host-gpu-video-memory"></a>裝載 GPU 視訊記憶體

每個啟用 vGPU 的虛擬桌面都會使用主機伺服器上的 GPU 硬體視頻記憶體來呈現桌面。 此外，編解碼器會使用視訊記憶體壓縮呈現的畫面。 轉譯和壓縮所需的記憶體數量會直接以布建至虛擬機器的監視器數目為基礎。 保留的視訊記憶體數量會隨著系統螢幕解析度和有多少監視器而有所不同。 某些使用者需要較高的螢幕解析度來處理特定工作，但如果所有其他設定仍維持不變，則會有較低解析度設定的擴充性。

### <a name="host-cpu"></a>主機 CPU

虛擬程式會排定 CPU 上的主機和 Vm。 啟用 RemoteFX 的主機會增加額外負荷，因為系統會在每個啟用 vGPU 的虛擬桌面 ( # A0) 執行額外的處理常式。 此程式會使用圖形設備磁碟機在 GPU 上執行命令。 編解碼器也會使用 CPU 來壓縮需要傳送回用戶端的螢幕資料。

更多虛擬處理器表示更好的使用者體驗。 建議您為每個啟用 vGPU 的虛擬桌面配置至少兩個虛擬 Cpu。 針對啟用 vGPU 的虛擬桌面，我們也建議使用 x64 架構，因為 x64 虛擬機器上的效能比 x86 虛擬機器更好。

### <a name="gpu-processing-power"></a>GPU 處理能力

每個啟用 vGPU 的虛擬桌面都有一個在主機伺服器上執行的相對應 DirectX 進程。 此程式會將它從 RemoteFX 虛擬桌面收到的所有圖形命令重新放置到實體 GPU 上。 這就像是在相同的實體 GPU 上同時執行多個 DirectX 應用程式。

通常，圖形裝置和驅動程式會調整成隻在桌面上執行幾個應用程式，但 RemoteFX 會延伸 Gpu 以進行更進一步的操作。 Vgpu 隨附效能計數器，可測量 RemoteFX 要求的 GPU 回應，並協助您確定 Gpu 不會延伸到太遠。

當 GPU 的資源不足時，讀取和寫入作業需要很長的時間才能完成。 系統管理員可以使用效能計數器來瞭解調整資源的時機，並防止使用者停機。

深入瞭解在 [遠端桌面診斷圖形效能問題](/azure/virtual-desktop/remotefx-graphics-performance-counters)時，用來監視 RemoteFX vGPU 行為的效能計數器。
