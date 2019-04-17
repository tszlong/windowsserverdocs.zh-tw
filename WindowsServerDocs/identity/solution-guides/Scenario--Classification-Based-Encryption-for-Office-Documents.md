---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: "Office 文件案例分類型加密"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 38e058f36522ba6a2c81694cb883d0946b04adda
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>Office 文件案例：分類型加密

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

保護的機密資訊是主要緩和組織的風險。 健康保證移植性責任動作 (HIPAA) 及付款卡片 Industry 資料安全標準 (PCI-DSS)，例如不同 compliance 法規聽寫加密的詳細資訊，並有許多企業原因加密機密資訊。 不過，資訊加密很高，或其可能會影響生產力。 因此，組織通常會有不同的方式與加密其資訊的優先順序。  
  
## <a name="BKMK_OVER"></a>案例描述  
 Windows Server 2012 提供會自動加密機密 Microsoft Office 檔案，根據其分類的能力。 這是透過叫用 Active Directory Rights Management Services (AD RMS) 保護的機密文件幾秒後檔案被視為敏感的檔案，檔案伺服器上的檔案管理工作。 這可協助來連續檔案管理工作檔案伺服器上。  
  
AD RMS 加密提供一層保護的檔案。 即使存取敏感檔案的人員不小心傳送電子郵件透過該檔案，檔案受 AD RMS 加密。 想要存取檔案，使用者必須先驗證本身接收加密金鑰 AD RMS 伺服器。 下圖顯示此程序。  
  
![方案指南](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**圖 6**架構分類 RMS 保護  
  
非 Microsoft 廠商透過使用非 Microsoft 的檔案格式的支援。 AD RMS 加密的受保護檔案後之後，已不再提供該檔案的資料例如搜尋或 content 為基礎分類的管理功能。  
  
## <a name="in-this-scenario"></a>本案例中  
以下是本案例可指導方針：  
  
-   [規劃考量加密的 Office 文件](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [部署加密的 Office 檔案和 #40; 示範步驟和 #41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>角色與包含在本案例中的功能  
下表列出的角色與本案例的功能，並告訴他們支援的方式。  
  
|角色/功能|它如何支援此案例|  
|-----------------|---------------------------------|  
|Active Directory Domain Services 角色 (AD DS)|AD DS 提供分散式的資料庫來儲存及管理網路資源和應用程式特定資料的相關資訊從 directory 功能的應用程式。 在本案例中，Windows Server 2012 中的 AD DS 引進了可讓使用者宣告裝置宣告、複合身分（使用者加上裝置宣告），新的中央存取原則模型，並在 [授權決策檔案分類資訊的使用建立宣告為基礎的授權平台。|  
|檔案與儲存空間服務的角色<br /><br />檔案伺服器資源管理員|檔案與儲存空間服務提供技術，以協助您設定及管理您的網路位置您可以將檔案儲存並分享的使用者提供中央位置的一或多個檔案伺服器。 若您的網路使用者的存取權的相同的檔案和應用程式，或如果備份與檔案的集中的管理您的組織重要，您應該設定一或多部電腦做為檔案伺服器來將檔案與儲存空間服務角色與適當的角色服務新增到電腦。 在本案例中，檔案伺服器的系統管理員可以設定叫用 AD RMS 保護的機密文件幾秒後檔案被視為敏感的檔案，檔案伺服器（連續檔案管理工作檔案伺服器上）上的檔案管理工作。|  
|Active Directory Rights Management Services (AD RMS) 角色|AD RMS 讓個人和系統管理員（透過資訊 Rights Management (IRM) 原則），指定的文件、活頁簿及簡報的存取權限。 這可協助防止機密資訊列印、轉送，或複製未經授權的人員。 使用 IRM 受到檔案的使用權限之後，存取和使用量限制無論資訊的地方，因為本身文件檔案中儲存檔案的使用權限。 在本案例中，AD RMS 加密提供一層保護的檔案。 即使存取敏感檔案的人員不小心傳送電子郵件透過該檔案，檔案受 AD RMS 加密。 想要存取檔案，使用者必須先驗證本身接收加密金鑰 AD RMS 伺服器。|  
  


