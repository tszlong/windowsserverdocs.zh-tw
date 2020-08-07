---
title: 設定 Hyper-v 的虛擬區域網路絡
description: 提供設定虛擬區域網路的指示， (VLAN) 供 Hyper-v 主機上的虛擬機器使用。
manager: dongill
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
author: kbdazure
ms.author: kathydav
ms.date: 10/11/2016
ms.openlocfilehash: dea2da2d0a10839fd9fe69dbb7b3974290b85975
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963614"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>設定 Hyper-v 的虛擬區域網路絡
虛擬區域網路 \( vlan \) 提供一種隔離網路流量的方式。 Vlan 是在支援 802.1 q 的交換器和路由器中設定。 如果您設定多個 Vlan，而且想要在兩者之間進行通訊，您必須設定網路裝置以允許。

您將需要下列設定 Vlan：

- 支援 802.1 q VLAN 標記的實體網路介面卡和驅動程式。
- 支援 802.1 q VLAN 標記的實體網路交換器。

在主機上，您會將虛擬交換器設定為允許實體交換器埠上的網路流量。 這適用于您想要在內部與虛擬機器搭配使用的 VLAN 識別碼。 接下來，您要設定虛擬機器來指定虛擬機器將用於所有網路通訊的 VLAN。

#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>允許虛擬交換器使用 VLAN

1. 開啟 [Hyper-v \- 管理員]。

2. 從 [動作]  功能表上，按一下 [虛擬交換器管理員] ****。

3. 在 [**虛擬交換器**] 底下，選取連線到支援 vlan 之實體網路介面卡的虛擬交換器。

4. 在右窗格的 [VLAN ID] 底下，選取 [**啟用虛擬 LAN 識別**]，然後輸入 VLAN ID 的數位。

    所有經過實體網路介面卡連線到虛擬交換器的流量，都會以您設定的 VLAN 識別碼標記。

#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>允許虛擬機器使用 VLAN

1. 開啟 [Hyper-v \- 管理員]。

2. 在 [結果] 窗格的 [**虛擬機器**] 底下，選取適當的虛擬機器，然後以滑鼠右鍵按一下 [**設定**]。

3. 在 [**硬體**] 底下，選取以 VLAN 設定的虛擬交換器。

4. 在右窗格中，選取 [**啟用虛擬 LAN 識別**]，然後輸入與您為虛擬交換器指定的相同 VLAN ID。

如果虛擬機器需要使用更多的 Vlan，請執行下列其中一項動作：

- 將更多虛擬網路介面卡連線到適當的虛擬交換器，並指派 VLAN 識別碼。 請務必正確地設定 IP 位址，而且您要透過 VLAN 路由傳送的流量也會使用正確的 IP 位址。

- 使用[Set \- set-vmnetworkadaptervlan](https://technet.microsoft.com/library/hh848475.aspx) Cmdlet，在主幹模式中設定虛擬網路介面卡。

## <a name="see-also"></a>另請參閱

[Hyper-v \- 虛擬交換器](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/hyper-v-virtual-switch)
