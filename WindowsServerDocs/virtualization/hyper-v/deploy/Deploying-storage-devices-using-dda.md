---
title: 使用離散裝置指派部署 NVMe 存放裝置
description: 瞭解如何使用 DDA 來部署存放裝置
ms.prod: windows-server
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: eb76b25e8ff1428b2c03b37dde1f76562751d3bb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364322"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>使用離散裝置指派部署 NVMe 存放裝置

>適用於：Microsoft Hyper-v Server 2016、Windows Server 2016

從 Windows Server 2016 開始，您可以使用離散裝置指派或 DDA，將整個 PCIe 裝置傳遞至 VM。  這可讓您對裝置的高效能存取，例如從 VM 內 NVMe 儲存體或圖形卡，同時能夠利用裝置原生驅動程式。  請流覽[使用離散裝置指派來部署裝置的計畫](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)，以取得更多裝置的工作、可能的安全性含意等等。使用具有 DDA 的裝置有三個步驟：
-   為 DDA 設定 VM
-   從主機磁碟分割卸載裝置
-   將裝置指派給來賓 VM

所有命令都可以在 Windows PowerShell 主控台的主機上，以系統管理員身分執行。

## <a name="configure-the-vm-for-dda"></a>為 DDA 設定 VM
個別的裝置指派會對 Vm 施加一些限制，而且必須採取下列步驟。

1.  藉由執行，將 VM 的「自動停止動作」設定為 Turnon

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>從主機磁碟分割卸載裝置

### <a name="locating-the-devices-location-path"></a>尋找裝置的位置路徑
必須要有 PCI 位置路徑，才能從主機卸載並掛接裝置。  範例位置路徑看起來如下所示： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`。   如需位置路徑的詳細資訊，請參閱：[規劃使用離散裝置指派來部署裝置](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="disable-the-device"></a>停用裝置
使用 Device Manager 或 PowerShell，確定裝置已「停用」。  

### <a name="dismount-the-device"></a>卸載裝置
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>將裝置指派給來賓 VM
最後一個步驟是告訴 Hyper-v，VM 應具有裝置的存取權。  除了上面找到的位置路徑，您還必須知道 vm 的名稱。

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>下一步
在 VM 中成功掛接裝置之後，您現在可以啟動該 VM 並與裝置互動，如同您在裸機系統上執行一般。  您可以在來賓 VM 中開啟 [裝置管理員]，並看到硬體現在顯示，以驗證這一點。

## <a name="removing-a-device-and-returning-it-to-the-host"></a>移除裝置並將它傳回主機
如果您想要讓裝置回到其原始狀態，您將需要停止 VM 併發出下列問題：
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
接著，您可以在 [裝置管理員] 中重新啟用裝置，主機作業系統將能夠再次與裝置互動。
