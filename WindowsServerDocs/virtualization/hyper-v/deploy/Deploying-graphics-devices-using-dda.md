---
title: 使用離散裝置指派部署圖形裝置
description: 瞭解如何在 Windows Server 中使用 DDA 部署圖形裝置
ms.topic: article
ms.author: benarm
author: BenjaminArmstrong
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.date: 08/21/2019
ms.openlocfilehash: 3ea25da7b8ac3be2c7d2c194df6ead57653b2c76
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746093"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>使用離散裝置指派部署圖形裝置

> 適用于： Microsoft Hyper-V Server 2016、Windows Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

從 Windows Server 2016 開始，您可以使用離散裝置指派（或 DDA）將整個 PCIe 裝置傳遞至 VM。  這可讓裝置的高效能存取，例如 [NVMe 儲存體](./Deploying-storage-devices-using-dda.md) 或來自 VM 內的圖形卡，同時能夠利用裝置原生驅動程式。  請參閱 [使用離散裝置指派部署裝置的規劃](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) ，以取得哪些裝置適用的詳細資料、可能有哪些安全性含意等等。

使用具有個別裝置指派的裝置有三個步驟：
-   設定 VM 以進行離散裝置指派
-   從主機磁碟分割卸載裝置
-   將裝置指派給來賓 VM

所有命令都可以在 Windows PowerShell 主控台的主機上以系統管理員身分執行。

## <a name="configure-the-vm-for-dda"></a>設定 VM 以進行 DDA
離散裝置指派會對 Vm 強加一些限制，而且必須採取下列步驟。

1.  藉由執行，將 VM 的「自動停止動作」設定為 TurnOff

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>圖形裝置需要一些額外的 VM 準備

如果 VM 以特定方式設定，某些硬體的執行效能會更好。  如需您的硬體是否需要下列設定的詳細資訊，請洽詢硬體廠商。 如需其他詳細資料，請參閱[規劃使用離散裝置指派](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)和此[Blog 文章](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266)來部署裝置。

1. 啟用 CPU 的寫入組合
   ```
   Set-VM -GuestControlledCacheTypes $true -VMName VMName
   ```
2. 設定32位 MMIO 空間
   ```
   Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
   ```
3. 設定大於32位 MMIO 空間
   ```
   Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
   ```
   > [!TIP]
   > 上述的 MMIO 空間值是要針對使用單一 GPU 進行實驗而設定的合理值。  如果啟動 VM 之後，裝置會報告與資源不足相關的錯誤，您可能需要修改這些值。 請參閱 [規劃使用離散裝置指派部署裝置](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) ，以瞭解如何精確地計算 MMIO 需求。

## <a name="dismount-the-device-from-the-host-partition"></a>從主機磁碟分割卸載裝置
### <a name="optional---install-the-partitioning-driver"></a>選擇性-安裝資料分割驅動程式
離散裝置指派可讓硬體 venders 能夠以其裝置提供安全性緩和驅動程式。  請注意，此驅動程式與將安裝在來賓 VM 中的設備磁碟機不同。  由硬體廠商自行決定是否要提供此驅動程式; 不過，如果它們提供此驅動程式，請在從主機磁碟分割卸載裝置之前先安裝它。  如需瞭解是否有緩和驅動程式，請與硬體廠商聯繫以取得詳細資訊
> 如果未提供任何分割驅動程式，則在卸載期間，您必須使用 `-force` 選項來略過安全性警告。 請閱讀更多有關 [使用離散裝置指派部署裝置的規劃](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)安全性含意。

### <a name="locating-the-devices-location-path"></a>找出裝置的位置路徑
必須要有 PCI 位置路徑，才能從主機卸載並掛接裝置。  範例位置路徑看起來如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"` 。  您可以在這裡找到位置路徑的詳細資訊： [規劃使用離散裝置指派來部署裝置](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="disable-the-device"></a>停用裝置
使用裝置管理員或 PowerShell，確認裝置為「已停用」。

