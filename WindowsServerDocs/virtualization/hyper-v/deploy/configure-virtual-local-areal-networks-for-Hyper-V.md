---
title: 設定適用於 HYPER-V 的虛擬區域網路
description: 提供使用 HYPER-V 主機上的虛擬機器所設定的虛擬區域網路 (VLAN) 的指示。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
author: KBDAzure
ms.author: kathydav
ms.date: 10/11/2016
ms.openlocfilehash: 5b5eaf175e7c09124aaa3f7a33523e8b87a9ae84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848459"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>設定適用於 HYPER-V 的虛擬區域網路
虛擬區域網路\(Vlan\)提供一個方式，來隔離的網路流量。 Vlan 會在交換器和路由器，支援 802.1q 設定。 如果您設定多個 Vlan，以及想讓它們之間進行通訊，您必須設定網路裝置，以允許的。 

您需要下列命令來設定 Vlan:  
  
-   實體網路介面卡和驅動程式支援 802.1q VLAN 標記。  
-   實體網路交換器支援 802.1q VLAN 標記。  
  
在主機上，您將設定可讓實體交換器連接埠上的網路流量的虛擬交換器。 這是您想要與虛擬機器在內部使用的 VLAN Id。 接下來，您會設定虛擬機器，以指定虛擬機器將使用的所有網路通訊的 VLAN。  
  
#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>若要讓虛擬交換器，以使用 VLAN  
  
1.  開啟超\-V 管理員。  
  
2.  從 [動作] 功能表中，按一下**虛擬交換器管理員**。  
  
3.  底下**虛擬交換器**，選取虛擬交換器連接至實體網路介面卡支援 Vlan。 

4. 在 VLAN ID，底下的右窗格中選取**啟用虛擬 LAN 識別碼**然後輸入 VLAN id。 數字  
  
    透過實體網路介面卡會連線至虛擬交換器的所有流量會都加上您所設定的 VLAN ID。  
  
#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>若要讓虛擬機器使用 VLAN  
  
1.  開啟超\-V 管理員。  
  
2.  在 [結果] 窗格中，在**虛擬機器**選取適當的虛擬機器，然後以滑鼠右鍵按一下**設定**。  

3.  底下**硬體**，選取 使用 VLAN 設定的虛擬交換器。
  
4.  在右窗格中，選取**啟用虛擬 LAN 識別碼**，然後輸入您指定虛擬交換器的 相同的 VLAN ID。 

如果虛擬機器需要使用更多的 Vlan，請執行下列其中一項動作：  
  
-   連接到適當的虛擬交換器，以及指派 VLAN 識別碼的多個虛擬網路介面卡。 請務必正確設定的 IP 位址，然後，您要透過 VLAN 也路由傳送流量會使用正確的 IP 位址。  
  
-   設定在主幹模式中使用的虛擬網路 word 配接器[設定\-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx) cmdlt。
  
## <a name="see-also"></a>另請參閱  
 
[超\-V 虛擬交換器](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/hyper-v-virtual-switch)