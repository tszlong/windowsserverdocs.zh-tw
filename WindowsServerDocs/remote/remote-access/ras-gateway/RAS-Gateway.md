---
title: RAS 閘道
description: 本主題中，這是適用於資訊技術 (IT) 專業人員，提供 RAS 閘道，包括 Windows Server 2016 中的 RAS 閘道部署模式和功能的概觀資訊。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: pashort
author: shortpatti
ms.date: 05/23/2018
ms.openlocfilehash: d61dcdbb61449bd2af57b8e2c99ced6235c4deca
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281257"
---
# <a name="ras-gateway"></a>RAS 閘道

>適用於：Windows Server （半年通道），Windows Server 2016

RAS 閘道是軟體路由器，以及您可以使用多租用戶的模式或單一租用戶模式中的閘道。  
  
- **單一租用戶**模式可讓部署成外部，或網際網路對向 edge 虛擬私人網路 (VPN) 和 DirectAccess 伺服器的閘道各種規模的組織。 在單一租用戶模式中，您可以在實體伺服器或執行 Windows Server 2016 的虛擬機器 (VM) 上部署 RAS 閘道。  
  
- **多租用戶模式**可讓雲端服務提供者 (Csp) 和企業使用 RAS 閘道，以啟用資料中心和雲端虛擬和實體網路，包括網際網路之間路由的網路流量。 多租用戶模式中，建議您將部署 RAS 閘道，在執行 Windows Server 2016 的 Vm 上。  
  
> [!NOTE]  
> RAS 閘道支援 IPv4 與 IPv6，包括 IPv4 與 IPv6 轉送。 當您設定 RAS 閘道與網路位址轉譯 (NAT) 時，只支援 nat44。  
  
## <a name="who-will-be-interested-in-ras-gateway"></a>誰會想要 RAS 閘道？
  
如果您是系統管理員、 網路架構師或其他 IT 專業人員，RAS 閘道可能會引起您對一或多個下列情況：  
  
-   您為正在使用或計劃使用 Hyper-V 在虛擬網路上部署虛擬機器 (VM) 的組織設計或支援 IT 基礎結構。  
  
-   您為已部署或計劃部署雲端技術的組織設計或支援 IT 基礎結構。  
  
-   您想提供實體網路和虛擬網路之間的完整網路連線。  
  
-   您想要透過網際網路存取其虛擬網路提供組織的客戶。  
  
-   您想要提供您的組織網路的遠端存取您組織的員工。  
  
-   您想要透過網際網路連線位於不同實體位置的辦公室。  
 
本主題中，這是適用於資訊技術 (IT) 專業人員，提供 RAS 閘道，包括 RAS 閘道部署模式和功能的概觀資訊。 
  