### <a name="dismount-the-device"></a>卸載裝置
根據廠商是否提供緩和驅動程式而定，您必須使用 "-force" 選項。
- 如果已安裝緩和驅動程式
  ```
  Dismount-VMHostAssignableDevice -LocationPath $locationPath
  ```
- 如果未安裝緩和驅動程式
  ```
  Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
  ```

## <a name="assigning-the-device-to-the-guest-vm"></a>將裝置指派給來賓 VM
最後一個步驟是告知 Hyper-v VM 應該具有裝置的存取權。  除了上述的位置路徑之外，您還需要知道 vm 的名稱。

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>後續步驟
在裝置成功掛接到 VM 之後，您現在可以啟動該 VM，並與裝置互動，如同您在裸機系統上執行一般。  這表示您現在可以在 VM 中安裝硬體廠商的驅動程式，而應用程式將能夠看到硬體存在。  您可以在來賓 VM 中開啟 [裝置管理員]，並看到現在已顯示硬體，來確認這一點。

## <a name="removing-a-device-and-returning-it-to-the-host"></a>移除裝置並將它返回主機
如果您想要讓裝置回到其原始狀態，您將需要停止 VM 併發出下列各項：
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
然後您可以在 [裝置管理員] 中重新啟用裝置，主機作業系統就能夠再次與裝置互動。

## <a name="example"></a>範例

### <a name="mounting-a-gpu-to-a-vm"></a>將 GPU 裝載至 VM
在此範例中，我們使用 PowerShell 來設定名為 "ddatest1" 的 VM，以取得製造商 NVIDIA 所提供的第一個 GPU，並將其指派給 VM。
```
#Configure the VM for a Discrete Device Assignment
$vm =   "ddatest1"
#Set automatic stop action to TurnOff
Set-VM -Name $vm -AutomaticStopAction TurnOff
#Enable Write-Combining on the CPU
Set-VM -GuestControlledCacheTypes $true -VMName $vm
#Configure 32 bit MMIO space
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
#Configure Greater than 32 bit MMIO space
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm

#Find the Location Path and disable the Device
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
#Disable the PNP Device
Disable-PnpDevice  -InstanceId $gpudevs[0].InstanceId

#Dismount the Device from the Host
Dismount-VMHostAssignableDevice -force -LocationPath $locationPath

#Assign the device to the guest VM.
Add-VMAssignableDevice -LocationPath $locationPath -VMName $vm
```

## <a name="troubleshooting"></a>疑難排解

如果您已將 GPU 傳遞至 VM，但遠端桌面或應用程式無法辨識 GPU，請檢查是否有下列常見的問題：

- 確定您已安裝最新版的 GPU 廠商支援驅動程式，且驅動程式未藉由在裝置管理員中檢查裝置狀態來報告錯誤。
- 確定您的裝置已在 VM 內配置足夠的 MMIO 空間。 若要深入瞭解，請參閱 [MMIO 空間](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md#mmio-space)。
- 請確定您使用的是廠商支援在此設定中使用的 GPU。 例如，某些廠商會在傳遞至 VM 時防止其取用者卡片運作。
- 請確定執行的應用程式支援在 VM 內執行，而且應用程式支援 GPU 與其相關聯的驅動程式。 有些應用程式具有 Gpu 和環境的允許清單。
- 如果您是在來賓上使用遠端桌面工作階段主機角色或 Windows Multipoint 服務，則必須確定特定的群組原則專案已設定為允許使用預設 GPU。 使用套用至來賓 (的群組原則物件或來賓) 上的本機群組原則編輯器，流覽至下列群組原則專案： [電腦設定**Computer Configuration**  >  **系統管理員範本**  >  **Windows 元件**] 遠端桌面服務  >  **Remote Desktop Services**  >  **遠端桌面工作階段主機**  >  **遠端會話環境**  >  **使用硬體預設圖形介面卡進行所有遠端桌面服務會話**。 將此值設定為 [已啟用]，然後在套用原則之後將 VM 重新開機。
