---
title: Windows Server 2016 中 HYPER-V 網路模擬中的新功能
description: 本主題提供 HYPER-V 網路模擬 Windows Server 2016 中的新功能的相關資訊
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
ms.openlocfilehash: 0954768944e44848debfbb7fb752a13ca47031c2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Windows Server 2016 中 HYPER-V 網路模擬中的新功能

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題描述 HYPER-V 網路模擬 (HNV) 新增或變更 Windows Server 2016 中的功能。  
  
## <a name="BKMK_IPAM2012R2"></a>HNV 更新  
HNV 提供美化的支援下列幾個方面：  
  
|功能日功能|新的或改進|描述|  
|--------------------------|-------------------|---------------|  
|[可程式 HYPER-V 開關切換至](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|新|HNV 原則是透過 Microsoft Network Controller 可程式。|  
|[VXLAN 封裝支援](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|新|HNV 現在支援 VXLAN 封裝。|  
|[軟體負載平衡器 (SLB) 交互操作](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|新|Microsoft 軟體負載平衡器完全整合 HNV。|  
|[相容 IEEE 乙太網路標頭](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|已改進|相容的標準 IEEE 乙太網路|  
  
### <a name="SDN"></a>可程式 HYPER-V 開關切換至  
HNV 是 Microsoft 的更新的軟體定義網路 (SDN) 方案的基礎建置組塊，完整地整合 SDN 堆疊。  
  
Microsoft 的新 Network Controller 推入 HNV 原則下每一部主機做為 SouthBound 介面 (SBI) 使用開放 vSwitch 資料庫管理通訊協定 (OVSDB) 上執行主機代理程式。 主機代理儲存這項原則使用自訂的[VTEP 架構](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema)和程式複雜流程規則 HYPER-V 切換中無法運作的可能性流程引擎。  
  
Flow 中的引擎 HYPER-V 開關切換至是使用 Microsoft Azure 相同引擎&trade;，它已在超縮放公用雲端 Microsoft Azure 證明。 此外，在整個 SDN 堆疊上透過網路控制器，請及網路資源提供者 （即將推出的詳細資料） 是使用 Microsoft Azure，因此我們企業版和裝載服務提供者針對提供的公用 Microsoft Azure 雲端力量一致的。  
  
> [!NOTE]  
> 如需 OVSDB 的詳細資訊，請查看[RFC 7047](http://www.rfc-editor.org/info/rfc7047)。  
  
HYPER-V 開關切換至支援根據簡單 '符合動作' 中 Microsoft flow 引擎兩個無和狀態流程規則。  
 
![Windows Server 2016 HYPER-V 開關切換至](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>VXLAN 封裝支援  
虛擬延伸區域網路 (VXLAN- [RFC 7348](http://www.rfc-editor.org/info/rfc7348)) 通訊協定已被普遍在市場，並可支援從等 Cisco、 錦緞、 Dell、 HP 和其他廠商取得。 HNV 現在也支援這種封裝配置使用 MAC distribution 模式，透過 Microsoft 網路控制器程式對應為承租人覆疊網路的 IP 位址 （客戶地址或 CA） 底圖實體網路的 IP 位址 （提供者地址或 PA）。 改善效能透過協力廠商驅動程式支援 NVGRE 和 VXLAN 工作卸載。  
  
### <a name="SLB"></a>軟體負載平衡器 (SLB) 交互操作  
Windows Server 2016 包含軟體負載平衡器 (SLB) virtual 網路流量和順暢互動 HNV 完整支援。 透過資料平面 v 式開關切換至中無法運作的可能性流程引擎實作和受 Network Controller 的 Virtual IP (VIP) / 動態 SLB IP (DIP) 對應。  
  
### <a name="L2"></a>相容 IEEE 乙太網路標頭  
HNV 實作正確 L2 乙太網路標頭，以確保交互操作而定業界標準通訊協定第三方 virtual 和實體裝置使用。 Microsoft 可確保所有傳送封包為了確保此交互操作所有欄位中有相容的值。 此外，支援的 L2 實體網路的巨大畫面 (MTU > 1780年)，則必須的負荷同時確保來賓連接至 HNV Virtual 網路引進了封裝通訊協定 （NVGRE、 VXLAN） 封包維護 1514 MTU。  
  
## <a name="see-also"></a>也了  
  
-   [HYPER-V 網路模擬概觀](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [HYPER-V 網路模擬技術的詳細資料](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [軟體定義網路](../../Software-Defined-Networking--SDN-.md)  
  