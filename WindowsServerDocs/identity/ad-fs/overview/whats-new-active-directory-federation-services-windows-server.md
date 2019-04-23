---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Windows Server 2016 的 Active Directory 同盟服務新功能
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/26/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: faa0590dc38921a56952aa54bf38243b6ff84d82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867709"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Active Directory 同盟服務的新功能


>適用於：Windows Server 2019，Windows Server 2016

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>在 Active Directory 同盟服務的 Windows Server 2019 最新消息

### <a name="protected-logins"></a>受保護的登入
更新受保護的登入，AD FS 2019 中可用的簡短摘要如下：
- **外部驗證提供者，為主要**-客戶可以現在使用第 3 方驗證產品，做為第一個因素，並公開密碼做為第一個因素。 在外部驗證提供者可以在其中證明 2 個因素的情況下，它可以宣告 MFA。 
- **密碼驗證作為其他驗證**-客戶有完整支援的收件匣選項，以使用密碼，僅針對其他因素之後密碼較少的選項使用做為第一個因素。 這可改善客戶體驗從 ADFS 2016 的客戶必須下載 github 配接器因為是受支援。 
- **隨插即用的威脅模組 Framework** -客戶現在可以建置自己的隨插即用來封鎖特定類型的要求在預先驗證階段的模組中。 這可讓您將雲端智慧，例如 Identity protection 封鎖有風險的使用者或有風險的交易的登入的客戶更容易。
- **ESL 改進**-藉由新增下列功能改善了 ESL QFE 2016
    - 可讓客戶為稽核模式時受保護的 「 傳統 」 的外部網路鎖定自 ADFS 2012 r2 起可用的功能。 目前 2016年客戶必須在稽核模式中的沒有保護。 
    - 可讓獨立的鎖定閾值，熟悉的位置。 這可讓多個執行個體，以變換密碼最少的影響與一般的服務帳戶執行的應用程式。 

### <a name="additional-security-improvements"></a>額外的安全性增強功能
下列額外的安全性改進項目會在 AD FS 2019 中：
- **使用智慧卡登入的遠端 PSH** -客戶可以現在使用智慧卡遠端連線到透過 PSH 的 ADFS，並且使用，來管理所有 PSH 函式包含多節點 PSH cmdlet。
- **HTTP 標頭自訂**-客戶現在可以自訂在 ADFS 回應期間發出的 HTTP 標頭。 這包括下列標頭
     - HSTS:這表示，ADFS 端點只能在相容的瀏覽器的 HTTPS 端點上強制執行
     - x 框架選項：允許以允許特定信賴憑證者的合作對象內嵌 ADFS 互動式登入頁面的 Iframe 的 ADFS 系統管理員。 這應該小心，而且只在 HTTPS 主機上使用。 
     - 未來的標頭：您可以也設定其他未來的標頭。 

