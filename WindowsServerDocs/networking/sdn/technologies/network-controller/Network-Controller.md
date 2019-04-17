---
title: Network Controller
description: 本主題提供 Windows Server 2016 中 Network Controller 的概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: acb9abbd716e9930fb01431e7004abb72a7da10c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller"></a>Network Controller

>適用於：Windows Server（以每年次管道）、Windows Server 2016

新的 Windows Server 2016 中 Network Controller 提供管理、設定、監視，以及疑難排解 virtual 和實體網路基礎結構，在您的資料中心自動化打造、程式化的點。 

您可以使用網路控制器，請將網路基礎結構，而不是執行手動設定網路的裝置和服務的設定。

> [!NOTE]
> 本主題中，除了下列 Network Controller 的文件會提供。
> - [網路控制器可用性](network-controller-high-availability.md)
> - [安裝和部署 Network Controller 準備需求](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [部署 Network Controller 使用 Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [安裝使用伺服器管理員 Network Controller 伺服器角色](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [後 Post-Deployment 步驟 Network Controller](post-deploy-steps-nc.md)
> - [網路控制器 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>網路控制器概觀

Network Controller 高度可用和擴充伺服器角色，並且提供一個應用程式的程式設計介面，可讓與網路通訊的 Network Controller \(API\) 和第二個的 API，讓您與 Network Controller。

您可以部署 Network Controller 的非網域環境和網域。 在網域環境中，Network Controller 驗證使用者與網路的裝置使用 Kerberos;在非網域環境中，您必須部署驗證的憑證。

>[!IMPORTANT]
>不要部署實體主機上的 Network Controller 伺服器角色。 若要部署 Network Controller，您必須安裝網路控制站伺服器角色 HYPER-V 一樣上 \(VM\) HYPER-V 主機上安裝。 有三種不同的 Hyper\ HYPER-V 主機上 Vm 上安裝 Network Controller 之後，您必須讓 Hyper\ HYPER-V 主機的網路軟體定義 \(SDN\) 加到使用 Windows PowerShell 命令 Network Controller 的主機**新-NetworkControllerServer**。 如此一來，您會讓 SDN 軟體負載平衡器函式。 如需詳細資訊，請查看[新-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver)。

Network Controller 通訊網路的裝置、服務與元件使用 Southbound API。 Southbound api，Network Controller 可以探索網路的裝置、偵測服務設定，以及收集所有的網路所需的資訊。 此外，Southbound API 提供 Network Controller 路徑，以將資訊傳送至網路基礎結構，例如您所做的變更設定。

網路控制器 Northbound API 為您提供從網路控制器收集網路的資訊，並使用它來監視和設定網路的能力。

網路控制器 Northbound API 可讓您設定、監視、的疑難排解，以及使用 Windows PowerShell、代表狀態傳輸 \(REST\) API 或管理應用程式的圖形使用者介面，例如 System Center 一樣 Manager 中部署網路上的新裝置。

>[!NOTE]
>以其他介面係網路控制器 Northbound API。

您可以使用管理應用程式，例如 System Center 一樣 Manager \(SCVMM\) 和 System Center Operations Manager \(SCOM\)，來管理 datacenter 網路 Network Controller 的因為 Network Controller 可讓您設定、監控計畫，及的疑難排解網路基礎結構其控制。

使用 Windows PowerShell、REST API 或管理應用程式，您可以使用 Network Controller 管理下列實體和 virtual 網路基礎結構：

- HYPER-V Vm 和 virtual 切換

- Datacenter 防火牆

- 遠端存取服務 \(RAS\) Multitenant 閘道、Virtual 閘道和閘道集區

- 軟體負載平衡器

下圖系統管理員會直接與 Network Controller 管理工具互動。 Network Controller 提供資訊的網路基礎結構，包括 virtual 和實體基礎結構，管理工具，並可設定的變更依據使用工具時，系統管理員的動作。  

![網路控制器概觀](../../../media/Network-Controller/NetController_overview.png)  

如果您要部署 Network Controller 實驗室測試環境中，您可以在 HYPER-V 一樣執行 Network Controller 伺服器角色 \(VM\) HYPER-V 主機上安裝。

可用性高較大的資料中心，您可以使用的三個或更多 HYPER-V 主機上已安裝的三個 Vm 部署叢集。 如需詳細資訊，請查看[網路控制器可用性](network-controller-high-availability.md)。

## <a name="bkmk_features"></a>網路控制器功能

下列 Network Controller 功能可讓您如何設定及管理 virtual 和實體網路的裝置和服務。  
  
-   [防火牆管理](#bkmk_firewall)  
  
-   [軟體負載平衡器管理](#bkmk_slb)  
  
-   [管理 virtual 網路](#bkmk_virtual)  
  
-   [RAS 閘道管理](#bkmk_gateway)

>[!IMPORTANT]
>網路控制器備份與還原不是在 Windows Server 2016 中目前可用。
  
### <a name="bkmk_firewall"></a>防火牆管理

此 Network Controller 功能可讓您設定及管理允許日拒絕存取控制免針對您的工作負載 Vm 東日西和北日南網路流量在您的資料中心。 免之 vSwitch 連接埠工作負載 Vm 中，讓分散在您的工作負載 datacenter 中。 使用 Northbound API，您可以定義傳入和傳出工作負載 VM 流量免。 您也可以設定來登入的資料傳輸已允許或拒絕規則每個防火牆規則。  

如需詳細資訊，請查看[Datacenter 防火牆概觀](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。

### <a name="bkmk_slb"></a>軟體負載平衡器管理

此 Network Controller 功能可讓您讓多個主機相同的工作負載、可用性和延展性伺服器。  
  
如需詳細資訊，請查看[軟體負載平衡和 #40;SLB 與 #41;適用於 SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
### <a name="bkmk_virtual"></a>管理 virtual 網路

這個 Network Controller 的功能可讓您部署及設定 HYPER-V 網路模擬，包括 HYPER-V Virtual 切換和個人 Vm 上的 virtual 網路介面卡以及市集並散發 virtual 的網路原則。

Network Controller 支援網路模擬一般路由封裝 (NVGRE) 和 Virtual 最具擴充性的區域網路 (VXLAN)。

### <a name="bkmk_gateway"></a>RAS 閘道管理

此 Network Controller 功能可讓您部署、設定及管理虛擬電腦 (Vm) 提供您 tenants 閘道服務 RAS 閘道集區的成員。 Network Controller 可讓您將會自動部署 Vm RAS 閘道執行下列閘道功能：

> [!NOTE]
> 在 System Center 一樣管理員 RAS 閘道稱為 Windows 伺服器閘道。

- 新增從叢集移除閘道 Vm 並指定備份所需的層級。

- 網站-virtual 私人網路 (VPN) 閘道器連接遠端承租人網路和您使用 IPsec 的資料中心。

- 網站-VPN 閘道器連接遠端承租人網路和您使用一般路由封裝 (GRE) 的資料中心。

- 層級 3 轉接功能。

- 邊境閘道通訊協定 (BGP) 路由，可讓您管理您 tenants' VM 網路與他們遠端網站間網路流量的路由。

Network Controller 可以放不同閘道房客的不同連接。 您可以使用單一公用 IP 的所有閘道器連接或有不同公用 Ip 的子集的連接。 Network Controller 登所有閘道設定和狀態變更，可用於稽核和進行疑難排解。

適用於 BGP 的詳細資訊，請查看[邊境閘道通訊協定與 #40;BGP 和 #41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

適用於 RAS 閘道詳細資訊，請查看[適用於 SDN RAS 閘道](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。

## <a name="network-controller-deployment-options"></a>網路控制器部署選項

若要使用 System Center 一樣 Manager \(VMM\) 部署網路控制器，請查看[設定中 VMM fabric SDN Network Controller](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller)。

若要部署 Network Controller 使用指令碼，查看[部署軟體定義網路基礎結構使用指令碼](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)。

若要部署使用 Windows PowerShell 網路控制器，請查看[使用 Windows PowerShell 部署 Network Controller](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
