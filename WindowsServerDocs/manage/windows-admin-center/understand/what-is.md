---
title: 什麼是 Windows Admin Center
description: 什麼是 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d374704768d06aaeb7ee6bc9c984cc78d49cb4b0
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080905"
---
# 什麼是 Windows Admin Center？

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows Admin Center 是在本機部署以瀏覽器為基礎的全新管理工具組，可讓您在沒有任何 Azure 或雲端相依性的情況下管理 Windows Server。 Windows Admin Center 可讓您完整控制伺服器基礎結構的各個層面，這在未連線至網際網路的私人網路上管理伺服器特別有用。

Windows Admin Center 是「隨附」管理工具 (例如伺服器管理員和 MMC) 的現代化演進。 它補充 System Center-這並不是用來取代。

![](../media/wac-complements.png)

## Windows Admin Center 如何運作？

Windows Admin Center 在網頁瀏覽器中執行，並透過 Windows Server 2016 和 Windows 10 上安裝的 **Windows Admin Center 閘道**管理 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows 10 等系統。 閘道透過遠端 PowerShell 和透過 WinRM 的 WMI 來管理伺服器。 閘道以單一輕量型 .msi 套件 (您可以[下載](https://aka.ms/windowsadmincenter)) 隨附於 Windows Admin Center。

將 Windows Admin Center 閘道發佈至 DNS 並透過對應的公司防火牆授與存取權時，此閘道可讓您隨處透過 Microsoft Edge 或 Google Chrome 安全地連接和管理您的伺服器。

![](../media/architecture.png)

## 了解 Windows Admin Center 如何改善您的管理環境

### **熟悉的功能**

Windows Admin Center 是像 Microsoft Management Console (MMC) 這樣眾所周知管理平台由來已久的演進，從頭到尾完全依循現今建構和管理系統的方式建置而成。 Windows Admin Center 包含許多您目前用於管理 Windows 伺服器及用戶端的熟悉工具。

### **易於安裝和使用**

[安裝](../deploy/install.md)在 Windows 10 電腦上，幾分鐘即可開始管理，或是當做閘道安裝在 Windows 2016 Server，即可讓您整個組織從網頁瀏覽器管理電腦。

### **與現有解決方案互補** 

Windows Admin Center 搭配解決方案，例如 System Center 及 Azure 管理和安全性，功能來執行繁雜瑣碎的單一電腦管理工作。

### **隨處管理**

將您的 Windows Admin Center 閘道伺服器發佈到公用網際網路，即可完全以安全的方式從任何地方連線至伺服器並進行管理。

### **增強管理平台的安全性**

Windows Admin Center 有許多增強功能可讓您[更安全](../plan/user-access-options.md)管理平台。 角色型存取控制可讓您微調哪些系統管理員可以存取哪些管理功能。 閘道驗證選項包括本機群組、本機網域型 Active Directory 和雲端式 Azure Active Directory。  此外，還能[深入了解](../use/logging.md)您環境中執行的管理動作。

### **Azure 整合**

Windows Admin Center 有許多點[與 Azure 服務整合](../plan/azure-integration-options.md)，包括 Azure Active Directory、 Azure 備份、 Azure Site Recovery 等。

### **管理超融合式叢集**

Windows Admin Center 提供[管理超融合式叢集](../use/manage-hyper-converged.md)的最佳體驗，包括虛擬化電腦、儲存空間和網路元件。

### **擴充性**

Windows Admin Center 從一開始即是以擴充性為考量而建置，讓 Microsoft 和協力廠商開發人員有能建置超越目前供應項目的工具和解決方案 Microsoft 提供可讓開發人員針對 Windows Admin Center 自行建立其工具的 [SDK](../extend/extensibility-overview.md)。

> [!Tip]
> 準備好安裝 Windows Admin Center 了嗎？ [立即下載](https://aka.ms/windowsadmincenter)
