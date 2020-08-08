---
title: Windows Server 2016 中 Hyper-v 網路虛擬化的新功能
description: 本主題提供 Windows Server 2016 中 Hyper-v 網路虛擬化新功能的相關資訊
manager: grcusanz
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: anpaul
author: AnirbanPaul
ms.date: 03/19/2018
ms.openlocfilehash: 9daab9358cbfd337b755605e7233b1bcea555f22
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994268"
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Windows Server 2016 中 Hyper-v 網路虛擬化的新功能

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明在 Windows Server 2016 中新增或變更的 Hyper-v 網路虛擬化 (HNV) 功能。

## <a name="updates-in-hnv"></a><a name="BKMK_IPAM2012R2"></a>HNV 中的更新
HNV 提供下列領域的增強支援：

|特色/功能|新功能或增強功能|描述|
|--------------------------|-------------------|---------------|
|[可程式化 Hyper-v 交換器](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|新增|HNV 原則可透過 Microsoft 網路控制站來進行程式設計。|
|[VXLAN 封裝支援](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|新增|HNV 現在支援 VXLAN 封裝。|
|[軟體 Load Balancer (SLB) 互通性](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|新增|HNV 已與 Microsoft 軟體 Load Balancer 完全整合。|
|[相容的 IEEE Ethernet 標頭](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|改善|符合 IEEE Ethernet 標準|

### <a name="programmable-hyper-v-switch"></a><a name="SDN"></a>可程式化 Hyper-v 交換器
HNV 是 Microsoft 更新過的軟體定義網路功能 (SDN) 解決方案的基礎大樓，並已完全整合到 SDN 堆疊中。

Microsoft 的新網路控制站會使用 Open vSwitch 資料庫管理通訊協定，將 HNV 原則推送至每部主機上執行的主機代理程式， (OVSDB) 做為 SouthBound 介面 (為) 。 主機代理程式會使用[VTEP 架構](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema)的自訂來儲存此原則，並將複雜流程規則程式化至 hyper-v 交換器中的高效能流程引擎。

Hyper-v 交換器內的流程引擎與 Microsoft Azure 中使用的引擎相同 &trade; ，其已在 Microsoft Azure 公用雲端中以超大規模證明。 此外，透過網路控制卡的整個 SDN 堆疊，以及即將推出的網路資源提供者 (詳細資料) 與 Microsoft Azure 一致，因此將 Microsoft Azure 公用雲端的威力帶入企業和主機服務提供者客戶。

> [!NOTE]
> 如需 OVSDB 的詳細資訊，請參閱[RFC 7047](https://www.rfc-editor.org/info/rfc7047)。

Hyper-v 交換器會根據 Microsoft 的流程引擎內簡單的「符合動作」，支援無狀態和可設定狀態的流程規則。

![Windows Server 2016 Hyper-v 交換器](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)

### <a name="vxlan-encapsulation-support"></a><a name="VXLAN"></a>VXLAN 封裝支援
虛擬可延伸的區域網路 (VXLAN- [RFC 7348](https://www.rfc-editor.org/info/rfc7348)) 通訊協定已廣泛採用于市場上，並提供來自 Cisco、織錦、DELL、HP 等廠商的支援。 HNV 現在也支援透過 Microsoft 網路控制卡使用 MAC 散佈模式的此封裝配置，以程式將租使用者重迭網路 IP 位址對應至 (客戶位址，或 CA) 的實體 underlay 網路 IP 位址 (提供者位址或 PA) 。 NVGRE 和 VXLAN 工作卸載都支援透過協力廠商驅動程式改善效能。

### <a name="software-load-balancer-slb-interoperability"></a><a name="SLB"></a>軟體 Load Balancer (SLB) 互通性
Windows Server 2016 包括軟體負載平衡器 (SLB) 具有虛擬網路流量的完整支援，以及與 HNV 的順暢互動。 SLB 會透過資料平面 v-交換器中的高效能流程引擎來執行，並由網路控制卡用於虛擬 IP (VIP) /動態 IP (DIP) 對應。

### <a name="compliant-ieee-ethernet-headers"></a><a name="L2"></a>相容的 IEEE Ethernet 標頭
HNV 會執行正確的 L2 乙太網路標頭，以確保與相依于業界標準通訊協定的協力廠商虛擬和實體設備具有互通性。 Microsoft 會確保所有已傳輸的封包在所有欄位中都有相容的值，以確保此互通性。 此外，實體 L2 網路中 (MTU > 1780) 的大型框架支援，必須考慮封裝通訊協定所引進的封包額外負荷 (NVGRE，VXLAN) ，同時確保虛擬機器連接到 HNV 的來賓虛擬網路維持 1514 MTU。

## <a name="additional-references"></a>其他參考資料

-   [Hyper-V 網路虛擬化概觀](hyperv-network-virtualization-overview-windows-server.md)

-   [Hyper-V 網路虛擬化技術詳細資料](hyperv-network-virtualization-technical-details-windows-server.md)

-   [軟體定義網路](../../software-defined-networking.md)