---
title: 什麼是 Windows Server 2016 中的 HYPER-V 網路虛擬化的新功能
description: 本主題提供 Windows Server 2016 中的 HYPER-V 網路虛擬化的新功能的相關資訊
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: pashort
author: shortpatti
ms.date: 03/19/2018
ms.openlocfilehash: 24ec9e52be3acdfced35eae4fb5f98f16d8e18f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837949"
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>什麼是 Windows Server 2016 中的 HYPER-V 網路虛擬化的新功能

>適用於：Windows Server （半年通道），Windows Server 2016

本主題說明新增或變更 Windows Server 2016 中的 HYPER-V 網路虛擬化 (HNV) 功能。  
  
## <a name="BKMK_IPAM2012R2"></a>HNV 中的更新  
HNV 提供下列方面的增強的支援：  
  
|特色/功能|新功能或增強功能|描述|  
|--------------------------|-------------------|---------------|  
|[可程式化的 HYPER-V 交換器](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|新的|HNV 原則是透過 Microsoft 網路控制站的可程式化項目。|  
|[VXLAN 封裝支援](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|新的|HNV 現在支援 VXLAN 封裝。|  
|[軟體負載平衡器 (SLB) 互通性](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|新的|HNV 與 Microsoft 軟體負載平衡器，是完全整合。|  
|[符合規範的 IEEE 乙太網路標頭](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|增強功能|符合 IEEE 乙太網路標準|  
  
### <a name="SDN"></a>可程式化的 HYPER-V 交換器  
HNV 是基本的建置組塊，Microsoft 已更新的軟體定義網路 (SDN) 解決方案，並已完全整合到 SDN 堆疊。  
  
Microsoft 的新網路控制卡推送到使用開啟的 vSwitch 資料庫管理通訊協定 (OVSDB) 做為 SouthBound 介面 (SBI) 每個主機上執行的主機代理程式的 HNV 原則。 主機代理程式會儲存此原則使用的自訂[VTEP 結構描述](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema)和程式複雜的流程規則到 HYPER-V 交換器中的高效能資料流程引擎。  
  
HYPER-V 交換器內資料流程引擎是在 Microsoft Azure 中使用的相同引擎&trade;，已證明其在 Microsoft Azure 公用雲端中的超規模。 此外，透過網路控制站和網路資源提供者 （即將推出的詳細資料） 設定整個 SDN 堆疊做法與 Microsoft Azure，因此將 Microsoft Azure 公用雲端的威力帶到我們的企業和主機服務提供者的客戶。  
  
> [!NOTE]  
> 如需 OVSDB 的詳細資訊，請參閱[RFC 7047](https://www.rfc-editor.org/info/rfc7047)。  
  
HYPER-V 交換器支援這兩個無狀態與具狀態的流程規則，根據簡單 '相符的動作' 在 Microsoft 的資料流程引擎。  
 
![Windows Server 2016 HYPER-V 交換器](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>VXLAN 封裝支援  
虛擬可延伸區域網路 (VXLAN- [RFC 7348](https://www.rfc-editor.org/info/rfc7348)) 通訊協定已被廣泛採用在市場中，例如 Cisco、 Brocade、 Dell、 HP 和其他廠商的支援。 HNV 現在也支援使用 MAC 分配模式，透過 Microsoft 網路控制站，計劃對應到租用戶重疊網路 IP 位址 （客戶地址或 CA） 的實體為網路 IP 位址 （提供者的這個封裝配置地址或 PA）。 為了提升效能，透過協力廠商驅動程式支援 NVGRE 和 VXLAN 工作卸載。  
  
### <a name="SLB"></a>軟體負載平衡器 (SLB) 互通性  
Windows Server 2016 包含的軟體負載平衡器 (SLB) 與虛擬網路流量和 HNV 的無縫式互動的完整支援。 SLB 會透過高效能資料流程引擎，資料平面 v-交換器中實作及受網路控制站的虛擬 IP (VIP) / 動態 IP (DIP) 對應。  
  
### <a name="L2"></a>符合規範的 IEEE 乙太網路標頭  
HNV 會實作正確的 L2 乙太網路標頭，以確保互通性與業界標準通訊協定而定的第三方虛擬和實體設備。 Microsoft 可確保所有傳送的封包，會有相容的值，以確保這個互通性的所有欄位中。 此外，支援在實體網路中，L2 的訊框 (MTU > 1780年) 都是必要的額外負荷導入的封裝通訊協定 （NVGRE、 VXLAN） 同時確保維護連接至 HNV 的虛擬網路的虛擬機器的來賓的封包的帳戶1514 MTU。  
  
## <a name="see-also"></a>另請參閱  
  
-   [Hyper-V 網路虛擬化概觀](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Hyper-V 網路虛擬化技術詳細資料](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [軟體定義網路](../../Software-Defined-Networking--SDN-.md)  
  