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
ms.openlocfilehash: 063fe828e906cfdb6b787322942272335591d404
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407048"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>將 Windows Server 連線到 Azure 混合式服務

>適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

您可以使用 Azure 混合式服務將 Windows Server 的內部部署延伸至雲端。 這些雲端服務提供大量實用功能，包括下列項目：

- 保護虛擬機器並透過 Azure Site Recovery 使用雲端式備份及災害復原 (HA/DR)。 
- 透過 Azure 監視器中的進階分析和機器學習，追蹤應用程式、網路及基礎結構間所發生的事件。 
- 簡化 Azure 與 Azure 網路介面卡的網路連線。
- 透過 Azure 更新管理維持最新的虛擬機器狀態。

Azure 混合式服務會在下列組態中與 Windows Server 搭配使用：

- 獨立實體伺服器和虛擬機器 (VM)
- 叢集，包括經過 [Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview) 和 [Windows Server 軟體定義 (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter) 程式認證的程超融合式叢集

雖然您可以使用 Azure 入口網站和進行一兩個下載來設定大部分的 Azure 混合式服務，但有許多服務是直接整合到 Windows Admin Center 中，藉此提供簡化的安裝體驗和以伺服器為主的服務檢視。

## <a name="azure-hybrid-services-tool"></a>Azure 混合式服務工具

[Windows Admin Center](../understand/windows-admin-center.md) 中的 Azure 混合式服務工具會將所有整合的 Azure 服務合併到集中式中樞，您可以在其中輕鬆地找到所有可用的 Azure 服務，並將值帶到您內部部署或混合式環境中。 

![顯示 Azure 混合式服務工具的 Windows Admin Center 螢幕擷取畫面](../media/azure-services/ahs-discover.png)

如果您連線到已啟用 Azure 服務的伺服器，Azure 混合式服務工具會以單一窗格形式提供，用來查看該伺服器上所有已啟用的服務。 您可以輕鬆地在 Windows Admin Center 內取得相關工具，使用 Azure 入口網站深入地管理這些 Azure 服務，或是從隨手可得的文件中了解更多資訊。 

![顯示伺服器上已安裝 Azure 服務的 Windows Admin Center 螢幕擷取畫面](../media/azure-services/ahs-dayN.png)

您可以從 Azure 混合式服務工具中執行下列動作：
- 使用 [Azure 備份](azure-backup.md)從 Windows Admin Center 中備份 Windows Server
- 與 [Azure Site Recovery](azure-site-recovery.md) 搭配使用 Windows Admin Center 以保護您的 Hyper-V 虛擬機器
- 使用 [Azure 檔案同步](azure-file-sync.md)來同步您的檔案伺服器與雲端
- 使用 [Azure 更新管理](azure-update-management.md)，為您所有的內部部署或雲端 Windows 伺服器管理作業系統更新
- 使用 [Azure 監視器](azure-monitor.md)來監視內部部署或雲端中的伺服器並設定警示
- 使用 [Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)將您的內部部署伺服器連線到 Azure 虛擬網路

## <a name="services-for-stand-alone-servers-and-vms"></a>獨立伺服器和 VM 的服務

以下是為獨立伺服器和 VM 提供功能的 Azure 服務完整清單：

- **(新功能) 使用 [Azure 檔案同步](https://aka.ms/afs)來同步您的檔案伺服器與雲端**  
同步此伺服器上的檔案與 Azure 檔案共用。 在本機保留所有檔案，或使用雲分層來只快取伺服器上最常使用的檔案，並將冷資料分層放至雲端。 您可以備份雲端中的資料，不必費力備份內部部署伺服器。 此外，多站台同步可讓一組檔案在多部伺服器間保持同步。

- **藉由新增 [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) 驗證來將安全性層級加入 Windows Admin Center**  
若要將其他安全性層級加入 Windows Admin Center，您可以要求使用者使用 Azure Active Directory (Azure AD) 身分識別進行驗證來存取閘道。 Azure AD 驗證也可讓您充分利用 Azure AD 的安全性功能，例如條件式存取和多重要素驗證。  
如需詳細資訊，請參閱[設定 Windows Admin Center 的 Azure AD 驗證。](../configure/user-access-control.md#azure-active-directory)  

- **使用 [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) 保護您的 Hyper-V 虛擬機器**  
您可以複寫 VM 上執行的工作負載，這樣您的業務關鍵基礎結構就會在發生嚴重損壞時受到保護。 Windows Admin Center 可簡化 HYPER-V 伺服器或叢集上的虛擬機器設定及複寫程序，方便您透過 Azure Site Recovery 的災害復原服務來加強環境的復原能力。  
如需詳細資訊，請參閱[使用 Azure Site Recovery 和 Windows Admin Center 保護您的 VM](azure-site-recovery.md)。

- **使用 [Azure 備份](https://docs.microsoft.com/azure/backup/backup-overview)來備份您的 Windows 伺服器**  
您可以將 Windows 伺服器備份至 Azure，以防禦意外或惡意的刪除、損毀及勒索軟體。  
如需詳細資訊，請參閱[使用 Azure 備份來備份伺服器](azure-backup.md)。

- **使用[適用於虛擬機器的 Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)來監視及取得環境中所有伺服器的電子郵件警示**  
您可以使用 Azure 監視器 (也稱為虛擬機器深入解析) 來監視伺服器健康情況和事件、建立電子郵件警示、取得環境中伺服器效能的彙總檢視，以及視覺化連線到指定伺服器的應用程式、系統和服務。  
如需詳細資訊，請參閱[將伺服器連線至 Azure 監視器並設定電子郵件通知](azure-monitor.md)。

- **使用 [Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)集中管理所有 Windows Server 的作業系統更新**  
您可以從單一位置管理多個伺服器和 VM 的更新及修補，而不是以每個伺服器為基礎來進行管理。 透過 Azure 更新管理，您可以快速評估可用更新的狀態、排定必要更新的安裝時間，以及檢閱部署結果來確認是否已成功套用更新。 無論您的伺服器是否是 Azure VM、是否由其他雲端提供者裝載，或是位於內部部署環境，您都可以這麼做。  
如需詳細資訊，請參閱[來使用 Azure 更新管理](azure-update-management.md)。

- **使用 [Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)將您的內部部署伺服器連線到 Azure 虛擬網路**  
您可以將 Azure 網路介面卡新增至內部部署伺服器，以協助您安全地將伺服器連線到 Azure 虛擬網路。  
如需詳細資訊，請參閱[在內部部署 Windows Server 和 Azure 虛擬網路之間設定點對站 VPN 連線](https://aka.ms/WACNetworkAdapter)。

- **使用 [Windows Admin Center](manage-azure-vms.md) 管理 Azure IaaS 虛擬機器**  
您可以使用 Windows Admin Center 來管理您的 Azure VM 及內部部署機器。 藉由設定 Windows Admin Center 閘道來連線至 Azure VNet，您可以在 Azure 中使用 Windows Admin Center 所提供一致且簡化的工具來管理虛擬機器。 如需詳細資訊，請參閱[設定 Windows Admin Center 來管理 Azure 中的 VM](manage-azure-vms.md)。

## <a name="services-for-clusters"></a>叢集服務

以下是以一個整體來提供功能給叢集的 Azure 服務 (這些服務尚未全部整合至 Windows Admin Center)：

- [使用 Azure 監視器來監視超融合式叢集](../../../storage/storage-spaces/configure-azure-monitor.md)
- [使用 Azure Site Recovery 保護您的 VM](azure-site-recovery.md)
- [部署叢集雲端見證](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="see-also"></a>請參閱

- [將 Windows Admin Center 連線到 Azure ](azure-integration.md)
- [在 Azure 中部署 Windows Admin Center](deploy-wac-in-azure.md)