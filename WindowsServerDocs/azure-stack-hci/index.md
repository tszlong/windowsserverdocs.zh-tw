---
title: Azure Stack HCI 概觀
description: Azure Stack HCI 是超融合式的 Windows Server 2019 叢集，其使用經過驗證的硬體來執行內部部署虛擬化工作負載，並可選擇性地連線到 Azure 服務，以進行雲端備份和站台復原。 Azure Stack HCI 解決方案使用由 Microsoft 驗證的硬體來確保最佳效能和可靠性，並包含 NVMe 磁碟機、持續性記憶體及遠端直接記憶體存取 (RDMA) 網路等技術支援。
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/22/2019
ms.openlocfilehash: 92d600eeb833cd70bd714702b1fd950c4fe2cd87
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/22/2019
ms.locfileid: "66008971"
---
# <a name="azure-stack-hci-overview"></a>Azure Stack HCI 概觀

>適用於：Windows Server 2019

Azure Stack HCI 是超融合式的 Windows Server 2019 叢集，其使用經過驗證的硬體來執行內部部署虛擬化工作負載，並可選擇性地連線到 Azure 服務，以進行雲端備份和站台復原。 Azure Stack HCI 解決方案使用由 Microsoft 驗證的硬體來確保最佳效能和可靠性，並包含 NVMe 磁碟機、持續性記憶體及遠端直接記憶體存取 (RDMA) 網路等技術支援。

Azure Stack HCI 是結合多項產品的解決方案：

- 來自 OEM 夥伴的硬體

- Windows Server 2019 Datacenter 版本

- Windows Admin Center

- Azure 服務 (選擇性)

![Azure Stack HCI 是可由各種不同硬體合作夥伴提供的 Microsoft 超融合解決方案。](media/AS_HCI_solution.png)

Azure Stack HCI 是可由各種不同硬體合作夥伴提供的 Microsoft 超融合解決方案。 請考量下列適用超融合解決方案的情況，以協助您判斷 Azure Stack HCI 是否是最適合您需求的解決方案：

- **重新整理過時硬體。** 取代較舊的伺服器和儲存體基礎結構，並使用現有 IT 技能及工具，在內部部署環境和邊緣執行 Windows 和 Linux 虛擬機器。

- **合併虛擬化工作負載。** 在有效率的超融合式基礎結構上合併舊版應用程式。 探索用來執行大規模資料中心 (例如 Microsoft Azure) 的相同雲端效率類型。

- **連線到 Azure 以取得混合式雲端服務。** 簡化 Azure 中雲端管理和安全性服務的存取，包括異地備份、站台復原及雲端式監控等。

## <a name="the-azure-stack-family"></a>Azure Stack 系列

Azure Stack HCI 屬於 Azure 和 Azure Stack 系列，與 Azure Stack 使用相同的軟體定義計算、儲存體和網路軟體。 以下是不同解決方案的快速摘要：

- [Azure](https://azure.microsoft.com) - 使用公用雲端服務
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack) - 在內部部署環境操作雲端服務
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) - 在內部部署環境執行虛擬化應用程式，並且可選擇性地連線到 Azure

![Azure 和 Azure Stack 可執行雲端服務，而 Azure Stack HCI 可在內部部署環境執行虛擬化應用程式](media/azure_family.png)

|Azure：使用公用雲端服務|Azure Stack：在內部部署環境操作雲端服務|Azure Stack HCI：在內部部署環境執行虛擬化應用程式|
|-----------------|-----------------|-----------------|
|運用隨選的自助式計算資源來移轉和現代化現有應用程式，以及建立新的雲端原生應用程式。|在邊緣建置和執行雲端應用程式，當連線中斷或為了符合法規要求時，在內部部署環境使用一致的 Azure 服務。| 在內部部署環境執行虛擬化應用程式、取代及合併過時的伺服器基礎結構，以及連線到 Azure 雲端服務。|
|在全球 54 個區域中有 100 個以上的服務可用|適用於 Windows 和 Linux 的 Azure VM、Azure Web Apps 及 Functions、Azure Key Vault、Azure Resource Manager、Azure Marketplace、容器、Azure IoT 及事件中樞、系統管理工具 (方案、供應項目、RBAC)|由 Hyper V 和「儲存空間直接存取」搭配 Windows Server 2019 及 Windows Admin Center 支援的已驗證 HCI 解決方案，可對 Azure 服務進行管理及整合存取。|

