---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: Office 檔的案例分類為基礎的加密
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: fa6de270d7d6acf4bf7c99f9a5f9f9457b80b55d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861131"
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>Scenario: Classification-Based Encryption for Office Documents

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

保護敏感資訊主要是為組織免除風險。 各種標準規定 (如健康保險流通與責任法案 (Health Insurance Portability and Accountability Act，HIPAA) 或付款卡產業資料安全標準 (Payment Card Industry Data Security Standard，PCI-DSS)) 要求加密資訊，而且還有各種業務方面的原因需要加密敏感的商業資訊。 不過，加密資訊所費不貲，也可能會削弱企業生產力。 正因如此，所以組織傾向用不同的方式及優先順序來加密他們的資訊。  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>案例描述  
 Windows Server 2012 可讓您根據機密 Microsoft Office 檔案的分類來自動加密這些檔案。 它的實現方式是透過檔案管理工作，當檔案伺服器上將檔案識別為敏感檔案之後的數秒內，就為這些敏感文件叫用 Active Directory Rights Management Services (AD RMS) 保護。 這是透過在檔案伺服器上連續執行檔案管理工作來實現。  
  
AD RMS 加密為檔案提供另一層保護。 即使擁有機密檔案存取權的人不小心透過電子郵件傳送該檔案時，它仍然受到 AD RMS 加密的保護。 想要存取檔案的使用者，必須先向 AD RMS 伺服器驗證，才能夠收到解密金鑰。 下圖顯示這個程序。  
  
![解決方案指南](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**圖 6** 以分類為基礎的 RMS 保護  
  
非 Microsoft 檔案格式的支援可透過非 Microsoft 廠商取得。 檔案經過 AD RMS 加密的保護之後，資料管理功能 (如以搜尋或內容為基礎的分類) 就不再適用於該檔案。  
  
## <a name="in-this-scenario"></a>在這個案例中  
以下是可供此案例使用的指導方針：  
  
-   [Office 檔加密的規劃考慮](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [部署 Office 檔案加密&#40;示範步驟&#41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [動態存取控制：案例總覽](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>此案例中包含的角色和功能  
下表列出這個案例中的角色與功能，並說明它們如何支援這個案例。  
  
|角色/功能|如何支援本案例|  
|-----------------|---------------------------------|  
|Active Directory 網域服務角色 (AD DS)|AD DS 提供一個分散式資料庫，用來儲存和管理網路資源的相關資訊，以及啟用目錄之應用程式的應用程式特定資料。 在此案例中，Windows Server 2012 中的 AD DS 引進了以宣告為基礎的授權平臺，可讓您建立使用者宣告和裝置宣告、複合身分識別（使用者加上裝置宣告）、新的集中存取原則模型，以及在授權決策中使用檔案分類資訊。|  
|檔案和存放服務角色<p>File Server Resource Manager|檔案和存放服務提供的技術可幫助您設定和管理一或多個檔案伺服器，這些伺服器會在網路上提供儲存檔案以及讓使用者共用檔案的中心位置。 如果您的網路使用者需要存取相同的檔案和應用程式，或集中式的備份與檔案管理對您的組織來說很重要，則應透過新增檔案和存放服務角色與適當的角色服務，將一或多個電腦設成檔案伺服器。 在此案例中，檔案伺服器系統管理員可以設定檔案管理工作，當檔案伺服器上的檔案被識別為機密檔案之後的幾秒內，叫用 AD RMS 保護機密文件 (在伺服器上持續的檔案管理工作)。|  
|Active Directory Rights Management Services (AD RMS) 角色|AD RMS 可以讓個人與系統管理員透過資訊版權管理 (IRM) 原則指定文件、活頁簿及簡報的存取權限。 這有助於防止未經授權的使用者列印、轉寄或複製機密資訊。 使用 IRM 限制檔案的使用權限後，無論資訊位於何處，都會強制執行存取權與使用限制，因為檔案的使用權限是儲存在文件檔案本身。 在此案例中，AD RMS 加密為檔案提供另一層保護。 即使擁有機密檔案存取權的人不小心透過電子郵件傳送該檔案時，它仍然受到 AD RMS 加密的保護。 想要存取檔案的使用者，必須先向 AD RMS 伺服器驗證，才能夠收到解密金鑰。|  
  


