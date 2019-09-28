---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: 案例檔案存取審核
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 37a3b17360112d958b59a7e9c3f64aed5e6f6a5b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406991"
---
# <a name="scenario-file-access-auditing"></a>案例：檔案存取稽核

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

安全性稽核是可以協助維護企業安全最有力的工具之一。 安全性稽核的其中一個關鍵目標就是法規相符性。 沙氏法案、健康保險流通與責任法案 (HIPAA) 以及付款卡產業 (PCI)) 等業界標準都要求企業遵循一套與資料安全性和隱私權相關的嚴格規定。 安全性稽核可以協助確定您的企業已經設定這些原則，並檢驗您的企業符合這些標準。 此外，安全性稽核還可以協助偵測異常行為、發現和補強安全性原則漏洞，以及透過建立使用者活動記錄 (可以用在資料安全分析上) 遏止不負責任的行為。  
  
稽核原則需求通常是在以下層級產生：  
  
-   **資訊安全性。** 檔案存取稽核記錄通常用於資料安全分析和入侵偵測。 能夠取得有關存取重要資訊的目標事件，讓組織大幅改善回應時間和調查準確度。  
  
-   **組織原則。** 例如，受到 PCI 標準規範的組織可以有一個集中原則，用於監視標記為包含信用卡資訊和個人識別資訊 (PII) 的所有檔案的存取。  
  
-   **部門原則。** 例如，財務部門可能要求某些財務文件 (例如每季盈餘報告) 只限財務部門可以修改，因此，該部門想要監視所有其他變更這些文件的嘗試。  
  
-   **業務原則。** 例如，業務擁有者可能想要監視所有未經授權檢視隸屬於其專案之資料的嘗試。  
  
此外，法務部門可能想要監視集中授權原則和原則建構 (例如，使用者、電腦及資源屬性) 的所有變更。  
  
安全性稽核的一個最大考量就是收集、儲存及分析稽核事件的成本。 如果稽核原則的範圍過大，收集的稽核事件數量就會上升，進而導致成本增加。 如果稽核原則的範圍過小，很可能會遺漏重要事件。  
  
有了 Windows Server 2012，您可以使用宣告和資源內容來編寫稽核原則。 這會產生更豐富、更具目標性及更容易管理的稽核原則。 它可以解決到目前為止仍無法或難以執行稽核原則的案例。 以下為系統管理員可以編寫的稽核原則範例：  
  
-   稽核每位未具備高度安全性許可但嘗試存取 HBI 文件的人員。 例如，Audit | Everyone | All-Access | Resource.BusinessImpact=HBI AND User.SecurityClearance!=High。  
  
-   稽核嘗試存取與自身的專案無關之文件的所有廠商。 例如，Audit | Everyone | All-Access | User.EmploymentStatus=Vendor AND User.Project Not_AnyOf Resource.Project。  
  
這些原則可以協助控制稽核事件的數量，並限制只稽核最相關的資料或使用者。  
  
當系統管理員建立並套用稽核原則後，下一個考量就是從收集的稽核事件中蒐集有意義的資訊。 以運算式為基礎的稽核事件可以協助減少稽核數量。 不過，使用者需要一種方式來查詢這些事件，以取得有意義的資訊，並詢問「誰正在存取我的 HBI 資料？」等問題。 或「是否有未經授權的嘗試存取敏感性資料？」  
  
 Windows Server 2012 利用使用者、電腦及資源宣告，增強現有資料存取事件的許可權。 這些事件會以每一伺服器的基礎產生。 為提供整個組織的完整事件檢視，Microsoft 與合作夥伴共同提供了事件集合與分析工具，像是 System Center Operation Manager 的稽核收集服務。  
  
圖 4 顯示集中稽核原則的概觀。  
  
![解決方案指南](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**圖 4** 集中稽核使用體驗  
  
設定和耗用安全性稽核通常包含下列一般步驟：  
  
1.  識別要監視的正確資料和使用者組合  
  
2.  建立和套用適當的稽核原則  
  
3.  收集和分析稽核事件  
  
4.  管理和監視已建立的原則  
  
## <a name="in-this-scenario"></a>在這個案例中  
下列主題提供此案例的其他指導方針：  
  
-   [規劃檔案存取稽核](Plan-for-File-Access-Auditing.md)  
  
-   [使用集中稽核原則&#40;部署安全性審核示範步驟&#41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="BKMK_NEW"></a>此案例中包含的角色和功能  
下表列出這個案例中的角色與功能，並說明它們如何支援這個案例。  
  
|角色/功能|如何支援本案例|  
|-----------------|---------------------------------|  
|Active Directory 網域服務角色|Windows Server 2012 中的 AD DS 引進了宣告型授權平臺，可讓您建立使用者宣告和裝置宣告、複合身分識別、（使用者加上裝置宣告）、新的集中存取原則（CAP）模型，以及使用檔案分類授權決策中的資訊。|  
|檔案和存放服務角色|Windows Server 2012 中的檔案伺服器提供使用者介面，讓系統管理員可以在其中查看使用者對於檔案或資料夾的有效許可權，並針對存取問題進行疑難排解，並視需要授與存取權。|  
  


