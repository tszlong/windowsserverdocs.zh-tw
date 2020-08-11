---
title: 什麼是 Windows Admin Center
description: 什麼是 Windows Admin Center (Project Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 3075c571fb933692745a872df138ef5cbfa6b283
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995307"
---
# <a name="what-is-windows-admin-center"></a>什麼是 Windows Admin Center？

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows Admin Center 是在本機部署以瀏覽器為基礎的全新管理工具組，可讓您在沒有任何 Azure 或雲端相依性的情況下管理 Windows Server。 Windows Admin Center 可讓您完整控制伺服器基礎結構的各個層面，這在未連線至網際網路的私人網路上管理伺服器特別有用。

Windows Admin Center 是「隨附」管理工具 (例如伺服器管理員和 MMC) 的現代化演進。 與 System Center 相輔相成，而非加以取代。

![與其他解決方案搭配運作的 Windows Admin Center 圖表](../media/wac-complements.png)

## <a name="how-does-windows-admin-center-work"></a>Windows Admin Center 如何運作？

Windows Admin Center 在網頁瀏覽器中執行，並透過 Windows Server 或加入網域的 Windows 10 上安裝的 **Windows Admin Center 閘道**來管理 Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10、Azure Stack HCI 等系統。 閘道使用遠端 PowerShell 和透過 WinRM 的 WMI 來管理伺服器。 閘道以單一輕量型 .msi 套件 (您可以[下載](https://aka.ms/windowsadmincenter)) 隨附於 Windows Admin Center。

將 Windows Admin Center 閘道發佈至 DNS 並授與通過對應公司防火牆的存取權時，此閘道可讓您隨處透過 Microsoft Edge 或 Google Chrome 安全地連線至伺服器並進行管理。

![Windows Admin Center 架構圖表](../media/architecture.png)

## <a name="learn-how-windows-admin-center-improves-your-management-environment"></a>了解 Windows Admin Center 如何改善您的管理環境

### <a name="familiar-functionality"></a>**熟悉的功能**

Windows Admin Center 是如 Microsoft Management Console (MMC) 這類知名管理平台的長期演進，其徹底以現今建構和管理系統的方式建置而成。 Windows Admin Center 包含許多您目前用於管理 Windows 伺服器及用戶端的熟悉工具。

### <a name="easy-to-install-and-use"></a>**易於安裝和使用**

[安裝](../deploy/install.md)在 Windows 10 電腦上，幾分鐘即可開始管理，或是當作閘道安裝在 Windows 2016 Server 上，即可讓整個組織從網頁瀏覽器來管理電腦。

### <a name="complements-existing-solutions"></a>**與現有解決方案互補**

Windows Admin Center 可與 System Center 以及 Azure 管理和安全性等解決方案搭配使用，增進這些解決方案的功能來執行繁雜瑣碎的單一電腦管理工作。

### <a name="manage-from-anywhere"></a>**隨處管理**

將您的 Windows Admin Center 閘道伺服器發佈到公用網際網路，即可完全以安全的方式從任何地方連線至伺服器並進行管理。

### <a name="enhanced-security-for-your-management-platform"></a>**增強管理平台的安全性**

Windows Admin Center 有許多增強功能可讓您管理平台[更加安全](../plan/user-access-options.md)。 角色型存取控制可讓您微調哪些系統管理員可以存取哪些管理功能。 閘道驗證選項包括本機群組、本機網域型 Active Directory 和雲端式 Azure Active Directory。  此外，還能[深入了解](../use/logging.md)您環境中執行的管理動作。

### <a name="azure-integration"></a>**Azure 整合**

Windows Admin Center 有許多[與 Azure 服務整合](../azure/index.md)之處，包括 Azure Active Directory、Azure 備份、Azure Site Recovery 等功能。

### <a name="deploy-hyper-converged-and-failover-clusters"></a>**部署超融合式和容錯移轉叢集**

Windows Admin Center 可讓您透過簡單易用的精靈，[順暢地部署超融合式和容錯移轉叢集](../use/deploy-hyperconverged-infrastructure.md)。

### <a name="manage-hyper-converged-clusters"></a>**管理超融合式叢集**

Windows Admin Center 提供[管理超融合式叢集](../use/manage-hyper-converged.md)的最佳體驗，包括將計算、儲存體和網路元件虛擬化。

### <a name="extensibility"></a>**擴充性**

Windows Admin Center 從一開始即是以擴充性為考量而建置，讓 Microsoft 和協力廠商開發人員有能建置超越目前供應項目的工具和解決方案。 Microsoft 提供可讓開發人員針對 Windows Admin Center 自行建置其工具的 [SDK](../extend/extensibility-overview.md)。

> [!Tip]
> 準備好安裝 Windows Admin Center 了嗎？ [立即下載](https://aka.ms/windowsadmincenter)