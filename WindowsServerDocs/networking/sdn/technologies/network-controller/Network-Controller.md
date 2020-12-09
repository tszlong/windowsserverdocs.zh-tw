---
title: 網路控制站
description: 本主題概要說明 Windows Server 2016 中的網路控制站。
manager: grcusanz
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 5f789904903838e838e5de0c8de78266055fbcd6
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866497"
---
# <a name="network-controller"></a>網路控制站

>適用於：Windows Server (半年度管道)、Windows Server 2016

網路控制站是 Windows Server 2016 中的新功能，提供集中、可程式化的自動化點，以管理、設定、監視及疑難排解資料中心內的虛擬和實體網路基礎結構。

使用網路控制器時，您可以自動化網路基礎結構的設定，而不用執行網路裝置與服務的手動設定。

> [!NOTE]
> 除了本主題之外，還提供下列網路控制站檔。
> - [網路控制站高可用性](network-controller-high-availability.md)
> - [部署網路控制站的安裝與準備需求](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)
> - [使用 Windows PowerShell 部署網路控制卡](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
> - [使用伺服器管理員安裝網路控制卡伺服器角色](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [網路控制站的部署後步驟](post-deploy-steps-nc.md)
> - [網路控制站 Cmdlet](/powershell/module/networkcontroller/)

## <a name="network-controller-overview"></a><a name="bkmk_overview"></a>網路控制站總覽

網路控制站是高度可用且可擴充的伺服器角色，並提供一個應用程式開發介面 API，可 \( \) 讓網路控制器與網路通訊，並提供可讓您與網路控制站通訊的第二個 api。

您可以在網域和非網域環境中部署網路控制站。 在網域環境中，網路控制站會使用 Kerberos 來驗證使用者和網路裝置;在非網域環境中，您必須部署憑證以進行驗證。

>[!IMPORTANT]
>請勿在實體主機上部署網路控制站伺服器角色。 若要部署網路控制站，您必須在 \( \) 安裝于 hyper-v 主機上的 hyper-v 虛擬機器 VM 上安裝網路控制站伺服器角色。 在三部不同的 Hyper-v 主機上的 Vm 上安裝網路控制站之後 \- ，您必須 \- \( \) 使用 Windows PowerShell 命令 **NetworkControllerServer**，將主機新增至網路控制站，以啟用軟體定義網路 SDN 的 hyper-v 主機。 如此一來，您就可以讓 SDN 軟體 Load Balancer 運作。 如需詳細資訊，請參閱 [NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

網路控制器會使用 Southbound API 和網路裝置、服務和元件通訊。 利用 Southbound API，網路控制器可以探索網路裝置、偵測服務組態，以及收集所有您所需的網路相關資訊。 此外，Southbound API 可提供路徑讓網路控制器將資訊傳送至網路基礎結構，例如您所做的組態變更。

網路控制器 Northbound API 會提供您從網路控制器收集網路資訊，並使用它來監視和設定網路的能力。

Network Controller Northbound API 可讓您使用 Windows PowerShell、具象狀態傳輸 \( REST \) API，或使用圖形化使用者介面（例如 System Center Virtual Machine Manager）的管理應用程式，在網路上設定、監視、疑難排解和部署新的裝置。

>[!NOTE]
>網路控制器 Northbound API 會實作為 REST 介面。

您可以使用管理應用程式（例如 System Center Virtual Machine Manager SCVMM 和 System Center Operations Manager SCOM）來管理您的資料中心網路， \( \) \( \) 因為網路控制站可讓您對其控制之下的網路基礎結構進行設定、監視、程式及疑難排解。

使用 Windows PowerShell、REST API 或管理應用程式時，您可以使用網路控制器來管理下列實體和虛擬網路基礎結構：

- HYPER-V VM 和虛擬交換器

- 資料中心防火牆

- 遠端存取服務 RAS 多租使用者 \( \) 閘道、虛擬閘道和閘道集區

- 軟體負載平衡器

在下圖中，系統管理員會使用與網路控制器直接互動的管理工具。 網路控制站會將網路基礎結構的相關資訊（包括虛擬和實體基礎結構）提供給管理工具，並在使用此工具時，根據系統管理員的動作進行設定變更。

![網路控制器概觀](../../../media/Network-Controller/NetController_overview.png)

如果您要在測試實驗室環境中部署網路控制站，您可以在安裝在 hyper-v 主機上的 Hyper-v 虛擬機器 VM 上執行網路控制站伺服器角色 \( \) 。

若要在較大的資料中心內提供高可用性，您可以使用安裝在三部或多部 Hyper-v 主機上的三個 Vm 來部署叢集。 如需詳細資訊，請參閱 [網路控制站高可用性](network-controller-high-availability.md)。

## <a name="network-controller-features"></a><a name="bkmk_features"></a>網路控制器功能

下列網路控制器功能可讓您設定與管理虛擬和實體網路裝置和服務。

-   [防火牆管理](#bkmk_firewall)

-   [軟體負載平衡器管理](#bkmk_slb)

-   [虛擬網路管理](#bkmk_virtual)

-   [RAS 閘道管理](#bkmk_gateway)

>[!IMPORTANT]
>目前無法在 Windows Server 2016 中使用網路控制卡備份和還原。

### <a name="firewall-management"></a><a name="bkmk_firewall"></a>防火牆管理

此網路控制器功能可讓您設定與管理允許/拒絕防火牆存取控制規則，以存取資料中心內東/西和北/南網路流量的工作負載 VM。 防火牆規則會套用在工作負載 VM 的 vSwitch 連接埠，因此它們會分散到資料中心的所有工作負載。 使用 Northbound API 時，您可以定義來自工作負載 VM 之傳入和傳出流量的防火牆規則。 您也可以設定每個防火牆規則，以記錄規則允許或拒絕的流量。

如需詳細資訊，請參閱 [資料中心防火牆總覽](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。

### <a name="software-load-balancer-management"></a><a name="bkmk_slb"></a>軟體負載平衡器管理

此網路控制器功能可讓您啟用多部伺服器，以裝載相同的工作負載，並提供高度可用性和延展性。

如需詳細資訊，請參閱 [SDN&#41; &#40;SLB 的軟體負載平衡](../network-function-virtualization/software-load-balancing-for-sdn.md)。

### <a name="virtual-network-management"></a><a name="bkmk_virtual"></a>虛擬網路管理

此網路控制器功能可讓您部署與設定 HYPER-V 網路虛擬化 (包括個別 VM 上的 HYPER-V 虛擬交換器和虛擬網路介面卡)，並且儲存及散發虛擬網路原則。

網路控制器支援網路虛擬化 Generic Routing Encapsulation (NVGRE) 和虛擬可延伸區域網路 (VXLAN)。

### <a name="ras-gateway-management"></a><a name="bkmk_gateway"></a>RAS 閘道管理

此網路控制器功能可讓您部署、設定及管理 (Vm) 的虛擬機器，這些 Vm 是 RAS 閘道集區的成員，提供閘道服務給您的租使用者。 網路控制站可讓您使用下列閘道功能，自動部署執行 RAS 閘道的 Vm：

> [!NOTE]
> 在 System Center Virtual Machine Manager 中，RAS 閘道名為 Windows Server Gateway。

- 從叢集新增和移除閘道 VM 並指定所需的備份層級。

- 在遠端租用戶網路與資料中心之間使用 IPsec 的站對站虛擬私人網路 (VPN) 閘道連線。

- 在遠端租用戶網路與資料中心之間使用 Generic Routing Encapsulation (GRE) 的站對站 VPN 閘道連線。

- 層級 3 轉送功能。

- 邊界閘道協定 (BGP) 路由，可讓您管理租使用者 VM 網路與其遠端網站之間的網路流量路由。

網路控制站可以在不同的閘道上放置不同的租使用者連接。 您可以針對所有閘道連線使用單一公用 IP，或針對某個連接子集使用不同的公用 IP。 網路控制站會記錄所有閘道設定和狀態變更，這些變更可用於進行審核和疑難排解之用。

如需 BGP 的詳細資訊，請參閱 [邊界閘道協定 &#40;bgp&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)。

如需 RAS 閘道的詳細資訊，請參閱 [SDN 的 Ras 閘道](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。

## <a name="network-controller-deployment-options"></a>網路控制站部署選項

若要使用 System Center Virtual Machine Manager VMM 來部署網路控制站 \( \) ，請參閱在 [vmm 網狀架構中設定 SDN 網路控制](/system-center/vmm/sdn-controller)站。

若要使用腳本部署網路控制站，請參閱 [使用腳本部署軟體定義的網路基礎結構](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要使用 Windows PowerShell 部署網路控制站，請參閱[使用 Windows PowerShell 部署網路控制](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)站
