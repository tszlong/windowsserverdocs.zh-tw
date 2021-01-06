---
title: 適用於 SDN 的 RAS 閘道
description: 您可以使用本主題來瞭解 RAS 閘道，這是 Windows Server 2016 中以軟體為基礎的多租使用者邊界閘道協定 (BGP) 功能的路由器。
manager: grcusanz
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/07/2020
ms.openlocfilehash: d8bd138599001bdf31aa52c817b64d2475e9b783
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946024"
---
# <a name="ras-gateway-for-sdn"></a>適用於 SDN 的 RAS 閘道

>適用于： Windows Server (半年通道) 、適用于 SDN 的 Windows Server 2016 # # RAS 閘道


RAS 閘道是一種以軟體為基礎的多租使用者，邊界閘道協定 (BGP) 可使用 Hyper-v 網路虛擬化裝載多個租使用者虛擬網路的雲端服務提供者 (Csp) 和企業。 RAS 閘道會在實體網路與 VM 網路資源之間路由傳送網路流量，而不論其位置為何。 您可以在相同的實體位置或許多不同的位置，路由傳送網路流量。

多組織使用者共用是雲端基礎結構支援多個租使用者虛擬機器工作負載的能力，但會彼此隔離，但所有工作負載都是在相同的基礎結構上執行。 個別租用戶的多個工作負載可以互連並從遠端管理，但是這些系統不會與其他租用戶的工作負載互連，其他租用戶也無法從遠端管理它們。


> [!NOTE]
> 除了本主題之外，還提供下列 RAS 閘道主題。
>
> -   [RAS 閘道的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)
> -   [RAS 閘道部署架構](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)
> -   [RAS 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)
> -   [邊界閘道協定 &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)
> -   [BGP Windows PowerShell 命令參考](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)


## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>安裝 SDN 的 RAS 閘道的必要條件
當您想要以多租使用者模式部署 RAS 閘道以搭配 SDN 使用時，您無法使用 Windows 介面安裝遠端存取。 您必須改用 Windows PowerShell。

但您必須先使用 Windows PowerShell 來新增 **RemoteAccess** Windows 功能，才能使用 WINDOWS POWERSHELL 安裝 RAS 閘道。 若要這樣做，請在 Windows PowerShell 提示字元中執行下列命令。

`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`

此命令會新增 **RemoteAccess** 功能和功能的 Windows PowerShell 命令。

將 **RemoteAccess** 新增至伺服器之後，您可以將遠端存取安裝為具有多租使用者模式和邊界閘道協定 (BGP) 的 RAS 閘道。

如需詳細資訊，請參閱 Windows PowerShell 參考主題 [安裝-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx)。

## <a name="ras-gateway-features"></a>RAS 閘道功能
以下是 Windows Server 2016 中的 RAS 閘道功能。 您可以在高可用性集區中部署 RAS 閘道，以便一次使用所有這些功能。

-   **站對站 VPN**。 此 RAS 閘道功能可讓您使用站對站 VPN 連線，將兩個網路連線到網際網路上的不同實體位置。 對於在其資料中心裝載許多租使用者的 Csp，RAS 閘道提供多租使用者閘道解決方案，可讓您的租使用者透過來自遠端網站的站對站 VPN 連線來存取和管理其資源，並允許資料中心內的虛擬資源與其實體網路之間的網路流量流動。

-   **點對站 VPN**。 此 RAS 閘道功能可讓組織員工或系統管理員從遠端位置連線到您組織的網路。  針對多租使用者部署，租使用者網路系統管理員可以使用點對站 VPN 連線來存取 CSP 資料中心的虛擬網路資源。

-   **GRE 通道**。  (GRE) 型通道的一般路由封裝可讓租使用者虛擬網路與外部網路之間的連線。 由於 GRE 通訊協定是輕量型並且大部分的網路裝置均支援 GRE，它是資料不需要加密的通道的理想選擇。 站對站 (S2S) 通道中的 GRE 支援可解決使用多租使用者閘道在租使用者虛擬網路和租使用者外部網路之間轉送的問題，如本主題稍後所述。

-   **具有邊界閘道協定 (BGP) 的動態路由**。 BGP 可以降低在路由器上手動路由設定的需求，因為它是動態路由通訊協定，並且會自動學習使用站台對站台 VPN 連線來連接的網站之間的路由。 如果您的組織有多個使用啟用 BGP 的路由器（例如 RAS 閘道）來連線的網站，BGP 可讓路由器在發生網路中斷或失敗時，自動計算和使用有效的路由。 如需詳細資訊，請參閱 [RFC 4271](https://tools.ietf.org/html/rfc4271)。





