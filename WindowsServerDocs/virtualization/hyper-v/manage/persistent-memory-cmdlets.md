---
title: 設定持續性記憶體裝置的 HYPER-V Vm 的 Cmdlet
description: 如何設定持續性記憶體裝置的 HYPER-V Vm
ms.prod: windows-server-threshold
ms.service: na
manager: jasgroce
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
author: coreyp-at-msft
ms.author: coreyp
ms.openlocfilehash: fd1b04ce74f0b8d490529d2a7f65091f5847d0f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878179"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>設定持續性記憶體裝置的 HYPER-V Vm 的 Cmdlet

>適用於：Windows Server 2019

這篇文章會提供系統管理員和 IT 專業人員使用持續性記憶體 （也稱為儲存類別記憶體或 NVDIMM-N） 中設定 HYPER-V Vm 的相關資訊。 JDEC 相容的 NVDIMM-N 持續性記憶體裝置支援在 Windows Server 2016 和 Windows 10 中，並提供極低延遲的靜態裝置的位元層級存取。 在 Windows Server 2019 支援 VM 的持續性記憶體裝置。 

## <a name="create-a-persistent-memory-device-for-a-vm"></a>建立 VM 的持續性記憶體裝置

使用**[NEW-VHD](https://docs.microsoft.com/powershell/module/hyper-v/new-vhd?view=win10-ps)** cmdlet 來建立 VM 的持續性記憶體裝置。 裝置必須在現有的 NTFS DAX 磁碟區上建立。  新的檔案名稱副檔名 (.vhdpmem) 用來指定裝置是持續性記憶體裝置。 支援只固定的 VHD 檔案格式。

**範例：** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>建立具有持續性記憶體控制站的 VM



使用**NEW-VM cmdlet**來建立具有指定的記憶體大小及 VHDX 映像路徑的第 2 代 VM。 然後，使用**新增 VMPmemController**將持續性記憶體控制器新增至 VM。

**範例:** 
    
    New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

    Add-VMPmemController ProductionVM1x

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>將持續性記憶體裝置連接至 VM

使用**[Add-vmharddiskdrive](https://docs.microsoft.com/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** 將持續性記憶體裝置連接到 VM

**範例：** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

HYPER-V VM 內的持續性記憶體裝置顯示為持續性記憶體裝置来取用和管理客體作業系統。 客體作業系統可以做的區塊或 DAX 磁碟區的裝置。 在 VM 內的持續性記憶體裝置使用時為 DAX 磁碟區，它們會從低延遲位元層級位址能力的主機裝置 （程式碼路徑上沒有 I/O 虛擬化） 獲益。 

>[!NOTE] 
>持續性記憶體只有 HYPER-V Gen2 Vm 上執行。 即時移轉和存放裝置移轉不支援 Vm 的持續性記憶體。 生產檢查點的 Vm 不包含持續性記憶體狀態。 