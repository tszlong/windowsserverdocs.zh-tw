---
title: Windows Server 軟體定義資料中心
Description: Windows Server SDDC 概觀
ms.topic: get-started article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/04/2019
ms.localizationpriority: medium
ms.openlocfilehash: 632e08a6e560b9b9e4ab7010794eaf36d1873cac
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990612"
---
# <a name="windows-server-software-defined-datacenter"></a>Windows Server 軟體定義資料中心

>適用於：Windows Server 2019、Windows Server 2016

![標題影像](media/sddc/heading.png)

## <a name="what-is-windows-server-software-defined-datacenter"></a>什麼是 Windows Server 軟體定義資料中心？

軟體定義資料中心 (SDDC) 為常見業界術語，一般是指所有基礎結構皆已虛擬化的資料中心。 虛擬化是關鍵，簡單意思就是資料中心的硬體與軟體比例已擴大超出傳統的一比一。 作業系統和應用程式可以透過軟體 Hypervisor 模擬硬體從實體硬體中撤離，並倍增構成處理器、記憶體、I/O 及網路的彈性資源集區。

Microsoft 的 SDDC 實作包含本文中提出的 Windows Server 技術。 這是從 Hyper-V Hypervisor 開始實作，提供建置網路及儲存體的虛擬化平台。 針對虛擬化基礎結構獨特挑戰所開發的安全性技術減輕內部與外部威脅的影響。 利用內建於 Windows Server 的 PowerShell，加上 [System Center](/system-center/) 和/或 [Operations Management Suite](/azure/operations-management-suite/operations-management-suite-overview)，您就可以將佈建、部署、設定和管理程式化和自動化。

內建於 Windows Server 和 System Center 的技術是 Windows Server SDDC 體驗的主要建置組塊。 但即使是虛擬化平台，底層仍需要正確的硬體來支援。 參與 **Windows Server 軟體定義 (WSSD) 解決方案**和 **Azure Stack HCI 解決方案**計畫的 Microsoft 合作夥伴，可協助您的企業取得正確的硬體，並提前做好啟動和執行的準備。

