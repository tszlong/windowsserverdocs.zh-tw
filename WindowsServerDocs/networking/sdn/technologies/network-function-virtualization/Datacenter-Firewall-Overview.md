---
title: 資料中心防火牆概觀
description: 您可以使用本主題以了解資料中心防火牆，這是網路層、 5-tuple （通訊協定、 來源和目的地連接埠號碼、 來源和目的地 IP 位址）、 Windows Server 2016 中的具狀態、 多租用戶的防火牆。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67576533-206b-428a-956c-ed8c53218d9b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f1de50dc61639f4985c9d28fdde6072af650f42e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890829"
---
# <a name="datacenter-firewall-overview"></a>資料中心防火牆概觀

>適用於：Windows Server （半年通道），Windows Server 2016

資料中心防火牆是隨附於 Windows Server 2016 的新服務。 它是網路層、 5-tuple （通訊協定、 來源和目的地連接埠號碼、 來源和目的地 IP 位址）、 可設定狀態、 多租用戶的防火牆。 當部署，並由服務提供者以服務形式提供，租用戶系統管理員可以安裝並設定防火牆原則，以協助保護他們防止來自網際網路的不必要流量的虛擬網路和網路的內部網路。  
  
![網路堆疊中的資料中心防火牆](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
服務提供者系統管理員或租用戶系統管理員可以管理的資料中心防火牆原則透過網路控制站和 northbound Api。  
  
資料中心防火牆雲端服務提供者，提供下列優點：  
  
-   可以提供給租用戶的高度可調整、 易管理，且診斷軟體為基礎的防火牆解決方案  
  
-   能夠自由地將租用戶虛擬機器移到不同的計算主機，而不會中斷租用戶的防火牆原則  
  
    -   部署為 vSwitch 連接埠主機代理程式防火牆  
  
    -   租用戶虛擬機器取得指派給其 vSwitch 主機代理程式的防火牆原則  
  
    -   每個 vSwitch 連接埠，獨立於實際執行虛擬機器的主機中所設定的防火牆規則  
  
-   提供給租用戶獨立租用戶客體作業系統的虛擬機器的保護  
  
資料中心防火牆租用戶提供下列優點：  
  
-   若要定義防火牆規則，以協助保護網際網路對向虛擬網路上的工作負載的能力  
  
-   能夠定義來協助保護相同 L2 虛擬子網路上以及在不同的 L2 虛擬子網路上的虛擬機器之間的虛擬機器之間流量的防火牆規則  
  
-   能夠定義來協助保護和隔離租用戶之間的網路流量的防火牆規則為內部網路和其服務提供者上的虛擬網路  
  


