---
title: Datacenter 防火牆概觀
description: 您可以使用本主題以深入了解 Datacenter 防火牆，這是網路層級 5-有序元組（通訊協定，來源和目的地的連接埠號碼、來源和目的地的 IP 位址）、Windows Server 2016 中的狀態，multitenant 防火牆。
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
ms.openlocfilehash: 0c9b9fb5b0fb9aa09ed783b2b66a8ad370a627c3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="datacenter-firewall-overview"></a>Datacenter 防火牆概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

Datacenter 防火牆是與 Windows Server 2016 包含新的服務。 它是網路層級 5-有序元組通訊協定，來源和目的地的連接埠號碼（[來源和目的地的 IP 位址）、狀態、multitenant 防火牆。 當部署，即服務提供者服務提供承租人系統管理員可以安裝，並設定防火牆原則，以協助保護其 virtual 垃圾流量來自網際網路的網路及內部網路。  
  
![Datacenter 防火牆網路堆疊中](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
服務提供者系統管理員或承租人系統管理員可以管理透過網路控制器和 northbound Api Datacenter 防火牆原則。  
  
Datacenter 防火牆的雲端服務提供者提供下列優點：  
  
-   可以提供 tenants 高度延展性，可管理及 diagnosable 軟體防火牆方案  
  
-   移至不同運算主機的承租人虛擬電腦，而不會中斷承租人防火牆原則自由  
  
    -   部署 vSwitch 主機連接埠代理程式防火牆為  
  
    -   承租人虛擬電腦取得指派給其 vSwitch 主機代理程式防火牆原則  
  
    -   免中每個 vSwitch 連接埠，獨立執行一樣的實際主機設定  
  
-   提供承租人虛擬機器獨立承租人來賓作業系統的保護  
  
Datacenter 防火牆的 tenants 提供下列優點：  
  
-   定義來協助保護網際網路面對工作負載 virtual 網路上的免功能  
  
-   定義免來協助保護虛擬相同 L2 virtual 子網路，以及例如虛擬不同 L2 virtual 子網路上的電腦上的電腦間的流量的能力  
  
-   定義免，以協助保護並找出承租人間網路流量的能力先網路與他們 virtual 網路的服務提供者。  
  


