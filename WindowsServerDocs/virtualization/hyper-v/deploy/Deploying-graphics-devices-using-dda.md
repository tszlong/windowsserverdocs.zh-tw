---
title: 使用離散裝置指派來部署圖形裝置
description: 瞭解如何在 Windows Server 中使用 DDA 部署圖形裝置
ms.prod: windows-server
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.openlocfilehash: 3b37abaf5a2341aff66ff0064ecc4f52faf47f06
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393001"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>使用離散裝置指派來部署圖形裝置

>適用於：Microsoft Hyper-v Server 2016、Windows Server 2016、Windows Server 2019、Microsoft Hyper-v Server 2019  

從 Windows Server 2016 開始，您可以使用離散裝置指派或 DDA，將整個 PCIe 裝置傳遞至 VM。  這可讓您對裝置的高效能存取，例如從 VM 內[NVMe 儲存體](./Deploying-storage-devices-using-dda.md)或圖形卡，同時能夠利用裝置原生驅動程式。  請流覽[使用離散裝置指派來部署裝置的計畫](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)，以取得更多裝置的工作、可能的安全性含意等等。

使用具有離散裝置指派的裝置有三個步驟：
-   設定 VM 以進行離散裝置指派
-   從主機磁碟分割卸載裝置
-   將裝置指派給來賓 VM

所有命令都可以在 Windows PowerShell 主控台的主機上，以系統管理員身分執行。

## <a name="configure-the-vm-for-dda"></a>為 DDA 設定 VM
個別的裝置指派會對 Vm 施加一些限制，而且必須採取下列步驟。

1.  藉由執行，將 VM 的「自動停止動作」設定為 Turnon

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>圖形裝置需要一些額外的 VM 準備

如果 VM 是以某種方式設定，某些硬體的執行效能會更好。  如需有關您的硬體是否需要下列設定的詳細資訊，請聯繫硬體廠商。 如需其他詳細資料，請參閱[規劃使用離散裝置指派來部署裝置](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)和此[blog 文章。](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266)

1. 在 CPU 上啟用寫入合併
   ```
   Set-VM -GuestControlledCacheTypes $true -VMName VMName
   ```
2. 設定32位 MMIO 空間
   ```
   Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
   ```
3. 設定大於32位的 MMIO 空間
   ```
   Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
   ```
   > [!TIP] 
   > 上述的 MMIO 空間值是合理的值，可設定為使用單一 GPU 進行實驗。  如果啟動 VM 之後，裝置會報告與資源不足相關的錯誤，您可能需要修改這些值。 請參閱[使用離散裝置指派部署裝置的計畫](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)，以瞭解如何精確地計算 MMIO 需求。

## <a name="dismount-the-device-from-the-host-partition"></a>從主機磁碟分割卸載裝置
### <a name="optional---install-the-partitioning-driver"></a>選擇性-安裝資料分割驅動程式
個別的裝置指派可讓硬體 venders 提供安全性緩和驅動程式與其裝置的能力。  請注意，此驅動程式與將安裝在來賓 VM 中的設備磁碟機不同。  硬體廠商會自行決定是否要提供此驅動程式，不過，如果它們確實提供，請先安裝它，然後再從主機磁碟分割卸載裝置。  如需其是否有緩和驅動程式的詳細資訊，請與硬體廠商聯繫
> 如果未提供任何資料分割驅動程式，在卸載期間， `-force`您必須使用選項來略過安全性警告。 請在[規劃使用離散裝置指派來部署裝置](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)時，閱讀更多關於此動作的安全性含意。

### <a name="locating-the-devices-location-path"></a>尋找裝置的位置路徑
必須要有 PCI 位置路徑，才能從主機卸載並掛接裝置。  範例位置路徑看起來如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`。  如需位置路徑的詳細資訊，請參閱：[規劃使用離散裝置指派來部署裝置](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="disable-the-device"></a>停用裝置
使用 Device Manager 或 PowerShell，確定裝置已「停用」。  

### <a name="dismount-the-device"></a>卸載裝置
根據廠商是否提供緩和驅動程式而定，您將需要使用 "-force" 選項。
- 如果已安裝緩和驅動程式
  ```
  Dismount-VMHostAssignableDevice -LocationPath $locationPath
  ```
- 如果未安裝緩和驅動程式
  ```
  Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
  ```

## <a name="assigning-the-device-to-the-guest-vm"></a>將裝置指派給來賓 VM
最後一個步驟是告訴 Hyper-v，VM 應具有裝置的存取權。  除了上面找到的位置路徑，您還必須知道 vm 的名稱。

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>下一步
在 VM 中成功掛接裝置之後，您現在可以啟動該 VM 並與裝置互動，如同您在裸機系統上執行一般。  這表示您現在可以在 VM 中安裝硬體廠商的驅動程式，應用程式將能夠看到硬體存在。  您可以在來賓 VM 中開啟 [裝置管理員]，並看到硬體現在顯示，以驗證這一點。

## <a name="removing-a-device-and-returning-it-to-the-host"></a>移除裝置並將它傳回主機
如果您想要讓裝置回到其原始狀態，您將需要停止 VM 併發出下列問題：
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
接著，您可以在 [裝置管理員] 中重新啟用裝置，主機作業系統將能夠再次與裝置互動。

## <a name="examples"></a>範例

### <a name="mounting-a-gpu-to-a-vm"></a>將 GPU 掛接至 VM
在此範例中，我們使用 PowerShell 來設定名為 "ddatest1" 的 VM，以取得製造商 NVIDIA 提供的第一個 GPU，並將其指派給 VM。  
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
