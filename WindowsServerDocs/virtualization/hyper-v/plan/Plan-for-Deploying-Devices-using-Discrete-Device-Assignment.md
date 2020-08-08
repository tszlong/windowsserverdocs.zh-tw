---
title: 規劃使用離散裝置指派來部署裝置
description: 瞭解在 Windows Server 中的 DDA 運作方式
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 08/21/2019
ms.openlocfilehash: 189a4f399ac76f1b7f30c5b45725c3a4fb6a8215
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989849"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>規劃使用離散裝置指派來部署裝置
>適用于： Microsoft Hyper-v Server 2016、Windows Server 2016、Microsoft Hyper-v Server 2019、Windows Server 2019

離散裝置指派可讓實體 PCIe 硬體直接從虛擬機器存取。  本指南將討論可以使用離散裝置指派、主機系統需求、在虛擬機器上施加的限制，以及個別裝置指派的安全性含意的裝置類型。

針對離散裝置指派的初始版本，我們將焦點放在 Microsoft 正式支援的兩個裝置類別：圖形配接器和 NVMe 存放裝置。  其他裝置可能會正常執行，而硬體廠商則能夠提供這些裝置的支援聲明。  針對這些其他裝置，請與這些硬體廠商聯繫以取得支援。

若要瞭解 GPU 虛擬化的其他方法，請參閱[在 Windows Server 中規劃 GPU 加速](plan-for-gpu-acceleration-in-windows-server.md)。 如果您已準備好嘗試使用離散裝置指派，您可以跳至[使用離散裝置指派來部署圖形裝置](../deploy/Deploying-graphics-devices-using-dda.md)，或[使用離散裝置指派來部署存放裝置](../deploy/Deploying-storage-devices-using-dda.md)以開始。

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>支援的虛擬機器和客體作業系統
第1代或第二部 Vm 支援個別的裝置指派。  此外，支援的來賓包括 Windows 10、Windows Server 2019、Windows Server 2016、已套用[KB 3133690](https://support.microsoft.com/kb/3133690)的 windows server 2012r2，以及[Linux OS](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)的各種發行版本。

## <a name="system-requirements"></a>系統需求
除了[Windows Server 的系統需求](../../../get-started/system-requirements.md)和[Hyper-v 的系統需求](../System-requirements-for-Hyper-V-on-Windows.md)之外，離散裝置指派需要伺服器類別硬體，能夠授與作業系統控制設定 PCIe 網狀架構 (原生 PCI Express 控制) 。 此外，PCIe Root 複雜，必須支援「存取控制服務」 (ACS) ，讓 Hyper-v 能夠透過 i/o MMU 來強制所有的 PCIe 流量。

這些功能通常不會直接在伺服器的 BIOS 中公開，而且通常會隱藏在其他設定後面。  例如，SR-IOV 支援需要相同的功能，而且在 BIOS 中，您可能需要設定「啟用 SR-IOV」。  如果您無法在 BIOS 中識別正確的設定，請與您的系統廠商聯繫。

為了協助確保硬體能夠進行不連續的裝置指派，我們的工程師結合了一組[電腦設定檔腳本](#machine-profile-script)，可讓您在已啟用 hyper-v 的主機上執行，以測試您的伺服器是否已正確設定，以及哪些裝置可以進行不同的裝置指派。

## <a name="device-requirements"></a>裝置需求
並非每個 PCIe 裝置都可以與個別的裝置指派搭配使用。  例如，不支援利用舊版 (INTx) PCI 中斷的舊版裝置。 Jake Oshin 的[blog 文章](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/)會更詳細說明-不過，針對取用者，執行[電腦設定檔腳本](#machine-profile-script)會顯示哪些裝置可以用於個別的裝置指派。

裝置製造商可以與 Microsoft 代表聯繫，以取得更多詳細資料。

## <a name="device-driver"></a>設備磁碟機
當個別的裝置指派將整個 PCIe 裝置傳遞至來賓 VM 時，不需要在掛接到 VM 內的裝置之前安裝主機驅動程式。  主機上的唯一需求是可以判斷裝置的[PCIe 位置路徑](#pcie-location-path)。  如果這有助於識別裝置，則可以選擇性地安裝裝置的驅動程式。  例如，沒有安裝在主機上之設備磁碟機的 GPU，可能會顯示為 Microsoft 基本轉譯裝置。  如果已安裝設備磁碟機，則可能會顯示其製造商和型號。

一旦裝置掛接在來賓內部，製造商的設備磁碟機現在就可以像平常一樣在來賓虛擬機器中安裝。

## <a name="virtual-machine-limitations"></a>虛擬機器限制
由於獨立裝置指派的執行方式，在連接裝置時，會限制虛擬機器的某些功能。  無法使用下列功能：
- VM 儲存/還原
- VM 的即時移轉
- 動態記憶體的使用
- 將 VM 新增至高可用性 (HA) 叢集

## <a name="security"></a>安全性
離散裝置指派會將整個裝置傳遞至 VM。  這表示可以從客體作業系統存取該裝置的所有功能。 某些功能（例如，「固件更新」）可能會對系統的穩定性造成不良的影響。 因此，從主機卸載裝置時，系統管理員會看到許多警告。 我們強烈建議您只在 Vm 的租使用者受到信任的情況下，才使用離散裝置指派。

如果系統管理員想要使用裝置與不受信任的租使用者，我們提供了裝置製造商，讓您能夠建立可安裝在主機上的裝置緩和驅動程式。  請洽詢裝置製造商，以取得其是否提供裝置緩和驅動程式的詳細資料。

如果您想要略過不具有裝置緩和驅動程式之裝置的安全性檢查，您必須將 `-Force` 參數傳遞至 `Dismount-VMHostAssignableDevice` Cmdlet。  瞭解這一點，您已變更該系統的安全性設定檔，而且只有在原型化或受信任的環境中才建議使用。

## <a name="pcie-location-path"></a>PCIe 位置路徑
需要有 PCIe 位置路徑，才能從主機卸載和掛接裝置。  範例位置路徑看起來如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"` 。   [電腦設定檔腳本](#machine-profile-script)也會傳回 PCIe 裝置的位置路徑。

### <a name="getting-the-location-path-by-using-device-manager"></a>使用 Device Manager 取得位置路徑
![裝置管理員](../deploy/media/dda-devicemanager.png)
- 開啟 Device Manager 並尋找裝置。
- 以滑鼠右鍵按一下裝置，然後選取 [屬性]。
- 流覽至 [詳細資料] 索引標籤，然後選取 [屬性] 下拉式集中的 [位置路徑]。
- 以滑鼠右鍵按一下開頭為 "PCIROOT" 的專案，然後選取 [複製]。  您現在有該裝置的位置路徑。

## <a name="mmio-space"></a>MMIO 空間
某些裝置（特別是 Gpu）需要將額外的 MMIO 空間配置給 VM，才能存取該裝置的記憶體。 根據預設，每個 VM 會以128MB 的低 MMIO 空間，以及配置給它的512MB 高 MMIO 空間來啟動。 不過，裝置可能需要更多的 MMIO 空間，或可能傳遞多個裝置，使組合的需求超出這些值。  變更 MMIO 空間相當簡單，可以使用下列命令在 PowerShell 中執行：

```PowerShell
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```

若要判斷要配置多少 MMIO 空間，最簡單的方式是使用[電腦設定檔腳本](#machine-profile-script)。 若要下載並執行電腦設定檔腳本，請在 PowerShell 主控台中執行下列命令：

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

低 MMIO 空間僅供32位作業系統和使用32位位址的裝置使用。 在大部分情況下，設定 VM 的 high MMIO 空間將會夠大，因為32位設定並不常見。

> [!IMPORTANT]
> 將 MMIO 空間指派給 VM 時，使用者必須確定將 MMIO 空間設定為所有所需指派之裝置所要求的 MMIO 空間總和，並在有其他虛擬裝置需要幾 MB 的 MMIO 空間時，再加上額外的緩衝區。 使用以上所述的預設 MMIO 值做為低和高 MMIO 的緩衝區 (128 MB 和 512 MB，分別) 。

如果使用者要指派單一 K520 GPU （如上述範例所示），則必須將 VM 的 MMIO 空間設定為電腦設定檔腳本輸出的值，加上緩衝區--176 MB + 512 MB。 如果使用者要指派三個 K520 Gpu，它們必須將 MMIO 空間設定為 176 MB 加上緩衝區，或 528 MB + 512 MB 的3倍。

若要深入瞭解 MMIO 空間，請參閱 TechCommunity blog 上的[個別裝置指派-gpu](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266) 。

## <a name="machine-profile-script"></a>電腦設定檔腳本
為了簡化識別伺服器是否正確設定的情況，以及哪些裝置可透過使用離散裝置指派來傳遞，我們的其中一位工程師將下列 PowerShell 腳本放在一起： [SurveyDDA.ps1。](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

使用腳本之前，請確定您已安裝 Hyper-v 角色，而且您是從具有系統管理員許可權的 PowerShell 命令視窗執行腳本。

如果系統不正確地設定為支援離散裝置指派，此工具將會顯示錯誤訊息，表示發生錯誤。 如果工具找到正確設定的系統，它會列舉它可以在 PCIe 匯流排上找到的所有裝置。

針對它找到的每個裝置，此工具會顯示其是否能夠與離散裝置指派搭配使用。 如果將裝置識別為與不同的裝置指派相容，則腳本會提供原因。  當裝置成功識別為相容時，將會顯示裝置的位置路徑。  此外，如果該裝置需要[MMIO 空間](#mmio-space)，則會一併顯示。

![SurveyDDA.ps1](./images/hyper-v-surveydda-ps1.png)