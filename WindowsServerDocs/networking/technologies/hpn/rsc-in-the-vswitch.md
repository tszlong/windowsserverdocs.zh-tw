---
title: 在 vSwitch 中接收區段聯合 (RSC)
description: 接收區段聯合 (RSC) 中的 vSwitch 是在 Windows Server 2019 的功能和 Windows 10 年 10 月 2018 Update 可協助藉由聯合成較少但較大的多個 TCP 區段，降低主機 CPU 使用率和虛擬工作負載的增加輸送量區段。 處理較少的區段 （聯集） 會更有效率，比處理很多，小型的區段。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.author: dacuo
author: shortpatti
ms.date: 09/07/2018
ms.openlocfilehash: 667e795e398443cadd4c966cc31e65eeee4962f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827779"
---
# <a name="rsc-in-the-vswitch"></a>RSC 在 vSwitch
>適用於：Windows Server 2019

接收區段聯合 (RSC) 中的 vSwitch 是在 Windows Server 2019 的功能和 Windows 10 年 10 月 2018 Update 可協助藉由聯合成較少但較大的多個 TCP 區段，降低主機 CPU 使用率和虛擬工作負載的增加輸送量區段。 處理較少的區段 （聯集） 會更有效率，比處理很多，小型的區段。

Windows Server 2012 及更新版本包含的技術，也就是接收區段聯合的僅限硬體的卸載版本 （已實作於實體網路介面卡）。 Windows 的更新版本中仍然可用此卸載的版本，才可以使用 rsc。 不過，它會與虛擬工作負載不相容，並在實體網路介面卡附加至 vSwitch 已停用。 如需有關僅限硬體的版本，才可以使用 rsc 的詳細資訊，請參閱[接收區段聯合 (RSC)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11))。

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>在 vSwitch 受益 RSC 案例

其資料路徑周遊虛擬交換器的工作負載受益於這項功能。

例如: 

-   包括主機的虛擬 Nic:

    -   軟體定義的網路功能

    -   Hyper-V 主機

    -   儲存空間直接存取

-   HYPER-V 客體的虛擬 Nic

-   軟體定義網路 GRE 閘道

-   容器

與這項功能不相容的工作負載包括：

-   軟體定義網路 IPSEC 閘道

-   SR-IOV 啟用虛擬 Nic

-   SMB 直接傳輸

## <a name="configure-rsc-in-the-vswitch"></a>設定 RSC 在 vSwitch


根據預設，在外部 Vswitch，會啟用 RSC。

**檢視目前的設定：**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**啟用或停用 RSC，vSwitch 中**


>[!IMPORTANT]
>重要事項：可啟用及停用現有的連線不會影響即時在 vSwitch 的 RSC。


**停用 RSC vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**重新啟用在 vSwitch 的 RSC**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
如需詳細資訊，請參閱 < [Set-vmswitch](https://docs.microsoft.com/powershell/module/hyper-v/set-vmswitch?view=win10-ps)。
