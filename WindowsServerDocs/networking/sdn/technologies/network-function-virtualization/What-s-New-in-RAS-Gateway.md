---
title: RAS 閘道的新功能
description: 您可以使用本主題以了解新功能 RAS 閘道，也就是以軟體為基礎，多租用戶、 邊界閘道通訊協定 (BGP) Windows Server 2016 中功能的路由器。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 709cb192-313a-47b5-954e-eb5f6fee51a7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5cc7d8bab3f2783750dbd723da745b1df3c2e462
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863019"
---
# <a name="whats-new-in-ras-gateway"></a>RAS 閘道的新功能

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解新功能 RAS 閘道，也就是以軟體為基礎，多租用戶、 邊界閘道通訊協定 (BGP) Windows Server 2016 中功能的路由器。 RAS 閘道的多租用戶 BGP 路由器被專為雲端服務提供者 (Csp) 和裝載使用 HYPER-V 網路虛擬化的多個租用戶虛擬網路的企業。  
  
> [!NOTE]  
> 在 Windows Server 2012 R2，RAS 閘道的名稱為 RRAS 閘道;並在 「 System Center Virtual Machine Manager 」 中，RAS 閘道名為 Windows Server 閘道。  
  
本主題涵蓋下列各節。  
  
-   [站對站連線能力選項](#bkmk_s2s)  
  
-   [閘道集區](#bkmk_pools)  
  
-   [閘道集區擴充性](#bkmk_gps)  
  
-   [M + N 閘道集區備援](#bkmk_m)  
  
-   [路由反射程式](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>站對站連線能力選項  
RAS 閘道現在支援三種類型的 VPN 站對站連線：網際網路金鑰交換版本 2 (IKEv2) 站台對站虛擬私人網路 (VPN)、 第 3 層 (L3) VPN 和 Generic Routing Encapsulation (GRE) 通道。  
  
如需 GRE 的詳細資訊，請參閱[Windows Server 2016 中的 GRE 通道](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
## <a name="bkmk_pools"></a>閘道集區  
在 Windows Server 2016 中，您可以建立不同類型的閘道集區。 閘道集區包含 RAS 閘道的許多執行個體，並將實體和虛擬網路之間的網路流量路由傳送。 閘道集區可以執行任何個別的閘道函式-網際網路金鑰交換版本 2 (IKEv2) 的站對站虛擬私人網路 (VPN)、 第 3 層 (L3) VPN 和 Generic Routing Encapsulation (GRE) 通道-或集區可以執行所有這些函式，做為混合的集區。  
  
您可以建立使用您偏好使用根據基礎結構需求的任何邏輯閘道集區。 例如，您可以建立任何下列特性為基礎的閘道集區。  
  
-   通道類型 (IKEv2 VPN，L3 VPN GRE VPN)  
  
-   容量  
  
-   備援層級 （依據您的租用戶的計費方案的可靠性）  
  
-   自訂的區隔的客戶  
  
如需詳細資訊，請參閱 < [RAS 閘道高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_gps"></a>閘道集區擴充性  
您可以輕鬆地會藉由新增或移除閘道 Vm 集區中調整閘道集區增加或相應減少。 移除或加入閘道不會破壞的集區所提供的服務。 您也可以新增和移除的閘道的整個集區。  
  
如需詳細資訊，請參閱 < [RAS 閘道高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_m"></a>M + N 閘道集區備援  
每個閘道集區是 M + N 備援。 這表示，是 'active 閘道虛擬機器 (Vm) 的數目會由待命的閘道 Vm 的' n ' 數目。 M + N 備援會將您提供更靈活地決定您需要部署 RAS 閘道時的可靠性層級。 您現在可以設定多個待命 Vm，視需要而不是使用只有一個待命 RAS 閘道，每個作用中-這是唯一的組態選項，與 Windows Server 2012 R2-RAS 閘道 VM。 網路控制站閘道 Service Manager 功能有效率地使用待命的 RAS 閘道 VM 容量，以提供可靠的容錯移轉，如果作用 RAS 閘道 VM 失敗或失去連線。  
  
如需詳細資訊，請參閱 < [RAS 閘道高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_rr"></a>路由反射程式  
邊界閘道通訊協定 (BGP) 路由反映程式現已隨附 RAS 閘道，並提供 BGP 完整網狀拓撲所需的路由路由器之間的同步處理的替代方案。 完整網狀的同步處理，所有的 BGP 路由器必須連接與路由拓樸中的所有其他路由器。 當您使用路由反映程式時，不過，路由反映程式是唯一連接之路由器的所有其他路由器，呼叫 BGP 用戶端，藉此簡化路由同步處理並降低網路流量。 路由反映程式學習所有路由、 計算最佳的路由，並轉散發 BGP 用戶端的最佳路由。  
  
使用 Windows Server 2016，您可以設定終止一個以上的 RAS 閘道 VM 上的個別租用戶的遠端存取通道。 這提供更具有彈性的雲端服務提供者所面臨的情況下一個 RAS 閘道的 VM 無法符合其中的所有租用戶連線的頻寬需求。  
  
這項功能，不過，會帶來其他管理複雜的路由和有效的同步處理的租用戶的遠端站台與雲端資料中心在其虛擬資源之間的路由。 租用戶提供給多個 RAS 閘道的連線時，也會介紹企業結束時，其中每個租用戶站台會具有個別路由的鄰近項目組態中有額外的複雜性。  
  
在控制平面 BGP 路由反映程式解決這些問題，並讓企業租用戶透明 CSP 內部網狀架構部署。 以下是有關包含 RAS 閘道中，網路控制卡與整合在一起，BGP 路由反映程式的一些重點。  
  
-   軟體定義網路的部署中的路由反映程式是一種邏輯實體位於 RAS 閘道和網路控制站之間控制平面上。 它不會不過，參與資料平面路由。  
  
-   當您將新的租用戶新增至您的資料中心時，網路控制站會自動設定 RAS 閘道的第一個租用戶為路由反映程式。  
  
-   每個租用戶都有對應的路由反映程式，並位於其中一個 RAS 閘道 Vm 與該租用戶相關聯。  
  
-   路由反映程式的租用戶會當做路由反映程式的所有租用戶相關聯的 RAS 閘道 Vm。 RAS 閘道路由反映程式以外的租用戶閘道是路由反映程式用戶端。 路由反射程式可用來執行路由所有路由反映程式用戶端之間的同步處理，因此實際的資料路徑路由可能會發生。  
  
-   A 路由反映程式不提供在其中設定 RAS 閘道的路由反映程式服務。  
  
-   A 路由反映程式的企業路由對應到租用戶的企業網站更新網路控制站。 這可讓網路控制站端對端資料路徑存取的租用戶虛擬網路上設定必要的 HYPER-V 網路虛擬化原則。  
  
-   如果您的企業客戶使用 BGP 路由的客戶位址空間中，RAS 閘道路由反映程式就會是唯一的外部的 BGP (eBGP) 像素，所有對應的租用戶的站台。 這是不論企業租用戶的通道終止點，則為 true。 換句話說，不論哪一個 RAS 閘道 VM，在 CSP 資料中心會終止租用戶網站的站對站 VPN 通道，所有租用戶網站的 eBGP 對等路由反映程式。  
  
如需詳細資訊，請參閱 < [RAS 閘道部署架構](RAS-Gateway-Deployment-Architecture.md)和 Internet Engineering Task Force (IETF) 要求註解主題[RFC 4456 BGP 路由反映：替代完整網狀內部 BGP (IBGP)](https://tools.ietf.org/html/rfc4456)。  
  

