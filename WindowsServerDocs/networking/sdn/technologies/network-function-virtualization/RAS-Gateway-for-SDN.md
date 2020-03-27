---
title: 適用於 SDN 的 RAS 閘道
description: 您可以使用本主題來瞭解 RAS 閘道，這是 Windows Server 2016 中以軟體為基礎、多租使用者、邊界閘道協定（BGP）功能的路由器。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 415a5f4728b1b08e59630935e47f22d74b0267e8
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312935"
---
# <a name="ras-gateway-for-sdn"></a>適用於 SDN 的 RAS 閘道

>適用于： Windows Server （半年通道）、Windows Server 2016 # # 適用于 SDN 的 RAS 閘道  


RAS 閘道是一種以軟體為基礎、多租使用者、邊界閘道協定（BGP）功能的路由器，其適用于雲端服務提供者（Csp），以及裝載多個使用 Hyper-v 網路虛擬化之租使用者虛擬網路的企業。 RAS 閘道會在實體網路與 VM 網路資源之間路由傳送網路流量，而不論位置為何。 您可以在相同的實體位置或許多不同的位置路由傳送網路流量。   

多組織使用者管理是雲端基礎結構支援多個租使用者之虛擬機器工作負載的能力，但會彼此隔離，而所有的工作負載都是在相同的基礎結構上執行。 個別租用戶的多個工作負載可以互連並從遠端管理，但是這些系統不會與其他租用戶的工作負載互連，其他租用戶也無法從遠端管理它們。

  
> [!NOTE]  
> 除了本主題之外，還提供下列的 RAS 閘道主題。  
>   
> -   [RAS 閘道的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [RAS 閘道部署架構](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [RAS 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [邊界閘道協定&#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [BGP Windows PowerShell 命令參考](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
    
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>適用于 SDN 的 RAS 閘道安裝必要條件  
當您想要在多租使用者模式中部署 RAS 閘道以與 SDN 搭配使用時，您無法使用 Windows 介面來安裝遠端存取。 相反地，您必須使用 Windows PowerShell。  
  
但是，在您可以使用 Windows PowerShell 安裝 RAS 閘道之前，您必須使用 Windows PowerShell 來新增 [ **RemoteAccess** windows] 功能。 若要這麼做，請在 Windows PowerShell 提示字元中執行下列命令。  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
此命令會新增適用于此功能的**RemoteAccess**功能和 Windows PowerShell 命令。  
  
將**RemoteAccess**新增至伺服器之後，您可以將遠端存取安裝為具有多租使用者模式和邊界閘道協定（BGP）的 RAS 閘道。  
  
如需詳細資訊，請參閱 Windows PowerShell 參考主題[Install-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx)。  
  
## <a name="ras-gateway-features"></a>RAS 閘道功能  
以下是 Windows Server 2016 中的 RAS 閘道功能。 您可以在高可用性集區中部署 RAS 閘道，以便一次使用所有這些功能。  
  
-   **站對站 VPN**。 此 RAS 閘道功能可讓您使用站對站 VPN 連線，透過網際網路連接位於不同實體位置上的兩個網路。 對於在其資料中心裝載許多租使用者的 Csp，RAS 閘道會提供多租使用者閘道解決方案，讓您的租使用者可透過來自遠端網站的站對站 VPN 連線來存取和管理其資源，並允許其間的網路流量資料中心內的虛擬資源和其實體網路。  
  
-   **點對站 VPN**。 此 RAS 閘道功能可讓組織員工或系統管理員從遠端位置連線到您組織的網路。  針對多組織使用者共用部署，租使用者網路系統管理員可以使用點對站 VPN 連線來存取 CSP 資料中心的虛擬網路資源。  
  
-   **GRE 通道**。 一般路由封裝（GRE）通道會啟用租使用者虛擬網路與外部網路之間的連線。 由於 GRE 通訊協定是輕量型並且大部分的網路裝置均支援 GRE，它是資料不需要加密的通道的理想選擇。 站對站（S2S）通道中的 GRE 支援可解決使用多租使用者閘道在租使用者虛擬網路與租使用者外部網路之間轉送的問題，如本主題稍後所述。  
  
-   **使用邊界閘道協定（BGP）的動態路由**。 BGP 可以降低在路由器上手動路由設定的需求，因為它是動態路由通訊協定，並且會自動學習使用站台對站台 VPN 連線來連接的網站之間的路由。 如果您的組織有多個使用已啟用 BGP 的路由器連線的網站，例如 RAS 閘道，則 BGP 可讓路由器在網路中斷或失敗時，自動計算並使用有效的路由。 如需詳細資訊，請參閱[RFC 4271](https://tools.ietf.org/html/rfc4271)。  
  

  


