---
title: 管理 vRSS
description: 本主題中，您可以使用 Windows PowerShell 命令來管理 vRSS HYPER-V 主機和虛擬機器 (Vm) 中。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8af800608bee7037b48141a7a2edb0c872a7aac0
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133834"
---
# 管理 vRSS

本主題中，您可以使用 Windows PowerShell 命令來管理 vRSS 在虛擬機器 \(VMs\)，在 HYPER-V 主機上。

>[!NOTE]
>如需有關此主題中提及的指令的詳細資訊，請參閱[RSS 」 和 「 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

## 在 HYPER-V 主機上的 VMQ

HYPER-V 主機上，您必須使用控制 VMQ 處理器的關鍵字。

**檢視目前的設定：** 

```PowerShell
Get-NetAdapterVmq
```

**設定 VMQ 設定：** 

```PowerShell
Set-NetAdapterVmq
```


## 在 HYPER-V 上的 vRSS 切換連接埠

HYPER-V 主機上，您也必須啟用 vRSS 上的 HYPER-V 虛擬交換器連接埠。

**檢視目前的設定：**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
這兩個下列設定應該 **，則為 True**。 

- VrssEnabledRequested: True
- VrssEnabled: True
    
>[!IMPORTANT]
>某些資源限制條件下，HYPER-V 虛擬交換器連接埠可能無法啟用此功能。 這是暫時性的條件，以及功能可能會在後續次數可用。
>
>如果**VrssEnabled** **，則為 True**，則此功能已啟用此 HYPER-V 虛擬交換器連接埠 — 也就是此 VM 或 vNIC。

**設定交換器連接埠 vRSS 設定：**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## 在虛擬機器和主機 Vnic vRSS

您可以使用相同的命令的原生 RSS 用來設定在虛擬機器和主機 Vnic vRSS 設定，這也是在主機 Vnic 上啟用 RSS 的方式。  

**檢視目前的設定：**

```PowerShell
Get-NetAdapterRSS
```

**設定 vRSS 設定：**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> 設定在 VM 內的設定檔不會影響排程的工作。 HYPER-V 可讓所有排程的決策，並忽略在 VM 內的設定檔。

## 停用 vRSS

您可以停用 vRSS 停用任何先前所述的設定。

- 停用 VMQ 實體 NIC 或 VM。

  >[!CAUTION]
  >停用 VMQ 實體 NIC 會嚴重影響您的 HYPER-V 主機能夠處理連入的封包。

- 停用在 HYPER-V 主機上的 HYPER-V 虛擬交換器連接埠 VM vRSS。

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- 停用主機 vNIC 在 HYPER-V 主機上的 HYPER-V 虛擬交換器連接埠的 vRSS。

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- 停用在 VM 中的 RSS \(or host vNIC\) 在 VM 內 \ （或 host\）

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
