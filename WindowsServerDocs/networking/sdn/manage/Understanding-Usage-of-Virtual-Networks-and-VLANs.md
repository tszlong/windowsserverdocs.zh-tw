---
title: 了解虛擬網路和 Vlan 的使用方式
description: 本主題中，您會了解 HYPER-V 網路虛擬化的虛擬網路和它們之間的差異與虛擬區域網路 (Vlan)。 使用 HYPER-V 網路虛擬化，您可以建立重疊虛擬網路，也稱為 虛擬網路。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: pashort
author: shortpatti
ms.date: 08/26/2018
ms.openlocfilehash: d126e97a91e4c61ecff00cc2b5a527618b2d4d0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875529"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>了解虛擬網路和 Vlan 的使用方式

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，您會了解 HYPER-V 網路虛擬化的虛擬網路和它們之間的差異與虛擬區域網路 (Vlan)。 使用 HYPER-V 網路虛擬化，您可以建立重疊虛擬網路，也稱為 虛擬網路。



  
軟體定義網路 (SDN) Windows Server 2016 中根據程式設計覆疊內的虛擬網路的 HYPER-V 虛擬交換器的原則。 您可以建立重疊虛擬網路，也稱為 HYPER-V 網路虛擬化的虛擬網路。 
  
當您部署 HYPER-V 網路虛擬化時，覆疊網路被建立的封裝原始租用戶虛擬機器的第 2 層乙太網路框架重疊或通道標頭 （例如 VXLAN 或 NVGRE） 和第 3 層 IP 與第 2 層乙太網路標頭為 （或實體） 網路。 重疊虛擬網路會識別由 24 位元虛擬網路識別碼 (VNI) 維護租用戶流量隔離，並允許重疊的 IP 位址。 VNI 組成虛擬子網路識別碼 (VSID)、 邏輯交換器的識別碼，以及通道識別碼。  
  
此外，每個租用戶會指派路由網域 （類似於虛擬路由和轉送-VRF），以便在多個虛擬子網路首碼 （每個由 VNI） 可以直接路由傳送彼此。 跨租用戶 （或跨路由網域） 而不會通過閘道不支援路由。   
  
每個租用戶封裝的流量經過通道的實體網路被以稱為提供者邏輯網路的邏輯網路。 此提供者邏輯網路是由一個或多個子網路所組成，每一個由 IP 前置詞，並選擇性地 802.1q VLAN 標記。  
  
您可以建立其他的邏輯網路和子網路來執行管理流量，儲存體流量，基礎結構進行即時移轉流量等。  
  
Microsoft SDN 不支援使用 Vlan 隔離的租用戶網路。 租用戶隔離是僅透過 HYPER-V 網路虛擬化重疊虛擬網路和封裝來完成。 


