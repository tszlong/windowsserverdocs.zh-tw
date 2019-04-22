---
title: 設定和檢視 Hyper-V 虛擬交換器連接埠上的 VLAN 設定
description: 您可以使用本主題以了解設定及檢視 Windows Server 2016 中的 HYPER-V 虛擬交換器連接埠上的虛擬區域網路 (VLAN) 設定的最佳作法。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e4843b0ffee86d728736ae212b953bb7c8552c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820549"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>設定和檢視 Hyper-V 虛擬交換器連接埠上的 VLAN 設定

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解設定及檢視為 HYPER-V 虛擬交換器連接埠上的虛擬區域網路 (VLAN) 設定的最佳作法。

當您想要在 HYPER-V 虛擬交換器連接埠上設定 VLAN 設定時，您可以使用任一 Windows&reg; Server 2016 HYPER-V 管理員] 或 [System Center Virtual Machine Manager (VMM)。

如果您使用 VMM，則 VMM 會使用下列 Windows PowerShell 命令來設定交換器連接埠。

```
Set-VMNetworkAdapterIsolation <VM-name|-managementOS> -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
如果您未使用 VMM，並會在 Windows Server 中設定的交換器連接埠，您可以使用 HYPER-V 管理員主控台 或 下列 Windows PowerShell 命令。
```
Set-VMNetworkAdapterVlan <VM-name|-managementOS> -Access -VlanID <vlan-value>
```

因為 HYPER-V 虛擬交換器連接埠上設定 VLAN 設定這兩種方法，就可以，當您嘗試檢視的交換器連接埠設定，它會顯示您的 VLAN 設定未設定-即使在進行這些設定時。

## <a name="use-the-same-method-to-configure-and-view-switch-port-vlan-settings"></a>若要設定及檢視交換器連接埠 VLAN 設定使用相同的方法

若要確保不會遇到這些問題，您必須使用相同的方法若要檢視您用來設定交換器連接埠 VLAN 設定交換器連接埠 VLAN 設定。

若要設定及檢視交換器連接埠的 VLAN 設定，您必須執行下列作業：

- 如果您使用 VMM 或網路控制站來設定和管理您的網路，且您已部署軟體定義網路 (SDN)，您必須使用**VMNetworkAdapterIsolation** cmdlet。 
- 如果您使用 Windows Server 2016 HYPER-V 管理員] 或 [Windows PowerShell cmdlet，而且您尚未部署軟體定義網路 (SDN)，您必須使用**VMNetworkAdapterVlan** cmdlet。

### <a name="possible-issues"></a>可能的問題

如果您未遵循這些指導方針可能會遇到下列問題。

- 在情況下，您已部署 SDN，並使用 VMM，網路控制站，或**VMNetworkAdapterIsolation** cmdlet 來設定 HYPER-V 虛擬交換器連接埠上的 VLAN 設定：如果您使用 HYPER-V 管理員或**取得 VMNetworkAdapterVlan**若要檢視組態設定，命令輸出中不會顯示您的 VLAN 設定。 您必須改用**Get VMNetworkIsolation** cmdlet，以檢視的 VLAN 設定。
- 在情況下，您還未部署 SDN，並改為使用 HYPER-V 管理員或**VMNetworkAdapterVlan** cmdlet 來設定 HYPER-V 虛擬交換器連接埠上的 VLAN 設定：如果您使用**Get VMNetworkIsolation** cmdlet 來檢視組態設定中，命令輸出中將不會顯示您的 VLAN 設定。 您必須改用**取得 VMNetworkAdapterVlan** cmdlet，以檢視的 VLAN 設定。

此外，也不務必嘗試使用這兩種組態方法中設定相同的交換器連接埠 VLAN 設定。 如果您這麼做時，未正確設定交換器連接埠，和結果可能是網路通訊失敗。

### <a name="resources"></a>資源

如需有關本主題所述的 Windows PowerShell 命令的詳細資訊，請參閱下列各項：

- [Set-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464283.aspx)
- [Get-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464277.aspx)
- [Set-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx)
- [Get-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848516.aspx)





