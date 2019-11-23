---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Windows Server 2016 的 Active Directory 同盟服務新功能
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/23/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6294c7b6ead0a9fa338f8b2cc8134b750f7e3e8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385555"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Active Directory 同盟服務的新功能


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Windows Server 2019 Active Directory 同盟服務的新功能

### <a name="protected-logins"></a>受保護的登入
以下是 AD FS 2019 中可用之受保護登入更新的簡短摘要：
- **外部驗證提供者作為主要**-客戶現在可以使用協力廠商驗證產品作為第一個要素，而不會將密碼公開為第一個因素。 在外部驗證提供者可以證明兩個因素的情況下，它可以宣告 MFA。 
- **密碼驗證做為額外驗證**-客戶擁有完全支援的收件匣選項，只有在使用 [密碼較少] 選項做為第一個因素之後，才將密碼用於額外的因素。 這可改善客戶從 ADFS 2016 的體驗，其中客戶必須下載 github 介面卡（支援的方式）。 
- 隨插即用**風險評估模組**-客戶現在可以建立自己的外掛程式模組，以在預先驗證階段封鎖特定類型的要求。 這可讓客戶更輕鬆地使用身分識別保護之類的雲端智慧來封鎖有風險的使用者登入或有風險的交易。  如需詳細資訊，請參閱[使用 AD FS 2019 風險評估模型建立外掛程式](../../ad-fs/development/ad-fs-risk-assessment-model.md) 
- **ESL**改良-新增下列功能以改善2016中的 ESL QFE
    - 可讓客戶處於 audit 模式，同時受 ADFS 2012R2 後所提供的「傳統」外部網路鎖定功能保護。 目前2016客戶在 audit 模式下不會有任何保護。 
    - 針對熟悉的位置啟用獨立的鎖定閾值。 如此一來，使用泛型服務帳戶執行的多個應用程式實例，就能以最少的影響來變換密碼。 

### <a name="additional-security-improvements"></a>其他安全性增強功能
AD FS 2019 提供下列額外的安全性改進：
- **使用智慧卡登入的遠端 PSH** -客戶現在可以使用智慧卡透過 PSH 遠端連線到 ADFS，並使用它來管理所有 PSH 功能，包括多節點 PSH Cmdlet。
- **HTTP 標頭自訂**-客戶現在可以自訂在 ADFS 回應期間發出的 HTTP 標頭。 這包括下列標頭
     - HSTS：這會傳達 ADFS 端點只能用於相容瀏覽器的 HTTPS 端點，以強制執行
     - x 框架-選項：可讓 ADFS 系統管理員允許特定的信賴憑證者內嵌 ADFS 互動式登入頁面的 Iframe。 這應該謹慎使用，而且只適用于 HTTPS 主機。 
     - 未來標頭：其他未來的標頭也可以設定。 

