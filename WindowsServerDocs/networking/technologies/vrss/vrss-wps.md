---
title: RSS 和 vRSS 的 Windows PowerShell 命令
description: 本主題中，您會學習如何快速找出有關 Windows PowerShell 命令接收端調整 (RSS) 和虛擬 RSS (vRSS) 的技術參考資訊。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/05/2018
ms.openlocfilehash: 10039388009e32c10d71067b835bad65db5607ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833259"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>RSS 和 vRSS 的 Windows PowerShell 命令

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，了解如何快速找出有關 Windows PowerShell 命令的技術參考資訊，如接收端調整\(RSS\)和 虛擬 RSS \(vRSS\)。

使用下列的 RSS 命令來設定多個處理器或多個核心的實體電腦上的 RSS。 您可以使用相同的命令，在虛擬機器上設定 vRSS \(VM\)執行支援的作業系統。 如需詳細資訊，請參閱 <<c0> [ 在 Windows PowerShell 中的網路介面卡 Cmdlet](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps)。

## <a name="configure-vmq"></a>設定 VMQ

vRSS 需要 VMQ 已啟用並設定。 您可以使用下列 Windows PowerShell 命令來管理 VMQ 設定。

- [Disable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## <a name="enable-and-configure-rss-on-a-native-host"></a>啟用和設定原生的主機上的 RSS

使用下列 PowerShell 命令來設定原生主機上的 RSS，以及管理 RSS 在 VM 中或在主機上虛擬 NIC (vNIC)。 部分這些命令的參數也可能會影響虛擬機器佇列\(VMQ\)在 HYPER-V 主機。  

>[!IMPORTANT]
>啟用 RSS 的 vm 或主機 vNIC 上是啟用及使用 vRSS 的必要條件。

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>啟用在 Hyper-v 上的 vRSS\-V 虛擬交換器連接埠

除了啟用 RSS 的 VM 中，vRSS 會要求您啟用在 Hyper-v 上的 vRSS\-V 虛擬交換器連接埠。 

判斷 vRSS 存在設定和啟用或停用 vm 的功能。

   **檢視目前的設定：** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **啟用此功能：**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>啟用或停用主機 vNIC 上 vRSS

判斷目前的設定，如 vRSS，以及啟用或停用主機 vNIC 的功能。

   **檢視目前的設定：** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **啟用或停用此功能：** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>HYPER-V 虛擬交換器連接埠上設定排程的模式 
>適用於：Windows Server 2019

在 Windows Server 2019，vRSS 可以更新用來以動態方式處理網路流量的邏輯處理器。  支援的驅動程式的裝置已預設啟用此排程模式。 

判斷有排程模式，在系統上，或修改排程的模式適用於 VM。

   **檢視目前的設定：** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **設定或修改排程的模式：**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>主機 vNIC 上設定排程的模式
>適用於：Windows Server 2019

若要判斷有排程模式，或修改主機 vNIC 的排程模式，請使用下列 Windows PowerShell 命令：

   **檢視目前的設定：** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **設定或修改排程的模式：** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## <a name="related-topics"></a>相關主題 
如需詳細資訊，請參閱下列參考主題。

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

如需詳細資訊，請參閱 <<c0> [ 虛擬接收端調整 (vRSS)](vrss-top.md)。