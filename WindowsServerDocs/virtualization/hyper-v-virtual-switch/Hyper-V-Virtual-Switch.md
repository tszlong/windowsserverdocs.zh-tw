---
title: Hyper-V 虛擬交換器
description: 本主題概要說明 Windows Server 2016 中的 Hyper-v 虛擬交換器。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 398440ac-5988-41ce-b91e-eab343a255d3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fb2ebf485b5004e457558fc16d8535662c0c5ff2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307985"
---
# <a name="hyper-v-virtual-switch"></a>Hyper-V 虛擬交換器

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題提供 Hyper-v 虛擬交換器的總覽，此功能可讓您將虛擬機器 \(Vm\) 連線到超\-V 主機外部的網路，包括組織的內部網路和網際網路。 

當您部署軟體定義的網路功能 \(SDN\)時，您也可以連接到執行 Hyper-v\-V 之伺服器上的虛擬網路。

> [!NOTE]  
> 除了本主題之外，也提供下列 Hyper-v 虛擬交換器檔。  
>   
> - [管理 Hyper-V 虛擬交換器](Manage-Hyper-V-Virtual-Switch.md) 
> - [遠端直接記憶體存取 (RDMA) 和交換器內嵌小組 (SET)](RDMA-and-Switch-Embedded-Teaming.md)
> - [Windows PowerShell 中的網路交換器小組 Cmdlet](https://technet.microsoft.com/library/jj553812.aspx)
> - [VMM 2016 中的新功能](https://docs.microsoft.com/system-center/vmm/whats-new#networking)
> - [設定 VMM 網路網狀架構](https://docs.microsoft.com/system-center/vmm/manage-networks)
> - [使用 VMM 2012 建立網路](https://social.technet.microsoft.com/wiki/contents/articles/3140.create-networks-with-vmm-2012.aspx)  
> - [Hyper-v：設定 Vlan 和 VLAN 標記](https://social.technet.microsoft.com/wiki/contents/articles/1306.hyper-v-configure-vlans-and-vlan-tagging.aspx)  
> - [Hyper-v：若協力廠商擴充功能需要 WFP 虛擬交換器擴充功能，則應加以啟用](https://social.technet.microsoft.com/wiki/contents/articles/13071.hyper-v-the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions.aspx)
>
> 如需其他網路技術的詳細資訊，請參閱[Windows Server 2016 中的網路](https://docs.microsoft.com/windows-server/networking/networking)功能。
  
\-hyper-v 虛擬交換器是一種以軟體為基礎的第2層 Ethernet 網路交換器，當您安裝超\-V 伺服器角色時，它會在 [超\-V 管理員] 中提供。

Hyper-v 虛擬交換器包含以程式設計方式管理和可擴充的功能，可將 Vm 連線至虛擬網路和實體網路。 此外，Hyper-V 虛擬交換器提供安全性、隔離以及服務層級的原則強化。  
  
> [!NOTE]  
> Hyper-V 虛擬交換器只支援乙太網路，不支援任何其他的有線區域網路 (LAN) 技術，例如 Infiniband 和光纖通道。  
  
Hyper-v 虛擬交換器包括租使用者隔離功能、流量成形、防範惡意虛擬機器的保護，以及簡化的疑難排解。 

使用網路裝置介面規格的內建支援 \(NDIS\) 篩選驅動程式和 Windows 篩選平台 \(WFP\) 注標驅動程式，Hyper-v 虛擬交換器可讓獨立軟體廠商 \(Isv\) 建立可擴充的外掛程式（稱為虛擬交換器擴充功能），其可提供增強的網路和安全性功能。 您新增至 Hyper-V 虛擬交換器的虛擬交換器擴充功能會列示於 Hyper-V 管理員的 [虛擬交換器管理員] 功能中。
  
在下圖中，VM 具有透過交換器埠連線至 Hyper-v 虛擬交換器的虛擬 NIC。  
  
![虛擬交換器連接](../media/Hyper-V-Virtual-Switch/Vswitch_01.jpg)  
  
Hyper-v 虛擬交換器功能提供您更多的選項來強制執行租使用者隔離、塑造及控制網路流量，並針對惡意 Vm 採用保護措施。

>[!NOTE]
> 在 Windows Server 2016 中，具有虛擬 NIC 的 VM 會正確顯示虛擬 NIC 的最大輸送量。 若要在**網路**連線中查看虛擬 nic 速度，請在所需的虛擬 nic 圖示上按一下滑鼠右鍵，然後按一下 [**狀態**]。 [虛擬 NIC**狀態**] 對話方塊隨即開啟。 在 [連線]**中，[** **速度**] 的值會符合伺服器中安裝之實體 NIC 的速度。
  
## <a name="uses-for-hyper-v-virtual-switch"></a><a name="bkmk_apps"></a>使用 Hyper-v 虛擬交換器

以下是 Hyper-v 虛擬交換器的一些使用案例。

**顯示統計資料**：託管雲端廠商的開發人員會執行管理套件，以顯示 hyper-v 虛擬交換器的目前狀態。 這個管理套件可以使用 WMI 查詢全交換器目前的功能、組態設定以及個別連接埠網路統計資料。 然後顯示交換器的狀態，讓系統管理員快速檢視交換器的狀態。  
  
**資源追蹤**：主控公司會根據成員資格的層級來銷售託管服務。 各種成員資格層級包含不同的網路效能層級。 系統管理員以維持網路可用性平衡的方式分配資源，以符合 SLA。 系統管理員會以程式設計的方式追蹤資訊，例如指派的頻寬目前使用量，以及虛擬機器（VM）指派的虛擬機器佇列（VMQ）或的 [SR-IOV] 通道數目。 相同的程式除了記錄指派給雙重項目追蹤或資源的每一 VM 資源之外，也會定期記錄使用中的資源。  
  
**管理交換器擴充功能的順序**：企業在 hyper-v 主機上安裝延伸模組，以監視流量和報告入侵偵測。 在維護期間，可能會因為一些擴充功能的更新而使擴充功能的順序變更。 系統會在更新後執行簡單的指令碼程式，以重新排序擴充功能。  
  
**轉送擴充功能管理 VLAN 識別碼**：主要交換器公司正在建立轉送延伸模組，以套用所有的網路原則。 它所管理的一個元素就是虛擬區域網路 (VLAN) 識別碼。 虛擬交換器會放棄 VLAN 的控制而交給轉送擴充功能。 交換器公司的安裝以程式設計方式呼叫 Windows Management Instrumentation （WMI）應用程式開發介面（API），以開啟透明度，告知 Hyper-v 虛擬交換器通過 VLAN 標記而不採取任何動作。  
  
## <a name="hyper-v-virtual-switch-functionality"></a><a name="bkmk_func"></a>Hyper-v 虛擬交換器功能
 
Hyper-V 虛擬交換器包含下列主要功能：  
  
-   **ARP/ND 破壞（詐騙）保護**：使用位址解析協定（ARP）詐騙來竊取其他 VM 的 IP 位址，為惡意 VM 提供保護。 提供對使用芳鄰探索 (ND) 詐騙針對 IPv6 啟動攻擊的防護。  
  
-   **DHCP 防護保護**：保護惡意 VM，以動態主機設定通訊協定（DHCP）伺服器代表攔截式攻擊。  
  
-   **埠 acl**：提供以媒體存取控制（MAC）或網際網路通訊協定（IP）位址/範圍為基礎的流量篩選，可讓您設定虛擬網路隔離。  
  
-   **VM 的主幹模式**：可讓系統管理員將特定 VM 設定為虛擬應用裝置，然後將來自各種 vlan 的流量導向該 vm。  
  
-   **網路流量監視**：可讓系統管理員審查穿越網路交換器的流量。  
  
-   **隔離（私用） VLAN**：可讓系統管理員將多個 vlan 上的流量隔離，以更輕鬆地建立隔離的租使用者團體。  
  
以下是增強 Hyper-V 虛擬交換器可用性的功能清單：  
  
-   **頻寬限制及**高載支援：頻寬最小可保證保留的頻寬量。 頻寬上限則是 VM 可以耗用的最大頻寬量。  
  
-   **明確擁塞通知（ECN）標示支援**： ECN 標記（也稱為資料中心 TCP （DCTCP））可讓實體交換器與作業系統調整流量，使得交換器的緩衝區資源不會被淹沒，這會導致流量輸送量增加。  
  
-   **診斷**：診斷可讓您透過虛擬交換器輕鬆追蹤和監視事件和封包。
