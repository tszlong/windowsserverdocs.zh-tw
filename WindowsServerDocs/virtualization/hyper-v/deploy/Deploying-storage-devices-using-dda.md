---
title: 部署使用不連續的裝置指派的 NVMe 存放裝置
description: 了解如何部署存放裝置使用 DDA
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: d6fe54789d37386d5dc782ef8a2ca26b47adc69e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841359"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>部署使用不連續的裝置指派的 NVMe 存放裝置

>適用於：Microsoft HYPER-V Server 2016、windows Server 2016

從 Windows Server 2016 開始，您可以使用不連續的裝置指派] 或 [DDA，來將整個 PCIe 裝置傳遞至 VM。  這可讓如 NVMe 儲存體或從 VM 內的圖形卡的裝置存取高效能，同時能夠運用裝置的原生驅動程式。  請瀏覽[規劃部署的裝置，使用不連續的裝置指派](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)如需哪些裝置工作的詳細資訊，為何的潛在安全性風險，依此類推。有三個步驟 DDA 搭配使用的裝置：
-   設定 VM 的 DDA
-   卸載主機分割區的裝置
-   將裝置指派給客體 VM

身為系統管理員，可以在主機上的 Windows PowerShell 主控台執行所有命令。

## <a name="configure-the-vm-for-dda"></a>設定 VM 的 DDA
不連續的裝置指派限制一些 vm，並不需要採取下列步驟。

1.  設定 「 自動停止動作 」 來關閉 VM 的執行

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>卸載主機分割區的裝置

### <a name="locating-the-devices-location-path"></a>尋找裝置的位置路徑
PCI 位置路徑，才能卸除，並從主機中掛接裝置。  範例位置路徑看起來如下： `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`。   上找到更多詳細資料的位置路徑可以在這裡找到：[規劃部署的裝置，使用不連續的裝置指派](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)。

### <a name="disable-the-device"></a>停用裝置
使用裝置管理員或 PowerShell，請確定裝置"disabled"。  

### <a name="dismount-the-device"></a>卸除裝置
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>將裝置指派給客體 VM
最後一個步驟是告訴 HYPER-V VM 應該有裝置的存取權。  除了上面找到的位置路徑，您必須知道 vm 的名稱。

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>什麼是下一步
裝置已成功在 VM 中裝載之後，您現在可以啟動該 VM，並像平常一樣如果您已在裸機系統上執行，與裝置互動。  您可以驗證，請開啟 客體 VM 中的 裝置管理員，並看到硬體現在會出現。

## <a name="removing-a-device-and-returning-it-to-the-host"></a>移除裝置，並將其傳回給主應用程式
如果您想要傳回他裝置回到其原始狀態，您必須停止 VM，並發出下列：
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
您可以再重新啟用裝置管理員中的裝置和主機作業系統都能夠與裝置互動。
