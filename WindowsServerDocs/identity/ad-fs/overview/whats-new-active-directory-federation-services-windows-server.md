---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Active Directory 同盟服務適用於 Windows Server 2016 中的新功能
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b8dd9174a869110d98ba1e65cbf2c2b6ec000b71
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2018
---
# <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Active Directory 同盟服務適用於 Windows Server 2016 中的新功能

>適用於：Windows Server 2016

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Active Directory 同盟服務適用於 Windows Server 2016 中的新功能   
如果您正在尋找適用於舊版 AD FS 的詳細資訊，請查看下列的文件：  
 [ADFS 中的 Windows Server 2012 或 2012 R2](https://technet.microsoft.com/library/hh831502.aspx)與[AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Active Directory 同盟服務提供存取控制和單一登入在各種不同的應用程式包括 Office 365、 雲端為基礎的 SaaS 應用程式和公司網路上的應用程式。  
* 適用於 IT 的組織，讓您提供登入來存取控制現代化和傳統應用程式，在場所和雲端，根據認證和原則相同的設定。    
* 使用者的使用的相同、 熟悉 account 認證提供順暢的登入。  
* 適用於開發人員，提供以驗證其身分居住組織 directory，以便您可以在應用程式，不驗證或身分專注於您的使用者可以輕鬆地。  

此文章將描述在 Windows Server 2016 (AD FS 2016) AD FS 中的新功能。  

## <a name="eliminate-passwords-from-the-extranet"></a>從外部排除密碼  
AD FS 2016 讓三個新的選項登入，而不需要密碼，讓組織避免網路的風險危害從 phished，遺漏或遭竊密碼。 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>使用 Azure 多因素驗證登入
AD FS 2016 組建多因素驗證時 (MFA) 功能在 Windows Server 2012 R2 AD FS 的允許使用只 Azure MFA 驗證碼，而不需要第一次輸入使用者名稱和密碼登入。

* 使用 Azure MFA 做為主要的驗證方法，使用者會提示他們的使用者名稱與 OTP Azure 驗證器 app 的程式碼。  
* 使用 Azure MFA 作為次要或額外的驗證方法、 使用者提供或主要驗證認證 （使用 Windows 整合式驗證、 使用者名稱和密碼，智慧卡或裝置的使用者或憑證），然後看到的文字、 語音命令提示字元中 OTP Azure MFA 登入。  
* 新建 Azure MFA 介面卡，安裝程式與設定 Azure AD FS 使用的 MFA 從未已簡單。
* 組織可以利用 Azure MFA 而不需要在場所 Azure MFA 伺服器。
* 您可以設定 azure MFA 內部網路的外部，或任何存取控制原則的一部分。

如需有關 Azure AD FS 使用的 MFA
*  [AD FS 2016 與 Azure MFA 設定](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>無密碼的存取權的相容裝置
AD FS 2016 上登入，以便先前的裝置登記功能及存取控制根據裝置相容性狀態。 使用者可以使用裝置的認證，登入並 compliance 重新時評估變更裝置屬性，以便您隨時可以確保原則的正在執行。  例如，如此可讓原則

* 可以存取只從受管理和/或相容的裝置  
* 只從受管理和/或相容的裝置可以外部網路的存取權  
* 需要多因素驗證無法管理或不相容的電腦  

AD FS 提供條件存取原則在混合案例中的上場所元件。 當您登記裝置與雲端資源條件存取 Azure AD 時，裝置的身分可用於，以及 AD FS 原則。

![新互動的資訊](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 更多有關如何使用裝置的基礎條件在雲端中的存取   
 *  [Azure Active Directory 條件存取](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)

更多有關如何使用裝置的基礎條件 AD FS 使用的存取
*  [規劃區域的裝置會根據條件 AD FS 使用的存取](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [AD FS 中存取控制原則](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>登入 Windows Hello 企業版   
Windows 10 裝置引進了 Windows Hello 與 Windows Hello 企業版，使用者密碼取代穩固裝置繫結使用者認證使用者的手勢（釘選、指紋或臉部辨識等生物特徵辨識手勢）所保護。 AD FS 2016 支援這些新這些新的 Windows 10 功能讓使用者可以登入 AD FS 應用程式從內部網路或外部而不需要輸入密碼。

如需有關使用 Microsoft Windows Hello 企業版您在組織中
*  [讓 Windows Hello 企業版您在組織中](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>安全地存取應用程式

### <a name="modern-authentication"></a>現代化驗證
AD FS 2016 支援的最新的現代化通訊協定，以提供更好的使用者體驗 Windows 10，以及最新 iOS 和 Android 裝置和 app。  

如需詳細資訊請查看[適用於開發人員 AD FS 案例](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>存取控制原則設定而不需要知道取得規則語言  
在過去，AD FS 管理員必須設定原則，使用 AD FS 理賠要求規則語言，讓您更容易設定和維護原則。 存取控制原則，系統管理員可以使用組建中的範本，例如適用於通用原則
* [允許只有內部網路存取權
* 允許所有人和需要 MFA 從外部網路
* 允許所有人或 MFA 需要從指定的群組

很容易使用導向處理程序精靈將例外或額外的原則規則自訂的範本和可套用至一或多個應用程式執法一致的原則。

如需詳細資訊請查看[在 AD FS 存取控制原則。](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>在非 AD LDAP 目錄讓登入  
許多組織都有 Active Directory 和第三方目錄的組合。 AD FS 支援驗證使用者儲存在 LDAP v3 相容目錄加，AD FS 現在可以使用適用於：
* 第三方，LDAP v3 相容目錄使用者
* Active Directory 樹系的 Active Directory 雙向信任未設定中的使用者
* Active Directory 輕量 Directory Services (AD LDS) 中的使用者

如需詳細資訊請查看[設定 AD FS 進行驗證使用者儲存在 LDAP 目錄。](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>變得更好登入的體驗
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>自訂登入 AD FS 應用程式的體驗  
我們收到您的登入訂每個應用程式的能力會變得更好的可用性的改進，尤其是針對者登上提供多個不同的公司或品牌代表應用程式的組織。  

之前，在 Windows Server 2012 R2 AD FS 常見的登入的使用經驗提供所有信賴方應用程式的自訂文字子集的能力 content 每個應用程式。 與 Windows Server 2016，您可以自訂不僅訊息，但映像，商標和 web 主題每個應用程式。 此外，您可以建立新的自訂 web 主題，適用這些每可以派對。  

如需詳細資訊請查看[AD FS 使用者登入自訂。](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>管理性與操作改進  
下一節描述在 Windows Server 2016 的 Active Directory 同盟服務引進改善操作案例。  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>精簡稽核變得更容易管理  
AD FS 適用於 Windows Server 2012 R2 是許多稽核事件單一要求和的相關資訊的登入或權杖發行活動是缺少 （在某些 AD FS 版本） 或讓跨多個稽核事件。 AD FS 預設稽核事件是因為其詳細資訊的性質被關閉。  
AD FS 2016 發行，稽核已變更有效率且較低的詳細資訊。  

如需詳細資訊請查看[稽核要在 Windows Server 2016 AD FS 美化效果。](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>已改善使用 SAML 2.0 交互操作 confederations 參與  
AD FS 2016 包含其他 SAML 通訊協定支援，包括根據中繼資料包含多個項目信任匯入的支援。 這可讓您設定在 confederations InCommon 聯盟和其他實作 eGov 2.0 一般符合參與 AD FS。  

如需詳細資訊請查看[改進 SAML 2.0 與交互操作。](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>簡化的密碼管理聯盟 O365 使用者  
您可以設定 Active Directory 同盟 Services (AD FS) 傳送密碼到期宣告信賴廠商信任 （應用程式） 所保護 AD FS。 如何使用這些宣告應用程式而有所不同。 例如使用 Office 365，為您信賴，更新已實作通知聯盟的使用者他們即將-到--已過期的密碼換貨及 Outlook。  

如需詳細資訊請查看[設定密碼到期宣告傳送給 AD FS。](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>在 Windows Server 2012 R2 AD FS 從移到 Windows Server 2016 中的 AD FS 已變得更容易  
之前，移轉到新版本的 AD FS，您必須從舊發電廠匯出設定和匯入至全新、 平行發電廠。  

現在，前往 AD FS 從 Windows Server 2012 R2 上 AD FS 在 Windows Server 2016 上變得更加容易。 只是 Windows Server 2012 R2 陣列，以新增新的 Windows Server 2016 伺服器，並讓它看起來就像 Windows Server 2012 R2 發電廠的行為，在 Windows Server 2012 R2 發電廠行為層級，將會執行發電廠。  

然後發電廠加入新的 Windows Server 2016 伺服器驗證功能，從負載平衡器移除較舊的伺服器。 所有發電廠節點都執行 Windows Server 2016，一旦您已經準備好升級 2016年發電廠行為層級，並開始使用的新功能。  

如需詳細資訊請查看[升級到 Windows Server 2016 中的 AD FS。](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
