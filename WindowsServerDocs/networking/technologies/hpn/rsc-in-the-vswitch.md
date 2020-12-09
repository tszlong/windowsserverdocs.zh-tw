---
title: 在 vSwitch 中接收區段聯合 (RSC)
description: 在 vSwitch 中 (RSC) 的接收區段聯合是 Windows Server 2019 和 Windows 10 2018 年10月更新中的一項功能，可協助降低主機 CPU 使用率並增加虛擬工作負載的輸送量，方法是將多個 TCP 區段聯合成較少但較大的區段。 處理較少的大型區段 (結合的) 比處理許多較小的區段更有效率。
manager: dougkim
ms.topic: article
ms.author: dacuo
author: dcuomo
ms.date: 09/07/2018
ms.openlocfilehash: b9288e289da063b9a6175a9409154940a6d19d1f
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866247"
---
# <a name="rsc-in-the-vswitch"></a>VSwitch 中的 RSC
>適用於：Windows Server 2019

在 vSwitch 中 (RSC) 的接收區段聯合是 Windows Server 2019 和 Windows 10 2018 年10月更新中的一項功能，可協助降低主機 CPU 使用率並增加虛擬工作負載的輸送量，方法是將多個 TCP 區段聯合成較少但較大的區段。 處理較少的大型區段 (結合的) 比處理許多較小的區段更有效率。

Windows Server 2012 和更新版本包含僅限硬體的卸載版本， (在實體網路介面卡的技術) （也稱為接收區段聯合）中執行。 舊版的 Windows 仍有提供此卸載版本的 RSC。 不過，它與虛擬工作負載不相容，而且在實體網路介面卡附加至 vSwitch 之後，已停用。 如需僅限硬體版本的 RSC 的詳細資訊，請參閱 [接收區段聯合 (rsc) ](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11))。

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>從 vSwitch 中的 RSC 獲益的案例

資料路徑流經虛擬交換器的工作負載會從此功能獲益。

例如：

-   主機虛擬 Nic，包括：

    -   軟體定義網路

    -   Hyper-V 主機

    -   儲存空間直接存取

-   Hyper-v 來賓虛擬 Nic

-   軟體定義網路 GRE 閘道

-   容器

與這項功能不相容的工作負載包括：

-   軟體定義網路 IPSEC 閘道

-   SR-IOV 已啟用虛擬 Nic

-   SMB 直接傳輸

## <a name="configure-rsc-in-the-vswitch"></a>在 vSwitch 中設定 RSC


依預設，在外部 Vswitch 上會啟用 RSC。

**查看目前的設定：**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**在 vSwitch 中啟用或停用 RSC**


>[!IMPORTANT]
>重要事項：在此情況下，您可以在不影響現有連接的情況下，即時啟用和停用 RSC。


**在 vSwitch 中停用 RSC**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**在 vSwitch 中重新啟用 RSC**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
如需詳細資訊，請參閱 [設定-VMSwitch](/powershell/module/hyper-v/set-vmswitch)。
