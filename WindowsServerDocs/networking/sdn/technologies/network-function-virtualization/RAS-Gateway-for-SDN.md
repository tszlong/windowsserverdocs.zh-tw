---
title: 適用於 SDN RAS 閘道
description: 若要了解有關 RAS 閘道，是軟體為基礎，multitenant、Windows Server 2016 中的邊框閘道通訊協定 (BGP) 可路由器，您可以使用此主題。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 052911dcd52df82ef4e259de0c64078c54f00195
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-for-sdn"></a>適用於 SDN RAS 閘道

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要深入了解 RAS 閘道，也就是軟體、multitenant，邊境閘道通訊協定 (BGP) 可路由器專為雲端服務提供者 (Csp) 和主機多個承租人 virtual 網路使用 HYPER-V 網路模擬針對企業設計的 Windows Server 2016 中，您可以使用此主題。  
  
> [!NOTE]  
> 本主題中，除了 RAS 閘道下列主題會提供。  
>   
> -   [RAS 閘道中的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [RAS 閘道部署架構](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [RAS 閘道可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [邊境閘道通訊協定與 #40;BGP 和 #41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [BGP Windows PowerShell 命令參考資料](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
在 Windows Server 2016 RAS 閘道傳送網路流量之間的實體網路和 VM 網路資源，無論資源的所在位置。 您可以使用 RAS 閘道，將在相同的實體位置或網際網路上許多不同的所在位置的實體和 virtual 網路間網路流量。  
  
Multitenancy」是雲端基礎結構支援一樣工作負載的多個 tenants、尚未隔離它們各，相同的基礎結構所有的工作負載執行時的能力。 多個工作負載的個人承租人可以連接和管理遠端電腦上，但這些系統不執行的工作負載的其他 tenants、連接，也可以其他 tenants 遠端管理。  
  
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>適用於安裝 SDN RAS 閘道必要條件  
您無法使用 Windows 介面當您想要使用的 SDN multitenant 模式部署 RAS 閘道安裝遠端存取。 您必須改用 Windows PowerShell。  
  
但您可以使用 Windows PowerShell 來安裝 RAS 閘道之前，您必須使用 Windows PowerShell 來新增**執行**Windows 功能。 若要這樣做，請在 Windows PowerShell 命令提示字元中執行下列命令。  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
新增此命令**執行**功能，以及對於該功能的 Windows PowerShell 命令。  
  
您加入後**執行**以您的伺服器，您可以安裝遠端存取 multitenant 模式 RAS 閘道和邊境閘道通訊協定 (BGP)。  
  
如需詳細資訊，查看 Windows PowerShell 參考主題[安裝-執行](https://technet.microsoft.com/library/hh918408.aspx)。  
  
## <a name="ras-gateway-features"></a>RAS 閘道功能  
以下是 Windows Server 2016 RAS 閘道功能。 您可以部署 RAS 閘道一次使用所有的這些功能的可用性集區中。  
  
-   **網站-VPN**。 此 RAS 閘道功能可讓您使用的網站來 VPN 連接透過網際網路連接兩個實體的不同位置的網路。 主機在其 datacenter 許多 tenants Csp，RAS 閘道提供可讓您存取及管理他們的資源從遠端網站，以網站 VPN 連接到 tenants 和，可在您的資料中心 virtual 資源和實體網路間網路流量 multitenant 閘道方案。  
  
-   **點對網站 VPN**。 此 RAS 閘道功能可讓組織員工或從遠端位置連接至您組織的網路系統管理員。  Multitenant 部署，承租人網路系統管理員可以存取 virtual 網路資源，CSP datacenter 使用點對網站 VPN 連接。  
  
-   **GRE 通道**。 一般路由封裝 (GRE) 根據承租人 virtual 網路之間外部網路的通道讓連接。 因為 GRE 通訊協定輕量型與支援 GRE 是可在大部分的網路的裝置上使用它將會變成的資料加密不需要理想選擇的通道。 支援網站 (S2S) 通道 GRE 下轉接承租人 virtual 網路與承租人外部網路使用多承租人閘道，之間的稍後本主題中所述  
  
-   **動態路由的邊境上閘道通訊協定 (BGP)**。 BGP 減少需要手動路由路由器設定，因為它是動態路由通訊協定，並自動學習所使用的網站 VPN 連接連接之間的路徑。 如果您的組織會有多個網站使用 BGP 支援路由器，例如 RAS 閘道器連接，BGP 可路由器自動計算，並使用有效路徑彼此干擾網路或失敗。 如需詳細資訊，請查看[RFC 4271](https://tools.ietf.org/html/rfc4271)。  
  

  


