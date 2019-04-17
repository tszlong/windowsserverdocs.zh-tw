---
title: SDN 技術
description: 在本區段中主題提供概觀和隨附於 Windows Server 2016 的軟體定義網路技術的相關技術的資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f842ac0d1a09106c1898374cf8dd1c7823d7dae
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="sdn-technologies"></a>SDN 技術

>適用於：Windows Server（以每年次管道）、Windows Server 2016

在本區段中主題提供概觀和隨附於 Windows Server 2016 的軟體定義網路技術的相關技術的資訊。  
  
> [!NOTE]  
> 其他軟體所定義網路文件，您可以使用下列的媒體櫃區段。  
>   
> - [SDN 計劃](../plan/Plan-Software-Defined-Networking.md)
> - [部署 SDN](../deploy/Deploy-Software-Defined-Networking.md)
> - [管理 SDN](../manage/manage-sdn.md)
> - [安全性 SDN](../security/sdn-security-top.md)
> - [疑難排解 SDN](../troubleshoot/Troubleshoot-Software-Defined-Networking.md)

有許多的技術，建立的 Microsoft 軟體定義網路 (SDN) 方案，包括：  
  
-   **[邊境閘道通訊協定與 #40;BGP 和 #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)**  
  
    設定 Windows Server 2016 遠端存取服務 (RAS) 閘道，邊境閘道通訊協定 (BGP) 提供您管理您 tenants' VM 網路與他們遠端網站間網路流量的路由的能力。 BGP 減少需要手動路由路由器設定，因為它是動態路由通訊協定，並自動學習所使用的網站 VPN 連接連接之間的路徑。  
  
-   **[Datacenter 防火牆概觀](../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)**  
  
    Datacenter 防火牆是與 Windows Server 2016 包含新的服務。 它是網路層級 5-有序元組通訊協定，來源和目的地的連接埠號碼（[來源和目的地的 IP 位址）、狀態、multitenant 防火牆。 當部署，即服務提供者服務提供承租人系統管理員可以安裝，並設定防火牆原則，以協助保護其 virtual 垃圾流量來自網際網路的網路及內部網路。  
  
  
-   **[HYPER-V 網路模擬](../../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)**  
  
    HYPER-V 網路模擬 (HNV) 可模擬的客戶網路共用實體網路基礎結構上方。  
  
- **[內部 DNS 服務與 #40; Idn 和 #41;適用於 SDN](../../sdn/technologies/Idns-for-Sdn.md)**

    裝載的虛擬機器 \(VMs\) 和應用程式需要 DNS 通訊在自己的網路和網際網路上的外部資源。 與 Idn，您可以使用 DNS 名稱解析服務提供 tenants 其名稱隔離的本機空間，以及網際網路資源。

-   **[Network Controller](../../sdn/technologies/network-controller/Network-Controller.md)**  
  
    網路控制器提供的集中、 程式化點的管理、 設定、 監視，以及疑難排解 virtual 和實體網路基礎結構，在您的資料中心自動化。  
  
-   **[網路功能模擬](../../sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)**  
  
    藉由硬體裝置 （例如負載平衡器、 防火牆、 路由器、 參數，等等） 執行網路功能的越來越正在擬化檔案為 virtual 裝置。  
  
    網路、 參數，閘道、 Nat、 負載平衡器、 和防火牆，Microsoft 已擬化檔案。  

-   **[適用於 SDN RAS 閘道](../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)**
  
    RAS 閘道為軟體，multitenant，邊境閘道通訊協定 (BGP) 可路由器專為雲端服務提供者 (Csp) 和主機多個承租人 virtual 網路使用 HYPER-V 網路模擬針對企業設計的 Windows Server 2016 中。  
      
- **[遠端直接記憶體存取和 #40;RDMA 與 #41;切換 Embedded 小組與 #40; 以及設定與 #41;](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)**  
  
    您可以使用的聚合型的 NIC 結合 RDMA 和乙太網路流量使用單一網路介面卡。 聚合型的而可讓您的單一網路介面卡用於管理，遠端直接記憶體存取 RDMA 式存放裝置及承租人傳輸。 這樣可以降低相關聯的資料中心的每個伺服器大寫費用，因為您需要管理不同類型的資料傳輸每個伺服器較少的網路介面卡。  
  
    設定為 HYPER-V Virtual 切換中整合的小組 NIC 方案。 設定可讓成單一設定團隊，這可以改善可用性和提供容錯移轉的最多按的實體 NIC 小組。 您可以在 Windows Server 2016 建立會限制使用 RDMA 伺服器訊息區 (SMB) 的設定團隊。
  

-   **[軟體負載平衡和 #40;SLB 與 #41;適用於 SDN](../../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)**  

    雲端服務提供者 (Csp) 與要部署的軟體定義網路 (SDN) 在 Windows Server 2016 中的企業可以使用軟體負載平衡 (SLB) 平均散發承租人和承租人客戶網路流量分配 virtual 網路資源。 Windows Server SLB 可讓伺服器多個主機相同的工作負載，可用性和延展性。
  
-   **[System Center](../../sdn/Sc-Tech-for-Sdn.md) **您可以使用 System Center 2016 一樣 Manager (VMM) 和 Operations Manager 部署與管理 SDN 基礎結構，包括網路控制器、 軟體負載平衡器、 和閘道。 您也可以使用 VMM 集中定義控制 virtual 的網路原則並連結到您的應用程式或工作負載的原則。
  
- **[Windows 容器](../technologies/Containers/Container-networking-overview.md)**
    
    Windows Server 容器是用來與其他服務的容器主機上執行分開應用程式或服務的輕量型作業系統模擬方法。 若要於此，每個容器會有自己的作業系統，程序，檔案系統、登錄和 IP 位址的檢視。 與 Windows Server 2016，您現在可以連接 Windows Server 容器 virtual 網路。 Windows 容器功能類似虛擬電腦中的網路。 每個容器有 virtual 網路介面卡是連接到 virtual 切換，輸入 / 輸出流量轉送所。 若要執行的容器主機上之間隔離，網路區間是每個 Windows Server 和建立 HYPER-V 容器容器的網路介面卡已安裝的。 Windows Server 容器使用主機但 vNIC 附加至 virtual 切換。 HYPER-V 容器使用（不的公用程式 vm 公開）合成 VM NIC 附加至 virtual 切換。

