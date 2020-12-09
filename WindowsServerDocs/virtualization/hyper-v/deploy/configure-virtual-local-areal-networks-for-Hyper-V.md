---
title: 設定 Hyper-v 的虛擬區域網路絡
description: 提供在 Hyper-v 主機上設定虛擬區域網路絡 (VLAN) 供虛擬機器使用的指示。
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/11/2016
ms.openlocfilehash: 22f9890fd1a3a90fcbab59a9ed1de8481e117e75
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866157"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>設定 Hyper-v 的虛擬區域網路絡
虛擬區域網路絡 \( vlan \) 提供一種方式來隔離網路流量。 Vlan 設定在支援 802.1 q 的交換器和路由器中。 如果您設定多個 Vlan，並且想要在兩者之間進行通訊，您必須設定網路裝置來允許。

您將需要下列設定 Vlan：

- 支援 802.1 q VLAN 標記的實體網路介面卡和驅動程式。
- 支援 802.1 q VLAN 標記的實體網路交換器。

在主機上，您將設定虛擬交換器以允許實體交換器埠上的網路流量。 這適用于您想要在內部搭配虛擬機器使用的 VLAN 識別碼。 接下來，您要設定虛擬機器來指定虛擬機器將用於所有網路通訊的 VLAN。

#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>允許虛擬交換器使用 VLAN

1. 開啟 [Hyper-v \- 管理員]。

2. 從 [動作]  功能表上，按一下 [虛擬交換器管理員] 。

3. 在 [ **虛擬交換器**] 底下，選取連接至支援 vlan 之實體網路介面卡的虛擬交換器。

4. 在右窗格的 [VLAN ID] 底下，選取 [ **啟用虛擬 LAN 識別** ]，然後輸入 VLAN ID 的數位。

    所有透過連線至虛擬交換器的實體網路介面卡的流量，將會以您設定的 VLAN ID 標記。

#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>允許虛擬機器使用 VLAN

1. 開啟 [Hyper-v \- 管理員]。

2. 在結果窗格的 [ **虛擬機器**] 下，選取適當的虛擬機器，然後以滑鼠右鍵按一下 [ **設定**]。

3. 在 [ **硬體**] 底下，選取使用 VLAN 設定的虛擬交換器。

4. 在右窗格中，選取 [ **啟用虛擬 LAN 識別**]，然後輸入與您為虛擬交換器指定的 VLAN 識別碼相同的 VLAN ID。

如果虛擬機器需要使用更多的 Vlan，請執行下列其中一項：

- 將更多虛擬網路介面卡連接到適當的虛擬交換器，並指派 VLAN 識別碼。 請務必正確設定 IP 位址，而且您想要透過 VLAN 路由傳送的流量也會使用正確的 IP 位址。

- 使用 [Set \- get-vmnetworkadaptervlan](/powershell/module/hyper-v/set-vmnetworkadaptervlan) Cmdlet 在主幹模式中設定虛擬網路介面卡。

## <a name="see-also"></a>另請參閱

[Hyper-v \- 虛擬交換器](../../hyper-v-virtual-switch/hyper-v-virtual-switch.md)