本主題涵蓋下列各節。  
  
  
-   [RAS 閘道部署模式](#bkmk_modes)  
  
-   [高可用性叢集 RAS 閘道](#bkmk_clustering)  
  
-   [RAS 閘道功能](#bkmk_features)  
  
-   [RAS 閘道部署案例](#bkmk_deploy)  
  
-   [RAS 閘道管理工具](#bkmk_manage)  
  


  
## <a name="bkmk_modes"></a>RAS 閘道部署模式  
RAS 閘道包含下列部署模式。  
  
### <a name="single-tenant-mode"></a>單一租用戶模式  
對於大部分的組織而言，在單一租用戶模式中使用 RAS 閘道是一般的設定。 在單一租用戶模式中，您可以部署 RAS 閘道為邊緣 VPN 伺服器、 邊緣 DirectAccess 伺服器，或兩者同時。 在此組態中，RAS 閘道會提供遠端員工，連線到您的網路使用 VPN 或 DirectAccess 連線。 此外，單一租用戶模式可讓您透過網際網路連線位於不同實體位置的辦公室。  
  
### <a name="multitenant-mode"></a>多租用戶的模式  
如果您的組織是 CSP 或具備多個租用戶的企業，您可以在多租用戶的模式，以提供網路流量路由與虛擬和實體網路中部署 RAS 閘道。  
  
多租用戶是雲端基礎結構以支援多個租用戶的虛擬機器工作負載，尚未將它們隔離彼此，而所有的工作負載相同的基礎結構上執行的能力。 個別租用戶的多個工作負載可以互連並從遠端管理，但是這些系統不會與其他租用戶的工作負載互連，其他租用戶也無法從遠端管理它們。  
  
例如，一個企業可能有許多不同的虛擬子網路，每個虛擬子網路都專門用來服務特定的部門，像是研發部門或會計部門。 在另一個例子中，CSP 有許多租用戶，他們在相同的實體資料中心有隔離的虛擬子網路。 在這兩種情況下，RAS 閘道可以將流量路由與每個租用戶同時維持每個租用戶的獨立性。 這項功能可讓多名租戶感知 RAS 閘道。  
  
使用 HYPER-V 網路虛擬化，也就是一種技術，Windows Server 2012 中引進，進而改善 Windows Server 2016 中，會建立虛擬網路。 RAS 閘道與 HYPER-V 網路虛擬化整合，而且能夠有效路由傳送網路流量有許多不同客戶或租用戶-有隔離虛擬網路中相同的資料中心。  
  
HYPER-V 網路虛擬化為您提供部署的基礎實體網路無關的虛擬機器 (VM) 網路的能力。 利用一或多個虛擬子網路的組成的 VM 網路 IP 子網路的實際實體位置被分隔的虛擬網路拓撲。 如此一來，您可以輕鬆地將您在內部部署子網路移至雲端-，同時保留您現有的 IP 位址和在雲端中的拓撲中。 這種保留基礎結構的能力可讓現有服務繼續運作，不用知道子網路的實體位置。 也就是說，Hyper-V 網路虛擬化有助於順暢進行混合雲端。  
  
> [!NOTE]  
> HYPER-V 網路虛擬化是使用網路虛擬化 Generic Routing Encapsulation 的網路覆蓋技術 ([NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00))，允許租用戶帶入他們自己的位址空間，且讓 Csp 擁有更好的延展性比可使用的租用戶隔離的 Vlan 來實現。  
  
在 Windows Server 2016 中，RAS 閘道會路由傳送網路流量之間的實體網路和 VM 網路資源，無論資源位於何處。 您可以使用 RAS 閘道之間路由傳送網路流量位於相同的實體位置或許多不同實體位置的實體和虛擬網路。  
  
例如，如果您有實體網路和虛擬網路位於相同的實體位置時，您可以部署執行設定做為轉寄閘道，並將虛擬與實體之間的流量路由傳送到 RAS 閘道 vm 的 HYPER-V 的電腦網路。  
  
另舉一例，如果您的虛擬網路存在於雲端中，CSP 可以部署 RAS 閘道，讓您可以在您的 VPN 伺服器與 CSP 的 RAS 閘道; 之間建立虛擬私人網路 (VPN) 站台對站連線當建立這個連結之後您可以透過 VPN 連線來連接雲端中的虛擬資源。  
  
如需詳細資訊，請參閱 < [RAS 閘道高可用性](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_clustering"></a>高可用性叢集 RAS 閘道  
RAS 閘道部署在執行 HYPER-V 且有一個 VM 設定在專用電腦上。 VM 然後設定為 RAS 閘道。  
  
網路資源的高可用性，您可以使用兩部實體主機伺服器執行 HYPER-V 的每個同時執行的虛擬機器 (VM) 設定為閘道的 RAS 閘道部署具有容錯移轉。 然後，閘道 VM 設定為叢集，針對網路中斷和硬體故障提供容錯移轉保護。  
  
例如，如果您的組織是使用私人雲端部署的企業，您可能需要只有兩個 RAS 閘道 Vm，其中每個執行 HYPER-V 的不同電腦安裝。 在此案例中，RAS 閘道 Vm 會加入叢集，以提供高可用性。  
  
另舉一例，如果您的組織是雲端服務提供者 (CSP) 與兩百個租用戶中您的資料中心，您可以使用八個 RAS 閘道的 Vm，與每個成對叢集 RAS 閘道 Vm 提供五十個租用戶的路由服務。 在此案例中，兩部執行 HYPER-V 每個的電腦會有四個 Vm 設定為 RAS 閘道。 接著設定四個 RAS 閘道 VM 叢集，每個叢集包含一個 VM，每一部執行 HYPER-V 的電腦。  
  
當您部署 RAS 閘道，執行 HYPER-V 和您設定為閘道必須執行 Windows Server 2012 R2 或 Windows Server 2016 Vm 的主機伺服器。  
  
## <a name="bkmk_features"></a>RAS 閘道功能  
RAS 閘道包含下列功能。  
  
-   **站對站 VPN**。 此 RAS 閘道的功能可讓您在網際網路上的不同實體位置的兩個網路連接使用站對站 VPN 連線。 如果您有個主要辦公室和多個分公司辦公室，您可以部署 RAS 閘道，每個位置的邊緣，並建立站對站連接，以提供位置之間的網路流量。 裝載在其資料中心內的多個租用戶的 csp，RAS 閘道提供的多租用戶閘道解決方案，可讓您的租用戶來存取和管理其資源，透過站對站 VPN 連線，從遠端站台，並可讓之間的網路流量在您的資料中心和他們的實體網路的虛擬資源。  
  
-   **點對站 VPN**。 此 RAS 閘道的功能可讓組織員工或系統管理員從遠端位置連線到您的組織網路。 單一租用戶部署 RAS 閘道，遠端員工可以使用 VPN 連線連接到您的組織網路。 此連線可讓使用者使用內部網路資源，例如內部網路網站和檔案伺服器。 多租用戶的部署中，租用戶網路系統管理員可以使用點對站 VPN 連線存取 CSP 資料中心內的虛擬網路資源。  
  
-   **動態路由使用邊界閘道通訊協定 (BGP)** 。 BGP 可以降低在路由器上手動路由設定的需求，因為它是動態路由通訊協定，並且會自動學習使用站台對站台 VPN 連線來連接的網站之間的路由。 如果您的組織有多個站台使用例如 RAS 閘道啟用 BGP 的路由器連線，BGP 可讓自動計算，並使用彼此發生網路中斷或失敗時的有效路由的路由器。 如需詳細資訊，請參閱 < [RFC 4271](https://tools.ietf.org/html/rfc4271)。  
  
-   **網路位址轉譯 (NAT)** 。 網路位址轉譯 (NAT) 可讓您與單一的公用 IP 位址共用公用網際網路，透過單一介面的連線。 私人網路上的電腦使用私用、 非可路由的位址。 NAT 會將私人位址對應到公用位址。 此 RAS 閘道的功能可讓組織使用單一租用戶部署中的員工存取從閘道後方的網際網路資源。 Csp，這項功能可讓租用戶來存取網際網路的 Vm 執行的應用程式。 比方說，租用戶已設定為 Web 伺服器的 VM 可以連絡外部的財務資源可處理信用卡交易。  

  
## <a name="bkmk_deploy"></a>RAS 閘道部署案例  
以下是建議的部署案例，RAS 閘道。  
  
-   **企業邊緣-單一租用戶部署**。 使用單一租用戶的企業部署，您可以連接一個實體到多個其他實體位置透過網際網路使用站對站 VPN 功能-和邊界閘道通訊協定 (BGP) 可讓您使用動態路由。 您也可以提供您組織網路的遠端員工存取使用點對站 VPN 連線和 DirectAccess 連線。 （DirectAccess 連線會永遠啟用，而且也提供的好處是您可以輕鬆地管理連線的電腦使用 DirectAccess，因為它們連接時才在和網際網路連線）。您也可以設定使用 NAT，單一租用戶企業 RAS 閘道，以便在您的內部網路上的電腦可以輕鬆地會與網際網路通訊。  
  
-   **雲端服務提供者邊緣多租用戶部署**。 RAS 閘道的 Csp 的多租用戶部署可讓您提供給租用戶的所有功能所提供的企業邊緣單一租用戶部署。 在您的資料中心中的租用戶虛擬網路和網際網路表示租用戶需要不間斷地存取其雲端資源的時間之間的租用戶網路位置之間的站對站 VPN 連線。 租用戶的點對站 VPN 存取表示租用戶系統管理員可以隨時連接到其資料中心內管理其資源中的虛擬網路。 BGP 會提供動態路由，並讓租用戶連接到其資產，即使網路問題發生在網際網路上或其他位置。 與 NAT 可讓租用戶 Vm 連線到網際網路，例如處理資源的信用卡上的資源。  
  
## <a name="bkmk_manage"></a>RAS 閘道管理工具  
以下是 RAS 閘道的管理工具。  
  
-   在 Windows Server 2016 中，若要部署 RAS 閘道路由器，您必須使用 Windows PowerShell 命令。 如需詳細資訊，請參閱 <<c0> [ 的遠端存取 Cmdlet](https://docs.microsoft.com/powershell/module/remoteaccess)適用於 Windows Server 2016 與 Windows 10。  
  
-   在 System Center 2012 R2 Virtual Machine Manager (VMM)，RAS 閘道是名為 Windows Server 閘道。 一組有限的邊界閘道通訊協定 (BGP) 設定選項都是在 VMM 軟體介面，包括**本機 BGP IP 位址**並**自發系統編號 (ASN)** ， **BGP 對等 IP 位址清單**，並**ASN 值**。 不過，您可以使用遠端存取 Windows PowerShell BGP 命令來設定 Windows Server 閘道的其他所有功能。 如需詳細資訊，請參閱 < [Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/vmm)並[遠端存取 Cmdlet](https://technet.microsoft.com/library/hh918399.aspx)適用於 Windows Server 2016 與 Windows 10。  
  
## <a name="related-topics"></a>相關主題
- [RAS 閘道高可用性](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
- [Windows Server 中的 GRE 通道](gre-tunneling-windows-server.md)
- [RAS 閘道 GRE 通道輸送量及效能](RAS-Gateway-GRE-Perf.md)