![影片圖示](media/sddc/video.png)**[觀看影片進一步了解 Microsoft 的 SDDC](https://mva.microsoft.com/training-courses/whats-new-in-windows-server-2016-16457?l=YcsJR6sXC_1006218965)**

![海報圖示](media/sddc/poster-ico.png)**[下載此頁面的海報大小 .pdf 檔案](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/WindowsServerDocs/media/sddc/sddc_poster_0801417_ANSI-E.pdf)**

## <a name="azure-stack-hci-solutions"></a>Azure Stack HCI 解決方案

在正確的硬體基礎結構上建置 Windows Server 軟體定義資料中心，是邁向成功的首要步驟。 正因如此，我們才會與 15 個合作夥伴共同建立經 Microsoft 驗證的 SDDC 設計和最佳做法，以供部署使用。

Microsoft 合作夥伴提供了一系列的解決方案，可透過 Azure Stack HCI 計畫與 Windows Server 2019 搭配使用，或透過 Windows Server 軟體定義 (WSSD) 計畫與 Window Server 2016 搭配使用，以提供高效能、超融合式儲存體和網路基礎結構。 超融合式解決方案將業界標準伺服器及元件上的計算、儲存和網路功能整合在一起，以改善資料中心智慧與控制。

![學習圖示](media/sddc/learn.png)**[深入了解 Azure Stack HCI 解決方案](https://azure.microsoft.com/overview/azure-stack/hci)**

![學習圖示](media/sddc/learn.png)**[深入了解 WSSD 解決方案](https://www.microsoft.com/cloud-platform/software-defined-datacenter)**

## <a name="windows-server-virtualized-technologies"></a>Windows Server 虛擬化技術 ##

本主題的其餘部分會列出 Windows Server SDDC 技術，提供每項技術相關文件的連結。 這些技術如下表所列：

![可用的 Windows Server SDDC 技術](media/sddc/table.png)

![虛擬化任何標題列](media/sddc/virtualize.png)

### <a name="windows-server-hyper-converged"></a>Windows Server 超融合

Windows Server 虛擬化技術包含 Hyper-V、Hyper-V 虛擬交換器以及受防護網狀架構與受防護的虛擬機器 (VM) 的更新，可改善安全性、延展性及可靠性。 容錯移轉叢集、網路功能和儲存體的更新讓您更容易在搭配 Hyper-V 時部署和管理這些技術。

![僅供間距用途的影像](media/sddc/spacer1.png)![Windows Server，超融合式基礎結構圖表](media/sddc/hyper-converged.png)

![學習圖示](media/sddc/learn.png)**[深入了解 Windows Server 超融合](./get-started/whats-new-in-windows-server-2016.md#compute)**

### <a name="hyper-v-hypervisor"></a>Hyper-V Hypervisor

Hyper-V 是適用於 Windows、以 Hypervisor 為基礎的虛擬化技術。 Hypervisor 是虛擬化的核心。 這是處理器特定的虛擬化平台，可讓多個獨立的作業系統共用單一硬體平台。

![Hyper-V Hypervisor 圖表](media/sddc/spacer1.png)![Hyper](media/sddc/hypervisor.png)

![學習圖示](media/sddc/learn.png)**[深入了解 Hyper-V Hypervisor](https://www.microsoft.com/cloud-platform/server-virtualization)**

### <a name="guest-clustering-with-shared-vhdx"></a>含共用 VHDX 的客體叢集

![用於間距用途的線條影像](media/sddc/virtualize-line.png)

既靈活又安全，且未繫結於底層儲存體拓撲，共用 VHDX 不再需要向客體 OS 展示實體底層儲存體。 新的共用 VHDX 支援線上調整大小功能。

![僅供間距用途的影像](media/sddc/spacer1.png)![來賓叢集和共用的 VHDX 圖表](media/sddc/cluster.png)

- 共用 VHDX 可以存放在區塊儲存體或 SMB 檔案型儲存體上的叢集共用磁碟區 (CSV)。
- 受保護：共用 VHDX 支援 Hyper-V 複本和主機層級備份。

![學習圖示](media/sddc/learn.png)**[深入了解客體叢集與共用 VHDX](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn281956(v=ws.11))**

### <a name="hyper-v-replica"></a>Hyper-V 複本

![用於間距用途的線條影像](media/sddc/virtualize-line.png)

使用憑證以軟體為基礎的跨網路整合式 VM 複寫。 未繫結至任一網站上的伺服器、網路或儲存硬體。

![僅供間距用途的影像](media/sddc/spacer1.png)![Hyper-V 複本圖表](media/sddc/replica.png)

不需要其他虛擬機器複寫技術，並降低成本。
- 自動處理即時移轉。
- 簡單的設定及管理：可透過 Hyper-V 管理員、PowerShell 或 Azure Site Recovery 進行。

![學習圖示](media/sddc/learn.png)**[深入了解 Hyper-V 複本](./virtualization/hyper-v/manage/set-up-hyper-v-replica.md)**

![連接所有內容橫幅](media/sddc/networking.png)

### <a name="network-controller"></a>網路控制站

![用於間距用途的線條影像](media/sddc/networking-line.png)

集中式、可程式化的自動化點，可以管理、設定、監視和疑難排解資料中心的虛擬及實體網路基礎結構。

![僅供間距用途的影像](media/sddc/spacer1.png)![網路控制卡圖表](media/sddc/netcontroller.png)

系統管理員會使用與網路控制器直接互動的管理工具。 網路控制卡會將網路基礎結構 (包括虛擬及實體基礎結構) 的相關資訊提供給管理工具。

![學習圖示](media/sddc/learn.png)**[深入了解網路控制卡](./networking/sdn/technologies/network-controller/network-controller.md)**

### <a name="datacenter-firewall"></a>資料中心防火牆

![用於間距用途的線條影像](media/sddc/networking-line.png)

部署並提供作為服務時，租用戶系統管理員可安裝和設定防火牆原則來協助保護虛擬網路，不受網際網路及內部網路流量的干擾。

![僅供間距用途的影像](media/sddc/spacer1.png)![資料中心防火牆圖表](media/sddc/firewall.png)

服務提供者管理員或租用戶管理員可透過網路控制卡來管理資料中心防火牆原則。

![學習圖示](media/sddc/learn.png)**[深入了解資料中心防火牆](./networking/sdn/technologies/network-function-virtualization/datacenter-firewall-overview.md)**

### <a name="switch-embedded-teaming"></a>交換器內嵌小組

![用於間距用途的線條影像](media/sddc/networking-line.png)

SET 是替代的 NIC 小組解決方案，您可將其用於包含 Hyper-V 及[軟體定義的網路功能 (SDN)](./networking/sdn/software-defined-networking.md) 堆疊的環境中。

![僅供間距用途的影像](media/sddc/spacer1.png)![交換器內嵌小組圖表](media/sddc/teaming.png)

![學習圖示](media/sddc/learn.png)**[深入了解交換器內嵌小組](./networking/sdn/technologies/set-for-sdn.md)**

### <a name="software-load-balancing"></a>軟體負載平衡

![用於間距用途的線條影像](media/sddc/networking-line.png)

SLB 可讓多部伺服器裝載相同的工作負載，並提供高度可用性及延展性。 在您用於其他 VM 工作負載的相同 Hyper-V 伺服器上使用 SLB VM，以向外延展負載平衡功能。 SLB 支援快速建立和刪除雲端服務提供者作業的負載平衡端點。 SLB 針對每個叢集可支援數十 GB，且提供簡易佈建模型，並可輕鬆向外和向內延展。

![僅供間距用途的影像](media/sddc/spacer1.png)![軟體負載平衡圖表](media/sddc/balancer.png)

![學習圖示](media/sddc/learn.png)**[深入了解軟體負載平衡](./networking/sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)**

![儲存體圖表](media/sddc/storage.png)

### <a name="storage-spaces-direct"></a>儲存空間 Direct

![用於間距用途的線條儲存體影像](media/sddc/storage-line.png)

儲存空間直接存取將業界標準伺服器與本機連結磁碟機搭配使用，只需傳統 SAN 或 NAS 陣列的一小部分成本，就能提供高可用性、高延展性的軟體定義儲存體。 其架構大幅簡化了採購與部署作業。

![僅供間距用途的影像](media/sddc/spacer1.png)![儲存空間直接存取圖表](media/sddc/ssd.png)

儲存空間直接存取導入了新的軟體儲存匯流排，並利用目前在 Windows Server 中熟知的許多功能，例如容錯移轉叢集、叢集共用磁碟區 (CSV)、伺服器訊息區 (SMB) 3，以及儲存空間。

![學習圖示](media/sddc/learn.png)**[深入了解儲存空間直接存取](storage/storage-spaces/storage-spaces-direct-overview.md)**
### <a name="storage-quality-of-service"></a>存放裝置服務品質 ###

![間距用途的線條](media/sddc/storage-line.png)

使用 Hyper-V 與向外延展檔案伺服器角色來集中監視和管理虛擬機器的儲存體效能，並改善多個虛擬機器之間的儲存體資源公平性。

![僅供間距用途的影像](media/sddc/spacer1.png)![存放裝置服務品質圖表](media/sddc/qos.png)

儲存體 QoS 內建於向外延展檔案伺服器與 Hyper-V 使用 SMB3 通訊協定所提供的 Microsoft 軟體定義儲存體解決方案中。 新的原則管理員提供中央儲存體效能監視。

![學習圖示](media/sddc/learn.png)**[深入了儲存體 QoS](./storage/storage-qos/storage-qos-overview.md)**

### <a name="storage-replica"></a>儲存體複本

![用於間距用途的線條儲存體影像](media/sddc/storage-line.png)

災害復原和準備功能可更有效率地利用多個資料中心，透過同步保護位於不同機架、樓層、建物、校區及城市及國家/地區之資料的功能，將零資料遺失化為可能。

![僅供間距用途的影像](media/sddc/spacer1.png)
![儲存體複本圖表](media/sddc/storage-replica.png)

同步複寫

1. 應用程式寫入資料
2. 記錄檔資料已寫入，且資料已複寫至遠端站台
3. 記錄檔資料已在遠端站台寫入
4. 遠端站台做出確認
5. 應用程式寫入已確認

t & t1：資料排清到磁碟區，記錄一律直接寫入

![學習圖示](media/sddc/learn.png)**[深入了解儲存體複本](./storage/storage-replica/storage-replica-overview.md)**

![安全性圖表](media/sddc/security.png)

### <a name="guarded-fabric"></a>受防護網狀架構

![用於間距用途的線條安全性影像](media/sddc/security-line.png)

身為雲端服務提供者或企業私人雲端系統管理員，您可以使用受防護網狀架構為 VM 提供更安全的環境。 受防護網狀架構包含一個主機守護者服務 (HGS) (通常是有三個節點的叢集)，加上一或多個受防護的主機，以及一組受防護的虛擬機器 (VM)。

![受防護網狀架構圖表](media/sddc/spacer1.png)![受防護網狀架構圖表](media/sddc/guarded-fabric.png)

![學習圖示](media/sddc/learn.png)**[深入了解受防護網狀架構](./security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)**

### <a name="shielded-vms"></a>受防護的 VM

![用於間距用途的線條安全性影像](media/sddc/security-line.png)

受防護 VM 的資料及狀態受到保護，可防止惡意程式碼和資料中心管理員加以探查、竊取和竄改。

![僅供間距用途的影像](media/sddc/spacer1.png)![受防護的圖表](media/sddc/shielded.png)

- 受防護的 VM 只會在指定為 VM 擁有者的網狀架構中執行。
- 受防護的 VM 是透過 BitLocker 或其他方式加密，因此只有指定的擁有者才能加以執行。
- 執行中 VM 可以轉換成受防護的狀態。

![學習圖示](media/sddc/learn.png)**[深入了解受防護的 VM](./security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)**

### <a name="host-guardian-service"></a>主機守護者服務

![用於間距用途的線條安全性影像](media/sddc/security-line.png)

主機守護者服務會保存合法網狀架構及加密虛擬機器的金鑰。

![僅供間距用途的影像](media/sddc/spacer1.png)![守護者圖表](media/sddc/guardian.png)

![學習圖示](media/sddc/learn.png)**[深入了解主機守護者服務](./security/guarded-fabric-shielded-vm/guarded-fabric-manage-hgs.md)**

### <a name="device-health-attestation"></a>裝置健康情況證明

![用於間距用途的線條安全性影像](media/sddc/security-line.png)

此證明可讓企業將其組織的安全性基準提升至經過硬體監視和證明的安全性，而不影響作業成本，或只有些微影響。

![僅供間距用途的影像](media/sddc/spacer1.png)![證明圖表](media/sddc/attestation.png)

上述硬體信任模式可透過 TPM v2.0 硬體根信任，在符合金鑰發行程式碼完整性原則的情況下，提供最高等級的保證。

![學習圖示](media/sddc/learn.png)**[深入了解裝置健康情況證明](./security/device-health-attestation.md)**

![管理圖表](media/sddc/management.png)

### <a name="powershell-desired-state-configuration"></a>PowerShell Desired State Configuration

![用於間距用途的線條管理影像](media/sddc/management-line.png)

Windows PowerShell Desired State Configuration 是 Windows 根據開放式標準內建的組態管理平台。 DSC 的彈性足以因應部署生命週期 (開發、測試、生產階段前，生產環境) 各階段穩定且一致的運作，向外延展時亦然。

![僅供間距用途的影像](media/sddc/spacer1.png)![Desired State Configuration 圖表](media/sddc/dsc.png)

DSC 支援「連續部署」，讓您可以重複部署組態，而不會中斷任何作業。

-  DSC 組態只會套用已變更而與原始設定不同的設定，以便更快速進行部署。
-  DSC 可用於內部部署、公用雲端或私人雲端環境。
-  您可以將 DSC 與任何 Microsoft 或非 Microsoft 解決方案整合，只要您能在目標系統上執行 PowerShell 指令碼即可。

![學習圖示](media/sddc/learn.png)**[深入了解 PowerShell DSC](/powershell/dsc/overview)**

### <a name="system-center-vmm"></a>System Center VMM

![用於間距用途的線條管理影像](media/sddc/management-line.png)

Virtual Machine Manager 是 System Center 套件的一部分，用來設定、管理和轉換傳統資料中心，以在所有內部部署、服務提供者和 Azure 雲端間提供一致的管理體驗。

![僅供間距用途的影像](media/sddc/spacer1.png)![Virtual Machine Manager 圖表](media/sddc/vmm.png)

- 資料中心：將資料中心元件作為 VMM 中的單一網狀架構來設定和管理。
- 虛擬化主機：VMM 可新增、佈建和管理 Hyper-V 及 VMWare 虛擬化主機與叢集。
- 網路功能：VMM 提供網路虛擬化功能，包括支援建立和管理虛擬網路及網路閘道。
- 存放裝置：VMM 可探索、分類、佈建、配置和指派本機及遠端儲存體。

![學習圖示](media/sddc/learn.png)**[深入了解 System Center VMM](/system-center/vmm/)**

### <a name="windows-admin-center"></a>Windows Admin Center

![用於間距用途的線條管理影像](media/sddc/management-line.png)

Windows Admin Center 是本機部署的瀏覽器型管理工具組，可在沒有任何 Azure 或雲端相依性的情況下用來進行 Windows Servers 內部部署管理。 Windows Admin Center 賦予 IT 系統管理員對其伺服器基礎結構所有層面的完整控制權，這在管理未連線至網際網路的私人網路時，效用尤佳。

![僅供間距用途的影像](media/sddc/spacer1.png)![架構圖表](media/sddc/architecture.png)

將網頁伺服器發佈至 DNS 並設定公司防火牆，可讓您從公用網際網路存取 Windows Admin Center，而得以隨處使用 Microsoft Edge 或 Google Chrome 連接和管理您的伺服器。

![學習圖示](media/sddc/learn.png)**[深入了解 Windows Admin Center](manage/windows-admin-center/overview.md)**