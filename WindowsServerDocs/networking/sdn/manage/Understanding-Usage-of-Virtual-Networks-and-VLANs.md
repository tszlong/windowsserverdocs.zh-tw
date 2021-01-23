---
title: 瞭解虛擬網路和 Vlan 的使用方式
description: 在本主題中，您將瞭解 Hyper-v 網路虛擬化虛擬網路，以及它們與虛擬區域網路絡 (Vlan) 的不同之處。 使用 Hyper-v 網路虛擬化時，您可以建立重迭的虛擬網路（也稱為虛擬網路）。
manager: grcusanz
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/26/2018
ms.openlocfilehash: 58ab0a66e5f08ba9661326b418170563a56a86a4
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716654"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>瞭解虛擬網路和 Vlan 的使用方式

>適用於：Windows Server 2019、Windows Server 2016

在本主題中，您將瞭解 Hyper-v 網路虛擬化虛擬網路，以及它們與虛擬區域網路絡 (Vlan) 的不同之處。 使用 Hyper-v 網路虛擬化時，您可以建立重迭的虛擬網路（也稱為虛擬網路）。

Windows Server 2016 中的軟體定義網路 (SDN) 是以 Hyper-v 虛擬交換器內重迭虛擬網路的程式設計原則為基礎。 您可以使用 Hyper-v 網路虛擬化來建立重迭的虛擬網路（也稱為虛擬網路）。

當您部署 Hyper-v 網路虛擬化時，會藉由封裝原始租使用者虛擬機器的第2層 Ethernet 框架（具有重迭或通道標 (頭）來建立重迭網路，例如，VXLAN 或 NVGRE) 以及來自基礎 (或實體) 網路的第3層 IP 和第2層乙太網路標頭。 重迭的虛擬網路是由24位虛擬網路識別碼所識別 (VNI) 來維護租使用者流量隔離，以及允許重迭的 IP 位址。 VNI 是由虛擬子網識別碼組成 (VSID) 、邏輯交換器識別碼和通道識別碼。

此外，每個租使用者都會被指派一個路由網域 (類似于虛擬路由和轉送 VRF) ，如此一來，VNI) 所代表的多個虛擬子網首碼 (可以直接路由傳送到彼此。 不透過閘道，不支援跨租使用者 (或跨路由網域) 路由。

每個租使用者的封裝流量為通道的實體網路是由稱為「提供者邏輯網路」的邏輯網路所代表。 此提供者邏輯網路是由一或多個子網所組成，每個子網都以 IP 前置詞和（選擇性） VLAN 802.1 q 標記表示。

您可以建立額外的邏輯網路和子網，以供基礎結構使用，以攜帶管理流量、儲存流量、即時移轉流量等等。

Microsoft SDN 不支援使用 Vlan 隔離租使用者網路。 租使用者隔離只會使用 Hyper-v 網路虛擬化重迭虛擬網路和封裝來完成。
