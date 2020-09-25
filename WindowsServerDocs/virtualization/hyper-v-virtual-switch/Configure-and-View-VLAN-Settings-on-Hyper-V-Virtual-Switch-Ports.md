---
title: 設定和檢視 Hyper-V 虛擬交換器連接埠上的 VLAN 設定
description: 您可以使用本主題來瞭解在 Windows Server 2016 的 Hyper-v 虛擬交換器埠上設定和觀看虛擬區域網路絡 (VLAN) 設定的最佳做法。
manager: brianlic
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 85d622094ac81aea8a2d90e4ef6eb5d226d0b290
ms.sourcegitcommit: 50b295002d60f4183f452cc169f0768a347830ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2020
ms.locfileid: "91248581"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>設定和檢視 Hyper-V 虛擬交換器連接埠上的 VLAN 設定

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解在 Hyper-v 虛擬交換器埠上設定和觀看虛擬區域網路絡 (VLAN) 設定的最佳做法。

當您想要在 Hyper-v 虛擬交換器埠上設定 VLAN 設定時，您可以使用 Windows &reg; Server 2016 Hyper-v 管理員或 System Center Virtual Machine Manager (VMM) 。

如果您使用 VMM，VMM 會使用下列 Windows PowerShell 命令來設定交換器埠。

```powershell
Set-VMNetworkAdapterIsolation <VM-name|-ManagementOS -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
如果您不是使用 VMM，而且要在 Windows Server 中設定交換器埠，則可以使用 Hyper-v 管理員主控台或下列 Windows PowerShell 命令。
```powershell
Set-VMNetworkAdapterVlan <VM-name|-managementOS> -Access -VlanID <vlan-value>
```

因為這兩種方法可以在 Hyper-v 虛擬交換器埠上設定 VLAN 設定，所以當您嘗試查看切換通訊埠設定時，您可能會看到未設定 VLAN 設定，即使它們已設定亦同。

## <a name="use-the-same-method-to-configure-and-view-switch-port-vlan-settings"></a>使用相同的方法來設定和查看切換埠 VLAN 設定

為確保您不會遇到這些問題，您必須使用相同的方法來查看您用來設定交換器埠 VLAN 設定的切換埠 VLAN 設定。

若要設定和查看 VLAN 交換器埠設定，您必須執行下列動作：

- 如果您使用 VMM 或網路控制站來設定和管理網路，而且您已將軟體定義的網路功能部署 (SDN) ，您必須使用 **set-vmnetworkadapterisolation** Cmdlet。
- 如果您使用的是 Windows Server 2016 Hyper-v 管理員或 Windows PowerShell Cmdlet，而且您尚未將軟體定義的網路功能部署 (SDN) ，則必須使用 **get-vmnetworkadaptervlan** Cmdlet。

### <a name="possible-issues"></a>可能的問題

如果您未遵循這些指導方針，您可能會遇到下列問題。

- 在您已部署 SDN 的情況下，並使用 VMM、網路控制站或 **set-vmnetworkadapterisolation** Cmdlet 來設定 Hyper-v 虛擬交換器埠上的 VLAN 設定：如果您使用 hyper-v 管理員或 **取得 get-vmnetworkadaptervlan** 來查看設定，命令輸出不會顯示您的 vlan 設定。 相反地，您必須使用 **VMNetworkIsolation** 指令 Cmdlet 來查看 VLAN 設定。
- 在您尚未部署 SDN 的情況下，改為使用 Hyper-v 管理員或 **get-vmnetworkadaptervlan** Cmdlet 來設定 Hyper-v 虛擬交換器通訊埠上的 vlan 設定：如果您使用 **VMNetworkIsolation 指令程式** 來查看設定，命令輸出不會顯示您的 VLAN 設定。 相反地，您必須使用 **Get get-vmnetworkadaptervlan** Cmdlet 來查看 VLAN 設定。

也請務必不要嘗試使用這兩種設定方法來設定相同的交換器埠 VLAN 設定。 如果您這樣做，切換埠的設定不正確，且結果可能是網路通訊失敗。

### <a name="resources"></a>資源

如需有關本主題中所述 Windows PowerShell 命令的詳細資訊，請參閱下列主題：

- [設定-Set-vmnetworkadapterisolation](/powershell/module/hyper-v/set-vmnetworkadapterisolation?view=win10-ps)
- [Set-vmnetworkadapterisolation](/powershell/module/hyper-v/get-vmnetworkadapterisolation?view=win10-ps)
- [設定-Get-vmnetworkadaptervlan](/powershell/module/hyper-v/set-vmnetworkadaptervlan?view=win10-ps)
- [Get-vmnetworkadaptervlan](/powershell/module/hyper-v/get-vmnetworkadaptervlan?view=win10-ps)
