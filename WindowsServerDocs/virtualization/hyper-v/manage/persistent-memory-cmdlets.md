---
title: 用於設定 Hyper-v Vm 的持續性記憶體裝置的 Cmdlet
description: 如何設定 Hyper-v Vm 的持續性記憶體裝置
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
ms.author: benarm
author: BenjaminArmstrong
ms.openlocfilehash: 882e63b2119a2c6483234ef7993b47f0aeaf7915
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90744053"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>用於設定 Hyper-v Vm 的持續性記憶體裝置的 Cmdlet

>適用於：Windows Server 2019

本文提供系統管理員和 IT 專業人員有關使用持續性記憶體設定 Hyper-v Vm (也稱為儲存類別記憶體或 NVDIMM-N) 的相關資訊。 Windows Server 2016 和 Windows 10 支援符合 JDEC 規範的 NVDIMM-N 持續性記憶體裝置，並為極低延遲的非暫時性裝置提供位元組層級存取。 Windows Server 2019 支援 VM 持續性記憶體裝置。

## <a name="create-a-persistent-memory-device-for-a-vm"></a>建立 VM 的持續性記憶體裝置

使用 **[新的-VHD](/powershell/module/hyper-v/new-vhd?view=win10-ps)** Cmdlet 來建立 VM 的持續性記憶體裝置。 您必須在現有的 NTFS DAX 磁片區上建立裝置。  新的副檔名 (. vhdpmem) 用來指定裝置為持續性記憶體裝置。 只支援固定的 VHD 檔案格式。

**範例：** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>建立具有持續性記憶體控制器的 VM

使用 **新的-VM Cmdlet** 來建立第2代 VM，其中具有指定的記憶體大小和 VHDX 映射的路徑。 然後，使用 **VMPmemController** 將持續性記憶體控制器新增至 VM。

**範例︰**

```powershell
New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

Add-VMPmemController ProductionVM1x
```

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>將持續性記憶體裝置連結至 VM

使用 **[get-vmharddiskdrive](/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** 將持續性記憶體裝置連結至 VM

**範例：** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

Hyper-v VM 內的持續性記憶體裝置會顯示為可由客體作業系統取用及管理的持續性記憶體裝置。 客體作業系統可以將裝置當作區塊或 DAX 磁片區使用。 當 VM 內的持續性記憶體裝置作為 DAX 磁片區使用時，這些裝置會受益于主機裝置的低延遲位元組層級位址功能， (程式碼路徑) 的 i/o 虛擬化。

>[!NOTE]
>只有 Hyper-v Gen2 Vm 支援持續性記憶體。 具有持續性記憶體的 Vm 不支援即時移轉和儲存體遷移。 Vm 的生產檢查點不包含持續性的記憶體狀態。