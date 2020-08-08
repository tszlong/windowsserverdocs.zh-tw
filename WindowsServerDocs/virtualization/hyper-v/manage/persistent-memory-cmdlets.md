---
title: 用於設定 Hyper-v Vm 之持續性記憶體裝置的 Cmdlet
description: 如何設定 Hyper-v Vm 的持續性記憶體裝置
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
author: coreyp-at-msft
ms.author: coreyp
ms.openlocfilehash: 471dd9a7c3feb148b9ce27ec7ac4d714be0e86af
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996694"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>用於設定 Hyper-v Vm 之持續性記憶體裝置的 Cmdlet

>適用於：Windows Server 2019

本文提供系統管理員和 IT 專業人員有關設定具有持續性記憶體之 Hyper-v Vm 的資訊 (也稱為儲存類別記憶體或 NVDIMM-N) 。 Windows Server 2016 和 Windows 10 支援 JDEC 相容的 NVDIMM-N 持續性記憶體裝置，並為非常低延遲的非 volatile 裝置提供位元組層級的存取。 Windows Server 2019 支援 VM 持續性記憶體裝置。

## <a name="create-a-persistent-memory-device-for-a-vm"></a>為 VM 建立持續性記憶體裝置

使用**[新的-VHD](/powershell/module/hyper-v/new-vhd?view=win10-ps)** Cmdlet 來建立 VM 的持續性記憶體裝置。 裝置必須在現有的 NTFS DAX 磁片區上建立。  新的檔案名延伸 (. vhdpmem) 用來指定裝置是持續性記憶體裝置。 僅支援固定的 VHD 檔案格式。

**範例：** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>建立具有持續性記憶體控制器的 VM

使用**新的-VM Cmdlet**來建立具有指定之記憶體大小和 VHDX 映射路徑的第2代 VM。 然後，使用**VMPmemController**將持續性記憶體控制器新增至 VM。

**範例︰**

```powershell
New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

Add-VMPmemController ProductionVM1x
```

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>將持續性記憶體裝置連接至 VM

使用**[add-vmharddiskdrive](/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** 將持續性記憶體裝置附加至 VM

**範例：** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

Hyper-v VM 內的持續性記憶體裝置會顯示為持續性記憶體裝置，以供客體作業系統取用及管理。 客體作業系統可以使用裝置做為區塊或 DAX 磁片區。 當 VM 內的持續性記憶體裝置做為 DAX 磁片區使用時，它們會從低延遲的位元組層級位址中獲益-主機裝置 (在程式碼路徑) 不會有 i/o 虛擬化。

>[!NOTE]
>只有 Hyper-v Gen2 Vm 支援持續性記憶體。 具有持續性記憶體的 Vm 不支援即時移轉和儲存體遷移。 Vm 的生產檢查點不包含持續性記憶體狀態。