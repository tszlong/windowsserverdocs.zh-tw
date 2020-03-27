---
title: 設定和檢視 Hyper-V 虛擬交換器連接埠上的 VLAN 設定
description: 您可以使用本主題來瞭解在 Windows Server 2016 中設定和查看 Hyper-v 虛擬交換器埠上的虛擬區域網路（VLAN）設定的最佳作法。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 083558762051283115211d10d32ebb6fd3ad3953
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308038"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>設定和檢視 Hyper-V 虛擬交換器連接埠上的 VLAN 設定

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解在 Hyper-v 虛擬交換器埠上設定和查看虛擬區域網路（VLAN）設定的最佳作法。

當您想要在 Hyper-v 虛擬交換器埠上設定 VLAN 設定時，您可以使用 Windows&reg; Server 2016 Hyper-v 管理員或 System Center Virtual Machine Manager （VMM）。

如果您使用 VMM，VMM 會使用下列 Windows PowerShell 命令來設定交換器埠。

```
Set-VMNetworkAdapterIsolation <VM-name|-managementOS> -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
如果您不是使用 VMM，而且正在設定 Windows Server 中的交換器埠，您可以使用 Hyper-v 管理員主控台或下列 Windows PowerShell 命令。
```
Set-VMNetworkAdapterVlan <VM-name|-managementOS> -Access -VlanID <vlan-value>
```

由於這兩種方法可以在 Hyper-v 虛擬交換器埠上進行 VLAN 設定，因此當您嘗試查看交換器埠設定時，您會看到 VLAN 設定尚未設定，即使已設定也是如此。

## <a name="use-the-same-method-to-configure-and-view-switch-port-vlan-settings"></a>使用相同的方法來設定及查看交換器埠 VLAN 設定

為確保您不會遇到這些問題，您必須使用相同的方法來查看您用來設定交換器埠 VLAN 設定的交換器埠 VLAN 設定。

若要設定及查看 VLAN 交換器埠設定，您必須執行下列動作：

- 如果您使用 VMM 或網路控制卡來設定和管理您的網路，而且您已部署軟體定義網路功能（SDN），則必須使用**VMNetworkAdapterIsolation** Cmdlet。 
- 如果您使用 Windows Server 2016 Hyper-v 管理員或 Windows PowerShell Cmdlet，而且您尚未部署軟體定義網路功能（SDN），則必須使用**set-vmnetworkadaptervlan** Cmdlet。

### <a name="possible-issues"></a>可能的問題

如果您未遵循這些指導方針，您可能會遇到下列問題。

- 在您已部署 SDN 並使用 VMM、網路控制站或**VMNetworkAdapterIsolation** Cmdlet 來設定 Hyper-v 虛擬交換器埠上的 VLAN 設定的情況下：如果您使用 hyper-v 管理員或**取得 set-vmnetworkadaptervlan**來查看設定，命令輸出將不會顯示您的 VLAN 設定。 相反地，您必須使用**VMNetworkIsolation** Cmdlet 來查看 VLAN 設定。
- 如果您未部署 SDN，而改為使用 Hyper-v 管理員或**set-vmnetworkadaptervlan** Cmdlet 來設定 Hyper-v 虛擬交換器埠上的 VLAN 設定：如果您使用**VMNetworkIsolation** Cmdlet 來查看設定，命令輸出將不會顯示您的 VLAN 設定。 相反地，您必須使用**Get set-vmnetworkadaptervlan** Cmdlet 來查看 VLAN 設定。

也請務必不要嘗試使用這兩種設定方法來設定相同的交換器埠 VLAN 設定。 如果您這樣做，交換器埠的設定不正確，且結果可能是網路通訊失敗。

### <a name="resources"></a>資源

如需本主題中所述之 Windows PowerShell 命令的詳細資訊，請參閱下列各項：

- [設定-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464283.aspx)
- [VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464277.aspx)
- [設定-Set-vmnetworkadaptervlan](https://technet.microsoft.com/library/hh848475.aspx)
- [Set-vmnetworkadaptervlan](https://technet.microsoft.com/library/hh848516.aspx)





