---
title: 資料中心防火牆概觀
description: 您可以使用本主題來瞭解資料中心防火牆，這是一種網路層、5個元組 (通訊協定、來源和目的地埠號碼、來源和目的地 IP 位址) 、Windows Server 2016 中的具狀態、多租使用者防火牆。
manager: grcusanz
ms.topic: article
ms.assetid: 67576533-206b-428a-956c-ed8c53218d9b
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 40c827282d1145ea711670842e95db3c61139683
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952515"
---
# <a name="datacenter-firewall-overview"></a>資料中心防火牆概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

資料中心防火牆是 Windows Server 2016 隨附的新服務。 它是網路層、5個元組 (通訊協定、來源和目的地埠號碼、來源和目的地 IP 位址) 、具狀態、多租使用者防火牆。 當服務提供者部署並提供為服務時，租使用者系統管理員可以安裝和設定防火牆原則，以協助保護其虛擬網路免于來自網際網路和內部網路的不必要流量。

![網路堆疊中的資料中心防火牆](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)

服務提供者系統管理員或租使用者系統管理員可以透過網路控制站和 northbound Api 來管理資料中心防火牆原則。

資料中心防火牆為雲端服務提供者提供下列優點：

-   可提供給租使用者的高擴充性、易管理且對付的軟體型防火牆解決方案

-   自由地將租使用者虛擬機器移至不同的計算主機，而不中斷租使用者防火牆原則

    -   部署為 vSwitch 埠主機代理程式防火牆

    -   租使用者虛擬機器取得指派給其 vSwitch 主機代理程式防火牆的原則

    -   防火牆規則是在每個 vSwitch 埠中設定，與執行虛擬機器的實際主機無關

-   提供與租使用者客體作業系統無關的租使用者虛擬機器保護

資料中心防火牆為租使用者提供下列優點：

-   能夠定義防火牆規則，以協助保護虛擬網路上的網際網路對向工作負載

-   能夠定義防火牆規則，以協助保護相同 L2 虛擬子網上虛擬機器之間的流量，以及不同 L2 虛擬子網上的虛擬機器之間的流量

-   能夠定義防火牆規則，以協助保護和隔離租使用者內部部署網路與其虛擬網路之間的網路流量（服務提供者）



