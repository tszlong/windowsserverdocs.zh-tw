---
title: 將 Windows Server 連線到 Azure 混合式服務
description: 您可以使用 Azure 混合式服務將 Windows Server 的內部部署延伸至雲端。
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 05/31/2019
ms.openlocfilehash: ec01431b320c710eddedc2f9c5e174bea1355b1c
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475695"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>將 Windows Server 連線到 Azure 混合式服務

您可以使用 Azure 混合式服務將 Windows Server 的內部部署延伸至雲端。 這些雲端服務提供一系列有用的函式，可將內部部署擴充至 Azure，以及從 Azure 進行集中管理。

![此圖顯示了從內部部署到雲端的箭號，以將內部部署擴充至 Azure；以及顯示從雲端到內部部署的箭號，以透過 Azure 進行集中管理](../media/azure-services/hybrid-framing.png)

在 Windows Admin Center 內使用 Azure 混合式服務，您可以：

- [保護虛擬機器並使用雲端式備份及災害復原 (HA/DR)](#back-up-and-protect-your-on-premises-servers-and-vms)。
- [透過 Azure 中的儲存體和計算來擴充內部部署容量，並簡化與 Azure 的網路連線能力](#extend-on-premises-capacity-with-azure)。
- [透過雲端智慧型 Azure 管理服務，橫跨應用程式、網路和基礎結構來集中監視、管理、設定和安全性](#centrally-manage-your-hybrid-environment-from-azure)。

雖然您可以透過下載應用程式並進行一些手動配置來設定大部分的 Azure 混合式服務，但有許多服務是直接整合到 Windows Admin Center 中，藉此提供簡化的安裝體驗和以伺服器為主的服務檢視。 Windows Admin Center 也為 Azure 入口網站提供便利的智慧型超連結，以查看已連線的 Azure 資源以及混合式環境的集中檢視。

## <a name="discover-integrated-services-in-the-azure-hybrid-services-tool"></a>探索 Azure 混合式服務工具中的整合式服務

[Windows Admin Center](../overview.md) 中的 Azure 混合式服務工具會將所有整合的 Azure 服務合併到集中式中樞，您可以在其中輕鬆地找到所有可用的 Azure 服務，並將值帶到您內部部署或混合式環境中。

![顯示 Azure 混合式服務工具的 Windows Admin Center 螢幕擷取畫面](../media/azure-services/ahs-discover.png)

如果您連線到已啟用 Azure 服務的伺服器，Azure 混合式服務工具會以單一窗格形式提供，用來查看該伺服器上所有已啟用的服務。 您可以輕鬆地在 Windows Admin Center 內取得相關工具，使用 Azure 入口網站深入地管理這些 Azure 服務，或是從隨手可得的文件中了解更多資訊。

![顯示伺服器上已安裝 Azure 服務的 Windows Admin Center 螢幕擷取畫面](../media/azure-services/ahs-dayN.png)

您可以從 Azure 混合式服務工具中執行下列動作：

- 使用 [Azure 備份](azure-backup.md)從 Windows Admin Center 中備份 Windows Server
- 與 [Azure Site Recovery](azure-site-recovery.md) 搭配使用 Windows Admin Center 以保護您的 Hyper-V 虛擬機器
- 使用 [Azure 檔案同步](azure-file-sync.md)來同步您的檔案伺服器與雲端
- 使用 [Azure 更新管理](azure-update-management.md)，為您所有的內部部署或雲端 Windows 伺服器管理作業系統更新
- 使用 [Azure 監視器](azure-monitor.md)來監視內部部署或雲端中的伺服器並設定警示
- 使用[適用於伺服器的 Azure Arc](https://docs.microsoft.com/azure/azure-arc/servers/overview)，透過 Azure 原則將治理原則套用至內部部署伺服器
- 透過 [Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration)保護伺服器並取得進階威脅防護
- 使用 [Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)將您的內部部署伺服器連線到 Azure 虛擬網路
- 使用 [Azure 擴充網路讓 Azure VM 看起來像您的內部部署網路](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409)

## <a name="back-up-and-protect-your-on-premises-servers-and-vms"></a>備份並保護您的內部部署伺服器和 VM

- **使用 [Azure 備份](https://docs.microsoft.com/azure/backup/backup-overview)來備份您的 Windows 伺服器** ：您可以將 Windows 伺服器備份至 Azure，以防禦意外或惡意的刪除、損毀及勒索軟體。
如需詳細資訊，請參閱[使用 Azure 備份來備份伺服器](azure-backup.md)。

- **使用 [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) 保護您的 Hyper-V 虛擬機器**：您可以複寫 VM 上執行的工作負載，這樣您的業務關鍵基礎結構就會在發生嚴重損壞時受到保護。 Windows Admin Center 可簡化 HYPER-V 伺服器或叢集上的虛擬機器設定及複寫程序，方便您透過 Azure Site Recovery 的災害復原服務來加強環境的復原能力。
如需詳細資訊，請參閱[使用 Azure Site Recovery 和 Windows Admin Center 保護您的 VM](azure-site-recovery.md)。

- **使用 [儲存體複本](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-overview)對 Azure 中的 VM 使用同步或非同步以區塊為基礎的複寫**：您可以在伺服器層級上設定以區塊為基礎或以磁碟區為基礎的複寫，使用儲存體複本至次要伺服器或 VM。 Windows Admin Center 可讓您針對複寫目標建立專門的 Azure VM，協助您在新的 Azure VM 上適當地設定儲存體大小並正確設定。
如需詳細資訊，請參閱[使用儲存體複本伺服器對伺服器複寫](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)。

## <a name="extend-on-premises-capacity-with-azure"></a>使用 Azure 擴充內部部署容量

### <a name="extend-storage-capacity"></a>擴充儲存體容量

- **使用 [Azure 檔案同步](https://aka.ms/afs)來同步您的檔案伺服器與雲端**：同步此伺服器上的檔案與 Azure 檔案共用。 在本機保留所有檔案，或使用雲分層來釋出空間並只快取伺服器上最常使用的檔案，並將冷資料分層放至雲端。 您可以備份雲端中的資料，不必費力備份內部部署伺服器。 此外，多站台同步可讓一組檔案在多部伺服器間保持同步。
如需詳細資訊，請參閱[使用 Azure 檔案同步來同步您的檔案伺服器與雲端](azure-file-sync.md)。

- **使用[儲存體移轉服務](https://docs.microsoft.com/windows-server/storage/storage-migration-service/overview)將儲存體遷移至 Azure 中的 VM**：使用逐步工具來清查 Windows 和 Linux 伺服器上的資料，然後將資料傳輸至新的 Azure VM。 Windows Admin Center 可以為工作建立新的 Azure VM，以適當調整大小並正確設定，以接收來自來源伺服器的資料。
如需詳細資訊，請參閱[使用存放裝置移轉服務來移轉伺服器](https://docs.microsoft.com/windows-server/storage/storage-migration-service/migrate-data)。

### <a name="extend-compute-capacity"></a>擴充計算容量

- **建立新的 Azure 虛擬機器，而不脫離 Windows Admin Center**：從 Windows Admin Center 內的 [所有連線] 頁面，移至 [新增]，然後在 [Azure VM] 下選取 [新建]。 您甚至可以網域加入 Azure VM，並在此逐步建立工具中設定儲存體。

- **透過[雲端見證](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)，利用 Azure 在容錯移轉叢集上實現仲裁**：您可以使用 Azure 儲存體帳戶作為 Azure Stack HCI 叢集或其他容錯移轉叢集的叢集見證，而不需要投資額外的硬體以在 2 節點叢集上實現仲裁。
如需詳細資訊，請參閱[部署容錯移轉叢集的雲端見證](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)。

### <a name="simplify-network-connectivity-between-your-on-premises-and-azure-networks"></a>簡化內部部署和 Azure 網路之間的網路連線

- **使用 [Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)將您的內部部署伺服器連線到 Azure 虛擬網路**：讓 Windows Admin Center 簡化從內部部署伺服器到 Azure 虛擬網路的點對站 VPN 設定。

- **使用 [Azure 擴充網路](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409)讓 Azure VM 看起來像您的內部部署網路**：Windows Admin Center 可以設定站對站 VPN，並將您的內部部署 IP 位址擴充到您的 Azure vNet，讓您更輕鬆地將工作負載移轉至 Azure，而不會中斷 IP 位址的相依性。

## <a name="centrally-manage-your-hybrid-environment-from-azure"></a>從 Azure 集中管理您的混合式環境

- **使用[適用於虛擬機器的 Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)來監視及取得環境中所有伺服器的電子郵件警示**：您可以使用 Azure 監視器 (也稱為虛擬機器深入解析) 來監視伺服器健康情況和事件、建立電子郵件警示、取得環境中伺服器效能的彙總檢視，以及視覺化連線到指定伺服器的應用程式、系統和服務。 Windows Admin Center 也可以設定伺服器健康情況效能和叢集健康情況事件的預設電子郵件警示。
如需詳細資訊，請參閱[將伺服器連線至 Azure 監視器並設定電子郵件通知](azure-monitor.md)。

- **使用 [Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)集中管理所有 Windows Server 的作業系統更新**：您可以從單一位置管理多個伺服器和 VM 的更新及修補，而不是以每個伺服器為基礎來進行管理。 透過 Azure 更新管理，您可以快速評估可用更新的狀態、排定必要更新的安裝時間，以及檢閱部署結果來確認是否已成功套用更新。 無論您的伺服器是否是 Azure VM、是否由其他雲端提供者裝載，或是位於內部部署環境，您都可以這麼做。
如需詳細資訊，請參閱[來使用 Azure 更新管理](azure-update-management.md)。

- **透過 [Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center/security-center-intro)改善您的安全性狀態並取得進階威脅防護**：Azure 資訊安全中心是統一的基礎結構安全性管理系統，其可強化資料中心的安全性狀態，並在雲端的混合式工作負載 (無論是否在 Azure 中) 以及內部部署的混合式工作負載中提供進階威脅防護。 使用 Windows Admin Center，您可以輕鬆地設定伺服器並將其連線至 Azure 資訊安全中心。
如需詳細資訊，請參閱[整合 Azure 資訊安全中心與 Windows Admin Center (預覽)](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration)。

- **使用[適用於伺服器的 Azure Arc](https://docs.microsoft.com/azure/azure-arc/servers/overview) 和 [Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview)** ，在混合式環境中套用原則並確保合規性：清查、組織及管理 Azure 的內部部署伺服器。 您可以使用 Azure 原則來管理伺服器、使用 RBAC 來控制存取，以及從 Azure 啟用其他管理服務。

## <a name="clusters-versus-stand-alone-servers-and-vms"></a>叢集與獨立伺服器和 VM

Azure 混合式服務會在下列組態中與 Windows Server 搭配使用：

- 獨立實體伺服器和虛擬機器 (VM)
- 叢集，包括經過 [Azure Stack HCI](../../../azure-stack-hci/index.md) 和 [Windows Server 軟體定義 (WSSD)](https://www.microsoft.com/cloud-platform/software-defined-datacenter) 程式認證的程超融合式叢集

### <a name="services-for-stand-alone-servers-and-vms"></a>獨立伺服器和 VM 的服務

以下是為獨立伺服器和 VM 提供功能的 Azure 服務完整清單：

- 使用 [Azure 備份](azure-backup.md)從 Windows Admin Center 中備份 Windows Server
- 與 [Azure Site Recovery](azure-site-recovery.md) 搭配使用 Windows Admin Center 以保護您的 Hyper-V 虛擬機器
- 使用 [Azure 檔案同步](azure-file-sync.md)來同步您的檔案伺服器與雲端
- 使用 [Azure 更新管理](azure-update-management.md)，為您所有的內部部署或雲端 Windows 伺服器管理作業系統更新
- 使用 [Azure 監視器](azure-monitor.md)來監視內部部署或雲端中的伺服器並設定警示
- 使用[適用於伺服器的 Azure Arc](https://docs.microsoft.com/azure/azure-arc/servers/overview)，透過 Azure 原則將治理原則套用至內部部署伺服器
- 透過 [Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration)保護伺服器並取得進階威脅防護
- 使用 [Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)將您的內部部署伺服器連線到 Azure 虛擬網路
- 使用 [Azure 擴充網路讓 Azure VM 看起來像您的內部部署網路](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409)

### <a name="services-for-clusters"></a>叢集服務

這些是為叢集提供整體功能的 Azure 服務：

- [使用 Azure 監視器來監視超融合式叢集](../../../storage/storage-spaces/configure-azure-monitor.md)
- [使用 Azure Site Recovery 保護您的 VM](azure-site-recovery.md)
- [部署叢集雲端見證](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="other-azure-integrated-abilities-of-windows-admin-center"></a>Windows Admin Center 的其他 Azure 整合功能

- **[在 Windows Admin Center 新增 Azure VM 連線](manage-azure-vms.md)** ：您可以使用 Windows Admin Center 來管理您的 Azure VM 及內部部署機器。 藉由設定 Windows Admin Center 閘道來連線至 Azure VNet，您可以在 Azure 中使用 Windows Admin Center 所提供一致且簡化的工具來管理虛擬機器。
如需詳細資訊，請參閱[設定 Windows Admin Center 來管理 Azure 中的 VM](manage-azure-vms.md)。

- **藉由新增 [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) 驗證來將安全性層級加入 Windows Admin Center**：若要將其他安全性層級加入 Windows Admin Center，您可以要求使用者使用 Azure Active Directory (Azure AD) 身分識別進行驗證來存取閘道。 Azure AD 驗證也可讓您充分利用 Azure AD 的安全性功能，例如條件式存取和多重要素驗證。
如需詳細資訊，請參閱[設定 Windows Admin Center 的 Azure AD 驗證](../configure/user-access-control.md#azure-active-directory)。

- **直接透過內嵌於 Windows Admin Center 的 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)管理 Azure 資源**：利用 Azure Cloud Shell 在 Windows Admin Center 獲得 Bash 或 PowerShell 體驗，讓您輕鬆存取 Azure 管理工作。
如需詳細資訊，請參閱 [Azure Cloud Shell 概觀](https://docs.microsoft.com/azure/cloud-shell/overview)。


## <a name="additional-references"></a>其他參考資料

- [將 Windows Admin Center 連線到 Azure ](azure-integration.md)
- [在 Azure 中部署 Windows Admin Center](deploy-wac-in-azure.md)
