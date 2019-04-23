---
title: Windows Admin Center 相關的管理解決方案
description: Windows Admin Center 與比較的方式，並補充其他 Microsoft 監視和管理解決方案/產品 （專案檀香山）
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 385a066cb828f58d698c2ca47e0553e996a77733
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847319"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center 和 Microsoft 相關的管理解決方案

>適用於：Windows Admin Center，Windows Admin Center 預覽

[Windows Admin Center](windows-admin-center.md)是傳統的內建伺服器的進化的情況下，您可能使用遠端桌面 (RDP) 來連接到疑難排解或組態伺服器管理工具。 它具有不是要取代其他現有的 Microsoft 管理解決方案;而是可補充這些解決方案中，如下所述。

## <a name="remote-server-administration-tools-rsat"></a>遠端伺服器管理工具 (RSAT)

[遠端伺服器管理工具 (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)是 GUI 和 PowerShell 來管理 Windows Server 中選用的角色和功能工具的集合。 RSAT 有許多 Windows Admin Center 沒有的功能。 我們可以新增的一些最常用的工具在 RSAT 中 Windows Admin Center 未來。 任何新的 Windows 伺服器角色或功能需要管理 GUI 會進入 Windows Admin Center。

## <a name="intune"></a>Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune)是雲端式企業行動管理服務，可讓您管理 iOS、 Android、 Windows、 和 macOS 裝置，根據一組原則。 Intune 會著重於讓您藉由控制您的工作人員存取並共用資訊的方式來保護公司資訊。 相反地，Windows Admin Center 不是以原則為導向，但可讓您的 Windows 10 和 Windows Server 系統的特定管理透過 WinRM 使用遠端 PowerShell 和 WMI。

## <a name="azure-stack"></a>Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/)是混合式雲端平台，可讓您從您的資料中心提供 Azure 服務。 使用 PowerShell 或系統管理員入口網站中，類似於用來存取和管理傳統的 Azure 服務的傳統 Azure 入口網站管理 azure Stack。 Windows Admin Center 不想要管理 Azure Stack 基礎結構，但您可以使用它來[管理 Azure IaaS 虛擬機器](../configure/manage-azure-vms.md)（執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012） 或進行疑難排解在 Azure Stack 環境中部署個別的實體伺服器。

## <a name="system-center"></a>System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center)是內部部署資料中心管理 」 解決方案，來部署、 設定、 管理、 監視您的整個資料中心。 System Center 可讓您查看您的環境中的所有系統的狀態，而 Windows Admin Center 可讓您向下切入至特定的伺服器，以管理或使用更精細的工具進行疑難排解。

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **「 箱 」 平台和工具，重新發想** | **資料中心管理與監視** |
| Windows Server 授權 – 隨附**無需額外費用**，如同 MMC 與其他傳統的內建工具 | **完整**跨您的環境與平台的附加價值的解決方案套件 |
| **輕量型**，瀏覽器為基礎的 Windows 伺服器執行個體的遠端管理**隨處**; RDP 的其他選項 | 管理與監視**異質**systems**大規模**，包括 HYPER-V、 VMware 和 Linux |
|**深層**單一伺服器與單一叢集向下鑽研以進行疑難排解，組態與維護|基礎結構佈建;自動化與自助式; 基礎結構和工作負載監視**廣度**|
|最佳化的管理**個別**2 – 4 節點**HCI**整合 HYPER-V、 儲存空間直接存取和 SDN 的叢集|部署和管理 HYPER-V、 Windows Server 叢集**資料中心擴展**從**裸機**與 SCVMM|
|**監視對 HCI**只; 叢集健全狀況服務會儲存歷程記錄。 可擴充的平台，第 1 個和第 3 個合作對象**系統管理工具延伸模組**|**可延伸** & **可擴充監視**在 SCOM 中，使用警示、 通知、 第三方監視; 的工作負載的平台SQL 歷程記錄|
|最簡單的橋接器**混合式**; 上架，並使用各種 Azure 服務的資料保護、 複寫、 更新和更多功能|**內建**資料保護、 複寫、 更新 (DPM/VMM/SCCM)。 使用 Log Analytics 與服務對應的混合式整合|
|**平台功能會點亮**的 Windows Server:儲存體移轉服務，儲存體複本系統深入解析，依此類推。|**其他平台**:Orchestrator/SMA 中的自動化。SCSM 與其他服務管理工具的整合|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>每個獨立; 提供目標的值**相得益彰**互補的功能。
