---
title: RSS 」 和 「 vRSS 的 Windows PowerShell 命令
description: 本主題中，您將了解如何快速找出 Windows PowerShell 命令針對接收端調整 (RSS) 和虛擬 RSS (vRSS) 技術參考資訊。
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
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339266"
---
# RSS 」 和 「 vRSS 的 Windows PowerShell 命令

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題中，您將了解如何快速找出 Windows PowerShell 命令針對接收端調整 \(RSS\) 和虛擬 RSS \(vRSS\) 技術參考資訊。

使用下列的 RSS 命令來設定 RSS 具有多顆處理器或多個核心在實體電腦上。 您可以使用相同命令來設定 vRSS 的虛擬機器上執行支援之作業系統的 \(VM\)。 如需詳細資訊，請參閱[Windows PowerShell 中的網路介面卡 Cmdlet](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps)。

## 設定 VMQ

vRSS 需要 VMQ 已啟用並已設定。 您可以使用下列 Windows PowerShell 命令管理 VMQ 設定。

- [停用 NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [啟用 NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [設定 NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## 啟用與設定原生的主機上的 RSS

使用下列 PowerShell 命令來設定 RSS 原生的主機上，以及管理在 VM 中，或在主機上的 RSS 虛擬 NIC (vNIC)。 這些命令的參數的一些可能也會影響在 HYPER-V 主機中的虛擬機器佇列 \(VMQ\)。  

>[!IMPORTANT]
>啟用 RSS 在 VM 中，或在主機 vNIC 是啟用及使用 vRSS 的必要條件。

- [停用 NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [啟用 NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [設定 NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## 啟用 HYPER-V 虛擬交換器連接埠上 vRSS

啟用 RSS VM 中的，除了 vRSS 還需要您啟用 vRSS 上的 HYPER-V 虛擬交換器連接埠。 

判斷 vRSS 的存在設定並啟用或停用此功能的 VM。

   **檢視目前的設定：** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **在啟用功能：**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## 啟用或停用在主機 vNIC vRSS

判斷目前的設定，如 vRSS，並啟用或停用主機 vNIC 的功能。

   **檢視目前的設定：** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **啟用或停用此功能：** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## 在 HYPER-V 虛擬交換器連接埠上設定排程模式 
>適用於： Windows Server 2019

在 Windows Server 2019，vRSS 可以更新用來以動態方式處理的網路流量的邏輯處理器。  支援的驅動程式的裝置都有預設啟用此排程模式。 

判斷存在的排程模式在系統上，或修改排程模式的 VM。

   **檢視目前的設定：** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **設定，或修改排程的模式：**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## 在主機 vNIC 上設定排程模式
>適用於： Windows Server 2019

若要判斷存在的排程模式，或修改主機 vNIC 的排程模式，請使用下列 Windows PowerShell 命令：

   **檢視目前的設定：** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **設定，或修改排程的模式：** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## 相關主題 
如需詳細資訊，請參閱下列參考主題。

- [Get VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [設定 VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

如需詳細資訊，請參閱[虛擬接收端調整 (vRSS)](vrss-top.md)。