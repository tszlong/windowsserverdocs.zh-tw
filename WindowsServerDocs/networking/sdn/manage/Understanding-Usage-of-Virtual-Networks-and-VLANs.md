---
title: 瞭解虛擬網路和 Vlan 的使用方式
description: 在本主題中，您將瞭解 Hyper-v 網路虛擬化虛擬網路，以及它們與虛擬區域網路絡 (Vlan) 的差異。 使用 Hyper-v 網路虛擬化時，您會建立重迭虛擬網路，也稱為虛擬網路。
manager: grcusanz
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/26/2018
ms.openlocfilehash: 1f1f1f56fbac8c7faa7628ac0adb0cbeef3a78d3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962210"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>瞭解虛擬網路和 Vlan 的使用方式

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，您將瞭解 Hyper-v 網路虛擬化虛擬網路，以及它們與虛擬區域網路絡 (Vlan) 的差異。 使用 Hyper-v 網路虛擬化時，您會建立重迭虛擬網路，也稱為虛擬網路。




Windows Server 2016 中 (SDN) 的軟體定義網路功能，是以在 Hyper-v 虛擬交換器內重迭虛擬網路的程式設計原則為基礎。 您可以使用 Hyper-v 網路虛擬化來建立重迭虛擬網路，也稱為虛擬網路。

當您部署 Hyper-v 網路虛擬化時，會將原始租使用者虛擬機器的第2層乙太網路 (框架（例如 VXLAN 或 NVGRE) ）與 underlay (或實體) 網路中的第3層 IP 和第2層 Ethernet 標頭封裝，以建立重迭網路。 重迭的虛擬網路是由24位虛擬網路識別碼所識別 (VNI) 來維護租使用者流量隔離，並允許重迭的 IP 位址。 VNI 是由虛擬子網識別碼所組成 (VSID) 、邏輯交換器識別碼和通道識別碼。

此外，每個租使用者都會指派一個路由網域 (類似于虛擬路由和轉送-VRF) ，因此 VNI) 所代表的多個虛擬子網首碼 (可以直接路由傳送到彼此。 不需要透過閘道，就不支援跨租使用者 (或跨路由網域) 路由。

每個租使用者的封裝流量在其上的實體網路是由稱為提供者邏輯網路的邏輯網路表示。 此提供者邏輯網路是由一或多個子網組成，每個子網分別以 IP 首碼和 VLAN 802.1 q 標記（選擇性）表示。

您可以針對基礎結構目的建立額外的邏輯網路和子網，以攜帶管理流量、儲存體流量、即時移轉流量等等。

Microsoft SDN 不支援使用 Vlan 隔離租使用者網路。 租使用者隔離只會使用 Hyper-v 網路虛擬化重迭虛擬網路和封裝來完成。


