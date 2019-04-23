---
title: 適用於 SDN 的 RAS 閘道
description: 若要深入了解 RAS 閘道，也就是以軟體為基礎，多租用戶、 邊界閘道通訊協定 (BGP) 功能的路由器，Windows Server 2016 中，您可以使用本主題。
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
ms.openlocfilehash: 4f1ad0b3f0b5921a53faa8a45baae9f0b8711873
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856269"
---
# <a name="ras-gateway-for-sdn"></a>適用於 SDN 的 RAS 閘道

>適用於：Windows Server （半年通道），Windows Server 2016 的 # # 適用於 SDN 的 RAS 閘道  


RAS 閘道是軟體為基礎的多租用戶，邊界閘道通訊協定 (BGP) 功能的路由器專為雲端服務提供者 (Csp) 和企業該主控件使用 HYPER-V 網路虛擬化的多個租用戶虛擬網路。 RAS 閘道路由傳送網路流量之間的實體網路和 VM 網路資源，不論位置為何。 您可以路由傳送網路流量，在相同的實體位置或許多不同的位置。   

多租用戶是雲端基礎結構以支援多個租用戶的虛擬機器工作負載，尚未將它們隔離彼此，而所有的工作負載相同的基礎結構上執行的能力。 個別租用戶的多個工作負載可以互連並從遠端管理，但是這些系統不會與其他租用戶的工作負載互連，其他租用戶也無法從遠端管理它們。

  
> [!NOTE]  
> 本主題中，除了下列的 RAS 閘道主題可用。  
>   
> -   [RAS 閘道中最新消息](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [RAS 閘道部署架構](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [RAS 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [邊界閘道通訊協定&#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [BGP Windows PowerShell 命令參考](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
    
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>安裝適用於 SDN 的 RAS 閘道的必要條件  
您無法使用 Windows 介面安裝遠端存取，當您想要部署在多租用戶的模式，以搭配使用 SDN 的 RAS 閘道。 相反地，您必須使用 Windows PowerShell。  
  
您也可以使用 Windows PowerShell 來安裝 「 RAS 閘道之前，您必須使用 Windows PowerShell 來新增，但**RemoteAccess** Windows 功能。 若要這樣做，請在 Windows PowerShell 提示字元執行下列命令。  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
此命令會新增**RemoteAccess**功能和功能的 Windows PowerShell 命令。  
  
加入之後**RemoteAccess**到您的伺服器，您可以安裝遠端存取做為多租用戶的模式與 RAS 閘道和邊界閘道通訊協定 (BGP)。  
  
如需詳細資訊，請參閱 Windows PowerShell 參考主題[Install-remoteaccess](https://technet.microsoft.com/library/hh918408.aspx)。  
  
## <a name="ras-gateway-features"></a>RAS 閘道功能  
以下是 Windows Server 2016 中的 RAS 閘道功能。 您可以部署一次使用的所有這些功能的高可用性集區中的 RAS 閘道。  
  
-   **站對站 VPN**。 此 RAS 閘道的功能可讓您在網際網路上的不同實體位置的兩個網路連接使用站對站 VPN 連線。 裝載在其資料中心內的多個租用戶的 csp，RAS 閘道提供的多租用戶閘道解決方案，可讓您的租用戶來存取和管理其資源，透過站對站 VPN 連線，從遠端站台，並可讓之間的網路流量在您的資料中心和他們的實體網路的虛擬資源。  
  
-   **點對站 VPN**。 此 RAS 閘道的功能可讓組織員工或系統管理員從遠端位置連線到您的組織網路。  多租用戶的部署中，租用戶網路系統管理員可以使用點對站 VPN 連線存取 CSP 資料中心內的虛擬網路資源。  
  
-   **GRE 通道**。 Generic Routing Encapsulation (GRE) 為基礎租用戶虛擬網路與外部網路之間的通道啟用連線。 由於 GRE 通訊協定是輕量型並且大部分的網路裝置均支援 GRE，它是資料不需要加密的通道的理想選擇。 站對站 (S2S) 通道中的 GRE 支援解決租用戶虛擬網路與使用多租用戶閘道的租用戶外部網路之間轉送的問題，如本主題稍後所述。  
  
-   **動態路由使用邊界閘道通訊協定 (BGP)**。 BGP 可以降低在路由器上手動路由設定的需求，因為它是動態路由通訊協定，並且會自動學習使用站台對站台 VPN 連線來連接的網站之間的路由。 如果您的組織有多個站台使用例如 RAS 閘道啟用 BGP 的路由器連線，BGP 可讓自動計算，並使用彼此發生網路中斷或失敗時的有效路由的路由器。 如需詳細資訊，請參閱 < [RFC 4271](https://tools.ietf.org/html/rfc4271)。  
  

  


