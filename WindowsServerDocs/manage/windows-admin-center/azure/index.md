---
title: Windows 伺服器連線到 Azure 的混合式服務
description: 您可以使用 Azure 的混合式服務擴充至雲端的內部部署的 Windows Server。
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/31/2019
ms.openlocfilehash: 460399a57bc229b44d37a9fdd1e4938bf9e7d6ac
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455358"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>Windows 伺服器連線到 Azure 的混合式服務

>適用於：Windows Server 2019，Windows Server 2016 中，Windows Server （半年通道）

您可以使用 Azure 的混合式服務擴充至雲端的內部部署的 Windows Server。 這些雲端服務提供實用的功能，包括下列陣列：

- 保護虛擬機器，並使用 Azure Site Recovery 中使用雲端架構備份和災害復原 (HA/DR)。 
- 追蹤您的應用程式、 網路和基礎結構的進階的分析和機器學習在 Azure 監視器中協助狀況。 
- 簡化 Azure 與 Azure 網路介面卡的網路連線。
- 維持最新的 Azure 更新管理的虛擬機器。

Azure 的混合式服務會使用下列組態的 Windows 伺服器：

- 獨立實體伺服器和虛擬機器 (Vm)
- 叢集，包括超交集叢集經過[Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview)，並[Windows Server Software-Defined (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)程式

雖然您可以設定使用 Azure 入口網站和下載或兩者的大部分的 Azure 混合式服務，有許多是直接整合到 Windows Admin Center 提供簡化的安裝體驗和服務以伺服器為中心檢視。

## <a name="azure-hybrid-services-tool"></a>Azure 的混合式服務工具

Azure 的混合式服務工具[Windows Admin Center](../understand/windows-admin-center.md)將所有整合的 Azure 服務合併到集中式中樞，可以輕易地找到所有可用的 Azure 服務，將值帶到您內部部署或混合式環境。 

![螢幕擷取畫面的 Windows Admin Center 顯示 Azure 混合式服務工具](../media/azure-services/ahs-discover.png)

如果您已啟用的 Azure 服務連線到伺服器，Azure 的混合式的 [服務] 工具會當做單一窗格，可用來查看該伺服器上的所有已啟用的服務。 輕鬆地，您可以前往 Windows Admin Center 內的相關工具，輕鬆啟動更深入的這些 Azure 服務或讀取多個文件的管理的 Azure 入口網站。 

![螢幕擷取畫面的 Windows Admin Center 顯示在伺服器已安裝的 Azure 服務](../media/azure-services/ahs-dayN.png)

您可以從 Azure 的混合式服務工具：
- 備份您的 Windows Server 中使用的 Windows Admin Center [Azure 備份](azure-backup.md)
- 保護 HYPER-V 虛擬機器從使用的 Windows Admin Center [Azure Site Recovery](azure-site-recovery.md)
- 同步處理您的檔案伺服器與雲端中，使用[Azure 檔案同步](azure-file-sync.md)
- 管理作業系統更新針對所有 Windows 伺服器，同時對內部部署或雲端中，使用[Azure 更新管理](azure-update-management.md)
- 監視伺服器，同時對內部部署或雲端，並設定警示與[Azure 監視器](azure-monitor.md)
- 您的內部部署伺服器連接到 Azure 虛擬網路與[Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)

## <a name="services-for-stand-alone-servers-and-vms"></a>兩部獨立伺服器和 Vm 的服務

這是提供給兩部獨立伺服器和虛擬機器功能的 Azure 服務的完整清單：

- **（新）使用同步您的檔案伺服器與雲端[Azure 檔案同步](https://aka.ms/afs)**  
透過 Azure 檔案共用這部伺服器上的同步處理檔案。 保留所有檔案的本機或使用雲端階層處理和快取只有最常使用的檔案在伺服器上，冷資料分層至雲端。 在雲端中的資料可以備份，而不必擔心內部部署伺服器備份。 此外，多站台同步可讓一組檔案保持同步至多部伺服器。

- **加入 Windows Admin Center 中的一層的安全性，藉由新增[Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)驗證**  
您可以加入額外安全性的 Windows Admin Center 藉由要求使用者使用 Azure Active Directory (Azure AD) 身分識別存取閘道進行驗證。 Azure AD 驗證也可讓您充分利用 Azure AD 的安全性功能，例如條件式存取和 multi-factor authentication。  
如需詳細資訊，請參閱[Windows Admin Center 的設定 Azure AD 驗證。](../configure/user-access-control.md#azure-active-directory)  

- **保護 HYPER-V 虛擬機器與[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
您可以複寫在 Vm 上執行，以便在發生災害時保護您業務關鍵的基礎結構，而工作負載。 Windows Admin Center 簡化設定和複寫您的 HYPER-V 伺服器或叢集上的虛擬機器的程序，方便您以增強您的環境與 Azure Site Recovery 的災害復原服務復原功能。  
如需詳細資訊，請參閱 <<c0> [ 保護您的 Vm 使用 Azure Site Recovery 和 Windows Admin Center](azure-site-recovery.md)。

- **備份您的 Windows 伺服器[Azure 備份](https://docs.microsoft.com/azure/backup/backup-overview)**  
您可以備份至 Azure，協助保護您免於意外或惡意刪除、 損毀及勒索軟體的 Windows 伺服器。  
如需詳細資訊，請參閱 <<c0> [ 備份使用 Azure 備份伺服器](azure-backup.md)。

- **監視及取得您的環境中的所有伺服器的電子郵件警示[虛擬機器的 Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
您可以使用 Azure 監視器，也就是虛擬機器深入解析，監視伺服器健全狀況和事件、 建立電子郵件警示，在您的環境中，取得伺服器效能的彙總的檢視和視覺化應用程式，系統和服務連線到指定伺服器。  
如需詳細資訊，請參閱 <<c0> [ 將伺服器連接至 Azure 監視器及設定電子郵件通知](azure-monitor.md)。

- **集中管理與您所有的 Windows 伺服器的作業系統更新[Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)**  
從單一位置，而不是在每個伺服器上，您可以管理更新和修補多個伺服器和 Vm。 透過 Azure 更新管理，可以快速評估可用更新的狀態、 排定何時安裝必要的更新，並檢閱部署結果來確認已成功套用的更新。 這是可能您的伺服器是否在其他雲端提供者，所裝載的 Azure Vm，或在內部部署環境。  
如需詳細資訊，請參閱 <<c0> [ 針對以 Azure 更新管理設定伺服器](azure-update-management.md)。

- **您的內部部署伺服器連接到 Azure 虛擬網路與[Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)**  
您可以加入您的內部部署伺服器，以協助您安全地連線到 Azure 虛擬網路的 伺服器的 Azure 網路介面卡。  
如需詳細資訊，請參閱 <<c0> [ 設定點對站 VPN 連線的內部部署 Windows Server 和 Azure 虛擬網路之間](https://aka.ms/WACNetworkAdapter)。

- **管理 Azure IaaS 虛擬機器與[Windows Admin Center](manage-azure-vms.md)**  
您可以使用 Windows Admin Center 來管理您的 Azure Vm 以及內部部署機器。 藉由設定您的 Windows Admin Center 閘道連線至 Azure VNet，您可以管理虛擬機器，在 Azure 中使用 Windows Admin Center 提供一致且簡化工具。 如需詳細資訊，請參閱 <<c0> [ 來管理 Vm 在 Azure 中的設定 Windows Admin Center](manage-azure-vms.md)。

## <a name="services-for-clusters"></a>叢集服務

這些是提供整體叢集功能的 Azure 服務 （這些不所有整合至 Windows Admin Center 尚未）：

- [使用 Azure 監視器中監視的超交集叢集](../../../storage/storage-spaces/configure-azure-monitor.md)
- [保護您的 Vm 使用 Azure Site Recovery](azure-site-recovery.md)
- [部署叢集的雲端見證](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="see-also"></a>請參閱

- [連接到 Azure 的 Windows Admin Center](azure-integration.md)
- [部署在 Azure 中的 Windows Admin Center](deploy-wac-in-azure.md)