如需詳細資訊，請參閱[與 AD FS 2019 的自訂 HTTP 安全性回應標頭](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>驗證原則/功能
下列的驗證/原則功能是在 AD FS 2019 中：
- **指定驗證方法的其他驗證每個 RP** -客戶可以現在使用宣告規則來決定要為其他驗證提供者叫用哪一個額外的驗證提供者。 為 2 使用案例，這是很有用
    - 客戶會從一個額外的驗證提供者轉換，到另一個。 這種方式在其加入使用者至較新的驗證提供者，他們可以使用群組，以控制哪些額外的驗證提供者會呼叫。
    - 客戶有特定的其他驗證提供者 （例如憑證） 的需求，對於某些應用程式。 
- **TLS 型裝置驗證只限於需要它的應用程式**-客戶現在可以限制用戶端為 TLS 基礎裝置驗證，只執行裝置應用程式型條件式存取。 這不需要基礎 TLS 裝置驗證的應用程式防止任何不必要的提示進行裝置驗證 （或如果無法處理用戶端應用程式的失敗）。
- **MFA 有效期限支援**-AD FS 現在支援的功能若要重新執行的第 2 個因素認證有效期限為基礎的第 2 個因素認證。 這可讓客戶執行初始交易，2 個因素以及第 2 個因素，定期執行的提示字元。 這是僅適用於可以提供額外的參數在要求中並不是，您可以在 ADFS 中的設定的應用程式。 當 「 記得我 MFA X 天 」 針對而且 'supportsMFA' 旗標設為 Azure AD 所支援這個參數在 Azure AD 中的同盟的網域信任設定，則為 true。 

### <a name="sign-in-sso-improvements"></a>SSO 登入的增強功能
在 AD FS 2019 中，下列登入 SSO 改進所做的：

- [分頁與置中對齊的佈景主題的 UX](../operations/AD-FS-paginated-sign-in.md) -ADFS 現在已移到分頁的 UX 流程，可讓 ADFS 驗證，並提供更順暢的登入體驗。 ADFS 現在會使用集中的 UI （而不是畫面的右側）。 您可能需要較新的標誌和背景影像，配合這項體驗。 這也會反映在 Azure AD 中所提供的功能。
- **Bug 修正：Win10 裝置進行 PRT 驗證時的永續性 SSO 狀態**這解決其中 MFA 狀態已不再是適用於 Windows 10 裝置使用 PRT 驗證時的問題。 問題的結果就是，使用者會收到提示第 2 個因素認證 (MFA) 常見問題。 修正程式也會讓體驗一致時透過用戶端 TLS 和透過 PRT 機制，能成功執行裝置驗證。 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>用於建置現代化的特定業務應用程式的支援
建置現代化的 LOB 應用程式的下列支援已新增至 AD FS 2019:

 - **Oauth 裝置流程/設定檔**-AD FS 現在支援 OAuth 裝置流量設定檔，在沒有 UI 介面區，以支援豐富的登入體驗的裝置上執行的登入。 這可讓使用者完成登入體驗，在不同裝置上。 這項功能需要 Azure CLI 在 Azure Stack 中的體驗，而且可用在其他情況下。 
 - **移除 'Resource' 參數的**-AD FS 現在已移除指定的資源參數目前的 Oauth 規格符合的需求。 用戶端現在可以提供的信賴憑證者信任識別碼做為範圍參數另外要求的權限。 
 - **AD FS 回應中的 CORS 標頭**-客戶現在可以建置單一頁面應用程式可讓用戶端端 JS 程式庫，透過查詢 AD FS 上的 OIDC 探索文件中的簽署金鑰驗證 id_token 的簽章。 
 - **PKCE 支援**-AD FS 新增 PKCE 的支援服務，以提供 OAuth 內的安全驗證碼流程。 這會新增額外一層安全性以防止攔截程式碼，並從不同的用戶端將它重新上執行此流程。 
 - **Bug 修正：傳送 「 x5t 」 和 「 kid 」 宣告**-這是次要的 bug 修正。 AD FS 現在此外會傳送 'kid' 宣告來表示驗證簽章的金鑰識別碼提示。 先前 AD FS 才傳送這 'x5t' 宣告。

### <a name="supportability-improvements"></a>可支援性增強功能
下列可支援性增強功能不是 AD FS 2019 的一部分：
- **錯誤詳細資料傳送至 AD FS 系統管理員**-可讓系統管理員，若要設定將使用者驗證的失敗，因為壓縮的欄位，以方便您取用儲存相關的偵錯記錄檔的使用者。 系統管理員也可以設定的 automail 壓縮的檔案分級的電子郵件帳戶，或自動建立票證，根據電子郵件的 SMTP 連接。 

### <a name="deployment-updates"></a>部署更新
中 AD FS 2019 現在包含下列的部署更新：
- **伺服器陣列行為層級 2019年**-因為與 AD FS 2016，沒有新的伺服陣列行為層級版本，才能啟用上面所討論的新功能。 這可讓從：
    - 2012 R2-> 2019
    - 2016 -> 2019   

### <a name="saml-updates"></a>SAML 更新
下列 SAML 更新會在 AD FS 2019:
- **Bug 修正：修正彙總的同盟中的 bug** -已彙總的同盟支援方面的許多 bug 修正 (例如 InCommon)。 修正存在已有下列： 
  - 改善大型的彙總的同盟中繼資料文件中的實體數目的比例。先前，就會失敗，「 ADMIN0017 」 錯誤。 
  - 使用透過 Get AdfsRelyingPartyTrustsGroup PSH cmdlet 'ScopeGroupID' 參數的查詢。 
  - 處理重複的 entityID 周圍的錯誤狀況


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Azure AD 樣式資源規格的範圍參數 
之前，AD FS，您必須在不同的參數中的所有驗證要求的範圍與所需的資源。 例如，一般的 oauth 要求會如下所示：7 **https:&#47;&#47;fs.contoso.com/adfs/oauth2/authorize？</br>response_type = 程式碼 client_id = claimsxrayclient 資源 = urn: microsoft:</br>adfs:claimsxray & 範圍 = oauth 和 redirect_uri = https:&#47;&#47;adfshelp.microsoft.com/</br> ClaimsXray /TokenResponse & prompt = login**
 
使用 AD FS 伺服器 2019年上，您現在可以傳遞內嵌在 scope 參數中的資源值。 這是與一個可以如何針對 Azure AD 的驗證也一致。 

範圍參數現在可以按照空格分隔的清單，其中每個項目是資源/範圍的結構。 例如  

**< 建立有效的範例要求 >**
> [!NOTE]
> 驗證要求中，就可以指定只有一個資源。 如果要求中包含多個資源，則 AD FS 會傳回錯誤，並驗證將不會成功。 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>如需 oAuth 的程式碼交換 」 (PKCE) 支援的證明金鑰 
使用授權碼授與 OAuth 公用用戶端都很容易授權程式碼攔截攻擊。  攻擊是 RFC 7636 中也描述。 若要減輕此類攻擊，AD FS 伺服器 2019年中的支援證明金鑰的程式碼 Exchange (PKCE) OAuth 授權碼授與流程。 
 
若要利用 PKCE 的支援，此規格會將額外的參數並將其加入的 OAuth 2.0 授權和存取權杖要求。

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. 用戶端會建立並記錄名為"code_verifier 「 祕密，衍生轉換的版本"t(code_verifier) 」 （又稱為 「 code_challenge"），會傳送 OAuth 2.0 授權要求中以及的轉換方法 」 t_m"。 

B. 授權端點會如往常般回應記錄 「 t(code_verifier)"和轉換方法。 

C. 用戶端再如往常般傳送存取權杖要求中的授權碼，但包含在 (A) 產生 「 code_verifier 「 祕密。 

D. AD FS 轉換"code_verifier 」，並將它與比較 「 t(code_verifier)"(b)。  如果它們不相等，則會拒絕存取。 

#### <a name="faq"></a>常見問題集 
**Q.** 可以傳遞的資源值的範圍值，例如如何針對 Azure AD 完成要求的一部分？ 
</br>**A.** 使用 AD FS 伺服器 2019年上，您現在可以傳遞內嵌在 scope 參數中的資源值。 範圍參數現在可以按照空格分隔的清單，其中每個項目是資源/範圍的結構。 例如  
**< 建立有效的範例要求 >**

**Q.** AD FS 支援 PKCE 延伸模組？
</br>**A.** AD FS 伺服器 2019年中的支援證明金鑰的程式碼 Exchange (PKCE) OAuth 授權碼授與流程 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Windows Server 2016 的 Active Directory 同盟服務新功能   
如果您要尋找舊版 AD FS 的詳細資訊，請參閱下列文章：  
 [Windows Server 2012 或 2012 R2 中的 ADFS](https://technet.microsoft.com/library/hh831502.aspx)和[AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Active Directory Federation Services 提供存取控制和單一登入的各種不同的應用程式包括 Office 365，雲端架構 SaaS 應用程式和公司網路上的應用程式。  
* IT 組織，它可讓您提供登入和存取控制現代化和傳統應用程式，在內部部署和雲端，同一組認證和原則為基礎。    
* 針對使用者，它會提供無縫式登上使用相同且熟悉的帳戶認證。  
* 開發人員，它會提供簡單的方法，來驗證其身分識別位於組織的目錄，讓您可以專注於應用程式、 未驗證或身分識別的使用者。  

本文說明在 Windows Server 2016 (AD FS 2016) 中的 AD FS 中最新消息。  

## <a name="eliminate-passwords-from-the-extranet"></a>排除來自外部網路的密碼  
AD FS 2016 啟用三個新選項不需要密碼，讓組織得以避免網路的風險的登入的危害從防範外, 洩或遭竊的密碼。 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>使用 Azure multi-factor Authentication 登入
AD FS 2016 組建 multi-factor authentication (MFA) 功能的 Windows Server 2012 R2 中的 AD FS 藉由使用只有 Azure MFA 的程式碼，而沒有先進入的使用者名稱和密碼登入。

* 使用 Azure MFA 的主要驗證方法，會提示使用者輸入其使用者名稱和 OTP 程式碼，從 Azure Authenticator 應用程式。  
* 使用 Azure MFA 的第二或其他驗證方法，使用者會提供主要的驗證認證 （使用 Windows 整合式驗證、 使用者名稱和密碼、 智慧卡或使用者或裝置的憑證），則會看到的提示文字，語音或 OTP 基礎 Azure MFA 登入。  
* 使用新內建的 Azure MFA 配接器安裝和設定 Azure mfa 與 AD FS 變得更簡單。
* 組織可以利用 Azure mfa 而不需要在內部部署 Azure MFA server。
* 內部網路或外部網路或任何存取控制原則的一部分，則可以設定 azure MFA。

如需 Azure MFA 與 AD FS
*  [設定 AD FS 2016 和 Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>從符合規範的裝置的無密碼的存取
AD FS 2016 是根據先前的裝置註冊功能，以啟用登入和存取控制基礎的裝置合規性狀態。 使用者可以登入使用裝置的認證，和合規性時，會重新評估裝置的屬性變更時，使您永遠可以確保會強制執行原則。  例如，這可讓原則

* 啟用只從受管理和/或符合規範的裝置存取  
* 只從受管理和/或符合規範的裝置啟用外部網路存取  
* 不受管理或不符合規範的電腦需要多重要素驗證  

AD FS 提供在內部部署元件的混合式案例中的條件式存取原則。 當您與雲端資源的條件式存取的 Azure AD 註冊裝置時，裝置身分識別可用於 AD FS 的原則。

![新功能](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 如需使用裝置的詳細資訊以在雲端中的條件式存取   
 *  [Azure Active Directory 條件式存取](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

如需有關使用裝置型條件式存取搭配 AD FS
*  [規劃裝置型條件式存取搭配 AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [在 AD FS 存取控制原則](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>登入 Windows Hello for Business   
Windows 10 裝置介紹 Windows Hello 和 Windows hello 企業版，取代受使用者手勢 （PIN、 生物識別手勢指紋或臉部辨識等） 的強式裝置繫結使用者認證的使用者密碼。 AD FS 2016 支援這些新的 Windows 10 功能，讓使用者可以登入 AD FS 應用程式從內部網路或外部網路而不需要提供密碼。

如需有關使用 Microsoft Windows Hello for Business 中您的組織
*  [啟用 Windows hello 企業版中您的組織](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>安全存取應用程式

### <a name="modern-authentication"></a>新式驗證
AD FS 2016 支援的最新的現代通訊協定，提供較佳使用者體驗 Windows 10，以及最新的 iOS 和 Android 裝置和應用程式。  

如需詳細資訊，請參閱[適用於開發人員的 AD FS 案例](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>設定存取控制原則，而不需要知道宣告規則語言  
以前，AD FS 系統管理員必須使用 AD FS 宣告規則語言中，設定原則設定和維護原則不容易。 存取控制原則，系統管理員可以使用內建的範本套用常見的原則，例如
* 允許以僅限內部網路存取
* 允許所有人，並要求從外部網路的 MFA
* 允許所有人並需要來自特定群組進行 MFA

範本能夠容易地將例外狀況或其他原則規則中使用精靈導向處理程序，並可套用至一或多個應用程式，來強制執行一致的原則。

如需詳細資訊，請參閱[AD FS 中的存取控制原則。](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>啟用登入與非 AD LDAP 目錄  
許多組織都有 Active Directory 和協力廠商目錄的組合。 搭配 AD FS 驗證 LDAP v3 相容目錄中儲存的使用者的支援，AD FS 現在可用來：
* 第三方，LDAP v3 相容目錄中的使用者
* 未設定的 Active Directory 的雙向信任關係的 Active Directory 樹系中的使用者
* Active Directory 輕量型目錄服務 (AD LDS) 中的使用者

如需詳細資訊，請參閱[設定 AD FS 驗證 LDAP 目錄中儲存的使用者。](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>獲得更佳登入體驗
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>自訂登入 AD FS 應用程式體驗  
我們聽到您能夠自訂每個應用程式的登入體驗會是很棒的使用性改進，特別是針對組織對於代表多個不同的公司或品牌的應用程式中提供 登入。  

先前，在 Windows Server 2012 R2 的 AD FS 提供了常見的登入體驗，針對所有信賴憑證者的合作對象應用程式，以讓您自訂的文字子集為基礎的每個應用程式的內容。 使用 Windows Server 2016，您可以自訂不僅訊息，但映像，每個應用程式的標誌和 web 佈景主題。 此外，您可以在 建立新的自訂網頁佈景主題，並套用這些每個信賴憑證者合作對象。  

如需詳細資訊，請參閱[AD FS 登入自訂的使用者數。](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>管理性和作業的增強功能  
下一節會說明所引入的 Windows Server 2016 中的 Active Directory Federation Services 的改良操作案例。  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>簡化的更容易管理的管理稽核  
Ad FS 的 Windows Server 2012 R2 已針對單一要求和記錄檔中的相關資訊產生的多個稽核事件或權杖發行活動是不存在 （在某些版本的 AD FS 中） 或跨多個稽核事件呈現。 預設的 AD FS 稽核事件會關閉由於其詳細資訊的本質。  
AD FS 2016 的版本中，稽核已變得更有效率又較不詳細資訊。  

如需詳細資訊，請參閱[稽核 Windows Server 2016 中的 AD fs 的增強功能。](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>改善與 SAML 2.0 confederations 參與的互通性  
AD FS 2016 包含其他 SAML 通訊協定支援，包括支援匯入包含多個實體的中繼資料為基礎的信任。 這可讓您設定 AD FS 以參與 confederations InCommon 同盟等其他符合 eGov 2.0 標準的實作。  

如需詳細資訊，請參閱[改善與 SAML 2.0 的互通性。](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>簡化的密碼管理同盟 O365 使用者  
您可以設定 Active Directory Federation Services (AD FS) 來傳送密碼到期宣告至信賴憑證者信任 （應用程式） 所保護的 AD FS。 如何使用這些宣告，則應用程式而定。 例如，搭配您信賴憑證者的合作對象為 Office 365，更新已實作 Exchange 和 Outlook，通知他們即將-至--已過期的密碼的同盟的使用者。  

如需詳細資訊，請參閱[設定 AD FS 傳送密碼到期宣告。](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>從 Windows Server 2012 R2 中 AD FS 移至 Windows Server 2016 中的 AD FS 是更容易  
之前，移轉至新的 AD FS 版本，您必須從舊的伺服器陣列匯出組態，以及匯入至全新的平行陣列。  

現在，AD FS 從 Windows Server 2012 R2 上移至 Windows Server 2016 上的 AD FS 已變得更為容易。 只要將新的 Windows Server 2016 伺服器新增到 Windows Server 2012 R2 的伺服器陣列，並在伺服器陣列就在 Windows Server 2012 R2 伺服器陣列的行為層級，因此它的外觀與行為就像 Windows Server 2012 R2 伺服器陣列。  

然後，在伺服器陣列中加入新的 Windows Server 2016 伺服器驗證功能，從負載平衡器移除較舊的伺服器。 一旦所有的伺服器陣列節點正在執行 Windows Server 2016，您就準備好升級至 2016年的伺服器陣列行為層級，並開始使用新的功能。  

如需詳細資訊，請參閱[升級至 Windows Server 2016 中的 AD FS。](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
