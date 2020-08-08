---
title: 在 vSwitch 中接收區段聯合 (RSC)
description: 在 vSwitch 中 (RSC) 的接收區段聯合是 Windows Server 2019 和 Windows 10 2018 年10月更新中的一項功能，可將多個 TCP 區段結合成較少但較大的區段，以協助降低主機 CPU 使用率並增加虛擬工作負載的輸送量。 較少的處理、大型區段 (結合) 比處理許多小型區段更有效率。
manager: dougkim
ms.topic: article
ms.author: dacuo
author: dcuomo
ms.date: 09/07/2018
ms.openlocfilehash: 66f0f72dc6a577030ad43103e5f9d08a9458e64d
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995097"
---
# <a name="rsc-in-the-vswitch"></a>VSwitch 中的 RSC
>適用於：Windows Server 2019

在 vSwitch 中 (RSC) 的接收區段聯合是 Windows Server 2019 和 Windows 10 2018 年10月更新中的一項功能，可將多個 TCP 區段結合成較少但較大的區段，以協助降低主機 CPU 使用率並增加虛擬工作負載的輸送量。 較少的處理、大型區段 (結合) 比處理許多小型區段更有效率。

Windows Server 2012 和更新版本包含僅限硬體卸載版本， (實作為技術的實體網路介面卡) （也稱為接收區段聯合）。 這個卸載版的 RSC 仍然可以在較新版本的 Windows 中使用。 不過，它與虛擬工作負載不相容，一旦實體網路介面卡連接到 vSwitch 之後，就會停用它。 如需僅限硬體版本的 RSC 的詳細資訊，請參閱[ (rsc) 的接收區段](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11))聯合。

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>在 vSwitch 中受益于 RSC 的案例

其資料路徑從這項功能中受益于虛擬交換器的工作負載。

例如：

-   裝載虛擬 Nic，包括：

    -   軟體定義網路

    -   Hyper-V 主機

    -   儲存空間 Direct

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
如需詳細資訊，請參閱[設定-VMSwitch](/powershell/module/hyper-v/set-vmswitch?view=win10-ps)。