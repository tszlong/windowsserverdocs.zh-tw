---
title: 規劃使用離散裝置指派部署裝置
description: 瞭解 DDA 在 Windows Server 中的運作方式
ms.topic: article
ms.author: benarm
author: BenjaminArmstrong
ms.date: 08/21/2019
ms.openlocfilehash: f53cef755d52fe1fc1e1bc540f89e7c243007eb3
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745953"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>規劃使用離散裝置指派部署裝置
>適用于： Microsoft Hyper-V Server 2016、Windows Server 2016、Microsoft Hyper-V Server 2019、Windows Server 2019

離散裝置指派可讓您直接從虛擬機器存取實體 PCIe 硬體。  本指南將討論可以使用離散裝置指派的裝置類型、主機系統需求、在虛擬機器上施加的限制，以及離散裝置指派的安全性含意。

針對離散裝置指派的初始版本，我們將焦點放在 Microsoft 所正式支援的兩個裝置類別：圖形介面卡和 NVMe 儲存裝置。  其他裝置可能會運作，而且硬體廠商可以提供這些裝置的支援聲明。  針對這些其他裝置，請與這些硬體廠商聯繫以取得支援。

若要瞭解 GPU 虛擬化的其他方法，請參閱 [在 Windows Server 中規劃 gpu 加速](plan-for-gpu-acceleration-in-windows-server.md)。 如果您已準備好嘗試進行離散裝置指派，您可以跳至 [使用離散裝置指派來部署圖形裝置](../deploy/Deploying-graphics-devices-using-dda.md) ，或 [使用離散裝置指派部署存放裝置](../deploy/Deploying-storage-devices-using-dda.md) 以開始使用。

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>支援的虛擬機器和客體作業系統
第1代或第2代 Vm 支援離散裝置指派。  此外，支援的來賓包括 Windows 10、Windows Server 2019、Windows Server 2016、已套用[KB 3133690](https://support.microsoft.com/kb/3133690)的 windows server 2012r2，以及[Linux 作業系統](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)的各種散發套件。

## <a name="system-requirements"></a>系統需求
除了 [Windows Server 的系統需求](../../../get-started/system-requirements.md) 和 [Hyper-v 的系統需求](../System-requirements-for-Hyper-V-on-Windows.md)之外，離散裝置指派還需要能夠將 PCIe 網狀架構 (原生 PCI Express 控制項) 的伺服器類別硬體授與作業系統控制。 此外，PCIe 根目錄複雜必須支援「存取控制服務」 (ACS) ，如此可讓 Hyper-v 透過 i/o MMU 強制所有 PCIe 流量。

這些功能通常不會直接在伺服器的 BIOS 中公開，且通常會隱藏在其他設定的背後。  例如，SR-IOV 支援和 BIOS 中的相同功能是必要的，您可能需要設定「Enable SR-IOV」。  如果您無法在 BIOS 中找出正確的設定，請與您的系統廠商聯繫。

為了協助確保硬體硬體能夠進行分開的裝置指派，我們的工程師將 [電腦設定檔腳本](#machine-profile-script) 放在一起，讓您可以在已啟用 hyper-v 的主機上執行，以測試伺服器是否正確設定，以及哪些裝置能夠進行個別的裝置指派。

## <a name="device-requirements"></a>裝置需求
並非每個 PCIe 裝置都可以與離散裝置指派一起使用。  例如，不支援利用舊版 (INTx) PCI 中斷的舊版裝置。 Jake Oshin 的 [blog 文章](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/) 會更詳細地討論，不過，針對取用者，執行 [電腦設定檔腳本](#machine-profile-script) 會顯示哪些裝置可以用於分開的裝置指派。

裝置製造商如需詳細資料，可與 Microsoft 代表聯繫。

## <a name="device-driver"></a>設備磁碟機
當離散裝置指派將整個 PCIe 裝置傳送至來賓 VM 時，在 VM 內掛接裝置之前，不需要安裝主機驅動程式。  主機上的唯一需求是可以決定裝置的 [PCIe 位置路徑](#pcie-location-path) 。  如果這有助於識別裝置，則可以選擇性地安裝裝置的驅動程式。  例如，在主機上未安裝其設備磁碟機的 GPU，可能會顯示為 Microsoft 基本轉譯裝置。  如果已安裝設備磁碟機，則可能會顯示其製造商和型號。

一旦將裝置掛接在來賓內，製造商的設備磁碟機現在就可以像是在來賓虛擬機器內一樣安裝正常。

## <a name="virtual-machine-limitations"></a>虛擬機器限制
由於如何實行離散裝置指派的本質，因此在附加裝置時，會限制虛擬機器的某些功能。  無法使用下列功能：
- VM 儲存/還原
- 虛擬機器的即時移轉
- 使用動態記憶體
- 將 VM 新增至高可用性 (HA) 叢集

## <a name="security"></a>安全性
離散裝置指派會將整個裝置傳遞至 VM。  這表示可以從客體作業系統存取該裝置的所有功能。 某些功能（例如，固件更新）可能會對系統的穩定性造成負面影響。 因此，從主機卸載裝置時，系統管理員會看到許多警告。 強烈建議您只在 Vm 的租使用者受信任的情況下，才使用離散裝置指派。

如果系統管理員想要使用裝置與不受信任的租使用者，我們提供了裝置製造商建立可在主機上安裝之裝置緩和驅動程式的能力。  請洽詢裝置製造商，以取得其是否提供裝置緩和驅動程式的詳細資料。

如果您想要針對沒有裝置緩和驅動程式的裝置略過安全性檢查，您必須將 `-Force` 參數傳遞給 `Dismount-VMHostAssignableDevice` Cmdlet。  若要瞭解這一點，您已變更該系統的安全性設定檔，而且只有在原型設計或信任的環境中才建議使用。

## <a name="pcie-location-path"></a>PCIe 位置路徑
需要有 PCIe 位置路徑，才能從主機卸載並掛接裝置。  範例位置路徑看起來如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"` 。   [電腦設定檔腳本](#machine-profile-script)也會傳回 PCIe 裝置的位置路徑。

### <a name="getting-the-location-path-by-using-device-manager"></a>使用裝置管理員取得位置路徑
![裝置管理員](../deploy/media/dda-devicemanager.png)
- 開啟裝置管理員並找出裝置。
- 以滑鼠右鍵按一下裝置，然後選取 [屬性]。
- 流覽至 [詳細資料] 索引標籤，然後在屬性下拉式清單中選取 [位置路徑]。
- 以滑鼠右鍵按一下開頭為 "PCIROOT" 的專案，然後選取 [複製]。  您現在已有該裝置的位置路徑。

## <a name="mmio-space"></a>MMIO 空間
某些裝置（特別是 Gpu）需要額外的 MMIO 空間來配置給 VM，才能存取該裝置的記憶體。 根據預設，每個 VM 會從128MB 的低 MMIO 空間開始，並配置512個/MMIO 空間給它。 不過，裝置可能需要更多的 MMIO 空間，或可能傳遞多個裝置，因此組合的需求超過這些值。  變更 MMIO 空間相當簡單，可以使用下列命令在 PowerShell 中執行：

```PowerShell
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```

判斷要配置多少 MMIO 空間的最簡單方式，就是使用 [電腦設定檔腳本](#machine-profile-script)。 若要下載並執行電腦設定檔腳本，請在 PowerShell 主控台中執行下列命令：

```PowerShell
curl -o SurveyDDA.ps1 https://raw.githubusercontent.com/MicrosoftDocs/Virtualization-Documentation/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1
.\SurveyDDA.ps1
```

針對能夠指派的裝置，腳本會顯示指定裝置的 MMIO 需求，如下列範例所示：

```PowerShell
NVIDIA GRID K520
Express Endpoint -- more secure.
    ...
    And it requires at least: 176 MB of MMIO gap space
...
```

低 MMIO 空間只適用于32位的作業系統和使用32位位址的裝置。 在大多數情況下，設定 VM 的 high MMIO 空間將會足夠，因為32位設定並不常見。

> [!IMPORTANT]
> 將 MMIO 空間指派給 VM 時，如果有其他需要幾 MB 的 MMIO 空間的虛擬裝置，使用者必須確定將 MMIO 空間設定為所有所需指派裝置的要求 MMIO 空間總和，以及額外的緩衝區。 使用上面所述的預設 MMIO 值作為低和高 MMIO (128 MB 和 512 MB 的緩衝區，分別) 。

如果使用者要指派單一 K520 GPU （如上述範例所示），則必須將 VM 的 MMIO 空間設定為電腦設定檔腳本輸出的值，再加上緩衝區--176 MB + 512 MB。 如果使用者要指派三個 K520 Gpu，則必須將 MMIO 空間設定為3倍的 176 MB 加上緩衝區，或 528 MB + 512 MB。

如需有關 MMIO 空間的深入探討，請參閱 >techcommunity blog 上的 [離散裝置指派（gpu](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266) ）。

## <a name="machine-profile-script"></a>電腦設定檔腳本
為了簡化識別伺服器是否正確設定的方式，以及哪些裝置可透過不同的裝置指派傳遞，我們的其中一位工程師會將下列 PowerShell 腳本放在一起： [SurveyDDA.ps1。](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

使用腳本之前，請確定您已安裝 Hyper-v 角色，而且您是從具有系統管理員許可權的 PowerShell 命令視窗執行腳本。

如果系統設定錯誤，以支援離散裝置指派，則工具會顯示錯誤訊息，並顯示錯誤訊息。 如果工具找到正確設定的系統，則會列舉在 PCIe 匯流排上可找到的所有裝置。

針對它找到的每個裝置，此工具會顯示是否能夠與離散裝置指派一起使用。 如果裝置識別為與離散裝置指派相容，腳本將會提供原因。  當裝置成功識別為相容時，就會顯示裝置的位置路徑。  此外，如果該裝置需要 [MMIO 空間](#mmio-space)，也會顯示該裝置。

![SurveyDDA.ps1](./images/hyper-v-surveydda-ps1.png)