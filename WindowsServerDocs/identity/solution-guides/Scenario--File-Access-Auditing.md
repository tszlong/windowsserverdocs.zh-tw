---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: "案例檔案存取稽核"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 93d78bbefce38173198f991543fb3a06d145b373
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-file-access-auditing"></a>案例：檔案存取稽核

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

安全性稽核是一種可協助維護企業的安全性最有力的工具。 安全性稽核的主要目標是法規。 例如沙法案、健康保證移植性責任動作 (HIPAA)，並付款卡片 Industry (PCI) 業界標準需要遵循嚴格組規則的相關資料的安全性和隱私權的企業。 安全性稽核協助證明這些標準的相容性，並將該名的此類原則。 此外，安全性稽核協助偵測異常行為，找出並減少縫隙中的安全性原則，並建立的使用者活動，可用於法庭分析古道阻止 irresponsible 行為。  
  
稽核的原則下列層級皆通常是需求：  
  
-   **資訊的安全。** 法庭分析及入侵偵測常用可以沿著走存取稽核的檔案。 正在取得目標的活動的相關資訊高價值的存取權，讓我們組織大幅改善他們回應時間和調查準確度。  
  
-   **組織的原則。** 例如，PCI 標準，管理組織可能有監視標示為信用卡資訊，以及個人資訊 (PII) 包含的所有檔案的存取權中央原則。  
  
-   **部門原則。** 例如的修改（例如季獲利報告）特定財經文件限於財經部門，因此部門想要監視所有其他變更嘗試這些文件，可能需要財務部門。  
  
-   **企業的原則。** 例如，企業擁有者可能要監視所有未經授權的檢視屬於專案資料嘗試。  
  
此外，compliance 部門可能會想要監視中央授權原則和原則建構，例如使用者、電腦及資源屬性的所有變更。  
  
其中一個最大的安全性稽核考量會收集、儲存和分析稽核活動的費用。 如果稽核原則太大，彈稽核收集到的事件磁碟區，和這增加成本。 如果稽核原則不太小，您可能會遺失重要活動。  
  
與 Windows Server 2012，您可以使用宣告和資源屬性撰寫稽核原則。 這會導致更豐富、更目標，以及變得更容易管理稽核原則。 它可以讓案例，到目前為止，不可能或太難執行。 系統管理員可以撰寫稽核原則的範例如下：  
  
-   稽核不具有高安全性的距離，嘗試存取 HBI 文件的人。 例如，稽核 |每個人都 |存取所有 |Resource.BusinessImpact=HBI 和 User.SecurityClearance!=High。  
  
-   稽核所有廠商嘗試存取專案，無法運作的相關的文件。 例如，稽核 |每個人都 |存取所有 |User.EmploymentStatus=Vendor 和 User.Project Not_AnyOf Resource.Project。  
  
這些原則協助管理稽核事件音量和限於只最相關的資料或使用者。  
  
已建立系統管理員，並套用稽核原則之後，他們的下一步考量有些有意義的稽核事件它們收集的資訊。 事件運算式型稽核協助降低稽核的音量。 不過，使用者必須方式查詢這些事件有意義的資訊，並提出問題，例如，「人員存取我 HBI 的資料？」 或者「已有未經授權的嘗試存取敏感的資料？」  
  
 Windows Server 2012 美化使用者、電腦及資源宣告的現有資料存取事件。 下列事件專各伺服器上。 跨組織提供完整的事件檢視，Microsoft 會與協力廠商提供的事件收集與分析工具，例如稽核收集服務 System Center 作業 Manager 中運作。  
  
圖 4 顯示中央稽核原則的概觀。  
  
![方案指南](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**圖 4**稽核中央的體驗  
  
設定並使用安全性稽核通常會包含一般的下列步驟：  
  
1.  找出正確的資料和使用者監控一組  
  
2.  建立和套用適當稽核原則  
  
3.  會收集和分析稽核事件  
  
4.  管理及監視所建立的原則  
  
## <a name="in-this-scenario"></a>本案例中  
下列主題會提供額外的指導方針本案例：  
  
-   [檔案計劃存取稽核](Plan-for-File-Access-Auditing.md)  
  
-   [部署安全性稽核中央稽核原則和 #40; 示範步驟和 #41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="BKMK_NEW"></a>角色與包含在本案例中的功能  
下表列出的角色與本案例的功能，並告訴他們支援的方式。  
  
|角色/功能|它如何支援此案例|  
|-----------------|---------------------------------|  
|Active Directory 網域 Services 角色|Windows Server 2012 中的 AD DS 導入宣告為基礎的授權平台，可以讓使用者宣告和裝置宣告、複合的身分、（使用者加上裝置宣告），建立新中央存取原則（端點）模式，以及授權決策檔案分類資訊的使用。|  
|檔案與儲存空間服務的角色|Windows Server 2012 中的檔案伺服器提供其中系統管理員可以檢視有效的權限使用者的檔案或資料夾的存取問題的疑難排解並權限授與所需的使用者介面。|  
  


