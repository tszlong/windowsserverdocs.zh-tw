---
title: RAS 閘道
description: 本主題適用于 (IT) 專業人員提供的資訊技術，提供 RAS 閘道的總覽資訊，包括 Windows Server 2016 中的 RAS 閘道部署模式和功能。
manager: dougkim
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: lizross
author: eross-msft
ms.date: 05/23/2018
ms.openlocfilehash: 9e8dfd54fc919fa857ad360eb86baf47f8c3e274
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865207"
---
# <a name="ras-gateway"></a>RAS 閘道

>適用於：Windows Server (半年度管道)、Windows Server 2016

RAS 閘道是一種軟體路由器和閘道，您可以在單一租使用者模式或多租使用者模式中使用。

- **單一租** 使用者模式可讓任何規模的組織將閘道部署為外部或網際網路面向的邊緣虛擬私人網路， (VPN) 和 DirectAccess 伺服器。 在單一租使用者模式中，您可以在執行 Windows Server 2016 (VM) 的實體伺服器或虛擬機器上部署 RAS 閘道。

- 多租使用者 **模式** 可讓雲端服務提供者 (csp) 和企業使用 RAS 閘道，在虛擬和實體網路（包括網際網路）之間啟用資料中心和雲端網路流量路由。 若為多租使用者模式，建議您在執行 Windows Server 2016 的 Vm 上部署 RAS 閘道。

> [!NOTE]
> RAS 閘道支援 IPv4 和 IPv6，包括 IPv4 和 IPv6 轉送。 當您使用 (NAT) 的網路位址轉譯來設定 RAS 閘道時，僅支援 NAT44。

## <a name="who-will-be-interested-in-ras-gateway"></a>哪些人會對 RAS 閘道有興趣？

如果您是系統管理員、網路架構師或其他 IT 專業人員，RAS 閘道可能會在下列一或多種情況下感興趣：

-   您為正在使用或計劃使用 Hyper-V 在虛擬網路上部署虛擬機器 (VM) 的組織設計或支援 IT 基礎結構。

-   您為已部署或計劃部署雲端技術的組織設計或支援 IT 基礎結構。

-   您想提供實體網路和虛擬網路之間的完整網路連線。

-   您想要為組織的客戶提供透過網際網路存取其虛擬網路的許可權。

-   您想要為組織的員工提供組織網路的遠端存取權。

-   您想要將辦公室連接到網際網路上不同的實體位置。

本主題適用于 (IT) 專業人員提供的資訊技術，提供 RAS 閘道的總覽資訊，包括 RAS 閘道部署模式和功能。

本主題包含下列各節。


