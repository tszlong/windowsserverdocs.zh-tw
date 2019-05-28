---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: AD FS 部署拓撲考量
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2af0950e52d800202235bf674545f6c47e9cd88
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190777"
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>搭配使用 AD DS 宣告與 AD FS
  
  
您可以使用 Active Directory 網域服務，以啟用更豐富的同盟應用程式的存取控制\(AD DS\)\-發給使用者和裝置宣告，以及 Active Directory Federation Services \(AD FS\).  
  
## <a name="about-dynamic-access-control"></a>關於動態存取控制  
在 Windows Server® 2012 中，「 動態存取控制 」 功能可讓組織授與使用者宣告為基礎的檔案的存取權\(其由使用者帳戶屬性源自\)和裝置宣告\(這來源電腦帳戶的屬性\)的 Active Directory 網域服務所發出\(AD DS\)。 發出宣告的 AD DS 已整合至 Windows 整合式驗證，透過 Kerberos 驗證通訊協定。  
  
如需有關動態存取控制的詳細資訊，請參閱 <<c0> [ 動態存取控制內容藍圖](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。  
  
### <a name="whats-new-in-ad-fs"></a>AD FS 中的新功能？  
動態存取控制案例的擴充功能，為 Windows Server 2012 中的 AD FS 現在可以：  
  
-   除了從 AD DS 中的使用者帳戶屬性存取的電腦帳戶屬性。 在舊版的 AD FS 中，Federation Service 無法存取的電腦帳戶的屬性完全從 AD DS。  
  
-   使用 AD DS 發給使用者或裝置宣告，位於 Kerberos 驗證票證。 在舊版的 AD FS 中，宣告引擎是能夠讀取使用者和群組安全性識別碼\(Sid\)從 Kerberos，但卻是不能夠讀取任何宣告的 Kerberos 票證中包含的資訊。  
  
-   將 AD DS 簽發至信賴憑證者應用程式可用來執行更豐富的存取控制的 SAML 權杖的使用者或裝置宣告。  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>使用 AD DS 的優點與 AD FS 的宣告  
這些發出宣告的 AD DS 可以插入 Kerberos 驗證票證，並搭配 AD FS，以提供下列優點：  
  
-   需要更豐富的存取控制原則的組織可以啟用宣告\-型的存取，應用程式和使用 AD DS 資源發出指定的使用者或電腦帳戶的 AD DS 中儲存的屬性值為基礎的宣告。 這可以協助降低建立和管理相關的額外負荷的系統管理員：  
  
    -   AD DS 安全性群組，否則會用來控制存取應用程式和資源，可透過 Windows 整合式驗證存取。  
  
    -   樹系信任，否則會用來控制存取商務\-要\-商務\(B2B\) \/網際網路可存取的應用程式和資源。  
  
-   組織可以現在防止未經授權的存取網路資源特定的電腦帳戶是否屬性儲存在 AD DS 中的值為基礎的用戶端電腦\(比方說，電腦的 DNS 名稱\)符合存取控制資源的原則\(比方說，已經宣告過已納入 Acl 的檔案伺服器\)或 信賴憑證者原則\(比方說，宣告\-感知 Web 應用程式\)。 這可協助系統管理員設定的資源或應用程式時具有更細微的存取控制原則：  
  
    -   只能透過 Windows 整合式驗證。  
  
    -   透過網際網路存取透過 AD FS 驗證機制。 AD FS 可以用來轉換成可以封裝到 SAML 權杖，可供網際網路存取資源或信賴憑證者的合作對象應用程式的 AD FS 宣告發給裝置宣告的 AD DS 中。  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>AD DS 與 AD FS 之間的差異發出的宣告  
有兩個務必了解從 AD DS vs 發出宣告的區別因素。AD FS。 這些差異包括：  
  
-   AD DS 只能發行封裝在 Kerberos 票證，而不是 SAML 權杖的宣告。 如需有關 AD DS 如何發出宣告的詳細資訊，請參閱[動態存取控制內容藍圖](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。  
  
-   AD FS 只能發行封裝在 SAML 權杖，而不是 Kerberos 票證中的宣告。 如需有關 AD FS 如何發出宣告的詳細資訊，請參閱[The Role of the Claims Engine](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)。  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>已發出的 AD DS 宣告如何與 AD FS 搭配  
發出宣告的 AD DS 可以搭配 AD FS，直接從使用者的驗證內容，而非個別 LDAP 呼叫至 Active Directory 中存取使用者與裝置宣告。 下圖和相對應的步驟將告訴您此程序中啟用宣告的更詳細的運作方式\-型動態存取控制案例的存取控制。  
  
![使用宣告](media/UsingADDSClaimswithADFS.gif)  
  
1.  AD DS 系統管理員會在 AD DS 結構描述中使用 [Active Directory 管理中心] 主控台或 PowerShell cmdlet 來啟用特定的宣告型別物件。  
  
2.  AD FS 系統管理員使用 AD FS 管理主控台來建立及設定宣告提供者和信賴憑證者信任關係 pass\-透過或轉換宣告規則。  
  
3.  Windows 用戶端嘗試存取網路。 Kerberos 驗證程序的一部分，用戶端會向其使用者和電腦的票證\-授與票證\(TGT\)其中未包含網域控制站的任何宣告。 網域控制站會在 AD DS 中尋找已啟用的宣告類型，然後傳回 Kerberos 票證中包含任何產生的宣告。  
  
4.  當使用者\/用戶端嘗試存取已納入 Acl，需要宣告的檔案資源，他們可以存取資源，因為呈現從 Kerberos 的複合識別碼具有這些宣告。  
  
5.  當相同的用戶端嘗試存取設定為使用 AD FS 驗證的網站或 Web 應用程式時，將使用者重新導向設定為使用 Windows 整合式驗證的 AD FS 同盟伺服器。 用戶端會傳送要求，使用 Kerberos 的網域控制站。 網域控制站發出 Kerberos 票證，其中包含要求的宣告，而用戶端可以再出示同盟伺服器。  
  
6.  根據在宣告提供者已設定的宣告規則的方式和信賴憑證者信任是先前設定的系統管理員，和 AD FS 從 Kerberos 票證讀取宣告它們包含在 SAML 權杖中，就會發出用戶端。  
  
7.  用戶端會接收包含正確宣告的 SAML 權杖，並重新導向至網站。  
  
如需如何建立所需發出的 AD DS 宣告以搭配 AD FS 的宣告規則的詳細資訊，請參閱[建立規則來轉換傳入宣告](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
