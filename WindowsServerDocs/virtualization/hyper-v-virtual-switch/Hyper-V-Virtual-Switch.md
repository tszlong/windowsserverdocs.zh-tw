---
title: Hyper-V 虛擬交換器
description: 本主題提供 Windows Server 2016 中的 HYPER-V 虛擬交換器概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 398440ac-5988-41ce-b91e-eab343a255d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17962b9bbb90a5ea1b6a4a303c64d72f74be37b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860919"
---
# <a name="hyper-v-virtual-switch"></a>Hyper-V 虛擬交換器

>適用於：Windows Server （半年通道），Windows Server 2016

本主題概述 HYPER-V 虛擬交換器，提供您將虛擬機器連線能力\(Vm\)到網路外部的 Hyper-v\-主機，包括您的組織內部網路和網際網路。 

您也可以連接到執行 Hyper-v 的伺服器上的虛擬網路\-當您部署軟體定義網路 V \(SDN\)。

> [!NOTE]  
> 本主題中，除了下列 HYPER-V 虛擬交換器文件使用。  
>   
> - [管理 HYPER-V 虛擬交換器](Manage-Hyper-V-Virtual-Switch.md) 
> - [遠端直接記憶體存取 (RDMA) 和交換器內嵌小組 (SET)](RDMA-and-Switch-Embedded-Teaming.md)
> - [在 Windows PowerShell 中的網路交換器小組 Cmdlet](https://technet.microsoft.com/library/jj553812.aspx)
> - [在 VMM 2016 中最新消息](https://docs.microsoft.com/system-center/vmm/whats-new#networking)
> - [設定 VMM 網路網狀架構](https://docs.microsoft.com/system-center/vmm/manage-networks)
> - [透過 VMM 2012 中建立網路](https://social.technet.microsoft.com/wiki/contents/articles/3140.create-networks-with-vmm-2012.aspx)  
> - [Hyper-V:設定 Vlan 和 VLAN 標記](https://social.technet.microsoft.com/wiki/contents/articles/1306.hyper-v-configure-vlans-and-vlan-tagging.aspx)  
> - [Hyper-V:如果需要透過協力廠商擴充功能，就應該啟用 WFP 虛擬交換器擴充功能](https://social.technet.microsoft.com/wiki/contents/articles/13071.hyper-v-the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions.aspx)
>
> 如需有關其他網路技術的詳細資訊，請參閱[Windows Server 2016 中的網路功能](https://docs.microsoft.com/windows-server/networking/networking)。
  
超\-V 虛擬交換器是軟體式 layer-2 乙太網路交換器，可在 Hyper-v\-當您安裝 Hyper-v 管理員\-V 伺服器角色。

HYPER-V 虛擬交換器包含以程式設計方式管理和可擴充的功能，將 Vm 連線至虛擬網路與實體網路。 此外，Hyper-V 虛擬交換器提供安全性、隔離以及服務層級的原則強化。  
  
> [!NOTE]  
> Hyper-V 虛擬交換器只支援乙太網路，不支援任何其他的有線區域網路 (LAN) 技術，例如 Infiniband 和光纖通道。  
  
HYPER-V 虛擬交換器包含租用戶隔離功能、 流量成形、 免於受到惡意的虛擬機器的保護，以及簡化疑難排解。 

使用網路裝置介面規格的內建支援\(NDIS\)篩選器驅動程式和 Windows 篩選平台\(WFP\)圖說文字驅動程式中，HYPER-V 虛擬交換器可讓獨立軟體廠商\(Isv\)建立可擴充的外掛程式，稱為虛擬交換器擴充功能，可提供增強的網路和安全性功能。 您新增至 Hyper-V 虛擬交換器的虛擬交換器擴充功能會列示於 Hyper-V 管理員的 [虛擬交換器管理員] 功能中。
  
在下圖中，VM 會有已連線到 HYPER-V 虛擬交換器透過交換器連接埠的虛擬 NIC。  
  
![虛擬交換器連線](../media/Hyper-V-Virtual-Switch/Vswitch_01.jpg)  
  
HYPER-V 虛擬交換器功能會將您提供更多的選項，以強制執行租用戶隔離、 成形及控制網路流量，並採取保護措施針對惡意的 Vm。

>[!NOTE]
> 在 Windows Server 2016 中，具有虛擬 NIC 的 VM 正確顯示的最大輸送量虛擬 nic。 若要檢視中的虛擬 NIC 速度**網路連線**，以滑鼠右鍵按一下所需的虛擬 NIC 圖示，然後按一下**狀態**。 虛擬 NIC**狀態**對話方塊隨即開啟。 在 **連接**，值**速度**符合安裝在伺服器中的實體 NIC 的速度。
  
## <a name="bkmk_apps"></a>使用 HYPER-V 虛擬交換器

以下是一些使用案例的 HYPER-V 虛擬交換器。

**顯示統計資料**:代管雲端廠商的開發人員實作管理套件，以顯示 Hyper-V 虛擬交換器的目前狀態。 這個管理套件可以使用 WMI 查詢全交換器目前的功能、組態設定以及個別連接埠網路統計資料。 然後顯示交換器的狀態，讓系統管理員快速檢視交換器的狀態。  
  
**資源追蹤**:代管公司銷售主機服務的價格是以成員資格的層級為依據。 各種成員資格層級包含不同的網路效能層級。 系統管理員以維持網路可用性平衡的方式分配資源，以符合 SLA。 系統管理員以程式設計方式追蹤資訊，例如目前的使用量的指派，頻寬和指派給虛擬機器佇列 (VMQ) 或 IOV 通道的虛擬機器 (VM) 數目。 相同的程式除了記錄指派給雙重項目追蹤或資源的每一 VM 資源之外，也會定期記錄使用中的資源。  
  
**管理交換器擴充功能的順序**:企業在 Hyper-V 主機上安裝擴充功能，以監控流量和報告入侵偵測。 在維護期間，可能會因為一些擴充功能的更新而使擴充功能的順序變更。 系統會在更新後執行簡單的指令碼程式，以重新排序擴充功能。  
  
**轉送擴充功能管理 VLAN 識別碼**:主要的交換器公司正在建置轉送擴充功能，可套用所有的網路原則。 它所管理的一個元素就是虛擬區域網路 (VLAN) 識別碼。 虛擬交換器會放棄 VLAN 的控制而交給轉送擴充功能。 交換器公司的安裝以程式設計方式呼叫 Windows Management Instrumentation (WMI) 應用程式設計介面 (API)，以開啟透明度，通知 HYPER-V 虛擬交換器傳遞和 VLAN 標記不採取任何動作。  
  
## <a name="bkmk_func"></a>HYPER-V 虛擬交換器的功能
 
Hyper-V 虛擬交換器包含下列主要功能：  
  
-   **ARP/ND 破壞 （詐騙） 保護**:防止惡意 VM 使用位址解析通訊協定(ARP) 詐騙從其他 VM 竊取 IP 位址。 提供對使用芳鄰探索 (ND) 詐騙針對 IPv6 啟動攻擊的防護。  
  
-   **DHCP 防護保護**:防止惡意 VM 假冒動態主機設定通訊協定 (DHCP) 伺服器進行攔截式攻擊。  
  
-   **連接埠 Acl**:提供以媒體存取控制 (MAC) 或網際網路通訊協定 (IP) 位址/範圍為基礎的流量篩選，可以讓您設定虛擬網路隔離。  
  
-   **VM 的主幹模式**:可讓系統管理員將特定 VM 設定為虛擬應用裝置，然後將流量從各個 VLAN 引導到該 VM。  
  
-   **網路流量監視**:可讓系統管理員檢閱周遊於網路交換器的流量。  
  
-   **隔離 （私人） VLAN**:可讓系統管理員隔離多重 VLAN 的流量，以更容易建立隔離的租用戶社群。  
  
以下是增強 Hyper-V 虛擬交換器可用性的功能清單：  
  
-   **頻寬限制及高載支援**:頻寬下限可以保證保留的頻寬量。 頻寬上限則是 VM 可以耗用的最大頻寬量。  
  
-   **明確壅塞通知 (ECN) 標示支援**:ECN 標記，也稱為資料中心 Tcp (DCTCP)，可讓實體交換器與作業系統調整流量的流動，交換器的緩衝區資源不會因為填滿而導致流量輸送量增加。  
  
-   **診斷**:診斷可讓您透過虛擬交換器輕易追蹤和監視事件及封包。