-   [RAS 閘道部署模式](#bkmk_modes)

-   [叢集 RAS 閘道以獲得高可用性](#bkmk_clustering)

-   [RAS 閘道功能](#bkmk_features)

-   [RAS 閘道部署案例](#bkmk_deploy)

-   [RAS 閘道管理工具](#bkmk_manage)




## <a name="ras-gateway-deployment-modes"></a><a name="bkmk_modes"></a>RAS 閘道部署模式
RAS 閘道包含下列部署模式。

### <a name="single-tenant-mode"></a>單一租使用者模式
對於大部分的組織而言，在單一租使用者模式中使用 RAS 閘道是一般的設定。 在單一租使用者模式中，您可以將 RAS 閘道部署為邊緣 VPN 伺服器、edge DirectAccess 伺服器，或同時部署兩者。 在此設定中，RAS 閘道可讓遠端員工使用 VPN 或 DirectAccess 連線來連線到您的網路。 此外，單一租使用者模式可讓您將辦公室連接到網際網路上不同的實體位置。

### <a name="multitenant-mode"></a>多租使用者模式
如果您的組織是具有多個租使用者的 CSP 或企業，您可以在多租使用者模式中部署 RAS 閘道，以提供進出虛擬和實體網路的網路流量。

多組織使用者共用是雲端基礎結構支援多個租使用者虛擬機器工作負載的能力，但會彼此隔離，但所有工作負載都是在相同的基礎結構上執行。 個別租用戶的多個工作負載可以互連並從遠端管理，但是這些系統不會與其他租用戶的工作負載互連，其他租用戶也無法從遠端管理它們。

例如，一個企業可能有許多不同的虛擬子網路，每個虛擬子網路都專門用來服務特定的部門，像是研發部門或會計部門。 在另一個例子中，CSP 有許多租用戶，他們在相同的實體資料中心有隔離的虛擬子網路。 在這兩種情況下，RAS 閘道可以將流量路由傳送至每個租使用者，同時維持每個租使用者的設計隔離。 這項功能可讓 RAS 閘道多租使用者感知。

虛擬網路是使用 Hyper-v 網路虛擬化建立的，這是 Windows Server 2012 中引進的一項技術，並已在 Windows Server 2016 中改善。 RAS 閘道已與 Hyper-v 網路虛擬化整合，並且能夠在有許多不同的客戶或租使用者（在相同資料中心內有隔離的虛擬網路）的情況下，有效地路由傳送網路流量。

Hyper-v 網路虛擬化可讓您將虛擬機器部署在與基礎實體網路無關的 (VM) 網路上。 使用由一或多個虛擬子網組成的 VM 網路，IP 子網的確切實體位置會與虛擬網路拓撲分離。 因此，您可以輕鬆地將內部部署子網移至雲端，同時保留雲端中現有的 IP 位址和拓撲。 這種保留基礎結構的能力可讓現有服務繼續運作，不用知道子網路的實體位置。 也就是說，Hyper-V 網路虛擬化有助於順暢進行混合雲端。

> [!NOTE]
> Hyper-v 網路虛擬化是使用網路虛擬化泛型路由封裝 ([NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00)) 的網路重迭技術，可讓租使用者攜帶自己的位址空間，並讓 csp 比使用 vlan 進行租使用者隔離更能提供更佳的擴充性。

在 Windows Server 2016 中，RAS 閘道會在實體網路與 VM 網路資源之間路由傳送網路流量，而不論資源位於何處。 您可以使用 RAS 閘道，在相同實體位置或許多不同的實體位置上的實體和虛擬網路之間路由傳送網路流量。

例如，如果您的實體網路和虛擬網路位於相同的實體位置，則您可以部署一部執行 Hyper-v 的電腦，該電腦已設定為使用 RAS 閘道 VM 作為轉送閘道，並在虛擬和實體網路之間路由傳送流量。

在另一個範例中，如果您的虛擬網路存在於雲端中，則您的 CSP 可以部署 RAS 閘道，讓您可以在 VPN 伺服器與 CSP 的 RAS 閘道之間建立虛擬私人網路， (VPN) 站對站連線;建立此連結時，您可以透過 VPN 連線連接到雲端中的虛擬資源。

如需詳細資訊，請參閱 [RAS 閘道高可用性](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。

## <a name="clustering-ras-gateway-for-high-availability"></a><a name="bkmk_clustering"></a>叢集 RAS 閘道以獲得高可用性
RAS 閘道是部署在執行 Hyper-v 且已設定一個 VM 的專用電腦上。 然後 VM 會設定為 RAS 閘道。

若要取得網路資源的高可用性，您可以使用兩部執行 Hyper-v 的實體主機伺服器來部署 RAS 閘道，這些伺服器分別執行設定為閘道的虛擬機器 (VM) 。 然後，閘道 VM 設定為叢集，針對網路中斷和硬體故障提供容錯移轉保護。

例如，如果您的組織是具有私人雲端部署的企業，您可能只需要兩個 RAS 閘道 Vm，每個 Vm 都安裝在另一部執行 Hyper-v 的電腦上。 在此案例中，RAS 閘道 Vm 會新增至叢集以提供高可用性。

在另一個範例中，如果您的組織是您的資料中心內有200個租使用者的雲端服務提供者 (CSP) ，您可以使用八個 RAS 閘道 Vm，並為50租使用者提供路由服務的每一對叢集 RAS 閘道 Vm。 在此案例中，兩部執行 Hyper-v 的電腦都有四個設定為 RAS 閘道的 Vm。 接著，您可以設定四個 RAS 閘道 VM 叢集，每個叢集都包含一部執行 Hyper-v 的電腦的 VM。

當您部署 RAS 閘道時，執行 Hyper-v 的主機伺服器以及您設定為閘道的 Vm，都必須執行 Windows Server 2012 R2 或 Windows Server 2016。

## <a name="ras-gateway-features"></a><a name="bkmk_features"></a>RAS 閘道功能
RAS 閘道包含下列功能。

-   **站對站 VPN**。 此 RAS 閘道功能可讓您使用站對站 VPN 連線，將兩個網路連線到網際網路上的不同實體位置。 如果您有總公司和多個分公司，您可以在每個位置部署 edge RAS 閘道，並建立站對站連線，以提供位置之間的網路流量。 對於在其資料中心裝載許多租使用者的 Csp，RAS 閘道提供多租使用者閘道解決方案，可讓您的租使用者透過來自遠端網站的站對站 VPN 連線來存取和管理其資源，並允許資料中心內的虛擬資源與其實體網路之間的網路流量流動。

-   **點對站 VPN**。 此 RAS 閘道功能可讓組織員工或系統管理員從遠端位置連線到您組織的網路。 針對 RAS 閘道的單一租使用者部署，遠端員工可以使用 VPN 連線來連接到您的組織網路。 此連接可讓它們使用內部網路資源，例如內部網路網站和檔案伺服器。 針對多租使用者部署，租使用者網路系統管理員可以使用點對站 VPN 連線來存取 CSP 資料中心的虛擬網路資源。

-   **具有邊界閘道協定 (BGP) 的動態路由**。 BGP 可以降低在路由器上手動路由設定的需求，因為它是動態路由通訊協定，並且會自動學習使用站台對站台 VPN 連線來連接的網站之間的路由。 如果您的組織有多個使用啟用 BGP 的路由器（例如 RAS 閘道）來連線的網站，BGP 可讓路由器在發生網路中斷或失敗時，自動計算和使用有效的路由。 如需詳細資訊，請參閱 [RFC 4271](https://tools.ietf.org/html/rfc4271)。

-   **(NAT) 的網路位址轉譯**。 網路位址轉譯 (NAT) 可讓您透過單一公用 IP 位址的單一介面，共用與公用網際網路的連線。 私人網路上的電腦會使用私人、無法路由傳送的位址。 NAT 會將私人位址對應到公用位址。 此 RAS 閘道功能讓具有單一租使用者部署的組織員工可以從閘道後方存取網際網路資源。 針對 Csp，這項功能可讓在租使用者 Vm 上執行的應用程式存取網際網路。 例如，設定為 Web 服務器的租使用者 VM 可以聯繫外部財務資源來處理信用卡交易。


## <a name="ras-gateway-deployment-scenarios"></a><a name="bkmk_deploy"></a>RAS 閘道部署案例
以下是針對 RAS 閘道建議的部署案例。

-   **企業邊緣-單一租使用者部署**。 透過單一租使用者企業部署，您可以使用站對站 VPN 功能，在網際網路上將一個實體連接到多個其他實體位置，而邊界閘道協定 (BGP) 可讓您使用動態路由。 您也可以透過點對站 VPN 連線和 DirectAccess 連線，為遠端員工提供組織網路的存取權。  (DirectAccess 連線一律為開啟狀態，而且也提供可讓您輕鬆管理使用 DirectAccess 連線之電腦的優勢，因為它們會在每次連線並聯機到網際網路時連線。 ) 您也可以使用 NAT 設定單一租使用者企業 RAS 閘道，讓內部網路上的電腦可以輕鬆地與網際網路通訊。

-   **雲端服務提供者 Edge-** 多租使用者部署。 適用于 Csp 的 RAS 閘道多租使用者部署，可讓您為租使用者提供企業 Edge 單一租使用者部署所提供的所有功能。 您資料中心內的租使用者虛擬網路與網際網路上的租使用者網路位置之間的站對站 VPN 連線，表示租使用者隨時都能順暢地存取其雲端資源。 租使用者的點對站 VPN 存取，表示租使用者系統管理員一律可以連線至您資料中心內的虛擬網路，以管理其資源。 BGP 提供動態路由，即使網際網路上或其他地方發生網路問題，也會讓租使用者與其資產保持連線。 和 NAT 可讓租使用者 Vm 連接到網際網路上的資源，例如信用卡處理資源。

## <a name="ras-gateway-management-tools"></a><a name="bkmk_manage"></a>RAS 閘道管理工具
以下是 RAS 閘道的管理工具。

-   在 Windows Server 2016 中，若要部署 RAS 閘道路由器，您必須使用 Windows PowerShell 命令。 如需詳細資訊，請參閱適用于 Windows Server 2016 的  [遠端存取 Cmdlet](/powershell/module/remoteaccess) 及 Windows 10。

-   在 System Center 2012 R2 Virtual Machine Manager (VMM) 中，RAS 閘道名為 Windows Server Gateway。 VMM 軟體介面中有一組有限的邊界閘道協定 (BGP) 設定選項，包括 **本機 BGP Ip 位址** 和自發 **系統編號 (ASN)**、 **BGP 對等 ip 地址清單** 和 **asn 值**。 不過，您可以使用遠端存取 Windows PowerShell BGP 命令來設定 Windows Server 閘道的其他所有功能。 如需詳細資訊，請參閱  [Virtual Machine Manager (VMM) ](/system-center/vmm/overview) 和 Windows Server 2016 的 [遠端存取 Cmdlet](/system-center/vmm/overview) 和 Windows 10。

## <a name="related-topics"></a>相關主題
- [RAS 閘道高可用性](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)
- [Windows Server 中的 GRE 通道](gre-tunneling-windows-server.md)
- [RAS 閘道 GRE 通道輸送量及效能](RAS-Gateway-GRE-Perf.md)
