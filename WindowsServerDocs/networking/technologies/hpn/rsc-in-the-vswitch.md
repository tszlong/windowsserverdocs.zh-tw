---
title: 在 vSwitch 中接收區段聯合 (RSC)
description: VSwitch 中的接收區段聯合（RSC）是 Windows Server 2019 和 Windows 10 2018 年10月更新中的一項功能，可透過將多個 TCP 區段結合成較少但較大的時間，協助降低主機 CPU 使用率並增加虛擬工作負載的輸送量部門. 較少的處理、大型區段（結合）比處理許多小型區段更有效率。
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.author: dacuo
author: dcuomo
ms.date: 09/07/2018
ms.openlocfilehash: 0ffb417728bbdb73d8fb462ff7783b17b511bcd3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814771"
---
# <a name="rsc-in-the-vswitch"></a>VSwitch 中的 RSC
>適用于： Windows Server 2019

VSwitch 中的接收區段聯合（RSC）是 Windows Server 2019 和 Windows 10 2018 年10月更新中的一項功能，可透過將多個 TCP 區段結合成較少但較大的時間，協助降低主機 CPU 使用率並增加虛擬工作負載的輸送量部門. 較少的處理、大型區段（結合）比處理許多小型區段更有效率。

Windows Server 2012 和更新版本包含硬體專用卸載版本（實作為實體網路介面卡），這項技術也稱為接收區段聯合。 這個卸載版的 RSC 仍然可以在較新版本的 Windows 中使用。 不過，它與虛擬工作負載不相容，一旦實體網路介面卡連接到 vSwitch 之後，就會停用它。 如需僅限硬體版本的 RSC 的詳細資訊，請參閱[接收區段聯合（RSC）](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11))。

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>在 vSwitch 中受益于 RSC 的案例

其資料路徑從這項功能中受益于虛擬交換器的工作負載。

例如，

-   裝載虛擬 Nic，包括：

    -   軟體定義的網路功能

    -   Hyper-V 主機

    -   儲存空間直接存取

-   Hyper-v 來賓虛擬 Nic

-   軟體定義網路 GRE 閘道

-   容器

與此功能不相容的工作負載包括：

-   軟體定義網路 IPSEC 閘道

-   啟用 SR-IOV 的虛擬 Nic

-   SMB 直接傳輸

## <a name="configure-rsc-in-the-vswitch"></a>在 vSwitch 中設定 RSC


根據預設，在外部 Vswitch 上會啟用 RSC。

**查看目前的設定：**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**啟用或停用 vSwitch 中的 RSC**


>[!IMPORTANT]
>重要：您可以立即啟用和停用 vSwitch 中的 RSC，而不會影響現有的連接。


**停用 vSwitch 中的 RSC**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**在 vSwitch 中重新啟用 RSC**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
如需詳細資訊，請參閱[設定-VMSwitch](https://docs.microsoft.com/powershell/module/hyper-v/set-vmswitch?view=win10-ps)。
