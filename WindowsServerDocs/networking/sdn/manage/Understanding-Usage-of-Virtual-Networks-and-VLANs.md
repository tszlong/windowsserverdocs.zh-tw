---
title: 了解使用 Virtual 網路與 Vlan
description: 本主題是輔助的軟體定義網路上如何管理承租人工作負載和 Windows Server 2016 Virtual 網路的一部分。
manager: brianlic
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
ms.openlocfilehash: fcf84c5c1f0be2fa1c7524592f8d02e4b11d3a2b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="understanding-usage-of-virtual-networks-and-vlans"></a>了解使用 Virtual 網路與 Vlan

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以深入了解 HYPER-V 網路模擬 Virtual 網路，以及如何有所不同區域網路 (Vlan)。  
  
軟體定義網路 (SDN) 在 Windows Server 2016 中根據程式設計覆疊 virtual 網路 Virtual 切換 HYPER-V 中的原則。 您可以建立覆疊 virtual 網路，也稱為 Virtual 網路，HYPER-V 網路模擬。   
  
當部署 HYPER-V 網路模擬時，會建立覆疊網路封裝覆疊-或通道-標題 （例如，VXLAN 或 NVGRE） 和層級 3 IP 和層級 2 乙太網路標頭底圖 （或實體） 從網路與原始承租人一樣的層級 2 乙太網路畫面。 覆疊網路 virtual 都會來 24 元 Virtual 網路識別碼 (VNI) 維護承租人流量隔離，並允許重疊的 IP 位址。 VNI 組成 virtual 子網路 ID (VSID)、 邏輯切換 ID 和通道 id。  
  
此外，每個承租人，以便在多個 （每個由 VNI） 的 virtual 子網路首碼直接傳送彼此指派路由網域 （類似 virtual 路由並轉接-VRF）。 跨-承租人 （或跨路由網域） 而不需透過閘道路由不支援。   
  
使用的通道每個承租人的封裝的流量之實體網路會以邏輯網路稱為邏輯網路提供者。 這提供者邏輯網路包含了一或多個子網路，每由 IP 首碼，或者，VLAN 802.1q 標記。  
  
您可以建立其他邏輯網路且子網路基礎結構用途以執行管理傳輸，儲存的資料傳輸移轉流量等。  
  
Microsoft SDN 不支援使用 Vlan 隔離的承租人網路。 承租人隔離被透過僅使用 HYPER-V 網路模擬覆疊網路 Virtual 和封裝。 


