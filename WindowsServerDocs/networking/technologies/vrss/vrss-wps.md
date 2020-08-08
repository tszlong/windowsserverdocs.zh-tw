---
title: 適用于 RSS 和 vRSS 的 Windows PowerShell 命令
description: 在本主題中，您將瞭解如何快速找出關於接收端調整的 Windows PowerShell 命令的技術參考資訊 (RSS) 和虛擬 RSS (vRSS) 。
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/05/2018
ms.openlocfilehash: 6b44cdfec4778cf7f36f541021f23a073cb17806
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964004"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>適用于 RSS 和 vRSS 的 Windows PowerShell 命令

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，您將瞭解如何快速找出關於接收端調整 \( RSS \) 和虛擬 RSS VRSS 的 Windows PowerShell 命令的技術參考資訊 \( \) 。

使用下列 RSS 命令，在具有多個處理器或多個核心的實體電腦上設定 RSS。 您可以使用相同的命令，在執行支援之作業系統的虛擬機器 VM 上設定 vRSS \( \) 。 如需詳細資訊，請參閱[Windows PowerShell 中的網路介面卡 Cmdlet](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps)。

## <a name="configure-vmq"></a>設定 VMQ

vRSS 需要啟用和設定 VMQ。 您可以使用下列 Windows PowerShell 命令來管理 VMQ 設定。

- [停用-Get-netadaptervmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [啟用-Get-netadaptervmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-netadaptervmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [設定-Get-netadaptervmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## <a name="enable-and-configure-rss-on-a-native-host"></a>在原生主機上啟用和設定 RSS

使用下列 PowerShell 命令來設定原生主機上的 RSS，以及管理 VM 或主機虛擬 NIC 上的 RSS (vNIC) 。 這些命令的某些參數可能也會影響 \( \) hyper-v 主機中的虛擬機器佇列 VMQ。

>[!IMPORTANT]
>在 VM 或主機 vNIC 上啟用 RSS 是啟用和使用 vRSS 的必要條件。

- [停用-Set-netadapterrss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [啟用-Set-netadapterrss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Set-netadapterrss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [設定-Set-netadapterrss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>在 Hyper-v \- 虛擬交換器埠上啟用 vRSS

除了在 VM 中啟用 RSS，vRSS 也會要求您在 Hyper-v \- 虛擬交換器埠上啟用 vrss。

判斷 vRSS 的目前設定，並啟用或停用 VM 的功能。

   **查看目前的設定：**

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **已啟用此功能：**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>在主機 vNIC 上啟用或停用 vRSS

判斷 vRSS 的目前設定，以及啟用或停用主機 vNIC 的功能。

   **查看目前的設定：**

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **啟用或停用功能：**

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>設定 Hyper-v 虛擬交換器埠上的排程模式
>適用於：Windows Server 2019

在 Windows Server 2019 中，vRSS 可以更新用來動態處理網路流量的邏輯處理器。  具有支援之驅動程式的裝置預設會啟用此排程模式。

判斷系統上目前的排程模式，或修改 VM 的排程模式。

   **查看目前的設定：**

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **設定或修改排程模式：**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>在主機 vNIC 上設定排程模式
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

- [VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

如需詳細資訊，請參閱[虛擬接收端調整 (vRSS) ](vrss-top.md)。