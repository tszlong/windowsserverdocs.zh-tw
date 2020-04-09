---
title: 瞭解虛擬網路和 Vlan 的使用方式
description: 在本主題中，您將瞭解 Hyper-v 網路虛擬化虛擬網路，以及它們與虛擬區域網路（Vlan）的差異。 使用 Hyper-v 網路虛擬化時，您會建立重迭虛擬網路，也稱為虛擬網路。
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/26/2018
ms.openlocfilehash: 56e90966d38b8a138dd8781863a4eaad1db639fb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854481"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>瞭解虛擬網路和 Vlan 的使用方式

>適用於：Windows Server (半年通道)、Windows Server 2016

在本主題中，您將瞭解 Hyper-v 網路虛擬化虛擬網路，以及它們與虛擬區域網路（Vlan）的差異。 使用 Hyper-v 網路虛擬化時，您會建立重迭虛擬網路，也稱為虛擬網路。



  
Windows Server 2016 中的軟體定義網路（SDN）是以在 Hyper-v 虛擬交換器中覆迭虛擬網路的程式設計原則為基礎。 您可以使用 Hyper-v 網路虛擬化來建立重迭虛擬網路，也稱為虛擬網路。 
  
當您部署 Hyper-v 網路虛擬化時，會將原始租使用者虛擬機器的第2層 Ethernet 框架與重迭或通道標頭（例如 VXLAN 或 NVGRE）和第3層 IP 和第2層乙太網路封裝，以建立重迭網路。underlay （或實體）網路中的標頭。 重迭虛擬網路是由24位虛擬網路識別碼（VNI）所識別，以維護租使用者流量隔離，並允許重迭的 IP 位址。 VNI 是由虛擬子網識別碼（VSID）、邏輯交換器識別碼和通道識別碼所組成。  
  
此外，每個租使用者都會被指派一個路由網域（類似于虛擬路由和轉送-VRF），如此一來，多個虛擬子網首碼（每個都是由 VNI 代表）就可以直接彼此路由傳送。 不支援跨租使用者（或跨路由網域）路由，而不需要通過閘道。   
  
每個租使用者的封裝流量在其上的實體網路是由稱為提供者邏輯網路的邏輯網路表示。 此提供者邏輯網路是由一或多個子網組成，每個子網分別以 IP 首碼和 VLAN 802.1 q 標記（選擇性）表示。  
  
您可以針對基礎結構目的建立額外的邏輯網路和子網，以攜帶管理流量、儲存體流量、即時移轉流量等等。  
  
Microsoft SDN 不支援使用 Vlan 隔離租使用者網路。 租使用者隔離只會使用 Hyper-v 網路虛擬化重迭虛擬網路和封裝來完成。 


