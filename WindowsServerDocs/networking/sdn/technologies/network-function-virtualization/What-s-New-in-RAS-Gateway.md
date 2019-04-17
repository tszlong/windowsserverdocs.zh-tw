---
title: RAS 閘道中的新功能
description: 您可以使用此主題 multitenant、 Windows Server 2016 中的邊框閘道通訊協定 (BGP) 可路由器 RAS 閘道，是軟體為基礎，了解新功能。
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
ms.openlocfilehash: 1a9ac762f6cd80d3889cf72478b7a7f8ce9e5cb7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ras-gateway"></a>RAS 閘道中的新功能

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此主題 multitenant、 Windows Server 2016 中的邊框閘道通訊協定 (BGP) 可路由器 RAS 閘道，是軟體為基礎，了解新功能。 RAS 閘道 Multitenant BGP 路由器是雲端服務提供者 (Csp) 和主機多個承租人 virtual 網路使用 HYPER-V 網路模擬針對企業設計。  
  
> [!NOTE]  
> 在 Windows Server 2012 R2，名稱為 RAS 閘道 RRAS 閘道;在系統中心一樣管理員 RAS 閘道為 Windows 伺服器閘道]。  
  
本主題包含下列各節。  
  
-   [連接至網站選項](#bkmk_s2s)  
  
-   [閘道集區](#bkmk_pools)  
  
-   [閘道集區擴充性](#bkmk_gps)  
  
-   [M + N 閘道集區冗餘](#bkmk_m)  
  
-   [之前的路徑反映](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>連接至網站選項  
RAS 閘道現在支援三種類型的 VPN 網站-連接： 網際網路金鑰交換版本 (IKEv2) 2-網站 virtual 私人網路 (VPN)、 層級 3 (L3) VPN 和一般路由封裝 (GRE) 的通道。  
  
如需 GRE 的詳細資訊，請查看[在 Windows Server 2016 的 GRE 通道](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
## <a name="bkmk_pools"></a>閘道集區  
您可以在 Windows Server 2016 建立閘道集區的不同類型。 閘道集區包含許多 RAS 閘道的執行個體與路由實體和 virtual 網路間網路流量。 閘道集區可以執行任何個人閘道函式-網際網路金鑰交換版本 (IKEv2) 2-網站 virtual 私人網路 (VPN)、 層級 3 (L3) VPN 和一般路由封裝 (GRE) 就會-或集區可以執行這些功能的所有做為混合集區。  
  
您可以建立閘道集區使用任何您想要根據您的基礎結構需求的邏輯。 例如，您可以建立任何下列特徵為基礎的閘道集區。  
  
-   通道類型 (IKEv2 VPN、 L3 VPN、 GRE VPN)  
  
-   容量  
  
-   重複層級 （根據您的帳單計劃的 tenants 可靠性）  
  
-   針對的自訂的分離  
  
如需詳細資訊，請查看[RAS 閘道可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_gps"></a>閘道集區擴充性  
您可以輕鬆地縮放閘道集區向上或向下新增或移除閘道 Vm 集區中。 移除或額外的閘道不會不會中斷集區所提供的服務。 您也可以新增與移除閘道整個集區。  
  
如需詳細資訊，請查看[RAS 閘道可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_m"></a>M + N 閘道集區冗餘  
每個閘道集區是 M + N 備援。 這表示已 ' 數目的作用中閘道虛擬 (Vm) 「 n 」 多待命閘道 Vm 的備份。 M + N 冗餘為您提供更具彈性判斷您需要時部署 RAS 閘道可靠性的層級。 您現在可以設定最多待命 Vm 視需要而非只有一個待命 RAS 閘道依據作用中 RAS 閘道 VM-的選項是 \ [僅設定與 Windows Server 2012 R2 的。 網路控制器閘道服務管理員功能有效率使用待命 RAS 閘道 VM 容量如果主動 RAS 閘道 VM 失敗或遺失連接提供可靠容錯移轉。  
  
如需詳細資訊，請查看[RAS 閘道可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_rr"></a>之前的路徑反映  
邊境閘道通訊協定 (BGP) 路由反映現在已隨附 RAS 閘道，並提供所需的之前的路徑同步處理路由器之間 BGP 完整網格拓撲的另一個方法。 完整網格同步處理的所有 BGP 路由器必須與所有其他路由器路由拓撲都連接。 當您使用之前的路徑反映時，不過，路由反映是與其他路由器，稱為 BGP 戶端，藉以簡化路由同步處理和降低網路流量的所有連接只有路由器。 之前的路徑反映學習所有路徑、 計算最佳路徑，並重新其 BGP 戶端的最佳路由分配。  
  
與 Windows Server 2016，您可以設定個人承租人遠端存取通道在多個 RAS 閘道 VM 終止。 這提供提高的彈性的雲端服務提供者所面臨環境的其中一個 RAS 閘道 VM 不符合所有的承租人連接的頻寬需求。  
  
這項功能，但是引進了其他複雜的之前的路徑管理及生效路徑承租人遠端網站和雲端的資料中心他們 virtual 資源之間同步處理。 連接 tenants 提供多個 RAS 閘道也引進了在企業結束時，每個承租人網站會有不同路由鄰居的位置設定的其他複雜。  
  
在控制平面 BGP 路由反映這些問題，並讓企業 tenants 透明 CSP 內部 fabric 部署。 以下是一些重點 BGP 路由反映隨附 RAS 閘道並整合 Network Controller 的相關。  
  
-   在軟體定義網路部署 A 路由反映是位於 RAS 閘道之間 Network Controller 的控制項平面邏輯實體。 這不會但是，參與資料平面路由。  
  
-   當您新增新的承租人資料中心時，Network Controller 會自動設定的第一個承租人 RAS 閘道為路由反映。  
  
-   每個承租人對應路由反映程式，而且位於 RAS 閘道 Vm 該承租人相關聯的其中一個。  
  
-   之前的路徑反映承租人作為路由反映 RAS 閘道 Vm 承租人相關聯的所有。 承租人閘道 RAS 閘道之前的路徑反映以外的之前的路徑反映戶端。 之前的路徑反映程式執行之前的路徑同步所有路由反映戶端之間，可能是實際資料路徑路由。  
  
-   A 路由反映不提供 RAS 閘道設定時，它之前的路徑反映服務。  
  
-   更新企業路徑對應至承租人的企業網站 Network Controller A 路由反映。 這可讓網路所需的 HYPER-V 網路模擬原則設定端點-資料路徑存取承租人 virtual 網路上的控制器。  
  
-   如果您的企業針對使用 BGP 路由客戶位址空間，RAS 閘道之前的路徑反映是針對所有網站的對應承租人只外部 BGP (eBGP) 鄰居。 也是如此無論企業承租人的通道結束點。 亦即，不論 CSP 中的 RAS 閘道 VM datacenter 終止承租人網站的網站來 VPN 通道，針對所有承租人網站 eBGP 等，路由反映。  
  
如需詳細資訊，請查看[RAS 閘道部署架構](RAS-Gateway-Deployment-Architecture.md)意見主題網際網路工程設計工作推動 (IETF) 要求和[RFC 4456 BGP 路由反映： 另一種方式來完整 Mesh 內部 BGP (IBGP)](https://tools.ietf.org/html/rfc4456)。  
  