若要深入了解：

- 深入了解我們的 [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) 解決方案網站。
- 觀看 Microsoft 專家 Jeff Woolsey 和 Vijay Tewari [討論新的 Azure Stack HCI 解決方案](https://aka.ms/AzureStackOverviewVideo)。

## <a name="hyperconverged-efficiencies"></a>超融合的效率

Azure Stack HCI 解決方案在業界標準的 x86 伺服器和元件上，結合高度虛擬化的計算、儲存體和網路功能在。 在相同叢集中結合資源，可讓您更輕鬆地進行部署、管理及調整。 使用您選擇的命令列自動化或 Windows Admin Center 來管理。

透過 Hyper-V (Microsoft 雲端的基礎 Hypervisor 技術) 和儲存空間直接存取，以及內建的 NVMe、持續性記憶體和遠端直接記憶體存取 (RDMA) 網路支援，您的伺服器應用程式可擁有領先業界的虛擬機器效能。

以受防護的虛擬機器、網路微分割及待用資料與傳輸中資料的原生加密來協助保護應用程式和資料安全。

## <a name="hybrid-capabilities"></a>混合功能

您可以透過公用雲端中的超融合式基礎結構平台，充分運用一起使用雲端和內部部署項目的優勢。 您的小組可以透過內建整合開始對 Azure 基礎結構管理服務建置雲端技能：

- Azure Site Recovery，用於高可用性及災害復原即服務 (disaster recovery as a service，DRaaS)。

- Azure 監視器，用於追蹤應用程式、網路及基礎結構間所發生事件的集中式中樞。

- 雲端見證 (Cloud Witness)，使用 Azure 作為叢集仲裁的輕量決勝原則。

- Azure 備份，用於保護異地資料及防範勒索軟體。

- Azure 更新管理，用於評估更新，以及更新 Azure 和內部部署環境中執行的 Windows VM 部署。

- Azure 網路介面卡，透過點對站 VPN 來連結 Azure 中的 VM 與內部部署環境中的資源。

- 若要同步您的檔案伺服器與雲端，可使用 Azure 檔案同步。

如需詳細資訊，請參閱[將 Windows Server 連線到 Azure 混合式服務](..\manage\windows-admin-center\azure\index.md)。

## <a name="management-tools-and-system-center"></a>管理工具和 System Center

Azure Stack HCI 會與 Azure Stack 使用相同的虛擬化及軟體定義儲存體和網路軟體。 透過 Azure Stack HCI，您在叢集上便具有完整的系統管理員權限：您可以使用 [Windows Admin Center](..\manage\windows-admin-center\overview.md)、[System Center](https://www.microsoft.com/cloud-platform/system-center)，以及 [HYPER-V](../virtualization/hyper-v/hyper-v-on-windows-server.md)、[儲存空間直接存取](..\storage\storage-spaces\storage-spaces-direct-overview.md)、PowerShell 及第三方工具 (例如 5Nine Manager) 的任何功能。

Microsoft Azure 會在 Windows Server 包含的相同 Windows Server 和 Hyper-V 平台上執行。 Windows Server 和 System Center 包括 Microsoft 在操作全球資料中心網路 (如 Microsoft Azure) 所加入的增強功能及體驗的最佳做法，讓您在使用軟體設計的網路功能技術時，可以部署相同的技術以實現彈性、自動化和控制的功能。

使用 System Center 虛擬機器管理 (VMM) 與 System Center Operations Manager 來部署及管理基礎結構。 透過 VMM，您可以佈建及管理在私人雲端中建立及部署虛擬機器和服務所需的資源。 透過 Operations Manager，您可以監視整個企業中的服務、裝置和作業，進而找出問題並立即採取行動。

## <a name="hardware-partners"></a>硬體合作夥伴

您可以向 15 個合作夥伴購買執行 Windows Server 2019 的已驗證 Azure Stack HCI 解決方案。 您偏好的 Microsoft 合作夥伴會為您啟動及執行方案，完全不需要冗長的設計和建置階段，並且會提供實作和支援服務的單一連絡點。

請瀏覽 [Azure Stack HCI 網站](https://azure.microsoft.com/overview/azure-stack/hci)，以檢視目前可從 Microsoft 合作夥伴取得的超過 70 個 Azure Stack HCI 解決方案：ASUS、Axellio、bluechip、DataON、Dell EMC、Fujitsu、HPE、Hitachi、Huawei、Lenovo、NEC、primeLine Solutions、QCT、SecureGUARD 及 Supermicro。

## <a name="faq"></a>常見問題集

### <a name="what-do-azure-stack-and-azure-stack-hci-solutions-have-in-common"></a>Azure Stack 和 Azure Stack HCI 解決方案有哪些共通項目？

Azure Stack HCI 解決方案與 Azure Stack 同樣有 HYPER-V 架構的軟體定義計算、儲存和網路技術功能。 這兩個供應項目皆符合嚴格的測試和驗證準則，目的是確保與基礎硬體平台搭配時的可靠性和相容性。

### <a name="how-are-they-different"></a>這兩者有何不同？

透過 Azure Stack，您可以在內部部署環境執行雲端服務。 您可以在內部部署環境執行 Azure IaaS 和 PaaS 服務，無論位於任何位置，都能以一致的方式建置並執行雲端應用程式，並且可在內部部署環境中使用 Azure 入口網站進行管理。

透過 Azure Stack HCI，您可以在內部部署環境執行虛擬化工作負載，並使用 Windows Admin Center 和熟悉的 Windows Server 工具進行管理。 您可以選擇性地連線到 Azure 進行混合式案例，例如雲端式站台復原、監視等其他項目。

### <a name="why-is-microsoft-bringing-its-hci-offering-to-the-azure-stack-family"></a>Microsoft 為什麼將 HCI 供應項目帶入 Azure Stack 系列？

Microsoft 的超融合技術已經是 Azure Stack 的基礎。

許多 Microsoft 客戶都擁有複雜的 IT 環境，而我們的目標是提供符合客戶需求的解決方案，讓他們使用適當的技術來因應適當的業務需求。 Azure Stack HCI 是以 Windows Server 2016 為基礎的創新 Windows Server 軟體定義 (WSSD) 解決方案，之前是由我們的硬體合作夥伴所提供。 之所以將其帶入 Azure Stack 系列，是因為我們已開始提供新選項來無縫連結 Azure，進而取得基礎結構管理服務。

### <a name="does-azure-stack-hci-need-to-be-connected-to-azure"></a>Azure Stack HCI 需要連線到 Azure 嗎？

不需要，這完全是選擇性項目。 您可以利用與 Azure 整合來進行混合式案例，例如異地備份和災害復原、以雲端為基礎的監視和更新管理，但這些都是選擇性項目。 我們完全了解且願意配合您希望或必須中斷連線來執行方案的情況。

### <a name="how-does-azure-stack-hci-relate-to-windows-server"></a>Windows Server 與 Azure Stack HCI 有何關係？

Windows Server 2019 幾乎是每個 Azure 產品的基礎。 您重視的所有功能都會繼續存在 Windows Server 中，並且受到支援。 Azure Stack HCI 是在內部部署 HCI 的建議方式，您可以從我們的合作夥伴端使用經過 Microsoft 驗證的硬體。

### <a name="will-i-be-able-to-upgrade-from-azure-stack-hci-to-azure-stack"></a>我可以從 Azure Stack HCI 升級至 Azure Stack 嗎？ 

不能，但客戶可以將工作負載從 Azure Stack HCI 遷移至 Azure Stack 或 Azure。

### <a name="what-azure-services-can-i-connect-to-azure-stack-hci"></a>哪些 Azure 服務可連結到 Azure Stack HCI？

如需可連結 Azure Stack HCI 的 Azure 服務更新清單，請參閱[將 Windows Server 連結到 Azure 混合式服務](../azure-hybrid-services/index.md)。

### <a name="how-does-the-cost-of-azure-stack-hci-compare-to-azure-stack"></a>Azure Stack HCI 和 Azure Stack 的成本比較為何？ 

Azure Stack 是以整個整合系統售出，其中包含服務和支援。 您購買的 Azure Stack 可以是由您管理的系統，或是來自我們合作夥伴的完全受控服務。 除了基礎系統，在 Azure Stack 或 Azure 上執行的 Azure 服務皆以隨用隨付為基礎來販售。

Azure Stack HCI 解決方案遵循傳統的購買模式。 您可以向 Azure Stack HCI 合作夥伴購買經驗證的硬體，以及從現有的各種管道購買軟體 (搭配軟體定義資料中心功能的 Windows Server 2019 Datacenter 版本與 Windows Admin Center)。 若是與 Windows Admin Center 搭配使用的 Azure 服務，您可以透過 Azure 訂用帳戶支付。

### <a name="how-do-i-buy-azure-stack-hci-solutions"></a>我要如何購買 Azure Stack HCI 解決方案？

請執行下列步驟：

1. 向您偏好的硬體合作夥伴購買經 Microsoft 驗證的硬體系統。
1. 安裝 Windows Server 2019 Datacenter 版本和 Windows Admin Center 來取得管理功能，以及連線到 Azure 以取得雲端服務的能力
1. 您可以選擇性地使用 Azure 帳戶來將雲端式管理及安全性服務連結至您的工作負載。

![若要購買 Azure Stack HCI 解決方案，請選擇最適合您需求的硬體合作夥伴和設定。](media/howbuy_ashci.png)

## <a name="compare-azure-stack-and-azure-stack-hci"></a>比較 Azure Stack 和 Azure Stack HCI

當您的組織進行數位轉換時，藉由使用公用雲端服務在新式架構上進行建置及重新整理舊版應用程式，您會發現您可以更快速地移動。 不過，基於技術及法規障礙等理由，許多工作負載必須保留在內部部署環境中。 下表可協助您判斷哪一個 Microsoft 混合式雲端策略可隨時隨地提供您需要的服務，並為任何位置上的工作負載提供雲端創新。

|Azure Stack|Azure Stack HCI|
|--------|-------|
|新的技能，創新的程序|相同的技能，熟悉的程序|
|Azure 服務在您資料中心內|將您的資料中心連結到 Azure 服務|

### <a name="when-to-use-azure-stack"></a>使用 Azure Stack 的時機

|Azure Stack|Azure Stack HCI|
|--------|-------|
|針對自助式結構即服務 (IaaS) 使用 Azure Stack，其具備強式隔離，並且可對多個位於相同位置的租用戶精確地追蹤使用情況及進行扣款。 適用於服務提供者和企業私人雲端。 範本來自 Azure Marketplace。|Azure Stack HCI 本身不會針對多租用戶強制執行或提供項目。|
|在內部部署環境使用 Azure Stack 開發及執行依賴 Web Apps、Functions 或事件中樞等平台即服務 (PaaS) 的應用程式。 在 Azure Stack 上執行的這些服務，完全與在 Azure 中執行一樣，提供一致的混合式開發和執行階段環境。|Azure Stack HCI 不會在內部部署環境中執行 PaaS 服務。
|您可以使用 Azure Stack 來現代化應用程式部署及搭配 DevOps 做法的作業，這些做法包括基礎結構程式碼、持續整合和持續部署 (CI/CD)，以及像是與 Azure 一致的 VM 擴充功能等方便功能。 適用於開發和 DevOps 小組。|Azure Stack HCI 本身不包含任何 DevOps 工具。

### <a name="when-to-use-azure-stack-hci"></a>使用 Azure Stack HCI 的時機

|Azure Stack|Azure Stack HCI|
|---------------|---------------|
|Azure Stack 需要最少 4 個節點和其本身的網路交換器。|針對遠端辦公室/分公司 (ROBO) 的最少使用量使用 Azure Stack HCI。 為達到最高簡易度和可負擔程度，只需 2 個伺服器節點和無交換器的背對背網路即可開始。 硬體供應項目只需 4 個磁碟機、64 GB 的記憶體即可啟動，一切可在 $10k/節點下順利進行。
|為了與 Azure 一致，Azure Stack 會限制 Hyper V 可設定性和功能。|使用 Azure Stack HCI 為傳統企業應用程式 (例如 Exchange、SharePoint 和 SQL Server) 實現單純實用的 Hyper-V 虛擬化，以及虛擬化 Windows Server 角色，例如檔案伺服器、DNS、DHCP、IIS 和 AD。 無限制地的存取所有 HYPER-V 功能，例如受防護的 VM。|
|Azure Stack 不會公開這些基礎結構技術。|使用 Azure Stack HCI 來取代軟體定義基礎結構，代替過時的存放裝置陣列或網路應用裝置，而且不需要大肆重新建構。 內建儲存空間直接存取和軟體定義網路 (SDN) 可讓您順利地整合 Hyper-V 環境。|