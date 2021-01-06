---
title: 使用離散裝置指派部署 NVMe 儲存裝置
description: 瞭解如何使用 DDA 來部署存放裝置
ms.topic: article
ms.author: benarm
author: BenjaminArmstrong
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.date: 09/17/2020
ms.openlocfilehash: b80a4e18027add69c4f70e63174c571a2aa90c0b
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948124"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>使用離散裝置指派部署 NVMe 儲存裝置

>適用于： Microsoft Hyper-V Server 2016、Windows Server 2016

從 Windows Server 2016 開始，您可以使用離散裝置指派（或 DDA）將整個 PCIe 裝置傳遞至 VM。  這可讓裝置的高效能存取，例如 NVMe 儲存體或來自 VM 內的圖形卡，同時能夠利用裝置原生驅動程式。  請參閱 [使用離散裝置指派部署裝置的規劃](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) ，以取得哪些裝置適用的詳細資料、可能有哪些安全性含意等等。使用具有 DDA 的裝置有三個步驟：
-   設定 VM 以進行 DDA
-   從主機磁碟分割卸載裝置
-   將裝置指派給來賓 VM

所有命令都可以在 Windows PowerShell 主控台的主機上以系統管理員身分執行。

## <a name="configure-the-vm-for-dda"></a>設定 VM 以進行 DDA
離散裝置指派會對 Vm 強加一些限制，而且必須採取下列步驟。

1.  藉由執行，將 VM 的「自動停止動作」設定為 TurnOff

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>從主機磁碟分割卸載裝置

### <a name="locating-the-devices-location-path"></a>找出裝置的位置路徑
必須要有 PCI 位置路徑，才能從主機卸載並掛接裝置。  範例位置路徑看起來如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"` 。   您可以在這裡找到位置路徑的詳細資訊： [規劃使用離散裝置指派來部署裝置](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="disable-the-device"></a>停用裝置
使用裝置管理員或 PowerShell，確認裝置為「已停用」。

### <a name="dismount-the-device"></a>卸載裝置
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>將裝置指派給來賓 VM
最後一個步驟是告知 Hyper-v VM 應該具有裝置的存取權。  除了上述的位置路徑之外，您還需要知道 vm 的名稱。

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>後續步驟
在裝置成功掛接到 VM 之後，您現在可以啟動該 VM，並與裝置互動，如同您在裸機系統上執行一般。  您可以在來賓 VM 中開啟 [裝置管理員]，並看到現在已顯示硬體，來確認這一點。

## <a name="removing-a-device-and-returning-it-to-the-host"></a>移除裝置並將它返回主機
如果您想要讓裝置回到其原始狀態，您將需要停止 VM 併發出下列各項：
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
然後您可以在 [裝置管理員] 中重新啟用裝置，主機作業系統就能夠再次與裝置互動。
