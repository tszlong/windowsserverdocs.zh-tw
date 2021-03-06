---
title: 管理 vRSS
description: 在本主題中，您會使用 Windows PowerShell 命令來管理虛擬機器中 (Vm) 和 Hyper-v 主機上的 vRSS。
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 553be94dd3fe94a74ee9deb84a5ac6d47265afec
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955535"
---
# <a name="manage-vrss"></a>管理 vRSS

在本主題中，您會使用 Windows PowerShell 命令來管理虛擬機器 \( vm \) 和 hyper-v 主機上的 vRSS \- 。

>[!NOTE]
>如需本主題中所述之命令的詳細資訊，請參閱[適用于 RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

## <a name="vmq-on-hyper-v-hosts"></a>Hyper-v 主機上的 VMQ

在 Hyper-v 主機上，您必須使用控制 VMQ 處理器的關鍵字。

**查看目前的設定：**

```PowerShell
Get-NetAdapterVmq
```

**設定 VMQ 設定：**

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>Hyper-v 交換器埠上的 vRSS

在 Hyper-v 主機上，您也必須在 Hyper-v \- 虛擬交換器埠上啟用 vRSS。

**查看目前的設定：**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```

下列兩個設定都應該為**True**。

- VrssEnabledRequested： True
- VrssEnabled： True

>[!IMPORTANT]
>在某些資源限制條件下，Hyper-v \- 虛擬交換器埠可能無法啟用這項功能。 這是暫時性的狀況，而且該功能可能會在後續的時間提供使用。
>
>如果**VrssEnabled**為**True**，則會為此 hyper-v 虛擬交換器埠啟用此功能， \- 也就是針對此 VM 或 vNIC。

**設定交換器埠 vRSS 設定：**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE

Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>Vm 和主機 Vnic 中的 vRSS

您可以使用原生 RSS 所用的相同命令來設定 Vm 和主機 Vnic 中的 vRSS 設定，這也是在主機 Vnic 上啟用 RSS 的方式。

**查看目前的設定：**

```PowerShell
Get-NetAdapterRSS
```

**設定 vRSS 設定：**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> 在 VM 內設定設定檔並不會影響工作的排程。 Hyper-v \- 會進行所有排程決策，並忽略 VM 內的設定檔。

## <a name="disable-vrss"></a>停用 vRSS

您可以停用 vRSS 來停用先前所述的任何設定。

- 停用實體 NIC 或 VM 的 VMQ。

  >[!CAUTION]
  >停用實體 NIC 上的 VMQ，會嚴重影響您的 Hyper-v \- 主機處理傳入封包的能力。

- 在 \- hyper-v 主機上的 Hyper-v 虛擬交換器埠上，停用 VM 的 vRSS \- 。

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- 針對 Hyper-v 主機上的 Hyper-v 虛擬交換器埠上的主機 vNIC 停用 vRSS \- \- 。

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- 在 vm 或主機內的 VM \( 或主機 vNIC 中停用 RSS \) \(\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
