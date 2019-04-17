---
ms.assetid: 460792e4-9f1d-4e7b-b6b2-53e057f839df
title: "AD FS 部署拓撲注意事項"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 46692653ba10558a9236bd321127591bc7c8a275
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="using-ad-ds-claims-with-ad-fs"></a>請使用 AD FS 使用 AD DS 宣告
  
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
  
您可以讓聯盟應用程式為豐富存取控制 Active Directory Domain Services \(AD DS\)\-issued 使用者和裝置宣告 Active Directory 同盟服務 \(AD FS\) 一起使用。  
  
## <a name="about-dynamic-access-control"></a>關於動態存取控制  
在 Windows Server® 2012 的動態存取控制功能可以讓組織權限授與檔案根據使用者宣告 \ （這來源使用者 account attributes\） 和裝置宣告 \ （這來源電腦 account attributes\） 所發行的 Active Directory Domain Services \(AD DS\)。 Windows 透過 Kerberos 驗證通訊協定的整合式驗證已經整合 AD DS 發出主張。  
  
如需動態存取控制的詳細資訊，請查看[動態存取控制內容藍圖](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。  
  
### <a name="whats-new-in-ad-fs"></a>AD FS 中的新功能？  
AD FS Windows Server 2012 中的為動態存取控制案例擴充功能，可以立即：  
  
-   除了從 AD DS 中的使用者 account 屬性存取電腦 account 屬性。 在舊版 AD FS，同盟服務無法存取電腦 account 屬性完全 AD DS 從。  
  
-   請使用 AD DS 發出使用者或裝置宣告位於 Kerberos 驗證票證。 AD FS 舊版，宣告引擎是讀取使用者和群組安全性 Id \(SIDs\) 從 Kerberos 但找不到任何朗讀宣告 Kerberos 票證中所包含的資訊。  
  
-   轉換 AD DS 發出的使用者或裝置宣告到 SAML 權杖信賴的應用程式可以用來執行更豐富的存取控制。  
  
## <a name="benefits-of-using-ad-ds-claims-with-ad-fs"></a>優點 AD DS AD FS 使用的宣告  
發行宣告這些 AD DS 可以插入 Kerberos 驗證票證及使用 AD FS 進行提供下列優點：  
  
-   需要更豐富的存取控制原則組織可以使用 AD DS 發出宣告儲存在指定的使用者或電腦 account AD DS 屬性的值為基礎的讓 claims\ 型存取應用程式和資源。 這可以幫助系統管理員可以降低其他建立及管理相關聯的費用：  
  
    -   AD DS 安全性群組，否則會用來控制存取應用程式與資源，可透過 Windows 整合式驗證。  
  
    -   否則會用來控制 Business\ to\ 商務 \(B2B\) 存取信任的樹系 \ 日網際網路無障礙應用程式和資源。  
  
-   組織可以立即防止未經授權的存取網路資源從依據特定電腦 account 是否屬性儲存在 AD DS 值 client 電腦 \ (例如電腦的 DNS name\) 符合存取資源的控制項原則 \ （例如，檔案伺服器的 claims\ 已 ACLd） 或信賴的派對原則 \ (例如，claims\ 感知 Web application\)。 這可以幫助系統管理員設定美好存取控制原則資源或的應用程式：  
  
    -   只存取透過 Windows 整合式驗證。  
  
    -   AD FS 驗證機制透過可以存取網際網路。 AD FS 可用於轉換成 SAML 發行，可由網際網路存取資源或信賴方應用程式可以封裝 AD FS 宣告發出裝置宣告 AD DS。  
  
## <a name="differences-between-ad-ds-and-ad-fs-issued-claims"></a>AD DS 與 AD FS 不同發出宣告  
有兩個區分因素重要了解有關宣告所發行的 AD DS 與 AD FS。 這些不同包括：  
  
-   AD DS 只能發行封裝 Kerberos 門票，不 SAML 權杖中的主張。 如需有關如何 AD DS 問題宣告，請查看[動態存取控制內容藍圖](../../solution-guides/Dynamic-Access-Control--Scenario-Overview.md#BKMK_APP)。  
  
-   AD FS 只能發行，會在 SAML 發行，而不 Kerberos 門票封裝宣告。 如需有關如何 AD FS 問題宣告，請查看[的角色宣告引擎的](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)。  
  
## <a name="how-ad-ds-issued-claims-work-with-ad-fs"></a>AD DS 發行宣告 AD FS 使用的方式  
發行宣告 AD DS 可以搭配直接從使用者的驗證操作，而另外 LDAP 電話給 Active Directory 存取使用者及裝置宣告 AD FS。 下圖與對應的步驟討論此程序中更多詳細資料，以便 claims\ 式存取控制動態存取控制案例的運作方式。  
  
![使用宣告](media/UsingADDSClaimswithADFS.gif)  
  
1.  AD DS 系統管理員會使用 Active Directory 管理中心主機或允許特定理賠要求輸入物件 PowerShell cmdlet AD DS 結構描述。  
  
2.  AD FS 管理員來建立和設定宣告提供者和信賴使用 AD FS 管理主控台信任的其中一個 pass\ 透過或轉換宣告規則。  
  
3.  Windows client 嘗試存取該網路。 Kerberos 驗證程序的一部分，client 提供使用者和電腦 ticket\ 授與票證 \(TGT\) 這並不會尚未包含網域控制站，任何主張。 網域控制站 AD DS 尋找讓的宣告類型，然後傳回 Kerberos 票證包含任何結果主張。  
  
4.  當使用者 \ 日 client 嘗試存取 ACLd 需要宣告檔案資源時，他們就可以存取資源，因為已從 Kerberos 提出複合 ID 這些宣告。  
  
5.  當相同 client 嘗試存取設定為使用 AD FS 驗證網站或 Web 應用程式時，使用者會重新導向至設定為 Windows 整合驗證，AD FS 聯盟伺服器。 Client 網域控制站使用 Kerberos 傳送要求。 網域控制站問題 Kerberos 票證包含要求的宣告 client 可以再呈現聯盟伺服器。  
  
6.  根據宣告規則宣告提供者已設定的方式，可以廠商信任的系統管理員先前設定，AD FS 讀取宣告 Kerberos 票證，並在該問題的 client SAML 權杖中包含他們。  
  
7.  Client 會收到包含正確宣告 SAML 預付碼和會重新導向至該網站。  
  
如需如何建立理賠要求規則所需的發行 AD DS 宣告 AD FS 使用的詳細資訊，請查看[建立規則轉換取得連入](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
