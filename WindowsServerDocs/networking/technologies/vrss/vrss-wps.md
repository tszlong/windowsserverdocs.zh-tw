---
title: Windows PowerShell RSS 和 vRSS 的命令
description: 在本主題中，您將瞭解如何快速找出有關接收端調整的 Windows PowerShell 命令的技術參考資訊 (RSS) 和虛擬 RSS (vRSS) 。
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/05/2018
ms.openlocfilehash: a5cc6d3b95b842e806c9f2c9a5762e5f29ce4c7b
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96863967"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>Windows PowerShell RSS 和 vRSS 的命令

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，您將瞭解如何快速找出有關接收端調整 \( RSS \) 和虛擬 RSS vRSS Windows PowerShell 命令的技術參考資訊 \( \) 。

使用下列 RSS 命令，在具有多個處理器或多個核心的實體電腦上設定 RSS。 您可以使用相同的命令，在執行支援之作業系統的虛擬機器 VM 上設定 vRSS \( \) 。 如需詳細資訊，請參閱 [Windows PowerShell 中的網路介面卡 Cmdlet](/powershell/module/netadapter/)。

## <a name="configure-vmq"></a>設定 VMQ

vRSS 需要啟用和設定 VMQ。 您可以使用下列 Windows PowerShell 命令來管理 VMQ 設定。

- [停用-NetAdapterVmq](/powershell/module/netadapter/disable-netadaptervmq)
- [啟用-NetAdapterVmq](/powershell/module/netadapter/enable-netadaptervmq)
- [NetAdapterVmq](/powershell/module/netadapter/get-netadaptervmq)
- [設定-NetAdapterVmq](/powershell/module/netadapter/set-netadaptervmq)

## <a name="enable-and-configure-rss-on-a-native-host"></a>在原生主機上啟用和設定 RSS

使用下列 PowerShell 命令來設定原生主機上的 RSS，以及管理 VM 或主機虛擬 NIC 上的 RSS (vNIC) 。 這些命令的某些參數可能也會影響 \( \) hyper-v 主機中的虛擬機器佇列 VMQ。

>[!IMPORTANT]
>在 VM 或主機 vNIC 上啟用 RSS 是啟用和使用 vRSS 的先決條件。

- [停用-Get-netadapterrss](/powershell/module/netadapter/disable-netadapterrss)
- [啟用-Get-netadapterrss](/powershell/module/netadapter/enable-netadapterrss)
- [Get-netadapterrss](/powershell/module/netadapter/get-netadapterrss)
- [設定-Get-netadapterrss](/powershell/module/netadapter/Set-NetAdapterRss)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>在 Hyper-v \- 虛擬交換器埠上啟用 vRSS

除了在 VM 中啟用 RSS，vRSS 還要求您在 Hyper-v \- 虛擬交換器埠上啟用 vrss。

判斷 vRSS 的目前設定，並啟用或停用 VM 的功能。

   **查看目前的設定：**

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **啟用功能：**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>啟用或停用主機 vNIC 上的 vRSS

判斷 vRSS 的目前設定，並啟用或停用主機 vNIC 的功能。

   **查看目前的設定：**

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **啟用或停用此功能：**

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>設定 Hyper-v 虛擬交換器通訊埠上的排程模式
>適用於：Windows Server 2019

在 Windows Server 2019 中，vRSS 可以更新用來動態處理網路流量的邏輯處理器。  具有受支援驅動程式的裝置預設會啟用此排程模式。

判斷系統上的目前排程模式，或修改 VM 的排程模式。

   **查看目前的設定：**

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **設定或修改排程模式：**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>設定主機 vNIC 上的排程模式
>適用於：Windows Server 2019

若要判斷目前的排程模式或修改主機 vNIC 的排程模式，請使用下列 Windows PowerShell 命令：

   **查看目前的設定：**

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **設定或修改排程模式：**

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## <a name="related-topics"></a>相關主題
如需詳細資訊，請參閱下列參考主題。

- [VMNetworkAdapter](/powershell/module/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](/powershell/module/hyper-v/set-vmnetworkadapter)

如需詳細資訊，請參閱 [ (vRSS 的虛擬接收端調整) ](vrss-top.md)。
