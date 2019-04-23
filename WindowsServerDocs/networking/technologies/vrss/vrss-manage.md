---
title: 管理 vRSS
description: 本主題中，您可以使用的 Windows PowerShell 命令來管理 vRSS 在虛擬機器 (Vm) 和 HYPER-V 主機上。
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856189"
---
# <a name="manage-vrss"></a>管理 vRSS

本主題中，您必須使用 Windows PowerShell 命令來管理虛擬機器中的 vRSS \(Vm\)並在 Hyper-v 上\-HYPER-V 主機。

>[!NOTE]
>如需有關本主題中所述的命令的詳細資訊，請參閱[Windows PowerShell 命令，RSS 和 vRSS](vrss-wps.md)。

## <a name="vmq-on-hyper-v-hosts"></a>在 HYPER-V 主機上的 VMQ

在 HYPER-V 主機上，您必須使用控制 VMQ 處理器的關鍵字。

**檢視目前的設定：** 

```PowerShell
Get-NetAdapterVmq
```

**設定 VMQ 設定：** 

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>vRSS hyper-v 交換器連接埠

在 HYPER-V 主機上，您也必須啟用 vRSS 上 Hyper-v\-V 虛擬交換器連接埠。

**檢視目前的設定：**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
這兩個下列的設定應該 **，則為 True**。 

- VrssEnabledRequested:True
- VrssEnabled:True
    
>[!IMPORTANT]
>某些資源的限制狀況下，Hyper-v\-V 虛擬交換器連接埠可能會無法啟用此功能。 這是暫時的狀況，並在後續的階段，功能可能會變成可用。
>
>如果**VrssEnabled**是 **，則為 True**，然後啟用此 Hyper-v 功能\-V 虛擬交換器連接埠，也就是此 VM 或 vNIC。

**設定交換器連接埠 vRSS 設定：**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>在 Vm 和主機 Vnic vRSS

您可以使用相同的命令，用於原生 RSS vRSS 中設定 Vm 和主機 Vnic，這也是在主機 Vnic 上啟用 RSS 的方式。  

**檢視目前的設定：**

```PowerShell
Get-NetAdapterRSS
```

**設定 vRSS 設定：**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> 設定在 VM 內的設定檔不會影響工作排程。 超\-V 讓所有排程決策，並忽略在 VM 內的設定檔。

## <a name="disable-vrss"></a>停用 vRSS

您可以停用 vRSS，若要停用的任何先前所述的設定。

- 停用 VMQ 的實體 NIC 或 VM。

  >[!CAUTION]
  >在實體中停用 VMQ NIC 會嚴重影響開發您的 Hyper\-主機來處理連入封包。

- 停用 Hyper-v 上的 VM vRSS\-Hyper V 虛擬交換器連接埠\-主機。

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- 停用主機 vNIC 上 Hyper-v 的 vRSS\-Hyper V 虛擬交換器連接埠\-主機。

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- 停用 VM 中的 RSS\(或主機 vNIC\) VM 內\(或主機上\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
