---
title: 網路控制卡
description: 本主題提供 Windows Server 2016 中的網路控制站的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ace628c6ae9802c0c65d360aedfac8c80ac5537
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875679"
---
# <a name="network-controller"></a>網路控制卡

>適用於：Windows Server （半年通道），Windows Server 2016

新增在 Windows Server 2016 中，網路控制站提供集中式、 可程式化的自動化點來管理、 設定、 監視和疑難排解您的資料中心內的虛擬和實體網路基礎結構。 

使用網路控制器時，您可以自動化網路基礎結構的設定，而不用執行網路裝置與服務的手動設定。

> [!NOTE]
> 本主題中，除了下列的網路控制卡文件使用。
> - [網路控制站的高可用性](network-controller-high-availability.md)
> - [安裝和部署網路控制站準備需求](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [部署使用 Windows PowerShell 的網路控制站](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [安裝網路控制卡伺服器角色，使用 伺服器管理員](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [網路控制站的部署後步驟](post-deploy-steps-nc.md)
> - [網路控制站 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>網路控制器概觀

網路控制器是高度可用且可擴充的伺服器角色，並提供一個應用程式開發介面\(API\) ，可讓網路控制站進行通訊的網路，並可讓您的第二個 API與網路控制站進行通訊。

您可以部署網域與非網域環境中的網路控制站。 在網域環境中，網路控制站會驗證使用者和網路裝置使用 Kerberos;在非網域環境中，您必須部署憑證來進行驗證。

>[!IMPORTANT]
>不會部署在實體主機上的網路控制卡伺服器角色。 若要部署網路控制站，您必須為 HYPER-V 虛擬機器上安裝網路控制卡伺服器角色\(VM\)安裝於 HYPER-V 主機。 在三個不同的 Hyper-v Vm 上安裝網路控制站之後\-Hyperv 主機，您必須啟用 Hyper-v\-的軟體定義網路的 HYPER-V 主機\(SDN\)加上要使用網路控制站的主機Windows PowerShell 命令**新增 NetworkControllerServer**。 如此一來，您將使 SDN 軟體負載平衡器，函式。 如需詳細資訊，請參閱 <<c0> [ 新增 NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

網路控制器會使用 Southbound API 和網路裝置、服務和元件通訊。 利用 Southbound API，網路控制器可以探索網路裝置、偵測服務組態，以及收集所有您所需的網路相關資訊。 此外，Southbound API 可提供路徑讓網路控制器將資訊傳送至網路基礎結構，例如您所做的組態變更。

網路控制器 Northbound API 會提供您從網路控制器收集網路資訊，並使用它來監視和設定網路的能力。

網路控制器 Northbound API 可讓您設定、 監視、 疑難排解和部署新的裝置，在網路上使用 Windows PowerShell，代表性狀態傳輸\(REST\) API 或管理應用程式圖形化使用者介面中，例如 System Center Virtual Machine Manager。

>[!NOTE]
>網路控制器 Northbound API 會實作為 REST 介面。

您可以使用 管理應用程式，例如 System Center Virtual Machine Manager 管理您的資料中心網路與網路控制卡\(SCVMM\)，和 System Center Operations Manager \(SCOM\)，因為網路控制器可讓您設定，監視、 發展以及疑難排解受其控制的網路基礎結構。

使用 Windows PowerShell、REST API 或管理應用程式時，您可以使用網路控制器來管理下列實體和虛擬網路基礎結構：

- HYPER-V VM 和虛擬交換器

- 資料中心防火牆

- 遠端存取服務\(RAS\)多租用戶閘道、 虛擬閘道，以及閘道集區

- 軟體負載平衡器

在下圖中，系統管理員會使用與網路控制器直接互動的管理工具。 網路控制站提供網路基礎結構，包括虛擬和實體基礎結構，管理工具的相關資訊並根據系統管理員的動作時使用此工具的組態變更。  

![網路控制器概觀](../../../media/Network-Controller/NetController_overview.png)  

如果您要在測試實驗室環境中部署網路控制站，您可以在 HYPER-V 虛擬機器上執行網路控制卡伺服器角色\(VM\)安裝於 HYPER-V 主機。

較大的資料中心內的高可用性，您可以使用三個或多個 HYPER-V 主機所安裝的三個 Vm 來部署叢集。 如需詳細資訊，請參閱 <<c0> [ 網路控制站的高可用性](network-controller-high-availability.md)。

## <a name="bkmk_features"></a>網路控制器功能

下列網路控制器功能可讓您設定與管理虛擬和實體網路裝置和服務。  
  
-   [防火牆管理](#bkmk_firewall)  
  
-   [軟體負載平衡器管理](#bkmk_slb)  
  
-   [虛擬網路管理](#bkmk_virtual)  
  
-   [RAS 閘道管理](#bkmk_gateway)

>[!IMPORTANT]
>網路控制卡備份和還原不是目前無法在 Windows Server 2016。
  
### <a name="bkmk_firewall"></a>防火牆管理

此網路控制器功能可讓您設定與管理允許/拒絕防火牆存取控制規則，以存取資料中心內東/西和北/南網路流量的工作負載 VM。 防火牆規則會套用在工作負載 VM 的 vSwitch 連接埠，因此它們會分散到資料中心的所有工作負載。 使用 Northbound API 時，您可以定義來自工作負載 VM 之傳入和傳出流量的防火牆規則。 您也可以設定每個防火牆規則，以記錄規則允許或拒絕的流量。  

如需詳細資訊，請參閱 <<c0> [ 資料中心防火牆概觀](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。

### <a name="bkmk_slb"></a>軟體負載平衡器管理

此網路控制器功能可讓您啟用多部伺服器，以裝載相同的工作負載，並提供高度可用性和延展性。  
  
如需詳細資訊，請參閱 <<c0> [ 軟體負載平衡&#40;SLB&#41;適用於 SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。</c0>  
  
### <a name="bkmk_virtual"></a>虛擬網路管理

此網路控制器功能可讓您部署與設定 HYPER-V 網路虛擬化 (包括個別 VM 上的 HYPER-V 虛擬交換器和虛擬網路介面卡)，並且儲存及散發虛擬網路原則。

網路控制器支援網路虛擬化 Generic Routing Encapsulation (NVGRE) 和虛擬可延伸區域網路 (VXLAN)。

### <a name="bkmk_gateway"></a>RAS 閘道管理

此網路控制器功能可讓您部署、 設定及管理屬於 RAS 閘道集區，提供閘道服務給您的租用戶的虛擬機器 (Vm)。 網路控制站可讓您自動部署具有下列閘道功能執行 RAS 閘道的 Vm:

> [!NOTE]
> 在 「 System Center Virtual Machine Manager 」 中，RAS 閘道是名為 Windows Server 閘道。

- 從叢集新增和移除閘道 VM 並指定所需的備份層級。

- 在遠端租用戶網路與資料中心之間使用 IPsec 的站對站虛擬私人網路 (VPN) 閘道連線。

- 在遠端租用戶網路與資料中心之間使用 Generic Routing Encapsulation (GRE) 的站對站 VPN 閘道連線。

- 層級 3 轉送功能。

- 邊界閘道通訊協定 (BGP) 路由，可讓您管理的租用戶 VM 網路和其遠端站台之間的網路流量路由。

網路控制站可以在個別閘道上放置不同的租用戶的連線。 您可以使用所有的閘道連線的單一公用 IP，或有不同的公用 Ip 子集的連線。 網路控制卡會記錄所有閘道組態和狀態變更時，它可以用於稽核和疑難排解之用。

如需有關 BGP 的詳細資訊，請參閱 <<c0> [ 邊界閘道協定&#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)。</c0>

如需有關 RAS 閘道的詳細資訊，請參閱[適用於 SDN 的 RAS 閘道](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。

## <a name="network-controller-deployment-options"></a>網路控制站部署選項

若要使用 System Center Virtual Machine Manager 部署網路控制卡\(VMM\)，請參閱[設定 VMM 光纖中的 SDN 網路控制站](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

若要部署網路控制站使用指令碼，請參閱[部署軟體定義網路基礎結構使用的指令碼](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要部署使用 Windows PowerShell 的網路控制站，請參閱[部署網路控制站使用 Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
