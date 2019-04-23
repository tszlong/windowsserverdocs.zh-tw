---
title: 規劃的部署使用不連續的裝置指派的裝置
description: 深入了解 DDA Windows Server 中的運作方式
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 02/06/2018
ms.openlocfilehash: c64c2b75c00f97622278c3e590db46995e108218
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840199"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>規劃部署的裝置，使用不連續的裝置指派
>適用於：Microsoft HYPER-V Server 2016、windows Server 2016 中，Microsoft HYPER-V Server 2019，Windows Server 2019

不連續的裝置指派可讓您能夠直接從虛擬機器內的實體 PCIe 硬體。  本指南會討論裝置的類型，可以使用不連續的裝置指派，主機系統需求，在虛擬機器，以及離散的裝置指派的安全性含意加諸的限制。

針對離散裝置指派的初始版本中，我們已經著重於 Microsoft 正式支援的兩個裝置類別：圖形卡和 NVMe 儲存體裝置。  其他裝置都可能運作，而且能夠提供陳述式對這些裝置的支援硬體廠商。  針對這些其他的裝置，請連絡硬體廠商支援。

如果您準備好要試試看離散的裝置指派，您可以跳到[部署圖形裝置使用離散裝置指派](../deploy/Deploying-graphics-devices-using-dda.md)或是[部署存放裝置的使用不連續的裝置指派](../deploy/Deploying-storage-devices-using-dda.md)若要開始使用 ！

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>支援的虛擬機器和客體作業系統
第 1 代或 2 個 Vm 支援離散的裝置指派。  此外，客體支援包括 Windows 10，Windows Server 2019、 Windows Server 2016 與 Windows Server 2012 r2 [KB 3133690](https://support.microsoft.com/kb/3133690)套用，以及各種不同的散發版本[Linux OS。](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)

## <a name="system-requirements"></a>系統需求
除了[適用於 Windows Server 系統需求](../../../get-started/System-Requirements--and-Installation.md)並[適用於 HYPER-V 的系統需求](../System-requirements-for-Hyper-V-on-Windows.md)，離散的裝置指派需要能夠授與的 server 類別硬體作業系統控制設定 PCIe 網狀架構 （原生 PCI Express 控制）。 颾魤 ㄛ PCIe 根複雜必須支援 「 存取控制服務 」 (ACS)，可讓 HYPER-V 能夠強制所有 PCIe 流量透過 I/O MMU。

這些功能通常不直接在伺服器的 BIOS 中公開，而且通常會隱藏之後的其他設定。  例如，相同的功能所需的 SR-IOV 支援，而在 BIOS 中，您可能需要設定 「 啟用 SR-IOV。 」  請連絡您的系統廠商如果您無法識別您的 BIOS 中的正確設定。

為了協助確保裝置的硬體都可以不連續的裝置指派，我們的工程師有將集結[電腦設定檔指令碼](#machine-profile-script)您可以測試您的伺服器已正確安裝，以及是否已啟用 HYPER-V 主機上執行能夠不連續的裝置指派的裝置。

## <a name="device-requirements"></a>裝置需求
並非每個 PCIe 裝置可以搭配離散的裝置指派。  比方說，不支援較舊利用舊版 (INTx) PCI 插斷的裝置。 Jake Oshin[部落格文章](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/)移至 詳細資料-但是，取用者，並執行[機器設定檔指令碼](#machine-profile-script)會顯示哪些裝置有可用於離散的裝置指派。

裝置製造商可以連到其 Microsoft 代表，以取得更多詳細資料。

## <a name="device-driver"></a>裝置驅動程式
如不連續的裝置指派到客體 VM 通過整個 PCIe 裝置，不需要掛接在 VM 內的裝置之前，先安裝主應用程式的驅動程式。  在主機上的唯一需求是裝置的[PCIe 位置路徑](#pcie-location-path)可以決定。  如果這有助於識別裝置，可以選擇性地安裝裝置驅動程式。  比方說，而不需要在主機上安裝其裝置驅動程式的 GPU 可能會顯示為 Microsoft 基本轉譯的裝置。  如果已安裝裝置驅動程式，可能會顯示其製造商和型號。

裝置一經掛接之後在客體內，製造商的裝置驅動程式現在可以安裝在客體虛擬機器內的正常的方式。  

## <a name="virtual-machine-limitations"></a>虛擬機器限制
由於不連續的裝置指派的實作方式，附加的裝置時，會限制虛擬機器的某些功能。  找不到提供的下列功能：
- VM 儲存/還原
- 即時移轉的 VM
- 使用動態記憶體
- 將 VM 加入高可用性 (HA) 叢集

## <a name="security"></a>安全性
不連續的裝置指派會將整個裝置傳遞至 VM。  這表示該裝置的所有功能都都可從客體作業系統中存取。 某些功能，例如韌體更新，可能會造成負面影響系統的穩定性。 因此，許多警告，會向系統管理員從主應用程式裝置時。 我們強烈建議不連續的裝置指派只用租用戶的 Vm 是受信任的位置。  

如果系統管理員想要使用不受信任的租用戶中的裝置，我們能夠建立可在主機安裝的裝置風險降低驅動程式提供裝置製造商。  請如需有關他們是否提供裝置風險降低驅動程式，連絡裝置製造商。

如果您想要跳過的裝置，並沒有裝置風險降低驅動程式的安全性檢查，您必須將`-Force`參數來`Dismount-VMHostAssignableDevice`cmdlet。  了解這樣做，您已變更該系統的安全性設定檔，這只是建議您在原型設計期間或受信任的環境。

## <a name="pcie-location-path"></a>PCIe 位置路徑
PCIe 位置路徑，才能卸除，並從主機中掛接裝置。  範例位置路徑看起來如下： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`。   [電腦設定檔指令碼](#machine-profile-script)也會傳回 PCIe 裝置的位置路徑。

### <a name="getting-the-location-path-by-using-device-manager"></a>開始使用裝置管理員的位置路徑
![裝置管理員](../deploy/media/dda-devicemanager.png)
- 開啟裝置管理員，並找出裝置。  
- 以滑鼠右鍵按一下裝置，然後選取 [內容]。
- 瀏覽至 [詳細資料] 索引標籤，然後選取 「 位置路徑 」 屬性下拉式清單中。  
- 以滑鼠右鍵按一下 「 PCIROOT"為開頭的項目，然後選取 [複製]。  現在，您會有該裝置的位置路徑。

## <a name="mmio-space"></a>MMIO 空間
某些裝置，特別是 Gpu，需要額外的 MMIO 空間，以配置給 VM，以便該裝置的記憶體存取。 根據預設，每個 VM 會開頭 128 MB 的 MMIO 空間不足，而 512 MB 的高 MMIO 空間配置給它。 不過，裝置可能需要更多的 MMIO 空間，或可能透過傳遞多個裝置，使之結合的需求超過這些值。  變更 MMIO 空間很簡單，而且可以執行在 PowerShell 中使用下列命令：

```
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```
最簡單的方式，來判斷要使用配置空間量 MMIO[電腦設定檔指令碼](#machine-profile-script)。  或者，您可以計算使用裝置管理員。 請參閱 TechNet 部落格文章[離散的裝置指派-Gpu](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/)如需詳細資訊。

## <a name="machine-profile-script"></a>電腦設定檔指令碼
為了簡化識別如果伺服器已正確設定，及哪些裝置有要傳遞使用不連續的裝置指派，我們的工程師之一匯集了下列 PowerShell 指令碼：[SurveyDDA.ps1.](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

在使用前指令碼，請確定您已安裝 HYPER-V 角色，以及您從具有系統管理員權限的 PowerShell 命令視窗執行指令碼。

如果系統未正確設定為支援離散的裝置指派，則工具會顯示有關什麼是錯誤的錯誤訊息。 如果此工具找到正確的系統，它會列舉它可以尋找 PCIe 匯流排的所有裝置。

它會尋找每個裝置，是否能夠搭配離散的裝置指派，就會顯示一個工具。 如果裝置被識別為相容與不連續的裝置指派，指令碼會提供原因。  當裝置已成功識別為不相容時，就會顯示裝置的位置路徑。  此外，如果該裝置需要[MMIO 空間](#mmio-space)，它會顯示出來。

![SurveyDDA.ps1](./images/hyper-v-surveydda-ps1.png)

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="how-does-remote-desktops-remotefx-vgpu-technology-relate-to-discrete-device-assignment"></a>遠端桌面的 RemoteFX vGPU 技術與離散的裝置指派有如何關聯？
也就是完全不同的技術。 RemoteFX vGPU 不需要安裝為不連續的裝置指派給工作。 此外，任何額外的角色不都需要安裝。 RemoteFX vGPU 需要 RDVH 角色安裝 RemoteFX vGPU 驅動程式會在 VM 中出現的順序。 用於離散指派的裝置，您將需要安裝硬體廠商的驅動程式到虛擬機器，因為沒有任何其他角色必須存在。  

### <a name="ive-passed-a-gpu-into-a-vm-but-remote-desktop-or-an-application-isnt-recognizing-the-gpu"></a>我已傳入 VM 中的 GPU，但遠端桌面或應用程式無法辨識 GPU
有數種原因會發生此問題，但下面列出幾個常見的問題。
- 請確定已安裝最新的 GPU 廠商的驅動程式，且藉由檢查裝置狀態 裝置管理員中未回報錯誤。
- 請確定該裝置具有足夠[MMIO 空間](#mmio-space)VM 內配置給它。
- 請確定您使用的廠商支援使用在此組態中的 GPU。 比方說，某些廠商會將其取用者卡防止工作時傳遞到 VM。
- 請確定 VM 內執行的支援和支援 GPU 和其相關聯的驅動程式，應用程式正在執行的應用程式。 某些應用程式具有 Gpu 和環境的允許清單。
- 如果您使用遠端桌面工作階段主機角色或 Windows Multipoint 服務客體上，您必須確定特定群組原則項目已設定為允許使用 GPU 的預設值。 使用群組原則物件套用至客體 （或本機群組原則編輯器，在客體上），請瀏覽至下列群組原則項目：
   - 電腦設定
   - 系統管理員範本
   - Windows 元件
   - 遠端桌面服務
   - 遠端桌面工作階段主機
   - 遠端工作階段環境
   - 所有的遠端桌面服務工作階段中使用的硬體預設圖形配接器

    將此值設定為 已啟用，然後在套用原則之後，重新啟動 VM。

### <a name="can-discrete-device-assignment-take-advantage-of-remote-desktops-avc444-codec"></a>可以不連續的裝置指派利用遠端桌面 AVC444 轉碼器？
是，請造訪這個部落格文章，如需詳細資訊：[在 Windows 10 和 Windows Server 2016 Technical Preview 的遠端桌面通訊協定 (RDP) 10 AVC/H.264 改進。](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

### <a name="can-i-use-powershell-to-get-the-location-path"></a>可以使用 PowerShell 取得的位置路徑嗎？
是，若要這樣做的各種方式。 以下是其中一個範例：
```
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
```

### <a name="can-discrete-device-assignment-be-used-to-pass-a-usb-device-into-a-vm"></a>不連續的裝置指派可用來將 USB 裝置傳遞至 VM？
雖然未正式支援，但我們的客戶已使用不連續的裝置指派，來執行這項操作將整個 USB3 控制器傳遞至 VM。  如在傳遞整個控制器，則插入該控制站的每個 USB 裝置也可以存取在 VM 中。  請注意，只有一些 USB3 控制站可能可以運作，USB2 控制站不能搭配離散的裝置指派。