如需詳細資訊，請參閱[使用 AD FS 2019 自訂 HTTP 安全性回應標頭](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>驗證/原則功能
以下是 AD FS 2019 中的驗證/原則功能：
- **針對每個 RP 指定額外驗證的驗證方法**-客戶現在可以使用宣告規則來決定要針對其他驗證提供者叫用的其他驗證提供者。 這適用于2個使用案例
    - 客戶正從一個額外的驗證提供者轉換到另一個。 如此一來，當他們將使用者上線至較新的驗證提供者時，他們就可以使用群組來控制要呼叫的其他驗證提供者。
    - 針對特定應用程式，客戶需要特定的額外驗證提供者（例如憑證）。 
- **僅將 TLS 型裝置驗證限制為需要 it 的應用程式**-客戶現在可以限制只有執行以裝置為基礎的條件式存取的應用程式才能驗證用戶端 TLS 裝置。 這可避免對不需要 TLS 型裝置驗證的應用程式進行裝置驗證的任何不必要提示（如果用戶端應用程式無法處理，則會失敗）。
- **MFA 時效性支援**-AD FS 現在支援根據第二個要素認證的有效期限重新執行第二個要素認證的功能。 這可讓客戶執行具有2個因素的初始交易，並只定期提示第二個因素。 這僅適用于可在要求中提供額外參數，而且在 ADFS 中不是可設定的應用程式。 當已設定 [記住我的 supportsMFA X 天]，且 Azure AD 中的同盟網域信任設定上的 ' ' 旗標設為 true 時，Azure AD 支援此參數。 

### <a name="sign-in-sso-improvements"></a>登入 SSO 改良功能
已在 AD FS 2019 中進行下列登入 SSO 改善：

- [具有](../operations/AD-FS-paginated-sign-in.md)置中主題的編頁 ux： adfs 現在已移至分頁 ux 流程，可讓 adfs 驗證並提供更流暢的登入體驗。 ADFS 現在使用中央 UI （而不是畫面的右側）。 您可能需要較新的標誌和背景影像，才能與此體驗一致。 這也會反映 Azure AD 中提供的功能。
- **Bug 修正：執行 PRT auth 時 Win10 裝置的持續 SSO 狀態**  這可解決使用 Windows 10 裝置的 PRT authentication 時，未保存 MFA 狀態的問題。 問題的結果是終端使用者經常會收到第二個要素認證（MFA）的提示。 修正程式也會在透過用戶端 TLS 和透過 PRT 機制成功執行裝置驗證時，讓體驗保持一致。 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>建立現代化企業營運應用程式的支援
下列建立新式 LOB 應用程式的支援已新增至 AD FS 2019：

 - **Oauth 裝置流程/設定檔**-AD FS 現在支援 oauth 裝置流程設定檔，以在沒有 UI 介面區的裝置上執行登入，以支援豐富的登入體驗。 這可讓使用者在不同的裝置上完成登入體驗。 這是 Azure Stack 中 Azure CLI 體驗的必要功能，而且可以在其他情況下使用。 
 - **移除 ' Resource ' 參數**-AD FS 現在已移除指定資源參數的需求，這是以目前的 Oauth 規格為行。 除了要求的許可權之外，用戶端現在可以提供信賴憑證者信任識別碼做為範圍參數。 
 - **AD FS 回應中的 CORS 標頭**-客戶現在可以建立單一頁面應用程式，讓用戶端 JS 程式庫從 AD FS 上的 OIDC 探索檔查詢簽署金鑰，以驗證 id_token 的簽章。 
 - **PKCE 支援**-AD FS 新增 PKCE 支援，以在 OAuth 中提供安全的驗證程式代碼流程。 這會在此流程中增加額外的安全性層級，以避免攔截程式碼，並從不同的用戶端重新執行它。 
 - **Bug 修正：傳送 x5t 和小孩索賠**-這是次要的錯誤修正。 AD FS 現在會另外傳送 ' 小孩 ' 宣告，以表示用來驗證簽章的金鑰識別碼提示。 先前 AD FS 只會將此傳送為 ' x5t ' 宣告。

### <a name="supportability-improvements"></a>支援性改善
下列可支援性改進不是 AD FS 2019 的一部分：
- **將錯誤詳細資料傳送給 AD FS 系統管理員**-可讓系統管理員將終端使用者驗證失敗的相關 debug 記錄檔，設定為以壓縮的方式儲存，以方便取用。 系統管理員也可以設定 SMTP 連線，將壓縮檔案 automail 到分級電子郵件帳戶，或根據電子郵件自動建立票證。 

### <a name="deployment-updates"></a>部署更新
AD FS 2019 現在包含下列部署更新：
- **伺服器陣列行為層級 2019** -如同 AD FS 2016，有一個新的伺服器陣列行為層級版本，必須啟用以上所討論的新功能。 這可讓您進行下列動作：
    - 2012 R2-> 2019
    - 2016-> 2019   

### <a name="saml-updates"></a>SAML 更新
下列 SAML 更新位於 AD FS 2019：
- **Bug 修正：修正匯總同盟中的 bug** -針對匯總的同盟支援（例如 InCommon），已有許多 bug 修正。 已解決下列問題： 
  - 已改善匯總同盟元資料檔案中大量實體的縮放比例。之前，這會失敗，並出現 "ADMIN0017" 錯誤。 
  - 透過 AdfsRelyingPartyTrustsGroup PSH Cmdlet 使用 ' ScopeGroupID ' 參數進行查詢。 
  - 處理重複 entityID 前後的錯誤狀況


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>範圍參數中 Azure AD 樣式資源規格 
之前，AD FS 需要在任何驗證要求中，將所需的資源和範圍放在不同的參數中。 例如，一般的 oauth 要求看起來會像下面這樣： 7 **HTTPs&#47;&#47;： fs.contoso.com/adfs/oauth2/authorize？</br>response_type = code & client_id = claimsxrayclient & resource = urn： microsoft：</br>adfs： claimsxray & 範圍 = oauth & redirect_uri = HTTPs&#47;&#47;： adfshelp.microsoft.com/</br> claimsxray/TokenResponse & prompt = login**
 
使用伺服器2019上的 AD FS，您現在可以傳遞內嵌在範圍參數中的資源值。 這與 Azure AD 也可以對其進行驗證的方式一致。 

範圍參數現在可以組織成以空格分隔的清單，其中每個專案的結構都是資源/範圍。 例如：  

**< 建立有效的範例要求 >**
> [!NOTE]
> 只有一個資源可以在驗證要求中指定。 如果要求中包含一個以上的資源，AD FS 將會傳回錯誤，且驗證將會失敗。 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>OAuth 的程式碼交換（PKCE）支援的證明金鑰 
使用授權碼授與的 OAuth 公用用戶端，很容易遭受授權碼攔截攻擊。  在 RFC 7636 中，這項攻擊的詳細說明。 為了減輕這種攻擊，伺服器2019中的 AD FS 支援 OAuth 授權碼授與流程的程式碼交換（PKCE）證明金鑰。 
 
為了利用 PKCE 支援，此規格會將其他參數新增至 OAuth 2.0 授權和存取權杖要求。

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. 用戶端會建立並記錄名為 "code_verifier" 的密碼，並衍生轉換後的 "t （code_verifier）" （稱為「code_challenge」），該版本會在 OAuth 2.0 授權要求中傳送，以及轉換方法 "t_m"。 

B. 授權端點會如往常般回應，但會記錄 "t （code_verifier）" 和轉換方法。 

C. 接著，用戶端會如往常般傳送存取權杖要求中的授權碼，但包含在（A）產生的「code_verifier」秘密。 

d. AD FS 會轉換 "code_verifier"，並將它與（B）的 "t （code_verifier）" 進行比較。  如果存取不相等，則會遭到拒絕。 

#### <a name="faq"></a>常見問題 
**問題解答.** 我可以將資源值當做範圍值的一部分來傳遞，像是如何對 Azure AD 進行要求？ 
</br>**為.** 使用伺服器2019上的 AD FS，您現在可以傳遞內嵌在範圍參數中的資源值。 範圍參數現在可以組織成以空格分隔的清單，其中每個專案的結構都是資源/範圍。 例如：  
**< 建立有效的範例要求 >**

**問題解答.** AD FS 是否支援 PKCE 延伸模組？
</br>**為.** 伺服器2019中的 AD FS 支援 OAuth 授權碼授與流程的程式碼交換的證明金鑰（PKCE） 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Windows Server 2016 的 Active Directory 同盟服務新功能   
如果您要尋找舊版 AD FS 的相關資訊，請參閱下列文章：  
 [Windows Server 2012 或 2012 R2](https://technet.microsoft.com/library/hh831502.aspx)和[AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)中的 ADFS  

 Active Directory 同盟服務在各種不同的應用程式（包括 Office 365、雲端式 SaaS 應用程式和公司網路上的應用程式）上提供存取控制和單一登入。  
* 針對 IT 組織，它可讓您根據相同的一組認證和原則，針對內部部署和雲端中的現代化和繼承應用程式提供登入和存取控制。    
* 對於使用者，它會使用相同、熟悉的帳號憑證來提供順暢的登入。  
* 對於開發人員來說，它提供簡單的方法來驗證身分識別位於組織目錄中的使用者，讓您可以專注于應用程式，而不是驗證或身分識別。  

本文說明 Windows Server 2016 （AD FS 2016）中 AD FS 的新功能。  

## <a name="eliminate-passwords-from-the-extranet"></a>排除外部網路的密碼  
AD FS 2016 提供三個新的登入選項，而不需要密碼，讓組織可以避免因誘騙、洩漏或遭竊密碼而造成網路洩露的風險。 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>使用 Azure 多重要素驗證登入
AD FS 2016 是以 Windows Server 2012 R2 中 AD FS 的多重要素驗證（MFA）功能為基礎，只允許使用 Azure MFA 程式碼登入，而不需要先輸入使用者名稱和密碼。

* 使用 Azure MFA 做為主要驗證方法時，系統會提示使用者輸入其使用者名稱和來自 Azure 驗證器應用程式的 OTP 代碼。  
* 使用 Azure MFA 做為次要或其他驗證方法時，使用者會提供主要的驗證認證（使用 Windows 整合式驗證、使用者名稱和密碼、智慧卡或使用者或裝置憑證），然後看到文字提示，語音或以 OTP 為基礎的 Azure MFA 登入。  
* 有了新的內建 Azure MFA 介面卡，使用 AD FS 進行 Azure MFA 的安裝和設定並不容易。
* 組織可以利用 Azure MFA，而不需要內部部署 Azure MFA server。
* Azure MFA 可設定為內部網路或外部網路，或做為任何存取控制原則的一部分。

如需 Azure MFA 與 AD FS 的詳細資訊
*  [設定 AD FS 2016 和 Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>相容裝置的無密碼存取
AD FS 2016 建置於先前的裝置註冊功能上，以根據裝置合規性狀態啟用登入和存取控制。 使用者可以使用裝置認證登入，而當裝置屬性變更時，會重新評估合規性，讓您可以隨時確保強制執行原則。  這會啟用之類的原則

* 僅從受管理及/或相容的裝置啟用存取  
* 只能從受管理和/或相容的裝置啟用外部網路存取  
* 針對未受管理或不符合規範的電腦，需要多重要素驗證  

AD FS 在混合式案例中提供條件式存取原則的內部部署元件。 當您使用 Azure AD 對雲端資源的條件式存取註冊裝置時，裝置身分識別也可以用於 AD FS 原則。

![新的](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 如需在雲端中使用以裝置為基礎的條件式存取的詳細資訊   
 *  [Azure Active Directory 條件式存取](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

如需有關使用以裝置為基礎的條件式存取與 AD FS 的詳細資訊
*  [使用 AD FS 規劃以裝置為基礎的條件式存取](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [AD FS 中的存取控制原則](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>使用 Windows Hello 企業版登入   
Windows 10 裝置引進 Windows Hello 和 Windows Hello 企業版，以使用者的手勢所保護的強式裝置系結使用者認證來取代使用者密碼（PIN、指紋之類的生物識別手勢或臉部辨識）。 AD FS 2016 支援這些新的 Windows 10 功能，讓使用者可以登入來自內部網路或外部網路的 AD FS 應用程式，而不需要提供密碼。

如需在您的組織中使用 Microsoft Windows Hello 企業版的詳細資訊
*  [在您的組織中啟用 Windows Hello 企業版](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>保護應用程式的存取

### <a name="modern-authentication"></a>新式驗證
AD FS 2016 支援最新的新式通訊協定，可為 Windows 10 以及最新的 iOS 和 Android 裝置和應用程式提供更好的使用者體驗。  

如需詳細資訊，請參閱[開發人員的 AD FS 案例](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>設定存取控制原則，而不需要知道宣告規則語言  
之前，AD FS 系統管理員必須使用 AD FS 宣告規則語言來設定原則，因此很容易設定和維護原則。 使用存取控制原則時，系統管理員可以使用內建範本來套用常見的原則，例如
* 僅允許內部網路存取
* 允許所有人，並要求來自外部網路的 MFA
* 允許所有人，並要求特定群組的 MFA

您可以使用 wizard 驅動的程式來新增例外狀況或其他原則規則，輕鬆地自訂範本，並可套用至一或多個應用程式，以強制執行一致的原則。

如需詳細資訊，請參閱[AD FS 中的存取控制原則。](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>啟用使用非 AD LDAP 目錄登入  
許多組織都有 Active Directory 和協力廠商目錄的組合。 由於新增了 AD FS 支援來驗證儲存在 LDAP v3 相容目錄中的使用者，因此 AD FS 現在可以用於：
* 協力廠商的使用者，LDAP v3 相容目錄
* 未設定 Active Directory 雙向信任 Active Directory 樹系中的使用者
* Active Directory 輕量型目錄服務中的使用者（AD LDS）

如需詳細資訊，請參閱[設定 AD FS 來驗證 LDAP 目錄中儲存的使用者。](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>更好的登入體驗
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>自訂 AD FS 應用程式的登入體驗  
我們聽說您可以自訂每個應用程式的登入體驗，這是一項絕佳的使用性改善，特別是針對提供代表多家不同公司或品牌之應用程式的組織。  

先前，Windows Server 2012 R2 中的 AD FS 提供了所有信賴憑證者應用程式的一般登入體驗，而且能夠自訂每個應用程式的文字內容子集。 有了 Windows Server 2016，您就可以自訂每個應用程式的訊息，但是影像、標誌和網頁主題。 此外，您可以建立新的自訂 web 主題，並套用每個信賴憑證者的應用。  

如需詳細資訊，請參閱[AD FS 使用者登入自訂。](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>管理能力和操作增強功能  
下一節說明 Windows Server 2016 中 Active Directory 同盟服務引進的改良操作案例。  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>簡化的審核程式，更容易管理管理  
在 Windows Server 2012 R2 的 AD FS 中，有許多針對單一要求產生的 audit 事件，而且登入或權杖發行活動的相關資訊不存在（在某些版本的 AD FS 中），或散佈在多個 audit 事件中。 根據預設，AD FS audit 事件會因為其詳細資訊性質而關閉。  
隨著 AD FS 2016 的發行，審核功能變得更有效率且較不詳細。  

如需詳細資訊，請參閱[Windows Server 2016 中 AD FS 的審核功能增強。](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>已改善 SAML 2.0 與參與 confederations 的互通性  
AD FS 2016 包含額外的 SAML 通訊協定支援，包括根據包含多個實體的中繼資料來匯入信任的支援。 這可讓您設定 AD FS 以參與 confederations，例如 InCommon 同盟和符合 eGov 2.0 標準的其他執行。  

如需詳細資訊，請參閱[改善與 SAML 2.0 的互通性。](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>同盟 O365 使用者的簡化密碼管理  
您可以設定 Active Directory 同盟服務（AD FS），將密碼到期宣告傳送給受 AD FS 保護的信賴憑證者信任（應用程式）。 這些宣告的使用方式取決於應用程式。 例如，使用 Office 365 做為您的信賴憑證者，更新已實作為 Exchange 和 Outlook，以通知同盟使用者其即將過期的密碼。  

如需詳細資訊，請參閱[設定 AD FS 傳送密碼到期宣告。](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>從 Windows Server 2012 R2 中的 AD FS 移至 Windows Server 2016 中的 AD FS 更容易  
之前，遷移至新版本的 AD FS 需要從舊的伺服器陣列匯出設定，然後匯入全新的平行伺服器陣列。  

現在，從 Windows Server 2012 R2 上的 AD FS 移至 Windows Server 2016 上的 AD FS 已變得更容易。 只要將新的 Windows Server 2016 伺服器新增到 Windows Server 2012 R2 伺服器陣列，伺服器陣列就會在 Windows Server 2012 R2 伺服器陣列行為層級執行，因此它的外觀和行為就像是 Windows Server 2012 R2 伺服器陣列。  

然後，將新的 Windows Server 2016 伺服器新增至伺服器陣列，驗證功能並從負載平衡器移除較舊的伺服器。 當所有伺服器陣列節點都執行 Windows Server 2016 之後，您就可以將伺服器陣列行為層級升級為2016，並開始使用新的功能。  

如需詳細資訊，請參閱[升級至 Windows Server 2016 中的 AD FS。](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
