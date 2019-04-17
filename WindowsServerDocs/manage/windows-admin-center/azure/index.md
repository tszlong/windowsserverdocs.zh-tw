---
title: 連線到 Azure 混合式服務的 Windows Server
description: 您可以延伸至雲端的 Windows Server 的內部部署使用 Azure 混合式服務。
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: 625bd4b79d277dfaa81767cd781c2ba1316d637e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296935"
---
# 連線到 Azure 混合式服務的 Windows Server

>適用於： Windows Server 2019、 Windows Server 2016

您可以延伸至雲端的 Windows Server 的內部部署使用 Azure 混合式服務。 這些雲端服務提供實用功能，包括下列的陣列：

- 保護虛擬機器，並使用雲端式備份及災害復原 (DR HA /) 使用 Azure Site Recovery。 
- 追蹤您的應用程式、 網路及基礎結構的進階的分析和機器學習 Azure 監視器協助間發生了什麼。 
- 簡化網路連線至 Azure，Azure 網路介面卡。
- 保持在最新狀態使用 Azure 更新管理虛擬機器。

混合式 azure 服務搭配 Windows Server 中，下列設定：

- 獨立的實體伺服器和虛擬機器 (Vm)
- 叢集，包括[Azure 堆疊 HCI](../../../azure-stack-hci/index.md)和[Windows server 軟體定義 (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)程式經過認證的超融合式叢集

雖然您可以設定使用 Azure 入口網站及下載或兩個最 Azure 混合式服務，許多已整合直接將 Windows Admin Center 提供簡化的安裝體驗和服務的伺服器為主檢視。

## 混合式 azure 服務工具

[Windows Admin Center](../understand/windows-admin-center.md)中的 「 Azure hybrid 服務工具將所有整合式的 Azure 服務彙總至集中式中樞可以輕鬆地探索所有可用 Azure 服務將值帶至您的內部部署或混合式環境。 

![螢幕擷取畫面的 Windows Admin Center 顯示 Azure 混合式服務工具](../media/azure-services/ahs-discover.png)

如果連線到伺服器與 Azure 服務已啟用，混合式 Azure 服務工具做為單一窗格以查看該伺服器上的所有已啟用的服務窗口。 可以輕鬆地前往 Windows Admin Center 中的相關工具、 您手邊啟動更深入的管理這些 Azure 服務或閱讀更多與文件的 Azure 入口網站。 

![螢幕擷取畫面的 Windows Admin Center 顯示已安裝在伺服器上的 Azure 服務](../media/azure-services/ahs-dayN.png)

從 Azure 混合式服務工具，您可以：
- 備份您的 Windows Server 從[Azure 備份](azure-backup.md)與 Windows Admin Center
- 從 Windows Admin Center，使用[Azure Site Recovery](azure-site-recovery.md)保護您的 HYPER-V 虛擬機器
- 同步您的檔案伺服器與雲端，使用[Azure 檔案同步](azure-file-sync.md)
- 管理作業系統更新對於所有您的 Windows 伺服器，同時在內部或在雲端，使用[Azure 更新管理](azure-update-management.md)
- 監視伺服器，同時在內部或在雲端，並以[Azure 監視器](azure-monitor.md)設定警示
- 將您的內部部署伺服器連接至具有[Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)的 Azure 虛擬網路

## 適用於獨立伺服器與 Vm 的服務

這是提供功能給獨立伺服器和 Vm 的 Azure 服務的完整清單：

- **（新）使用[Azure 檔案同步](https://aka.ms/afs)同步您的檔案伺服器與雲端**  
使用 Azure 檔案共用此伺服器上的檔案同步。 保留您的所有檔案的本機或使用 tiering 並且快取僅限雲端最常使用的伺服器，tiering 非經常存取的資料，以在雲端上的檔案。 在雲端中的資料可以備份，而無需擔心內部伺服器備份。 此外，多台同步能將一組檔案同步跨多部伺服器。

- **加入 Windows Admin Center 藉由加入[Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)驗證的安全性層級**  
您可以新增到 Windows Admin Center 的額外的安全性層級，要求使用者進行驗證來存取閘道使用 Azure Active Directory (Azure AD) 的身分識別。 Azure AD 驗證也可讓您充分利用 Azure AD 的安全性功能，例如條件式存取及多重要素驗證。  
如需詳細資訊，請參閱[設定 Azure AD 驗證 Windows Admin center。](../configure/user-access-control.md#azure-active-directory)  

- **保護您的 HYPER-V 虛擬機器使用[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
您可以複寫 Vm 上執行，這樣您的業務關鍵基礎結構發生嚴重損壞時受到保護的工作負載。 Windows Admin Center 簡化安裝與複寫在您的 HYPER-V 伺服器或叢集，虛擬機器的程序，讓您更容易來增強您的環境使用 Azure Site Recovery 的災害復原服務的復原功能。  
如需詳細資訊，請參閱[保護您的 Vm 與 Azure Site Recovery 和 Windows Admin Center](azure-site-recovery.md)。

- **備份您的 Windows 伺服器與[Azure 備份](https://docs.microsoft.com/azure/backup/backup-overview)**  
您可以備份您的 Windows 伺服器至 Azure，協助保護您免於意外或惡意的刪除、 損毀，以及勒索軟體。  
如需詳細資訊，請參閱[備份您的伺服器與 Azure 備份](azure-backup.md)。

- **監視並取得您的環境使用[Azure 監視器的虛擬機器](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)中的所有伺服器的電子郵件警示**  
您可以使用 Azure 監視器，也稱為虛擬機器見解，來監視伺服器健康情況和事件、 建立電子郵件警示、 您的環境間取得空間以提供統一的伺服器效能檢視和視覺化應用程式，系統，而且服務連接到特定伺服器。  
如需詳細資訊，請參閱[連線到 Azure 監視您的伺服器，並設定電子郵件通知](azure-monitor.md)。

- **使用[Azure 更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)您所有的 Windows 伺服器集中管理作業系統更新**  
您可以從單一位置，而不是以個別伺服器為基礎管理更新和適用於多個伺服器與 Vm 修補程式。 使用 Azure 更新管理，您可以快速地評估可用的更新的狀態、 排程安裝所需的更新，以及檢閱部署結果，以確認已成功套用更新。 這是可能您的伺服器是否為 Azure Vm，裝載由其他雲端提供者，或在內部。  
如需詳細資訊，請參閱[Azure 更新管理設定伺服器](azure-update-management.md)。

- **將您的內部部署伺服器連接至具有[Azure 網路介面卡](https://aka.ms/WACNetworkAdapter)的 Azure 虛擬網路**  
您可以將 Azure 網路介面卡新增到您的內部部署伺服器，可協助您安全地將伺服器連線到 Azure 虛擬網路。  
如需詳細資訊，請參閱[內部部署 Windows Server 和 Azure 虛擬網路之間設定點對站 VPN 連線](https://aka.ms/WACNetworkAdapter)。

- **管理 Azure IaaS 虛擬機器使用[Windows Admin Center](manage-azure-vms.md)**  
您可以使用 Windows Admin Center 來管理您的 Azure Vm 以及內部部署電腦。 藉由設定您的 Windows Admin Center 閘道連線至您的 Azure VNet，您可以管理 Azure 使用 Windows Admin Center 提供一致、 簡化工具中的虛擬機器。 如需詳細資訊，請參閱[設定 Windows Admin Center 來管理 Vm 在 Azure 中](manage-azure-vms.md)。

## 叢集的服務

這些是提供功能給叢集整體的 Azure 服務 （這些不所有已整合至 Windows Admin Center 尚未）：

- [與 Azure 監視器，監視超融合式叢集](../../../storage/storage-spaces/configure-azure-monitor.md)
- [使用 Azure Site Recovery 保護您 Vm](azure-site-recovery.md)
- [部署叢集的雲端見證](../../../failover-clustering/deploy-cloud-witness.md)

## 也請參閱

- [Windows Admin Center 連線至 Azure](azure-integration.md)
- [部署在 Azure 中的 Windows Admin Center](deploy-wac-in-azure.md)