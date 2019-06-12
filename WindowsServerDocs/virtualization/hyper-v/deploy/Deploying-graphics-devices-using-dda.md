---
title: 部署使用不連續的裝置指派的圖形裝置
description: 了解如何使用 DDA 部署 Windows Server 中的圖形裝置
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.openlocfilehash: 6c528535fd34f57957a37992843933d4cd9f8824
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447876"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>部署使用不連續的裝置指派的圖形裝置

>適用於：Microsoft HYPER-V Server 2016，Windows Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019  

從 Windows Server 2016 開始，您可以使用不連續的裝置指派] 或 [DDA，來將整個 PCIe 裝置傳遞至 VM。  這可讓裝置，例如高效能存取[NVMe 儲存體](./Deploying-storage-devices-using-dda.md)或從同時能夠運用裝置的原生驅動程式在 VM 內的圖形卡。  請瀏覽[規劃部署的裝置，使用不連續的裝置指派](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)如需哪些裝置工作的詳細資訊，為何的潛在安全性風險，依此類推。

有三個步驟，使用不連續的裝置指派的裝置：
-   將 VM 設定為不連續的裝置指派
-   卸載主機分割區的裝置
-   將裝置指派給客體 VM

身為系統管理員，可以在主機上的 Windows PowerShell 主控台執行所有命令。

## <a name="configure-the-vm-for-dda"></a>設定 VM 的 DDA
不連續的裝置指派限制一些 vm，並不需要採取下列步驟。

1.  設定 「 自動停止動作 」 來關閉 VM 的執行

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>一些其他的 VM 準備工作是必要的圖形裝置

如果以特定方式設定的 VM 中的，有些硬體的效能更好。  如需為您的硬體需要下列設定的詳細資訊，請連絡硬體廠商。 其他詳細資料可於[規劃部署的裝置，使用不連續的裝置指派](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)和這[部落格文章。](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/)

1. 啟用在 CPU 上的寫入結合
   ```
   Set-VM -GuestControlledCacheTypes $true -VMName VMName
   ```
2. 設定 32 位元 MMIO 空間
   ```
   Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
   ```
3. 設定大於 32 位元 MMIO 空間
   ```
   Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
   ```
   請注意，上述的 MMIO 空間值會設為試驗單一 GPU 的合理值。  如果啟動 VM 之後, 裝置會回報關於資源不足的錯誤，您可能需要修改這些值。  此外，如果您要指派多個 Gpu，您必須增加這些值。

## <a name="dismount-the-device-from-the-host-partition"></a>卸載主機分割區的裝置
### <a name="optional---install-the-partitioning-driver"></a>選用-安裝的資料分割的驅動程式
不連續的裝置指派提供硬體廠商提供的安全性風險降低驅動程式使用其裝置的能力。  請注意，此驅動程式不會安裝在客體 VM 中的裝置驅動程式相同。  它具有最多提供此驅動程式的硬體廠商的酌情判斷，不過，如果它們並提供它，請安裝它之前至主機分割區從卸載該裝置。  請連絡硬體廠商，如需詳細資訊有風險降低驅動程式
> 如果未不提供任何資料分割的驅動程式，則在卸載時您必須使用`-force`選項以略過安全性警告。 請閱讀更多有關執行此動作上的安全性含意[規劃部署的裝置，使用不連續的裝置指派](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="locating-the-devices-location-path"></a>尋找裝置的位置路徑
PCI 位置路徑，才能卸除，並從主機中掛接裝置。  範例位置路徑看起來如下： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`。  上找到更多詳細資料的位置路徑可以在這裡找到：[規劃部署的裝置，使用不連續的裝置指派](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="disable-the-device"></a>停用裝置
使用裝置管理員或 PowerShell，請確定裝置"disabled"。  

### <a name="dismount-the-device"></a>卸除裝置
取決於如果廠商所提供的風險降低驅動程式，您將需要使用"-強制執行 」 選項或不。
- 如果已安裝的風險降低驅動程式
  ```
  Dismount-VMHostAssignableDevice -LocationPath $locationPath
  ```
- 如果未安裝風險降低驅動程式
  ```
  Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
  ```

## <a name="assigning-the-device-to-the-guest-vm"></a>將裝置指派給客體 VM
最後一個步驟是告訴 HYPER-V VM 應該有裝置的存取權。  除了上面找到的位置路徑，您必須知道 vm 的名稱。

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>什麼是下一步
裝置已成功在 VM 中裝載之後，您現在可以啟動該 VM，並像平常一樣如果您已在裸機系統上執行，與裝置互動。  這表示您現在可以在 VM 中安裝的硬體廠商的驅動程式，而且應用程式將能夠看到該硬體存在。  您可以驗證，請開啟 客體 VM 中的 裝置管理員，並看到硬體現在會出現。

## <a name="removing-a-device-and-returning-it-to-the-host"></a>移除裝置，並將其傳回給主應用程式
如果您想要傳回他裝置回到其原始狀態，您必須停止 VM，並發出下列：
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
您可以再重新啟用裝置管理員中的裝置和主機作業系統都能夠與裝置互動。

## <a name="examples"></a>範例

### <a name="mounting-a-gpu-to-a-vm"></a>掛接到 VM 的 GPU
在此範例中，我們可以使用 PowerShell 來設定名為"ddatest1"，取得依製造商 NVIDIA 提供第一個 GPU，並將它指派至 VM 的 VM。  
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
